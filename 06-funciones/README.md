class: center, middle

<img src="../assets/images/rustmx-logo.svg" alt="RustMX" width="250rem" height="auto">

# Sesión 6: 
## Funciones, Métodos y Closures

---
## Definición
- Las funciones son declaradas con `fn`,
- Los argumentos deben indicar sus tipos de datos `arg: i8`
- Si retorna un valor debe indicarlo con el tipo : ` -> bool`
- La expresion final es usada como valor de retorno.  Tambien se puede usar `return`

```
// Retorna un valor boleano

fn es_divisible_por(numerador: u32, divisor: u32) -> bool {

    if divisor == 0 {
        return false;
    }

    // No es necesario especificar `return`
    numerador % divisor == 0
}
```

---
## Métodos

Son funciones agregadas a objetos como estructuras o enums.

```
struct Point {                 struct Rectangle {
    x: f64,                        p1: Point,
    y: f64,                        p2: Point,
}                              }

impl Rectangle {

    // `&self` es un "sintax sugar" para `self: &Self`.
    // Donde `Self` es el tipo de objeto que manda llamar la funcion.
    // En este caso `Self` = `Rectangle`.

    fn perimeter(&self) -> f64 {
        let Point { x: x1, y: y1 } = self.p1;
        let Point { x: x2, y: y2 } = self.p2;
        2.0 * ((x1 - x2).abs() + (y1 - y2).abs())
    }

}

```

---
## Closures
Tambien llamadas _lambdas_ son funciones que puede _capturar las variables_ del contexto desde el cual son ejecutadas.
Son muy convenientes para usarlas al vuelo, los tipos de datos a la entrada y salida son inferidos.

- Usa `||` en lugar `()` para los argumentos
- Las `{}` para el cuerpo de la funcion de _una sola linea_ son opcionales

```

// function
fn  function            (i: i32) -> i32 { i + 1 }

let closure_annotated = |i: i32| -> i32 { i + 1 };
let closure_inferred  = |i     |          i + 1  ;

let i = 1;
println!("function: {}", function(i));
println!("closure_annotated: {}", closure_annotated(i));
println!("closure_inferred: {}", closure_inferred(i));


println!("The smallest closure: {}", (|| 1)() );
```