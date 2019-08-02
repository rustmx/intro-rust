class: center, middle

<img src="../assets/images/rustmx-logo.svg" alt="RustMX" width="250rem" height="auto">

# Sesión 9: 
## Vectores

---
## Intro
Rust incluye algunas colecciones en su librería estándar.

Estos datos se guardan en el `heap` de la memoria.

* **vectores** - `Vec` almacena valores de distinto tipo juntos uno del otro
* **cadenas** - `String` una coleccion de caracteres
* **hash maps** - `HashMap` asocia valores con una llave particular

.

Pero existen mas en el modulo de collecciones
  - [std::collections](https://doc.rust-lang.org/std/collections/index.html)

---
## `Vec<T>`
Almacena varios valores de **un mismo tipo** en una estructura de datos que pone todos los elementos juntos en memoria.

`let v: Vec<i32> = Vec::new();`

Usa **genericos** pero es mas comun usarlo dejando que Rust infiera los tipos con los primeros valores.

```
let mut v = vec![1, 2, 3];
v.push(5);
v.push(6);
let tercero: &i32 = &v[2];   // indice empieza en `cero` (puede causar Panics)
println!("Tercer elemento con posicion: {}", tercero);

// `get()` devuelve un tipo `Option<&T>`
match v.get(2) {
  Some(tercero) => println!("Tercer elemento con match: {}", tercero),
  None => println!("No hay tercer elemento"),
}

if let Some(tercero) = v.get(2) {()}
println!("Tercer elemento con if let: {}", tercero);
```

Cuando se libera la **memoria** de un vector, se liberan todos sus elementos.

---
## Vectores y referencias
Las mismas reglas de `ownership` y `borrowing` aplican a Vectores

```
let mut v = vec![1, 2, 3, 4, 5];
let first = &v[0];   // referencia no mutable
v.push(6);           // referencia mutable causa `error` : `first` sigue en este scope
println!("The first element is: {}", first);
```

mejor

```
let mut v = vec![1, 2, 3, 4, 5];
let first = &v[0];
println!("The first element is: {}", first);
v.push(6);   // 👍
```

El error es porque `Vec` al agregar un elemento, puede reservar nueva memoria para copiar todos los elementos a un nuevo espacio de forma que se mantengan juntos.

---
## Iterar un Vector
```
let v = vec![100, 32, 57];
for i in &v {
    println!("{}", i);
}
```

Tambien sobre referencias mutables para cambiar sus valores
```
let mut v = vec![100, 32, 57];
for i in &mut v {
    *i += 50;  // `*i` se lee como `dereference operator`
}
```

**Nota:**
- Es posible usar enums con vectores para guardar multiples vectores