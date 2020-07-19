# Datos de mi Empresa

Aplicación de uso general para la visualización de Facturas

## Front-End
> Framework: Angular 8,
> Dependencies : Bootstrap 4, Material 8

## Back-End
> NetCore 2.2
En el siguiente link se encuentra la documentacion de la API - [Swagger](https://desktopdesa.fibercorp.com.ar/v2/swagger/index.html) 

## :blue_book: Documentación de la API

Puedes encontrar la documentación sobre cada endpoint y cada modelo asociado a los mismos en el siguiente [link](https://gestiondesa.fibercorp.com.ar/v2/swagger).

## :orange_book: Casos de uso 

| Módulo | Nombre | Descripción |
| --- | --- | --- |
| `Buscar Cliente` | [Buscar Cliente](#gem-inicio-de-sesión) | Enumera |
| `Fibercorp` | Enumera todos los archivos *nuevos o modificados* | Enumera |
| `Telecom Voz` | Enumera todos los archivos *nuevos o modificados* | Enumera |
| `Telecom Datos` | Enumera todos los archivos *nuevos o modificados* | Enumera |
| `Persona` | Enumera todos los archivos *nuevos o modificados* | Enumera |

## :gem: Inicio de sesión

El inició de sesión consta de recibir un usuario y contraseña, que va a ser validado contra la base de datos _(comparando el password con el hashed guardado en la base)_; este en caso de ser valido, va a generar cookies que van a ser guardados en el browser y la sesión del navegador. Estos cookies son los que se validan luego en cada request que se vaya a hacer en la UI luego. El más importante a tener en cuenta es el `oauth_token` _(token temporal)_, el cual permite identificar si el usuario es valido o no y tiene permiso para realizar las diferentes operaciones sobre la plataforma.



