# Expresiones Regulares en Laravel
-----

- Son patrones de búsqueda y manipulación de texto utilizados para validar y filtrar datos de manera precisa.
- Laravel proporciona una serie de métodos y utilidades para trabajar con expresiones regulares de manera conveniente. 

- Se pueden utilizar expresiones regulares para realizar tareas como la validación de campos de entrada, la extracción de información específica de una cadena de texto, la búsqueda y reemplazo de patrones en cadenas, entre otros usos.

### Validación de variables en las rutas

**Ejemplo de una expresión que valida una variable enviada por la ruta:**

```
Route::get('/expresion/{valor}', function($valor){
    return $valor;
})->where('valor', '[0-9]+');
```

**Ejemplo con dos variables**

```
Route::get('/expresion/{valor}/{valor2?}', function($valor, $valor2 = null){
    return $valor . ' ' . $valor2;
})->where(['valor' => '[0-9]+', 'valor2' => '[A-Z]+']);
```
### Validación de variables en los controladores












