<properties
    pageTitle="Tworzenie indeksu wyszukiwania Azure | Microsoft Azure | Usługa wyszukiwania hostowanej chmury"
    description="Co to jest indeks wyszukiwania Azure i jak są one używane?"
    services="search"
    manager="jhubbard"
    documentationCenter=""
    authors="ashmaka"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="create-an-azure-search-index"></a>Tworzenie indeksu wyszukiwania Azure
> [AZURE.SELECTOR]
- [Omówienie](search-what-is-an-index.md)
- [Portal](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [POZOSTAŁE](search-create-index-rest-api.md)

## <a name="what-is-an-index"></a>Co to jest indeks?

*Indeks* jest Magazyn trwały *dokumentów* i innych konstrukcji używane przez usługę wyszukiwania Azure. Dokument jest jednostką wyszukiwania danych w indeksie. Na przykład u sprzedawcy detalicznego elektronicznego mogą mieć dokumentu dla każdego elementu, który sprzedaży, organizacji wiadomości może być dokumentu dla każdego artykułu, itp. Mapowanie poniższe pojęcia na bardziej przyjaznej odpowiedniki bazy danych: *indeks* jest z definicji podobny do *tabeli*, a *dokumenty* są w przybliżeniu równa *wierszy* w tabeli.

Dodawanie i przekaż dokumenty i przesyłanie kwerend wyszukiwania do wyszukiwania Azure powoduje przesłaniu wezwaniach do określonego indeksu w tej usługi wyszukiwania.

## <a name="field-types-and-attributes-in-an-azure-search-index"></a>Typy pól i atrybuty do indeksu wyszukiwania Azure

Podczas definiowania schematu Podaj nazwę, typ i atrybuty poszczególnych pól w indeksie. Pole klasyfikowane jako dane, które są przechowywane w tym polu. Atrybuty są ustawione na poszczególnych pól Określ, jak to pole jest używane. Poniższe tabele wyliczanie typów i atrybuty, które można określić.


### <a name="field-types"></a>Typy pól
|Typ|Opis|
|------------|-----------|
|*Edm.String*|Tekst, który opcjonalnie można tokenized wyszukiwanie pełnotekstowe (dzielenia wyrazów wynikających, itp).|
|*Collection(EDM.String)*|Lista ciągów, które opcjonalnie można tokenized dla przeszukiwania pełnego tekstu. Istnieje teoretyczna górny limit liczby elementów w kolekcji, ale maksymalnie 16 MB na rozmiar ładunku zastosowanie do kolekcji.|
|*Edm.Boolean*|Zawiera wartości PRAWDA i FAŁSZ.|
|*Edm.Int32*|32-bitowych liczb całkowitych.|
|*Edm.Int64*|64-bitowych liczb całkowitych.|
|*Edm.Double*|Dane liczbowe podwójnej precyzji.|
|*Edm.DateTimeOffset*|Wartości czasu reprezentowane w formacie 4 OData daty (np. `yyyy-MM-ddTHH:mm:ss.fffZ` lub `yyyy-MM-ddTHH:mm:ss.fff[+/-]HH:mm`).|
|*Edm.GeographyPoint*|Punkt reprezentującą lokalizacji geograficznej na całym świecie.|

Można znaleźć więcej szczegółowych informacji na temat wyszukiwania Azure [obsługiwane typy danych w witrynie MSDN](https://msdn.microsoft.com/library/azure/dn798938.aspx).



### <a name="field-attributes"></a>Atrybuty pól
|Atrybut|Opis|
|------------|-----------|
|*Klawisz*|Ciąg, który zawiera unikatowy identyfikator każdego dokumentu na potrzeby wyszukiwania dokumentu. Każdy indeks musi mieć jeden klawisz. Tylko jedno pole może być klawisza i jego typ musi być ustawiona na Edm.String.|
|*Uwzględnianej podczas pobierania wyników*|Określa, czy pole może zostać zwrócony w wynikach wyszukiwania.|
|*Podatne*|Umożliwia pola do użycia w kwerendach filtru.|
|*Uwzględnianą w sortowaniu*|Umożliwia zapytania do sortowania wyników wyszukiwania za pomocą tego pola.|
|*Facetable*|Umożliwia pola do użycia w strukturze [nawigacji aspektowej](search-faceted-navigation.md) samodzielnej nauki filtrowania użytkownika. Zazwyczaj pola zawierające powtarzające się wartości, które służą do grupowania wielu dokumentów (na przykład wielu dokumentów zaliczanych do jednej marki lub usługi kategorii) najlepiej jako aspekty.|
|*Można wyszukiwać*|Zaznacza pole jako pełnotekstowym wyszukiwania.|

Można znaleźć więcej szczegółowych informacji na temat wyszukiwania Azure [atrybuty indeksu w witrynie MSDN](https://msdn.microsoft.com/library/azure/dn798941.aspx).



## <a name="guidance-for-defining-an-index-schema"></a>Wskazówki dotyczące definiowania schematu indeksu

Podczas projektowania indeksu, Poświęć trochę czasu na etapie planowania myśleć za pośrednictwem każdej decyzji. Należy zachować wyszukiwania użytkownika środowiska i małych firm potrzeb pamiętać podczas projektowania indeksu podczas każdego pola, musi mieć przypisane [atrybuty pisane z wielkiej litery](https://msdn.microsoft.com/library/azure/dn798941.aspx). Zmienianie indeksu po wdrożeniu obejmuje odbudowanie i ponowne ładowanie danych.


Zmiana wymagania dotyczące przechowywania danych w czasie można zwiększyć lub zmniejszyć wydajność przez dodanie lub usunięcie partycje. Aby uzyskać szczegółowe informacje zobacz [Zarządzanie usługą wyszukiwania w Azure](search-manage.md) lub [Ograniczenia usługi](search-limits-quotas-capacity.md).
