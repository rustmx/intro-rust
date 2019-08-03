class: center, middle

<img src="../assets/images/rustmx-logo.svg" alt="RustMX" width="250rem" height="auto">

# Apendice B: Rust y WebAssembly 

---

## ¿Qué es WebAssembly?

<br>

WebAssembly es un standar web que define un formato binario y un formato de 
texto estilo ensamblador para código ejecutable en páginas web.

<br>

- El formato de texto es llamado WebAssembly Text, la extensión que usa es 
  `.wat` y su sintaxis es muy parecida a los lenguajes de la familia de Lisp.


- El formato binario es usa la extensión `.wasm` es consumido directamente por 
  las máquinas virtuales de wasm y el formato es conceptualmente similar a ELF o 
  Mach-O.

---

## Formato .wat

<br>

```lisp
(module
  (func (export "addTwo") (param i32 i32) (result i32)
    local.get 0
    local.get 1
    i32.add))
```

---

## Formato .wasm

```assembly
0000000: 0061 736d                                 ; WASM_BINARY_MAGIC
0000004: 0100 0000                                 ; WASM_BINARY_VERSION
; section "Type" (1)
0000008: 01                                        ; section code
0000009: 00                                        ; section size (guess)
000000a: 01                                        ; num types
; type 0
000000b: 60                                        ; func
000000c: 02                                        ; num params
000000d: 7f                                        ; i32
000000e: 7f                                        ; i32
000000f: 01                                        ; num results
0000010: 7f                                        ; i32
0000009: 07                                        ; FIXUP section size
; section "Function" (3)
0000011: 03                                        ; section code
0000012: 00                                        ; section size (guess)
0000013: 01                                        ; num functions
0000014: 00                                        ; function 0 signature index
0000012: 02                                        ; FIXUP section size
; section "Export" (7)
```
---

<br>

```assembly
0000015: 07                                        ; section code
0000016: 00                                        ; section size (guess)
0000017: 01                                        ; num exports
0000018: 06                                        ; string length
0000019: 6164 6454 776f                           addTwo  ; export name
000001f: 00                                        ; export kind
0000020: 00                                        ; export func index
0000016: 0a                                        ; FIXUP section size
; section "Code" (10)
0000021: 0a                                        ; section code
0000022: 00                                        ; section size (guess)
0000023: 01                                        ; num functions
; function body 0
0000024: 00                                        ; func body size (guess)
0000025: 00                                        ; local decl count
0000026: 20                                        ; local.get
0000027: 00                                        ; local index
0000028: 20                                        ; local.get
0000029: 01                                        ; local index
000002a: 6a                                        ; i32.add
000002b: 0b                                        ; end
0000024: 07                                        ; FIXUP func body size
```

---

<br>

```assembly
0000022: 09                                        ; FIXUP section size
; section "name"
000002c: 00                                        ; section code
000002d: 00                                        ; section size (guess)
000002e: 04                                        ; string length
000002f: 6e61 6d65                                name  ; custom section name
0000033: 02                                        ; local name type
0000034: 00                                        ; subsection size (guess)
0000035: 01                                        ; num functions
0000036: 00                                        ; function index
0000037: 02                                        ; num locals
0000038: 00                                        ; local index
0000039: 00                                        ; string length
000003a: 01                                        ; local index
000003b: 00                                        ; string length
0000034: 07                                        ; FIXUP subsection size
000002d: 0e                                        ; FIXUP section size
```

---

class: center, middle

# DEMO TIME!

---

- Vamos a crear un carpeta para nuestro proyecto de WASM

<br>

```shell
mkdir wasm-demo
cd wasm-demo
```
<br>

- Configurar la versión estable para el directorio actual

<br>

```shell
rustup override set stable
```

---

- Cargar las fuentes y documentación para uso fuera de línea

<br>

```shell
rustup component add rust-src
rustup component add clippy-preview
rustup install nightly
```

<br>

- Instalar `wasm-pack`

<br>

```shell
curl https://rustwasm.github.io/wasm-pack/installer/init.sh -sSf | sh
```

<br>

> Para Windows revisar las instrucciones de instalación en [https://rustwasm.github.io/wasm-pack/installer/#]()

---

- Instalar `cargo-generate`

<br>

```shell
cargo install cargo-generate
```

<br>

- Instalar la última versión de npm

<br>

```shell
npm install npm@latest -g
```

<br>

> Si no tienes `npm` lo puedes instalar con tu manejador de paquetes o desde el
> sitio [https://nodejs.org/en/]()

---

## Vamos a clonar el proyecto plantilla

<br>

```shell
cargo generate --git https://github.com/rustwasm/wasm-pack-template
```
<br>

El nombre del proyecto será: `simple-alert`

<br>

```shell
cd simple-alert
tree -L 2
```

---

## ¡Ahora a construir el proyecto!

<br>

```shell
wasm-pack build
```

<br>

```shell
tree -L 2 pkg
```

---

## Ponerlo en una página web

<br>

```shell
npm init wasm-app www
```

<br>

```shell
tree -L 2 www
```

---

## Instalar las dependencias

<br>

```shell
cd www
npm install
```

---

## Crear un link 

<br>

```shell
cd ..
cd pkg
npm link
cd ..
cd www
npm link simple-alert
```

---

## Editaremos el archivo `www/index.js`

<br>

```javascript
import * as wasm from "simple-alert";

wasm.greet();
```

---

## A correr nuestra primera app en WebAssembly

<br>

```shell
npm run start
```

> Abrir `localhost:8080` en el navegador

---

## Vamos a editar nuestro código para poder imprimir nuestro nombre en el alert

<br>

```rust
// simple-alert/src/lib.rs
mod utils;

use wasm_bindgen::prelude::*;

#[cfg(feature = "wee_alloc")]
#[global_allocator]
static ALLOC: wee_alloc::WeeAlloc = wee_alloc::WeeAlloc::INIT;

#[wasm_bindgen]
extern {
    fn alert(s: &str);
}

#[wasm_bindgen]
pub fn greet(name: &str) {
    alert(&format!("Hello, {}!", name));
}
```

---

<br>

```javascript
// simple-alert/www/index.js
import * as wasm from "simple-alert";

wasm.greet("Maricela");
```
---

## Vamos a construir de nuevo

<br>

```shell
cd ..
wasm-pack build
```

---

## Revisemos los resultados

<br>

```shell
npm run start
```

> Abrir `localhost:8080` en el navegador

---

class: center, middle

¡Ahora el límite es tu imaginación!
