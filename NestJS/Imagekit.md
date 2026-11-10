# imagekit

```ts
import { Injectable, InternalServerErrorException, Logger } from '@nestjs/common';
import { ConfigService } from '@nestjs/config';
import ImageKit, { toFile } from '@imagekit/nodejs';

@Injectable()
export class ImagekitService {
  private readonly logger = new Logger(ImagekitService.name);
  private readonly client: ImageKit;

  constructor(config: ConfigService) {
    this.client = new ImageKit({ privateKey: config.getOrThrow<string>('IMAGEKIT_PRIVATE_KEY') });
  }

  async upload(file: any, folder: string) {
    const res = await this.client.files.upload({
      file: await toFile(file.buffer, file.originalname),

      fileName: file.originalname,

      folder,

      useUniqueFileName: true,
    });

    if (!res.url || !res.fileId) {
      throw new InternalServerErrorException('Image upload failed');
    }

    return { url: res.url, fileId: res.fileId };
  }

  async delete(fileId: string) {
    try {
      await this.client.files.delete(fileId);
    } catch (err) {
      // شدن کل درخواست بشه fail حذف عکس نباید باعث
      this.logger.warn(`Failed to delete image ${fileId}: ${String(err)}`);
    }
  }
}
```

اپلود عکس

```ts
  async createRoom(roomData: CreateRoomDto, image: any) {
    if (await this.roomRepo.existsBy({ roomNumber: roomData.roomNumber })) {
      throw new ConflictException('This room already exists');
    }

    const { url, fileId } = await this.imagekitService.upload(image, '/rooms');

    try {
      const room = this.roomRepo.create({ ...roomData, imageUrl: url, imageFileId: fileId });
      return await this.roomRepo.save(room);
    } catch (err) {
      await this.imagekitService.delete(fileId);
      throw err;
    }
  }
```

اپدیت عکس

```ts
 async updateRoom(id: string, updateRoomData: UpdateRoomDto, image?: any) {
    const hasFields = Object.values(updateRoomData).some((value) => value !== undefined);

    if (!hasFields && !image) {
      throw new BadRequestException('At least one field or an image is required');
    }

    const room = await this.roomRepo.findOneBy({ id });
    if (!room) throw new NotFoundException('room not found');

    if (
      updateRoomData.roomNumber && // اگر شماره اتاق نبود اصلا تکراری بودن چک نکن
      updateRoomData.roomNumber !== room.roomNumber && // شماره اتاق دوباره همین شماره فعلی بود ارور نده الکی
      (await this.roomRepo.existsBy({ roomNumber: updateRoomData.roomNumber }))
    ) {
      throw new ConflictException('This room number already exists');
    }

    const oldFileId = room.imageFileId;
    let newFileId: string | undefined;

    Object.assign(room, updateRoomData);

    if (image) {
      const uploaded = await this.imagekitService.upload(image, '/rooms');
      newFileId = uploaded.fileId;
      room.imageUrl = uploaded.url;
      room.imageFileId = uploaded.fileId;
    }

    // این برای اینه که اگه روی دیتابیس ذخیره نشد عکسی که روی سرویس ذخیره شده حذف شه
    try {
      const saved = await this.roomRepo.save(room);

      if (newFileId && oldFileId) await this.imagekitService.delete(oldFileId);

      return saved;
    } catch (err) {
      if (newFileId) await this.imagekitService.delete(newFileId);

      throw err;
    }
  }
```

حذف عکس

```ts
async deleteOne(id: string) {
    const room = await this.roomRepo.findOne({
      where: { id: id },

      relations: {
        reservations: true,
      },
    });

    if (!room) {
      throw new NotFoundException('Room not found');
    }

    if (room.reservations.length > 0) {
      throw new ConflictException({
        message: 'Cannot delete a room that has reservations',
        reservations: room.reservations,
      });
    }

    const fileId = room.imageFileId;

    await this.roomRepo.remove(room);

    if (fileId) await this.imagekitService.delete(fileId);

    return { message: 'room deleted!' };
  }
```
