<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você incorporará o Microsoft Graph no aplicativo. Para este aplicativo, você usará a biblioteca [Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para fazer chamadas para o Microsoft Graph.

## <a name="get-calendar-events-from-outlook"></a>Obter eventos de calendário do Outlook

Comece adicionando um novo método ao `./src/GraphService.js` arquivo para obter os eventos do calendário. Adicione a função a seguir.

```js
export async function getEvents(accessToken) {
  const client = getAuthenticatedClient(accessToken);

  const events = await client
    .api('/me/events')
    .select('subject,organizer,start,end')
    .orderby('createdDateTime DESC')
    .get();

  return events;
}
```

Considere o que esse código está fazendo.

- A URL que será chamada é `/me/events`.
- O `select` método limita os campos retornados para cada evento para apenas aqueles que o modo de exibição realmente usará.
- O `orderby` método classifica os resultados pela data e hora em que foram criados, com o item mais recente em primeiro lugar.

Agora, crie um componente reagir para exibir os resultados da chamada. Crie um novo arquivo no `./src` diretório chamado `Calendar.js` e adicione o código a seguir.

```JSX
import React from 'react';
import { Table } from 'reactstrap';
import moment from 'moment';
import config from './Config';
import { getEvents } from './GraphService';

// Helper function to format Graph date/time
function formatDateTime(dateTime) {
  return moment.utc(dateTime).local().format('M/D/YY h:mm A');
}

export default class Calendar extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      events: []
    };
  }

  async componentDidMount() {
    try {
      // Get the user's access token
      var accessToken = await window.msal.acquireTokenSilent({
        scopes: config.scopes
      });
      // Get the user's events
      var events = await getEvents(accessToken);
      // Update the array of events in state
      this.setState({events: events.value});
    }
    catch(err) {
      this.props.showError('ERROR', JSON.stringify(err));
    }
  }

  render() {
    return (
      <pre><code>{JSON.stringify(this.state.events, null, 2)}</code></pre>
    );
  }
}
```

Por ora, isso apenas renderiza a matriz de eventos em JSON na página. Adicione esse novo componente ao aplicativo. Abra `./src/App.js` e adicione a seguinte `import` instrução à parte superior do arquivo.

```js
import Calendar from './Calendar';
```

Em seguida, adicione o seguinte componente logo após `<Route>`o existente.

```JSX
<Route exact path="/calendar"
  render={(props) =>
    <Calendar {...props}
      showError={this.setErrorMessage.bind(this)} />
  } />
```

Salve suas alterações e reinicie o aplicativo. Entre e clique no link **calendário** na barra de navegação. Se tudo funcionar, você deverá ver um despejo JSON de eventos no calendário do usuário.

## <a name="display-the-results"></a>Exibir os resultados

Agora você pode atualizar o `Calendar` componente para exibir os eventos de forma mais amigável. Substitua a função `render` existente na `./src/Calendar.js` função a seguir.

```JSX
render() {
  return (
    <div>
      <h1>Calendar</h1>
      <Table>
        <thead>
          <tr>
            <th scope="col">Organizer</th>
            <th scope="col">Subject</th>
            <th scope="col">Start</th>
            <th scope="col">End</th>
          </tr>
        </thead>
        <tbody>
          {this.state.events.map(
            function(event){
              return(
                <tr key={event.id}>
                  <td>{event.organizer.emailAddress.name}</td>
                  <td>{event.subject}</td>
                  <td>{formatDateTime(event.start.dateTime)}</td>
                  <td>{formatDateTime(event.end.dateTime)}</td>
                </tr>
              );
            })}
        </tbody>
      </Table>
    </div>
  );
}
```

Isso faz um loop pela coleção de eventos e adiciona uma linha de tabela para cada um. Salve as alterações e reinicie o aplicativo. Clique no link do **calendário** e o aplicativo agora deve renderizar uma tabela de eventos.

![Uma captura de tela da tabela de eventos](./images/add-msgraph-01.png)
