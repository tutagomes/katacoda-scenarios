E também nossas mutações de estado, chamando APIs a cada alteração

> Para facilitar https://[[HOST_SUBDOMAIN]]-3000-[[KATACODA_HOST]].environments.katacoda.com/clients

```js

export default function appReducer(state, action) {
    switch (action.type) {
      case "LIST_CLIENTE":
        return {
            ...state,
            clientes: [ ...action.payload.data],
        }
      case "ADD_CLIENTE":
        return {
          ...state,
          clientes: [ ...action.payload.data],
        };
      case "EDIT_CLIENTE":
        return {
          ...state,
          clientes: [ ...action.payload.data ],
        };
  
      case "REMOVE_CLIENTE":
        return {
            ...state,
            clientes: [ ...action.payload.data ],
        };
  
      default:
        return state;
    }
  };
  
```{{copy}}

https://[[HOST_SUBDOMAIN]]-8080-[[KATACODA_HOST]].environments.katacoda.com
