# Controller

Controllers are responsible for handling incoming requests and sending responses
back to the client.

```typescript
// cats.controller.ts
import {
  Controller,
  Get,
  Query,
  Post,
  Body,
  Put,
  Param,
  Delete,
} from "@nestjs/common";
import { CreateCatDto, UpdateCatDto, ListAllEntities } from "./dto";

@Controller("cats") // route path prefix of `/cats`
export class CatsController {
  @Post() // HTTP verb (Post) and path (/) => `POST /cats`
  create(@Body() createCatDto: CreateCatDto) {
    return "This action adds a new cat";
  }

  @Get()
  findAll(@Query() query: ListAllEntities /* Capture query parameters */) {
    return `This action returns all cats (limit: ${query.limit} items)`;
  }

  @Get(":id") // route path of `/:id` => `GET /cats/:id`
  findOne(@Param("id") id: string /* capture the `id` route parameter */) {
    return `This action returns a #${id} cat`;
  }

  @Put(":id")
  update(
    @Param("id") id: string,
    @Body() updateCatDto: UpdateCatDto /* Capture the body of the request */,
  ) {
    return `This action updates a #${id} cat`;
  }

  @Delete(":id")
  remove(@Param("id") id: string) {
    return `This action removes a #${id} cat`;
  }
}

// create-cat.dto.ts
export class CreateCatDto {
  name: string;
  age: number;
  breed: string;
}
/* ... */

// app.module.ts
import { Module } from "@nestjs/common";
import { CatsController } from "./cats/cats.controller";

@Module({
  controllers: [CatsController], // register the controller
  /* ... */
})
export class AppModule {}
```
