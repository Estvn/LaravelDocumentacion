
# ORM de Laravel y el Uso de Modelos
---------

- Es buena práctica crear las tablas de la base de datos haciendo uso del ORM de Laravel.
- De esta forma, el ORM sabe de forma más precisa a que tabla tiene que dirigirse.

- Cada tabla se asocia con un modelo.
- Para no tener conflictos de incongruencia de nombres, el nombre del modelo se crea en singular, ORM hace un mapeo de la tabla con el nombre del modelo convertido en plural.

**Comando para crear modelos:**

```
php artisan make:model NombreModelo
```

- Crea el archivo del modelo en la carpeta app/Models.

En los casos donde el nombre de una tabla no puede representarse de forma singular en el nombre del modelo, se hace los siguiente:

```
class Propiedad extends Model{
	protected $table = 'propiedades';
}
```

- En el código anterior se indica que el modelo Propiedad tiene que relacionarse con la tabla llamada propiedades.

> [!NOTA] Propósito de los modelos
> Los modelos se usan para la manipulación de los datos con la base de datos.

**Funciones de los modelos:**
- **save**: Inserta y actualiza datos en una tabla.
- **delete**: Elimina registros de una tabla.
- **find**: Busca un registro.
- **where**: Aplica un registro para la búsqueda de los datos.

## Detalles a considerar en el archivo de modelos

- Cuando se va a hacer una inserción en las tablas, los modelos asumen que algunos campos (campos creados por id y timestamps) existen en la tabla.
- Para evitar este problema, se especifica que el campo timestamps no existe.
- Si el nombre del campo identificador no es id se tiene que especificar este detalle en el modelo, de igual forma si no es un campo incremental automático.

```
class Propiedad extends Model{
	protected $table = 'propiedades';
	public $timestamps = false;
	protected $primaryKey = 'campoID';
	public $incrementing = false;
}
```

**La siguiente variable se define en el modelo, e indica solo las columnas de la tabla que pueden ser alteradas con un create o update:**

```
protected $fillable = ['nombre', 'descripcion', 'precio'];
```
## Tinker

- Tinker se usa para verificar el funcionamiento correcto de los modelos, verifica que los mapeos no tengan errores y se puedan ejecutar las operaciones de manera correcta.

```
composer require laravel/tinker
php artisan tinker
```

- En el comando de tinker, se tiene que especificar el Modelo que se desea verificar.




