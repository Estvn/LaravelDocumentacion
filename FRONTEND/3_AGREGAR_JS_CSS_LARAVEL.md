
# Agregar Javascript y CSS en Laravel
------------

- Laravel es un Framework Full-stack, lo que quiere decir que también provee herramientas para compilar y optimizar archivos CSS y Javascript.
- Laravel ofrece herramientas que ayuda a acelerar el proceso de desarrollo en el Front-end.
- **En Laravel se usa la herramienta Vite.js**

#### Agregar Archivos CSS y javascript

- Por temas de seguridad, la única carpeta accesible desde el navegador es la llamada "public".
- La carpeta public es la raíz del proyecto Laravel.
- **En la carpeta public se puede agregar la carpeta assets para añadir la carpeta css y js**

- **La creación de archivos por medio de esta carpeta se hace cuando no se usa una herramienta como Vite.js**
- A continuación se explica el uso de Vite.js

## **Instalación y Uso de Vite.js**

- Vite.js tiene una carpeta por defecto en los proyectos de Laravel con el nombre **vite.config.js**  
- En este archivo se encuentran las rutas de los archivos css y js que usará Vite.js. Por defecto, estos archivos están ubicados en la carpeta **resources**
- En los archivos que usa Vite.js usted tiene que hacer las modificaciones de estilos y funcionalidad Javascript.

- Para importar los archivos de estilos y Javascript en las vistas de la página tiene que usar el siguiente tipo de importación:

```
@vite(['/resources/css/app.css', 'resources/js/app.js'])
```

- Con esta importación el proyecto estará realizando toda la funcionalidad del lado del cliente con los beneficios de Vite.js

**Con el siguiente comando se instalarán las dependencias de las herramientas que contiene el proyecto, incluyendo Vite.js** 

```
npm install
```

- Después de ejecutar este comando se creará una carpeta llamada **node_modules** donde se encuentran las dependencias de Vite.js

#### Uso del servidor de desarrollo de Vite.js

- Vite.js tiene un servidor de desarrollo en el que los archivos de estilo y Javascript pueden ser utilizados sin haber sido compilados, esto permite que usted pueda revisar el avance del proyecto sin compilar estos archivos.

**El comando para levantar el servidor de Vite.js es el siguiente:**

```
npm run dev
```

- Después de ejecutar este comando, usted puede ingresar a la ruta donde está probando el proyecto, y los archivos css y js estarán siendo llamados desde el servidor de Vite.js, ya que aún no son compilados.

#### Compilar archivos usados por Vite.js

- Mencionamos que los archivos que usa Vite.js se encuentran en la carpeta resources.
- Cuando haya terminado su proyecto, tiene que compilar los archivos de la carpeta resources.

**Este es el comando para compilar los archivos de estilo y Javascript con funcionalidad de Vite:**

```
npm run build
```

- Al ejecutar este comando, los archivos de la carpeta resources se pasarán ya compilados a la carpeta public, con la funcionalidad de Vite.js, y el proyecto estará listo para instalarse en producción.

- **Cada modificación en los archivos Javascript o CSS se tienen que hacer desde resources, luego hacer la compilación para ver los cambios en producción.**

## **Activar Links**



