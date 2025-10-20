
# Bases de Datos y Migraciones en Laravel

## Configuración de la Base de Datos en Laravel
--------

- Dentro del proyecto de Laravel se encuentra un archivo **.env**
- En este archivo se encuentra toda la información de la conexión a la base de datos.

A continuación se describen algunas variables importantes dentro del .env:

- **APP_URL**: Esta ruta es la que indica donde está instalado el proyecto Laravel.
- **DB-DATABASE**: En esta variable se encuentra el nombre de la base de datos con la que interactúa el proyecto.
- **DB-USERNAME**: Tiene el nombre del usuario encargado de la base de datos dentro del gestor.
- **DB-PASSWORD**: Contraseña del usuario de la base de datos.

- Laravel de forma predeterminada, a parte de las tablas del negocio crea otras tablas que se relacionan con la seguridad del acceso hacia la aplicación hecha en Laravel.
- **Tiene que crear una base de datos y configurar sus datos en el archivo .env antes de poder realizar interacciones entre el sistema y la base de datos.**

## Migraciones en Laravel
-------------

- Permite manejar la estructura de las tablas de la base de datos desde la aplicación.
- Las migraciones contienen la estructura de las tablas, es decir, contienen los nombres de los campos y sus atributos (atributo refiere al tipo de dato del campo).

En estos archivos se encuentra toda la lógica de conexión con la base de datos :

```
config/database.php
.env -> En este archivo se configura la conexión a la DB.
database/migrations -> En esta ruta se crean las migraciones
```

- Debido a que las tablas contienen múltiples instancias que representan una entidad, las tablas tienen que crearse con un nombre en plural.
- Si los nombres de las tablas no se escriben en plural, puede haber problemas de mapeo con el ORM.

- En las migraciones se define la estructura de cada tabla.
- Los archivos de migración extienden de la clase **Migration** y heredan dos métodos: up() y down().
- Usamos el método **up()** para crear la estructura de la tabla.

> [!NOTA]
> **Comando para crear un archivo de migración**:
> ```
php artisan make:migration create_nombreTabla_table


A continuación se presenta un ejemplo de una estructura de código en un archivo de migración.

```
<?php

return new class extends Migration{
	
	public function up():void{
		Schema::create('users', function(Blueprint $table)){
			$table -> id('columnaID');
			$table -> String('name', 50);
			$table -> String('email') -> unique();
			$table -> timestamp('email-verified-at') -> nullable();
			$table -> double('costo', 12, 2) -> default(000.00);
		}
	}
}
```

- **id('columnaID')** es el campo id en la tabla de la BD, es de tipo entero y será llave primaria.
- **String('name', 50)** es un campo de tipo String con 50 caracteres.
- **String('email')->unique()** es un campo donde no se pueden repetir valores.
- **timestamp('email-verified-at') -> nullable()** crea un campo que acepta valores nulos y guarda fecha y hora.
- **double('costo', 12, 2) -> default(000.00)**
- **$table -> rememberToken()** es una función que crea un campo booleano.
- **$table -> timestamps()** crea dos campos que guardan fecha y hora llamados "create-at" y "update-at".

- Estas estructuras creadas en el proyecto deben pasar por un proceso de migración para conectarse con la DB.
#### **AppServiceProviders**

**Hay una configuración por defecto que tiene que ser modificada para evitar errores al hacer impacto a la base de datos:**

```
app/providers/appServiceProvider.php

use Illuminate\Support\Facades\Schema; // Importación necesaria

public function boot(): void{
	// Define el tamaño de los nombres de los campos
	Schema::defaultStringLength(191);
}
```

- La ruta indicada anteriormente dirige hacia un archivo de configuración donde se especifica como se van a crear o que tamaño pueden tener los nombres de las estructuras y los campos.

> [!NOTA]
> **El siguiente comando actualiza todas las tablas que hay en el proyecto y las "impacta" en la base de datos (se hace una migración):**
> ```
php artisan migrate

### Consideraciones de la tabla **migrations**

- La tabla **migrations** se crea por defecto con este comando. Contiene información de las fechas donde se creó alguna tabla de la DB.
- La tabla migrations contiene una columna llamada **batch(lote)**, es un contador que contiene la cantidad de actualizaciones realizadas en una tabla después de realizar una migración.
- **batch** es útil al momento de realizar un rollback, cada que se hace un rollback el contador de batch indica cuál fue la actualización y estado anterior de cada tabla para utilizarlo.

> [!IMPORTANT]
> Antes de hacer una migración tiene que existir una base de datos donde se realice el impacto.
> **Cuando se hace una migración, se crea la tabla en la base de datos.**

-------------------
#### **Métodos para editar tablas desde las migraciones

**Existen dos métodos para modificar, editar o agregar campos a una tabla:**
1. El primer método borra toda la tabla con su información, y la vuelve a crear.

```
php artisan migrate:fresh
```

- Borra todas las tablas de la base de datos y los vuelve a crear con la edición o agregación más reciente en los campos, sin los datos.
- Cuando se usa el comando anterior, el contador batch vuelve a 1.

2. El segundo método crea nuevos campos en las tablas, sin borrar las tablas ni los datos que se encuentran en ellas

```
php artisan make:migration add_nombreCampo_to_nombreTabla_table
```

- Este comando no hace directamente los cambios en la base de datos, solo crea un archivo con los cambios más recientes, y ese archivo será el que haga los cambios en la base de datos con el comando **php artisan migrate**
- Este comando es útil cuando se quieren hacer cambios a una tabla con datos, ya que no borra su contenido.

- **Al ejecutar este comando se creará un archivo donde se deben definir los nuevos campos, y al definir los campos nuevos se tiene que hacer la migración para que se agreguen en la tabla existente de la base de datos.**
- **El archivo add generado puede servir para futuras modificaciones en la tabla.**

> [!NOTA] RESUMEN DE COMANDOS 
> Se usa cuando se migran tablas por primera vez:
> - php artisan migrate
> 
> Se usa para actualizar los cambios de una tabla con la que se encuentra en la DB, pero borra todos los datos de todas las tablas:
> - php artisan migration:fresh
> 
> Se usa para actualizar una tabla sin borrar su contenido
> Al usar este comando se crea un nuevo archivo donde se harán las modificaciones futuras de la tabla, **AÚN NO HAY CAMBIOS EN LA BASE DE DATOS**.
> A partir del nuevo archivo generado por medio de este comando, se deberá modificar la tabla con el comando **php artisan migrate**
> - php artisan make:migration add_nombreCampo_to_nombreTabla_table









