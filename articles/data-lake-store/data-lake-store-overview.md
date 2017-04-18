<properties
   pageTitle="Omówienie magazynu Lake danych Azure | Microsoft Azure"
   description="Opis, co to jest Azure magazynu Lake danych i wartość, która zapewnia on przez innych magazynów"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="overview-of-azure-data-lake-store"></a>Omówienie magazynu Lake danych Azure

Magazyn Lake danych Azure to repozytorium wyraźny obejmujących obciążenia analitycznym duży danych. Azure Lake danych pozwala na przechwytywanie danych każdej rozmiar, typ i spożyciu prędkości w jednym miejscu pojedynczego działania i badawczych analiz.

> [AZURE.TIP] Za pomocą funkcji [ścieżka nauki magazynu Lake danych](https://azure.microsoft.com/documentation/learning-paths/data-lake-store-self-guided-training/) Rozpocznij poznawanie usługi Azure danych Lake magazynu.

Azure magazynu Lake danych są dostępne z Hadoop (dostępne z klastrem HDInsight) za pomocą interfejsów API pozostałych zgodnego z programem WebHDFS. Przeznaczone do włączyć analizy na przechowywane dane, a jest dostosowanych do wydajność dla scenariuszy analizy danych. Okno zawiera wszystkie funkcje klasy korporacyjnej — zabezpieczenia, zarządzanie, skalowalność, niezawodność i dostępność — niezbędne dla przedsiębiorstwa rzeczywisty przypadków użycia.


![Azure Lake danych](./media/data-lake-store-overview/data-lake-store-concept.png)

Niektóre z najważniejszych funkcji Azure Lake danych są następujące.

### <a name="built-for-hadoop"></a>Utworzono dla Hadoop

Magazyn Azure Lake danych jest Apache system plików usługi Hadoop zgodny z System plików Distributed Hadoop (HDFS) i współpraca z ekosystemie Hadoop.  Istniejące aplikacje HDInsight lub usług, które korzystają z interfejsu API WebHDFS łatwo można zintegrować z magazynu Lake danych. Magazyn Lake danych udostępnia również interfejsu REST zgodnego z programem WebHDFS dla aplikacji

Dane przechowywane w magazynie Lake danych można łatwo analizować za pomocą Hadoop analitycznym struktury, takich jak MapReduce lub gałęzi. Usługa HDInsight systemu Microsoft Azure klastrów można obsługi administracyjnej i skonfigurowane tak, aby uzyskać bezpośredni dostęp do danych przechowywanych w magazynie Lake danych.

### <a name="unlimited-storage-petabyte-files"></a>Nieograniczoną miejscem do magazynowania, petabyte plików

Sklep Lake danych Azure umożliwia przechowywanie nieograniczoną i nadaje się do przechowywania różnych danych w celu analizy. Nie nakłada jakichkolwiek ograniczeń rozmiary konta, rozmiarów plików lub ilości danych, które mogą być przechowywane w lake danych. Pojedyncze pliki zakresu od KB do petabytes w rozmiarze co doskonałe rozwiązanie do przechowywania danych dowolnego typu. Dane są przechowywane trwale dokonując wielu kopii, a nie ma żadnych ograniczeń na czas, dla których dane mogą być przechowywane w lake danych.

### <a name="performance-tuned-for-big-data-analytics"></a>Wydajność dostosowanych do analizy danych duży

Azure magazynu Lake danych jest przeznaczony dla systemami dużą skalę analityczny wymagających dużych przepustowość kwerendy i analizowanie dużych ilości danych. Lake danych strony widzące części pliku ilość miejsca do magazynowania poszczególnych serwerach. Ulepszony odczytu przepustowość, podczas czytania pliku równolegle do wykonywania analizy danych.


### <a name="enterprise-ready-highly-available-and-secure"></a>Gotowe Enterprise: Wysokiej dostępności i bezpieczeństwo

Magazyn Lake danych Azure zapewnia dostępność standardowych i niezawodności. Aktywów dane są przechowywane trwale tworząc zbędne kopie, aby zabezpieczyć się przed wszelkie nieoczekiwane błędy. Przedsiębiorstw można użyć Lake danych Azure w ich rozwiązania jako ważną częścią platformy istniejących danych.

Magazyn Lake danych również zapewnia zabezpieczenie klasy korporacyjnej przechowywane dane. Aby uzyskać więcej informacji zobacz [Zabezpieczanie danych w magazynie Lake danych Azure](#DataLakeStoreSecurity).


### <a name="all-data"></a>Wszystkie dane

Azure magazynu Lake danych można przechowywać wszystkie dane w ich pierwotnych formatach jest, bez konieczności wszelkie poprzednich przekształcenia. Magazyn Lake danych nie wymaga schematu określonych przed załadowaniu danych pozostawić go do poszczególnych strukturę analityczna interpretowania danych i zdefiniować schematu podczas przeprowadzania analizy. Możliwość przechowywania plików dowolnego rozmiarów i formatów umożliwia magazynu Lake danych powinien obsługiwać dane strukturalne, półstrukturalne i niestrukturalne.

Kontenery magazynu Lake danych Azure danych są zasadniczo foldery i pliki. Możesz pracować na przechowywane dane przy użyciu SDK, Azure Portal i Azure programu Powershell. Jak długo umieszczeniu danych w magazynie przy użyciu tych interfejsów i używanie odpowiednie opakowania mogą zawierać dowolny typ danych. Magazyn Lake danych nie jest sprawdzana dowolnego specjalnej obsługi danych oparty na typie danych przechowywanych w niej.


## <a name="DataLakeStoreSecurity"></a>Zabezpieczanie danych w magazynie Lake danych Azure

Magazyn Lake danych Azure korzysta z usługi Azure Active Directory dla uwierzytelniania i listy kontroli dostępu (ACL) do zarządzania dostęp do danych.

| Funkcja                                 | Opis                              |
|-----------------------------------------|------------------------------------------|
| Uwierzytelnianie | Azure magazynu Lake danych można zintegrować z Azure Active Directory (AAD) do zarządzania tożsamości i dostęp do wszystkich danych przechowywanych w magazynie Lake danych Azure. W wyniku integracji Lake danych Azure korzyści ze wszystkich funkcji AAD tym uwierzytelnianie wieloskładnikowe, dostępu warunkowego, kontrola dostępu oparta na rolach, monitorowania zastosowania aplikacji, zabezpieczeń monitorowania i alertów z wyprzedzeniem, itd. Magazyn Lake danych Azure obsługuje protokół OAuth 2.0 do uwierzytelniania przy użyciu interfejsu pozostałych. |
| Kontrola dostępu                          | Magazyn Lake danych Azure udostępnia kontrola dostępu dzięki obsłudze uprawnienia POSIX ujawnionego przez protokół WebHDFS. W bieżącej wersji ACL można włączyć na główny folder, podfoldery, jak również pojedyncze pliki. ACL, które możesz zastosować do folderu głównego spowoduje również stosowane do wszystkich podrzędnych folderów i plików oraz.|

Chcesz dowiedzieć się więcej o zabezpieczaniu danych w magazynie Lake danych. Skorzystaj z poniższych łączy.

* Aby uzyskać instrukcje dotyczące do zabezpieczania danych w magazynie Lake danych zobacz [Zabezpieczanie danych w magazynie Lake danych Azure](data-lake-store-secure-data.md).
* Wolisz wideo? [Obejrzyj ten klip wideo](https://mix.office.com/watch/1q2mgzh9nn5lx) na temat zabezpieczyć dane przechowywane w magazynie Lake danych.

## <a name="applications-compatible-with-azure-data-lake-store"></a>Aplikacje zgodne z magazynu Lake danych Azure

Azure magazynu Lake danych jest zgodny z najbardziej otwartych składniki źródła w ekosystemie Hadoop. Integruje także właściwego z innymi usługami Azure. Dzięki temu magazynu Lake danych idealnego opcji dla potrzeb miejsca do magazynowania danych. Wykonaj poniższe łącza, aby dowiedzieć się więcej na temat sposobu użycia magazynu Lake danych zarówno z składniki Otwórz źródło, a także innych usług Azure.

* Aby uzyskać listę aplikacji Otwórz źródło współdziała z magazynu Lake danych, zobacz [aplikacji i usług, które są zgodne z magazynu Lake danych Azure](data-lake-store-compatible-oss-other-applications.md) .
* Zobacz [Integracja z innymi usługami Azure](data-lake-store-integrate-with-other-services.md) , aby dowiedzieć się, jak magazynu Lake danych można z innymi usługami Azure umożliwiające szerszej grupie scenariuszy.
* Zobacz [scenariusze korzystania z magazynu Lake danych](data-lake-store-data-scenarios.md) , aby dowiedzieć się, jak używać magazynu Lake danych w scenariuszy, takich jak ingesting danych, przetwarzanie danych pobierania danych i wizualizacji danych.

## <a name="what-is-azure-data-lake-store-file-system-adl"></a>Co to jest Azure danych Lake magazyn plików systemu Windows (adl: / /)?

Magazyn Lake danych są dostępne nowe systemu plików AzureDataLakeFilesystem (adl: / /), w środowiskach Hadoop (dostępne z klastrem HDInsight). Aplikacje i usługi, które używają adl: / / będą mogli korzystać z dalszej optymalizacji wydajności nie są obecnie dostępne w WebHDFS. W wyniku magazynu Lake danych pozwala albo korzystać najlepszą wydajność z opcją zalecane używania adl: / / lub zachować istniejący kod kontynuując bezpośrednio za pomocą interfejsu API WebHDFS. Usługa Azure HDInsight wykorzystuje w pełni AzureDataLakeFilesystem, aby zapewnić najlepszą wydajność magazynu Lake danych.

Można uzyskać dostęp do danych przy użyciu magazynu Lake danych `adl://<data_lake_store_name>.azuredatalakestore.net`. Aby uzyskać więcej informacji na temat uzyskiwania dostępu do danych w magazynie Lake danych, zobacz [Wyświetlanie właściwości przechowywane dane](data-lake-store-get-started-portal.md#properties)

## <a name="how-do-i-start-using-azure-data-lake-store"></a>Jak rozpocząć korzystanie z magazynu Lake danych Azure?

Zobacz [Rozpoczynanie pracy z magazynu Lake danych przy użyciu Azure Portal](data-lake-store-get-started-portal.md), w sposób inicjowania obsługi magazynu Lake danych przy użyciu Azure Portal. Gdy masz obsługi administracyjnej Lake danych Azure, możesz dowiedzieć się, jak używać ofert duży danych, takich jak analizy Lake danych Azure lub usługa Azure HDInsight z magazynu Lake danych. Można także tworzyć aplikacji .NET utworzyć konto Azure magazynu Lake danych i wykonywania operacji, takich jak danych przekazywania, pobierania danych itp.

- [Wprowadzenie do analizy Lake Azure danych](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Usługa Azure HDInsight za pomocą magazynu Lake danych](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Rozpoczynanie pracy z magazynu Lake danych Azure przy użyciu zestawu SDK .NET](data-lake-store-get-started-net-sdk.md)


## <a name="data-lake-store-videos"></a>Klipy wideo magazynu Lake danych

Jeśli wolisz oglądanie klipów wideo, aby dowiedzieć się, magazynu Lake danych zawiera klipy wideo na wiele funkcji.

* [Utwórz konto Azure magazynu Lake danych](https://mix.office.com/watch/1k1cycy4l4gen)
* [Zarządzanie danymi w magazynie Lake danych Azure za pomocą Eksploratora danych](https://mix.office.com/watch/icletrxrh6pc)
* [Łączenie danych Azure Lake analizy z magazynu Lake danych Azure](https://mix.office.com/watch/qwji0dc9rx9k)
* [Dostęp Azure danych Lake magazynu za pomocą analizy Lake danych](https://mix.office.com/watch/1n0s45up381a8)
* [Łączenie usługa Azure HDInsight z magazynu Lake danych Azure](https://mix.office.com/watch/l93xri2yhtp2)
* [Magazynu Lake danych programu Access Azure za pomocą gałęzi i świnka](https://mix.office.com/watch/1n9g5w0fiqv1q)
* [Skopiuj dane do i z magazynu Lake danych Azure za pomocą DistCp (Hadoop Distributed kopia)](https://mix.office.com/watch/1liuojvdx6sie)
* [Przenoszenie danych między relacyjnych źródeł i magazynu Lake danych Azure za pomocą Apache Sqoop](https://mix.office.com/watch/1butcdjxmu114)
* [Aranżacji danych przy użyciu Factory danych Azure dla magazynu Lake danych Azure](https://mix.office.com/watch/1oa7le7t2u4ka)
* [Zabezpieczanie danych w magazynie Lake danych Azure](https://mix.office.com/watch/1q2mgzh9nn5lx)



