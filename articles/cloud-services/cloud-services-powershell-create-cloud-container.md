<properties
   pageTitle="Tworzenie kontenera usługi cloud przy użyciu programu PowerShell | Microsoft Azure"
   description="W tym artykule wyjaśniono, jak utworzyć kontenerze usługi cloud przy użyciu programu PowerShell. Kontener obsługuje role sieci web i pracownika."
   services="cloud-services"
   documentationCenter=".net"
   authors="cawaMS"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="powershell"
   ms.workload="na"
   ms.date="07/29/2016"
   ms.author="cawa"/>

# <a name="use-an-azure-powershell-command-to-create-an-empty-cloud-service-container"></a>Tworzenie kontenera usługi cloud pustego za pomocą polecenia programu Azure PowerShell
W tym artykule wyjaśniono, jak szybko utworzyć kontenera usług w chmurze przy użyciu poleceń cmdlet środowiska PowerShell Azure. Wykonaj poniższe czynności:

1. Zainstaluj polecenia cmdlet programu PowerShell Azure Microsoft ze strony [pobierania Azure programu PowerShell](http://aka.ms/webpi-azps) .
2. Otwórz okno wiersza polecenia programu PowerShell.
3. Zaloguj się za pomocą [AzureAccount Dodaj](https://msdn.microsoft.com/library/dn495128.aspx) .

    > [AZURE.NOTE] Dalsze instrukcje dotyczące instalowania polecenia cmdlet programu PowerShell Azure i nawiązywanie połączeń z subskrypcji usługi Azure można znaleźć w [sposób instalowania i konfigurowania środowiska PowerShell Azure](../powershell-install-configure.md).

4. Tworzenie pustego chmura Azure kontenera usługi, należy użyć polecenia cmdlet **AzureService nowy** .

    ```
    New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
    New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
```

5. Należy wykonać w tym przykładzie do wywołania polecenia cmdlet:
```
New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
```

Aby uzyskać więcej informacji na temat tworzenia usług w chmurze Azure Uruchom:
```
Get-help New-AzureService
```

### <a name="next-steps"></a>Następne kroki

 * Aby zarządzać wdrażanie usługi w chmurze, skorzystaj z polecenia [Get-AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx), [Usuń AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx)i [AzureService zestawu](https://msdn.microsoft.com/library/azure/dn495242.aspx) . Aby uzyskać więcej informacji może również dotyczyć [sposobu konfigurowania usług w chmurze](cloud-services-how-to-configure.md) .

 * Aby opublikować projekt usługi cloud Azure, zapoznaj się z przykładowy kod **PublishCloudService.ps1** z [ciągłym dostawy dla usługi w chmurze platformy Azure](cloud-services-dotnet-continuous-delivery.md).
