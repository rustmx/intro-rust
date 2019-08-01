class: center, middle

<img src="../assets/images/rustmx-logo.svg" alt="RustMX" width="250rem" height="auto">

# Sesión 7: 
## Ownership

---
## Opciones para manejar la memoria con otros lenguajes

1. 🧠 Responsabilidad delegada al usuario
  - Ej. C y C++
  - Propenso a crear punteros nulos
  - Se te olvida hacerlo
2. 🗑 Un recolector de basura
  - Ej. Java, Python o Ruby
  - Sacrificas un poco el rendimiento
  - Usa algoritmos para revisar que se puede limpiar

```
// Ejemplo usando Python

a = [1, 2, 3, 4]
b = a
a.push(5)
print(b)
# [1, 2, 3, 4, 5]
```

---
## Opciones para manejar la memoria con *Rust* Ⓡ
[<img src="https://www.worksafe.tas.gov.au/__data/assets/file/0005/305375/hatmono.svg" width="32"> Safety ]

**_Ownership_**
  1. Cada valor tiene una variable como dueño _(owner)_
  2. Solo puede haber un `owner` a la vez
    - En _tiempo compilación_ se verifica toda variable tenga un valor válido
  3. Cuando el `owner` sale del _alcance_, se libera la memoria 
    - No es necesario un _Recolector de basura_

_Nada de lo anterior afecta el performance del sistema_

```
// Ejemplo usando Rust

fn main() {
    let mut a = vec![1, 2, 3, 4];
    let b = a;
    a.push(5);
    println!("{:?}", b);
    // No compila porque el valor se movió
}
```

---
### En funciones
En el paso de valores a funciones aplican las mismas reglas.

```
#[derive(Debug)]
struct Point(i32);

fn foo(x: Point) {
}

fn main() {
    let p = Point(5);
    foo(p);
    println!("{:?}", p);
    // No compila :(
}
```

Es como asignar un valor a una nueva variable.

---
## Stack & Heap
Ambos son areas de la memoria disponibles para el codigo en tiempo de ejecución pero estructuradas diferente

|   | `Stack` | `Heap` |
|---|---------|--------|
| Tamaño de datos | Fijo | Variable |
| Orden de datos | `last in, first out`  _(el tope de la pila siempre es conocido)_ | Devuelve un puntero al primer espacio libre en memoria. _(mas trabajo)_ |

<div style="display: flex;flex-direction: row;">
  <div width="50%" style="display: flex;flex-grow:2;align-content: center;align-items: center;">
    <img src="https://anandabhisheksingh.me/wp-content/uploads/2017/10/stack.png">
  </div>
  <div style="display: flex;flex-grow:1; align-content: center;align-items: center;">
    <span style="padding: 10px;">👆Saber :
      <ul>
        <li>Que codigo esta usando que data en el <em>heap</em></li>
        <li>Minimizar duplicados en el <em>heap</em>, y
        <li>Limpiar datos no usadas <em>heap</em></li>
      </ul>
      es un trabajo del <strong>Ownership</strong></span>
  </div>
</div>

---
## `String`
Compuesta de tres partes en el `stack`
- Puntero a la memoria (`heap`) que guarda el guarda el contenido de la cadena
- Una longitud y una capacidad

<div style="display: flex;flex-direction: row;">
  <!--div width="50%" style="display: flex;flex-grow:2;align-content: center;align-items: center;">
    <img src="https://doc.rust-lang.org/book/img/trpl04-02.svg">
  </div-->
  <pre width="50%" style="display: flex;flex-grow:2;align-content: center;align-items: center;">
{
  let s1 = String::from("hello");
  let s2 = s1;  // s1 se "movió"
}  // memoria liberada 1 vez  
  </pre>
  <div style="display: flex;flex-grow:1; align-content: center;align-items: center;">
    <span style="padding: 10px;">
      <em>Rust no hace deep copies</em>
      <br/>Copiar el <em>heap</em> seria muy costoso
      <br/><pre>Prueba: s1.clone()</pre>
    </span>
  </div>
</div>
<center><img src="https://doc.rust-lang.org/book/img/trpl04-02.svg" width="30%"></center>

---
## Copy en tipos de datos en el stack

1. Se almacenan en el stack y su acceso es rápido
2. Rust tiene una anotacion (_Trait_) llamada `Copy`.  Si un tipo de dato la tiene, realiza un `deep copy`.  _No usan `clone()`, _

```
let x = 5;
let y = x;
println!("x = {}, y = {}", x, y);  //si compila?
```

Los tipos de dato escalares o similares se conoce su tamaño en tiempo de compilación.

- Enteros, como `u32`
- Booleanos `bool`
- Punto floate, como `f64`
- Caracter : `char`
- Tuplas, solo si contienen tipos con el trait `Copy`
  - como, `(i32, i32)` es Copy, pero (i32, String) no

---
## Funciones
Misma semántica, al _pasar/regresar_ un valor a una función, este se mueve o se copia de acuerdo a las mismas reglas que en asignaciones.

```
fn main() {
    let s = String::from("hello");
    takes_ownership(s);
    let s = String::from("world");
    let s = takes_and_gives_ownership(s);
    
    let x = 5;
    makes_copy(x); // i32 es Copy
    println!("x={}, s={:?}", x, s);
} // `x` y `s` se liberan


fn takes_ownership(some_string: String) {
    println!("Owning {}", some_string);
} // `some_string` se libera

fn takes_and_gives_ownership(some_string: String) -> String {
    some_string
} // `some_string` regresa ownership

fn makes_copy(some_integer: i32) {
    println!("Copying {}", some_integer);
} // `some_integer` se libera
```