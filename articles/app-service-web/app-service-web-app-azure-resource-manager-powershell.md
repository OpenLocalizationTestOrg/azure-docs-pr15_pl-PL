<properties
    pageTitle="Polecenia Azure programu PowerShell, które są oparte na Menedżera zasobów dla aplikacji sieci Web Azure | Microsoft Azure"
    description="Dowiedz się, jak zarządzać swoimi aplikacjami sieci Web Azure za pomocą nowe polecenia programu PowerShell oparte na Azure Menedżera zasobów."
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

# <a name="using-azure-resource-manager-based-powershell-to-manage-azure-web-apps"></a>Za pomocą Azure zasobów oparte na Menedżera Zarządzaj aplikacjami Azure Web#

> [AZURE.SELECTOR]
- [Polecenie Azure](app-service-web-app-azure-resource-manager-xplat-cli.md)
- [Azure programu PowerShell](app-service-web-app-azure-resource-manager-powershell.md)

Przy użyciu programu PowerShell usługi Microsoft Azure wersji 1.0.0 zostały dodane nowe polecenia, które powodują użytkownikowi możliwość używania polecenia programu PowerShell oparte na Azure Menedżera zasobów do zarządzania aplikacjami sieci Web.

Aby uzyskać informacje o zarządzaniu grup zasobów, zobacz [Używanie Azure przy użyciu Menedżera zasobów Azure](../powershell-azure-resource-manager.md). 

Aby poznać pełną listę parametrów i opcji polecenia cmdlet programu PowerShell, zobacz [Pełne informacje dotyczące poleceń Cmdlet poleceń cmdlet programu PowerShell na podstawie Menedżera zasobów Azure aplikacji sieci Web](https://msdn.microsoft.com/library/mt619237.aspx)

## <a name="managing-app-service-plans"></a>Zarządzanie usługą aplikacji plany ##

### <a name="create-an-app-service-plan"></a>Tworzenie planu aplikacji usługi ###
Aby utworzyć plan usług aplikacji, należy użyć polecenia cmdlet **AzureRmAppServicePlan nowy** .

Poniżej przedstawiono opisy różnych parametrów:

-   **Nazwa**: Nazwa aplikacji plan usług.
-   **Lokalizacja**: usługi plan lokalizacji.
-   **ResourceGroupName**: Grupa zasobów, która zawiera plan usług nowo utworzonej aplikacji.
-   **Warstwy**: żądany poziom cennik (domyślny jest bezpłatna, inne opcje są udostępnione, Basic, Standard i Premium).
-   **WorkerSize**: rozmiar pracowników (wartość domyślna to małe, jeśli określono parametr Warstwa jako Basic, Standard lub Premium. Inne opcje to: średnich i dużych.)
-   **NumberofWorkers**: liczba pracowników w aplikacji usługi plan (wartością domyślną jest 1). 

Przykład, aby użyć tego polecenia cmdlet:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -Tier Premium -WorkerSize Large -NumberofWorkers 10

### <a name="create-an-app-service-plan-in-an-app-service-environment"></a>Tworzenie planu usługi aplikacji w środowisku usługi aplikacji ###
Aby utworzyć plan usług aplikacji w środowisku usługi aplikacji, za pomocą tego samego polecenia **Nowy AzureRmAppServicePlan** polecenia dodatkowe parametry Określ nazwę ASE i nazwa grupy zasobów w ASE.

Przykład, aby użyć tego polecenia cmdlet:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -AseName constosoASE -AseResourceGroupName contosoASERG -Tier Premium -WorkerSize Large -NumberofWorkers 10

Aby dowiedzieć się więcej o środowisku usługi aplikacji, zaznacz pozycję [Wprowadzenie do środowiska usługi aplikacji](app-service-app-service-environment-intro.md)

### <a name="list-existing-app-service-plans"></a>Plany usługi aplikacji z istniejącej listy ###

Aby wyświetlić listę istniejących planów usługi aplikacji, należy użyć polecenia cmdlet **Get-AzureRmAppServicePlan** .

Aby wyświetlić listę wszystkich planów usługi aplikacji w obszarze subskrypcji, należy użyć: 

    Get-AzureRmAppServicePlan

Aby wyświetlić listę wszystkich planów usługi aplikacji w obszarze danej grupy zasobów, należy użyć:

    Get-AzureRmAppServicePlan -ResourceGroupname ContosoAzureResourceGroup

Aby uzyskać plan usług konkretnej aplikacji, należy użyć:

    Get-AzureRmAppServicePlan -Name ContosoAppServicePlan


### <a name="configure-an-existing-app-service-plan"></a>Konfigurowanie istniejącej aplikacji usługi Plan ###

Aby zmienić ustawienia dla istniejącego planu usługi aplikacji, należy użyć polecenia cmdlet **Set-AzureRmAppServicePlan** . Możesz zmienić warstwie, pracownik rozmiar i liczbę pracowników 

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard -WorkerSize Medium -NumberofWorkers 9

#### <a name="scaling-an-app-service-plan"></a>Skalowanie Plan usług aplikacji ####

Aby przeskalować istniejącej aplikacji usługi Plan, należy użyć:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -NumberofWorkers 9

#### <a name="changing-the-worker-size-of-an-app-service-plan"></a>Zmienianie rozmiaru pracownik Plan aplikacji usługi ####

Aby zmienić rozmiar pracowników w istniejącej aplikacji usługi Planowanie, należy użyć:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -WorkerSize Medium

#### <a name="changing-the-tier-of-an-app-service-plan"></a>Zmienianie poziomu planu aplikacji usługi ####

Aby zmienić poziom planu istniejącej aplikacji usługi, należy użyć:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard

### <a name="delete-an-existing-app-service-plan"></a>Usuwanie istniejącej aplikacji usługi Plan ###

Aby usunąć istniejący plan usług aplikacji, wszystkie aplikacje web przydzielonych konieczne można przenosić ani usuwać najpierw. Następnie przy użyciu polecenia cmdlet **Usuń AzureRmAppServicePlan** , możesz usunąć plan usług aplikacji.

    Remove-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup

## <a name="managing-app-service-web-apps"></a>Zarządzanie aplikacjami sieci Web aplikacji usługi ##

### <a name="create-a-web-app"></a>Tworzenie aplikacji sieci Web ###

Aby utworzyć aplikację sieci web, należy użyć polecenia cmdlet **AzureRmWebApp nowy** .

Poniżej przedstawiono opisy różnych parametrów:

- **Nazwa**: Nazwa aplikacji sieci web.
- **AppServicePlan**: Nazwa plan usług używany do obsługi aplikacji sieci web.
- **ResourceGroupName**: Grupa zasobów, która obsługuje plan usług aplikacji.
- **Lokalizacja**: Lokalizacja aplikacji sieci web.

Przykład, aby użyć tego polecenia cmdlet:

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"

### <a name="create-a-web-app-in-an-app-service-environment"></a>Tworzenie aplikacji sieci Web w środowisku usługi aplikacji ###

Aby utworzyć aplikację sieci web w środowisku aplikacji (ASE). Za pomocą tego samego polecenia **Nowy AzureRmWebApp** dodatkowe parametry Określ nazwę ASE i nazwa grupy zasobów, której należy ASE.

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"  -ASEName ContosoASEName -ASEResourceGroupName ContosoASEResourceGroupName

Aby dowiedzieć się więcej o środowisku usługi aplikacji, zaznacz pozycję [Wprowadzenie do środowiska usługi aplikacji](app-service-app-service-environment-intro.md)

### <a name="delete-an-existing-web-app"></a>Usuwanie istniejącej aplikacji sieci Web ###

Aby usunąć istniejącej aplikacji sieci web można użyć polecenia cmdlet **Usuń AzureRmWebApp** , musisz podać nazwę aplikacji sieci web i nazwa grupy zasobów.

    Remove-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup


### <a name="list-existing-web-apps"></a>W aplikacjach sieci Web z istniejącej listy ###

Aby wyświetlić listę istniejących aplikacji sieci web, należy użyć polecenia cmdlet **Get-AzureRmWebApp** .

Aby wyświetlić listę wszystkich aplikacji sieci web w obszarze subskrypcji, należy użyć:

    Get-AzureRmWebApp

Aby wyświetlić listę wszystkich aplikacji sieci web w obszarze danej grupy zasobów, należy użyć:

    Get-AzureRmWebApp -ResourceGroupname ContosoAzureResourceGroup

Aby pobrać aplikację sieci web określonych, należy użyć:

    Get-AzureRmWebApp -Name ContosoWebApp

### <a name="configure-an-existing-web-app"></a>Konfigurowanie istniejącej aplikacji sieci Web ###

Aby zmienić ustawienia i konfiguracji istniejącej aplikacji sieci web, użyj polecenia cmdlet **Set-AzureRmWebApp** . Aby uzyskać pełną listę parametrów Sprawdź [łącza odwołania polecenia Cmdlet](https://msdn.microsoft.com/library/mt652487.aspx)

Przykład (1): Użyj tego polecenia cmdlet, aby zmienić parametry połączenia

    $connectionstrings = @{ ContosoConn1 = @{ Type = “MySql”; Value = “MySqlConn”}; ContosoConn2 = @{ Type = “SQLAzure”; Value = “SQLAzureConn”} }
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -ConnectionStrings $connectionstrings

Przykład (2): Dodawanie lub zmienianie ustawień aplikacji

    $appsettings = @{appsetting1 = "appsetting1value"; appsetting2 = "appsetting2value"}
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -AppSettings $appsettings


Przykład (3): Ustaw aplikacji sieci web w trybie 64-bitowej

    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -Use32BitWorkerProcess $False

### <a name="change-the-state-of-an-existing-web-app"></a>Zmienianie stanu istniejącej aplikacji sieci Web ###

#### <a name="restart-a-web-app"></a>Uruchom ponownie aplikację sieci web ####

Aby ponownie uruchomić aplikację sieci web, podaj nazwę i zasobów grupy aplikacji sieci web.

    Restart-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a>Zatrzymywanie aplikacji sieci web ####

Aby zatrzymać aplikację sieci web, podaj nazwę i zasobów grupy aplikacji sieci web.

    Stop-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a>Uruchamianie aplikacji sieci web ####

Aby uruchomić aplikację sieci web, podaj nazwę i zasobów grupy aplikacji sieci web.

    Start-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a>Zarządzanie profilami publikowania aplikacji sieci Web ###

Każdej aplikacji sieci web ma publikowania profil, który może służyć do publikowania aplikacji, można wykonać kilka czynności na publikowania profile.

#### <a name="get-publishing-profile"></a>Uzyskiwanie publikowania profilu ####

Aby uzyskać Publikowanie profilu dla aplikacji sieci web, należy użyć:

    Get-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -OutputFile .\publishingprofile.txt

To polecenie zwraca profilu publikowania do wiersza polecenia jako również wyjściowe profilu publikowania do pliku tekstowego.

#### <a name="reset-publishing-profile"></a>Resetowanie publikowania profilu ####

Aby zresetować oba publikowania hasła dla sieci web i FTP wdrożyć dla aplikacji sieci web, użyj:

    Reset-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-certificates"></a>Zarządzanie certyfikaty aplikacji sieci Web ###

Aby dowiedzieć się o zarządzaniu certyfikaty aplikacji sieci web, zobacz [powiązanie certyfikatów SSL przy użyciu programu PowerShell](app-service-web-app-powershell-ssl-binding.md)


### <a name="next-steps"></a>Następne kroki ###
- Aby uzyskać informacje o pomocy technicznej Azure Menedżera zasobów programu PowerShell, zobacz [za pomocą programu Azure przy użyciu Menedżera zasobów Azure.](../powershell-azure-resource-manager.md)
- Aby uzyskać informacje o aplikacji usługi środowisk, zobacz [Wprowadzenie do środowiska usługi aplikacji.](app-service-app-service-environment-intro.md)
- Aby uzyskać informacje o zarządzaniu certyfikatów SSL usługi aplikacji przy użyciu programu PowerShell, zobacz [powiązanie certyfikatów SSL przy użyciu programu PowerShell.](app-service-web-app-powershell-ssl-binding.md)
- Aby poznać pełną listę poleceń cmdlet środowiska PowerShell oparte na Azure Menedżera zasobów dla aplikacji sieci Web Azure, zobacz [informacje dotyczące poleceń Cmdlet Azure poleceń cmdlet programu PowerShell Menedżera zasobów Web Apps Azure.](https://msdn.microsoft.com/library/mt619237.aspx)
- - Aby dowiedzieć się o zarządzaniu usługi aplikacji przy użyciu interfejsu wiersza polecenia, zobacz [Using Azure Resource Manager-Based XPlat interfejsu wiersza polecenia dla aplikacji sieci Web Azure](app-service-web-app-azure-resource-manager-xplat-cli.md)
