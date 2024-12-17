# Redirecciones 301 y 302 en Laravel
------

- A veces tenemos la necesidad de poder redirigir una URL en nuestra web. Si ya no necesitamos estar en una vista podemos hacer una redirección hacia otra URL de nuestra misma web.

```
Route::redirect('/origen', '/destino');
```

- Las redirecciones nos permiten indicar a un motor de búsqueda que nuestra redirección va a ser temporal o permanente. 

**Formas de hacer una redirección de tipo permanente:**
```
Route::redirect('/origen', '/destino', 301);
Route::permanentRedirect('/origen', '/destino');
```

- 301 es una redirección permanente. -> Se guarda en caché.
- 302 es una redirección temporal. -> No se guarda en caché.

**Ejemplo práctico:**

```
Route::redirect('/origen', '/destino', 301);
Route::get('/destino', function(){
    return "Estoy en la ruta destino";
});
```
## Otra forma de redireccionar

```
Route
```












