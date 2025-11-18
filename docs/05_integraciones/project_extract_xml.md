# 5.3 Extracción y Procesamiento de XML (Project Extract XML)

Complementando la funcionalidad del Bot DIAN, existe un proyecto local (un microservicio) basado en Node.js que reside en la misma máquina virtual donde opera el Bot. Este proyecto es el encargado de procesar los archivos XML de las facturas electrónicas descargadas, extrayendo su información estructurada y preparándola para la inserción en la base de datos de Qonta.

## 5.3.1 Propósito del Project Extract XML

El objetivo principal de este proyecto es:

1.  **Parsear:** Leer y analizar los archivos XML de las facturas electrónicas (UBL 2.1).
2.  **Transformar:** Convertir la información estructurada del XML en un formato JSON manejable.
3.  **Normalizar:** Distribuir los datos del JSON en las estructuras de las tablas de Qonta.
4.  **Insertar:** Utilizar un ORM (Prisma) para insertar los datos extraídos en las tablas correspondientes de la base de datos de Qonta.
5.  **Extender:** Disparar lógica de negocio adicional post-inserción, como la creación o actualización de proveedores.

## 5.3.2 Arquitectura y Flujo de Operación

El "Project Extract XML" actúa como un intermediario vital entre el Bot DIAN y la base de datos de Qonta.

```mermaid
graph TD
    A[Bot DIAN] --> B{Descarga XML/PDF};
    B --> C[Almacena XML/PDF Localmente];
    C --> D[Project Extract XML (Node.js)];
    D -- Lee XML localmente --> E[Parseo y Transformación a JSON];
    E -- Normalización de Datos --> F[Preparación para Inserción (Prisma)];
    F -- Inserta en DB Qonta --> G[CufesXML];
    F -- Inserta en DB Qonta --> H[ItemsCufeXML];
    F -- Inserta en DB Qonta --> I[ItemCufeXMLTaxes];
    F -- Inserta en DB Qonta --> J[ItemCufeXMLTaxSubtotals];
    F -- Inserta en DB Qonta --> K[ItemCufeXMLAllowanceCharges];
    K -- Después de Inserción --> L[Llama API de Qonta para correr Hook];
    L --> M[Hook 'CrearProveedor'];
    M --> N[Crea/Actualiza registro en Proveedores];
```

1.  **Descarga del Bot:** El Bot DIAN descarga el archivo XML y el PDF de una factura electrónica y los almacena temporalmente en un directorio local en la máquina virtual.
2.  **Detección y Procesamiento:** El Project Extract XML monitoriza este directorio. Cuando detecta un nuevo archivo XML:
      * Lo lee y lo parsea utilizando librerías XML en Node.js.
      * Extrae todos los campos relevantes de la estructura UBL 2.1.
      * Construye objetos JSON que corresponden directamente a los modelos de Prisma/Sequelize.
3.  **Inserción en la Base de Datos:**
      * Utiliza **Prisma** (u otro ORM como Sequelize) para interactuar con la base de datos.
      * Inserta los datos extraídos en las siguientes tablas:
          * `CufesXML`: Información general de la factura.
          * `ItemsCufeXML`: Detalles de cada ítem (producto/servicio) de la factura.
          * `ItemCufeXMLTaxes`: Impuestos generales por ítem.
          * `ItemCufeXMLTaxSubtotals`: Subtotales detallados de los impuestos por ítem.
          * `ItemCufeXMLAllowanceCharges`: Descuentos y cargos aplicados a los ítems.
4.  **Activación de Hooks:** Una vez que la información de la factura ha sido insertada exitosamente en la base de datos, el Project Extract XML realiza una llamada a una API específica del servidor GraphQL de Qonta.
      * Esta llamada tiene como objetivo disparar el hook `CrearProveedor`.
      * El hook `CrearProveedor` se encarga de verificar si el emisor de la factura ya existe en la tabla `Proveedores`. Si no existe, lo crea; si existe, puede actualizar su información.

## 5.3.3 Tecnologías Utilizadas

  * **Node.js:** Entorno de ejecución para el microservicio.
  * **Librerías XML para Node.js:** Para el parsing y manipulación de XML.
  * **Prisma (ORM):** Para la interacción con la base de datos SQL Server, simplificando las operaciones CRUD.
  * **API GraphQL de Qonta:** Para invocar el hook `CrearProveedor` y otras lógicas de negocio.