# prisma-generator-fake-data

![Generated with Bing Image Creator. Prompt: create a logo with a cute panda smiling and holding a triangular prisma, digital art](assets/logo.jpg)

_Generated with Bing Image Creator. Prompt: create a logo with a cute panda smiling and holding a triangular prisma, digital art_

## The easiest way to generate mock data based on your Prisma models!

#### What is Prisma-Generator-Fake-Data?

Prisma-Generator-Fake-Data is a generator that uses [faker.js](https://fakerjs.dev/) to create realistic-looking fake data for your Prisma models. With this generator, you can quickly and easily create fake data for your Prisma models, without having to write any code.

#### Get started

- Setup your Prisma project as usual ([Get Started With Prisma](https://www.prisma.io/docs/getting-started))
- Install this package
  - `npm install -D prisma-generator-fake-data`
  - `yarn add -D prisma-generator-fake-data`
  - `pnpm install -D prisma-generator-fake-data`
- Modify your Prisma model file
  ```prisma
  generator custom_generator {
      provider = "prisma-generator-fake-data"
      /// File will be generated in this path
      output   = "../types/fake-data.ts"
  }
  ```
- Run `npx prisma generate`

You're all done!

#### Usage

Once the file is generated, you can import it in your project.

```prisma
generator client {
  provider = "prisma-client-js"
}

generator custom_generator {
  provider = "prisma-generator-fake-data"
  /// by default, the file will be generated at `./prisma/fake-data.ts`
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model User {
  id       String        @id @default(cuid())
  email    String        @unique
  name     String
  ///FAKE:{notificationsEnabled: faker.datatype.boolean(), preferredColor: faker.color.rgb()}
  settings Json
  status   UserStatus
}

enum UserStatus {
  active
  inactive
}
```

```ts
import { fakeUser } from './prisma/fake-data'; //or your custom output path

console.log(fakeUser());
/*
{
  email: 'Marianne_Zulauf96@yahoo.com',
  name: 'Gerald Schulist',
  age: 30,
  settings: { notificationsEnabled: true, preferredColor: '#a7e5db' },
  status: 'inactive'
}
*/
```

### Fields with special treatment:

- `name`, `fullName`, `firstName`, `lastName`, `age` will use specific faker-js methods to look more realistic
- If you have a `JSON` Prisma field, you can add your own data shape by adding a special comment above your field
  - Example: `///FAKE:{test: faker.lorem.word()}`
  - Keep in mind that the generator will just relay whatever is after `FAKE:` to your generated code, so it needs to be valid TypeScript

<sup><sub>Inspired by https://github.com/toyamarinyon/prisma-factory-generator</sub></sup>

<sup><sub>This generator was bootstraped using [create-prisma-generator](https://github.com/YassinEldeeb/create-prisma-generator)
</sub></sup>
