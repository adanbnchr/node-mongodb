# Editor de código



## Instalación
* Utilizaremos [Visual Studio Code](https://code.visualstudio.com/)
* Es un producto open source de Microsoft realizado mediante node.js ([electron](https://electron.atom.io/))
* Uso muy similar a Sublime Text (empezando con la paleta de comandos CTRL + ALT + P)
* Tiene un debugger para node.js muy bueno.



## Extensiones
- La base de la personalización son las **extensiones**
* Se pueden ver e instalar desde el propio software o desde su [market](https://marketplace.visualstudio.com)
    - Las instrucciones de uso aparecen en el propio editor
    - A menudo es necesario recargar la ventana para poner usarlas
    - En función de las instaladas, el editor hace recomendaciones



## Extensiones preinstaladas
- Las extensiones más habituales ya vienen preinstaladas
    - Debugger
    - Emmet
    - Terminal
    - Control de versiones
    - Linter para css
- No lleva linter de JS (hay muchos para elegir y es dificil acertar para todos los usuarios)



## Settings

- Cambian la configuración por defecto
    - Del editor de código
    - De las extensiones instaladas
- Se definen varios ámbitos, en órden de prioridad inversa:
    * Settings por defecto (se pueden copiar, no modificar)
    * User Settings (sobreescriben los anteriores)
    * Workspace Settings para nuestras especificaciones en un proyecto en particular



    
## Cambiar settings
- En node.js se anidan muy a menudo funciones.
- Resulta útil indentar el código solo con dos espacios 
- Cambia el el tabulador de 4 a 2 espacios desde tus settings de usuario:
```
"editor.tabSize": 2
``` 



## Live Server
* Instalaremos por ej. live server
    - Hace falta recargar
    - Detecta cambios en vivo    



## Autocompletado de ficheros
* Para autocompletar ficheros utilizaremos **path autocomplete**
    - Mediante CONTROL + ESPACIO podemos lanzarlo
    - Prueba con un fichero css (para insertar el fichero utiliza la abreviatura de emmet link)
    - Puede que necesitemos configurar los settings de usario con:
    ```
    "path-autocomplete.extensionOnImport": true
    ```



## Ingellisense para html
* Intellisense:
     - Por defecto nos da sugerencias (ej. teclea < y pulsa CTRL + espacio)
     - Si no utilizamos angular o ionic por ej, puede ser útil quitarlo del intellisense (en los settings)

    ```
     {
        "html.suggest.angular1": false,
        "html.suggest.ionic": false,

    }

    ```



## Validador para HTML

* Linter para html con la extensión HTMLHint
    * Necesitamos instalar el [paquete de node.js HTMLHint](https://www.npmjs.com/package/htmlhint)



  
## IntelliSense para CSS
 - Puede ser útil que nos de sugerendias respecto a clases de CSS que queremos utilizar:
     - [IntelliSense for CSS class names](https://marketplace.visualstudio.com/items?itemName=Zignd.html-css-class-completion)
     - [HTML CSS Support](https://marketplace.visualstudio.com/items?itemName=ecmel.vscode-html-css)
 - Instalaremos HTML CSS Support
         



## Comprobar HTML CSS Support
 - Al rellenar el atributo class de algún elemento html el editor te da sugerencias (pulsando CTRL+espacio)
 

 - Fichero css de prueba, por ejemplo *styles/main.css*:
     ```
     .rojo { color: red; }
     .azul { color: blue; }
     .importante { font-weight: bold; }
    ```



## Generar fichero html para pruebas
- Crea un fichero html vacío (extensión html para que Emmet funcione)
- Pulsa ! + TAB para crear una plantilla html
- Dentro del head introduce *link:src + TAB* (abreviatura Emmet)
- Si en el body creas algún elemento, comprueba que salga autocompletado para el atributo class del párrafo:
``` 
<p class="">Prueba</p>
```



## Linter para JavaScript
- Utilizaremos eslint (el más habitual)
- Instalaremos la extensión eslint dentro de Visual Code Editor
- Además de la extensión es necesario instalar un ejecutable en node:
    ```
    npm install -g eslint
    ```
- Utilizaremos eslint como dependencia de desarrollo dentro de nuestros proyectos
- Abrir paleta de comandos (CTRL + MAYS + P) y elegir *Create a .eslintrc.json file*



## Formateador de código
- [Beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify) formatea HTML, JS, JSON, CSS y SASS

- Configuramos un atajo de teclado
    - Accedemos a la paleta de comandos (CTRL + MAY + P)
    - Filtramos por shortcuts
    - Seleccionamos *Preferencias: abrir métodos abreviados de teclado*
    - Filtramos de nuevo, ahora por beautify
    - Configuramos el atajo mediante ALT + F



## Extensiones para node
- *ExpressSnippet*: para autocompletado de Express
- [npm intellisense](https://marketplace.visualstudio.com/items?itemName=christian-kohler.npm-intellisense): para autocompletado de los módulos que se instalan vía require.
- [Search node modules](https://marketplace.visualstudio.com/items?itemName=jasonnutter.search-node-modules): para buscar en la carpeta node_modules (a menudo se "elimina" del editor de código).