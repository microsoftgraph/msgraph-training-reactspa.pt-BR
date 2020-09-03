<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você incorporará o Microsoft Graph no aplicativo. Para este aplicativo, você usará a biblioteca [Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para fazer chamadas para o Microsoft Graph.

## <a name="get-calendar-events-from-outlook"></a>Obter eventos de calendário do Outlook

1. Abra `./src/GraphService.ts` e adicione a função a seguir.

    :::code language="typescript" source="../demo/graph-tutorial/src/GraphService.ts" id="getUserWeekCalendarSnippet":::

    Considere o que esse código está fazendo.

    - A URL que será chamada é `/me/calendarview` .
    - O `header` método adiciona o `Prefer: outlook.timezone=""` cabeçalho à solicitação, fazendo com que os horários na resposta estejam no fuso horário preferencial do usuário.
    - O `query` método adiciona os `startDateTime` `endDateTime` parâmetros e, definindo a janela de tempo para o modo de exibição calendário.
    - O `select` método limita os campos retornados para cada evento para apenas aqueles que o modo de exibição realmente usará.
    - O `orderby` método classifica os resultados pela data e hora em que foram criados, com o item mais recente em primeiro lugar.
    - O `top` método limita os resultados para os primeiros eventos 50.
    - Se a resposta contiver um `@odata.nextLink` valor, indicando que há mais resultados disponíveis, um `PageIterator` objeto é usado para [percorrer a coleção](https://docs.microsoft.com/graph/sdks/paging?tabs=typeScript) e obter todos os resultados.

1. Criar um componente reagir para exibir os resultados da chamada. Crie um novo arquivo no `./src` diretório chamado `Calendar.tsx` e adicione o código a seguir.

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

    Por ora, isso apenas renderiza a matriz de eventos em JSON na página.

1. Adicione esse novo componente ao aplicativo. Abra `./src/App.tsx` e adicione a seguinte `import` instrução à parte superior do arquivo.

    ```typescript
    import Calendar from './Calendar';
    ```

1. Adicione o seguinte componente logo após o existente `<Route>` .

    ```typescript
    <Route exact path="/calendar"
      render={(props) =>
        this.props.isAuthenticated ?
          <Calendar {...props} /> :
          <Redirect to="/" />
      } />
    ```

1. Salve suas alterações e reinicie o aplicativo. Entre e clique no link **calendário** na barra de navegação. Se tudo funcionar, você deverá ver um despejo JSON de eventos no calendário do usuário.

## <a name="display-the-results"></a>Exibir os resultados

Agora você pode atualizar o `Calendar` componente para exibir os eventos de forma mais amigável.

1. Crie um novo arquivo no `./src` diretório chamado `Calendar.css` e adicione o código a seguir.

    :::code language="css" source="../demo/graph-tutorial/src/Calendar.css":::

1. Criar um componente reagir para renderizar eventos em um único dia como linhas de tabela. Crie um novo arquivo no `./src` diretório chamado `CalendarDayRow.tsx` e adicione o código a seguir.

    :::code language="typescript" source="../demo/graph-tutorial/src/CalendarDayRow.tsx" id="CalendarDayRowSnippet":::

1. Adicione as seguintes `import` instruções à parte superior de **Calendar. TSX**.

    ```typescript
    import CalendarDayRow from './CalendarDayRow';
    import './Calendar.css';
    ```

1. Substitua a `render` função existente na `./src/Calendar.tsx` função a seguir.

    :::code language="typescript" source="../demo/graph-tutorial/src/Calendar.tsx" id="renderSnippet":::

    Isso divide os eventos em seus respectivos dias e renderiza uma seção de tabela para cada dia.

1. Salve as alterações e reinicie o aplicativo. Clique no link do **calendário** e o aplicativo agora deve renderizar uma tabela de eventos.

    ![Uma captura de tela da tabela de eventos](./images/add-msgraph-01.png)
