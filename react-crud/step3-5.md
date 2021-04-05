Pronto, agora com nosso backend funcionando, podemos voltar ao que interessa!

Vamos começar configurando nossa Store, que será responsável por executar todas as nossas operações de REST

Criamos então todas nossas páginas

`mkdir src/context`{{execute "T1"}}

`touch src/context/GlobalState.js`{{execute "T1"}}

`touch src/context/AppReducer.js`{{execute "T1"}}

Vamos configurar nosso estado inicial e suas demais mutações:

```js




import React, { createContext, useReducer } from 'react';
import axios from 'axios';
import appReducer from './AppReducer';

const API_URL = "https://[[HOST_SUBDOMAIN]]-3000-[[KATACODA_HOST]].environments.katacoda.com/clients";

const initialState = {
  clientes: [ ]
};

export const GlobalContext = createContext(initialState);

export const GlobalProvider = ({ children }) => {
  let [state, dispatch] = useReducer(appReducer, initialState);

  function listaCliente() {
    axios.get(API_URL).then(response => {
        console.log('Buscando Clientes')
        console.log(response.data)
        console.log(state)
        dispatch({
            type: "LIST_CLIENTE",
            payload: response
        })
    })
  }

  async function addCliente(cliente) {
    await axios.post(API_URL, cliente)
    let resultado = await axios.get(API_URL)
    console.log(state)
    dispatch({
      type: "ADD_CLIENTE",
      payload: resultado
    });
  }

  async function editCliente(cliente) {
    const clienteAtualizado = cliente;
    await axios.put(API_URL + '/' + clienteAtualizado.id, clienteAtualizado)
    let resultado = await axios.get(API_URL)
    dispatch({
      type: "EDIT_CLIENTE",
      payload: resultado
    });
  }

  function removeCliente(id) {
    axios.delete(API_URL + '/' + id).then(() => {
        axios.get(API_URL).then(resultado => {
            dispatch({
                type: "REMOVE_CLIENTE",
                payload: resultado
              });
        })
    })
  }

  return (
    <GlobalContext.Provider
      value={{
        clientes: state.clientes,
        addCliente,
        editCliente,
        removeCliente,
        listaCliente
      }}
    >
      {children}
    </GlobalContext.Provider>
  );
};



```{{copy}}

https://[[HOST_SUBDOMAIN]]-8080-[[KATACODA_HOST]].environments.katacoda.com
