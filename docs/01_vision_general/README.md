# 1. Visión General del Sistema Qonta

Esta sección proporciona una introducción de alto nivel al sistema Qonta, sus objetivos y el contexto de las aplicaciones que lo componen.

## Propósito General

Qonta es la solución principal para la **automatización de la causación y acuse de facturas electrónicas en Colombia**. Su objetivo es garantizar la eficiencia operativa y el cumplimiento legal de la normativa DIAN/Radián, minimizando la intervención manual del personal contable.

## Aplicaciones en el Ecosistema

Qonta opera dentro de un ecosistema que comparte una base de datos central. Las tablas se dividen por módulo lógico:

| Módulo/Aplicación | Prefijo en Tablas | Enfoque Principal |
| :--- | :--- | :--- |
| **Qonta Core** | *Ninguno* (ej. `Causaciones`, `Clientes`) | Causación contable, procesamiento de XML, gestión de impuestos. |
| **CRM** | `*Crm` (ej. `EmpresasCrm`, `OportunidadesCrm`) | Gestión del ciclo de ventas y relación con clientes. |
| **Infraestructura** | (Ninguno o genérico) | Usuarios, Roles, Países, Municipios, Variables. |

## Puntos de Interés

* **Acuse:** Se realiza mediante la API de Kiai.
* **Causación:** Se automatiza mediante reglas complejas (Hooks) que discriminan ReteFuente, ReteIVA y ReteICA.
* **Integración:** La descarga se realiza con un bot, y la inserción de datos se hace con un microservicio de Node.js/Prisma.