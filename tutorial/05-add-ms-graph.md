<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="fe806-101">Neste exercício, você incorporará o Microsoft Graph ao aplicativo.</span><span class="sxs-lookup"><span data-stu-id="fe806-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="fe806-102">Para este aplicativo, você usará a biblioteca [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para fazer chamadas para o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="fe806-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="fe806-103">Obter eventos de calendário do Outlook</span><span class="sxs-lookup"><span data-stu-id="fe806-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="fe806-104">Abra `./src/GraphService.ts` e adicione a função a seguir.</span><span class="sxs-lookup"><span data-stu-id="fe806-104">Open `./src/GraphService.ts` and add the following function.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/GraphService.ts" id="getUserWeekCalendarSnippet":::

    <span data-ttu-id="fe806-105">Considere o que este código está fazendo.</span><span class="sxs-lookup"><span data-stu-id="fe806-105">Consider what this code is doing.</span></span>

    - <span data-ttu-id="fe806-106">A URL que será chamada é `/me/calendarview` .</span><span class="sxs-lookup"><span data-stu-id="fe806-106">The URL that will be called is `/me/calendarview`.</span></span>
    - <span data-ttu-id="fe806-107">O método adiciona o header à solicitação, fazendo com que os horários na resposta sejam no fuso horário `header` `Prefer: outlook.timezone=""` preferido do usuário.</span><span class="sxs-lookup"><span data-stu-id="fe806-107">The `header` method adds the `Prefer: outlook.timezone=""` header to the request, causing the times in the response to be in the user's preferred time zone.</span></span>
    - <span data-ttu-id="fe806-108">O método adiciona os parâmetros e a janela de `query` tempo para o modo de `startDateTime` `endDateTime` exibição de calendário.</span><span class="sxs-lookup"><span data-stu-id="fe806-108">The `query` method adds the `startDateTime` and `endDateTime` parameters, defining the window of time for the calendar view.</span></span>
    - <span data-ttu-id="fe806-109">O `select` método limita os campos retornados para cada evento apenas àqueles que o modo de exibição realmente usará.</span><span class="sxs-lookup"><span data-stu-id="fe806-109">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
    - <span data-ttu-id="fe806-110">O método classifica os resultados pela data e hora em que foram criados, com `orderby` o item mais recente sendo o primeiro.</span><span class="sxs-lookup"><span data-stu-id="fe806-110">The `orderby` method sorts the results by the date and time they were created, with the most recent item being first.</span></span>
    - <span data-ttu-id="fe806-111">O `top` método limita os resultados em uma única página a 25 eventos.</span><span class="sxs-lookup"><span data-stu-id="fe806-111">The `top` method limits the results in a single page to 25 events.</span></span>
    - <span data-ttu-id="fe806-112">Se a resposta contiver um valor, indicando que há mais resultados disponíveis, um objeto será usado na página da coleção para obter todos `@odata.nextLink` `PageIterator` os resultados. [](https://docs.microsoft.com/graph/sdks/paging?tabs=typeScript)</span><span class="sxs-lookup"><span data-stu-id="fe806-112">If the response contains an `@odata.nextLink` value, indicating there are more results available, a `PageIterator` object is used to [page through the collection](https://docs.microsoft.com/graph/sdks/paging?tabs=typeScript) to get all of the results.</span></span>

1. <span data-ttu-id="fe806-113">Crie um componente React para exibir os resultados da chamada.</span><span class="sxs-lookup"><span data-stu-id="fe806-113">Create a React component to display the results of the call.</span></span> <span data-ttu-id="fe806-114">Crie um novo arquivo no `./src` diretório nomeado e adicione o código a `Calendar.tsx` seguir.</span><span class="sxs-lookup"><span data-stu-id="fe806-114">Create a new file in the `./src` directory named `Calendar.tsx` and add the following code.</span></span>

    ```typescript
    import React from 'react';
    import { NavLink as RouterNavLink } from 'react-router-dom';
    import { Table } from 'reactstrap';
    import moment, { Moment } from 'moment-timezone';
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
        if (this.props.user && !this.state.eventsLoaded)
        {
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
          catch (err) {
            this.props.setError('ERROR', JSON.stringify(err));
          }
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

    <span data-ttu-id="fe806-115">Por enquanto, isso apenas renderiza a matriz de eventos em JSON na página.</span><span class="sxs-lookup"><span data-stu-id="fe806-115">For now this just renders the array of events in JSON on the page.</span></span>

1. <span data-ttu-id="fe806-116">Adicione esse novo componente ao aplicativo.</span><span class="sxs-lookup"><span data-stu-id="fe806-116">Add this new component to the app.</span></span> <span data-ttu-id="fe806-117">Abra `./src/App.tsx` e adicione a `import` instrução a seguir na parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="fe806-117">Open `./src/App.tsx` and add the following `import` statement to the top of the file.</span></span>

    ```typescript
    import Calendar from './Calendar';
    ```

1. <span data-ttu-id="fe806-118">Adicione o seguinte componente logo após o `<Route>` existente.</span><span class="sxs-lookup"><span data-stu-id="fe806-118">Add the following component just after the existing `<Route>`.</span></span>

    ```typescript
    <Route exact path="/calendar"
      render={(props) =>
        this.props.isAuthenticated ?
          <Calendar {...props} /> :
          <Redirect to="/" />
      } />
    ```

1. <span data-ttu-id="fe806-119">Salve suas alterações e reinicie o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="fe806-119">Save your changes and restart the app.</span></span> <span data-ttu-id="fe806-120">Entre e clique no link **Calendário** na barra de inv.</span><span class="sxs-lookup"><span data-stu-id="fe806-120">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="fe806-121">Se tudo funcionar, você deverá ver um despejo JSON de eventos no calendário do usuário.</span><span class="sxs-lookup"><span data-stu-id="fe806-121">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="fe806-122">Exibir os resultados</span><span class="sxs-lookup"><span data-stu-id="fe806-122">Display the results</span></span>

<span data-ttu-id="fe806-123">Agora você pode atualizar o `Calendar` componente para exibir os eventos de maneira mais amigável.</span><span class="sxs-lookup"><span data-stu-id="fe806-123">Now you can update the `Calendar` component to display the events in a more user-friendly manner.</span></span>

1. <span data-ttu-id="fe806-124">Crie um novo arquivo no `./src` diretório nomeado e adicione o código a `Calendar.css` seguir.</span><span class="sxs-lookup"><span data-stu-id="fe806-124">Create a new file in the `./src` directory named `Calendar.css` and add the following code.</span></span>

    :::code language="css" source="../demo/graph-tutorial/src/Calendar.css":::

1. <span data-ttu-id="fe806-125">Crie um componente react para renderizar eventos em um único dia como linhas de tabela.</span><span class="sxs-lookup"><span data-stu-id="fe806-125">Create a React component to render events in a single day as table rows.</span></span> <span data-ttu-id="fe806-126">Crie um novo arquivo no `./src` diretório nomeado e adicione o código a `CalendarDayRow.tsx` seguir.</span><span class="sxs-lookup"><span data-stu-id="fe806-126">Create a new file in the `./src` directory named `CalendarDayRow.tsx` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/CalendarDayRow.tsx" id="CalendarDayRowSnippet":::

1. <span data-ttu-id="fe806-127">Adicione as instruções `import` a seguir na parte superior de **Calendar.tsx**.</span><span class="sxs-lookup"><span data-stu-id="fe806-127">Add the following `import` statements to the top of **Calendar.tsx**.</span></span>

    ```typescript
    import CalendarDayRow from './CalendarDayRow';
    import './Calendar.css';
    ```

1. <span data-ttu-id="fe806-128">Substitua a função `render` existente pela função a `./src/Calendar.tsx` seguir.</span><span class="sxs-lookup"><span data-stu-id="fe806-128">Replace the existing `render` function in `./src/Calendar.tsx` with the following function.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/Calendar.tsx" id="renderSnippet":::

    <span data-ttu-id="fe806-129">Isso divide os eventos em seus respectivos dias e renderiza uma seção de tabela para cada dia.</span><span class="sxs-lookup"><span data-stu-id="fe806-129">This splits the events into their respective days and renders a table section for each day.</span></span>

1. <span data-ttu-id="fe806-130">Salve as alterações e reinicie o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="fe806-130">Save the changes and restart the app.</span></span> <span data-ttu-id="fe806-131">Clique no link **Calendário** e o aplicativo deve renderizar uma tabela de eventos.</span><span class="sxs-lookup"><span data-stu-id="fe806-131">Click on the **Calendar** link and the app should now render a table of events.</span></span>

    ![Uma captura de tela da tabela de eventos](./images/add-msgraph-01.png)
