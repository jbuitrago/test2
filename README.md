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

````javascript
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





## :green_book: Secciones

La aplicacion se compone de las siguientes secciones:

|              Modulo               | Caso de Uso
| --- | --- |
| `Clientes` |  [Buscar Cliente](#buscar-cliente)
| `Fibercorp` | [Fibercorp](#fibercorp) 
| `Telecom Voz` | [Telecom Voz](#telecom-voz) 
| `Telecom Datos` | [TelecomDatos](#telecom-datos)
| `Personal` | [Personal](#personal) 




## Buscar Cliente

Es la seccion principal y sirve para buscar un cliente ingresando el Tipo de Documento y  Nro Documento: 30500010084 , 30500006613, 20260490347, 20295944537, 30500051309, 30500057102,30500094156,  30500247882, 30500487379


### Listado de Componentes

| Componente | Descripción |
|--------|--------
|CuicComponent|Este componente se usa para buscar un cliente|


##### CuicComponent


```` javascript
import { Component, OnInit, OnDestroy } from '@angular/core';
import { FormGroup, FormBuilder, Validators } from '@angular/forms';
import { MatSnackBar } from '@angular/material';
import { AppConstants } from 'src/app/app.constants';
import { CuicSearchServiceService } from 'src/app/services/cuic-search-service.service';
import { ClienteModel } from 'src/app/models/clientes.model';
import { Subscription } from 'rxjs';

@Component({
  selector: 'app-cuic',
  templateUrl: './cuic.component.html',
  styleUrls: ['./cuic.component.scss']
})
export class CuicComponent implements OnInit, OnDestroy {
  // Declaro la letiable para saber si es GOL o Desktop, por default es Desktop
  isGestionEnviroment: boolean;
  cliente: ClienteModel = null;
  tiposDocumentos: any = [];
  cuicSearched: string = "";
  typeCuicSearched: string = "";
  subscriptions = new Subscription();

  // Declaro el formulario de busqueda del CUIC
  public cuicForm: FormGroup;

  constructor(
    private formBuilder: FormBuilder,
    public snackBar: MatSnackBar,
    public CuicSearchServiceService: CuicSearchServiceService
  ) {
    // Seteo el flag para saber si es GOL o DESKTOP
    this.isGestionEnviroment = AppConstants.isGestionEnviroment;
    this.buildForm();
  }

  buildForm() {

    // Armo el FormBuild para las validaciones de los campos
    this.cuicForm = this.formBuilder.group({
      cuic: ['', [Validators.required, Validators.minLength(11), Validators.maxLength(11)]],
      tipocuic: ['', [Validators.required]]
    });
  }

  ngOnInit(): void {
    /**
     * Chequeo el ambiente desde donde estoy ejecutando la App ( GOL / DESKTOP)
     * Si es GOL, debo directamente ejecutar la App, si es DESKTOP debo mostrar el form de busqueda y cargar los tipo de documentos.
     **/
    if (!this.isGestionEnviroment) {
      const subscription = this.CuicSearchServiceService.getDocumentsType().subscribe(data => {
        this.tiposDocumentos = data;
      });
      this.subscriptions.add(subscription);
    }
  }

  // Funcion encargada de la busqueda del CUIC solicitado.
  onSearch_Click() {
    if (this.cuicForm.dirty && this.cuicForm.valid) {
      let cuicValue = this.cuicForm.get(['tipocuic']).value;
      if (cuicValue == '-1' || cuicValue == '00') {
        cuicValue = '';
      }
      this.cuicSearched = cuicValue + this.cuicForm.get(['cuic']).value;
      const subscription = this.CuicSearchServiceService.getClientByCuic(this.cuicSearched.toString())
        .subscribe(data => {
          this.cliente = data;
          this.CuicSearchServiceService.showApp = false;
          AppConstants.clientData = data;
        });
      this.subscriptions.add(subscription);
    }
  }

  // Funcion que se encarga de no dejar escribir codigos especiales, no permite el ENTER.
  cuic_KeyUp($event) {
    if ($event.which != '37' && $event.which != '39') {
      let result = this.cuicForm.get(['cuic']).value.replace(/[^0-9]/g, '');
      this.cuicForm.get(['cuic']).setValue(result);
    }
  }

  // Funcion que forma al Nombre del cliente encontrado
  getNameClient(cliente) {
    return `${cliente.NOMBRE_CLIENTE} ${cliente.APELLIDO_CLIENTE}`;
  }

  // Funcion que se encarga de disparar la App en base al cuic elegido
  clienteNameOn_Click(cuic) {
    AppConstants.cuic = cuic;
    this.CuicSearchServiceService.showApp = true;
  }

  // Obtengo el texto del item seleccionado del combo Tipo de Documento
  itemSelected(item) {
    this.cliente = null; // debo limpiar en cada cambio el resultado obtenido, ya que corresponde a una nueva busqueda por hacer.
    this.typeCuicSearched = item.source.selected._element.nativeElement.innerText;
  }

  ngOnDestroy() {
    this.subscriptions.unsubscribe();
  }

}

````


### Servicios


| Servicio | Descripción |
|--------|--------
|getUserAccountFromToken|Obtención los datos del usuario de GOL / Deesktop|
|getDocumentsType|Obtención del listado de tipos de documento ( tabla ExternalData > DIM_TiposDocumentos )|
|getClientByCuic|Obtención de los datos de la empresa asignada del usuario GOL / Desktop|
|getUserAccountFromToken|Obtención de los datos de la empresa asignada del usuario GOL / Desktop|


````javascript

import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { AppConstants } from 'src/app/app.constants';
import { CustomHandlerRowsResponse } from 'src/app/models/response.model/response.model';
import 'rxjs/add/operator/map';
import "rxjs/add/operator/catch";
import { GlobalServicesConfiguration } from './global-services-config';
import { ClienteModel } from 'src/app/models/clientes.model';
import { TeamAtentionModel, ContactoModel } from 'src/app/models/contactos.model';
import { CookieService } from 'ngx-cookie-service';

@Injectable({
  providedIn: 'root'
})
export class CuicSearchServiceService extends GlobalServicesConfiguration {
  // A traves del servicio, se maneja la visualización directa o no de la App. ( GOL true, Desktop false ). El default es Desktop.
  showApp: boolean = false;

  constructor(
    private _http: HttpClient,
    private cookieService: CookieService
  ) {
    super()
  }

  // Obtención los datos del usuario de GOL / Deesktop
  getUserAccountFromToken() {
    let cookie = this.cookieService.get('oauth_token') ? this.cookieService.get('oauth_token') : AppConstants.onlyForDevelop;
    let _url = `${this.getBaseUrl()}${AppConstants.urlGerUserFromToken}?token=${cookie}`
    return this._http.get(_url, { headers: this.head, withCredentials: true })
      .map(data => {
        return data as CustomHandlerRowsResponse
      })
  };

  // Obtención del listado de tipos de documento ( tabla ExternalData > DIM_TiposDocumentos )
  getDocumentsType() {
    let _url = `${this.getBaseUrl()}` + AppConstants.urlTiposDocumentosSearch
    return this._http.get(_url,
      {
        headers: this.head,
        withCredentials: true
      }
    )
      .map(data => {
        let results = data as CustomHandlerRowsResponse;
        return results;
      })
      .catch(this.handleError);

  };

  // Obtención de los datos de la empresa asignada del usuario GOL / Desktop
  getClientByCuic(cuic: string) {
    let _url = `${this.getBaseUrl()}${AppConstants.urlCuicSearch}?likecuic=${cuic}`
    return this._http.get<ClienteModel>(_url,
      {
        headers: this.head,
        withCredentials: true
      }
    )
      .map(data => {
        return data[0] as ClienteModel;
      })
      .catch(this.handleError);
  };

  
}


````


## FiberCorp

En el modulode Fibercorp puede consultar facturas y/o Notas de credito utilizando el filtro de Contratos.

### Listado de Componentes

| Componente | Descripción |
|--------|--------
|BillFibercorpComponent|Este componente se usa para mostrar el listado facturas de FiberCorp|


##### BillFibercorpComponent


````javascript
import { Component, OnInit, OnDestroy } from '@angular/core';
import { FormGroup, FormBuilder, Validators } from '@angular/forms';
import { MatSnackBar, MatDialogConfig, MatDialog } from '@angular/material';
import { FibercorpServiceService } from 'src/app/services/fibercorp-search-service.service';
import { AppConstants } from 'src/app/app.constants';
import { DomSanitizer } from '@angular/platform-browser';
import { DownloadFileService } from 'src/app/services/download-file/download-file.service';
import { Subscription } from 'rxjs/internal/Subscription';
import * as moment from 'moment';
import { DialogOverviewPDFComponent } from 'src/app/shared/overview-pdf/overview-pdf.component';
import { Observable, forkJoin, of, combineLatest } from 'rxjs';
import { switchMap, map, mergeMap, catchError } from 'rxjs/operators';
import { ModalLoadingComponent } from 'src/app/shared/modal-waiting/modal-loading.component';
import { ToastService } from 'src/app/shared/toast/toast.service';
moment.locale('es');

interface Vouchers {
  value: string;
  viewValue: string;
}

interface Downloads {
  value: string;
  viewValue: string;
}
const displayedColumns = ['date', 'type', 'documentNumber', 'dueDate', 'amount', 'contract', 'Clase']
@Component({
  selector: 'app-bill-fibercorp',
  templateUrl: './bill-fibercorp.component.html',
  styleUrls: ['./bill-fibercorp.component.scss']
})

export class BillFibercorpComponent implements OnInit, OnDestroy {
  selectVoucher = 'I';
  selectContract = 0;
  contracts: any = [];
  dolar: any = [];
  dolarToday: String = "";
  invoices: any = [];
  creditNotes: any = [];
  displayedColumns = displayedColumns;
  displayedColumnsInvoice: string[];
  displayedColumnsCreditNote: string[];
  vouchers: Vouchers[] = [
    { value: 'T', viewValue: 'Todos' },
    { value: 'I', viewValue: 'Facturas' },
    { value: 'CN', viewValue: 'Notas de Crédito' },

  ];
  downloads: Downloads[] = [
    { value: 'excel', viewValue: 'EXCEL' },
    { value: 'csv', viewValue: 'CSV' },
    { value: 'pdf', viewValue: 'PDF' },
  ];
  subscriptions = new Subscription();
  public billForm: FormGroup;
  pdfContent: any;
  file: any;
  items$ = new Observable<any>();

  constructor(
    private formBuilder: FormBuilder,
    public snackBar: MatSnackBar,
    private sanitizer: DomSanitizer,
    private dialog: MatDialog,
    private FibercorpServiceService: FibercorpServiceService,
    private downloadFileService: DownloadFileService,
    private toastService: ToastService
  ) { }

  ngOnInit() {
    this.buildForm();
    this.loadItems();
  }

  buildForm() {
    this.billForm = this.formBuilder.group({
      contract: ['', [Validators.required]],
      voucher: ['', [Validators.required]],
      download: ['', [Validators.required]]
    });
  }

  getAmount(amount) {
    return `$ ${amount}`
  }

  loadItems() {
    let cotizationDolars = [];
    this.items$ =
      combineLatest(
        [
          this.FibercorpServiceService.getContract(AppConstants.cuic),
          this.FibercorpServiceService.getDolar()
        ])
        .pipe(
          switchMap(([contract, cotizations]) => {
            if (contract.rows.length > 0) {
              cotizationDolars = cotizations;
              this.selectContract = contract.rows[0].ID_SUSCRIPCION;
              this.contracts = contract.rows;
              let REQUEST = []
              switch (this.selectVoucher) {
                case "I":
                  REQUEST.push(this.FibercorpServiceService.getInvoice(this.selectContract.toString(), 12))
                  break;
                case "CN":
                  REQUEST.push(this.FibercorpServiceService.getCreditNotes(this.selectContract.toString(), 12))
                  break;
                case "T":
                  REQUEST = [
                    this.FibercorpServiceService.getInvoice(this.selectContract.toString(), 24).pipe(catchError(error => of([]))),
                    this.FibercorpServiceService.getCreditNotes(this.selectContract.toString(), 24).pipe(catchError(error => of([])))
                  ]
                  break;
              }
              return forkJoin(REQUEST)
            } else {
              return of([])
            }
          }),
          map(result => {
            if (result.length > 0) {
              let items = result.length > 1 ? result[0].concat(result[1]) : result[0];
              let mapItems = items.map(item => {
                // Obtengo el Array de cotizaciones el primer dato, que es el valido para el titulo.
                this.dolarToday = cotizationDolars[0].cotization;

                cotizationDolars.forEach((cotizationItem, Index) => {
                  // Only for FACTURAS no Notas de Credito
                  if (item.documentType == "F") {
                    // FIX parse para quitar el -3:00 que viene en el ITEM.
                    let parseItemDate = item.date.split("-3:00")
                    item.date = parseItemDate[0]

                    // Armo un objeto moment parseado a EN-US para validacion.
                    let date = moment(moment(item.date).format('YYYY-MM-DD'));
                    let dateFrom = moment(moment(cotizationItem.from).format('YYYY-MM-DD'));
                    let dateTo = moment(moment(cotizationItem.to).format('YYYY-MM-DD'));

                    if (date.isSameOrAfter(dateFrom, 'year') && date.isSameOrBefore(dateTo, 'year')) {
                      // El año debe estar entre los de la cotizacion.
                      if (date.isSame(dateFrom, 'month')) {
                        // El mes debe ser igual.
                        item.cotization = cotizationItem.cotization;
                      }
                    }
                  }
                })
                // Tranformo la F a FACTURA sino a NOTA DE CREDITO.
                item.documentType = item.documentType == "F" ? "FACTURA" : "NOTA DE CRÉDITO"
                return item;
              });
              return mapItems.sort((a, b) => new Date(b.date).getTime() - new Date(a.date).getTime());
            } else {
              return of([])
            }
          })
        )
  }

  onSearch_Click() {
    let contractValue = this.billForm.get(['contract']).value.toString();
    let cotizationDolars = [];
    this.items$ =
      this.FibercorpServiceService.getDolar()
        .pipe(
          mergeMap((cotizations: any) => {
            cotizationDolars = cotizations;
            let REQUEST = []
            switch (this.selectVoucher) {
              case "I":
                REQUEST.push(this.FibercorpServiceService.getInvoice(contractValue, 12))
                break;
              case "CN":
                REQUEST.push(this.FibercorpServiceService.getCreditNotes(contractValue, 12))
                break;
              case "T":
                REQUEST.push(this.FibercorpServiceService.getInvoice(contractValue, 12))
                REQUEST.push(this.FibercorpServiceService.getCreditNotes(contractValue, 12))
                break;
            }
            return forkJoin(REQUEST)
          }),
          map(result => {
            let items = result.length > 1 ? result[0].concat(result[1]) : result[0];
            let mapItems = items.map(item => {
              cotizationDolars.forEach((cotizationItem, Index) => {
                // Only for FACTURAS no Notas de Credito
                if (item.documentType == "F") {
                  // FIX parse para quitar el -3:00 que viene en el ITEM.
                  let parseItemDate = item.date.split("-3:00")
                  item.date = parseItemDate[0]

                  // Armo un objeto moment parseado a EN-US para validacion.
                  let date = moment(moment(item.date).format('YYYY-MM-DD'));
                  let dateFrom = moment(moment(cotizationItem.from).format('YYYY-MM-DD'));
                  let dateTo = moment(moment(cotizationItem.to).format('YYYY-MM-DD'));

                  if (date.isSameOrAfter(dateFrom, 'year') && date.isSameOrBefore(dateTo, 'year')) {
                    // El año debe estar entre los de la cotizacion.
                    if (date.isSame(dateFrom, 'month')) {
                      // El mes debe ser igual.
                      item.cotization = cotizationItem.cotization;
                    }
                  }
                }
              })
              // Tranformo la F a FACTURA sino a NOTA DE CREDITO.
              item.documentType = item.documentType == "F" ? "FACTURA" : "NOTA DE CRÉDITO"
              return item;
            });
            return mapItems.sort((a, b) => new Date(b.date).getTime() - new Date(a.date).getTime());
          }),
          catchError(() => {
            return of([]);
          })
        )
  }

  openLoadingModal() {
    const dialogConfig = new MatDialogConfig();
    dialogConfig.disableClose = true;
    dialogConfig.autoFocus = true;
    dialogConfig.width = "200px";
    dialogConfig.height = "200px";
    dialogConfig.data = {
      display: true
    };
    this.dialog.open(ModalLoadingComponent, dialogConfig);
  }

  downloadFile() {
    this.openLoadingModal();
    const subscription = this.items$.subscribe(items => {
      if (items.length > 0) {
        let values = this.mapFileKeys(items);
        let extensionFile = this.billForm.get(['download']).value.toLowerCase();
        let keys;
        switch (extensionFile) {
          case "excel":
            this.downloadFileService.exportAsExcelFile(values, 'Facturas de Telecom', extensionFile);
            break;
          case "csv":
            values = this.mapFileKeys(items).map(value => {
              return [...Object.values(value)]
            });
            keys = {
              Fecha: 'Fecha',
              [`Tipo Comprobante`]: 'Tipo Comprobante',
              [`N° Comprobante`]: 'N° Comprobante',
              Vencimiento: 'Vencimiento',
              Importe: 'Importe',
              [`Cotización`]: 'Cotización'
            };
            this.downloadFileService.exportCSV('Facturas de Telecom', keys, values);
            break;
          case "pdf":
            keys = ['Fecha', 'Tipo Comprobante', 'N° Comprobante', 'Vencimiento', 'Importe', 'Cotización'];
            values = this.mapFileKeys(items).map(value => {
              return [...Object.values(value)]
            });
            this.downloadFileService.exportPDF('Facturas y Notas de Credito de Fibercorp', keys, values);
            break;
        };
        this.dialog.closeAll();
      } else {
        this.dialog.closeAll();
        this.toastService.message({
          title: 'No se pudo descargar.',
          text: 'No tenés registros.',
          type: 'Warning',
        });
      }
    });
    this.subscriptions.add(subscription);
  }

  mapFileKeys(items) {
    return items.map(
      invoice => {
        return {
          Fecha: moment(invoice.date).format('MM/YYYY'),
          [`Tipo Comprobante`]: invoice.documentType,
          [`N° Comprobante`]: invoice.documentNumber,
          Vencimiento: moment(invoice.dueDate).format('DD/MM/YYYY'),
          Importe: `$ ${invoice.amount}`,
          [`Cotización`]: ``
        }
      })
  }

  capitalize(value) {
    return value.charAt(0).toUpperCase() + value.slice(1).toLowerCase();
  }

  getFile(documentType, contract, letter, date, documentNumber, amount) {
    let item = {
      type: documentType,
      contract_number: contract,
      letter,
      date: moment(date).format('YYYYMMDD'),
      fullDate: moment(date).format('DD/MM/YYYY'),
      documentNumber,
      amount: amount === "No saldo" ? "No saldo" : `$ ${amount}`,
      typeFile: documentType.toLowerCase() === "factura" ? "F" : "N"
    };
    this.pdfContent = this.sanitizer.bypassSecurityTrustResourceUrl(this.FibercorpServiceService.getFile(item));
    this.openModal(item);
  }

  getDialogTitle(item) {
    return `${this.capitalize(item.type)}: ${item.documentNumber} (${moment(item.date).format('DD/MM/YYYY')}) - ${item.amount}`;
  }

  openModal(item) {
    const dialogConfig = new MatDialogConfig();
    dialogConfig.disableClose = true;
    dialogConfig.autoFocus = true;
    dialogConfig.width = "800px";
    dialogConfig.height = "590px";
    dialogConfig.data = {
      id: 1,
      title: this.getDialogTitle(item),
      stream: this.pdfContent
    };
    this.dialog.open(DialogOverviewPDFComponent, dialogConfig);
  }

  ngOnDestroy() {
    this.subscriptions.unsubscribe();
  }
}


````

### Servicios




# Telecom Voz

Este módulo no esta desarrollado actualmente.



## Telecom Datos

En el modulo de Telecom Datos puede consultar facturas o Notas de credito utilizando el filtro de Acuerdos.

## Personal

En el modulo de Personal puede consultar facturas o Notas de credito utilizando el filtro de Acuerdos.




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
