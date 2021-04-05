Agora, vamos criar nossa página de Adição de clientes, com todos os campos necessários:


`vuejs-crud/src/pages/clientes/create.vue`{{open}}

```js
<template>
  <q-page padding>
    <div class='row justify-center'>
      <div class='col-8'>
        <q-card>
          <q-card-section>
            <q-input outlined v-model="cliente.first_name" label="Primeiro Nome" />
            <q-input outlined v-model="cliente.last_name" label="Último Nome" />
            <q-input outlined v-model="cliente.email" label="Email" />
            <q-input outlined v-model="cliente.zipcode" label="CEP" />
            <q-input outlined v-model="cliente.street" label="Rua" />
            <q-input outlined v-model="cliente.number" label="Número" />
            <q-input outlined v-model="cliente.complement" label="Complemento" />
            <q-btn color="positive" label="Salvar" icon="save" @click="salvar()"/>
          </q-card-section>
        </q-card>
      </div>
    </div>
  </q-page>
</template>

<script>
export default {
  created() {
    console.log(this.$route.params)
  },
  data() {
    return {
      cliente: {
        id: -1,
        first_name: '',
        last_name: '',
        email: '',
        street: '',
        number: '',
        zipcode: '',
        complement: ''
      }      
    }
  },
  methods: {
    salvar () {
      if (this.cliente.id === -1) {
        delete this.cliente.id
        this.$axios.post('clients', this.cliente).then(response => {
          this.$router.push('/clientes/home')
        })
      } else {
        this.$axios.put('clients/' + this.cliente.id, this.cliente).then(response => {
          this.$router.push('/clientes/home')
        })
      }
    }
  }
}
</script>



```{{copy}}

