# Documentación ALUExtendida

## Descripción

La **ALUExtendida** parte del funcionamiento de la ALU original y añade cinco nuevas operaciones:

* XOR
* NAND
* NOR
* EQ
* ABS

Para seleccionar cada operación se utilizan los bits de control `zx`, `nx`, `zy`, `ny`, `f` y `no`.



Antes de seleccionar las nuevas operaciones se calcula normalmente la salida de la ALU original. Luego, dependiendo de la combinación de los bits de control, se selecciona entre esta salida y alguna de las operaciones extendidas.

---

## XOR

**Código:** `010110`

**Operación:**

```text
out = x XOR y
```

La operación XOR se implementó utilizando las operaciones OR, AND y NOT.

Primero se calcula:

```text
x OR y
```

y también:

```text
x AND y
```

El resultado del AND se niega:

```text
NOT(x AND y)
```

Finalmente, ambos resultados se combinan mediante un AND:

```text
(x OR y) AND NOT(x AND y)
```

---

## NAND

**Código:** `000001`

**Operación:**

```text
out = NOT(x AND y)
```

Para implementar NAND primero se realiza un AND bit a bit entre las entradas `x` y `y`:

```text
x AND y
```

Después, el resultado completo se niega utilizando `Not16`:

```text
NOT(x AND y)
```

---

## NOR

**Código:** `110100`

**Operación:**

```text
out = NOT(x OR y)
```

Para implementar NOR primero se realiza un OR bit a bit entre `x` y `y` utilizando `Or16`.

Luego se niega el resultado mediante `Not16`:

```text
NOT(x OR y)
```
---

## EQ

**Código:** `101000`

**Operación:**

```text
Si x = y:
    out = 0000000000000001

Si x != y:
    out = 0000000000000000
```

Para determinar si `x` y `y` son iguales se reutiliza el resultado de la operación XOR.

El XOR tiene la propiedad de que un bit de salida es `1` cuando los bits correspondientes de `x` y `y` son diferentes.

Por lo tanto, si:

```text
x XOR y = 0000000000000000
```

significa que todos los bits de `x` y `y` son iguales.

Para comprobarlo, los 16 bits del XOR se dividen en dos grupos de 8 bits y se utilizan dos `Or8Way`.

Después se hace un OR entre ambos resultados. Si el resultado es `0`, significa que no existe ningún bit diferente entre `x` y `y`.

Este valor se niega para obtener la señal que indica que ambos números son iguales.

Finalmente:

```text
si x = y entonces out = 0000000000000001
si x != y entonces out = 0000000000000000
```

---

## ABS

**Código:** `100010`

**Operación:**

```text
out = |x|
```

Para esta operación únicamente se utiliza la entrada `x`.

Cuando `x` es positivo, la salida es directamente:

```text
out = x
```

Cuando `x` es negativo, se calcula su complemento a dos.

Primero se niegan todos los bits:

```text
NOT(x)
```

y después se suma `1`:

```text
NOT(x) + 1
```

Para esto se utilizaron los componentes `Not16` e `Inc16`.

Finalmente, mediante un `Mux16` controlado por `x[15]`, se selecciona:

```text
x[15] = 0 -> x
x[15] = 1 -> -x
```

obteniendo así el valor absoluto de `x`.

---

## Selección de las operaciones

Para identificar cuándo debe ejecutarse cada operación extendida, se comprueba la combinación de los seis bits:

```text
zx nx zy ny f no
```

Para facilitar estas comparaciones también se generan sus valores negados:

```text
notZx
notNx
notZy
notNy
notF
notNo
```

Luego se utilizan compuertas `And` para detectar cada uno de los códigos definidos.

Por ejemplo, para XOR:

```text
010110
```

se comprueba:

```text
!zx AND nx AND !zy AND ny AND f AND !no
```

Cuando esta condición es verdadera se activa la señal `isXor`.

El mismo procedimiento se utiliza para generar:

```text
isNand
isNor
isEq
isAbs
```

Estas señales controlan una cadena de `Mux16` que permite seleccionar la salida correspondiente.

Si ninguno de los códigos extendidos se encuentra activo, se conserva el resultado producido por la ALU original.
