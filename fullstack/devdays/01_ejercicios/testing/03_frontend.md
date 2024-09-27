1. instalar jest: yarn add --dev jest
2. pnpm add @types/jest -D
3. quitar alguna dependencia: yarn remove <nombre_dependencia>
4. luego: npx jest --init

![[Pasted image 20240917095524.png]]

como el anterior script hace una serie de preguntas no es necesario modificar el test.config.js que se genera.

Test de prueba:

![[Pasted image 20240917115144.png]]

# INSTALANDO REACT TESTING LIBRARY Y JEST PARA FRONTEND

```
pnpm install --save-dev @testing-library/react @testing-library/dom @types/react @types/react-dom
pnpm add --save-dev @babel/preset-react

pnpm install @testing-library/jest-dom

pnpm add -D jest @testing-library/react @testing-library/jest-dom @testing-library/user-event @testing-library/jest-dom

pnpm add -D jest ts-jest @types/jest
npx jest --init

pnpm add -D redux-mock-store
pnpm add -D @types/redux-mock-store

pnpm add -D jest-environment-jsdom

```

![[Pasted image 20240917135108.png]]

==archivos que necesitan actualizarse para soportar jest+react testing library+ts:

```
jest.config.ts
jest.setup.ts
tsconfig.json
```

