<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="2220c-101">Neste exercício, você incorporará o Microsoft Graph no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2220c-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="2220c-102">Para este aplicativo, você usará a biblioteca [Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="2220c-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="2220c-103">Obter eventos de calendário do Outlook</span><span class="sxs-lookup"><span data-stu-id="2220c-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="2220c-104">Abra `./src/GraphService.ts` e adicione a função a seguir.</span><span class="sxs-lookup"><span data-stu-id="2220c-104">Open `./src/GraphService.ts` and add the following function.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/GraphService.ts" id="getUserWeekCalendarSnippet":::

    <span data-ttu-id="2220c-105">Considere o que esse código está fazendo.</span><span class="sxs-lookup"><span data-stu-id="2220c-105">Consider what this code is doing.</span></span>

    - <span data-ttu-id="2220c-106">A URL que será chamada é `/me/calendarview` .</span><span class="sxs-lookup"><span data-stu-id="2220c-106">The URL that will be called is `/me/calendarview`.</span></span>
    - <span data-ttu-id="2220c-107">O `header` método adiciona o `Prefer: outlook.timezone=""` cabeçalho à solicitação, fazendo com que os horários na resposta estejam no fuso horário preferencial do usuário.</span><span class="sxs-lookup"><span data-stu-id="2220c-107">The `header` method adds the `Prefer: outlook.timezone=""` header to the request, causing the times in the response to be in the user's preferred time zone.</span></span>
    - <span data-ttu-id="2220c-108">O `query` método adiciona os `startDateTime` `endDateTime` parâmetros e, definindo a janela de tempo para o modo de exibição calendário.</span><span class="sxs-lookup"><span data-stu-id="2220c-108">The `query` method adds the `startDateTime` and `endDateTime` parameters, defining the window of time for the calendar view.</span></span>
    - <span data-ttu-id="2220c-109">O `select` método limita os campos retornados para cada evento para apenas aqueles que o modo de exibição realmente usará.</span><span class="sxs-lookup"><span data-stu-id="2220c-109">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
    - <span data-ttu-id="2220c-110">O `orderby` método classifica os resultados pela data e hora em que foram criados, com o item mais recente em primeiro lugar.</span><span class="sxs-lookup"><span data-stu-id="2220c-110">The `orderby` method sorts the results by the date and time they were created, with the most recent item being first.</span></span>
    - <span data-ttu-id="2220c-111">O `top` método limita os resultados para os primeiros eventos 50.</span><span class="sxs-lookup"><span data-stu-id="2220c-111">The `top` method limits the results to the first 50 events.</span></span>
    - <span data-ttu-id="2220c-112">Se a resposta contiver um `@odata.nextLink` valor, indicando que há mais resultados disponíveis, um `PageIterator` objeto é usado para [percorrer a coleção](https://docs.microsoft.com/graph/sdks/paging?tabs=typeScript) e obter todos os resultados.</span><span class="sxs-lookup"><span data-stu-id="2220c-112">If the response contains an `@odata.nextLink` value, indicating there are more results available, a `PageIterator` object is used to [page through the collection](https://docs.microsoft.com/graph/sdks/paging?tabs=typeScript) to get all of the results.</span></span>

1. <span data-ttu-id="2220c-113">Criar um componente reagir para exibir os resultados da chamada.</span><span class="sxs-lookup"><span data-stu-id="2220c-113">Create a React component to display the results of the call.</span></span> <span data-ttu-id="2220c-114">Crie um novo arquivo no `./src` diretório chamado `Calendar.tsx` e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="2220c-114">Create a new file in the `./src` directory named `Calendar.tsx` and add the following code.</span></span>

    ```typescript
    import React from 'react';
    import { NavLink as RouterNavLink } from 'react-router-dom';
    import { Table } from 'reactstrap';
    import moment from 'moment-timezone';
    import { findOneIana } from "windows-iana";
    import { Event } from 'microsoft-graph';
    import { config } from './Config';
    import { getUserWeekCalendar } from './GraphService';
    import withAuthProvider, { AuthComponentProps } from './AuthProvider';

    interface CalendarState {
      eventsLoaded: boolean;
      events: Event[];
      startOfWeek: Moment | undefined;
    }

    class Calendar extends React.Component<AuthComponentProps, CalendarState> {
      constructor(props: any) {
        super(props);

        this.state = {
          eventsLoaded: false,
          events: [],
          startOfWeek: undefined
        };
      }

      async componentDidUpdate() {
        try {
          // Get the user's access token
          var accessToken = await this.props.getAccessToken(config.scopes);
          // Convert user's Windows time zone ("Pacific Standard Time")
          // to IANA format ("America/Los_Angeles")
          // Moment needs IANA format
          var ianaTimeZone = findOneIana(this.props.user.timeZone);

          // Get midnight on the start of the current week in the user's timezone,
          // but in UTC. For example, for Pacific Standard Time, the time value would be
          // 07:00:00Z
          var startOfWeek = moment.tz(ianaTimeZone!.valueOf()).startOf('week').utc();

          // Get the user's events
          var events = await getUserWeekCalendar(accessToken, this.props.user.timeZone, startOfWeek);

          // Update the array of events in state
          this.setState({
            eventsLoaded: true,
            events: events,
            startOfWeek: startOfWeek
          });
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

    <span data-ttu-id="2220c-115">Por ora, isso apenas renderiza a matriz de eventos em JSON na página.</span><span class="sxs-lookup"><span data-stu-id="2220c-115">For now this just renders the array of events in JSON on the page.</span></span>

1. <span data-ttu-id="2220c-116">Adicione esse novo componente ao aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2220c-116">Add this new component to the app.</span></span> <span data-ttu-id="2220c-117">Abra `./src/App.tsx` e adicione a seguinte `import` instrução à parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="2220c-117">Open `./src/App.tsx` and add the following `import` statement to the top of the file.</span></span>

    ```typescript
    import Calendar from './Calendar';
    ```

1. <span data-ttu-id="2220c-118">Adicione o seguinte componente logo após o existente `<Route>` .</span><span class="sxs-lookup"><span data-stu-id="2220c-118">Add the following component just after the existing `<Route>`.</span></span>

    ```typescript
    <Route exact path="/calendar"
      render={(props) =>
        this.props.isAuthenticated ?
          <Calendar {...props} /> :
          <Redirect to="/" />
      } />
    ```

1. <span data-ttu-id="2220c-119">Salve suas alterações e reinicie o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2220c-119">Save your changes and restart the app.</span></span> <span data-ttu-id="2220c-120">Entre e clique no link **calendário** na barra de navegação.</span><span class="sxs-lookup"><span data-stu-id="2220c-120">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="2220c-121">Se tudo funcionar, você deverá ver um despejo JSON de eventos no calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="2220c-121">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="2220c-122">Exibir os resultados</span><span class="sxs-lookup"><span data-stu-id="2220c-122">Display the results</span></span>

<span data-ttu-id="2220c-123">Agora você pode atualizar o `Calendar` componente para exibir os eventos de forma mais amigável.</span><span class="sxs-lookup"><span data-stu-id="2220c-123">Now you can update the `Calendar` component to display the events in a more user-friendly manner.</span></span>

1. <span data-ttu-id="2220c-124">Crie um novo arquivo no `./src` diretório chamado `Calendar.css` e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="2220c-124">Create a new file in the `./src` directory named `Calendar.css` and add the following code.</span></span>

    :::code language="css" source="../demo/graph-tutorial/src/Calendar.css":::

1. <span data-ttu-id="2220c-125">Criar um componente reagir para renderizar eventos em um único dia como linhas de tabela.</span><span class="sxs-lookup"><span data-stu-id="2220c-125">Create a React component to render events in a single day as table rows.</span></span> <span data-ttu-id="2220c-126">Crie um novo arquivo no `./src` diretório chamado `CalendarDayRow.tsx` e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="2220c-126">Create a new file in the `./src` directory named `CalendarDayRow.tsx` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/CalendarDayRow.tsx" id="CalendarDayRowSnippet":::

1. <span data-ttu-id="2220c-127">Adicione as seguintes `import` instruções à parte superior de **Calendar. TSX**.</span><span class="sxs-lookup"><span data-stu-id="2220c-127">Add the following `import` statements to the top of **Calendar.tsx**.</span></span>

    ```typescript
    import CalendarDayRow from './CalendarDayRow';
    import './Calendar.css';
    ```

1. <span data-ttu-id="2220c-128">Substitua a `render` função existente na `./src/Calendar.tsx` função a seguir.</span><span class="sxs-lookup"><span data-stu-id="2220c-128">Replace the existing `render` function in `./src/Calendar.tsx` with the following function.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/Calendar.tsx" id="renderSnippet":::

    <span data-ttu-id="2220c-129">Isso divide os eventos em seus respectivos dias e renderiza uma seção de tabela para cada dia.</span><span class="sxs-lookup"><span data-stu-id="2220c-129">This splits the events into their respective days and renders a table section for each day.</span></span>

1. <span data-ttu-id="2220c-130">Salve as alterações e reinicie o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2220c-130">Save the changes and restart the app.</span></span> <span data-ttu-id="2220c-131">Clique no link do **calendário** e o aplicativo agora deve renderizar uma tabela de eventos.</span><span class="sxs-lookup"><span data-stu-id="2220c-131">Click on the **Calendar** link and the app should now render a table of events.</span></span>

    ![Uma captura de tela da tabela de eventos](./images/add-msgraph-01.png)
