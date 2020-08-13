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
### Para poder ejecutar el proyecto 

Dependiendo del administrador de dependencias (***NPM (https://www.npmjs.com/)*** o ***YARN (https://yarnpkg.com/)


```
npm run start - yarn start
```

### Crear empaquetado para deployar una nueva versión

```
npm run build-prod - yarn run build-prod  
```

### :blue_book: Arquitectura del proyecto

. La estructura del proyecto esta compuesto de la siguiente manera:

- AppModule


##### AppModule

Este es el modulo principal del proyecto y contiene todos los componentes y servicios del proyecto.


| Component | Descripción |
|--------|--------
|CuicComponent|Este componente se usa para buscar un cliente|
|BillFibercorpComponent|Este componente se usa para mostrar Facturas de Fibercorp|
|BillTelecomvozComponent|Este componente se usa para mostrar Facturas de Telecom Voz|
|BillTelecomdatosComponent|Este componente se usa para mostrar Facturas de Telecom Datos|
|BillPersonalComponent|Este componente se usa para mostrar Facturas de Personal|


##### Composición

````
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { CuicComponent } from './components/cuic/cuic.component';
import { BillFibercorpComponent } from './components/bill-fibercorp/bill-fibercorp.component';
import { BillTelecomvozComponent } from './components/bill-telecomvoz/bill-telecomvoz.component';
import { BillTelecomdatosComponent } from './components/bill-telecomdatos/bill-telecomdatos.component';
import { BillPersonalComponent } from './components/bill-personal/bill-personal.component';
@NgModule({
  declarations: [
    AppComponent,
    CuicComponent,
    BillFibercorpComponent,
    BillTelecomvozComponent,
    BillTelecomdatosComponent,
    BillPersonalComponent,
  ],
  imports: [

  ],
  entryComponents: [],
  providers: [

  ],
  bootstrap: [AppComponent]
})
export class AppModule { }

````

## :blue_book: Documentación de la API

Puedes encontrar la documentación sobre cada endpoint y cada modelo asociado a los mismos en el siguiente [link](https://gestiondesa.fibercorp.com.ar/v2/swagger).

## :orange_book: Casos de uso 

En la siguiente tabla se listan los diferentes casos de uso que soporta actualemente la API:

|              Modulo               |                                     Nombre                                      |                          Descripcion                           | Constante
| --- | --- | --- | --- |
| `Clientes` | [Tipos documento](#mag-tipos-documento-clientes) |  Listado de Tipos de Documento| urlTiposDocumentosSearch
| `Clientes` | [Cliente](#mag-clientes) | Busqueda de Cliente | urlCuicSearch
| `Fibercorp` | [Contratos](#computer-contratos-fibercorp) | Listado de Contratos |urlContractSearch
| `Fibercorp` | [Facturas](#computer-facturas-fibercorp) | Listado de Facturas de Fibercorp | urlGetInfoMobileListResource 
| `Fibercorp` | [Notas de Credito](#computer-notas-de-credito-fibercorp) |  Listado de Notas de Crédito |FibercorpFacturaCreditNotes
| `Fibercorp` | [Facturas PDF](#computer-factura-fibercorp-pdf) | Factura en PDF |  urlObtainPDFFibercorpReal
| `Fibercorp` | [DOLAR](#computer-cotizador-dolar) |Cotizacion Dolar |  getCotizadorDolarData
| `Telecom Datos` | [Acuerdos](#phone-acuerdos-telecom-datos) | Listado de Acuerdos de Telecom Datos | urlAgreementsSearch
| `Telecom Datos` | [Facturas](#phone-facturas-telecom-datos) | Listado de Facturas de Telecom Datos | urlAgreementsSearch
| `Telecom Datos` | [Notas de Crédito](#phone-notas-de-credito-telecom-datos) | Listado de Notas de Crédito de Telecom Datos | urlAgreementsSearch
| `Telecom Datos` | [Notas de Débito](#phone-notas-de-debito-telecom-datos) |Listado de Notas de Debito de Telecom Datos | urlAgreementsSearch
| `Telecom Datos` | [Facturas PDF](#phone-facturas-telecom-datos-pdf) | Telecom Datos en PDF | urlObtainPdfMobileResources
| `Personal` | [Referencias de Pago](#iphone-referencias-de-pago-personal) | Listado de Referencias de Pago| urlReferencesSearch
| `Personal` | [Números de Linea](#iphone-numeros-de-linea-personal) | Listado de Núumeros de Línea| urlReferencesSearch
| `Personal` | [Facturas ](#iphone-facturas-personal) | Facturas | urlGetInfoMobileListResource
| `Personal` | [Facturas PDF](#iphone-facturas-personal-pdf) | Personal en PDF | urlObtainPdfMobileResources

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

## :computer: Contratos Fibercorp

Listado de Contratos 

```
GET /v2/api/Da/v2/api/search/fibercorp_contratosbytoken

```

## :computer: Facturas Fibercorp

El servicio retorna la información de las facturas para clientes de Open para los servicios de televisión por cable y conectividad a internet.

```
GET /v2/api/search/billingmanagement.invoices

```


## :computer: Notas de credito Fibercorp

Listado de Notas de credito

```
GET /v2/api/Data/tiposDocumentosSearch

```

## :computer: Factura Fibercorp PDF

Obtener Factura en PDF

```
GET /v2/api/Data/tiposDocumentosSearch

```

## :computer: Cotizador Dolar

Obtener todas las cotizaciones del dolar

```
GET /v2/api/Data/getCotizadorDolarData

```

## :phone: Acuerdos Telecom Datos

Listado de Acuerdos

```
GET /v2/api/search/billingmanagement.getinfofijadatosresources

```


## :phone: Facturas Telecom Datos

Listado de Facturas

```
GET /v2/api/search/billingmanagement.getinfofijadatosresources

```


## :phone: Notas de credito Telecom Datos

Listado de Notas de Crédito

```
GET /v2/api/Data/

```


## :phone: Notas de debito Telecom Datos

Listado de Notas de Débito

```
GET /v2/api/Data/

```


## :phone: Facturas Telecom Datos PDF

Obtener Factura en PDF

```
GET /v2/api/search/billingmanagement.getinfofijadatosresources

```


## :iphone: Referencias de pago Personal

El servicio retorna información para obtener documentos (facturas, notas decrédito o debito) de un cliente.Se podrá llamar solo por cuit y retornara todas las referencia de pago.Se podrá llamar con cuit y referencia de pago para obtener los números de líneas

```
GET /v2/api/search/billingmanagement.getinfomobilecustomerdata

```
## :iphone: Números de Linea Personal

El servicio retorna información para obtener documentos (facturas, notas decrédito o debito) de un cliente.Se podrá llamar solo por cuit y retornara todas las referencia de pago.Se podrá llamar con cuit y referencia de pago para obtener los números de líneas

```
GET /v2/api/Data/

```


## :iphone: Facturas Personal

Listado de Facturas

```
GET  /v2/api/search/billingmanagement.obtainpdfmobileresources

```




## :green_book: Secciones

La aplicacion se compone de las siguientes secciones:

|              Modulo               | Caso de Uso
| --- | --- |
| `Clientes` |  [Buscar Cliente](#mag-buscar-cliente)
| `Fibercorp` | [Fibercorp](#mag-fibercorp) 
| `Telecom Voz` | [Telecom Voz](#mag-telecom-voz) 
| `Telecom Datos` | [TelecomDatos](#mag-telecom-datos)
| `Personal` | [Personal](#mag-personal) 




## :mag: Buscar Cliente

Es la seccion principal y sirve para buscar un cliente ingresando el Tipo de Documento y  Nro Documento: 30500010084 , 30500006613, 20260490347, 20295944537, 30500051309, 30500057102,30500094156,  30500247882, 30500487379


### Componente



### Servicio





## :computer: FiberCorp

En el modulode Fibercorp puede consultar facturas y/o Notas de credito utilizando el filtro de Contratos.



## :phone: Telecom Voz

Este módulo no esta desarrollado actualmente.

## :phone: Telecom Datos

En el modulo de Telecom Datos puede consultar facturas o Notas de credito utilizando el filtro de Acuerdos.

## :iphone: Personal

En el modulo de Personal puede consultar facturas o Notas de credito utilizando el filtro de Acuerdos.




