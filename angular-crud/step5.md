Pronto, agora vamos à parte que interessa! A programação!

Vamos começar com nosso primeiro arquivo, o módulo de clientes. Ele será responsável por centralizar todo nosso subsistema.

`angular-crud/src/app/clientes/cliente.module.ts`{{open}}

```ts

import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';

import { ClientesRoutingModule } from './clientes-routing.module';
import { HomeComponent } from './home/home.component';
import { DetailComponent } from './detail/detail.component';
import { CreateComponent } from './create/create.component';
import { UpdateComponent } from './update/update.component';

import { FormsModule, ReactiveFormsModule } from '@angular/forms';
import { HttpClientModule } from '@angular/common/http';


@NgModule({
  declarations: [
    HomeComponent,
    DetailComponent,
    CreateComponent,
    UpdateComponent
  ],
  imports: [
    CommonModule,
    ClientesRoutingModule,
    HttpClientModule,
    FormsModule,
    ReactiveFormsModule
  ]
})
export class ClientesModule { }

```{{copy}}