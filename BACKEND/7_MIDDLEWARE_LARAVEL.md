
# Middleware en Laravel
-------------

- En Laravel, los Middleware proporcionan una forma conveniente de filtrar solicitudes de tipo HTTP que ingresan a la aplicación.
- **Los Middleware se ejecutan antes o después de que lleguen a las rutas de la aplicación y permitan realizar acciones específicas en la solicitud.**

- Por ejemplo, con Middleware puedes verificar la autenticación de un usuario, el manejo de las sesiones de los login, entre otro tipo de verificaciones previo a que se ejecute.
- El Middleware evitará que alguien se loguee a la aplicación antes de poder hacer las verificaciones necesarias.
- Por ejemplo, el Middleware va a verificar si ese usuario existe como administrador o no en el sistema de base de datos

> [!NOTA]
> Los Middleware se ejecutan en el orden que se han definido.

**Comando para crear un Middleware:**

```
php artisan make:middleware NombreValidacionMiddleware
```

- Un middleware es una clase que tiene un código que hace un trabajo en concreto que nosotros le asignamos.

**De esta forma se aplica un Middleware a una ruta:**

```
Route::middleware([AdminCheckMiddleware::class])->group(function(){
    Route::get('/ruta1', function() {
        return view('welcome');
    });
});
```

## **¿Qué son los Middleware?

- Son una parte fundamental del sistema de trabajo de solicitudes HTTP.
- Son una capa de código que se ejecuta entre la recepción de una solicitud y el envío de una respuesta.
- Los middleware permiten realizar diversas tareas de manera modular antes que la solicitud llegue a su destino final, como por ejemplo, verificar la autenticación del usuario, realizar validaciones de datos, agregar encabezados HTTP, etc.

- Son útiles para separar la lógica del manejo de solicitudes en componentes reutilizables y bien estructurados. 
- Puedes utilizar middleware para aplicar lógica a nivel de ruta o a nivel global para todas las solicitudes entrantes.

**Comando de Artisan para crear un Middleware:**

```
php artisan make:middleware NombreMiddleware
```

- Los Middleware con clases de Laravel que utilizan el paradigma orientado a objetos.
- **En la función handle() del middleware se agrega la lógica.**

**Ejemplo de un Middleware:**

```
public function handle(Request $request, Closure $next): Response
{
	if($request->age > 15){
		return redirect('/nombre/estiven');
	}
	return $next($request);
}
```

- En Versiones anteriores a Laravel 11, es necesario registrar en el archivo kernel.php los Middleware que se han creado.

## **Ejemplo de un Middleware

**Código en la ruta

```
Route::get('/nombre', [ValidarMwController::class, 'index'])->middleware(CheckAgeMiddleware::class);

Route::get('/nombre/confirmado/{edad}', function($edad){
	return "La edad es " . $edad;
})->name('middle');
```

**Código del Middleware**

```
public function handle(Request $request, Closure $next)
{
	if($request->age > 15 && $request->has('age')){
		return $next($request);
	}
	return redirect('/origen');
}
```

**Código del controlador

```
class ValidarMwController extends Controller
{
    public function index(Request $request){
        return redirect()->route('middle', $request->age);
    }
}
```

> [!NOTA]
> **Un solo Middleware también puede ser asignado a un grupo de rutas.**
## **Parámetros de un Middleware

- En los parámetros de un Middleware se puede obtener un Request (el mismo que se recibe en el Controlador.
- Para poder enviar parámetros al Middleware, tiene que existir el archivo kernel.php en  el proyecto para poder definir alias a los middleware

```
Route::get('/nombre', [ValidarMwController::class, 'index'])->middleware('nombreMiddleware:80'); //-> el parámetro es 80

Route::get('/nombre/confirmado/{edad}', function($edad){
    return "La edad es " . $edad;
})->name('middle');
```

```
public function handle(Request $request, Closure $next, $minAge)
{
	if($request->age > $minAge && $request->has('age')){
		return $next($request);
	}
	return redirect('/origen');
}
```

## **Alias de Middleware en Laravel 11

- En el archivo **\bootstrap\app.php** se tiene que añadir los alias de los Middleware de la siguiente manera:

```
    ->withMiddleware(function (Middleware $middleware) {

        // Rutas que no pasan por la validación csrf
        $middleware->validateCsrfTokens(except:[
            'http://127.0.0.1:8000/productos',
            'http://127.0.0.1:8000/productos/3'
        ]);

        // Definición de alias para los Middleware
        $middleware->alias(['nombreMiddleware' => \App\Http\Middleware\CheckAgeMiddleware::class]);

    })
```

- Los alias de los middleware se definen en la misma función donde se agregan las URL que no pasan por la verificación csrf.
- Esta es la diferencia entre usar alias en Laravel 1 y versiones anteriores, lo demás puede realizarse igual que en las versiones anteriores.

## **Middleware en grupos de rutas, y agrupamiento de Middleware**

- Se pueden agrupar varios Middleware con el propósito de asignar sus validaciones con un solo llamado a una o varias rutas.
- Esto ayuda a reducir código.

> [!IMPORTANT]
> **Es importante mencionar, que si los middleware se asignan en grupos, no deben recibir parámetros.**

#### Agrupando Middleware
 
- Los grupos de las rutas se crean en el archivo **\bootstrap\app.php**, y se agregan por su alias.

**Creación de un grupo de Middleware:**

```
    ->withMiddleware(function (Middleware $middleware) {

        // Rutas que no pasan por la validación csrf
        $middleware->validateCsrfTokens(except:[
            'http://127.0.0.1:8000/productos',
            'http://127.0.0.1:8000/productos/3'
        ]);

        // Definición de alias para los Middleware
        $middleware->alias(['check.age' => \App\Http\Middleware\CheckAgeMiddleware::class]);

		// Los alias de los Middleware se definen en los grupo
        $middleware->group('grupoMiddleware',[
            'check.age'
        ]);
    }
```

#### Asignando grupos de Middleware en grupos de rutas

**Grupo de Middleware asignado a una ruta** 

```
Route::get('/nombre', [ValidarMwController::class, 'index'])->middleware('grupoMiddleware');
```

**Grupo de Middleware asignado en un grupo de rutas**

```
Route::prefix('prestamo')->middleware('grupoMiddleware')->group(function(){

	Route::get('/', [PrestamoController::class, 'prestamoInicio'])->name('prestamo.inicio');
	Route::post('/guardar', [PrestamoController::class, 'prestamoGuardar'])->name('prestamo.guardar');
   
});
```

- Cuando se asigna un grupo de Middleware en una o varias rutas, el flujo de control pasa por todos los Middleware, y se devuelve al controlador o función de la ruta cuando ya han terminado de ejecutarse satisfactoriamente todos los Middleware.

> [!NOTA]
> - Un Middleware puede ser asignado a una sola ruta.
> - Un Middleware puede ser asignado a un grupo de rutas.
> - Un grupo de Middleware puede ser asignado a una sola ruta.
> - Un grupo de Middleware puede ser asignado a un grupo de rutas.

## **Middleware dentro de Controladores** 

> [!WARNING] ADVERTENCIA!
> - En Laravel 11, no es recomendable definir middleware dentro de los controladores.
> - Los siguientes ejemplos son para versiones anteriores de Laravel.

- En lugar de asociar los Middleware en las rutas, también pueden ser asociados dentro de un controlador. 

- **Cuando un Middleware se define en el constructor, no es necesario que se defina en la ruta.**

```
// El Middleware está en el constructor de la clase controlador
Route::get('/nombre', [ValidarMwController::class, 'index']);

```

- **Para definir un Middleware en un controlador debe crear un constructor donde guardará los middleware.
- Esto se realiza para asignar el middleware a cada instancia que se crea del constructor.

```
    public function __construct() {
        //$this->middleware('grupoMiddleware');
        $this->middleware('check.age');
    }
```

- **Puede ejecutar el Middleware solo para algunos métodos del controlador

```
public function __construct() {
	//$this->middleware('grupoMiddleware');
	$this->middleware('check.age')->only('metodo1', 'metodo2');
}
```

- **Puede exceptuar el Middleware en algunos métodos del controlador

```
public function __construct() {
	//$this->middleware('grupoMiddleware');
	$this->middleware('check.age')->only('metodo1', 'metodo2');
	$this->middleware('check.age')->except('metodo3', 'metodo4');
}
```













