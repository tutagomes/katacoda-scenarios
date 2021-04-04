Agora, vamos injetar nosso módulo de clientes no nosso módulo raiz do Angular, para que ele possa ser utilizado na nossa aplicação:


{{open angular-crud/src/app/app.module.ts}}

```ts
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { ClientesModule } from './clientes/clientes.module';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    ClientesModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }


```{{copy}}

