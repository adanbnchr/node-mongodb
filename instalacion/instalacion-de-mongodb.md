## Instalación de MongoDB en Linux

* [Instalaremos primero mongodb](https://docs.mongodb.com/master/tutorial/install-mongodb-on-ubuntu/):

```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
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
- Lo [instalamos y lo configuramos como un servicio](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/) según las instrucciones del sitio web de MongoDB

- * Y para entrar a su consola, mediante **mongo**, o mediante algún gui como por ejemplo [Robomongo](https://robomongo.org/), que también podemos instalar desde su web.
