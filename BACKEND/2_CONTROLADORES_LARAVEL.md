
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

#### Método findOrFail()

- Funciona igual que el método find(), pero si no encuentra un valor, devuelve una respuesta de tipo 404.

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

## **Retornar una instancia de Tabla sin Usar los Métodos find() o findOrFail()**

- Cuando envía el ID de un campo para devolver sus valores, no es necesario hacer una consulta manual desde la base usando métodos del modelo.
- Para evitar el uso de métodos del modelo para consultas a la base de datos, Laravel puede hacerlo de forma interna.

- El ID que se obtiene desde los parámetros del método del controlador se tiene que parsear al objeto modelo que se vincula con la tabla.

```
public function mostrar(Post $id){
	return $id;
}
```

- **La variable con la que inicialmente se obtiene el ID, luego de ser parseada se convierte en una variable que guarda un objeto que contiene todos los valores de la instancia correspondiente al ID que guardaba anteriormente.

```
public function mostrar(Post $id){
	return $id->title;
}
```

- Su ventaja es que obtiene los valores de la tabla si usar un método find(), y también devuelve un resultado 404 si el valor no existe.
## Tres formas para retornar a una vista desde un controlador
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

#### Método to_route('nombreRuta)

- Hace lo mismo que redirect()->route() pero con menos caracteres.

```
return to_route->('nombreRuta');
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

##  **Tres formas de crear y actualizar un registro en la base de datos**


> [!NOTA] 
> - Las alternativas se mencionan desde la menos eficiente hasta la más eficiente. 
> - Para añadir en la alternativa 3 validaciones de usuarios y dejar el trabajo de la validación de los datos a un archivo dedicado para este trabajo, vea el siguiente documento
> - [Crear archivo Form Request](FORM_REQUEST_LARAVEL.md)
> 
#### Alternativa 1

- Para crear un nuevo registro en la base de datos usted puede crear una instancia de registro para una tabla y asignarle valores para posteriormente guardarla.

```
public function store(Request $request){

	$request->validate([
		'titulo' => ['required', 'numeric', 'min:2']
	]);

	$post = new Post();
	$post->title = $request->input('titulo');
	$post->save();

	session()->flash('respuesta', 'Post created!');
	return redirect()->route('blog');
}
```

#### Alternativa 2

> [!NOTA]
> Esta alternativa puede ser usada en cualquier ocasión donde se esté usando una instancia de registro de una tabla.

- Una alternativa con menos líneas de código y sin necesidad de crear instancias es usar **Post::create([]);**

```
public function store(Request $request){

	$request->validate([
		'titulo' => ['required', 'numeric', 'min:2']
	]);

	Post::create([
		'title' => $request->input('titulo')
	]);

	session()->flash('respuesta', 'Post created!');
	return redirect()->route('blog');
}
```

- Si usted va a usar esta alternativa tenga en cuenta que debe agregar la propiedad **fillable** en el Modelo de la tabla que se está tratando. Se agrega este campo para que el sistema pueda cargar archivos de forma masiva.

Este error surge si no tiene la propiedad fillable en el modelo

```
Add [title] to fillable property to allow mass assignment on [App\Models\Post].
```

Propiedad **fillable** agregada en el modelo

```
protected $fillable = ['title'];
```

> [!WARNING]
> Si usa fillable, tiene que agregar todas las columnas que son llenadas por el usuario.

Usando la segunda alternativa para actualizar un registro

```
public function update(Request $request, Post $id){
	
	$request->validate([
		'titulo' => ['required', 'numeric', 'min:2']
	]);

	$id->update([
		'title' => $request->input('titulo')
	]);

	session()->flash('respuesta', 'Post updated!');
	return redirect()->route('get.onepost', $id->id);
}
```

#### Alternativa 3

- Ya vimos que nos podemos ahorrar la creación de instancias y el llamado del método save() cuando hacemos uso de métodos que realizar este trabajo de forma interna.
- **Pero podemos reducir aún más el código si en lugar de definir los campos en estos métodos, le pasamos la variable que contiene los campos que ya pasaron por el filtro de validación.**
- Es importante mencionar que esta variable de datos validados siempre tendrá todos los campos para actualizar o crear un registro, porque en caso que un campo no sea validado se lanzará un error en la vista.

```
public function store(Request $request){

	$validated = $request->validate([
		'titulo' => ['required', 'numeric', 'min:2']
	]);

	Post::create($validated);

	session()->flash('respuesta', 'Post created!');
	return redirect()->route('blog');
}
```

Alternativa usada en la actualización de un registro

```
public function update(Request $request, Post $id){

	$validated = $request->validate([
		'titulo' => ['required', 'numeric', 'min:2']
	]);

	$id->update($validated);

	session()->flash('respuesta', 'Post updated!');
	return redirect()->route('get.onepost', $id->id);

}
```

> [!WARNING]
> Para usar esta alternativa, tanto los campos del formulario como los campos de la base de datos tiene que ser los mismos.
# Consultas a Base de Datos desde los Controladores

- Cuando se realizan consultas a la base de datos desde los controladores no se requiere el uso de modelos ni migraciones
- Para lograr esto se agrega esta importación en el controlador:

```
use Illuminate\Support\Facades\DB;
```

- Las consultas desde el controlador lucen de esta forma

```
public function index(){

	$posts = DB::table('posts')->get();
	return view('blog', ['posts' => $posts]);
}
```

- Cada fila que se obtiene de la base de datos se obtiene como un objeto del tipo **stdClass** 
- Por lo tanto, cada columna de la instancia obtenida se toma como si fuera un método o atributo

```
@foreach ($posts as $post)
	{{$post->title}}  <br>
@endforeach
```

