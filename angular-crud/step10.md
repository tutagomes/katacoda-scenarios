E por último, vamos adicionar Clientes!

{{open angular-crud/src/app/clientes/create/create.component.ts}}

```ts

import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup } from '@angular/forms';
import { Router } from '@angular/router';
import { ClienteService } from '../cliente.service';

@Component({
  selector: 'app-create',
  templateUrl: './create.component.html',
  styleUrls: ['./create.component.css']
})
export class CreateComponent implements OnInit {

  clienteForm!: FormGroup;

  constructor(public fb: FormBuilder,
    private router: Router,
    public clienteService: ClienteService) { }

  ngOnInit(): void {
    this.clienteForm = this.fb.group({
      first_name: [''],
      last_name: [''],
      email: [''],
      street: [''],    
      number: [''],    
      zipcode: [''],    
      complement: [''],    
    })
  }
  submitForm() {
    this.clienteService.create(this.clienteForm.value).subscribe(response => {
      console.log('Cliente criado!')
      console.log(response)
      this.router.navigateByUrl('/clientes/home')
    })
  }

}


```{{copy}}

E nosso HTML

{{ open angular-crud/src/app/clientes/create/create.component.html }}

```html
 <div>
            <h1>Criar um Cliente</h1>
            <form [formGroup]="clienteForm" (ngSubmit)="submitForm()" novalidate>
                <div class="form-group">
                    <label>Primeiro Nome</label>
                    <input type="text" formControlName="first_name" class="form-control" maxlength="20">
                </div>
                <div class="form-group">
                    <label>Segundo Nome</label>
                    <input type="text" formControlName="first_name" class="form-control" maxlength="20">
                </div>
                <div class="form-group">
                    <label>Email</label>
                    <input type="email" formControlName="email" class="form-control" maxlength="20">
                </div>
                <div class="form-group">
                    <label>Rua</label>
                    <input type="text" formControlName="street" class="form-control" maxlength="100">
                </div>
                <div class="form-group">
                    <label>Número</label>
                    <input type="text" formControlName="number" class="form-control" maxlength="20">
                </div>
                <div class="form-group">
                    <label>CEP</label>
                    <input type="text" formControlName="zipcode" class="form-control" maxlength="20">
                </div>
                <div class="form-group">
                    <label>Complemento</label>
                    <input type="text" formControlName="complement" class="form-control" maxlength="200">
                </div>
                <button type="submit">Salvar</button>
            </form>
</div>

```{{copy}}
