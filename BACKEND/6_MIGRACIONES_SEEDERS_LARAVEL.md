
# Migraciones y Seeders en Laravel
---------

- En Laravel, las migraciones y los seeders son herramientas esenciales para manejar la estructura de la base de datos, y poblarla con datos de prueba.
### Migraciones

- Las **migraciones** son archivos de PHP, que se crean dentro del directorio **database/migrations**.
- La migraciones definen la estructura en la base de datos, y permiten realizar cambios de manera programática y controlada.
- La migraciones son reversibles, lo que significa que puede deshacer un cambio si es necesario.

**Comando para crear un archivo de migración:**

```
php artisan make:migration create_nombreTabla_table
```
### Seeders

- Los seeders son clases que se utilizan para poblar las tablas de la base de datos, pero con datos de prueba o semillas.
- Permiten insertar registros específicos en la base de datos de manera automática.

**Comando para crear un seeder:**

```
php artisan make:seeder NombreSeeder
```

**Debe importar este archivo en la clase seeder:**

```
use Illuminate\Support\Facades\DB;
```

**Ejemplo de la estructura de los datos a insertar que se debe definir en el método run() de la clase seeder:**

```
use Carbon\Carbon;

DB::table('clientes')->insert([
            ['clienteID' => 2,
             'pNombre' => 'Estiven2',
             'pApellido' => 'Mejia',
             'clienteFrecuente' => 1,
             'fechaNacimiento' => '2024-12-12',
             'telefono' => '99887766',
             'created_at' => Carbon::now(),
             'updated_at' => Carbon::now()],
        ]);
```

**Comando para ejecutar el seeder:**

```
php artisan db:seed --class=NombreSeeder
```









