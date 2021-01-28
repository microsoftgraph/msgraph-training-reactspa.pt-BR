<!-- markdownlint-disable MD002 MD041 -->

Nesta seção, você criará um novo aplicativo React.

1. Abra sua CLI (interface de linha de comando), navegue até um diretório onde você tenha direitos para criar arquivos e execute os seguintes comandos para criar um novo aplicativo React.

    ```Shell
    npx create-react-app@4.0.1 graph-tutorial --template typescript
    ```

1. Depois que o comando terminar, mude para o diretório em sua CLI e execute o `graph-tutorial` seguinte comando para iniciar um servidor Web local.

    ```Shell
    yarn start
    ```

    > [!NOTE]
    > Se você não tiver [o Yarn](https://yarnpkg.com/) instalado, poderá `npm start` usá-lo.

O navegador padrão é aberto [https://localhost:3000/](https://localhost:3000) com uma página padrão do React. Se o navegador não abrir, abra-o e procure [https://localhost:3000/](https://localhost:3000) verificar se o novo aplicativo funciona.

## <a name="add-node-packages"></a>Adicionar pacotes de nó

Antes de continuar, instale alguns pacotes adicionais que você usará mais tarde:

- [react-router-dom](https://github.com/ReactTraining/react-router) para roteamento declarativo dentro do aplicativo React.
- [bootstrap](https://github.com/twbs/bootstrap) para estilo e componentes comuns.
- [reactstrap para](https://github.com/reactstrap/reactstrap) componentes react com base no Bootstrap.
- [fontawesome-free](https://github.com/FortAwesome/Font-Awesome) para ícones.
- [momento](https://github.com/moment/moment) para formatar datas e horas.
- [windows-iana para](https://github.com/rubenillodo/windows-iana) traduzir fusos horário do Windows para o formato IANA.
- [msal-browser](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser) para autenticação no Azure Active Directory e recuperação de tokens de acesso.
- [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.

Execute o seguinte comando em sua CLI.

```Shell
yarn add react-router-dom@5.2.0 @types/react-router-dom@5.1.7 bootstrap@4.6.0 reactstrap@8.9.0 @types/reactstrap@8.7.2 @fortawesome/fontawesome-free@5.15.2
yarn add moment@2.29.1 moment-timezone@0.5.32 windows-iana@4.2.1 @azure/msal-browser@2.10.0 @microsoft/microsoft-graph-client@2.2.1 @types/microsoft-graph@1.28.0
```

## <a name="design-the-app"></a>Projetar o aplicativo

Comece criando uma barra de ferramentas para o aplicativo.

1. Crie um novo arquivo no `./src` diretório nomeado e adicione o código a `NavBar.tsx` seguir.

    :::code language="typescript" source="../demo/graph-tutorial/src/NavBar.tsx" id="NavBarSnippet":::

1. Crie uma home page para o aplicativo. Crie um novo arquivo no `./src` diretório nomeado e adicione o código a `Welcome.tsx` seguir.

    :::code language="typescript" source="../demo/graph-tutorial/src/Welcome.tsx" id="WelcomeSnippet":::

1. Crie uma exibição de mensagem de erro para exibir mensagens para o usuário. Crie um novo arquivo no `./src` diretório nomeado e adicione o código a `ErrorMessage.tsx` seguir.

    :::code language="typescript" source="../demo/graph-tutorial/src/ErrorMessage.tsx" id="ErrorMessageSnippet":::

1. Abra o arquivo `./src/index.css` e substitua o conteúdo inteiro pelo seguinte:

    :::code language="css" source="../demo/graph-tutorial/src/index.css":::

1. Abra `./src/App.tsx` e substitua todo o conteúdo pelo seguinte.

    ```typescript
    import React, { Component } from 'react';
    import { BrowserRouter as Router, Route, Redirect } from 'react-router-dom';
    import { Container } from 'reactstrap';
    import NavBar from './NavBar';
    import ErrorMessage from './ErrorMessage';
    import Welcome from './Welcome';
    import 'bootstrap/dist/css/bootstrap.css';

    class App extends Component<any> {
      render() {
        let error = null;
        if (this.props.error) {
          error = <ErrorMessage
            message={this.props.error.message}
            debug={this.props.error.debug} />;
        }

        return (
          <Router>
            <div>
              <NavBar
                isAuthenticated={this.props.isAuthenticated}
                authButtonMethod={this.props.isAuthenticated ? this.props.logout : this.props.login}
                user={this.props.user} />
              <Container>
                {error}
                <Route exact path="/"
                  render={(props) =>
                    <Welcome {...props}
                      isAuthenticated={this.props.isAuthenticated}
                      user={this.props.user}
                      authButtonMethod={this.props.login} />
                  } />
              </Container>
            </div>
          </Router>
        );
      }
    }

    export default App;
    ```

1. Salve todas as suas alterações e reinicie o aplicativo. Agora, o aplicativo deve parecer muito diferente.

    ![Uma captura de tela da home page reprojetada](images/create-app-01.png)
