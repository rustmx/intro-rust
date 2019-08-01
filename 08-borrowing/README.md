class: center, middle

<img src="../assets/images/rustmx-logo.svg" alt="RustMX" width="250rem" height="auto">

# Sesión 8: 
## Borrowing

---
## Referencias
Se usan `&` antes de una variable para "prestar" su valor. Se lee como _referencia a un valor_.

```
fn main() {
    let s1 = String::from("hello");
    let len = calculate_length(&s1);
    println!("The length of '{}' is {}.", s1, len);
} // `s1` y `len` se liberan

fn calculate_length(s: &String) -> usize { // `s` es una referencia a `s1`
    s.len()
} // `s` no se libera
```
<center><img src="https://doc.rust-lang.org/book/img/trpl04-05.svg" width="70%"></center>

---
## Referencias Mutables
Se usa la misma nomenclatura `&` pero agregando `mut` para indicar que es mutable.

```
fn main() {
    let mut s = String::from("hello");
    change(&mut s);
    println!("{}", s);
}

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```
Solo se puede tener **una referencia mutable** con `&mut` para un dato en un _scope_ particular.
```
let mut s = String::from("hello");
let r1 = &mut s;
let r2 = &mut s; // error en compilación
```
[<img src="https://www.worksafe.tas.gov.au/__data/assets/file/0005/305375/hatmono.svg" width="24"> Safety ] Asi se previenen _condiciones de carrera_ difíciles de diagnosticar y arreglar.

---
## Scope de Referencias Mutables
1. Multiples referencias son válidas
2. Una referencia mutable no es válida cuando existe otra referencia inmutable

```
let mut s = String::from("hello");
let r1 = &s;     // ok
let r2 = &s;     // ok
let r3 = &mut s; // NOOO!
println!("{}, {}, and {}", r1, r2, r3);
```
**Nota**
- El _alcance_ de una referencia va desde que se introduce hasta la última vez que fue usada
```
let mut s = String::from("hello");
let r1 = &s;     // ok
let r2 = &s;     // ok
println!("{} and {}", r1, r2);
let r3 = &mut s;
println!("OK ! {}", r3);
```

---
## Referencias Colgadas
Un puntero que hace referencia a una ubicación en memoria que pudo haber sido liberada por alguien mas es un `Dangling Reference`.
- Rust lo previene en tiempo de compilación

```
fn main() {
    let reference_to_nothing = dangle();
}

fn dangle() -> &String {
    let s = String::from("hello");
    &s
} // `s` se libera
//Error: this function's return type contains a borrowed value, but there is no value for it to be borrowed from
```

Como `s` se creo dentro de `dangle()`, esta se libera al terminar la función.

**Solución**
- Devolver el _ownership_ de `s` en lugar de una _referencia_ `&s`_

---
## String Slice
El compilador se asegura que las referencias echas con `&str` a un String permanezcan válidas.

```
fn main() {
    let mut cadena = String::from("hola mundo");
    let word = first_word(&cadena);  // inmutable borrow
    cadena.clear(); // `Error` - mutable borrow
    println!("La primer palabra es: {}", word);
}

fn first_word(cadena: &String) -> &str {
    let bytes = cadena.as_bytes();
    for (indice, &elemento) in bytes.iter().enumerate() {
        if elemento == b' ' {
            return &cadena[0..indice];
        }
    }
    &cadena[..]
}
```
**Nota**
- Usar Slice como argumento lo hace más generico y recibe datos tipo `&str` o `String`.
  - `first_word(&cadena[..])`
  - `first_word("hola mundo")`

---
## Otros Slices
También podemos hacer referencia a una parte de un arreglo.

```
let a = [1, 2, 3, 4, 5];

let slice = &a[1..3];

println!("{:?}", slice);
```

- Este slice tiene un tipo `&[i32]`
- Asi como este se pueden tener slices de mas tipos de datos.
  - `&[f64]`
  - `&[bool]`
  - `&[&str]`
  - `&[String]`
  - `&[(i32, f64, u8)]`


