# Proyecto Integrador – Organización y Arquitectura de Computadores

## Estudiantes

- Juan Esteban Palacio Betancur
- Thomas Alejandro Serna Saldarriaga

## Descripción

Este repositorio contiene la solución al Proyecto Integrador del primer corte
del curso de Organización y Arquitectura de Computadores.

El proyecto se basa en los Proyectos 1, 2 y 3 de la plataforma Nand2Tetris e
incluye una extensión académica denominada **ALUExtendida**, desarrollada
específicamente para este curso.

---

# Contenido

## Proyecto 1

Implementación de las compuertas lógicas básicas y estructuras
combinacionales derivadas.

### Componentes

- Not
- And
- Or
- Xor
- Mux
- DMux
- Not16
- And16
- Or16
- Mux16
- Or8Way
- Mux4Way16
- Mux8Way16
- DMux4Way
- DMux8Way

---

## Proyecto 2

Implementación de circuitos aritméticos y de la ALU.

### Componentes

- HalfAdder
- FullAdder
- Add16
- Inc16
- ALU

---

## Proyecto 3

Implementación de componentes secuenciales y memoria.

### Componentes

- Bit
- Register
- RAM8
- RAM64
- RAM512
- RAM4K
- RAM16K
- PC

---

## Extensión: ALUExtendida

La ALUExtendida conserva la interfaz de la ALU original de Nand2Tetris y
agrega cinco operaciones nuevas.

### Documentación

La explicación de la implementación de las operaciones de la ALUExtendida se
encuentra en:

`alu_extendida/DOCUMENTACION/ALUExtendida.md`

### Operaciones soportadas

- XOR — `010110`
- NAND — `000001`
- NOR — `110100`
- EQ — `101000`
- ABS — `100010`

---

# Estructura del Repositorio

```text
proyecto01/
proyecto02/
proyecto03/
alu_extendida/
README.md
```
