<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você incorporará o Microsoft Graph ao aplicativo. Para este aplicativo, você usará a biblioteca [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para fazer chamadas para o Microsoft Graph.

## <a name="get-calendar-events-from-outlook"></a>Obter eventos de calendário do Outlook

1. Abra `./src/GraphService.ts` e adicione a função a seguir.

    :::code language="typescript" source="../demo/graph-tutorial/src/GraphService.ts" id="getUserWeekCalendarSnippet":::

    Considere o que este código está fazendo.

    - A URL que será chamada é `/me/calendarview` .
    - O método adiciona o header à solicitação, fazendo com que os horários na resposta sejam no fuso horário `header` `Prefer: outlook.timezone=""` preferido do usuário.
    - O método adiciona os parâmetros e a janela de `query` tempo para o modo de `startDateTime` `endDateTime` exibição de calendário.
    - O `select` método limita os campos retornados para cada evento apenas àqueles que o modo de exibição realmente usará.
    - O método classifica os resultados pela data e hora em que foram criados, com `orderby` o item mais recente sendo o primeiro.
    - O `top` método limita os resultados em uma única página a 25 eventos.
    - Se a resposta contiver um valor, indicando que há mais resultados disponíveis, um objeto será usado na página da coleção para obter todos `@odata.nextLink` `PageIterator` os resultados. [](https://docs.microsoft.com/graph/sdks/paging?tabs=typeScript)

1. Crie um componente React para exibir os resultados da chamada. Crie um novo arquivo no `./src` diretório nomeado e adicione o código a `Calendar.tsx` seguir.

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

    Por enquanto, isso apenas renderiza a matriz de eventos em JSON na página.

1. Adicione esse novo componente ao aplicativo. Abra `./src/App.tsx` e adicione a `import` instrução a seguir na parte superior do arquivo.

    ```typescript
    import Calendar from './Calendar';
    ```

1. Adicione o seguinte componente logo após o `<Route>` existente.

    ```typescript
    <Route exact path="/calendar"
      render={(props) =>
        this.props.isAuthenticated ?
          <Calendar {...props} /> :
          <Redirect to="/" />
      } />
    ```

1. Salve suas alterações e reinicie o aplicativo. Entre e clique no link **Calendário** na barra de inv. Se tudo funcionar, você deverá ver um despejo JSON de eventos no calendário do usuário.

## <a name="display-the-results"></a>Exibir os resultados

Agora você pode atualizar o `Calendar` componente para exibir os eventos de maneira mais amigável.

1. Crie um novo arquivo no `./src` diretório nomeado e adicione o código a `Calendar.css` seguir.

    :::code language="css" source="../demo/graph-tutorial/src/Calendar.css":::

1. Crie um componente react para renderizar eventos em um único dia como linhas de tabela. Crie um novo arquivo no `./src` diretório nomeado e adicione o código a `CalendarDayRow.tsx` seguir.

    :::code language="typescript" source="../demo/graph-tutorial/src/CalendarDayRow.tsx" id="CalendarDayRowSnippet":::

1. Adicione as instruções `import` a seguir na parte superior de **Calendar.tsx**.

    ```typescript
    import CalendarDayRow from './CalendarDayRow';
    import './Calendar.css';
    ```

1. Substitua a função `render` existente pela função a `./src/Calendar.tsx` seguir.

    :::code language="typescript" source="../demo/graph-tutorial/src/Calendar.tsx" id="renderSnippet":::

    Isso divide os eventos em seus respectivos dias e renderiza uma seção de tabela para cada dia.

1. Salve as alterações e reinicie o aplicativo. Clique no link **Calendário** e o aplicativo deve renderizar uma tabela de eventos.

    ![Uma captura de tela da tabela de eventos](./images/add-msgraph-01.png)
