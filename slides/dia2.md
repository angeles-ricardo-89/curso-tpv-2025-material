---
marp: true
theme: default
class: invert
paginate: true
---

# Día 2 – Introducción a C aplicada al Browser

---

## Objetivos del Día
- Aprender fundamentos mínimos de C necesarios para mantener la **VM WMLScript**.  
- Comprender tipos de datos, control de flujo y operaciones con cadenas.  
- Explorar operaciones a nivel de bits aplicadas a pagos (ej. TVR/TSI).  
- Realizar laboratorios prácticos: memoria dinámica y malloc/free didáctico.  

---

## Tipos de Datos en C
- **Primitivos**: `int`, `char`, `float`, `double`.  
- **Enteros con signo y sin signo** (`int` vs `unsigned int`).  
- **Tamaño dependiente de la arquitectura** (32/64 bits).  

Ejemplo:
```c
int a = 10;
char b = 'X';
unsigned int c = 300;
```

---

## Control de Flujo
- **Condicionales**: `if`, `else`, `switch`.  
- **Bucles**: `for`, `while`, `do-while`.  
- Uso en la VM: interpretación de instrucciones (opcodes).  

Ejemplo:
```c
if (opcode == ADD) {
    result = a + b;
} else {
    // otro caso
}
```

---

## Control de Flujo
- **Condicionales**: `if`, `else`, `switch`.  
- **Bucles**: `for`, `while`, `do-while`.  
- Uso en la VM: interpretación de instrucciones (opcodes).  

Ejemplo:
```c
switch(opcode) {
    case ADD:
    ...
    break;
    case MULT:
    ...
    break;
    case DIV:
    ...
    break;
    default:
    ...
};
```


---

## Control de Flujo
- **Condicionales**: `if`, `else`, `switch`.  
- **Bucles**: `for`, `while`, `do-while`.  
- Uso en la VM: interpretación de instrucciones (opcodes).  

Ejemplo:
```c
for(i=0; i<20; i++){
    ...
}
```

---

## Control de Flujo
- **Condicionales**: `if`, `else`, `switch`.  
- **Bucles**: `for`, `while`, `do-while`.  
- Uso en la VM: interpretación de instrucciones (opcodes).  

Ejemplo:
```c
while(space_left){
    ...
}
```

---

## Control de Flujo
- **Condicionales**: `if`, `else`, `switch`.  
- **Bucles**: `for`, `while`, `do-while`.  
- Uso en la VM: interpretación de instrucciones (opcodes).  

Ejemplo:
```c
do {
    ...
} while(attempts_left)
```

---

## Arreglos y Cadenas
- Arreglos: bloques contiguos de memoria.  
- Cadenas: arreglos de `char` terminados en `\0`.  

Ejemplo:
```c
char name[10] = "PAGO";
printf("%s\n", name); // imprime PAGO
```

---

## Operaciones a Nivel de Bits
- Muy usadas en **EMV** (ej. interpretación de TVR/TSI).  
- Operadores:  
  - AND `&`  
  - OR `|`  
  - XOR `^`  
  - SHIFT `<<`, `>>`  

Ejemplo:
```c
int tvr = 0x80; // bit 8 activado
if (tvr & 0x80) {
    printf("Offline data authentication fallida\n");
}
```

---

## Memoria Dinámica
- Permite reservar memoria en tiempo de ejecución.  
- Funciones clave:  
  - `malloc`, `calloc`, `realloc`, `free`.  
- Importante **liberar memoria** para evitar fugas.  

Ejemplo:
```c
int *arr = (int*)malloc(10 * sizeof(int));
// usar arr...
free(arr);
```

---

## Implementación Didáctica de malloc/free
- Ejercicio conceptual para entender cómo se gestiona memoria.  
- Idea básica: usar un **buffer global** y entregar punteros.  
- En producción siempre se usan las funciones de libc.  

---

## Laboratorio Día 2
1. Escribir un programa en C que pida al usuario dos números y los sume.  
2. Reservar memoria dinámica para un arreglo de 5 enteros y llenarlo con cuadrados.  

3. Implementar una versión **didáctica** de `malloc` y `free` con un buffer fijo.  


---

## Cierre del Día
- Ya conoces lo esencial de C para moverte en la **VM del Browser**.  
- Practicaste arreglos, cadenas, punteros y operaciones a nivel de bits.  
- Entendiste cómo funciona la memoria dinámica.  
- Próximo paso: **C avanzado + NDK** (Día 3).  