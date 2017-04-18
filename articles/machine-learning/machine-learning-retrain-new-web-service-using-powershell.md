<properties
    pageTitle="Przekwalifikować usługi sieci web przy użyciu polecenia cmdlet programu PowerShell zarządzania nauki komputera | Microsoft Azure"
    description="Dowiedz się, jak programowo przy przekwalifikowaniu modelu i zaktualizować usługi sieci web używać modelu nowo przeszkolony w Azure nauki komputera przy użyciu poleceń cmdlet programu PowerShell zarządzania nauki komputera."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondlaghaeian"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="v-donglo"/>

# <a name="retrain-a-new-web-service-using-the-machine-learning-management-powershell-cmdlets"></a>Przekwalifikować usługi sieci web przy użyciu polecenia cmdlet programu PowerShell zarządzania nauki komputera

Po przekwalifikować Nowa usługa sieci web, możesz zaktualizować definicji usługi sieci web przewidywanych do odwołania do nowego modelu przeszkolony.  

## <a name="prerequisites"></a>Wymagania wstępne

Musisz skonfigurowano eksperyment szkolenia i eksperyment przewidywanych przedstawione [przy przekwalifikowaniu maszynowego uczenia modeli programowy](machine-learning-retrain-models-programmatically.md). 

>[AZURE.IMPORTANT] Należy wdrożyć przewidywanych doświadczenie jako Menedżer zasobów Azure (nowe) podstawie nauki usługi sieci web. 
 
Dodatkowe informacje na temat wdrażanie usług sieci web zobacz temat [Deploy usługi sieci web Azure maszynowego uczenia](machine-learning-publish-a-machine-learning-web-service.md).

Ten proces wymaga, że zainstalowano cmdlet Azure maszynowego uczenia. Uzyskać instalowania maszynowego uczenia polecenia cmdlet zobacz informacje dotyczące [Poleceń cmdlet nauki maszynowego Azure](https://msdn.microsoft.com/library/azure/mt767952.aspx) w witrynie MSDN.

Skopiowane następujące informacje z przekwalifikowania wyników:

* BaseLocation
* RelativeLocation

Dostępne są następujące czynności, które możesz wykonać:

1.  Zaloguj się do swojego konta Azure Menedżera zasobów.
2.  Pobieranie definicji usługi sieci web
3.  Eksportowanie definicji usługi sieci Web jako JSON
4.  Zaktualizuj odwołanie do obiektów blob ilearner w formacie JSON.
5.  Importowanie JSON do definicji usługi sieci Web
6.  Aktualizowanie usługi sieci web przy użyciu nowych definicji usługi sieci Web

## <a name="sign-in-to-your-azure-resource-manager-account"></a>Zaloguj się do konta Menedżera zasobów Azure

Najpierw musisz się zalogować do swojego konta Azure z poziomu środowiska PowerShell przy użyciu polecenia cmdlet [AzureRmAccount Dodaj](https://msdn.microsoft.com/library/mt619267.aspx) .

## <a name="get-the-web-service-definition"></a>Pobieranie definicji usługi sieci Web

Następnie Uzyskaj usługi sieci Web, dzwoniąc do polecenia cmdlet [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) . Definicji usługi sieci Web jest reprezentacji wewnętrznej przeszkolony modelu usługi sieci web i nie można bezpośrednio modyfikować. Upewnij się, że są pobierane definicji usługi sieci Web dla swojej przewidywanych doświadczenia i nie doświadczenia szkolenie.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Aby określić nazwę grupy zasobów istniejące usługi sieci web, uruchom polecenie cmdlet Get-AzureRmMlWebService bez parametrów do wyświetlenia w ramach subskrypcji usługi sieci web. Znajdź usługi sieci web, a następnie spójrz na jej identyfikator sieci web usługi. Nazwa grupy zasobów jest czwartym elementu w polu Identyfikator zaraz po elemencie *resourceGroups* . W poniższym przykładzie nazwa grupy zasobów jest domyślne — MachineLearning — SouthCentralUS.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Możesz też ustalenie Nazwa grupy zasobów istniejące usługi sieci web, zaloguj się do portalu usługi sieci Web nauki Microsoft Azure komputera. Wybierz usługę sieci web. Nazwa grupy zasobów jest piątym element adresu URL usługi sieci web zaraz po elemencie *resourceGroups* . W poniższym przykładzie nazwa grupy zasobów jest domyślne — MachineLearning — SouthCentralUS.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-as-json"></a>Eksportowanie definicji usługi sieci Web jako JSON

Modyfikowania definicji przeszkolony modelu używać nowo przeszkolony modelu, należy najpierw umożliwia [Eksportowanie AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) polecenia cmdlet go wyeksportować do pliku formatu JSON.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob-in-the-json"></a>Zaktualizuj odwołanie do obiektów blob ilearner w formacie JSON.

W elementy zawartości zlokalizuj [model przeszkolony], zaktualizować wartość *identyfikatora uri* w węźle *locationInfo* identyfikator URI obiektów blob ilearner. Identyfikator URI jest generowany przez połączenie *BaseLocation* i *RelativeLocation* z wyników BES przeszkolenie połączenia.

     "asset3": {
        "name": "Retrain Samp.le [trained model]",
        "type": "Resource",
        "locationInfo": {
          "uri": "https://mltestaccount.blob.core.windows.net/azuremlassetscontainer/baca7bca650f46218633552c0bcbba0e.ilearner"
        },
        "outputPorts": {
          "Results dataset": {
            "type": "Dataset"
          }
        }
      },

## <a name="import-the-json-into-a-web-service-definition"></a>Importowanie JSON do definicji usługi sieci Web

Do konwertowania zmodyfikowany plik JSON do definicji usługi sieci Web, która umożliwia aktualizowanie doświadczenia Predicative należy użyć polecenia cmdlet [AzureRmMlWebService importu](https://msdn.microsoft.com/library/azure/mt767925.aspx) .

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service-with-new-web-service-definition"></a>Aktualizowanie usługi sieci web przy użyciu nowych definicji usługi sieci Web

Na koniec możesz użyć polecenia cmdlet [AzureRmMlWebService aktualizacji](https://msdn.microsoft.com/library/azure/mt767922.aspx) zaktualizować przewidywanych doświadczenia.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'  -ServiceUpdates $wsd

## <a name="summary"></a>Podsumowanie

Polecenia cmdlet zarządzania programu PowerShell nauki komputera można aktualizować przeszkolony modelu przewidywanych usługi sieci Web, umożliwiając scenariuszy, takich jak:

* Model okresowych przeszkolenie nowymi danymi.
* Rozkład modelu dla klientów w celu co ich przekwalifikować modelu przy użyciu własnych danych.
