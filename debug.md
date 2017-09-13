# Debug




## Programa de ejemplo con node

* Vamos a utilizar un ejemplo muy sencillo que nos sirva para:
* Aprender a hacer debug
* Conocer como funcionan los módulos en nodejs
* Familiarizarnos con el editor de código



## Configuración del linter
- Creamos carpeta para el proyecto y generamos el fichero de configuración del linter
- El JSON tiene intellisense en el editor (YAML no)

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
? What format do you want your config file to be in? JSON
Successfully created .eslintrc.yml file in /Users/juandaniel/Desktop/debug-node
```



## Módulo de ejemplo
* Fichero *suma.js*
  ```
  var suma = function(a, b) {
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
  console.log ('La suma de 3 y 5 es ' + suma(3,5))
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
// import suma from './suma'
let suma = require('./suma.js')
console.log (`La suma de 3 y 5 es ${suma(3,5)}`)
```



## Opciones de debug

* Utilizando la consola
* Utilizando Chrome Developer Tools
* Mediante Visual Studio




## Debug en consola

* Ejecutamos el siguiente comando
  ```
  node debug app.js
  ```
* Podemos incluir puntos de interrrupción añadiendo líneas con la instrucción :
  ```
  debugger;
  ```



## Teclas de acceso rápido  
* **c**: Ir al siguiente breackpoint
* **n**: Ir a la siguiente línea de código
* **s**: Entrar en función:
* **o**: Salir de función:



## Mediante Chrome Developer Tools

* Hay dos opciones dependiendo de la versión de node:
  - Mediante el propio node
    ``` 
    node --inspect app.js
    ```
    - Experimental en v6.x
  - Mediante un paquete adicional, **node-inspector**
    - Deprecated en v7.x




## node --inspect

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

* [Abrimos mejor con Chrome la url *about:inspect*](https://medium.com/@paul_irish/debugging-node-js-nightlies-with-chrome-devtools-7c4a1b95ae27) 
* Pulsando *ESC* tenemos un intérprete de JS para interaccionar con el código





## Debug con node-inspector y Chrome
* Versiones anteriores a node 6.3
* Instalo el paquete node-inspector:
  ```
  npm i -g node-inspector
  ```
* Ejecuto el ejecutable node-debug:
  ```
  node-debug app.js
  ```




## Debug en Visual Studio Code

* En la vista debug creamos un fichero de configuración _launch.json_ pulsando en la ruleta
![](/img/sublime-debug.png)




* Ya podemos hacer debug como en cualquier otro programa:
  * Play para empezar
  * Break points pulsando a la izquierda de la numeración de lineas del código
  * Puedes pulsar con el botón derecho y poner breakpoints condicionales.
  
* En la pestaña de *Debug Console* podemos interaccionar con el fuente.

{% youtube src="https://www.youtube.com/watch?v=hfpkMyvSOp4" %}{% endyoutube %}