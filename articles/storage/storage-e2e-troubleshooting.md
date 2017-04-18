<properties
    pageTitle="Rozwiązywanie problemów przy użyciu metryki magazynowania Azure i rejestrowanie, AzCopy i Analizator wiadomości zakończenia do zakończenia | Microsoft Azure"
    description="Samouczek przedstawiający sposób zakończenia do końca Rozwiązywanie problemów z Azure analizy miejsca do magazynowania, AzCopy i Analizator wiadomości programu Microsoft"
    services="storage"
    documentationCenter="dotnet"
    authors="robinsh"
    manager="carmonm"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>


# <a name="end-to-end-troubleshooting-using-azure-storage-metrics-and-logging-azcopy-and-message-analyzer"></a>Rozwiązywanie problemów z końca do końca przy użyciu metryki magazynowania Azure i rejestrowanie, AzCopy i Analizator wiadomości

[AZURE.INCLUDE [storage-selector-portal-e2e-troubleshooting](../../includes/storage-selector-portal-e2e-troubleshooting.md)]

## <a name="overview"></a>Omówienie

Diagnozowanie i rozwiązywanie problemów jest kluczowe umiejętności do tworzenia i pomocniczych aplikacje klienckie z magazynem tabel platformy Microsoft Azure. Ze względu na Rozproszony charakter Azure aplikacji może być bardziej złożony niż w środowiskach tradycyjnych diagnozowanie i rozwiązywanie problemów: błędy i występują problemy z wydajnością.

W tym samouczku przedstawi możemy rozpoznawania klienta w przypadku niektórych błędów, które mogą mieć wpływ na wydajność i rozwiązywania tych problemów od końca do końca przy użyciu narzędzia udostępniane przez firmę Microsoft i miejscem do magazynowania Azure, aby zoptymalizować z aplikacją kliencką.

Ten samouczek zawiera praktycznych badań kompleksowe — scenariusz rozwiązywania problemów. Szczegółowo pojęć związanych z przewodnikiem do rozwiązywania problemów z aplikacji Azure przestrzeni dyskowej zobacz [Monitorowanie diagnozowanie i rozwiązywanie problemów z magazynem tabel platformy Microsoft Azure](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="tools-for-troubleshooting-azure-storage-applications"></a>Narzędzia do rozwiązywania problemów z aplikacjami magazyn Azure

Za pomocą kombinacji narzędzi ustalenie, kiedy wystąpił problem i co może być przyczyną tego problemu, rozwiązywać aplikacje klienckie przy użyciu programu Microsoft Azure magazynu. Do tych narzędzi należą:

- **Magazyn azure analizy**. [Analizy miejsca do magazynowania Azure](http://msdn.microsoft.com/library/azure/hh343270.aspx) udostępnia metryki i rejestrowanie magazyn Azure.
    - **Metryki magazynowania** śledzi metryki transakcji i pojemnością metryki dla Twojego konta miejsca do magazynowania. Używanie miar, można określić, jak aplikacja działa zgodnie z różnymi różnych środków. Aby uzyskać więcej informacji o typach metryki śledzona przez analizy miejsca do magazynowania, zobacz [Schematu tabeli metryki analizy miejsca do magazynowania](http://msdn.microsoft.com/library/azure/hh343264.aspx) .

    - **Rejestrowanie miejsca do magazynowania** rejestruje każdego żądania usługi Azure miejsca do magazynowania w dzienniku po stronie serwera. Dziennik śledzi szczegółowych danych dla każdego żądania, w tym operację, stan operacji i opóźnienie informacji. Aby uzyskać więcej informacji o dane i odpowiadania na wezwania, które są zapisywane pliki dziennika, analizy miejsca do magazynowania, zobacz [Format dziennika analizy miejsca do magazynowania](http://msdn.microsoft.com/library/azure/hh343259.aspx) .

> [AZURE.NOTE] Metryki lub funkcja rejestrowania włączone w tej chwili nie jest miejsca do magazynowania konta typu replikacji z zbędne strefy przestrzeni dyskowej (ZRS). 

- **Azure Portal**. Rejestrowanie i metryki można skonfigurować dla Twojego konta miejsca do magazynowania w [Azure Portal](https://portal.azure.com). Można także wyświetlić wykresy i schematy, w których są wyświetlane informacje dotyczące wydajności aplikacji w czasie i skonfigurować alerty powiadamiające o, gdy aplikacja wykonuje niezgodnie z oczekiwaniami dla określonej metryki.

    Informacje dotyczące konfigurowania monitorowanie Azure Portal, zobacz [Monitorowanie konto miejsca do magazynowania w Azure Portal](storage-monitor-storage-account.md) .

- **AzCopy**. Dzienniki serwera magazyn Azure są przechowywane jako obiektów blob, aby móc używać AzCopy kopiowania obiektów blob dziennika do katalogu lokalnego na potrzeby analizy za pomocą analizatora wiadomości programu Microsoft. Aby uzyskać więcej informacji na temat AzCopy, zobacz [Przenoszenie danych za pomocą narzędzia wiersza polecenia AzCopy](storage-use-azcopy.md) .

- **Analizatora wiadomości firmy Microsoft**. Analizatora wiadomości to narzędzie używa pliki dziennika i wyświetla dane dziennika w formacie wizualne, który ułatwia filtrowanie, wyszukiwanie i grupowanie danych dziennika w przydatne zestawy, które służą do analizowania błędów i występują problemy z wydajnością. Zobacz [Microsoft wiadomości analizatora operacyjnym Podręcznik](http://technet.microsoft.com/library/jj649776.aspx) uzyskać więcej informacji o analizatora wiadomości.

## <a name="about-the-sample-scenario"></a>Informacje o przykładowy scenariusz

Ten samouczek firma Microsoft będzie zbadać scenariusza, gdzie metryki magazynowania Azure wskazuje stawkę niskim sukcesu procent dla aplikacji, która wymaga Azure miejsca do magazynowania. Metryka stopa niskim sukcesu procentu (wyświetlane jako **PercentSuccess** w [Azure Portal](https://portal.azure.com) i w tabelach metryki) umożliwia śledzenie operacji, które kończą się pomyślnie, ale które zwracają kod stanu HTTP, która jest większa niż 299. W plikach dziennika magazynu po stronie serwera te operacje są rejestrowane ze stanem transakcji **ClientOtherErrors**. Aby uzyskać więcej informacji o metryki niski procent sukcesu zobacz [metryki Pokaż niskim PercentSuccess lub wpisy dziennika analizy mieć operacji z stan ClientOtherErrors](storage-monitoring-diagnosing-troubleshooting.md#metrics-show-low-percent-success).

Azure operacji magazynu może zwracać kody stanu HTTP większa niż 299 jako część ich działanie normalny. Jednak tych błędów, w niektórych przypadkach wskazują, że można zoptymalizować aplikacji klienckiej zwiększyć wydajność.

W tym scenariuszu teraz przyjrzymy się stawki niskim sukcesu procentu być cokolwiek poniżej 100%. Możesz jednak wybrać inny poziom metryki, zależnie od potrzeb. Zaleca się, że podczas testowania aplikacji, możesz ustanowić uszkodzenia według planu bazowego dla swojego kluczowe wskaźniki. Na przykład mogą określić na podstawie testów, że aplikacja ma stopień zgodne powodzenia procentu 90% lub 85%. Jeśli dane metryki wskazuje, że aplikacja jest różniących się od tego numeru, a następnie można sprawdzić, co może powodować wzrost.

Dla nasz przykładowy scenariusz po został określony, że metryka stopa procentowa sukcesu jest poniżej 100%, firma Microsoft będzie sprawdzić, czy dzienniki, aby znaleźć błędy, które odpowiada metryki i używać ich do wiadomo, co powoduje dolnym stopa procentowa sukcesu. Przyjrzymy specjalnie błędów w zakresie 400. Następnie możemy lepiej będzie badanie błędami 404 (nie można odnaleźć).

### <a name="some-causes-of-400-range-errors"></a>Niektóre przyczyny błędów w zakresie 400

Poniższych przykładach pokazano przykłady błędy 400 zakres dla żądań z magazynem obiektów Blob platformy Azure i ich możliwe przyczyny. Dowolny z tych błędów, a także błędów w zakresie 300 i 500 zakresu, może przyczynić się do stawki niski procent sukcesu.

Należy zauważyć, że z poniższej listy daleko od wykonania. Aby uzyskać szczegółowe informacje o ogólnych błędów magazyn Azure i o błędach specyficzne dla każdego z usługi Magazyn, zobacz [Stan i kody błędów](http://msdn.microsoft.com/library/azure/dd179382.aspx) w witrynie MSDN.

**Przykłady kodu 404 (nie można odnaleźć) stanu**

Występuje, gdy operacja odczytu przed kontenera lub obiektów blob kończy się niepowodzeniem, ponieważ nie odnaleziono obiektów blob lub kontener.

- Występuje, gdy kontener lub obiektów blob została usunięta przez innego klienta przed to żądanie.
- Występuje, jeśli korzystasz z połączenia interfejsu API, który tworzy kontener lub obiektów blob po sprawdzanie, czy istnieje. Interfejsy API CreateIfNotExists dzwonić głowy najpierw w celu sprawdzenia obecności kontenera lub obiektów blob; Jeśli nie istnieje, zwracany jest błąd 404, a następnie drugiego położenie rozmowy do pisania na kontenerze lub obiektów blob.

**Kod stanu 409 przykłady (konflikt)**

- Występuje, jeśli tworzenia nowego kontenera lub obiektów blob, bez Sprawdzanie istnienia najpierw za pomocą interfejsu API tworzenie i kontener lub obiektów blob o tej nazwie już istnieje.
- Występuje, gdy kontener jest usuwana, a następnie spróbuj utworzyć nowy kontener o takiej samej nazwie, przed zakończeniem operacji usuwania.
- Występuje, jeśli Określ dzierżawę na kontenerze lub obiektów blob, a istnieje już dzierżawę Prezentuj.

**Kod stanu 412 (nie powiodło się wstępny) przykłady**

- Występuje, gdy warunek określony przez nagłówek warunkowy nie zostały spełnione.
- Występuje, gdy określony identyfikator dzierżawy jest niezgodny z Identyfikatorem dzierżawy na zbiorniku lub obiektów blob.

## <a name="generate-log-files-for-analysis"></a>Generowanie pliki dziennika do analizy

W tym samouczku użyjemy analizatora wiadomości do pracy z trzy różne typy plików dziennika, mimo że można wybrać do pracy z dowolną z następujących:

- **Dziennik serwera**, który jest tworzony po włączeniu rejestrowania magazyn Azure. Dziennik serwera zawiera dane o każdorazowym nazywanych względem jednej z usługi Magazyn Azure — obiektów blob, kolejki tabeli i plik. Dziennik serwera wskazuje, która operacja był nazywany i jakie kodu stanu został zwrócony, a także inne szczegóły dotyczące żądania i odpowiedzi.
- **Dziennik klienta .NET**, który jest tworzony po włączeniu rejestrowania po stronie klienta z poziomu aplikacji .NET. Dziennik klienta zawiera szczegółowe informacje na temat sposobu klient przygotowanie wniosku i odbiera i przetwarza odpowiedź.
- **Dziennik śledzenia sieci HTTP**zbiera dane na protokołu HTTP/HTTPS i odpowiadania na wezwania danych, w tym dla operacji przed magazyn Azure. W tym samouczku możemy wygenerowany śledzenia sieci przy użyciu analizatora wiadomości.

### <a name="configure-server-side-logging-and-metrics"></a>Konfigurowanie rejestrowania po stronie serwera i miar

Najpierw zostanie należy skonfigurować rejestrowanie magazyn Azure i miar, tak, aby mamy danych z aplikacji klienckiej do analizy. Na różne sposoby — przez [Azure Portal](https://portal.azure.com), można skonfigurować rejestrowanie i wskaźniki przy użyciu programu PowerShell, lub programowo. Aby uzyskać szczegółowe informacje o konfigurowaniu rejestrowania i metryki, zobacz [Włączanie metryki magazynowania i przeglądania danych metryki](http://msdn.microsoft.com/library/azure/dn782843.aspx) i [Włączanie rejestrowania miejsca do magazynowania i uzyskiwanie dostępu do danych dziennika](http://msdn.microsoft.com/library/azure/dn782840.aspx) w witrynie MSDN.

**Za pośrednictwem portalu Azure**

Aby skonfigurować rejestrowanie i metryki dla Twojego konta miejsca do magazynowania za pomocą [Azure Portal](https://portal.azure.com), postępuj zgodnie z instrukcjami na [monitorze konto miejsca do magazynowania w Azure Portal](storage-monitor-storage-account.md).

> [AZURE.NOTE] Nie jest możliwe ustalenie minuta metryki przy użyciu Azure Portal. Jednak zaleca się ustawienie je na potrzeby tego samouczka i badanie problemów z wydajnością z aplikacją. Można ustawić minuta metryki przy użyciu programu PowerShell, tak jak pokazano poniżej lub programowo przy użyciu Biblioteka klienta miejsca do magazynowania.
>
> Należy zauważyć, że Azure Portal nie można wyświetlić metryki minuta, tylko godzinę metryki.

**Za pomocą programu PowerShell**

Aby rozpocząć pracę z programem PowerShell dla Azure, zobacz, [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).

1. Aby dodać konto użytkownika Azure do okna programu PowerShell, należy użyć polecenia cmdlet [AzureAccount Dodaj](http://msdn.microsoft.com/library/azure/dn722528.aspx) :

    ```
    Add-AzureAccount
    ```

2. W oknie **Logowanie się do platformy Microsoft Azure** wpisz adres e-mail i hasło skojarzone z kontem. Azure uwierzytelnia i zapisuje informacje poświadczeń, a następnie zamyka okno.
3. Ustawianie domyślnego magazynu konta do konta przestrzeni dyskowej, którego używasz samouczka, wykonując te polecenia w oknie programu PowerShell:

    ```
    $SubscriptionName = 'Your subscription name'
    $StorageAccountName = 'yourstorageaccount'
    Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName
    ```

4. Włącz rejestrowanie magazyn obiektów Blob usługi:

    ```
    Set-AzureStorageServiceLoggingProperty -ServiceType Blob -LoggingOperations Read,Write,Delete -PassThru -RetentionDays 7 -Version 1.0
    ```
5. Włączanie metryki magazynowania w usłudze obiektów Blob, pamiętając ustawić **- MetricsType** `Minute`:

    ```
    Set-AzureStorageServiceMetricsProperty -ServiceType Blob -MetricsType Minute -MetricsLevel ServiceAndApi -PassThru -RetentionDays 7 -Version 1.0
    ```

### <a name="configure-net-client-side-logging"></a>Konfigurowanie rejestrowania po stronie klienta .NET

Aby skonfigurować rejestrowanie po stronie klienta dla aplikacji .NET, należy włączyć diagnostyki .NET w pliku konfiguracji aplikacji (web.config lub app.config). Aby uzyskać szczegółowe informacje, zobacz [rejestrowania po stronie klienta z biblioteką klienta .NET miejsca do magazynowania](http://msdn.microsoft.com/library/azure/dn782839.aspx) i [klienta rejestrowania z SDK Microsoft Azure miejsca do magazynowania dla języka Java](http://msdn.microsoft.com/library/azure/dn782844.aspx) w witrynie MSDN.

Dziennik po stronie klienta zawiera szczegółowe informacje na temat sposobu klient przygotowanie wniosku i odbiera i przetwarza odpowiedź.

Biblioteka klienta miejsca do magazynowania są przechowywane dane dziennika po stronie klienta w lokalizacji określonej w pliku konfiguracji aplikacji (web.config lub app.config).

### <a name="collect-a-network-trace"></a>Zbieranie śledzenia sieci

Za pomocą analizatora wiadomości do zbierania śledzenia sieci protokołu HTTP/HTTPS jest uruchomiona aplikacja klienta. Wiadomości przy użyciu analizatora [Fiddler](http://www.telerik.com/fiddler) na końcu Wstecz. Przed zbieranie śledzenia sieci, zaleca się skonfigurowanie Fiddler nagrywać niezaszyfrowanym ruchu HTTPS:

1. Zainstaluj [Fiddler](http://www.telerik.com/download/fiddler).
2. Uruchom program Fiddler.
2. Wybierz pozycję narzędzia **| Opcje fiddler**.
3. W oknie dialogowym Opcje upewnij się, że **Połączenie HTTPS Przechwytywanie** i **Odszyfrowywanie ruchu HTTPS** są zaznaczone, tak jak pokazano poniżej.

![Konfigurowanie opcji Fiddler](./media/storage-e2e-troubleshooting/fiddler-options-1.png)

Samouczek zbieranie najpierw zapisać śledzenia sieci w analizatorze wiadomości, a następnie utworzyć sesji analizy, aby przeprowadzić analizę śledzenia i dzienniki. Zbieranie śledzenia sieci w analizatorze wiadomości:

1. W analizatorze wiadomości, wybierz pozycję **pliku | Szybkie śledzenia | Bez szyfrowania HTTPS**.
2. Śledzenie rozpocznie się natychmiast. Wybierz pozycję **Zatrzymaj** , aby zatrzymać śledzenie, dzięki czemu firma Microsoft go skonfigurować do śledzenia ruchu miejsca do magazynowania.
3. Wybierz pozycję **Edytuj** , aby edytować sesji śledzenia.
4. Wybierz łącze **Konfiguruj** po prawej stronie dostawcy ETW **Firmy Microsoft — Pef-WebProxy** .
5. W oknie dialogowym **Ustawienia zaawansowane** kliknij kartę **dostawcy** .
6. W polu **Nazwa hosta filtr** Określ punktów końcowych miejsca do magazynowania, oddzielonych spacjami. Na przykład można określić punktów końcowych w następujący sposób; Zmienianie `storagesample` nazwę konta magazynu:

    ```
    storagesample.blob.core.windows.net storagesample.queue.core.windows.net storagesample.table.core.windows.net
    ```

7. Zamknij okno dialogowe, a następnie kliknij przycisk **Uruchom ponownie** zacząć zbieranie śledzenie przy użyciu filtru hostname, w miejscu, tak aby tylko ruch sieciowy magazyn Azure znajduje się w wynikach śledzenia.

>[AZURE.NOTE] Po zakończeniu pobierania śledzenia sieci zdecydowanie zaleca się przywrócenie ustawień, które zmieniono w Fiddler ruchu HTTPS odszyfrowywanie. W oknie dialogowym Opcje Fiddler Usuń zaznaczenie pola wyboru **Przechwytywanie połączenie HTTPS** i **Odszyfrowywanie ruchu HTTPS** .

Zobacz [Korzystanie z funkcji śledzenia sieci](http://technet.microsoft.com/library/jj674819.aspx) w witrynie Technet, aby uzyskać więcej informacji.

## <a name="review-metrics-data-in-the-azure-portal"></a>Przeglądanie danych metryki w Azure Portal

Po upływie pewnego czasu działaniu aplikacji, możesz przejrzeć wykresy metryk, które są wyświetlane w [Azure Portal](https://portal.azure.com) obserwować informacje zostały dotyczące wydajności usługi. Najpierw przejdź do swojego konta miejsca do magazynowania w Azure Portal i Dodawanie wykresu metryki **Procent sukcesu** .

W portalu Azure teraz pojawi się **Procent sukcesu** na wykresie monitorowania, razem z innymi metryki, które zostały dodane. W tego scenariusza firma Microsoft będzie badanie dalej analizując dzienniki w analizatorze wiadomości, stopa procentowa sukcesu jest nieco poniżej 100%.

Aby uzyskać więcej szczegółowych informacji na temat dodawania metryki na stronę monitorowania, zobacz [jak: Dodawanie metryki do tabeli metryki](storage-monitor-storage-account.md#how-to-add-metrics-to-the-metrics-table).

> [AZURE.NOTE] Może upłynąć trochę czasu, są wyświetlane w Azure Portal po włączeniu metryki magazynowania danych metryki. Jest to spowodowane co godzinę metryki dla poprzedniej godziny nie są wyświetlane w Azure Portal przed upływem bieżącej godziny. Ponadto minuta metryki nie są obecnie wyświetlane w Azure Portal. Tak w zależności od po włączeniu metryki, może potrwać do dwóch godzin Zobacz metryki danych.

## <a name="use-azcopy-to-copy-server-logs-to-a-local-directory"></a>Skopiuj dzienniki serwera do katalogu lokalnego przy użyciu AzCopy

Azure magazynowania zapisuje dane dzienników serwera obiektów blob, gdy metryki są zapisywane w tabelach. Blob dziennika są dostępne w dobrze znane `$logs` kontenera dla Twojego konta miejsca do magazynowania. Dziennik BLOB są nazywane hierarchicznie za rok, miesiąc, dzień i godzinę, tak, aby łatwo można znaleźć przedział czasu, który chcesz sprawdzić. Na przykład w `storagesample` konto, kontenera dla obiektów blob dziennika dla 01-02-2015 z 8-9 am, jest `https://storagesample.blob.core.windows.net/$logs/blob/2015/01/08/0800`. Poszczególnych obiektów blob w tym kontenerze są nazywane kolejno począwszy od `000000.log`.

Aby pobrać te pliki dziennika po stronie serwera na wybraną lokalizację na komputerze lokalnym, można użyć narzędzia wiersza AzCopy. Na przykład następujące polecenie umożliwia pobieranie plików dziennika dla operacji obiektów blob, które miały miejsce 2 stycznia 2015 do folderu `C:\Temp\Logs\Server`; zamienianie `<storageaccountname>` z nazwą swojego konta miejsca do magazynowania i `<storageaccountkey>` przy użyciu klucza dostępu do konta:

    AzCopy.exe /Source:http://<storageaccountname>.blob.core.windows.net/$logs /Dest:C:\Temp\Logs\Server /Pattern:"blob/2015/01/02" /SourceKey:<storageaccountkey> /S /V

AzCopy jest dostępna do pobrania na stronie [Pobierania Azure](https://azure.microsoft.com/downloads/) . Aby uzyskać szczegółowe informacje o korzystaniu z AzCopy zobacz [Przenoszenie danych za pomocą narzędzia wiersza polecenia AzCopy](storage-use-azcopy.md).

Aby uzyskać dodatkowe informacje na temat pobierania dzienniki po stronie serwera zobacz [Pobieranie rejestrowania miejsca do magazynowania danych dziennika](http://msdn.microsoft.com/library/azure/dn782840.aspx#DownloadingStorageLogginglogdata).

## <a name="use-microsoft-message-analyzer-to-analyze-log-data"></a>Analizowanie danych dziennika przy użyciu analizatora wiadomości firmy Microsoft

Microsoft wiadomości Analyzer to narzędzie do przechwytywania, wyświetlania i analizowania protokołu wiadomości ruchu, zdarzeń i inne wiadomości systemu lub aplikacji w scenariusze dotyczące rozwiązywania problemów i diagnostyczne. Analizatora wiadomości również umożliwia ładowanie, agregowanie i analizowania danych z dziennika i zapisywania plików śledzenia. Aby uzyskać więcej informacji na temat Analyzer wiadomości zobacz [Przewodnik Microsoft wiadomości analizatora operacyjnym po](http://technet.microsoft.com/library/jj649776.aspx).

Analizatora wiadomość zawiera elementy zawartości do przechowywania Azure, które ułatwiają analizowanie dzienników sieci, klienta i serwera. W tej sekcji omówiono w sposób używania tych narzędzi Aby rozwiązać problem niskiej procent sukcesu w dziennikach miejsca do magazynowania.

### <a name="download-and-install-message-analyzer-and-the-azure-storage-assets"></a>Pobieranie i instalowanie analizatora wiadomości i zasoby magazynowania Azure

1. Pobrać [Analizatora wiadomość](http://www.microsoft.com/download/details.aspx?id=44226) z programu Microsoft Download Center, a następnie uruchom Instalatora.
2. Uruchom analizatora wiadomości.
3. W menu **Narzędzia** wybierz pozycję **Menedżer zasobów**. W oknie dialogowym **Menedżer zasobów** zaznacz **pliki do pobrania**, a następnie filtrowanie ilość miejsca do magazynowania **Azure**. Zostanie wyświetlona Azure aktywów miejsca do magazynowania, jak pokazano na poniższym obrazie.
4. Kliknij przycisk **Synchronizuj wszystkie elementy wyświetlane** zainstalować składniki majątku Azure miejsca do magazynowania. Dostępne aktywa obejmują:
    - **Reguły kolor azure miejsca do magazynowania:** Azure reguły kolor miejsca do magazynowania umożliwiają definiowanie filtrów specjalnych, które umożliwia wyróżnianie wiadomości, które zawierają określone informacje w ramach śledzenia koloru, tekstu i stylów czcionek.
    - **Wykresy azure miejsca do magazynowania:** Azure wykresy przestrzeni dyskowej są wstępnie zdefiniowanych wykresy, które graficznego przedstawiania danych dziennika serwera. Pamiętaj, aby użyć wykresy magazyn Azure w tej chwili, może być tylko ładowanie dziennik serwera na siatkę analizy.
    - **Parsery azure miejsca do magazynowania:** Parsery magazyn Azure przeanalizować magazyn Azure klienta, serwer i dzienniki HTTP, aby je wyświetlić w siatce analizy.
    - **Filtrów azure miejsca do magazynowania:** Azure filtry przestrzeni dyskowej są wstępnie zdefiniowane kryteria, które umożliwia wyszukiwanie w siatce analizy danych.
    - **Układy widoku azure miejsca do magazynowania:** Azure układy widoku miejsca do magazynowania są układy kolumn wstępnie zdefiniowane i grupowanie w siatce analizy.
4. Po zainstalowaniu składniki majątku, należy ponownie uruchomić analizatora wiadomości.

![Menedżer zasobów analizatora wiadomości](./media/storage-e2e-troubleshooting/mma-start-page-1.png)

> [AZURE.NOTE] Zainstaluj wszystkie zasoby magazyn Azure wyświetlane na potrzeby tego samouczka.

### <a name="import-your-log-files-into-message-analyzer"></a>Importowanie plików dziennika do analizowania wiadomości

Wszystkie pliki dziennika zapisanych (po stronie serwera, po stronie klienta i sieci) można zaimportować do jednej sesji w analizatorze wiadomości Microsoft na potrzeby analizy.

1. W menu **plik** w programie Microsoft Analyzer wiadomości kliknij **Nową sesję**, a następnie kliknij **Pusty sesji**. W oknie dialogowym **Nowa sesja** wprowadź nazwę sesji analizy. W okienku **Szczegółów sesji** kliknij przycisk **pliki** .
1. Aby załadować dane śledzenia sieci wygenerowane przez analizatora wiadomości, wybierz polecenie **Dodaj pliki**, przejdź do lokalizacji, w której zapisano plik .matp w sesji śledzenia sieci web, wybierz plik .matp i kliknij przycisk **Otwórz**.
1. Aby załadować dane dziennika po stronie serwera, wybierz polecenie **Dodaj pliki**, przejdź do lokalizacji, w której został pobrany dzienniki po stronie serwera, zaznacz pliki dziennika zakres czasu, który chcesz przeanalizować i kliknij przycisk **Otwórz**. Następnie w okienku **Szczegółów sesji** ustaw **Konfigurację dziennika tekstu** listy rozwijanej dla każdego pliku dziennika po stronie serwera **AzureStorageLog** , aby upewnić się, że analizatora wiadomości Microsoft można analizować poprawnie pliku dziennika.
1. Aby załadować dane dziennika po stronie klienta, wybierz polecenie **Dodaj pliki**, przejdź do lokalizacji, w której zapisano dzienniki po stronie klienta, zaznacz pliki dziennika, które mają być analizowane i kliknij przycisk **Otwórz**. Następnie w okienku **Szczegółów sesji** ustawienie **Konfiguracji dziennika tekstu** listy rozwijanej dla każdego pliku dziennika po stronie klienta do **AzureStorageClientDotNetV4** , aby upewnić się, że analizatora wiadomości Microsoft można analizować poprawnie pliku dziennika.
1. Kliknij przycisk **Uruchom** okno dialogowe **Nowej sesji** na ładowanie i analizowanie danych dziennika. Dane dziennika są wyświetlane w siatce analizy analizatora wiadomości.

Na poniższym obrazie przedstawiono sesję przykład skonfigurowany z serwera, klientów i plików dziennika śledzenia sieci.

![Konfigurowanie sesji analizatora wiadomości](./media/storage-e2e-troubleshooting/configure-mma-session-1.png)

Należy zauważyć, że wiadomość analizatora ładuje pliki dziennika do pamięci. Jeśli masz duże zestawu danych dziennika, należy filtrowania, aby można było uzyskiwać najlepszą wydajność z analizatora wiadomości.

Najpierw należy określić przedział czasu, który Cię interesuje przeglądanie i ramce czasu możliwie najmniejszy. W większości przypadków chcesz przejrzeć w okresie minut lub godzin co najwyżej. Importowanie najmniejszego zestawu dzienników, które mogą nie spełnia określonych wymagań.

Jeśli nadal masz dużej ilości danych dziennika, może być Określ sesji pozwalający filtrować dane dziennika, zanim będzie można go załadować. W oknie dialogowym **Filtr sesji** wybierz przycisk **biblioteki** , aby wybrać wstępnie zdefiniowany filtr; na przykład wybrać **globalnej czasu filtr I** filtry magazyn Azure do filtru na przedział czasu. Następnie można edytować kryteriów filtru w celu rozpoczęcia i zakończenia sygnatura czasowa interwału, które mają być wyświetlane. Można również filtrować według określony kod stanu; na przykład można załadować tylko wpisy dziennika umożliwiającej kodu stanu 404.

Aby uzyskać więcej informacji na temat importowania danych dziennika do analizowania wiadomości firmy Microsoft zobacz [Pobieranie danych wiadomości](http://technet.microsoft.com/library/dn772437.aspx) w witrynie TechNet.

### <a name="use-the-client-request-id-to-correlate-log-file-data"></a>Za pomocą Identyfikatora żądania klienta przeniesionym dane pliku dziennika

Biblioteka klienta magazynowania Azure umożliwia automatyczne generowanie Identyfikatora żądania klienta unikatowe dla każdego żądania. Tej wartości są zapisywane w dzienniku klienta, dziennik serwera i śledzenia sieci, więc służy do grupowania danych przez wszystkie trzy dzienniki w analizatora wiadomości. Zobacz [identyfikator żądania klienta](storage-monitoring-diagnosing-troubleshooting.md#client-request-id) dodatkowe informacje dotyczące żądania klienta identyfikator.

Poniższych sekcjach opisano, jak dostosować za pomocą widoków wstępnie skonfigurowane i niestandardowego układu i grupowanie danych według identyfikatora klienta wezwanie.

### <a name="select-a-view-layout-to-display-in-the-analysis-grid"></a>Wybierz układ widoku, aby wyświetlić w siatce analizy

Składniki majątku miejsca do magazynowania dla analizatora wiadomości obejmują Azure miejsca do magazynowania widoku układy, które są wstępnie skonfigurowane widoków, które służy do wyświetlania danych za pomocą przydatnych grupowania i kolumn w różnych scenariuszach. Można również utworzyć widok niestandardowy układów i zapisywanie ich do późniejszego użycia.

Na poniższym obrazie przedstawiono **Widoku Układ** menu dostępne, wybierając **Widok Układ** na wstążce paska narzędzi. Układy widok do przechowywania Azure są zgrupowane w menu węźle **Magazyn Azure** . Możesz wyszukiwać `Azure Storage` w polu wyszukiwania, aby odfiltrować magazyn Azure wyświetlanie tylko układy. Można też zaznaczyć gwiazdkę obok widoku Układ mu Ulubione, aby ją wyświetlić w górnej części menu.

![Menu Widok układu](./media/storage-e2e-troubleshooting/view-layout-menu.png)

Najpierw wybierz **grupowanego ClientRequestID i w Module**. Ten widok układu grup najpierw zaloguj dane z wszystkich trzech dzienników, identyfikator klienta, a następnie przez źródło pliku dziennika (lub **modułu** w analizatorze wiadomości). Z tego widoku możesz przechodzić do identyfikator żądania określonego klienta, a Zobacz danych z wszystkich plików dziennika trzy dla tego żądania klienta identyfikator.

Obraz wymieniono ten widok układu zastosowany do przykładowych danych dziennika z podzbiorem wyświetlane kolumny. Widać, że identyfikatora żądania określonego klienta analizy są wyświetlane w siatce danych z dziennika klienta, dziennik serwera i śledzenia sieci.

![Azure magazynowania widoku układu](./media/storage-e2e-troubleshooting/view-layout-client-request-id-module.png)

>[AZURE.NOTE] Pliki dziennika różnych mają różne kolumny, aby po wyświetleniu w siatce analizy danych z wielu plików dziennika kilka kolumn nie może zawierać dowolne dane dla danego wiersza. Na przykład na powyższym obrazie wierszy dziennika klienta nie są wyświetlane wszystkie dane **sygnatury czasowej**, **TimeElapsed**, **źródła**i **miejsca docelowego** kolumny, ponieważ tych kolumn są niedostępne w dzienniku klienta, ale istnieje w wynikach śledzenia sieci. Podobnie kolumny **sygnatury czasowej** są wyświetlane dane sygnatury czasowej w dzienniku serwera, ale bez danych jest wyświetlany w przypadku kolumn **TimeElapsed**, **źródłowej**i **docelowej** , które nie są częścią dziennik serwera.

Oprócz korzystania z układów widoku magazyn Azure, można również definiować i zapisywanie układach widoku. Można wybierz inne żądane pola do grupowania danych i zapisać grupowanie jako część także układu niestandardowego.

### <a name="apply-color-rules-to-the-analysis-grid"></a>Stosowanie reguł koloru do siatki analizy

Składniki majątku miejsca do magazynowania także kolor reguł, które oferują wizualną oznacza, że do identyfikowania różnych typów błędów w siatce analizy. Reguły wstępnie zdefiniowane schematy kolorów dotyczą błędy HTTP, aby były widoczne tylko w przypadku śledzenia serwera w dzienniku i sieci.

Aby zastosować kolor reguł, wybierz pozycję **Kolor** na wstążce paska narzędzi. Zobaczysz reguły kolor magazyn Azure w menu. Samouczek wybierz **Klienta błędy (StatusCode między 400 i 499)**, jak pokazano na poniższym obrazie.

![Azure magazynowania widoku układu](./media/storage-e2e-troubleshooting/color-rules-menu.png)

Oprócz korzystania z regułami kolor magazyn Azure, można też definiować i zapisać reguły koloru.

### <a name="group-and-filter-log-data-to-find-400-range-errors"></a>Znajdowanie błędów w zakresie 400 grupy i filtrowanie danych dziennika

Następnie firma Microsoft będzie grupowania i filtrowania danych dziennika, aby znaleźć wszystkie błędy w zakresie 400.

1. Znajdź kolumnę **StatusCode** w siatce analizy, kliknij prawym przyciskiem myszy nagłówek kolumny i wybierz **grupę**.
2. Grupa dalej w kolumnie **ClientRequestId** . Pojawi się, że dane w siatce analizy jest teraz zorganizowane według kodu stanu oraz identyfikatora klienta wezwanie.
1. Wyświetlanie okna narzędzia filtr widoku, jeśli nie jest wyświetlane. Na wstążce paska narzędzi wybierz pozycję **Narzędzia Windows**następnie **Filtr widoku**.
2. Aby filtrować dane dziennika, aby wyświetlić tylko zakres 400 błędy, dodać następujących kryteriów filtrowania do okna **Filtru widoku** , a następnie kliknij przycisk **Zastosuj**:

        (AzureStorageLog.StatusCode >= 400 && AzureStorageLog.StatusCode <=499) || (HTTP.StatusCode >= 400 && HTTP.StatusCode <= 499)

Na poniższym obrazie pokazano wyniki grupowania i filtrowania. Rozwijanie pola **ClientRequestID** poniżej grupowania dla kodu stanu 409, na przykład pokazuje operacji, które spowodowały kodu stanu.

![Azure magazynowania widoku układu](./media/storage-e2e-troubleshooting/400-range-errors1.png)

Po zastosowaniu tego filtru, zobaczysz, że wiersze z dziennika klienta są wyłączone, jako klient dziennika nie zawiera kolumny **StatusCode** . Przede wszystkim firma Microsoft może przejrzeć serwera i dzienniki śledzenia sieci zlokalizować błędami 404, a następnie będzie możemy powrócić w dzienniku klienta przyjrzenie się operacje klienckie, które doprowadziły do nich.

>[AZURE.NOTE] Można filtrować według kolumny **StatusCode** i nadal wyświetlanie danych z wszystkich trzech dzienniki, w tym dzienniku klienta, dodając wyrażenia filtru, który zawiera pozycje dziennika, gdzie kodu stanu jest null. Aby utworzyć wyrażenie filtru, użyj:
>
> <code>&#42;StatusCode >= 400 or !&#42;StatusCode</code>
>
> Ten filtr zwraca wszystkie wiersze w kliencie dziennika i tylko wiersze z dziennika serwera i dziennika HTTP których kodu stanu jest większa niż 400. Jeśli zostanie zastosowany do układu widoku pogrupowane według identyfikator żądania klienta i moduł, możesz wyszukiwać lub przewiń do pozycji dziennika, aby znaleźć te, gdzie znajdują się wszystkie trzy dzienniki.   

### <a name="filter-log-data-to-find-404-errors"></a>Filtrowanie danych dziennika, aby znaleźć błędami 404

Aktywa miejsca do magazynowania obejmują wstępnie zdefiniowanych filtrów, które można wykorzystać do zawężenia danych dziennika, aby znaleźć błędy lub trendów, którego szukasz. Następnie będzie stosowanie dwóch wstępnie zdefiniowanych filtrów: te filtry serwera i sieci dzienników błędów 404 oraz takie, które umożliwia filtrowanie danych w zakresie określonym czasie.

1. Wyświetlanie okna narzędzia filtr widoku, jeśli nie jest wyświetlane. Na wstążce paska narzędzi wybierz pozycję **Narzędzia Windows**następnie **Filtr widoku**.
2. W oknie Filtr widoku, wybierz **bibliotekę**, a wyszukać `Azure Storage` Aby znaleźć magazyn Azure filtry. Wybierz filtr dla **404 wiadomości (nie odnaleziono) we wszystkich dziennikach**.
3. Wyświetlanie menu **biblioteki** ponownie i Znajdź i zaznacz **Globalny filtru czasu**.
4. Edytowanie sygnatury czasowe wyświetlane w filtrze do zakresu, który chcesz wyświetlić. Dzięki temu można zawęzić zakres danych, aby przeanalizować.
5. Filtr powinien zostać wyświetlony podobnie jak w poniższym przykładzie. Kliknij przycisk **Zastosuj** , aby zastosować filtr do siatki analizy.

        ((AzureStorageLog.StatusCode == 404 || HTTP.StatusCode == 404)) And
        (#Timestamp >= 2014-10-20T16:36:38 and #Timestamp <= 2014-10-20T16:36:39)

![Azure magazynowania widoku układu](./media/storage-e2e-troubleshooting/404-filtered-errors1.png)

### <a name="analyze-your-log-data"></a>Analizowanie danych dziennika

Teraz, gdy masz zgrupowane i filtrować dane, możesz przejrzeć szczegóły poszczególnych wniosków wygenerowanych błędami 404. W układzie widoku bieżącego dane zostaną zgrupowane według identyfikatorów żądania klienta, a następnie według źródła dziennika. Ponieważ firma Microsoft filtrujesz żądania których pole StatusCode zawiera 404, poznamy tylko serwer i dane śledzenia sieci, nie danych dziennika klienta.

Na poniższym obrazie przedstawiono żądanie określone miejsce, w którym operacji uzyskiwanie obiektów Blob uzyskane 404, ponieważ to nie istnieje. Należy zauważyć, że niektóre kolumny zostały usunięte w widoku standardowym w celu wyświetlenia odpowiednich danych.

![Filtrowane serwera i dzienniki śledzenia sieci](./media/storage-e2e-troubleshooting/server-filtered-404-error.png)

Firma Microsoft będzie następnie dostosować ten identyfikator klienta z danymi dziennika klienta, aby wyświetlić akcje miał klienta, gdy wystąpił błąd. Nowy widok siatki analizy dla tej sesji w celu wyświetlenia danych dziennika klienta, który jest otwierany w drugiej karcie można wyświetlać:

1. Najpierw skopiować wartość do pola **ClientRequestId** do Schowka. Możesz to zrobić, zaznaczając albo wiersza, lokalizowanie pole **ClientRequestId** , kliknięcie prawym przyciskiem myszy wartość danych i wybierając **Kopiuj "ClientRequestId"**.
1. Na wstążce paska narzędzi wybierz pozycję **Nowa przeglądarka**, a następnie wybierz **Analizy siatki** , aby otworzyć nową kartę. Nowa karta zawiera wszystkie dane w plikach dziennika bez grupowania, filtrowania i reguł koloru.
2. Na wstążce paska narzędzi wybierz pozycję **Widok układu**, a następnie zaznacz **Wszystkie kolumny klienta .NET** w sekcji **Magazyn Azure** . Ten układ widoku są wyświetlane dane w kliencie dziennika, a także dzienniki śledzenia serwera i sieci. Domyślnie sortowane według kolumny **MessageNumber** .
3. Następnie wyszukaj dziennika klienta identyfikatora klienta wezwanie. Na wstążce paska narzędzi wybierz pozycję **Znajdź wiadomości**, a następnie określ filtru niestandardowego na identyfikator żądania klienta w polu **Znajdź** . Filtru, określając własnego Identyfikatora żądania klienta, należy użyć następującej składni:

        *ClientRequestId == "398bac41-7725-484b-8a69-2a9e48fc669a"

Analizatora wiadomości znajduje i zaznacza pierwszy wpis dziennika, jeśli kryteria wyszukiwania odpowiada identyfikator klienta wezwanie. W dzienniku klienta istnieje kilka wpisów dla każdego Identyfikatora żądania klienta, więc możesz grupować w polu **ClientRequestId** , aby ułatwić ich jednocześnie wyświetlić. Na poniższym obrazie przedstawiono wszystkich wiadomości w dzienniku klienta dla określonego klienta identyfikatora żądania.

![Wyświetlanie dziennika klienta błędami 404](./media/storage-e2e-troubleshooting/client-log-analysis-grid1.png)

Dane wyświetlane w układach widoku w tych dwóch kart można analizować dane żądania do określenia, co powoduje błąd. Można także przeglądać żądania, których poprzedzane to wystąpienie, aby zobaczyć, jeśli poprzedniego wydarzenia może doprowadzić do błąd 404. Na przykład możesz przejrzeć wpisy dziennika klienta poprzedzającego ten identyfikator żądania klienta do określenia, czy to zostały usunięte lub z powodu z aplikacją kliencką wywołanie interfejs API CreateIfNotExists w kontenerze lub obiektów blob wystąpił błąd. W dzienniku klienta możesz znaleźć obiektów blob adres w polu **Opis** . serwer i dzienniki śledzenia sieci te informacje są wyświetlane w polu **Podsumowanie** .

Znając adres blob, w którym uzyskane błąd 404, możesz przejrzeć dalej. W przypadku wyszukiwania pozycje dziennika inne wiadomości skojarzone z operacjami wykonywanymi na tym samym obiektów blob, można sprawdzić, czy klient usunięty wcześniej jednostki.

## <a name="analyze-other-types-of-storage-errors"></a>Analizowanie innych typów błędów miejsca do magazynowania

Teraz, gdy znasz przy użyciu analizatora wiadomości do analizowania danych dziennika, można analizować innych typów błędów przy użyciu widoku układy, reguły koloru i wyszukiwanie i filtrowanie. W poniższej tabeli przedstawiono niektóre problemy, które mogą wystąpić i kryteria filtrowania, których można użyć, aby zlokalizować je. Uzyskać więcej informacji dotyczących tworzenia filtrów i analizowania wiadomości filtrowania języka zobacz [Filtrowanie danych wiadomości](http://technet.microsoft.com/library/jj819365.aspx).

|    Aby zbadać...                                                                                               |    Użyj wyrażenia filtru...                                                                                                                                                                                                                                        |    Wyrażenie dotyczy dziennika (klienta, serwer sieci, wszystkie)    |
|------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------|
|    Nieoczekiwane opóźnienia w dostarczenia wiadomości w kolejce                                                              |    AzureStorageClientDotNetV4.Description zawiera "Ponawianie nie powiodło się operacji."                                                                                                                                                                                |    Klienta                                                        |
|    HTTP wzrost PercentThrottlingError                                                                       |    HTTP. Response.StatusCode == 500 & #124; & #124; HTTP. Response.StatusCode == 503                                                                                                                                                                                          |    Sieci                                                       |
|    Wzrost PercentTimeoutError                                                                               |    HTTP. Response.StatusCode == 500                                                                                                                                                                                                                             |    Sieci                                                       |
|    Wzrost PercentTimeoutError (wszystkie)                                                                         |    * StatusCode == 500                                                                                                                                                                                                                                          |    Wszystkie                                                           |
|    Wzrost PercentNetworkError                                                                               |    AzureStorageClientDotNetV4.EventLogEntry.Level < 2                                                                                                                                                                                                          |    Klienta                                                        |
|    HTTP 403 (Dostęp zabroniony) wiadomości                                                                                 |    HTTP. Response.StatusCode == 403                                                                                                                                                                                                                             |    Sieci                                                       |
|    HTTP 404 (nie znaleziono) wiadomości                                                                                 |    HTTP. Response.StatusCode == 404                                                                                                                                                                                                                             |    Sieci                                                       |
|    404 (wszystkie)                                                                                                     |    * StatusCode == 404                                                                                                                                                                                                                                          |    Wszystkie                                                           |
|    Udostępnione problem autoryzacji podpisu programu Access (SA)                                                             |    AzureStorageLog.RequestStatus == "SASAuthorizationError"                                                                                                                                                                                                     |    Sieci                                                       |
|    HTTP 409 (konflikt) wiadomości                                                                                  |    HTTP. Response.StatusCode == 409                                                                                                                                                                                                                             |    Sieci                                                       |
|    409 (wszystkie)                                                                                                     |    * StatusCode == 409                                                                                                                                                                                                                                          |    Wszystkie                                                           |
|    Operacje ze stanem transakcji ClientOtherErrors mają PercentSuccess min. lub analizy wpisy dziennika    |    AzureStorageLog.RequestStatus == "ClientOtherError"                                                                                                                                                                                                         |    Serwer                                                        |
|    Ostrzeżenie o Nagle'a                                                                                               |    ((AzureStorageLog.EndToEndLatencyMS-AzureStorageLog.ServerLatencyMS) > (AzureStorageLog.ServerLatencyMS * 1.5)) i (AzureStorageLog.RequestPacketSize < 1460) i (AzureStorageLog.EndToEndLatencyMS - AzureStorageLog.ServerLatencyMS > = 200)        |    Serwer                                                        |
|    Przedział czasu w dziennikach serwera i sieci                                                                    |    #Sygnatura czasowa > = 2014-10-20T16:36:38 i #Timestamp < = 2014-10-20T16:36:39                                                                                                                                                                                     |    Serwer, sieci                                               |
|    Przedział czasu w dziennikach serwera                                                                                |    AzureStorageLog.Timestamp > = 2014-10-20T16:36:38 i AzureStorageLog.Timestamp < = 2014-10-20T16:36:39                                                                                                                                                     |    Serwer                                                        |


## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat rozwiązywania problemów scenariusze zakończenia do końca w magazynie Azure, zobacz następujące zasoby:

- [Monitorowanie, diagnozowanie i rozwiązywanie problemów z magazynem tabel platformy Microsoft Azure](storage-monitoring-diagnosing-troubleshooting.md)
- [Analizy miejsca do magazynowania](http://msdn.microsoft.com/library/azure/hh343270.aspx)
- [Monitorowanie konto przestrzeni dyskowej w Azure Portal](storage-monitor-storage-account.md)
- [Przesyłanie danych za pomocą narzędzia wiersza polecenia AzCopy](storage-use-azcopy.md)
- [Przewodnik operacyjnym analizatora wiadomości firmy Microsoft](http://technet.microsoft.com/library/jj649776.aspx)
