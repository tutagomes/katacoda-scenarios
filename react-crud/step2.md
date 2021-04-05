Agora, com tudo instalado e configurado, podemos começar a executar alguns comandos para criar nossa aplicação React!

O que o comando `npx` pode fazer?

https://blog.rocketseat.com.br/conhecendo-o-npx-executor-de-pacote-do-npm/

`npx --help`{{execute "T1"}}

Bom, vamos começar então criando um projeto com o nome `react-crud`

`npx create-react-app react-crud`{{execute "T1"}}

Agora, o CLI deve executar todos os comandos necessários para instalar as dependências do projeto além de criar para nós todos os componentes base

Então, navegamos até a pasta

`cd react-crud`{{execute "T1"}}

E precisamos fazer uma pequena edição no nosso .env a fim de possibilitar o acesso externo ao nosso servidor de desenvolvimento:

```env
PORT=8080
HOST=0.0.0.0
```

Precisamos também instalar algumas dependências:

`npm install react-router-dom axios`{{execute "T1"}}

`cd src`{{execute "T1"}}

`npx tailwindcss-cli@0.1.2 build --output tailwind.css`{{execute "T1"}}

`cd ..`{{execute "T1"}}

E finalmente podemos ver a cara da aplicação básica:

`npm start`{{execute "T1"}}

E acessá-la através da URL!
https://[[HOST_SUBDOMAIN]]-8080-[[KATACODA_HOST]].environments.katacoda.com


E já vamos preparar nossos estilos!

`react-crud/src/index.js`{{open}}
```js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';
import { BrowserRouter } from 'react-router-dom';
import './tailwind.css';

ReactDOM.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>,
  document.getElementById('root')
);


// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();

```