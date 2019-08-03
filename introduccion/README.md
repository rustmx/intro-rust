class: center, middle

<img src="../assets/images/rustmx-logo.svg" alt="RustMX" width="250rem" height="auto">

# Introducción a Rust

---

# Temario

- [Instalacion](../01-instalacion/index.html)
- [Variables y Shadowing](../02-variables/index.html)
- [Tipos de datos y Panic](../03-sintaxis/index.html)
- [Estructuras y Enums](../04-enums/index.html)
- [Control de flujo y Pattern Matching](../05-control-flujo/index.html)
- [Funciones](../06-funciones/index.html)
- [Ownership](../07-ownership/index.html)
- [Borrowing](../08-borrowing/index.html)
- [Vectores](../09-vectores/index.html)
- [Traits](../10-traits/index.html)
- [Modulos](../11-modulos/index.html)


---

## ¿Qué es Rust?

Rust, es un lenguaje de programación creado en los laboratorios de Mozilla. Sus 
principales caracteristicas son la estabilidad, el manejo seguro de memoria y la 
concurrencia.

---

## Multiparadigma

- Imperativo
- Estructurado
- Funcional
- Concurrente
- Genérico
- Compilado

```fn  function            (i: i32) -> i32 { i + 1 }
let closure_annotated = |i: i32| -> i32 { i + 1 };
let closure_inferred  = |i     |          i + 1  ;
```

---

## Línea temporal

- Nació en 2006 como un proyecto de personal de Graydon Hoare
- En 2009 Mozilla empieza a patrocinar el proyecto
- 2010 es anunciado por Mozilla
- 2012 es lanzada la primera versión alpha
- 2015, Rust 1.0 es liberado. Nace Redox
- Agosto 2019 estamos en la versión 1.36

---
## Comparación con otros lenguajes

|                       | C/C++ | GC    | Rust |
| --------------------- | ----- | ----- | ---- |
| Control, flexibility  | 🎉:) | 🤷:) | :D |
| Minimal to no runtime | 🎉:/| :) | :D |
| Double free           | :/ | :) | :D |
| Use after free        | :/ | :) | :D |
| Data race             | :/ | :/ | :D |

---

¿Qué provee Rust?

- Good abstraction
- Memory safety ~ No GC
- Good concurrency Handling

---
## ¿En qué puedo usar Rust?

Rust, es un lenguaje de programación de sistemas, esto quiere decir que se usa 
para desarrollar software que proveerá servicios a otro software y que tiene 
requerimientos de rendimiento muy estrictos(sistemas operativos, motores de 
videojuegos, etc.)

---

## ¿Quiénes usan Rust?

- Mozilla
- Dropbox
- Facebook
- Google
- Amazon
- Microsoft

---

## ¡Empecemos a aprender!

Capitulo siguiente: [Instalacion](../01-instalacion/index.html)
