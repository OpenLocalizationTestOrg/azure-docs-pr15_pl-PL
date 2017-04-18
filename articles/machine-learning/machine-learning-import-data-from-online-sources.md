<properties
    pageTitle="Importowanie danych do komputera nauki Studio ze źródeł danych w trybie online | Microsoft Azure"
    description="Jak zaimportować dane szkolenia Azure maszynowego uczenia Studio z różnych źródeł online."
    keywords="Importowanie danych, format danych, typów danych, źródeł danych, dane szkolenia"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="bradsev;garye" />


# <a name="import-data-into-azure-machine-learning-studio-from-various-online-data-sources-with-the-import-data-module"></a>Importowanie danych do Azure maszynowego uczenia Studio z różnych źródeł danych z modułem importowanie danych

W tym artykule opisano obsługę importowanie danych z różnych źródeł i informacje potrzebne do przeniesienia danych z tych źródeł do doświadczenia Azure maszynowego uczenia.

> [AZURE.NOTE] Ten artykuł zawiera ogólne informacje o [Importowanie danych] [ import-data] modułu. Uzyskać więcej informacji na temat typów danych, możesz skorzystać z formatów, parametry i odpowiedzi na często zadawane pytania, zobacz temat informacje modułu [Importowanie danych] [ import-data] modułu.

<!-- -->

[AZURE.INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

## <a name="introduction"></a>Wprowadzenie

Można dostęp danych z poziomu Studio nauki komputera Azure z jednej z kilku źródeł danych jest uruchomiona z doświadczenia przy użyciu [Importowanie danych] [ import-data] modułu:

- Adres URL sieci Web przy użyciu protokołu HTTP
- Hadoop przy użyciu HiveQL
- Magazyn obiektów blob platformy Azure
- Tabel platformy Azure
- Azure SQL database lub SQL Server na maszyn wirtualnych Azure
- Lokalnej bazy danych SQL Server
- Dostawca OData obecnie strumieniowego źródła danych
 
Przepływ pracy dla prowadzenia doświadczenia w Azure maszynowego uczenia Studio składa się z przeciąganie i upuszczanie składników na obszar roboczy. Aby uzyskać dostęp do źródeł danych w trybie online, dodać [Importowanie danych] [ import-data] modułu do swojego doświadczenia wybierz **źródło danych**, a następnie przekaż parametry potrzebne do uzyskania dostępu do danych. Źródła danych online, które są obsługiwane są wyszczególnione w poniższej tabeli. W poniższej tabeli podsumowano również formaty plików, które są obsługiwane i parametry, które są używane do uzyskiwania dostępu do danych.

Należy zauważyć, że ponieważ te dane szkolenia dotyczące uzyskiwania dostępu do swojego doświadczenia jest uruchomiona, jest on dostępny tylko w tym doświadczeniu. Przez porównanie danych przechowywanych w module zestawu danych są dostępne dla każdego doświadczenia w obszarze roboczym.

> [AZURE.IMPORTANT] Obecnie [Importowanie danych] [ import-data] i [Eksportowanie danych] [ export-data] moduły można odczytywanie i zapisywanie danych tylko z Azure magazynu utworzony przy użyciu modelu wdrożenia klasyczny. Innymi słowy nowy typ klienta magazyn obiektów Blob platformy Azure oferuje poziomu dostępu ciepłej miejsca do magazynowania lub poziomu dostępu atrakcyjne miejsca do magazynowania nie jest jeszcze obsługiwane. 

> Zazwyczaj kont Azure magazynowania może zostać utworzone przed uzyskiwali tej opcji usługa nie narusza. Jeśli chcesz utworzyć nowe konto, zaznacz **klasycznego** modelu wdrożenia lub za pomocą Menedżera zasobów i dla **rodzaju konta**, wybierz pozycję **uniwersalny** zamiast **Magazyn obiektów Blob**. 

> Aby uzyskać więcej informacji, zobacz [Magazyn obiektów Blob platformy Azure: ciepłej i atrakcyjne poziomów miejsca do magazynowania](../storage/storage-blob-storage-tiers.md).



## <a name="supported-online-data-sources"></a>Obsługiwane źródła danych w trybie online
Moduł usługi Azure maszynowego uczenia **Importowanie danych** obsługuje następujących źródeł danych:

Źródła danych | Opis | Parametry |
---|---|---|
Adres URL sieci Web za pośrednictwem protokołu HTTP |Odczytuje dane w wartości rozdzielanych przecinkami (CSV), karta w formacie CSV (TSV), format pliku relacja atrybutu (ARFF) i formatów maszyn wektorowa pomocy technicznej (SVM light), adres URL sieci web, która korzysta z protokołu HTTP | <b>Adres URL</b>: Określa pełną nazwę pliku, w tym adres URL witryny i nazwę pliku, z dowolnymi. <br/><br/><b>Format danych</b>: umożliwia określenie jednego z obsługiwanych danych formaty: CSV, TSV, ARFF lub SVM light. Jeśli dane mają wiersz nagłówka, służy do przypisywania nazw kolumn.|
Usługi Hadoop-HDFS|Odczytuje dane z rozkładem miejsca do magazynowania w Hadoop. Określ potrzebne przy użyciu HiveQL, języka kwerend SQL podobne dane. HiveQL można również agregowanie danych i wykonać filtrowania przed dodaniem danych do komputera nauki Studio danych.|<b>Gałąź zapytania bazy danych</b>: Określa kwerendy gałęzi użyto w celu wyświetlenia danych.<br/><br/><b>Identyfikator URI serwera HCatalog</b> : określone nazwy klaster w formacie * &lt;nazwy klaster&gt;. azurehdinsight.net.*<br/><br/><b>Nazwa konta użytkownika Hadoop</b>: Określa nazwę konta użytkownika Hadoop, używane do obsługi administracyjnej klaster.<br/><br/><b>Hasło do konta użytkownika Hadoop</b> : Określa poświadczenia podczas inicjowania obsługi administracyjnej klaster. Aby uzyskać więcej informacji zobacz [Tworzenie Hadoop klastrów w HDInsight](../hdinsight/hdinsight-provision-clusters.md).<br/><br/><b>Lokalizację danych wyjściowych</b>: umożliwia określenie, czy dane są przechowywane w rozproszonej system plików usługi Hadoop (HDFS) lub Azure. <br/><ul>Jeśli dane są przechowywane w HDFS, określ serwer HDFS identyfikatora URI. (Należy użyć nazwy klaster HDInsight bez prefiksu HTTPS://). <br/><br/>Jeśli dane wyjściowe są przechowywane w Azure, podaj nazwę konta magazynu platformy Azure, klawisz dostępu miejsca do magazynowania i nazwa kontenera miejsca do magazynowania.</ul>|
Baza danych SQL |Odczytuje danych, który jest przechowywany w bazie danych programu Azure SQL lub w bazie danych programu SQL Server uruchomionych Azure maszyn wirtualnych. | <b>Nazwa serwera bazy danych</b>: Nazwa serwera, na którym działa bazy danych.<br/><ul>W przypadku bazy danych SQL Azure wprowadź nazwę serwera, który jest generowany. Zazwyczaj ma formularza * &lt;generated_identifier&gt;. database.windows.net.* <br/><br/>W przypadku programu SQL server obsługiwany na komputerze wirtualnych Azure wprowadź *tcp:&lt;nazwy DNS maszyn wirtualnych&gt;, 1433*</ul><br/><b>Nazwa bazy danych </b>: Określa nazwę bazy danych na serwerze. <br/><br/><b>Nazwa konta użytkownika serwera</b>: nazwa użytkownika konta, które ma uprawnienia dostępu do bazy danych. <br/><br/><b>Hasło do konta użytkownika serwera</b>: Określa hasło dla konta użytkownika.<br/><br/><b>Zaakceptuj wszystkie certyfikat</b>: Użyj tej opcji (mniej bezpieczne), jeśli chcesz pominąć przeglądanie certyfikatu witryny przed przeczytaniem danych.<br/><br/><b>Kwerendy bazy danych</b>: wpisz instrukcję SQL, opisujące dane chcesz odczytać.|
Lokalnej bazy danych SQL |Odczytuje dane, które są przechowywane w bazie danych SQL lokalnego. | <b>Brama danymi</b>: Nazwa bramy zarządzania danymi, które są zainstalowane na komputerze, na którym będzie miał dostęp do bazy danych programu SQL Server. Aby uzyskać informacje na temat konfiguracji bramy zobacz [Wykonywanie zaawansowanych analizy z Azure nauki komputera przy użyciu danych z lokalnego programu SQL server](machine-learning-use-data-from-an-on-premises-sql-server.md).<br/><br/><b>Nazwa serwera bazy danych</b>: Nazwa serwera, na którym działa bazy danych.<br/><br/><b>Nazwa bazy danych </b>: Określa nazwę bazy danych na serwerze. <br/><br/><b>Nazwa konta użytkownika serwera</b>: nazwa użytkownika konta, które ma uprawnienia dostępu do bazy danych. <br/><br/><b>Nazwa użytkownika i hasło</b>: kliknij pozycję <b>Wprowadź wartości</b> o podanie poświadczeń bazy danych. Za pomocą zintegrowanego uwierzytelniania systemu Windows lub uwierzytelnianie programu SQL Server w zależności od sposobu skonfigurowania lokalnego serwera SQL.<br/><br/><b>Kwerendy bazy danych</b>: wpisz instrukcję SQL, opisujące dane chcesz odczytać.|
Tabel platformy Azure|Odczytuje dane z tabeli usługi w magazynie Azure.<br/><br/>Jeśli rzadko możesz odczytać dużych ilości danych, za pomocą usługi Azure tabeli. Udostępnia elastyczne-relacyjnych (NoSQL), umożliwiające skalowalna, tanie oraz wysokiej dostępności masowej.| Opcje w oknie **Importowanie danych** zmieniają się w zależności od tego, czy dostęp do informacji publicznej lub konta prywatne miejsca do magazynowania, która wymaga poświadczeń logowania. To jest określona przez <b>Typ uwierzytelniania</b> ma wartość "PublicOrSAS" lub "Konta", z których każda ma własny zestaw parametrów. <br/><br/><b>Publicznych lub udostępnionych dostępu podpisu (SA) identyfikatora URI</b>: parametry są:<br/><br/><ul><b>Identyfikator URI tabeli</b>: Określa publiczna lub adresu URL skojarzeń zabezpieczeń dla tabeli.<br/><br/><b>Określa wiersze skanowanie w poszukiwaniu nazwy właściwości</b>: wartości są <i>TopN</i> , aby zeskanować o określoną liczbę wierszy lub <i>ScanAll</i> uzyskiwania wszystkich wierszy w tabeli. <br/><br/>Jeśli dane są jednolite i przewidywalne, zaleca się wybierz pozycję *TopN* , a następnie wprowadź numer N. Dla dużych tabel może to powodować szybsze czasów odczytu.<br/><br/>Jeśli dane składa się z zestawami właściwości, które różnią się w zależności od głębokości i położenie tabeli, wybierz opcję *ScanAll* skanowania wszystkich wierszy. Dzięki temu integralności wyniku właściwości i konwersji metadanych.<br/><br/></ul><b>Konto miejsca do magazynowania prywatne</b>: parametry są: <br/><br/><ul><b>Nazwa konta</b>: Określa nazwę konta, które zawiera tabelę do odczytu.<br/><br/><b>Klucz konta</b>: Określa klucz miejsca do magazynowania skojarzone z kontem.<br/><br/><b>Nazwa tabeli</b> : Nazwa tabeli zawierającej dane, których chcesz odczytać.<br/><br/><b>Wiersze skanowanie w poszukiwaniu nazwy właściwości</b>: wartości są <i>TopN</i> , aby zeskanować o określoną liczbę wierszy lub <i>ScanAll</i> uzyskiwania wszystkich wierszy w tabeli.<br/><br/>Jeśli dane są jednolite i przewidywalne, zalecamy wybierz *TopN* , a następnie wprowadź numer N. Dla dużych tabel może to powodować szybsze czasów odczytu.<br/><br/>Jeśli dane składa się z zestawami właściwości, które różnią się w zależności od głębokości i położenie tabeli, wybierz opcję *ScanAll* skanowania wszystkich wierszy. Dzięki temu integralności wyniku właściwości i konwersji metadanych.<br/><br/>|
Magazyn obiektów Blob platformy Azure | Odczytuje dane przechowywane w usłudze obiektów Blob w magazynie Azure, łącznie z obrazami, niestrukturalne tekst lub dane binarne.<br/><br/>Za pomocą usługi obiektów Blob publicznie udostępniania danych lub prywatnie przechowywane dane aplikacji. Można uzyskać dostęp do danych z dowolnego miejsca przy użyciu połączenia protokołu HTTP lub HTTPS. | Opcje w module **Importowanie danych** zmieniają się w zależności od tego, czy dostęp do informacji publicznej lub konta prywatne miejsca do magazynowania, która wymaga poświadczeń logowania. To jest określona przez <b>Typ uwierzytelniania</b> , który może zawierać wartość "PublicOrSAS" lub "Konta".<br/><br/><b>Publicznych lub udostępnionych dostępu podpisu (SA) identyfikatora URI</b>: parametry są:<br/><br/><ul><b>Identyfikator URI</b>: Określa publiczna lub adresu URL skojarzeń zabezpieczeń dla obiektów blob miejsca do magazynowania.<br/><br/><b>Format pliku</b>: Określa format danych w usłudze obiektów Blob. Obsługiwane formaty to CSV, TSV i ARFF.<br/><br/></ul><b>Konto miejsca do magazynowania prywatne</b>: parametry są: <br/><br/><ul><b>Nazwa konta</b>: Określa nazwę konta, które zawiera blob, który chcesz przeczytać.<br/><br/><b>Klucz konta</b>: Określa klucz miejsca do magazynowania skojarzone z kontem.<br/><br/><b>Ścieżka do kontenera, katalog lub obiektów blob</b> : nazwa obiektów blob, zawierający dane, do odczytu.<br/><br/><b>Format pliku obiektów blob</b>: Określa format danych w usłudze obiektów blob. Formaty danych obsługiwane są CSV, TSV, ARFF, CSV z określoną kodowanie i programu Excel. <br/><br/><ul>W przypadku formatu CSV lub TSV, pamiętaj wskazać, czy plik zawiera wiersz nagłówka.<br/><br/>Opcja programu Excel do odczytywania danych ze skoroszytów programu Excel. W przypadku opcji <i>format danych w programie Excel</i> wskazują, czy dane są w zakresie arkusza programu Excel lub w tabeli programu Excel. W przypadku opcji <i>arkusza programu Excel lub osadzonego tabeli </i>Określ nazwę arkusza lub tabeli, którą chcesz odczytać z.</ul><br/>|
Dostawca strumieniowych źródeł danych | Odczytuje dane z obsługiwanych dostawcy podawania. Obecnie jest obsługiwana tylko format protokołu Open Data (OData). | <b>Typ zawartości danych</b>: Określa OData format.<br/><br/><b>Źródłowy adres URL</b>: Określa pełnego adresu URL dla źródła danych. <br/>Na przykład następujący adres URL odczytuje z przykładowej bazy danych Northwind: http://services.odata.org/northwind/northwind.svc/|


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C/
