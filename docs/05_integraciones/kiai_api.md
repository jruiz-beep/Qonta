# 5.2 Integración con Kiai (Proveedor Tecnológico)

Kiai es el Proveedor Tecnológico (PT) que facilita a Qonta el cumplimiento de los requisitos de la **Factura Electrónica como Título Valor** o **Radián**, lo que incluye la generación de eventos de acuse de recibo y aceptación/rechazo.

## 5.2.1 Propósito de la Integración

La integración con Kiai permite a Qonta gestionar el ciclo de vida de los eventos de la factura electrónica, un proceso vital para que la factura pueda ser descontada, endosada o utilizada como título valor.

Los eventos gestionados a través de esta API son:

1.  **Acuse de Recibo de la Factura Electrónica de Venta.**
2.  **Recibo del Bien o Prestación del Servicio.**
3.  **Aceptación Expresa o Tácita de la Factura.**
4.  **Rechazo de la Factura.**

## 5.2.2 Mecanismo de Integración

Qonta se integra con Kiai a través de una **API REST** mediante llamadas seguras (HTTPS).

| Operación en Qonta | Servicio Kiai Consumido | Detalle del Flujo |
| :--- | :--- | :--- |
| **Generar Acuse** | `/api/events/acuse_recibo` | Qonta identifica la factura y el cliente, y envía el CUFE junto con las credenciales del cliente a Kiai. Kiai procesa el evento y retorna la respuesta de la DIAN. |
| **Generar Recibo de Bien** | `/api/events/recibo_bien` | Se ejecuta cuando el cliente confirma la recepción del bien o servicio. Qonta envía la solicitud a Kiai. |
| **Generar Aceptación Expresa** | `/api/events/aceptacion_expresa` | Se utiliza para dar conformidad de forma explícita antes de la aceptación tácita. |
| **Generar Rechazo** | `/api/events/rechazo_factura` | Se utiliza si la factura presenta inconsistencias. Requiere un `MotivoRechazoFK` de la tabla `MotivosRechazoFacturas`. |
| **Consulta de Eventos** | `/api/events/consulta_eventos` | Permite consultar el estado actual de los eventos de una factura en particular en el sistema de Kiai/DIAN. |

## 5.2.3 Manejo de Credenciales y Seguridad

* **Credenciales:** Cada cliente de Qonta debe tener credenciales configuradas en Kiai (token de acceso, NIT, etc.). Estas credenciales se almacenan de forma segura y cifrada en la configuración de `Clientes`.
* **Trazabilidad:** La respuesta de la API de Kiai (incluyendo el XML del evento y el CUNE - Código Único de Notificación de Evento) es vital y debe ser almacenada en el sistema de Qonta para garantizar la trazabilidad legal de cada evento.
* **Modelo de Datos:** La ejecución de estas integraciones tiene un impacto directo en el campo `Estado` de la tabla `FacturasRecibidasDian` y puede generar nuevos registros en la tabla `TrazabilidadesCausaciones` para reflejar el cambio en el ciclo de vida de la factura.