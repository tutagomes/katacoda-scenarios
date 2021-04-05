Agora, vamos finalizar nossa página de criar o cliente

`react-crud/src/clientes/create.js`{{open}}


```js

import React, { useState, useContext } from 'react';
import { Link, useHistory } from 'react-router-dom';

import { GlobalContext } from '../context/GlobalState';

export const CriaCliente = () => {
  let history = useHistory();

  const { addCliente } = useContext(GlobalContext);

  const [first_name, setName] = useState("");
  const [last_name, setLastName] = useState("");
  const [zipcode, setZipCode] = useState("");
  const [email, setEmail] = useState("");

  const onSubmit = (e) => {
    e.preventDefault();
    const newEmployee = {
        first_name,
        last_name,
        zipcode,
    };
    addCliente(newEmployee);
    history.push("/");
  };

  return (
    <React.Fragment>
      <div className="w-full max-w-sm container mt-20 mx-auto">
        <form onSubmit={onSubmit}>
          <div className="w-full mb-5">
            <label
              className="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2"
              htmlFor="first_name"
            >
              Primeiro Nome
            </label>
            <input
              className="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:text-gray-600"
              value={first_name}
              onChange={(e) => setName(e.target.value)}
              type="text"
              placeholder="Primeiro Nome"
            />
          </div>
          <div className="w-full mb-5">
            <label
              className="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2"
              htmlFor="last_name"
            >
              Último Nome
            </label>
            <input
              className="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:text-gray-600 focus:shadow-outline"
              value={last_name}
              onChange={(e) => setLastName(e.target.value)}
              type="text"
              placeholder="Segundo Nome"
            />
          </div>
          <div className="w-full mb-5">
            <label
              className="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2"
              htmlFor="email"
            >
              Email
            </label>
            <input
              className="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:text-gray-600 focus:shadow-outline"
              value={email}
              onChange={(e) => setEmail(e.target.value)}
              type="text"
              placeholder="Email para notificações"
            />
          </div>
          <div className="w-full mb-5">
            <label
              className="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2"
              htmlFor="zipcode"
            >
              CEP
            </label>
            <input
              className="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:text-gray-600"
              value={zipcode}
              onChange={(e) => setZipCode(e.target.value)}
              type="text"
              placeholder="Digite o CEP"
            />
          </div>
          <div className="flex items-center justify-between">
            <button className="mt-5 bg-green-400 w-full hover:bg-green-500 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline">
              Adicionar Cliente
            </button>
          </div>
          <div className="text-center mt-4 text-gray-500">
            <Link to="/">Cancel</Link>
          </div>
        </form>
      </div>
    </React.Fragment>
  );
};

```{{copy}}
