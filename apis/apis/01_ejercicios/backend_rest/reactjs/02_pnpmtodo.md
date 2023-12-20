![[Pasted image 20231213154250.png]]

Install critical dependencies:

pnpm add react react-dom react-router-dom webpack webpack-dev-server babel-loader @babel/preset-env @babel/core @babel/polyfill

![[Pasted image 20231213155029.png]]

![[Pasted image 20231213155257.png]]

crear un archivo webpack.config.js con el siguiente contenido:



"
const path = require("path"); module.exports = { entry: "./src/index.js", output: { filename: "bundle.js", path: path.resolve(__dirname,

"dist"), }, module: { rules: [ { test: /\.jsx?$/, exclude: /node_modules/, use: { loader: "babel-loader", options: { presets: ["@babel/preset-env"], }, }, }, ], }, devServer: { contentBase: "./dist", port: 3000, open: true, }, };
"

![[Pasted image 20231213155500.png]]

pnpm run start o pnpm run build si quiero que la aplicacion salga a produccion:

![[Pasted image 20231213174351.png]]

este error se supone que es porque hay 2 versiones de eslint en mi pnpm y la que el proyecto esta tratando de levantar, entonces para resolver:

pnpm upgrade eslint-plugin-react

y luego pnpm run start

![[Pasted image 20231213175003.png]]

Instalando tailwind:

pnpm add -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
contenido del archivo tailwind.config.js

![[Pasted image 20231213175805.png]]

actualizando el contenido de index.css:

![[Pasted image 20231215151909.png]]


en una sigueinte sesion me volvio a dar error de las versiones disponibles, este comando me resolvio:

pnpm install --shamefully-flatten


sin embargo seguia dando el problema, pues para resolver totalmente e problema use esto:

pnpm update eslint

luego pnpm run start y ya no dio el error.


probando si tailwind esta funcionando:

![[Pasted image 20231215154251.png]]
![[Pasted image 20231216183153.png]]

parece que funciona parcialmente, por algun motivo tailwind no aparece en package.json, volviendo a instalar pero esta vez sin el -D:

pnpm uninstall -D tailwindcss postcss autoprefixer


![[Pasted image 20231216191602.png]]

I also installed postcss-cli and these scripts:

![[Pasted image 20231216204604.png]]

the final versions of the config files are:

![[Pasted image 20231216204639.png]]

![[Pasted image 20231216204658.png]]

![[Pasted image 20231216204716.png]]

luego corri el script en watch para generar el css y luego el start:

pnpm run watch:css
pnpm run start

tambien en el index.css puse de referencia el css que se genera con el tailwind:

![[Pasted image 20231216210424.png]]


script de prueba

![[Pasted image 20231216210523.png]]

