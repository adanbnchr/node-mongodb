# Debug en node.js


## Opciones de debug
- Mediante Visual Studio
- Utilizando la consola

## Programa de ejemplo
- Vamos a utilizar un ejemplo muy sencillo que nos sirva para:
 - Aprender a hacer debug
 - Conocer como funcionan los módulos en nodejs
 - Familiarizarnos con el editor de código
 
- Fichero suma.js
```
 let suma = function(a, b) {
    return a+b;
}
module.exports = suma; 
```
- La función suma la queremos utilizar desde nuestro programa, por eso lleva el *module.exports*, que indica las partes de este módulo (fichero) que se pueden exportar (utilizar desde otro fichero).

- Mi aplicación
 - Cargo las librerías que necesito mediante require y las utilizo
 
 ```
 let suma = require('./suma.js');
console.log (suma(3,5));
```

## Debug en Visual Studio Code
- Para hacer debug vamos a la vista de Debug, pulsando el icono correspondiente:
![](/assets/icono-debug.png)

- Para hacer debug necesitamos un fichero launch.json, que es el que tiene la configuración de debug
- Para obtener el fichero pulsamos en la ruleta

![](/assets/sublime-debug.png)