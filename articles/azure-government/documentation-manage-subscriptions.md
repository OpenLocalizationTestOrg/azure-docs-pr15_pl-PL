<properties
    pageTitle="Usługi Azure dla instytucji rządowych | Microsoft Azure"
    description="Informacje o zarządzaniu subskrypcji w Azure dla instytucji rządowych"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="zakramer"
    manager="liki"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/21/2016"
    ms.author="zakramer" />


#  <a name="managing-and-connecting-to-your-subscription-in-azure-government"></a>Zarządzanie i nawiązywanie połączeń z subskrypcji w Azure dla instytucji rządowych

Rząd Azure ma unikatowe adresy URL i punktów końcowych do zarządzania środowiska. Należy korzystać z połączenia prawo do zarządzania środowiska za pośrednictwem portalu lub programu PowerShell. Po nawiązaniu połączenia dla środowiska dla instytucji rządowych Azure normalnej pracy do zarządzania usługą wtedy, gdy składnik został wdrożony.

## <a name="connecting-via-the-portal"></a>Łączenie za pośrednictwem portalu
Portalu jest głównym sposobem większość osoby łączące się Azure dla instytucji rządowych.  Aby połączyć, przejdź do portalu pod adresem [https://portal.azure.us](https://portal.azure.us).  Starszych wersji Azure portal są dostępne za pośrednictwem [https://manage.windowsazure.us](https://manage.windowsazure.us).

Subskrypcje można utworzyć dla Twojego konta przez nawiązanie połączenia z [https://account.windowsazure.us](https://account.windowsazure.us).

## <a name="connecting-via-powershell"></a>Nawiązywanie połączenia przy użyciu programu PowerShell

Czy używasz Azure programu PowerShell do zarządzania subskrypcją dużych za pośrednictwem skrypt lub dostępu do funkcji, które nie są obecnie dostępne w portalu Azure, które należy nawiązać Azure dla instytucji rządowych zamiast Azure publicznej.  PowerShell użycie w publicznej Azure jest głównie takie same.  Różnice w Azure dla instytucji rządowych są następujące:

+ Nawiązywanie połączenia z kontem
+ Nazwy regionów

>[AZURE.NOTE] Jeśli nie zastosowano jeszcze programu PowerShell, zapoznaj się z [Wprowadzenie do programu PowerShell Azure](../powershell-install-configure.md).

Po uruchomieniu programu PowerShell, musisz określić Azure programu PowerShell, aby nawiązać połączenie dla instytucji rządowych Azure określając parametr środowiska.  Parametr gwarantuje, że programu PowerShell łączy się poprawne punktów końcowych.  Kolekcja punkty końcowe zależy po połączeniu zalogowany do konta.  Różne interfejsy API wymagają różnych wersji przełącznika środowiska:

Typ połączenia | Polecenie
---|----
[Zarządzanie usługą](https://msdn.microsoft.com/library/dn708504.aspx) poleceń | `Add-AzureAccount -Environment AzureUSGovernment`
Polecenia [Zarządzanie zasobami](https://msdn.microsoft.com/library/mt125356.aspx) | `Add-AzureRmAccount -EnvironmentName AzureUSGovernment`
Polecenia [Usługi Azure Active Directory](https://msdn.microsoft.com/library/azure/jj151815.aspx) | `Connect-MsolService -AzureEnvironment UsGovernment`
[Azure Active Directory polecenia w wersji 2](https://msdn.microsoft.com/library/azure/mt757189.aspx) | `Connect-AzureAD -AzureEnvironmentName AzureUSGovernment`

Może też podczas nawiązywania połączenia konto miejsca do magazynowania przy użyciu nowego AzureStorageContext należy użyć przełącznika środowiska i określ AzureUSGovernment.

### <a name="determining-region"></a>Określanie obszaru

Po nawiązaniu połączenia istnieje jedna różnica dodatkowe — regionów używanych kierowania usługi.  Co chmura Azure ma różnych regionów.  Ty możesz je wyświetlać wymienione na stronie Dostępność usługi.  Można używać w parametrze Lokalizacja obszaru polecenia.

Istnieje jeden efektywnej.  Regiony dla instytucji rządowych Azure należy są sformatowane inaczej niż ich typowych nazw:

Typowe nazwy | Polecenie
---|----
Virginia Gov Stanów Zjednoczonych | USGov Virginia
Stany Zjednoczone Gov Iowa | USGov Iowa

>[AZURE.NOTE] Brakuje miejsca między USA oraz Gov przy użyciu parametru lokalizacji.

Jeśli kiedykolwiek chcesz sprawdzić poprawność regionów dostępne w Azure dla instytucji rządowych, można uruchom następujące polecenia i drukowanie bieżącej listy:

    Get-AzureLocation

Jeśli zastanawiasz się, jak dostępnych środowisk przez Azure, można uruchomić:

    Get-AzureEnvironment

## <a name="next-steps"></a>Następne kroki

Jeśli użytkownik chce uzyskać więcej informacji, zapoznaj się z:

+ [Dokumenty programu PowerShell na GitHub](https://github.com/Azure/azure-powershell)
+ [Instrukcje krok po kroku dotyczące nawiązywania połączenia do zarządzania zasobami](https://blogs.msdn.microsoft.com/azuregov/2015/10/08/configuring-arm-on-azure-gc/)
+ [Azure dokumenty programu PowerShell w witrynie MSDN](https://msdn.microsoft.com/library/mt619274.aspx)

Aby uzyskać dodatkowe informacje i aktualizacje subskrybowanie [Microsoft Azure dla instytucji rządowych blogu] (https://blogs.msdn.microsoft.com/azuregov/)
