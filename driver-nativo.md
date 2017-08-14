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
- Creamos un fichero de configuración de eslint desde la paleta de comandos (CTRL/COMMAND + MAYS + p)
- Cambiamos la configuración del fichero .estlintrc.json exigiendo por ejemplo que las líneas no acaben en punto y coma:
```
  "semi": ["error", "never"]
```
- Observamos los errores en el editor
- Desde la paleta de comandos le pedimos a eslint que arregle los fallos :-)


## Creamos una colección con validación:
```
var createValidated = function(db, callback) {
  db.createCollection("contacts", 
	  {
	   'validator': { 
        '$or':
	        [
	          { 'phone': { '$type': "string" } },
	          { 'email': { '$regex': /@mongodb\.com$/ } },
	          { 'status': { '$in': [ "Unknown", "Incomplete" ] } }
	        ],
        name: {$type: "string"}, 
        age: {$type: "int", $gte: 18 }
      }  
    },
    function(err, results) {
      if (err) console.log(err);
      else console.log("Collection created.");
      callback(err, results);
    }
  );
};
```

- ¿Sabrías integrar esto en el código anterior?
- Recuerda que node.js 
    - funciona de modo asíncrono
    - hay que utilizar las funciones de callback
    - ¡ojo donde cierras la base de datos, se puede quedar la colección sin hacer!

```
var MongoClient = require('mongodb').MongoClient;

// Connection URL
var url = 'mongodb://localhost:27017/test';

var createValidated = function(db, callback) {
  db.createCollection("contacts", 
	  {
	   'validator': { 
        '$or':
	        [
	          { 'phone': { '$type': "string" } },
	          { 'email': { '$regex': /@mongodb\.com$/ } },
	          { 'status': { '$in': [ "Unknown", "Incomplete" ] } }
	        ],
        name: {$type: "string"}, 
        age: {$type: "int", $gte: 18 }
      }  
    },
    function(err, results) {
      if (err) console.log(err);
      else console.log("Collection created.");
      callback(err, results);
    }
  );
};

// Use connect method to connect to the server
MongoClient.connect(url, function(err, db) {
  if (err) console.log(err.message);
  else {
    console.log("Connected successfully to server");
    createValidated(db, function () {
      db.close();
    })
  }
});
```

## Insertamos registros






## Búscamos registros
```
var MongoClient = require('mongodb').MongoClient;

// Connection URL
var url = 'mongodb://localhost:27017/test';

var createValidated = function(db, callback) {
  db.createCollection("contacts", 
	  {
	   'validator': { 
        '$or':
	        [
	          { 'phone': { '$type': "string" } },
	          { 'email': { '$regex': /@mongodb\.com$/ } },
	          { 'status': { '$in': [ "Unknown", "Incomplete" ] } }
	        ],
        name: {$type: "string"}, 
        age: {$type: "int", $gte: 18 }
      }  
    },
    function(err, results) {
      if (err) console.log(err);
      else console.log("Collection created.");
      callback(err, results);
    }
  );
};

var findDocuments = function(db) {
  // Get the documents collection
  var collection = db.collection( 'restaurants' );
  // Find some documents
  collection.find({ 'cuisine' : 'Brazilian' }, { 'name' : 1, 'cuisine' : 1 }).toArray(function(err, docs) {
    if (err) console.log(err);
    else {
      console.log("Found the following records");
      console.log(docs)
      // callback(null, docs);
    }
  });
}

// Use connect method to connect to the server
MongoClient.connect(url, function(err, db) {
  if (err) console.log(err.message);
  else {
    console.log("Connected successfully to server");
    findDocuments(db);
    createValidated(db, function () {
      db.close();
    })
  }
});
```


## Borramos registros
- Se deja como ejercicio, utilizando el [Manual referencia](http://mongodb.github.io/node-mongodb-native/2.2/)  



## ¿Evitamos el callback hell?
- Vamos a utilizar async/await (ES7)
- Para que nos funcione deberemos utilizar babel-node, o en producción compilarlo.