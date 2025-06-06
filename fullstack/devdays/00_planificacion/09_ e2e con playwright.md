
## Instalacion

```
mkdir e2e
cd e2e
npm init -y
npm install --save-dev playwright
npx playwright install
npm create playwright
```

## Setup

the file playwright.config.ts need update
the file tsconfig.json needs to be created manually and have the proper config:

![[Pasted image 20240920103400.png]]

por favor referirse a las ultimas versiones de playwright.config.ts, tsconfig.json, package.json y turbo.json

comando para correrlo en el caso del monorepo desde el ROOTDIR:

==pnpm exec turbo e2e:run

![[Pasted image 20240920112748.png]]

## generar la aplicacion antes de correr los test de playwright

- generar la aplicacion por separado usando el script pnpm run build
- favor revisar los tsconfig.json del server y del cliente para ajustes de dist y otros aspectos
- si por algun motivo tengo problemas durante el build puedo usar comandos como este:

```
find apps/server/src -name '*.js' -delete
find apps/client/src -name '*.js' -delete

Si quiero correr un script en particular:
node apps/server/dist/backend.js

```

- 

## corriendo los tests

verificar el turbo.json:

![[Pasted image 20240920145451.png]]

y en el package.json del ROOTDIR algo asi como:
cd apps/e2e && npx playwright test