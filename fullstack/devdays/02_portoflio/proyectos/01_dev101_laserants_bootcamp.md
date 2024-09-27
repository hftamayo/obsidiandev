
==negocios de ejemplo:

https://morningsave.com/deals/hakol-mesh-magnetic-mosquito-door-net


# PROYECTO 3 : monorepo backend

primer sprint: 7 horas
segundo sprint: 2.8 horas


volta, turbo y husky:
```
Sure, here are some similar products for Volta and Turbo:

### Alternatives to Volta:
1. **nvm (Node Version Manager)**:
   - **Purpose**: Manages multiple versions of Node.js.
   - **Usage**: Allows you to switch between different Node.js versions easily.

2. **asdf**:
   - **Purpose**: Manages multiple runtime versions (Node.js, Python, Ruby, etc.).
   - **Usage**: A universal version manager that can handle multiple languages and tools.

### Alternatives to Turbo (Turborepo):
1. **Lerna**:
   - **Purpose**: Manages JavaScript projects with multiple packages.
   - **Usage**: Helps with versioning and publishing monorepos.

2. **Nx**:
   - **Purpose**: A build system with first-class monorepo support.
   - **Usage**: Provides powerful tools for managing and optimizing monorepos, including caching and task running.

These tools offer similar functionalities and can be used as alternatives depending on your specific needs and preferences.
```

![[Pasted image 20240829080412.png]]

==corriendo las migraciones por primera vez al db container:

npx prisma init
--agregas los models en el schema.prisma
npx prisma migrate dev --name init_models_version  <- este nombre puede ir cambiando
npx prisma generate


![[Pasted image 20240829133046.png]]

==status del container:

![[Pasted image 20240829133127.png]]

==endpoints de health check:
![[Pasted image 20240829134203.png]]

## Architecture considerations

### Aplicando el patron: repository pattern:

ya que prisma genera y gestiona los scaffolds de las entidades no es necesario tener modulos tipo models, ==aca se quiebra el patron MVC.== por tanto lo más logico es saltar a otro tipo de patron ==Repository pattern, este incluye modulos Repository->Sercvicios -> Controller -> Routes favoreciendo la aplicacion de los principios SOLID==.

### patron singleton para gestion de conexiones con la data layer:

==transcribo platica con Copilot == :

```
Your intention to open a single database connection and reuse it across your application is a good practice. With Prisma, you can achieve this by creating a single instance of `PrismaClient` and reusing it throughout your application. This approach avoids opening multiple connections and ensures efficient resource usage.

### Steps to Implement:

1. **Create a Singleton for `PrismaClient`**:
   Create a separate module to instantiate and export a single `PrismaClient` instance.

2. **Use the Singleton Instance in Repositories**:
   Import and use the singleton `PrismaClient` instance in your repository modules.

3. **Ensure Connection Management in Setup**:
   Manage the connection lifecycle (connect and disconnect) in your setup module.

### Example Implementation:

#### `prismaClient.ts` (Singleton for `PrismaClient`):
```typescript
import { PrismaClient } from '@prisma/client';

const prisma = new PrismaClient();

export default prisma;
```

#### `roleRepository.ts` (Using Singleton `PrismaClient`):
```typescript
import prisma from '../prismaClient';
import { Role } from '@prisma/client';

type CreateUpdateRole = Omit<Role, 'id'>;

export const getRoles = async (): Promise<Role[]> => {
  return prisma.role.findMany();
};

export const getRoleById = async (id: number): Promise<Role | null> => {
  return prisma.role.findUnique({
    where: { id },
  });
};

export const createRole = async (role: CreateUpdateRole): Promise<Role> => {
  return prisma.role.create({
    data: role,
  });
};
```

#### [`besetup.ts`](command:_github.copilot.openRelativePath?%5B%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2Fapps%2Fserver%2Fsrc%2Fbesetup.ts%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%5D "/home/hftamayo/sourcecode/nodejs/myprojects/laserants_tsbc_hw03/apps/server/src/besetup.ts") (Managing Connection Lifecycle):
```typescript
import express, { Request, Response, NextFunction } from 'express';
import cors from 'cors';
import cookieParser from 'cookie-parser';
import prisma from './prismaClient'; // Import the singleton PrismaClient
import healthCheckRouter from './routes/healthCheckRouter'; // Example route

const backend = express();
const port = process.env.PORT || 5001;
const mode = process.env.NODE_ENV || 'development';

const dbConnection = async () =>

 {


  try {
    await prisma.$connect();
    console.log(`Database connected in ${mode} mode`);
  } catch (error) {
    console.log("Database connection error: " + (error as Error).message);
    process.exit(1);
  }
};

async function StartBackend() {
  try {
    await dbConnection();
    backend.use(cors());
    backend.use(express.json());
    backend.use(express.urlencoded({ extended: true }));
    backend.use(cookieParser());

    // Method for request monitoring and logging
    backend.use((req: Request, res: Response, next: NextFunction) => {
      console.log(`Request received: [${req.method}] ${req.path}`);
      console.log(`Request headers: ${JSON.stringify(req.headers)}`);
      next();
    });

    backend.use('/health', healthCheckRouter);

    backend.listen(port, () => {
      console.log(`Server is running on port ${port}`);
    });
  } catch (error) {
    console.error("Failed to start backend:", error);
    process.exit(1);
  }
}

StartBackend();
```

### Explanation:
- **Singleton `PrismaClient`**: The `prismaClient.ts` file creates and exports a single instance of `PrismaClient`.
- **Repository Usage**: The repository modules import and use the singleton `PrismaClient` instance, ensuring that only one connection is used.
- **Connection Management**: The [`dbConnection`](command:_github.copilot.openSymbolFromReferences?%5B%22dbConnection%22%2C%5B%7B%22uri%22%3A%7B%22%24mid%22%3A1%2C%22fsPath%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2Fapps%2Fserver%2Fsrc%2Fbesetup.ts%22%2C%22external%22%3A%22file%3A%2F%2F%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2Fapps%2Fserver%2Fsrc%2Fbesetup.ts%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2Fapps%2Fserver%2Fsrc%2Fbesetup.ts%22%2C%22scheme%22%3A%22file%22%7D%2C%22pos%22%3A%7B%22line%22%3A15%2C%22character%22%3A10%7D%7D%5D%5D "Go to definition") function in [`besetup.ts`](command:_github.copilot.openRelativePath?%5B%7B%22scheme%22%3A%22file%22%2C%22authority%22%3A%22%22%2C%22path%22%3A%22%2Fhome%2Fhftamayo%2Fsourcecode%2Fnodejs%2Fmyprojects%2Flaserants_tsbc_hw03%2Fapps%2Fserver%2Fsrc%2Fbesetup.ts%22%2C%22query%22%3A%22%22%2C%22fragment%22%3A%22%22%7D%5D "/home/hftamayo/sourcecode/nodejs/myprojects/laserants_tsbc_hw03/apps/server/src/besetup.ts") manages the connection lifecycle, connecting to the database when the backend starts.

### Benefits:
- **Single Connection**: Ensures that only one database connection is opened and reused across the application.
- **Resource Efficiency**: Reduces the overhead of multiple connections, leading to better resource management.
- **Centralized Connection Management**: Simplifies connection management by centralizing it in the setup module.

By following this approach, you can efficiently manage your database connections with Prisma in a monorepo setup.


# SUBIR LAS IMAGENES ESTABLES AL DOCKER HUB

```
docker login --username <username> --password <docker pat>
docker push <usuario>/<imagen>:<tag>

```

# OPERACIONES CON CONTAINERS:

- conectarse al container de la base de datos:  psql -h localhost -U $POSTGRES_USER -d $POSTGRES_DB


# DEPLOY EN BARE METAL:

- si el monorepo esta fuera de contenedor todos los endpoints deben hacer referencia a localhost
- si la app ya esta en contenedor el requisito clave es que éste y el container de la BD esten en la misma red
- 



# DEPLOY EN AZURE