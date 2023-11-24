En este link explican las ventajas de utilizar  pnpm frente a los demas gestores de paquetes: 
https://hackernoon.com/choosing-the-right-package-manager-npm-yarn-or-pnpm

Instalacion:

npm install -g pnpm

![[Pasted image 20231108100205.png]]

verificar instalacion:

![[Pasted image 20231108100928.png]]

iniciar un proyecto react + ts:
=

pnpm create react-app pymewebapp --template typescript

![[Pasted image 20231108103652.png]]

iniciar un proyecto pnpm+react+ts+vite ideal para una PYME:
=

![[Pasted image 20231108110818.png]]

en esta imagen, SWC significa soporte para Next.JS, se supone que no hay clavo pero hay que poner mucha atencion a la hora del deploy a produccion, asi que actualmente sólo selecciono TS:

![[Pasted image 20231108110916.png]]


iniciar un proyecto pnpm+react+ts+webpack ideal para un cliente grande:
=

pnpm create react-app eprisewebapp --template typescript --dev-builder webpack

![[Pasted image 20231108111823.png]]


instalar paquetes y correr el proyecto:
=

pnpm install
pnpm remove "nombre_paquete"
pnpm install --save-dev "nombre_paquete"
pnpm dev


