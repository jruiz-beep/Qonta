# 2.2 Detalle de Tablas

Este documento proporciona una descripción detallada de cada tabla en la base de datos, incluyendo su propósito y los campos más relevantes. Las tablas se han organizado por el módulo o aplicación a la que pertenecen lógicamente.

## Módulo Qonta (Core de Facturación y Causación)

Estas tablas forman el corazón del sistema Qonta, gestionando la información de facturas electrónicas, su causación contable y los datos maestros relacionados con clientes y proveedores.

### `ActividadesEconomicas`
* **Propósito:** Almacena el listado de actividades económicas de acuerdo a la clasificación CIIU.
* **Campos:**
	- `Nombre` varchar(1000)
	- `Codigo` varchar(10)
	- `Descripcion` varchar(max)
	- `UsuarioCreacion` uniqueidentifier
	- `UsuarioModificacion` uniqueidentifier
	- `FechaCreacion` datetime2
	- `FechaModificacion` datetime2
	- `Id` uniqueidentifier

### `AplicacionesxUsuarios`
* **Propósito:** Asocia a los usuarios con las diferentes aplicaciones o módulos a los que tienen acceso.
* **Campos:**
	- `UsuarioFK` uniqueidentifier
	- `ModuloFK` uniqueidentifier
	- `Descripcion` varchar(500)
	- `UsuarioCreacion` uniqueidentifier
	- `UsuarioModificacion` uniqueidentifier
	- `FechaCreacion` datetime2
	- `FechaModificacion` datetime2
	- `Id` uniqueidentifier

### `Causaciones`
* **Propósito:** Contiene los registros detallados de las causaciones contables generadas para cada ítem de una factura.
* **Campos:**
	- `FacturaFK` uniqueidentifier
	- `CufeXMLFK` uniqueidentifier
	- `ItemCufeXMLFK` uniqueidentifier
	- `ClasificacionFK` uniqueidentifier
	- `CuentaFK` uniqueidentifier
	- `Debito` decimal(10,2)
	- `Credito` decimal(10,2)
	- `UsuarioCreacion` uniqueidentifier
	- `UsuarioModificacion` uniqueidentifier
	- `FechaCreacion` datetime2
	- `FechaModificacion` datetime2
	- `ConceptoFK` uniqueidentifier
	- `Observaciones` varchar(500)
	- `Posicion` decimal(19,3)
	- `ProveedorFK` uniqueidentifier
	- `Auxiliar` varchar(max)
	- `ReteFuente` bit
	- `ReteIva` bit
	- `ReteICA` bit
	- `TerceroFK` uniqueidentifier
	- `Comprobante` varchar(100)
	- `CentroCostosFK` uniqueidentifier
	- `BaseRF` decimal(19,3)
	- `Fecha` datetime2
	- `CuentaPorPagar` bit
	- `Gasto` bit
	- `RetencionAsumida` bit
	- `ImpMayorValor` bit
	- `ImpuestoBit` bit
	- `ImpuestoFK` uniqueidentifier
	- `NoDeducible` bit
	- `Id` uniqueidentifier

### `CausacionesOriginales`
* **Propósito:** Almacena una copia de las causaciones originales generadas antes de cualquier modificación, para propósitos de auditoría y trazabilidad.
* **Campos:**
	- `FacturaFK` uniqueidentifier
	- `CufeXMLFK` uniqueidentifier
	- `ClasificacionFK` uniqueidentifier
	- `CuentaFK` uniqueidentifier
	- `Debito` decimal(19,3)
	- `Credito` decimal(19,3)
	- `ConceptoFK` uniqueidentifier
	- `Observaciones` varchar(500)
	- `Posicion` decimal(19,3)
	- `ProveedorFK` uniqueidentifier
	- `Impuesto` bit
	- `UsuarioCreacion` uniqueidentifier
	- `FechaCreacion` datetime2
	- `Modificada` bit
	- `ReteFuente` bit
	- `Comprobante` varchar(100)
	- `CentroCostosFK` uniqueidentifier
	- `BaseRF` decimal(19,3)
	- `Fecha` datetime2
	- `CuentaPorPagar` bit
	- `Gasto` bit
	- `RetencionAsumida` bit
	- `Id` uniqueidentifier

### `CentrosCostos`
* **Propósito:** Gestiona los centros de costos definidos por cada cliente.
* **Campos:**
	- `Nombre` varchar(100)
	- `Codigo` varchar(100)
	- `ClienteFK` uniqueidentifier
	- `Descripcion` varchar(500)
	- `UsuarioCreacion` uniqueidentifier
	- `UsuarioModificacion` uniqueidentifier
	- `FechaCreacion` datetime2
	- `FechaModificacion` datetime2
	- `Id` uniqueidentifier

### `CentrosCostosxfacturas`
* **Propósito:** Relaciona los centros de costos con las facturas, permitiendo la distribución de valores.
* **Campos:**
	- `CentroCostosFK` uniqueidentifier
	- `CuentaFK` uniqueidentifier
	- `FacturaFK` uniqueidentifier
	- `Porcentaje` decimal(19,3)
	- `Valor` decimal(19,3)
	- `Descripcion` varchar(500)
	- `UsuarioCreacion` uniqueidentifier
	- `UsuarioModificacion` uniqueidentifier
	- `FechaCreacion` datetime2
	- `FechaModificacion` datetime2
	- `ClasificacionCCFK` uniqueidentifier
	- `Id` uniqueidentifier

### `CierresMeses`
* **Propósito:** Registra los cierres de mes contables para cada cliente, indicando si un período está cerrado para causaciones.
* **Campos:**
	- `Mes` varchar(100)
	- `Anio` varchar(10)
	- `Estado` varchar(100)
	- `UsuarioCreacion` uniqueidentifier
	- `UsuarioModificacion` uniqueidentifier
	- `FechaCreacion` datetime2
	- `FechaModificacion` datetime2
	- `ClienteFK` uniqueidentifier
	- `Id` uniqueidentifier

### `CodigosPostales`
* **Propósito:** Contiene los códigos postales asociados a municipios y su descripción.
* **Campos:**
	- `MunicipioFK` uniqueidentifier
	- `Tipo` varchar(100)
	- `PostalCodigo` varchar(100)
	- `UsuarioCreacion` uniqueidentifier
	- `UsuarioModificacion` uniqueidentifier
	- `FechaCreacion` datetime2
	- `FechaModificacion` datetime2
	- `Descripcion` varchar(max)
	- `Id` uniqueidentifier

### `ConceptosRetencion`
* **Propósito:** Catálogo de conceptos de retención y sus tarifas aplicables.
* **Campos:**
	- `Nombre` varchar(100)
	- `TarifaDeclarante` decimal(10,2)
	- `TarifaNoDeclarante` decimal(10,2)
	- `Base` decimal(10,2)
	- `Descripcion` varchar(500)
	- `UsuarioCreacion` uniqueidentifier
	- `UsuarioModificacion` uniqueidentifier
	- `FechaCreacion` datetime2
	- `FechaModificacion` datetime2
	- `Operacion` varchar(100)
	- `ReteIva` varchar(100)
	- `Tipo` varchar(100)
	- `Id` uniqueidentifier

### `ConceptosRetencionxClientes`
* **Propósito:** Asocia conceptos de retención específicos a clientes y sus cuentas.
* **Campos:**
	- `ConceptoFK` uniqueidentifier
	- `ClienteFK` uniqueidentifier
	- `CuentaFK` uniqueidentifier
	- `Descripcion` varchar(500)
	- `UsuarioCreacion` uniqueidentifier
	- `UsuarioModificacion` uniqueidentifier
	- `FechaCreacion` datetime2
	- `FechaModificacion` datetime2
	- `CuentaNoDeclaranteFK` uniqueidentifier
	- `CuentaReteIvaFK` uniqueidentifier
	- `Id` uniqueidentifier

### `CufesXML`
* **Propósito:** Guarda los metadatos y el CUFE de los documentos electrónicos recibidos/enviados.
* **Campos:**
	- `Valorpropina` decimal(19,3)
	- `NumeroFactura` varchar(100)
	- `Emisor` varchar(100)
	- `TipoFactura` varchar(100)
	- `FechaEmision` datetime2
	- `FechaVencimiento` datetime2
	- `CUFE` varchar(1000)
	- `FormaPago` varchar(10)
	- `ValorTotal` decimal(19,3)
	- `Auxiliar` varchar(max)
	- `Notas` varchar(max)
	- `Id` uniqueidentifier

### `Departamentos`
* **Propósito:** Lista de departamentos/provincias del país usados para direccionamiento.
* **Campos:**
	- `Nombre` varchar(500)
	- `Codigo` varchar(10)
	- `Descripcion` varchar(max)
	- `UsuarioCreacion` uniqueidentifier
	- `UsuarioModificacion` uniqueidentifier
	- `FechaCreacion` datetime2
	- `FechaModificacion` datetime2
	- `PaisFK` uniqueidentifier
	- `Id` uniqueidentifier

### `DetallesClasificacionesCentrosCostos`
* **Propósito:** Detalle de la clasificación de centros de costos y sus cuentas asociadas.
* **Campos:**
	- `ClasificacionCCFK` uniqueidentifier
	- `CentroCostosFK` uniqueidentifier
	- `Porcentaje` decimal(19,3)
	- `CuentaFK` uniqueidentifier
	- `Descripcion` varchar(500)
	- `UsuarioCreacion` uniqueidentifier
	- `UsuarioModificacion` uniqueidentifier
	- `FechaCreacion` datetime2
	- `FechaModificacion` datetime2
	- `Id` uniqueidentifier

### `DetallesClasificacionesConceptos`
* **Propósito:** Descripciones y metadatos para las clasificaciones de conceptos contables.
* **Campos:**
	- `ClasificacionFK` uniqueidentifier
	- `Descripcion` varchar(500)
	- `UsuarioCreacion` uniqueidentifier
	- `UsuarioModificacion` uniqueidentifier
	- `FechaCreacion` datetime2
	- `FechaModificacion` datetime2
	- `Nombre` varchar(500)
	- `TipoConcepto` varchar(100)
	- `PalabrasClaves` varchar(1000)
	- `Id` uniqueidentifier

### `DetallesFacturasRecibidasDianxAsignaciones`
* **Propósito:** Registra asignaciones y trazabilidad de facturas DIAN a usuarios para gestión.
* **Campos:**
	- `FacturaFK` uniqueidentifier
	- `UsuarioCreacion` uniqueidentifier
	- `UsuarioModificacion` uniqueidentifier
	- `FechaCreacion` datetime2
	- `FechaModificacion` datetime2
	- `UsuarioFK` uniqueidentifier
	- `Id` uniqueidentifier

### `DetallesImpuestos`
* **Propósito:** Configuración detallada de impuestos y cuentas contables relacionadas.
* **Campos:**
	- `ImpuestoFK` uniqueidentifier
	- `Nombre` varchar(500)
	- `CodigoContable` varchar(100)
	- `Descripcion` varchar(500)
	- `UsuarioCreacion` uniqueidentifier
	- `UsuarioModificacion` uniqueidentifier
	- `FechaCreacion` datetime2
	- `FechaModificacion` datetime2
	- `ClienteFK` uniqueidentifier
	- `CuentaFK` uniqueidentifier
	- `MayorValor` bit
	- `DiscriminaSyB` bit
	- `CuentaServicioFK` uniqueidentifier
	- `CuentaBienFK` uniqueidentifier
	- `Id` uniqueidentifier

### `DetallesProveedores`
* **Propósito:** Información adicional y reglas específicas aplicables a proveedores.
* **Campos:**
	- `UsuarioFK` uniqueidentifier
	- `RangoMaximo` decimal(10,2)
	- `ProveedorFK` uniqueidentifier
	- `UsuarioCreacion` uniqueidentifier
	- `UsuarioModificacion` uniqueidentifier
	- `FechaCreacion` datetime2
	- `FechaModificacion` datetime2
	- `ClienteFK` uniqueidentifier
	- `RangoMinimo` decimal(10,2)
	- `Id` uniqueidentifier

### `FacturasRecibidasDian`
* **Propósito:** Representa facturas recibidas desde la DIAN con su estado, importes y metadatos.
* **Campos:**
	- `Cufe` varchar(300)
	- `NombreEmisor` varchar(1000)
	- `NitEmisor` varchar(20)
	- `FechaVencimiento` datetime2
	- `Estado` varchar(100)
	- `Valor` decimal(19,3)
	- `FechaEmision` datetime2
	- `Iva` decimal(19,3)
	- `FechaModificacion` datetime2
	- `UsuarioModificacion` uniqueidentifier
	- `FechaCreacion` datetime2
	- `ClienteFK` uniqueidentifier
	- `PdfBase` varchar(max)
	- `UsuarioAsignadoFK` uniqueidentifier
	- `Numero` varchar(100)
	- `Causada` bit
	- `Prefijo` varchar(100)
	- `Folio` varchar(100)
	- `CufeXMLFK` uniqueidentifier
	- `MotivoRechazoFK` uniqueidentifier
	- `FechaRechazo` datetime2
	- `UsuarioRechazoFK` uniqueidentifier
	- `Ica` decimal(10,2)
	- `Ic` decimal(10,2)
	- `Inc` decimal(10,2)
	- `Timbre` decimal(10,2)
	- `IncBolsas` decimal(10,2)
	- `InCarbono` decimal(10,2)
	- `InCombustibles` decimal(10,2)
	- `IcDatos` decimal(10,2)
	- `Icl` decimal(10,2)
	- `Inpp` decimal(10,2)
	- `Ibua` decimal(10,2)
	- `Icui` decimal(10,2)
	- `ReteIva` decimal(10,2)
	- `ReteRenta` decimal(10,2)
	- `ReteIca` decimal(10,2)
	- `UsuarioAprobacionFK` uniqueidentifier
	- `FechaAprobacion` datetime2
	- `ProveedorFK` uniqueidentifier
	- `CuentaPorPagarFK` uniqueidentifier
	- `CentroCostos` bit
	- `Pagada` bit
	- `ValorPagado` decimal(19,3)
	- `RetencionAsumida` bit
	- `ValorAPagar` decimal(19,3)
	- `FechaVencimientoAprobacion` datetime2
	- `Auxiliar` varchar(max)
	- `FormaPago` varchar(100)
	- `MayorGasto` bit
	- `NoDeducible` bit
	- `Id` uniqueidentifier

### `Impuestos`
* **Propósito:** Catálogo general de impuestos usados en los cálculos y causaciones.
* **Campos:**
	- `Nombre` varchar(500)
	- `Codigo` varchar(100)
	- `Descripcion` varchar(500)
	- `UsuarioCreacion` uniqueidentifier
	- `UsuarioModificacion` uniqueidentifier
	- `FechaCreacion` datetime2
	- `FechaModificacion` datetime2
	- `CodigoContable` varchar(100)
	- `Porcentaje` varchar(100)
	- `Id` uniqueidentifier

### `ImpuestosICA`
* **Propósito:** Configuración de impuestos ICA por municipio y sus porcentajes.
* **Campos:**
	- `Nombre` varchar(250)
	- `Base` decimal(19,3)
	- `UsuarioCreacion` uniqueidentifier
	- `UsuarioModificacion` uniqueidentifier
	- `FechaCreacion` datetime2
	- `FechaModificacion` datetime2
	- `Descripcion` varchar(500)
	- `Porcentaje` decimal(19,3)
	- `MunicipioFK` uniqueidentifier
	- `Id` uniqueidentifier

### `ImpuestosICAxClientes`
* **Propósito:** Asocia impuestos ICA específicos a clientes y cuentas contables.
* **Campos:**
	- `ICAFK` uniqueidentifier
	- `ClienteFK` uniqueidentifier
	- `CuentaFK` uniqueidentifier
	- `UsuarioCreacion` uniqueidentifier
	- `UsuarioModificacion` uniqueidentifier
	- `FechaCreacion` datetime2
	- `FechaModificacion` datetime2
	- `Id` uniqueidentifier

### `ItemCufeXMLAllowanceCharges`
* **Propósito:** Detalle de cargos y descuentos aplicados a ítems dentro del CUFE XML.
* **Campos:**
	- `ItemId` varchar(100)
	- `AllowanceChargeReason` varchar(5000)
	- `AmountCurrency` varchar(10)
	- `BaseAmountCurrency` varchar(10)
	- `ItemCufeXMLFK` uniqueidentifier
	- `ChargeIndicator` bit
	- `MultiplierFactorNumeric` decimal(19,3)
	- `Amount` decimal(19,3)
	- `BaseAmount` decimal(19,3)
	- `Id` uniqueidentifier

### `ItemCufeXMLTaxSubtotals`
* **Propósito:** Subtotales de impuestos por ítem dentro del CUFE XML.
* **Campos:**
	- `ItemCufeXMLTaxFK` uniqueidentifier
	- `TaxableAmountCurrency` varchar(10)
	- `TaxAmountCurrency` varchar(10)
	- `TaxSchemeID` varchar(10)
	- `TaxSchemeName` varchar(30)
	- `TaxableAmount` decimal(19,3)
	- `TaxAmount` decimal(19,3)
	- `NumPercent` decimal(19,3)
	- `Id` uniqueidentifier

### `ItemCufeXMLTaxes`
* **Propósito:** Impuestos específicos asociados a ítems del CUFE XML.
* **Campos:**
	- `ItemCufeXMLFK` uniqueidentifier
	- `TaxAmountCurrency` varchar(10)
	- `RoundingAmountCurrency` varchar(10)
	- `TaxAmount` decimal(14,2)
	- `RoundingAmount` decimal(14,2)
	- `Id` uniqueidentifier

### `ItemsCufeXML`
* **Propósito:** Ítems (líneas) de facturas almacenadas dentro del CUFE XML con sus metadatos.
* **Campos:**
	- `CufeXMLFK` uniqueidentifier
	- `ItemId` varchar(100)
	- `InvoiceQuantityUnitCode` varchar(10)
	- `LineExtensionAmountCurrency` varchar(10)
	- `FreeOfChargeIndicator` bit
	- `Description` varchar(1000)
	- `PriceAmountCurrency` varchar(10)
	- `BaseQuantityUnitCode` varchar(100)
	- `ClasificacionFK` uniqueidentifier
	- `CuentaK` uniqueidentifier
	- `ConceptoFK` uniqueidentifier
	- `CuentaNueva` bit
	- `InvoicedQuantity` decimal(19,3)
	- `LineExtensionAmount` decimal(19,3)
	- `PriceAmount` decimal(19,3)
	- `BaseQuantity` decimal(19,3)
	- `TipoConcepto` varchar(100)
	- `PalabrasClaves` varchar(1000)
	- `Id` uniqueidentifier

### `Modulos`
* **Propósito:** Catálogo de módulos/aplicaciones disponibles en el sistema.
* **Campos:**
	- `Nombre` varchar(100)
	- `Logo` varchar(max)
	- `Descripcion` varchar(500)
	- `IdMenu` varchar(100)
	- `Id` uniqueidentifier

### `MotivosRechazoFacturas`
* **Propósito:** Lista de motivos de rechazo para facturas recibidas de la DIAN.
* **Campos:**
	- `Codigo` varchar(100)
	- `Nombre` varchar(100)
	- `Descripcion` varchar(500)
	- `UsuarioCreacion` uniqueidentifier
	- `UsuarioModificacion` uniqueidentifier
	- `FechaCreacion` datetime2
	- `FechaModificacion` datetime2
	- `Id` uniqueidentifier

### `MovimientosSistemas`
* **Propósito:** Registro de tipos de movimientos o eventos del sistema para auditoría.
* **Campos:**
	- `Nombre` varchar(250)
	- `Campos` varchar(max)
	- `Descripcion` varchar(500)
	- `UsuarioCreacion` uniqueidentifier
	- `UsuarioModificacion` uniqueidentifier
	- `FechaCreacion` datetime2
	- `FechaModificacion` datetime2
	- `Id` uniqueidentifier

### `Municipios`
* **Propósito:** Catálogo de municipios, enlazados a departamentos y usados en direcciones.
* **Campos:**
	- `Nombre` varchar(500)
	- `Codigo` varchar(10)
	- `Descripcion` varchar(max)
	- `UsuarioCreacion` uniqueidentifier
	- `UsuarioModificacion` uniqueidentifier
	- `FechaCreacion` datetime2
	- `FechaModificacion` datetime2
	- `DepartamentoFK` uniqueidentifier
	- `Id` uniqueidentifier

### `Paises`
* **Propósito:** Catálogo de países para datos geográficos y direccionamiento.
* **Campos:**
	- `Nombre` varchar(100)
	- `Codigo` varchar(100)
	- `Descripcion` varchar(max)
	- `UsuarioCreacion` uniqueidentifier
	- `UsuarioModificacion` uniqueidentifier
	- `FechaCreacion` datetime2
	- `FechaModificacion` datetime2
	- `Id` uniqueidentifier

### `PlandeCuentas`
* **Propósito:** Plan de cuentas contables asociado a clientes y su estructura.
* **Campos:**
	- `Nombre` varchar(100)
	- `NumCuenta` varchar(100)
	- `UsuarioCreacion` uniqueidentifier
	- `UsuarioModificacion` uniqueidentifier
	- `FechaCreacion` datetime2
	- `FechaModificacion` datetime2
	- `Descripcion` varchar(500)
	- `ClienteFK` uniqueidentifier
	- `CuentaOperador` varchar(100)
	- `CuentaAuxiliar` bit
	- `Id` uniqueidentifier

### `Proveedores`
* **Propósito:** Registro maestro de proveedores con datos legales y contables.
* **Campos:**
	- `TipoOrganizacionFK` uniqueidentifier
	- `TipoDocumentoFK` uniqueidentifier
	- `NumeroDocumento` varchar(100)
	- `RazonSocial` varchar(250)
	- `ActividadEconomicaFK` uniqueidentifier
	- `Direccion` varchar(100)
	- `CodigoPostalFK` uniqueidentifier
	- `TelefonoPrincipal` varchar(100)
	- `TelefonoAlterntivo` varchar(100)
	- `Correo` varchar(100)
	- `TipoDocumentoRepresentanteFK` uniqueidentifier
	- `NumeroDocumentoRepresentante` varchar(100)
	- `NombreCompletoRepresentante` varchar(250)
	- `PepRepresentante` bit
	- `CorreoRepresentante` varchar(100)
	- `TelefonoRepresentante` varchar(100)
	- `ClienteFK` uniqueidentifier
	- `FechaCreacion` datetime2
	- `FechaModificacion` datetime2
	- `UsuarioCreacion` uniqueidentifier
	- `UsuarioModificacion` uniqueidentifier
	- `Estado` varchar(100)
	- `AplicaRango` bit
	- `RFMayorValorFacturado` bit
	- `RFMayorTarifa` bit
	- `RFConBase` bit
	- `RFSinBase` bit
	- `CausarTotales` bit
	- `Pensionado` bit
	- `InteresesVivienda` bit
	- `FechaVigenciaIntereses` datetime2
	- `ValorIntereses` decimal(10,2)
	- `Dependientes` bit
	- `ValorDependientes` decimal(10,2)
	- `FechaVigenciaPolizaSalud` datetime2
	- `ValorPolizaSalud` decimal(10,2)
	- `PolizaSalud` bit
	- `CuentaPorPagarFK` uniqueidentifier
	- `ConceptoFK` uniqueidentifier
	- `CuentaFK` uniqueidentifier
	- `ReteIvaConBase` bit
	- `ReteIvaSinBase` bit
	- `RST` bit
	- `ICAFK` uniqueidentifier
	- `MunicipioFK` uniqueidentifier
	- `ClasificacionCCFK` uniqueidentifier
	- `ImpuestoMayorValor` bit
	- `Autoretenedor` bit
	- `Id` uniqueidentifier

### `ResponsabilidadesFiscales`
* **Propósito:** Catálogo de responsabilidades fiscales aplicables a terceros.
* **Campos:**
	- `Nombre` varchar(1000)
	- `Descripcion` varchar(max)
	- `UsuarioCreacion` uniqueidentifier
	- `UsuarioModificacion` uniqueidentifier
	- `FechaCreacion` datetime2
	- `FechaModificacion` datetime2
	- `Codigo` varchar(10)
	- `PalabrasClave` varchar(500)
	- `Id` uniqueidentifier

### `ResponsabilidadesFiscalesClientes`
* **Propósito:** Asocia responsabilidades fiscales específicas a clientes.
* **Campos:**
	- `ClienteFK` uniqueidentifier
	- `ResposabilidadFiscalFK` uniqueidentifier
	- `Descripcion` varchar(max)
	- `UsuarioCreacion` uniqueidentifier
	- `UsuarioModificacion` uniqueidentifier
	- `FechaCreacion` datetime2
	- `FechaModificacion` datetime2
	- `Id` uniqueidentifier

### `ResponsabilidadesFiscalesProveedores`
* **Propósito:** Asocia responsabilidades fiscales específicas a proveedores.
* **Campos:**
	- `ProveedorFK` uniqueidentifier
	- `ResposabilidadFiscalFK` uniqueidentifier
	- `Descripcion` varchar(500)
	- `UsuarioCreacionFK` uniqueidentifier
	- `UsuarioModificacionFK` uniqueidentifier
	- `FechaCreacion` datetime2
	- `FechaModificacion` datetime2
	- `Id` uniqueidentifier

### `ResponsabilidadesReteIva`
* **Propósito:** Configuración de responsabilidades relacionadas con retenciones de IVA.
* **Campos:**
	- `ResponsabilidadClienteFK` uniqueidentifier
	- `ResponsabilidadProveedorFK` uniqueidentifier
	- `Descripcion` varchar(500)
	- `UsuarioCreacion` uniqueidentifier
	- `UsuarioModificacion` uniqueidentifier
	- `FechaCreacion` datetime2
	- `FechaModificacion` datetime2
	- `Id` uniqueidentifier

### `ResponsabilidadesRetencion`
* **Propósito:** Configura responsabilidades relacionadas con retenciones en general.
* **Campos:**
	- `ResponsabilidadClienteFK` uniqueidentifier
	- `ResponsabilidadProveedorFK` uniqueidentifier
	- `Descripcion` varchar(500)
	- `UsuarioCreacion` uniqueidentifier
	- `UsuarioModificacion` uniqueidentifier
	- `FechaCreacion` datetime2
	- `FechaModificacion` datetime2
	- `Id` uniqueidentifier

### `Roles`
* **Propósito:** Define roles de usuario dentro del sistema y su módulo asociado.
* **Campos:**
	- `Nombre` nvarchar(200)
	- `ModuloFK` uniqueidentifier
	- `Id` uniqueidentifier

### `RolesxUsuarios`
* **Propósito:** Asociación de roles a usuarios (mapeo many-to-many).
* **Campos:**
	- `UsuarioId` uniqueidentifier
	- `RoleId` uniqueidentifier
	- `Id` uniqueidentifier

### `Terceros`
* **Propósito:** Entidades externas (terceros) relacionadas a clientes para diversas operaciones.
* **Campos:**
	- `ClienteFK` uniqueidentifier
	- `Nombre` varchar(500)
	- `TipoDocumentoFK` uniqueidentifier
	- `NumeroDocumento` varchar(100)
	- `Celular` varchar(100)
	- `Email` varchar(300)
	- `UsuarioCreacion` uniqueidentifier
	- `UsuarioModificacion` uniqueidentifier
	- `FechaCreacion` datetime2
	- `FechaModificacion` datetime2
	- `Id` uniqueidentifier

### `TiposDocumentos`
* **Propósito:** Catálogo de tipos de documentos identificativos usados por terceros.
* **Campos:**
	- `Nombre` varchar(200)
	- `Descripcion` varchar(max)
	- `UsuarioCreacion` uniqueidentifier
	- `UsuarioModificacion` uniqueidentifier
	- `FechaCreacion` datetime2
	- `FechaModificacion` datetime2
	- `Codigo` varchar(100)
	- `TipoOrganizacionFK` uniqueidentifier
	- `Id` uniqueidentifier

### `TiposOrganizaciones`
* **Propósito:** Clasificación de tipos de organizaciones (ej. empresa, persona natural).
* **Campos:**
	- `Nombre` varchar(200)
	- `Codigo` varchar(10)
	- `Descripcion` varchar(max)
	- `UsuarioCreacion` uniqueidentifier
	- `UsuarioModificacion` uniqueidentifier
	- `FechaCreacion` datetime2
	- `FechaModificacion` datetime2
	- `Tipo` varchar(100)
	- `Id` uniqueidentifier

### `TrazabilidadesCausaciones`
* **Propósito:** Registra la trazabilidad y cambios sobre causaciones contables.
* **Campos:**
	- `CausacionFK` uniqueidentifier
	- `UsuarioFK` uniqueidentifier
	- `Fecha` datetime2
	- `Cambios` varchar(max)
	- `Id` uniqueidentifier

### `Usuarios`
* **Propósito:** Tabla principal de usuarios del sistema con credenciales y metadatos.
* **Campos:**
	- `Nombre` varchar(200)
	- `Apellidos` varchar(200)
	- `Identificacion` varchar(200)
	- `Clave` varchar(200)
	- `Correo` varchar(300)
	- `Foto` varchar(max)
	- `ClienteFK` uniqueidentifier
	- `CierreMes` varchar(100)
	- `Id` uniqueidentifier

### `UsuariosxClientes`
* **Propósito:** Relaciona usuarios con clientes específicos y su contexto operativo.
* **Campos:**
	- `ClienteFK` uniqueidentifier
	- `UsuarioFK` uniqueidentifier
	- `UsuarioCreacion` uniqueidentifier
	- `UsuarioModificacion` uniqueidentifier
	- `FechaCreacion` datetime2
	- `FechaModificacion` datetime2
	- `Descripcion` varchar(500)
	- `Id` uniqueidentifier

### `Variables`
* **Propósito:** Valores y parámetros configurables por año o cliente utilizados en cálculos.
* **Campos:**
	- `Nombre` varchar(100)
	- `Anio` varchar(100)
	- `UsuarioCreacion` uniqueidentifier
	- `UsuarioModificacion` uniqueidentifier
	- `FechaCreacion` datetime2
	- `FechaModificacion` datetime2
	- `Descripcion` varchar(500)
	- `Valor` decimal(10,2)
	- `Id` uniqueidentifier

