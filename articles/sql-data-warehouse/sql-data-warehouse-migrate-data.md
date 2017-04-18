<properties
   pageTitle="Migrację danych do magazynu danych SQL | Microsoft Azure"
   description="Porady dotyczące migrację danych do magazynu danych SQL Azure dla opracowania rozwiązań."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="migrate-your-data"></a>Migrowanie danych
Do magazynu danych SQL przy użyciu narzędzi różnych przenoszenia z różnych źródeł danych.  Kopiowanie ADF, SSIS i bcp wszystkie można osiągnąć ten cel. Jednak jako kwota podwyżki danych należy wziąć pod uwagę podziału na etapy procesu migracji danych. Zapewnia możliwość zoptymalizować każdego kroku zarówno wydajność i odporność zapewnienie migracji wygładzonymi danych.

W tym artykule omówiono najpierw scenariusze prosty migracji kopii ADF, SSIS i bcp. Następnie wygląda nieco bardziej do jak może być zoptymalizowany migracji.

## <a name="azure-data-factory-adf-copy"></a>Kopiowanie danych Factory (ADF) Azure
[Kopiowanie ADF][] jest częścią [Azure danych Factory][]. Kopiowanie ADF umożliwia eksportowanie danych do prostym plików znajdujących się na magazynu lokalnego do zdalnego prostym plików przechowywanych w magazynie obiektów blob platformy Azure lub bezpośrednio do magazynu danych SQL.

Jeśli dane rozpoczynają się w plikach płaskie, a następnie musisz najpierw przenieść do Azure magazyn obiektów blob przed rozpoczęciem załaduj go do magazynu danych SQL. Gdy dane są przesyłane do magazynem obiektów blob Azure można ponownie użyć [ADF Kopiuj][] , aby przekazać danych do magazynu danych SQL.

PolyBase udostępnia opcję wysokiej wydajności ładowania danych. Jednak to oznacza, za pomocą dwóch narzędzi zamiast jednej. Jeśli potrzebujesz najlepszą wydajność, należy użyć PolyBase. Jeśli chcesz obsługi jednego narzędzia (i dane nie znajdują się duże) ADF jest odpowiedzi na Twoje pytanie.

> [AZURE.NOTE] PolyBase wymaga pliki danych w przypadku kodowania UTF-8. To jest domyślna kopii ADF kodowanie, aby nic nie może zmienić. Jest to po prostu przypomnienia aby nie zmienić domyślne zachowanie kopii ADF.

Głowy nad z następującym artykułem niektórych doskonałe [Przykłady ADF][].

## <a name="integration-services"></a>Usługi integracji ##
Integration Services (SSIS) to zaawansowane i elastyczne wyodrębnić przekształcenie i ładowania (ETL) narzędzie obsługuje złożone przepływy pracy, przekształcanie i kilka opcji ładowania danych. Po prostu transferować dane Azure lub jako część szerszego migracji za pomocą SSIS.

> [AZURE.NOTE] SSIS można eksportować do UTF-8 bez znacznika kolejności bajtów w pliku. Aby skonfigurować to należy najpierw użyj składnika pochodnych kolumny przekonwertować dane znaków w przepływie danych do korzystania z 65001 strony kod UTF-8. Po przekonwertowaniu kolumn, zapisane na karcie docelowego pliku prostego zapewnienie, że 65001 również zostało zaznaczone jako strona kodowa pliku.

SSIS łączy do magazynu danych SQL tak, jak chcesz nawiązać wdrożenia programu SQL Server. Jednak połączenia należy korzystać z Menedżera połączeń ADO.NET. Możesz również należy zwrócić uwagę, aby Konfigurowanie "Użyj zbiorcze Wstaw gdy są dostępne" ustawienie maksymalizować przepustowość. Można znaleźć w artykule [ADO.NET docelowego karty][] , aby dowiedzieć się więcej na temat tej właściwości

> [AZURE.NOTE] Łączenie z magazynu danych SQL Azure za pomocą OLEDB nie jest obsługiwane.

Ponadto istnieje możliwość, że pakiet może się nie powieść ukończenia problemów ograniczania lub siecią. Projektowanie pakietów w celu ich można wznowić punkcie błąd, bez ponawianie zakończenia przed wystąpieniem przerwy w pracy.

Aby uzyskać więcej informacji zapoznaj się z [dokumentacją SSIS][].

## <a name="bcp"></a>BCP
BCP to narzędzie wiersza polecenia, która jest przeznaczona dla pliku prostego importowania i eksportowania danych. Niektóre przekształcenia może się odbywać podczas eksportowania danych. Do wykonywania prostych transformacji umożliwia zapytania zaznacz i przekształcania danych. Po wyeksportowane, plików prostych może zostać załadowana bezpośrednio do docelowej bazy danych SQL magazynu danych.

> [AZURE.NOTE] Często jest dobrym pomysłem jest umieszczać przekształcenia używane podczas eksportowania danych w widoku w źródłowym systemie. Dzięki temu logikę jest zachowywana, a proces jest powtarzalnych.

Zalety bcp są następujące:

- Prostota. prosty do tworzenia i wykonywanie są polecenia BCP
- Proces Załaduj ponownie będzie można uruchomić systemu. Raz wyeksportowane, załaduj można wykonać dowolną liczbę razy.

Ograniczenia dotyczące bcp są:

- BCP współpracuje tylko tabelaryczne plików płaskich. Nie działa z plikami, takich jak pliki xml lub JSON
- BCP nie obsługuje eksportowania do UTF-8. Może to uniemożliwić PolyBase na bcp wyeksportowane dane
- Możliwości przekształcania danych są ograniczone do tylko scenie eksportu i są proste charakter
- BCP nie został dostosowany za niezawodne podczas ładowania danych przez internet. Niestabilności sieci może spowodować błąd ładowania.
- BCP zależy od schematu obecną w docelowej bazie danych przed Załaduj

Aby uzyskać więcej informacji zobacz [Używanie bcp, aby załadować dane do magazynu danych SQL][].

## <a name="optimizing-data-migration"></a>Optymalizacja migracji danych
Proces migracji danych SQLDW mogą skutecznie podzielone na trzy osobne kroki:

1. Eksportowanie danych źródłowych
2. Przesyłanie danych do Azure
3. Ładowanie do SQLDW docelowej bazy danych

Każdy krok może być zoptymalizowany pojedynczo do tworzenia procesu migracji niezawodne, będzie można ponownie uruchomić systemu i mechanizm, który maksymalizuje wydajność na poszczególnych etapach.

## <a name="optimizing-data-load"></a>Optymalizowanie ładowania danych
W tych w odwrotnej kolejności przez chwilę; jest to najszybszy sposób, aby załadować dane za pośrednictwem PolyBase. Optymalizowanie pod kątem procesu ładowania PolyBase umieści wstępnych na powyższych kroków, warto to opis poświęcić. Są to:

1. Kodowanie plików danych
2. Format plików danych
3. Lokalizacje plików danych

### <a name="encoding"></a>Kodowanie
PolyBase wymaga plików danych zakodowany UTF-8. Oznacza to, że podczas eksportowania danych musi być zgodna tego wymagania. Jeśli dane zawierają tylko podstawowe znaków ASCII (extended nie ASCII) następnie te mapy bezpośrednio do standardowego UTF-8, dzięki czemu nie musisz się martwić zbyt dużo kodowania. Jednak jeśli dane zawierają żadnych znaków specjalnych, takich jak umlaut, akcenty lub symbole lub danych obsługuje języków innych niż łaciński następnie należy zapewnić plików eksportu prawidłowo zakodowany UTF-8.

> [AZURE.NOTE] BCP nie obsługuje eksportowanie danych do UTF-8. W związku z tym usługi najlepszym rozwiązaniem jest użycie usług Integration Services lub kopiowanie ADF dla eksportu danych. Warto wskazujące, że znacznika kolejności bajtów UTF-8 (BOM) nie jest konieczne w pliku danych.

Wszystkie pliki zakodowany przy użyciu UTF-16 będzie konieczne ponowne ***poprzednich*** w celu zapisania transfer danych.

### <a name="format-of-data-files"></a>Format plików danych
PolyBase wymaga zakończyć stały wiersza \n lub nowego wiersza. Pliki danych muszą być zgodne z tym standardowe. Nie ma ograniczeń ciąg lub kolumna elementy końcowe.

Należy zdefiniować każdej kolumny w pliku jako części tabeli zewnętrznej w PolyBase. Upewnij się, że wszystkie kolumny wyeksportowane są wymagane i czy typy są zgodne ze standardami wymagane.

Należy korzystać z [Migrowanie schematu] artykułu do szczegółów na obsługiwane typy danych.

### <a name="location-of-data-files"></a>Lokalizacje plików danych
Program SQL Data Warehouse używa PolyBase wyłącznie załadować dane z magazynem obiektów Blob platformy Azure. W związku z tym dane należy najpierw przeniesiono do magazyn obiektów blob.

## <a name="optimizing-data-transfer"></a>Optymalizacja przeniesienia danych
Jedną z najmniejszą części migracji danych jest transfer danych Azure. Nie tylko przepustowość sieci można problemu, ale również niezawodności sieci mogą znacząco utrudniać postępu. Migrowanie danych Azure jest domyślnie przez internet, rozsądnie prawdopodobnie prawdopodobieństwo wystąpienia błędów przełączania. Jednak tych błędów może wymagać danych w celu ponownego wysyłania w całości lub częściowo.

Na szczęście istnieje kilka opcji, aby zwiększyć szybkość i odporność tego procesu:

### <a name="expressroute"></a>[ExpressRoute][]
Warto rozważyć użycie [ExpressRoute][] aby przyspieszyć transfer. [ExpressRoute][] oferuje nawiązane połączenie prywatne Azure, połączenie wykracza publicznie w Internecie. W żadnym wypadku nie jest obowiązkowe kroku. Jednak go może zwiększyć przepustowość po przekazywanie danych Azure ze źródeł lokalnych lub Współtworzenie lokalizacji.

Korzyści wynikające z używania [ExpressRoute][] są następujące:

1. Zwiększona niezawodność
2. Szybsze sieci
3. Mniejsze opóźnienia sieci
4. lepsze zabezpieczenia sieci

[ExpressRoute][] jest korzystne dla liczby scenariusze; nie tylko migracji.

Cię? Więcej informacji i sprawdź ceny można znaleźć w [dokumentacji ExpressRoute][].

### <a name="azure-import-and-export-service"></a>Azure importu i eksportu usługi
Azure importu i eksportu usługi jest przeznaczona dla dużych (GB ++) do duże (TB ++) przeniesienia danych do Azure procesu transfer danych. Obejmuje zapisywania danych na dyskach i wysyłania ich do centrum danych Azure. Zawartości dysku zostanie następnie załadowana do obiektów blob miejsca do magazynowania Azure w Twoim imieniu.

Ogólny widok eksportu importowania jest następująca:

1. Konfigurowanie kontenerem magazyn obiektów Blob platformy Azure do odbierania danych
2. Eksportowanie danych do lokalnego magazynu
2. Skopiuj dane do 3,5 cala SATA II III dysków twardych narzędzie [Azure Importuj/Eksportuj]
3. Tworzenie zadania importu przy użyciu Azure importowanie i eksportowanie usługi, dostarczając pliki dziennika tworzone przez [narzędzie Importuj/Eksportuj Azure]
4. Wysłać dysków Centrum wyznaczone Azure danych
5. Dane są przekazywane do kontenera usługi Magazyn obiektów Blob platformy Azure
6. Ładowanie danych do SQLDW przy użyciu PolyBase

### <a name="azcopy-utility"></a>Narzędzie [AZCopy][]
Narzędzie [AZCopy][] jest doskonałym narzędziem do pobierania danych do obiektów blob miejsca do magazynowania Azure. Jest przeznaczony dla małych (MB ++) do bardzo duże przesyłania danych (GB ++). Zapewnienie, że to doskonałe rozwiązanie kroku transfer danych, warto przepustowość mechanizm przesyłania danych Azure oraz tak również opracowano [AZCopy] . Raz przeniesione, można załadować dane za pomocą PolyBase do magazynu danych SQL. Można także dołączać AZCopy do swojego pakietów SSIS przy użyciu zadania "Wykonywanie procesu".

Aby użyć AZCopy będzie należy najpierw Pobierz i zainstaluj go. Brak dostępnej [wersji produkcji][] i w [wersji preview][] .

Aby przekazać plik systemu plików należy polecenie, taki jak przedstawiony poniżej:

```
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:abc.txt
```

Podsumowanie procesu może być:

1. Konfigurowanie w celu odbioru danych kontenera obiektów blob Azure magazynu
2. Eksportowanie danych do lokalnego magazynu
3. AZCopy danych w kontenerze magazyn obiektów Blob platformy Azure
4. Ładowanie danych do magazynu danych SQL przy użyciu PolyBase

Pełna dokumentacja: [AZCopy][].

## <a name="optimizing-data-export"></a>Optymalizowanie eksportu danych
Oprócz zapewnianie, że eksportowania spełnia wymagania ustanowione przez PolyBase można również Szukanie wyniku w celu zoptymalizowania eksportowania danych, aby poprawić proces dodatkowo.

> [AZURE.NOTE] PolyBase wymaga dane mają być w formacie UTF-8, że prawdopodobnie zostaną użyte bcp do wykonania eksportu danych. BCP nie obsługuje akceptujących pliki danych w celu UTF-8. SSIS lub kopiowanie ADF nadają się znacznie lepiej do wykonywania tego rodzaju Eksport danych.

### <a name="data-compression"></a>Kompresję danych
PolyBase można przeczytać gzip skompresowane dane. Można skompresować danych w celu pliki gzip zostanie zminimalizowane ilość danych jest przypisany przez sieć.

### <a name="multiple-files"></a>Wielu plików
Podzieleniem dużych tabel do kilku plików nie tylko pomaga Aby zwiększyć szybkość eksportu, ale również z re-startability przeniesienia i ogólnego zarządzania danymi raz w magazynie obiektów blob platformy Azure. Jedną z wielu funkcji i PolyBase jest, że odczytuje wszystkie pliki w folderze i traktowania jej jako jednej tabeli. Dlatego jest dobrym pomysłem jest wyodrębnienia plików dla każdej tabeli w osobnym folderze.

PolyBase obsługuje także funkcji znanej jako "cykliczne przechodzenie folderu". Ta funkcja umożliwia ulepszanie organizacji eksportowanych danych, aby poprawić zarządzanie danymi.

Aby dowiedzieć się więcej na temat ładowania danych za pomocą PolyBase, zobacz [Używanie PolyBase, aby załadować dane do magazynu danych SQL][].


## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej informacji o migracji zobacz [Migrowanie rozwiązania do magazynu danych SQL][].
Aby uzyskać więcej porad rozwoju zobacz [Omówienie tworzenia][].

<!--Image references-->

<!--Article references-->
[AZCopy]: ../storage/storage-use-azcopy.md
[Kopiowanie ADF]: ../data-factory/data-factory-data-movement-activities.md 
[Przykłady ADF]: ../data-factory/data-factory-samples.md
[ADF Copy examples]: ../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md
[Omówienie tworzenia]: sql-data-warehouse-overview-develop.md
[Migrowanie rozwiązania do magazynu danych SQL]: sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[Aby załadować dane do magazynu danych SQL za pomocą bcp]: sql-data-warehouse-load-with-bcp.md
[Aby załadować dane do magazynu danych SQL za pomocą PolyBase]: sql-data-warehouse-get-started-load-with-polybase.md


<!--MSDN references-->

<!--Other Web references-->
[Factory Azure danych]: http://azure.microsoft.com/services/data-factory/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[Dokumentacja ExpressRoute]: http://azure.microsoft.com/documentation/services/expressroute/

[Wersja produkcji]: http://aka.ms/downloadazcopy/
[wersji Preview]: http://aka.ms/downloadazcopypr/
[Karta obiektu docelowego ADO.NET]: https://msdn.microsoft.com/library/bb934041.aspx
[Dokumentacja SSIS]: https://msdn.microsoft.com/library/ms141026.aspx
