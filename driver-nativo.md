# Acceso a MongoDB mediante driver nativo

## Cargar datos de ejemplo
- MongoDB proporciona unos [datos de ejemplo](https://docs.mongodb.com/getting-started/shell/import-data/)
- Obtener datos de: https://raw.githubusercontent.com/mongodb/docs-assets/primer-dataset/primer-dataset.json
- Importar datos:
    - Se pueden añadir parámetros --host y --port
    ```
    mongoimport --db test --collection restaurants --drop --file ~/downloads/primer-dataset.json
    ```

## Proyecto con driver mongodb
- Creamos la estructura de ficheros e instalamos las dependencias
``` 
mkdir proyecto1
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

