# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="1c7be-101">Como executar o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="1c7be-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1c7be-102">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="1c7be-102">Prerequisites</span></span>

<span data-ttu-id="1c7be-103">Para executar o projeto concluído nessa pasta, você precisará do seguinte:</span><span class="sxs-lookup"><span data-stu-id="1c7be-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="1c7be-104">[Visual Studio](https://visualstudio.microsoft.com/vs/) instalado em sua máquina de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="1c7be-104">[Visual Studio](https://visualstudio.microsoft.com/vs/) installed on your development machine.</span></span> <span data-ttu-id="1c7be-105">Se você não tiver o Visual Studio, visite o link anterior para opções de download.</span><span class="sxs-lookup"><span data-stu-id="1c7be-105">If you do not have Visual Studio, visit the previous link for download options.</span></span> <span data-ttu-id="1c7be-106">(**Observação:** este tutorial foi escrito com o Visual Studio 2017 versão 15,81.</span><span class="sxs-lookup"><span data-stu-id="1c7be-106">(**Note:** This tutorial was written with Visual Studio 2017 version 15.81.</span></span> <span data-ttu-id="1c7be-107">As etapas deste guia podem funcionar com outras versões, mas que não foram testadas.</span><span class="sxs-lookup"><span data-stu-id="1c7be-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="1c7be-108">Uma conta pessoal da Microsoft com uma caixa de correio no Outlook.com ou uma conta corporativa ou de estudante da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="1c7be-108">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="1c7be-109">Se você não tem uma conta da Microsoft, há algumas opções para obter uma conta gratuita:</span><span class="sxs-lookup"><span data-stu-id="1c7be-109">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="1c7be-110">Você pode [se inscrever para uma nova conta pessoal da Microsoft](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="1c7be-110">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="1c7be-111">Você pode [se inscrever no programa para desenvolvedores do office 365](https://developer.microsoft.com/office/dev-program) para obter uma assinatura gratuita do Office 365.</span><span class="sxs-lookup"><span data-stu-id="1c7be-111">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="1c7be-112">Registrar um aplicativo Web com o centro de administração do Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1c7be-112">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="1c7be-113">Abra um navegador e navegue até o [centro de administração do Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1c7be-113">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="1c7be-114">Faça logon usando uma **conta pessoal** (aka: conta da Microsoft) ou **conta corporativa ou de estudante**.</span><span class="sxs-lookup"><span data-stu-id="1c7be-114">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="1c7be-115">Selecione **Azure Active Directory** na navegação à esquerda e, em seguida, selecione **registros de aplicativo (visualização)** em **gerenciar**.</span><span class="sxs-lookup"><span data-stu-id="1c7be-115">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations (Preview)** under **Manage**.</span></span>

    ![<span data-ttu-id="1c7be-116">Uma captura de tela dos registros de aplicativo</span><span class="sxs-lookup"><span data-stu-id="1c7be-116">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="1c7be-117">Selecione **novo registro**.</span><span class="sxs-lookup"><span data-stu-id="1c7be-117">Select **New registration**.</span></span> <span data-ttu-id="1c7be-118">Na página **registrar um aplicativo** , defina os valores da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="1c7be-118">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="1c7be-119">Defina \*\*\*\* o nome `React Graph Tutorial`como.</span><span class="sxs-lookup"><span data-stu-id="1c7be-119">Set **Name** to `React Graph Tutorial`.</span></span>
    - <span data-ttu-id="1c7be-120">Defina os **tipos de conta com suporte** para **contas em qualquer diretório organizacional e contas pessoais da Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="1c7be-120">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="1c7be-121">Em **URI**de redirecionamento, defina o primeiro menu `Web` suspenso como e defina o `http://localhost:3000`valor como.</span><span class="sxs-lookup"><span data-stu-id="1c7be-121">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `http://localhost:3000`.</span></span>

    ![Uma captura de tela da página registrar um aplicativo](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="1c7be-123">Escolha **registrar**.</span><span class="sxs-lookup"><span data-stu-id="1c7be-123">Choose **Register**.</span></span> <span data-ttu-id="1c7be-124">Na página **tutorial do gráfico angular** , copie o valor da **ID do aplicativo (cliente)** e salve-o, você precisará dele na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="1c7be-124">On the **Angular Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Uma captura de tela da ID do aplicativo do novo registro de aplicativo](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="1c7be-126">Selecione **autenticação** em **gerenciar**.</span><span class="sxs-lookup"><span data-stu-id="1c7be-126">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="1c7be-127">Localize a seção **Grant implícita** e habilite tokens de **acesso** e tokens de **ID**.</span><span class="sxs-lookup"><span data-stu-id="1c7be-127">Locate the **Implicit grant** section and enable **Access tokens** and **ID tokens**.</span></span> <span data-ttu-id="1c7be-128">Selecione **Salvar**.</span><span class="sxs-lookup"><span data-stu-id="1c7be-128">Choose **Save**.</span></span>

    ![Uma captura de tela da seção Grant implícita](/tutorial/images/aad-implicit-grant.png)

## <a name="configure-the-sample"></a><span data-ttu-id="1c7be-130">Configurar o exemplo</span><span class="sxs-lookup"><span data-stu-id="1c7be-130">Configure the sample</span></span>

1. <span data-ttu-id="1c7be-131">ReNomear `./graph-tutorial/src/Config.js.example` o `./graph-tutorial/src/Config.js`arquivo para.</span><span class="sxs-lookup"><span data-stu-id="1c7be-131">Rename the `./graph-tutorial/src/Config.js.example` file to `./graph-tutorial/src/Config.js`.</span></span>
1. <span data-ttu-id="1c7be-132">Edite `./graph-tutorial/src/Config.js` o arquivo e faça as seguintes alterações.</span><span class="sxs-lookup"><span data-stu-id="1c7be-132">Edit the `./graph-tutorial/src/Config.js` file and make the following changes.</span></span>
    1. <span data-ttu-id="1c7be-133">Substitua `YOUR_APP_ID_HERE` pela **ID do aplicativo** obtida do portal de registro do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1c7be-133">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="1c7be-134">Na sua interface de linha de comando (CLI), navegue até `graph-tutorial` o diretório e execute o comando a seguir para instalar os requisitos.</span><span class="sxs-lookup"><span data-stu-id="1c7be-134">In your command-line interface (CLI), navigate to the `graph-tutorial` directory and run the following command to install requirements.</span></span>

    ```Shell
    npm install
    ```

## <a name="run-the-sample"></a><span data-ttu-id="1c7be-135">Executar o exemplo</span><span class="sxs-lookup"><span data-stu-id="1c7be-135">Run the sample</span></span>

1. <span data-ttu-id="1c7be-136">Execute o comando a seguir em sua CLI para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1c7be-136">Run the following command in your CLI to start the application.</span></span>

    ```Shell
    npm start
    ```

1. <span data-ttu-id="1c7be-137">Abra um navegador e navegue até `http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="1c7be-137">Open a browser and browse to `http://localhost:3000`.</span></span>