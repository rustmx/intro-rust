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
  - `Integer Overflow` es cuando un tipo u8 pasa de 255 a 256 como valor
     - Causa un `Panic` en modo debug
     - Rust hace un `two's complement wrapping`, convierte el 256 en 0, el 257 en 1 y asi sucesivamente 
* **Punto flotante:**  `f32` y `f64` _(default)_
* **Logico:**  `bool` y puede ser `true` o `false`
* **Caracter**
  - Se especifica con comillas simples `'😻'`.  Representa un valor Unicode. 

_Note:_ Las operaciones matematicas clasicas son suma (+), resta (-), multiplicacion (*), division (/) y residuo (%)


---
## Compuestos
Son tipos de dato que pueden agrupar multiples valores.

### Tuplas
### Arreglos
---
## Intro

---
## Intro


