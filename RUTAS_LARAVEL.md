
# Routing en Laravel
-------------

```
Route::view('/', 'welcome', ["nombre" => 'Estiven']);
```

```
<title>Laravel {{$nombre}}</title>
```

- Las rutas son las URLs que se utilizan para acceder a diferentes recursos y ejecutar acciones en una aplicación web.
- Las rutas en Laravel definen la correspondencia entre una RUL y la lógica de aplicación que debe ejecutarse cuando se accede a una URL específica.
- Las rutas se definen en el archivo routes.

- **Las rutas se almacenan en el archivo routes/web.php
- En el contexto de Laravel, routing se refiere al proceso de definir cómo una aplicación web responde a las solicitudes entrantes. 
- La rutas determinan que acción o controlador se debe ejecutar cuando un usuario accede a una URL específica dentro del proyecto de Laravel. 

- Laravel utiliza un sistema de routing muy flexible que permite definir rutas de manera clara y concisa, lo que facilita la organización de la lógica de la aplicación.

> [!NOTA]
> Las rutas se definen en el archivo: **routes/web.php**

Definición de una ruta get que obtiene una vista ubicada en resources/views:

```
Route::get('/', function(){
	return view('welcome');
});
```

- Si la vista tiene una extensión **.blade** solo es necesario definir el nombre de la vista
- Las rutas que no reciben parámetros se muestran primero que las que si reciben.
## **Rutas personalizadas

- **El parámetro '/' indica la raiz del proyecto, pero también se puede personalizar, por ejemplo, se puede definir '/ruta1' en lugar de '/'.**

```
Route::get('/rutaPersonalizada', function(){
	return view('welcome');
});
```

- El sistema de routing de Laravel es eficiente para personalizar rutas.
- Laravel admite métodos como get, post, put, delete entre otros para las rutas.
## **Paso de parámetros a través de las rutas
#### Envío de un parámetro estático 
- Para enviar parámetros a través de una ruta, en la función **view()** se tiene que definir un tercer parámetro adicional donde se define un arreglo asociativo.
- **Esta es una forma de enviar parámetros en las rutas de forma implícita.**

```
Route::view('/sinparam', 'hola', ['mensaje' => 'Esto es un mensaje'])
```

En la vista se define de esta forma:

```
<h1>{{$mensaje}</h1>
```
#### Envío de un parámetro variable obligatorio
- Otra forma de enviar parámetros a través de la ruta, es definiendo la ruta en primer parámetro del método **get()**
- **Esta es una forma de enviar un parámetro variable en la ruta:**

```
Route::get('/rutaPersonalizada/{parametro}', function($parametro){
	return view('vista', ['parametro' => $parametro]);
});
```
#### Envío de un parámetro variable opcional
- **Se puede enviar un parámetro opcional a través de la ruta de la siguiente forma:**

```
Route::get('/ruta1/{hola?}', function($hola = null) {
    return view('welcome', ['hola' => $hola]);
});
```

> [!IMPORTANT] Importar Controladores
> Si se van a utilizar controladores en las rutas, de la misma forma en la que se importan los modelos en los controladores, se tienen que importar los controladores en las rutas
> .
> ```
> use App\Http\Controllers\NombreControlador;
  
## **Nombre único de rutas 

```
Route::get('/clientes', [ClienteController::class,'index'])->name('cliente.index');
```

- Se define una ruta personalizada (no es necesario que sea personalizada).
- En el segundo parámetro se define la clase y el método de la clase controlador que se desea ejecutar.
- Esto lo va entender luego de ver el documento de controladores de Laravel.

- La función name('cliente.index') asigna un nombre único a esa ruta.
- Dentro de todo el proyecto, en lugar de llamar a esta ruta de la manera tradicional, puede ser llamada por el nombre que se le ha definido.

```
<!-- Llamando una ruta de forma tradicional -->
<a href="/tipoAsiento/agregarTipo">Agregar Tipo</a>

<!-- Llamando una ruta por el nombre definido -->
<a href="{{ route('asiento.agregartipo') }}">Agregar Tipo</a>

```
## **Agrupamiento de Rutas

#### Agrupamiento de rutas por prefijo

- Cuando varias rutas comparten un mismo parámetro estático, se pueden agrupar en una función que le asigna a las rutas el parámetro en común
- En el siguiente ejemplo se muestra el **agrupamiento de rutas** que tienen en sus parámetros el **llamado a un método de un constructor**.

- **Esta es la estructura del agrupamiento de rutas:**

```
Route::prefix('productos')->group(function(){
    Route::get('/', [ProductoController::class, 'index'])->name('producto.index');
    Route::get('/{id}', [ProductoController::class, 'show'])->name('producto.show');
    Route::post('/', [ProductoController::class, 'store'])->name('producto.store');
    Route::put('/{id}', [ProductoController::class, 'update'])->name('producto.update');
    Route::delete('/{id}', [ProductoController::class, 'destroy'])->name('producto.destroy');
    //Route::patch()
});

```

#### Agrupamiento de rutas por carpeta (namespace) de controlador

```
Route::namespace('Prestamo')->group(function(){
    Route::get('/hola', [ProductoController::class, 'index'])->name('producto.index');
    Route::get('/', [PrestamoController::class, 'prestamoInicio'])->name('prestamo.inicio');
    Route::post('/guardar', [PrestamoController::class, 'prestamoGuardar'])->name('prestamo.guardar');

});
```

- Tanto ProductoController, como PrestamoController se encuentran dentro de la carpeta Prestamo.
- Ambos archivos dentro de la carpeta Prestamo deben ser importados en el archivo de rutas.

**En los controladores dentro de la carpeta Prestamo, se debe  incluir estas dos líneas:**

```
namespace App\Http\Controllers\Prestamo;
use App\Http\Controllers\Controller;
```

> [!NOTA]
> Esto se puede utilizar cuando hay varios módulos en el sistema, y se desea tener sus controladores separados por carpetas.

## **Llamado de Controlador desde las rutas

```
Route::get('/clientes', [ClienteController::class,'index'])->name('cliente.index');
```

- Se define una ruta personalizada (no es necesario que sea personalizada).
- En el segundo parámetro se define la clase y el método de la clase controlador que se desea ejecutar.

- La función name('cliente.index') asigna un nombre único a esa ruta.
- Dentro de todo el proyecto, en lugar de llamar a esta ruta de la manera tradicional, puede ser llamada por el nombre que se le ha definido.

```
<!-- Llamando una ruta de forma tradicional -->
<a href="/tipoAsiento/agregarTipo">Agregar Tipo</a>

<!-- Llamando una ruta por el nombre definido -->
<a href="{{ route('asiento.agregartipo') }}">Agregar Tipo</a>

```

## **Peticiones HTTP desde las rutas

**GET**

```
Route::get('/ventas/mostrar', [VentasController::class, 'ventasMostrar'])->name('ventas.mostrar');

Route::get('/productos/destruir/{id}', [ProductosController::class, 'productosDestruir'])->name('productos.destruir');
```

**POST**

```
Route::post('/productos/guardar', [ProductosController::class, 'productosGuardar'])->name('productos.guardar');
```

**PUT**

```
Route::put('/productos/actualizar/{id}', [ProductosController::class, 'productosActualizar'])->name('productos.actualizar');
```

**DELETE**

```
Route::delete('/productos/{id}', [ProductoController::class, 'destroy'])->middleware('auth');
```

## **Redireccionamiento en las rutas

#### Método redirect()

- Se define la ruta origen y destino.

```
Route::redirect('/origen', '/destino', 301);
Route::get('/destino', function(){
    return "Estoy en la ruta destino";
});
```

#### Método redirect()->route('nombreRuta')

- Se define el nombre que se asignó en la ruta destino.

```
Route::get('/redirigir', function(){
    return redirect()->route('nombre.destino');
});

Route::get('/destino', function(){
    return "Estoy en la ruta destino";
})->name('nombre.destino');
```






