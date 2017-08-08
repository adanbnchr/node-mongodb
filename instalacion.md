# Instalación y configuración del software


## nvm
- Instalaremos y utilizaremos node vía nvm (node virtual manager)
- Esto nos permitirá:
- Poder cambiar de versión de node de forma transparente
- Evitar tener que hacer sudo cuando instalemos paquetes de forma global
```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.1/install.sh | bash
```


- Instalar una versión de node:

```
nvm install 5.0
```
- Ver las versiones que hay instaladas:

```
nvm ls
```

- Usar una versión en particular:
```
nvm use 5.0
```

- Usar una versión en particular siempre que abrimos un shell:

```
nvm alias default 5.0
```


## node
- La otra opción sería instalar directamente node:

```
curl -sL https://deb.nodesource.com/setup_5.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install build-essential
```

- Comprobamos que esté instalado:

```
npm -v
node -v
```

## npm
- Es el gestor de paquetes de node
- **Debemos crear un usuario** en https://www.npmjs.com/
- Podemos buscar los paquetes que nos interese instalar
- Podemos publicar nuestra librería :-)
    

## Configuración de npm
- Cuando creemos un nuevo proyecto nos interesa que genere automaticamente datos como nuestro nombre o email
- Ver [documentación para su configuación](https://docs.npmjs.com/) o mediante consola (```npm --help```)	:
- Mediante ```npm config --help``` vemos los comandos de configuración
- Mediante ```npm config ls -l``` vemos los parámetros de configuración

```
npm set init-author-name pepe
npm set init-author-email pepe@pepe.com
npm set init-author-url http://pepe.com
npm set init-license MIT
npm adduser
```

- Los cambios se guardan en el fichero $HOME/.npmrc
- ```npm adduser``` genera un authtoken = login automático al publicar en el registro de npm

## Versiones en node
- Se utiliza [Semantic Versioning](http://semver.org/)

```
npm set save-exact true
```

- Las versiones tienen el formato MAJOR.MINOR.PATCH
- Cambios de versión:
    - MAJOR: Cambios en compatibilidad de API,
    - MINOR: Se añade funcionalidad. Se mantiene la compatibilidad.
    - PATCH: Se solucionan bug. Se mantiene compatibilidad.

- ¡Puede obligarnos a cambiar el MAJOR muy a menudo! 



## git

- Instalación de git:

```
sudo apt-get update
sudo apt-get install git
```

- Configuración necesaria para cada commit que haga:

```
git config --global user.name "Your Name"
git config --global user.email "youremail@domain.com"
```

- Opcionalmente el editor (si no me gusta el que hay por defecto):

```
git config --global core.editor vi
```


### Configuración de git

- Git tiene 3 niveles de configuración, cada nivel sobreescribe el anterior:
- Para todos los usuarios: */etc/gitconfig*
- Para un usuario: *~/.gitconfig* (opción --global)
- Para un repositorio: *.git/config*

- Para ver los parámetros configurados:

```
git config --list
```
- Viene bien tener una [chuleta de comandos de Git](https://services.github.com/kit/downloads/github-git-cheat-sheet.pdf)


### Configuración de GitHub
- Nos registramos en [Github](https://github.com/)
- Accedemos a nuestra cuenta
- Vamos a los settings y asociamos una ssh-key
- Evitaremos introducir usuario/contraseña en cada *git push*
- Como creamos nuestra ssh-key:

```
ssh-keygen
```
- Copiaremos el contenido de *~/.ssh/id_rsa.pub* a una nueva clave ssh en GitHub


## Instalación de zsh

- Algunos prefieren fish
- Otros son fieles a bash

```
sudo apt-get install zsh
chsh -s $(which zsh)
```
- Instalo [oh-my-zsh](http://ohmyz.sh/)
- Añado el plugin para nvm y git

## Instalación de MongoDB
- [Instalaremos primero mongodb](https://docs.mongodb.com/master/tutorial/install-mongodb-on-ubuntu/):

```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
sudo apt-get update
sudo apt-get install -y mongodb-org
```

- El servicio se levanta como otros servicios de Linux:
```
sudo service mongod start
```

- Y para entrar a su consola, mediante **mongo**, o mediante algún gui como por ejemplo [Robomongo](https://robomongo.org/), que también podemos instalar desde su web.

