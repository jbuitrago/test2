## :blue_book:  Datos de mi Empresa

Aplicación de uso general para la visualización de Facturas

## :blue_book: Front-End
> Framework: Angular 8,
> Dependencies : Bootstrap 4, Material 8

## :blue_book: Back-End
> NetCore 2.2
En el siguiente link se encuentra la documentacion de la API - [Swagger](https://desktopdesa.fibercorp.com.ar/v2/swagger/index.html) 

## :blue_book: Instalacion

Clonar el repositorio:

```
git clone ssh://git@git.int.fibercorp.com.ar:2201/synaptic/facturasapp.git

cd facturasapp

```

Instalar las dependencias:

```
npm install
```

Ejecutar en Desarrollo:

```
npm run dev
```

## :blue_book: Documentación de la API

Puedes encontrar la documentación sobre cada endpoint y cada modelo asociado a los mismos en el siguiente [link](https://gestiondesa.fibercorp.com.ar/v2/swagger).

## :orange_book: Casos de uso 

En la siguiente tabla se listan los diferentes casos de uso que soporta actualemente la API:

|              Modulo               |                                     Nombre                                      |                          Descripcion                           | Constante
| --- | --- | --- |
| `Clientes` | [Tipos documento](#mag-tipos-documento-clientes) |  Listado de Tipos de Documento| urlTiposDocumentosSearch
| `Clientes` | [Cliente](#mag-clientes) | Busqueda de Cliente | urlCuicSearch
| `Fibercorp` | [Contratos](#mag-contratos-fibercorp) | Listado de Contratos |urlContractSearch
| `Fibercorp` | [Facturas](#mag-facturas-fibercorp) | Listado de Facturas de Fibercorp | urlGetInfoMobileListResource 
| `Fibercorp` | [Notas de Credito](#mag-notas-de-credito-fibercorp) |  Listado de Notas de Crédito |FibercorpFacturaCreditNotes
| `Fibercorp` | [PDF](#mag-factura-fibercorp-pdf) | Factura en PDF |  urlObtainPDFFibercorpReal
| `Fibercorp` | [PDF](#mag-cotizador-dolar) |Cotizacion Dolar |  getCotizadorDolarData
| `Telecom Datos` | [Acuerdos](#mag-acuerdos-telecom-datos) | Listado de Acuerdos de Telecom Datos | urlAgreementsSearch
| `Telecom Datos` | [Facturas](#mag-facturas-telecom-datos) | Listado de Facturas de Telecom Datos |
| `Telecom Datos` | [Notas de Crédito](#mag-notas-de-credito-telecom-datos) | Listado de Notas de Crédito de Telecom Datos |
| `Telecom Datos` | [Notas de Débito](#mag-notas-de-debito-telecom-datos) |Listado de Notas de Debito de Telecom Datos |
| `Telecom Datos` | [Facturas PDF](#mag-facturas-telecom-datos-pdf) | Telecomo Datos en PDF |
| `Personal` | [Referencias de Pago](#mag-referencias-de-pago-personal) | Listado de Referencias de Pago|
| `Personal` | [Números de Linea](#mag-numeros-de-linea-personal) | Listado de Núumeros de Línea|
| `Personal` | [Facturas ](#mag-facturas-personal) | Facturas PDF |

## :mag: Tipos documento clientes

Listado de Tipos de Documento 

```
GET /v2/api/Data/tiposDocumentosSearch

```

## :mag: Clientes

Busqueda de Cliente por tipo de documento

```
GET /v2/api/Data//v2/api/Data/getClienteByCuicBaseInstalada?cuic=cuic

```

## :mag: Contratos Fibercorp

Listado de Contratos 

```
GET /v2/api/Da/v2/api/search/fibercorp_contratosbytoken

```

## :mag: Facturas Fibercorp

El servicio retorna la información de las facturas para clientes de Open para los servicios de televisión por cable y conectividad a internet.

```
GET /v2/api/search/billingmanagement.invoices

```


## :mag: Notas de credito Fibercorp

Listado de Notas de credito

```
GET /v2/api/Data/tiposDocumentosSearch

```

## :mag: Factura Fibercorp PDF

Obtener Factura en PDF

```
GET /v2/api/Data/tiposDocumentosSearch

```

## :mag: Cotizador Dolar

Obtener todas las cotizaciones del dolar

```
GET /v2/api/Data/getCotizadorDolarData

```

## :mag: Acuerdos Telecom Datos

Listado de Acuerdos

```
GET /v2/api/search/billingmanagement.getinfofijadatosresources

```


## :mag: Facturas Telecom Datos

Listado de Facturas

```
GET /v2/api/Data/

```


## :mag: Notas de credito Telecom Datos

Listado de Notas de Crédito

```
GET /v2/api/Data/

```


## :mag: Notas de debito Telecom Datos

Listado de Notas de Débito

```
GET /v2/api/Data/

```


## :mag: Facturas Telecom Datos PDF

Obtener Factura en PDF

```
GET /v2/api/Data/tiposDocumentosSearch

```


## :mag: Referencias de pago Personal

El servicio retorna información para obtener documentos (facturas, notas decrédito o debito) de un cliente.Se podrá llamar solo por cuit y retornara todas las referencia de pago.Se podrá llamar con cuit y referencia de pago para obtener los números de líneas

```
GET /v2/api/search/billingmanagement.getinfomobilecustomerdata

```
## :mag: Números de Linea Personal

El servicio retorna información para obtener documentos (facturas, notas decrédito o debito) de un cliente.Se podrá llamar solo por cuit y retornara todas las referencia de pago.Se podrá llamar con cuit y referencia de pago para obtener los números de líneas

```
GET /v2/api/Data/

```


## :mag: Facturas Personal

Listado de Facturas

```
GET /v2/api/search/billingmanagement.getinfomobilelistresources

```




## :green_book: Casos de uso para testing

En la siguiente lista, se muestra el orden y descripción de cada paso a probar:




## :mag: Buscar Cliente

Buscar un cliente para ver las facturas puede utilizar la opción Nro Documento y utilizar alguno de los  de los siguientes  cuit : 30500010084 , 30500006613, 20260490347, 20295944537, 30500051309, 30500057102,30500094156,  30500247882, 30500487379


## :computer: FiberCorp

En el modulode Fibercorp puede consultar facturas y/o Notas de credito utilizando el filtro de Contratos.

## :phone: Telecom Voz

Este módulo no esta desarrollado actualmente.

## :phone: Telecom Datos

En el modulo de Telecom Datos puede consultar facturas o Notas de credito utilizando el filtro de Acuerdos.

## :iphone: Personal

En el modulo de Personal puede consultar facturas o Notas de credito utilizando el filtro de Acuerdos.




