# 2.1 Modelo Entidad-Relación (MER)

Este documento describe el modelo de datos de Qonta, presentando las tablas principales del sistema y sus relaciones. El diseño de la base de datos es fundamental para el manejo eficiente y la integridad de la información de las facturas electrónicas y los procesos de causación.

## Diagramas por Módulo Funcional

Para facilitar la comprensión, el modelo de datos se ha dividido en los siguientes diagramas funcionales.

### 1. Módulo de Facturación y Causación

Este diagrama muestra las entidades centrales del proceso de recepción, aprobación y contabilización de facturas.

``` mermaid
    classDiagram
    direction LR
    class FacturasRecibidasDian {
        varchar(300) Cufe
        varchar(100) Estado
        decimal(19,3) Valor
        datetime2 FechaEmision
        datetime2 FechaVencimiento
        uniqueidentifier ClienteFK
        uniqueidentifier ProveedorFK
        uniqueidentifier UsuarioAsignadoFK
        bit Causada
        bit Pagada
        uniqueidentifier Id
    }
    class CufesXML {
        varchar(100) NumeroFactura
        datetime2 FechaEmision
        varchar(1000) CUFE
        decimal(19,3) ValorTotal
        uniqueidentifier Id
    }
    class ItemsCufeXML {
        uniqueidentifier CufeXMLFK
        varchar(1000) Description
        decimal(19,3) LineExtensionAmount
        uniqueidentifier ClasificacionFK
        uniqueidentifier CuentaK
        uniqueidentifier Id
    }
    class Causaciones {
        uniqueidentifier FacturaFK
        uniqueidentifier ItemCufeXMLFK
        uniqueidentifier CuentaFK
        decimal(10,2) Debito
        decimal(10,2) Credito
        uniqueidentifier CentroCostosFK
        datetime2 Fecha
        uniqueidentifier Id
    }
    class TrazabilidadesCausaciones {
        uniqueidentifier CausacionFK
        uniqueidentifier UsuarioFK
        datetime2 Fecha
        varchar(max) Cambios
        uniqueidentifier Id
    }
    class CentrosCostos {
        varchar(100) Nombre
        varchar(100) Codigo
        uniqueidentifier ClienteFK
        uniqueidentifier Id
    }

    FacturasRecibidasDian --> CufesXML : CufeXMLFKID
    FacturasRecibidasDian --> Proveedores : ProveedorFKID
    FacturasRecibidasDian --> Clientes : ClienteFKID
    ItemsCufeXML --> CufesXML : CufeXMLFKID
    Causaciones --> FacturasRecibidasDian : FacturaFKID
    Causaciones --> ItemsCufeXML : ItemCufeXMLFKID
    Causaciones --> PlandeCuentas : CuentaFKID
    Causaciones --> CentrosCostos : CentroCostosFKID
    TrazabilidadesCausaciones --> Causaciones : CausacionFKID
    TrazabilidadesCausaciones --> Usuarios : UsuarioFKID
```

### 2. Módulo de Terceros (Clientes, Proveedores y Usuarios)

Este modelo representa las entidades principales que interactúan con el sistema, como clientes, proveedores y usuarios internos.

``` mermaid
    classDiagram
    direction RL
    class Clientes {
        varchar(100) NumeroDocumento
        varchar(1000) RazonSocial
        uniqueidentifier ActividadEconomicaFK
        uniqueidentifier Id
    }
    class Proveedores {
        varchar(100) NumeroDocumento
        varchar(250) RazonSocial
        uniqueidentifier ClienteFK
        uniqueidentifier ActividadEconomicaFK
        uniqueidentifier Id
    }
    class Usuarios {
        varchar(200) Nombre
        varchar(200) Apellidos
        varchar(300) Correo
        uniqueidentifier ClienteFK
        uniqueidentifier Id
    }
    class Terceros {
        uniqueidentifier ClienteFK
        varchar(500) Nombre
        varchar(100) NumeroDocumento
        uniqueidentifier Id
    }
    class ActividadesEconomicas {
        varchar(10) Codigo
        varchar(1000) Nombre
        uniqueidentifier Id
    }
    class TiposDocumentos {
        varchar(100) Codigo
        varchar(200) Nombre
        uniqueidentifier Id
    }

    Proveedores --> Clientes : ClienteFKID
    Usuarios --> Clientes : ClienteFKID
    Terceros --> Clientes : ClienteFKID
    Clientes --> ActividadesEconomicas : ActividadEconomicaFKID
    Proveedores --> ActividadesEconomicas : ActividadEconomicaFKID
    Clientes --> TiposDocumentos : TipoDocumentoFKID
    Proveedores --> TiposDocumentos : TipoDocumentoFKID
    Terceros --> TiposDocumentos : TipoDocumentoFKID
```

### 3. Módulo de Configuración Contable y Fiscal

Aquí se definen las tablas para la configuración de planes de cuentas, impuestos, retenciones y sus clasificaciones.

``` mermaid
    classDiagram
    class PlandeCuentas {
        varchar(100) NumCuenta
        varchar(100) Nombre
        uniqueidentifier ClienteFK
        uniqueidentifier Id
    }
    class Impuestos {
        varchar(100) Codigo
        varchar(500) Nombre
        varchar(100) Porcentaje
        uniqueidentifier Id
    }
    class ConceptosRetencion {
        varchar(100) Nombre
        decimal(10,2) TarifaDeclarante
        decimal(10,2) Base
        uniqueidentifier Id
    }
    class ClasificacionesConceptos {
        uniqueidentifier ProveedorFK
        uniqueidentifier ConceptoFK
        uniqueidentifier CuentaFK
        uniqueidentifier ClienteFK
        uniqueidentifier Id
    }
    class ImpuestosICA {
        varchar(250) Nombre
        decimal(19,3) Porcentaje
        uniqueidentifier MunicipioFK
        uniqueidentifier Id
    }
    class ResponsabilidadesFiscales {
        varchar(10) Codigo
        varchar(1000) Nombre
        uniqueidentifier Id
    }

    PlandeCuentas --> Clientes : ClienteFKID
    ClasificacionesConceptos --> Clientes : ClienteFKID
    ClasificacionesConceptos --> Proveedores : ProveedorFKID
    ClasificacionesConceptos --> ConceptosRetencion : ConceptoFKID
    ClasificacionesConceptos --> PlandeCuentas : CuentaFKID
    ImpuestosICAxClientes --> ImpuestosICA : ICAFKID
    ImpuestosICAxClientes --> Clientes : ClienteFKID
    ResponsabilidadesFiscalesClientes --> ResponsabilidadesFiscales : ResposabilidadFiscalFKID
    ResponsabilidadesFiscalesProveedores --> ResponsabilidadesFiscales : ResposabilidadFiscalFKID
```

### 4. Módulo CRM

Este diagrama agrupa todas las entidades relacionadas con la Gestión de Relaciones con Clientes (CRM).

``` mermaid
    classDiagram
    class OportunidadesCrm {
        varchar(100) Nombre
        uniqueidentifier EmpresaFK
        uniqueidentifier EtapasFk
        uniqueidentifier UsuarioFk
        uniqueidentifier Id
    }
    class EmpresasCrm {
        varchar(100) Nombre
        varchar(100) Identificacion
        uniqueidentifier SectorFK
        uniqueidentifier Id
    }
    class ContactosEmpresasCrm {
        varchar(100) Nombre
        varchar(100) Correo
        uniqueidentifier EmpresaCrmFk
        uniqueidentifier Id
    }
    class GestionesCrm {
        varchar(100) Nombre
        datetime2 FechaGestion
        uniqueidentifier OportunidadCrmFK
        uniqueidentifier Id
    }
    class EtapasCrm {
        varchar(100) Nombre
        int Orden
        uniqueidentifier Id
    }
    class SectoresCrm {
        varchar(100) Nombre
        uniqueidentifier Id
    }

    OportunidadesCrm --> EmpresasCrm : EmpresaFKID
    OportunidadesCrm --> EtapasCrm : EtapasFkID
    OportunidadesCrm --> Usuarios : UsuarioFkID
    OportunidadesCrm --> ContactosEmpresasCrm : ContactoEmpresaCrmFkID
    ContactosEmpresasCrm --> EmpresasCrm : EmpresaCrmFkID
    GestionesCrm --> OportunidadesCrm : OportunidadCrmFKID
    EmpresasCrm --> SectoresCrm : SectorFKID
```

### 5. Módulo de Datos Maestros y Geográficos

Contiene las tablas de soporte con información que no cambia frecuentemente, como la estructura geográfica.

``` mermaid
    classDiagram
    class Paises {
        varchar(100) Nombre
        varchar(100) Codigo
        uniqueidentifier Id
    }
    class Departamentos {
        varchar(500) Nombre
        varchar(10) Codigo
        uniqueidentifier PaisFK
        uniqueidentifier Id
    }
    class Municipios {
        varchar(500) Nombre
        varchar(10) Codigo
        uniqueidentifier DepartamentoFK
        uniqueidentifier Id
    }
    class CodigosPostales {
        varchar(100) PostalCodigo
        uniqueidentifier MunicipioFK
        uniqueidentifier Id
    }
    
    Departamentos --> Paises : PaisFKID
    Municipios --> Departamentos : DepartamentoFKID
    CodigosPostales --> Municipios : MunicipioFKID
```

### 6. Módulo de Seguridad y Acceso

Define cómo los usuarios se relacionan con roles, módulos y aplicaciones para gestionar permisos.

``` mermaid
    classDiagram
    class Usuarios {
        varchar(200) Nombre
        varchar(300) Correo
        uniqueidentifier Id
    }
    class Roles {
        nvarchar(200) Nombre
        uniqueidentifier ModuloFK
        uniqueidentifier Id
    }
    class Modulos {
        varchar(100) Nombre
        varchar(100) IdMenu
        uniqueidentifier Id
    }
    class RolesxUsuarios {
        uniqueidentifier UsuarioId
        uniqueidentifier RoleId
        uniqueidentifier Id
    }
    class AplicacionesxUsuarios {
        uniqueidentifier UsuarioFK
        uniqueidentifier ModuloFK
        uniqueidentifier Id
    }

    RolesxUsuarios --> Usuarios : UsuarioIdID
    RolesxUsuarios --> Roles : RoleIdID
    Roles --> Modulos : ModuloFKID
    AplicacionesxUsuarios --> Usuarios : UsuarioFKID
    AplicacionesxUsuarios --> Modulos : ModuloFKID
```

<!-- El siguiente bloque contiene la definición de todas las tablas para que Mermaid pueda renderizar los diagramas. Se puede mantener colapsado. -->

<details>
<summary>Definición de Clases (Tablas)</summary>

``` mermaid
    classDiagram
    direction BT
    class ActividadesEconomicas {
        varchar(1000) Nombre
        varchar(10) Codigo
        varchar(max) Descripcion
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        uniqueidentifier Id
    }
    class AplicacionesxUsuarios {
        uniqueidentifier UsuarioFK
        uniqueidentifier ModuloFK
        varchar(500) Descripcion
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        uniqueidentifier Id
    }
    class Causaciones {
        uniqueidentifier FacturaFK
        uniqueidentifier CufeXMLFK
        uniqueidentifier ItemCufeXMLFK
        uniqueidentifier ClasificacionFK
        uniqueidentifier CuentaFK
        decimal(10,2) Debito
        decimal(10,2) Credito
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        uniqueidentifier ConceptoFK
        varchar(500) Observaciones
        decimal(19,3) Posicion
        uniqueidentifier ProveedorFK
        varchar(max) Auxiliar
        bit ReteFuente
        bit ReteIva
        bit ReteICA
        uniqueidentifier TerceroFK
        varchar(100) Comprobante
        uniqueidentifier CentroCostosFK
        decimal(19,3) BaseRF
        datetime2 Fecha
        bit CuentaPorPagar
        bit Gasto
        bit RetencionAsumida
        bit ImpMayorValor
        bit ImpuestoBit
        uniqueidentifier ImpuestoFK
        bit NoDeducible
        uniqueidentifier Id
    }
    class CausacionesOriginales {
        uniqueidentifier FacturaFK
        uniqueidentifier CufeXMLFK
        uniqueidentifier ClasificacionFK
        uniqueidentifier CuentaFK
        decimal(19,3) Debito
        decimal(19,3) Credito
        uniqueidentifier ConceptoFK
        varchar(500) Observaciones
        decimal(19,3) Posicion
        uniqueidentifier ProveedorFK
        bit Impuesto
        uniqueidentifier UsuarioCreacion
        datetime2 FechaCreacion
        bit Modificada
        bit ReteFuente
        varchar(100) Comprobante
        uniqueidentifier CentroCostosFK
        decimal(19,3) BaseRF
        datetime2 Fecha
        bit CuentaPorPagar
        bit Gasto
        bit RetencionAsumida
        uniqueidentifier Id
    }
    class CentrosCostos {
        varchar(100) Nombre
        varchar(100) Codigo
        uniqueidentifier ClienteFK
        varchar(500) Descripcion
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        uniqueidentifier Id
    }
    class CentrosCostosxfacturas {
        uniqueidentifier CentroCostosFK
        uniqueidentifier CuentaFK
        uniqueidentifier FacturaFK
        decimal(19,3) Porcentaje
        decimal(19,3) Valor
        varchar(500) Descripcion
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        uniqueidentifier ClasificacionCCFK
        uniqueidentifier Id
    }
    class CierresMeses {
        varchar(100) Mes
        varchar(10) Anio
        varchar(100) Estado
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        uniqueidentifier ClienteFK
        uniqueidentifier Id
    }
    class ClasificacionesCentrosCostos {
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        varchar(500) Descripcion
        varchar(500) Nombre
        uniqueidentifier ClienteFK
        uniqueidentifier Id
    }
    class ClasificacionesConceptos {
        uniqueidentifier ProveedorFK
        uniqueidentifier ConceptoFK
        uniqueidentifier ClienteFK
        uniqueidentifier CuentaFK
        bit AplicaCentroCosto
        varchar(100) Estado
        varchar(500) Descripcion
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        uniqueidentifier CuentaReteIvaFK
        uniqueidentifier Id
    }
    class Clientes {
        uniqueidentifier TipoOrganizacionFK
        uniqueidentifier TipoDocumentoFK
        varchar(100) NumeroDocumento
        varchar(1000) RazonSocial
        uniqueidentifier ActividadEconomicaFK
        varchar(2000) Direccion
        uniqueidentifier CodigoPostalFK
        varchar(100) TelefonoPrincipal
        varchar(100) TelefonoAlterntivo
        varchar(500) Correo
        uniqueidentifier TipoDocumentoRepresentanteFK
        varchar(100) NumeroDocumentoRepresentante
        varchar(1000) NombreCompletoRepresentante
        bit PepRepresentante
        varchar(200) CorreoRepresentante
        varchar(100) TelefonoRepresentante
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        bit Estado
        bit RFMayorValorFacturado
        bit RFMayorTarifa
        bit RFConBase
        bit RFSinBase
        bit RST
        decimal(10,2) ReteIva
        bit ReteIvaConBase
        bit ReteIvaSinBase
        varchar(100) CuentaReteIvaBienFK
        varchar(100) CuentaReteIvaServicioFK
        varchar(50) CuentaReteFuenteAsumidaFK
        varchar(50) CuentaReteIvaAsumidaFK
        varchar(50) CuentaReteICAAsumidaFK
        varchar(100) ComprobanteCredito
        varchar(100) ComprobanteDebito
        uniqueidentifier MovimientoSistemaFK
        varchar(250) CorreoToken
        bit Acuse
        bit Autoretenedor
        uniqueidentifier Id
    }
    class CodenullNotifications {
        nvarchar(1000) Title
        nvarchar(1000) Message
        nvarchar(max) Action
        nvarchar(2000) UserTo
        datetimeoffset CreatedDate
        datetimeoffset ReadDate
        bit Read
        nvarchar(200) Type
        nvarchar(200) MessageType
        char(36) Id
    }
    class CodigosPostales {
        uniqueidentifier MunicipioFK
        varchar(100) Tipo
        varchar(100) PostalCodigo
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        varchar(max) Descripcion
        uniqueidentifier Id
    }
    class ConceptosRetencion {
        varchar(100) Nombre
        decimal(10,2) TarifaDeclarante
        decimal(10,2) TarifaNoDeclarante
        decimal(10,2) Base
        varchar(500) Descripcion
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        varchar(100) Operacion
        varchar(100) ReteIva
        varchar(100) Tipo
        uniqueidentifier Id
    }
    class ConceptosRetencionxClientes {
        uniqueidentifier ConceptoFK
        uniqueidentifier ClienteFK
        uniqueidentifier CuentaFK
        varchar(500) Descripcion
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        uniqueidentifier CuentaNoDeclaranteFK
        uniqueidentifier CuentaReteIvaFK
        uniqueidentifier Id
    }
    class ContactosEmpresasCrm {
        varchar(100) Nombre
        varchar(100) Cargo
        varchar(100) Correo
        varchar(100) Celular
        uniqueidentifier EmpresaCrmFk
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        uniqueidentifier UsuarioCreacionFK
        uniqueidentifier UsuarioModificacionFK
        uniqueidentifier Id
    }
    class CufesXML {
        varchar(100) NumeroFactura
        varchar(100) Emisor
        varchar(100) TipoFactura
        datetime2 FechaEmision
        datetime2 FechaVencimiento
        varchar(1000) CUFE
        varchar(10) FormaPago
        decimal(19,3) ValorTotal
        varchar(max) Auxiliar
        varchar(max) Notas
        uniqueidentifier Id
    }
    class Departamentos {
        varchar(500) Nombre
        varchar(10) Codigo
        varchar(max) Descripcion
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        uniqueidentifier PaisFK
        uniqueidentifier Id
    }
    class DetallesClasificacionesCentrosCostos {
        uniqueidentifier ClasificacionCCFK
        uniqueidentifier CentroCostosFK
        decimal(19,3) Porcentaje
        uniqueidentifier CuentaFK
        varchar(500) Descripcion
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        uniqueidentifier Id
    }
    class DetallesClasificacionesConceptos {
        uniqueidentifier ClasificacionFK
        varchar(500) Descripcion
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        varchar(500) Nombre
        varchar(100) TipoConcepto
        varchar(1000) PalabrasClaves
        uniqueidentifier Id
    }
    class DetallesFacturasRecibidasDianxAsignaciones {
        uniqueidentifier FacturaFK
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        uniqueidentifier UsuarioFK
        uniqueidentifier Id
    }
    class DetallesImpuestos {
        uniqueidentifier ImpuestoFK
        varchar(500) Nombre
        varchar(100) CodigoContable
        varchar(500) Descripcion
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        uniqueidentifier ClienteFK
        uniqueidentifier CuentaFK
        bit MayorValor
        bit DiscriminaSyB
        uniqueidentifier CuentaServicioFK
        uniqueidentifier CuentaBienFK
        uniqueidentifier Id
    }
    class DetallesProveedores {
        uniqueidentifier UsuarioFK
        decimal(10,2) RangoMaximo
        uniqueidentifier ProveedorFK
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        uniqueidentifier ClienteFK
        decimal(10,2) RangoMinimo
        uniqueidentifier Id
    }
    class EmpresasCrm {
        varchar(100) Nombre
        varchar(100) TipoIdentificacion
        varchar(100) Identificacion
        varchar(100) NombreContacto
        varchar(100) CargoContacto
        varchar(100) CorreoContacto
        varchar(100) CelularContacto
        varchar(500) Descripcion
        uniqueidentifier SectorFK
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        uniqueidentifier UsuarioCreacionFK
        uniqueidentifier UsuarioModificacionFK
        uniqueidentifier Id
    }
    class EtapasCrm {
        varchar(100) Nombre
        varchar(500) Descripcion
        int Orden
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        uniqueidentifier UsuarioCreacionFK
        uniqueidentifier UsuarioModificacionFK
        uniqueidentifier Id
    }
    class FacturasRecibidasDian {
        varchar(300) Cufe
        varchar(1000) NombreEmisor
        varchar(20) NitEmisor
        datetime2 FechaVencimiento
        varchar(100) Estado
        decimal(19,3) Valor
        datetime2 FechaEmision
        decimal(19,3) Iva
        datetime2 FechaModificacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        uniqueidentifier ClienteFK
        varchar(max) PdfBase
        uniqueidentifier UsuarioAsignadoFK
        varchar(100) Numero
        bit Causada
        varchar(100) Prefijo
        varchar(100) Folio
        uniqueidentifier CufeXMLFK
        uniqueidentifier MotivoRechazoFK
        datetime2 FechaRechazo
        uniqueidentifier UsuarioRechazoFK
        decimal(10,2) Ica
        decimal(10,2) Ic
        decimal(10,2) Inc
        decimal(10,2) Timbre
        decimal(10,2) IncBolsas
        decimal(10,2) InCarbono
        decimal(10,2) InCombustibles
        decimal(10,2) IcDatos
        decimal(10,2) Icl
        decimal(10,2) Inpp
        decimal(10,2) Ibua
        decimal(10,2) Icui
        decimal(10,2) ReteIva
        decimal(10,2) ReteRenta
        decimal(10,2) ReteIca
        uniqueidentifier UsuarioAprobacionFK
        datetime2 FechaAprobacion
        uniqueidentifier ProveedorFK
        uniqueidentifier CuentaPorPagarFK
        bit CentroCostos
        bit Pagada
        decimal(19,3) ValorPagado
        bit RetencionAsumida
        decimal(19,3) ValorAPagar
        datetime2 FechaVencimientoAprobacion
        varchar(max) Auxiliar
        varchar(100) FormaPago
        bit MayorGasto
        bit NoDeducible
        uniqueidentifier Id
    }
    class FallidosCrm {
        varchar(100) Nombre
        varchar(100) TipoFallo
        varchar(500) Descripcion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        uniqueidentifier UsuarioCreacionFK
        uniqueidentifier UsuarioModificacionFK
        uniqueidentifier Id
    }
    class GestionesCrm {
        varchar(100) Nombre
        uniqueidentifier TipoGestionFk
        datetime2 FechaGestion
        datetime2 ProximaGestion
        uniqueidentifier CausaFK
        varchar(100) Recordatorio
        varchar(500) DescripcionGestion
        varchar(100) NombreProximaGestion
        uniqueidentifier OrigenFK
        uniqueidentifier OportunidadCrmFK
        uniqueidentifier UsuarioCreacionFK
        uniqueidentifier UsuarioModificacionFK
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        varchar(500) Descripcion
        uniqueidentifier Id
    }
    class ImportsAuxiliar {
        varchar(max) Json
        uniqueidentifier Id
    }
    class Impuestos {
        varchar(500) Nombre
        varchar(100) Codigo
        varchar(500) Descripcion
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        varchar(100) CodigoContable
        varchar(100) Porcentaje
        uniqueidentifier Id
    }
    class ImpuestosICA {
        varchar(250) Nombre
        decimal(19,3) Base
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        varchar(500) Descripcion
        decimal(19,3) Porcentaje
        uniqueidentifier MunicipioFK
        uniqueidentifier Id
    }
    class ImpuestosICAxClientes {
        uniqueidentifier ICAFK
        uniqueidentifier ClienteFK
        uniqueidentifier CuentaFK
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        uniqueidentifier Id
    }
    class ItemCufeXMLAllowanceCharges {
        varchar(100) ItemId
        varchar(5000) AllowanceChargeReason
        varchar(10) AmountCurrency
        varchar(10) BaseAmountCurrency
        uniqueidentifier ItemCufeXMLFK
        bit ChargeIndicator
        decimal(19,3) MultiplierFactorNumeric
        decimal(19,3) Amount
        decimal(19,3) BaseAmount
        uniqueidentifier Id
    }
    class ItemCufeXMLTaxSubtotals {
        uniqueidentifier ItemCufeXMLTaxFK
        varchar(10) TaxableAmountCurrency
        varchar(10) TaxAmountCurrency
        varchar(10) TaxSchemeID
        varchar(30) TaxSchemeName
        decimal(19,3) TaxableAmount
        decimal(19,3) TaxAmount
        decimal(19,3) NumPercent
        uniqueidentifier Id
    }
    class ItemCufeXMLTaxes {
        uniqueidentifier ItemCufeXMLFK
        varchar(10) TaxAmountCurrency
        varchar(10) RoundingAmountCurrency
        decimal(14,2) TaxAmount
        decimal(14,2) RoundingAmount
        uniqueidentifier Id
    }
    class ItemsCufeXML {
        uniqueidentifier CufeXMLFK
        varchar(100) ItemId
        varchar(10) InvoiceQuantityUnitCode
        varchar(10) LineExtensionAmountCurrency
        bit FreeOfChargeIndicator
        varchar(1000) Description
        varchar(10) PriceAmountCurrency
        varchar(100) BaseQuantityUnitCode
        uniqueidentifier ClasificacionFK
        uniqueidentifier CuentaK
        uniqueidentifier ConceptoFK
        bit CuentaNueva
        decimal(19,3) InvoicedQuantity
        decimal(19,3) LineExtensionAmount
        decimal(19,3) PriceAmount
        decimal(19,3) BaseQuantity
        varchar(100) TipoConcepto
        varchar(1000) PalabrasClaves
        uniqueidentifier Id
    }
    class Modulos {
        varchar(100) Nombre
        varchar(max) Logo
        varchar(500) Descripcion
        varchar(100) IdMenu
        uniqueidentifier Id
    }
    class MotivosRechazoFacturas {
        varchar(100) Codigo
        varchar(100) Nombre
        varchar(500) Descripcion
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        uniqueidentifier Id
    }
    class MovimientosSistemas {
        varchar(250) Nombre
        varchar(max) Campos
        varchar(500) Descripcion
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        uniqueidentifier Id
    }
    class Municipios {
        varchar(500) Nombre
        varchar(10) Codigo
        varchar(max) Descripcion
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        uniqueidentifier DepartamentoFK
        uniqueidentifier Id
    }
    class OportunidadesCrm {
        varchar(100) Nombre
        uniqueidentifier ContactoEmpresaCrmFk
        uniqueidentifier ProductosCrmFk
        uniqueidentifier OrigenCrmFk
        uniqueidentifier UsuarioFk
        uniqueidentifier EtapasFk
        uniqueidentifier CausasFk
        uniqueidentifier TipoOportunidadFK
        varchar(100) Prioridad
        bit CambiarEtapa
        varchar(500) Descripcion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        uniqueidentifier UsuarioCreacionFK
        uniqueidentifier UsuarioModificacionFK
        uniqueidentifier EmpresaFK
        uniqueidentifier Id
    }
    class OrigenesContactosCrm {
        varchar(100) Nombre
        varchar(100) Cargo
        varchar(250) Correo
        varchar(100) Celular
        uniqueidentifier OrigenFK
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        uniqueidentifier UsuarioCreacionFK
        uniqueidentifier UsuarioModificacionFK
        varchar(500) Descripcion
        uniqueidentifier Id
    }
    class OrigenesCrm {
        varchar(100) Nombre
        varchar(500) Descripcion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        uniqueidentifier UsuarioCreacionFK
        uniqueidentifier UsuarioModificacionFK
        bit Parner
        uniqueidentifier Id
    }
    class Paises {
        varchar(100) Nombre
        varchar(100) Codigo
        varchar(max) Descripcion
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        uniqueidentifier Id
    }
    class PlandeCuentas {
        varchar(100) Nombre
        varchar(100) NumCuenta
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        varchar(500) Descripcion
        uniqueidentifier ClienteFK
        varchar(100) CuentaOperador
        bit CuentaAuxiliar
        uniqueidentifier Id
    }
    class ProductosCrm {
        varchar(100) Nombre
        varchar(500) Descripcion
        datetime2 FechaModificacion
        uniqueidentifier UsuarioCreacionFK
        uniqueidentifier UsuarioModificacionFK
        datetime2 FechaCreacion
        uniqueidentifier Id
    }
    class Proveedores {
        uniqueidentifier TipoOrganizacionFK
        uniqueidentifier TipoDocumentoFK
        varchar(100) NumeroDocumento
        varchar(250) RazonSocial
        uniqueidentifier ActividadEconomicaFK
        varchar(100) Direccion
        uniqueidentifier CodigoPostalFK
        varchar(100) TelefonoPrincipal
        varchar(100) TelefonoAlterntivo
        varchar(100) Correo
        uniqueidentifier TipoDocumentoRepresentanteFK
        varchar(100) NumeroDocumentoRepresentante
        varchar(250) NombreCompletoRepresentante
        bit PepRepresentante
        varchar(100) CorreoRepresentante
        varchar(100) TelefonoRepresentante
        uniqueidentifier ClienteFK
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        varchar(100) Estado
        bit AplicaRango
        bit RFMayorValorFacturado
        bit RFMayorTarifa
        bit RFConBase
        bit RFSinBase
        bit CausarTotales
        bit Pensionado
        bit InteresesVivienda
        datetime2 FechaVigenciaIntereses
        decimal(10,2) ValorIntereses
        bit Dependientes
        decimal(10,2) ValorDependientes
        datetime2 FechaVigenciaPolizaSalud
        decimal(10,2) ValorPolizaSalud
        bit PolizaSalud
        uniqueidentifier CuentaPorPagarFK
        uniqueidentifier ConceptoFK
        uniqueidentifier CuentaFK
        bit ReteIvaConBase
        bit ReteIvaSinBase
        bit RST
        uniqueidentifier ICAFK
        uniqueidentifier MunicipioFK
        uniqueidentifier ClasificacionCCFK
        bit ImpuestoMayorValor
        bit Autoretenedor
        uniqueidentifier Id
    }
    class ResponsabilidadesFiscales {
        varchar(1000) Nombre
        varchar(max) Descripcion
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        varchar(10) Codigo
        varchar(500) PalabrasClave
        uniqueidentifier Id
    }
    class ResponsabilidadesFiscalesClientes {
        uniqueidentifier ClienteFK
        uniqueidentifier ResposabilidadFiscalFK
        varchar(max) Descripcion
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        uniqueidentifier Id
    }
    class ResponsabilidadesFiscalesProveedores {
        uniqueidentifier ProveedorFK
        uniqueidentifier ResposabilidadFiscalFK
        varchar(500) Descripcion
        uniqueidentifier UsuarioCreacionFK
        uniqueidentifier UsuarioModificacionFK
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        uniqueidentifier Id
    }
    class ResponsabilidadesReteIva {
        uniqueidentifier ResponsabilidadClienteFK
        uniqueidentifier ResponsabilidadProveedorFK
        varchar(500) Descripcion
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        uniqueidentifier Id
    }
    class ResponsabilidadesRetencion {
        uniqueidentifier ResponsabilidadClienteFK
        uniqueidentifier ResponsabilidadProveedorFK
        varchar(500) Descripcion
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        uniqueidentifier Id
    }
    class Roles {
        nvarchar(200) Nombre
        uniqueidentifier ModuloFK
        uniqueidentifier Id
    }
    class RolesxUsuarios {
        uniqueidentifier UsuarioId
        uniqueidentifier RoleId
        uniqueidentifier Id
    }
    class SectoresCrm {
        varchar(100) Nombre
        varchar(500) Descripcion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        uniqueidentifier UsuarioCreacionFK
        uniqueidentifier UsuarioModificacionFK
        uniqueidentifier Id
    }
    class Terceros {
        uniqueidentifier ClienteFK
        varchar(500) Nombre
        uniqueidentifier TipoDocumentoFK
        varchar(100) NumeroDocumento
        varchar(100) Celular
        varchar(300) Email
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        uniqueidentifier Id
    }
    class Tickets {
        varchar(250) Aplicacion
        varchar(250) Email
        varchar(250) Asunto
        varchar(max) Cuerpo
        varchar(100) Estado
        varchar(100) Prioridad
        datetime2 FechaCreacion
        datetime2 FechaFin
        varchar(100) Clasificacion
        varchar(250) UsuarioAsignado
        int Numero
        uniqueidentifier Id
    }
    class TiposDocumentos {
        varchar(200) Nombre
        varchar(max) Descripcion
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        varchar(100) Codigo
        uniqueidentifier TipoOrganizacionFK
        uniqueidentifier Id
    }
    class TiposGestionCrm {
        varchar(100) Nombre
        varchar(500) Descripcion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        uniqueidentifier UsuarioCreacionFK
        uniqueidentifier UsuarioModificacionFK
        uniqueidentifier Id
    }
    class TiposOportunidadesCrm {
        varchar(100) Nombre
        varchar(500) Descripcion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        uniqueidentifier UsuarioCreacionFK
        uniqueidentifier UsuarioModificacionFK
        uniqueidentifier Id
    }
    class TiposOrganizaciones {
        varchar(200) Nombre
        varchar(10) Codigo
        varchar(max) Descripcion
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        varchar(100) Tipo
        uniqueidentifier Id
    }
    class TrazabilidadesCausaciones {
        uniqueidentifier CausacionFK
        uniqueidentifier UsuarioFK
        datetime2 Fecha
        varchar(max) Cambios
        uniqueidentifier Id
    }
    class Usuarios {
        varchar(200) Nombre
        varchar(200) Apellidos
        varchar(200) Identificacion
        varchar(200) Clave
        varchar(300) Correo
        varchar(max) Foto
        uniqueidentifier ClienteFK
        varchar(100) CierreMes
        uniqueidentifier Id
    }
    class UsuariosClientes {
        uniqueidentifier ClienteFK
        uniqueidentifier UsuarioModuloFK
        uniqueidentifier UsuarioContablerFK
        uniqueidentifier UsuarioUboraFK
        uniqueidentifier Id
    }
    class UsuariosContabler {
        uniqueidentifier ClienteFK
        uniqueidentifier Id
    }
    class UsuariosModulos {
        uniqueidentifier Id
    }
    class UsuariosUbora {
        uniqueidentifier Id
    }
    class UsuariosxClientes {
        uniqueidentifier ClienteFK
        uniqueidentifier UsuarioFK
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        varchar(500) Descripcion
        uniqueidentifier Id
    }
    class Variables {
        varchar(100) Nombre
        varchar(100) Anio
        uniqueidentifier UsuarioCreacion
        uniqueidentifier UsuarioModificacion
        datetime2 FechaCreacion
        datetime2 FechaModificacion
        varchar(500) Descripcion
        decimal(10,2) Valor
        uniqueidentifier Id
    }
    
    AplicacionesxUsuarios --> Modulos : ModuloFKID
    AplicacionesxUsuarios --> Usuarios : UsuarioFKID
    Causaciones --> CentrosCostos : CentroCostosFKID
    Causaciones --> ClasificacionesConceptos : ClasificacionFKID
    Causaciones --> ConceptosRetencion : ConceptoFKID
    Causaciones --> CufesXML : CufeXMLFKID
    Causaciones --> FacturasRecibidasDian : FacturaFKID
    Causaciones --> Impuestos : ImpuestoFKID
    Causaciones --> ItemsCufeXML : ItemCufeXMLFKID
    Causaciones --> PlandeCuentas : CuentaFKID
    Causaciones --> Proveedores : ProveedorFKID
    Causaciones --> Terceros : TerceroFKID
    CausacionesOriginales --> CentrosCostos : CentroCostosFKID
    CausacionesOriginales --> ClasificacionesConceptos : ClasificacionFKID
    CausacionesOriginales --> ConceptosRetencion : ConceptoFKID
    CausacionesOriginales --> CufesXML : CufeXMLFKID
    CausacionesOriginales --> FacturasRecibidasDian : FacturaFKID
    CausacionesOriginales --> PlandeCuentas : CuentaFKID
    CausacionesOriginales --> Proveedores : ProveedorFKID
    CentrosCostos --> Clientes : ClienteFKID
    CentrosCostosxfacturas --> CentrosCostos : CentroCostosFKID
    CentrosCostosxfacturas --> ClasificacionesCentrosCostos : ClasificacionCCFKID
    CentrosCostosxfacturas --> FacturasRecibidasDian : FacturaFKID
    CentrosCostosxfacturas --> PlandeCuentas : CuentaFKID
    CierresMeses --> Clientes : ClienteFKID
    ClasificacionesCentrosCostos --> Clientes : ClienteFKID
    ClasificacionesConceptos --> Clientes : ClienteFKID
    ClasificacionesConceptos --> ConceptosRetencion : ConceptoFKID
    ClasificacionesConceptos --> PlandeCuentas : CuentaReteIvaFKID
    ClasificacionesConceptos --> PlandeCuentas : CuentaFKID
    ClasificacionesConceptos --> Proveedores : ProveedorFKID
    Clientes --> ActividadesEconomicas : ActividadEconomicaFKID
    Clientes --> CodigosPostales : CodigoPostalFKID
    Clientes --> MovimientosSistemas : MovimientoSistemaFKID
    Clientes --> TiposDocumentos : TipoDocumentoFKID
    Clientes --> TiposDocumentos : TipoDocumentoRepresentanteFKID
    Clientes --> TiposOrganizaciones : TipoOrganizacionFKID
    CodigosPostales --> Municipios : MunicipioFKID
    ConceptosRetencionxClientes --> Clientes : ClienteFKID
    ConceptosRetencionxClientes --> ConceptosRetencion : ConceptoFKID
    ConceptosRetencionxClientes --> PlandeCuentas : CuentaFKID
    ConceptosRetencionxClientes --> PlandeCuentas : CuentaNoDeclaranteFKID
    ConceptosRetencionxClientes --> PlandeCuentas : CuentaReteIvaFKID
    ContactosEmpresasCrm --> EmpresasCrm : EmpresaCrmFkID
    Departamentos --> Paises : PaisFKID
    DetallesClasificacionesCentrosCostos --> CentrosCostos : CentroCostosFKID
    DetallesClasificacionesCentrosCostos --> ClasificacionesCentrosCostos : ClasificacionCCFKID
    DetallesClasificacionesCentrosCostos --> PlandeCuentas : CuentaFKID
    DetallesClasificacionesConceptos --> ClasificacionesConceptos : ClasificacionFKID
    DetallesFacturasRecibidasDianxAsignaciones --> FacturasRecibidasDian : FacturaFKID
    DetallesFacturasRecibidasDianxAsignaciones --> Usuarios : UsuarioFKID
    DetallesImpuestos --> Clientes : ClienteFKID
    DetallesImpuestos --> Impuestos : ImpuestoFKID
    DetallesImpuestos --> PlandeCuentas : CuentaServicioFKID
    DetallesImpuestos --> PlandeCuentas : CuentaBienFKID
    DetallesImpuestos --> PlandeCuentas : CuentaFKID
    DetallesProveedores --> Clientes : ClienteFKID
    DetallesProveedores --> Proveedores : ProveedorFKID
    DetallesProveedores --> Usuarios : UsuarioFKID
    EmpresasCrm --> SectoresCrm : SectorFKID
    FacturasRecibidasDian --> Clientes : ClienteFKID
    FacturasRecibidasDian --> CufesXML : CufeXMLFKID
    FacturasRecibidasDian --> MotivosRechazoFacturas : MotivoRechazoFKID
    FacturasRecibidasDian --> PlandeCuentas : CuentaPorPagarFKID
    FacturasRecibidasDian --> Proveedores : ProveedorFKID
    FacturasRecibidasDian --> Usuarios : UsuarioAsignadoFKID
    FacturasRecibidasDian --> Usuarios : UsuarioRechazoFKID
    FacturasRecibidasDian --> Usuarios : UsuarioAprobacionFKID
    GestionesCrm --> FallidosCrm : CausaFKID
    GestionesCrm --> OportunidadesCrm : OportunidadCrmFKID
    GestionesCrm --> OrigenesCrm : OrigenFKID
    GestionesCrm --> TiposGestionCrm : TipoGestionFkID
    ImpuestosICA --> Municipios : MunicipioFKID
    ImpuestosICAxClientes --> Clientes : ClienteFKID
    ImpuestosICAxClientes --> ImpuestosICA : ICAFKID
    ImpuestosICAxClientes --> PlandeCuentas : CuentaFKID
    ItemCufeXMLAllowanceCharges --> ItemsCufeXML : ItemCufeXMLFKID
    ItemCufeXMLTaxSubtotals --> ItemCufeXMLTaxes : ItemCufeXMLTaxFKID
    ItemCufeXMLTaxes --> ItemsCufeXML : ItemCufeXMLFKID
    ItemsCufeXML --> ClasificacionesConceptos : ClasificacionFKID
    ItemsCufeXML --> ConceptosRetencion : ConceptoFKID
    ItemsCufeXML --> CufesXML : CufeXMLFKID
    ItemsCufeXML --> PlandeCuentas : CuentaKID
    Municipios --> Departamentos : DepartamentoFKID
    OportunidadesCrm --> ContactosEmpresasCrm : ContactoEmpresaCrmFkID
    OportunidadesCrm --> EmpresasCrm : EmpresaFKID
    OportunidadesCrm --> EtapasCrm : EtapasFkID
    OportunidadesCrm --> FallidosCrm : CausasFkID
    OportunidadesCrm --> OrigenesCrm : OrigenCrmFkID
    OportunidadesCrm --> ProductosCrm : ProductosCrmFkID
    OportunidadesCrm --> TiposOportunidadesCrm : TipoOportunidadFKID
    OportunidadesCrm --> Usuarios : UsuarioFkID
    OrigenesContactosCrm --> OrigenesCrm : OrigenFKID
    PlandeCuentas --> Clientes : ClienteFKID
    Proveedores --> ActividadesEconomicas : ActividadEconomicaFKID
    Proveedores --> ClasificacionesCentrosCostos : ClasificacionCCFKID
    Proveedores --> Clientes : ClienteFKID
    Proveedores --> CodigosPostales : CodigoPostalFKID
    Proveedores --> ConceptosRetencion : ConceptoFKID
    Proveedores --> ImpuestosICA : ICAFKID
    Proveedores --> Municipios : MunicipioFKID
    Proveedores --> PlandeCuentas : CuentaFKID
    Proveedores --> PlandeCuentas : CuentaPorPagarFKID
    Proveedores --> TiposDocumentos : TipoDocumentoFKID
    Proveedores --> TiposDocumentos : TipoDocumentoRepresentanteFKID
    Proveedores --> TiposOrganizaciones : TipoOrganizacionFKID
    ResponsabilidadesFiscalesClientes --> Clientes : ClienteFKID
    ResponsabilidadesFiscalesClientes --> ResponsabilidadesFiscales : ResposabilidadFiscalFKID
    ResponsabilidadesFiscalesProveedores --> Proveedores : ProveedorFKID
    ResponsabilidadesFiscalesProveedores --> ResponsabilidadesFiscales : ResposabilidadFiscalFKID
    ResponsabilidadesReteIva --> ResponsabilidadesFiscales : ResponsabilidadClienteFKID
    ResponsabilidadesReteIva --> ResponsabilidadesFiscales : ResponsabilidadProveedorFKID
    ResponsabilidadesRetencion --> ResponsabilidadesFiscales : ResponsabilidadProveedorFKID
    ResponsabilidadesRetencion --> ResponsabilidadesFiscales : ResponsabilidadClienteFKID
    Roles --> Modulos : ModuloFKID
    RolesxUsuarios --> Roles : RoleIdID
    RolesxUsuarios --> Usuarios : UsuarioIdID
    Terceros --> Clientes : ClienteFKID
    Terceros --> TiposDocumentos : TipoDocumentoFKID
    TiposDocumentos --> TiposOrganizaciones : TipoOrganizacionFKID
    TrazabilidadesCausaciones --> Causaciones : CausacionFKID
    TrazabilidadesCausaciones --> Usuarios : UsuarioFKID
    Usuarios --> Clientes : ClienteFKID
    UsuariosClientes --> AplicacionesxUsuarios : UsuarioModuloFKID
    UsuariosClientes --> UsuariosContabler : UsuarioContablerFKID
    UsuariosClientes --> UsuariosUbora : UsuarioUboraFKID
    UsuariosContabler --> Clientes : ClienteFKID
    UsuariosxClientes --> Clientes : ClienteFKID
    UsuariosxClientes --> Usuarios : UsuarioFKID
```