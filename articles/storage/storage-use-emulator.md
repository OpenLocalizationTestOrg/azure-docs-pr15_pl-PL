<properties 
    pageTitle="Używanie emulatora Azure miejsca do magazynowania dla projektowania i testowania | Microsoft Azure" 
    description="Emulator magazynowania Azure oferuje środowisko bezpłatne rozwoju lokalnego do tworzenia i testowania przed magazyn Azure. Informacje na temat emulatora miejsca do magazynowania, w tym sposób uwierzytelniania żądania, sposobie łączenia się emulatorze z poziomu aplikacji i sposobu używania narzędzia wiersza polecenia." 
    services="storage" 
    documentationCenter="" 
    authors="tamram" 
    manager="carmonm" 
    editor="tysonn"/>
<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/07/2016" 
    ms.author="tamram"/>

# <a name="use-the-azure-storage-emulator-for-development-and-testing"></a>Używanie emulatora Azure miejsca do magazynowania dla projektowania i testowania

## <a name="overview"></a>Omówienie

Emulator miejsca do magazynowania Microsoft Azure udostępnia środowisku lokalnym, która emuluje usługi obiektów Blob platformy Azure, kolejki i tabeli na potrzeby projektowania. Za pomocą emulatora miejsca do magazynowania, możesz przetestować aplikacji przed usług magazynu lokalnie, bez tworzenia subskrypcji usługi Azure lub ponoszenia koszty. Po zakończeniu pracy aplikacji w emulatorze, można przełączyć się przy użyciu konta usługi Azure magazynu w chmurze.

> [AZURE.NOTE] Emulator miejsca do magazynowania jest dostępne jako część [Microsoft Azure SDK](https://azure.microsoft.com/downloads/). Można również zainstalować emulatora miejsca do magazynowania za pomocą [Instalatora autonomicznego](https://go.microsoft.com/fwlink/?linkid=717179&clcid=0x409). Aby skonfigurować emulatora miejsca do magazynowania, musisz mieć uprawnienia administracyjne na tym komputerze.
> 
> Emulator miejsca do magazynowania obecnie działa tylko w systemie Windows.
>  
> Należy zauważyć, że dane utworzone w jednej wersji emulatora miejsca do magazynowania nie jest gwarantowana była dostępna w przypadku korzystania z innej wersji. Jeśli potrzebujesz długoterminowe są przechowywane dane, zaleca się przechowywać te dane przy użyciu konta z miejsca do magazynowania Azure, a nie w emulatorze miejsca do magazynowania.

## <a name="how-the-storage-emulator-works"></a>Jak działa emulatora miejsca do magazynowania
 
Emulator magazynu używa lokalnego wystąpienia programu Microsoft SQL Server i lokalny system plików w celu emulacji pól usług Azure magazynu. Domyślnie emulatora magazynu używa bazy danych programu Microsoft SQL Server 2012 Express LocalDB.  Możesz skonfigurować emulatora przestrzeni dyskowej, aby uzyskać dostęp do lokalnego wystąpienia programu SQL Server zamiast wystąpienia LocalDB. Aby uzyskać więcej informacji, zobacz [rozpoczęcia i inicjowania emulatora miejsca do magazynowania](#start-and-initialize-the-storage-emulator) poniżej.

Możesz zainstalować programu SQL Server Management Studio Express do zarządzania instalacji LocalDB. Emulator miejsca do magazynowania łączy się z programu SQL Server lub LocalDB przy użyciu funkcji uwierzytelniania systemu Windows. 

Między emulatora miejsca do magazynowania i usługi Azure magazynu występują pewne różnice w funkcjach. Aby uzyskać więcej informacji na temat tych różnic zobacz [różnice między emulatora miejsca do magazynowania i miejsca do magazynowania Azure](#differences-between-the-storage-emulator-and-azure-storage).

## <a name="authenticating-requests-against-the-storage-emulator"></a>Uwierzytelnianie żądania emulatora miejsca do magazynowania

Tak jak w przypadku Azure miejscem do magazynowania w chmurze, co zgody przed emulatora miejsca do magazynowania muszą zostać uwierzytelnione, o ile nie jest anonimowe żądanie. Można uwierzytelnić żądania przed emulatora miejsca do magazynowania przy użyciu funkcji uwierzytelniania klucza udostępnionego lub z podpisem udostępniania (SA).

### <a name="authentication-with-shared-key-credentials"></a>Uwierzytelnianie przy użyciu poświadczeń klucz udostępniony

[AZURE.INCLUDE [storage-emulator-connection-string-include](../../includes/storage-emulator-connection-string-include.md)]

Aby uzyskać więcej informacji na parametry połączenia zobacz [Konfigurowanie parametry połączenia miejsca do magazynowania Azure](storage-configure-connection-string.md). 

### <a name="authentication-with-a-shared-access-signature"></a>Uwierzytelnianie przy użyciu podpisu udostępniania 

Niektóre bibliotek klienta Azure miejsca do magazynowania, takie jak biblioteka Xamarin obsługuje tylko uwierzytelnianie za pomocą tokenu podpisu (SA) udostępnionego dostępu. Należy utworzyć ten token skojarzenia zabezpieczeń za pomocą narzędzia lub aplikacji, która obsługuje uwierzytelnianie klucz udostępniony. Łatwy sposób wygenerowania tokenu skojarzeń zabezpieczeń jest za pośrednictwem Azure programu PowerShell:

1. Jeśli jeszcze tego nie zrobiono, zainstaluj Azure programu PowerShell. Zaleca się korzystać z najnowszych wersji polecenia cmdlet programu PowerShell Azure. Zobacz, [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md#Install) instrukcje dotyczące instalacji.

2. Otwórz Azure programu PowerShell i uruchom następujące polecenia. Pamiętaj, aby zamienić *nazwa_konta* i *ACCOUNT_KEY ==* przy użyciu własnej poświadczeń. Zamień nazwę *CONTAINER_NAME* .

        $context = New-AzureStorageContext -StorageAccountName "ACCOUNT_NAME" -StorageAccountKey "ACCOUNT_KEY=="
        
        New-AzureStorageContainer CONTAINER_NAME -Permission Off -Context $context
        
        $now = Get-Date 
        
        New-AzureStorageContainerSASToken -Name CONTAINER_NAME -Permission rwdl -ExpiryTime $now.AddDays(1.0) -Context $context -FullUri

Wyniku podpisu udostępnienia identyfikatora URI dla nowego kontenera powinny być podobny do następującego:

    https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2015-07-08T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3Dsss

Podpis udostępnienia utworzone za pomocą w tym przykładzie jest prawidłowy jeden dzień. Podpis udziela pełnego dostępu (to znaczy Odczyt, zapis, Usuń, lista) do obiektów blob w kontenerze.

Aby uzyskać więcej informacji na podpisów udostępniania zobacz [Przy użyciu podpisów dostępu do udostępnionej (SA)](storage-dotnet-shared-access-signature-part-1.md).


## <a name="start-and-initialize-the-storage-emulator"></a>Rozpoczynanie i zainicjowania emulatora miejsca do magazynowania

Aby rozpocząć emulatora Azure miejsca do magazynowania, kliknij przycisk Start, lub naciśnij klawisz systemu Windows. Zacznij wpisywać **Azure emulatora miejsca do magazynowania**, a następnie wybierz emulatorze z listy aplikacji. 

Po uruchomieniu emulatorze pojawi się ikona w obszarze powiadomień paska zadań systemu Windows.

Po uruchomieniu emulatora miejsca do magazynowania, pojawi się okno wiersza polecenia. Można to okno wiersza polecenia służy do Rozpocznij i Zatrzymaj emulatora miejsca do magazynowania, a także Wyczyść dane, Pobierz bieżący stan i zainicjować emulatorze. Aby uzyskać więcej informacji zobacz [Miejsca do magazynowania emulatora wiersza polecenia narzędzia odwołań](#storage-emulator-command-line-tool-reference).

Po zamknięciu okna wiersza polecenia emulatora miejsca do magazynowania będzie działać. Aby ponownie wyświetlić wiersza polecenia, wykonaj powyższe kroki, tak jakby uruchamianie emulatora miejsca do magazynowania.

Podczas pierwszego uruchomienia emulatorze miejsca do magazynowania środowisko lokalne przechowywanie został zainicjowany dla Ciebie. Proces inicjowania tworzy bazę danych w LocalDB i zastrzega sobie portów protokołu HTTP dla każdej usługi magazynu lokalnego. 

Domyślnie do katalogu C:\Program Files (x86) \Microsoft SDKs\Azure\Storage emulatora jest zainstalowany emulatora miejsca do magazynowania. 

### <a name="initialize-the-storage-emulator-to-use-a-different-sql-database"></a>Inicjowanie emulatora miejsca do magazynowania, aby użyć innej bazy danych SQL

Za pomocą narzędzia wiersza polecenia emulatora miejsca do magazynowania zainicjować emulatora przestrzeni dyskowej, aby wskazywały inne niż domyślne wystąpienie LocalDB wystąpienie bazy danych SQL. Zwróć uwagę, że musisz korzystać z narzędzia wiersza polecenia z uprawnieniami administratora, aby zainicjować wewnętrznej bazy danych dla emulatora miejsca do magazynowania:

1. Kliknij przycisk **Start** , lub naciśnij klawisz **systemu Windows** . Zacznij wpisywać `Azure Storage Emulator` i wybierz ją, jeśli wydaje się, aby wyświetlić narzędzia wiersza polecenia emulatora miejsca do magazynowania.
2. W oknie wiersza polecenia wpisz następujące polecenie, gdzie `<SQLServerInstance>` to nazwa wystąpienie programu SQL Server. Aby użyć LocalDb, określ `(localdb)\v11.0` jako wystąpienie programu SQL Server.

        AzureStorageEmulator init /server <SQLServerInstance> 
    
    Umożliwia także następujące polecenie, które kieruje emulatorze, aby użyć domyślnego wystąpienia programu SQL Server:

        AzureStorageEmulator init /server .\\ 

    Lub można użyć następujące polecenie, które ponownie inicjuje domyślne wystąpienie LocalDB w bazie danych:

        AzureStorageEmulator init /forceCreate 

Aby uzyskać więcej informacji na temat tych poleceń zobacz [Miejsca do magazynowania emulatora wiersza polecenia narzędzia odwołań](#storage-emulator-command-line-tool-reference).

## <a name="addressing-resources-in-the-storage-emulator"></a>Adresowanie zasobów w emulatorze miejsca do magazynowania

Punkty końcowe usługi dla emulatora miejsca do magazynowania różnią się od tych konto Azure miejsca do magazynowania. Różnica polega na fakt, że komputer lokalny wykonuje rozpoznawanie nazw domen, tak więc punkty końcowe emulatora miejsca do magazynowania wymagać lokalny adres, a nie nazwę domeny.

Adres zasobu przy użyciu konta z miejsca do magazynowania Azure, jest przy użyciu schemat następujących, miejsce, w którym nazwa konta jest część nazwy hosta identyfikatora URI i zasobów skierowane jest częścią ścieżki identyfikatora URI:

    <http|https>://<account-name>.<service-name>.core.windows.net/<resource-path>

Na przykład następujący identyfikator URI jest prawidłowy adres obiektów blob przy użyciu konta z miejsca do magazynowania Azure:

    https://myaccount.blob.core.windows.net/mycontainer/myblob.txt

Ponieważ komputer lokalny nie jest sprawdzana rozpoznawanie nazw domen, nazwę konta w emulatorze miejsca do magazynowania jest część ścieżki URI zamiast nazwa hosta. Następujący plan można użyć dla zasobu w emulatorze miejsca do magazynowania:

    http://<local-machine-address>:<port>/<account-name>/<resource-path>

Na przykład następujący adres mogą być używane do uzyskiwania dostępu do obiektów blob w emulatorze miejsca do magazynowania:

    http://127.0.0.1:10000/myaccount/mycontainer/myblob.txt

Punkty końcowe usługi dla emulatora przestrzeni dyskowej są:

    Blob Service: http://127.0.0.1:10000/<account-name>/<resource-path>
    Queue Service: http://127.0.0.1:10001/<account-name>/<resource-path>
    Table Service: http://127.0.0.1:10002/<account-name>/<resource-path>

### <a name="addressing-the-account-secondary-with-ra-grs"></a>Adresowanie pomocniczej z GRS pomoc Zdalna konta

Począwszy od wersji 3.1, konto emulatora miejsca do magazynowania obsługuje replikacji zbędne geo odczytu (Pomoc Zdalna GRS). Dla zasobów magazynowania zarówno w chmurze, jak i w emulatorze lokalnych, mają dostęp do lokalizacji pomocniczej dołączanie przez - pomocniczej do nazwy konta. Na przykład następujący adres mogą być używane do uzyskiwania dostępu do obiektów blob przy użyciu pomocniczego tylko do odczytu w emulatorze miejsca do magazynowania:

    http://127.0.0.1:10000/myaccount-secondary/mycontainer/myblob.txt 

> [AZURE.NOTE] Dla programowy dostęp do pomocniczego emulatorze miejsca do magazynowania należy użyć biblioteki klienta miejsca do magazynowania dla środowiska .NET wersji 3,2 lub nowszej. Zobacz [Biblioteka klienta programu Microsoft Azure miejsca do magazynowania dla środowiska .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx) , aby uzyskać szczegółowe informacje.

## <a name="storage-emulator-command-line-tool-reference"></a>Odwołanie wiersza polecenia narzędzia emulatora miejsca do magazynowania

Począwszy od wersji 3.0, po uruchomieniu emulatora miejsca do magazynowania, zobaczysz okno wiersza polecenia wyskakujące. Okno wiersza polecenia służy do rozpoczynania i kończenia emulatorze także wyszukiwać stanu i wykonywać inne zadania.

> [AZURE.NOTE] Jeśli masz Microsoft Azure obliczyć emulatora zainstalowane, ikona na pasku zadań pojawi się po uruchomieniu emulatora miejsca do magazynowania. Kliknij prawym przyciskiem myszy ikonę, aby wyświetlić menu, który umożliwia graficzne do rozpoczynania i kończenia emulatora miejsca do magazynowania.

### <a name="command-line-syntax"></a>Składnia wiersza polecenia

    AzureStorageEmulator [start] [stop] [status] [clear] [init] [help]

### <a name="options"></a>Opcje

Aby wyświetlić listę opcji, wpisz `/help` w wierszu polecenia.

| Opcja | Opis                                                    | Polecenie                                                                                                 | Argumenty                                                                                                         |
|--------|----------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| **Rozpoczynanie**  | Uruchamiania emulatora miejsca do magazynowania.                                | `AzureStorageEmulator start [-inprocess]`                                                                    | *-inprocess*: rozpoczynanie emulatorze w bieżącym procesie zamiast tworzenia nowego procesu.                          |
| **Zatrzymywanie**   | Zatrzymuje emulatora miejsca do magazynowania.                                    | `AzureStorageEmulator stop`                                                                                  |                                                                                                                   |
| **Stan** | Umożliwia wydrukowanie stanu emulatora miejsca do magazynowania.                     | `AzureStorageEmulator status`                                                                                |                                                                                                                   |
| **Wyczyść**  | Pozwala wyczyścić dane w wszystkich usług określonego w wierszu polecenia. | `AzureStorageEmulator clear [blob] [table] [queue] [all]                                                    `| *obiektów blob*: Czyści blob danych. <br/>*Kolejka*: usuwa kolejki danych. <br/>*Tabela*: pozwala wyczyścić dane w tabeli. <br/>*Wszystkie*: usuwa wszystkie dane w usługach wszystkie. |
| **Inicjowanie**   | Wykonuje jednorazowego inicjowania, aby skonfigurować emulatorze.       | `AzureStorageEmulator.exe init [-server serverName] [-sqlinstance instanceName] [-forcecreate] [-inprocess]` | *-serverName\instanceName serwera*: Określa serwera obsługującego wystąpienie programu SQL. <br/>*nazwa_wystąpienia - sqlinstance*: Nazwa wystąpienie programu SQL może być używany w domyślnym wystąpieniu serwera. <br/>*-forcecreate*: wymusza utworzenia bazy danych SQL, nawet jeśli już istnieje. <br/>*-inprocess*: wykonuje inicjowanie w bieżącym procesie zamiast duplikowanie nowy proces. Należy uruchomić bieżącego procesu z podwyższonym poziomem uprawnień w celu wykonania inicjowanie.          |
                                                                                                                  
## <a name="differences-between-the-storage-emulator-and-azure-storage"></a>Różnice między emulatora miejsca do magazynowania i Magazyn Azure

Ponieważ emulatora miejsca do magazynowania jest emulowanej środowiska działa w lokalne wystąpienie programu SQL, istnieją pewne różnice w funkcjach między emulatorze i konto Azure miejsca do magazynowania w chmurze:

- Emulator miejsca do magazynowania obsługuje tylko jednego konta stały i klucz znanego uwierzytelniania.

- Emulator miejsca do magazynowania jest usługa skalowalna magazynu i nie obsługuje duża liczba klientów jednocześnie.

- Zgodnie z opisem w [Adresowanie zasobów w emulatorze miejsca do magazynowania](#addressing-resources-in-the-storage-emulator), zasoby dotyczą inaczej emulatora miejsca do magazynowania, a konto Azure miejsca do magazynowania. Ta różnica jest z faktu, że rozpoznawanie nazw domen jest dostępna w chmurze, ale nie na komputerze lokalnym.

- Począwszy od wersji 3.1, konto emulatora miejsca do magazynowania obsługuje replikacji zbędne geo odczytu (Pomoc Zdalna GRS). W emulatorze wszystkie konta mają GRS pomoc Zdalna, włączony, a nigdy nie jest dowolnego czasu zwłoki między replikami głównego i pomocniczego. Uzyskiwanie statystyki usługi obiektów Blob, uzyskać statystykę usługi kolejki i uzyskiwanie tabeli statystyki usługi działania są obsługiwane na rachunku pomocniczej i zawsze zwraca wartość `LastSyncTime` element odpowiedzi jako bieżącą godzinę według podstawowej bazy danych SQL.

    Dla programowy dostęp do pomocniczego emulatorze miejsca do magazynowania należy użyć biblioteki klienta miejsca do magazynowania dla środowiska .NET wersji 3,2 lub nowszej. Zobacz [Biblioteka klienta programu Microsoft Azure miejsca do magazynowania dla środowiska .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx) , aby uzyskać szczegółowe informacje.

- Usługa plików i punktów końcowych usługi protokołu SMB nie są obecnie obsługiwane w emulatorze miejsca do magazynowania.

- Emulator miejsca do magazynowania zwraca błąd VersionNotSupportedByEmulator (kod stanu HTTP 400 — Nieprawidłowe żądanie), jeśli używasz wersji usługi Magazyn, który jeszcze nie jest obsługiwany przez wersję emulatora, którego używasz.

### <a name="differences-for-blob-storage"></a>Różnice w magazyn obiektów Blob 

Występują następujące różnice dotyczą magazyn obiektów Blob w emulatorze:

- Miejsca do magazynowania emulatora tylko obsługuje blob o rozmiarze do 2 GB.

- Operacja umieszczanie obiektów Blob może powiodła się przed obiektów blob, który istnieje w emulatorze miejsca do magazynowania i aktywnej dzierżawy, nawet wtedy, gdy nie określono identyfikator dzierżawy w ramach żądania. 

- Dołączanie obiektów Blob operacje nie są obsługiwane przez emulator. Próby operacji na dołączanie obiektów blob zwraca błąd FeatureNotSupportedByEmulator (kod stanu HTTP 400 — Nieprawidłowe żądanie).

### <a name="differences-for-table-storage"></a>Różnice w magazyn tabel 

Występują następujące różnice dotyczą magazyn tabel w emulatorze:

- Właściwości daty w usłudze tabeli w emulatorze miejsca do magazynowania obsługuje tylko zakres obsługiwanych przez program SQL Server 2005 (*to znaczy*, są one wymagane później niż 1 stycznia 1753). Wszystkie daty przed 1 stycznia 1753 są zamieniane na tę wartość. Dokładność dat jest ograniczone do dokładności programu SQL Server 2005, co oznacza, że daty są precyzyjnie 1/300th sekundy.

- Emulator miejsca do magazynowania obsługuje wartości partition klucz i wiersza właściwości klucza mniej niż 512 bajtów każdego. Ponadto całkowity rozmiar nazwę konta, nazwa tabeli i nazwy właściwości klucza razem nie może przekraczać 900 bajtów.

- Całkowity rozmiar wiersza w tabeli w emulatorze miejsca do magazynowania jest ograniczone do mniej niż 1 MB.

- W emulatorze miejsca do magazynowania właściwości danych wpisz `Edm.Guid` lub `Edm.Binary` obsługuje tylko `Equal (eq)` i `NotEqual (ne)` operatory porównania w kwerendzie filtrowanie ciągów.

### <a name="differences-for-queue-storage"></a>Różnice w kolejce miejsca do magazynowania

Nie ma żadnych różnic specyficzne dla miejsca do magazynowania kolejki w emulatorze.

## <a name="storage-emulator-release-notes"></a>Informacje o wersji emulatora miejsca do magazynowania

### <a name="version-45"></a>Wersja 4,5

- Stałe błąd powodujący inicjowanie i instalacji emulatora miejsca do magazynowania kończy się niepowodzeniem, gdy zmieniono kopii bazy danych.

### <a name="version-44"></a>Wersja 4.4

- Emulator miejsca do magazynowania obsługuje teraz wersję 2015-12-11 usługi miejsca do magazynowania na punkty końcowe usługi obiektów Blob, kolejki i tabeli.

- Zbieranie śmieci emulatora miejsca do magazynowania danych obiektów blob teraz jest bardziej efektywne, zajmujące dużej liczby obiektów blob.

- Stałe błąd powodujący kontenera XML list ACL będzie sprawdzana poprawność nieco inaczej z usługą Magazyn skąd go.

- Stałe błąd powodujący czasami min i max wartości daty/godziny, należy podać w strefie czasowej niepoprawne.

### <a name="version-43"></a>Wersja 4.3

- Emulator miejsca do magazynowania obsługuje teraz wersji 2015-07-08 usług magazynu o obiektów Blob, kolejki i tabeli usługi.

### <a name="version-42"></a>Wersji 4.2

- Emulator miejsca do magazynowania obsługuje teraz wersji 2015-04-05 usług magazynu o obiektów Blob, kolejki i tabeli usługi.

### <a name="version-41"></a>Wersja 4.1

- Emulator miejsca do magazynowania obsługuje teraz wersji 2015-02-21 usług miejsca do magazynowania na obiektów Blob, kolejki i tabeli usługi punkty końcowe, z wyjątkiem nowych funkcji dołączanie obiektów Blob. 

- Emulator miejsca do magazynowania teraz zwraca komunikat o błędzie użyteczny, jeśli używasz wersji usługi Magazyn, który jeszcze nie jest obsługiwany przez tę wersję emulatora. Zalecamy używanie najnowszej wersji emulatora. W przypadku napotkania błędu VersionNotSupportedByEmulator (kod stanu HTTP 400 — Nieprawidłowe żądanie) Pobierz najnowszą wersję pakietu emulatora miejsca do magazynowania.

- Stałe błędu którym dane jednostki wyścigowe warunek uszkodzenie tabeli są niepoprawne podczas operacji Scal jednocześnie.

### <a name="version-40"></a>W wersji 4.0

- Wykonywalny emulatora miejsca do magazynowania została zmieniona na *AzureStorageEmulator.exe*.

### <a name="version-32"></a>Wersja 3,2

- Emulator miejsca do magazynowania obsługuje teraz wersję 2014-02-14 usługi miejsca do magazynowania na punkty końcowe usługi obiektów Blob, kolejki i tabeli. Należy zauważyć, że punkty końcowe usługi pliku nie są obecnie obsługiwane w emulatorze miejsca do magazynowania. Zobacz [przechowywania wersji dla usługi Azure przestrzeni dyskowej](https://msdn.microsoft.com/library/azure/dd894041.aspx) , aby uzyskać szczegółowe informacje o wersji 2014-02-14.

### <a name="version-31"></a>Wersji 3.1

- Dostęp do odczytu zbędne geo miejsca do magazynowania (Pomoc Zdalna GRS) są teraz obsługiwane emulatora miejsca do magazynowania. Uzyskiwanie statystyki usługi obiektów Blob, uzyskać statystykę usługi kolejki uzyskiwanie API statystykę usługi tabeli są obsługiwane w przypadku konta pomocniczej i zawsze zwraca wartość elementu odpowiedź LastSyncTime jako bieżącą godzinę według podstawowej bazy danych SQL. Dla programowy dostęp do pomocniczego emulatorze miejsca do magazynowania należy użyć biblioteki klienta miejsca do magazynowania dla środowiska .NET wersji 3,2 lub nowszej. Aby uzyskać szczegółowe informacje, zobacz Biblioteka klienta programu Microsoft Azure miejsca do magazynowania dla odwołania .NET.

### <a name="version-30"></a>W wersji 3.0

- Emulator Azure miejsca do magazynowania już nie jest dostarczany w pakiecie jako emulator obliczeń.

- Miejsca do magazynowania emulatora graficznego interfejsu użytkownika jest zastąpiona skryptowych interfejs wiersza polecenia. Aby uzyskać szczegółowe informacje na interfejsu wiersza polecenia Zobacz miejsca do magazynowania emulatora wiersza polecenia narzędzia odwołań. Graficznego interfejsu użytkownika będzie w dalszym ciągu występować w wersji 3.0, ale można będą dostępne tylko wtedy, gdy Emulator obliczyć jest instalowany przez kliknięcie prawym przyciskiem myszy ikonę w obszarze powiadomień systemu i wybierając pozycję Pokaż UI emulatora miejsca do magazynowania.

- W wersji 2013-08-15 usług Azure miejsca do magazynowania jest teraz w pełni obsługiwane. (Wcześniej tej wersji została obsługiwany tylko przez Emulator przechowywania wersji 2.2.1 podglądu.)
