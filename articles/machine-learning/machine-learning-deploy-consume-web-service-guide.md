<properties
    pageTitle="Azure usług sieci Web uczenia maszynowego: Wdrożenie i zużycie | Microsoft Azure"
    description="Zasoby dotyczące wdrażania i przez inne usługi sieci web."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondl"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="v-donglo"/>

# <a name="azure-machine-learning-web-services-deployment-and-consumption"></a>Azure usług sieci Web nauki komputera: Wdrożenie i zużycie

Nauki maszynowego Azure umożliwia wdrażanie maszynowego uczenia się przepływy pracy i modeli jako usługi sieci web. Następnie można tych usług sieci web połączenie modeli maszynowego uczenia z aplikacji w Internecie zrobić przewidywań w czasie rzeczywistym lub w trybie partię. Ponieważ usługi sieci web są RESTful, możesz je połączenie z różnych języków programowania i platform, takich jak .NET i Java i aplikacjach, takich jak program Excel.

Kolejne sekcje zawierają łącza do instruktaży, kod i dokumentacji, które ułatwiają rozpoczęcie pracy.

## <a name="deploy-a-web-service"></a>Wdrażanie usługi sieci web

### <a name="with-azure-machine-learning-studio"></a>Z komputera Azure nauki Studio

Studio nauki komputera i portalu usługi sieci Web firmy Microsoft Azure maszynowego nauki ułatwiają wdrażanie i zarządzanie nią usługi sieci web bez pisania kodu.

Poniższe łącza udostępniają informacje ogólne dotyczące rozmieszczania Nowa usługa sieci web:

* Zawiera omówienie dotyczące rozmieszczania nowej usługi sieci web opartej na Menedżera zasobów Azure zobacz temat [Deploy Nowa usługa sieci web](machine-learning-webservice-deploy-a-web-service.md).
* Aby uzyskać instrukcje dotyczące wdrażania usługi sieci web zobacz [Wdrażanie usługi sieci web Azure maszynowego uczenia](machine-learning-publish-a-machine-learning-web-service.md).
* Aby uzyskać pełne instrukcje temat Tworzenie i wdrażanie usługi sieci web, zobacz [Przewodnik krok 1: tworzenie obszaru roboczego maszynowego uczenia](machine-learning-walkthrough-1-create-ml-workspace.md).
* Aby uzyskać określone przykłady wdrażanie usługi sieci web zobacz:

    * [Przewodnik krok 5: Wdrażanie usługi sieci web nauki maszynowego Azure](machine-learning-walkthrough-5-publish-web-service.md)
    * [Jak wdrożyć usługi sieci web do wielu regionów](machine-learning-how-to-deploy-to-multiple-regions.md)

### <a name="with-web-services-resource-provider-apis-azure-resource-manager-apis"></a>U dostawcy zasobów usług sieci web interfejsy API (API Menedżera zasobów Azure)

Azure maszynowego uczenia dostawcy zasobów dla usług sieci web umożliwia wdrożenia i zarządzanie usług sieci web przy użyciu interfejsu API usługi REST połączeń. Aby uzyskać więcej informacji zobacz informacje dotyczące [Usługi sieci Web uczenia komputera (RESZTA)](https://msdn.microsoft.com/library/azure/mt767538.aspx) w witrynie MSDN.

### <a name="with-powershell-cmdlets"></a>Przy użyciu poleceń cmdlet programu PowerShell

Azure maszynowego uczenia dostawcy zasobów dla usług sieci web umożliwia wdrożenia i zarządzanie usług sieci web przy użyciu poleceń cmdlet programu PowerShell.

Aby użyć poleceń cmdlet, musisz najpierw zalogować się na konto Azure z w środowisku programu PowerShell przy użyciu polecenia cmdlet [AzureRmAccount Dodaj](https://msdn.microsoft.com/library/mt619267.aspx) . Jeśli znasz wywoływanie polecenia programu PowerShell, które są oparte na Menedżera zasobów, zobacz [Używanie Azure przy użyciu Menedżera zasobów Azure](../powershell-azure-resource-manager.md#login-to-your-azure-account).

Aby wyeksportować do przewidywanych doświadczenia, należy użyć [tego przykładu kodu](https://github.com/ritwik20/AzureML-WebServices). Po utworzeniu pliku .exe z kodu, można wpisać:

    C:\<folder>\GetWSD <experiment-url> <workspace-auth-token>

Po uruchomieniu aplikacji tworzony szablon JSON usługi sieci web. Aby wdrożyć usługi sieci web za pomocą szablonu, należy dodać następujące informacje:

* Nazwę konta magazynu i klawiszy

    Można uzyskać nazwę konta magazynu i klucza z [Azure portal](https://portal.azure.com/) lub [portal Azure klasyczny](http://manage.windowsazure.com/).
* Identyfikator planu zatwierdzenia

    Zostanie wyświetlony identyfikator planu z portalu [Usługi sieci Web uczenia maszynowego Azure](https://services.azureml.net) , logowanie się i klikając nazwy planu.

Dodaj je do szablonu JSON jako węzła *Właściwości* na tym samym poziomie co węzeł *MachineLearningWorkspace* .

Oto przykład:

    "StorageAccount": {
            "name": "YourStorageAccountName",
            "key": "YourStorageAccountKey"
    },
    "CommitmentPlan": {
        "id": "subscriptions/YouSubscriptionID/resourceGroups/YourResourceGroupID/providers/Microsoft.MachineLearning/commitmentPlans/YourPlanName"
    }

Zobacz następujące artykuły i przykładowy kod, aby uzyskać dodatkowe informacje:

* [Polecenia cmdlet nauki maszynowego Azure]( https://msdn.microsoft.com/library/azure/mt767952.aspx) odwołanie w witrynie MSDN
* Przykładowe [Instruktaż](https://github.com/raymondlaghaeian/azureml-webservices-arm-powershell/blob/master/sample-commands.txt) na GitHub

## <a name="consume-the-web-services"></a>Używanie usług sieci web

### <a name="from-the-azure-machine-learning-web-services-ui-testing"></a>Z usług sieci Web uczenia Azure maszynowego interfejsu użytkownika (badań)

Istnieje możliwość przetestowania usługi sieci web z portalu usługi sieci Web uczenia maszynowego Azure. Zawiera testowania usługi żądanie odpowiedź (RR) oraz interfejsy usługi wsadowe (BES).

* [Wdrażanie nowej usługi sieci web](machine-learning-webservice-deploy-a-web-service.md)
* [Wdrażanie usługi sieci web nauki komputera Azure](machine-learning-publish-a-machine-learning-web-service.md)
* [Przewodnik krok 5: Wdrażanie usługi sieci web nauki maszynowego Azure](machine-learning-walkthrough-5-publish-web-service.md)

### <a name="from-excel"></a>Z programu Excel

Można pobrać szablon programu Excel, która korzysta z usługi sieci web:

* [Przez inne usługi sieci web Azure maszynowego uczenia z programu Excel](machine-learning-consuming-from-excel.md)
* [Dodatek programu Excel dla usług sieci Web uczenia maszynowego Azure](machine-learning-excel-add-in-for-web-services.md)


### <a name="from-a-rest-based-client"></a>W kliencie oparte na pozostałe

Azure usług sieci Web uczenia komputera są RESTful API. Mogą używać tych interfejsów API z różnych platform, takich jak .NET Python, R, Java, itp. Strona **Consume** usługi sieci web, w [portalu usługi sieci Web firmy Microsoft Azure komputera nauki](https://services.azureml.net) ma przykładowy kod, który pomoże Ci rozpocząć pracę. Aby uzyskać więcej informacji zobacz [jak korzystać z usługi sieci web Azure nauki komputera, który został wdrożony na komputerze nauki doświadczenia](machine-learning-consume-web-services.md).

