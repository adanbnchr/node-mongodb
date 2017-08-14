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

``` 
mkdir mongodbDriver
cd mongodbDriver
npm init
npm install mongodb --save
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
    - ojo donde cierras la base de datos, se puede quedar la colección sin hacer

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

## Insertamos un select:
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


## Proyecto MongoDB
- Creamos un nuevo proyecto
```
mkdir mongodbCrud
cd mongodbCrud
npm init
mkdir src
touch src/index.js
```
- Todo nuestro código irá en la carpeta src
- index.js será el fichero que arrancaremos para empezar

```
- En el fichero package.json añadiremos dentro de scripts:
```
    "start": "node src/index.js"
```
- Ahora podremos ejecutar nuestro script mediante

```
npm start
```

- Instalamos nodemon...

