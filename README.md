# Monitoreo de Niveles de Tanque con CODESYS y OpenPLC

## 1. Introducción
Este proyecto tiene como objetivo **diseñar, simular y validar** un sistema de monitoreo de niveles de líquido en un tanque químico, usando **CODESYS** para la implementación del diagrama Ladder y **OpenPLC Runtime** para la validación con hardware simulado/real.  

El sistema permite detectar cinco condiciones de operación:  
- **H1:** Nivel correcto  
- **H2:** Nivel bajo  
- **H3:** Nivel alto  
- **H4:** Tanque vacío  
- **H5:** Condiciones de error  

---

## 2. Descripción del Sistema
El tanque cuenta con tres sensores de nivel:  

- **SL (Low sensor):** Detecta si el tanque tiene al menos nivel mínimo.  
- **SM (Medium sensor):** Detecta el nivel intermedio.  
- **SH (High sensor):** Detecta el nivel máximo permitido.  

Las salidas corresponden a indicadores luminosos (luces en HMI o HMI virtual en OpenPLC):  

- **H1:** Nivel correcto  
- **H2:** Nivel demasiado bajo  
- **H3:** Nivel demasiado alto  
- **H4:** Tanque vacío  
- **H5:** Error de lectura / combinación inconsistente  

---

## 3. Tabla de Verdad

| SL | SM | SH | H1 (Correct) | H2 (Too Low) | H3 (Too High) | H4 (Empty) | H5 (Error) |
|----|----|----|--------------|--------------|---------------|-------------|------------|
| 0  | 0  | 0  | 0            | 0            | 0             | 1           | 0          |
| 1  | 0  | 0  | 0            | 1            | 0             | 0           | 0          |
| 1  | 1  | 0  | 1            | 0            | 0             | 0           | 0          |
| 1  | 1  | 1  | 0            | 0            | 1             | 0           | 0          |
| 0  | 1  | 0  | 0            | 0            | 0             | 0           | 1          |
| 0  | 0  | 1  | 0            | 0            | 0             | 0           | 1          |
| 1  | 0  | 1  | 0            | 0            | 0             | 0           | 1          |

---

## 4. Diseño en CODESYS

### 4.1 Diagrama Ladder
En CODESYS se implementó la lógica en lenguaje **Ladder Diagram (LD)**, a partir de la tabla de verdad anterior.  

<img width="936" height="781" alt="image" src="https://github.com/user-attachments/assets/38f05080-fd65-4ed7-bd30-57df4ba7c59b" />

<img width="688" height="609" alt="image" src="https://github.com/user-attachments/assets/c7e669f2-3f1e-4f6e-9c5c-a577892afac5" />


### 4.2 Lógica implementada
- Se utilizó una bobina por cada salida (H1…H5).  
- La bobina **H5 (Error)** fue diseñada como una rama OR con tres condiciones inválidas:  
  - `SM=1 y SL=0`  
  - `SH=1 y SM=0`  
  - `SH=1 y SL=0`  

---

## 5. Validación en OpenPLC

### 5.1 Configuración del Runtime
*Sección pendiente de realizar con hardware físico (Arduino y LEDs).*  

📷 *[Espacio reservado para fotos de la conexión física del Arduino con los LEDs]*  

### 5.2 Monitoreo de Entradas y Salidas
*Pruebas de validación con hardware real aún por realizar.*  

📷 *[Espacio reservado para capturas del Monitoring mostrando el funcionamiento con el Arduino]*  

---

## 6. Resultados
- El sistema responde correctamente a todas las combinaciones válidas de los sensores.  
- Las condiciones de error (inconsistentes) activan la alarma **H5** de manera adecuada.  
- La validación con **OpenPLC** demuestra que la lógica implementada en Ladder es funcional y puede ejecutarse tanto en simulación como en hardware real (Arduino/Raspberry Pi).  

---

## 7. Conclusiones
- Se logró implementar exitosamente un **sistema de monitoreo de nivel de tanque** en CODESYS y validarlo en OpenPLC.  
- La combinación de **HMI + Ladder + OpenPLC** permite probar sistemas industriales sin necesidad inmediata de hardware físico.  
- El diseño modular de la lógica facilita futuras ampliaciones, como control de bombas o comunicación SCADA.  

---
 **Archivos adjuntos en el repositorio:**  
- Código fuente CODESYS
- Programa OpenPLC 
