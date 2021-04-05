Agora, vamos começar a realmente implementar um código "personalizado". Iniciando pela nossa interface de Cliente, que será responsável por definir os campos e propriedades das nossas entidades:

`angular-crud/src/app/clientes/client.ts`{{open}}

```ts

export interface Cliente {
    id: number;
    first_name: string;
    last_name: string;
    email: string;
    street: string;
    number: string;
    zipcode: string;
    complement: string;
}
 
```{{copy}}

E vamos aproveitar e já implementar todo nosso serviço de comunicação com o nosso backend


`angular-crud/src/app/clientes/cliente.service.ts`{{open}}

src/app/clientes/cliente.service.ts

```ts

import { Injectable } from '@angular/core';
import { environment } from '../../environments/environment'
import { HttpClient, HttpHeaders } from "@angular/common/http";
import {  Observable, throwError } from 'rxjs';
import { catchError } from 'rxjs/operators';
import { Cliente } from './cliente';

@Injectable({
  providedIn: 'root'
})
export class ClienteService {

  private apiServer = environment.apiUrl;

  httpOptions = {
    headers: new HttpHeaders({
      'Content-Type': 'application/json'
    })
  }

  constructor (private httpClient: HttpClient) { }

  create(cliente: Cliente): Observable<Cliente> {
    return this.httpClient.post<Cliente>(this.apiServer + '/clients/', JSON.stringify(cliente), this.httpOptions)
    .pipe(
      catchError(this.errorHandler)
    )
  }  
  getById(id: number): Observable<Cliente> {
    return this.httpClient.get<Cliente>(this.apiServer + '/clients/' + id)
    .pipe(
      catchError(this.errorHandler)
    )
  }

  getAll(): Observable<Cliente[]> {
    return this.httpClient.get<Cliente[]>(this.apiServer + '/clients/')
    .pipe(
      catchError(this.errorHandler)
    )
  }

  update(id: number, cliente: Cliente): Observable<Cliente> {
    return this.httpClient.put<Cliente>(this.apiServer + '/clients/' + id, JSON.stringify(cliente), this.httpOptions)
    .pipe(
      catchError(this.errorHandler)
    )
  }

  delete(id: number){
    return this.httpClient.delete<Cliente>(this.apiServer + '/clients/' + id, this.httpOptions)
    .pipe(
      catchError(this.errorHandler)
    )
  }
  errorHandler(error: { error: { message: string; }; status: any; message: any; }) {
     let errorMessage = '';
     if(error.error instanceof ErrorEvent) {
       // Get client-side error
       errorMessage = error.error.message;
     } else {
       // Get server-side error
       errorMessage = `Error Code: ${error.status}\nMessage: ${error.message}`;
     }
     console.log(errorMessage);
     return throwError(errorMessage);
  }
}


```{{copy}}

Agora, é uma boa hora de limpar toda a tela inicial da aplicação e deixá-la somente com o nosso injetor de rotas!

`angular-crud/src/app/app.component.html`{{open}}

```html

    <router-outlet></router-outlet>

```{{copy}}