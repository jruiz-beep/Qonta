# 2.1 Modelo Entidad-Relación (MER)

Este documento describe el modelo de datos de Qonta, presentando las tablas principales del sistema y sus relaciones. El diseño de la base de datos es fundamental para el manejo eficiente y la integridad de la información de las facturas electrónicas y los procesos de causación.

## Diagramas por Módulo Funcional

Para facilitar la comprensión, el modelo de datos se ha dividido en los siguientes diagramas funcionales.

### 1. Módulo de Facturación y Causación

Este diagrama muestra las entidades centrales del proceso de recepción, aprobación y contabilización de facturas.
``` mermaid
    flowchart LR
    %% Estilos para que las cajas sean blancas y cuadradas

    %% Definición de Nodos (Tablas)
    FacturasRecibidasDian[FacturasRecibidasDian]:::tables
    CufesXML[CufesXML]:::tables
    ItemsCufeXML[ItemsCufeXML]:::tables
    ItemCufeXMLAllowanceCharges[ItemCufeXMLAllowanceCharges]:::tables
    ItemCufeXMLTaxes[ItemCufeXMLTaxes]:::tables
    ItemCufeXMLTaxSubtotals[ItemCufeXMLTaxSubtotals]:::tables
    Causaciones[Causaciones]:::tables
    CausacionesOriginales[CausacionesOriginales]:::tables
    CentrosCostosxfacturas[CentrosCostosxfacturas]:::tables
    TrazabilidadesCausaciones[TrazabilidadesCausaciones]:::tables
    DetallesFacturasRecibidasDianxAsignaciones[DetallesFacturasRecibidasDianxAsignaciones]:::tables
    Proveedores[Proveedores]:::tables
    Clientes[Clientes]:::tables
    PlandeCuentas[PlandeCuentas]:::tables
    CentrosCostos[CentrosCostos]:::tables
    Impuestos[Impuestos]:::tables
    Usuarios[Usuarios]:::tables

    %% Relaciones
    FacturasRecibidasDian -->|CufeXMLFK| CufesXML
    FacturasRecibidasDian -->|ProveedorFK| Proveedores
    FacturasRecibidasDian -->|ClienteFK| Clientes
    ItemsCufeXML -->|CufeXMLFK| CufesXML
    ItemCufeXMLAllowanceCharges -->|ItemCufeXMLFK| ItemsCufeXML
    ItemCufeXMLTaxes -->|ItemCufeXMLFK| ItemsCufeXML
    ItemCufeXMLTaxSubtotals -->|ItemCufeXMLTaxFK| ItemCufeXMLTaxes
    Causaciones -->|FacturaFK| FacturasRecibidasDian
    Causaciones -->|ItemCufeXMLFK| ItemsCufeXML
    Causaciones -->|CuentaFK| PlandeCuentas
    Causaciones -->|CentroCostosFK| CentrosCostos
    Causaciones -->|ImpuestoFK| Impuestos
    CausacionesOriginales -->|FacturaFK| FacturasRecibidasDian
    CentrosCostosxfacturas -->|FacturaFK| FacturasRecibidasDian
    CentrosCostosxfacturas -->|CentroCostosFK| CentrosCostos
    TrazabilidadesCausaciones -->|CausacionFK| Causaciones
    TrazabilidadesCausaciones -->|UsuarioFK| Usuarios
    DetallesFacturasRecibidasDianxAsignaciones -->|FacturaFK| FacturasRecibidasDian
    DetallesFacturasRecibidasDianxAsignaciones -->|UsuarioFK| Usuarios
```

### 2. Módulo de Terceros (Clientes, Proveedores y Terceros)

Este modelo representa las entidades principales que interactúan con el sistema, como clientes, proveedores y usuarios internos.

``` mermaid
    flowchart RL
    %% Módulo 2: sólo nombres de tablas y campos de relación (estilo flowchart)
    Clientes[Clientes]:::tables
    Proveedores[Proveedores]:::tables
    Usuarios[Usuarios]:::tables
    Terceros[Terceros]:::tables
    ActividadesEconomicas[ActividadesEconomicas]:::tables
    TiposDocumentos[TiposDocumentos]:::tables
    TiposOrganizaciones[TiposOrganizaciones]:::tables
    CodigosPostales[CodigosPostales]:::tables
    MovimientosSistemas[MovimientosSistemas]:::tables
    ResponsabilidadesFiscalesProveedores[ResponsabilidadesFiscalesProveedores]:::tables
    DetallesProveedores[DetallesProveedores]:::tables

    Proveedores -->|ClienteFK| Clientes
    Usuarios -->|ClienteFK| Clientes
    Terceros -->|ClienteFK| Clientes
    Clientes -->|ActividadEconomicaFK| ActividadesEconomicas
    Proveedores -->|ActividadEconomicaFK| ActividadesEconomicas
    Clientes -->|TipoDocumentoFK| TiposDocumentos
    Proveedores -->|TipoDocumentoFK| TiposDocumentos
    Terceros -->|TipoDocumentoFK| TiposDocumentos
    Clientes -->|TipoOrganizacionFK| TiposOrganizaciones
    Clientes -->|CodigoPostalFK| CodigosPostales
    Proveedores -->|CodigoPostalFK| CodigosPostales
    Clientes -->|MovimientoSistemaFK| MovimientosSistemas
    Proveedores -->|MunicipioFK| Municipios
    ResponsabilidadesFiscalesProveedores -->|ProveedorFK| Proveedores
    ResponsabilidadesFiscalesProveedores -->|ResposabilidadFiscalFK| ResponsabilidadesFiscales
    DetallesProveedores -->|ProveedorFK| Proveedores
    DetallesProveedores -->|ClienteFK| Clientes
    DetallesProveedores -->|UsuarioFK| Usuarios
```

### 3. Módulo de Configuración Contable y Fiscal

Aquí se definen las tablas para la configuración de planes de cuentas, impuestos, retenciones y sus clasificaciones.
    
``` mermaid
    flowchart LR
    %% Módulo 3: configuración contable y fiscal (estilo flowchart)
    PlandeCuentas[PlandeCuentas]:::tables
    Impuestos[Impuestos]:::tables
    ConceptosRetencion[ConceptosRetencion]:::tables
    ClasificacionesConceptos[ClasificacionesConceptos]:::tables
    ImpuestosICA[ImpuestosICA]:::tables
    ResponsabilidadesFiscales[ResponsabilidadesFiscales]:::tables
    ActividadesEconomicas[ActividadesEconomicas]:::tables
    TiposOrganizaciones[TiposOrganizaciones]:::tables
    Paises[Paises]:::tables
    Departamentos[Departamentos]:::tables
    Municipios[Municipios]:::tables
    CodigosPostales[CodigosPostales]:::tables
    ResponsabilidadesRetencion[ResponsabilidadesRetencion]:::tables
    ResponsabilidadesReteIva[ResponsabilidadesReteIva]:::tables
    DetallesClasificacionesConceptos[DetallesClasificacionesConceptos]:::tables
    Variables[Variables]:::tables
    MotivosRechazoFacturas[MotivosRechazoFacturas]:::tables

    PlandeCuentas -->|ClienteFK| Clientes
    ClasificacionesConceptos -->|ClienteFK| Clientes
    ClasificacionesConceptos -->|ProveedorFK| Proveedores
    ClasificacionesConceptos -->|ConceptoFK| ConceptosRetencion
    ClasificacionesConceptos -->|CuentaFK| PlandeCuentas
    ImpuestosICAxClientes -->|ICAFK| ImpuestosICA
    ImpuestosICAxClientes -->|ClienteFK| Clientes
    ResponsabilidadesFiscalesClientes -->|ResposabilidadFiscalFK| ResponsabilidadesFiscales
    ResponsabilidadesFiscalesProveedores -->|ResposabilidadFiscalFK| ResponsabilidadesFiscales
    Clientes -->|ActividadEconomicaFK| ActividadesEconomicas
    TiposDocumentos -->|TipoOrganizacionFK| TiposOrganizaciones
    Departamentos -->|PaisFK| Paises
    Municipios -->|DepartamentoFK| Departamentos
    CodigosPostales -->|MunicipioFK| Municipios
    DetallesClasificacionesConceptos -->|ClasificacionFK| ClasificacionesConceptos
    DetallesClasificacionesConceptos -->|CuentaFK| PlandeCuentas
    Clientes -->|MovimientoSistemaFK| Variables
    MotivosRechazoFacturas -->|MotivoRechazoFK| FacturasRecibidasDian
    %% Relaciones hacia ConceptosRetencion (se agrega explicitamente en Módulo 3)
    ClasificacionesConceptos -->|ConceptoFK| ConceptosRetencion
    Causaciones -->|ConceptoFK| ConceptosRetencion
    ItemsCufeXML -->|ConceptoFK| ConceptosRetencion
    ResponsabilidadesRetencion -->|ConceptoFK| ConceptosRetencion
    ResponsabilidadesReteIva -->|ConceptoFK| ConceptosRetencion
```

### 4. Mi empresa

Define cómo los clientes se relacionan con las tablas que se utilizan para la causación.

``` mermaid
    flowchart LR
    %% Módulo 4: Mi empresa (estilo flowchart)
    Clientes[Clientes]:::tables
    ResponsabilidadesFiscalesClientes[ResponsabilidadesFiscalesClientes]:::tables
    PlandeCuentas[PlandeCuentas]:::tables
    CentrosCostos[CentrosCostos]:::tables
    ClasificacionesCentrosCostos[ClasificacionesCentrosCostos]:::tables
    ConceptosRetencionxClientes[ConceptosRetencionxClientes]:::tables
    DetallesImpuestos[DetallesImpuestos]:::tables
    ImpuestosICAxClientes[ImpuestosICAxClientes]:::tables
    CierresMeses[CierresMeses]:::tables

    ResponsabilidadesFiscalesClientes -->|ClienteFK| Clientes
    PlandeCuentas -->|ClienteFK| Clientes
    CentrosCostos -->|ClienteFK| Clientes
    ClasificacionesCentrosCostos -->|ClienteFK| Clientes
    ConceptosRetencionxClientes -->|ClienteFK| Clientes
    DetallesImpuestos -->|ClienteFK| Clientes
    ImpuestosICAxClientes -->|ClienteFK| Clientes
    CierresMeses -->|ClienteFK| Clientes
```

### 5. Módulo de Seguridad y Acceso

Define cómo los usuarios se relacionan con roles, módulos y aplicaciones para gestionar permisos.

``` mermaid
    flowchart LR
    %% Módulo 5: Seguridad y acceso (estilo flowchart)
    Usuarios[Usuarios]:::tables
    Roles[Roles]:::tables
    Modulos[Modulos]:::tables
    RolesxUsuarios[RolesxUsuarios]:::tables
    AplicacionesxUsuarios[AplicacionesxUsuarios]:::tables
    UsuariosxClientes[UsuariosxClientes]:::tables

    RolesxUsuarios -->|UsuarioId| Usuarios
    RolesxUsuarios -->|RoleId| Roles
    Roles -->|ModuloFK| Modulos
    AplicacionesxUsuarios -->|UsuarioFK| Usuarios
    AplicacionesxUsuarios -->|ModuloFK| Modulos
    UsuariosxClientes -->|ClienteFK| Clientes
    UsuariosxClientes -->|UsuarioFK| Usuarios
    %% Usuario tiene FK hacia Cliente
    Usuarios -->|ClienteFK| Clientes
```

<!-- El siguiente bloque contiene la definición de todas las tablas para que Mermaid pueda renderizar los diagramas. -->

<details>
<summary>Definición de Clases (Tablas) (flowchart LR)</summary>

``` mermaid
    flowchart LR
    %% ESTILOS
    classDef tables fill:none,stroke:#9b59b6,stroke-width:2px,color:#fff;

    %% NODOS (TABLAS)
    ActividadesEconomicas[ActividadesEconomicas]:::tables
    AplicacionesxUsuarios[AplicacionesxUsuarios]:::tables
    Causaciones[Causaciones]:::tables
    CausacionesOriginales[CausacionesOriginales]:::tables
    CentrosCostos[CentrosCostos]:::tables
    CentrosCostosxfacturas[CentrosCostosxfacturas]:::tables
    CierresMeses[CierresMeses]:::tables
    ClasificacionesCentrosCostos[ClasificacionesCentrosCostos]:::tables
    ClasificacionesConceptos[ClasificacionesConceptos]:::tables
    Clientes[Clientes]:::tables
    CodigosPostales[CodigosPostales]:::tables
    ConceptosRetencion[ConceptosRetencion]:::tables
    ConceptosRetencionxClientes[ConceptosRetencionxClientes]:::tables
    CufesXML[CufesXML]:::tables
    Departamentos[Departamentos]:::tables
    DetallesClasificacionesCentrosCostos[DetallesClasificacionesCentrosCostos]:::tables
    DetallesClasificacionesConceptos[DetallesClasificacionesConceptos]:::tables
    DetallesFacturasRecibidasDianxAsignaciones[DetallesFacturasRecibidasDianxAsignaciones]:::tables
    DetallesImpuestos[DetallesImpuestos]:::tables
    DetallesProveedores[DetallesProveedores]:::tables
    FacturasRecibidasDian[FacturasRecibidasDian]:::tables
    Impuestos[Impuestos]:::tables
    ImpuestosICA[ImpuestosICA]:::tables
    ImpuestosICAxClientes[ImpuestosICAxClientes]:::tables
    ItemCufeXMLAllowanceCharges[ItemCufeXMLAllowanceCharges]:::tables
    ItemCufeXMLTaxSubtotals[ItemCufeXMLTaxSubtotals]:::tables
    ItemCufeXMLTaxes[ItemCufeXMLTaxes]:::tables
    ItemsCufeXML[ItemsCufeXML]:::tables
    Modulos[Modulos]:::tables
    MotivosRechazoFacturas[MotivosRechazoFacturas]:::tables
    MovimientosSistemas[MovimientosSistemas]:::tables
    Municipios[Municipios]:::tables
    Paises[Paises]:::tables
    PlandeCuentas[PlandeCuentas]:::tables
    Proveedores[Proveedores]:::tables
    ResponsabilidadesFiscales[ResponsabilidadesFiscales]:::tables
    ResponsabilidadesFiscalesClientes[ResponsabilidadesFiscalesClientes]:::tables
    ResponsabilidadesFiscalesProveedores[ResponsabilidadesFiscalesProveedores]:::tables
    ResponsabilidadesReteIva[ResponsabilidadesReteIva]:::tables
    ResponsabilidadesRetencion[ResponsabilidadesRetencion]:::tables
    Roles[Roles]:::tables
    RolesxUsuarios[RolesxUsuarios]:::tables
    Terceros[Terceros]:::tables
    TiposDocumentos[TiposDocumentos]:::tables
    TiposOrganizaciones[TiposOrganizaciones]:::tables
    TrazabilidadesCausaciones[TrazabilidadesCausaciones]:::tables
    Usuarios[Usuarios]:::tables
    UsuariosxClientes[UsuariosxClientes]:::tables
    Variables[Variables]:::tables

    %% RELACIONES
    AplicacionesxUsuarios -->|ModuloFKID| Modulos
    AplicacionesxUsuarios -->|UsuarioFKID| Usuarios
    Causaciones -->|CentroCostosFKID| CentrosCostos
    Causaciones -->|ClasificacionFKID| ClasificacionesConceptos
    Causaciones -->|ConceptoFKID| ConceptosRetencion
    Causaciones -->|CufeXMLFKID| CufesXML
    Causaciones -->|FacturaFKID| FacturasRecibidasDian
    Causaciones -->|ImpuestoFKID| Impuestos
    Causaciones -->|ItemCufeXMLFKID| ItemsCufeXML
    Causaciones -->|CuentaFKID| PlandeCuentas
    Causaciones -->|ProveedorFKID| Proveedores
    Causaciones -->|TerceroFKID| Terceros
    CausacionesOriginales -->|CentroCostosFKID| CentrosCostos
    CausacionesOriginales -->|ClasificacionFKID| ClasificacionesConceptos
    CausacionesOriginales -->|ConceptoFKID| ConceptosRetencion
    CausacionesOriginales -->|CufeXMLFKID| CufesXML
    CausacionesOriginales -->|FacturaFKID| FacturasRecibidasDian
    CausacionesOriginales -->|CuentaFKID| PlandeCuentas
    CausacionesOriginales -->|ProveedorFKID| Proveedores
    CentrosCostos -->|ClienteFKID| Clientes
    CentrosCostosxfacturas -->|CentroCostosFKID| CentrosCostos
    CentrosCostosxfacturas -->|ClasificacionCCFKID| ClasificacionesCentrosCostos
    CentrosCostosxfacturas -->|FacturaFKID| FacturasRecibidasDian
    CentrosCostosxfacturas -->|CuentaFKID| PlandeCuentas
    CierresMeses -->|ClienteFKID| Clientes
    ClasificacionesCentrosCostos -->|ClienteFKID| Clientes
    ClasificacionesConceptos -->|ClienteFKID| Clientes
    ClasificacionesConceptos -->|ConceptoFKID| ConceptosRetencion
    ClasificacionesConceptos -->|CuentaReteIvaFKID| PlandeCuentas
    ClasificacionesConceptos -->|CuentaFKID| PlandeCuentas
    ClasificacionesConceptos -->|ProveedorFKID| Proveedores
    Clientes -->|ActividadEconomicaFKID| ActividadesEconomicas
    Clientes -->|CodigoPostalFKID| CodigosPostales
    Clientes -->|MovimientoSistemaFKID| MovimientosSistemas
    Clientes -->|TipoDocumentoFKID| TiposDocumentos
    Clientes -->|TipoDocumentoRepresentanteFKID| TiposDocumentos
    Clientes -->|TipoOrganizacionFKID| TiposOrganizaciones
    CodigosPostales -->|MunicipioFKID| Municipios
    ConceptosRetencionxClientes -->|ClienteFKID| Clientes
    ConceptosRetencionxClientes -->|ConceptoFKID| ConceptosRetencion
    ConceptosRetencionxClientes -->|CuentaFKID| PlandeCuentas
    ConceptosRetencionxClientes -->|CuentaNoDeclaranteFKID| PlandeCuentas
    ConceptosRetencionxClientes -->|CuentaReteIvaFKID| PlandeCuentas
    Departamentos -->|PaisFKID| Paises
    DetallesClasificacionesCentrosCostos -->|CentroCostosFKID| CentrosCostos
    DetallesClasificacionesCentrosCostos -->|ClasificacionCCFKID| ClasificacionesCentrosCostos
    DetallesClasificacionesCentrosCostos -->|CuentaFKID| PlandeCuentas
    DetallesClasificacionesConceptos -->|ClasificacionFKID| ClasificacionesConceptos
    DetallesFacturasRecibidasDianxAsignaciones -->|FacturaFKID| FacturasRecibidasDian
    DetallesFacturasRecibidasDianxAsignaciones -->|UsuarioFKID| Usuarios
    DetallesImpuestos -->|ClienteFKID| Clientes
    DetallesImpuestos -->|ImpuestoFKID| Impuestos
    DetallesImpuestos -->|CuentaServicioFKID| PlandeCuentas
    DetallesImpuestos -->|CuentaBienFKID| PlandeCuentas
    DetallesImpuestos -->|CuentaFKID| PlandeCuentas
    DetallesProveedores -->|ClienteFKID| Clientes
    DetallesProveedores -->|ProveedorFKID| Proveedores
    DetallesProveedores -->|UsuarioFKID| Usuarios
    FacturasRecibidasDian -->|ClienteFKID| Clientes
    FacturasRecibidasDian -->|CufeXMLFKID| CufesXML
    FacturasRecibidasDian -->|MotivoRechazoFKID| MotivosRechazoFacturas
    FacturasRecibidasDian -->|CuentaPorPagarFKID| PlandeCuentas
    FacturasRecibidasDian -->|ProveedorFKID| Proveedores
    FacturasRecibidasDian -->|UsuarioAsignadoFKID| Usuarios
    FacturasRecibidasDian -->|UsuarioRechazoFKID| Usuarios
    FacturasRecibidasDian -->|UsuarioAprobacionFKID| Usuarios
    ImpuestosICA -->|MunicipioFKID| Municipios
    ImpuestosICAxClientes -->|ClienteFKID| Clientes
    ImpuestosICAxClientes -->|ICAFKID| ImpuestosICA
    ImpuestosICAxClientes -->|CuentaFKID| PlandeCuentas
    ItemCufeXMLAllowanceCharges -->|ItemCufeXMLFKID| ItemsCufeXML
    ItemCufeXMLTaxSubtotals -->|ItemCufeXMLTaxFKID| ItemCufeXMLTaxes
    ItemCufeXMLTaxes -->|ItemCufeXMLFKID| ItemsCufeXML
    ItemsCufeXML -->|ClasificacionFKID| ClasificacionesConceptos
    ItemsCufeXML -->|ConceptoFKID| ConceptosRetencion
    ItemsCufeXML -->|CufeXMLFKID| CufesXML
    ItemsCufeXML -->|CuentaKID| PlandeCuentas
    Municipios -->|DepartamentoFKID| Departamentos
    PlandeCuentas -->|ClienteFKID| Clientes
    Proveedores -->|ActividadEconomicaFKID| ActividadesEconomicas
    Proveedores -->|ClasificacionCCFKID| ClasificacionesCentrosCostos
    Proveedores -->|ClienteFKID| Clientes
    Proveedores -->|CodigoPostalFKID| CodigosPostales
    Proveedores -->|ConceptoFKID| ConceptosRetencion
    Proveedores -->|ICAFKID| ImpuestosICA
    Proveedores -->|MunicipioFKID| Municipios
    Proveedores -->|CuentaFKID| PlandeCuentas
    Proveedores -->|CuentaPorPagarFKID| PlandeCuentas
    Proveedores -->|TipoDocumentoFKID| TiposDocumentos
    Proveedores -->|TipoDocumentoRepresentanteFKID| TiposDocumentos
    Proveedores -->|TipoOrganizacionFKID| TiposOrganizaciones
    ResponsabilidadesFiscalesClientes -->|ClienteFKID| Clientes
    ResponsabilidadesFiscalesClientes -->|ResposabilidadFiscalFKID| ResponsabilidadesFiscales
    ResponsabilidadesFiscalesProveedores -->|ProveedorFKID| Proveedores
    ResponsabilidadesFiscalesProveedores -->|ResposabilidadFiscalFKID| ResponsabilidadesFiscales
    ResponsabilidadesReteIva -->|ResponsabilidadClienteFKID| ResponsabilidadesFiscales
    ResponsabilidadesReteIva -->|ResponsabilidadProveedorFKID| ResponsabilidadesFiscales
    ResponsabilidadesRetencion -->|ResponsabilidadProveedorFKID| ResponsabilidadesFiscales
    ResponsabilidadesRetencion -->|ResponsabilidadClienteFKID| ResponsabilidadesFiscales
    Roles -->|ModuloFKID| Modulos
    RolesxUsuarios -->|RoleIdID| Roles
    RolesxUsuarios -->|UsuarioIdID| Usuarios
    Terceros -->|ClienteFKID| Clientes
    Terceros -->|TipoDocumentoFKID| TiposDocumentos
    TiposDocumentos -->|TipoOrganizacionFKID| TiposOrganizaciones
    TrazabilidadesCausaciones -->|CausacionFKID| Causaciones
    TrazabilidadesCausaciones -->|UsuarioFKID| Usuarios
    Usuarios -->|ClienteFKID| Clientes
    UsuariosxClientes -->|ClienteFKID| Clientes
    UsuariosxClientes -->|UsuarioFKID| Usuarios
```