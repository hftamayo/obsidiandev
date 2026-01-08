
## Serie de comandos para crear el scaffold:

```
pnpm dlx create-nx-workspace@latest absencesboffice --preset=apps --packageManager=pnpm

cd absencesboffice

pnpm add -D @nx/react @nx/vite @nx/vitest

GENERANDO LA REACT-APP USANDO VITE Y SWC:
pnpm nx g @nx/react:app apps/main-app \
  --bundler=vite \
  --compiler=swc \
  --unitTestRunner=vitest \
  --e2eTestRunner=none \
  --style=tailwind \
  --routing=false \
  --strict
  
questions: which Linter: No, port for dev server: 4201

INSTALLING CORE DEPENDENCIES:
pnpm add @tanstack/react-query zustand zod wouter react-hook-form sonner lucide-react clsx tailwind-merge  
pnpm add -D tailwindcss-animate


SETTING UP BIOME:
pnpm add -D @biomejs/biome  
pnpm biome init

SETTING UP HUSKY AND LINT-STAGED:
pnpm add -D husky lint-staged
pnpm exec husky init

# Create the lint-staged config in package.json or a separate file
# For Biome, your lint-staged config should look like:
# "lint-staged": {
#   "*.{js,ts,cjs,mjs,d.cts,d.mts,jsx,tsx,json,jsonc}": [
#     "biome check --apply --no-errors-on-unmatched"
#   ]
# }

SETTING UP DE SHADCN/UI:
- por favor crear los ui core components manuales puesto que shadcn siempre buscar un setup next.js y el monorepo es distinto

- en su lugar instalar las core deps de shadcn:
pnpm add lucide-react clsx tailwind-merge class-variance-authority @radix-ui/react-slot tailwindcss-animate

ACTUALIZAR  SWC:
pnpm update @swc/core @swc-node/core @swc-node/register --latest
pnpm dedupe -> eliminar versiones flotantes

  
EN CASO QUE BIOME.JSON NO FUE GENERADO:
pnpm biome init

hacer esta actualizacion:
{
  "$schema": "https://biomejs.dev/schemas/2.3.11/schema.json",
  "vcs": {
    "enabled": true,
    "clientKind": "git",
    "useIgnoreFile": true
  },
  "files": {
    "ignoreUnknown": false,
    "ignore": ["node_modules", "dist", ".nx"]
  },
  "formatter": {
    "enabled": true,
    "indentStyle": "space",
    "indentWidth": 2,
    "lineWidth": 100
  },
  "linter": {
    "enabled": true,
    "rules": {
      "recommended": true,
      "style": {
        "useImportType": "error",
        "useNamingConvention": "warn"
      },
      "correctness": {
        "noUnusedVariables": "warn"
      },
      "nursery": {
        "useTailwindProperties": "warn"
      }
    }
  },
  "javascript": {
    "formatter": {
      "quoteStyle": "single",
      "jsxQuoteStyle": "double",
      "trailingCommas": "all"
    }
  },
  "assist": {
    "enabled": true,
    "actions": {
      "source": {
        "organizeImports": "on"
      }
    }
  }
}


ORCHESTATION DEL DOMAIN SHARED:
# Shared UI (Where Shadcn components and 'cn' helper will live)
pnpm nx g @nx/react:library shared-ui --name=shared-ui --directory=libs/shared/ui --bundler=vite --linter=none --unitTestRunner=vitest --importPath=@boabsenses/shared/ui
RESPUESTAS: (bundler: VITE, linter: NONE, UNIT TEST RUNNER: VITEST)

# Shared Logic (Domain, Application, Infrastructure)
pnpm nx g @nx/js:library shared-domain --name=shared-domain --directory=libs/shared/domain --bundler=none --linter=none --unitTestRunner=vitest --importPath=@boabsenses/shared/domain
RESPUESTAS: (bundler: NONE, linter: NONE, UNIT TEST RUNNER: VITEST)

pnpm nx g @nx/js:library shared-application --name=shared-application --directory=libs/shared/application --bundler=none --linter=none --unitTestRunner=vitest --importPath=@boabsenses/shared/application
RESPUESTAS: (bundler: NONE, linter: NONE, UNIT TEST RUNNER: VITEST)

pnpm nx g @nx/js:library shared-infrastructure --name=shared-infrastructure --directory=libs/shared/infrastructure --bundler=none --linter=none --unitTestRunner=vitest --importPath=@boabsenses/shared/infrastructure
RESPUESTAS: (bundler: NONE, linter: NONE, UNIT TEST RUNNER: VITEST)

--IMPORTANTE--
1. durante la ejecucion de estos comandos me generó error en la creacion de shared/ui:
2. tuve que actualizar el apps/main-app/vite.config.mts -> PROBABLEMENET ESTO NO ERA NECESARIO
3. luego el comando: pnpm nx reset
4. luego pnpm install
5. luego intentar el command, si me pide las preguntas de: a) cual bundler usar (none), 2) cual linter usar (none)            


ORCHESTATION DEL DOMINIO COMPANIES: (bundler: NONE, linter: NONE, UNIT TEST RUNNER: VITEST)
# Companies Domain (Where CompanyResponseDto and types go)
pnpm nx g @nx/js:library companies-domain --name=companies-domain --directory=libs/companies/domain --bundler=none --linter=none --unitTestRunner=vitest --importPath=@boabsenses/companies/domain
RESPUESTAS: (bundler: NONE, linter: NONE, UNIT TEST RUNNER: VITEST) 

# Companies Application (Where useCompanies hook and useCompanyData go)
pnpm nx g @nx/js:library companies-application --name=companies-application --directory=libs/companies/application --bundler=none --linter=none --unitTestRunner=vitest --importPath=@boabsenses/companies/application
RESPUESTAS: (bundler: NONE, linter: NONE, UNIT TEST RUNNER: VITEST)

# Companies Feature (Where CompanyContainer and CompanyTable go)
pnpm nx g @nx/react:library companies-feature --name=companies-feature --directory=libs/companies/feature --bundler=vite --linter=none --unitTestRunner=vitest --importPath=@boabsenses/companies/feature
RESPUESTAS: (bundler: VITE, linter: NONE, UNIT TEST RUNNER: VITEST)
```

## Creacion de un github repo que pueda servir de template

- el proceso de creación es totalmente normal, unicamente se activa que el repo servirá como template
- este repo quedó como privado

## Uso del repo como template para crear otro

```
- clonar el repo (git clone)
- pnpm install
- pnpm husky install
- pnpm lint
- pnpm lint:fix (si es necesario)  
- pnpm biome check --write --unsafe . (si es necesario)  
- pnpm dev
- pnpm nx graph

```  


