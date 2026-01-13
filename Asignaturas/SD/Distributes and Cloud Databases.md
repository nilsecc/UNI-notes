# **TEMA 6 – DISTRIBUTED & CLOUD DATABASES**

## **1. ¿Qué es una base de datos?**

Una **base de datos** es:

- Un repositorio para **almacenar datos**
    
- Permite:
    
    - Consultar
        
    - Actualizar
        
    - Mantener **integridad**
        

Se accede mediante un **lenguaje de consulta** (ej. SQL).

### **Tipos principales**

- **Relacionales (SQL)**
    
- **No Relacionales (NoSQL)**
    

---

## **2. Sistema de ficheros vs Base de datos**

|Característica|File System|DBMS|
|---|---|---|
|Organización|Archivos|Tablas|
|Integridad|Baja|Alta|
|Seguridad|Básica|Avanzada|
|Consultas|Limitadas|Complejas|
|Recuperación|Manual|Automática|
|Relaciones|Pobres|Fuertes|
|Concurrencia|Baja|Alta|

👉 Para datos grandes y multiusuario → **DBMS**

---

![[Pasted image 20260113202105.png]]

## **3. Bases de datos relacionales (SQL)**

### **Características**

- Datos en **tablas**
    
- Esquema **fijo**
    
- Relaciones claras
    
- Garantizan:
    
    - Consistencia
        
    - Integridad
        

### **Casos típicos**

- Bancos
    
- Universidades
    
- Empresas
    

### **Escalabilidad**

- **Vertical** (más CPU/RAM)
    

---

## **4. Limitaciones de las BBDD relacionales**

- Diseñadas en los años 70
    
- No pensadas para **Big Data**
    
- Problemas:
    
    - Caída de rendimiento
        
    - Locks
        
    - Bottlenecks
        
    - Single Point of Failure
        

👉 Horizontalizar es **complejo**

---

## **5. Bases de datos NoSQL**

### **Qué son**

- No usan tablas + joins
    
- Esquema flexible
    
- Datos semi/no estructurados
    

### **Tipos**

- Key–Value
    
- Document
    
- Column-family
    
- Graph
    

### **Ventajas**

- Escalabilidad **horizontal**
    
- Alta disponibilidad
    
- Alto rendimiento
    
- Bajo coste
    

---

## **6. Bases de datos distribuidas**

Una **Distributed Database**:

- Datos repartidos en **múltiples nodos**
    
- Para el usuario parece **una sola BD**
    
- Mejora:
    
    - Rendimiento
        
    - Disponibilidad
        
    - Tolerancia a fallos
        

---

## **7. Sharding (CLAVE)**

### **Qué es**

- **Particionado horizontal**
    
- Cada shard contiene **parte de los datos**
    

### **Ventajas**

- Evita sobrecargar un servidor
    
- Permite millones de usuarios
    

### **Ejemplo: Instagram Likes**

- Cada región tiene un contador local
    
- Se agregan resultados periódicamente
    
- Reduce latencia
    

---

## **8. Contadores distribuidos**

Problema:

- Millones de likes simultáneos
    

Solución:

- Contadores locales por shard
    
- **Background jobs**:
    
    - Agregan valores
        
    - Guardan en caché (Redis)
        
    - Se actualiza cada X minutos
        

👉 Sacrificas exactitud inmediata por rendimiento

---

## **9. HyperLogLog (HLL)** 🧠

### **Qué es**

- Algoritmo probabilístico
    
- Cuenta **elementos únicos**
    
- Usa **muy poca memoria**
    

### **Idea clave**

- Usa hashes
    
- Cuenta ceros iniciales
    
- Estima cardinalidad
    

### **Ventajas**

- Rápido
    
- Escalable
    
- Error controlado
    

❌ No es exacto  
✔ Ideal para analytics

---

## **10. Consenso en sistemas distribuidos**

Cuando necesitas **exactitud total**:

- **Paxos**
    
- **Raft**
    

### **Características**

- Fuerte consistencia
    
- Locks distribuidos
    
- Más lentos
    

👉 Se usan cuando **no puedes fallar**

---

## **11. El problema del tiempo en sistemas distribuidos**

- No hay **reloj global**
    
- Cada nodo tiene su propio reloj
    
- Hay **drift**
    

👉 Solución: **relojes lógicos**

---

## **12. Lamport Clocks**

### **Objetivo**

- Ordenar eventos
    
- Mantener causalidad
    

### **Reglas**

1. Evento interno → `Ci = Ci + 1`
    
2. Enviar mensaje → enviar timestamp
    
3. Recibir mensaje →  
    `Cj = max(Cj, tm) + 1`
    

### **Limitación**

- No detecta concurrencia
    
- Impone orden artificial
    

---

## **13. Vector Clocks**

### **Qué mejoran**

- Detectan eventos concurrentes
    

### **Funcionamiento**

- Vector con N posiciones (una por proceso)
    
- Permite comparar causalidad real
    

👉 Más precisos que Lamport  
👉 Más costosos

---

## **14. Database as a Service (DBaaS)**

### **Idea**

- No gestionas servidores
    
- Solo:
    
    - Modelo
        
    - Consultas
        
    - Optimización
        

AWS gestiona:

- Backups
    
- Escalado
    
- Parches
    
- Alta disponibilidad
    

---

## **15. Amazon RDS (Relacional)**

### **Qué es**

- Servicio gestionado SQL
    

### **Motores**

- MySQL
    
- PostgreSQL
    
- Oracle
    
- SQL Server
    
- MariaDB
    
- Aurora
    

### **Arquitectura**

- Engine en EC2
    
- Datos en **EBS**
    

---

## **16. RDS Alta disponibilidad**

### **Multi-AZ**

- Réplica síncrona
    
- Failover automático
    
- No accesible directamente
    

### **RDS Proxy**

- Pool de conexiones
    
- Failover transparente
    
- Reduce carga
    

---

## **17. RDS Read Replicas**

- Réplicas **asíncronas**
    
- Solo lectura
    
- Ideales para:
    
    - Lecturas intensivas
        
    - Escalar horizontalmente
        

---

## **18. DynamoDB (NoSQL)** 🚀

### **Características**

- Key–Value + Document
    
- Ultra baja latencia
    
- Escala automáticamente
    
- Sin esquema fijo
    

### **Modelo**

- Table
    
- Item ≈ fila
    
- Attribute ≈ columna
    

---

## **19. Claves en DynamoDB**

- **Partition key** (obligatoria)
    
- **Sort key** (opcional)
    
- **Primary key**:
    
    - Simple (partition)
        
    - Compuesta (partition + sort)
        

Ejemplo:

- IoT
    
- Partition → deviceID
    
- Sort → timestamp
    

---

## **20. DynamoDB Global Tables**

- Multi-región
    
- Multi-master
    
- Replicación automática
    
- Alta disponibilidad global
    

---

## **21. Resumen mental de examen 🧠**

- SQL → consistencia, relaciones
    
- NoSQL → escalabilidad
    
- Sharding → horizontal
    
- HLL → aproximado
    
- Paxos/Raft → exactitud
    
- Lamport → orden
    
- Vector clocks → concurrencia
    
- RDS → SQL gestionado
    
- DynamoDB → NoSQL extremo