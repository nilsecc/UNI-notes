
# **TEMA 5 – CLOUD STORAGE (APUNTES PERFECTOS)**

## **1. File Server tradicional**

Un **file server** es un ordenador o servicio que:

- Almacena archivos
    
- Los organiza
    
- Los comparte a través de una red
    

👉 En vez de que cada usuario tenga sus archivos en su PC, **todo está centralizado**.

### **Problemas cuando hay muchos usuarios**

- **Congestión de red** → muchos accesos simultáneos
    
- **Cuello de botella de disco** → demasiadas lecturas/escrituras
    
- **CPU y RAM limitadas**
    
- **Límite físico de almacenamiento**
    
- **Single Point of Failure** → si cae el servidor, nadie accede
    

---

## **2. Distributed File System (DFS)**

Un **DFS**:

- Almacena archivos en **varios servidores**
    
- Para el usuario parece **un único sistema**
    
- Mejora:
    
    - Escalabilidad
        
    - Rendimiento
        
    - Tolerancia a fallos
        

Ejemplo:

- Servidor 1 → documentos
    
- Servidor 2 → multimedia
    
- Servidor 3 → backups
    

Si piden un `.jpg`, el sistema sabe a qué servidor ir.

---

## **3. Replicación en sistemas distribuidos**

### **Qué es**

Guardar **copias del mismo dato en varios nodos**.

### **Ventajas**

- **Tolerancia a fallos**
    
- **Balanceo de carga**
    
- **Acceso más rápido** (servidor más cercano)
    

### **Problemas**

- Más **espacio usado**
    
- **Consistencia** entre copias
    
- **Coste de sincronización**
    

---

## **4. RAID (base conceptual)**

RAID combina discos físicos en una unidad lógica.

|RAID|Protección|Tolera|
|---|---|---|
|RAID 0|Ninguna|0 discos|
|RAID 1|Replicación|1 disco|
|RAID 5|1 paridad (XOR)|1 disco|
|RAID 6|2 paridades|2 discos|

### **RAID 5**

- Usa **XOR**
    
- Paridad ≠ copia
    
- Si falla **1 disco**, se reconstruye
    
- Si fallan **2**, se pierde
    

---

## **5. Erasure Coding (EC)** 🔥 (IMPORTANTE)

### **Idea**

- Protege datos **sin replicar todo**
    
- Inspirado en **RAID 6**
    
- Usado en **sistemas distribuidos**
    

### **Funcionamiento**

- Divide datos en `k` bloques
    
- Genera `m` bloques de paridad
    
- Total = `k + m`
    
- Se puede recuperar con **cualquier k bloques**
    

Ejemplo:

- **RS(6,3)** → 6 datos + 3 paridades
    
- Se puede perder **hasta 3 nodos**
    

👉 Mucho más eficiente que replicar

---

## **6. Reed–Solomon (GF(2⁸))**

- Opera **byte a byte**
    
- Usa **álgebra en campos finitos**
    
- Si un cálculo supera 8 bits:
    
    - Se aplica el polinomio  
        `x⁸ + x⁴ + x³ + x² + 1`
        

Ejemplo:

`200 × 2 = 141   (en GF(2⁸))`

Clave para entender:

- XOR = suma
    
- Multiplicaciones siguen reglas del campo finito
    

---

## **7. Ejemplo práctico de Erasure Coding**

Datos:

`5, 9, 2, 8`

Paridades:

`P1 = (5+9)+(2+8) = 24 P2 = (5×2)+(9×8) = 82`

Si pierdes 5 y 9:

- Resuelves sistema de ecuaciones
    
- Recuperas los valores originales
    

👉 Exactamente lo que hace EC en cloud

---

## **8. Consistency Models**

### **Strong Consistency**

- Todos ven **siempre la última versión**
    
- Costosa
    
- Ejemplo: **bancos**
    

### **Eventual Consistency**

- Al principio puede haber versiones distintas
    
- Con el tiempo todo converge
    
- Más rápida y escalable
    

### **Mixed Consistency**

- Combina ambas
    
- Según tipo de operación
    

---

## **9. I/O en Cloud – Instance Store**

- Disco **local** de la VM
    
- **Ephemeral** (se borra al apagar)
    
- Gratis
    
- Ideal para:
    
    - Cachés
        
    - Buffers
        
    - Datos temporales
        

❌ No persistente  
❌ No cifrado  
❌ Solo accesible desde la VM

---

## **10. Tipos de almacenamiento en Cloud**

|Tipo|Persistente|Compartido|Acceso|
|---|---|---|---|
|Instance Store|❌|❌|Local|
|EBS|✅|❌|Misma AZ|
|EFS|✅|✅|Varias EC2|
|S3|✅|✅|Global|
|Glacier|✅|❌|Lento|

---

## **11. Tipos de almacenamiento (File / Block / Object)**

### **File Storage**

- Jerárquico
    
- Cambiar 1 byte → reescribir todo
    
- Ejemplo: **EFS**
    

### **Block Storage**

- Datos en bloques
    
- Solo se actualiza el bloque modificado
    
- Baja latencia
    
- Ejemplo: **EBS**
    

### **Object Storage**

- Objetos + metadata
    
- Sin jerarquía real
    
- Escalable y barato
    
- Ejemplo: **S3**
    

---

## **12. Amazon EBS (Block Storage)**

- Volúmenes persistentes
    
- Solo **1 EC2** por volumen
    
- Replicado en la AZ
    
- Ideal para:
    
    - Bases de datos
        
    - Sistemas operativos
        

### **Features**

- Snapshots
    
- Encriptación
    
- Elasticidad
    

---

## **13. Amazon S3 (Object Storage)**

- Buckets + Objects
    
- Objetos **inmutables**
    
- Hasta **5 TB por objeto**
    
- 11 nueves de durabilidad
    
- Acceso vía URL
    

### **Storage Classes**

- Standard
    
- Intelligent-Tiering
    
- Standard-IA
    
- One Zone-IA
    
- Glacier Flexible
    
- Glacier Deep Archive
    

---

## **14. Amazon EFS (File Storage compartido)**

- Sistema de archivos compartido
    
- Montado por **varias EC2**
    
- Escala automáticamente
    
- Ideal para:
    
    - Web
        
    - Big Data
        
    - Directorios home
        

### **Arquitecturas**

- Regional (multi-AZ)
    
- One-Zone (más barato, menos resiliente)
    

---

## **15. Amazon S3 Glacier (Cold Storage)**

- Archivo a largo plazo
    
- Muy barato
    
- 11 nueves de durabilidad
    

### **Retrieval**

|Tipo|Tiempo|
|---|---|
|Expedited|1–5 min|
|Standard|3–5 h|
|Bulk|5–12 h|

---

## **16. Resumen mental para el examen 🧠**

- **DFS** → escalabilidad + tolerancia a fallos
    
- **Replication** → simple pero costosa
    
- **Erasure Coding** → eficiente y matemática
    
- **EBS** → block, rápido, persistente
    
- **EFS** → file, compartido
    
- **S3** → object, global
    
- **Glacier** → barato, lento