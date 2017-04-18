<properties
    pageTitle="Aplikacja sieci Web narzędzia Azure wiersza polecenia i platform opartych na Menedżera zasobów dla Azure | Microsoft Azure"
    description="Dowiedz się, jak zarządzać swoimi aplikacjami sieci Web Azure za pomocą nowych narzędzi wiersza polecenia oparte na Menedżera zasobów Azure między platformami."
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="aelnably"/>

# <a name="using-azure-resource-manager-based-xplat-cli-for-azure-web-app"></a>Przy użyciu Azure zasobów Menedżer XPlat interfejs wiersza polecenia dla aplikacji sieci Web Azure#

> [AZURE.SELECTOR]
- [Polecenie Azure](app-service-web-app-azure-resource-manager-xplat-cli.md)
- [Azure programu PowerShell](app-service-web-app-azure-resource-manager-powershell.md)

W wersji narzędzia wiersza polecenia i platform Microsoft Azure wersji 0.10.5 zostały dodane nowe polecenia. Te polecenia umożliwiania użytkownika za pomocą polecenia programu PowerShell oparte na Menedżera zasobów Azure Zarządzanie aplikacjami sieci Web.

Aby uzyskać informacje o zarządzaniu grup zasobów, zobacz [Używanie polecenie Azure Zarządzanie Azure zasobów i grup zasobów](../xplat-cli-azure-resource-manager.md). 


## <a name="managing-app-service-plans"></a>Zarządzanie usługą aplikacji plany ##

### <a name="create-an-app-service-plan"></a>Tworzenie planu aplikacji usługi ###
Aby utworzyć plan usług aplikacji, użyj polecenia **Utwórz azure appserviceplan** .

Poniżej przedstawiono opisy różnych parametrów:

-   **— Grupa zasobów**: Grupa zasobów, która zawiera aplikację nowo utworzony plan usług.
-   **— Nazwa**: Nazwa aplikacji plan usług.
-   **— lokalizacji**: Lokalizacja plan usługi aplikacji.
-   **— Warstwa**: odpowiedniej wersji cennik (są następujące opcje: F1 (bezpłatnie). D1 (udostępniony). B1 (mała podstawowe), B2 (średnia podstawowe) i B3 (Basic duże). S1 (mała standardowe), S2 (standardowy średnia) i S3 (Standard duże). P1 (mała Premium), P2 (Premium średnia), a P3 (Premium dużych).)
-   **--wystąpienia**: liczba pracowników w aplikacji usługi plan (wartością domyślną jest 1).

Przykład, aby użyć tego polecenia cmdlet:

    azure appserviceplan create --name ContosoAppServicePlan --location "South Central US" --resource-group ContosoAzureResourceGroup --sku P1 --instances 10

### <a name="list-existing-app-service-plans"></a>Plany usługi aplikacji z istniejącej listy ###

Aby wyświetlić listę istniejących planów usługi aplikacji, użyj polecenia **azure appserviceplan listy** .

Aby wyświetlić listę wszystkich planów usługi aplikacji w obszarze danej grupy zasobów, należy użyć:

    azure appserviceplan list --resource-group ContosoAzureResourceGroup

Aby uzyskać plan usług konkretnej aplikacji, użyj polecenia **Pokaż azure appserviceplan** :

    azure appserviceplan show --name ContosoAppServicePlan --resource-group southeastasia

### <a name="configure-an-existing-app-service-plan"></a>Konfigurowanie istniejącej aplikacji usługi Plan ###

Aby zmienić ustawienia dla istniejącego planu usługi aplikacji, użyj polecenia **azure appserviceplan konfiguracji** . Można zmienić używaną wersję i liczbę pracowników 

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1 --instances 9

#### <a name="scaling-an-app-service-plan"></a>Skalowanie Plan usług aplikacji ####

Aby przeskalować istniejącej aplikacji usługi Plan, należy użyć:

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --instances 9

#### <a name="changing-the-sku-of-an-app-service-plan"></a>Zmienianie SKU planu aplikacji usługi ####

Aby zmienić sku planu istniejącej aplikacji usługi, należy użyć:

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1


### <a name="delete-an-existing-app-service-plan"></a>Usuwanie istniejącej aplikacji usługi Plan ###

Aby usunąć istniejący plan usług aplikacji, wszystkie aplikacje web przydzielonych konieczne można przenosić ani usuwać najpierw. Następnie za pomocą polecenia **Usuń azure aplikacji sieci Web** , które można usunąć plan usług aplikacji.

    azure appserviceplan delete --name ContosoAppServicePlan --resource-group southeastasia

## <a name="managing-app-service-web-apps"></a>Zarządzanie aplikacjami sieci Web aplikacji usługi ##

### <a name="create-a-web-app"></a>Tworzenie aplikacji sieci Web ###

Aby utworzyć aplikację sieci web, użyj polecenia **Utwórz azure aplikacji sieci Web** .

Poniżej przedstawiono opisy różnych parametrów:

- **--Nazwa**: Nazwa aplikacji sieci web.
- **— plan**: Nazwa plan usług używany do obsługi aplikacji sieci web.
- **— Grupa zasobów**: Grupa zasobów, która obsługuje plan usług aplikacji.
- **— lokalizacji**: Lokalizacja aplikacji sieci web.

Przykład, aby użyć tego polecenia cmdlet:

    azure webapp create --name ContosoWebApp --resource-group ContosoAzureResourceGroup --plan ContosoAppServicePlan --location "South Central US"

### <a name="delete-an-existing-web-app"></a>Usuwanie istniejącej aplikacji sieci Web ###

Aby usunąć istniejącej aplikacji sieci web możesz użyć polecenia **Usuń azure aplikacji sieci Web** , musisz podać nazwę aplikacji sieci web i nazwa grupy zasobów.

    azure webapp delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="list-existing-web-apps"></a>W aplikacjach sieci Web z istniejącej listy ###

Aby wyświetlić listę istniejących aplikacji sieci web, użyj polecenia **listy azure aplikacji sieci Web** .

Aby wyświetlić listę wszystkich aplikacji sieci web w obszarze danej grupy zasobów, należy użyć:

    azure webapp list --resource-group ContosoAzureResourceGroup

Aby pobrać aplikację sieci web określonych, użyj polecenia **Pokaż azure aplikacji sieci Web** .

    azure webapp show --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="configure-an-existing-web-app"></a>Konfigurowanie istniejącej aplikacji sieci Web ###

Aby zmienić ustawienia i konfiguracji istniejącej aplikacji sieci web, użyj polecenia **set config azure aplikacji sieci Web** .

Przykład (1): Zmień wersję php aplikacji web App 

    azure webapp config set --name ContosoWebApp --resource-group ContosoAzureResourceGroup --phpversion 5.6

Przykład (2): Dodawanie lub zmienianie ustawień aplikacji

    webapp config appsettings set --name ContosoWebApp --resource-group ContosoAzureResourceGroup appsetting1=appsetting1value,appsetting2=appsetting2value

Aby wie, co może być innych konfiguracji zmienione, użyj polecenia **konfiguracji azure aplikacji sieci Web set -h** .

### <a name="change-the-state-of-an-existing-web-app"></a>Zmienianie stanu istniejącej aplikacji sieci Web ###

#### <a name="restart-a-web-app"></a>Uruchom ponownie aplikację sieci web ####

Aby ponownie uruchomić aplikację sieci web, podaj nazwę i zasobów grupy aplikacji sieci web.

    azure webapp restart --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a>Zatrzymywanie aplikacji sieci web ####

Aby zatrzymać aplikację sieci web, podaj nazwę i zasobów grupy aplikacji sieci web.

    azure webapp stop --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a>Uruchamianie aplikacji sieci web ####

Aby uruchomić aplikację sieci web, podaj nazwę i zasobów grupy aplikacji sieci web.

    azure webapp start --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a>Zarządzanie profilami publikowania aplikacji sieci Web ###

Każdej aplikacji sieci web ma publikowania profil, który może służyć do publikowania aplikacji.

#### <a name="get-publishing-profile"></a>Uzyskiwanie publikowania profilu ####

Aby uzyskać Publikowanie profilu dla aplikacji sieci web, należy użyć:

    azure webapp publishingprofile --name ContosoWebApp --resource-group ContosoAzureResourceGroup

To polecenie zwraca publikowania profilu nazwę użytkownika i hasło w wierszu polecenia.

### <a name="manage-web-app-hostnames"></a>Zarządzanie nazwy hostów aplikacji sieci Web ###

Aby zarządzać powiązań hostname dla aplikacji sieci web, użyj polecenia **nazwy hostów konfiguracji azure aplikacji sieci Web**  

#### <a name="list-hostname-bindings"></a>Lista powiązań nazwa hosta ####

Aby uzyskać bieżącą powiązań hostname dla aplikacji sieci web, należy użyć:

    azure webapp config hostnames list --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="add-hostname-bindings"></a>Dodawanie powiązań nazwa hosta ####

Aby dodać powiązań hostname do aplikacji sieci web, należy użyć:

    azure webapp config hostnames add --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

#### <a name="delete-hostname-bindings"></a>Usuwanie powiązań nazwa hosta ####

Aby usunąć powiązań hostname, należy użyć:

    azure webapp config hostnames delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

### <a name="next-steps"></a>Następne kroki ###
- Aby uzyskać informacje o Azure polecenie Menedżer zasobów pomocy technicznej, zobacz [Zarządzanie Azure zasobów i grup zasobów za pomocą interfejsu wiersza polecenia Azure.](../xplat-cli-azure-resource-manager.md)
- Aby uzyskać informacje o zarządzaniu aplikacji usługi przy użyciu programu PowerShell, zobacz [Using Azure Resource Manager-Based programu PowerShell do zarządzania aplikacjami sieci Web Azure.](app-service-web-app-azure-resource-manager-powershell.md)
