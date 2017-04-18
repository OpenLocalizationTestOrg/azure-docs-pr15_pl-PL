<properties
    pageTitle="Przekazywanie danych w wyszukiwaniu Azure | Microsoft Azure | Usługa wyszukiwania hostowanej chmury"
    description="Dowiedz się, jak przekazać danych do indeksu wyszukiwania Azure."
    services="search"
    documentationCenter=""
    authors="ashmaka"
    manager="jhubbard"
    editor=""
    tags=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="upload-data-to-azure-search"></a>Przekazywanie danych do wyszukiwania Azure
> [AZURE.SELECTOR]
- [Omówienie](search-what-is-data-import.md)
- [.NET](search-import-data-dotnet.md)
- [POZOSTAŁE](search-import-data-rest-api.md)


## <a name="data-upload-models-in-azure-search"></a>Przekazywanie modeli Azure wyszukiwania danych
Istnieją dwa sposoby do wypełnienia kolumny indeksu wyszukiwania Azure z danymi. Pierwszą opcję ręcznie jest naciśnięcie danych do indeksu przy użyciu wyszukiwania Azure [Interfejsu API usługi REST](search-import-data-rest-api.md) lub [.NET SDK](search-import-data-dotnet.md). Jest to druga opcja jest, aby [wskazywały źródła danych obsługiwane](search-indexer-overview.md) indeksu wyszukiwania Azure i umożliwić wyszukiwania Azure automatycznie pobierać dane do usługi wyszukiwania.

Ten przewodnik obejmuje tylko instrukcje na temat korzystania z modelu wypychanych przekazywania danych, (który jest obsługiwana tylko w [Interfejsie API usługi REST](search-import-data-rest-api.md) i [.NET SDK](search-import-data-dotnet.md)), ale nadal można więcej informacji o modelu pobieraj poniżej.

### <a name="push-data-to-an-index"></a>Przekazania danych do indeksu

Tej metody dotyczą programowy wysyłanie danych do Azure Wyszukaj, aby był dostępny podczas wyszukiwania. Dla aplikacji o bardzo niskie opóźnienie wymagania (np. Jeśli potrzebujesz wyszukiwania operacji, które mają być synchronizowane z bazami danych dynamicznych zapasów), model wypychanych jest jedynym możliwym.

Za pomocą [Interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/dn798930.aspx) lub [.NET SDK](search-import-data-dotnet.md) do przekazania danych do indeksu. Jest obecnie nie obsługuje narzędzie położenia danych za pośrednictwem portalu.

Ta metoda jest bardziej elastyczne niż modelu pobieraj, ponieważ można przekazać dokumenty indywidualnie lub partiami (maksymalnie 1000 na partii lub 16 MB, niezależnie od limitu jest pierwsze). Model wypychanych umożliwia przekazywanie dokumentów do wyszukiwania Azure niezależnie od tego, gdzie się dane.

### <a name="pull-data-into-an-index"></a>Wciąganie danych do indeksu

Model pobieraj przeszukiwania źródła danych obsługiwane i automatycznie przekazuje dane do indeksu wyszukiwania Azure za Ciebie. Przez funkcję śledzenia zmian i usuwa do istniejących dokumentów oprócz rozpoznawanie nowych dokumentów, indeksatory usunąć konieczności aktywnie zarządzać danymi w indeksie.

W polu Wyszukaj Azure ta funkcja jest zaimplementowana za pośrednictwem *indeksatory*, obecnie dostępne dla [Magazyn obiektów Blob (wersja preview)](search-howto-indexing-azure-blob-storage.md), [DocumentDB](http://aka.ms/documentdb-search-indexer) [bazy danych Azure SQL i SQL Server na maszyny wirtualne Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md).

Funkcje indeksowania są prezentowane w [Azure Portal](search-import-data-portal.md) oraz jak [Interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/dn946891.aspx).
