<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="1e030-101">Nesta seção, você criará um novo aplicativo React.</span><span class="sxs-lookup"><span data-stu-id="1e030-101">In this section you'll create a new React app.</span></span>

1. <span data-ttu-id="1e030-102">Abra sua CLI (interface de linha de comando), navegue até um diretório onde você tenha direitos para criar arquivos e execute os seguintes comandos para criar um novo aplicativo React.</span><span class="sxs-lookup"><span data-stu-id="1e030-102">Open your command-line interface (CLI), navigate to a directory where you have rights to create files, and run the following commands to create a new React app.</span></span>

    ```Shell
    npx create-react-app@4.0.1 graph-tutorial --template typescript
    ```

1. <span data-ttu-id="1e030-103">Depois que o comando terminar, mude para o diretório em sua CLI e execute o `graph-tutorial` seguinte comando para iniciar um servidor Web local.</span><span class="sxs-lookup"><span data-stu-id="1e030-103">Once the command finishes, change to the `graph-tutorial` directory in your CLI and run the following command to start a local web server.</span></span>

    ```Shell
    yarn start
    ```

    > [!NOTE]
    > <span data-ttu-id="1e030-104">Se você não tiver [o Yarn](https://yarnpkg.com/) instalado, poderá `npm start` usá-lo.</span><span class="sxs-lookup"><span data-stu-id="1e030-104">If you do not have [Yarn](https://yarnpkg.com/) installed, you can use `npm start` instead.</span></span>

<span data-ttu-id="1e030-105">O navegador padrão é aberto [https://localhost:3000/](https://localhost:3000) com uma página padrão do React.</span><span class="sxs-lookup"><span data-stu-id="1e030-105">Your default browser opens to [https://localhost:3000/](https://localhost:3000) with a default React page.</span></span> <span data-ttu-id="1e030-106">Se o navegador não abrir, abra-o e procure [https://localhost:3000/](https://localhost:3000) verificar se o novo aplicativo funciona.</span><span class="sxs-lookup"><span data-stu-id="1e030-106">If your browser doesn't open, open it and browse to [https://localhost:3000/](https://localhost:3000) to verify that the new app works.</span></span>

## <a name="add-node-packages"></a><span data-ttu-id="1e030-107">Adicionar pacotes de nó</span><span class="sxs-lookup"><span data-stu-id="1e030-107">Add Node packages</span></span>

<span data-ttu-id="1e030-108">Antes de continuar, instale alguns pacotes adicionais que você usará mais tarde:</span><span class="sxs-lookup"><span data-stu-id="1e030-108">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="1e030-109">[react-router-dom](https://github.com/ReactTraining/react-router) para roteamento declarativo dentro do aplicativo React.</span><span class="sxs-lookup"><span data-stu-id="1e030-109">[react-router-dom](https://github.com/ReactTraining/react-router) for declarative routing inside the React app.</span></span>
- <span data-ttu-id="1e030-110">[bootstrap](https://github.com/twbs/bootstrap) para estilo e componentes comuns.</span><span class="sxs-lookup"><span data-stu-id="1e030-110">[bootstrap](https://github.com/twbs/bootstrap) for styling and common components.</span></span>
- <span data-ttu-id="1e030-111">[reactstrap para](https://github.com/reactstrap/reactstrap) componentes react com base no Bootstrap.</span><span class="sxs-lookup"><span data-stu-id="1e030-111">[reactstrap](https://github.com/reactstrap/reactstrap) for React components based on Bootstrap.</span></span>
- <span data-ttu-id="1e030-112">[fontawesome-free](https://github.com/FortAwesome/Font-Awesome) para ícones.</span><span class="sxs-lookup"><span data-stu-id="1e030-112">[fontawesome-free](https://github.com/FortAwesome/Font-Awesome) for icons.</span></span>
- <span data-ttu-id="1e030-113">[momento](https://github.com/moment/moment) para formatar datas e horas.</span><span class="sxs-lookup"><span data-stu-id="1e030-113">[moment](https://github.com/moment/moment) for formatting dates and times.</span></span>
- <span data-ttu-id="1e030-114">[windows-iana para](https://github.com/rubenillodo/windows-iana) traduzir fusos horário do Windows para o formato IANA.</span><span class="sxs-lookup"><span data-stu-id="1e030-114">[windows-iana](https://github.com/rubenillodo/windows-iana) for translating Windows time zones to IANA format.</span></span>
- <span data-ttu-id="1e030-115">[msal-browser](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser) para autenticação no Azure Active Directory e recuperação de tokens de acesso.</span><span class="sxs-lookup"><span data-stu-id="1e030-115">[msal-browser](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser) for authenticating to Azure Active Directory and retrieving access tokens.</span></span>
- <span data-ttu-id="1e030-116">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="1e030-116">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>

<span data-ttu-id="1e030-117">Execute o seguinte comando em sua CLI.</span><span class="sxs-lookup"><span data-stu-id="1e030-117">Run the following command in your CLI.</span></span>

```Shell
yarn add react-router-dom@5.2.0 @types/react-router-dom@5.1.7 bootstrap@4.6.0 reactstrap@8.9.0 @types/reactstrap@8.7.2 @fortawesome/fontawesome-free@5.15.2
yarn add moment@2.29.1 moment-timezone@0.5.32 windows-iana@4.2.1 @azure/msal-browser@2.10.0 @microsoft/microsoft-graph-client@2.2.1 @types/microsoft-graph@1.28.0
```

## <a name="design-the-app"></a><span data-ttu-id="1e030-118">Projetar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="1e030-118">Design the app</span></span>

<span data-ttu-id="1e030-119">Comece criando uma barra de ferramentas para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1e030-119">Start by creating a navbar for the app.</span></span>

1. <span data-ttu-id="1e030-120">Crie um novo arquivo no `./src` diretório nomeado e adicione o código a `NavBar.tsx` seguir.</span><span class="sxs-lookup"><span data-stu-id="1e030-120">Create a new file in the `./src` directory named `NavBar.tsx` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/NavBar.tsx" id="NavBarSnippet":::

1. <span data-ttu-id="1e030-121">Crie uma home page para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1e030-121">Create a home page for the app.</span></span> <span data-ttu-id="1e030-122">Crie um novo arquivo no `./src` diretório nomeado e adicione o código a `Welcome.tsx` seguir.</span><span class="sxs-lookup"><span data-stu-id="1e030-122">Create a new file in the `./src` directory named `Welcome.tsx` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/Welcome.tsx" id="WelcomeSnippet":::

1. <span data-ttu-id="1e030-123">Crie uma exibição de mensagem de erro para exibir mensagens para o usuário.</span><span class="sxs-lookup"><span data-stu-id="1e030-123">Create an error message display to display messages to the user.</span></span> <span data-ttu-id="1e030-124">Crie um novo arquivo no `./src` diretório nomeado e adicione o código a `ErrorMessage.tsx` seguir.</span><span class="sxs-lookup"><span data-stu-id="1e030-124">Create a new file in the `./src` directory named `ErrorMessage.tsx` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/ErrorMessage.tsx" id="ErrorMessageSnippet":::

1. <span data-ttu-id="1e030-125">Abra o arquivo `./src/index.css` e substitua o conteúdo inteiro pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="1e030-125">Open the `./src/index.css` file and replace its entire contents with the following.</span></span>

    :::code language="css" source="../demo/graph-tutorial/src/index.css":::

1. <span data-ttu-id="1e030-126">Abra `./src/App.tsx` e substitua todo o conteúdo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="1e030-126">Open `./src/App.tsx` and replace its entire contents with the following.</span></span>

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

1. <span data-ttu-id="1e030-127">Salve todas as suas alterações e reinicie o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1e030-127">Save all of your changes and restart the app.</span></span> <span data-ttu-id="1e030-128">Agora, o aplicativo deve parecer muito diferente.</span><span class="sxs-lookup"><span data-stu-id="1e030-128">Now, the app should look very different.</span></span>

    ![Uma captura de tela da home page reprojetada](images/create-app-01.png)
