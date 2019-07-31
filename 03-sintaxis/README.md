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

---
## Match 1/2
Compara un valor contra una serie de patrones y asi ejecuta un codigo para cada caso.

- Un patron puede usar literales, variables o hasta wildcards (*)
- `_` es un placeholder usado para empatar con el resto de posibilidades

```
        enum UsState {                     enum Coin {
            Alabama,                           Penny,
            Alaska,                            Nickel,
            Houston,                           Dime,
            // and more                        Quarter(UsState), 
        }                                  }

fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => {
            println!("Lucky penny!");
            1
        },
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter(state) => {
            println!("State quarter from {:?}", state);
            25
          },
    }
}
```

---
## Match 2/2
Obtener el valor interno `T` de `Some` en caso de usar `Option<T>`

```
fn plus_one(x: Option<i32>) -> Option<i32> {
    match x {
        None => None,    // compilaria sin esta linea?
        Some(i) => Some(i + 1),
    }
}

let five = Some(5);
let six = plus_one(five);
let none = plus_one(None);
println!("Valores {} y {}", six, none);
```

El patron `_` debe usarse al final y empata todos los casos posibles no especificados.
Usualmente usado con _(unit value)_ `()` que significa no hacer nada.

```
let some_u8_value = 0u8;
match some_u8_value {
    1 => println!("one"),
    3 => println!("three"),
    7 => println!("seven"),
    _ => (),
}
println!("El valor es {}", some_u8_value);
```

---
## if let
Permite combinar `if` y `let` para manejar valores que empatan con un solo patron e ignorando el resto.

```
if let Some(3) = some_u8_value {
  "three"
}
println!("El valor es {}", some_u8_value);
```

`if let` es para escribir/indentar menos, es un _syntax sugar_ para un `match`.

Tambien se puede usar un caso `else` para el resto de patrones.

```
let mut count = 0;
if let Coin::Quarter(state) = coin {
    println!("State quarter from {:?}!", state);
} else {
    count += 1;
}
```