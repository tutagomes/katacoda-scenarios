Pronto, Agora podemos configurar nossas rotas!

`react-crud/src/App.js`{{open}}

```js

import { ListaClientes } from './clientes/home';
import { CriaCliente } from './clientes/create';
import { GlobalProvider } from './context/GlobalState';
import { Route, Switch } from 'react-router-dom';

import './App.css';

function App() {
  return (
    <GlobalProvider>
      <div className="App">
      <Switch>
          <Route path="/" component={ListaClientes} exact />
          <Route path="/add" component={CriaCliente} exact />
        </Switch>
      </div>
    </GlobalProvider>
  );
}

export default App;

```{{copy}}

