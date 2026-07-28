
pnpm nx reset
pnpm nx show projects
pnpm nx test shared-infrastructure --verbose

Compilar la aplicacion:
pnpm nx build main-app --verbose
pnpm lint


pnpm nx g @nx/js:library companies --directory=libs/companies --skipFormat

pnpm nx generate @nx/workspace:remove --projectName=companies-application --no-interactive

pnpm nx g @nx/js:library domain --directory=libs/companies --skipFormat
pnpm nx g @nx/js:library infrastructure --directory=libs/companies --skipFormat