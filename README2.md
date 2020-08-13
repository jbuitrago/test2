# Gestiones M贸viles

La versi贸n usada para este proyecto es Angular 8 (https://angular.io/).

###  馃捇 Instalaci贸n

Dependiendo del administrador de dependencias (***NPM (https://www.npmjs.com/)*** o ***YARN (https://yarnpkg.com/)***)

```
npm install - yarn install
```


### 馃挕 Para poder ejecutar el proyecto 

Dependiendo del administrador de dependencias (***NPM (https://www.npmjs.com/)*** o ***YARN (https://yarnpkg.com/)***)

```
npm run start - yarn start
```

### 馃挕 Crear empaquetado para deployar una nueva versi贸n

```
npm run build-prod - yarn run build-prod  
```


### 馃搶 Arquitectura del proyecto

La documentaci贸n de como esta compuesto el proyecto puede verse de manera local usando Compodoc, para esto se debe ejecutar el siguiente comando ***npm run doc***, en la consola de comandos se mostrar谩 que se debe abrir la documentaci贸n en el siguiente puerto => https://localhost:5001. 

. La estructura del proyecto esta compuesta de la siguiente manera:

- SharedModule
- ServiceModule
- ManagementsModule
- ManagerModule
- AdministratorModule
- ModalModule
- ZonesModule
- CountriesModule

##### SharedModule

Dentro de este m贸dulo se encuentran todos los componentes que son usados en la aplicaci贸n.

| Component | Descripci贸n |
|--------|--------|
|CardInfoComponent|Este componente se usa para mostrar cartas flotantes, compuestos con un t铆tulo y una descripci贸n|
|IconOptionComponent|Este componente se usa para mostrar opciones con iconos|
|NavBarComponent|Este componente es el barra de navegaci贸n usada entre las pantallas de la aplicaci贸n|
|DrodpownComponent|Este componente se usa para poder listar elementos con un formato gen茅rico y tambi茅n poder seleccionarlos|
|TabItemsComponent|Este componente se usa para los tabs usados en las distintas gestiones|
|ListItemComponent|Este componente se usa para listar valores simples|
|MessageComponent|Este componente se usa para mostrar un mensaje informativo con un icono que indica la magnitud del mensaje|
|LoadingComponent|Este componente se usa para mostrar un cargador|
|SpinnerComponent|Este componente se usa para mostar un logo de carga mientras se espera alguna acci贸n|
|InputSearchComponent|Este componente se usa para buscar segun un criterio de busqueda|
|LoadingMessageComponent|Este componente se usa en los modales para mostrar un mensaje mientras se espera|
|PaginationComponent|Este componente se usa para gestionar la paginaci贸n de un bloque de datos|
|CheckBoxComponent|Este componente se usa para seleccionar o no elemeentos|
|LinesComponent|Este componente se usa para listar numeros de telefono en los modales|

##### Composici贸n

````
import { NgModule, CUSTOM_ELEMENTS_SCHEMA } from "@angular/core";
import { CommonModule } from '@angular/common';
import { FormsModule } from '@angular/forms';
import { COMPONENTS } from './shared';

@NgModule({
  declarations: [...COMPONENTS],
  imports: [CommonModule, FormsModule],
  providers: [],
  exports: [...COMPONENTS],
  bootstrap: [...COMPONENTS],
  schemas: [CUSTOM_ELEMENTS_SCHEMA]
})
export class SharedModule {}

````

##### Como usarlo

Dependiendo de donde se quiera usar, se debe importar el m贸dulo SharedModule. Por ejemplo en el siguiente ejemplo, se importa el m贸dulo de SharedModule en el m贸dulo ***CountriesModule***.

````
import { NgModule, CUSTOM_ELEMENTS_SCHEMA } from "@angular/core";
import { NgxPermissionsModule } from "ngx-permissions";
import { ReactiveFormsModule, FormsModule } from "@angular/forms";
import { CountriesComponent } from "./countries.component";
import { CountriesRoutingModule } from "./countries-routing.module";
import { HttpClientModule } from "@angular/common/http";
import { CommonModule } from "@angular/common";
import { SharedModule } from "@app/shared/shared.module";
import { ModalModule } from "@app/shared/modals/modal.module";

@NgModule({
  declarations: [CountriesComponent],
  imports: [
    HttpClientModule,
    FormsModule,
    NgxPermissionsModule,
    ReactiveFormsModule,
    CountriesRoutingModule,
    CommonModule,
    FormsModule,
    SharedModule,
    ModalModule
  ],
  providers: [],
  exports: [],
  bootstrap: [],
  schemas: [CUSTOM_ELEMENTS_SCHEMA]
})
export class CountriesModule {}

````

#### ServiceModule

Dentro de este m贸dulo se encuentran todos los servicios que son usados en la aplicaci贸n.

| Servicio | Descripci贸n |
|--------|--------|
|StandardUserOptionService|Este servicio contiene las opciones de la aplicaci贸n para las gestiones|
|CountriesService|Este servicio se usa para gestionar los pa铆ses usados en la aplicaci贸n|
|StandardUserOtherOptionService|Este servicio contiene las otras opciones de la aplicacion para las gestiones|
|ManagementsService|Este servicio se usa para consumir las gestiones realizadas de la aplicaci贸n|
|PackService|Este servicio se usa para gestionar los paquetes usados en la aplicaci贸n|
|PlanService|Este servicio se usa para gestionar los planes usados en la aplicaci贸n|
|TelephoneNumberService|Este servicio se usa para consumir las l铆neas telef贸nicas del usuario logueado|
|UserService|Este servicio se usa para consumir datos del usuario logueado|
|TypeFieldService|Este servicio se usa para gestionar los tipos de campos de los formularios|
|FieldService|Este servicio se usa para gestionar los campos de los formularios|
|TransactionsService|Este servicio se usa para gestionar las transacciones realizadas en la aplicaci贸n|
|DownloadFileService|Este servicio se usa para descargar los distintos tipos de reportes|
|InstalledBaseService|Este servicio se usa para obtener usuarios dependiendo a un listado de criterios|
|EnvironmentService|Este servicio se usa para verificar en que entorno se esta ejecutando la aplicaci贸n|
|EloquaService|Este servicio se usa para la gesti贸n del env铆o de mails por eloqua|
|QuotaService|Este servicio se usa para gestionar las cuotas usados en la aplicaci贸n|
|DatesService|Este servicio contiene metodos que dan distintos formatos fechas usados en la aplicaci贸n|

##### Composici贸n

````
import { NgModule, CUSTOM_ELEMENTS_SCHEMA } from "@angular/core";
import { CommonModule } from "@angular/common";
import { HttpClientModule, HTTP_INTERCEPTORS } from "@angular/common/http";
import { GlobalHttpInterceptorService } from "./interceptor/global-interceptor-http.interceptor";
import { SERVICES } from './services';

@NgModule({
  declarations: [],
  imports: [CommonModule, HttpClientModule],
  providers: [
    ...SERVICES,
    {
      provide: HTTP_INTERCEPTORS,
      useClass: GlobalHttpInterceptorService,
      multi: true
    }
  ],
  exports: [],
  bootstrap: [],
  schemas: [CUSTOM_ELEMENTS_SCHEMA]
})
export class ServicesModule {}

````

##### Como usarlo

Este m贸dulo esta importando en app.module.ts

````
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { ManagerModule } from './views/manager/manager.module';
import { AdministratorModule } from './views/manager/administrator/administrator.module';
import { AppRoutingModule } from './app-routing.module';
import { BrowserAnimationsModule } from '@angular/platform-browser/animations';
import { environment } from '../environments/environment';
import { NgxPermissionsModule } from 'ngx-permissions';
import { ModalModule } from './shared/modals/modal.module';
import { ServicesModule } from './services/services.module'; 
import { COMPONENTS } from './app';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [...COMPONENTS],
  imports: [
    BrowserModule, 
    ManagerModule,
    AdministratorModule,
    ModalModule,
    AppRoutingModule,
    BrowserAnimationsModule,
    ServicesModule,
    NgxPermissionsModule.forRoot(),
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule {}


````

#### ManagementsModule

Dentro de este m贸dulo se encuentran que componentes y modulos son usados dentro del m贸dulo de ***Managements***.

| Components | Descripci贸n |
|--------|--------|
|ManagamentComponent|Este componente contiene todo lo referente a las gestiones del usuario|

| M贸dulos | Descripci贸n |
|--------|--------|
|MatDatepickerModule|Este es un m贸dulo de date picker propio de Angular Material|
|MatNativeDateModule|Este es un m贸dulo de date propio de Angular Material|
|MatInputModule|Este es un m贸dulo de input propio de Angular Material|
|MatFormFieldModule|Este es un m贸dulo de formularios propio de Angular Material|
|MatSelectModule|Este es un m贸dulo de selector propio de Angular Material|
|MatSnackBarModule|Este es un m贸dulo de snackBar propio de Angular Material|
|SharedModule|Este es el m贸dulo de los componentes de la aplicaci贸n|
|ModalModule|Este es el m贸dulo contienen los modales usados de la aplicaci贸n|
  

##### Composici贸n

````
import { NgModule, CUSTOM_ELEMENTS_SCHEMA } from "@angular/core";
import { CommonModule } from "@angular/common";
import { ModalModule } from "@app/shared/modals/modal.module";
import { HttpClientModule } from "@angular/common/http";
import { NgxPermissionsModule } from "ngx-permissions";
import { SharedModule } from "@app/shared/shared.module";
import { ReactiveFormsModule } from "@angular/forms";
import { ManagementRoutingModule } from "./managements-routing.component";
import { COMPONENTS } from "./managements";
import {
  MatDatepickerModule,
  MatNativeDateModule,
  MatInputModule,
  MatFormFieldModule,
  MatSelectModule,
  MatSnackBarModule,
  MAT_DATE_LOCALE,
} from "@angular/material";

const MODULES = [
  CommonModule,
  SharedModule,
  ModalModule,
  ManagementRoutingModule,
  HttpClientModule,
  NgxPermissionsModule,
  ReactiveFormsModule,
];

const MATERIAL_MODULES = [
  MatDatepickerModule,
  MatNativeDateModule,
  MatInputModule,
  MatSelectModule,
  MatFormFieldModule,
  MatSnackBarModule,
];

@NgModule({
  declarations: [...COMPONENTS],
  imports: [...MODULES, ...MATERIAL_MODULES],
  providers: [
    ...MATERIAL_MODULES,
    { provide: MAT_DATE_LOCALE, useValue: "es-ES" },
  ],
  exports: [...COMPONENTS],
  bootstrap: [...COMPONENTS],
  schemas: [CUSTOM_ELEMENTS_SCHEMA],
})
export class ManagementsModule {}

````

##### Como usarlo

Este m贸dulo esta agregado en el m贸dulo de Manager.

````
import { NgModule, CUSTOM_ELEMENTS_SCHEMA } from "@angular/core";
import { ManagerRoutingModule } from "./manager-routing.module";
import { CommonModule } from "@angular/common";
import { SharedModule } from "../../shared/shared.module";
import { ModalModule } from "@app/shared/modals/modal.module";
import { HttpClientModule } from "@angular/common/http";
import { NgxPermissionsModule } from "ngx-permissions";
import { ReactiveFormsModule } from "@angular/forms";
import { COMPONENTS } from "./manager";
import { FilterNumbersPipe } from "@app/pipes/filter-number.filter";
import {
  MatInputModule,
  MatSelectModule,
  MatFormFieldModule,
  MatDatepickerModule,
  MatNativeDateModule,
  MatSnackBarModule,
  MAT_DATE_LOCALE,
} from "@angular/material";
import { ManagementsModule } from "./managements/management.module";

const MATERIAL_MODULES = [
  MatDatepickerModule,
  MatNativeDateModule,
  MatInputModule,
  MatSelectModule,
  MatFormFieldModule,
  MatSnackBarModule,
];

const _MODULES = [
  CommonModule,
  ManagerRoutingModule,
  SharedModule,
  ModalModule,
  HttpClientModule,
  NgxPermissionsModule,
  ReactiveFormsModule,
  ManagementsModule,
];

@NgModule({
  declarations: [...COMPONENTS, FilterNumbersPipe],
  imports: [..._MODULES, ...MATERIAL_MODULES],
  providers: [
    ...MATERIAL_MODULES,
    { provide: MAT_DATE_LOCALE, useValue: "es-ES" },
  ],
  exports: [],
  bootstrap: [],
  schemas: [CUSTOM_ELEMENTS_SCHEMA],
})
export class ManagerModule {}

````

#### ManagerModule

Dentro de este m贸dulo se encuentran que componentes y modulos son usados dentro del m贸dulo de ***Managements***.

| Components | Descripci贸n |
|--------|--------|
|ManagerComponent|Este componente contiene todo lo referente a la pantalla principal y sus secciones del usuario loqueado en la aplicaci贸n|
|ChangePlanComponent|Este componente contiene todo lo referente a la secci贸n de cambio de plan|
|RoamingDDIComponent|Este componente contiene todo lo referente a la secci贸n de RoamingDDI|
|QuotaRenewalComponent|Este componente contiene todo lo referente a la secci贸n de renovaci贸n de cuota|
|FibercorpSearchComponent|Este componente es el buscador de usuarios usado para la versi贸n Desktop de Gestiones M贸viles|


| M贸dulos | Descripci贸n |
|--------|--------|
|MatDatepickerModule|Este es un m贸dulo de date picker propio de Angular Material|
|MatNativeDateModule|Este es un m贸dulo de date propio de Angular Material|
|MatInputModule|Este es un m贸dulo de input propio de Angular Material|
|MatFormFieldModule|Este es un m贸dulo de formularios propio de Angular Material|
|MatSelectModule|Este es un m贸dulo de selector propio de Angular Material|
|MatSnackBarModule|Este es un m贸dulo de snackBar propio de Angular Material|
|SharedModule|Este es el m贸dulo de los componentes de la aplicaci贸n|
|ModalModule|Este es el m贸dulo contienen los modales usados de la aplicaci贸n|

##### Composici贸n

````
import { NgModule, CUSTOM_ELEMENTS_SCHEMA } from "@angular/core";
import { ManagerRoutingModule } from "./manager-routing.module";
import { CommonModule } from "@angular/common";
import { SharedModule } from "../../shared/shared.module";
import { ModalModule } from "@app/shared/modals/modal.module";
import { HttpClientModule } from "@angular/common/http";
import { NgxPermissionsModule } from "ngx-permissions";
import { ReactiveFormsModule } from "@angular/forms";
import { COMPONENTS } from "./manager";
import { FilterNumbersPipe } from "@app/pipes/filter-number.filter";
import {
  MatInputModule,
  MatSelectModule,
  MatFormFieldModule,
  MatDatepickerModule,
  MatNativeDateModule,
  MatSnackBarModule,
  MAT_DATE_LOCALE,
} from "@angular/material";
import { ManagementsModule } from "./managements/management.module";

const MATERIAL_MODULES = [
  MatDatepickerModule,
  MatNativeDateModule,
  MatInputModule,
  MatSelectModule,
  MatFormFieldModule,
  MatSnackBarModule,
];

const _MODULES = [
  CommonModule,
  ManagerRoutingModule,
  SharedModule,
  ModalModule,
  HttpClientModule,
  NgxPermissionsModule,
  ReactiveFormsModule,
  ManagementsModule,
];

@NgModule({
  declarations: [...COMPONENTS, FilterNumbersPipe],
  imports: [..._MODULES, ...MATERIAL_MODULES],
  providers: [
    ...MATERIAL_MODULES,
    { provide: MAT_DATE_LOCALE, useValue: "es-ES" },
  ],
  exports: [],
  bootstrap: [],
  schemas: [CUSTOM_ELEMENTS_SCHEMA],
})
export class ManagerModule {}

````
##### Como usarlo

Este m贸dulo esta inyectado mediante lazy loading en el modulo de routing de manager.

````
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { ErrorPageComponent } from './views/manager/error-page/error-page.component';
 
const appRoutes: Routes = [
    { path: '', redirectTo: '/error-page', pathMatch: 'full' },
    { path: 'error-page', component: ErrorPageComponent },
    {
      path: 'gestion',
      loadChildren: () =>
        import('./views/manager/manager.module').then(mod => mod.ManagerModule)
    } 
];

@NgModule({
  imports: [RouterModule.forRoot(appRoutes, { useHash: true })],
  exports: [RouterModule]
})
export class AppRoutingModule {}

````

#### AdministratorModule

Dentro de este m贸dulo se encuentra todo lo referente al perfil de Administrador que tiene Gestiones M贸viles.

| Components | Descripci贸n |
|--------|--------|
|AdministratorComponent|Este componente contiene todo lo referente a la secci贸n de Administrador y sus acciones|
|PlansComponent|Este componente tiene todo lo referente a la gesti贸n de los planes que se van a usar en la aplicaci贸n|
|PacksComponent|Este componente tiene todo lo referente a la gesti贸n de los paquetes que se van a usar en la aplicaci贸n|
|QuotasComponent|Este componente tiene todo lo referente a la gesti贸n de las cuotas que se van a usar en la aplicaci贸n|

| M贸dulos | Descripci贸n |
|--------|--------|
|MatDatepickerModule|Este es un m贸dulo de date picker propio de Angular Material|
|MatNativeDateModule|Este es un m贸dulo de date propio de Angular Material|
|MatInputModule|Este es un m贸dulo de input propio de Angular Material|
|MatFormFieldModule|Este es un m贸dulo de formularios propio de Angular Material|
|MatSelectModule|Este es un m贸dulo de selector propio de Angular Material|
|MatSnackBarModule|Este es un m贸dulo de snackBar propio de Angular Material|
|SharedModule|Este es el m贸dulo de los componentes de la aplicaci贸n|
|ModalModule|Este es el m贸dulo contienen los modales usados de la aplicaci贸n|
 
##### Composici贸n

````
import { NgModule, CUSTOM_ELEMENTS_SCHEMA } from "@angular/core";
import { CommonModule } from "@angular/common";
import { MatDatepickerModule } from "@angular/material/datepicker";
import { MatNativeDateModule } from "@angular/material/core";
import { MatButtonModule } from "@angular/material/button";
import { MatInputModule } from "@angular/material/input";
import { ModalModule } from "@app/shared/modals/modal.module";
import { HttpClientModule } from "@angular/common/http";
import { NgxPermissionsModule } from "ngx-permissions";
import { SharedModule } from "@app/shared/shared.module";
import { AdministratorRoutingModule } from "./administrator-routing.module";
import { ReactiveFormsModule } from "@angular/forms";
import { COMPONENTS } from "./administrator";
import { DIRECTIVES } from "@app/directives/directives";

@NgModule({
  declarations: [...COMPONENTS, ...DIRECTIVES],
  imports: [
    CommonModule,
    AdministratorRoutingModule,
    ModalModule,
    SharedModule,
    MatDatepickerModule,
    MatButtonModule,
    MatInputModule,
    MatNativeDateModule,
    HttpClientModule,
    NgxPermissionsModule,
    ReactiveFormsModule,
  ],
  providers: [MatDatepickerModule, MatInputModule, MatButtonModule],
  exports: [],
  bootstrap: [],
  schemas: [CUSTOM_ELEMENTS_SCHEMA],
})
export class AdministratorModule {}

````

##### Como usarlo

Debido a que este m贸dulo es core, se lo importa en AppModule.

````
 import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { ManagerModule } from './views/manager/manager.module';
import { AdministratorModule } from './views/manager/administrator/administrator.module';
import { AppRoutingModule } from './app-routing.module';
import { BrowserAnimationsModule } from '@angular/platform-browser/animations';
import { StoreModule } from '@ngrx/store';
import { StoreDevtoolsModule } from '@ngrx/store-devtools';
import { environment } from '../environments/environment';
import { planReducer } from './store/reducers/plan.reducer';
import { counterReducer } from './store/reducers/count.reducer';
import { telephoneNumberReducer } from './store/reducers/telephone-number.reducer';
import { NgxPermissionsModule } from 'ngx-permissions';
import { ModalModule } from './shared/modals/modal.module';
import { ServicesModule } from './services/services.module'; 
import { COMPONENTS } from './app';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [...COMPONENTS],
  imports: [
    BrowserModule, 
    ManagerModule,
    AdministratorModule,
    ModalModule,
    AppRoutingModule,
    BrowserAnimationsModule,
    ServicesModule,
    NgxPermissionsModule.forRoot(),
    StoreModule.forRoot({
      count: counterReducer,
      plans: planReducer,
      telephones: telephoneNumberReducer
    }),
    StoreDevtoolsModule.instrument({
      maxAge: 25,
      logOnly: environment.production
    })
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule {}

````

#### ModalModule

Con este m贸dulo se puede levantar un modal personalizado en cualqueir parte de la aplicaci贸n.

| Components | Descripci贸n |
|--------|--------|
|ModalComponent|Este componente tiene los decoradores y emisores de mensajes necesarios para usar el ModalComponent|
 

##### Composici贸n

````
import { NgModule, CUSTOM_ELEMENTS_SCHEMA } from "@angular/core";
import { ModalComponent } from "./modal/modal.component";
import { CommonModule } from "@angular/common";
import { FormsModule } from "@angular/forms";
@NgModule({
  declarations: [ModalComponent],
  imports: [CommonModule, FormsModule],
  providers: [],
  exports: [ModalComponent],
  bootstrap: [ModalComponent],
  schemas: [CUSTOM_ELEMENTS_SCHEMA]
})
export class ModalModule {}
````
##### Como usarlo

Para poder usar este modal, se debe importar su m贸dulo donde se lo quiera usar. Como por ejemplo en AdministratorModule.

````
import { NgModule, CUSTOM_ELEMENTS_SCHEMA } from "@angular/core";
import { CommonModule } from "@angular/common";
import { MatDatepickerModule } from "@angular/material/datepicker";
import { MatNativeDateModule } from "@angular/material/core";
import { MatButtonModule } from "@angular/material/button";
import { MatInputModule } from "@angular/material/input";
import { ModalModule } from "@app/shared/modals/modal.module";
import { HttpClientModule } from "@angular/common/http";
import { NgxPermissionsModule } from "ngx-permissions";
import { SharedModule } from "@app/shared/shared.module";
import { AdministratorRoutingModule } from "./administrator-routing.module";
import { ReactiveFormsModule } from "@angular/forms";
import { COMPONENTS } from "./administrator";
import { DIRECTIVES } from "@app/directives/directives";

@NgModule({
  declarations: [...COMPONENTS, ...DIRECTIVES],
  imports: [
    CommonModule,
    AdministratorRoutingModule,
    ModalModule,
    SharedModule,
    MatDatepickerModule,
    MatButtonModule,
    MatInputModule,
    MatNativeDateModule,
    HttpClientModule,
    NgxPermissionsModule,
    ReactiveFormsModule,
  ],
  providers: [MatDatepickerModule, MatInputModule, MatButtonModule],
  exports: [],
  bootstrap: [],
  schemas: [CUSTOM_ELEMENTS_SCHEMA],
})
export class AdministratorModule {}

````

#### ZonesModule

Dentro de este m贸dulo se encuentra todo lo referente a la gesti贸n de zonas usadas en las gestiones.

| Components | Descripci贸n |
|--------|--------|
|ZonesComponent|Este componente contiene todo lo referente a la gesti贸n de zonas usadas en la aplicaci贸n|


##### Composici贸n

````
import { NgModule, CUSTOM_ELEMENTS_SCHEMA } from "@angular/core";
import { HttpClientModule } from "@angular/common/http";
import { NgxPermissionsModule } from "ngx-permissions";
import { ReactiveFormsModule, FormsModule } from "@angular/forms";
import { ZonesComponent } from "./zones.component";
import { ZonesRoutingModule } from "./zones-routing.module";
import { SharedModule } from "@app/shared/shared.module";
import { CommonModule } from "@angular/common";
import { ModalModule } from "@app/shared/modals/modal.module";

const MODULES = [
  HttpClientModule,
  NgxPermissionsModule,
  ReactiveFormsModule,
  ZonesRoutingModule,
  CommonModule,
  FormsModule,
  SharedModule,
  ModalModule
];

@NgModule({
  declarations: [ZonesComponent],
  imports: [
    ...MODULES
  ],
  providers: [],
  exports: [],
  bootstrap: [],
  schemas: [CUSTOM_ELEMENTS_SCHEMA]
})
export class ZonesModule {}

````

##### Como usarlo

Este m贸dulo se inyecta mediante Lazy loading, en el archivo de routing de RegionsRoutingModule.

````
import { NgModule } from "@angular/core";
import { RouterModule, Routes } from "@angular/router";
import { RegionsComponent } from "./regions.component";

const ROUTES: Routes = [
  { path: "", redirectTo: "regions" },
  {
    path: "regions",
    component: RegionsComponent,
    children: [
      { path: "", redirectTo: "zones", pathMatch: "full" },
      {
        path: "zones",
        loadChildren: () =>
          import("./sections/zones/zones.module").then(mod => mod.ZonesModule)
      },
      {
        path: "countries",
        loadChildren: () =>
          import("./sections/countries/countries.module").then(
            mod => mod.CountriesModule
          )
      }
    ]
  }
];

@NgModule({
  imports: [RouterModule.forChild(ROUTES)],
  exports: [RouterModule]
})
export class RegionsRoutingModule {}

````

#### CountriesModule

Dentro de este m贸dulo se encuentra todo lo referente a la gesti贸n de pa铆ses usadas en las gestiones.

| Components | Descripci贸n |
|--------|--------|
|CountriesComponent|Este componente contiene todo lo referente a la gesti贸n de pa铆ses usadas en la aplicaci贸n|

##### Composici贸n

````
import { NgModule, CUSTOM_ELEMENTS_SCHEMA } from "@angular/core";
import { NgxPermissionsModule } from "ngx-permissions";
import { ReactiveFormsModule, FormsModule } from "@angular/forms";
import { CountriesComponent } from "./countries.component";
import { CountriesRoutingModule } from "./countries-routing.module";
import { HttpClientModule } from "@angular/common/http";
import { CommonModule } from "@angular/common";
import { SharedModule } from "@app/shared/shared.module";
import { ModalModule } from "@app/shared/modals/modal.module";

@NgModule({
  declarations: [CountriesComponent],
  imports: [
    HttpClientModule,
    FormsModule,
    NgxPermissionsModule,
    ReactiveFormsModule,
    CountriesRoutingModule,
    CommonModule,
    FormsModule,
    SharedModule,
    ModalModule
  ],
  providers: [],
  exports: [],
  bootstrap: [],
  schemas: [CUSTOM_ELEMENTS_SCHEMA]
})
export class CountriesModule {}

````

##### Como usarlo

Este m贸dulo se inyecta mediante Lazy loading, en el archivo de routing de RegionsRoutingModule.

````
import { NgModule } from "@angular/core";
import { RouterModule, Routes } from "@angular/router";
import { RegionsComponent } from "./regions.component";

const ROUTES: Routes = [
  { path: "", redirectTo: "regions" },
  {
    path: "regions",
    component: RegionsComponent,
    children: [
      { path: "", redirectTo: "zones", pathMatch: "full" },
      {
        path: "zones",
        loadChildren: () =>
          import("./sections/zones/zones.module").then(mod => mod.ZonesModule)
      },
      {
        path: "countries",
        loadChildren: () =>
          import("./sections/countries/countries.module").then(
            mod => mod.CountriesModule
          )
      }
    ]
  }
];

@NgModule({
  imports: [RouterModule.forChild(ROUTES)],
  exports: [RouterModule]
})
export class RegionsRoutingModule {}

````

### 馃殌 Estructura del servicio de Eloqua

Se implement贸 el servicio **EloquaService**, que contiene los m茅todos asociados al env铆o del formulario de Eloqua. El siguiente c贸digo muestra el contenido del servicio:


    export class EloquaService {
      constructor(
        private http: HttpClient,
        private headerService: HeaderService,
        private environmentService: EnvironmentService
      ) {}
    
      sendMail(payload): Observable<any> {
        return this.http.post<Observable<any>>(
          this.getUrl(),
          this.getFormData(payload),
          {
            headers: this.headerService.header(),
          }
        );
      }
    
      getUrl() {
        return this.environmentService
          .getEnvironment()
          .value.env.includes("gestiondesa")
          ? URL_DESA
          : URL_DESKTOP;
      }
    
      getFormData(payload) {
        let formData = new FormData();
        Object.keys(payload).forEach(function (item) {
          formData.append(item, payload[item]);
        });
        return formData;
      }
    }
    


Los metodos asociados a este servicio son los siguientes:
- **sendMail**: es el m茅todo qu茅 env铆a el payload de Eloqua, con el formato de FormData. Se usa el m茅todo de peticion **POST**.
- **getUrl**: este m茅todo se usa para obtener la url base dependiendo, dado que puede ser DesktopDesa o GestionDesa.
-**getFormData**: con este m茅todo se le da formato a los atributos del payload que se quiere enviar. Devuelve el payload en formato **FormData**.


### 馃殌 Uso del servicio de EloquaService en los componentes

1 - Para poder usar el servicio se lo debe importar en el componente.

`import { EloquaService } from "@app/services/eloqua/eloqua.service";`

2 - Se debe declarar el servicio en el constructor, para poder usarlo en el contexto del componente.

` constructor( private eloquaService: EloquaService) {}`

3 - En Gestion M贸viles se tiene 3 tipos de gestiones, como as铆 tambien 3 formatos de payload que se manda al Servicio de Eloqua. 

&#128276; En el caso de **Cambio de plan**, el formato del payload debe ser el siguiente

````javascript
payload = {
	TemplateId: TEMPLATE_ID,
	SendFromUserId: userID,
	UserEmail: userEmail,
	FormName: formName,
	ECP_GSF_Plan: planName,
	ECP_GSF_Linea: planLines,
	debug: true,
};
this.eloquaService.sendMail(payload);
````

Los atributos asociados al payload son los siguientes:
- TemplateId: es el ID asociado al template que se usa para mostrar la data que se env铆a en el payload.
- SendFromUserId: es el identificador del usuario que esta realizando la gesti贸n.
- UserEmail: es el email del usuario que esta realizando la gesti贸n.
- FormName: es el nombre del formulario del cambio de plan.
- ECP_GSF_Plan: es el nombre del plan.
- ECP_GSF_Linea: son las lineas asociadas al plan.
- debug(opcional): este parametro es opcional.


&#128276; En el caso de **Renovaci贸n de cuota**, el formato del payload debe ser el siguiente

````javascript
payload = {
	TemplateId: TEMPLATE_ID,
	SendFromUserId: userID,
	UserEmail: userEmail,
	FormName: formName,
	ECP_GSF_Cuota: quotaName,
	ECP_GSF_Linea: quotaLines,
	debug: true,
};
this.eloquaService.sendMail(payload);
````
Los atributos asociados al payload son los siguientes:
- TemplateId: es el ID asociado al template que se usa para mostrar la data que se env铆a en el payload.
- SendFromUserId: es el identificador del usuario que esta realizando la gesti贸n.
- UserEmail: es el email del usuario que esta realizando la gesti贸n.
- FormName: es el nombre del formulario de la renovaci贸n de cuota.
- ECP_GSF_Cuota: es el nombre de la renovaci贸n de cuota.
- ECP_GSF_Linea: son las lineas asociadas a la renovaci贸n de cuota.
- debug(opcional): este parametro es opcional.

&#128276; En el caso de **Roaming/DDI**, el formato del payload debe ser el siguiente

````javascript 
payload = {
	TemplateId: TEMPLATE_ID,
	SendFromUserId: userID,
	UserEmail: userEmail,
	FormName: formName,
	ECP_GSF_Pack: packName,
	ECP_GSF_Consumo: consumptionName,
	ECP_GSF_Fecha: date,
	ECP_GSF_Duracion: duration,
	ECP_GSF_Servicio: serviceNAme,
	ECP_GSF_Linea: roamingLines,
	debug: true,
};
this.eloquaService.sendMail(payload)
````
Los atributos asociados al payload son los siguientes:
- TemplateId: es el ID asociado al template que se usa para mostrar la data que se env铆a en el payload.
- SendFromUserId: es el identificador del usuario que esta realizando la gesti贸n.
- UserEmail: es el email del usuario que esta realizando la gesti贸n.
- FormName: es el nombre del formulario de la renovaci贸n de cuota.
- ECP_GSF_Pack: es el nombre del pack seleccionado.
- ECP_GSF_Consumo: es el nombre del consumo seleccionado.
- ECP_GSF_Fecha: es la fecha seleccionada.
- ECP_GSF_Duracion: es la duraci贸n seleccionada.
- ECP_GSF_Sercicio: es el nombre del servicio seleccionado.
- ECP_GSF_Linea: son las l铆neas seleccionadas del Roaming.
- debug(opcional): este parametro es opcional.
 
