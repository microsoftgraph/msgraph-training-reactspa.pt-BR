# <a name="getting-started-with-create-react-app"></a><span data-ttu-id="239a0-101">Getting Started with Create React App</span><span class="sxs-lookup"><span data-stu-id="239a0-101">Getting Started with Create React App</span></span>

<span data-ttu-id="239a0-102">Esse projeto foi inicializado com [Create React App.](https://github.com/facebook/create-react-app)</span><span class="sxs-lookup"><span data-stu-id="239a0-102">This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).</span></span>

## <a name="available-scripts"></a><span data-ttu-id="239a0-103">Scripts disponíveis</span><span class="sxs-lookup"><span data-stu-id="239a0-103">Available Scripts</span></span>

<span data-ttu-id="239a0-104">No diretório do projeto, você pode executar:</span><span class="sxs-lookup"><span data-stu-id="239a0-104">In the project directory, you can run:</span></span>

### `yarn start`

<span data-ttu-id="239a0-105">Executa o aplicativo no modo de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="239a0-105">Runs the app in the development mode.</span></span>\
<span data-ttu-id="239a0-106">Abra [http://localhost:3000](http://localhost:3000) para exibi-lo no navegador.</span><span class="sxs-lookup"><span data-stu-id="239a0-106">Open [http://localhost:3000](http://localhost:3000) to view it in the browser.</span></span>

<span data-ttu-id="239a0-107">A página será recarregada se você fizer edições.</span><span class="sxs-lookup"><span data-stu-id="239a0-107">The page will reload if you make edits.</span></span>\
<span data-ttu-id="239a0-108">Você também verá quaisquer erros de lint no console.</span><span class="sxs-lookup"><span data-stu-id="239a0-108">You will also see any lint errors in the console.</span></span>

### `yarn test`

<span data-ttu-id="239a0-109">Inicia o test runner no modo de relógio interativo.</span><span class="sxs-lookup"><span data-stu-id="239a0-109">Launches the test runner in the interactive watch mode.</span></span>\
<span data-ttu-id="239a0-110">Consulte a seção sobre [a execução de testes](https://facebook.github.io/create-react-app/docs/running-tests) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="239a0-110">See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.</span></span>

### `yarn build`

<span data-ttu-id="239a0-111">Cria o aplicativo para produção na `build` pasta.</span><span class="sxs-lookup"><span data-stu-id="239a0-111">Builds the app for production to the `build` folder.</span></span>\
<span data-ttu-id="239a0-112">Ele agrupa corretamente o React no modo de produção e otimiza a com build para melhorar o desempenho.</span><span class="sxs-lookup"><span data-stu-id="239a0-112">It correctly bundles React in production mode and optimizes the build for the best performance.</span></span>

<span data-ttu-id="239a0-113">O build é minificado e os nomes de arquivo incluem os hashes.</span><span class="sxs-lookup"><span data-stu-id="239a0-113">The build is minified and the filenames include the hashes.</span></span>\
<span data-ttu-id="239a0-114">Seu aplicativo está pronto para ser implantado!</span><span class="sxs-lookup"><span data-stu-id="239a0-114">Your app is ready to be deployed!</span></span>

<span data-ttu-id="239a0-115">Consulte a seção sobre [implantação](https://facebook.github.io/create-react-app/docs/deployment) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="239a0-115">See the section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.</span></span>

### `yarn eject`

<span data-ttu-id="239a0-116">**Observação: esta é uma operação de ida e volta. Depois que `eject` você , você não pode voltar!**</span><span class="sxs-lookup"><span data-stu-id="239a0-116">**Note: this is a one-way operation. Once you `eject`, you can’t go back!**</span></span>

<span data-ttu-id="239a0-117">Se você não estiver satisfeito com a ferramenta de com build e as opções de configuração, poderá `eject` a qualquer momento.</span><span class="sxs-lookup"><span data-stu-id="239a0-117">If you aren’t satisfied with the build tool and configuration choices, you can `eject` at any time.</span></span> <span data-ttu-id="239a0-118">Esse comando removerá a dependência de com build única do seu projeto.</span><span class="sxs-lookup"><span data-stu-id="239a0-118">This command will remove the single build dependency from your project.</span></span>

<span data-ttu-id="239a0-119">Em vez disso, ele copiará todos os arquivos de configuração e as dependências transitivas (webpack, Quel, ESLint, etc.) para que você tenha controle total sobre eles.</span><span class="sxs-lookup"><span data-stu-id="239a0-119">Instead, it will copy all the configuration files and the transitive dependencies (webpack, Babel, ESLint, etc) right into your project so you have full control over them.</span></span> <span data-ttu-id="239a0-120">Todos os comandos, exceto ainda funcionarão, mas eles apontarão para os `eject` scripts copiados para que você possa ajustá-los.</span><span class="sxs-lookup"><span data-stu-id="239a0-120">All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them.</span></span> <span data-ttu-id="239a0-121">Neste ponto, você está por conta própria.</span><span class="sxs-lookup"><span data-stu-id="239a0-121">At this point you’re on your own.</span></span>

<span data-ttu-id="239a0-122">Você não precisa usar `eject` nunca.</span><span class="sxs-lookup"><span data-stu-id="239a0-122">You don’t have to ever use `eject`.</span></span> <span data-ttu-id="239a0-123">O conjunto de recursos formatados é adequado para implantações de pequeno e médio porte, e você não deve se sentir obrigado a usar esse recurso.</span><span class="sxs-lookup"><span data-stu-id="239a0-123">The curated feature set is suitable for small and middle deployments, and you shouldn’t feel obligated to use this feature.</span></span> <span data-ttu-id="239a0-124">No entanto, entendemos que essa ferramenta não seria útil se você não pudesse personalizá-la quando estiver pronto para ela.</span><span class="sxs-lookup"><span data-stu-id="239a0-124">However we understand that this tool wouldn’t be useful if you couldn’t customize it when you are ready for it.</span></span>

## <a name="learn-more"></a><span data-ttu-id="239a0-125">Saiba Mais</span><span class="sxs-lookup"><span data-stu-id="239a0-125">Learn More</span></span>

<span data-ttu-id="239a0-126">Você pode saber mais na documentação [Criar Aplicativo React.](https://facebook.github.io/create-react-app/docs/getting-started)</span><span class="sxs-lookup"><span data-stu-id="239a0-126">You can learn more in the [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).</span></span>

<span data-ttu-id="239a0-127">Para saber mais sobre React, confira a [documentação do React.](https://reactjs.org/)</span><span class="sxs-lookup"><span data-stu-id="239a0-127">To learn React, check out the [React documentation](https://reactjs.org/).</span></span>
