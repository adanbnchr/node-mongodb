# Acceso a MongoDB



## Módulos en node para acceso a MongoDB

- Como hemos visto, node es altamente modular
- Existen dos módulos para conexión a MongoDB
    - [mongodb](https://www.npmjs.com/package/mongodb)
    - [mongoose](https://www.npmjs.com/package/mongoose)



## ¿Qué es [mongodb](https://www.npmjs.com/package/mongodb)?
- Es un módulo de node.js (además de una base de datos)
- Es el driver oficial para acceso a MongoDB
- Es la forma más básica de acceso a una base de datos MongoDB desde Node.js
    - Más sencillo de aprender
    - Menos productivo
    - Desde la versión 3.2 (diciembre de 2015) tiene validación



## ¿Qué es [mongoose](https://www.npmjs.com/package/mongoose)?
- Es un módulo de node.js
- Es un ODM (Object Document Mapper)
- La forma más productiva (y algo más compleja) de trabajar con MongoDB en node.



## Ventajas de usar mongoose
- Un ODM simplifica el código: 
    - Nuestros objetos tendrán por defecto métodos como save o remove (delete es una keyword en JavaScript)
```
var miCerveza = new Cerveza({ name: 'Ambar' });
miCerveza.save(function (err, miCerveza) {
    if (err) return console.error(err);
    console.log('Guardada en bbdd' + miCerveza.name);
})
```



- Permite usar middlewares
    - Son hooks que se ejecutan antes o después (pre o post) de determinados eventos (validate, save, remove...)
- Permite validar los documentos antes de guardarlos
    - La propia validación es un middleware previo al evento save 
- Resumiendo, más productivo (con mayor curva de aprendizaje)




## ¿Qué vamos a utilizar?
- Utilizaremos mongodb para ver las operaciones básicas [siguiendo la documentación](http://mongodb.github.io/node-mongodb-native/2.2/)
- Posteriormente utilizaremos mongoose marcando sus ventajas en un ejemplo típico y habitual, el login de usuarios


