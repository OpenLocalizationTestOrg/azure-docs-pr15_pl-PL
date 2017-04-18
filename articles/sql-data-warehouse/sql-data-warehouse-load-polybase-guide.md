<properties
   pageTitle="Przewodnik dotyczący przy użyciu PolyBase w magazynie danych SQL | Microsoft Azure"
   description="Wskazówki i zalecenia dotyczące korzystania z PolyBase w scenariuszach magazynu danych SQL."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/30/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="guide-for-using-polybase-in-sql-data-warehouse"></a>Przewodnik dotyczący przy użyciu PolyBase w magazynie danych SQL

Ten przewodnik zawiera wszystkie stosowne informacje dotyczące korzystania z PolyBase w magazynie danych SQL.

Aby rozpocząć pracę, zobacz samouczek [ładowania danych za pomocą PolyBase][] .


## <a name="rotating-storage-keys"></a>Obracanie klawiszy miejsca do magazynowania

Od czasu do czasu można zmienić klawisz dostępu z magazynem obiektów blob ze względów bezpieczeństwa.

Najbardziej elegancki sposób, aby wykonać to zadanie polega na wykonaniu proces znany jako "obracanie kluczy". Możesz zauważyć ma dwa klucze miejsca do magazynowania dla Twojego konta magazyn obiektów blob. To może przejść

Obracanie kluczy konto Azure miejsca do magazynowania jest procesem prosty trzech fazach

1. Utwórz drugą poświadczeń występujące w bazie danych według magazynu pomocniczego klawisz dostępu
2. Tworzenie drugiego zewnętrznego źródła danych opartego na nowe poświadczenia
3. Usuwanie i tworzenie tabel zewnętrznych, wskazująca nowe źródło danych zewnętrznych

Po dokonaniu migracji że zewnętrzne tabele do nowego źródła danych zewnętrznych, a następnie można wykonywać zadania oczyszczania:

1. Usuwanie pierwszej zewnętrznego źródła danych
2. Poświadczenia oparte na klucz podstawowy dostęp występujące upuszczania pierwszej bazy danych
3. Zaloguj się do Azure i ponownie wygenerować klucz podstawowy dostęp gotowa do użycia w przyszłości

## <a name="query-azure-blob-storage-data"></a>Kwerendy danych magazyn obiektów blob platformy Azure
Tak, jakby było Tabela relacyjna zapytań tabele zewnętrzne po prostu użyć nazwy tabeli.

```sql
-- Query Azure storage resident data via external table.
SELECT * FROM [ext].[CarSensor_Data]
;
```

> [AZURE.NOTE] Kwerendy z tabeli zewnętrznej można się niepowodzeniem z powodu błędu *"Kwerendy zostało przerwane — maksimum odrzucić próg upłynął podczas czytania ze źródła zewnętrznego"*. Oznacza to, że dane zewnętrzne zawiera *zmienionych* rekordów. Rekord danych jest traktowany jako "dirty", jeśli dane typy i liczba kolumn nie odpowiadają definicji kolumny tabeli zewnętrznej lub jeśli dane nie jest zgodny z formatem pliku zewnętrznych. Aby rozwiązać ten problem, upewnij się, że poprawność tabeli zewnętrznej i definicji formatów plików zewnętrznych i dane zewnętrzne jest zgodna z tych definicji. W przypadku, gdy podzbioru rekordów danych zewnętrznych zostały zmienione, można odrzucić tych rekordów zapytań za pomocą opcji Odrzuć Tworzenie zewnętrznych DDL tabeli.


## <a name="load-data-from-azure-blob-storage"></a>Ładowanie danych z magazynem obiektów blob Azure
W tym przykładzie ładuje dane z magazynem obiektów blob Azure do bazy danych SQL magazynu danych.

Dane są przechowywane bezpośrednio usuwa czasu przeniesienia danych dla kwerend. Przechowywanie danych z indeksem columnstore zwiększa wydajność kwerendy dla kwerendy analizy według maksymalnie 10 x.

W tym przykładzie użyto instrukcji Utwórz tabelę jako wybierz, aby załadować dane. Nowa tabela dziedziczy kolumn określona w kwerendzie. Typy danych tych kolumn dziedziczy z definicji tabeli zewnętrznej.

Tworzenie tabeli jako wybierz pozycję jest wysoce performant instrukcji Transact-SQL, ładującego dane równolegle do wszystkich węzłów obliczeń magazynu danych SQL.  Opracowany aparatu przetwarzanie znacznym stopniu równoległe (MPP) w systemie platformy analizy, a jest teraz w magazynie danych SQL.

```sql
-- Load data from Azure blob storage to SQL Data Warehouse

CREATE TABLE [dbo].[Customer_Speed]
WITH
(   
    CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([CarSensor_Data].[CustomerKey])
)
AS
SELECT *
FROM   [ext].[CarSensor_Data]
;
```

Zobacz [Tworzenie tabeli jako Wybieranie (Transact-SQL)][].

## <a name="create-statistics-on-newly-loaded-data"></a>Tworzenie statystyk załadowaniu danych

Magazyn danych SQL Azure czy jeszcze nie pomocy technicznej automatyczne tworzenie lub automatycznej aktualizacji statystyki.  W celu uzyskania najlepszej wydajności należy przejść z zapytań, należy pamiętać, że statystyki można tworzyć na wszystkich kolumn wszystkie tabele po załadowaniu pierwszego lub wystąpienia jakichkolwiek znacznych zmian w danych.  Aby uzyskać szczegółowy opis statystyki zobacz temat [Statystyki][] w grupie opracowanie tematów.  Oto szybki przykład umożliwiające utworzenie tabeli statystyki załadowanego w tym przykładzie.

```sql
create statistics [SensorKey] on [Customer_Speed] ([SensorKey]);
create statistics [CustomerKey] on [Customer_Speed] ([CustomerKey]);
create statistics [GeographyKey] on [Customer_Speed] ([GeographyKey]);
create statistics [Speed] on [Customer_Speed] ([Speed]);
create statistics [YearMeasured] on [Customer_Speed] ([YearMeasured]);
```

## <a name="export-data-to-azure-blob-storage"></a>Eksportowanie danych z magazynem obiektów blob platformy Azure
W tej sekcji przedstawiono sposób eksportowania danych z magazynu danych SQL z magazynem obiektów blob platformy Azure. W tym przykładzie użyto Tworzenie zewnętrznych tabeli jako wybrać jest wysoce performant instrukcji Transact-SQL, aby wyeksportować dane równolegle ze wszystkich węzłów obliczeń.

Poniższy przykład tworzy tabeli zewnętrznej Weblogs2014 za pomocą danych z dbo i definicje kolumn. Tabela dzienniki w sieci Web. Definicja tabeli zewnętrznej są przechowywane w magazynie danych SQL i wyniki instrukcji SELECT są eksportowane do katalogu "/ archiwizowanie-log2014-" w kontenerze obiektów blob określony przez źródło danych. Dane są eksportowane w formacie pliku określony tekst.

```sql
CREATE EXTERNAL TABLE Weblogs2014 WITH
(
    LOCATION='/archive/log2014/',
    DATA_SOURCE=azure_storage,
    FILE_FORMAT=text_file_format
)
AS
SELECT
    Uri,
    DateRequested
FROM
    dbo.Weblogs
WHERE
    1=1
    AND DateRequested > '12/31/2013'
    AND DateRequested < '01/01/2015';
```


## <a name="working-around-the-polybase-utf-8-requirement"></a>Praca wokół wymagania PolyBase UTF-8
Na bieżącą PolyBase obsługuje ładowania plików danych, które zostały zakodowany UTF-8. Jak UTF-8 używa tego samego kodu znaku, jako ASCII PolyBase będzie również obsługa ładowania dane, które są ASCII zakodowany. Jednak PolyBase nie obsługuje inne kodowanie znaków, takich jak UTF-16-Unicode lub rozszerzonych znaków ASCII. Rozszerzone ASCII zawiera znaki z akcentami, takich jak znak umlaut, która jest powszechnie w języku niemieckim.

Aby obejść ten wymóg najlepszej odpowiedzi jest ponownie zapisać kodowania UTF-8.

Istnieje kilka sposobów wykonaj następujące czynności. Poniżej znajdują się dwie metody przy użyciu programu Powershell:

### <a name="simple-example-for-small-files"></a>Prosty przykład dla małych plików

Poniżej znajduje się jeden wiersz prosty skrypt programu Powershell, który tworzy plik.

```PowerShell
Get-Content <input_file_name> -Encoding Unicode | Set-Content <output_file_name> -Encoding utf8
```

O ile jest to prosty sposób ponownie kodowanie danych jest jednak w najbardziej skuteczne. W poniższym przykładzie przesyłanie strumieniowe Jo jest duża, znacznie szybsze i uzyskuje ten sam wynik.

### <a name="io-streaming-example-for-larger-files"></a>Przykład Streaming Jo większe pliki

Poniższy przykładowy kod jest bardziej złożony, ale podczas jej strumieniowego przesyłania wierszy danych ze źródła, aby ją uaktywnić jest wydajniejsza. Użyj tej metody dla większych plików.

```PowerShell
#Static variables
$ascii = [System.Text.Encoding]::ASCII
$utf16le = [System.Text.Encoding]::Unicode
$utf8 = [System.Text.Encoding]::UTF8
$ansi = [System.Text.Encoding]::Default
$append = $False

#Set source file path and file name
$src = [System.IO.Path]::Combine("C:\input_file_path\","input_file_name.txt")

#Set source file encoding (using list above)
$src_enc = $ansi

#Set target file path and file name
$tgt = [System.IO.Path]::Combine("C:\output_file_path\","output_file_name.txt")

#Set target file encoding (using list above)
$tgt_enc = $utf8

$read = New-Object System.IO.StreamReader($src,$src_enc)
$write = New-Object System.IO.StreamWriter($tgt,$append,$tgt_enc)

while ($read.Peek() -ne -1)
{
    $line = $read.ReadLine();
    $write.WriteLine($line);
}
$read.Close()
$read.Dispose()
$write.Close()
$write.Dispose()
```

## <a name="next-steps"></a>Następne kroki
Aby dowiedzieć się więcej na temat przenoszenia danych do magazynu danych SQL, zobacz [Omówienie migracji danych][].

<!--Image references-->

<!--Article references-->
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Ładowanie danych za pomocą PolyBase]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Statystyki]: ./sql-data-warehouse-tables-statistics.md
[Omówienie migracji danych]: ./sql-data-warehouse-overview-migrate.md

<!--MSDN references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx

[CREATE EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/dn935022.aspx
[Tworzenie zewnętrznego formatu pliku (Transact-SQL)]: https://msdn.microsoft.com/library/dn935026) .aspx [tworzenie tabeli zewnętrznej (Transact-SQL)]: https://msdn.microsoft.com/library/dn935021.aspxx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/mt130698.aspx

[Tworzenie tabeli jako wybierz pozycję (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[INSERT...SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/ms174335.aspx
[CREATE MASTER KEY (Transact-SQL)]: https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189522.aspx
[CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189450.aspx

<!-- External Links -->
