# Acceso a MongoDB


## Objetivos
- Ver diferentes métodos de acceso a MongoDB
- Configurar url de acceso a MongoDB
- Realizar un CRUD básico

## Módulos de node para acceso a MongoDB

- Como hemos visto, node es altamente modular
- Eligiremos un módulo para conexión a la base de datos
- [mongodb](https://www.npmjs.com/package/mongodb)
- [mongoose](https://www.npmjs.com/package/mongoose)

## ¿Qué es mongodb?
- [mongodb](https://www.npmjs.com/package/mongodb)
    - Es un módulo de node.js
    - Es el driver oficial para acceso a MongoDB
    - Es la forma más básica de acceso a una base de datos MongoDB desde Node.js


## ¿Qué es mongoose?
- [mongoose](https://www.npmjs.com/package/mongoose)
    - Es un módulo de node.js
    - Es un ODM (Object Document Mapper)

## Ventajas de usar mongodb
- Más sencillo de aprender

## Desventajas de usar mongodb
- Es más propenso a errores
- Nos da menos (aunque con menos curva de aprendizaje)

## Ventajas de usar mongoose
- Un ODM simplifica el código: 
    - Nuestros objetos tendrán por defecto métodos como save o remove (delete es una keyword en JavaScript)
- Permite usar middlewares
    - Son hooks que se ejecutan antes o después (pre o post) de determinador eventos (validate, save, remove...)
- Permite validar los documentos antes de guardarlos
    - La propia validación es un middleware previo al evento save (presente en mongodb nativo desde versión 3.2 Diciembre 2015)
- Resumiendo, más productivo

## Desventajas de usar mongoose
- Mayor curva de aprendizaje


## ¿Qué vamos a utilizar?
- Utilizaremos mongodb para ver las operaciones básicas [siguiendo la documentación](http://mongodb.github.io/node-mongodb-native/2.2/)
- Posteriormente utilizaremos mongoose marcando sus ventajas en un ejemplo típico y habitual, el login de usuarios


