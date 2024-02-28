
==a continuacion los comandos para instalar jest y quitar el soporte de mocha==

2016  npm uninstall chai chai-http mocha mocha-junit-reporter should
 2017  npm install --save-dev jest
 2018  npm install --save-dev ts-jest @types/jest


==instalando typescript:== 

 2019  npm install --save-dev typescript
 2020  npx tsc --init

![[Pasted image 20240228100629.png]]

notese que estoy usando commonjs (require modules..) ==al final de la migracion a typescript deberia utilizar Ecmmascript modules== (import ...)

