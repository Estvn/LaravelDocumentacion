# Etiquetas Especiales en Laravel

## **\<x-alert/>**

- Cuando creamos un nuevo componente por medio del comando **Artisan**, en la carpeta **\App\View** se generará una carpeta llamada **Components** que contendrá todos los componentes creados por medio del comando Artisan.
- Estos archivos sirven para renderizar los componentes.

- **Crear componentes por medio del comando Artisan nos permite usar etiquetas especiales.**
- Si ha creado un componente llamado "Peligro" podrá usar la etiqueta especial llamada **"\<x-peligro/>"** que se encarga de renderizar el contenido del componente en el lugar donde se defina la etiqueta.

## **Paso de variables a la etiqueta especial**

- Cuando definimos parámetros en la etiqueta especial, estos parámetros pasan por el constructor del componente, y luego pasan por la vista perteneciente del componente, para devolver el componente renderizado hacia el archivo donde fue llamado.

#### **Paso de valores sencillo con la etiqueta especial

- En la etiqueta especial se define un atributo de etiqueta y se iguala a un contenido quemado.
- Este valor es recibido en los parámetros del constructor del componente, cuando se guarde en una variable de la clase del componente puede ser enviado a la vista de la alerta
- En la vista de la alerta, las variables del componente se definen con el nombre que se les asignó en el constructor de la clase componente.

**Enviando valor desde la etiqueta especial:**

```
<x-alerta type="Este es el contenido de la variable simple"/>
```

**Recibiendo y guardando variable en el constructor del componente Alerta**

```
public $type;
public function __construct($type)
{
	$this->type = $type;
}
```

**Recibiendo contenido del constructor en la vista del componente**

```
<div>
    <h1>Este es el componente renderizado con la etiqueta especial </h1>
    {{$type}}
</div>
```

#### **Paso de variables usando la etiqueta especial

- Se puede definir una variable en la etiqueta especial para pasarla a la vista de su componente.

- Primero, se envía una variable desde la ruta que llama a la vista donde se encuentra la etiqueta especial.

```
Route::get('/example', function(){
    $nombre = 'Estiven';
    $apellido = 'Mejia';
    $edad = 23;

    return view('layouts.example', [
                                    'nombre' => $nombre,
                                    'apellido' => $apellido,
                                    'edad' => $edad,
                                    'variable' => 'Contenido de la variable enviada desde la ruta'
                                    ]);
});
```

- La etiqueta especial pone la variable de la ruta en sus parámetros.

```
<x-alerta type="error" :variable="$variable"/>
```

- La variable pasa a la clase del componente para guardarla en su constructor.

```
class Alerta extends Component
{
    public $type;
    public $variable;
    public function __construct($type, $variable)
    {
        $this->type = $type;
        $this->variable = $variable;
    }
```

- La vista del componente finalmente recibe la variable enviada desde la ruta.

```
<div>
    <h1>Este es el componente renderizado con la etiqueta especial </h1>
    {{$type}} <br>
    {{$variable}}
</div>
```

## **Métodos en los Componentes**

- Se pueden crear métodos en las clases de los componentes que realicen una tarea en específico.
- **Estos métodos pueden ser llamados directamente desde la vista del componente.**

**Llamando al método desde la vista del componente**

```
<div>
    <h1>Este es el componente renderizado con la etiqueta especial </h1>

    @foreach ($lenguajes('Java') as $lenguaje)
        <h4>{{$lenguaje}}</h4>
    @endforeach
</div>
```

**Método en la clase del componente**

```
public function lenguajes($lengua){
	return ['c++', 'c#', 'kotlin', 'python', $lengua];
}
```

























