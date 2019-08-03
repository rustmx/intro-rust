class: center, middle

<img src="../assets/images/rustmx-logo.svg" alt="RustMX" width="250rem" height="auto">

# Sesión 10:
## Traits 

---
## Generics 1/5
Crean definiciones para elementos en la firma de una funcion o enums que sirven para representar ciertos tipos de dato.

```
fn busca_mayor(lista: &[i32]) -> i32 {
    let mut mayor = lista[0];
    for &elemento in lista.iter() {
        if elemento > mayor {
            mayor = elemento;
        }
    }
    mayor
}

fn main() {
    let lista = vec![34, 50, 25, 100, 65];
    let resultado = busca_mayor(&lista);
    println!("El elemento mas grande es {}", resultado);
}
```
Se requiere otra funcion para encontrar el caracter mayor en una colección de caracteres `vec!['y', 'm', 'a', 'q']`.

[Demo](https://play.rust-lang.org/) - _duplicaremos código?_

---
## Generics 2/5
En Rust un generic se define con una letra y la mas comun a usar es `T` por convencion.  Se define en la firma de la función.

```
fn busca_mayor<T: Copy + PartialOrd>(lista: &[T]) -> T {
    let mut mayor = lista[0];
    for &elemento in lista.iter() {
        if elemento > mayor {
            mayor = elemento;
        }
    }
    mayor
}

fn main() {
    let lista = vec![34, 50, 25, 100, 65];
    let resultado = busca_mayor(&lista);
    println!("El elemento mas grande es {}", resultado);
    let lista = vec![4.5, 3.2, 1.0,];
    let resultado = busca_mayor(&lista);
    println!("El elemento mas grande es {}", resultado);
}
```

Una operacion con `>` debe ejecutarse en tipos de dato que implementen la logica del metodo `gt()` en el trait [std::cmp::PartialOrd](https://doc.rust-lang.org/std/cmp/trait.PartialOrd.html#examples-4).


---
## Generics 3/5
Podemos usar la definicion de **estructuras** para usar genericos en uno o mas elementos internos.

```
#[derive(Debug)]
struct Point<T> {
    x: T,
    y: T,
}

fn main() {
    let integer = Point { x: 5, y: 10 };
    let float = Point { x: 1.0, y: 4.0 };
    println!("{:?}\n{:#?}", integer, float);
}
```
[Demo](https://play.rust-lang.org/) _Compila el codigo si se instancia Point con `x` y `y` de distinto tipo de dato?_

**Nota**
- Podemos usar mas genericos separandos por coma, por ej. `<T, U>`.
- Todos los tipos de datos para ser impresos requieren usar la libreria `std::fmt`.  Los datos escalares la usan pero otros requiere que se implemente.  Sin embargo todos los tipos pueden derivar una implementación de `fmt::Debug` con [#[derive(Debug)]](https://doc.rust-lang.org/beta/rust-by-example/hello/print/print_debug.html).

---
## Generics 4/5
Podemos uno o mas genericos también en **enumeraciones**.  Recordemos los que ya existen en la librería estádar.

```
  enum Option<T> {         enum Result<T, E> {
      Some(T),                 Ok(T),
      None,                    Err(E),
  }                        }
```

y en la definición de métodos

```
impl<T> Point<T> {
    fn x(&self) -> &T {
        &self.x
    }
}
```

---
## Generics 5/5

- No hay falla en rendimiento
- **Monomorphization**
  - Convierte el código generico en tipos de dato concretos
  - Al usar tipos de datos concretos el tiempo de ejecución es menor

```
let integer = Some(5);
let float = Some(5.0);
```

El compilador genera dos tipos concretos de `Option<T>`
- i32
- f64

```
    enum Option_i32 {        enum Option_f64 {
        Some(i32),               Some(f64),
        None,                    None,
    }                        }

fn main() {
    let integer = Option_i32::Some(5);
    let float = Option_f64::Some(5.0);
}
```

---
## Traits 1/4
Le indica al compilador sobre una funcionalidad (`impl`) que un tipo de dato debe tener.

```
struct Circulo {
    x: f64,
    y: f64,
    radio: f64,
}

impl Circulo {
    fn area(&self) -> f64 {
        std::f64::consts::PI * (self.radio * self.radio)
    }
}
```

Definimos el trait
```
trait TieneArea {
    fn area(&self) -> f64;
}

impl TieneArea for Circulo {
    fn area(&self) -> f64 {
        std::f64::consts::PI * (self.radio * self.radio)
    }
}
```

---
## Traits 2/4
Las funciones genericas pueden explotar los traits para reestringir el tipo de datos que aceptan.
```
fn imrimir_area<T>(figura: T) {
    println!("Esta figura tiene un area de {}", figura.area());
}
```
[Demo](https://play.rust-lang.org/)

`T` puede ser cualquier tipo, no se puede saber quien tiene un metodo `area()`.  Se reestringe con el trait `<T: TieneArea>`, esto se lee como:

_Cualquier tipo que implemente el trait `TieneArea`_

Para otra figura:

```
    struct Cuadrado {           impl TieneArea for Cuadrado {
        x: f64,                     fn area(&self) -> f64 {
        y: f64,                         self.lado * self.lado
        lado: f64,                  }
    }                           }
```

---
## Traits 3/4
Ahora lo ejecutamos todo
```
fn main() {
    let c = Circulo {
        x: 0.0f64,
        y: 0.0f64,
        radio: 1.0f64,
    };

    let s = Cuadrado {
        x: 0.0f64,
        y: 0.0f64,
        lado: 1.0f64,
    };

    imrimir_area(c);
    imrimir_area(s);
}
```

_¿Que sucede si se manda un tipo de dato distinto?_

`imrimir_area(3)`

---
## Traits 4/4
Si se requiere **mas de un limite** se puede hacer uso de `+` 
```
fn foo<T: Clone + Debug>(x: T) {
    x.clone();
    println!("{:?}", x);
}
```

Existe otra nomenclatura para cuando se trabajan **con varios tipos** y en lugar de hacer esto:
```
fn foo<T: Clone, K: Clone + Debug>(x: T, y: K) {
```

haces esto:
```
fn foo<T, K>(x: T, y: K) where T: Clone, K: Clone + Debug
```

Tambien se puede **heredar** limites entre traits
```
    trait Foo {            trait FooBar : Foo {
        fn foo(&self)          fn foobar(&self);
    }                      }
```