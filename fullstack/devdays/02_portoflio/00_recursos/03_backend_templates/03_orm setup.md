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
npx dotenv -e .env-migrate -- npx prisma migrate dev --name init_models_version

npx prisma migrate
```


Ejecutando el backend con la bandera de data seeding en true:

generando las migrations a mano:

![[Pasted image 20241206151114.png]]


ahora bien, si ya tengo creada las migraciones pero restaure mi data container el comando que necesito ejecutar es este:

```
npx dotenv -e .env-migrate -- npx prisma migrate deploy

```

![[Pasted image 20241209114434.png]]

corriendo el sistema con la bandera de seeding en true:

![[Pasted image 20241209114551.png]]


6. al ejecutar el server verifica si hay necesidad de hacer un seeding, en caso positivo ejecuta las tareas y sigue con el proceso de loading del server hasta dejarlo en escucha

7.  corriendo el server apagando la bandera de data seeding

![[Pasted image 20241209114757.png]]

==Automatizando el proceso de migraciones mas spin up del server:

I have concern about how to manage the manual processes, specifically this commands:

1. npx dotenv -e .env-migrate -- npx prisma migrate dev --name init_models_version
    
2. npx dotenv -e .env-migrate -- npx prisma migrate deploy -in case it is necessary -
    

the intention is to automate the needed processes to spin up the application, moreover, I will use a CI/CD pipeline so having manual steps break the entire login of CI

which -in theory- has the needed steps but I have these cons:

1. if the context change the application get into an infinite loop
2. running that script requires admin privileges
3. I'm not sure if the best approach should be: run an independent task for run the migrations, run a second task to spin up the application, moreover, I'm not sure if these -or other approach- should be managed by the codebase or for the CI/CD pipeline

==Copilot me dio esta sugerencia:

To address your concerns and ensure a smooth CI/CD pipeline, it's important to separate the concerns of running migrations and starting the application. This can be achieved by creating separate scripts for running migrations and starting the application, and then orchestrating these scripts in your CI/CD pipeline.

### Approach

1. **Separate Scripts**: Create separate scripts for running migrations and starting the application.
2. **CI/CD Pipeline**: Configure your CI/CD pipeline to run the migration script before starting the application.

### Example Scripts

1. **Migration Script**: A script to run migrations and generate the Prisma Client.
2. **Start Script**: A script to start the application.

### Migration Script (`scripts/migrate.js`)

Create a script to handle migrations and Prisma Client generation.

```javascript
import { exec } from 'child_process';
import { databaseUrl } from '../../../config/envvars.js';

function execCommand(command) {
  return new Promise((resolve, reject) => {
    exec(
      command,
      {
        env: {
          ...process.env,
          DATABASE_URL: databaseUrl,
        },
      },
      (error, stdout, stderr) => {
        if (error) {
          console.error(`Error executing command: ${command}\n${stderr}`);
          return reject(error);
        }
        console.log(`Command executed successfully: ${command}\n${stdout}`);
        resolve();
      },
    );
  });
}

async function runMigrations() {
  try {
    console.log('Running prisma migrate deploy...');
    await execCommand('npx prisma migrate deploy');

    console.log('Running prisma generate...');
    await execCommand('npx prisma generate');

    console.log('Migrations and generation completed successfully.');
  } catch (error) {
    console.error('Error running migrations and generation:', error);
    process.exit(1);
  }
}

runMigrations();
```

### Start Script (`scripts/start.js`)

Create a script to start the application.

```javascript
import { exec } from 'child_process';

function execCommand(command) {
  return new Promise((resolve, reject) => {
    exec(command, (error, stdout, stderr) => {
      if (error) {
        console.error(`Error executing command: ${command}\n${stderr}`);
        return reject(error);
      }
      console.log(`Command executed successfully: ${command}\n${stdout}`);
      resolve();
    });
  });
}

async function startApplication() {
  try {
    console.log('Starting application...');
    await execCommand('npm run start');
    console.log('Application started successfully.');
  } catch (error) {
    console.error('Error starting application:', error);
    process.exit(1);
  }
}

startApplication();
```

### CI/CD Pipeline Configuration

Configure your CI/CD pipeline to run the migration script before starting the application. Here is an example configuration for a GitHub Actions workflow:

```yaml
name: CI/CD Pipeline

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '14'

      - name: Install dependencies
        run: npm install

      - name: Run migrations
        run: node scripts/migrate.js

      - name: Start application
        run: node scripts/start.js
```

### Explanation

1. **Separate Scripts**: Create separate scripts for running migrations (`scripts/migrate.js`) and starting the application (`scripts/start.js`).
2. **CI/CD Pipeline**: Configure the CI/CD pipeline to run the migration script before starting the application.
3. **Error Handling**: Ensure proper error handling in both scripts to exit with a non-zero status code if an error occurs.

### Summary

- **Separate Concerns**: Separate the concerns of running migrations and starting the application into different scripts.
- **CI/CD Pipeline**: Configure the CI/CD pipeline to run the migration script before starting the application.
- **Error Handling**: Ensure proper error handling in both scripts to exit with a non-zero status code if an error occurs.

This approach ensures that your database schema is up-to-date before starting the application, and it allows you to automate the necessary processes in your CI/CD pipeline without manual intervention.

Similar code found with 2 license types


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

