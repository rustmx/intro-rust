class: center, middle

<img src="../assets/images/rustmx-logo.svg" alt="RustMX" width="250rem" height="auto">

# Sesión 11: 
## Módulos

---
## Crate
Es una biblioteca o paquete que se manipula con `Cargo`

Un `Crate` produce un ejecutable o una biblioteca.

Cada `Crate` tiene un _módulo raíz_ implícito con el código para dicho crate pero se puede definir un árbol de sub módulos.

**Dividir por módulos**

```
                      +-- saludos
         +-- ingles --|
         |            +-- despedidas
frases --|
         |            +-- saludos
         +-- japones--|
                      +-- despedidas

$ cargo new frases
$ cd frases
$ vi src/lib.rs

    mod ingles {                 mod japones {
        mod saludos {}                mod saludos {}
        mod despedidas {}             mod despedidas {}
    }                            }

$ cargo build
```

---
## Dividir por archivos
Cambia `src/lib.rs` a
```
mod ingles;
mod japones;
```
Rust espera encontrar un archio `ingles.rs` o un `ingles\mod.rs` con mas submodulos descritos
```
├── Cargo.lock
├── Cargo.toml
├── src
│   ├── ingles
│   │   ├── despedidas.rs
│   │   ├── saludos.rs
│   │   └── mod.rs
│   ├── japones
│   │   ├── despedidas.rs
│   │   ├── saludos.rs
│   │   └── mod.rs
│   └── lib.rs
```
`ingles\mod.rs` y `japones\mod.rs` deben lucir:
```
mod saludos;
mod despedidas;
```

---
## Implementar modulos
Agreguemos funcionalidad `pub`lica a los modulos inferiores `despedidas.rs` y `saludos.rs` de ambos idiomas.
```
    pub fn hola() -> String {           pub fn hola() -> String {
        "こんにちは".to_string()            "さようなら.".to_string()
    }                               }
```
Creamos un `src/main.rs` ejecutable con `extern crate` que le dice a Rust q enlazamos al crate `frases`.
```
extern crate frases;

fn main() {
    println!("Hola en Ingles: {}", frases::ingles::saludos::hola());
    println!("Adios en Ingles: {}", frases::ingles::despedidas::adios());

    println!("Hola en Japones: {}", frases::japones::saludos::hola());
    println!("Adios en Japones: {}", frases::japones::despedidas::adios());
}
```
Es necesario tambien hacer `pub`licos los submodulos declarados en `mod.rs` y en `src/lib.rs`

---
## Importar con `use`
Importar modulos en el ambito local, permite no ser repetitivo y dejar mas legible el código.

1. Agrega `saludos` y `despedidas` de esta forma `use frases::ingles::saludos;` antes del main en `src/main.rs`
2. Ahora puedes invocar como `saludos::hola()` y `despedidas::adios()`

**Buena práctica**
- importar hasta el ultimo submodulo y no directamente la función para evitar conflictos de nombres con otras librerías

_Forma corta :_ `use phrases::ingles::{saludos, despedidas};`

**Re exporta con `pub use`**

Usar `japones::hola()` y `japones::adios()` require exportar de nuevo el submodulo en `src/japones/mod.rs`
```
pub use self::saludos::hola;
pub use self::despedidas::adios;
mod saludos;
mod despedidas;
```
