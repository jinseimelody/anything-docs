# [Prisma](https://www.prisma.io/docs/concepts/database-connectors/mysql)
Prisma là next-generation ORM bao gồm 3 chức năng sau:

  - [Prisma Client](https://www.prisma.io/docs/concepts/components/prisma-client): Tự động tạo query an toàn cho Node.js & TypeScript
  - [Prisma Migrate](https://www.prisma.io/docs/concepts/components/prisma-migrate): khai báo, chuyển đổi modeling - database
  - [Prisma Studio](https://github.com/prisma/studio): GUI dùng để xem và chỉnh sửa db

## Ready-to-run example projects
[rest-express](https://github.com/prisma/prisma-examples/tree/latest/typescript/rest-express)

## Getting start
### I. Cài đặt prisma
```
npm install prisma --save-dev
```

### II. Init & Generate
Chạy lệnh sau để init prisma folder và schema file`
```
npx prisma init
````
Chỉnh sửa file `schema.prisma` trong thư mục ./prisma cùng cấp với file `package.json` tương tự sample bên dưới xem thêm [cheatsheet](https://www.prisma.io/docs/concepts/components/prisma-schema).
```
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "mysql"
  url      = env(DATABASE_URL)
}

model Sample {
  id       Int     @id @default(autoincrement())
  task     String?
  status   Int?
}
```
Chạy lệnh sau để generate model
```
npx prisma generate
```
### III. Sử dụng prisma
1. Cấu hình logging [cheatsheet](https://www.prisma.io/docs/concepts/components/prisma-client/working-with-prismaclient/logging)

    ```
    const prisma = new PrismaClient({
      log: ['query', 'info', 'warn', 'error'],
    })
    ```
2. Cấu hình logging middleware `optional` (thường dùng để debug thời gian chạy query)
    ```
    const prisma = new PrismaClient()

    prisma.$use(async (params, next) => {
    const before = Date.now()

    const result = await next(params)

    const after = Date.now()

    console.log(`Query ${params.model}.${params.action} took ${after - before}ms`)

    return result
    })
    ```
3. CURD [cheatsheet](https://www.prisma.io/docs/concepts/components/prisma-client/crud)
4. Sử dụng raw database access `optional` (thường sử dụng để thực thi các câu query phức tạp)
Có 2 cách chính để gửi query tới DB

    - `$queryRaw` để thực thi các câu select.
    - `$executeRaw` dùng để thực thi các câu lệnh CUD.

    Bên cạnh đó có thể sử dụng `$queryRawUnsafe` và `$executeRawUnsafe` để thực thi raw query tuy nhiên mình sẽ không sử dụng vì nó tiềm ẩn SQL injection risk

        ```
        # Read
        const email = 'emelie@prisma.io'
        const result = await prisma.$queryRaw`SELECT * FROM User WHERE email = ${email}`

        # Create, Update and Delete
        const emailValidated = true
        const active = true

        const result: number =
        await prisma.$executeRaw`UPDATE User SET active = ${active} WHERE emailValidated = ${emailValidated};`
        ```

## Prisma CLI

### Cài đặt
Prisma cli được cài đặt khi cài đặt prisma
```
npm install prisma --save-dev
```

### Sử dụng prisma cli
```
Prisma is a modern DB toolkit to query, migrate and model your database (https://prisma.io)

Usage

  $ prisma [command]

Commands

            init   Setup Prisma for your app
        generate   Generate artifacts (e.g. Prisma Client)
              db   Manage your database schema and lifecycle
         migrate   Migrate your database
          studio   Browse your data with Prisma Studio
          format   Format your schema

Flags

     --preview-feature   Run Preview Prisma commands

Examples

  Setup a new Prisma project
  $ prisma init

  Generate artifacts (e.g. Prisma Client)
  $ prisma generate

  Browse your data
  $ prisma studio

  Create migrations from your Prisma schema, apply them to the database, generate artifacts (e.g. Prisma Client)
  $ prisma migrate dev

  Pull the schema from an existing database, updating the Prisma schema
  $ prisma db pull

  Push the Prisma schema state to the database
  $ prisma db push
```