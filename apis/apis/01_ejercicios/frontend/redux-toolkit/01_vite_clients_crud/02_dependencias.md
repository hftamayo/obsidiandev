yarn add tailwindcss postcss autoprefixer
yarn add react-redux @reduxjs/toolkit bootstrap react-router-dom
yarn add @popperjs/core

======================
configurar tailwind
======================

yarn tailwindcss init -p
modificar los archivos tailwind.config.js, index.css, vite.config.js

==============
tailwind.config.js
===
/** @type {import('tailwindcss').Config} */

module.exports = {

content: [

"./src/**/*.{js,jsx,ts,tsx}",

],

theme: {

extend: {},

},

plugins: [],

}

=========
index.css
========

@tailwind base;

@tailwind components;

@tailwind utilities;

========
vite.config.js
===

import { defineConfig, transformWithEsbuild } from "vite";

import react from "@vitejs/plugin-react";

  

// https://vitejs.dev/config/

export default defineConfig({

plugins: [

{

name: "treat-js-files-as-jsx",

async transform(code, id) {

if (!id.match(/src\/.*\.js$/)) return null;

  

return transformWithEsbuild(code, id, {

loader: "jsx",

jsx: "automatic",

});

},

},

react(),

],

optimizeDeps: {

force: true,

esbuildOptions: {

loader: {

".js": "jsx",

},

},

},

});

yarn run dev:

![[Pasted image 20230927120145.png]]


