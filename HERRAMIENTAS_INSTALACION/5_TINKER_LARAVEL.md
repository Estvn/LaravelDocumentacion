 
# Uso de Tinker para Modelos

- Tinker es una herramienta utilizada para verificar si el mapeo de los modelos hacia las tablas de la base de datos funcionan correctamente

**Con este comando se habilita en la consola el uso de Tinker:**

```
php artisan tinker
```

- Dentro de la consola de Tinker se puede insertar código php.

- Para probar el funcionamiento de los modelos por medio de Tinker, se **tiene que acceder a la ruta en la que se encuentra el modelo que desea testear, y luego agrega "::" seguido de métodos proporcionados por Eloquent para probar consultas**

```
App\Models\Post::get();
App\Models\Post::find(1);

$post->save();
$post->delete();
```

- Puede guardar los valores obtenidos en las variables y modificarlos para volver a insertarlos.
- Las modificaciones realizadas a los recursos de la base de datos por medio de Tinker puede afectar o alterar a la información real de la base de datos. Pero depende del método de Eloquent que se utilice.


















