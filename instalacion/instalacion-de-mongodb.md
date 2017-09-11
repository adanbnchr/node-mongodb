## Instalación de MongoDB en Linux

* [Instrucciones de instalación en Ubuntu](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/)

* Para 16.04
  - Se importa la clave pública
  - Se añade el repositorio
  - Finalmente se instala


    ```
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
    echo "deb [ arch=amd64,arm64 ] http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
    sudo apt-get update
    sudo apt-get install -y mongodb-org
    ```

* El servicio se levanta como otros servicios de Linux:

  ```
  sudo service mongod start
  ```

* Y para entrar a su consola, mediante **mongo**, o mediante algún gui como por ejemplo [Robomongo](https://robomongo.org/), que también podemos instalar desde su web.


## Instalación de MongoDB en Windows
- [Descargamos el fichero msi correspondiente](https://www.mongodb.com/download-center#community)

- Abrimos un terminal como administrador y creamos los directorios de datos y logs:
```
mkdir c:\data\db
mkdir c:\data\log
```

- Creamos el fichero de configuración *
C:\Program Files\MongoDB\Server\3.4\mongod.cfg* con el siguiente contenido:
```
systemLog:
    destination: file
    path: c:\data\log\mongod.log
storage:
    dbPath: c:\data\db
```

- Instalamos el servicio de Mongodb
```
"C:\Program Files\MongoDB\Server\3.4\bin\mongod.exe" --config "C:\Program Files\MongoDB\Server\3.4\mongod.cfg" --install
```
- Arrancamos el servicio:
```
net start MongoDB
```


## Clientes de MongoDB
- Podemos utilizar mongo.exe (consola de JavaScript)

- Podemos utilizar algún gui como por ejemplo [Robo3T](https://robomongo.org/), hasta hace poco llamado Robomongo.

- Instalar Robo3T desde su web y prueba la conexión con MongoDB.