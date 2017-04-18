<properties
   pageTitle="Tworzenie indeksu dla dokumentów w wielu językach w wyszukiwaniu Azure | Microsoft Azure | Usługa wyszukiwania hostowanej chmury"
   description=" Wyszukiwanie Azure obsługuje 56 języków, używanie programy do analizowania język z Lucene i przetwarzania w języku naturalnym technologii firmy Microsoft."
   services="search"
   documentationCenter=""
   authors="yahnoosh"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="na"
   ms.workload="search"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.date="07/14/2016"
   ms.author="jlembicz"/>

# <a name="create-an-index-for-documents-in-multiple-languages-in-azure-search"></a>Tworzenie indeksu dla dokumentów w wielu językach w wyszukiwaniu Azure
> [AZURE.SELECTOR]
- [Portal](search-language-support.md)
- [POZOSTAŁE](https://msdn.microsoft.com/library/azure/dn879793.aspx)
- [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.analyzername.aspx)

Unleashing możliwości języka programy do analizowania jest tak proste jak ustawienie jednej właściwości w polu wyszukiwania w definicji indeksu. Teraz można wykonać ten krok w portalu.

Poniżej przedstawiono zrzuty ekranu kart Azure Portal wyszukiwanie Azure Zezwalaj użytkownikom na definiowanie schematu indeksu. Z tego karta użytkownicy mogą tworzyć wszystkie pola i ustawić właściwość analizatora dla każdego z nich.

> [AZURE.IMPORTANT] Można tylko ustawić analizatora języka w definicji pola, jak podczas tworzenia nowego indeksu od podstaw w górę lub w przypadku dodanie nowego pola do istniejącego indeksu. Upewnij się, że określony wszystkich atrybutów, łącznie z analizatora podczas tworzenia pola. Nie będzie można edytować atrybuty lub zmienianie typu analizatora po zapisaniu zmiany.

## <a name="define-a-new-field-definition"></a>Zdefiniuj nową definicję pola

1. Zaloguj się do usługi [Azure Portal](https://portal.azure.com) i otwórz karta usługi usługi wyszukiwania.
2. Kliknij przycisk **Dodaj indeks** na pasku poleceń u góry pulpitu nawigacyjnego usługi, aby rozpocząć nowy indeks, lub Otwórz istniejący indeks do ustawiania analizatora na nowe pola, które dodajesz do istniejącego indeksu.
3. Pojawi się karta pól, opcjami schematów indeksu, w tym karty analizatora używany do wybierania analizatora języka.
4. W polach Rozpoczęcie definicji pola przez podanie nazwy, wybierając typ danych i ustawianie atrybuty, które mają zaznaczone pole jako pełny tekst można wyszukiwać, uwzględnianej podczas pobierania wyników w wynikach wyszukiwania, mogą być stosowane w struktury nawigacji Faseta, sortowaniu i tak dalej. 
5. Przed przejściem do następnego pola, otwórz kartę **analizatora** . 

   
![][1]
*Aby zaznaczyć analizatora, kliknij kartę analizatora na karta pola*

## <a name="choose-an-analyzer"></a>Wybierz pozycję analizatora

6. Przewiń, aby znaleźć pole, które są definiowane. 
7. Jeśli jeszcze tego nie oznaczone jako wyszukiwania, zaznacz pole wyboru teraz, aby oznaczyć wiadomość jako **z możliwością wyszukiwania**.
8. Kliknij obszar analizatora, aby wyświetlić listę dostępnych programy do analizowania.
9. Wybierz pozycję analizatora, którego chcesz użyć.

![][2]
*Wybierz jedną z obsługiwane programy do analizowania dla każdego pola*

Domyślnie wszystkie pola wyszukiwania za pomocą [analizatora Lucene standardowy](http://lucene.apache.org/core/4_10_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) , który jest niezależne od języka. Aby wyświetlić pełną listę obsługiwanych programy do analizowania, zobacz [Obsługa języków w wyszukiwaniu Azure](https://msdn.microsoft.com/library/azure/dn879793.aspx).

Po wybraniu analizatora języka dla pola będzie można używać z każdego żądania indeksowania i wyszukiwania dla tego pola. Kwerendy jest oparte na wielu polach przy użyciu różnych programy do analizowania, gdy kwerenda będą przetwarzane niezależnie przez prawo narzędzia analizy serwera dla każdego pola.

Wiele sieci web i aplikacji dla urządzeń przenośnych służyć użytkowników na całym świecie przy użyciu innych języków. Istnieje możliwość definiowania indeksu w scenariuszu tak przez utworzenie pola dla poszczególnych języków obsługiwanych.

![][3]
*Definicja indeksu przy użyciu pola opisu dla poszczególnych języków obsługiwanych*

Jeśli jest znany języka agenta wydawania kwerendy, żądania wyszukiwania można występujące w określonym polu przy użyciu parametru zapytania **searchFields** . Tylko przed opis w polskim zostanie wydana następującej kwerendy:

`https://[service name].search.windows.net/indexes/[index name]/docs?search=darmowy&searchFields=description_pl&api-version=2015-02-28`

Czy kwerendę indeksu w portalu za pomocą **Eksploratora wyszukiwania** , aby wkleić w kwerendzie podobny do tego, jak pokazano powyżej. Wyszukiwanie jest dostępne na pasku poleceń w karta usługi. Aby uzyskać szczegółowe informacje, zobacz [kwerendy indeksu wyszukiwania Azure w portalu](search-explorer.md) .

Czasami języka agenta wydawania kwerendy jest nieznany, w tym przypadku kwerendy mogą być wydawane dla wszystkich pól jednocześnie. W razie potrzeby preferencji wyniki w pewnym języku można zdefiniować za pomocą [wyników profile](https://msdn.microsoft.com/library/azure/dn798928.aspx). W poniższym przykładzie odpowiedników w polu Opis w języku angielskim będą punktowane wyższej względem dopasowania w Polski i francuskim:

    "scoringProfiles": [
      {
        "name": "englishFirst",
        "text": {
          "weights": { "description_en": 2 }
        }
      }
    ]

`https://[service name].search.windows.net/indexes/[index name]/docs?search=Microsoft&scoringProfile=englishFirst&api-version=2015-02-28`

Jeśli jesteś deweloperem .NET, zwróć uwagę skonfigurowania programy do analizowania języka przy użyciu [Zestawu SDK usługi Azure wyszukiwania .NET](http://www.nuget.org/packages/Microsoft.Azure.Search). Najnowszej wersji obsługuje Microsoft języka programy do analizowania także.

<!-- Image References -->
[1]: ./media/search-language-support/AnalyzerTab.png
[2]: ./media/search-language-support/SelectAnalyzer.png
[3]: ./media/search-language-support/IndexDefinition.png
