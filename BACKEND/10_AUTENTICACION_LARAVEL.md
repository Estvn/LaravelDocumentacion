
# Autenticaciones en Laravel
-------------

- La interfaz que se muestra en el inicio del proyecto se encuentra en el archivo **welcome.blade.php** ubicado en la ruta resources/views/welcome.blade.php
- En este archivo se encuentran unas rutas, son módulos que tenemos que instalar en nuestro proyecto Laravel. Al instalar los módulos necesarios se activarán las rutas, y en el proyecto nos habilitará la posibilidad de crear funcionalidad login.
- En la pantalla principal no se muestran estas funcionalidades de login, porque aún no se han instalado los módulos necesarios para activarlas.

### Activar rutas para funcionalidades de login en Laravel

> [!NOTA]
> **A partir de Laravel 11, las herramientas Breeze y Jetstream ya incluyen la funcionalidad de las autenticaciones, así que no es necesario ejecutar estas líneas de código.**

 - https://laravel.com/docs/11.x/authentication#password-confirmation-routing

**Forma tradicional de habilitar las autenticaciones en Laravel:**

```
composer require laravel/ui
php artisan ui vue --auth

npm i
npm run development
```

- Al ejecutar estas dos líneas de código en el proyecto de Laravel se instalarán los módulos para usar autenticaciones, se modificará el contenido de App/Http/Controller.php y routes/web.php, también se creará el archivo App/Http/HomeController.php 

-----------------------

- La autenticación en Laravel se refiere al proceso de verificar la identidad de un usuario que intenta acceder a la aplicación.
- Laravel ofrece un sistema de autenticación integrado que facilita la implementación de funciones de registro, inicio de sesión y gestión de usuarios.
- Laravel utiliza una herramienta llamada **Guards** para definir como se autentican los usuarios.
- Guards predeterminado es el guardia web que utiliza sesiones para mantener la autenticación.

- También existen los proveedores de usuarios, o User Providers.
- Un User Provider define como se obtienen y validan los usuarios.
- Laravel incluye proveedores para la base de datos y Eloquent por defecto.

> [!IMPORTANT]
> **Las validaciones en Laravel se hacen en los Middleware.**

## Breeze para habilitar autenticaciones

- Descargar el paquete de breeze en el proyecto de Laravel:

```
composer require laravel/breeze --dev 
```

- Instalar breeze:

```
php artisan breeze:install -h

php artisan breeze:install blade --dark
```

- Tiene que tener instalado nodejs para ejecutar este comando 

> [!NOTA]
> **En Laravel 11, la herramienta breeze puede ser agregada al proyecto durante su instalacion.**





























