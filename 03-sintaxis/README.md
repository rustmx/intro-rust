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
## Cadenas
Cadenas literales : `let x: &str = "una prueba"`
 - No mutables
 - Se conoce su valor desde tiempo de compilación
 - Su valor se vuelve parte del código ejecutable, no en la memoria.

`String`
 - Es mutable
 - Se reserva en el Heap de la memoria
 - Su valor se libera cuando sale del _alcance_

```
{
  let hola: &str = "hola"; // `hola` es válida de aquí en adelante
  let cadena = String::from(hola); // `cadena` es válida de aquí en adelante
  // ... 
} // Rust mando limpiar `cadena` y `hola` aquí
// `cadena` y `hola` ya no son válidas
```

---
## String Slice
Permite hacer referencias a una secuencia de elementos dentro de una colección, más que la colección completa.

La nomenclatura del tipo de dato Slice es `&str`.

```
let cadena = String::from("hello world");
let hello: &str = &cadena[..5]; // Es lo mismo que `&s[0..5]`
let world = &cadena[6..]; // Es lo mismo que indicar el último índice
```
<center><img src="https://doc.rust-lang.org/book/img/trpl04-06.svg" width="40%"></center>

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
## Panic

**Panic** es el termino que Rust usa cuando un programa termina con error.

![Panic](https://static.javatpoint.com/tutorial/rust/images/rust-recoverable-errors3.png "Panic")

Se puede lanzar un Panic de forma manual:

```
panic!("This is a Panic :)");
```
