## :blue_book:  Descripcion del Proyecto

Aplicación de uso general para la visualización de Facturas de FiberCorp, Telecom Datos y Personal.

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



<details>
  <summary>CuicComponent</summary>

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

</details>

### Servicios

Esta sección utiliza los siguientes servicios:

| Servicio | Descripción |
|--------|--------
|getUserAccountFromToken|Obtención los datos del usuario de GOL / Deesktop|
|getDocumentsType|Obtención del listado de tipos de documento ( tabla ExternalData > DIM_TiposDocumentos )|
|getClientByCuic|Obtención de los datos de la empresa asignada del usuario GOL / Desktop|
|getUserAccountFromToken|Obtención de los datos de la empresa asignada del usuario GOL / Desktop|

<details>
  <summary>FibercorpServiceService</summary>

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

</details>

## FiberCorp

En el modulode Fibercorp puede consultar facturas y/o Notas de credito utilizando el filtro de Contratos y Tipo de Comprobante,tambien permite descargar el listado de la grilla a Excel/CSV/PDF.


### Listado de Componentes

| Componente | Descripción |
|--------|--------
|BillFibercorpComponent|Este componente se usa para mostrar el listado facturas de FiberCorp|


<details>
  <summary>BillFibercorpComponent</summary> 

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
</details>

### Servicios

| Servicio | Descripción |
|--------|--------
|getAgreements|Listado de Acuerdos|
|getCustomers|Listado de Clientes|
|getInvoice|Listado de Facturas|
|getPdf|Obtenciion PDF por Letra y Nro de Documento|

<details>
  <summary>FibercorpServiceService</summary> 

````javascript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { AppConstants } from 'src/app/app.constants';
import 'rxjs/add/operator/map';
import "rxjs/add/operator/catch";
import { GlobalServicesConfiguration } from './global-services-config';
import { FacturasTelecomDatosModel } from 'src/app/models/facturastelecomdatos.model';
import { AcuerdosModel } from 'src/app/models/acuerdos.model';
import { CustomerModel } from 'src/app/models/customer.model';
import { PdfModel } from 'src/app/models/pdf.model';
@Injectable({
  providedIn: 'root'
})
export class TelecomdatosSearchServiceService extends GlobalServicesConfiguration {
  showBill: boolean = false;
  showPdf: boolean = false;
  constructor(
    private _http: HttpClient
  ) {
    super()
  }

  //Acuerdos
  getAgreements(nroCliente: string, quantity: number) {
    let _url = `${this.getBaseUrl()}${AppConstants.urlAgreementsSearch}?nroCliente=${nroCliente}&cantFacturasPorAcuerdo=${quantity}`

    return this._http.get<AcuerdosModel>(_url,
      {
        headers: this.head,
        withCredentials: true
      }
    )
      .map((data: any) => {
        return data.httpCode === "200" ? data.response.GetFacturaListResponse.GetFacturaListResult.Acuerdos.Acuerdo : [];
      })
      .catch(this.handleError);
  };

  getCustomers(cuit: string) {
    let _url = `${this.getBaseUrl()}${AppConstants.urlGetInfoFijaCustomerData}?cuit=${cuit}`
    return this._http.get<CustomerModel>(_url,
      {
        headers: this.head,
        withCredentials: true
      }
    )
      .map(data => {
        return data.response;
      })
      .catch(this.handleError);
  };

  //Facturas
  getInvoice(nroCliente: string, quantity: number, agreement: string, voucher: string) {
    let _url = `${this.getBaseUrl()}${AppConstants.urlAgreementsSearch}?nroCliente=${nroCliente}&cantFacturasPorAcuerdo=${quantity}`
    return this._http.get<FacturasTelecomDatosModel>(_url,
      {
        headers: this.head,
        withCredentials: true
      }
    )
      .map(data => {
        let dataInvoice = data.response.GetFacturaListResponse.GetFacturaListResult.Acuerdos.Acuerdo;
        let Facturas;
        dataInvoice.forEach(element => {
          if (element.Numero == agreement) {
            //ALL
            if (voucher == 'T' || voucher == '') {
              Facturas = element.Facturas.Factura;
            } else {
              Facturas = element.Facturas.Factura.filter(factura => factura.Tipo == voucher);
            }
          }
        });
        // return Facturas as FacturasTelecomDatosModel;
        return Facturas;
      })
      .catch(this.handleError);
  };

  getPdf(nroDocumento: number, letraDocumento: string) {
    let _url = `${this.getBaseUrl()}${AppConstants.urlObtainPdfFijadatosResources}?nroDocumento=${nroDocumento}&letraDocumento=${letraDocumento}`
    return this._http.get<PdfModel>(_url,
      {
        headers: this.head,
        withCredentials: true
      }
    )
      .map(data => {
        return data.response.GetFacturaImageResponse;
      })
      .catch(this.handleError);
  };
}

````

</details>

# Telecom Voz

Este módulo no esta desarrollado actualmente.



## Telecom Datos

En el modulo de Telecom Datos puede consultar facturas o Notas de credito de Telecom datos utilizando el filtro de Acuerdos y Tipo de Comprobante,tambien permite descargar el listado de la grilla a Excel/CSV/PDF.


### Listado de Componentes

| Componente | Descripción |
|--------|--------
|BillTelecomdatosComponent|Este componente se usa para mostrar el listado facturas y Notas de crédito de Telecom Datos|


<details>
  <summary>BillTelecomdatosComponent</summary> 


````javascript

import { Component, OnInit, OnDestroy } from '@angular/core';
import { FormGroup, FormBuilder, Validators } from '@angular/forms';
import { MatSnackBar, MatDialog, MatDialogConfig } from '@angular/material';
import { TelecomdatosSearchServiceService } from 'src/app/services/telecomdatos-search-service.service';
import { AppConstants } from 'src/app/app.constants';
import { DomSanitizer } from '@angular/platform-browser';
import { Subscription, Observable, of, } from 'rxjs';
import { switchMap, take, catchError } from "rxjs/operators";
import { DownloadFileService } from 'src/app/services/download-file/download-file.service';
import * as moment from 'moment';
import { DialogOverviewPDFComponent } from 'src/app/shared/overview-pdf/overview-pdf.component';
import { ModalLoadingComponent } from 'src/app/shared/modal-waiting/modal-loading.component';
import { ToastService } from 'src/app/shared/toast/toast.service';
moment.locale('es');

interface Vouchers {
  value: string;
  viewValue: string;
  single: string;
}

interface Downloads {
  value: string;
  viewValue: string;
}

@Component({
  selector: 'app-bill-telecomdatos',
  templateUrl: './bill-telecomdatos.component.html',
  styleUrls: ['./bill-telecomdatos.component.scss']
})
export class BillTelecomdatosComponent implements OnInit, OnDestroy {
  pdf: string;
  agreements: any = [];
  invoices: any = [];
  displayedColumns: string[] = ['Tipo', 'Documento', 'Vencimiento', 'Bruto', 'Clase'];
  selectAgreement = 0;
  selectVoucher = 'T';
  vouchers: Vouchers[] = [
    { value: 'T', viewValue: 'Todos', single: "Todos" },
    { value: 'FAC', viewValue: 'Facturas', single: "Factura" },
    { value: 'CRE', viewValue: 'Notas de Crédito', single: "Nota de crédito" },
    { value: 'DEB', viewValue: 'Notas de Débito', single: "Nota de débito" },
  ];
  downloads: Downloads[] = [
    { value: 'excel', viewValue: 'EXCEL' },
    { value: 'csv', viewValue: 'CSV' },
    { value: 'pdf', viewValue: 'PDF' },

  ];
  showPDF = false;
  pdfContent;
  billForm: FormGroup;
  subscriptions = new Subscription();
  invoices$ = new Observable<any>();

  constructor(
    private formBuilder: FormBuilder,
    public snackBar: MatSnackBar,
    private sanitizer: DomSanitizer,
    private dialog: MatDialog,
    private TelecomdatosSearchServiceService: TelecomdatosSearchServiceService,
    private downloadFileService: DownloadFileService,
    private toastService: ToastService
  ) { }

  ngOnInit() {
    this.buildForms();
    this.loadInvoices();
  }

  loadInvoices() {
    this.invoices$ =
      this.TelecomdatosSearchServiceService.getCustomers(AppConstants.clientData.NUMERO_IDENTIFICACION_CLIENTE).pipe(
        switchMap((customer: any) => {
          return this.TelecomdatosSearchServiceService.getAgreements(customer.nroCliente, 24)
        }),
        take(1),
        switchMap((result) => {
          this.selectAgreement = result[0].Numero;
          this.agreements = result;
          return this.TelecomdatosSearchServiceService.getCustomers(AppConstants.clientData.NUMERO_IDENTIFICACION_CLIENTE);
        }),
        switchMap((customer: any) => {
          return this.TelecomdatosSearchServiceService.getInvoice(customer.nroCliente, 24, this.selectAgreement.toString(), this.selectVoucher)
        }),
        catchError(_ => {
          return of([])
        })
      )
  }

  buildForms() {
    this.billForm = this.formBuilder.group({
      agreement: ['', [Validators.required]],
      voucher: ['', [Validators.required]],
      download: ['', [Validators.required]]
    });
  }

  onSearch_Click() {
    this.invoices$ =
      this.TelecomdatosSearchServiceService.getCustomers(AppConstants.clientData.NUMERO_IDENTIFICACION_CLIENTE).pipe(
        switchMap((customer: any) => {
          return this.TelecomdatosSearchServiceService.getInvoice(
            customer.nroCliente,
            24,
            this.billForm.get(['agreement']).value,
            this.billForm.get(['voucher']).value)
        })
      )
  }

  getPdf(nroDocumento, letraDocumento, clase, importe, fecha) {
    let item = {
      nroDocumento,
      letraDocumento,
      importe,
      fecha,
      type: this.vouchers.find(voucher => voucher.value === letraDocumento).single
    }
    const subscription =
      this.TelecomdatosSearchServiceService.getPdf(nroDocumento, `${letraDocumento}%20${clase}`).subscribe(data => {
        this.pdf = data.pdfBytes;
        let file = new Blob([this.base64ToUint8Array(this.pdf)], { type: 'application/pdf' });
        let fileURL = URL.createObjectURL(file);
        this.pdfContent = this.sanitizer.bypassSecurityTrustResourceUrl(fileURL);;
        this.openModal(item);
      },
        error => {
        });
    this.subscriptions.add(subscription);
  }

  base64ToUint8Array(base64) {
    var raw = atob(base64);
    var uint8Array = new Uint8Array(raw.length);
    for (var i = 0; i < raw.length; i++) {
      uint8Array[i] = raw.charCodeAt(i);
    }
    return uint8Array;
  }

  formatDate(date) {
    return moment(date.replace(/ /g, '/')).format('MM/YYYY');
  }

  getDialogTitle(item) {
    return `${item.type}: ${item.nroDocumento} (${this.formatDate(item.fecha)}) - $${item.importe}`;
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

 //Descargar documentos en Excel,CSV o PDF
  downloadFile() {
    this.openLoadingModal();
    const subscription = this.invoices$.subscribe(invoices => {
      if (invoices.length > 0) {
        let values = this.mapFileKeys(invoices);
        let extensionFile = this.billForm.get(['download']).value.toLowerCase();
        switch (extensionFile) {
          case "excel":
            this.downloadFileService.exportAsExcelFile(values, 'Facturas de Telecom', extensionFile);
            break;
          case "csv":
            this.downloadFileService.exportAsExcelFile(values, 'Facturas de Telecom', extensionFile);
            break;
          case "pdf":
            values = this.mapFileKeys(invoices).map(value => {
              return [...Object.values(value)]
            });
            let keys = ['Tipo Comprobante', 'N° Comprobante', 'Vencimiento', 'Importe'];
            this.downloadFileService.exportPDF('Facturas de Telecom', keys, values);
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

    })
    this.subscriptions.add(subscription);
  }

  mapFileKeys(invoices) {
    return invoices.map(
      invoice => {
        return {
          [`Tipo Comprobante`]: invoice.Tipo,
          [`N° Comprobante`]: invoice.Documento,
          Vencimiento: this.formatDate(invoice.Vencimiento),
          Bruto: `${invoice.Moneda} ${invoice.Bruto}`
        }
      })
  }

  ngOnDestroy() {
    this.subscriptions.unsubscribe();
  }


}

````

</details>

### Servicios 

Esta sección utiliza los siguientes servicios:

| Servicio | Descripción |
|--------|--------
|getAgreements|Listado de Acuerdos|
|getCustomers|Listado de Clientes|
|getInvoice|Listado de Facturas y Notas de Crédito|
|getPdf|Obtencion PDF por Letra y Nro de Documento|

<details>
  <summary>TelecomdatosSearchServiceService</summary> 

````javascript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { AppConstants } from 'src/app/app.constants';
import 'rxjs/add/operator/map';
import "rxjs/add/operator/catch";
import { GlobalServicesConfiguration } from './global-services-config';
import { FacturasTelecomDatosModel } from 'src/app/models/facturastelecomdatos.model';
import { AcuerdosModel } from 'src/app/models/acuerdos.model';
import { CustomerModel } from 'src/app/models/customer.model';
import { PdfModel } from 'src/app/models/pdf.model';
@Injectable({
  providedIn: 'root'
})
export class TelecomdatosSearchServiceService extends GlobalServicesConfiguration {
  showBill: boolean = false;
  showPdf: boolean = false;
  constructor(
    private _http: HttpClient
  ) {
    super()
  }

  //Acuerdos
  getAgreements(nroCliente: string, quantity: number) {
    let _url = `${this.getBaseUrl()}${AppConstants.urlAgreementsSearch}?nroCliente=${nroCliente}&cantFacturasPorAcuerdo=${quantity}`

    return this._http.get<AcuerdosModel>(_url,
      {
        headers: this.head,
        withCredentials: true
      }
    )
      .map((data: any) => {
        return data.httpCode === "200" ? data.response.GetFacturaListResponse.GetFacturaListResult.Acuerdos.Acuerdo : [];
      })
      .catch(this.handleError);
  };

  getCustomers(cuit: string) {
    let _url = `${this.getBaseUrl()}${AppConstants.urlGetInfoFijaCustomerData}?cuit=${cuit}`
    return this._http.get<CustomerModel>(_url,
      {
        headers: this.head,
        withCredentials: true
      }
    )
      .map(data => {
        return data.response;
      })
      .catch(this.handleError);
  };

  //Facturas
  getInvoice(nroCliente: string, quantity: number, agreement: string, voucher: string) {
    let _url = `${this.getBaseUrl()}${AppConstants.urlAgreementsSearch}?nroCliente=${nroCliente}&cantFacturasPorAcuerdo=${quantity}`
    return this._http.get<FacturasTelecomDatosModel>(_url,
      {
        headers: this.head,
        withCredentials: true
      }
    )
      .map(data => {
        let dataInvoice = data.response.GetFacturaListResponse.GetFacturaListResult.Acuerdos.Acuerdo;
        let Facturas;
        dataInvoice.forEach(element => {
          if (element.Numero == agreement) {
            //ALL
            if (voucher == 'T' || voucher == '') {
              Facturas = element.Facturas.Factura;
            } else {
              Facturas = element.Facturas.Factura.filter(factura => factura.Tipo == voucher);
            }
          }
        });
        // return Facturas as FacturasTelecomDatosModel;
        return Facturas;
      })
      .catch(this.handleError);
  };

  getPdf(nroDocumento: number, letraDocumento: string) {
    let _url = `${this.getBaseUrl()}${AppConstants.urlObtainPdfFijadatosResources}?nroDocumento=${nroDocumento}&letraDocumento=${letraDocumento}`
    return this._http.get<PdfModel>(_url,
      {
        headers: this.head,
        withCredentials: true
      }
    )
      .map(data => {
        return data.response.GetFacturaImageResponse;
      })
      .catch(this.handleError);
  };
}

````

</details>


## Personal

En el modulo de Personal puede consultar facturas o Notas de credito utilizando el filtro de Referecias de Pago, Numero de Linea y Tipo de Comprobante,tambien permite descargar el listado de la grilla a Excel/CSV/PDF.


### Listado de Componentes

| Componente | Descripción |
|--------|--------
|BillPersonalComponent|Este componente se usa para mostrar el listado facturas y Notas de crédito de Personal|


<details>
  <summary>BillPersonalComponent</summary>  

````javascript

import { Component, OnInit, OnDestroy } from '@angular/core';
import { FormGroup, FormBuilder, Validators } from '@angular/forms';
import { MatSnackBar, MatDialog, MatDialogConfig } from '@angular/material';
import { PersonalSearchServiceService } from 'src/app/services/personal-search-service.service';
import { AppConstants } from 'src/app/app.constants';
import { DomSanitizer } from '@angular/platform-browser';
import { DialogOverviewPDFComponent } from 'src/app/shared/overview-pdf/overview-pdf.component';
import { Subscription, Observable, of, combineLatest } from 'rxjs';
import { DownloadFileService } from 'src/app/services/download-file/download-file.service';
import { switchMap, map, catchError } from 'rxjs/operators';
import { ModalLoadingComponent } from 'src/app/shared/modal-waiting/modal-loading.component';
import { ToastService } from 'src/app/shared/toast/toast.service';
import * as moment from 'moment';
moment.locale('es');


enum TypeDocument {
  nc = "Nota de Crédito",
  nd = "Nota de Débito"
}

interface Downloads {
  value: string;
  viewValue: string;
}

var docType = "ciclica";

@Component({
  selector: 'app-bill-personal',
  templateUrl: './bill-personal.component.html',
  styleUrls: ['./bill-personal.component.scss']
})
export class BillPersonalComponent implements OnInit, OnDestroy {
  selectVoucher = 'ciclica';
  references: any = [];
  lines: any = [];
  invoices: any = [];
  displayedColumns: string[] = ['fechaEmision', 'tipoComprobante', 'nrofactura', 'fechaCierre', 'monto', 'Clase'];
  selectReference = 0;
  selectLine = 0;
  subscriptions = new Subscription();
  downloads: Downloads[] = [
    { value: 'excel', viewValue: 'EXCEL' },
    { value: 'csv', viewValue: 'CSV' },
    { value: 'pdf', viewValue: 'PDF' },
  ];
  vouchers = [
    { value: 'ciclica', viewValue: 'Facturas' },
    { value: 'ND', viewValue: 'Notas de Débito' },
    { value: 'NC', viewValue: 'Notas de Crédito' },
  ];
  billForm: FormGroup;
  pdf: any;
  pdfContent: any;
  invoices$ = new Observable<any>();

  constructor(
    private formBuilder: FormBuilder,
    private dialog: MatDialog,
    private downloadFileService: DownloadFileService,
    public snackBar: MatSnackBar,
    private PersonalSearchServiceService: PersonalSearchServiceService,
    private sanitizer: DomSanitizer,
    private toastService: ToastService
  ) { }

  ngOnInit() {
    this.buildForm();
    this.loadItems();
  }

  loadItems() {
    this.invoices$ =
      this.PersonalSearchServiceService.getReferences(AppConstants.clientData.NUMERO_IDENTIFICACION_CLIENTE)
        .pipe(
          switchMap(references => {
            this.selectReference = references[0].referenciaPago;
            this.references = references;
            return this.PersonalSearchServiceService.getLines(AppConstants.clientData.NUMERO_IDENTIFICACION_CLIENTE, this.selectReference);
          }),
          switchMap(lines => {
            this.selectLine = lines[0].nroLinea;
            this.lines = lines;
            return this.loadInvoices(docType, 12, this.selectReference, '')
          }),
          catchError(_ => {
            return of([])
          })
        );
  }

  buildForm() {
    this.billForm = this.formBuilder.group({
      reference: ['', [Validators.required]],
      download: ['', [Validators.required]],
      line: ['', [Validators.required]],
      voucher: ['', [Validators.required]]
    });
  }

  getDate(date) {
    return moment(date).format('MM/YYYY');
  }

  getDateCierre(invoice) {
    // Para la obtencion de la fecha de vencimiento, debo parsear YYYYMMDD cuando son las Facturas.
    let voucherSelected = this.billForm.get(['voucher']).value;
    if (voucherSelected == "ciclica") {
      let y = invoice.fechaVencimiento.substr(0, 4),
        m = invoice.fechaVencimiento.substr(4, 2),
        d = invoice.fechaVencimiento.substr(6, 2);
      let finalDate = d + '/' + m + '/' + y
      return finalDate
    } else {
      // Para la obtencion de la fecha de vencimiento, devuelvo directo el valor que ya viene parseado.
      return invoice.fechaVencimiento
    }
  }

  get typeDocument() {
    return this.billForm.get(['voucher']).value.toLowerCase() === "ciclica"
  }

  onSearch() {
    let voucherSelected = this.billForm.get(['voucher']).value
    let reference = this.billForm.get(['reference']).value

    if (voucherSelected != "ciclica") {
      docType = "NCND"
      reference = AppConstants.clientData.NUMERO_IDENTIFICACION_CLIENTE
    } else {
      docType = "ciclica"
      voucherSelected = ""
    }
    this.invoices$ = this.loadInvoices(docType, 12, reference, voucherSelected)
  }

  loadInvoices(docType, cant, reference, voucherSelected): Observable<any> {

    return combineLatest(
      [
        this.PersonalSearchServiceService.getGetInfoMobileListResource(docType, cant, reference),
        this.PersonalSearchServiceService.getLines(AppConstants.clientData.NUMERO_IDENTIFICACION_CLIENTE, this.selectReference)
      ])
      .pipe(
        map(([invoices, lines]) => {
          this.selectLine = lines[0].nroLinea;
          this.lines = lines;
          return invoices.filter(invoice => { return invoice.tipoComprobante === voucherSelected.toLowerCase() })
        }),
        map(invoices => {
          return invoices.map(invoice => {
            invoice.tipoComprobante = invoice.tipoComprobante === "" ? "Factura" : TypeDocument[invoice.tipoComprobante]
            return invoice
          })
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
    const subscription =
      this.invoices$.subscribe(invoices => {
        if (invoices.length > 0) {
          let values = this.mapFileKeys(invoices);
          let extensionFile = this.billForm.get(['download']).value.toLowerCase();
          switch (extensionFile) {
            case "excel":
              this.downloadFileService.exportAsExcelFile(values, 'Facturas de Telecom', extensionFile);
              break;
            case "csv":
              this.downloadFileService.exportAsExcelFile(values, 'Facturas de Telecom', extensionFile);
              break;
            case "pdf":
              values = this.mapFileKeys(invoices).map(value => {
                return [...Object.values(value)]
              });
              let keys = ['Fecha', 'Tipo Comprobante', 'N° Comprobante', 'Vencimiento', 'Importe'];
              this.downloadFileService.exportPDF('Facturas de Telecom', keys, values);
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
    let keys = {
      fechaEmision: 'Fecha',
      tipoComprobante: 'Tipo Comprobante',
      nrofactura: 'N° Comprobante',
      fechaVencimiento: 'Vencimiento',
      monto: 'Importe'
    }
    return items.map(invoice => {
      let newItem = { Fecha: '', [`Tipo Comprobante`]: '', [`N° Comprobante`]: '', Vencimiento: '', Importe: '' };
      Object.entries(invoice).map((item) => {
        if (keys[item[0]]) {
          newItem[`${keys[item[0]]}`] = item[0] === 'tipoComprobante' ? 'Factura' : item[1];
        }
      })
      return newItem;
    })
  }

  getPdf(element) {
    console.log("ITEM => ", element);
    let item = {
      typeDocument: this.billForm.get(['voucher']).value === 'ciclica' ? 'Factura' : TypeDocument[this.billForm.get(['voucher']).value.toLowerCase()],
      nroDocumento: this.billForm.get(['voucher']).value === 'ciclica' ? element.referenciaPago : element.nrofactura,
      importe: element.monto,
      fecha: element.fechaEmision
    }
    const dialogConfig = new MatDialogConfig();
    dialogConfig.disableClose = true;
    dialogConfig.autoFocus = true;
    dialogConfig.width = "200px";
    dialogConfig.height = "200px";
    dialogConfig.data = {
      display: true
    };
    this.dialog.open(ModalLoadingComponent, dialogConfig);
    let voucherSelected = this.billForm.get(['voucher']).value
    let docType = voucherSelected != "ciclica" ? "NCND" : "ciclica";
    const subscription =
      this.PersonalSearchServiceService.postPDF(element, docType).subscribe(data => {
        if (data.httpCode === "200") {
          this.pdf = JSON.parse(data.response.renderComprobantePdfResult.pdfBytes);
          let file = new Blob([new Uint8Array(this.pdf)], { type: 'application/pdf' });
          let fileURL = URL.createObjectURL(file);
          this.pdfContent = this.sanitizer.bypassSecurityTrustResourceUrl(fileURL);;
          this.dialog.closeAll();
          this.openModal(item);
        }
        else {
          this.toastService.message({
            title: 'Algo fallo.',
            text: 'No se pudo obtener el PDF.',
            type: 'Warning',
          });
          this.dialog.closeAll();
        }
      },
        error => {
          this.dialog.closeAll();
        });
    this.subscriptions.add(subscription);
  }

  getDialogTitle(item) {
    return `${item.typeDocument}: ${item.nroDocumento} (${item.fecha}) - ${item.importe}`;
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
</details>

### Servicios

Esta sección utiliza los siguientes servicios:

| Servicio | Descripción |
|--------|--------
|getReferences|Obtención de Referencias de Pago|
|getLines|Obtención de Números de Linea|
|getGetInfoMobileListResource|Obtención los datos del usuario de GOL / Deesktop|
|postPdf||

<details>
  <summary>PersonalSearchServiceService</summary>  

````javascript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { AppConstants } from 'src/app/app.constants';
import 'rxjs/add/operator/map';
import "rxjs/add/operator/catch";
import { GlobalServicesConfiguration } from './global-services-config';
import { ReferenciasModel } from 'src/app/models/referencias.model';
import { FacturasPersonalModel } from 'src/app/models/facturaspersonal.model';
import { LinesModel } from 'src/app/models/lines.model';
import { PdfModel } from 'src/app/models/pdf.model';

@Injectable({
  providedIn: 'root'
})
export class PersonalSearchServiceService extends GlobalServicesConfiguration {
  showBill: boolean = false;
  constructor(
    private _http: HttpClient
  ) {
    super()
  }

//Listado de Referencias
  getReferences(cuit: string) {
    let _url = `${this.getBaseUrl()}${AppConstants.urlReferencesSearch}?cuit=${cuit}`

    return this._http.get<ReferenciasModel>(_url,
      {
        headers: this.head,
        withCredentials: true
      }
    )
      .map((data: any) => {
        return data.httpCode === "200" ? data.response.mobileData : [];
      })
      .catch(this.handleError);

  };

//Listado de Número de Líneas
  getLines(cuit: string, reference: number) {
    let _url = `${this.getBaseUrl()}${AppConstants.urlReferencesSearch}?cuit=${cuit}&referenciaPago=${reference}`
    return this._http.get<LinesModel>(_url,
      {
        headers: this.head,
        withCredentials: true
      }
    )
      .map(data => {
        const results = data.response.mobileData;
        return results;
      })
      .catch(this.handleError);
  };

//Listado de Facturas y Notas de Crédito/Débito
  getGetInfoMobileListResource(tipoDocs: string, cantDocs: number, referentePago: number) {
    const _url = `${this.getBaseUrl()}${AppConstants.urlGetInfoMobileListResource}?tipoDocs=${tipoDocs}&cantDocs=${cantDocs}&referentePago=${referentePago}`
    return this._http.get<FacturasPersonalModel>(_url,
      {
        headers: this.head,
        withCredentials: true
      }
    )
      .map((data: any) => {
        return data.httpCode === "200" ? data.response.GetLastComprobantesInfoResponse.GetLastComprobantesInfoResult.documentos.documento : [];
      })
      .catch(this.handleError);
  };


  postPDF(item, type) {
    let body = {
      "renderComprobantePdf": {
        "database": type,
        "doc": {
          "referenciaPago": item.referenciaPago,
          "nrofactura": item.nrofactura,
          "fechaEmision": item.fechaEmision,
          "totalPaginas": item.totalPaginas,
          "type": item.type,
          "file": item.file,
          "offset": item.offset,
          "nroPagInicial": 1,
          "nroPagFinal": item.totalPaginas > 10 ? 10 : item.totalPaginas,
          "tipoComprobante": item.tipoComprobante === "Factura" ? "" : item.tipoComprobante === "Nota de Débito" ? 'nd' : 'nc',
          "letra": item.letra
        }
      }
    }
    let _url = `${this.getBaseUrl()}${AppConstants.urlObtainPdfMobileResources}`;
    return this._http.post<any>(_url, body,
      {
        headers: this.head,
        withCredentials: true,
      });
  }

}

````
</details>

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
