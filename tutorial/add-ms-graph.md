<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="52598-101">Neste exercício, você incorporará o Microsoft Graph no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="52598-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="52598-102">Para este aplicativo, você usará a biblioteca [Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="52598-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="52598-103">Obter eventos de calendário do Outlook</span><span class="sxs-lookup"><span data-stu-id="52598-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="52598-104">Comece adicionando um novo método ao `./src/GraphService.js` arquivo para obter os eventos do calendário.</span><span class="sxs-lookup"><span data-stu-id="52598-104">Start by adding a new method to the `./src/GraphService.js` file to get the events from the calendar.</span></span> <span data-ttu-id="52598-105">Adicione a função a seguir.</span><span class="sxs-lookup"><span data-stu-id="52598-105">Add the following function.</span></span>

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

<span data-ttu-id="52598-106">Considere o que esse código está fazendo.</span><span class="sxs-lookup"><span data-stu-id="52598-106">Consider what this code is doing.</span></span>

- <span data-ttu-id="52598-107">A URL que será chamada é `/me/events`.</span><span class="sxs-lookup"><span data-stu-id="52598-107">The URL that will be called is `/me/events`.</span></span>
- <span data-ttu-id="52598-108">O `select` método limita os campos retornados para cada evento para apenas aqueles que o modo de exibição realmente usará.</span><span class="sxs-lookup"><span data-stu-id="52598-108">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
- <span data-ttu-id="52598-109">O `orderby` método classifica os resultados pela data e hora em que foram criados, com o item mais recente em primeiro lugar.</span><span class="sxs-lookup"><span data-stu-id="52598-109">The `orderby` method sorts the results by the date and time they were created, with the most recent item being first.</span></span>

<span data-ttu-id="52598-110">Agora, crie um componente reagir para exibir os resultados da chamada.</span><span class="sxs-lookup"><span data-stu-id="52598-110">Now create a React component to display the results of the call.</span></span> <span data-ttu-id="52598-111">Crie um novo arquivo no `./src` diretório chamado `Calendar.js` e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="52598-111">Create a new file in the `./src` directory named `Calendar.js` and add the following code.</span></span>

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
      var accessToken = await window.msal.acquireTokenSilent(config.scopes);
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

<span data-ttu-id="52598-112">Por ora, isso apenas renderiza a matriz de eventos em JSON na página.</span><span class="sxs-lookup"><span data-stu-id="52598-112">For now this just renders the array of events in JSON on the page.</span></span> <span data-ttu-id="52598-113">Adicione esse novo componente ao aplicativo.</span><span class="sxs-lookup"><span data-stu-id="52598-113">Add this new component to the app.</span></span> <span data-ttu-id="52598-114">Abra `./src/App.js` e adicione a seguinte `import` instrução à parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="52598-114">Open `./src/App.js` and add the following `import` statement to the top of the file.</span></span>

```js
import Calendar from './Calendar';
```

<span data-ttu-id="52598-115">Em seguida, adicione o seguinte componente logo após `<Route>`o existente.</span><span class="sxs-lookup"><span data-stu-id="52598-115">Then add the following component just after the existing `<Route>`.</span></span>

```JSX
<Route exact path="/calendar"
  render={(props) =>
    <Calendar {...props}
      showError={this.setErrorMessage.bind(this)} />
  } />
```

<span data-ttu-id="52598-116">Salve suas alterações e reinicie o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="52598-116">Save your changes and restart the app.</span></span> <span data-ttu-id="52598-117">Entre e clique no link **calendário** na barra de navegação.</span><span class="sxs-lookup"><span data-stu-id="52598-117">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="52598-118">Se tudo funcionar, você deverá ver um despejo JSON de eventos no calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="52598-118">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="52598-119">Exibir os resultados</span><span class="sxs-lookup"><span data-stu-id="52598-119">Display the results</span></span>

<span data-ttu-id="52598-120">Agora você pode atualizar o `Calendar` componente para exibir os eventos de forma mais amigável.</span><span class="sxs-lookup"><span data-stu-id="52598-120">Now you can update the `Calendar` component to display the events in a more user-friendly manner.</span></span> <span data-ttu-id="52598-121">Substitua a função `render` existente na `./src/Calendar.js` função a seguir.</span><span class="sxs-lookup"><span data-stu-id="52598-121">Replace the existing `render` function in `./src/Calendar.js` with the following function.</span></span>

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
            function(event, index){
              return(
                <tr>
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

<span data-ttu-id="52598-122">Isso faz um loop pela coleção de eventos e adiciona uma linha de tabela para cada um.</span><span class="sxs-lookup"><span data-stu-id="52598-122">This loops through the collection of events and adds a table row for each one.</span></span> <span data-ttu-id="52598-123">Salve as alterações e reinicie o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="52598-123">Save the changes and restart the app.</span></span> <span data-ttu-id="52598-124">Clique no link do **calendário** e o aplicativo agora deve renderizar uma tabela de eventos.</span><span class="sxs-lookup"><span data-stu-id="52598-124">Click on the **Calendar** link and the app should now render a table of events.</span></span>

![Uma captura de tela da tabela de eventos](./images/add-msgraph-01.png)