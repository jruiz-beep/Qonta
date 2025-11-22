# 2.1 Modelo Entidad-Relación (MER)

Este documento describe el modelo de datos de Qonta, presentando las tablas principales del sistema y sus relaciones. El diseño de la base de datos es fundamental para el manejo eficiente y la integridad de la información de las facturas electrónicas y los procesos de causación.

## Diagramas por Módulo Funcional

Para facilitar la comprensión, el modelo de datos se ha dividido en los siguientes diagramas funcionales.

### 1. Módulo de Facturación y Causación

Este diagrama muestra las entidades centrales del proceso de recepción, aprobación y contabilización de facturas.
``` mermaid
    classDiagram
    direction LR
    %% Módulo 1: sólo nombres de tablas y campos de relación
    class FacturasRecibidasDian { uniqueidentifier Id\nuniqueidentifier CufeXMLFK\nuniqueidentifier ClienteFK\nuniqueidentifier ProveedorFK }
    class CufesXML { uniqueidentifier Id }
    class ItemsCufeXML { uniqueidentifier Id\nuniqueidentifier CufeXMLFK }
    class ItemCufeXMLAllowanceCharges { uniqueidentifier Id\nuniqueidentifier ItemCufeXMLFK }
    class ItemCufeXMLTaxes { uniqueidentifier Id\nuniqueidentifier ItemCufeXMLFK }
    class ItemCufeXMLTaxSubtotals { uniqueidentifier Id\nuniqueidentifier ItemCufeXMLTaxFK }
    class Causaciones { uniqueidentifier Id\nuniqueidentifier FacturaFK\nuniqueidentifier ItemCufeXMLFK\nuniqueidentifier CuentaFK\nuniqueidentifier CentroCostosFK }
    class CausacionesOriginales { uniqueidentifier Id\nuniqueidentifier FacturaFK }
    class CentrosCostosxfacturas { uniqueidentifier Id\nuniqueidentifier FacturaFK\nuniqueidentifier CentroCostosFK }
    class TrazabilidadesCausaciones { uniqueidentifier Id\nuniqueidentifier CausacionFK\nuniqueidentifier UsuarioFK }
    class DetallesFacturasRecibidasDianxAsignaciones { uniqueidentifier Id\nuniqueidentifier FacturaFK\nuniqueidentifier UsuarioFK }

    FacturasRecibidasDian --> CufesXML : CufeXMLFK
    FacturasRecibidasDian --> Proveedores : ProveedorFK
    FacturasRecibidasDian --> Clientes : ClienteFK
    ItemsCufeXML --> CufesXML : CufeXMLFK
    ItemCufeXMLAllowanceCharges --> ItemsCufeXML : ItemCufeXMLFK
    ItemCufeXMLTaxes --> ItemsCufeXML : ItemCufeXMLFK
    ItemCufeXMLTaxSubtotals --> ItemCufeXMLTaxes : ItemCufeXMLTaxFK
    Causaciones --> FacturasRecibidasDian : FacturaFK
    Causaciones --> ItemsCufeXML : ItemCufeXMLFK
    Causaciones --> PlandeCuentas : CuentaFK
    Causaciones --> CentrosCostos : CentroCostosFK
    Causaciones --> Impuestos : ImpuestoFK
    CausacionesOriginales --> FacturasRecibidasDian : FacturaFK
    CentrosCostosxfacturas --> FacturasRecibidasDian : FacturaFK
    CentrosCostosxfacturas --> CentrosCostos : CentroCostosFK
    TrazabilidadesCausaciones --> Causaciones : CausacionFK
    TrazabilidadesCausaciones --> Usuarios : UsuarioFK
    DetallesFacturasRecibidasDianxAsignaciones --> FacturasRecibidasDian : FacturaFK
    DetallesFacturasRecibidasDianxAsignaciones --> Usuarios : UsuarioFK
```

### 2. Módulo de Terceros (Clientes, Proveedores y Terceros)

Este modelo representa las entidades principales que interactúan con el sistema, como clientes, proveedores y usuarios internos.

``` mermaid
    classDiagram
    direction RL
    %% Módulo 2: sólo nombres de tablas y campos de relación
    class Clientes { uniqueidentifier Id\nuniqueidentifier TipoOrganizacionFK\nuniqueidentifier TipoDocumentoFK\nuniqueidentifier ActividadEconomicaFK\nuniqueidentifier CodigoPostalFK\nuniqueidentifier TipoDocumentoRepresentanteFK\nuniqueidentifier MovimientoSistemaFK }
    class Proveedores { uniqueidentifier Id\nuniqueidentifier TipoOrganizacionFK\nuniqueidentifier TipoDocumentoFK\nuniqueidentifier ActividadEconomicaFK\nuniqueidentifier CodigoPostalFK\nuniqueidentifier TipoDocumentoRepresentanteFK\nuniqueidentifier ClienteFK\nuniqueidentifier CuentaPorPagarFK\nuniqueidentifier ConceptoFK\nuniqueidentifier CuentaFK\nuniqueidentifier ICAFK\nuniqueidentifier MunicipioFK\nuniqueidentifier ClasificacionCCFK }
    class Usuarios { uniqueidentifier Id\nuniqueidentifier ClienteFK }
    class Terceros { uniqueidentifier Id\nuniqueidentifier ClienteFK\nuniqueidentifier TipoDocumentoFK }
    class ActividadesEconomicas { uniqueidentifier Id }
    class TiposDocumentos { uniqueidentifier Id }
    class TiposOrganizaciones { uniqueidentifier Id }
    class CodigosPostales { uniqueidentifier Id\nuniqueidentifier MunicipioFK }
    class MovimientosSistemas { uniqueidentifier Id }
    class ResponsabilidadesFiscalesProveedores { uniqueidentifier Id\nuniqueidentifier ProveedorFK\nuniqueidentifier ResposabilidadFiscalFK }
    class DetallesProveedores { uniqueidentifier Id\nuniqueidentifier ProveedorFK\nuniqueidentifier ClienteFK\nuniqueidentifier UsuarioFK }

    Proveedores --> Clientes : ClienteFK
    Usuarios --> Clientes : ClienteFK
    Terceros --> Clientes : ClienteFK
    Clientes --> ActividadesEconomicas : ActividadEconomicaFK
    Proveedores --> ActividadesEconomicas : ActividadEconomicaFK
    Clientes --> TiposDocumentos : TipoDocumentoFK
    Proveedores --> TiposDocumentos : TipoDocumentoFK
    Terceros --> TiposDocumentos : TipoDocumentoFK
    Clientes --> TiposOrganizaciones : TipoOrganizacionFK
    Clientes --> CodigosPostales : CodigoPostalFK
    Proveedores --> CodigosPostales : CodigoPostalFK
    Clientes --> MovimientosSistemas : MovimientoSistemaFK
    Proveedores --> Municipios : MunicipioFK
    ResponsabilidadesFiscalesProveedores --> Proveedores : ProveedorFK
    ResponsabilidadesFiscalesProveedores --> ResponsabilidadesFiscales : ResposabilidadFiscalFK
    DetallesProveedores --> Proveedores : ProveedorFK
    DetallesProveedores --> Clientes : ClienteFK
    DetallesProveedores --> Usuarios : UsuarioFK
```

### 3. Módulo de Configuración Contable y Fiscal

Aquí se definen las tablas para la configuración de planes de cuentas, impuestos, retenciones y sus clasificaciones.

``` mermaid
    classDiagram
    %% Módulo 3: configuración contable y fiscal (sólo nombres y campos FK)
    class PlandeCuentas { uniqueidentifier Id\nuniqueidentifier ClienteFK }
    class Impuestos { uniqueidentifier Id }
    class ConceptosRetencion { uniqueidentifier Id }
    class ClasificacionesConceptos { uniqueidentifier Id\nuniqueidentifier ProveedorFK\nuniqueidentifier ConceptoFK\nuniqueidentifier CuentaFK\nuniqueidentifier ClienteFK }
    class ImpuestosICA { uniqueidentifier Id\nuniqueidentifier MunicipioFK }
    class ResponsabilidadesFiscales { uniqueidentifier Id }
    class ActividadesEconomicas { uniqueidentifier Id }
    class TiposOrganizaciones { uniqueidentifier Id }
    class Paises { uniqueidentifier Id }
    class Departamentos { uniqueidentifier Id\nuniqueidentifier PaisFK }
    class Municipios { uniqueidentifier Id\nuniqueidentifier DepartamentoFK }
    class CodigosPostales { uniqueidentifier Id\nuniqueidentifier MunicipioFK }
    class ResponsabilidadesRetencion { uniqueidentifier Id }
    class ResponsabilidadesReteIva { uniqueidentifier Id }
    class DetallesClasificacionesConceptos { uniqueidentifier Id\nuniqueidentifier ClasificacionFK\nuniqueidentifier CuentaFK }
    class Variables { uniqueidentifier Id }
    class MotivosRechazoFacturas { uniqueidentifier Id }

    PlandeCuentas --> Clientes : ClienteFK
    ClasificacionesConceptos --> Clientes : ClienteFK
    ClasificacionesConceptos --> Proveedores : ProveedorFK
    ClasificacionesConceptos --> ConceptosRetencion : ConceptoFK
    ClasificacionesConceptos --> PlandeCuentas : CuentaFK
    ImpuestosICAxClientes --> ImpuestosICA : ICAFK
    ImpuestosICAxClientes --> Clientes : ClienteFK
    ResponsabilidadesFiscalesClientes --> ResponsabilidadesFiscales : ResposabilidadFiscalFK
    ResponsabilidadesFiscalesProveedores --> ResponsabilidadesFiscales : ResposabilidadFiscalFK
    Clientes --> ActividadesEconomicas : ActividadEconomicaFK
    TiposDocumentos --> TiposOrganizaciones : TipoOrganizacionFK
    Departamentos --> Paises : PaisFK
    Municipios --> Departamentos : DepartamentoFK
    CodigosPostales --> Municipios : MunicipioFK
    DetallesClasificacionesConceptos --> ClasificacionesConceptos : ClasificacionFK
    DetallesClasificacionesConceptos --> PlandeCuentas : CuentaFK
    Clientes --> Variables : MovimientoSistemaFK
    MotivosRechazoFacturas --> FacturasRecibidasDian : MotivoRechazoFK
```

### 4. Mi empresa

Define cómo los clientes se relacionan con las tablas que se utilizan para la causación.

``` mermaid
    classDiagram
    %% Módulo 4: Mi empresa (sólo nombres y campos FK)
    class Clientes { uniqueidentifier Id }
    class ResponsabilidadesFiscalesClientes { uniqueidentifier Id\nuniqueidentifier ClienteFK\nuniqueidentifier ResposabilidadFiscalFK }
    class PlandeCuentas { uniqueidentifier Id\nuniqueidentifier ClienteFK }
    class CentrosCostos { uniqueidentifier Id\nuniqueidentifier ClienteFK }
    class ClasificacionesCentrosCostos { uniqueidentifier Id\nuniqueidentifier ClienteFK }
    class ConceptosRetencionxClientes { uniqueidentifier Id\nuniqueidentifier ClienteFK\nuniqueidentifier ConceptoFK\nuniqueidentifier CuentaFK }
    class DetallesImpuestos { uniqueidentifier Id\nuniqueidentifier ClienteFK\nuniqueidentifier ImpuestoFK\nuniqueidentifier CuentaFK }
    class ImpuestosICAxClientes { uniqueidentifier Id\nuniqueidentifier ICAFK\nuniqueidentifier ClienteFK\nuniqueidentifier CuentaFK }
    class CierresMeses { uniqueidentifier Id\nuniqueidentifier ClienteFK }

    ResponsabilidadesFiscalesClientes --> Clientes : ClienteFK
    PlandeCuentas --> Clientes : ClienteFK
    CentrosCostos --> Clientes : ClienteFK
    ClasificacionesCentrosCostos --> Clientes : ClienteFK
    ConceptosRetencionxClientes --> Clientes : ClienteFK
    DetallesImpuestos --> Clientes : ClienteFK
    ImpuestosICAxClientes --> Clientes : ClienteFK
    CierresMeses --> Clientes : ClienteFK
```

### 5. Módulo de Seguridad y Acceso

Define cómo los usuarios se relacionan con roles, módulos y aplicaciones para gestionar permisos.

``` mermaid
    classDiagram
    %% Módulo 5: Seguridad y acceso (sólo nombres y campos FK)
    class Usuarios { uniqueidentifier Id }
    class Roles { uniqueidentifier Id\nuniqueidentifier ModuloFK }
    class Modulos { uniqueidentifier Id }
    class RolesxUsuarios { uniqueidentifier Id\nuniqueidentifier UsuarioId\nuniqueidentifier RoleId }
    class AplicacionesxUsuarios { uniqueidentifier Id\nuniqueidentifier UsuarioFK\nuniqueidentifier ModuloFK }
    class UsuariosxClientes { uniqueidentifier Id\nuniqueidentifier ClienteFK\nuniqueidentifier UsuarioFK }

    RolesxUsuarios --> Usuarios : UsuarioId
    RolesxUsuarios --> Roles : RoleId
    Roles --> Modulos : ModuloFK
    AplicacionesxUsuarios --> Usuarios : UsuarioFK
    AplicacionesxUsuarios --> Modulos : ModuloFK
    UsuariosxClientes --> Clientes : ClienteFK
    UsuariosxClientes --> Usuarios : UsuarioFK
```

<!-- El siguiente bloque contiene la definición de todas las tablas para que Mermaid pueda renderizar los diagramas. -->

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
    FacturasRecibidasDian --> Clientes : ClienteFKID
    FacturasRecibidasDian --> CufesXML : CufeXMLFKID
    FacturasRecibidasDian --> MotivosRechazoFacturas : MotivoRechazoFKID
    FacturasRecibidasDian --> PlandeCuentas : CuentaPorPagarFKID
    FacturasRecibidasDian --> Proveedores : ProveedorFKID
    FacturasRecibidasDian --> Usuarios : UsuarioAsignadoFKID
    FacturasRecibidasDian --> Usuarios : UsuarioRechazoFKID
    FacturasRecibidasDian --> Usuarios : UsuarioAprobacionFKID
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
    UsuariosxClientes --> Clientes : ClienteFKID
    UsuariosxClientes --> Usuarios : UsuarioFKID
```