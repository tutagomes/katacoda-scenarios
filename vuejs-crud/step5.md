Pronto, agora vamos à parte que interessa! A programação!

Vamos começar configurando nossas rotas. Aqui, podemos decidir se nossas páginas utilizarão o Layout padrão já criado ou se serão páginas completas. Para facilitar a navegação, podemos aproveitar o código do layout já criado!

`vuejs-crud/src/router/routes.js`{{open}}

```js

const routes = [
  {
    path: '/',
    component: () => import('layouts/MainLayout.vue'),
    children: [
      { path: '', component: () => import('pages/Index.vue') },
      { path: 'clientes/home', component: () => import('pages/clientes/home.vue')},
      { path: 'clientes/create', component: () => import('pages/clientes/create.vue')},
      { path: 'clientes/edit/:id', component: () => import('pages/clientes/create.vue')}
    ]
  },

  // Always leave this as last one,
  // but you can also remove it
  {
    path: '*',
    component: () => import('pages/Error404.vue')
  }
]

export default routes

```{{copy}}

E podemos também modificar nossa página de layout para acomodar os links no canto menu lateral!

`vuejs-crud/src/layouts/MainLayout.vue`{{open}}

```html

<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="leftDrawerOpen = !leftDrawerOpen"
        />

        <q-toolbar-title>
          VueJS Crud
        </q-toolbar-title>

      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
      content-class="bg-grey-1"
    >
      <q-list>
        <q-item-label
          header
          class="text-grey-8"
        >
          Essential Links
        </q-item-label>
          <q-item
            clickable
            v-for="link in essentialLinks"
            :href="link"
            :to="link.link"
            v-bind:key="link.title"
          >
            <q-item-section
              v-if="link.icon"
              avatar
            >
              <q-icon :name="link.icon" />
            </q-item-section>

            <q-item-section>
              <q-item-label>{{ link.title }}</q-item-label>
              <q-item-label caption>
                {{ link.caption }}
              </q-item-label>
            </q-item-section>
          </q-item>
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
const linksData = [
  {
    title: 'Início',
    caption: '',
    icon: 'home',
    link: '/'
  },
  {
    title: 'Listar Cliente',
    caption: 'Lista todos os clientes do sistema',
    icon: 'people',
    link: '/clientes/home'
  },
  {
    title: 'Criar Cliente',
    caption: 'Adiciona um cliente ao sistema',
    icon: 'person_add',
    link: '/clientes/create'
  }
];

export default {
  name: 'MainLayout',
  data () {
    return {
      leftDrawerOpen: false,
      essentialLinks: linksData
    }
  }
}
</script>


```{{copy}}
