# Instalación y configuración del software

## Gestor de versiones

* Es habitual utilizar varias versiones de node en nuestra máquina de desarrollo o por cada usuario.
* Esto nos permitirá:
  * Poder cambiar de versión de node de forma transparente
  * Evitar tener que hacer sudo cuando instalemos paquetes de forma global
    * Los paquetes globales se instalan para un único usuario y version de node
    * Los paquetes globales sirven para cualquier proyecto

* Los gestores de versiones más habituales son:
  * [nvm](https://github.com/creationix/nvm) para Linux/Mac
  * [nvm-windows](https://github.com/coreybutler/nvm-windows) para Windows


## Instalación de nvm en Linux

* Instalaremos y utilizaremos node vía nvm \(node virtual manager\)

  ```
  curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.1/install.sh | bash
  ```

  * Instalar una versión de node:

    ```
    nvm install 6
    ```

  * Ver las versiones que hay instaladas:

    ```
    nvm ls
    ```

  * Usar una versión en particular:

    ```
    nvm use 6
    ```
  
  * Usar una versión en particular siempre que abrimos un shell:

    ```
    nvm alias default 6
    ```

## Instalación de node

* Si hemos utilizado un gestor de versiones de node, ya habremos instalado node.
* Instalación en Linux: 

```
curl -sL https://deb.nodesource.com/setup_5.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install build-essential
```

* En Windows [descargando el paquete msi](https://github.com/coreybutler/nvm-windows)

* Comprobamos que esté instalado:

```
npm -v
node -v
```


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
 

## Editor de código
* Utilizaremos Visual [Code Editor](https://code.visualstudio.com/)
* Es un producto open source de Microsoft realizado mediante node.js (electron)
* Uso muy similar a Sublime Text
* Tiene un excepcional debugger para node.js







