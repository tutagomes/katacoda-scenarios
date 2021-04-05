Agora, vamos começar a realmente implementar um código "personalizado". Iniciando pela nossa interface de Cliente, que será responsável por definir a tabela inicial:

`vuejs-crud/src/pages/clientes/home.vue`{{open}}

```js

<template>
  <q-page padding>
    <q-btn color="positive" label="Criar Cliente" icon="add" to='/clientes/create'/>
    <q-markup-table>
      <thead>
        <tr>
          <th class="text-left">#</th>
          <th class="text-right">Primeiro Nome</th>
          <th class="text-right">Segundo Nome</th>
          <th class="text-right">Email</th>
          <th class="text-right">Rua</th>
          <th class="text-right">Número</th>
          <th class="text-right">CEP</th>
          <th class="text-right">Complemento</th>
          <th class="text-right">Ações</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for='cliente in clientes' :key='cliente.id'>
          <td class="text-left">{{cliente.id}}</td>
          <td class="text-left">{{cliente.first_name}}</td>
          <td class="text-right">{{cliente.last_name}}</td>
          <td class="text-right">{{cliente.email}}</td>
          <td class="text-right">{{cliente.street}}</td>
          <td class="text-right">{{cliente.number}}</td>
          <td class="text-right">{{cliente.zipcode}}</td>
          <td class="text-right">{{cliente.complement}}</td>
          <td class="text-right">
            <q-btn @click="excluir(cliente.id)" icon="delete" color="red"></q-btn>
            <q-btn @click="edit(cliente.id)" icon="edit" color="yellow"></q-btn>
          </td>
        </tr>
      </tbody>
    </q-markup-table>
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      clientes: []
    }
  },
  mounted() {
    this.carregaClientes()
  },
  methods: {
    async carregaClientes () {
      const response = await this.$axios.get('clients')
      this.clientes = [...response.data]
    },
    async excluir (id) {
      await this.$axios.delete('clients/' + id)
      this.carregaClientes()
    },
    edit (id) {
      this.$router.push('/clientes/edit/' + id)
    }
  }
}
</script>
 
```{{copy}}

