<properties
   pageTitle="Tworzenie szablonu wdrażania aplikacji logika | Microsoft Azure"
   description="Dowiedz się, jak utworzyć szablon wdrażania aplikacji logiki i używać go do zarządzania wersji"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="create-a-logic-app-deployment-template"></a>Tworzenie szablonu wdrażania aplikacji warunków logicznych

Po utworzeniu aplikacji logika, można go utworzyć jako szablonu Azure Menedżera zasobów. W ten sposób można łatwo rozmieścić aplikacji logika środowiska lub grupa zasobów, w której może być konieczne jej. Wprowadzenie do szablonów Menedżera zasobów należy wyewidencjonować artykuły dotyczące [tworzenia szablonów Menedżera zasobów Azure](../resource-group-authoring-templates.md) i [Wdrażanie zasobów przy użyciu szablonów Azure Menedżera zasobów](../resource-group-template-deploy.md).

## <a name="logic-app-deployment-template"></a>Szablon wdrażania aplikacji warunków logicznych

Aplikacja logiczny obejmuje trzy podstawowe składniki:

* **Logika aplikacji zasobów**. Ten zasób zawiera informacje dotyczące elementów takich jak ceny planu, lokalizację i definicję przepływu pracy.
* **Definicja przepływu pracy**. Jest to, co jest widoczne w widoku kodu. Zawiera definicję kroki przepływu oraz jak mają być wykonywane aparat. Jest to `definition` właściwości zasobu aplikacji logicznych.
* **Połączenia**. Są to oddzielnych zasobów, które są przechowywane bezpiecznie metadanych dotyczących wszystkie połączenia łącznika, takie jak parametry połączenia i token dostępu. Odwołanie w aplikacji dla warunków logicznych w `parameters` sekcji zasobów aplikacji logicznych.

Można wyświetlić wszystkie sztuk w przypadku istniejących aplikacji logika za pomocą narzędzia, takich jak [Azure Eksploratora zasobów](http://resources.azure.com).

Aby utworzyć szablon dla aplikacji logika do użytku z wdrożeniach grupa zasobów, należy zdefiniować zasobów i definiowanie parametrów stosownie do potrzeb. Na przykład jeśli jest instalowany rozwoju, testowanie i środowisku produkcyjnym, będą prawdopodobnie ma być innymi ciągami połączenia z bazą danych SQL poszczególnych środowisk. Możesz też wdrożyć w różnych subskrypcjach lub grup zasobów.  

## <a name="create-a-logic-app-deployment-template"></a>Tworzenie szablonu wdrażania aplikacji warunków logicznych

Najprostszym sposobem na prawidłową logiczny szablon wdrażania aplikacji jest [Visual Studio Tools dla aplikacji logicznych](./app-service-logic-deploy-from-vs.md).  Narzędzia Visual Studio generują prawidłowe wdrożenie szablonu, który można stosować w przypadku dowolnej subskrypcji lub lokalizację.

Jak utworzyć szablon wdrażania aplikacji logika może pomóc kilku innych narzędzi. Można pisać ręcznie, oznacza to, że przy użyciu zasobów już omawiane tutaj, aby utworzyć parametry, stosownie do potrzeb. Innym rozwiązaniem jest użycie modułu programu PowerShell [Kreator szablonu aplikacji logicznych](https://github.com/jeffhollan/LogicAppTemplateCreator) . Ten moduł Otwórz źródło oblicza najpierw aplikacja logiki i wszystkie połączenia go korzysta z, a następnie generuje zasoby szablonu z parametrów do wdrożenia. Na przykład jeśli masz aplikację logiczny, która odbiera wiadomości z kolejki Bus usługi Azure i dodaje dane do bazy danych programu Azure SQL, narzędzie zachować wszystkie logiki aranżacji i definiowanie parametrów parametry połączenia SQL i Bus usługi tak, aby można ustawić w wdrożenia.

>[AZURE.NOTE] Połączenia muszą być w tej samej grupy zasobów jako aplikacja logicznych.

### <a name="install-the-logic-app-template-powershell-module"></a>Instalowanie modułu programu PowerShell logiki aplikacji szablonu

Najłatwiejszym sposobem zainstalować moduł jest za pośrednictwem [Galerii programu PowerShell](https://www.powershellgallery.com/packages/LogicAppTemplate/0.1)przy użyciu polecenia `Install-Module -Name LogicAppTemplate`.  

Możesz również zainstalować moduł programu PowerShell ręcznie:

1. Pobierz najnowszą wersję [Kreator szablonu aplikacji logicznych](https://github.com/jeffhollan/LogicAppTemplateCreator/releases).  
1. Wyodrębnianie folder w folderze modułu programu PowerShell (zazwyczaj `%UserProfile%\Documents\WindowsPowerShell\Modules`).

Dla modułu do pracy z dostęp do wszystkich dzierżawy i subskrypcji token, zalecamy korzystanie z narzędzia wiersza polecenia [ARMClient](https://github.com/projectkudu/ARMClient) .  Ten [Ogłoszenie w blogu](http://blog.davidebbo.com/2015/01/azure-resource-manager-client.html) w tym artykule omówiono ARMClient bardziej szczegółowo.

### <a name="generate-a-logic-app-template-by-using-powershell"></a>Generowanie szablonu aplikacji logiczny przy użyciu programu PowerShell

Po zainstalowaniu programu PowerShell można wygenerować szablonu przy użyciu następującego polecenia:

`armclient token $SubscriptionId | Get-LogicAppTemplate -LogicApp MyApp -ResourceGroup MyRG -SubscriptionId $SubscriptionId -Verbose | Out-File C:\template.json`

`$SubscriptionId`to identyfikator Azure subskrypcji. Ten wiersz najpierw uzyskuje dostęp token za pośrednictwem ARMClient, a następnie przewody go za pomocą skryptu programu PowerShell, a następnie tworzy szablonu w pliku JSON.

## <a name="add-parameters-to-a-logic-app-template"></a>Dodawanie parametrów do szablonu aplikacji warunków logicznych

Po utworzeniu szablonu aplikacji logika, można dodać lub zmodyfikować parametry, które może być konieczne. Na przykład jeśli do definicji zawiera identyfikator zasobu Azure funkcji lub zagnieżdżonych przepływu pracy, który ma zostać umieszczony w jednym wdrożenia, możesz dodać więcej zasobów do szablonu i definiowanie parametrów identyfikatorów, stosownie do potrzeb. To samo dotyczy odwołań do niestandardowego interfejsy API ani Swagger punkty końcowe oczekuje wdrożenia z poszczególnych grup zasobów.

## <a name="deploy-a-logic-app-template"></a>Rozmieszczanie szablonu aplikacji logika

Za pomocą dowolnego numeru narzędzi, takich jak programu PowerShell, interfejsu API usługi REST Visual Studio wersji zarządzania i wdrażania szablon Portal Azure, należy wdrożyć szablonu. Zobacz ten artykuł o [Wdrażanie zasobów przy użyciu szablonów Azure Menedżera zasobów](../resource-group-template-deploy.md) , aby uzyskać dodatkowe informacje. Ponadto zaleca się utworzenie [pliku parametrów](../resource-group-template-deploy.md#parameter-file) do przechowywania wartości parametru.

### <a name="authorize-oauth-connections"></a>Autoryzuj OAuth połączenia

Po wdrożeniu kompleksowe-z prawidłowych parametrów działania aplikacji logika. Jednak połączenia OAuth nadal muszą mieć możliwość wygenerowania tokenu upoważniona do uzyskiwania dostępu. Możesz to zrobić otwierając aplikacji logicznych w projektancie, a następnie autoryzowanie połączeń. Lub, jeśli chcesz zautomatyzować, umożliwia skrypt wyraża zgodę na każdego połączenia OAuth. Ma w obszarze Projekt [LogicAppConnectionAuth](https://github.com/logicappsio/LogicAppConnectionAuth) przykładowy skrypt GitHub.

## <a name="visual-studio-release-management"></a>Zarządzanie wersji programu Visual Studio

Typowy scenariusz dotyczących wdrażania i zarządzania środowiskiem jest narzędzie, takie jak zarządzanie wersji programu Visual Studio, za pomocą szablonu wdrażania aplikacji logicznych. Visual Studio Team Services zawiera zadania [Wdrażanie Azure grupa zasobów](https://github.com/Microsoft/vsts-tasks/tree/master/Tasks/DeployAzureResourceGroup) , można dodać do dowolnego kompilacji lub zwolnij procesu. Musisz mieć [głównej usługi](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/) autoryzacji do wdrożenia, a następnie można Wygeneruj definicji wersji.

1. W obszarze Zarządzanie wersji aby utworzyć nową definicję, wybierz **pusty** rozpoczynać pusta definicja.

    ![Utworzenie nowego, pustego definicji][1]   

1. Wybierz pozycję wszystkie zasoby, które są potrzebne w tym. Prawdopodobnie będzie szablonu aplikacji logika wygenerowane ręcznie lub jako część procesu tworzenia.
1. Dodawanie zadań **Rozmieszczania grupa zasobów Azure** .
1. Konfigurowanie z [głównej usługi](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/), a odwołanie pliku szablonu i parametrów szablonu.
1. Przejdź do tworzenia etapów procesu wersji dla dowolnego środowiska, test automatyczną lub osoby zatwierdzające stosownie do potrzeb.

<!-- Image References -->
[1]: ./media/app-service-logic-create-deploy-template/emptyReleaseDefinition.PNG
