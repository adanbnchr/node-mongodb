# Acceso a MongoDB mediante driver nativo

## Objetivos
- Saber realizar operaciones básicas
  - CRUD
- Familiarizarnos con la documentación y saber usar la API del driver
- Trabajar de forma correcta con el modelo asíncrono de node.js


## Referencias
- [Web MongoDB Node.JS driver](http://mongodb.github.io/node-mongodb-native/)
- [Manual referencia versión 2.2](http://mongodb.github.io/node-mongodb-native/2.2/)  
- La documentación que hay a continuación está pensada para Linux/Mac. Los cambios para Windows son mínimos y evidentes.


## Crear el proyecto
- Visual Code Editor tiene una consola integrada que nos puede ayudar
- Creamos la carpeta para nuestro proyecto llamada mongodbDriver
- Inicializamos nuestro proyecto mediante el comando **npm init**
- Instalamos nuestras dependencias (mongodb)
- Instalamos eslint para controlar fallos de sintáxis

``` 
mkdir mongodbDriver
cd mongodbDriver
npm init
npm install mongodb --save
npm install eslint --save-dev
```


## Probar conexión
- Creamos un fichero de conexión (app.js):

  ```
  var MongoClient = require('mongodb').MongoClient;
  
  // Connection URL
  var url = 'mongodb://localhost:27017/test';
  
  // Use connect method to connect to the server
  MongoClient.connect(url, function(err, db) {
    if (err) console.log(err.message);
    else {
      console.log("Connected successfully to server");
      db.close();
    }
  });
  ```
- Probamos que funcione:

  ```
  node app.js
  ```
  
- [Modificamos la url si fuera necesario](http://mongodb.github.io/node-mongodb-native/2.2/tutorials/connect/)


## Uso de eslint
- Creamos un fichero de configuración de eslint desde la paleta de comandos (*CTRL/COMMAND + MAYS + P*)
- Cambiamos la configuración del fichero *.estlintrc.json* exigiendo por ejemplo que las líneas no acaben en punto y coma:
```
  "semi": ["error", "never"]
```
- Observamos los errores en el editor
- Desde la paleta de comandos le pedimos a eslint que arregle los fallos :-)


## Creamos una colección con validación:
- [Siguiendo la documentación de nuestra API](http://mongodb.github.io/node-mongodb-native/2.2/api/Db.html#createCollection), crearemos una colección con validación:

```
var createValidated = function (db, callback) {
  db.createCollection("contactos",
    {
      'validator': {
        '$or':
        [
          { 'telefono': { '$type': "string" } },
          { 'email': { '$regex': /@mongodb\.com$/ } },
          { 'estadoCivil': { '$in': ["Soltero", "Casado"] } }
        ],
        nombre: { $type: "string" },
        edad: { $type: "int", $gte: 18 }
      }
    },
    function (err, results) {
      if (err) console.log(err)
      else console.log("Colección creada")
      callback(err, results)
    }
  )
}



```

- ¿Sabrías integrar esto en el código del fichero app.js?
- Recuerda que node.js 
    - funciona de modo asíncrono
    - hay que utilizar las funciones de callback
    - ¡ojo donde cierras la base de datos, se puede quedar la colección sin hacer!

```
var MongoClient = require('mongodb').MongoClient
// Connection URL
var url = 'mongodb://localhost:27017/test'
// Use connect method to connect to the server
MongoClient.connect(url, function (err, db) {
  if (err) console.log(err.message)
  else {
    console.log("Connected successfully to server")
    
    createValidated(db, function(){
      db.close()
    })
  }
})

var createValidated = function (db, callback) {
  db.createCollection("contactos",
    {
      'validator': {
        '$or':
        [
          { 'telefono': { '$type': "string" } },
          { 'email': { '$regex': /@mongodb\.com$/ } },
          { 'estadoCivil': { '$in': ["Soltero", "Casado"] } }
        ],
        nombre: { $type: "string" },
        edad: { $type: "int", $gte: 18 }
      }
    },
    function (err, results) {
      if (err) console.log(err)
      else console.log("Colección creada")
      callback(err, results)
    }
  )
}

```

- Comprobamos que la colección se inserta correctamente:
  - Recoger un error por consola
  - Comprobar en Robo3T:
  ```
  db.getCollectionInfos()
  ```
  - Ojo, si la colección ya está creada no se modifica 

## Insertamos registros

- Queremos que a la vez que se crea la colección, se inserten unos registros de ejemplo.

- Datos de ejemplo:
```
const contactos = [
  {
    nombre: 'pepe',
    edad: 20,
    estadoCivil: 'Soltero',
    telefono: '444444444'
  },
  {
    nombre: 'juan',
    edad: 40,
    estadoCivil: 'Casado',
    email: 'juan@midominio.com',
  },
  {
    nombre: 'marta',
    edad: 30,
    estadoCivil: 'Soltero',
    telefono: '999999999'
  }
]
```
- Función para insertar contactos:
```
var insertarContactos = function (db, contactos, callback) {
  // Obtenemos la colección
  var collection = db.collection('contactos')
  // Insertamos los documentos
  collection.insertMany(contactos, function (err, result) {
    console.log('Insertados los contactos')
    callback(result)
  })
}
```
- Intenta ingegrarlo y probar que funcione. Podría quedar así:
  
  ```
  var MongoClient = require('mongodb').MongoClient
  // Connection URL
  var url = 'mongodb://localhost:27017/test'
  // Use connect method to connect to the server
  MongoClient.connect(url, function (err, db) {
    if (err) console.log(err.message)
    else {
      console.log("Connected successfully to server")
      createValidated(db, function () {
        insertarContactos(db, contactos, function () {
          db.close()
        })
      })
    }
  })
  
  var createValidated = function (db, callback) {
    db.createCollection("contactos",
      {
        'validator': {
          '$or':
          [
            { 'telefono': { '$type': "string" } },
            { 'email': { '$regex': /@midominio\.com$/ } }
          ],
          nombre: { $type: "string" },
          edad: { $type: "int", $gte: 18 },
          estadoCivil: { '$in': ["Soltero", "Casado"] }
        }
      },
      function (err, results) {
        if (err) console.log(err)
        else console.log("Colección creada")
        callback(err, results)
      }
    )
  }
  
  const contactos = [
    {
      nombre: 'pepe',
      edad: 20,
      estadoCivil: 'Soltero',
      telefono: '444444444'
    },
    {
      nombre: 'juan',
      edad: 40,
      estadoCivil: 'Casado',
      email: 'juan@midominio.com',
    },
    {
      nombre: 'marta',
      edad: 30,
      estadoCivil: 'Soltero',
      telefono: '999999999'
    }
  ]
  
  var insertarContactos = function (db, contactos, callback) {
    // Obtenemos la colección
    var collection = db.collection('contactos')
    // Insertamos los documentos
    collection.insertMany(contactos, function (err, result) {
      console.log('Insertados los contactos')
      callback(result)
    })
  }
  ```


## Búscamos registros
- Se utiliza el método find del objeto collection:

  ```
  var encontrarContactosPorNombre = function (db, nombre, callback) {
    var collection = db.collection('contactos')
    collection.find({ 'nombre': nombre }, { 'nombre': 1, 'edad': 1 }).toArray(function (err, docs) {
      if (err) console.log(err)
      else {
        console.log("Encontrados los siguientes contactos")
        console.log(docs)
        callback(null, docs)
      }
    })
  }
  ```


## Borramos registros
- Se deja como ejercicio, utilizando el [Manual referencia](http://mongodb.github.io/node-mongodb-native/2.2/)  


## Estructura acceso a base de datos en una aplicación
- Para cada operación en la base de datos necesitaremos ejecutar cierto método  del objeto db.
- El método [connect](http://mongodb.github.io/node-mongodb-native/2.2/api/MongoClient.html#connect) no es [singleton](https://es.wikipedia.org/wiki/Singleton)
- La conexión a base de datos es un proceso costoso (asíncrono) y lo suyo sería hacerlo una sola vez al arrancar el sistema, no por cada operación sobre la base de datos. 
- Si no utilizamos el parámetro de callback, el método connect devuelve una promesa.


## Módulo de conexión a base de datos

```
// bbdd.js
var MongoClient = require('mongodb').MongoClient;
let connection = null;

module.exports.connect = () => new Promise((resolve, reject) => {
    MongoClient.connect(url, option, function(err, db) {
        if (err) { reject(err); return; };
        resolve(db);
        connection = db;
    });
});

module.exports.get = () => {
    if(!connection) {
        throw new Error('Call connect first!');
    }
    return connection;
}
```

## Estructura aplicación
- Cargamos el módulo de base de datos y si todo va bien, arrancamos el resto de la aplicación

```
  // sería nuestro fichero index.js o app.js
  const db = require('./bbdd');
  db.connect()
      .then(() => console.log('database connected'))
      .then(() => bootMyApplication())
      .catch((e) => {
          console.error(e);
          // Always hard exit on a database connection error
          process.exit(1);
      });
 ```

- Cualquier módulo que acceda a la base de datos:

  ```
  const db = require('./bbdd');
  db.get().find(...)...
  ```


## ¿Evitamos el callback hell?
- Vamos a utilizar async/await (ES7)
- Para que nos funcione deberemos utilizar babel-node, o en producción compilarlo.