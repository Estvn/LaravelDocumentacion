
# Mensaje de Sesión en Laravel
---------

- Cuando se ingresa un registro en la base de datos, por ejemplo, en lugar de solo hacer un retorno se puede enviar un mensaje de la respuesta.
- Use el método **session()-flash('respuesta', 'mensaje')**

**Usando un método para enviar la respuesta desde el controlador y retornarlo con una redirección**

```
public function store(Request $request){

	$post = new Post();
	$post->title = $request->input('titulo');
	$post->save();

	session()->flash('respuesta', 'Post created!');
	return redirect()->route('blog');
}
```

#### Uso del método **with()** para enviar el mensaje

- En lugar de usar session()->flash(), se puede reducir el código usando el método with() para enviar la respuesta en la redirección.

```
public function store(SavePostResquest $request){

	Post::create($request->validated());
	return redirect()->route('blog')->with('respuesta', 'Post Created!');
}
```

- Como puede ver, ahora el mensaje de la acción completada se envía en la redirección.

**En las vistas se puede obtener el mensaje de las siguiente dos maneras:**

```
@if (session('respuesta'))
	<div class='status'>
		{{ session('respuesta') }}
	</div>                
@endif
```

```
@session('respuesta')
	<div class='status'>
		{{ $value }}
	</div>                
@endsession
```








