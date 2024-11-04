### Prisma

npx prisma init

--agregas los models en el schema.prisma
npx prisma migrate dev --name init_models_version  <- este nombre puede ir cambiando
npx prisma generate


## TypeORM

In TypeORM, the process to set up and migrate your database schema is different from Prisma. Here are the steps to replicate the models defined using TypeORM:

1. **Initialize TypeORM**:
   Ensure that your TypeORM configuration is set up correctly and that your entities are included.

2. **Generate Migrations**:
   Use the TypeORM CLI to generate migrations based on your entities.

3. **Run Migrations**:
   Apply the migrations to your database.

### Step-by-Step Instructions

1. **Ensure TypeORM CLI is Installed**:
   If you haven't already, install the TypeORM CLI as a development dependency.

   ```sh
   npm install typeorm -D
   ```

2. **Create a TypeORM Configuration File**:
   Ensure you have a `ormconfig.ts` or equivalent configuration file that includes your database connection settings and entities.

   **Example `ormconfig.ts`**:
   ```typescript
   import { DataSourceOptions } from 'typeorm';
   import dotenv from 'dotenv';
   import { Role } from './entity/Role';
   import { User } from './entity/User';
   import { Task } from './entity/Task';

   dotenv.config();

   const config: DataSourceOptions = {
     type: 'postgres',
     host: process.env.TYPEORM_HOST,
     port: parseInt(process.env.TYPEORM_PORT || '5432', 10),
     username: process.env.TYPEORM_USERNAME,
     password: process.env.TYPEORM_PASSWORD,
     database: process.env.TYPEORM_DATABASE,
     synchronize: false, // Set to false when using migrations
     logging: process.env.TYPEORM_LOGGING === 'true',
     entities: [Role, User, Task],
     migrations: ['src/migration/**/*.ts'],
     subscribers: ['src/subscriber/**/*.ts'],
   };

   export default config;
   ```

3. **Generate Migration**:
   Use the TypeORM CLI to generate a migration based on your entities. Ensure you are in the root directory of your project when running these commands.

   ```sh
   npx typeorm migration:generate -n InitialMigration -d src/migration
   ```

4. **Run Migration**:
   Apply the generated migrations to your database.

   ```sh
   npx typeorm migration:run
   ```

### Example Directory Structure

Your directory structure should look something like this:

```
ROOTDIR
├── src
│   ├── entity
│   │   ├── Role.ts
│   │   ├── User.ts
│   │   └── Task.ts
│   ├── migration
│   │   └── <generated_migrations>.ts
│   ├── subscriber
│   └── ormconfig.ts
├── .env
├── package.json
└── tsconfig.json
```

### Running the Commands

1. **Generate Migration**:
   ```sh
   npx typeorm migration:generate -n InitialMigration -d src/migration
   ```

2. **Run Migration**:
   ```sh
   npx typeorm migration:run
   ```

By following these steps, you will generate and apply migrations to your database using TypeORM, similar to how you would with Prisma. This ensures that your database schema is in sync with your TypeORM entities.


==comando que me funciono:
```

npx typeorm migration:generate -d src/typeorm/ormconfig.js src/typeorm/migration/InitialMigration

npx ts-node -r tsconfig-paths/register ./node_modules/typeorm/cli.js migration:generate -d src/typeorm/ormconfig.ts src/typeorm/migration/InitialMigration

```

