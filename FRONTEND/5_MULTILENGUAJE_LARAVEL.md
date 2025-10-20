# Soporte Multi-lenguaje en Laravel
------------

```
php artisan lang:publish
```

- Al ejecutar este comando se va a crear una carpeta **lang/en**
- Para cambiar el lenguaje al español tenemos que crear la carpeta "es" en el mismo nivel que la carpeta "en".

- En la carpeta es tiene que guardar los mismos archivos que tiene en la carpeta "en", pero todo el contenido referenciado a las variables que se encuentra en los archivos debe estar traducido al español.

- Una opción para traducir todo el contenido necesario al lenguaje que usted desee, simplemente tiene que hacer la instalación de este paquete:

```
composer requiere --dev laravel-lang/lang
```

- Una vez instalado el paquete tiene que ejecutar el siguiente comando

```
php artisan lang:update
```

**Para agregar soporte a otro lenguaje tiene que ejecutar el siguiente comando:**

```
php artisan lang:add pt
```

- En este ejemplo se está requiriendo traducción de valores al idioma portugués.

**Para usar las traducciones en las vistas tiene que usar la siguiente función:**

```
{{__('Title')}}
```

- Se devolverá una traducción si existen para la palabra Title, caso contrario mostrará el contenido que se encuentra dentro de la función.
