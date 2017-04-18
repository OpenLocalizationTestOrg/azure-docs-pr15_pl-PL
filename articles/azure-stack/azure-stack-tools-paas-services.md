<properties
    pageTitle="Narzędzia i PaaS usługi Azure stosu | Microsoft Azure"
    description="Dowiedz się, jak rozpocząć pracę z usługami PaaS w stos Azure."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="tools-for-azure-stack"></a>Narzędzia stosu Azure

Możesz pobrać narzędzia opisane poniżej. Jeśli chcesz otrzymywać powiadomienia o nowych usług i narzędzi wykonaj #AzureStack Twitter.

## <a name="template-tools"></a>Narzędzia do szablonu

### <a name="azure-stack-github-templates"></a>Szablony Github stos Azure
Eksplorowanie rosnących zbiór [Szablonów GitHub stos Azure](https://github.com/Azure/AzureStack-QuickStart-Templates). Podobnie jak [Azure](https://github.com/Azure/azure-quickstart-templates)tych szablonów "Szybkie uruchamianie" Menedżera zasobów Azure mają na celu rozpoczęcie pracy z prostych w blokach konstrukcyjnych i przykłady gotowe do wdrożenia na Microsoft Azure stos Technical Preview dowód z koncepcja środowisko. Obejmuje pierwszej strony obciążenie pracą przykłady [ad-non-ha](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/ad-non-ha), [sql-2014-non-ha](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sql-2014-non-ha), [sharepoint-2013-non-ha](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sharepoint-2013-non-ha), jak również w kilku prostych szablonów 101 [101-proste — systemu windows — maszyn wirtualnych](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-simple-windows-vm).


### <a name="marketplace-item-packaging-tool-and-sample"></a>Narzędzie opakowań elementu Marketplace i próbki
[Pobieranie i użyj narzędzia opakowań](http://www.aka.ms/azurestackmarketplaceitem) utworzyć własne szablony niestandardowe dodać do witryny marketplace stos Azure marketplace elementów. Instrukcje dotyczące tworzenia elementu marketplace i udostępnić go z dzierżawami można znaleźć w [elemencie tworzenie Marketplace](azure-stack-create-and-publish-marketplace-item.md).

## <a name="developer-tools"></a>Narzędzi dla deweloperów


### <a name="use-visual-studio-and-azure-stack-tp2-on-the-mas-con01-virtual-machine"></a>Korzystanie z programu Visual Studio i TP2 stos Azure na komputerze wirtualnych MAS CON01
Jeśli chcesz pracować z szablonami stos Azure za pomocą programu Visual Studio na konsoli maszyn wirtualnych, należy zainstalować poprawne wersje narzędzia potrzebne. Poniższa procedura umożliwia instalowanie obsługiwanych wersji dla TP2.

1. Podłączanie pulpitu zdalnego umożliwia logowanie się do komputera wirtualnych MAS CON01 przy użyciu poświadczeń azurestack\azurestackadmin.
2. Instalowanie i otwórz Instalatora platformy sieci Web.
3. Znajdowanie i instalowanie **programu Visual Studio 2015 społeczności z Microsoft Azure SDK - 2.9.5**.
4. Korzystając z funkcji **Dodaj lub usuń programy**, odinstaluj **Microsoft Azure programu PowerShell (marzec)** , która jest instalowana jako część zestawu SDK.
5. Otwórz Instalatora platformy sieci Web.
6. Znajdowanie i instalowanie **Programu Microsoft Azure programu PowerShell - Azure stos Technical Preview 2**. 
7. Uruchom ponownie komputer wirtualnych MAS CON01.
8. Podłączanie pulpitu zdalnego umożliwia logowanie się do komputera wirtualnych MAS CON01 przy użyciu poświadczeń azurestack\azurestackadmin.
9. Otwórz program Visual Studio, a następnie sprawdź, czy można nawiązać środowiska stosem Azure, pobrać szablony i tak dalej. 

### <a name="azure-powershell-sdk"></a>Azure SDK programu PowerShell
Azure PowerShell to modułu, który zawiera polecenia cmdlet Azure i stos Azure przy użyciu programu Windows PowerShell do zarządzania. Można użyć poleceń cmdlet tworzyć, testowanie, wdrażanie i zarządzanie rozwiązaniach i usługach dostarczona platformy Azure stosu.
[Pobierz zestaw SDK programu PowerShell Azure](http://aka.ms/azStackPsh) i [Instalowanie programu PowerShell](azure-stack-connect-powershell.md).

### <a name="azure-cross-platform-command-line-interfaces"></a>Azure krzyżowe interfejsów wiersza polecenia platformy
Szybko zainstalować interfejsu wiersza polecenia Azure (polecenie Azure) do użycia zestaw open source oparty na powłoce poleceń do tworzenia i zarządzania zasobami Microsoft Azure stosu.

[Pobierz interfejsu wiersza polecenia systemu Windows](http://aka.ms/azstack-windows-cli)

[Pobierz Mac interfejsu wiersza polecenia](http://aka.ms/azstack-linux-cli)

[Pobierz polecenie Linux](http://aka.ms/azstack-mac-cli)

>[AZURE.NOTE]
>
> + Jeśli na komputerze Mac lub Linux, możesz również wyświetlić interfejs wiersza polecenia przy użyciu polecenia`npm install -g azure-cli@0.9.11`</br>
> + Jeśli w razie wystąpienia problemów sprawdzania poprawności certyfikatów, uruchom polecenie`set NODE_TLS_REJECT_UNAUTHORIZED=0`
