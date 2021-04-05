E agora vamos para a parte interessante! Listar e remover clientes! Para isso, vamos trabalhar em cima do nosso componente `Home`, criado alguns passos atrás.

Vamos começar implementando as funcionalidades do componente:

`angular-crud/src/app/clientes/home/home.component.ts`{{open}}

```ts

import { Component, OnInit } from '@angular/core';
import { Cliente } from '../cliente';
import { ClienteService } from '../cliente.service';

@Component({
  selector: 'app-home',
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.css']
})
export class HomeComponent implements OnInit {

  public clientes: Cliente[] = [];

  constructor (public clienteService: ClienteService) { }

  ngOnInit(): void {
    this.listClients()
  }
  listClients () {
    this.clienteService.getAll().subscribe((data: Cliente[]) => {
      this.clientes = [ ...data]
    })
  }
  removeClient (id: number) {
    console.log("Removendo")
    this.clienteService.delete(id).subscribe(res => {
      this.listClients()
    })
  }

}

```{{copy}}

E também nosso HTML

`angular-crud/src/app/clientes/home/home.component.html`{{open}}

```html
<div>
    <h1>Meus Clientes</h1>
    <button type="button" [routerLink]="['/clientes/create']">Novo Cliente</button>
    <table>
        <thead>
            <tr>
                <th>#</th>
                <th>Primeiro Nome</th>
                <th>Segundo Nome</th>
                <th>Email</th>
                <th>Rua</th>                            
                <th>CEP</th>
                <th>Complemento</th>
            </tr>
        </thead>
        <tbody>
            <tr *ngFor="let cliente of clientes">
                <td>{{cliente.id}}</td>
                <td>{{cliente.first_name}}</td>
                <td>{{cliente.last_name}}</td>
                <td>{{cliente.email}}</td>
                <td>{{cliente.street}}</td>
                <td>{{cliente.number}}</td>
                <td>{{cliente.zipcode}}</td>
                <td>{{cliente.complement}}</td>
                <td>
                    <button type="button" [routerLink]="['/clientes/update/', cliente.id]">Atualizar</button>
                    <button type="button" (click)="removeClient(cliente.id)">Remover</button>
                </td>
            </tr>
        </tbody>
    </table>
</div>

```{{copy}}

Pronto! Agora podemos acessar nossa URL e validar se todos nossos clientes estão lá listados!

https://[[HOST_SUBDOMAIN]]-4200-[[KATACODA_HOST]].environments.katacoda.com/clientes/home

