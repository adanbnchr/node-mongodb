# Instalación y configuración del software

## nvm

* nvm solo está disponible en Linux/Mac. En Windows se puede usar [nvm-windows](https://github.com/coreybutler/nvm-windows)
* Las instrucciones siguientes son para Linux
* Instalaremos y utilizaremos node vía nvm \(node virtual manager\)
* Esto nos permitirá:
* Poder cambiar de versión de node de forma transparente
* Evitar tener que hacer sudo cuando instalemos paquetes de forma global
  ```
  curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.1/install.sh | bash
  ```

* Instalar una versión de node:

```
nvm install 5.0
```

* Ver las versiones que hay instaladas:

```
nvm ls
```

* Usar una versión en particular:

  ```
  nvm use 5.0
  ```

* Usar una versión en particular siempre que abrimos un shell:

```
nvm alias default 5.0
```

## node

* La otra opción sería instalar directamente node:

```
curl -sL https://deb.nodesource.com/setup_5.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install build-essential
```

* En Windows utilizaremos esta opción [descargando el paquete msi](https://github.com/coreybutler/nvm-windows)

* Comprobamos que esté instalado:

```
npm -v
node -v
```


## Instalación de MongoDB

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


## Editor de código
* Utilizaremos Visual [Code Editor](https://code.visualstudio.com/)
* Es un producto open source de Microsoft realizado mediante node.js (electron)
* Uso muy similar a Sublime Text
* Tiene un excepcional debugger para node.js







