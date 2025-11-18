# 5.1 Integración con la DIAN (Bot de Web Scraping)

El sistema Qonta necesita acceder a las facturas electrónicas de sus clientes que se encuentran almacenadas en el portal de la Dirección de Impuestos y Aduanas Nacionales (DIAN) de Colombia. Debido a que la DIAN no ofrece una API pública y estable para la consulta y descarga masiva, esta funcionalidad se implementa mediante un Bot de Web Scraping.

## 5.1.1 Propósito del Bot

El objetivo principal del Microservicio Bot DIAN es **automatizar la descarga periódica y masiva** de archivos XML y la representación gráfica (PDF) de todas las facturas electrónicas recibidas por los clientes de Qonta.

## 5.1.2 Arquitectura y Componentes

El Bot es un microservicio independiente, separado del servidor principal de GraphQL de Qonta, para garantizar la estabilidad y evitar sobrecargar el servidor de aplicaciones principal con tareas de larga duración.

| Componente | Descripción |
| :--- | :--- |
| **Motor de Automatización** | Utiliza librerías de automatización de navegador (ej. Puppeteer o Selenium) para simular las acciones de un usuario (login, navegación, filtros, descarga). |
| **Almacén de Credenciales** | Almacenamiento seguro de las credenciales de la DIAN del cliente (usuario y contraseña/firma). |
| **API de Qonta** | El Bot consume un *endpoint* específico del API Gateway de Qonta para inyectar los archivos descargados (`XML` y `PDF Base`) en la tabla `FacturasRecibidasDian` y `CufesXML` para su posterior procesamiento. |

## 5.1.3 Flujo de Operación

1.  **Programación:** El sistema Qonta programa la ejecución del Bot a intervalos regulares (ej. cada 4 horas) para cada cliente activo.
2.  **Inicio de Sesión:** El Bot inicia una instancia de navegador virtual (headless) y utiliza las credenciales almacenadas para autenticarse en el portal de la DIAN.
3.  **Navegación y Filtros:** Navega a la sección de "Facturas Recibidas" y aplica filtros de fecha (desde la última descarga exitosa hasta la fecha actual) y estado.
4.  **Descarga Masiva:** Itera sobre el listado de facturas, descargando el archivo XML y, si está disponible, el PDF asociado a la factura.
5.  **Inyección en Qonta:** Por cada factura descargada, el Bot invoca la API de Qonta, enviando el contenido del XML para la extracción del CUFE y la información clave, y el PDF para su almacenamiento.
6.  **Actualización de Estado:** Qonta actualiza el registro de la última descarga exitosa para ese cliente, asegurando que no se reprocesen facturas en el siguiente ciclo.
7.  **Manejo de Errores:** El Bot incluye lógica para manejar CAPTCHAs, errores de conexión, cambios menores en la interfaz de la DIAN y fallos de autenticación, notificando a los administradores de Qonta sobre cualquier problema persistente.

## 5.1.4 Consideraciones de Seguridad y Riesgos

* **Riesgo de Cambios de Interfaz:** La DIAN puede cambiar su interfaz sin previo aviso, lo que requiere un mantenimiento constante y una adaptación rápida del código del Bot.
* **Gestión de Credenciales:** El almacenamiento de credenciales debe ser altamente seguro, utilizando cifrado robusto y siguiendo las mejores prácticas de seguridad.
* **Limitación de Velocidad:** El Bot debe operar con una cadencia adecuada para evitar ser detectado y bloqueado por la DIAN por exceso de peticiones.