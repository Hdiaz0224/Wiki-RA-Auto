# Monitoreo de Niveles de Tanque con CODESYS y OpenPLC Hector Diaz - Juan Pinilla

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


### 5.1 Monitoreo de Entradas y Salidas
<img width="1181" height="546" alt="image" src="https://github.com/user-attachments/assets/009b8af2-4d08-4652-b99c-570a59b69080" />
  

---

## 6. Resultados
- El sistema responde correctamente a todas las combinaciones válidas de los sensores.  
- Las condiciones de error (inconsistentes) activan la alarma **H5** de manera adecuada.  
- La validación con **OpenPLC** demuestra que la lógica implementada en Ladder es funcional y puede ejecutarse tanto en simulación como en hardware real (Arduino/Raspberry Pi).  

---

## 7. Conclusiones
- Se logró implementar exitosamente un **sistema de monitoreo de nivel de tanque** en CODESYS y validarlo en OpenPLC.  
- La combinación de **HMI + Ladder + OpenPLC** permite probar sistemas industriales sin necesidad inmediata de hardware físico.  

---
 **Archivos adjuntos en el repositorio:**  
- Código fuente CODESYS
- Programa OpenPLC

## 8. Videos
- Parte 1: https://teams.microsoft.com/l/meetingrecap?driveId=b%21-kXli7fJREuqEOu348eaZiLYJB6K7H5Mk9Sgk_I8sgCuXuKwWdo-QYEN_pKQQtUz&driveItemId=01S7ZM5AEMJKXVZY2BVZC2L2YJ2633MYTM&sitePath=https%3A%2F%2Funisabanaedu-my.sharepoint.com%2F%3Av%3A%2Fg%2Fpersonal%2Fhectordial_unisabana_edu_co%2FEYxKr1zjQa5FpesJ17e2YmwBHfHVIATMIP7-FkkLx0Swcg&fileUrl=https%3A%2F%2Funisabanaedu-my.sharepoint.com%2F%3Av%3A%2Fg%2Fpersonal%2Fhectordial_unisabana_edu_co%2FEYxKr1zjQa5FpesJ17e2YmwBHfHVIATMIP7-FkkLx0Swcg&threadId=19%3A2d0b93be-fad5-40ae-82db-23f844f8b3bb_ed4b34ad-f88d-491b-af33-c1a5af1d0dae%40unq.gbl.spaces&callId=61c5f922-86fb-45c1-84e5-ef2207baaf57&threadType=OneOnOneChat&meetingType=Unknown&subType=RecapSharingLink_RecapChiclet
- Parte 2: https://youtu.be/HDot0iQZR7k
