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

