# Para configurar el Linter

- Debemos entender que un linter se encarga de que estamos escribiendo de que significado tiene lo que estamos escribiendo
- Un formater se encarga de la organizacion y la presentacion osea de como lo estamos escribiendo.

Muchas veces chocan en muchas partes.

## Para instalar el Linter

```bash
npm i eslint -D
```

solo nos sirve en desarrollo cuando el codigo estara en produccion no hara nada
ahora necesitamos una archivo de configuracion con una serie de reglas que va tener que utilizar

```bash
npx eslint --init
```

con esto nos saldra un asistente que nos ira preguntando ciertas cosas para ver que vamos a configurar:
al cual las configuraciones seran:

```bash
√ How would you like to use ESLint? · style
√ What type of modules does your project use? · esm
√ Which framework does your project use? · react
√ Does your project use TypeScript? · No / Yes
√ Where does your code run? · browser
√ How would you like to define a style for your project? · guide
√ Which style guide do you want to follow? · standard
√ What format do you want your config file to be in? · JavaScript
Checking peerDependencies of eslint-config-standard@latest
The config that you've selected requires the following dependencies:

eslint-plugin-react@latest eslint-config-standard@latest eslint@^8.0.1 eslint-plugin-import@^2.25.2 eslint-plugin-n@^15.0.0 || ^16.0.0  eslint-plugin-promise@^6.0.0
√ Would you like to install them now? · No / Yes
√ Which package manager do you want to use? · npm
```

Y ya con esto nos creara un archivo _.eslintrc.cjs_

> De momento si vamos a nuestros archivos no funcionara ya que tenemos que configurar mas cosas

Lo que podriamos hacer para que valide nuestro codigo es en la consola digitar

```bash
npx eslint src/main.jsx
```

con esto buscara los problemas dentro del archivo main.jsx

Y nos indicara que tiene errores que puede arreglar si ejecutamos el siguiente comando

```bash
npx eslint src/main.jsx --fix
```

el archivo estaba:

```javascript
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App.jsx';
import './index.css';

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>

    <App />

  </React.StrictMode>
)
```

y luego

```javascript
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App.jsx'
import './index.css'

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>

    <App />

  </React.StrictMode>
)
```

Para configurar con el editor en este caso VScode instalar _ESLint_ la que esta verificada por Microsoft

Y ahora ya el editor de codigo nos ayudara visualmente a ver los problemas y arreglar los mismos

Y habra reglas que podremos desactivar como el echo de usando Vite no es necesario importar React

Como la desabilitamos:

Ir al archivo .eslintrc.cjs y configurar con los tres valores posibles que son:

1. warn
2. error
3. off

```bash
rules: {
    'react/react-in-jsx-scope':'error'
}
```

o bien en la seccion de:

```bash
extends: [
    'standard',
    'plugin:react/recommended',
    'plugin:react/jsx-runtime'
  ],
```

Y ya podriamos estar añadiendo las reglas que queramos configurar para el desarrollo en equipo

otra regla seria para deshabilitar que nos muertre que las varibles que no estan usadas muestre con erroro eso con:

```bash
rules: {
    'no-unused-vars':'warn'
}
```

Ahora otra de las herramientas que tenemos es prettier para formatear el codigo:

```bash
npm i prettier -D
```

y luego

```bash
npx prettier src/main.jsx
```

y ese archivo nos lo formateara y lo devolvera en consola pero lo que se puede hacer es

```bash
npx prettier src/main.jsx --write
```

para que escriba el archivo

> Ahora como configuramos prettier para que no coloque espacios saltos de linea puntos y coma etc.

creamos un archivo _.prettierrc_

```
trailingComma: "es5"
tabWidth: 2
semi: false
singleQuote: true
jsxSingleQuote: true
arrowParens: "avoid"
```

Una vez echo eso

1. ahora tambien debemos instalar la extension de _prettier_ en VSCode
2. Click derecho a cualquier archivo _format document_ escoger prettier
3. En los ajustes de vscode habilitar el check que dice _format on save_

Ahora que pasaria si en prettier colocamos que si aceptamos ; y en eslint no

Para ello debemos instalar un paquete

```bash
npm i -D eslint-config-prettier
```

Y luego en el archivo _.eslintrc.cjs_

```bash
extends: [
    'standard',
    'plugin:react/recommended',
    'plugin:react/jsx-runtime',
    'eslint-config-prettier' <- añadimos esta linea
  ],
```

Y con esto se quedara con las reglas de prettier por encima de las de eslint.
Es decir donde exista alguna confusion tomara esa configuracion.
De esta manera estaria configurado

## Lo ultimo que tendriamos que hacer es:

Dejar unos comandos configurados en el package.json para hacer este binding y formating

Y siempre tenemos que dar esta opcion por terminal
Asi en el _package.json_

```bash
"scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "format": "prettier --write .", <- añadimos esta linea
    "lint": "eslint --fix . --ext .js,.jsx,.ts,-tsx" <- añadimos esta linea
  },
```

y para que al ejecutar esa linea no nos toque la carpeta dist que es la de produccion creamos un archivo _.prettierignore_

```
dist
package-lock.json
```

y _.eslintignore_

```
dist
```

y ejecutamos los comandos

```bash
npm run format
npm run lint
```

Por ultimo nos saldra un warning por react si queremos arreglar eso en el archivo _eslintrc.cjs_
colocamos

```bash
settings:{
    react:{
      version:'detect'
    }
  },
```
