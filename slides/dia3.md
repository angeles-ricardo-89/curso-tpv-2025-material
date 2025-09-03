---
marp: true
theme: default
paginate: true
class: invert
---

# Día 3 – C Avanzado y NDK

---

## Objetivos del Día
- Profundizar en punteros y memoria dinámica en la VM.  
- Comprender estructuras (`struct`) usadas en el runtime.  
- Aprender cómo exponer funciones C a Java mediante **JNI**.  
- Practicar manejo de strings y arrays en JNI.  
- Laboratorio práctico con depuración.  

---

## Punteros y Memoria Dinámica
- **Puntero**: variable que almacena dirección de memoria.  
- Aritmética de punteros (`p+1`, `*(p+i)`).  
- Reservar y liberar memoria en tiempo de ejecución:  
  - `malloc`, `calloc`, `realloc`, `free`.  

Ejemplo:
```c
int *arr = malloc(5 * sizeof(int));
for (int i=0; i<5; i++) arr[i] = i*i;
free(arr);
```

---

## Estructuras en C
- Agrupan datos relacionados.  
- Usadas en la VM para representar contexto de ejecución, pilas, etc.  

Ejemplo:
```c
typedef struct {
    int opcode;
    int operand;
} Instruction;

Instruction ins = {1, 42};
printf("Op=%d, Operand=%d\n", ins.opcode, ins.operand);
```

---

## NDK y JNI – Conceptos
- **NDK**: permite compilar C/C++ y usarlos en Android.  
- **JNI (Java Native Interface)**: puente entre Java y C.  
- Definición en Java con `native`.  
- Implementación en C con firma específica.  

Ejemplo Java:
```java
public class NativeDemo {
    static { System.loadLibrary("native-lib"); }
    public native int sum(int a, int b);
}
```

---

## Implementación en C (JNI)
Ejemplo en `native-lib.c`:

```c
#include <jni.h>

JNIEXPORT jint JNICALL
Java_com_example_NativeDemo_sum(JNIEnv* env, jobject thiz, jint a, jint b) {
    return a + b;
}
```

---

## Manejo de Strings en JNI
- `jstring` ↔ `const char*`  
- Conversión con `GetStringUTFChars` / `ReleaseStringUTFChars`.  

Ejemplo:
```c
JNIEXPORT jstring JNICALL
Java_com_example_NativeDemo_hello(JNIEnv* env, jobject thiz) {
    const char* msg = "Hola desde C";
    return (*env)->NewStringUTF(env, msg);
}
```

---

## Manejo de Arrays en JNI
- `jintArray`, `jbyteArray` ↔ punteros en C.  
- Funciones: `GetArrayLength`, `GetIntArrayElements`, etc.  

Ejemplo:
```c
JNIEXPORT jint JNICALL
Java_com_example_NativeDemo_sumArray(JNIEnv* env, jobject thiz, jintArray arr) {
    jsize n = (*env)->GetArrayLength(env, arr);
    jint* elems = (*env)->GetIntArrayElements(env, arr, NULL);
    int sum = 0;
    for (int i=0; i<n; i++) sum += elems[i];
    (*env)->ReleaseIntArrayElements(env, arr, elems, 0);
    return sum;
}
```

---

## Depuración Nativa
- **Herramientas**:  
  - `logcat` para imprimir desde C (`__android_log_print`).  
  - `ndk-gdb` para depurar código nativo.  
  - Breakpoints en Android Studio.  
- Estrategia: reproducir bug → aislar → inspeccionar variables.  

---

## Laboratorio Día 3
1. Crear una función nativa `sum(int a, int b)` en C.  
2. Llamarla desde Java y mostrar resultado en pantalla.  
3. Agregar función nativa que devuelva un string “Hola desde C”.  
4. Probar depuración con logs (`logcat`) y/o breakpoints.  

---

## Cierre del Día
- Aprendiste a trabajar con punteros y estructuras en la VM.  
- Comprendiste cómo funciona JNI y cómo conectar C ↔ Java.  
- Practicaste strings y arrays en JNI.  
- Estás listo para abordar **debugging avanzado** (Día 4).  