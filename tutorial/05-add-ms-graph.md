<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="5a5d9-101">Neste exercício, você incorporará o Microsoft Graph no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5a5d9-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="5a5d9-102">Para este aplicativo, você usará a biblioteca [Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="5a5d9-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="5a5d9-103">Obter eventos de calendário do Outlook</span><span class="sxs-lookup"><span data-stu-id="5a5d9-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="5a5d9-104">Abra `./src/GraphService.ts` e adicione a função a seguir.</span><span class="sxs-lookup"><span data-stu-id="5a5d9-104">Open `./src/GraphService.ts` and add the following function.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/GraphService.ts" id="getEventsSnippet":::

    <span data-ttu-id="5a5d9-105">Considere o que esse código está fazendo.</span><span class="sxs-lookup"><span data-stu-id="5a5d9-105">Consider what this code is doing.</span></span>

    - <span data-ttu-id="5a5d9-106">A URL que será chamada é `/me/events`.</span><span class="sxs-lookup"><span data-stu-id="5a5d9-106">The URL that will be called is `/me/events`.</span></span>
    - <span data-ttu-id="5a5d9-107">O `select` método limita os campos retornados para cada evento para apenas aqueles que o modo de exibição realmente usará.</span><span class="sxs-lookup"><span data-stu-id="5a5d9-107">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
    - <span data-ttu-id="5a5d9-108">O `orderby` método classifica os resultados pela data e hora em que foram criados, com o item mais recente em primeiro lugar.</span><span class="sxs-lookup"><span data-stu-id="5a5d9-108">The `orderby` method sorts the results by the date and time they were created, with the most recent item being first.</span></span>

1. <span data-ttu-id="5a5d9-109">Criar um componente reagir para exibir os resultados da chamada.</span><span class="sxs-lookup"><span data-stu-id="5a5d9-109">Create a React component to display the results of the call.</span></span> <span data-ttu-id="5a5d9-110">Crie um novo arquivo no `./src` diretório chamado `Calendar.tsx` e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="5a5d9-110">Create a new file in the `./src` directory named `Calendar.tsx` and add the following code.</span></span>

    ```typescript
    import React from 'react';
    import { Table } from 'reactstrap';
    import moment from 'moment';
    import { Event } from 'microsoft-graph';
    import { config } from './Config';
    import { getEvents } from './GraphService';
    import withAuthProvider, { AuthComponentProps } from './AuthProvider';

    interface CalendarState {
      events: Event[];
    }

    // Helper function to format Graph date/time
    function formatDateTime(dateTime: string | undefined) {
      if (dateTime !== undefined) {
        return moment.utc(dateTime).local().format('M/D/YY h:mm A');
      }
    }

    class Calendar extends React.Component<AuthComponentProps, CalendarState> {
      constructor(props: any) {
        super(props);

        this.state = {
          events: []
        };
      }

      async componentDidMount() {
        try {
          // Get the user's access token
          var accessToken = await this.props.getAccessToken(config.scopes);
          // Get the user's events
          var events = await getEvents(accessToken);
          // Update the array of events in state
          this.setState({events: events.value});
        }
        catch(err) {
          this.props.setError('ERROR', JSON.stringify(err));
        }
      }

      render() {
        return (
          <pre><code>{JSON.stringify(this.state.events, null, 2)}</code></pre>
        );
      }
    }

    export default withAuthProvider(Calendar);
    ```

    <span data-ttu-id="5a5d9-111">Por ora, isso apenas renderiza a matriz de eventos em JSON na página.</span><span class="sxs-lookup"><span data-stu-id="5a5d9-111">For now this just renders the array of events in JSON on the page.</span></span>

1. <span data-ttu-id="5a5d9-112">Adicione esse novo componente ao aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5a5d9-112">Add this new component to the app.</span></span> <span data-ttu-id="5a5d9-113">Abra `./src/App.tsx` e adicione a seguinte `import` instrução à parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="5a5d9-113">Open `./src/App.tsx` and add the following `import` statement to the top of the file.</span></span>

    ```typescript
    import Calendar from './Calendar';
    ```

1. <span data-ttu-id="5a5d9-114">Adicione o seguinte componente logo após o existente `<Route>`.</span><span class="sxs-lookup"><span data-stu-id="5a5d9-114">Add the following component just after the existing `<Route>`.</span></span>

    ```typescript
    <Route exact path="/calendar"
      render={(props) =>
        this.props.isAuthenticated ?
          <Calendar {...props} /> :
          <Redirect to="/" />
      } />
    ```

1. <span data-ttu-id="5a5d9-115">Salve suas alterações e reinicie o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5a5d9-115">Save your changes and restart the app.</span></span> <span data-ttu-id="5a5d9-116">Entre e clique no link **calendário** na barra de navegação.</span><span class="sxs-lookup"><span data-stu-id="5a5d9-116">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="5a5d9-117">Se tudo funcionar, você deverá ver um despejo JSON de eventos no calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="5a5d9-117">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="5a5d9-118">Exibir os resultados</span><span class="sxs-lookup"><span data-stu-id="5a5d9-118">Display the results</span></span>

<span data-ttu-id="5a5d9-119">Agora você pode atualizar o `Calendar` componente para exibir os eventos de forma mais amigável.</span><span class="sxs-lookup"><span data-stu-id="5a5d9-119">Now you can update the `Calendar` component to display the events in a more user-friendly manner.</span></span>

1. <span data-ttu-id="5a5d9-120">Substitua a função `render` existente na `./src/Calendar.js` função a seguir.</span><span class="sxs-lookup"><span data-stu-id="5a5d9-120">Replace the existing `render` function in `./src/Calendar.js` with the following function.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/Calendar.tsx" id="renderSnippet":::

    <span data-ttu-id="5a5d9-121">Isso faz um loop pela coleção de eventos e adiciona uma linha de tabela para cada um.</span><span class="sxs-lookup"><span data-stu-id="5a5d9-121">This loops through the collection of events and adds a table row for each one.</span></span>

1. <span data-ttu-id="5a5d9-122">Salve as alterações e reinicie o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5a5d9-122">Save the changes and restart the app.</span></span> <span data-ttu-id="5a5d9-123">Clique no link do **calendário** e o aplicativo agora deve renderizar uma tabela de eventos.</span><span class="sxs-lookup"><span data-stu-id="5a5d9-123">Click on the **Calendar** link and the app should now render a table of events.</span></span>

    ![Uma captura de tela da tabela de eventos](./images/add-msgraph-01.png)
