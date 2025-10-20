
# Como Eliminar un Registro

- Si agregamos un enlace que tiene como propósito eliminar un registro, pero hay una ruta tipo get con una ruta similar, se ejecutará la ruta GET y no la DELETE.

- Esto ocurre porque las etiquetas enlace, al redirigir a otra vista está haciendo una petición GET.
- **Para solucionar este problema, la redirección para eliminar un registro se tiene que hacer dentro de un formulario.**

```
<h2>
	<a href="{{route('getpost', ['id' => $post->id])}}">Ir a {{$post->titulo}}</a> 
	<a href="{{route ('post.edit', ['id' => $post->id])}}">Editar</a>

	<form action="{{route ('post.delete', ['id' => $post->id])}}" method="POST">
		@csrf
		@method('DELETE')
		<button type="submit">Eliminar</button>
	</form>
</h2>
```

**En el controlador, el método luciría así:**

```
public function delete(Post $id){

	$id->delete();
	return to_route('blog')->with('respuesta', 'Registro eliminado');
}
```













