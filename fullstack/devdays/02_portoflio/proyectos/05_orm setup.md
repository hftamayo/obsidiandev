### Prisma

==Paso 1:

El comando se ejecuta en el ROOTDIR del proyecto:
npx prisma init

Si quiero usar el datasource provider mysql:
npx prisma init --datasource-provider mysql

![[Pasted image 20241120102531.png]]

Es mejor no mover la carpeta PRISMA adentro de SRC.

==Paso 2:

agregas los models en el schema.prisma

==Paso 3:

npx prisma migrate dev --name init_models_version  <- este nombre puede ir cambiando
npx prisma generate

==Paso 4:


prisma requiere un usuario con privilegios de root, darle semejante acceso al usuario de la apluicacion seria lo peor, entonces puedo hacer un .env-migrate donde desnudo la cadena de conexion y utilizo un usuario de alto nivel:

![[Pasted image 20241203095744.png]]


Tambien para correr la creacion de las migraciones con un env personalizado necesito tener instalado el dotenv-cli:

![[Pasted image 20241203101443.png]]

una vez hecho eso, si la carpeta prisma pertenece al ROOTDIR, ejecuto el siguiente comando:

![[Pasted image 20241203101529.png]]

se generarán al menos 2 archivos y éstos debo incluirlos en el codebase y replicarlo al repo:

![[Pasted image 20241203101620.png]]

==Paso 5:

ejecutar prisma generate:

![[Pasted image 20241203102210.png]]

### Setup de la capa de datos usando mis sistemas de información:

==este es el approach a diciembre 2024:

1. el dev a mano debe haber generado a pie al menos 1 migracion inicial que incluye los esquemas de las tablas, esta migración es parte del codebase
2. en el package.json hay un script denominado "setup" que permite generar en forma manual el esquema de las tablas a un data layer ya sea local o remoto (inclusive dockerizado), esto es por medio de un script en el codebase que se llama dataLayerSetup.ts. Para ejecutar el proceso de migracion se requiere un usuario administrador, cada sistema tiene un env-migrate con la cadena de conexion
3. dataLayerSetup levanta las enviro vars, genera migraciones, las aplica en el datalayer y ejecuta el seeding de la data inicial, finaliza la ejecución regresando al prompt
4. Demo de la ejecucion manual de la migracion:

```
px dotenv -e .env-migrate -- npx prisma migrate dev --name init_models_version

npx prisma migrate
```


Ejecutando el backend con la bandera de data seeding en true:





5. Demo de la ejecución del script setup (requiere usuario con permisos de root):


6. al ejecutar el server verifica si hay necesidad de hacer un seeding, en caso positivo ejecuta las tareas y sigue con el proceso de loading del server hasta dejarlo en escucha

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

