class: center, middle

<img src="../assets/images/rustmx-logo.svg" alt="RustMX" width="250rem" height="auto">

# Sesión 2: 

## Variables & Shadowing
---
### let
Las variables son referencias no mutables a los espacios reservados en memoria para almacenar datos.
```
let foo = 5; // no mutable
foo = 6; // error en tiempo de compilacion
```

### mut
Con `mut` el compilador sabe que una variable sera mutable todo su ciclo de via.
```
let mut bar = String::new(); // mutable
```
.

- `::` se utiliza para tener acceso a funciones asociadas a los tipos de dato.
- `new` es una funcion comun en muchos tipos de dato y es usada para crear nuevos valores.

---
### Shadowing
Permite definir una serie de transformaciones sobre los valores de la variable pero al final esta se mantiene como _no mutable_.  Puede cambiar el tipo de dato.

Por ejemplo, dado lo sig.:
```
let var = "four";
```

#### Caso 1 - redefinir el valor
`let var = 8;`

#### Caso 2 - reutilizar el valor previo
`let var = var * 2;`

.

Compruebalo con:
```
   println!("The value is {}", var);
```

---
### Constantes
Siempre son _no mutables_ y deben ser anotadas con su _tipo de dato_ explicitamente.

.

- No se pueden definir en tiempo de ejecucion.
- Pero si a traves de expresiones.

```
const MAX_RESULTS_PER_REQUEST: u32 = 100_000;
```

- Son validas por el tiempo en que se ejecuta el programa y ademas dentro del _scope_ en el que fueron declaradas.
- La convencion es usar _mayusculas_ y _subrayados_.

---

### [Demo](https://play.rust-lang.org/)
 
 1. _¿Se puede cambiar de tipo a una variable mutable?_
 2. _¿Se puedee hacer shadowing sin `let`?_ [Intenta](https://repl.it/@wdonet/shadowing-mut)
 
 ![Go demo it!](https://linpack-for-tableau.com/uploads/actualites/livedemo-1.png "Demo")
