
# Validación de Datos en Laravel
--------

- Es un proceso fundamental para asegurarnos que los datos que están ingresando en la aplicación cumplan con ciertos criterios antes de ser procesados o almacenados en una base de datos.
- Laravel proporciona un conjunto poderoso de herramientas para realizar la validación de manera clara y concisa.
- **Las validaciones normalmente van en los controladores.**

**Ejemplo de una función con lógica de validación:**

```
public function store(Request $request){
	$validated = $request->validate([
		'email' => ['required, unique:users, email, bail'],
		'name' => ['required],
	]);
}

public function store(Request $request){
	$validated = $request->validate([
		'email' => 'required|unique:users|email|bail',
		'name' => 'required',
	]);
}
```










