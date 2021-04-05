Pronto, agora com nosso backend funcionando, podemos voltar ao que interessa!

Vamos começar criando nossos componentes Angular, sendo cada um respnsável por uma funcionalidade no nosso módulo de Clientes

Criamos então todas nossas páginas

`mkdir src/clientes`{{execute "T1"}}

`touch src/clientes/home.js`{{execute "T1"}}

`touch src/clientes/create.js`{{execute "T1"}}

`react-crud/src/clientes/home.js`{{open}}

```js
import React, { useContext, useEffect } from 'react';

import { GlobalContext } from '../context/GlobalState';
import { Link } from "react-router-dom";

export const ListaClientes = () => {
  const { clientes, listaCliente, removeCliente } = useContext(GlobalContext);
  useEffect(()=>{
    listaCliente()
    }, [])
  return (
    <React.Fragment>
        <Link to="/add">
            <button className="bg-green-400 hover:bg-green-500 text-white font-semibold py-2 px-4 rounded inline-flex items-center">
              <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className="feather feather-plus-circle"><circle cx="12" cy="12" r="10"></circle><line x1="12" y1="8" x2="12" y2="16"></line><line x1="8" y1="12" x2="16" y2="12"></line></svg>
              <span className="pl-2">Adiciona Cliente</span>
            </button>
        </Link>
      {clientes && clientes.length > 0 ? (
        <React.Fragment>
          {clientes.map((cliente) => (
            <div
              className="flex items-center bg-gray-100 mb-10 shadow"
              key={cliente.id}
            >
              <div className="flex-auto text-left px-4 py-2 m-2">
                <p className="text-gray-900 leading-none">
                  {cliente.first_name}
                </p>
                <p className="text-gray-600">
                  {cliente.last_name}
                </p>
                <span className="inline-block text-sm font-semibold mt-1">
                  {cliente.zipcode}
                </span>
              </div>
              <div className="flex-auto text-right px-4 py-2 m-2">
               <button
                  onClick={() => removeCliente(cliente.id)}
                  className="block bg-gray-300 hover:bg-gray-400 text-gray-800 font-semibold py-2 px-4 rounded-full inline-flex items-center"
                  title="Remove Employee"
                >
                  <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className="feather feather-trash-2"><polyline points="3 6 5 6 21 6"></polyline><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"></path><line x1="10" y1="11" x2="10" y2="17"></line><line x1="14" y1="11" x2="14" y2="17"></line></svg>
                </button>
              </div>
            </div>
          ))}
        </React.Fragment>
      ) : (
        <p className="text-center bg-gray-100 text-gray-500 py-5">No data.</p>
      )}
    </React.Fragment>
  );
};

```{{copy}}


https://[[HOST_SUBDOMAIN]]-8080-[[KATACODA_HOST]].environments.katacoda.com
