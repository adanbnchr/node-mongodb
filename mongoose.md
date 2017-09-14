# Acceso a MongoDB mediante ODM



## Objetivos
- Saber realizar operaciones básicas
  - CRUD
- Entender el concepto de ODM
- Familiarizarnos con la documentación y saber usar la API del driver
- Apreciar "las vitaminas" que aporta Mongoose



## Referencias
- [Web Mongoose](http://mongoosejs.com/docs/guide.html)




## Crear el proyecto
- Creamos la carpeta para nuestro proyecto llamada mongodbDriver
- Inicializamos nuestro proyecto mediante el comando **npm init**
- Instalamos nuestras dependencias (mongodb y eslint)

``` 
mkdir mongodbDriver
cd mongodbDriver
npm init
npm install mongodb --save
npm install eslint --save-dev
```



## Configurar eslint

```
./node_modules/.bin/eslint --init
? How would you like to configure ESLint? Answer questions about your style
? Are you using ECMAScript 6 features? Yes
? Are you using ES6 modules? Yes
? Where will your code run? Node
? Do you use JSX? No
? What style of indentation do you use? Spaces
? What quotes do you use for strings? Single
? What line endings do you use? Windows
? Do you require semicolons? No
? What format do you want your config file to be in? JSON
Successfully created .eslintrc.json file in ....
```






## Proyecto MongoDB
- Creamos un nuevo proyecto

```
mkdir mongoose
cd mongoose
npm init
mkdir src
touch src/index.js
```

- Todo nuestro código irá en la carpeta src
- index.js será el fichero que arrancaremos para empezar

- En el fichero package.json añadiremos dentro de scripts:

```
    "start": "node src/index.js"
```

- Ahora podremos ejecutar nuestro script mediante

```
npm start
```

- Instalamos [nodemon](https://nodemon.io/) (detecta cambios en nuestra app y la reinicia)

- Modificamos nuestro script anterior para que ejecute nodemon en vez de node.
- Probamos el funcionamiento de nodemon.



## Conexión a base de datos

- Cargamos el módulo mongoose

```
var mongoose = require('mongoose');
```

- Nos conectamos 

```
mongoose.connect('mongodb://localhost/database');
```


- [La URI puede ser más compleja](https://docs.mongodb.com/manual/reference/connection-string/):

```
mongodb://[username:password@]host1[:port1][,host2[:port2],...[,hostN[:portN]]][/[database][?options]]
```

    - Varios hosts para conectarse a una replica set
    - Puerto, por defecto el 27107

- El método connect puede aceptar un objeto de opciones que tiene preferencia sobre las opciones que vengan en la URI
```
var options = {
  db: { native_parser: true },
  server: { poolSize: 5 },
  replset: { rs_name: 'myReplicaSetName' },
  user: 'myUserName',
  pass: 'myPassword'
}
mongoose.connect(uri, options);
```


## Debug
```
mongoose.set('debug', true)
```

- Otra opción: 
```
mongoose.connect(MONGODB_URI);

// we simplify this
// mongoose.connection.on('error', handleError);

var db = mongoose.connection;
 
db.on('error', function(err){
    console.log('connection error', err);
});
 
db.once('open', function(){
    console.log('Connection to DB successful');
});

```

## Modelos
- Nuestros objetos se basarán en modelos

```
var Cat = mongoose.model('Cat', { name: String });
```
- Un modelo se asocia con una colección en MongoDB
    - Primer parámetro
    - La colección será en plural
- Un modelo se rige por un esquema
    - Segundo parámetro

    


## Ejemplo de modelo

- Un documento es una instancia de un modelo

```
var Cat = mongoose.model('Cat', { name: String });

var kitty = new Cat({ name: 'Zildjian' });
kitty.save(function (err) {
  if (err) {
    console.log(err);
  } else {
    console.log('meow');
  }
});
```

## Esquemas
- Sirven para definir:
    - La estructura del documento
    - El tipo de datos (SchemaType)
    - Métodos de instancia
    - Métodos estáticos (del modelo)
    - Índices compuestos
    - Middlewares


## Patrón de diseño
- Un esquema por cada modelo
- Un modelo en cada fichero
- Se hace el export exclusivamente del modelo
- Se obtiene el documento (instancia del modelo) desde donde nos interes```e

## Ejemplo

```
var mongoose = require('mongoose');
var Schema = mongoose.Schema;

var blogSchema = new Schema({
  title:  String,
  author: String,
  body:   String,
  comments: [{ body: String, date: Date }],
  date: { type: Date, default: Date.now },
  hidden: Boolean,
  meta: {
    votes: Number,
    favs:  Number
  }
});


var Blog = mongoose.model('Blog', blogSchema);
```

## Creación de un modelo
```
const UserSchema = new Schema({
   firstName: String,
   lastName: String,
   username: {
       type: String,
       index: {
           unique: true
       }
   },
   password: {
       type: String,
       required: true,
       match: /(?=.*[a-zA-Z])(?=.*[0-9]+).*/,
       minlength: 12
   },
   email: {
       type: String,
       require: true,
       match: /^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i
   },
   created: {
       type: Date,
       required: true,
       default: new Date()
   }
});
```


## Securizando password
- Al guardar un usuario no guardaremos su contraseña sino un hash de la misma
    - Utilizaremos el [módulo bcrypt](https://www.npmjs.com/package/bcrypt)
    - Los middelware son asíncronos, así que usaremos bcrypt de forma asíncrona
- Una medida de seguridad es que cueste tiempo generar los hashes
    - Definimos un valor de saltRound (ciclos de hashes)
    - Autogeneramos la semilla

```
var bcrypt = require('bcrypt');
const saltRounds = 10;
bcrypt.hash(myPlaintextPassword, saltRounds, function(err, hash) {
  // Store hash in your password DB. 
});
```


## Comparación de contraseñas
- La semilla y las saltRounds quedan incorporadas en la password
    - Si queremos chequear una contraseña no necesitamos ni salt ni saltRound
    - En cualquier momento podemos aumentar la seguridad (saltRound) sin afectar lo anterior.

```
bcrypt.compare(myPlaintextPassword, hash, function(err, res) {
    // res == true 
});
```

- Asociamos un método (comparar con https://medium.com/of-all-things-tech-progress/starting-with-authentication-a-tutorial-with-node-js-and-mongodb-25d524ca0359)

//authenticate input against database
UserSchema.statics.authenticate = function (email, password, cb) {
  User.findOne({ email: email })
    .exec(function (err, user) {
      if (err) {
        return cb(err)
      } else if (!user) {
        var err = new Error('User not found.');
        err.status = 401;
        return cb(err);
      }
      bcrypt.compare(password, user.password, function (err, result) {
        if (result === true) {
          return cb(null, user);
        } else {
          return cb();
        }
      })
    });
}




```
UserSchema.methods.passwordIsValid = function (password) {
   try {
       return bcrypt.compareAsync(password, this.password);
   }
   catch (err) {
       throw err;
   }
};
```


## Incorporar hash a nuestro modelo de usuario

UserSchema.pre('save', function (next) {
  var user = this;
  if (!user.isModified("password")) {
     return next();
  }

  bcrypt.hash(user.password, saltRounds, function (err, hash){
    if (err) {
      return next(err);
    }
    user.password = hash;
    next();
  })# Acceso a MongoDB mediante driver nativo

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





## Conexión a base de datos

- Cargamos el módulo mongoose:

```
var mongoose = require('mongoose');
```
- Nos conectamos 

```
mongoose.connect('mongodb://localhost/database');
```


- [La URI puede ser más compleja](https://docs.mongodb.com/manual/reference/connection-string/):
```
mongodb://[username:password@]host1[:port1][,host2[:port2],...[,hostN[:portN]]][/[database][?options]]
```
    - Varios hosts para conectarse a una replica set
    - Puerto, por defecto el 27107

- El método connect puede aceptar un objeto de opciones que tiene preferencia sobre las opciones que vengan en la URI
```
var options = {
  db: { native_parser: true },
  server: { poolSize: 5 },
  replset: { rs_name: 'myReplicaSetName' },
  user: 'myUserName',
  pass: 'myPassword'
}
mongoose.connect(uri, options);
```


## Debug
```
mongoose.set('debug', true)
```

- Otra opción: 
```
mongoose.connect(MONGODB_URI);

// we simplify this
// mongoose.connection.on('error', handleError);

var db = mongoose.connection;
 
db.on('error', function(err){
    console.log('connection error', err);
});
 
db.once('open', function(){
    console.log('Connection to DB successful');
});

```

## Modelos
- Nuestros objetos se basarán en modelos

```
var Cat = mongoose.model('Cat', { name: String });
```
- Un modelo se asocia con una colección en MongoDB
    - Primer parámetro
    - La colección será en plural
- Un modelo se rige por un esquema
    - Segundo parámetro

    


## Ejemplo de modelo

- Un documento es una instancia de un modelo

```
var Cat = mongoose.model('Cat', { name: String });

var kitty = new Cat({ name: 'Zildjian' });
kitty.save(function (err) {
  if (err) {
    console.log(err);
  } else {
    console.log('meow');
  }
});
```

## Esquemas
- Sirven para definir:
    - La estructura del documento
    - El tipo de datos (SchemaType)
    - Métodos de instancia
    - Métodos estáticos (del modelo)
    - Índices compuestos
    - Middlewares


## Patrón de diseño
- Un esquema por cada modelo
- Un modelo en cada fichero
- Se hace el export exclusivamente del modelo
- Se obtiene el documento (instancia del modelo) desde donde nos interes```e

## Ejemplo

```
var mongoose = require('mongoose');
var Schema = mongoose.Schema;

var blogSchema = new Schema({
  title:  String,
  author: String,
  body:   String,
  comments: [{ body: String, date: Date }],
  date: { type: Date, default: Date.now },
  hidden: Boolean,
  meta: {
    votes: Number,
    favs:  Number
  }
});


var Blog = mongoose.model('Blog', blogSchema);
```

## Creación de un modelo
```
const UserSchema = new Schema({
   firstName: String,
   lastName: String,
   username: {
       type: String,
       index: {
           unique: true
       }
   },
   password: {
       type: String,
       required: true,
       match: /(?=.*[a-zA-Z])(?=.*[0-9]+).*/,
       minlength: 12
   },
   email: {
       type: String,
       require: true,
       match: /^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i
   },
   created: {
       type: Date,
       required: true,
       default: new Date()
   }
});
```


## Securizando password
- Al guardar un usuario no guardaremos su contraseña sino un hash de la misma
    - Utilizaremos el [módulo bcrypt](https://www.npmjs.com/package/bcrypt)
    - Los middelware son asíncronos, así que usaremos bcrypt de forma asíncrona
- Una medida de seguridad es que cueste tiempo generar los hashes
    - Definimos un valor de saltRound (ciclos de hashes)
    - Autogeneramos la semilla

```
var bcrypt = require('bcrypt');
const saltRounds = 10;
bcrypt.hash(myPlaintextPassword, saltRounds, function(err, hash) {
  // Store hash in your password DB. 
});
```


## Comparación de contraseñas
- La semilla y las saltRounds quedan incorporadas en la password
    - Si queremos chequear una contraseña no necesitamos ni salt ni saltRound
    - En cualquier momento podemos aumentar la seguridad (saltRound) sin afectar lo anterior.

```
bcrypt.compare(myPlaintextPassword, hash, function(err, res) {
    // res == true 
});
```

- Asociamos un método (comparar con https://medium.com/of-all-things-tech-progress/starting-with-authentication-a-tutorial-with-node-js-and-mongodb-25d524ca0359)

//authenticate input against database
UserSchema.statics.authenticate = function (email, password, cb) {
  User.findOne({ email: email })
    .exec(function (err, user) {
      if (err) {
        return cb(err)
      } else if (!user) {
        var err = new Error('User not found.');
        err.status = 401;
        return cb(err);
      }
      bcrypt.compare(password, user.password, function (err, result) {
        if (result === true) {
          return cb(null, user);
        } else {
          return cb();
        }
      })
    });
}




```
UserSchema.methods.passwordIsValid = function (password) {
   try {
       return bcrypt.compareAsync(password, this.password);
   }
   catch (err) {
       throw err;
   }
};
```


## Incorporar hash a nuestro modelo de usuario

UserSchema.pre('save', function (next) {
  var user = this;
  if (!user.isModified("password")) {
     return next();
  }

  bcrypt.hash(user.password, saltRounds, function (err, hash){
    if (err) {
      return next(err);
    }
    user.password = hash;
    next();
  })
});


## Test inserción usuario

var testdata = new  User({
   name: "admin",
   password: "test123"
});
 
testdata.save(function(err, data){
    if(err) console.log(err);
    else console.log ('Sucess:' , data);
});


## Test de nuestros modelos de  MongoDB
- Un fallo en un modelo puede ser un error grave en la aplicación
- Hacer un test de modelos no es fácil
    - Antes de hacer cada test tenemos que tener nuestra base de datos en un estado "conocido"
    - Necesitamos una base de datos "específica para los tests": usaremos [CI con Travis](https://docs.travis-ci.com/user/database-setup/#MongoDB)
- Lo ideal sería no conectarnos a una base de datos:
    - Los test serían muy lentos
    - Más dificiles de preparar, al tenernos que preocupar del estado de la base de datos
- Utilizaremos mocha para hacer los tests
Mocha te permite utilizar cualquier librería de afirmaciones como por ejemplo should.js, expect.js, chai y better-assert lo que hace que sea más flexible a los gustos de los programadores por una librería en particular


https://codeutopia.net/blog/2016/06/10/mongoose-models-and-unit-tests-the-definitive-guide/
});


## Test inserción usuario

var testdata = new  User({
   name: "admin",
   password: "test123"
});
 
testdata.save(function(err, data){
    if(err) console.log(err);
    else console.log ('Sucess:' , data);
});


## Test de nuestros modelos de  MongoDB
- Un fallo en un modelo puede ser un error grave en la aplicación
- Hacer un test de modelos no es fácil
    - Antes de hacer cada test tenemos que tener nuestra base de datos en un estado "conocido"
    - Necesitamos una base de datos "específica para los tests": usaremos [CI con Travis](https://docs.travis-ci.com/user/database-setup/#MongoDB)
- Lo ideal sería no conectarnos a una base de datos:
    - Los test serían muy lentos
    - Más dificiles de preparar, al tenernos que preocupar del estado de la base de datos
- Utilizaremos mocha para hacer los tests
Mocha te permite utilizar cualquier librería de afirmaciones como por ejemplo should.js, expect.js, chai y better-assert lo que hace que sea más flexible a los gustos de los programadores por una librería en particular


https://codeutopia.net/blog/2016/06/10/mongoose-models-and-unit-tests-the-definitive-guide/