<properties
    pageTitle="Metadane aplikacji interfejsu API usługi aplikacji do generowania odnajdowanie i kod interfejsu API | Microsoft Azure"
    description="Korzystanie z jak aplikacje interfejsu API usługi aplikacji Azure metadanych Swagger ułatwiające Generowanie odnajdowanie i kod interfejsu API."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="rachelap"/>

# <a name="app-service-api-apps-metadata-for-api-discovery-and-code-generation"></a>Metadane aplikacji interfejsu API usługi aplikacji do generowania odnajdowanie i kod interfejsu API 

Obsługa [Swagger 2.0](http://swagger.io/) interfejsu API metadanych jest wbudowany w interfejs API usługi aplikacji. Nie musisz użyć Swagger, ale jeśli używasz, można korzystać z funkcji interfejsu API aplikacje, które ułatwić odnajdowanie i zużycie.   

## <a name="swagger-endpoint"></a>Swagger punktu końcowego

Możesz określić punktu końcowego, zawierającego Swagger 2.0 JSON metadanych aplikacji interfejsu API we właściwości aplikacji interfejsu API. Punkt końcowy może być względem podstawowy adres URL aplikacji interfejsu API lub bezwzględnego adresu URL. Bezwzględne adres URL można wskazać spoza aplikacji interfejsu API. 

Wielu klientów podrzędne (na przykład generowanie kodu programu Visual Studio i PowerApps przepływu "Dodaj interfejsu API"), adres URL musi być publicznie (nie był chroniony przez użytkownika lub uwierzytelniania usługi). Oznacza to, że jeśli korzystasz z aplikacji usługi uwierzytelniania i udostępnić definicję interfejsu API z samej aplikacji, należy użyć uwierzytelniania opcja, która umożliwia anonimowym ruchu nawiązywać połączenia z interfejsu API. Aby uzyskać więcej informacji zobacz [uwierzytelniania i autoryzacji dla interfejsu API usługi aplikacji](app-service-api-authentication.md).

### <a name="portal-blade"></a>Karta portalu

W [portalu Azure](https://portal.azure.com/) adres URL punktu końcowego można widoczne i zmieniony na karta **Definicji interfejsu API** .

![](./media/app-service-api-metadata/apidefblade.png)

### <a name="azure-resource-manager-property"></a>Azure właściwości Menedżera zasobów

Adres URL definicji interfejsu API dla aplikacji interfejsu API można również skonfigurować przy użyciu [Eksploratora zasobów](https://resources.azure.com/) lub [Szablony Menedżera zasobów Azure](../resource-group-authoring-templates.md) narzędzi wiersza polecenia, takich jak [Azure programu PowerShell](../powershell-install-configure.md) i [Polecenie Azure](../xplat-cli-install.md). 

W **Eksploratorze zasobów**, przejdź do **Subskrypcje > {subskrypcji} > resourceGroups > {grupy zasobów} > dostawców > Microsoft.Web > witryny > {witryny} > konfiguracji > sieci web**, a zobaczysz `apiDefinition` właściwości:

        "apiDefinition": {
          "url": "https://contactslistapi.azurewebsites.net/swagger/docs/v1"
        }

Przykład szablonu Menedżera zasobów Azure, który ustawia `apiDefinition` właściwości, otwórz [plik azuredeploy.json w przykładowej aplikacji listy zadań do wykonania](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json). Znajdź sekcję szablonu, który wygląda jak przykład JSON, jak pokazano powyżej.

### <a name="default-value"></a>Wartość domyślna

Gdy używasz programu Visual Studio do tworzenia aplikacji interfejsu API punkt końcowy definicji interfejsu API jest automatycznie ustawiany na podstawowy adres URL aplikacji interfejsu API plus `/swagger/docs/v1`. To jest domyślny adres URL korzystającego z pakietu [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet służyć generowanych dynamicznie metadanych Swagger w projekcie interfejsu API sieci Web programu ASP.NET. 

## <a name="code-generation"></a>Generowanie kodu

Jedną z zalet integracji Swagger aplikacje Azure API jest generowanie kodu automatyczne. Klienta wygenerowany zajęcia ułatwiają wpisz kod, który wymaga aplikacji interfejsu API.

Istnieje możliwość wygenerowania kodu klienta aplikacji interfejsu API przy użyciu programu Visual Studio lub z poziomu wiersza polecenia. Aby uzyskać informacje o sposobach generowania klasy klienta w programie Visual Studio w projekcie interfejsu API sieci Web programu ASP.NET zobacz [Wprowadzenie do aplikacji interfejsu API i ASP.NET](app-service-api-dotnet-get-started.md#codegen). Aby dowiedzieć się, jak to zrobić z poziomu wiersza polecenia dla wszystkich obsługiwanych języków zobacz plik readme repozytorium [Azure i autorest](https://github.com/azure/autorest) na GitHub.com.
 
## <a name="next-steps"></a>Następne kroki

Samouczek krok po kroku, który przeprowadzi Cię przez proces tworzenia wdrażania i używające aplikację interfejsu API, zobacz [Wprowadzenie do aplikacji interfejsu API usługi aplikacji Azure](app-service-api-dotnet-get-started.md).

Jeśli używasz usługi zarządzania interfejsu API Azure z aplikacji interfejsu API służy Swagger metadanych do importowania z interfejsu API do zarządzania interfejsu API. Aby uzyskać więcej informacji zobacz [Importowanie definicji interfejsu API z operacjami wykonywanymi w zarządzaniu interfejsu API Azure](../api-management/api-management-howto-import-api.md). 
