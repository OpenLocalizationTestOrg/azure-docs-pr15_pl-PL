<properties 
    pageTitle="Ładowanie terabajtów danych do magazynu danych SQL | Microsoft Azure" 
    description="Pokazuje, jak 1 TB danych mogą być ładowane do magazynu danych SQL Azure w obszarze 15 minut z Factory danych Azure" 
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="jingwang"/>

# <a name="load-1-tb-into-azure-sql-data-warehouse-under-15-minutes-with-azure-data-factory-copy-wizard"></a>Ładowanie 1 TB do magazynu danych SQL Azure w obszarze 15 minut z Factory danych Azure [kreatora kopiowania]
[Magazyn danych SQL Azure](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) jest oparte na chmurze, poza skalowanie bazy danych do przetwarzania dużych ilości danych relacyjnych i -relacyjnych.  Oparte na Architektura przetwarzania znacznym stopniu równoległe (MPP), magazynu danych SQL jest zoptymalizowana pod kątem obciążenia magazynu danych przedsiębiorstwa.  Zapewnia on elastyczność chmury możliwość skalowania miejsca do magazynowania i obliczyć niezależnie.

Wprowadzenie do magazynu danych SQL Azure teraz jest łatwiejsze dzięki programowi **Azure danych Factory**.  Azure Factory danych jest usługi integracji w pełni zarządzane opartej na chmurze danych, które mogą być używane do wypełniania magazynu danych SQL przy użyciu danych z istniejącego systemu i zapisywanie możesz czasu przydatne podczas obliczania magazynu danych SQL i tworzenie rozwiązań do analizy znajdujący się na nim.  Poniżej przedstawiono kluczowe zalety ładowania danych do magazynu danych SQL Azure za pomocą Azure Factory danych:

- **Łatwy w celu skonfigurowania**: kroku 5 kreatora intuicyjny z bez skryptu wymagane.
- **Przechowywanie danych sformatowanych pomocy technicznej**: Obsługa bogatego zestawu lokalnego i opartej na chmurze magazynów.
- **Bezpieczne i zgodne z**: dane są przesyłane przez HTTPS lub ExpressRoute, i obecności globalne usługi zapewnia, danych nigdy nie pozostawia granicę geograficznych
- **Bezkonkurencyjne wydajności za pomocą PolyBase** — za pomocą Polybase jest najskuteczniejszą metodą na przenoszenie danych do magazynu danych SQL Azure. Funkcja tymczasowy obiektów blob, można uzyskać szybkości wysokie obciążenie ze wszystkich typów magazynów oprócz magazyn obiektów Blob platformy Azure, który obsługuje Polybase domyślnie.

W tym artykule pokazano, jak za pomocą Kreatora kopiowania Factory danych ładowania danych 1 TB z magazynem obiektów Blob platformy Azure do magazynu danych SQL Azure w obszarze 15 minut w ponad 1,2 przepustowość GB.

Ten artykuł zawiera instrukcje krok po kroku przenoszenia danych do magazynu danych SQL Azure za pomocą Kreatora kopiowania. 

> [AZURE.NOTE] Zobacz artykuł [Przenoszenie danych do i z magazynu danych SQL Azure za pomocą Factory danych Azure](data-factory-azure-sql-data-warehouse-connector.md) ogólnych informacji o możliwości fabryki danych w przenoszenie danych do/z magazynu danych SQL Azure. 
> 
> Można również tworzyć procesy za pomocą portalu Azure, Visual Studio, programu PowerShell, itp. Zobacz [Samouczek: kopiowanie danych z obiektów Blob platformy Azure do bazy danych SQL Azure](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) dla krótki Instruktaż z instrukcje krok po kroku dotyczące używania aktywności kopii w Azure danych Factory.  

## <a name="prerequisites"></a>Wymagania wstępne
- Magazyn obiektów Blob platformy Azure: tego doświadczenia używa magazyn obiektów Blob platformy Azure (GRS) do przechowywania TPC-H testowania zestawu danych.  Jeśli nie masz konta usługi Azure miejsca do magazynowania, Dowiedz się, [jak utworzyć konto miejsca do magazynowania](../storage/storage-create-storage-account.md#create-a-storage-account).
- Dane [TPC-H](http://www.tpc.org/tpch/) : Firma Microsoft będzie używany TPC-H jako testowania zestawu danych.  W tym celu należy użyć `dbgen` z zestawu narzędzi TPC-H, który ułatwia generowanie zestawu danych.  Kod źródłowy albo można pobrać `dbgen` z [Narzędzia TPC](http://www.tpc.org/tpc_documents_current_versions/current_specifications.asp) i kompilowania samodzielnie lub pobrać skompilowany plik binarny z [GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TPCHTools).  Uruchamianie dbgen.exe z następujących poleceń, aby wygenerować plik prostym 1 TB dla `lineitem` tabeli rozpowszechniania przez 10 plików:
    - `Dbgen -s 1000 -S **1** -C 10 -T L -v`
    - `Dbgen -s 1000 -S **2** -C 10 -T L -v`
    - …
    - `Dbgen -s 1000 -S **10** -C 10 -T L -v` 

    Teraz można skopiować wygenerowane pliki do obiektów Blob platformy Azure.  Odwoływanie się do [przenoszenia danych do i z lokalnego systemu plików za pomocą Factory danych Azure](data-factory-onprem-file-system-connector.md) , jak to zrobić za pomocą kopiowania ADF.   
- Magazyn danych SQL Azure: tego doświadczenia załadowaniu danych do magazynu danych SQL Azure utworzone za pomocą 6000 DWUs

    Aby uzyskać szczegółowe instrukcje na temat tworzenia bazy danych SQL Data Warehouse odwołują się do [Tworzenie magazynu danych SQL Azure](../sql-data-warehouse/sql-data-warehouse-get-started-provision/) .  Aby uzyskać najlepszą wydajność ładowania możliwe do magazynu danych SQL przy użyciu Polybase, możemy wybierz maksymalna liczba jednostek magazynu danych (DWUs) dozwolone w ustawienie wydajności, które jest 6000 DWUs.

    > [AZURE.NOTE] 
    > Podczas ładowania z obiektów Blob platformy Azure, wydajności ładowania danych jest wprost proporcjonalny do liczby DWUs można skonfigurować w magazynie danych SQL:
    > 
    > Ładowanie 1 TB do 1000 DWU SQL Data Warehouse trwa 87min (~ 200MBps przepustowość) ładowania 1 TB do 2000 DWU SQL Data Warehouse trwa 46min (~ 380MBps przepustowość) ładowania 1 TB do 6000 DWU SQL Data Warehouse trwa 14min (~1.2GBps przepustowość) 

    Aby utworzyć magazynu danych SQL z 6000 DWUs, przesuń suwak wydajności maksymalnie w prawo:

    ![Suwak wydajności](media/data-factory-load-sql-data-warehouse/performance-slider.png)

    Do istniejącej bazy danych nie jest skonfigurowany z 6000 DWUs można go skalować przy użyciu Azure portal.  Przejdź do bazy danych w portalu Azure i znajduje się przycisk **Skala** w panelu **Przegląd** pokazano na poniższej ilustracji:

    ![Przycisk Skala](media/data-factory-load-sql-data-warehouse/scale-button.png)    

    Kliknij przycisk **Skala** , aby otworzyć panel następujących, przeciągnij suwak maksymalną wartość, a następnie kliknij przycisk **Zapisz** .

    ![Okno dialogowe skalowania](media/data-factory-load-sql-data-warehouse/scale-dialog.png)
    
    Tego doświadczenia załadowaniu danych do magazynu danych SQL Azure za pomocą `xlargerc` klasy zasobów.

    Aby osiągnąć najlepszą wydajność możliwe, wystarczy wykonać przy użyciu magazynu danych SQL użytkownika należącego do kopiowania `xlargerc` klasy zasobów.  Dowiedz się, jak to zrobić przez następujące [zmiany przykład klasy zasobów użytkownika](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#change-a-user-resource-class-example).  

- Tworzenie schematu tabeli docelowej w bazie danych magazynu danych SQL Azure, wykonując następującą instrukcję DDL:

        CREATE TABLE [dbo].[lineitem]
        (
            [L_ORDERKEY] [bigint] NOT NULL,
            [L_PARTKEY] [bigint] NOT NULL,
            [L_SUPPKEY] [bigint] NOT NULL,
            [L_LINENUMBER] [int] NOT NULL,
            [L_QUANTITY] [decimal](15, 2) NULL,
            [L_EXTENDEDPRICE] [decimal](15, 2) NULL,
            [L_DISCOUNT] [decimal](15, 2) NULL,
            [L_TAX] [decimal](15, 2) NULL,
            [L_RETURNFLAG] [char](1) NULL,
            [L_LINESTATUS] [char](1) NULL,
            [L_SHIPDATE] [date] NULL,
            [L_COMMITDATE] [date] NULL,
            [L_RECEIPTDATE] [date] NULL,
            [L_SHIPINSTRUCT] [char](25) NULL,
            [L_SHIPMODE] [char](10) NULL,
            [L_COMMENT] [varchar](44) NULL
        )
        WITH
        (
            DISTRIBUTION = ROUND_ROBIN,
            CLUSTERED COLUMNSTORE INDEX
        )

Czynności wstępne wykonane możemy przystąpić do konfigurowania aktywności kopii za pomocą Kreatora kopii.

## <a name="launch-copy-wizard"></a>Uruchamianie Kreatora kopiowania

1.  Zaloguj się do [portalu Azure](https://portal.azure.com).
2.  Kliknij pozycję **+ Nowe** od lewego górnego rogu, kliknij **analizy + analizy**, a następnie kliknij przycisk **Factory danych**. 
6. W karta **nowego factory danych** :
    1. Wprowadź **nazwę** **LoadIntoSQLDWDataFactory** .
        Nazwa fabryki Azure danych musi być globalnie unikatowa. Jeśli zostanie wyświetlony komunikat o błędzie: **factory danych o nazwie "LoadIntoSQLDWDataFactory" nie jest dostępny**, Zmień nazwę fabryki danych (na przykład yournameLoadIntoSQLDWDataFactory) i spróbuj ponownie utworzyć. Temat [Danych Factory - reguł nazewnictwa](data-factory-naming-rules.md) dla reguł nazewnictwa artefakty Factory danych.  
     
    2. Wybierz pozycję Azure **subskrypcji**.
    3. Grupa zasobów wykonaj jedną z następujących czynności: 
        1. Wybierz opcję **Użyj istniejącego** , aby zaznaczyć istniejącej grupy zasobów.
        2. Wybierz pozycję **Utwórz nowy** wprowadź nazwę dla grupy zasobów.
    3. Wybierz **lokalizację** dla factory danych.
    4. Zaznacz pole wyboru **numer Pin do pulpitu nawigacyjnego** w dolnej części karta.  
    5. Kliknij przycisk **Utwórz**.
10. Po zakończeniu tworzenia możesz zobaczyć karta **Factory danych** , jak pokazano na poniższej ilustracji:

    ![Strona główna factory danych](media/data-factory-load-sql-data-warehouse/data-factory-home-page-copy-data.png)
11. Na stronie głównej Factory danych kliknij kafelków **Kopiowanie danych** , aby uruchomić **Kreatora kopiowania**. 

    > [AZURE.NOTE] Jeśli zobaczysz, że przeglądarki sieci web zatrzymuje się na "Autoryzowanie...", wyłącz i wyczyść pole wyboru ustawienie **Blokowanie plików cookie innych firm i witryny danych** (lub) pozostaw je włączony i utworzyć wyjątek **login.microsoftonline.com** , a następnie spróbuj ponownie uruchamianie kreatora.


## <a name="step-1-configure-data-loading-schedule"></a>Krok 1: Konfigurowanie ładowania harmonogramu danych
Pierwszym krokiem jest skonfigurowanie ładowania harmonogramu danych.  

Na stronie **Właściwości** :
1. Wprowadź **CopyFromBlobToAzureSqlDataWarehouse** dla **nazwy zadania**
2. Wybierz opcję **Uruchom raz teraz** .   
3. Kliknij przycisk **Dalej**.  

![Kopiowanie Wizard — strona właściwości](media/data-factory-load-sql-data-warehouse/copy-wizard-properties-page.png)

## <a name="step-2-configure-source"></a>Krok 2: Konfigurowanie źródła
W tej sekcji przedstawiono kroki, aby skonfigurować źródła: obiektów Blob platformy Azure zawierającego wiersz 1 TB TPC-H element plików.

Wybierz **Magazyn obiektów Blob platformy Azure** , jak przechowywanie danych i kliknij przycisk **Dalej**.

![Kopiowanie Wizard — wybierz źródło strony](media/data-factory-load-sql-data-warehouse/select-source-connection.png)

Wypełnij informacje o połączeniu dla konta magazyn obiektów Blob platformy Azure i kliknij przycisk **Dalej**.

![Kopiowanie Wizard — informacje o połączeniu źródła](media/data-factory-load-sql-data-warehouse/source-connection-info.png)

Wybierz **folder** zawierający pliki pozycji TPC-H, a następnie kliknij przycisk **Dalej**.

![Kopiowanie kreatora — wybierz folder wejściowy](media/data-factory-load-sql-data-warehouse/select-input-folder.png)

Po kliknięciu przycisku **Dalej**, automatycznie zostaną wykryte ustawienia formatu pliku.  Upewnij się, ogranicznik tej kolumny jest "|"zamiast przecinka domyślne",".  Po przejrzeniu danych, kliknij przycisk **Dalej** .

![Kopiowanie Wizard — ustawienia formatu pliku](media/data-factory-load-sql-data-warehouse/file-format-settings.png)

## <a name="step-3-configure-destination"></a>Krok 3: Konfigurowanie miejsca docelowego
W tej sekcji przedstawiono sposób konfigurowania miejsce docelowe: `lineitem` tabeli w bazie danych magazynu danych SQL Azure.

Wybierz pozycję **Magazynu danych SQL Azure** jako magazynu docelowego, a następnie kliknij przycisk **Dalej**.

![Kopiowanie Wizard — magazynu danych zaznacz miejsce docelowe](media/data-factory-load-sql-data-warehouse/select-destination-data-store.png)

Wypełnij informacje o połączeniu dla magazynu danych SQL Azure.  Upewnij się, określ użytkownika należącego do roli `xlargerc` (zobacz sekcję **wymagania wstępne** , aby uzyskać szczegółowe instrukcje) i kliknij przycisk **Dalej**. 

![Kopiowanie Wizard — miejsce docelowe informacji o połączeniu](media/data-factory-load-sql-data-warehouse/destination-connection-info.png)

Wybierz tabelę docelową, a następnie kliknij przycisk **Dalej**.

![Kopiowanie Wizard — strona mapowania tabeli](media/data-factory-load-sql-data-warehouse/table-mapping-page.png)

Zaakceptuj ustawienia domyślne dla mapowania kolumn i kliknij przycisk **Dalej**.

![Kopiowanie Wizard — strona mapowania schematu](media/data-factory-load-sql-data-warehouse/schema-mapping.png)


## <a name="step-4-performance-settings"></a>Krok 4: Ustawienia wydajności

Domyślnie jest zaznaczone **polybase Zezwalaj** .  Kliknij przycisk **Dalej**.

![Kopiowanie Wizard — strona mapowania schematu](media/data-factory-load-sql-data-warehouse/performance-settings-page.png)

## <a name="step-5-deploy-and-monitor-load-results"></a>Krok 5: Wdrażanie i monitorowanie efekty ładowania
Kliknij przycisk **Zakończ** , aby wdrożyć. 

![Kopiowanie Wizard — strona podsumowania](media/data-factory-load-sql-data-warehouse/summary-page.png)

Po zakończeniu rozmieszczania kliknij `Click here to monitor copy pipeline` monitorowanie kopii uruchamianie postępu.

Wybierz proces kopiowania utworzonego na liście **Windows aktywności** .

![Kopiowanie Wizard — strona podsumowania](media/data-factory-load-sql-data-warehouse/select-pipeline-monitor-manage-app.png)

Możesz wyświetlić kopii uruchamianie szczegóły w **Aktywności okna Eksploratora** w prawym panelu, w tym wielkość danych ze źródła i odczytywana do miejsca docelowego, czas trwania i Średnia produktywność do uruchomienia.

Jak widać na ekranie następujące zrzut kopiowany do magazynu danych SQL Azure magazyn obiektów Blob 1 TB zajęła 14 minut skuteczne osiągnięcia 1,22 przepustowość GB!

![Kopiowanie Wizard — okno dialogowe pomyślnie](media/data-factory-load-sql-data-warehouse/succeeded-info.png)

## <a name="best-practices"></a>Najważniejsze wskazówki
Poniżej przedstawiono kilka najważniejsze wskazówki dotyczące uruchamiania magazynu danych SQL Azure bazy danych:

- Podczas ładowania do INDEKSU COLUMNSTORE GRUPOWANY za pomocą większych klasy zasobu.
- Dla sprzężeń bardziej efektywne warto rozważyć użycie skrótu rozkładu według zaznacz kolumnę zamiast domyślne zaokrąglanie rozdzielania cyklicznego.
- Szybsze ładowanie szybkości warto rozważyć użycie stosu przejściowych danych.
- Tworzenie statystyki po zakończeniu ładowania magazynu danych SQL Azure.

Aby uzyskać szczegółowe informacje, zobacz [Najważniejsze wskazówki dotyczące magazynu danych SQL Azure](../sql-data-warehouse/sql-data-warehouse-best-practices.md) . 

## <a name="next-steps"></a>Następne kroki
- [Kreator kopii Factory danych](data-factory-copy-wizard.md) — ten artykuł zawiera szczegółowe informacje dotyczące Kreatora kopiowania. 
- [Wydajność działania kopii i dostosowywania przewodnik](data-factory-copy-activity-performance.md) — w tym artykule zawiera odwołanie pomiary wydajności i dostosowywania przewodnik.

