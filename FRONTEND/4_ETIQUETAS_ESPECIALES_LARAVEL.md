# Etiquetas Especiales en Laravel

## **\<x-alert/> ó \<x-alert>...\<x-alert/>**

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

> [!WARNING]
> Si el componente se encuentra en otra carpeta a parte de components, se tiene que referenciar de la siguiente forma:
> .
> ```
> \<x-carpeta.archivoComponente/>> 

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

## **Definiendo propiedades del componente con etiquetas especiales**

- La etiquetas especiales puede usarse para hacer componentes de la siguiente manera

```
<x-ejemploComponente> Contenido del componente </x-ejemploComponente>
```

- Y en el archivo del componente, el contenido ingresado dentro de las etiquetas se imprime de la siguiente manera

```
<body>
	{{$slot}}
</body>
```

**Las propiedades dentro del componente se puede definir de dos maneras:

```
<x-slot name="slot1">
	Este es el contenido del slot 1
<x-slot/>
```

- La alternativa más simple

```
<x-slot:slot2>
	Este es el contenido del slot 1
<x-slot:slot2/>
```

**En ambos casos, los el contenido dentro de estos slot se imprime de esta forma en el archivo del componente:**

```
<body>
	{{$slot}}

	@if(isset($slot1))
		<div> $slot1 </div>
	@endif
	
	@isset($slot2)
		<div> $slot2 </div>
	@endisset
</body>
```

#### Valores de propiedades del componente por defecto

- Si el contenido de la propiedad de un componente llega sin valores, se puede definir un valor por defecto de la siguiente manera:

```
<title> {{$metaTitle ?? 'Título por defecto'}} </title>
```

#### paso de variables desde la etiqueta de componente

- Las variables enviadas directamente desde la etiqueta del componente, igual que a los slot del componente, se llaman por medio de una variable desde el archivo que contiene el cuerpo base del componente.

- Se define de esta manera:

```
<x-layout metatitle='Contenido del título como propiedad del componente'>
	Hola
<x-layout/>
```

- Se imprime de esta forma:

```
<body>
    <title>{{$metatitle ?? 'Título por defecto'}}</title>
	{{$slot}}
</body>
```

- **Se pueden pasar varias variables desde la etiqueta del componente.**

> [!WARNING]
> - Si se define simplemente una variable, su contenido será tratado como texto plano, es decir, si ingresamos la variable "$var" se imprimirá tal cuál.
> - Si agregamos dos puntos ":" antes del nombre de la variable, su contenido será tratado como código y no como texto plano, es decir, se puede enviar variables, operaciones matemáticas y demás valores por medio de esa variable.

```
<x-alerta type="error" :variable="$variable"/>
<x-alerta type="error" variable="$variable"/>
```



