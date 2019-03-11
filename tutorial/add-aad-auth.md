<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="50151-101">Neste exercício, você estenderá o aplicativo do exercício anterior para oferecer suporte à autenticação com o Azure AD.</span><span class="sxs-lookup"><span data-stu-id="50151-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="50151-102">Isso é necessário para obter o token de acesso OAuth necessário para chamar o Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="50151-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="50151-103">Nesta etapa, você integrará a biblioteca da [biblioteca de autenticação da Microsoft](https://github.com/AzureAD/microsoft-authentication-library-for-js) ao aplicativo.</span><span class="sxs-lookup"><span data-stu-id="50151-103">In this step you will integrate the [Microsoft Authentication Library](https://github.com/AzureAD/microsoft-authentication-library-for-js) library into the application.</span></span>

<span data-ttu-id="50151-104">Crie um novo arquivo no `./src` diretório chamado `Config.js` e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="50151-104">Create a new file in the `./src` directory named `Config.js` and add the following code.</span></span>

```js
module.exports = {
  appId: 'YOUR_APP_ID_HERE',
  scopes: [
    "user.read",
    "calendars.read"
  ]
};
```

<span data-ttu-id="50151-105">Substitua `YOUR_APP_ID_HERE` pela ID do aplicativo do portal de registro do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="50151-105">Replace `YOUR_APP_ID_HERE` with the application ID from the Application Registration Portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="50151-106">Se você estiver usando o controle de origem como o Git, agora seria uma boa hora para excluir `Config.js` o arquivo do controle de origem para evitar vazar inadvertidamente sua ID de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="50151-106">If you're using source control such as git, now would be a good time to exclude the `Config.js` file from source control to avoid inadvertently leaking your app ID.</span></span>

<span data-ttu-id="50151-107">Abra `./src/App.js` e adicione as seguintes `import` instruções à parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="50151-107">Open `./src/App.js` and add the following `import` statements to the top of the file.</span></span>

```js
import config from './Config';
import { UserAgentApplication } from 'msal';
```

## <a name="implement-sign-in"></a><span data-ttu-id="50151-108">Implementar logon</span><span class="sxs-lookup"><span data-stu-id="50151-108">Implement sign-in</span></span>

<span data-ttu-id="50151-109">Comece atualizando o `constructor` para a `App` classe para criar uma instância da `UserAgentApplication` classe e chamar `getUser` para ver se já existe um usuário conectado.</span><span class="sxs-lookup"><span data-stu-id="50151-109">Start by updating the `constructor` for the `App` class to create an instance of the `UserAgentApplication` class and call `getUser` to see if there's already a logged-in user.</span></span> <span data-ttu-id="50151-110">Substitua o existente `constructor` pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="50151-110">Replace the existing `constructor` with the following.</span></span>

```js
constructor(props) {
  super(props);

  this.userAgentApplication = new UserAgentApplication(config.appId, null, null);

  var user = this.userAgentApplication.getUser();

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

<span data-ttu-id="50151-111">Este código inicializa a `UserAgentApplication` classe com a ID do aplicativo e verifica a presença de um usuário.</span><span class="sxs-lookup"><span data-stu-id="50151-111">This code initializes the `UserAgentApplication` class with your application ID and checks for the presence of a user.</span></span> <span data-ttu-id="50151-112">Se houver um usuário, ele será definido `isAuthenticated` como true.</span><span class="sxs-lookup"><span data-stu-id="50151-112">If there is a user, it sets `isAuthenticated` to true.</span></span> <span data-ttu-id="50151-113">O `getUserProfile` método ainda não foi implementado.</span><span class="sxs-lookup"><span data-stu-id="50151-113">The `getUserProfile` method isn't implemented yet.</span></span> <span data-ttu-id="50151-114">Você vai implementar isso um pouco mais tarde.</span><span class="sxs-lookup"><span data-stu-id="50151-114">You will implement this a bit later.</span></span>

<span data-ttu-id="50151-115">Em seguida, adicione uma função à `App` classe para fazer o logon.</span><span class="sxs-lookup"><span data-stu-id="50151-115">Next, add a function to the `App` class to do the login.</span></span> <span data-ttu-id="50151-116">Adicione a função a seguir à classe `App`.</span><span class="sxs-lookup"><span data-stu-id="50151-116">Add the following function to the `App` class.</span></span>

```js
async login() {
  try {
    await this.userAgentApplication.loginPopup(config.scopes);
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

<span data-ttu-id="50151-117">Este método chama a `loginPopup` função para fazer o logon e, em seguida `getUserProfile` , chama a função.</span><span class="sxs-lookup"><span data-stu-id="50151-117">This method calls the `loginPopup` function to do the login, then calls the `getUserProfile` function.</span></span>

<span data-ttu-id="50151-118">Agora, adicione uma função para fazer logoff.</span><span class="sxs-lookup"><span data-stu-id="50151-118">Now add a function to logout.</span></span> <span data-ttu-id="50151-119">Adicione a função a seguir à classe `App`.</span><span class="sxs-lookup"><span data-stu-id="50151-119">Add the following function to the `App` class.</span></span>

```js
logout() {
  this.userAgentApplication.logout();
}
```

<span data-ttu-id="50151-120">Agora que os `login` métodos `logout` e são implementados, atualize `NavBar` os `Welcome` elementos e no `render` método da `App` classe.</span><span class="sxs-lookup"><span data-stu-id="50151-120">Now that the `login` and `logout` methods are implemented, update the `NavBar` and `Welcome` elements in the `render` method of the `App` class.</span></span> <span data-ttu-id="50151-121">Substitua o elemento `NavBar` existente pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="50151-121">Replace the existing `NavBar` element with the following.</span></span>

```JSX
<NavBar
  isAuthenticated={this.state.isAuthenticated}
  authButtonMethod={this.state.isAuthenticated ? this.logout.bind(this) : this.login.bind(this)}
  user={this.state.user}/>
```

<span data-ttu-id="50151-122">Substitua o elemento `Welcome` existente pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="50151-122">Replace the existing `Welcome` element with the following.</span></span>

```JSX
<Welcome {...props}
  isAuthenticated={this.state.isAuthenticated}
  user={this.state.user}
  authButtonMethod={this.login.bind(this)} />
```

<span data-ttu-id="50151-123">Por fim, implemente a `getUserProfile` função.</span><span class="sxs-lookup"><span data-stu-id="50151-123">Finally, implement the `getUserProfile` function.</span></span> <span data-ttu-id="50151-124">Adicione a função a seguir à classe `App`.</span><span class="sxs-lookup"><span data-stu-id="50151-124">Add the following function to the `App` class.</span></span>

```js
async getUserProfile() {
  try {
    // Get the access token silently
    // If the cache contains a non-expired token, this function
    // will just return the cached token. Otherwise, it will
    // make a request to the Azure OAuth endpoint to get a token

    var accessToken = await this.userAgentApplication.acquireTokenSilent(config.scopes);

    if (accessToken) {
      // TEMPORARY: Display the token in the error flash
      this.setState({
        isAuthenticated: true,
        error: { message: "Access token:", debug: accessToken }
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

<span data-ttu-id="50151-125">Este código chama `acquireTokenSilent` para obter um token de acesso e, em seguida, só gera o token como um erro.</span><span class="sxs-lookup"><span data-stu-id="50151-125">This code calls `acquireTokenSilent` to get an access token, then just outputs the token as an error.</span></span>

<span data-ttu-id="50151-126">Salve suas alterações e atualize o navegador.</span><span class="sxs-lookup"><span data-stu-id="50151-126">Save your changes and refresh the browser.</span></span> <span data-ttu-id="50151-127">Clique no botão entrar e você deverá ser redirecionado para `https://login.microsoftonline.com`o.</span><span class="sxs-lookup"><span data-stu-id="50151-127">Click the sign-in button and you should be redirected to `https://login.microsoftonline.com`.</span></span> <span data-ttu-id="50151-128">Faça logon com sua conta da Microsoft e concorde com as permissões solicitadas.</span><span class="sxs-lookup"><span data-stu-id="50151-128">Login with your Microsoft account and consent to the requested permissions.</span></span> <span data-ttu-id="50151-129">A página do aplicativo deve ser atualizada, mostrando o token.</span><span class="sxs-lookup"><span data-stu-id="50151-129">The app page should refresh, showing the token.</span></span>

### <a name="get-user-details"></a><span data-ttu-id="50151-130">Obter detalhes do usuário</span><span class="sxs-lookup"><span data-stu-id="50151-130">Get user details</span></span>

<span data-ttu-id="50151-131">Comece criando um novo arquivo para conter todas as chamadas do Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="50151-131">Start by creating a new file to hold all of your Microsoft Graph calls.</span></span> <span data-ttu-id="50151-132">Crie um novo arquivo no `./src` diretório chamado `GraphService.js` e adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="50151-132">Create a new file in the `./src` directory called `GraphService.js` and add the following code.</span></span>

```js
var graph = require('@microsoft/microsoft-graph-client');

function getAuthenticatedClient(accessToken) {
  // Initialize Graph client
  const client = graph.Client.init({
    // Use the provided access token to authenticate
    // requests
    authProvider: (done) => {
      done(null, accessToken);
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

<span data-ttu-id="50151-133">Isso implementa a `getUserDetails` função, que usa o SDK do Microsoft Graph para chamar `/me` o ponto de extremidade e retornar o resultado.</span><span class="sxs-lookup"><span data-stu-id="50151-133">This implements the `getUserDetails` function, which uses the Microsoft Graph SDK to call the `/me` endpoint and return the result.</span></span>

<span data-ttu-id="50151-134">Atualize o `getUserProfile` método no `./src/App.js` para chamar essa função.</span><span class="sxs-lookup"><span data-stu-id="50151-134">Update the `getUserProfile` method in `./src/App.js` to call this function.</span></span> <span data-ttu-id="50151-135">Primeiro, adicione a seguinte `import` instrução à parte superior do arquivo.</span><span class="sxs-lookup"><span data-stu-id="50151-135">First, add the following `import` statement to the top of the file.</span></span>

```js
import { getUserDetails } from './GraphService';
```

<span data-ttu-id="50151-136">Substitua a função `getUserProfile` existente pelo código seguinte.</span><span class="sxs-lookup"><span data-stu-id="50151-136">Replace the existing `getUserProfile` function with the following code.</span></span>

```js
async getUserProfile() {
  try {
    // Get the access token silently
    // If the cache contains a non-expired token, this function
    // will just return the cached token. Otherwise, it will
    // make a request to the Azure OAuth endpoint to get a token

    var accessToken = await this.userAgentApplication.acquireTokenSilent(config.scopes);

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

<span data-ttu-id="50151-137">Agora, se você salvar as alterações e iniciar o aplicativo, depois de entrar, você deverá terminar de volta na Home Page, mas a interface do usuário deverá mudar para indicar que você está conectado.</span><span class="sxs-lookup"><span data-stu-id="50151-137">Now if you save your changes and start the app, after sign-in you should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

![Uma captura de tela da Home Page após entrar](./images/add-aad-auth-01.png)

<span data-ttu-id="50151-139">Clique no avatar do usuário no canto superior direito para acessar o \*\*\*\* link sair.</span><span class="sxs-lookup"><span data-stu-id="50151-139">Click the user avatar in the top right corner to access the **Sign Out** link.</span></span> <span data-ttu-id="50151-140">Clicar \*\*\*\* em sair redefine a sessão e retorna à Home Page.</span><span class="sxs-lookup"><span data-stu-id="50151-140">Clicking **Sign Out** resets the session and returns you to the home page.</span></span>

![Uma captura de tela do menu suspenso com o link sair](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a><span data-ttu-id="50151-142">Armazenar e atualizar tokens</span><span class="sxs-lookup"><span data-stu-id="50151-142">Storing and refreshing tokens</span></span>

<span data-ttu-id="50151-143">Nesse ponto, seu aplicativo tem um token de acesso, que é enviado no `Authorization` cabeçalho das chamadas de API.</span><span class="sxs-lookup"><span data-stu-id="50151-143">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="50151-144">Este é o token que permite que o aplicativo acesse o Microsoft Graph em nome do usuário.</span><span class="sxs-lookup"><span data-stu-id="50151-144">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="50151-145">No enTanto, esse token é de vida curta.</span><span class="sxs-lookup"><span data-stu-id="50151-145">However, this token is short-lived.</span></span> <span data-ttu-id="50151-146">O token expira uma hora após sua emissão.</span><span class="sxs-lookup"><span data-stu-id="50151-146">The token expires an hour after it is issued.</span></span> <span data-ttu-id="50151-147">É onde o token de atualização se torna útil.</span><span class="sxs-lookup"><span data-stu-id="50151-147">This is where the refresh token becomes useful.</span></span> <span data-ttu-id="50151-148">O token de atualização permite que o aplicativo solicite um novo token de acesso sem exigir que o usuário entre novamente.</span><span class="sxs-lookup"><span data-stu-id="50151-148">The refresh token allows the app to request a new access token without requiring the user to sign in again.</span></span>

<span data-ttu-id="50151-149">Como o aplicativo está usando a biblioteca MSAL, você não precisa implementar qualquer lógica de armazenamento ou de atualização.</span><span class="sxs-lookup"><span data-stu-id="50151-149">Because the app is using the MSAL library, you do not have to implement any token storage or refresh logic.</span></span> <span data-ttu-id="50151-150">O `UserAgentApplication` método armazena em cache o token na sessão do navegador.</span><span class="sxs-lookup"><span data-stu-id="50151-150">The `UserAgentApplication` method caches the token in the browser session.</span></span> <span data-ttu-id="50151-151">O `acquireTokenSilent` método primeiro verifica o token em cache e, se ele não tiver expirado, ele o retornará.</span><span class="sxs-lookup"><span data-stu-id="50151-151">The `acquireTokenSilent` method first checks the cached token, and if it is not expired, it returns it.</span></span> <span data-ttu-id="50151-152">Se ele tiver expirado, ele usará o token de atualização em cache para obter um novo.</span><span class="sxs-lookup"><span data-stu-id="50151-152">If it is expired, it uses the cached refresh token to obtain a new one.</span></span> <span data-ttu-id="50151-153">Você usará mais este método no módulo a seguir.</span><span class="sxs-lookup"><span data-stu-id="50151-153">You'll use this method more in the following module.</span></span>