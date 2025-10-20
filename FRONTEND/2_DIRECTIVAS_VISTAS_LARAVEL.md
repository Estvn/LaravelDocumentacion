
# Directivas en Laravel

### @extends e @include

- **@extends('carpeta.archivo')** establece una relación padre-hijo sobre la vista donde se va a incluir y mejora el rendimiento al trabajar con @sections y @yield.
- **@include('carpeta.archivo')** solo copia y pega el contenido de una vista en otra, es decir, reutiliza fragmentos de otras vistas e inserta el contenido sin ninguna optimización.
- **@extends** se usa para fragmentos de código que son dinámicos, porque mejora la optimización del uso de algunas directivas.
## **@section @parent y @yield** 
### @yield

- **@yield('content')** muestra el contenido que se encuentra entre las etiquetas **@section('content')**  o **@endsection** que se encuentra en otro archivo

```
@yield('title')
```

### @section

- **@section()** se puede usar en una sola línea o anidando más contenido
- una sección de código html que se encuentra en las directivas **@section('nombreContent)** y **@endsection**.

> [!NOTA]
> El contenido de estas etiquetas no se mostrará hasta que se llame la sección por medio de un @yield

```
@section('title', 'este es el título')
```

```
@section('contenido')
...
@endsection
```

### @parent

- Se puede usar dentro de las etiquetas @section, en caso de que la sección se llene de contenido en otro archivo, si se agrega @parent no se pierde el contenido original

```
    @section('padre')
        <p>Linea original</p>
    @endsection
```

```
@section('padre')
    @parent
    <p>Esta linea deberia reemplazar el contenido original</p>
@endsection

@yield('padre)
```
 
### @csrf

- **@csrf** se agregar dentro y al inicio de los formularios, es una medida de seguridad.

```
<form action="" method="">
	@csrf
	...
</form>
```

## **@component y @slot**


> [!NOTA] Componentes desde la consola
> Lo genera en la carpeta components.
> ```
> php artisan make:component Alert

- Se agrega en el código para generar una sección con contenido dinámico.
- En la parte donde se define se generará el contenido dinámico.

- La sección **@component** llama al contenido de otro archivo que se mostrará en lugar de lo que se encuentra en @component.
- En @component se definen valores para poder enviarlos al archivo que va a llamar, para poder cargar la estructura de ese archivo con el contenido indicado dentro de @component.

- **@slot** genera contenidos anidados dentro de @component, se pueden utilizar cuando se llama a una variable con el nombre que se indica en los parámetros de @slot.

```
{{-- Se va a renderizar el contenido del archivo alert --}}
@component('componentes.alert', ['var' => 'una variable'])

	@slot('title')
		Advertencia
	@endslot

	<p>Esto es un mensaje de alerta</p>
@endcomponent
```

- El contenido dentro de @component se renderiza en el archivo alert con el nombre de variable **$slot**.
- Los **@slot** anidados dentro de @component se renderizan el archivo alert con el nombre de variable indicado en sus parámetros (en este caso la variable de @slot('title') será **$title**)
- La variable enviada desde los parámetros de @component se usará en el archivo alert con el mismo nombre con el que se ha enviado.

```
<section>
    <br>
    <h2>{{$title}}</h2>
    {{$slot}}
    {{$var}} <hr>
</section>
```

- Las variables que refieren a slots dentro de un componente son conocidas como **propiedades del componente**.

> [!NOTA]
> En el archivo de etiquetas especiales puede ver la forma en la que se usan los componentes en Laravel 11.
# Inyectar y Evitar la Inyección de Código en Vistas

### Evitar inyección de código desde escapes de PHP

- Con las etiquetas de escape para inyectar código PHP **"\<?= ?>"** se puede realizar inyección de código malintencionado.

```
<?= "<script>alert('Mensaje inyectado!')</script>" ?>
```

- Usando la función **e()** de PHP  se evita la inyección de código por medio de estos escapes

```
<?= e("<script>alert('Mensaje no inyectado')</script>") ?>
```

### Inyectar inyección de código desde escapes Blade

- Los dobles corchetes de Blade "{{$var}}"  internamente se parsean como un escape de PHP dentro de una función e(), por lo tanto, estos evitan la inyección de código malicioso.

```
{{"<script>alert('Esta alerta no se ejecutará!')</script>"}}
```

- Puede existir el caso donde sea necesario inyectar código en el programa, por lo tanto, esta es la sintaxis para permitirlo

```
{{!! "<script>alert('Esta alerta SI se ejecutará!')</script>" !!}}
```

> [!NOTA]
> Los códigos maliciosos que no logran ejecutarse, simplemente se imprimirán en pantalla.
