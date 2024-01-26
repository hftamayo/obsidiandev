V1:
=

==Starting point para aplicaciones basadas en:
- pnpm
- webpack
- babel
- React+Express
- Tailwind

==Todo:

A futuro espero agregar soporte para TypeScript y Jest

==proceso de como fue creado:

Basado en el boilerplate00 funcional segui estos pasos:
- git clone https://github.com/hftamayo/boilerauth.git startingenterprise
- cd startingenterprise
- git checkout e1c0b59
- pnpm install
- pnpm run start
- git checkout -b experimental
- make the needed changes
- git commit -m "my new changes"
- git checkout -b main
- git reset --hard experimental
- git remote set-url origin https://github.com/hftamayo/boilerstartingenterprise.git
- git push -u origin main

V2:
=

* pnpm create react-app startingenterprise2
* cd startingenterprise2
* pnpm add react react-dom react-router-dom webpack webpack-dev-server babel-loader @babel/preset-env @babel/core @babel/polyfill
* pnpm install @mui/material @mui/icons-material @emotion/react @emotion/styled
* pnpm install react-icons
* copiar el webpack.config.js de auth y el .babelrc
* arranque el proyecto con pnpm run start y da el mismo error de versiones de react
* borré node_modules y luego pnpm install y ==no tuve necesidad del siguiente comando:
* pnpm install --shamefully-flattern


