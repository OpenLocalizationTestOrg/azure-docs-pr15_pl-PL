<properties
    pageTitle="Tworzenie indeksu wyszukiwania Azure za pomocą portalu Azure | Microsoft Azure | Usługa wyszukiwania hostowanej chmury"
    description="Tworzenie indeksu przy użyciu Azure Portal."
    services="search"
    manager="jhubbard"
    authors="ashmaka"
    documentationCenter=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="create-an-azure-search-index-using-the-azure-portal"></a>Tworzenie indeksu wyszukiwania Azure za pomocą portalu Azure
> [AZURE.SELECTOR]
- [Omówienie](search-what-is-an-index.md)
- [Portal](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [POZOSTAŁE](search-create-index-rest-api.md)

Ten artykuł prowadzi użytkownika przez proces tworzenia Azure wyszukiwania [indeksu](search-what-is-an-index.md) przy użyciu Azure Portal.

Przed wykonywaniem tego przewodnika i tworzenie indeksu, należy użyć już [utworzone usługa Azure wyszukiwania](search-create-service-portal.md).


## <a name="i-go-to-your-azure-search-blade"></a>I. Przejdź do swojej karta wyszukiwanie Azure
1. Kliknij pozycję "Wszystkich zasobów" w menu po lewej stronie [Azure Portal](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices)
2. Wybierz pozycję usługi Azure wyszukiwania

## <a name="ii-add-and-name-your-index"></a>II. Dodawanie i nadaj nazwę indeksu
1. Kliknij przycisk "Dodaj index"
2. Nadaj nazwę indeksu wyszukiwania Azure. Ponieważ jest tworzone indeksu wyszukiwania hotele w tym przewodniku, możemy mają o nazwie indeksu "hotele".
  * Nazwa indeksu musi rozpoczynać się literą i zawierają tylko małe litery, cyfry lub kreski ("-").
  * Podobnie jak nazwę usługi, nazwa indeksu, które możesz wybrać będzie również część adresu URL punktu końcowego miejsce, w którym będzie wysyłać do żądania HTTP dla interfejsu API wyszukiwania Azure
3. Kliknij pozycję "Pola", aby otworzyć nowy kart

![](./media/search-create-index-portal/add-index.png)


## <a name="iii-create-and-define-the-fields-of-your-index"></a>III. Tworzenie i zdefiniuj pola indeksu
1. Wybierając pozycję "Pola" nowe karta zostanie otwarty z formularza do wprowadzania do definicji indeksu.
2. Dodawanie pól do indeksu przy użyciu formularza.

  * Pole *klucza* typu Edm.String jest wymagane dla każdej indeksu wyszukiwania Azure. Tego pola klucza jest tworzona domyślnie z pola Nazwa "identyfikator". Firma Microsoft zostały zmienione go na "hotelId" w indeksie.
  * Niektóre właściwości schematu indeksu można ustawić tylko raz i nie można zaktualizować w przyszłości. Z tego powodu wszelkie aktualizacje schematu, które wymagają ponownego indeksowania, takie jak zmiana typy pól nie są obecnie możliwe po początkowej konfiguracji.
  * Starannie wybrany wartości dla każdego pola, według jak firma Microsoft wydaje się, że będą używane w aplikacji. Podczas projektowania indeksu podczas każdego pola, musi mieć przypisane [odpowiednie właściwości](https://msdn.microsoft.com/library/azure/dn798941.aspx), należy pamiętać o potrzeb środowiska i małych firm użytkownika wyszukiwania. Zastosuj te Określanie właściwości wyszukiwania funkcji (filtrowanie, faceting, sortowanie, wyszukiwanie pełnotekstowe, itp.) do pola, które. Na przykład prawdopodobnie hotele, wyszukując osoby będą zainteresować dopasowania słowa kluczowego dla pola "opis", możemy włączyć wyszukiwanie pełnotekstowe dla tego pola przez ustawienie właściwości "Z możliwością wyszukiwania".
    * Można także ustawić [język analizatora](https://msdn.microsoft.com/en-us/library/azure/dn879793.aspx) dla każdego pola, klikając na karcie "Analizatora" w górnej części karta. Poniżej można zobaczyć w indeksie przeznaczone do tekstu w języku francuskim Wybraliśmy francuskiego analizatora dla pola.

3. Kliknij **przycisk OK** w karta "Pola", aby potwierdzić definicji pola
4. Kliknij **przycisk OK** na karta "Dodaj index", aby zapisać i utworzyć indeks, który właśnie zdefiniowane przez użytkownika.

W poniższych zrzutów ekranu można Zobacz, jak mają możemy o nazwie i zdefiniowanych pól dla indeksu "hotele".

![](./media/search-create-index-portal/field-definitions.png)

![](./media/search-create-index-portal/set-analyzer.png)

## <a name="next"></a>Następny
Po utworzeniu indeksu wyszukiwania Azure, będzie gotowy do [przekazania zawartość w indeksie](search-what-is-data-import.md) , możesz rozpocząć wyszukiwanie danych.
