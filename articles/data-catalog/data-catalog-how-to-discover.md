<properties
   pageTitle="Jak odnaleźć źródła danych | Microsoft Azure"
   description="Artykule wyróżnianie sposoby wykrywania zasobami zarejestrowane dane za pomocą wykazu danych Azure, w tym wyszukiwania i filtrowania oraz przy użyciu przewaga wyróżnianie możliwości portal Azure wykazu danych."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/04/2016"
   ms.author="maroche"/>

# <a name="how-to-discover-data-sources"></a>Jak odnaleźć źródła danych

## <a name="introduction"></a>Wprowadzenie
**Wykaz danych Microsoft Azure** to usługa w chmurze w pełni zarządzane, która pełni funkcję systemu rejestracji i system odnajdowania dla źródła danych przedsiębiorstwa. Innymi słowy **Wykaz danych Azure** jest pomaga osobom odnajdowanie, opis i więcej korzyści z ich istniejących danych przy użyciu źródeł danych i organizacji Pomoc. Po zarejestrowaniu źródła danych dzięki **Wykazowi danych Azure**metadanych jest indeksowane przez usługę, tak, aby użytkownicy mogą łatwo przeszukiwać do znalezienia danych, które są potrzebne.

## <a name="searching-and-filtering"></a>Wyszukiwanie i filtrowanie

Odnajdowanie w **Wykazie danych Azure** używa dwa podstawowe mechanizmy: wyszukiwanie i filtrowanie.

Wyszukiwanie ma być intuicyjny i zaawansowanych — domyślnie, wyszukiwane terminy są dopasowywane do dowolnej właściwości w wykazie, łącznie z adnotacjami użytkownika.

Filtrowanie jest przeznaczony do uzupełnienia wyszukiwania. Użytkownicy mogą wybrać określonych cech, takich jak ekspertów, typ źródła danych, typ obiektu i znaczniki, aby wyświetlić tylko pasujące dane aktywa i aby ograniczyć wyniki wyszukiwania, aby także aktywa dopasowane.

Przy użyciu kombinacji wyszukiwania i filtrowania, użytkownicy mogą szybko przejść źródła danych, które zostały zarejestrowane w **Azure wykaz danych** do znalezienia źródła danych, które są potrzebne.

## <a name="search-syntax"></a>Składnia wyszukiwania

Mimo że wyszukiwania niezależnej domyślny jest proste i intuicyjnej, użytkownicy mogą używać również składni wyszukiwania **Wykazu danych usługi Azure**mieć większą kontrolę nad wynikami wyszukiwania. Wyszukiwanie w **Wykazie danych Azure** obsługuje następujące techniki:

| Metoda                 | Użyj                                                                                                                                     | Przykład                                                   |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| Podstawowe wyszukiwanie              | Podstawowe wyszukiwanie przy użyciu wyszukiwanych terminów. Wyniki są jakichkolwiek trwałych, które są zgodne z dowolnej właściwości z jedną lub więcej z warunkami określonymi. | dane dotyczące sprzedaży                                                |
| Określanie zakresu właściwości          | Tylko zwrotu miejsce, w którym wyszukiwany termin jest dopasowany do określonej właściwości źródła danych                                                   | Nazwa: finansowym                                              |
| Operatory logiczne         | Poszerzyć lub zawęzić wyszukiwanie przy użyciu operacji logicznych                                                                                     | Finanse nie firmowe                                     |
| Grupowanie za nawias okrągły | Używanie nawiasów do grupy części kwerendy w celu uzyskania logiczne oddzielnie, zwłaszcza w połączeniu z operatorów logicznych              | Nazwa: Finanse i (tagi: K1 lub znaczniki: K2) |
| Operatory porównania      | Używanie porównań niż równości dla właściwości, które mają typów danych liczbowych i daty                                                | modifiedTime > "2014-11-05"                                 |

Aby uzyskać więcej informacji na wyszukiwanie w **Wykazie danych Azure** zobacz [https://msdn.microsoft.com/library/azure/mt267594.aspx](https://msdn.microsoft.com/library/azure/mt267594.aspx).

## <a name="hit-highlighting"></a>Trafienie wyróżnienia
Podczas wyświetlania wyników wyszukiwania, aby ułatwić zidentyfikowanie Dlaczego zawartości danego danych został zwrócony przez danego wyszukiwania zostaną wyróżnione wyświetlane właściwości, pasujące do określonych kryteriów wyszukiwania — takie jak danych składników majątku nazwę, opis i znaczniki —.

> [AZURE.NOTE] Użytkowników można włączyć trafień, wyróżnianie wyłączyć, w razie potrzeby w portalu **Wykazu danych Azure** za pomocą przełącznika "Wyróżnianie".

Podczas wyświetlania wyników wyszukiwania, może nie zawsze być oczywiste Dlaczego zbiór danych są dołączone, nawet w przypadku włączone wyróżnianie trafień. Ponieważ domyślnie przeszukiwane są wszystkie właściwości, zbiór danych mogą być zwracane z powodu dopasowanie na poziomie kolumny właściwości. A ponieważ wielu użytkowników mogą dodawać adnotacje do aktywów zarejestrowane dane z własnych znaczników i opisami, nie wszystkie metadane mogą być wyświetlane na liście wyników wyszukiwania.

Widok sąsiadująco w domyślnym, każdego fragmentu wyświetlanych w wynikach wyszukiwania będzie zawierać ikonę "pasuje do widoku wyszukiwany termin", która umożliwia użytkownikowi, aby szybko wyświetlić liczbę dopasowania i ich lokalizacji i przejść do nich w razie potrzeby.

 ![Naciśnij, wyróżnianie i wyszukiwania dopasowanie w portalu wykazu danych usługi Azure](./media/data-catalog-how-to-discover/search-matches.png)

## <a name="summary"></a>Podsumowanie
Rejestrowanie źródła danych dzięki **Wykazowi danych Azure** ułatwia tego źródła danych rozpoznać i zrozumieć, kopiując strukturalne i opisową metadanych ze źródła danych w usłudze wykazu. Po zarejestrowaniu źródła danych, użytkownicy mogą one znaleźć go przy użyciu filtrowanie i wyszukiwanie w obrębie portal **Azure wykazu danych** .

## <a name="see-also"></a>Zobacz też
- [Rozpoczynanie pracy z wykazem danych Azure](data-catalog-get-started.md) samouczek krok po kroku szczegółowe informacje na temat sposobu odnaleźć źródła danych.
