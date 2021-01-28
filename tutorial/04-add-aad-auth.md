<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="84674-101">Neste exercício, você estenderá o aplicativo do exercício anterior para dar suporte à autenticação com o Azure AD.</span><span class="sxs-lookup"><span data-stu-id="84674-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="84674-102">Isso é necessário para obter o token de acesso OAuth necessário para chamar o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="84674-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="84674-103">Nesta etapa, você integrará a biblioteca da Biblioteca de [Autenticação](https://github.com/AzureAD/microsoft-authentication-library-for-js) da Microsoft ao aplicativo.</span><span class="sxs-lookup"><span data-stu-id="84674-103">In this step you will integrate the [Microsoft Authentication Library](https://github.com/AzureAD/microsoft-authentication-library-for-js) library into the application.</span></span>

1. <span data-ttu-id="84674-104">Crie um novo arquivo no `./src` diretório nomeado e adicione o código a `Config.ts` seguir.</span><span class="sxs-lookup"><span data-stu-id="84674-104">Create a new file in the `./src` directory named `Config.ts` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/Config.example.ts":::

    <span data-ttu-id="84674-105">Substitua `YOUR_APP_ID_HERE` pela ID do aplicativo do Portal de Registro de Aplicativos.</span><span class="sxs-lookup"><span data-stu-id="84674-105">Replace `YOUR_APP_ID_HERE` with the application ID from the Application Registration Portal.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="84674-106">Se você estiver usando o controle de código-fonte, como git, agora seria um bom momento para excluir o arquivo do controle de código-fonte para evitar o vazamento `Config.ts` inadvertida da ID do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="84674-106">If you're using source control such as git, now would be a good time to exclude the `Config.ts` file from source control to avoid inadvertently leaking your app ID.</span></span>

## <a name="implement-sign-in"></a><span data-ttu-id="84674-107">Implementar a login</span><span class="sxs-lookup"><span data-stu-id="84674-107">Implement sign-in</span></span>

<span data-ttu-id="84674-108">Nesta seção, você criará um provedor de autenticação e implementará o login e a saída.</span><span class="sxs-lookup"><span data-stu-id="84674-108">In this section you'll create an authentication provider and implement sign-in and sign-out.</span></span>

1. <span data-ttu-id="84674-109">Crie um novo arquivo no `./src` diretório nomeado e adicione o código a `AuthProvider.tsx` seguir.</span><span class="sxs-lookup"><span data-stu-id="84674-109">Create a new file in the `./src` directory named `AuthProvider.tsx` and add the following code.</span></span>

    ```typescript
    import React from 'react';
    import { PublicClientApplication } from '@azure/msal-browser';

    import { config } from './Config';

    export interface AuthComponentProps {
      error: any;
      isAuthenticated: boolean;
      user: any;
      login: Function;
      logout: Function;
      getAccessToken: Function;
      setError: Function;
    }

    interface AuthProviderState {
      error: any;
      isAuthenticated: boolean;
      user: any;
    }

    export default function withAuthProvider<T extends React.Component<AuthComponentProps>>
      (WrappedComponent: new (props: AuthComponentProps, context?: any) => T): React.ComponentClass {
      return class extends React.Component<any, AuthProviderState> {
        private publicClientApplication: PublicClientApplication;

        constructor(props: any) {
          super(props);
          this.state = {
            error: null,
            isAuthenticated: false,
            user: {}
          };

          // Initialize the MSAL application object
          this.publicClientApplication = new PublicClientApplication({
            auth: {
              clientId: config.appId,
              redirectUri: config.redirectUri
            },
            cache: {
              cacheLocation: "sessionStorage",
              storeAuthStateInCookie: true
            }
          });
        }

        componentDidMount() {
          // If MSAL already has an account, the user
          // is already logged in
          const accounts = this.publicClientApplication.getAllAccounts();

          if (accounts && accounts.length > 0) {
            // Enhance user object with data from Graph
            this.getUserProfile();
          }
        }

        render() {
          return <WrappedComponent
            error={ this.state.error }
            isAuthenticated={ this.state.isAuthenticated }
            user={ this.state.user }
            login={ () => this.login() }
            logout={ () => this.logout() }
            getAccessToken={ (scopes: string[]) => this.getAccessToken(scopes) }
            setError={ (message: string, debug: string) => this.setErrorMessage(message, debug) }
            { ...this.props } />;
        }

        async login() {
          try {
            // Login via popup
            await this.publicClientApplication.loginPopup(
              {
                scopes: config.scopes,
                prompt: "select_account"
              });

            // After login, get the user's profile
            await this.getUserProfile();
          }
          catch (err) {
            this.setState({
              isAuthenticated: false,
              user: {},
              error: this.normalizeError(err)
            });
          }
        }

        logout() {
          this.publicClientApplication.logout();
        }

        async getAccessToken(scopes: string[]): Promise<string> {
          try {
            const accounts = this.publicClientApplication
              .getAllAccounts();

            if (accounts.length <= 0) throw new Error('login_required');
            // Get the access token silently
            // If the cache contains a non-expired token, this function
            // will just return the cached token. Otherwise, it will
            // make a request to the Azure OAuth endpoint to get a token
            var silentResult = await this.publicClientApplication
              .acquireTokenSilent({
                scopes: scopes,
                account: accounts[0]
              });

            return silentResult.accessToken;
          } catch (err) {
            // If a silent request fails, it may be because the user needs
            // to login or grant consent to one or more of the requested scopes
            if (this.isInteractionRequired(err)) {
              var interactiveResult = await this.publicClientApplication
                .acquireTokenPopup({
                  scopes: scopes
                });

              return interactiveResult.accessToken;
            } else {
              throw err;
            }
          }
        }

        async getUserProfile() {
          try {
            var accessToken = await this.getAccessToken(config.scopes);

            if (accessToken) {
              // TEMPORARY: Display the token in the error flash
              this.setState({
                isAuthenticated: true,
                error: { message: "Access token:", debug: accessToken }
              });
            }
          }
          catch(err) {
            this.setState({
              isAuthenticated: false,
              user: {},
              error: this.normalizeError(err)
            });
          }
        }

        setErrorMessage(message: string, debug: string) {
          this.setState({
            error: { message: message, debug: debug }
          });
        }

        normalizeError(error: string | Error): any {
          var normalizedError = {};
          if (typeof (error) === 'string') {
            var errParts = error.split('|');
            normalizedError = errParts.length > 1 ?
              { message: errParts[1], debug: errParts[0] } :
              { message: error };
          } else {
            normalizedError = {
              message: error.message,
              debug: JSON.stringify(error)
            };
          }
          return normalizedError;
        }

        isInteractionRequired(error: Error): boolean {
          if (!error.message || error.message.length <= 0) {
            return false;
          }

          return (
            error.message.indexOf('consent_required') > -1 ||
            error.message.indexOf('interaction_required') > -1 ||
            error.message.indexOf('login_required') > -1 ||
            error.message.indexOf('no_account_in_silent_request') > -1
          );
        }
      }
    }
    ```

1. <span data-ttu-id="84674-110">Abra `./src/App.tsx` e adicione a `import` instrução a seguir na parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="84674-110">Open `./src/App.tsx` and add the following `import` statement to the top of the file.</span></span>

    ```typescript
    import withAuthProvider, { AuthComponentProps } from './AuthProvider';
    ```

1. <span data-ttu-id="84674-111">Substitua a linha `class App extends Component<any> {` pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="84674-111">Replace the line `class App extends Component<any> {` with the following.</span></span>

    ```typescript
    class App extends Component<AuthComponentProps> {
    ```

1. <span data-ttu-id="84674-112">Substitua a linha `export default App;` pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="84674-112">Replace the line `export default App;` with the following.</span></span>

    ```typescript
    export default withAuthProvider(App);
    ```

1. <span data-ttu-id="84674-113">Salve suas alterações e atualize o navegador.</span><span class="sxs-lookup"><span data-stu-id="84674-113">Save your changes and refresh the browser.</span></span> <span data-ttu-id="84674-114">Clique no botão de login e você deverá ver uma janela pop-up que é `https://login.microsoftonline.com` carregada.</span><span class="sxs-lookup"><span data-stu-id="84674-114">Click the sign-in button and you should see a pop-up window that loads `https://login.microsoftonline.com`.</span></span> <span data-ttu-id="84674-115">Faça logon com sua conta da Microsoft e consenta com as permissões solicitadas.</span><span class="sxs-lookup"><span data-stu-id="84674-115">Login with your Microsoft account and consent to the requested permissions.</span></span> <span data-ttu-id="84674-116">A página do aplicativo deve ser atualizada, mostrando o token.</span><span class="sxs-lookup"><span data-stu-id="84674-116">The app page should refresh, showing the token.</span></span>

### <a name="get-user-details"></a><span data-ttu-id="84674-117">Obter detalhes do usuário</span><span class="sxs-lookup"><span data-stu-id="84674-117">Get user details</span></span>

<span data-ttu-id="84674-118">Nesta seção, você obterá os detalhes do usuário no Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="84674-118">In this section you will get the user's details from Microsoft Graph.</span></span>

1. <span data-ttu-id="84674-119">Crie um novo arquivo no diretório `./src` chamado e adicione o código a `GraphService.ts` seguir.</span><span class="sxs-lookup"><span data-stu-id="84674-119">Create a new file in the `./src` directory called `GraphService.ts` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/GraphService.ts" id="graphServiceSnippet1":::

    <span data-ttu-id="84674-120">Isso implementa a função, que usa o SDK do Microsoft Graph para chamar o ponto de `getUserDetails` extremidade e retornar o `/me` resultado.</span><span class="sxs-lookup"><span data-stu-id="84674-120">This implements the `getUserDetails` function, which uses the Microsoft Graph SDK to call the `/me` endpoint and return the result.</span></span>

1. <span data-ttu-id="84674-121">Abra `./src/AuthProvider.tsx` e adicione a `import` instrução a seguir na parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="84674-121">Open `./src/AuthProvider.tsx` and add the following `import` statement to the top of the file.</span></span>

    ```typescript
    import { getUserDetails } from './GraphService';
    ```

1. <span data-ttu-id="84674-122">Substitua a função `getUserProfile` existente pelo código seguinte.</span><span class="sxs-lookup"><span data-stu-id="84674-122">Replace the existing `getUserProfile` function with the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/AuthProvider.tsx" id="getUserProfileSnippet" highlight="6-18":::

1. <span data-ttu-id="84674-123">Salve suas alterações e inicie o aplicativo. Depois de entrar, você deve voltar à home page, mas a interface do usuário deve mudar para indicar que você está entrar.</span><span class="sxs-lookup"><span data-stu-id="84674-123">Save your changes and start the app, after sign-in you should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

    ![Uma captura de tela da home page após entrar](./images/add-aad-auth-01.png)

1. <span data-ttu-id="84674-125">Clique no avatar do usuário no canto superior direito para acessar o link **Sair.**</span><span class="sxs-lookup"><span data-stu-id="84674-125">Click the user avatar in the top right corner to access the **Sign Out** link.</span></span> <span data-ttu-id="84674-126">Clicar em **Sair** redefine a sessão e o retorna para a home page.</span><span class="sxs-lookup"><span data-stu-id="84674-126">Clicking **Sign Out** resets the session and returns you to the home page.</span></span>

    ![Uma captura de tela do menu suspenso com o link Sair](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a><span data-ttu-id="84674-128">Armazenar e atualizar tokens</span><span class="sxs-lookup"><span data-stu-id="84674-128">Storing and refreshing tokens</span></span>

<span data-ttu-id="84674-129">Neste ponto, seu aplicativo tem um token de acesso, que é enviado no `Authorization` header de chamadas de API.</span><span class="sxs-lookup"><span data-stu-id="84674-129">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="84674-130">Esse é o token que permite que o aplicativo acesse o Microsoft Graph em nome do usuário.</span><span class="sxs-lookup"><span data-stu-id="84674-130">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="84674-131">No entanto, esse token tem curta duração.</span><span class="sxs-lookup"><span data-stu-id="84674-131">However, this token is short-lived.</span></span> <span data-ttu-id="84674-132">O token expira uma hora após sua emissão.</span><span class="sxs-lookup"><span data-stu-id="84674-132">The token expires an hour after it is issued.</span></span> <span data-ttu-id="84674-133">É aqui que o token de atualização se torna útil.</span><span class="sxs-lookup"><span data-stu-id="84674-133">This is where the refresh token becomes useful.</span></span> <span data-ttu-id="84674-134">O token de atualização permite que o aplicativo solicite um novo token de acesso sem exigir que o usuário entre novamente.</span><span class="sxs-lookup"><span data-stu-id="84674-134">The refresh token allows the app to request a new access token without requiring the user to sign in again.</span></span>

<span data-ttu-id="84674-135">Como o aplicativo está usando a biblioteca MSAL, você não precisa implementar qualquer armazenamento de token ou lógica de atualização.</span><span class="sxs-lookup"><span data-stu-id="84674-135">Because the app is using the MSAL library, you do not have to implement any token storage or refresh logic.</span></span> <span data-ttu-id="84674-136">O `PublicClientApplication` token é armazenado em cache na sessão do navegador.</span><span class="sxs-lookup"><span data-stu-id="84674-136">The `PublicClientApplication` caches the token in the browser session.</span></span> <span data-ttu-id="84674-137">O `acquireTokenSilent` método primeiro verifica o token armazenado em cache e, se não tiver expirado, ele o retornará.</span><span class="sxs-lookup"><span data-stu-id="84674-137">The `acquireTokenSilent` method first checks the cached token, and if it is not expired, it returns it.</span></span> <span data-ttu-id="84674-138">Se ele tiver expirado, ele usará o token de atualização em cache para obter um novo.</span><span class="sxs-lookup"><span data-stu-id="84674-138">If it is expired, it uses the cached refresh token to obtain a new one.</span></span> <span data-ttu-id="84674-139">Você usará esse método mais no módulo a seguir.</span><span class="sxs-lookup"><span data-stu-id="84674-139">You'll use this method more in the following module.</span></span>
