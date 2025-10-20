
# Form Request Laravel
---------

```
php artisan make:request NombreRequest
```

- En este archivo se incluirá la verificación de las autorizaciones que tiene un usuario.
- Este archivo también se encargará de realizar las validaciones de la información que se desee alterar en la base de datos, es decir, **la respuesta del formulario deberá pasar por este archivo.**

- Los archivos creados con el comando anterior se almacenarán en la ruta del archivo **\Http\requests**
- El archivo Request contiene dos métodos por defecto
	- authorize() contiene la lógica de las autorizaciones.
	- rules() contiene las validaciones de los campos.

## Método rules()

**Las validaciones en el documento Request lucen de la siguiente forma:**

```
public function rules(): array
{
	if($this->isMethod('PATCH') || $this->isMethod('POST')){
		return [
			'titulo' => ['required', 'min:2']
		];
	}
}
```

**En el controlador de llama y se usa de la siguiente forma:**

```
public function store(SavePostResquest $request){

	Post::create($request->validated());
	return redirect()->route('blog')->with('respuesta', 'Post Created!');
}
```

- En lugar de Request, en los parámetros se llama al archivo que creamos para manejar la petición.
- Para guardar la información usamos el método **validated()**









