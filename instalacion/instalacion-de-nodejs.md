# Instalación y configuración de node.js



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
  curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.4/install.sh | bash
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


## Instalación de node en Linux

* Si hemos utilizado un gestor de versiones de node, ya habremos instalado node.
* Instalación en Linux: 

  ```
  curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
  sudo apt-get install -y nodejs
  sudo apt-get install build-essential
  ```


## Instalación de node.js en Windows

* [Descargando el paquete msi](https://github.com/coreybutler/nvm-windows)


## Comprobación de instalación
* Comprobamos que esté instalado:

```
npm -v
node -v
```


## Actualización de npm
* Es habitual actualizar la versión de npm (la que viene por defecto empaquetada suele estar desfasada).
```
npm i -g npm
```

* En Windows, la versión 5.4.x da problemas con el paquete *nodemon*, en este caso instalaremos una versión menor:
```
npm i -g npm@5.3.0
```

