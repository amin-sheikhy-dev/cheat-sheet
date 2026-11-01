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

import { FileInterceptor, File } from '@nest-lab/fastify-multer';

@Post()
@UseGuards(JwtGuard, RolesGuard)
@Roles(Role.Admin, Role.Owner)
@UseInterceptors(FileInterceptor('image'))
createRoom(
  @Body() roomData: CreateRoomDto,
  @UploadedFile(imagePipe(true)) image: File,
) {
  return this.roomsService.createRoom(roomData, image);
}
```

npm i @fastify/multipart

```ts
import multipart from '@fastify/multipart';

await app.register(multipart, {
  limits: { fileSize: 2 * 1024 * 1024 },
});

@Post()
@UseGuards(JwtGuard, RolesGuard)
@Roles(Role.Admin, Role.Owner)
async createRoom(@Req() req: FastifyRequest) {
  const file = await req.file();               // فایل
  const buffer = await file.toBuffer();

  // اعتبارسنجی دستی
  if (!/^image\/(jpeg|png|webp)$/.test(file.mimetype)) {
    throw new BadRequestException('Invalid file type');
  }

  // فیلدهای متنی: file.fields
  return this.roomsService.createRoom(/* ... */, buffer);
}
```
