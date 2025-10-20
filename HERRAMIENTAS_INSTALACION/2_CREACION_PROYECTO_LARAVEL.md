
# Creación de un proyecto en Laravel
-------------

- Una vez realizadas todas las instalaciones necesarias, ya puede crear un proyecto de Laravel.
- Haremos la creación de un proyecto de Laravel en el SO Windows.
- Antes de crear un proyecto de Laravel, nos tenemos que ubicar desde la cmd/terminal en la carpeta donde estará el proyecto de Laravel.
-  El comando para crear un proyecto creará una carpeta, y dentro se encontrarán todos los archivos y dependencias necesarias para hacer funcionar un proyecto de Laravel.

Comandos para crear proyecto de Laravel:
```
laravel new {nombre del proyecto}
```

![[Pasted image 20241212202808.png]]

Al momento de ejecutar el comando que crea un proyecto de Laravel, antes de su creación pregunta cuál kit desea implementar en su proyecto:

- **Laravel Breeze**
	- Es una implementación simple y minimalista de autenticación en Laravel.
	- Proporciona autenticación básica (registro, inicio de sesión, restablecimiento de contraseña).
	- Usa a blade como un motor de plantillas por defecto.
	- Puede integrar a Tailwind CSS para estilos.
	- Es ideal para proyectos pequeños, o para integrar funcionalidades sin tener que crear la lógica desde cero.
	
- **Laravel Jetstream**
	- Sistema de scaffolding (técnica para hacer todo un CRUD) más avanzado y completo. 
	- Incluye todo lo que ofrece Laravel Breeze.
	- Gestión de equipos.
	- API con soporte de Laravel Sanctum.
	- Sesiones de usuario.
	- Interfaz de usuario basada en Livewire o Inertia.js
	- Ofrece más funcionalidad lista para usar en proyecto que necesiten características complejas.
	- Se usa en proyectos medianos o grandes donde se necesita autenticación avanzada, gestión de equipos o soporte para SPA (aplicaciones de una sola página).

Antes de crear el proyecto, también preguntará que Framework de testeo se prefiere utilizar:

- **PHPUnit**
	- Es el Framework de pruebas más popular en PHP y el predeterminado para Laravel.
	- Ofrece un enfoque tradicional basado en clases y métodos para escribir pruebas.
	- Es robusto, ampliamente utilizado, y tiene una extensa documentación.
	- Perfecto para proyectos grandes o si está acostumbrado a un estilo más formal y estructurado.
	- Compatible con herramientas de terceros.
	
- **Pest**
	- Un Framework moderno y minimalista basado en PHPUnit, diseñado para ser más intuitivo y amigable.
	- Usa una sintaxis más simple y expresiva.
	- Está optimizado para desarrolladores que prefieren un enfoque menos formal y más legible.
	- Tiene soporte para plugins y extensiones.

![[Pasted image 20241212202835.png]]

Durante la instalación preguntará que gestor de base de datos se usará para el proyecto (no seleccione ninguna):
- SQLite
- MySQL
- MariaDB
- PostgreSQL
- SQL Server

![[Pasted image 20241212203249.png]]

- Para ejecutar el proyecto desde la consola sugiere los siguiente comandos (tiene que estar ubicado en la carpeta donde se creó el proyecto):

```
cd {nombre del proyecto}
npm install && npm run build
composer run dev
```

- Otra forma de ejecutar un proyecto en Laravel, es dirigirse a localhost/carpeta de proyecto/public/index.php (si está usando Xampp).





