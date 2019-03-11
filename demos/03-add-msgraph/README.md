# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="defa6-101">Como executar o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="defa6-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="defa6-102">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="defa6-102">Prerequisites</span></span>

<span data-ttu-id="defa6-103">Para executar o projeto concluído nessa pasta, você precisará do seguinte:</span><span class="sxs-lookup"><span data-stu-id="defa6-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="defa6-104">[Visual Studio](https://visualstudio.microsoft.com/vs/) instalado em sua máquina de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="defa6-104">[Visual Studio](https://visualstudio.microsoft.com/vs/) installed on your development machine.</span></span> <span data-ttu-id="defa6-105">Se você não tiver o Visual Studio, visite o link anterior para opções de download.</span><span class="sxs-lookup"><span data-stu-id="defa6-105">If you do not have Visual Studio, visit the previous link for download options.</span></span> <span data-ttu-id="defa6-106">(**Observação:** este tutorial foi escrito com o Visual Studio 2017 versão 15,81.</span><span class="sxs-lookup"><span data-stu-id="defa6-106">(**Note:** This tutorial was written with Visual Studio 2017 version 15.81.</span></span> <span data-ttu-id="defa6-107">As etapas deste guia podem funcionar com outras versões, mas que não foram testadas.</span><span class="sxs-lookup"><span data-stu-id="defa6-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="defa6-108">Uma conta pessoal da Microsoft com uma caixa de correio no Outlook.com ou uma conta corporativa ou de estudante da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="defa6-108">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="defa6-109">Se você não tem uma conta da Microsoft, há algumas opções para obter uma conta gratuita:</span><span class="sxs-lookup"><span data-stu-id="defa6-109">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="defa6-110">Você pode [se inscrever para uma nova conta pessoal da Microsoft](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="defa6-110">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="defa6-111">Você pode [se inscrever no programa para desenvolvedores do office 365](https://developer.microsoft.com/office/dev-program) para obter uma assinatura gratuita do Office 365.</span><span class="sxs-lookup"><span data-stu-id="defa6-111">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-application-registration-portal"></a><span data-ttu-id="defa6-112">Registrar um aplicativo Web com o portal de registro do aplicativo</span><span class="sxs-lookup"><span data-stu-id="defa6-112">Register a web application with the Application Registration Portal</span></span>

1. <span data-ttu-id="defa6-113">Abra um navegador e navegue até o [portal de registro do aplicativo](https://apps.dev.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="defa6-113">Open a browser and navigate to the [Application Registration Portal](https://apps.dev.microsoft.com).</span></span> <span data-ttu-id="defa6-114">Faça logon usando uma **conta pessoal** (aka: conta da Microsoft) ou **conta corporativa ou de estudante**.</span><span class="sxs-lookup"><span data-stu-id="defa6-114">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="defa6-115">Selecione **Adicionar um aplicativo** na parte superior da página.</span><span class="sxs-lookup"><span data-stu-id="defa6-115">Select **Add an app** at the top of the page.</span></span>

    > <span data-ttu-id="defa6-116">**Observação:** Se você vir mais de um botão **Adicionar um aplicativo** na página, selecione aquele que corresponde à lista **aplicativos convergidos** .</span><span class="sxs-lookup"><span data-stu-id="defa6-116">**Note:** If you see more than one **Add an app** button on the page, select the one that corresponds to the **Converged apps** list.</span></span>

1. <span data-ttu-id="defa6-117">Na página **registrar seu aplicativo** , defina o **nome do aplicativo** como o **tutorial de gráfico reagir** e selecione **criar**.</span><span class="sxs-lookup"><span data-stu-id="defa6-117">On the **Register your application** page, set the **Application Name** to **React Graph Tutorial** and select **Create**.</span></span>

    ![Captura de tela da criação de um novo aplicativo no site do portal de registro de aplicativo](/tutorial/images/arp-create-app-01.png)

1. <span data-ttu-id="defa6-119">Na página enviar o **tutorial do gráfico de reagir** , na seção **Propriedades** , copie a **ID do aplicativo** , pois você precisará dela mais tarde.</span><span class="sxs-lookup"><span data-stu-id="defa6-119">On the **React Graph Tutorial Registration** page, under the **Properties** section, copy the **Application Id** as you will need it later.</span></span>

    ![Captura de tela da ID do aplicativo recém-criado](/tutorial/images/arp-create-app-02.png)

1. <span data-ttu-id="defa6-121">Role para baixo até a seção **plataformas** .</span><span class="sxs-lookup"><span data-stu-id="defa6-121">Scroll down to the **Platforms** section.</span></span>

    1. <span data-ttu-id="defa6-122">Selecione **Adicionar plataforma**.</span><span class="sxs-lookup"><span data-stu-id="defa6-122">Select **Add Platform**.</span></span>
    1. <span data-ttu-id="defa6-123">Na caixa de diálogo **Adicionar plataforma** , selecione **Web**.</span><span class="sxs-lookup"><span data-stu-id="defa6-123">In the **Add Platform** dialog, select **Web**.</span></span>

        ![Captura de tela criando uma plataforma para o aplicativo](/tutorial/images/arp-create-app-03.png)

    1. <span data-ttu-id="defa6-125">Na caixa plataforma **da Web** , insira `http://localhost:3000` para as **URLs**de redirecionamento.</span><span class="sxs-lookup"><span data-stu-id="defa6-125">In the **Web** platform box, enter `http://localhost:3000` for the **Redirect URLs**.</span></span>

        ![Captura de tela da nova plataforma Web adicionada para o aplicativo](/tutorial/images/arp-create-app-04.png)

1. <span data-ttu-id="defa6-127">Role até a parte inferior da página e selecione **salvar**.</span><span class="sxs-lookup"><span data-stu-id="defa6-127">Scroll to the bottom of the page and select **Save**.</span></span>

## <a name="configure-the-sample"></a><span data-ttu-id="defa6-128">Configurar o exemplo</span><span class="sxs-lookup"><span data-stu-id="defa6-128">Configure the sample</span></span>

1. <span data-ttu-id="defa6-129">ReNomear `./graph-tutorial/src/Config.js.example` o `./graph-tutorial/src/Config.js`arquivo para.</span><span class="sxs-lookup"><span data-stu-id="defa6-129">Rename the `./graph-tutorial/src/Config.js.example` file to `./graph-tutorial/src/Config.js`.</span></span>
1. <span data-ttu-id="defa6-130">Edite `./graph-tutorial/src/Config.js` o arquivo e faça as seguintes alterações.</span><span class="sxs-lookup"><span data-stu-id="defa6-130">Edit the `./graph-tutorial/src/Config.js` file and make the following changes.</span></span>
    1. <span data-ttu-id="defa6-131">Substitua `YOUR_APP_ID_HERE` pela **ID do aplicativo** obtida do portal de registro do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="defa6-131">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="defa6-132">Na sua interface de linha de comando (CLI), navegue até `graph-tutorial` o diretório e execute o comando a seguir para instalar os requisitos.</span><span class="sxs-lookup"><span data-stu-id="defa6-132">In your command-line interface (CLI), navigate to the `graph-tutorial` directory and run the following command to install requirements.</span></span>

    ```Shell
    npm install
    ```

## <a name="run-the-sample"></a><span data-ttu-id="defa6-133">Executar o exemplo</span><span class="sxs-lookup"><span data-stu-id="defa6-133">Run the sample</span></span>

1. <span data-ttu-id="defa6-134">Execute o comando a seguir em sua CLI para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="defa6-134">Run the following command in your CLI to start the application.</span></span>

    ```Shell
    npm start
    ```

1. <span data-ttu-id="defa6-135">Abra um navegador e navegue até `http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="defa6-135">Open a browser and browse to `http://localhost:3000`.</span></span>