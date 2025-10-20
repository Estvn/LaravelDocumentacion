
laravel.com/docs/validation
# Validación de Datos en Laravel
--------

- Es un proceso fundamental para asegurarnos que los datos que están ingresando en la aplicación cumplan con ciertos criterios antes de ser procesados o almacenados en una base de datos.
- Laravel proporciona un conjunto poderoso de herramientas para realizar la validación de manera clara y concisa.
- **Las validaciones normalmente van en los controladores.**

**Ejemplo de una función con lógica de validación:**

```
public function store(Request $request){
	$validated = $request->validate([
		'email' => ['required, unique:users, email, bail'],
		'name' => ['required],
	]);
}

public function store(Request $request){
	$validated = $request->validate([
		'email' => 'required|unique:users|email|bail',
		'name' => 'required',
	]);
}
```

# Mostrar Errores de Validación en las Vistas
-------------

- Las validaciones del lado del servidor pueden quitarse por un usuario mal intencionado y enviar información incorrecta a la base de datos.
- Es importante realizar validaciones del lado del servidor para evitar este tipo de problemas.
- **Cuando las validaciones del lado del servidor han fallado, se retorna a la vista del formulario que causó el error, pero si no envía mensajes del error no se notará que ha habido un problema.**

- También es importante mostrar los errores que causa el envío de datos incorrectos para que el usuario sepa en que ha fallado.


**Se muestra la impresión de los errores**

```
@dump($errors->all())

@foreach ($errors->all as $error)
    <p>$error</p>
@endforeach

<form action="{{route('post.guardar')}}" method="POST">

    @csrf

    <label for="">Titulo</label>
    <input type="text" name="titulo"/>

    @error('titulo')
        <br>
        <small style="color:red;">{{$message}}</small>
    @enderror

    <br><br>

    <button type="submit">Enviar</button>
</form>
```

#### Mantener los valores en los campos al mostrar un error

- Si la validación ha fallado la página se va a reiniciar mostrando los mensajes de error, pero los campos del formulario se van a resetear.

- Para mantener el contenido en los formularios se agrega el siguiente método en los input

```    
<input type="text" name="titulo" value="{{old('titulo')}}"/>
```







