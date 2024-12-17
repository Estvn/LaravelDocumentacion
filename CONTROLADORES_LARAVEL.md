
# Controladores en Laravel
--------

- Los controladores son clases que se utilizan para agrupar la lógica relacionada con las manipulación de solicitudes HTTP y la gestión de la lógica de negocio de una aplicación.
- Los controladores actúan como intermediarios entre las rutas definidas del archivo y las respuestas enviadas al navegador.
- Un controlador en Laravel se define como una clase PHP que contiene métodos públicos que representan acciones específicas.
- Estos métodos llaman a métodos de acción y se encargan de manejar solicitudes HTTP entrantes y devolver una respuesta apropiada.

> [!NOTA] Comando para crear Controladores
>- 
> ```
> php artisan make:controller NombreControlador

- **En la clase de controlador se definen los métodos que tendrán llamados a los modelos para interactuar con la base de datos.**
- Cuando se crea un controlador, es buena práctica definir su nombre con letra capital y que termine con la palabra "Controller" concatenado por medio de CamelCase.

- Los controladores son clases que manejan la lógica de una aplicación, y responden a las solicitudes del usuario.
- Los controladores son responsables de procesar la entrada del usuario, interactuar con el modelo si es necesario, y devolver la respuesta adecuada, ya sea en forma de vista o datos para una API.

- En lugar de definir toda la lógica de manejo de solicitudes dentro de las rutas, Laravel alienta a los desarrolladores a organizar su código de la manera más estructurada mediante el uso de los controladores.
- Los controladores ayudan a separar las preocupaciones y a mantener el código más limpio y fácil de mantener.

## Tres Formas de Enviar Información del Controlador a la Vista

1. Usando la función compact()

```
public function index(){
	$clientes = Cliente::all();
	return view('cliente.index', compact('clientes'));
}
```

2.  Enviando un arreglo asociativo

```
public function index(){
	$clientes = Cliente::all();
	return view('cliente.index', ['clientes' => $clientes]);
}
```

3. Enviando un JSON

```
public function index(){
	return response()->json(Producto::all())
}
```

## Dos formas de definir la fecha actual

```
use Carbon\Carbon;

$propiedad->fechaCreacion = Carbon::now();
$propiedad->fechaModificacion = now();
```
## Métodos para consultar y modificar información de la DB en los controladores de Laravel

#### Método **all()**
- Obtiene todo el contenido de una tabla:

```
use App\Models\Cliente;

class AsientoController extends Controller{
	
	public function index(){
		$clientes = Cliente::all();
		return view('cliente.index', compact('clientes'));
	}
}

```

- Si se va a utilizar un modelo, se tiene que hacer una importación del archivo del modelo que se está utilizando.
- Todos los métodos dentro de la clase controladora tienen como propósito implementar lógica de CRUD para el sistema.

- el método **all()** del modelo Cliente se encarga de obtener todas las filas de la tabla cliente en la base de datos. 
	- Este tipo de métodos en los modelos está definidos de forma implícita, y son manipulados por el ORM Eloquent.
	
- **El contenido obtenido tiene que ser devuelvo hacia la vista.**
- El contenido obtenido por el método **all()** se devuelve a la vista por medio del segundo parámetro del método view().
- Existen dos formas de devolver el contenido hacia la vista:
	- Se usa la función **compact('nombreVariable')**, la cuál devuelve un arreglo asociativo
	- Se declara un arreglo asociativo.

#### Método **find()**

- Se usa para obtener un registro por medio del identificador que se le ha enviado.
- El registro encontrado puede ser devuelvo a la vista, modificado por la información enviada de un formulario o eliminado

**Ejemplo donde se actrualiza un registro:**

```
public function productosActualizar(Request $request, $id){

	$producto = Producto::find($id);

	$producto->CODIGO_PRODUCTO = $request->input('productCode');
	$producto->NOMBRE = $request->input('productName');
	$producto->PRECIO = $request->input('productPrice');
	$producto->STOCK = $request->input('productAmount');
	
	$producto->save();
	return redirect('/productos');
}
```

**Ejemplo donde se elimina un registro:**

```
public function productosDestruir($id){
	$producto = Producto::find($id);
	$producto->delete();
	return redirect('/productos');
    }
```

#### Método where();

- Busca uno o un grupo de registros que cumplan con el dato que se ha traído desde el parámetro.
- El registro o el grupo de registro obtenidos por medio del where pueden ser eliminados o devueltos a la vista.

**Ejemplo donde se devuelven a la vista:**

```
public function buscarApartamento(){

$apartamentosPorColor = Propiedad::where('color', 'azul');
return view('apartamentoDisponible', ['apartamentos' => $apartamentosPorColor]);

}
```

#### Método save()

- Se usa para guardar un registro que ha sido recien creado o modificado

**Registro recien creado:**

```
public function apartamentoCrear(Request $request){

	$propiedad = new Propiedad();
	$propiedad->area = $request->input('area');
	$propiedad->color = $request->input('color');
	$propiedad->piso = $request->input('piso');
	$propiedad->fechaCreacion = Carbon::now();
	$propiedad->fechaModificacion = now();

	$propiedad->save();
	return redirect('/apartamento'); // Redirige a una ruta
}
```

**Registro recien modificado:**

```
public function productosActualizar(Request $request, $id){

	$producto = Producto::find($id);
	$producto->CODIGO_PRODUCTO = $request->input('productCode');
	$producto->NOMBRE = $request->input('productName');
	$producto->PRECIO = $request->input('productPrice');
	$producto->STOCK = $request->input('productAmount');

	$producto->save();
	return redirect('/productos');
}
```

#### Método delete()

- Se usa para eliminar permanentemente un registro de la base de datos

```
public function productosDestruir($id){

	$producto = Producto::find($id);
	$producto->delete();
	return redirect('/productos');
}
```

## Dos formas para retornar a una vista desde un controlador
#### Método redirect()

- Redirige a una ruta

```
public function productosDestruir($id){

	$producto = Producto::find($id);
	$producto->delete();
	return redirect('/productos');
}
```

#### Método redirect()->route('nombreRuta')

- Redirige a una ruta por medio del nombre que se le asignó.

```
Route::get('/redirigir', function(){
    return redirect()->route('nombre.destino');
});

Route::get('/destino', function(){
    return "Estoy en la ruta destino";
})->name('nombre.destino');
```

#### Método view()

- Se usa cuando se quiere redirigir a una vista, enviando información en ella.

```
public function productosEliminar($id){

	$producto = Producto::find($id);
	return view('productosEliminar', compact('producto'));
}
```

- Retorna a una vista enviando contenido.







