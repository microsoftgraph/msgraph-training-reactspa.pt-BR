<!-- markdownlint-disable MD002 MD041 -->

Nesta seção, você adicionará a capacidade de criar eventos no calendário do usuário.

## <a name="add-method-to-graphservice"></a>Adicionar método a GraphService

1. Abra **./src/GraphService.TS** e adicione a função a seguir para criar um novo evento.

    :::code language="typescript" source="../demo/graph-tutorial/src/GraphService.ts" id="createEventSnippet":::

## <a name="create-new-event-form"></a>Criar novo formulário de eventos

1. Crie um novo arquivo no diretório **./src** chamado **NewEvent. TSX** e adicione o código a seguir.

    :::code language="typescript" source="../demo/graph-tutorial/src/NewEvent.tsx" id="NewEventSnippet":::

1. Abra **./src/app.TSX** e adicione a instrução a seguir `import` à parte superior do arquivo.

    ```typescript
    import NewEvent from './NewEvent';
    ```

1. Adicione uma nova rota ao novo formulário de evento. Adicione o seguinte código logo após os outros `Route` elementos.

    ```typescript
    <Route exact path="/newevent"
      render={(props) =>
        this.props.isAuthenticated ?
          <NewEvent {...props} /> :
          <Redirect to="/" />
      } />
    ```

    Agora, a `return` instrução completa deve ter a seguinte aparência.

    :::code language="typescript" source="../demo/graph-tutorial/src/App.tsx" id="renderSnippet" highlight="23-28":::

1. Atualize o aplicativo e navegue até o modo de exibição calendário. Clique no botão **novo evento** . Preencha os campos e clique em **criar**.

    ![Uma captura de tela do novo formulário de evento](./images/create-event-01.png)
