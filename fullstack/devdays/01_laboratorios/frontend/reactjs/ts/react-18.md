A partir de la version 17 de React ya no es necesario incluir el Import React from react en los componentes jsx, pero, es necesario hacer una serie de ajustes en el proyecto:

1. Asegurarse que React y React DOM estan usando una version superior a 18.0.0
2. Si el proyecto esta usando TS se necesita setear esta linea en el ==tsconfig.json:

![[Pasted image 20240920114616.png]]

3.  Si estoy usando Vite necesito hacer este ajuste en el vite.config.ts:

![[Pasted image 20240920114939.png]]


3.  por favor verifica cual es la ultima version de tsconfig porque siempre hay actualizaciones relativas a soporte de una version especifica de react, import the ts files, entre otros.