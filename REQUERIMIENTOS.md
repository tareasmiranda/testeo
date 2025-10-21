# Requerimientos Transportes Ltda.

## 1. Descripción del Cliente y Problema
### **Problema**
Transportes Ltda. requiere un sistema que permita planificar, asignar y dar seguimiento en tiempo real a los recorridos de entrega de mercancía, reduciendo la dependencia de planillas físicas y llamadas telefónicas, evitando pérdidas de información y retrasos en la entrega.

## **2. Usuarios**
- Administrador
- Empleado (Camionero o Peoneta)
- Generador de Carga
- Recepcionista de Carga

## **3. Funciones Necesarias (MVP)**
- Gestión de Empleados (CRUD)
- Gestión de Vehículos (CRUD)
- Gestión de Recorridos (crear, editar, cancelar)
- Asignación de Recorrido a Vehículo
- Consulta de Recorrido por parte del Empleado
- Visualización del Estado de Entrega (Generador y Recepcionista)
- Actualización del Estado de Entrega por el Empleado
- Confirmación de Entrega por el Recepcionista
- Verificación de Entrega por el Administrador

## **4. Datos a guardar**
**Empleado**
- ID, nombre, apellido, licencia, teléfono, rol (camionero/peoneta)

**Vehículo**
- Patente, marca, modelo, año, capacidad, estado

**Recorrido**
- ID, origen, destino, fecha_carga, fecha_descarga_estimada, fecha_descarga_real, estado, vehículo_asignado, empleados_asignados

**Estado de Entrega**
- ID, recorrido_id, timestamp, ubicación, observaciones, tipo_evento (en tránsito, en descarga, entregado, etc.)

**Confirmación de Entrega**
- ID, estado_entrega_id, firma digital, nombre receptor, fecha_hora, observaciones

## **5. Reglas del negocio**
1. Solo el Administrador puede crear/modificar empleados, vehículos y recorridos.
2. Un recorrido solo puede ser asignado a un vehículo con disponibilidad en las fechas indicadas.
3. La fecha de descarga estimada se calcula automáticamente mediante API externa a partir del origen y destino.
4. El Empleado solo puede actualizar el estado de entrega de los recorridos que tenga asignados.
5. La confirmación de entrega solo puede realizarse una vez el estado “entregado” haya sido registrado.
6. El Generador de Carga y el Recepcionista solo tienen lectura del estado de entrega; no pueden modificarlo.
7. La confirmación de entrega extiende la verificación del Administrador: sin confirmación, la entrega se considera pendiente.

## **6. Prioridades**
1. Gestión de Recorridos y asignación a vehículos
2. Consulta y actualización del estado de entrega por empleados
3. Visualización del estado de entrega para Generador y Recepcionista
4. Confirmación de entrega y verificación por Administrador
5. Gestión de Empleados y Vehículos

## **7. Flujos Principales**
**Flujo 1: Crear y asignar recorrido**
1. Administrador crea recorrido (origen, destino, fecha carga).
2. Sistema calcula fecha descarga estimada.
3. Administrador asigna vehículo disponible.
4. Sistema notifica a empleados asignados.

**Flujo 2: Ejecutar recorrido**
1. Empleado consulta recorrido asignado.
2. Empleado actualiza estado de entrega durante el trayecto.
3. Al llegar, empleado marca “entregado”.
4. Recepcionista confirma entrega con firma digital.
5. Administrador verifica entrega final.

**Flujo 3: Seguimiento externo**
1. Generador/Recepcionista consultan estado de entrega vía link único o dashboard.
2. Sistema muestra último evento y fecha estimada de descarga.

## **8. Requisitos Funcionales y No Funcionales**

### **Requisitos Funcionales**
- RF-01: CRUD de Empleados.
- RF-02: CRUD de Vehículos.
- RF-03: CRUD de Recorridos con cálculo automático de fecha estimada.
- RF-04: Asignación de recorrido a vehículo y empleados.
- RF-05: Consulta de recorrido por empleado.
- RF-06: Actualización de estado de entrega por empleado.
- RF-07: Visualización de estado de entrega para Generador y Recepcionista.
- RF-08: Confirmación de entrega con firma digital.
- RF-09: Verificación de entrega por Administrador.
- RF-10: Exportación de reporte de entregas (CSV/PDF).

### **Requisitos no funcionales**
- RNF-01: Autenticación JWT por rol.
- RNF-02: API externa de estimación de tiempos (Google Maps o similar) con cacheo de 24 h.
- RNF-03: Respuesta < 500 ms para consultas de estado.
- RNF-04: Disponibilidad 99 % en horario laboral (08:00-20:00).
- RNF-05: Compatible con navegadores Chrome, Edge, Safari últimas 2 versiones.
- RNF-06: Responsive para tablets de 8” mínimo.

### **Funciones a Futuro**
- App móvil offline para empleados.
- Geolocalización GPS en tiempo real.
- Notificaciones push a clientes.
- Integración con sistema de facturación.
- Dashboard de KPIs (tiempo promedio de entrega, % entregas tardías).

## **9. Plazo deseado**
3 meses desde la firma del contrato.

## **10. Presupuesto Fijo**

### **Alcance**
Sistema web multirol, hosting primer año, 5 usuarios concurrentes, capacitación 8 h, documentación técnica y de usuario.

### **Posibles Implementaciones**
- Opción A: Desarrollo full-stack en Laravel + MySQL + Vue.
- Opción B: Desarrollo en Firebase + Angular (menor tiempo, mayor costo mensual).

### **Presupuesto**
USD 9 500 (Opción A) – fijo, cerrado, pagos: 30 % inicio, 40 % entrega parcial, 30 % entrega final.

## **11. Propuesta formal y cronograma de trabajo**
**Mes 1**
- Semana 1-2: Levantamiento de requerimientos y diseño de base de datos.
- Semana 3-4: Desarrollo de módulos de autenticación y gestión de empleados/vehículos.

**Mes 2**
- Semana 1-2: Desarrollo de recorridos y asignaciones.
- Semana 3-4: Desarrollo de estados de entrega y confirmaciones.

**Mes 3**
- Semana 1-2: Integración de API de tiempos, reportes y pruebas internas.
- Semana 3: Capacitación y despliegue en producción.
- Semana 4: Periodo de observación y ajustes menores.

## **12. Soporte y Mantenimiento**

### **Garantía Gratis (2 meses)**
- Corrección de bugs críticos sin costo (respuesta < 4 h hábiles).
- Ajustes menores en textos o colores (máx 5 horas).

### **Garantía Opcional**
- Plan de soporte mensual post-garantía: USD 250 / mes (10 h de soporte, respuesta < 8 h hábiles).
- Horas adicionales: USD 35 / hora.