
# Vistas y Plantilla Blade en Laravel
-------------

## **¿Qué son las vistas?

- Plantillas Blade o motor de plantillas en blanco.
- Permiten separar la lógica de presentación del resto de la aplicación, facilitando la creación de las interfaces de usuario dinámicas y reutilizables.

- Las plantillas Blade son archivos que contienen HTML y la presentación de la aplicación.
- Pueden contener variables que se asignan en los contr00oladores y se muestran en las vistas.
- Estas plantillas se almacenan en el directorio: **resources/views**

- Blade es el "apellido" del archivo, no su extensión. Es un motor de plantillas integrado en Laravel.
- Proporciona características como herencia de plantillas de directivas, y sintaxis sencilla para trabajar con datos dinámicos.
- Facilita la creación de plantillas reutilizables y estructurales.
## Imprimir en las vistas el contenido devuelto por los controladores

**Se recorre el arreglo por medio de un foreach**

```
<body>
	<ul>
		@foreach ($valores as $valor)
			<li>{{$valor->atributo}}</li>
		@endforeach
	</ul>
</body>
```

**Usando una tabla:**

```
<tbody>
	@foreach ($propiedades as $propiedad)
		<tr>
			<td>{{$propiedad->codigoPropiedad}}</td>
			<td>{{$propiedad->meta}}</td>
		</tr>
	@endforeach
</tbody>
```

## Llamado de vistas desde las rutas

Ruta definida:

```
Route::get('/home', [Controller::class, 'crear'])->name(crearUsuario)
```

Como se llama desde una etiqueta html:

```
<a href = "{{route('crearUsuario')}}"> Crear Usuario </a>
```

> [!NOTA]
> Cuando se desea llamar a una vista por medio de otra vista, como enlace se tiene que utilizar el nombre de la ruta que se encarga de devolver la vista con la información correspondiente.

# Formularios en Laravel
----------

- Para enviar datos desde la vista hasta el servidor, esta información se tiene que enviar por medio de un **formulario**.

```
form>(div>(label+input[type='text' name]))*5
```

```
<form action="{{route('propiedad.crear')}}" method="post">
@csrf
...
</form>
```

- En el atributo **action**  se especifica la ruta a la que tiene que enviarse la información del formulario.
- Cuando se presiona submit en el formulario se ejecutará una acción, la cuál intentará viajar a la ruta definida.

- En el atributo action del formulario se ha llamado a una ruta por su nombre único que se le ha definido.
- **La ruta a la que apunta el formulario tiene un controlador con un método que recibirá la información del formulario. Esta información estará en el objeto Request, en el parámetro del método**

**En el siguiente ejemplo se crea un objeto de modelo, y se le asigna a uno de sus atributos un valor del formulario:**

```
public function store(Request $request){

	$nvaPropiedad = new Propiedad();
	$nvaPropiedad->color = $request->input('color);
	$nvaPropiedad->costoXmtr = $request->input('costoXmtr');
	
	$nvaPropiedad->save();
	return redirect('/home');
}
```

- En el método, se crea un objeto del modelo relacionado con el controlador para poder guardar la información en la base de datos.
- Los atributos del modelo, con los nombres de las columnas de la tablas Propiedades, ubicada en la base de datos.
- Los atributos del objeto Request son los nombres de los inputs en el formulario.

## Enviar información desde la vista a los controladores
#### Envío de datos del formulario desde la vista para actualizar un registro

- Para editar la información de un registro en la DB se hace el envío del identificador, desde la vista hasta el controlador.
- Se llama una ruta desde la vista y se envía el identificador como un parámetro.
- El identificador enviado desde la ruta se usará en el controlador para hacer la búsqueda de los datos en la base de datos.
- Para recibir la información que se envía desde el formulario, en el controlador se crea un método que recibe como parámetro un objeto de tipo **Request**

```
public function store(Request $request){
	echo $request;
}
```
#### Envío de datos por medio de los parámetros de las rutas

- Normalmente, los parámetros con valores variables en las rutas se usan para enviar identificadores de algún registro en la base de datos.
- El identificador se puede usar para obtener, actualizar o eliminar un registro o un grupo de registros que se pueden identificar por medio del valor enviado

**Enviando arreglo asociativo por la ruta:**

```
<form action="{{route('productos.actualizar', ['valor' => 1])}}" method="POST">
```

```
Route::put('/productos/actualizar/{id}', fuction($id){
	return $id; // -> es 1
});
```

**Dato enviado por los parámetro de la ruta en la vista:**

```
<form action="{{route('productos.actualizar', $producto->CODIGO_PRODUCTO)}}" method="POST">
```

```
Route::put('/productos/actualizar/{id}', [ProductosController::class, 'productosActualizar'])->name('productos.actualizar');
```
 
**Estructura del controlador que recibe el parámetro**

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

- En el ejemplo anterior, desde la vista se envío un formulario y un identificador.
- Como puede ver, para el envío del formulario no es necesario definir un parámetro.
- Para enviar un dato, de define el valor después del nombre de la ruta, y en el archivo web.php se define una variable en la ruta, que luego es recibida en los parámetros del método en el controlador

## **Editar Formularios**

- Cuando va a editar un registro de la base de datos, en Laravel tiene que enviar la información del registro a editar por la ruta que dirige a la vista con el formulario para realizar la edición.
- No puede simplemente enviar la información de una vista a otra.
- En los valores predeterminados del formulario, en el método **old()** puede poner dos valores

```
<input type="text" name="titulo" value="{{old('titulo', $post->title)}}"/>
```

- El primer parámetro sería el valor anterior, en caso de que ocurra una redirección a causa de un error durante el envío del formulario.
- El segundo parámetro es uno de los valores que se envía a la vista.

## Verbos HTTP

> [!NOTA]
> Se definen principalmente en las vistas y las rutas
#### get

- Se usa para obtener información.

**Como se define desde las vistas**
**Simplemente se llama la ruta que tiene el método get**
**No es común que se use en los formularios**

```
<a href="{{route('ventas.mostrar')}}" type="button">Mostrar Ventas</a>

<a href='{{route('productos.destruir', $producto->CODIGO_PRODUCTO)}}' type="submit">Eliminar</a>
```

**Como se define en las rutas**

```
Route::get('/ventas/mostrar', [VentasController::class, 'ventasMostrar'])->name('ventas.mostrar');

Route::get('/productos/destruir/{id}', [ProductosController::class, 'productosDestruir'])->name('productos.destruir');
```

- También se puede usar para realizar la eliminación de un registro
#### post

- Se usa para realizar inserciones de datos
- En el formulario, el valor de method tiene que ser **POST**

**Como se define desde las vistas**

```
<form action="{{route('productos.guardar')}}" method="POST">
     @csrf

     <div class='mb-3'><label>Codigo del producto</label><input type="text" name="productCode"></div>
     <div class='mb-3'><label>Nombre</label><input type="text" name="productName"></div>
     <div class='mb-3'><label>Precio</label><input type="text" name="productPrice"></div>
    <div class='mb-3'><label>Cantidad</label><input type="text" name="productAmount"></div>

    <button class="btn btn-success" type="submit">Agregar al registro</button>
	<a href='{{route('productos.disponibles')}}' type="submit">Cancelar</a>

  
</form>
```

**Como se define en las rutas**

```
Route::post('/productos/guardar', [ProductosController::class, 'productosGuardar'])->name('productos.guardar');
```
#### put o patch

- put se usa para realizar una actualización completa de un registro
- patch se usa para realizar actualización solo en los campos indicados de un registro.

- En el formulario, method tiene que ser **POST**, ya que put o patch no son argumentos válidos para el parámetro method del formulario.
- Para usar put o patch se ingresa una directiva dentro del formulario, igual que @csrf
- **Se agrega @method('PUT') o @method('PATCH')** 

**Como se define desde las vistas**

```
<form action="{{route('productos.actualizar', $producto->CODIGO_PRODUCTO)}}" method="POST">

	@csrf
	@method('PUT')

	<div class='mb-3'><label>Codigo del producto</label><input type="text" name="productCode" value='{{$producto->CODIGO_PRODUCTO}}' readonly></div>
	<div class='mb-3'><label>Nombre</label><input type="text" name="productName" value='{{$producto->NOMBRE}}'></div>
	<div class='mb-3'><label>Precio</label><input type="text" name="productPrice" value='{{$producto->PRECIO}}'></div>
	<div class='mb-3'><label>Cantidad</label><input type="text" name="productAmount" value='{{$producto->STOCK}}'></div>
	
	<button class="btn btn-success" type="submit">Actualizar datos</button>
	<a href='{{route('productos.disponibles')}}' type="submit">Cancelar</a>

</form>
```

**Como se define en las rutas**

```
Route::put('/productos/actualizar/{id}', [ProductosController::class, 'productosActualizar'])->name('productos.actualizar');
```
#### delete

- Eliminar información del sistema.
- No es común que se envíen formulario con el método delete, pero podría agregarse en un formulario la directiva **@method('delete')**

**Como se define desde las vistas**

```
<form action="{{ route('productos.destroy', $producto->id) }}" method="POST">
    @csrf
    @method('DELETE')

    <button type="submit" class="btn btn-danger">Eliminar Producto</button>
</form>

```

- O simplemente se puede ejecutar la ruta por medio de un botón.

**Como se define en las rutas**

```
Route::delete('/productos/{id}', [ProductoController::class, 'destroy'])->middleware('auth');
```

### Ejemplo de uso de rutas vistas y directivas

**Ruta que envía datos a la vista:**

```
Route::get('/example', function(){

    $nombre = 'Estiven';
    $apellido = 'Mejia';
    $edad = 23;

    return view('layouts.example', [
                                    'nombre' => $nombre,
                                    'apellido' => $apellido,
                                    'edad' => $edad]);
});
```

**Vista que usa directivas y toma los valores recibidos desde la función de la ruta:**

```
@extends('head')

<body>
    <h1>Hola {{$nombre}}, bienvenido</h1>
    
    @section('content')
        Tu nombre es {{$nombre}} {{$apellido}} <br>
        Tu edad es {{$edad}}
    @endsection
    
    @yield('content')
</body>

@extends('footer')
```


 


















