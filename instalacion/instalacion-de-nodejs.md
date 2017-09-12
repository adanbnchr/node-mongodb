# Instalación y configuración de node.js



## Gestor de versiones

* Permite utiizar varias versiones de node en nuestra máquina (por usuario)

* Los gestores de versiones más habituales son:
  * [nvm](https://github.com/creationix/nvm) para Linux/Mac
  * [nvm-windows](https://github.com/coreybutler/nvm-windows) para Windows



## Control de versiones: ventajas
* Poder cambiar de versión de node de forma transparente
* Evitar tener que hacer *sudo* cuando instalemos paquetes de forma global (Linux y Mac)
    * Los paquetes globales se instalan para un único usuario y version de node
    * Los paquetes globales sirven para cualquier proyecto



## Node mediante nvm (Linux)

* Instalamos nvm \(node virtual manager\)

```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.4/install.sh | bash
```

* Comandos habituales:

```
nvm install 6      # instalamos node v6
nvm ls              # vemos las versiones de node que hay instaladas
nvm use 6           # usamos la versión 6
nvm alias default 6 # usar una versión por defecto
  ```



## Instalación de node en Linux

* La versión que viene en el repositorio suele estar desfasada
  ```
  sudo apt-get purge nodejs npm
  ```
  
* Instalamos una versión actual (6_x, 7_x o 8_x): 
  ```
  curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
  sudo apt install -y nodejs
  sudo apt install build-essential
  ```



## Node mediante nvm-windows
- [Mediante nvm-window](https://github.com/coreybutler/nvm-windows/releases)
  - Descargamos *nvm-setup.zip* (latest release)
  - Se ejecuta y se configura si es necesario según su [wiki de instalación](https://github.com/coreybutler/nvm-windows/wiki)
- Sintaxis similar a la versión de Linux
- **Mucho menos recorrido que la versión de Linux (no lo usaré)**



## Instalación de node en Windows

- [Descargando el paquete msi](https://nodejs.org/es/download/)



## Comprobación de instalación
* Comprobamos que esté instalado:

```
npm -v
node -v
```



## Actualización de npm
* npm se actualiza a si mismo:
```
npm i -g npm
```

* En Windows, la versión 5.4.x da problemas con el paquete *nodemon*, en este caso instalaremos una versión menor:
```
npm i -g npm@5.3.0
```

)