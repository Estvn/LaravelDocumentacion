
# Proteger rutas en Laravel
---------

- Las rutas pueden ser protegidas por medio de Middleware.

**Esta es una manera de implementar el Middleware de autorización en una ruta:**

```
Route::get('/', [PostController::class, 'index'])->name('post.blog')->middleware('auth');
```

- Cuando se está utilizando el método **resource()** no se puede hacer uso de esta forma de implementación de Middleware, ya que las rutas están generadas por medio de este método.
- **Otra forma de implementar Middleware es directamente a los métodos por medio del constructor del controlador.**

### Implementar Middleware desde los constructores

- Antes de implementar Middleware dentro de un constructor se tiene que modificar la clase abstracta de la que extienden todos los controladores.

- En este controlador abstracto del que extienden todos los controladores se debe importar el controlador base del Framework Laravel
- Una vez importado, el controlador abstracto tiene que extender del controlador base.

```
use Illuminate\Routing\Controller as BaseController;

abstract class Controller extends BaseController
{
    //
}
```

- **A mí me funciona así:**

```
<?php
namespace App\Http\Controllers;

use Illuminate\Foundation\Auth\Access\AuthorizesRequests;
use Illuminate\Foundation\Validation\ValidatesRequests;

abstract class Controller extends \Illuminate\Routing\Controller
{
    use AuthorizesRequests, ValidatesRequests;
}
```

- Ahora se puede crear un constructor en los controladores del proyecto para implementar Middleware.

```
public function __construct(){
	$this->middleware('auth')->except('index', 'mostrar');
	//$this->middleware('auth')->only();
}
```

### Ocultar HTML con la directiva @auth

- Si no desea que un usuario pueda acceder o ver secciones de código HTML, puede ocultarlo con las directivas @auth

```
@auth
	<a href="{{route('post.create')}}">Crear nuevo post</a>
@endauth
```










