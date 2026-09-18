# React 16 Framework dla profesjonalistow
## Przerwsza aplikacja w React
### Tworzenie projektu
```
> npx create-react-app zadania
```
- pliki:
  - public/index.html - To plik HTML wyświetlany w przeglądarce. Zawiera on element, w którym jest wyświetlana aplikacja, oraz element script, który wczytuje pliki JavaScript aplikacji.
  - src/index.js - To plik JavaScript odpowiedzialny za konfigurację i uruchamianie aplikacji Reacta.
  - src/App.js - To komponent Reacta zawierający treści HTML wyświetlane użytkownikom aplikacji oraz kod JavaScript wymagany przez te treści HTML. 
  
#### Dodanie frameworka CSS Bootstrap
```
cd zadania
npm install bootstrap@4.1.2
```

- index.js

```

    import React from 'react';

    import ReactDOM from 'react-dom';

    import './index.css';

    import App from './App';

    import * as serviceWorker from './serviceWorker';

    import 'bootstrap/dist/css/bootstrap.css';

    ReactDOM.render(<App />, document.getElementById('root'));

    // If you want your app to work offline and load faster, you can change

    // unregister() to register() below. Note this comes with some pitfalls.

    // Learn more about service workers: https://bit.ly/CRA-PWA

    serviceWorker.unregister();
```

####  Uruchamianie narzędzi dla programistów
```
    npm start
```

### Usuwanie treści zastępczej
- scr/App.js
```javascript

    import React, { Component } from 'react';

    //import logo from './logo.svg';

    //import './App.css';

    export default class App extends Component {

      render() {

        return (

          <div>

            <h4 className="bg-primary text-white text-center p-2">

              Lista zadań

            </h4>

          </div>

        )

      };

    }
```
-  Plik App.js zawiera komponent Reacta o nazwie App. Komponenty są podstawowymi elementami konstrukcyjnymi aplikacji Reacta. Są one pisane w języku JSX

### Wyświetlanie treści dynamicznych
- Wyrażenie to fragment kodu JavaScript, który jest przetwarzany podczas wywołania metody render komponentu i który zapewnia możliwość wyświetlania danych użytkownikom aplikacji.

```
  import React, { Component } from 'react';

    export default class App extends Component {

      constructor(props) {

        super(props);

        this.state = {

          userName: "Adam"

        }

      }

      render() {

        return (

          <div>

            <h4 className="bg-primary text-white text-center p-2">

              Lista zadań użytkownika { this.state.userName }

            </h4>

          </div>

        )

      };

    }
```

- `constructor`  to specjalna metoda nazywana także konstruktorem, wywoływana podczas inicjalizacji komponentu. Wywołanie metody `super` jest niezbędne do zapewnienia prawidłowej konfiguracji komponentu.
- Parametr `props` zdefiniowany w konstruktorze jest ważny w aplikacjach Reacta, gdyż pozwala na konfigurowanie jednych komponentów przez inne.
- Właściwość o nazwie `state`, służy do definiowania danych stanu.

```

    this.state = {
       userName: "Adam"
    }
```
- Zmiana stanu wywoła ponownie `render`

```


      changeStateData = () => {

        this.setState({

          userName: this.state.userName === "Adam" ? "Jakub" : "Adam"

        })

      }

      render() {

        return (

          <div>

            <h4 className="bg-primary text-white text-center p-2">

              Lista zadań użytkownika { this.state.userName }

            </h4>

            <button className="btn btn-primary m-2"

                    onClick={this.changeStateData}>

              Zmień

            </button>

          </div>

        )
 };
``` 
- Podczas korzystania z właściwości i metod zdefiniowanych w komponencie, w tym także z metody `setState`, konieczne jest posługiwanie się słowem kluczowym this. Pomijanie tego słowa kluczowego jest błędem.
- Funkcje tworzone przy użyciu składni grubej strzałki nie wymagają stosowania instrukcji return ani zapisywania ciała funkcji wewnątrz nawiasów klamrowych.

### Dodawanie możliwości aplikacji listy zadań
```
    export default class App extends Component {

      constructor(props) {

        super(props);

        this.state = {

          userName: "Adam",

          todoItems: [{ action: "Kupić kwiaty", done: false },

                      { action: "Wziąć buty", done: false },

                      { action: "Zebrać bilety", done: true },

                      { action: "Zadzwonić do Jurka", done: false }],

          newItemText: ""

        }

      }

      updateNewTextValue = (event) => {

        this.setState({ newItemText: event.target.value });

      }

      createNewTodo = () => {

        if (!this.state.todoItems

            .find(item => item.action === this.state.newItemText)) {

          this.setState({

            todoItems: [...this.state.todoItems,

                { action: this.state.newItemText, done: false }],

            newItemText: ""

          });

        }

      }
      
    render = () =>

        <div>

 <h4 className="bg-primary text-white text-center p-2">
        Lista zadań użytkownika {this.state.userName}
        (Liczba zadań: {this.state.todoItems.filter((t) => !t.done).length})
      </h4>

          <div className="container-fluid">

            <div className="my-1">

              <input className="form-control"

                value={this.state.newItemText}

                onChange={this.updateNewTextValue} />

              <button className="btn btn-primary mt-1"

                onClick={this.createNewTodo}>Dodaj</button>

            </div>

          </div>

        </div>

    }      
```

- Operator rozproszenia `...` pozwala na wyodrębneinie elementów tablicy

```
 todoItems: [
          ...this.state.todoItems,
          { action: this.state.newItemText, done: false },
        ]
```
#### Wyświetlanie zadań do zrobienia
```


      toggleTodo = (todo) => this.setState({

        todoItems:

          this.state.todoItems.map(item => item.action === todo.action

            ? { ...item, done: !item.done } : item)

      });

      todoTableRows = () => this.state.todoItems.map(item =>

        <tr key={item.action}>

          <td>{item.action}</td>

          <td>

            <input type="checkbox" checked={item.done}

              onChange={() => this.toggleTodo(item)} />

          </td>

        </tr>);
        
  

      render = () =>

        <div>

          <h4 className="bg-primary text-white text-center p-2">

            Lista zadań użytkownika {this.state.userName}

            (Liczba zadań: {this.state.todoItems.filter(t => !t.done).length})

          </h4>

          <div className="container-fluid">

            <div className="my-1">

              <input className="form-control"

                value={this.state.newItemText}

                onChange={this.updateNewTextValue} />

              <button className="btn btn-primary mt-1"

                onClick={this.createNewTodo}>Dodaj</button>

            </div>

            <table className="table table-striped table-bordered">

              <thead>

                <tr><th>Opis</th><th>Wykonane</th></tr>

              </thead>

              <tbody>{this.todoTableRows()}</tbody>

            </table>

          </div>

        </div>      
```
- Format JSX pozwala także na dowolne mieszanie kodu HTML i JavaScript, a to oznacza, że funkcje JavaScript mogą zwracać kod HTML.

```

    todoTableRows = () => this.state.todoItems.map(item =>

      <tr key={item.action}>

        <td>{item.action}</td>

        <td>

          <input type="checkbox" checked={item.done}

            onChange={() => this.toggleTodo(item)} />

        </td>

      </tr>);
```

-  React wymaga właściwości key, by móc powiązać wyświetlane treści z danymi, na podstawie których zostały one wygenerowane, i efektywnie zarządzać zmianami.

### Wprowadzanie dodatkowych komponentów
-  Komponent podrzędny - komponent, do którego przekazujemy dane
-  src/TodoBanner.js
```
import React, { Component } from 'react';
  export class TodoBanner extends Component {
  render = () =>
  <h4 className="bg-primary text-white text-center p-2">
    Lista zadań użytkownika {this.props.name}
 
    (Liczba zadań: {this.props.tasks.filter(t => !t.done).length})

  </h4>
}
```

- Aby wyświetlić wartość właściwości name, w kodzie komponentu należy użyć wyrażenia `this.props.name`.
-  src/TodoRow.js
```
import React, { Component } from "react";

export class TodoRow extends Component {
  render = () => (
    <tr>
      <td>{this.props.item.action}</td>

      <td>
        <input
          type="checkbox"
          checked={this.props.item.done}
          onChange={() => this.props.callback(this.props.item)}
        />
      </td>
    </tr>
  );
}
```
- Właściwości `props` pozwalają przekazywać dane z komponentów nadrzędnych do podrzędnych, a właściwości funkcyjne pozwalają komponentom podrzędnym komunikować się ze swoimi komponentami nadrzędnymi.
- src/TodoCreator.js
```
import React, { Component } from "react";

export class TodoCreator extends Component {
  constructor(props) {
    super(props);

    this.state = { newItemText: "" };
  }

  updateNewTextValue = (event) => {
    this.setState({ newItemText: event.target.value });
  };

  createNewTodo = () => {
    this.props.callback(this.state.newItemText);

    this.setState({ newItemText: "" });
  };

  render = () => (
    <div className="my-1">
      <input
        className="form-control"
        value={this.state.newItemText}
        onChange={this.updateNewTextValue}
      />

      <button className="btn btn-primary mt-1" onClick={this.createNewTodo}>
        Nowe zadanie
      </button>
    </div>
  );
}

```

#### Stosowanie komponentów podrzędnych
-  src/App.js

```
//import logo from './logo.svg';
//import './App.css';
import React, { Component } from "react";
import { TodoBanner } from "./TodoBanner";
import { TodoCreator } from "./TodoCreator";
import { TodoRow } from "./TodoRow";

export default class App extends Component {
  constructor(props) {
    super(props);

    this.state = {
      userName: "Adam",

      todoItems: [
        { action: "Kupić kwiaty", done: false },

        { action: "Wziąć buty", done: false },

        { action: "Zebrać bilety", done: true },

        { action: "Zadzwonić do Jurka", done: false },
      ],

      //newItemText: ""
    };
  }

  updateNewTextValue = (event) => {
    this.setState({ newItemText: event.target.value });
  };

  createNewTodo = (task) => {
    if (!this.state.todoItems.find((item) => item.action === task)) {
      this.setState({
        todoItems: [...this.state.todoItems, { action: task, done: false }],
      });
    }
  };

  toggleTodo = (todo) =>
    this.setState({
      todoItems: this.state.todoItems.map((item) =>
        item.action === todo.action ? { ...item, done: !item.done } : item,
      ),
    });

  todoTableRows = () =>
    this.state.todoItems.map((item) => (
      <TodoRow key={item.action} item={item} callback={this.toggleTodo} />
    ));

  render = () => (
    <div>
      <TodoBanner name={this.state.userName} tasks={this.state.todoItems} />

      <div className="container-fluid">
        <TodoCreator callback={this.createNewTodo} />

        <table className="table table-striped table-bordered">
          <thead>
            <tr>
              <th>Opis</th>
              <th>Done</th>
            </tr>
          </thead>

          <tbody>{this.todoTableRows()}</tbody>
        </table>
      </div>
    </div>
  );
}
      
```
- Instrukcje import deklarują zależności z komponentami podrzędnym
- Atrybuty i wyrażenia definiują właściwości props przekazywane do komponentów.

```
<TodoBanner name={this.state.userName} tasks={this.state.todoItems} />
```

### Ostatnie szlify
-  src/VisibilityControl.js

```
import React, { Component } from "react";

export class VisibilityControl extends Component {
  render = () => (
    <div className="form-check">
      <input
        className="form-check-input"
        type="checkbox"
        checked={this.props.isChecked}
        onChange={(e) => this.props.callback(e.target.checked)}
      />

      <label className="form-check-label">Pokaż {this.props.description}</label>
    </div>
  );
}
```

- src/App.js

```
//import logo from './logo.svg';
//import './App.css';
import React, { Component } from "react";
import { TodoBanner } from "./TodoBanner";
import { TodoCreator } from "./TodoCreator";
import { TodoRow } from "./TodoRow";
import { VisibilityControl } from "./VisibilityControl";

export default class App extends Component {
  constructor(props) {
    super(props);

    this.state = {
      userName: "Adam",

      todoItems: [
        { action: "Kupić kwiaty", done: false },

        { action: "Wziąć buty", done: false },

        { action: "Zebrać bilety", done: true },

        { action: "Zadzwonić do Jurka", done: false },
      ],

      showCompleted: true,
    };
  }

  updateNewTextValue = (event) => {
    this.setState({ newItemText: event.target.value });
  };

  createNewTodo = (task) => {
    if (!this.state.todoItems.find((item) => item.action === task)) {
      this.setState({
        todoItems: [...this.state.todoItems, { action: task, done: false }],
      });
    }
  };

  toggleTodo = (todo) =>
    this.setState({
      todoItems: this.state.todoItems.map((item) =>
        item.action === todo.action ? { ...item, done: !item.done } : item,
      ),
    });

  todoTableRows = () =>
    this.state.todoItems.map((item) => (
      <TodoRow key={item.action} item={item} callback={this.toggleTodo} />
    ));

  todoTableRows = (doneValue) =>
    this.state.todoItems
      .filter((item) => item.done === doneValue)
      .map((item) => (
        <TodoRow key={item.action} item={item} callback={this.toggleTodo} />
      ));

  render = () => (
    <div>
      <TodoBanner name={this.state.userName} tasks={this.state.todoItems} />

      <div className="container-fluid">
        <TodoCreator callback={this.createNewTodo} />

        <table className="table table-striped table-bordered">
          <thead>
            <tr>
              <th>Opis</th>
              <th>Wykonane</th>
            </tr>
          </thead>

          <tbody>{this.todoTableRows(false)}</tbody>
        </table>

        <div className="bg-secondary text-white text-center p-2">
          <VisibilityControl
            description="wykonane zadania"
            isChecked={this.state.showCompleted}
            callback={(checked) => this.setState({ showCompleted: checked })}
          />
        </div>

        {this.state.showCompleted && (
          <table className="table table-striped table-bordered">
            <thead>
              <tr>
                <th>Opis</th>
                <th>Wykonane</th>
              </tr>
            </thead>

            <tbody>{this.todoTableRows(true)}</tbody>
          </table>
        )}
      </div>
    </div>
  );
}

```
- Podczas przetwarzania wyrażenia element table zostanie wstawiony do komponentu wyłącznie wtedy, gdy wartość właściwości `showCompleted` wyniesie `true`.

```
{ this.state.showCompleted && <table className="table table-striped table-bordered">
```

#### Trwałe przechowywanie danych
- [Local Storage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)
- Dostęp do API magazynu lokalnego zapewnia obiekt `localStorage`.
- Magazyn lokalny umożliwia zapisywanie wyłącznie łańcuchów, dlatego też przed zapisaniem obiekty z danymi zadań muszą zostać serializowane. Do metody setState można przekazywać funkcję, która zostanie wywołana po zaktualizowaniu danych stanu.
- src/App.js
```
//import logo from './logo.svg';
//import './App.css';
import React, { Component } from "react";
import { TodoBanner } from "./TodoBanner";
import { TodoCreator } from "./TodoCreator";
import { TodoRow } from "./TodoRow";
import { VisibilityControl } from "./VisibilityControl";

export default class App extends Component {
  constructor(props) {
    super(props);

    this.state = {
      userName: "Adam",

      todoItems: [
        { action: "Kupić kwiaty", done: false },

        { action: "Wziąć buty", done: false },

        { action: "Zebrać bilety", done: true },

        { action: "Zadzwonić do Jurka", done: false },
      ],

      showCompleted: true,
    };
  }

  updateNewTextValue = (event) => {
    this.setState({ newItemText: event.target.value });
  };

  createNewTodo = (task) => {
    if (!this.state.todoItems.find((item) => item.action === task)) {
      this.setState(
        {
          todoItems: [...this.state.todoItems, { action: task, done: false }],
        },
        () => localStorage.setItem("todos", JSON.stringify(this.state)),
      );
    }
  };

  toggleTodo = (todo) =>
    this.setState({
      todoItems: this.state.todoItems.map((item) =>
        item.action === todo.action ? { ...item, done: !item.done } : item,
      ),
    });

  todoTableRows = () =>
    this.state.todoItems.map((item) => (
      <TodoRow key={item.action} item={item} callback={this.toggleTodo} />
    ));

  todoTableRows = (doneValue) =>
    this.state.todoItems
      .filter((item) => item.done === doneValue)
      .map((item) => (
        <TodoRow key={item.action} item={item} callback={this.toggleTodo} />
      ));

  componentDidMount = () => {
    let data = localStorage.getItem("todos");

    this.setState(
      data != null
        ? JSON.parse(data)
        : {
            userName: "Adam",

            todoItems: [
              { action: "Kupić kwiaty", done: false },

              { action: "Wziąć buty", done: false },

              { action: "Zebrać bilety", done: true },

              { action: "Zadzwonić do Jurka", done: false },
            ],

            showCompleted: true,
          },
    );
  };

  render = () => (
    <div>
      <TodoBanner name={this.state.userName} tasks={this.state.todoItems} />

      <div className="container-fluid">
        <TodoCreator callback={this.createNewTodo} />

        <table className="table table-striped table-bordered">
          <thead>
            <tr>
              <th>Opis</th>
              <th>Wykonane</th>
            </tr>
          </thead>

          <tbody>{this.todoTableRows(false)}</tbody>
        </table>

        <div className="bg-secondary text-white text-center p-2">
          <VisibilityControl
            description="wykonane zadania"
            isChecked={this.state.showCompleted}
            callback={(checked) => this.setState({ showCompleted: checked })}
          />
        </div>

        {this.state.showCompleted && (
          <table className="table table-striped table-bordered">
            <thead>
              <tr>
                <th>Opis</th>
                <th>Wykonane</th>
              </tr>
            </thead>

            <tbody>{this.todoTableRows(true)}</tbody>
          </table>
        )}
      </div>
    </div>
  );
}

``` 

## Zrozumieć React
- [pro-react-16](https://github.com/Apress/pro-react-16)
- [informacje o poprawkach](https://helion.pl/errata.cgi?id=reac16)

## Podstawy HTML, JSX i CSS
### Przygotowania do prac
```
npx create-react-app podstawy

cd podstawy
npm install bootstrap@4.1.2
```

-  src/index.js
```

    import React from 'react';

    import ReactDOM from 'react-dom';

    import './index.css';

    import App from './App';

    import * as serviceWorker from './serviceWorker';

    import 'bootstrap/dist/css/bootstrap.css';

    ReactDOM.render(<App />, document.getElementById('root'));

    // If you want your app to work offline and load faster, you can change
    


    // unregister() to register() below. Note this comes with some pitfalls.

    // Learn more about service workers: https://bit.ly/CRA-PWA

    serviceWorker.unregister();    
```

-  src/index.html

```

    <!DOCTYPE html>

    <html lang="en">

      <head>

        <meta charset="utf-8" />

        <title>Podstawy</title>

      </head>

      <body>

        <h4 class="bg-primary text-white text-center p-2 m-1">

          Statyczny element HTML

        </h4>

        <div id="domParent"></div>

        <div id="root"></div>

      </body>

    </html>
```

-  src/App.js

```

    import React, { Component } from "react";

    export default class App extends Component {

      render = () =>

        <h4 className="bg-primary text-white text-center p-2 m-1">

          Element komponentu

    </h4>

    }
```
- uruchomienie

```

    npm start
```