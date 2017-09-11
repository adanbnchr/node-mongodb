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
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install build-essential
```

* En Windows [descargando el paquete msi](https://github.com/coreybutler/nvm-windows)

* Comprobamos que esté instalado:

```
npm -v
node -v
```



 

## Editor de código
* Utilizaremos [Visual Code Editor](https://code.visualstudio.com/)
* Es un producto open source de Microsoft realizado mediante node.js (electron)
* Uso muy similar a Sublime Text
* Tiene un excepcional debugger para node.js


## Linter para JavaScript
- Utilizaremos eslint (el más habitual)
- Instalaremos la extensión eslint dentro de Visual Code Editor
- Utilizaremos eslint como dependencia de desarrollo dentro de nuestros proyectos









