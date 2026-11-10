```ts
// fastify
import { FileInterceptor, UploadedFile } from '@nest-lab/fastify-multer';
// express
import { UploadedFile } from '@nestjs/common';
import { FileInterceptor } from '@nestjs/platform-express';

// برای ولیدیشن
function imagePipe(required: boolean) {
  return new ParseFilePipe({
    fileIsRequired: required,

    validators: [new MaxFileSizeValidator({ maxSize: 2 * 1024 * 1024 }), new FileTypeValidator({ fileType: /^image\/(jpeg|png|webp)$/ })],
  });
}

// استفاده در کنترلر

@Controller('rooms')
export class RoomsController {
  constructor(private readonly roomsService: RoomsService) {}

  @Patch(':id')
  @UseInterceptors(FileInterceptor('image'))
  updateRoom(@Param('id', ParseUUIDPipe) id: string, @Body() updateRoomData: UpdateRoomDto, @UploadedFile(imagePipe(false)) image?: any) {
    return this.roomsService.updateRoom(id, updateRoomData, image);
  }

  // استفاده در کنترلر

  @Post()
  @UseInterceptors(FileInterceptor('image'))
  createRoom(@Body() roomData: CreateRoomDto, @UploadedFile(imagePipe(true)) image: any) {
    return this.roomsService.createRoom(roomData, image);
  }
}
```

---

برای fastify باید اینو توی main.ts تنظیم کنی

```ts
import { NestFactory } from '@nestjs/core';
import { FastifyAdapter, NestFastifyApplication } from '@nestjs/platform-fastify';
import fastifyMulter from 'fastify-multer';

async function bootstrap() {
  const app = await NestFactory.create<NestFastifyApplication>(AppModule, new FastifyAdapter());

  await app.register(fastifyMulter.contentParser);

  await app.listen(3000, '0.0.0.0');
}
bootstrap();

//
```

راه رسمی fastify native

`npm i @fastify/multipart`

```ts
import multipart from '@fastify/multipart';
// main.ts داخل
await app.register(multipart, {
  limits: { fileSize: 2 * 1024 * 1024 },
});

//

@Post()
async createRoom(@Req() req: FastifyRequest) {
  const file = await req.file();
  const buffer = await file.toBuffer();

  // اعتبارسنجی دستی
  if (!/^image\/(jpeg|png|webp)$/.test(file.mimetype)) {
    throw new BadRequestException('Invalid file type');
  }

  // فیلدهای متنی: file.fields
  return this.roomsService.createRoom(/* ... */, buffer);
}
```
