# 6. Registro de Cambios (Changelog)

Este documento registra todas las modificaciones, adiciones, mejoras y correcciones realizadas al sistema Qonta a lo largo de su desarrollo. Sigue el formato [Keep a Changelog](https://keepachangelog.com/es/1.0.0/) para claridad.

## [1.2.0] - 2025-11-15 - Estabilización de Módulos y Integración CRM

### Añadido
-   Integración inicial del Módulo CRM (tablas `EmpresasCrm`, `OportunidadesCrm`, `GestionesCrm`).
-   Nuevo endpoint GraphQL para la gestión de **Hooks de Causación** (`Mutation: actualizarHook(movimientoId, codigoJs)`).
-   Se añadió la tabla `Terceros` para gestionar beneficiarios distintos a Proveedores y Clientes.
-   Implementación del evento de **Aceptación Tácita** en la integración con Kiai.

### Cambiado
-   Refactorización completa del Microservicio **Bot DIAN** a Python para mejorar la robustez del Web Scraping.
-   Actualización de la tabla `FacturasRecibidasDian` para incluir los campos `ValorAPagar`, `ValorPagado` y `RetencionAsumida`.
-   Modificación de la lógica de aplicación de ReteICA en `Causaciones` para depender de la tabla `ImpuestosICAxClientes` en lugar de una lógica hardcodeada.

### Corregido
-   Error en la clasificación de ítems: Palabras clave que contenían números ahora se procesan correctamente.
-   Fallo en la sincronización de proveedores que estaban marcados como Autoretenedores al momento de causar.
-   Se corrigió la validación de `NumeroDocumentoRepresentante` para permitir nulos si el tipo de organización es persona natural.

## [1.1.0] - 2025-08-20 - Motor de Causación Mejorado

### Añadido
-   Implementación del módulo de **Centros de Costos por Factura** (`CentrosCostosxfacturas`).
-   Se añadió la lógica para el manejo de **Causaciones Originales** (`CausacionesOriginales`) para auditoría.
-   Funcionalidad de **Cierre de Meses** (`CierresMeses`) para bloquear la edición de causaciones en periodos contables cerrados.

### Cambiado
-   Mejora del rendimiento de las consultas GraphQL para la pantalla de Bandeja de Facturas.
-   Actualización del esquema de la tabla `CufesXML` para manejar mejor los descuentos y cargos (`ItemCufeXMLAllowanceCharges`).

### Corregido
-   Se resolvió un *bug* donde la ReteFuente asumida se calculaba incorrectamente en facturas de no declarantes.
-   Corrección en el flujo de asignación de facturas a usuarios (`DetallesFacturasRecibidasDianxAsignaciones`).