class: center, middle

<img src="../assets/images/rustmx-logo.svg" alt="RustMX" width="250rem" height="auto">

# Sesión 4: 
## Structs & Enums

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
Es posible adjuntar funciones a objetos con `impl`.
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


