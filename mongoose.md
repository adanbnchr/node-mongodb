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
- Creamos la carpeta para nuestro proyecto llamada odmDriver
- Inicializamos nuestro proyecto mediante el comando **npm init**
- Instalamos nuestras dependencias (mongoose y eslint)

``` 
mkdir odmDriver
cd odmDriver
npm init
npm install mongoose --save
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



## Conexión a base de datos
````
var mongoose = require('mongoose')
var User = require('./User')
mongoose.Promise = global.Promise
mongoose.connect('mongodb://localhost/database', {useMongoClient: true})
var db = mongoose.connection

db.on('error', function(err){
  console.log('connection error', err)
})

db.once('open', function(){
  console.log('Connection to DB successful')
})
```



## Parámetros de conexión 

- [La URI puede ser más compleja](https://docs.mongodb.com/manual/reference/connection-string/):
    - Varios hosts para conectarse a una replica set
    - Puerto, por defecto el 27107

```
mongodb://[username:password@]host1[:port1][,host2[:port2],...[,hostN[:portN]]][/[database][?options]]
```



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



## Probar conexión

- Ejecutamos el fichero anterior mediante node:

  ```
  node app.js
  ```

- ¿Qué mensaje nos muestra la consola?



## Concepto de ODM
- Los documentos se mapean a objetos.
- Nuestros objetos tienen métodos como save, remote, update...
- El modelo se encarga de asociar nuestro objeto con la colección en MongoDB



## Sintaxis de un modelo

```
var Cat = mongoose.model('Cat', { name: String });
```

- Un modelo se asocia con una colección en MongoDB
    - La colección será en plural
- Un modelo se rige por un esquema




## ¿Qué es un documento?
- Para MongoDB será un elemento dentro de una colección en la base de datos
- Para Mongoose será la instancia de un modelo

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
- Un modelo y esquema en cada fichero
- Se hace el export exclusivamente del modelo
- Se obtiene el documento (instancia del modelo) desde donde nos interese




## Ejemplo modelo para un Blog

```
var mongoose = require('mongoose')
var Schema = mongoose.Schema

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
})


var Blog = mongoose.model('Blog', blogSchema)
module.exports = Blog
```



## Ejercicio
- Crea un modelo para una tabla de usuarios que cumpla:
  - Campos requeridos
    - *firstName*
    - *lastName*
    - *userName*
    - *email*
    - *password*
  - Campo *gender* podrá ser *Male* o *Female*
  - Campo created con la fecha y hora de registro



## Solución
```
var mongoose = require('mongoose')
var Schema = mongoose.Schema
const userSchema = new Schema({
  firstName: {
    type: String,
    required: true
  },
  lastName: {
    type: String,
    required: true
  },
  userName: {
    type: String,
    required: true
  },
  password: {
    type: String,
    required: true,
    match: /(?=.*[a-zA-Z])(?=.*[0-9]+).*/,
    minlength: 8
  },
  gender: {
    type: String,
    enum: ['Male', 'Female'],
  },
  email: {
    type: String,
    required: true,
    match: /^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i
  },
  created: {
    type: Date,
    default: new Date()
  }
})

var User = mongoose.model('User', userSchema)
module.exports = User
```



## Ejercicio
- Crea un usuario en MongoDB utilizando el modelo anterior
- Los datos del usuario son los siguientes:

````
const newUser = {
  firstName: 'Pedro',
  lastName: 'Pérez',
  password: 'P@ssw0rd',
  userName: 'pedrito',
  gender: 'Male',
  email: 'prueba@prueba.com'
}
```



## Solución

```
var mongoose = require('mongoose')
var User = require('./User')
mongoose.Promise = global.Promise
mongoose.connect('mongodb://localhost/database', {useMongoClient: true})
var db = mongoose.connection

db.on('error', function(err){
  console.log('connection error', err)
})

db.once('open', function(){
  console.log('Connection to DB successful')
})

const newUser = {
  firstName: 'Pedro',
  lastName: 'Pérez',
  password: 'P@ssw0rd',
  userName: 'pedrito',
  gender: 'Male',
  email: 'prueba@prueba.com'
}

var user = new User(newUser)
user.save(function (err) {
  if (err) {
    console.log(err)
  } else {
    console.log('Usuario añadido')
  }
  db.close(function(){
    console.log('Database closed')
  })
})
```



## Campos virtuales
- Vamos a crear un campo que sea el nombre completo del usuario
- Incluso en función de su sexo podríamos anteponer Sr. o Sra.
- [Mira la documentación con más posibilidades](http://mongoosejs.com/docs/advanced_schemas.html)

```
class UserClass {
  // `fullName` becomes a virtual
  get fullName() {
    return `${this.firstName} ${this.lastName}`
  }
}
userSchema.loadClass(UserClass)
```



## Securizando password
- Al guardar un usuario no guardaremos su contraseña sino un hash de la misma
    - Utilizaremos el [módulo bcryptjs](https://www.npmjs.com/package/bcryptjs)
    - Los middelware son asíncronos, así que usaremos bcrypt de forma asíncrona




- Una medida de seguridad es que cueste tiempo generar los hashes
    - Definimos un valor de saltRound (ciclos de hashes)
    - Autogeneramos la semilla

```
var bcrypt = require('bcryptjs');
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



## Encriptación de la password
- Nos basaremos en el concepto de [middleware](http://mongoosejs.com/docs/middleware.html)

```
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
})
```



## Comprobación de contraseña
- Utilizar un método estático en el Scheme
- Utilizar la función compare de bcryptjs
- Son los deberes para casa :-)