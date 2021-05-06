# Simple TypeScript Script Example with UUIDs

This example shows how to use [Prisma Client](https://www.prisma.io/docs/reference/tools-and-interfaces/prisma-client) in a **simple TypeScript script** to read and write data in a PostgreSQL database. 

## Getting started

1. Start the PostgreSQL development database with Docker Compose:

```bash
docker compose up -d
```

2. Run the migration

```bash
npm run migrate:dev
```

3. Run the script

```bash
npm run dev
```


## Using the native uuid type in PostgreSQL with Prisma

```prisma
model User {
  id        String   @id @default(uuid()) @db.Uuid
  createdAt DateTime @default(now())
  email     String   @unique
  name      String?
  posts     Post[]
}
```

Note the first line which adds the `@db.Uuid` annotation. This tells Prisma Migrate to generate a migration with the `uuid` type.

To verify, open the [migration](./prisma/migrations/20210506095939_/migration.sql):

```sql
CREATE TABLE "User" (
    "id" UUID NOT NULL,
    "createdAt" TIMESTAMP(3) NOT NULL DEFAULT CURRENT_TIMESTAMP,
    "email" TEXT NOT NULL,
    "name" TEXT,
    PRIMARY KEY ("id")
);
```


## Verifying the size of the PostgreSQL UUID type

To check the size of the type `uuid` in PostgreSQL, run the following query:

```sql
SELECT typlen FROM pg_type WHERE oid = 'uuid'::regtype::oid
```

The size of the `uuid` type is also documented in the [PostgreSQL docs](https://www.postgresql.org/docs/current/datatype-uuid.html):


> A UUID is written as a sequence of lower-case hexadecimal digits, in several groups separated by hyphens, specifically a group of 8 digits followed by three groups of 4 digits followed by a group of 12 digits, **for a total of 32 digits representing the 128 bits**. 



