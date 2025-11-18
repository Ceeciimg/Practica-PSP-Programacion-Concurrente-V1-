# Proyecto Amadzon (Versión 1 - V1)

**Autora:** Cecilia Molina  
**Asignatura:** Programación de Servicios y Procesos (PSP)  
**Curso:** 2º DAM  

---

## 📖 Descripción del proyecto
Este proyecto implementa una **simulación concurrente** inspirada en el funcionamiento interno de una tienda llamada **Amadzon**.

El objetivo principal es aplicar los conceptos clásicos de sincronización utilizando:

- `synchronized`
- `wait()`
- `notifyAll()`

La implementación se corresponde con la **Versión 1 (V1)**, basada exclusivamente en monitores y mecanismos clásicos de concurrencia en Java.

---

## 🧩 Componentes principales

### Clase `Pedido`
Representa un pedido con los siguientes atributos:
- `id`
- `producto`
- `estado` (`PENDIENTE`, `PROCESANDO`, `ENVIADO`)

---

### Clase `ColaPedidos` (Monitor compartido)
Simula un almacén donde los clientes depositan pedidos y los gestores/transportistas los retiran.

**Características:**
- Capacidad máxima: **25 pedidos**
- Métodos sincronizados:
  - `añadir()`
  - `retirar()`

**Control de concurrencia:**
- Esperar si la cola está llena
- Esperar si la cola está vacía
- Uso de `wait()` y `notifyAll()` para coordinar productores y consumidores

---

## 🔄 Hilos del sistema

### Cliente (`Cliente extends Thread`)
- Genera pedidos continuamente  
- Los añade a `ColaPedidos`  
- Intenta comprar productos exclusivos  

### Gestor de Almacén (`GestorAlmacen implements Runnable`)
- Retira pedidos de la cola  
- Marca los pedidos como **PROCESANDO**  
- Simula el procesado mediante pausas (`sleep()`)  

### Transportista (Thread mediante expresión lambda)
- Retira pedidos ya procesados  
- Los marca como **ENVIADOS**  

---

## 🛒 Gestión de Productos Exclusivos

Clase encargada de gestionar un conjunto de productos exclusivos con stock limitado.

**Características:**
- Stock de **1 unidad por producto (A–F)**

**Proceso de compra:**
1. Asegurar el primer producto  
2. Intentar asegurar el segundo mediante **timeout**  
3. Si falla la segunda adquisición, se libera el primero  

**Sincronización mediante:**
- `synchronized`
- `wait()`
- `notifyAll()`

**Otros aspectos:**
- Manejo de tiempos aleatorios para simular operaciones reales  
- Evita condiciones de carrera mediante exclusión mutua y notificación adecuada a hilos en espera  

---

## 🖥️ Clase `MainV1` (coordinación de la simulación)

Se encarga de:
- Crear los recursos compartidos:
  - `ColaPedidos`
  - `ProductosExclusivos`
- Lanzar:
  - Clientes
  - Gestores del almacén
  - Transportistas
- Permitir que la simulación se ejecute durante un tiempo determinado  
- Detener los hilos de forma controlada  

---

## ✅ Resumen del funcionamiento
- Implementación estricta con `synchronized`, `wait()` y `notifyAll()`  
- Cola de pedidos con capacidad máxima **25**  
- Stock exclusivo de **1 unidad por producto**  
- Si falla la adquisición del segundo producto, se libera el primero  
- Coordinación correcta para evitar condiciones de carrera y garantizar el acceso ordenado a los recursos compartidos  
