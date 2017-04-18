<properties 
    pageTitle="Przenoszenie danych między bazami danych w chmurze skalowanej | Microsoft Azure" 
    description="Wyjaśniono, jak do manipulowania odłamki i przenoszenie danych za pośrednictwem usługi siebie za pomocą elastyczne API bazy danych." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove" />

# <a name="moving-data-between-scaled-out-cloud-databases"></a>Przenoszenie danych między bazami danych w chmurze skalowanej

Jeśli jest oprogramowania Projektant usługi i nieoczekiwanie aplikacji podlega imponujące żądanie, należy tak zezwalały wzrostu. Dlatego możesz dodać więcej baz danych (odłamki). Jak rozpowszechniać danych do nowych baz danych bez przerywania integralności danych **Narzędzie do korespondencji seryjnej podziału** umożliwia przenoszenie danych z ograniczeniami baz danych do nowych baz danych.  

Narzędzie korespondencji seryjnej podziału działa jako usługa Azure sieci web. Administrator lub deweloper korzysta z narzędzia przenoszenie shardlets (dane z shard) między różnych baz danych (odłamki). Narzędzie używa shard mapy zarządzania, aby zachować bazy danych metadanych usługi i upewnij się, spójne mapowania.

![Omówienie][1]

## <a name="download"></a>Plik do pobrania
[Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/)


## <a name="documentation"></a>Dokumentacja
1. [Samouczek narzędzie elastyczne bazy danych korespondencji seryjnej podziału](sql-database-elastic-scale-configure-deploy-split-and-merge.md)
* [Konfiguracja zabezpieczeń korespondencji seryjnej podziału](sql-database-elastic-scale-split-merge-security-configuration.md)
* [Zagadnienia dotyczące zabezpieczeń korespondencji seryjnej podziału](sql-database-elastic-scale-split-merge-security-configuration.md)
* [Zarządzanie mapy shard](sql-database-elastic-scale-shard-map-management.md)
* [Migrowanie istniejącej bazy danych w celu skala w nowym oknie](sql-database-elastic-convert-to-use-elastic-tools.md)
* [Narzędzia elastyczne bazy danych](sql-database-elastic-scale-introduction.md)
* [Elastyczne słownik Narzędzia bazy danych](sql-database-elastic-scale-glossary.md)

## <a name="why-use-the-split-merge-tool"></a>Dlaczego warto używać narzędzi korespondencji seryjnej podziału?

**Elastyczność**

Aplikacje muszą rozciąganie elastycznie poza granicami jednej bazie danych bazy danych SQL Azure. Narzędzie do przenoszenia danych, w razie potrzeby do nowych baz danych, zachowując integralności.

**Podziel się rozrośnie** 

Należy zwiększyć ogólną wydajność obsługi dynamiczny rozwój. W tym celu należy utworzyć dodatkowe możliwości przez sharding danych i rozpowszechniania w porządku rosnącym więcej baz danych, dopóki wymagana pojemność są spełnione. To jest podstawowym przykład funkcji "dzielenie". 

**Scalanie w celu zmniejszenia**

Wydajność musi zmniejszyć ze względu na charakter sezonowy działalności. Narzędzie pozwala skali mniej jednostki skali po zmniejsza firm. Funkcja "scalania" w usłudze korespondencji seryjnej podziału elastyczne skali obejmuje to wymaganie. 

**Zarządzanie punkty aktywne, przenosząc shardlets**

Z wielu dzierżaw na bazę danych przydział shardlets do odłamki może prowadzić do gardła wydajność na niektórych odłamki. Wymaga ponownego przydzielania shardlets lub przenoszenie zajęty shardlets do nowych lub mniej wykorzystywane odłamki. 

## <a name="concepts--key-features"></a>Pojęcia i kluczowe funkcje

**Obsługiwana w środowisku klienta usług**

Scalanie podziału jest dostarczana jako usługa obsługiwana w środowisku klienta. Należy wdrożyć i hosta usługi w subskrypcji usługi Microsoft Azure. Pakiet pobranego z NuGet zawiera szablon konfiguracji do wykonania z informacjami dla określonych wdrożenia. Zobacz [Samouczek korespondencji seryjnej podziału](sql-database-elastic-scale-configure-deploy-split-and-merge.md) , aby uzyskać szczegółowe informacje. Ponieważ usługa działa w subskrypcji usługi Azure, można kontrolować i skonfigurować większość ustawień zabezpieczeń usługi. Domyślny szablon zawiera opcje konfigurowania SSL, uwierzytelnianie klienta na podstawie certyfikatu szyfrowania dla przechowywanych poświadczeń, co należy robić ochrona i ograniczenia dotyczące adresów IP. Więcej informacji dotyczących aspektów zabezpieczeń można znaleźć w następujących dokumentu [konfiguracji zabezpieczeń korespondencji seryjnej podziału](sql-database-elastic-scale-split-merge-security-configuration.md).

Domyślny wdrożone działa usługa z jednego pracownika i jedna rola sieci web. Rozmiar pamięci Wirtualnej A1 każdego używa usług w chmurze Azure. Gdy nie można zmodyfikować te ustawienia, podczas instalowania pakietu, można je zmienić po pomyślnego wdrożenia w usłudze uruchomionego chmury (przez Azure portal). Należy zauważyć, że roli Pracownik nie mogą być skonfigurowani dla więcej niż jedno wystąpienie przyczyn technicznych. 

**Integracja mapy shard**

Usługi scalania podziału współdziała z mapą shard aplikacji. Podczas korzystania z usługi korespondencji seryjnej Podziel, aby podzielić lub scalić zakresów lub shardlets między odłamki, usługa automatycznie aktualizuje mapy shard na bieżąco. Aby to zrobić, usługa realizuje połączenie z bazą danych shard mapy Menedżer aplikacji i zachowuje zakresów i mapowania żądania podziału-korespondencji seryjnej i Przenieś postępu. Dzięki temu, że mapy shard zawsze przedstawia aktualny widok podczas operacji Scal podziału będą. Dzielenie, operacje przepływu korespondencji seryjnej i shardlet są wykonywane przez przeniesienie partię shardlets z shard źródła do shard docelowej. Podczas shardlet operacji przeniesienia shardlets objęte bieżącego zadania są oznaczone jako trybu offline na mapie shard i są niedostępne w przypadku połączeń routingu zależne od danych przy użyciu **OpenConnectionForKey** interfejsu API. 

**Spójne shardlet połączenia**

Po uruchomieniu przepływu danych dla nowej partii shardlets dowolnej mapy shard, pod warunkiem zależne od danych routingu połączenia shard przechowywanie shardlet są zabite i kolejnych połączeń z mapy shard interfejsy API tych shardlets są blokowane podczas przenoszenia danych w celu uniknięcia niezgodności. Połączenia z innymi shardlets na tym samym shard również uzyskać zabite, ale powiedzie się ponownie natychmiast po jego ponów próbę. Po przeniesieniu partii shardlets są oznaczone online ponownie shard docelowej i danych źródłowych zostanie usunięta z shard źródła. Usługa przechodzi przez te kroki dla każdej partii, aż wszystkie shardlets zostały przeniesione. Spowoduje to prowadzić do kilka czynności funkcji połączenia w trakcie wykonania operacji podziału-korespondencji seryjnej i przenoszenia.  

**Zarządzanie shardlet dostępności**

Ograniczanie połączenia zabijania do bieżącej partii shardlets zgodnie z powyższym opisem ogranicza zakres niedostępności jednej partii shardlets naraz. Jest to preferowanej przez metody miejsce, w którym wykonane shard może pozostać w trybie offline dla wszystkich jej shardlets w trakcie operacji dzielenie i scalanie. Rozmiar partii, zdefiniowane jako liczba unikatowych shardlets, aby przenieść w danym momencie, jest parametr konfiguracji. Można zdefiniować dla każdej operacji dzielenie i scalanie, w zależności od potrzeb dostępności i wydajności aplikacji. Zauważ, że zakres, który jest zablokowany w planie shard może być większy niż określony rozmiar partii. Jest to spowodowane usługę wybiera rozmiar zakresu pozwalający rzeczywista liczba wartości klucza sharding w danych o około odpowiada rozmiarowi partię. To jest należy pamiętać o w szczególności dla kluczy sharding słabo wypełnione. 

**Magazyn metadanych**

Usługi scalania podziału używa bazy danych do utrzymania swojego statusu i przechowywać dzienniki podczas przetwarzania żądania. Użytkownik tworzy ta baza danych w swoją subskrypcję i przewiduje parametry połączenia w pliku konfiguracji wdrożenia usługi. Administratorzy z organizacji użytkownika można również nawiązać tej bazy danych umożliwia przeglądanie postępu żądania oraz do znajdowania szczegółowe informacje dotyczące potencjalne błędy.

**Sygnalizacja dostępności sharding**

Odróżnia usługi scalania podziału między tabelami (1) sharded, tabele odwołań (2) i (3) normalny tabel. Znaczenie operacji podziału-korespondencji seryjnej i Przenieś zależą od typu tabeli używanej i są definiowane następująco: 

* **Tabele sharded**: dzielenie, Scal i operacji przenoszenia przechodzenie shardlets ze źródła do shard docelowej. Po pomyślnego ukończenia ogólnego żądania tych shardlets nie są już dostępne w źródle. Należy zauważyć, że tabeli docelowej muszą znajdować się na shard docelowej i nie może zawierać dane w zakresie docelowej przed wykonania operacji. 

* **Tabele odwołań**: tabel odwołań, podział, korespondencji seryjnej i przenoszenie operacji skopiuj dane ze źródła do shard docelowej. Zauważ, że żadne zmiany występować na shard docelowej dla danej tabeli, jeśli każdy wiersz znajduje się już w tej tabeli miejsca docelowego. Tabela ma być puste dla dowolnej operacji kopiowania tabeli odwołania do przetwarzanie.

* **Tabele inne**: inne tabele mogą występować na źródle lub docelowych dzielenie i scalanie operacji. Usługi scalania podziału pomija w poniższych tabelach operacji kopiowania lub przenoszenia danych. Zauważ, że mogą zakłócać te operacje w przypadku ograniczeń.

Informacje o odwołaniu a tabele sharded działaniem **SchemaInfo** interfejsy API na mapie shard. Poniższy przykład przedstawia Użyj tych interfejsów API w danej shard mapy menedżera obiektu smm: 

    // Create the schema annotations 
    SchemaInfo schemaInfo = new SchemaInfo(); 

    // Reference tables 
    schemaInfo.Add(new ReferenceTableInfo("dbo", "region")); 
    schemaInfo.Add(new ReferenceTableInfo("dbo", "nation")); 

    // Sharded tables 
    schemaInfo.Add(new ShardedTableInfo("dbo", "customer", "C_CUSTKEY")); 
    schemaInfo.Add(new ShardedTableInfo("dbo", "orders", "O_CUSTKEY")); 

    // Publish 
    smm.GetSchemaInfoCollection().Add(Configuration.ShardMapName, schemaInfo); 

Tabele "region" i "kraj" są określane jako tabele odwołań, a zostaną skopiowane z operacjami podziału-korespondencji seryjnej i Przenieś. "klient" i "zamówienia" z kolei są określane jako tabele sharded. C_CUSTKEY i O_CUSTKEY służy jako klucz sharding. 

**Więzy integralności**

Usługi scalania podziału analizuje zależności między tabelami i używa relacje kluczy obcych klucza podstawowego do etapu operacje przenoszenia tabel odwołań i shardlets. Na ogół odwołanie tabele są kopiowane najpierw w kolejności współzależności, a następnie shardlets są kopiowane w kolejności ich zależności w każdej partii. Jest to konieczne, tak, aby ograniczenia klucza podstawowego klucza Obcego shard docelowej są uznane jako otrzymaniu nowych danych. 

**Shard Mapa spójności i ewentualnego ukończenia**

W obecności błędy usługi scalania podziału życiorysy operacje po dowolnej awarii i ma na celu wykonywania w żądania informacji o postępie. Jednak czasami może występować odzyskać sytuacjach, np po shard docelowej zostanie utracony lub zostało naruszone naprawić. W tych okolicznościach niektórych shardlets, które zostały mają zostać przeniesione mogą nadal znajdują się na shard źródła. Usługa zapewnia mapowania shardlet tylko zostaną zaktualizowane po potrzebnych danych został skopiowany do elementu docelowego. Shardlets tylko są usuwane w źródle, gdy wszystkie swoje dane zostały skopiowane do elementu docelowego, a odpowiadające im mapowania zostały zaktualizowane pomyślnie. Operacja usuwania się dzieje w tle podczas zakres jest w trybie online na shard docelowej. Usługa korespondencji seryjnej podziału zawsze gwarantuje poprawność mapowania przechowywana w planie shard.


## <a name="the-split-merge-user-interface"></a>Interfejs użytkownika korespondencji seryjnej podziału

Pakiet usługi scalania podziału zawiera roli Pracownik i ról w sieci web. Ról w sieci web jest używany do Przesyłaj żądania korespondencji seryjnej podziału w sposób interakcyjny. Główne składniki interfejsu użytkownika są następujące:

-    Typ operacji: Typu operacji jest przycisk opcji, który określa rodzaj operacji wykonanych przez usługę dla tego żądania. Możesz wybrać pozycję podział, scalanie i przenoszenie scenariuszy. Możesz też anulować operacji poprzednio przesłany. Możesz użyć podziału, korespondencji seryjnej i przenosić żądań zakres shard mapy. Lista shard mapy tylko pomocy technicznej przenoszenie operacji.

-    Mapa shard: Następnej sekcji parametrów żądania obejmuje informacji na temat shard map i hostingu mapy shard bazy danych. W szczególności musisz podać nazwę serwera bazy danych SQL Azure i hostingu shardmap, poświadczenia nawiązywania połączenia z bazą danych mapowania shard, a na końcu Nazwa mapy shard bazy danych. Obecnie operację akceptuje tylko jeden zestaw poświadczeń. Te poświadczenia muszą być wystarczających uprawnień do wykonania na odłamki zmiany do mapy shard, a także danych użytkownika.

-    Zakres źródłowy (dzielenie i scalanie): dzielenie i scalanie operacji przetwarza zakresu przy użyciu klucza jego dolnego i górnego. Aby określić operacji z bez ograniczeń wysokiej wartości klucza, zaznacz pole wyboru "wysoka klucza jest maksymalny" i pozostaw puste pole klucza wysoki. Zakres wartości klucza, określanych nie muszą dokładnie zgodnie z mapowania i jego granice na mapie shard. Jeśli nie określisz wszelkie ograniczenia zakresu w ogóle usługę ustala najbliższym zakres dla Ciebie automatycznie. Skrypt programu GetMappings.ps1 PowerShell służy do pobierania bieżących mapowań na mapie danej shard.

-    Zachowanie źródła podziału (dzielenie): dla operacji podziału Definiowanie wskaż dzielenie zakresie źródłowym. W tym celu dostarczając klucz sharding miejsce, w którym chcesz wstawić podział ma być wykonywana. Użyj przycisku radiowego Określ, czy chcesz u dołu zakresu (z wyłączeniem klucz podziału) przenieść lub czy ma górnej części, aby przenieść (w tym klucz podziału).

-    Źródła Shardlet (Przenieś): Przenoszenie operacji różnią się od operacji dzielenie i scalanie jako nie wymagają zakresu powinno zawierać opis źródła. Źródła Przenieś po prostu jest identyfikowany przez wartość klucza sharding, który chcesz przenieść.

-    Docelowe Shard (dzielenie): po dostarczeniu tych informacji w źródle operacji podziału należy zdefiniować miejsce, w którym dane mają być kopiowane do przez podanie nazwy serwera i bazy danych bazy danych SQL Azure docelowej.

-    Zakres docelowej (seryjną): operacji Scal przenieść shardlets do istniejącego shard. Należy określić istniejące shard, dostarczając ograniczenia zakresu istniejącego zakresu, który chcesz scalić.

-    Rozmiar partii: Wielkość partii określa liczbę shardlets, która będzie przejść do trybu offline w czasie przenoszenia danych. Jest wartością całkowitą, gdzie można używać mniejszymi wartościami po wielkość liter długie okresy przestoje shardlets. Większe wartości zwiększa czas danej shardlet trybu offline, ale może zwiększyć wydajność.

-    Identyfikator operacji (Anuluj): Jeśli masz wykonywana operacja, która nie jest już potrzebna, możesz anulować operację, dostarczając jej identyfikator operacji, w tym polu. Identyfikator operacji można pobrać z tabeli stanu żądania (zobacz sekcję 8.1) lub z zestawu wyników w przeglądarce sieci web, w którym przesłane żądanie.


## <a name="requirements-and-limitations"></a>Wymagania i ograniczenia 

Bieżąca implementacja usługi scalania podziału podlega następujące wymagania i ograniczenia: 

* Odłamki konieczne istnieją i zarejestrowane w planie shard, zanim będzie można dokonać operacji scalania podziału na tych odłamki. 

* Usługa nie zostanie utworzony, tabel i innych obiektów bazy danych automatycznie w ramach operacji. To oznacza, że schemat sharded wszystkie tabele i tabele odwołań muszą znajdować się na shard docelowej przed dowolnej operacji podziału-korespondencji seryjnej i przenoszenia. Tabele sharded w szczególności muszą być puste w zakresie miejsce, w którym mają być dodawane przez operację podziału-korespondencji seryjnej i Przenieś nowe shardlets. W przeciwnym razie operacja zakończy się niepowodzeniem Sprawdzanie spójności początkowej w shard docelowej. Pamiętaj również tego odwołanie, które dane są kopiowane tylko, jeśli odwołanie tabela jest pusta, i że nie ma żadnych gwarancji spójności w odniesieniu do innych równoczesne operacje na tabel odwołań zapisu. Jest to zalecane: podczas uruchamiania operacji podziału-korespondencji seryjnej, nie operacji zapisu wprowadzić zmiany tabel odwołań.

* Usługa zależy od ustalone przez unikatowy indeks lub klucz, który zawiera klucz sharding w celu zwiększenia wydajności i niezawodności dla dużych shardlets tożsamości wiersza. Dzięki temu usługi w celu przenoszenia danych na jeszcze bardziej szczegółowy od tylko wartości klucza sharding. Pomaga to w celu zmniejszenia maksymalnej ilości miejsca w dzienniku i blokad, które są wymagane podczas operacji. Warto rozważyć utworzenie indeks unikatowy klucz podstawowy, w tym klawisz sharding na danej tabeli, jeśli chcesz używać tej tabeli żądaniami podziału-korespondencji seryjnej i Przenieś. Ze względu na wydajność klucz sharding powinny być zera wiodące kolumny klucza lub indeksu.

* W trakcie przetwarzania żądania niektóre dane shardlet mogą występować zarówno na źródle i shard docelowej. Jest to konieczne do ochrony przed błędy podczas przemieszczania shardlet. Integracja podziału korespondencji seryjnej z mapą shard zapewnia połączenia za pomocą zależne routingu interfejsy API przy użyciu metody **OpenConnectionForKey** na mapie shard danych nie ma dowolną niespójne pośrednich stanów. Jednak podczas nawiązywania połączenia źródłowej lub docelowej odłamki bez przy użyciu metody **OpenConnectionForKey** , to niespójne pośrednich stanów może być widoczna, gdy żądania podziału-korespondencji seryjnej i Przenieś przechodzą. Te połączenia mogą wyświetlać wyniki częściowej lub zduplikowanych w zależności od momentu lub shard źródłowego połączenie. To ograniczenie zawiera obecnie połączenia przez elastyczne skali wielu-Shard-kwerendy.

* Metadane bazy danych usługi podziału scalania nie może być udostępniany między różne role. Na przykład rolę usługi korespondencji seryjnej podziału działającej podczas przenoszenia musi wskaż bazy danych metadanych innego niż roli produkcji.
 

## <a name="billing"></a>Rozliczenia 

Usługa korespondencji seryjnej podziału działa usługa w chmurze w subskrypcji usługi Microsoft Azure. W związku z tym związanych z usługami w chmurze dotyczą wystąpienia usługi. Jeśli często wykonuje operacje podziału/korespondencji seryjnej i przenoszenie, zalecamy zapoznanie usuwanie usługi cloud korespondencji seryjnej podziału. Umożliwia zapisanie kosztów pracy albo wdrożyć wystąpień usługi w chmurze. Można ponownie wdrażanie i rozpocząć konfiguracji łatwo możliwe do uruchomienia, możesz w dowolnym momencie możesz wykonywać operacje dzielenie i scalanie. 
 
## <a name="monitoring"></a>Monitorowanie 
### <a name="status-tables"></a>Stan tabel 

Usługi scalania podziału zawiera tabeli **RequestStatus** w metadanych przechowywania bazy danych do monitorowania żądań wykonanych i stałe. W tabeli przedstawiono wiersza dla każdego żądania korespondencji seryjnej podziału przesłania do tego wystąpienia usługi podziału scalania. Dostępne są następujące informacje dla każdego żądania:

* **Sygnatura czasowa**: godziny i daty rozpoczęcia wezwanie.

* **OperationId**: identyfikator GUID, który identyfikuje żądanie. Aby anulować operację, gdy jest on nadal trwających również można to żądanie.

* **Stan**: bieżącego stanu żądania. Trwających wniosków o również Wyświetla listę bieżącą fazę jest wezwanie.

* **CancelRequest**: flagę, która wskazuje, czy żądanie zostało anulowane.

* **Informacje o postępie**: oszacowanie wartości procentowej wykonania tej operacji. Wartość 50 wskazuje, że operacja jest wykonane około 50%.

* **Szczegóły**: wartość XML, która zawiera raport bardziej szczegółowe informacje o postępie. Raportu z postępu okresowo jest aktualizowana, gdy zestawy wierszy są kopiowane ze źródła do miejsca docelowego. W przypadku awarii lub wyjątków ta kolumna zawiera bardziej szczegółowe informacje o błędzie.


### <a name="azure-diagnostics"></a>Diagnostyka Azure

Usługa korespondencji seryjnej podziału używa diagnostyki Azure według 2,5 SDK Azure monitorowania i diagnostyki. Konfiguracją diagnostyki zgodnie z opisem w tym miejscu: [Włączanie diagnostyki w środowisku maszyn wirtualnych systemu i usług w chmurze Azure](../cloud-services/cloud-services-dotnet-diagnostics.md). Pobierz pakiet zawiera dwie konfiguracje diagnostyki — jedną dla ról w sieci web i jedną dla roli pracownika. Tę konfigurację diagnostyki usługi postępuj zgodnie z zawartymi w [Chmurze usługi podstawy platformy Microsoft Azure](https://code.msdn.microsoft.com/windowsazure/Cloud-Service-Fundamentals-4ca72649). Zawiera definicje logowania liczniki wydajności, dzienniki programu IIS, dzienniki zdarzeń systemu Windows i dzienniki zdarzeń aplikacji korespondencji seryjnej podziału. 

## <a name="deploy-diagnostics"></a>Wdrażanie narzędzia diagnostyczne 

Aby włączyć monitorowania i diagnostyki przy użyciu konfiguracji diagnostycznych dla ról w sieci web i roboczy dostarczony przez pakiet NuGet, uruchom następujące polecenia przy użyciu programu PowerShell Azure: 

    $storage_name = "<YourAzureStorageAccount>" 
    
    $key = "<YourAzureStorageAccountKey" 
    
    $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key  
    
    
    $config_path = "<YourFilePath>\SplitMergeWebContent.diagnostics.xml" 
    
    $service_name = "<YourCloudServiceName>" 
    
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWeb" 
    
    
    $config_path = "<YourFilePath>\SplitMergeWorkerContent.diagnostics.xml" 
    
    $service_name = "<YourCloudServiceName>" 
    
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWorker" 

Można znaleźć więcej informacji na temat konfigurowania i wdrażania opisane tutaj ustawienia diagnostyki: [Włączanie diagnostyki w środowisku maszyn wirtualnych systemu i usług w chmurze Azure](../cloud-services/cloud-services-dotnet-diagnostics.md). 

## <a name="retrieve-diagnostics"></a>Pobieranie narzędzia diagnostyczne 

Można łatwy dostęp do diagnostyki z programu Visual Studio Server Explorer Azure część drzewa Eksploratora serwera. Otwórz wystąpienie programu Visual Studio, a na pasku menu kliknij pozycję Widok, a Server Explorer. Kliknij ikonę Azure nawiązywania połączenia z subskrypcji usługi Azure. Następnie przejdź do Azure -> miejsca do magazynowania -> <your storage account> -> WADLogsTable -> tabele. Aby uzyskać więcej informacji zobacz [Przeglądanie zasobów magazynu w Eksploratorze serwera](http://msdn.microsoft.com/library/azure/ff683677.aspx). 

![WADLogsTable][2]

WADLogsTable wyróżniony na powyższym rysunku zawiera szczegółowe zdarzenia z dziennika aplikacji usługi korespondencji seryjnej podziału. Należy zauważyć, że domyślna konfiguracja pobrany pakiet jest przeznaczone dla wdrożenia produkcji. Dlatego interwał, w którym dzienniki i liczniki są pobierane z wystąpień usługi jest duży (5 minut). Badania i rozwój obniżyć interwał dostosowując ustawienia diagnostyki sieci web lub Rola pracownika do własnych potrzeb. Kliknij prawym przyciskiem myszy roli w aplikacji programu Visual Studio Server Explorer (patrz powyżej), a następnie dopasuj okresu przełączanie w oknie dialogowym Ustawienia konfiguracji diagnostyki: 

![Konfiguracja][3]


## <a name="performance"></a>Wydajność

Ogólnie lepszą wydajność jest należy się spodziewać im większa więcej performant warstwy usługi w bazie danych SQL Azure. Wyższa Jo, Procesora i pamięci przydziałów dla wyższej warstwy usługi korzystać kopiowania zbiorczego i Usuń operacje, używane przez usługę korespondencji seryjnej podziału. Z tego powodu zwiększanie poziomu usług tylko dla tych baz danych przez określone, ograniczony czas.

Usługa wykonuje również kwerendy sprawdzania poprawności jako część operacjach normalny. Te kwerendy sprawdzania poprawności Sprawdź nieoczekiwane obecności dane w zakresie docelowej i upewnij się, że wszystkie operacje podziału-korespondencji seryjnej i Przenieś zaczyna się od spójna. Te kwerendy wszystkie pracować nad sharding zakresy klucza zdefiniowane przez zakres działania i rozmiar partii w ramach definicji wezwanie na. Te kwerendy sprawdzana najlepiej indeks jest obecnie wyposażona w klawisz sharding jako kolumnę zera wiodące. 

Ponadto właściwość unikatowość kluczem sharding jako zera wiodące kolumnę, aby umożliwić usługę, aby użyć zoptymalizowane podejście, który ogranicza zużycie zasobów w odniesieniu do miejsca w dzienniku i pamięci. Aby przenieść dane dużych rozmiarów (zwykle powyżej 1GB) jest wymagane tej właściwości unikatowość. 

## <a name="how-to-upgrade"></a>Jak uaktualnić

1. Postępuj zgodnie z instrukcjami [Wdrażanie usługi korespondencji seryjnej podziału](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
2. Zmienianie pliku konfiguracji usługi cloud wdrożenia korespondencji seryjnej podziału uwzględnić nowe parametry konfiguracji. Nowy, wymagany parametr jest informacje dotyczące certyfikatu służącego do szyfrowania. Łatwy sposób, w tym celu jest porównanie nowy plik szablonu konfiguracji możliwość pobierania pod kątem zgodności z istniejącej konfiguracji. Upewnij się, że możesz dodać ustawienia "DataEncryptionPrimaryCertificateThumbprint" i "DataEncryptionPrimary" dla sieci web i roli pracownika.
3. Przed wdrożeniem aktualizacji Azure, upewnij się, ukończono wszystkie operacje aktualnie uruchomione seryjnej podziału. Można łatwo w tym przez badanie RequestStatus i PendingWorkflows tabele bazy danych korespondencji seryjnej podziału metadanych dla trwającego żądań.
4. Aktualizowanie istniejącego wdrożenia usługi podziału korespondencji seryjnej w ramach subskrypcji Azure chmury z pakietem nowych i zaktualizowanych usługi pliku konfiguracji.

Nie musisz obsługi administracyjnej nowej bazy danych metadanych dla korespondencji seryjnej podziału uaktualnienia. Nowa wersja automatycznie zaktualizuje istniejącej bazy danych metadanych do nowej wersji. 

## <a name="best-practices--troubleshooting"></a>Najważniejsze wskazówki dotyczące i rozwiązywanie problemów
 
-    Definiowanie dzierżawy test i korzystają z najważniejszych operacje podziału-korespondencji seryjnej i Przenieś z dzierżawą test przez kilka odłamki. Upewnij się, że wszystkie metadane jest poprawnie zdefiniowana na mapie shard i operacje nie naruszają ograniczenia lub kluczy obcych.
-    Zachowaj dzierżawy test problemy związane z danych, rozmiar powyżej rozmiar maksymalny danych dzierżawy usługi największa, aby upewnić się, nie wystąpią rozmiar danych. Pomaga to ocenić górną granicę na czas potrzebny do poruszania się pojedynczego dzierżawy. 
-    Upewnij się, że schemat umożliwia usunięcia. Usługa korespondencji seryjnej podziału wymaga możliwość usunięcie danych z shard źródła danych został skopiowany do elementu docelowego. Na przykład **Usuń wyzwalacze** mogą uniemożliwić usunięcie danych w źródle usługi i mogą powodować niepowodzenie operacji.
-    Klucz sharding należy zera wiodące kolumny klucza podstawowego lub w definicji indeks unikatowy. Który zapewnia najlepszą wydajność kwerend sprawdzania poprawności dzielenie i scalanie i rzeczywistych danych przeniesienie lub usunięcie operacji, które zawsze działać na zakresach klucza sharding.
-    Rozlokować usługi korespondencji seryjnej podziału pośrodku region i danych miejsce, w którym znajdują się baz danych. 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]



<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-overview-split-and-merge/split-merge-overview.png
[2]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics.png
[3]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics-config.png
 
