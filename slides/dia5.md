---
marp: true
theme: default
paginate: true
class: invert
---

# Día 5 – EMV Fundamentos I

---

## Objetivos del Día
- Introducir el estándar **EMV** y su importancia en pagos.  
- Comprender el **flujo de una transacción EMV con contacto**.  
- Analizar estructuras TLV y comandos APDU básicos.  
- Revisar conceptos de criptografía aplicada en EMV.  
- Laboratorio: parsear y entender una respuesta TLV.  

---

## ¿Qué es EMV?
- Estándar global para transacciones con tarjetas de chip.  
- Desarrollado por **Europay, MasterCard y Visa**.  
- Define cómo la tarjeta y el terminal se comunican.  
- Se usa en:  
  - Tarjetas de crédito y débito.  
  - Terminales punto de venta (POS).  
  - Cajeros automáticos (ATM).  

---

## Flujo EMV – Paso 1: Selección de Aplicación (SELECT AID)
- El terminal envía un **APDU SELECT** con un AID (Application Identifier).  
- La tarjeta responde con un **FCI (File Control Information)**.  
- Dentro del FCI se incluye el nombre de la aplicación y datos básicos.  

Ejemplo APDU:  
```
00 A4 04 00 07 A0 00 00 00 03 10 10 00
```  
- Selección de VISA (AID A0000000031010).  

---

## Flujo EMV – Paso 2: Lectura de Datos
- El terminal usa APDUs **READ RECORD** para obtener información de la tarjeta.  
- Datos importantes:  
  - PAN (Primary Account Number).  
  - Fecha de expiración.  
  - Nombre del titular (opcional).  
  - Parámetros de seguridad.  

Ejemplo:  
```
00 B2 01 0C 00
```  
- Lee el primer registro del archivo EMV.  

---

## Flujo EMV – Paso 3: Procesamiento de Restricciones
- El terminal verifica:  
  - Fecha de expiración.  
  - Restricciones de uso (ej. internacional, chip-only).  
- Si falla, se rechaza la transacción antes de continuar.  

---

## Flujo EMV – Paso 4: Verificación del Titular (CVM)
- Determina cómo se validará la identidad del titular:  
  - **PIN offline** (cifrado en el chip).  
  - **PIN online** (enviado al emisor para validación).  
  - Firma o “sin verificación”.  
- El terminal consulta la **CVM List** almacenada en la tarjeta.  

---

## Flujo EMV – Paso 5: Autenticación de Datos
- Métodos:  
  - **SDA (Static Data Authentication)** – basado en datos fijos.  
  - **DDA (Dynamic Data Authentication)** – usa llaves asimétricas.  
  - **CDA (Combined DDA/Application Cryptogram)** – autenticación más robusta.  
- El objetivo es validar que la tarjeta no ha sido clonada.  

---

## Flujo EMV – Paso 6: Procesamiento de Riesgo
- El terminal evalúa el riesgo de la transacción:  
  - Monto límite.  
  - País de origen.  
  - Uso de lista negra (revoked cards).  
- Se genera el **TVR (Terminal Verification Results)** con bits que indican los hallazgos.  

---

## Flujo EMV – Paso 7: Acción del Terminal
- El terminal decide si:  
  - Aprueba offline.  
  - Refiere al emisor (online).  
  - Rechaza la transacción.  
- Decisión basada en TVR, TSI y reglas del emisor.  

---

## Flujo EMV – Paso 8: Solicitud de Criptograma
- El terminal genera un **UN (Unpredictable Number)**.  
- Envía comando **GENERATE AC** a la tarjeta.  
- La tarjeta responde con un **ARQC (Authorization Request Cryptogram)**.  

ARQC = firma criptográfica única de la transacción.  

---

## Flujo EMV – Paso 9: Validación Online
- El ARQC se envía al **emisor** a través de la red.  
- El emisor valida con llaves compartidas y responde con un **ARPC (Authorization Response Cryptogram)**.  
- Si es válido, la transacción se aprueba.  

---

## Flujo EMV – Paso 10: Script Processing y Completado
- El emisor puede enviar **scripts** al terminal para actualizar la tarjeta.  
- Ejemplo: bloquear tarjeta, actualizar llaves.  
- Finalmente, se cierra la transacción con un **TC (Transaction Certificate)** o se rechaza.  

---

## TLV y APDU Básicos
- **TLV (Tag-Length-Value):** formato de datos en EMV.  
  - Ejemplo: `5A 08 1234567890123456` → Tag=5A, Len=08, Value=PAN.  
- **APDU (Application Protocol Data Unit):** comandos entre terminal y tarjeta.  
  - Ejemplo: SELECT, READ RECORD, GENERATE AC.  

---

## Criptografía en EMV
- Claves **asimétricas** para autenticación de tarjeta.  
- Claves **simétricas** (3DES) para generar ARQC/ARPC.  
- Importancia de **UN (Unpredictable Number)** en la seguridad.  

---

## Laboratorio Día 5
1. Analizar respuesta TLV ficticia de un **SELECT AID**.  
2. Identificar:  
   - AID.  
   - Nombre de aplicación.  
   - Parámetros adicionales.  
3. Explicar cómo estos datos participan en el flujo EMV.  

---

## Cierre del Día
- Entendiste el **flujo de transacción EMV contacto paso a paso**.  
- Practicaste análisis de TLV y comandos APDU.  
- Ya puedes relacionar EMV con **criptografía y seguridad en pagos**.  
- Próximo paso: **EMV Contactless y depuración en código** (Día 6).  