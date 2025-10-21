# Requerimientos Transportes Ltda.

## 1. Descripción del Cliente y Problema
**Importante Empresa de transporte de la región de la Araucanía**, encargada de distribuir mercadería en la zona, necesita un nuevo sistema de gestión de itinerarios.

### **Problema** 
Se necesita optimizar y modernizar su sistema de gestión de itinerarios. Actualmente la empresa cuenta con un sistema antiguo y complicado de manejar para los nuevos empleados, utilizando principalmente planillas físicas para gestionar todos los pedidos.

No se cuenta con un sistema de monitoreo de rutas de camiones, ni registro digital de entregas. El sistema actual requiere mucho input de los camioneros, quienes llevan el registro de forma manual y administran el pago del cliente. Utilizan una planilla de papel para ver su itinerario, la cual no siempre se sigue al pie de la letra y resulta difícil registrar cambios inesperados.

## **2. Usuarios**
- **Administrador**: Gestiona Empleados, Gestiona Recorridos, Gestiona Vehículos, Verifica Entrega, Asigna Recorrido a Vehículo
- **Empleado (Camionero o Peoneta)**: Consulta Recorrido, Actualiza Estado de Entrega
- **Generador de Carga**: Ver estado de Entrega
- **Recepcionista de Carga**: Ver estado de Entrega, Confirmar Entrega

## **3. Funciones Necesarias (MVP)**
1. **Creación de API para integraciones** a sistemas externos
2. **Gestión completa del transporte**:
   - Fecha y horas de carga y descarga de la mercancía
   - Volumen de los pedidos
   - Ubicación por GPS de los camiones en ruta para estimar fecha y hora de llegada
   - Confirmar la entrega al momento de la recepción del pedido
3. **Sistema de gestión de itinerarios digital** que reemplace planillas físicas
4. **Monitoreo en tiempo real** de rutas de camiones
5. **Registro digital de entregas** con confirmación de recepción

## **4. Datos a guardar**
**Empleado**
- ID, nombre, apellido, licencia, teléfono, rol (camionero/peoneta), horas conducidas diarias

**Vehículo**
- Patente, marca, modelo, año, capacidad, estado, revisión técnica, documentos en circulación, peso máximo soportado

**Recorrido**
- ID, origen, destino, fecha_carga, fecha_descarga_estimada, fecha_descarga_real, estado, vehículo_asignado, empleados_asignados, volumen_pedidos

**Estado de Entrega**
- ID, recorrido_id, timestamp, ubicación_GPS, observaciones, tipo_evento (en tránsito, en descarga, entregado, etc.)

**Confirmación de Entrega**
- ID, estado_entrega_id, firma digital, nombre receptor, fecha_hora, observaciones, pago_confirmado

## **5. Reglas del negocio**
1. **Días de la semana establecidos de reparto**
2. **Cantidad de horas que puede manejar un conductor por día no debe exceder las 8~ horas contextualizadas para la ubicación de la empresa con descansos incluidos** dentro del marco legal (descanzo entre 5 horas de recorrido establecido por ley, 180 horas mensuales)
3. **Camiones deben estar en óptimas condiciones**:
   - Revisión técnica al día
   - Documentos del vehículo siempre en circulación
   - No exceder el peso de carga máximo soportado por el camión
4. **La confirmación de entrega solo puede realizarse una vez el estado "entregado" haya sido registrado**
5. **El Generador de Carga y el Recepcionista solo tienen lectura del estado de entrega; no pueden modificarlo**
6. **La fecha de descarga estimada se calcula automáticamente** mediante API externa a partir del origen y destino

## **6. Prioridades**
1. **Gestión de Recorridos y asignación a vehículos**
2. **Consulta y actualización del estado de entrega por empleados**
3. **Visualización del estado de entrega para Generador y Recepcionista**
4. **Confirmación de entrega y verificación por Administrador**
5. **Gestión de Empleados y Vehículos**
6. **Implementación de API para integraciones externas**

## **7. Flujos Principales**
**Flujo 1: Crear y asignar recorrido**
1. Administrador crea recorrido (origen, destino, fecha carga, volumen)
2. Sistema calcula fecha descarga estimada vía API
3. Administrador asigna vehículo disponible y conductor
4. Sistema notifica a empleados asignados
5. Camionero accede a itinerario digital

**Flujo 2: Ejecutar recorrido con GPS**
1. Empleado consulta recorrido asignado en app web
2. Sistema registra ubicación GPS durante el trayecto
3. Empleado actualiza estado de entrega en tiempo real
4. Al llegar, empleado marca "entregado"
5. Recepcionista confirma entrega
6. Administrador verifica entrega final

**Flujo 3: Seguimiento externo**
1. Generador/Recepcionista consultan estado de entrega vía app web

## **8. Requisitos Funcionales y No Funcionales**

### **Requisitos Funcionales**
- RF-01: Creación de API REST para integraciones externas
- RF-02: CRUD de Empleados con control de horas conducidas
- RF-03: CRUD de Vehículos con validación de documentación
- RF-04: CRUD de Recorridos con cálculo automático de fecha estimada
- RF-05: Asignación de recorrido a vehículo y empleados
- RF-06: Consulta de recorrido por empleado vía app web
- RF-07: Actualización de estado de entrega por empleado
- RF-08: Visualización de estado de entrega para Generador y Recepcionista
- RF-09: Verificación de entrega por Administrador
- RF-10: Tracking GPS en tiempo real de camiones

### **Requisitos No Funcionales**
- RNF-01: Autenticación JWT por rol
- RNF-02: API externa de estimación de tiempos (Google Maps) con cacheo de 24 h
- RNF-03: Respuesta < 500 ms para consultas de estado
- RNF-04: Disponibilidad 99 % en horario laboral
- RNF-05: Compatible con navegadores modernos
- RNF-06: Compatible con Tablets de la empresa (por definir)
- RNF-07: Cumplimiento normativa de conducción (máx 8 horas)

### **Funciones a Futuro**:
- Integración con sistemas de facturación externos
- Dashboard de KPIs (tiempo promedio de entrega, % entregas tardías)
- Notificaciones push a clientes
- Sistema de pagos digitales integrado

## **9. Plazo deseado**:
3 meses desde la firma del contrato

## **10. Presupuesto Fijo**

### **Alcance**:
Sistema web multirol + app web empleados, hosting primer año, 10 usuarios concurrentes, capacitación 16 h, documentación técnica y de usuario, API de integración

### **Posibles Implementaciones**: 
- **Opción A**: Desarrollo full-stack en Node.js + MongoDB + React + React Native (estandar de la industria)
- **Opción B**: Desarrollo en Firebase + Angular + Flutter (sugreido para menor tiempo, mayor costo mensual)
- **Escalabilidad**: Sistema adaptable a flotas de 10-100 camiones (sugerido por inteligencia artificial)

### **Presupuesto no definido**:


## **11. Propuesta formal y cronograma de trabajo**

### Alcance propuesto – API / MVP Backend
- Endpoints CRUD completos para: empleados, vehículos, recorridos, estados de entrega, confirmaciones.  
- Búsqueda y filtrado por: patente, RUT conductor, fecha de carga, estado, destino.  
- Reservas (asignación de recorrido a vehículo/conductor) con validación de disponibilidad y límite de horas legales.  
- Autenticación JWT por rol (Administrador | Camionero | Peoneta | Generador | Recepcionista).   
- Dockerización: Dockerfile + docker-compose (app + MongoDB).  
- README con: levantamiento local, diagrama de arquitectura.

### **Cronograma por definir**

### **Roadmap del Proyecto por definir**

## **12. Soporte y Mantenimiento**

### **Garantía Gratis (2 meses)**:
- Servicio Técnico
- Parches de solución de errores
- Corrección de bugs críticos sin costo (respuesta < 4 h hábiles)
- Ajustes menores en textos o colores (máx 5 horas)

### **Garantía Opcional**:
- Mismos beneficios de la garantía gratis
- Mejoras al sistema
- **Plan de soporte mensual post-garantía**: USD 350 / mes (15 h de soporte, respuesta < 8 h hábiles)
- Horas adicionales: USD 45 / hora
- **Incluye**: actualizaciones de seguridad, mejoras de rendimiento, nuevos reportes básicos
