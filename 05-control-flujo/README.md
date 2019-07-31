class: center, middle

<img src="../assets/images/rustmx-logo.svg" alt="RustMX" width="250rem" height="auto">

# Sesión 5: 
## Control de flujo

---
## `If`
Es una expresion que revisa la condicion sea logica y resulte en un dato tipo `bool`.

```
  let grados = 10;
  if grados > 30 {
    println!("Apagar estufa en 1 minuto!");
  } else if grados < 5 {
    println!("Comenzando");
  } else {
    println!("Temperatura {}", grados);
  }
```

Se puede usar a la derecha de un `let`.  En esta presentacion, ambos casos resultantes deben ser del mismo tipo de dato.
```
  let estatus = if grados > 5 {
    "caliente"
  } else {
    "frio"
  };
  println!("Estado {}", estatus);
```
[Demo](https://repl.it/@wdonet/rust-control-if)

---
## `Match` 1/3
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
## `Match` 2/3
Usar muchos `else-if` puede hacer el codigo ilegible, para esos casos tenemos `match`.

Aqui estan las alternativas:

```
  let grados = 13;
  match grados {
        // Empatar un simple valor
        1 => println!("frio!"),
        // o varios valores
        2 | 3 | 5 | 7 => println!("tibio"),
        // o un rango
        13...19 => println!("caliente"),
        // En cualquier otro caso
        _ => println!("Temperatura {}", grados),
    }
```

El patron `_` debe usarse al final para empatar todos los casos posibles no especificados.
Usualmente usado con _(unit value)_ `()` que significa no hacer nada.

```
match grados {
    1 => println!("frio!"),
    // ...
    _ => (),
}
```

---
## `Match` 3/3
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


---
## `if let`
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
---
## Ciclos 1/2
| `loop` | `while` | `for` |
|:-------:|:------:|:--------:|
| Ejecuta el codigo por siempre o hasta que se termine manualmente (`^C`). | Ejecuta el codigo siempre que la condicion se cumpla. | Usual para recorrer elementos de un tipo de dato compuesto como un arreglo o tupla.  Tambien se suele usar con rangos `(1..4)`. |

```
/* Loop */

  let mut counter = 10;
  let status = loop {
    if counter == 0 {
      break "Terminado";
    }
    println!("Numero {}", counter);
    counter -= 1;
  };
  println!("=> {}", status);
```

---
## Ciclos 2/2
```
/* While */
  let mut counter = counter + 5;
  while counter !=0 {
    println!("Numero {}", counter);
    counter -= 1;
  }
  println!("While terminado");

/* For */
  let elements = [3, 2, 1];
  for element in elements.iter() {
    println!("Numero {}", element);
  }
  println!("For terminado");
```

Utilizando un [rango](https://doc.rust-lang.org/std/ops/struct.Range.html) `(1..4)`, se incluye el tope inicial pero excluye el tope final
```
  for number in (1..7).rev() {
    println!("Numero {}", number);
  }
```

_**Note : ** Se puede usar `break` en cualquier ciclo para dar por terminada su ejecucion._

[Demo](https://repl.it/@wdonet/rust-control-loops)