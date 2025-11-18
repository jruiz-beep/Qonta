# 2.2 Detalle de Tablas

Este documento proporciona una descripción detallada de cada tabla en la base de datos, incluyendo su propósito y los campos más relevantes. Las tablas se han organizado por el módulo o aplicación a la que pertenecen lógicamente.

## Módulo Qonta (Core de Facturación y Causación)

Estas tablas forman el corazón del sistema Qonta, gestionando la información de facturas electrónicas, su causación contable y los datos maestros relacionados con clientes y proveedores.

### `ActividadesEconomicas`
* **Propósito:** Almacena el listado de actividades económicas de acuerdo a la clasificación CIIU.
* **Campos Clave:** `Id` (PK), `Nombre`, `Codigo`.

### `Causaciones`
* **Propósito:** Contiene los registros detallados de las causaciones contables generadas para cada ítem de una factura.
* **Campos Clave:** `Id` (PK), `FacturaFK` (Factura relacionada), `CufeXMLFK` (XML CUFE relacionado), `ItemCufeXMLFK` (Ítem del XML CUFE), `ClasificacionFK` (Clasificación del concepto), `CuentaFK` (Cuenta contable), `Debito`, `Credito`, `ConceptoFK` (Concepto de retención/impuesto), `ProveedorFK`, `Observaciones`.

### `CausacionesOriginales`
* **Propósito:** Almacena una copia de las causaciones originales generadas antes de cualquier modificación, para propósitos de auditoría y trazabilidad.
* **Campos Clave:** `Id` (PK), `FacturaFK`, `CufeXMLFK`, `ClasificacionFK`, `CuentaFK`, `Debito`, `Credito`, `Modificada` (Indica si fue modificada).

### `CentrosCostos`
* **Propósito:** Gestiona los centros de costos definidos por cada cliente.
* **Campos Clave:** `Id` (PK), `Nombre`, `Codigo`, `ClienteFK`.

### `CentrosCostosxfacturas`
* **Propósito:** Relaciona los centros de costos con las facturas, permitiendo la distribución de valores.
* **Campos Clave:** `Id` (PK), `CentroCostosFK`, `CuentaFK`, `FacturaFK`, `Porcentaje`, `Valor`.

### `CierresMeses`
* **Propósito:** Registra los cierres de mes contables para cada cliente, indicando si un período está cerrado para causaciones.
* **Campos Clave:** `Id` (PK), `Mes`, `Anio`, `Estado`, `ClienteFK`.

### `ClasificacionesCentrosCostos`
* **Propósito:** Permite agrupar o clasificar centros de costos para una gestión más organizada.
* **Campos Clave:** `Id` (PK), `Nombre`, `Descripcion`, `ClienteFK`.

### `ClasificacionesConceptos`
* **Propósito:** Define clasificaciones para los conceptos de gasto o ingreso, asociándolos a proveedores y cuentas contables específicas.
* **Campos Clave:** `Id` (PK), `ProveedorFK`, `ConceptoFK`, `ClienteFK`, `CuentaFK`, `AplicaCentroCosto`.

### `Clientes`
* **Propósito:** Almacena la información de los clientes (empresas) que utilizan el sistema Qonta.
* **Campos Clave:** `Id` (PK), `NumeroDocumento`, `RazonSocial`, `ActividadEconomicaFK`, `Correo`, `Estado`, `Acuse`, `Autoretenedor`.

### `CodigosPostales`
* **Propósito:** Catálogo de códigos postales.
* **Campos Clave:** `Id` (PK), `PostalCodigo`, `MunicipioFK`.

### `ConceptosRetencion`
* **Propósito:** Define los diferentes conceptos de retención con sus tarifas y bases aplicables.
* **Campos Clave:** `Id` (PK), `Nombre`, `TarifaDeclarante`, `TarifaNoDeclarante`, `Base`, `Tipo` (ReteFuente, ReteIVA, ReteICA).

### `ConceptosRetencionxClientes`
* **Propósito:** Asocia conceptos de retención específicos a cada cliente, permitiendo la personalización de las cuentas contables asociadas.
* **Campos Clave:** `Id` (PK), `ConceptoFK`, `ClienteFK`, `CuentaFK`.

### `CufesXML`
* **Propósito:** Almacena los datos principales extraídos de los archivos XML de facturas electrónicas, identificados por su CUFE.
* **Campos Clave:** `Id` (PK), `CUFE`, `NumeroFactura`, `Emisor`, `FechaEmision`, `ValorTotal`.

### `Departamentos`
* **Propósito:** Catálogo de departamentos de Colombia.
* **Campos Clave:** `Id` (PK), `Nombre`, `Codigo`, `PaisFK`.

### `DetallesClasificacionesCentrosCostos`
* **Propósito:** Define los detalles y porcentajes de distribución para una clasificación de centro de costos.
* **Campos Clave:** `Id` (PK), `ClasificacionCCFK`, `CentroCostosFK`, `Porcentaje`, `CuentaFK`.

### `DetallesClasificacionesConceptos`
* **Propósito:** Proporciona los detalles de una clasificación de concepto, incluyendo palabras clave para el reconocimiento.
* **Campos Clave:** `Id` (PK), `ClasificacionFK`, `Nombre`, `TipoConcepto`, `PalabrasClaves`.

### `DetallesImpuestos`
* **Propósito:** Configuración detallada de los impuestos para cada cliente, incluyendo cuentas contables asociadas.
* **Campos Clave:** `Id` (PK), `ImpuestoFK`, `ClienteFK`, `CuentaFK`, `MayorValor`, `DiscriminaSyB`.

### `DetallesProveedores`
* **Propósito:** Define rangos y configuraciones específicas de usuarios para la gestión de proveedores.
* **Campos Clave:** `Id` (PK), `UsuarioFK`, `ProveedorFK`, `ClienteFK`, `RangoMinimo`, `RangoMaximo`.

### `FacturasRecibidasDian`
* **Propósito:** Registra las facturas electrónicas recibidas directamente de la DIAN o a través del proveedor tecnológico.
* **Campos Clave:** `Id` (PK), `Cufe`, `NombreEmisor`, `NitEmisor`, `Numero`, `FechaEmision`, `Valor`, `Estado`, `Causada`, `ClienteFK`, `ProveedorFK`.

### `ImportsAuxiliar`
* **Propósito:** Tabla auxiliar para la importación temporal de datos.
* **Campos Clave:** `Id` (PK), `Json` (Almacena el JSON a importar).

### `Impuestos`
* **Propósito:** Catálogo de impuestos generales (IVA, ICA, etc.).
* **Campos Clave:** `Id` (PK), `Nombre`, `Codigo`, `Porcentaje`.

### `ImpuestosICA`
* **Propósito:** Define las tarifas y bases de Impuesto de Industria y Comercio (ICA) por municipio.
* **Campos Clave:** `Id` (PK), `Nombre`, `Porcentaje`, `Base`, `MunicipioFK`.

### `ImpuestosICAxClientes`
* **Propósito:** Asocia los impuestos ICA a los clientes, con la cuenta contable correspondiente.
* **Campos Clave:** `Id` (PK), `ICAFK`, `ClienteFK`, `CuentaFK`.

### `ItemCufeXMLAllowanceCharges`
* **Propósito:** Detalles de descuentos o cargos aplicados a ítems específicos dentro del XML CUFE.
* **Campos Clave:** `Id` (PK), `ItemCufeXMLFK`, `AllowanceChargeReason`, `Amount`, `ChargeIndicator`.

### `ItemCufeXMLTaxSubtotals`
* **Propósito:** Subtotales de impuestos para cada ítem de una factura, como se detalla en el XML CUFE.
* **Campos Clave:** `Id` (PK), `ItemCufeXMLTaxFK`, `TaxSchemeName`, `TaxableAmount`, `TaxAmount`, `NumPercent`.

### `ItemCufeXMLTaxes`
* **Propósito:** Resumen de los impuestos aplicados a un ítem del XML CUFE.
* **Campos Clave:** `Id` (PK), `ItemCufeXMLFK`, `TaxAmount`, `RoundingAmount`.

### `ItemsCufeXML`
* **Propósito:** Almacena la información de cada ítem (producto/servicio) de una factura electrónica extraída del XML CUFE.
* **Campos Clave:** `Id` (PK), `CufeXMLFK`, `ItemId`, `Description`, `InvoicedQuantity`, `LineExtensionAmount`, `ClasificacionFK`, `ConceptoFK`.

### `MotivosRechazoFacturas`
* **Propósito:** Catálogo de motivos predefinidos para el rechazo de facturas.
* **Campos Clave:** `Id` (PK), `Codigo`, `Nombre`.

### `MovimientosSistemas`
* **Propósito:** Registra los tipos de movimientos contables o de sistema que pueden ser configurados por el cliente.
* **Campos Clave:** `Id` (PK), `Nombre`, `Campos` (JSON para configuración de campos).

### `Municipios`
* **Propósito:** Catálogo de municipios de Colombia.
* **Campos Clave:** `Id` (PK), `Nombre`, `Codigo`, `DepartamentoFK`.

### `Paises`
* **Propósito:** Catálogo de países.
* **Campos Clave:** `Id` (PK), `Nombre`, `Codigo`.

### `PlandeCuentas`
* **Propósito:** Almacena el plan de cuentas contable de cada cliente.
* **Campos Clave:** `Id` (PK), `Nombre`, `NumCuenta`, `ClienteFK`, `CuentaAuxiliar`.

### `Proveedores`
* **Propósito:** Gestiona la información de los proveedores de cada cliente, incluyendo datos fiscales y configuraciones de retención.
* **Campos Clave:** `Id` (PK), `NumeroDocumento`, `RazonSocial`, `ClienteFK`, `Estado`, `AplicaRango`, `Autoretenedor`, `CuentaPorPagarFK`, `ConceptoFK`, `ICAFK`.

### `ResponsabilidadesFiscales`
* **Propósito:** Catálogo de responsabilidades fiscales de la DIAN.
* **Campos Clave:** `Id` (PK), `Codigo`, `Nombre`.

### `ResponsabilidadesFiscalesClientes`
* **Propósito:** Asocia las responsabilidades fiscales a cada cliente.
* **Campos Clave:** `Id` (PK), `ClienteFK`, `ResposabilidadFiscalFK`.

### `ResponsabilidadesFiscalesProveedores`
* **Propósito:** Asocia las responsabilidades fiscales a cada proveedor.
* **Campos Clave:** `Id` (PK), `ProveedorFK`, `ResposabilidadFiscalFK`.

### `ResponsabilidadesReteIva`
* **Propósito:** Define las reglas de aplicación de retención de IVA basadas en las responsabilidades fiscales del cliente y proveedor.
* **Campos Clave:** `Id` (PK), `ResponsabilidadClienteFK`, `ResponsabilidadProveedorFK`.

### `ResponsabilidadesRetencion`
* **Propósito:** Define las reglas de aplicación de retención en la fuente basadas en las responsabilidades fiscales del cliente y proveedor.
* **Campos Clave:** `Id` (PK), `ResponsabilidadClienteFK`, `ResponsabilidadProveedorFK`.

### `Terceros`
* **Propósito:** Registro de terceros involucrados en transacciones, distintos de clientes y proveedores.
* **Campos Clave:** `Id` (PK), `ClienteFK`, `Nombre`, `NumeroDocumento`, `Email`.

### `TiposDocumentos`
* **Propósito:** Catálogo de tipos de documentos de identificación (CC, NIT, CE, etc.).
* **Campos Clave:** `Id` (PK), `Codigo`, `Nombre`.

### `TiposOrganizaciones`
* **Propósito:** Catálogo de tipos de organización (Persona Natural, Persona Jurídica, etc.).
* **Campos Clave:** `Id` (PK), `Codigo`, `Nombre`.

### `TrazabilidadesCausaciones`
* **Propósito:** Registra el historial de cambios y aprobaciones de cada causación.
* **Campos Clave:** `Id` (PK), `CausacionFK`, `UsuarioFK`, `Fecha`, `Cambios` (Descripción del cambio).

### `Usuarios`
* **Propósito:** Almacena la información de los usuarios que acceden al sistema.
* **Campos Clave:** `Id` (PK), `Nombre`, `Apellidos`, `Correo`, `Identificacion`, `ClienteFK`.

### `UsuariosxClientes`
* **Propósito:** Relaciona a los usuarios con los clientes a los que tienen acceso.
* **Campos Clave:** `Id` (PK), `ClienteFK`, `UsuarioFK`.

### `Variables`
* **Propósito:** Permite almacenar variables de configuración del sistema que pueden variar anualmente o por configuración.
* **Campos Clave:** `Id` (PK), `Nombre`, `Anio`, `Valor`.

## Módulo CRM (Gestión de Relación con el Cliente)

Estas tablas están dedicadas a la gestión de oportunidades, contactos, empresas y el seguimiento comercial.

### `ContactosEmpresasCrm`
* **Propósito:** Almacena la información de contactos dentro de empresas que son clientes o prospectos del CRM.
* **Campos Clave:** `Id` (PK), `Nombre`, `Correo`, `EmpresaCrmFk`.

### `EmpresasCrm`
* **Propósito:** Registra la información de las empresas con las que se interactúa a través del CRM.
* **Campos Clave:** `Id` (PK), `Nombre`, `Identificacion`, `SectorFK`.

### `EtapasCrm`
* **Propósito:** Define las etapas del embudo de ventas o proceso comercial dentro del CRM.
* **Campos Clave:** `Id` (PK), `Nombre`, `Orden`.

### `FallidosCrm`
* **Propósito:** Registra los motivos de oportunidades fallidas o no concretadas en el CRM.
* **Campos Clave:** `Id` (PK), `Nombre`, `TipoFallo`.

### `GestionesCrm`
* **Propósito:** Detalla las interacciones y seguimientos realizados con los contactos o empresas del CRM.
* **Campos Clave:** `Id` (PK), `Nombre`, `TipoGestionFk`, `FechaGestion`, `OportunidadCrmFK`.

### `OportunidadesCrm`
* **Propósito:** Representa una oportunidad de negocio o venta en el ciclo comercial.
* **Campos Clave:** `Id` (PK), `Nombre`, `ContactoEmpresaCrmFk`, `EtapasFk`, `Prioridad`, `EmpresaFK`.

### `OrigenesContactosCrm`
* **Propósito:** Registra los orígenes de los contactos del CRM (ej. referencia, web, evento).
* **Campos Clave:** `Id` (PK), `Nombre`, `OrigenFK`.

### `OrigenesCrm`
* **Propósito:** Define las fuentes o canales de donde provienen los leads y oportunidades.
* **Campos Clave:** `Id` (PK), `Nombre`, `Parner` (Indicador si es un partner).

### `ProductosCrm`
* **Propósito:** Catálogo de productos o servicios que ofrece la empresa en el contexto del CRM.
* **Campos Clave:** `Id` (PK), `Nombre`, `Descripcion`.

### `SectoresCrm`
* **Propósito:** Catálogo de sectores económicos a los que pertenecen las empresas del CRM.
* **Campos Clave:** `Id` (PK), `Nombre`.

### `TiposGestionCrm`
* **Propósito:** Define los tipos de gestiones o actividades que se pueden registrar en el CRM (llamada, reunión, correo).
* **Campos Clave:** `Id` (PK), `Nombre`.

### `TiposOportunidadesCrm`
* **Propósito:** Define los tipos de oportunidades de negocio (ej. nueva venta, upsell).
* **Campos Clave:** `Id` (PK), `Nombre`.

## Módulo General / Seguridad / Infraestructura (Comunes a todas las aplicaciones)

Estas tablas son de uso transversal y soportan la operación de múltiples aplicaciones, incluyendo Qonta y CRM.

### `AplicacionesxUsuarios`
* **Propósito:** Asocia a los usuarios con las diferentes aplicaciones o módulos a los que tienen acceso.
* **Campos Clave:** `Id` (PK), `UsuarioFK`, `ModuloFK`.

### `CodenullNotifications`
* **Propósito:** Almacena notificaciones internas del sistema para los usuarios.
* **Campos Clave:** `Id` (PK), `Title`, `Message`, `UserTo`, `Read`.

### `DetallesFacturasRecibidasDianxAsignaciones`
* **Propósito:** Gestiona la asignación de facturas DIAN a usuarios específicos para su gestión.
* **Campos Clave:** `Id` (PK), `FacturaFK`, `UsuarioFK`.

### `Modulos`
* **Propósito:** Catálogo de los diferentes módulos o aplicaciones que componen el sistema (Qonta, CRM, etc.).
* **Campos Clave:** `Id` (PK), `Nombre`, `IdMenu` (ID para la interfaz de usuario).

### `Roles`
* **Propósito:** Define los roles de usuario dentro del sistema, asociados a módulos específicos.
* **Campos Clave:** `Id` (PK), `Nombre`, `ModuloFK`.

### `RolesxUsuarios`
* **Propósito:** Asocia roles específicos a los usuarios.
* **Campos Clave:** `Id` (PK), `UsuarioId`, `RoleId`.

### `Tickets`
* **Propósito:** Sistema de tickets o incidencias.
* **Campos Clave:** `Id` (PK), `Numero`, `Aplicacion`, `Asunto`, `Estado`, `UsuarioAsignado`.

### `UsuariosClientes`
* **Propósito:** Relaciona los usuarios principales de un cliente con sus contrapartes en otros sistemas (Ubora, Contabler, etc.), si aplica.
* **Campos Clave:** `Id` (PK), `ClienteFK`, `UsuarioModuloFK`, `UsuarioContablerFK`, `UsuarioUboraFK`.

### `UsuariosContabler`
* **Propósito:** Tabla de usuarios específicos para el módulo de Contabilidad.
* **Campos Clave:** `Id` (PK), `ClienteFK`.

### `UsuariosModulos`
* **Propósito:** Tabla genérica para usuarios de módulos, probablemente para extensiones futuras.
* **Campos Clave:** `Id` (PK).

### `UsuariosUbora`
* **Propósito:** Tabla de usuarios específicos para el módulo Ubora.
* **Campos Clave:** `Id` (PK).