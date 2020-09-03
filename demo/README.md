# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="b782a-101">Como executar o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="b782a-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b782a-102">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="b782a-102">Prerequisites</span></span>

<span data-ttu-id="b782a-103">Para executar o projeto concluído nessa pasta, você precisará do seguinte:</span><span class="sxs-lookup"><span data-stu-id="b782a-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="b782a-104">[Node.js](https://nodejs.org) instalado em sua máquina de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="b782a-104">[Node.js](https://nodejs.org) installed on your development machine.</span></span> <span data-ttu-id="b782a-105">Se você não tiver Node.js, visite o link anterior para opções de download.</span><span class="sxs-lookup"><span data-stu-id="b782a-105">If you do not have Node.js, visit the previous link for download options.</span></span> <span data-ttu-id="b782a-106">(**Observação:** este tutorial foi escrito com o nó versão 12.16.1.</span><span class="sxs-lookup"><span data-stu-id="b782a-106">(**Note:** This tutorial was written with Node version 12.16.1.</span></span> <span data-ttu-id="b782a-107">As etapas deste guia podem funcionar com outras versões, mas que não foram testadas.</span><span class="sxs-lookup"><span data-stu-id="b782a-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="b782a-108">Uma conta pessoal da Microsoft com uma caixa de correio no Outlook.com ou uma conta corporativa ou de estudante da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b782a-108">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="b782a-109">Se você não tem uma conta da Microsoft, há algumas opções para obter uma conta gratuita:</span><span class="sxs-lookup"><span data-stu-id="b782a-109">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="b782a-110">Você pode [se inscrever para uma nova conta pessoal da Microsoft](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="b782a-110">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="b782a-111">Você pode [se inscrever no programa para desenvolvedores do office 365](https://developer.microsoft.com/office/dev-program) para obter uma assinatura gratuita do Office 365.</span><span class="sxs-lookup"><span data-stu-id="b782a-111">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="b782a-112">Registrar um aplicativo Web com o centro de administração do Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b782a-112">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="b782a-113">Abra um navegador e navegue até o [centro de administração do Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b782a-113">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="b782a-114">Faça logon usando uma **conta pessoal** (também conhecida como Conta da Microsoft) ou **Conta Corporativa ou de Estudante**.</span><span class="sxs-lookup"><span data-stu-id="b782a-114">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="b782a-115">Selecione **Azure Active Directory** na navegação esquerda e selecione **Registros de aplicativos** em **Gerenciar**.</span><span class="sxs-lookup"><span data-stu-id="b782a-115">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="b782a-116">Uma captura de tela dos registros de aplicativo</span><span class="sxs-lookup"><span data-stu-id="b782a-116">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

    > <span data-ttu-id="b782a-117">**Observação:** Usuários do Azure AD B2C só podem ver **registros de aplicativos (herdados)**.</span><span class="sxs-lookup"><span data-stu-id="b782a-117">**Note:** Azure AD B2C users may only see **App registrations (legacy)**.</span></span> <span data-ttu-id="b782a-118">Nesse caso, vá diretamente para [https://aka.ms/appregistrations](https://aka.ms/appregistrations) .</span><span class="sxs-lookup"><span data-stu-id="b782a-118">In this case, please go directly to [https://aka.ms/appregistrations](https://aka.ms/appregistrations).</span></span>

1. <span data-ttu-id="b782a-119">Selecione **Novo registro**.</span><span class="sxs-lookup"><span data-stu-id="b782a-119">Select **New registration**.</span></span> <span data-ttu-id="b782a-120">Na página **Registrar um aplicativo**, defina os valores da seguinte forma.</span><span class="sxs-lookup"><span data-stu-id="b782a-120">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="b782a-121">Defina **Nome** para `React Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="b782a-121">Set **Name** to `React Graph Tutorial`.</span></span>
    - <span data-ttu-id="b782a-122">Defina **Tipos de conta com suporte** para **Contas em qualquer diretório organizacional e contas pessoais da Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="b782a-122">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="b782a-123">Em **URI de Redirecionamento**, defina o primeiro menu suspenso para `Single-page application (SPA)` e defina o valor como `http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="b782a-123">Under **Redirect URI**, set the first drop-down to `Single-page application (SPA)` and set the value to `http://localhost:3000`.</span></span>

    ![Uma captura de tela da página registrar um aplicativo](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="b782a-125">Escolha **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="b782a-125">Choose **Register**.</span></span> <span data-ttu-id="b782a-126">Na página **tutorial do gráfico angular** , copie o valor da **ID do aplicativo (cliente)** e salve-o, você precisará dele na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="b782a-126">On the **Angular Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Uma captura de tela da ID do aplicativo do novo registro de aplicativo](/tutorial/images/aad-application-id.png)

## <a name="configure-the-sample"></a><span data-ttu-id="b782a-128">Configurar o exemplo</span><span class="sxs-lookup"><span data-stu-id="b782a-128">Configure the sample</span></span>

1. <span data-ttu-id="b782a-129">Renomear o `./graph-tutorial/src/Config.example.ts` arquivo para `./graph-tutorial/src/Config.ts` .</span><span class="sxs-lookup"><span data-stu-id="b782a-129">Rename the `./graph-tutorial/src/Config.example.ts` file to `./graph-tutorial/src/Config.ts`.</span></span>
1. <span data-ttu-id="b782a-130">Edite o `./graph-tutorial/src/Config.ts` arquivo e faça as seguintes alterações.</span><span class="sxs-lookup"><span data-stu-id="b782a-130">Edit the `./graph-tutorial/src/Config.ts` file and make the following changes.</span></span>
    1. <span data-ttu-id="b782a-131">Substitua `YOUR_APP_ID_HERE` pela **ID do aplicativo** obtida do portal de registro do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b782a-131">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="b782a-132">Na sua interface de linha de comando (CLI), navegue até o `graph-tutorial` diretório e execute o comando a seguir para instalar os requisitos.</span><span class="sxs-lookup"><span data-stu-id="b782a-132">In your command-line interface (CLI), navigate to the `graph-tutorial` directory and run the following command to install requirements.</span></span>

    ```Shell
    npm install
    ```

## <a name="run-the-sample"></a><span data-ttu-id="b782a-133">Executar o exemplo</span><span class="sxs-lookup"><span data-stu-id="b782a-133">Run the sample</span></span>

1. <span data-ttu-id="b782a-134">Execute o comando a seguir em sua CLI para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b782a-134">Run the following command in your CLI to start the application.</span></span>

    ```Shell
    npm start
    ```

1. <span data-ttu-id="b782a-135">Abra um navegador e navegue até `http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="b782a-135">Open a browser and browse to `http://localhost:3000`.</span></span>
