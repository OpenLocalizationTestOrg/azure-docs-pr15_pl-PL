<properties
    pageTitle="Rejestrowanie Azure klucza magazynu | Microsoft Azure"
    description="Użyj tego samouczka, ułatwiające rozpoczęcie pracy z magazynu klucza Azure rejestrowania."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/31/2016"
    ms.author="cabailey"/>

# <a name="azure-key-vault-logging"></a>Rejestrowanie Azure klucza magazynu #
Azure magazynu klucza jest dostępna w większości regionów. Aby uzyskać więcej informacji zobacz [Klucz magazynu ceny strony](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Wprowadzenie  
Po utworzeniu co najmniej jeden magazynami najważniejszych prawdopodobnie będzie monitorowanie sposobu i czasu z magazynami najważniejszych są używane i przez kogo. Możesz to zrobić przez włączenie rejestrowania dla magazynu klucz, który zapisuje informacje dostarczone konta Azure miejsca do magazynowania. Nowy kontener o nazwie **wniosków dzienniki auditevent** jest tworzone automatycznie dla Twojego konta określonego miejsca do magazynowania, a za pomocą tego samego konta miejsca do magazynowania dla zbierania dzienników wielu magazynami najważniejszych.

Możesz uzyskać co najwyżej dostęp do informacji rejestrowania, 10 minut po klucz magazynu operacji. W większości przypadków będą szybciej niż to.  Jest zarządzanie dzienniki na koncie miejsca do magazynowania:

- Zabezpieczanie dzienników ograniczając, kto może uzyskiwać do nich dostęp za pomocą metody kontroli dostępu Azure standardowy.
- Usuwanie dzienników, których już nie chcesz zachować na koncie miejsca do magazynowania.

Użyj tego samouczka, aby ułatwić wprowadzenie do magazynu klucza Azure rejestrowania, aby utworzyć konto miejsca do magazynowania, Włącz rejestrowanie i interpretować zbierane informacje o rejestrowaniu.  


>[AZURE.NOTE]  Ten samouczek nie zawiera instrukcje dotyczące sposobu tworzenia magazynami najważniejszych, klucze i hasła. Informacje zobacz [Rozpoczynanie pracy z magazynu klucza Azure](key-vault-get-started.md). Lub, aby interfejs wiersza polecenia i Platform, zobacz [tego samouczka równoważne](key-vault-manage-with-cli.md).
>
>Obecnie nie można skonfigurować Azure klucza magazynu w portalu Azure. Użyj zamiast tego następujące instrukcje programu PowerShell Azure.

Dzienniki, których zebrania można zwizualizować za pomocą analizy dziennika z zestawu zarządzania operacji. Aby uzyskać więcej informacji zobacz [rozwiązanie Azure klucza magazynu (wersja Preview) do analizy dziennika](../log-analytics/log-analytics-azure-key-vault.md).

Informacje o magazynu klucza Azure, zobacz [Co to jest Azure klucza magazynu?](key-vault-whatis.md)

## <a name="prerequisites"></a>Wymagania wstępne

Aby użyć tego samouczka, musisz mieć następujące czynności:

- Istniejące klucza magazynu, który był używany.  
- Azure programu PowerShell, **Minimalna wersja 1.0.1**. Aby zainstalować Azure programu PowerShell i skojarzyć subskrypcji Azure, zobacz, [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md). Jeśli zainstalowano już Azure programu PowerShell i nie znasz wersji, z poziomu konsoli programu PowerShell Azure, wpisz `(Get-Module azure -ListAvailable).Version`.  
- Magazyn wystarczających Azure dzienniki klucza magazynu.


## <a id="connect"></a>Nawiązywanie połączenia z subskrypcji ##

Rozpocznij sesję programu PowerShell Azure i zaloguj się do konta Azure za pomocą następującego polecenia:  

    Login-AzureRmAccount

W oknie podręcznym przeglądarki wprowadź Azure nazwa użytkownika i hasło. Azure programu PowerShell otrzymają wszystkie subskrypcje, które są skojarzone z tym kontem i domyślnie używa pierwszego.

Jeśli masz wiele subskrypcji, może być konieczne określanie określonej, który został użyty do utworzenia magazynu klucza usługi Azure. Wpisz poniższe czynności, aby wyświetlić subskrypcje dla Twojego konta:

    Get-AzureRmSubscription

Następnie aby określić subskrypcji, który jest skojarzony z usługi Magazyn klucza, który użytkownik będzie loguje, wpisz:

    Set-AzureRmContext -SubscriptionId <subscription ID>

Aby uzyskać więcej informacji o konfigurowaniu Azure programu PowerShell zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).


## <a id="storage"></a>Tworzenie nowego konta miejsca do magazynowania dla dzienników ##

Mimo że za pomocą istniejącego konta miejsca do magazynowania dla dzienników, utworzymy na nowe konto miejsca do magazynowania poświęcona dzienniki klucza magazynu. Dla wygody gdy mamy funkcja później będą przechowywane dane do zmiennej o nazwie **sa**.

Dodatkowe ułatwia zarządzanie również użyjemy tej samej grupy zasobów jako tabelę zawierającą naszych klucza magazynu. Na [wprowadzenie samouczka](key-vault-get-started.md)ta grupa zasobów o nazwie **ContosoResourceGroup** i będzie w dalszym ciągu używać tej lokalizacji wschodnioazjatycki. Podstaw te wartości do własnego odpowiednio:

    $sa = New-AzureRmStorageAccount -ResourceGroupName ContosoResourceGroup -Name ContosoKeyVaultLogs -Type Standard_LRS -Location 'East Asia'


>[AZURE.NOTE]  Jeśli zdecydujesz się na korzystanie z istniejącego konta miejsca do magazynowania, go jako swojego klucza magazynu należy użyć tej samej subskrypcji i może zawierać model wdrożenia Menedżera zasobów, zamiast modelu wdrożenia klasyczny.

## <a id="identify"></a>Identyfikowanie klucza magazynu dla dzienników ##

W naszych uzyskiwania samouczek wprowadzenie nazwę firmy klucza magazynu był **ContosoKeyVault**, będzie w dalszym ciągu użyć tej nazwy i przechowywać dane do zmiennej o nazwie **kv**:

    $kv = Get-AzureRmKeyVault -VaultName 'ContosoKeyVault'


## <a id="enable"></a>Włączanie rejestrowania ##

Aby włączyć rejestrowanie dla magazynu klucza, użyjemy cmdlet zestawu AzureRmDiagnosticSetting razem z zmienne utworzone dla naszych nowe konto miejsca do magazynowania i naszych klucza magazynu. Firma Microsoft będzie również ustawić **-włączone** flaga **$true** i ustaw odpowiednią kategorię AuditEvent (tylko kategorii do rejestrowania magazynu klucza):


    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

Wynik to zawiera:

    StorageAccountId   : /subscriptions/<subscription-GUID>/resourceGroups/ContosoResourceGroup/providers/Microsoft.Storage/storageAccounts/ContosoKeyVaultLogs
    ServiceBusRuleId   :
    StorageAccountName :
        Logs
        Enabled           : True
        Category          : AuditEvent
        RetentionPolicy
        Enabled : False
        Days    : 0


Jest to potwierdzenie, że dla swojego klucza magazynu zapisywania informacji do swojego konta miejsca do magazynowania teraz jest włączone rejestrowanie.

Opcjonalnie można także ustawić zasady przechowywania dla dzienników tak, aby dzienniki starsze zostaną automatycznie usunięte. Na przykład ustawić zasady przechowywania przy użyciu flaga **- RetentionEnabled** **$true** i **- RetentionInDays** parametr jest ustawiony na **90** , tak aby dzienniki starsze niż 90 dni, zostaną automatycznie usunięte.

    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent -RetentionEnabled $true -RetentionInDays 90

Co to jest rejestrowany:

- Są rejestrowane wszystkie uwierzytelniony żądania interfejsu API usługi REST zawierająca żądań zakończonych niepowodzeniem wyniku uprawnienia dostępu, błędy systemu lub nieprawidłowe żądania.
- Operacje na kluczu vault siebie, który zawiera tworzenie, usuwanie, ustawienie zasad dostępu klucza magazynu, i aktualizowanie klucza magazynu atrybutów, takich jak znaczniki.
- Operacje na klucze i hasła klucza magazynu, obejmujące tworzenie, modyfikowanie lub usuwanie tych klawiszy lub hasła; operacji, takich jak znak, sprawdź, szyfrowanie, odszyfrowywanie, Zawijanie i cofnąć zawijanie klawiszy, uzyskaj hasła, klawiszy listy i hasła oraz ich wersje.
- Nieuwierzytelnionych żądania, które powodują 401 odpowiedź. Na przykład żądania, które nie mają token okaziciela są nieprawidłowo lub wygasłą lub ma nieprawidłowy token.  


## <a id="access"></a>Dostęp do dzienników ##

Dzienniki klucza magazynu są przechowywane w kontenerze **wniosków dzienniki auditevent** na koncie miejsca do magazynowania, przez Ciebie informacje. Aby wyświetlić listę wszystkich obiektów blob w tym kontenerze, wpisz:

    Get-AzureStorageBlob -Container 'insights-logs-auditevent' -Context $sa.Context

Wynik będzie wyglądać podobnie do następującej:

**Identyfikator Uri kontenera: https://contosokeyvaultlogs.blob.core.windows.net/insights-logs-auditevent**


**Nazwa**

**----**

**resourceId = subskrypcje 361DA5D4-A47A-4C 79-AFDD-XXXXXXXXXXXX-RESOURCEGROUPS-CONTOSORESOURCEGROUP-dostawców i firmy MICROSOFT. KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=05/h=01/m=00/PT1H.json**

**resourceId = subskrypcje 361DA5D4-A47A-4C 79-AFDD-XXXXXXXXXXXX-RESOURCEGROUPS-CONTOSORESOURCEGROUP-dostawców i firmy MICROSOFT. KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=02/m=00/PT1H.json**

** resourceId = subskrypcje 361DA5D4-A47A-4C 79-AFDD-XXXXXXXXXXXX-RESOURCEGROUPS-CONTOSORESOURCEGROUP-dostawców i firmy MICROSOFT. KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=18/m=00/PT1H.json***


Jak widać z tego raportu, obiektów blob wykonaj konwencji nazewnictwa: **resourceId =<ARM resource ID>/y =<year>/m =<month>/d =<day of month>/h =<hour>/m =<minute>/filename.json**

Wartości daty i godziny za pomocą UTC.

Ponieważ tego samego konta miejsca do magazynowania umożliwia zbieranie dzienników dla wielu zasobów, identyfikator zasobu pełną nazwę obiektów blob jest bardzo przydatne uzyskać dostęp do lub pobrać tylko obiektów blob, które są potrzebne. Ale przed czynności, aby najpierw omówiono sposób pobierania wszystkich obiektów blob.

Najpierw należy utworzyć folder, aby pobrać obiektów blob. Na przykład:

    New-Item -Path 'C:\Users\username\ContosoKeyVaultLogs' -ItemType Directory -Force

Następnie uzyskać listę wszystkich obiektów blob:  

    $blobs = Get-AzureStorageBlob -Container $container -Context $sa.Context

Potok tej listy za pomocą "Get-AzureStorageBlobContent", aby pobrać BLOB do naszych folder docelowy:

    $blobs | Get-AzureStorageBlobContent -Destination 'C:\Users\username\ContosoKeyVaultLogs'

Po uruchomieniu tego polecenia drugi **/** ogranicznika w nazwach obiektów blob tworzenia struktury pełny folder w obszarze folder docelowy, a struktury są używane do pobierania i przechowywania obiektów blob jako pliki.

Aby pobrać selektywne obiektów blob, użyj symboli wieloznacznych. Na przykład:

- Jeśli masz wiele magazynami najważniejszych i pobrać dzienniki dla tylko jednego klucza magazynu, o nazwie CONTOSOKEYVAULT3:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/VAULTS/CONTOSOKEYVAULT3

- Jeśli masz wiele grup zasobów, aby pobrać dzienniki grupy zasobów tylko jeden za pomocą `-Blob '*/RESOURCEGROUPS/<resource group name>/*'`:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'

- Jeśli chcesz pobrać wszystkie dzienniki w miesiącu stycznia 2016, za pomocą `-Blob '*/year=2016/m=01/*'`:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/year=2016/m=01/*'

Możesz teraz rozpocząć wyszukiwanie, co jest w dziennikach. Ale zanim przejdziesz na które dwóch parametrów AzureRmDiagnosticSetting Get, który może być konieczne, aby dowiedzieć się więcej:

- Aby wykonać kwerendę dotyczącą stanu ustawień diagnostycznych dla zasobu magazynu kluczy:`Get-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId`

- Aby wyłączyć rejestrowanie dla zasobu magazynu kluczy:`Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $false -Categories AuditEvent`


## <a id="interpret"></a>Interpretowanie dzienników klucza magazynu ##

Poszczególne BLOB są przechowywane jako tekst w formacie JSON blob. To jest przykładowy wpis dziennika uruchamianie `Get-AzureRmKeyVault -VaultName 'contosokeyvault'`:

    {
        "records":
        [
            {
                "time": "2016-01-05T01:32:01.2691226Z",
                "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
                "operationName": "VaultGet",
                "operationVersion": "2015-06-01",
                "category": "AuditEvent",
                "resultType": "Success",
                "resultSignature": "OK",
                "resultDescription": "",
                "durationMs": "78",
                "callerIpAddress": "104.40.82.76",
                "correlationId": "",
                "identity": {"claim":{"http://schemas.microsoft.com/identity/claims/objectidentifier":"d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn":"live.com#username@outlook.com","appid":"1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},
                "properties": {"clientInfo":"azure-resource-manager/2.0","requestUri":"https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id":"https://contosokeyvault.vault.azure.net/","httpStatusCode":200}
            }
        ]
    }


W poniższej tabeli wymieniono nazwy pól i opisy.


| Nazwa pola        | Opis |
| ------------- |-------------|
| czas      | Data i godzina (UTC).|
| resourceId      | Identyfikator zasobu Azure Menedżera zasobów. W przypadku dzienników klucza magazynu jest zawsze identyfikator magazynu klucz zasobu.|
| operationName      | Nazwa operacji, opisane w następnej tabeli.|
| operationVersion      | To jest wersja interfejsu API usługi REST żądany przez klienta.|
| Kategoria      | Dzienniki magazynu klucza AuditEvent jest wartością jednym, dostępne.|
| resultType      | Wynik żądania interfejsu API usługi REST.|
| resultSignature      | Stan HTTP.|
| resultDescription     | Dodatkowe opis wynik, gdy są dostępne.|
| durationMs      | Czas potrzebny do obsługi żądania interfejsu API usługi REST, (w milisekundach). Aby razem, gdy zmierzyć po stronie klienta mogą być niezgodne z tym razem nie obejmuje czasu oczekiwania w sieci.|
| callerIpAddress      | Adres IP klienta, który wezwanie.|
| correlationId      | Opcjonalnie GUID, jaki klient może upłynąć do przeniesionym dzienniki po stronie klienta przy użyciu usługi rejestrowania po stronie (klawisz magazynu).|
| tożsamości      | Tożsamość z tokenu, które zostały przedstawione podczas tworzenia żądania interfejsu API usługi REST. Jest to zazwyczaj "użytkownika", "wystawcy usługi" lub "użytkownika + identyfikator aplikacji" kombinację tak jak w przypadku żądania wynikające z polecenia cmdlet programu PowerShell Azure.|
| właściwości      | To pole będzie zawierać różne informacje opartego na operacji (operationName). W większości przypadków zawiera informacje klienta (ciąg useragent przekazane przez klienta), dokładnie identyfikator URI żądania interfejsu API usługi REST i kod stanu HTTP. Ponadto gdy obiekt jest zwracany w wyniku żądania (na przykład KeyCreate lub VaultGet) on będzie również zawierać klucz identyfikator URI (jako "identyfikator"), identyfikator URI magazynu lub identyfikator URI tajny.|




Wartości pól **operationName** są dostępne w formacie ObjectVerb. Na przykład:

- Wszystkie operacje klucza magazynu "magazynu`<action>`" formatowania, takich jak `VaultGet` i `VaultCreate`.

- Wszystkie operacje klucza "klucz`<action>`" formatowania, takich jak `KeySign` i `KeyList`.

- Wszystkie operacje poufne "tajny`<action>`" formatowania, takich jak `SecretGet` i `SecretListVersions`.

W poniższej tabeli wymieniono operationName i odpowiednie polecenie interfejsu API usługi REST.

| operationName        | Polecenia interfejsu API REST |
| ------------- |-------------|
| Uwierzytelnianie      | Za pośrednictwem punktu końcowego usługi Azure Active Directory|
| VaultGet      | [Uzyskiwanie informacji na temat klucza magazynu](https://msdn.microsoft.com/en-us/library/azure/mt620026.aspx)|
| VaultPut      | [Tworzenie lub aktualizowanie klucza magazynu](https://msdn.microsoft.com/en-us/library/azure/mt620025.aspx)|
| VaultDelete      | [Usuwanie klucza magazynu](https://msdn.microsoft.com/en-us/library/azure/mt620022.aspx)|
| VaultPatch      | [Aktualizowanie klucza magazynu](https://msdn.microsoft.com/library/azure/mt620025.aspx)|
| VaultList      | [Lista wszystkich magazynami najważniejszych w grupie zasobów](https://msdn.microsoft.com/en-us/library/azure/mt620027.aspx)|
| KeyCreate      | [Tworzenie klucza](https://msdn.microsoft.com/en-us/library/azure/dn903634.aspx)|
| KeyGet      | [Uzyskiwanie informacji na temat klucza](https://msdn.microsoft.com/en-us/library/azure/dn878080.aspx)|
| KeyImport      | [Importowanie klucz do magazynu](https://msdn.microsoft.com/en-us/library/azure/dn903626.aspx)|
| KeyBackup      | [Kopia zapasowa klucza](https://msdn.microsoft.com/en-us/library/azure/dn878058.aspx).|
| KeyDelete      | [Usuwanie klucza](https://msdn.microsoft.com/en-us/library/azure/dn903611.aspx)|
| KeyRestore      | [Przywracanie klucza](https://msdn.microsoft.com/en-us/library/azure/dn878106.aspx)|
| KeySign      | [Zaloguj się przy użyciu klucza](https://msdn.microsoft.com/en-us/library/azure/dn878096.aspx)|
| KeyVerify      | [Sprawdź przy użyciu klucza](https://msdn.microsoft.com/en-us/library/azure/dn878082.aspx)|
| KeyWrap      | [Zawijanie klucza](https://msdn.microsoft.com/en-us/library/azure/dn878066.aspx)|
| KeyUnwrap      | [Cofnij zawijanie klucza](https://msdn.microsoft.com/en-us/library/azure/dn878079.aspx)|
| KeyEncrypt      | [Szyfruj przy użyciu klucza](https://msdn.microsoft.com/en-us/library/azure/dn878060.aspx)|
| KeyDecrypt      | [Odszyfrowywanie przy użyciu klucza](https://msdn.microsoft.com/en-us/library/azure/dn878097.aspx)|
| KeyUpdate      | [Aktualizowanie klucza](https://msdn.microsoft.com/en-us/library/azure/dn903616.aspx)|
| KeyList      | [Lista klucze z magazynu](https://msdn.microsoft.com/en-us/library/azure/dn903629.aspx)|
| KeyListVersions      | [Lista wersji klucza](https://msdn.microsoft.com/en-us/library/azure/dn986822.aspx)|
| SecretSet      | [Tworzenie hasła](https://msdn.microsoft.com/en-us/library/azure/dn903618.aspx)|
| SecretGet      | [Uzyskiwanie poufne](https://msdn.microsoft.com/en-us/library/azure/dn903633.aspx)|
| SecretUpdate      | [Zaktualizuj hasło](https://msdn.microsoft.com/en-us/library/azure/dn986818.aspx)|
| SecretDelete      | [Usuwanie hasła](https://msdn.microsoft.com/en-us/library/azure/dn903613.aspx)|
| SecretList      | [Lista haseł w magazynu](https://msdn.microsoft.com/en-us/library/azure/dn903614.aspx)|
| SecretListVersions      | [Wersje listy hasła](https://msdn.microsoft.com/en-us/library/azure/dn986824.aspx)|




## <a id="next"></a>Następne kroki ##

Samouczek, która używa magazynu klucza Azure w aplikacji sieci web zobacz [Używanie Azure klucza magazynu z aplikacji sieci Web](key-vault-use-from-web-application.md).

Programowania odwołania, zobacz [Przewodnik programisty Azure klucza magazynu](key-vault-developers-guide.md).

Aby uzyskać listę poleceń cmdlet Azure PowerShell 1.0 dla magazynu klucza Azure zobacz [Polecenia cmdlet magazynu klucza Azure](https://msdn.microsoft.com/library/azure/dn868052.aspx).

Samouczek dotyczący klucza obrót i dziennika inspekcji z magazynu klucza Azure zobacz [jak ustawienia klucza magazynu przy użyciu klucza kompleksowego obrót i inspekcja](key-vault-key-rotation-log-monitoring.md).
