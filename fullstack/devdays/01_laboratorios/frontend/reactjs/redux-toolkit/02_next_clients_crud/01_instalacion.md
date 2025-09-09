yarn create next-app

![[Pasted image 20230719132428.png]]

![[Pasted image 20230719132533.png]]

una ventaja es que se instala tailwind desde inicio.

La instalacion del proyecto es más tardada comparada con vite

instalacion de tailwind y dependencias asociadas:

![[Pasted image 20230720113400.png]]

![[Pasted image 20230720113736.png]]

modificar los siguientes archivos:

![[Pasted image 20230720114620.png]]

![[Pasted image 20230720114637.png]]

puesto que active EsLINT si tengo archivos jsx y js otendré este error:

![[Pasted image 20230720120245.png]]

tengo que hacer la siguiente modificacion a vite.config.js:

```javascript
import { defineConfig, transformWithEsbuild } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [
    {
      name: 'treat-js-files-as-jsx',
      async transform(code, id) {
        if (!id.match(/src\/.*\.js$/))  return null

        // Use the exposed transform from vite, instead of directly
        // transforming with esbuild
        return transformWithEsbuild(code, id, {
          loader: 'jsx',
          jsx: 'automatic',
        })
      },
    },
    react(),
  ],

  optimizeDeps: {
    force: true,
    esbuildOptions: {
      loader: {
        '.js': 'jsx',
      },
    },
  },
})
```

