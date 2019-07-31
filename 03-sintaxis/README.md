class: center, middle

<img src="../assets/images/rustmx-logo.svg" alt="RustMX" width="250rem" height="auto">

# Sesión 3: 

### Tipos de datos

---
## Intro

Rust debe saber el tipo de dato de cada variable en tiempo de compilacion para poder operar.

```
let foo: u32 = 4; // es entero sin signo de 32 bits
```

.

Sin embargo puede inferir el tipo basado en el valor y en como se utiliza.

```
let foo = 4.0; //es flotante
let foo = 4;  // es entero
```

---
## Escalares

Representan un valor simple de cuatro posibles:

* **Entero**
  - Son de longitud 8, 16, 32, 64 y 128 bits
  - Se definen como `i8`,`i16`, `i32`, `i64` y `i128` y sin signo `u8`, `u16`, `u32`, `u64` y `u128`.
  - `isize` y `usize` es para depender de la arquitectura.
  - `Integer Overflow` es cuando por ej. un tipo u8 pasa de valor 255 a 256
     - Causa un `Panic` en modo debug (default)
     - Otros modos de Rust hacen un `two's complement wrapping`, que convierte el 256 en 0, el 257 en 1 y asi sucesivamente 
* **Punto flotante:**  `f32` y `f64` _(default)_
* **Logico:**  `bool` y puede ser `true` o `false`
* **Caracter**
  - Se especifica con comillas simples `'😻'`.  Representa un valor Unicode.
  - Valores entre `U+0000` a `U+D7FF` y `U+E000` a `U+10FFFF` 

_Note:_ Las operaciones matematicas clasicas son suma (+), resta (-), multiplicacion (*), division (/) y residuo (%)


---
## Compuestos 1/2
Son tipos de dato que pueden agrupar multiples valores.

### Tuplas
 - Agrupar valores de tipos de dato distintos
 - Tamaño fijo
 - Se puede usar `pattern matching` a traves de `destructuring`

```
  let tupla = (500, 9.3, 'z', "cadena");
  println!("Tupla {:?}", tupla);

  let (entero, flotante, caracter, cadena) = tupla;
  println!("entero : {}", entero);
  println!("flotante : {}", flotante);
  println!("caracter : {}", caracter);
  println!("cadena : {}", cadena);

  let dos_enteros: (i32, u8) = (500, 1);
  println!("Dos enteros con indice : {} y {}", dos_enteros.0, dos_enteros.1);
```
[Demo](https://repl.it/@wdonet/rust-tuples)

---
## Compuestos 2/2
### Arreglos
 
 - Agrupar valores del mismo tipos de dato.
 - Tamaño fijo.
 - Indice comienza en cero.
 - Utiles parar el `Stack` en lugar del `Heap`.

 ```
  let a = ["second", "minute", "hour", "day", "month", "year"];
  println!("Arreglo a {:?}", a);

  let b:[u8; 3] = [10, 20, 30]; // indica tipo de dato y tamaño del arreglo
  println!("Elemento b[0] {}", b[0]);
  println!("Elemento b[1] {}", b[1]);
  println!("Elemento b[2] {}", b[2]);
```
[Demo](https://repl.it/@wdonet/rust-arrays)

_Nota:_ Un `vector` es una coleccion similar que puede crecer en tamaño.

---
## Struct 1/3
Hay tres tipos de estructuras que pueden ser creadas con `struct`:

### Tuple struct
Son basicamente tuplas nombradas.

```
struct Pair(i32, f32);
let pair = Pair(1, 0.1);
println!("El par contiene {:?} y {:?}", pair.0, pair.1);
let Pair(integer, decimal) = pair;   // destructuring
```

---
## Struct 2/3
### C struct (classic)
```
struct Point {
    x: f32,
    y: f32,
}
let point: Point = Point { x: 0.3, y: 0.4 };
println!("coordenadas: ({}, {})", point.x, point.y);
let second_point = Point { x: 0.1, ..point };
println!("otras coordenadas: ({}, {})", second_point.x, second_point.y);
let Point { x: my_x, y: my_y } = point;  // destructuring
```

---
## Struct 3/3
### Unit struct
Sin campos pero utiles para genericos.

```
struct Nil;
let _nil = Nil;
```

Las estructuras se puede re utilizar en otra estructura.
```
struct Rectangle {
  origin: Point,
  target: Point,
}
```

---
## Panic

**Panic** es el termino que Rust usa cuando un programa termina con error.

![Panic](https://static.javatpoint.com/tutorial/rust/images/rust-recoverable-errors3.png "Panic")

Se puede lanzar un Panic de forma manual:

```
panic!("This is a Panic :)");
```

---
## Enum 1/4
Define un tipo al enumerar sus posibles valores.
```
enum IpAddrKind {
    V4,
    V6,
}
struct IpAddr {
    kind: IpAddrKind,
    address: String,
}
let home = IpAddr {
    kind: IpAddrKind::V4,
    address: String::from("127.0.0.1"),
};
```
Se puede usar sin estructuras.
```
enum IpAddr {
    V4(String),
    V6(String),
}
let home = IpAddr::V4(String::from("127.0.0.1"));
```

---
## Enum 2/4
Aun mejor, se pueden tener distintos valores en cada variante de la enumeracion

```
enum IpAddr {
    V4(u8, u8, u8, u8),
    V6(String),
}
let home = IpAddr::V4(127, 0, 0, 1);
```
Es posible definir funciones con `impl`.
```
impl IpAddr {
  fn to_string(&self) -> String {
    let IpAddr::V4(a, b, c, d) = &self;
    return format!("{}.{}.{}.{}", a, b, c, d);
  }    // error?
}
println!("Mi ip es {:?}", home.to_string());
```

---
## Enum 3/4
### [Option](https://doc.rust-lang.org/std/option/enum.Option.html) [<img src="https://www.worksafe.tas.gov.au/__data/assets/file/0005/305375/hatmono.svg" width="32"> Safety ]
Representa un escenario comun en el que un valor devuelto puede ser algo o nada.
No existe el concepto de  `Null` o `Nil`

- Es parte de la libreria estandar.
- El compilador checa si se estan manejando los posibles casos.
 
```
enum Option<T> {
  Some(T),
  None,
}
let some_number = Some(5);
let some_string = Some("a string");
let absent_number: Option<i32> = None;
```
Compilation error
```
let x: i8 = 5;
let y: Option<i8> = Some(5);
let sum = x + y;  // Error : expected i8, found enum `std::option::Option`
```
---
## Enum 4/4
### [Result](https://doc.rust-lang.org/nightly/std/result/enum.Result.html)

Muchas funciones de I/O lo devuelven para retornar y propagar errores.

```
enum Result<T, E> {
    Ok(T),  // Contiene el valor en la operacion exitosa
    Err(E), // Representando un valor para el error
}
```
Ejemplo:
```
let ok: Result<i32, &str> = Ok(-3);
assert_eq!(ok.is_ok(), true);

let error: Result<i32, &str> = Err("Some error message");
assert_eq!(error.is_err(), true);
```

`Result` es una buena solucion para recuperarse de errores que usan la macro [panic!](https://doc.rust-lang.org/std/macro.panic.html). 
