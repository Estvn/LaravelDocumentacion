
Usuario -> Vsita -> Ruta -> Controlador - > Modelo -> DB

- Laravel Blade Snnipets
- PHP Intelephense
- PHP Intellisense
- PHP Extension Pack

### Evitar error de Csrf durante pruebas de APIs

- En la carpeta bootstrap/app.php tiene que agregar este código:

```
->withMiddleware(function (Middleware $middleware) {
        $middleware->validateCsrfTokens(except:[
            'http://127.0.0.1:8000/productos',
            'http://127.0.0.1:8000/productos/3'
        ]);
    })
```

- En las excepciones agrega las direcciones que no quiere que pasen por la verificación csrf.
- Esto solo se hace durante pruebas, en los entornos de producción no puede haber direcciones que omitan la verificación csrf.