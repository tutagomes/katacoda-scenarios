O próximo passo é definir uma URL base para nosso Axios! Assim, não precisaremos configurar cada uma das requisições manualmente. Para isso, existe um arquivo simplificado de boot que podemos colocar nossas configurações lá!

https://[[HOST_SUBDOMAIN]]-3000-[[KATACODA_HOST]].environments.katacoda.com

`vuejs-crud/src/boot/axios.js`{{open}}

> Para facilitar: https://[[HOST_SUBDOMAIN]]-3000-[[KATACODA_HOST]].environments.katacoda.com


```js

  import Vue from 'vue'
  import axios from 'axios'

  axios.defaults.baseURL = 'https://[[HOST_SUBDOMAIN]]-3000-[[KATACODA_HOST]].environments.katacoda.com/'
  Vue.prototype.$axios = axios


```{{copy}}


Dessa forma, todas nossas requisições serã direcionadas para a URL configurada acima!