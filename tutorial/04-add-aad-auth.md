<!-- markdownlint-disable MD002 MD041 -->

Neste exercício, você estenderá o aplicativo do exercício anterior para oferecer suporte à autenticação com o Azure AD. Isso é necessário para obter o token de acesso OAuth necessário para chamar o Microsoft Graph. Nesta etapa, você integrará a biblioteca da [biblioteca de autenticação da Microsoft](https://github.com/AzureAD/microsoft-authentication-library-for-js) ao aplicativo.

Crie um novo arquivo no `./src` diretório chamado `Config.js` e adicione o código a seguir.

```js
module.exports = {
  appId: 'YOUR_APP_ID_HERE',
  scopes: [
    "user.read",
    "calendars.read"
  ]
};
```

Substitua `YOUR_APP_ID_HERE` pela ID do aplicativo do portal de registro do aplicativo.

> [!IMPORTANT]
> Se você estiver usando o controle de origem como o Git, agora seria uma boa hora para excluir `Config.js` o arquivo do controle de origem para evitar vazar inadvertidamente sua ID de aplicativo.

Abra `./src/App.js` e adicione as seguintes `import` instruções à parte superior do arquivo.

```js
import config from './Config';
import { UserAgentApplication } from 'msal';
```

## <a name="implement-sign-in"></a>Implementar logon

Comece atualizando o `constructor` para a `App` classe para criar uma instância da `UserAgentApplication` classe e chamar `getUser` para ver se já existe um usuário conectado. Substitua o existente `constructor` pelo seguinte.

```js
constructor(props) {
  super(props);

  this.userAgentApplication = new UserAgentApplication({
        auth: {
            clientId: config.appId
        },
        cache: {
            cacheLocation: "localStorage",
            storeAuthStateInCookie: true
        }
    });

  var user = this.userAgentApplication.getAccount();

  this.state = {
    isAuthenticated: (user !== null),
    user: {},
    error: null
  };

  if (user) {
    // Enhance user object with data from Graph
    this.getUserProfile();
  }
}
```

Este código inicializa a `UserAgentApplication` classe com a ID do aplicativo e verifica a presença de um usuário. Se houver um usuário, ele será definido `isAuthenticated` como true. O `getUserProfile` método ainda não foi implementado. Você vai implementar isso um pouco mais tarde.

Em seguida, adicione uma função à `App` classe para fazer o logon. Adicione a função a seguir à classe `App`.

```js
async login() {
  try {
    await this.userAgentApplication.loginPopup(
        {
          scopes: config.scopes,
          prompt: "select_account"
      });
    await this.getUserProfile();
  }
  catch(err) {
    var errParts = err.split('|');
    this.setState({
      isAuthenticated: false,
      user: {},
      error: { message: errParts[1], debug: errParts[0] }
    });
  }
}
```

Este método chama a `loginPopup` função para fazer o logon e, em seguida `getUserProfile` , chama a função.

Agora, adicione uma função para fazer logoff. Adicione a função a seguir à classe `App`.

```js
logout() {
  this.userAgentApplication.logout();
}
```

Agora que os `login` métodos `logout` e são implementados, atualize `NavBar` os `Welcome` elementos e no `render` método da `App` classe. Substitua o elemento `NavBar` existente pelo seguinte.

```JSX
<NavBar
  isAuthenticated={this.state.isAuthenticated}
  authButtonMethod={this.state.isAuthenticated ? this.logout.bind(this) : this.login.bind(this)}
  user={this.state.user}/>
```

Substitua o elemento `Welcome` existente pelo seguinte.

```JSX
<Welcome {...props}
  isAuthenticated={this.state.isAuthenticated}
  user={this.state.user}
  authButtonMethod={this.login.bind(this)} />
```

Por fim, implemente a `getUserProfile` função. Adicione a função a seguir à classe `App`.

```js
async getUserProfile() {
  try {
    // Get the access token silently
    // If the cache contains a non-expired token, this function
    // will just return the cached token. Otherwise, it will
    // make a request to the Azure OAuth endpoint to get a token

    var accessToken = await this.userAgentApplication.acquireTokenSilent({
        scopes: config.scopes
      });

    if (accessToken) {
      // TEMPORARY: Display the token in the error flash
      this.setState({
        isAuthenticated: true,
        error: { message: "Access token:", debug: accessToken.accessToken }
      });
    }
  }
  catch(err) {
    var errParts = err.split('|');
    this.setState({
      isAuthenticated: false,
      user: {},
      error: { message: errParts[1], debug: errParts[0] }
    });
  }
}
```

Este código chama `acquireTokenSilent` para obter um token de acesso e, em seguida, só gera o token como um erro.

Salve suas alterações e atualize o navegador. Clique no botão entrar e você deverá ser redirecionado para `https://login.microsoftonline.com`o. Faça logon com sua conta da Microsoft e concorde com as permissões solicitadas. A página do aplicativo deve ser atualizada, mostrando o token.

### <a name="get-user-details"></a>Obter detalhes do usuário

Comece criando um novo arquivo para conter todas as chamadas do Microsoft Graph. Crie um novo arquivo no `./src` diretório chamado `GraphService.js` e adicione o código a seguir.

```js
var graph = require('@microsoft/microsoft-graph-client');

function getAuthenticatedClient(accessToken) {
  // Initialize Graph client
  const client = graph.Client.init({
    // Use the provided access token to authenticate
    // requests
    authProvider: (done) => {
      done(null, accessToken.accessToken);
    }
  });

  return client;
}

export async function getUserDetails(accessToken) {
  const client = getAuthenticatedClient(accessToken);

  const user = await client.api('/me').get();
  return user;
}
```

Isso implementa a `getUserDetails` função, que usa o SDK do Microsoft Graph para chamar `/me` o ponto de extremidade e retornar o resultado.

Atualize o `getUserProfile` método no `./src/App.js` para chamar essa função. Primeiro, adicione a seguinte `import` instrução à parte superior do arquivo.

```js
import { getUserDetails } from './GraphService';
```

Substitua a função `getUserProfile` existente pelo código seguinte.

```js
async getUserProfile() {
  try {
    // Get the access token silently
    // If the cache contains a non-expired token, this function
    // will just return the cached token. Otherwise, it will
    // make a request to the Azure OAuth endpoint to get a token

    var accessToken = await this.userAgentApplication.acquireTokenSilent({
      scopes: config.scopes
    });

    if (accessToken) {
      // Get the user's profile from Graph
      var user = await getUserDetails(accessToken);
      this.setState({
        isAuthenticated: true,
        user: {
          displayName: user.displayName,
          email: user.mail || user.userPrincipalName
        },
        error: null
      });
    }
  }
  catch(err) {
    var error = {};
    if (typeof(err) === 'string') {
      var errParts = err.split('|');
      error = errParts.length > 1 ?
        { message: errParts[1], debug: errParts[0] } :
        { message: err };
    } else {
      error = {
        message: err.message,
        debug: JSON.stringify(err)
      };
    }

    this.setState({
      isAuthenticated: false,
      user: {},
      error: error
    });
  }
}
```

Agora, se você salvar as alterações e iniciar o aplicativo, depois de entrar, você deverá terminar de volta na Home Page, mas a interface do usuário deverá mudar para indicar que você está conectado.

![Uma captura de tela da Home Page após entrar](./images/add-aad-auth-01.png)

Clique no avatar do usuário no canto superior direito para acessar o **** link sair. Clicar **** em sair redefine a sessão e retorna à Home Page.

![Uma captura de tela do menu suspenso com o link sair](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a>Armazenar e atualizar tokens

Nesse ponto, seu aplicativo tem um token de acesso, que é enviado no `Authorization` cabeçalho das chamadas de API. Este é o token que permite que o aplicativo acesse o Microsoft Graph em nome do usuário.

No entanto, esse token é de vida curta. O token expira uma hora após sua emissão. É onde o token de atualização se torna útil. O token de atualização permite que o aplicativo solicite um novo token de acesso sem exigir que o usuário entre novamente.

Como o aplicativo está usando a biblioteca MSAL, você não precisa implementar qualquer lógica de armazenamento ou de atualização. O `UserAgentApplication` método armazena em cache o token na sessão do navegador. O `acquireTokenSilent` método primeiro verifica o token em cache e, se ele não tiver expirado, ele o retornará. Se ele tiver expirado, ele usará o token de atualização em cache para obter um novo. Você usará mais este método no módulo a seguir.
