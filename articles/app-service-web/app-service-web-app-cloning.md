<properties
    pageTitle="Klonowanie aplikacji sieci Web przy użyciu programu PowerShell"
    description="Dowiedz się, jak klonowanie aplikacji sieci Web do nowej aplikacji sieci Web przy użyciu programu PowerShell."
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
    ms.date="01/13/2016"
    ms.author="ahmedelnably"/>

# <a name="azure-app-service-app-cloning-using-powershell"></a>Aplikacja usługi Azure aplikacji klonowanie, przy użyciu programu PowerShell#

W wersji programu Microsoft Azure programu PowerShell wersji 1.1.0 nowa opcja dodano AzureRMWebApp nowy, który chcesz umożliwiania użytkownika klonowanie istniejącej aplikacji sieci Web do nowo utworzonej aplikacji w innym regionie lub w tym samym regionie. Umożliwi to klientom wdrożyć liczbę aplikacji w różnych regionów, szybko i łatwo.

Klonowanie aplikacji jest obecnie obsługiwany tylko dla planów usług premium warstwy aplikacji. Nowa funkcja korzysta te same ograniczenia jako funkcja Kopia zapasowa aplikacji sieci Web, zobacz artykuł [kopię zapasową aplikacji sieci web w usłudze Azure aplikacji](web-sites-backup.md).

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

Aby informacje o korzystaniu z Menedżera zasobów Azure podstawą Azure programu PowerShell polecenia cmdlet, aby zarządzać swoimi aplikacjami sieci Web zaznacz pozycję [Menedżer zasobów Azure podstawie polecenia programu PowerShell dla aplikacji sieci Web Azure](app-service-web-app-azure-resource-manager-powershell.md)

## <a name="cloning-an-existing-app"></a>Klonowanie istniejącej aplikacji ##

Scenariusz: Istniejącej aplikacji sieci web w regionie Południe centralnej nam, użytkownik chce klonowanie zawartości do nowej aplikacji sieci web w regionie Północna centralnej US. Można to osiągnąć przy użyciu wersji Menedżera zasobów Azure polecenia cmdlet programu PowerShell do utworzenia nowej aplikacji sieci web z opcją - SourceWebApp.

Informacje o nazwy grupy zasobów, która zawiera aplikacji sieci web źródło, możemy Użyj następującego polecenia programu PowerShell Aby uzyskać informacje o źródle aplikacji sieci web (w tym przypadku o nazwie aplikacji sieci Web źródło):

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

Aby utworzyć nowy Plan aplikacji usługi, możemy użyć polecenia Nowy AzureRmAppServicePlan, tak jak w poniższym przykładzie

    New-AzureRmAppServicePlan -Location "South Central US" -ResourceGroupName DestinationAzureResourceGroup -Name NewAppServicePlan -Tier Premium

Za pomocą polecenia Nowy AzureRmWebApp było tworzenie nowej aplikacji sieci web w obszarze Północna centralnej nam i powiązanie jej istniejącego poziomu premium Plan usługi aplikacji, Ponadto firma Microsoft można użyć tej samej grupy zasobów jako źródła aplikacji sieci web lub zdefiniować nowej grupy zasobów, pokazano poniżej:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp

Na klonowanie istniejącej aplikacji sieci web w tym wdrażania wszystkich skojarzonych z nimi gniazda, użytkownik musi być parametr IncludeSourceWebAppSlots następującego polecenia programu PowerShell zaprezentowano Użyj tego parametru przy użyciu polecenia Nowy AzureRmWebApp:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots $true

Aby klonowanie istniejącej aplikacji sieci web w ramach tego samego regionu, użytkownik musi utworzyć nową grupę zasobów i nową usługę aplikacji planowanie w tym samym regionie, a następnie klonowanie aplikacji sieci web przy użyciu następującego polecenia programu PowerShell

    $destapp = New-AzureRmWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcap

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Klonowanie istniejącej aplikacji w środowisku usługi aplikacji ##

Scenariusz: Istniejącej aplikacji sieci web w regionie Południe centralnej nam, użytkownik chce klonowanie zawartości do nowej aplikacji sieci web do istniejącej aplikacji usługi środowiska (ASE).

Informacje o nazwy grupy zasobów, która zawiera aplikacji sieci web źródła, możemy Użyj następującego polecenia programu PowerShell Aby uzyskać informacje o źródle aplikacji sieci web (w tym przypadku o nazwie aplikacji sieci Web źródła):

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

Informacje o nazwę ASE, a nazwa grupy zasobów, której należy ASE, użytkownik umożliwia tworzenie nowej aplikacji sieci web w istniejącej ASE polecenie Nowy AzureRmWebApp, pokazano poniżej:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp

Parametr lokalizacji jest wymagany ze względu na powód starsze, ale w przypadku tworzenia aplikacji w ASE będą ignorowane. 

## <a name="cloning-an-existing-app-slot"></a>Klonowanie istniejących przedział aplikacji ##

Scenariusz: Użytkownik chce klonowanie istniejących przedział aplikacji sieci Web do albo nowej aplikacji sieci Web lub nowy przedział aplikacji sieci Web. Nowej aplikacji sieci Web może być w tym samym regionie jako oryginalny przedział aplikacji sieci Web lub w innym regionie.

Informacje o nazwy grupy zasobów, która zawiera aplikacji sieci web źródła, możemy Użyj następującego polecenia programu PowerShell Aby uzyskać informacje o przedział aplikacji sieci web źródła (w tym przypadku o nazwie webappslot źródła) z aplikacji Web App aplikacji sieci Web źródła:

    $srcappslot = Get-AzureRmWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-webapp -Slot source-webappslot

Poniżej pokazano, tworzenia klonowanie źródła aplikacji sieci web do nowej aplikacji sieci web:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot

## <a name="configuring-traffic-manager-while-cloning-a-app"></a>Konfigurowanie Menedżera ruch podczas klonowanie aplikacji ##

Tworzenie aplikacji sieci web w przypadku i konfigurowanie Menedżer ruchu Azure, aby skierować ruch do wszystkich aplikacji sieci web, jest to n scenariusz ważne, aby mieć pewność, że aplikacje klientów są bardzo dostępne, gdy klonowanie istniejącej aplikacji sieci web istnieje możliwość łączenie obu aplikacji sieci web do nowego profilu Menedżer ruchu lub istniejącej — należy zauważyć, że tylko Menedżer zasobów Azure wersji Menedżera ruch jest obsługiwana.

### <a name="creating-a-new-traffic-manager-profile-while-cloning-a-app"></a>Tworzenie nowego profilu Menedżer ruchu podczas klonowanie aplikacji ###

Scenariusz: Użytkownik chce klonowanie aplikacji sieci web do innego regionu, podczas konfigurowania profilu Menedżera ruch Menedżera zasobów Azure, zawierające zarówno aplikacji sieci web. Poniższy ilustruje tworzenie klonowanie źródła aplikacji sieci web do nowej aplikacji sieci web podczas konfigurowania nowego profilu Menedżer ruchu:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile

### <a name="adding-new-cloned-web-app-to-an-existing-traffic-manager-profile"></a>Dodawanie nowych sklonowany aplikacji sieci Web do istniejącego profilu Menedżer ruchu ###

Scenariusz: Użytkownik ma już Menedżera zasobów Azure ruch Menedżer profil, który chciałby dodać zarówno aplikacje sieci web jako punkty końcowe. W tym celu najpierw należy zebrania istniejący identyfikator profilu Menedżer ruchu, administrator powinien identyfikator subskrypcji, nazwa grupy zasobów i istniejącej nazwy profilu Menedżer ruchu.

    $TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"

Po identyfikator Menedżer ruchu, poniżej przedstawia, tworzenia klonowanie źródła aplikacji sieci web do nowej aplikacji sieci web podczas dodawania ich do istniejącego profilu Menedżer ruchu:

    $destapp = New-AzureRmWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID

## <a name="current-restrictions"></a>Bieżących ograniczeń ##

Ta funkcja jest obecnie w podglądzie, pracujemy nad dodawać nowe funkcje w czasie, poniżej są znane ograniczenia w bieżącej wersji klonowanie aplikacji:

- Automatyczne ustawień skali nie są sklonowany.
- Harmonogram wykonywania kopii zapasowych ustawienia nie są sklonowany.
- Ustawienia VNET nie są sklonowany.
- Aplikacja wnioski nie są automatycznie skonfigurowane w aplikacji sieci web miejsca docelowego
- Łatwe ustawienia uwierzytelniania nie są sklonowany.
- Rozszerzenie kudu nie są sklonowany.
- Porada zasady nie są sklonowany.
- Zawartość bazy danych nie są sklonowany


### <a name="references"></a>Odwołania ###
- [Menedżer zasobów Azure podstawie polecenia programu PowerShell dla aplikacji sieci Web Azure](app-service-web-app-azure-resource-manager-powershell.md)
- [Za pomocą Azure Portal klonowanie aplikacji sieci Web](app-service-web-app-cloning-portal.md)
- [Wykonywanie kopii zapasowej aplikacji sieci web w usłudze Azure aplikacji](web-sites-backup.md)
- [Azure Menedżera zasobów pomocy technicznej do podglądu Menedżer ruchu Azure](../../articles/traffic-manager/traffic-manager-powershell-arm.md)
- [Wprowadzenie do środowiska aplikacji usługi](app-service-app-service-environment-intro.md)
- [Przy użyciu programu PowerShell Azure przy użyciu Menedżera zasobów Azure](../powershell-azure-resource-manager.md)
