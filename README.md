# Práctica 1 — Mini Cloud Log Analyzer
## Variante C: Detección del primer evento crítico (503)

## Esteves Peña Samuel Num.Control: 22210762
---

## Descripción

Este programa lee códigos de estado HTTP desde la entrada estándar (uno por línea) e identifica en qué línea aparece por **primera vez** el código `503`. Si no existe ningún `503` en la entrada, reporta que no fue encontrado.

---

## Diseño y lógica

### Flujo general

```
stdin ──► leer byte a byte ──► acumular dígitos por línea
                                       │
                              fin de línea ('\n') o EOF
                                       │
                              ¿código acumulado == 503?
                               ├── Sí ──► imprimir línea y salir
                               └── No ──► incrementar contador, reiniciar acumulador
```

### Decisiones de diseño

| Aspecto | Decisión |
|---|---|
| Lectura | `read` syscall de 1 byte a la vez (sin biblioteca C) |
| Parsing | Acumulación manual: `acc = acc * 10 + (dígito - '0')` |
| Contador de líneas | Registro `x19`, se incrementa en cada `'\n'` |
| Conversión int→ASCII | Función `int_to_str` escrita en ARM64 (división sucesiva entre 10) |
| Salida | `write` syscall directa a stdout (fd=1) |

### Registros utilizados

| Registro | Uso |
|---|---|
| `x19` | Número de línea actual (inicia en 1) |
| `x20` | Acumulador del código HTTP de la línea en curso |
| `x21` | Flag: 503 encontrado (1) o no (0) |
| `w22` | Byte leído en cada iteración |
| `x0–x2` | Parámetros de syscalls (fd, buffer, tamaño) |
| `x8` | Número de syscall |

### Syscalls Linux AArch64 utilizadas

| Número | Nombre | Uso |
|---|---|---|
| 63 | `read` | Leer 1 byte desde stdin |
| 64 | `write` | Escribir mensajes a stdout |
| 93 | `exit` | Terminar el proceso |

---

## Compilación

```bash
make
```

Esto ejecuta internamente:
```bash
as -o analyzer.o analyzer.s
ld -o analyzer analyzer.o
```

---

## Ejecución

```bash
cat logs.txt | ./analyzer
```

---

## Pruebas

```bash
make test
```

### Casos de prueba incluidos

| Test | Entrada | Salida esperada |
|---|---|---|
| 1 | `200 404 503 200` | `Primer evento critico 503 encontrado en la linea 3` |
| 2 | `200 201 404 500` | `No se encontro ningun evento critico 503` |
| 3 | `503 200` | `Primer evento critico 503 encontrado en la linea 1` |
| 4 | `200 503 503` | `Primer evento critico 503 encontrado en la linea 2` |
| 5 | `logs.txt` | `Primer evento critico 503 encontrado en la linea 7` |

---

## Evidencia de ejecución

```
$ make
as -o analyzer.o analyzer.s
ld -o analyzer analyzer.o

$ cat logs.txt | ./analyzer
Primer evento critico 503 encontrado en la linea 7

$ make test
=== Test 1: 503 presente ===
Primer evento critico 503 encontrado en la linea 3
=== Test 2: Sin 503 ===
No se encontro ningun evento critico 503
=== Test 3: 503 en primera linea ===
Primer evento critico 503 encontrado en la linea 1
=== Test 4: Multiples 503 (solo reporta el primero) ===
Primer evento critico 503 encontrado en la linea 2
=== Test 5: logs.txt ===
Primer evento critico 503 encontrado en la linea 7
```

---

## Restricciones cumplidas

- ✅ Lógica implementada **100% en ARM64 Assembly**
- ✅ Sin uso de C, Python ni bibliotecas externas
- ✅ Compilación con **GNU Make**
- ✅ Lee desde **stdin** con `cat logs.txt | ./analyzer`
- ✅ Sin modificación de la variante asignada (C)
