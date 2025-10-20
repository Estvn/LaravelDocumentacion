
# Estructura de un Proyecto de Laravel
-------------------

> [!NOTA] Patrón que usa Laravel
> Laravel crea sus proyectos con el patrón Modelo, Vista y Controlador (MVC)
### app
- Este directorio contiene la lógica de la aplicación, incluyendo controladores, modelos, políticas y otros archivos relacionados con la aplicación.
- En esta carpeta de encuentra la carpeta de los Modelos: **app/Models**
- También tiene una carpeta que almacena los controladores: **app/Http/Controllers**
### bootstrap
- Contiene los archivos necesario para arrancar la aplicación, incluye el archivo **app.php** que es el archivo que carga la aplicación Laravel.
### config
- Contiene archivos de configuración para la aplicación, donde se pueden personalizar aspectos como bases de datos, servicios, entre otros.
### database
- En este directorio se encuentra las migraciones y seeders de la base de datos.
- Las **migraciones** son los archivos que describen cómo debe ser la estructura de la base de datos.
- Los **seeders** son utilizados para poblar la base de datos con datos de prueba. 
### public
- Esta carpeta es el punto de entrada público de la aplicación.
- Aquí se encuentran los archivos accesibles desde el navegador, como imágenes, hojas de estilo y **scripts**.
- En esta carpeta se encuentra el archivo **index.php**, en este archivo está el punto de entrada de todas las solicitudes de tipo HTTP.
### resources
- En esta carpeta se encuentra todo lo relacionado con la interfaz gráfica del proyecto, incluyendo las vistas: **resources/views**
- En este directorio se encuentran recursos no ejecutables, como **vistas**, archivos **.blade**, plantillas de la interfaz de usuario y archivos de traducción.
### routes
- Contiene archivos de definición de **rutas** que indican cómo deben manejarse las solicitudes HTTP.
- Las **rutas** son el mecanismo que Laravel utiliza para dirigir las solicitudes entrantes a los controladores correspondientes.
### storage
- Almacena archivos generados por la aplicación, como archivos de **registro, sesiones y archivos cargados por los usuarios**.
### tests
- Contiene archivos para realizar pruebas en la aplicación.
- Laravel alienta al desarrollo orientado a pruebas, y este directorio proporciona una ubicación estructurada para organizar las pruebas que el desarrollador va a ir haciendo a lo largo del ciclo del vida de la aplicación.
### vendor
- Esta carpeta contiene las **dependencias** de Composer, que es el administrador de paquetes utilizado por Laravel.
- Este directorio no debe ser modificado directamente.
### artisan
- Es el archivo ejecutable de la consola de comandos artisan.
- artisan es una herramienta de línea de comandos que facilita diversas tareas como la generación de código, la ejecución de migraciones, etc.









