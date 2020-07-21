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

|              Modulo               |                                     Nombre                                      |                          Descripcion                           |
| --- | --- | --- |
| `Clientes` | [Listado Tipos de documento](#mag-buscar-cliente) |  |
| `Fibercorp` | [Listado de Contratos](#computer-fibercorp) |  |
| `Fibercorp` | [Litado de Facturas](#computer-fibercorp) |  |
| `Fibercorp` | [Listado de Notas de Credito](#computer-fibercorp) |  |
| `Fibercorp` | [Ver Factura PDF](#computer-fibercorp) |  |
| `Telecom Datos` | [Listado de Acuerdos](#phone-telecom-datos) |  |
| `Telecom Datos` | [Listado de Facturas](#phone-telecom-datos) |  |
| `Telecom Datos` | [Listado de Notas de Crédito](#phone-telecom-datos) |  |
| `Telecom Datos` | [Listado de Notas de Débito](#phone-telecom-datos) |  |
| `Telecom Datos` | [Ver Factura PDF](#phone-telecom-datos) |  |
| `Personal` | [Referencias de Pago](#iphone-personal) |  |
| `Personal` | [Listado de Facturas](#iphone-personal) |  |
| `Personal` | [Personal](#iphone-personal) |  |
| `Personal` | [Personal](#iphone-personal) |  |

## :mag: Buscar Cliente

Buscar un cliente para ver las facturas puede utilizar la opción Nro Documento y utilizar alguno de los  de los siguientes  cuit : 30500010084 , 30500006613, 20260490347, 20295944537, 30500051309, 30500057102,30500094156,  30500247882, 30500487379



## :computer: FiberCorp

## :phone: Telecom Voz

## :phone: Telecom Datos

## :iphone: Personal


## :green_book: Casos de uso para testing

En la siguiente lista, se muestra el orden y descripción de cada paso a probar:




