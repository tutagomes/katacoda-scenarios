Agora, vamos aproveitar o embalo e já configurar todas as nossas rotas!

{{open angular-crud/src/app/clientes/clientes-routing.module.ts}}

```ts

  import { NgModule } from '@angular/core';
  import { RouterModule, Routes } from '@angular/router';

  import { HomeComponent } from './home/home.component';
  import { DetailComponent } from './detail/detail.component';
  import { CreateComponent } from './create/create.component';
  import { UpdateComponent } from './update/update.component';

  const routes: Routes = [
    { path: 'clientes', redirectTo: 'clientes/home', pathMatch: 'full'},
    { path: 'clientes/home', component: HomeComponent },
    { path: 'clientes/detail/:clienteId', component: DetailComponent },
    { path: 'clientes/create', component: CreateComponent },
    { path: 'clientes/update/:clienteId', component: UpdateComponent } 
  ];

  @NgModule({
    imports: [RouterModule.forChild(routes)],
    exports: [RouterModule]
  })
  export class ClientesRoutingModule { }

```{{copy}}


E aproveitar também e já configurar o nosso ambiente:

{{open angular-crud/src/environments/environment.ts}}

> Para facilitar: https://[[HOST_SUBDOMAIN]]-3000-[[KATACODA_HOST]].environments.katacoda.com

```ts
// This file can be replaced during build by using the `fileReplacements` array.
// `ng build --prod` replaces `environment.ts` with `environment.prod.ts`.
// The list of file replacements can be found in `angular.json`.

export const environment = {
  production: false,
  apiUrl: '<URL do json-server no katacoda>'
};

/*
 * For easier debugging in development mode, you can import the following file
 * to ignore zone related error stack frames such as `zone.run`, `zoneDelegate.invokeTask`.
 *
 * This import should be commented out in production mode because it will have a negative impact
 * on performance if an error is thrown.
 */
// import 'zone.js/dist/zone-error';  // Included with Angular CLI.

```{{copy}}