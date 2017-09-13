# Debug




## Programa de ejemplo con node

* Vamos a utilizar un ejemplo muy sencillo que nos sirva para:
* Aprender a hacer debug
* Conocer como funcionan los módulos en nodejs
* Familiarizarnos con el editor de código



## Configuración del linter
- Creamos carpeta para el proyecto y generamos el fichero de configuración del linter

```
juandaniel@juanda-portatil:~/Desktop/debug-node|
⇒  eslint --init
? How would you like to configure ESLint? Answer questions about your style
? Are you using ECMAScript 6 features? Yes
? Are you using ES6 modules? Yes
? Where will your code run? Node
? Do you use JSX? No
? What style of indentation do you use? Spaces
? What quotes do you use for strings? Single
? What line endings do you use? Unix
? Do you require semicolons? No
? What format do you want your config file to be in? YAML
Successfully created .eslintrc.yml file in /Users/juandaniel/Desktop/debug-node
```



## Módulo de ejemplo
* Fichero *suma.js*
  ```
  let suma = function(a, b) {
    return a+b
  }
  module.exports = suma
  ```

* **module.exports** indica las partes de este módulo \(fichero\) que se pueden exportar \(utilizar desde otro fichero\).



## Aplicación que usa el módulo
* **require**: para cargar los módulos (librerías) que necesito.

  ```
  // fichero app.js
  let suma = require('./suma.js')
  console.log (suma(3,5))
  ```
* Ejecutamos el código anterior mediante *node app.js*




## ¿Reescribimos en [ES6](http://es6-features.org/#Constants)?
- El código anterior se podría haber escrito utilizando [arrow functions](http://es6-features.org/#ExpressionBodies)
- Sin embargo no podemos usar módulos de ES6 para exportar
```
let suma = (a, b) =>a+b
module.exports = suma
// export default suma :-(
```

- Y lógicamente tampoco para importar:
```
let suma = require('./suma.js')
// import suma from './suma'
```



## Opciones de debug

* Utilizando la consola
* Utilizando Chrome Developer Tools
* Mediante Visual Studioç




## Debug en consola

* Ejecutamos el siguiente comando
  ```
  node debug app.js
  ```
* Podemos incluir puntos de interrrupción añadiendo líneas con la instrucción :
  ```
  debugger;
  ```
* Las teclas de acceso rápido son:
  * Hasta el siguiente breackpoint: c
  * Hasta la siguiente línea de código en el mismo fichero: n
  * Entrar en función: s
  * Salir de función: o




## Mediante Chrome Developer Tools

* Hay dos opciones dependiendo de la versión de node:
  - Mediante el propio node
    ``` 
    node --inspect app.js
    ```
    - Experimental en v6.x
  - Mediante un paquete adicional, **node-inspector**
    - Deprecated en v7.x



## Debug con node y Chrome

* Mediante el comando **node --inspect**

```
⇒  node --inspect app.js

Debugger listening on port 9229.

Warning: This is an experimental feature and could change at any time.

To start debugging, open the following URL in Chrome:

    chrome-devtools://devtools/remote/serve\_file/@60cd6e859b9f557d2312f5bf532f6aec5f284980/inspector.html?experiments=true&v8only=true&ws=127.0.0.1:9229/bb7de882-abe4-4c61-8099-22908239de18
```

* Se suele poner un punto de interrupción al empezar:
```
node --debug-brk --inspect app.js
```

* El enlace que nos proporciona se abre con el navegador Chrome y se puede inspeccionar el código mediante sus herramientas de desarrollo.


## Debug con node-inspector y Chrome

* Instalo el paquete node-inspector:
  ```
  npm i -g node-inspector
  ```
* Ejecuto el ejecutable node-debug:
  ```
  node-debug app.js
  ```




## Debug en Visual Studio Code

* Primero hay que ir a  la vista de debug, pulsando el icono correspondiente:  
  ![](/assets/icono-debug.png)
  
* Para hacer debug necesitamos un fichero de configuración _launch.json_, que se obtiene pulsandos en la ruleta

![](/assets/sublime-debug.png)

* Ya podemos hacer debug como en cualquier otro programa:
  * Play para empezar
  * Break points pulsando a la izquierda de la numeración de lineas del código
  * Puedes pulsar con el botón derecho y poner breakpoints condicionales.

* [Buen tutorial](https://www.youtube.com/watch?v=hfpkMyvSOp4)




