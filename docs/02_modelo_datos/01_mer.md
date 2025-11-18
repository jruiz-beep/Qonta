# 2.1 Modelo Entidad-Relación (MER)

Este documento describe el modelo de datos de Qonta, presentando las tablas principales del sistema y sus relaciones. El diseño de la base de datos es fundamental para el manejo eficiente y la integridad de la información de las facturas electrónicas y los procesos de causación.

## Diagrama Entidad-Relación General

El siguiente diagrama proporciona una vista de alto nivel de las entidades principales y sus interacciones en la base de datos de Qonta.

```mermaid
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
    
    AplicacionesxUsuarios --> Modulos : ModuloFK:Id
    AplicacionesxUsuarios --> Usuarios : UsuarioFK:Id
    Causaciones --> CentrosCostos : CentroCostosFK:Id
    Causaciones --> ClasificacionesConceptos : ClasificacionFK:Id
    Causaciones --> ConceptosRetencion : ConceptoFK:Id
    Causaciones --> CufesXML : CufeXMLFK:Id
    Causaciones --> FacturasRecibidasDian : FacturaFK:Id
    Causaciones --> Impuestos : ImpuestoFK:Id
    Causaciones --> ItemsCufeXML : ItemCufeXMLFK:Id
    Causaciones --> PlandeCuentas : CuentaFK:Id
    Causaciones --> Proveedores : ProveedorFK:Id
    Causaciones --> Terceros : TerceroFK:Id
    CausacionesOriginales --> CentrosCostos : CentroCostosFK:Id
    CausacionesOriginales --> ClasificacionesConceptos : ClasificacionFK:Id
    CausacionesOriginales --> ConceptosRetencion : ConceptoFK:Id
    CausacionesOriginales --> CufesXML : CufeXMLFK:Id
    CausacionesOriginales --> FacturasRecibidasDian : FacturaFK:Id
    CausacionesOriginales --> PlandeCuentas : CuentaFK:Id
    CausacionesOriginales --> Proveedores : ProveedorFK:Id
    CentrosCostos --> Clientes : ClienteFK:Id
    CentrosCostosxfacturas --> CentrosCostos : CentroCostosFK:Id
    CentrosCostosxfacturas --> ClasificacionesCentrosCostos : ClasificacionCCFK:Id
    CentrosCostosxfacturas --> FacturasRecibidasDian : FacturaFK:Id
    CentrosCostosxfacturas --> PlandeCuentas : CuentaFK:Id
    CierresMeses --> Clientes : ClienteFK:Id
    ClasificacionesCentrosCostos --> Clientes : ClienteFK:Id
    ClasificacionesConceptos --> Clientes : ClienteFK:Id
    ClasificacionesConceptos --> ConceptosRetencion : ConceptoFK:Id
    ClasificacionesConceptos --> PlandeCuentas : CuentaReteIvaFK:Id
    ClasificacionesConceptos --> PlandeCuentas : CuentaFK:Id
    ClasificacionesConceptos --> Proveedores : ProveedorFK:Id
    Clientes --> ActividadesEconomicas : ActividadEconomicaFK:Id
    Clientes --> CodigosPostales : CodigoPostalFK:Id
    Clientes --> MovimientosSistemas : MovimientoSistemaFK:Id
    Clientes --> TiposDocumentos : TipoDocumentoFK:Id
    Clientes --> TiposDocumentos : TipoDocumentoRepresentanteFK:Id
    Clientes --> TiposOrganizaciones : TipoOrganizacionFK:Id
    CodigosPostales --> Municipios : MunicipioFK:Id
    ConceptosRetencionxClientes --> Clientes : ClienteFK:Id
    ConceptosRetencionxClientes --> ConceptosRetencion : ConceptoFK:Id
    ConceptosRetencionxClientes --> PlandeCuentas : CuentaFK:Id
    ConceptosRetencionxClientes --> PlandeCuentas : CuentaNoDeclaranteFK:Id
    ConceptosRetencionxClientes --> PlandeCuentas : CuentaReteIvaFK:Id
    ContactosEmpresasCrm --> EmpresasCrm : EmpresaCrmFk:Id
    Departamentos --> Paises : PaisFK:Id
    DetallesClasificacionesCentrosCostos --> CentrosCostos : CentroCostosFK:Id
    DetallesClasificacionesCentrosCostos --> ClasificacionesCentrosCostos : ClasificacionCCFK:Id
    DetallesClasificacionesCentrosCostos --> PlandeCuentas : CuentaFK:Id
    DetallesClasificacionesConceptos --> ClasificacionesConceptos : ClasificacionFK:Id
    DetallesFacturasRecibidasDianxAsignaciones --> FacturasRecibidasDian : FacturaFK:Id
    DetallesFacturasRecibidasDianxAsignaciones --> Usuarios : UsuarioFK:Id
    DetallesImpuestos --> Clientes : ClienteFK:Id
    DetallesImpuestos --> Impuestos : ImpuestoFK:Id
    DetallesImpuestos --> PlandeCuentas : CuentaServicioFK:Id
    DetallesImpuestos --> PlandeCuentas : CuentaBienFK