# 4. Reglas de Negocio (Hooks)

El sistema Qonta incorpora un potente mecanismo de **Hooks** (también conocidos como "ganchos" o "disparadores") que permite a los clientes, y a los administradores del sistema, definir y ejecutar lógica de negocio personalizada en puntos clave del flujo de trabajo, sin necesidad de modificar el código fuente principal de la aplicación.

## 4.1 ¿Qué son los Hooks?

Los Hooks son fragmentos de código (actualmente en JavaScript) que se configuran y almacenan en la base de datos (`MovimientosSistemas.Campos`). Estos fragmentos son invocados por el sistema en momentos predefinidos durante la ejecución de una operación. Actúan como "interceptores" que pueden:

* **Leer** datos del contexto actual.
* **Modificar** datos antes de que se persistan o se utilicen en la siguiente etapa.
* **Validar** condiciones y, si no se cumplen, detener la operación y enviar un mensaje de error.
* **Ejecutar** lógica adicional (ej. notificaciones personalizadas, llamadas a APIs externas).

Este enfoque basado en eventos proporciona una gran flexibilidad para adaptar el comportamiento de Qonta a las necesidades específicas de cada cliente, especialmente en lo referente a las complejas reglas de causación contable y retenciones.

## 4.2 Arquitectura y Ejecución de Hooks

### 4.2.1 Almacenamiento

Los Hooks se almacenan en la base de datos, típicamente en la tabla `MovimientosSistemas`, en el campo `Campos` que podría contener un JSON o un bloque de texto con el código JavaScript. Cada `MovimientoSistema` puede representar un "tipo de evento" o un "punto de enganche" donde se puede inyectar lógica.

### 4.2.2 Puntos de Ejecución

El sistema Qonta define varios puntos de ejecución preestablecidos donde los hooks pueden ser activados. Ejemplos de estos puntos pueden ser:

* `antesDeProcesarFactura`: Ejecutado antes de que se inicie el análisis de una factura.
* `despuesDeAnalizarItemFactura`: Ejecutado después de que un ítem de factura ha sido clasificado.
* `antesDeGenerarCausacion`: Ejecutado inmediatamente antes de que se cree el registro de causación contable. Ideal para modificar cuentas, valores, o aplicar retenciones condicionales.
* `despuesDeGuardarCausacion`: Ejecutado tras la persistencia, ideal para disparar notificaciones o flujos de integración.

### 4.2.3 Contexto de Ejecución (Sandbox)

El código JavaScript de los Hooks se ejecuta en un entorno aislado (sandbox) para garantizar la seguridad y evitar que el código malicioso o erróneo afecte al sistema principal.

El hook recibe un objeto de contexto que contiene toda la información relevante para la operación actual:

| Propiedad del Contexto | Descripción |
| :--- | :--- |
| `factura` | Objeto completo de la factura actual (`FacturasRecibidasDian`). |
| `proveedor` | Objeto completo del proveedor asociado. |
| `cliente` | Objeto del cliente al que pertenece la factura. |
| `itemsCausar` | Array de ítems a ser causados (disponible en hooks de pre-causación). |
| `cuentasContables` | Array con las cuentas contables generadas hasta el momento (disponible en hooks de post-causación). |
| `logger` | Función para registrar mensajes de depuración en los logs del sistema. |
| `error` | Función para detener la ejecución y retornar un mensaje de error al usuario. |

## 4.3 Aplicaciones de los Hooks

Los hooks son la herramienta principal para la personalización de la lógica de negocio de Qonta:

1.  **Lógica de Retenciones Condicionales:** Aplicar una retención en la fuente o ICA solo si la suma de las bases de un proveedor supera un monto en un período específico.
2.  **Cuentas Contables Dinámicas:** Sobrescribir la cuenta contable de gasto por defecto basándose en palabras clave del ítem, en el centro de costos o en responsabilidades fiscales específicas que no están cubiertas por las reglas estándar.
3.  **Validación Personalizada:** Evitar la causación de facturas que no cumplan con una regla interna del cliente (ej. facturas mayores a X valor deben ser aprobadas manualmente por un usuario específico).
4.  **Integración de Datos:** Enviar una notificación a un sistema externo o actualizar un campo del CRM después de una causación exitosa.

## 4.4 Ejemplo de Hook (Pseudo-código)

```javascript
// Hook: antesDeGenerarCausacion
function customCausacionLogic(context) {
    const factura = context.factura;
    const proveedor = context.proveedor;
    
    // 1. Validar si la factura es mayor a 10 millones y el proveedor es Persona Natural
    if (factura.Valor > 10000000 && proveedor.TipoOrganizacionFK === 'PersonaNaturalID') {
        context.error("Factura excede el límite. Requiere aprobación manual de gerencia antes de causar.");
        return; // Detiene la causación
    }

    // 2. Modificar la cuenta de gasto para servicios de asesoría
    context.itemsCausar.forEach(item => {
        if (item.Description.toLowerCase().includes('asesoria') && item.CuentaFK === 'CuentaGastoGenericaID') {
            item.CuentaFK = 'CuentaAsesoriaEspecialID';
            context.logger("Cuenta de gasto modificada para ítem de asesoría.");
        }
    });
}
```