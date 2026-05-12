#  Taller de JavaScript — Node.js en Codespaces

> **Instrucciones:** Resuelve cada ejercicio creando un archivo `.js` y ejecútalo con `node nombre-archivo.js`. No uses el navegador, todo corre en la terminal.

---

## Módulo 1 — Variables y Tipos de Datos

### Ejercicio 1.1 — Declaración de variables
Declara tres variables: una con `var`, una con `let` y una con `const`. Asigna tu nombre, tu edad y tu ciudad. Imprime las tres en la consola con un mensaje descriptivo.

### Ejercicio 1.2 — Tipos de datos
Crea una variable para cada tipo de dato primitivo de JavaScript:
- `string`
- `number`
- `boolean`
- `null`
- `undefined`

Imprime el **valor** y el **tipo** (`typeof`) de cada una.

### Ejercicio 1.3 — Reasignación y constantes
Declara una variable con `let` y cámbiala de valor dos veces. Luego intenta hacer lo mismo con `const`. ¿Qué ocurre? Comenta en el código tu observación.

### Ejercicio 1.4 — Conversión de tipos
Tienes la variable `let precio = "350"` (es un string).
- Conviértela a número y súmale 100.
- Convierte el resultado a string y concaténalo con el texto `" pesos colombianos"`.
- Imprime cada paso.

### Ejercicio 1.5 — Scope
Declara una variable `x` fuera de un bloque `{}` y otra variable `y` dentro del bloque. Intenta imprimir ambas desde fuera del bloque. ¿Qué pasa con `y`? Usa `let` y explica el comportamiento con un comentario.

---

## Módulo 2 — Operadores

### Ejercicio 2.1 — Aritméticos
Dados dos números ingresados como variables, calcula e imprime:
- Suma
- Resta
- Multiplicación
- División
- Módulo (residuo)
- Potencia (`**`)

### Ejercicio 2.2 — Comparación
Declara dos variables numéricas. Imprime el resultado (true/false) de compararlas con: `==`, `===`, `!=`, `!==`, `>`, `<`, `>=`, `<=`.  
Luego compara el número `5` con el string `"5"` usando `==` y `===`. Comenta la diferencia.

### Ejercicio 2.3 — Lógicos
Crea tres variables booleanas: `esMayorDeEdad`, `tieneDocumento`, `esColombia`.  
Imprime el resultado de:
- `esMayorDeEdad && tieneDocumento`
- `esMayorDeEdad || esColombia`
- `!tieneDocumento`

### Ejercicio 2.4 — Incremento y decremento
Declara una variable `contador = 10`. Aplica `++` y `--` de forma pre y post (prefijo y sufijo). Imprime el valor en cada caso y explica con comentarios la diferencia.

### Ejercicio 2.5 — Operador ternario
Declara una variable `temperatura`. Si es mayor a 30, imprime `"Hace calor"`, si no, `"Clima agradable"`. Usa únicamente el operador ternario (sin `if`).

---

## Módulo 3 — Strings

### Ejercicio 3.1 — Métodos básicos
Declara la variable `let frase = "  Bienvenidos al taller de JavaScript  "`.  
Imprime:
- La frase en mayúsculas
- La frase en minúsculas
- La frase sin espacios al inicio y al final
- La longitud de la frase (después de recortarla)

### Ejercicio 3.2 — Template literals
Crea variables para nombre, edad y ciudad. Usando template literals (backticks), construye e imprime una presentación como:  
`"Hola, me llamo Ana, tengo 25 años y soy de Medellín."`

### Ejercicio 3.3 — Búsqueda en strings
Declara `let email = "usuario@dominio.com"`.  
- ¿Incluye el carácter `@`?
- ¿Con qué índice empieza `"dominio"`?
- ¿Termina con `".com"`?

### Ejercicio 3.4 — Extracción y reemplazo
Dada la cadena `"JavaScript es difícil"`:
- Extrae la palabra `"difícil"` usando `slice`.
- Reemplaza `"difícil"` por `"poderoso"` e imprime el resultado.

### Ejercicio 3.5 — Split y Join
Declara `let csv = "manzana,pera,uva,mango,banano"`.  
- Convierte el string en un array separando por coma.
- Imprime cuántos elementos tiene el array.
- Une el array de nuevo con ` - ` como separador.

---

## Módulo 4 — Condicionales

### Ejercicio 4.1 — if / else básico
Declara una variable `nota` (número del 0 al 100). Imprime:
- `"Aprobado"` si es mayor o igual a 60
- `"Reprobado"` si es menor

### Ejercicio 4.2 — if / else if / else
Usando la misma variable `nota`, imprime una escala de calificaciones:
- 90–100 → `"Excelente"`
- 75–89 → `"Bueno"`
- 60–74 → `"Suficiente"`
- menor a 60 → `"Insuficiente"`

### Ejercicio 4.3 — switch
Declara una variable `dia` (número del 1 al 7). Usando `switch`, imprime el nombre del día de la semana correspondiente. Si el número está fuera de rango, imprime `"Día inválido"`.

### Ejercicio 4.4 — Condiciones anidadas
Declara las variables `edad` y `tienePermiso` (boolean). Imprime:
- Si tiene 18 o más **y** tiene permiso → `"Puede entrar"`
- Si tiene 18 o más pero **no** tiene permiso → `"Necesita permiso"`
- Si es menor de 18 → `"No puede entrar"`

### Ejercicio 4.5 — Validación de formulario
Declara variables `usuario` (string) y `contrasena` (string).  
- Si `usuario` está vacío o `contrasena` tiene menos de 8 caracteres, imprime el error correspondiente.
- Si todo es válido, imprime `"Login exitoso"`.

---

## Módulo 5 — Loops (Bucles)

### Ejercicio 5.1 — for básico
Imprime los números del 1 al 20 usando un bucle `for`.

### Ejercicio 5.2 — for con condición
Imprime únicamente los números **pares** entre 1 y 50.

### Ejercicio 5.3 — while
Declara `let saldo = 1000`. Mientras el saldo sea mayor a 0, réstale 150 e imprime el saldo en cada iteración. Al terminar, imprime `"Saldo agotado"`.

### Ejercicio 5.4 — do...while
Crea un contador que empiece en 5 y se decremente en 1. Usa `do...while` para imprimir el conteo y al llegar a 0 imprima `"¡Despegue!"`.

### Ejercicio 5.5 — break y continue
Usando un `for` del 1 al 20:
- Salta (con `continue`) los múltiplos de 3.
- Detén (con `break`) el bucle si encuentras el número 17.
- Imprime los demás números.

### Ejercicio 5.6 — Tabla de multiplicar
Pide un número (decláralo como variable) e imprime su tabla de multiplicar del 1 al 10 con el formato:  
`3 x 1 = 3`  
`3 x 2 = 6`  
...

### Ejercicio 5.7 — Bucles anidados
Imprime un cuadrado de asteriscos de 5x5 usando bucles anidados:
```
* * * * *
* * * * *
* * * * *
* * * * *
* * * * *
```

### Ejercicio 5.8 — Triángulo
Usando bucles anidados, imprime un triángulo de asteriscos:
```
*
* *
* * *
* * * *
* * * * *
```

### Ejercicio 5.9 — FizzBuzz clásico
Imprime los números del 1 al 100. Pero:
- Si el número es múltiplo de 3 → imprime `"Fizz"`
- Si es múltiplo de 5 → imprime `"Buzz"`
- Si es múltiplo de ambos → imprime `"FizzBuzz"`
- Si no → imprime el número

### Ejercicio 5.10 — Suma acumulada
Usando un bucle, calcula la suma de todos los números del 1 al 100 y muestra el resultado al final. (El resultado esperado es 5050).

---

##  Reto Final — Integrando todo

Crea un programa que simule un cajero automático básico:

- Define un `saldo` inicial de `$500.000`
- Usando un bucle, simula 5 transacciones (decláralas como un array de números positivos o negativos)
- Para cada transacción:
  - Si es positiva → es un depósito
  - Si es negativa → es un retiro
  - Si un retiro supera el saldo disponible → imprime `"Fondos insuficientes"` y no lo apliques
- Al final, imprime el saldo total.
