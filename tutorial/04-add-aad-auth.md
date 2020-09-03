<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você estenderá o aplicativo do exercício anterior para oferecer suporte à autenticação com o Azure AD. Isso é necessário para obter o token de acesso OAuth necessário para chamar o Microsoft Graph. Nesta etapa, você integrará a biblioteca da [biblioteca de autenticação da Microsoft](https://github.com/AzureAD/microsoft-authentication-library-for-js) ao aplicativo.

1. Crie um novo arquivo no `./src` diretório chamado `Config.ts` e adicione o código a seguir.

    :::code language="typescript" source="../demo/graph-tutorial/src/Config.example.ts":::

    Substitua `YOUR_APP_ID_HERE` pela ID do aplicativo do portal de registro do aplicativo.

    > [!IMPORTANT]
    > Se você estiver usando o controle de origem como o Git, agora seria uma boa hora para excluir o `Config.ts` arquivo do controle de origem para evitar vazar inadvertidamente sua ID de aplicativo.

## <a name="implement-sign-in"></a>Implementar logon

Nesta seção, você criará um provedor de autenticação e implementará a entrada e a saída.

1. Crie um novo arquivo no `./src` diretório chamado `AuthProvider.tsx` e adicione o código a seguir.

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
      (WrappedComponent: new(props: AuthComponentProps, context?: any) => T): React.ComponentClass {
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
            error = { this.state.error }
            isAuthenticated = { this.state.isAuthenticated }
            user = { this.state.user }
            login = { () => this.login() }
            logout = { () => this.logout() }
            getAccessToken = { (scopes: string[]) => this.getAccessToken(scopes)}
            setError = { (message: string, debug: string) => this.setErrorMessage(message, debug)}
            {...this.props} {...this.state} />;
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
          catch(err) {
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
            error: {message: message, debug: debug}
          });
        }

        normalizeError(error: string | Error): any {
          var normalizedError = {};
          if (typeof(error) === 'string') {
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

1. Abra `./src/App.tsx` e adicione a seguinte `import` instrução à parte superior do arquivo.

    ```typescript
    import withAuthProvider, { AuthComponentProps } from './AuthProvider';
    ```

1. Substitua a linha `class App extends Component<any> {` pelo seguinte.

    ```typescript
    class App extends Component<AuthComponentProps> {
    ```

1. Substitua a linha `export default App;` pelo seguinte.

    ```typescript
    export default withAuthProvider(App);
    ```

1. Salve suas alterações e atualize o navegador. Clique no botão entrar e você deverá ver uma janela pop-up que é carregada `https://login.microsoftonline.com` . Faça logon com sua conta da Microsoft e concorde com as permissões solicitadas. A página do aplicativo deve ser atualizada, mostrando o token.

### <a name="get-user-details"></a>Obter detalhes do usuário

Nesta seção, você receberá os detalhes do usuário do Microsoft Graph.

1. Crie um novo arquivo no `./src` diretório chamado `GraphService.ts` e adicione o código a seguir.

    :::code language="typescript" source="../demo/graph-tutorial/src/GraphService.ts" id="graphServiceSnippet1":::

    Isso implementa a `getUserDetails` função, que usa o SDK do Microsoft Graph para chamar o `/me` ponto de extremidade e retornar o resultado.

1. Abra `./src/AuthProvider.tsx` e adicione a seguinte `import` instrução à parte superior do arquivo.

    ```typescript
    import { getUserDetails } from './GraphService';
    ```

1. Substitua a função `getUserProfile` existente pelo código seguinte.

    :::code language="typescript" source="../demo/graph-tutorial/src/AuthProvider.tsx" id="getUserProfileSnippet" highlight="6-18":::

1. Salve suas alterações e inicie o aplicativo, após entrar, você deve terminar de volta na Home Page, mas a interface do usuário deve ser alterada para indicar que você está conectado.

    ![Uma captura de tela da Home Page após entrar](./images/add-aad-auth-01.png)

1. Clique no avatar do usuário no canto superior direito para **acessar o link sair.** Clicar **em sair** redefine a sessão e retorna à Home Page.

    ![Uma captura de tela do menu suspenso com o link sair](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a>Armazenar e atualizar tokens

Nesse ponto, seu aplicativo tem um token de acesso, que é enviado no `Authorization` cabeçalho das chamadas de API. Este é o token que permite que o aplicativo acesse o Microsoft Graph em nome do usuário.

No entanto, esse token é de vida curta. O token expira uma hora após sua emissão. É onde o token de atualização se torna útil. O token de atualização permite que o aplicativo solicite um novo token de acesso sem exigir que o usuário entre novamente.

Como o aplicativo está usando a biblioteca MSAL, você não precisa implementar qualquer lógica de armazenamento ou de atualização. O `PublicClientApplication` armazena em cache o token na sessão do navegador. O `acquireTokenSilent` método primeiro verifica o token em cache e, se ele não tiver expirado, ele o retornará. Se ele tiver expirado, ele usará o token de atualização em cache para obter um novo. Você usará mais este método no módulo a seguir.
