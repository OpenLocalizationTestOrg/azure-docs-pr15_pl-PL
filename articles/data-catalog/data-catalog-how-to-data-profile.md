<properties
    pageTitle="Jak źródeł danych profilu danych"
    description="Artykule wyróżnianie sposobu obejmują profile danych tabeli i kolumny poziomu podczas rejestrowania źródła danych w wykazie danych Azure i opis źródeł danych za pomocą profile danych."
    services="data-catalog"
    documentationCenter=""
    authors="spelluru"
    manager="NA"
    editor=""
    tags=""/>
<tags
    ms.service="data-catalog"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-catalog"
    ms.date="09/13/2016"
    ms.author="spelluru"/>

# <a name="data-profile-data-sources"></a>Źródła danych profilu danych

## <a name="introduction"></a>Wprowadzenie

**Wykaz danych Microsoft Azure** to usługa w chmurze w pełni zarządzane, która pełni funkcję systemu rejestracji i system odnajdowania dla źródła danych przedsiębiorstwa. Innymi słowy **Azure wykazu danych** jest pomaga osobom odnajdowanie, opis i używanie źródeł danych i organizacji pomoc jeszcze lepiej korzystać ze swoich istniejących danych. Po zarejestrowaniu źródła danych dzięki **Wykazowi danych Azure**, metadanych jest kopiowana i indeksowania przez usługę, ale wątku nie kończą się.

**Profilowanie danych** funkcji wykazu **danych Azure** sprawdza, czy dane z obsługiwanych źródeł danych w wykazie i gromadzi statystyki i informacji na temat tych danych. Jest proste uwzględnić profil aktywów danych. Po zarejestrowaniu zbiór danych wybierz pozycję **Zawiera profil danych** w narzędziu rejestracji źródła danych.

## <a name="what-is-data-profiling"></a>Co to jest profilowanie danych

Profilowanie danych sprawdza, czy dane w źródle danych jest zarejestrowany i gromadzi statystyki i informacji na temat tych danych. Podczas odnajdowania źródła danych statystyki te mogą ułatwić podjęcie przydatności danych, aby rozwiązać problem ich firm.

<!-- In [How to discover data sources](data-catalog-how-to-discover.md), you learn about **Azure Data Catalog's** extensive search capabilities including searching for data assets that have a profile. See [How to include a data profile when registering a data source](#howto). -->

Następujące źródła danych obsługiwane profilowanie danych:

- Tabele programu SQL Server (w tym bazy danych SQL Azure i magazynu danych SQL Azure) i widoków
- Oracle tabel i widoków
- Teradata tabele i widoki
- Gałąź tabel

Tym profile danych, gdy rejestrowanie danych zasobów ułatwia użytkownikom odpowiedzi na pytania dotyczące źródeł danych, w tym:

-   Czy można używać do rozwiązywania problemów z firmą?
-   Dane jest zgodna z szczególne normy lub desenie?
-   Jakie są niektóre z różnic w odniesieniu do źródła danych?
-   Co to są potencjalnych problemów integrowania danych do mojej aplikacji?

> [AZURE.NOTE] Można również dodać dokumentacji do środka trwałego w celu opisania jak danych może być zintegrowany z aplikacji. Dowiedz się, [jak do dokumentu źródeł danych](data-catalog-how-to-documentation.md).


<a name="howto"/>
## <a name="how-to-include-a-data-profile-when-registering-a-data-source"></a>Jak dołączyć profilu danych podczas rejestrowania źródła danych

Jest proste uwzględnić profilu źródła danych. Podczas rejestrowania źródła danych w panelu **obiekty, aby zarejestrować się** narzędzie do rejestracji źródła danych, wybierz pozycję **Profil zawiera dane**.

![](media\data-catalog-data-profile\data-catalog-register-profile.png)

Aby dowiedzieć się więcej na temat sposobu rejestrowania źródeł danych, zobacz [jak zarejestrować źródeł danych](data-catalog-how-to-register.md) i [Rozpoczynanie pracy z wykazem danych Azure](data-catalog-get-started.md).


## <a name="filtering-on-data-assets-that-include-data-profiles"></a>Filtrowanie danych składników majątku, obejmujących profile danych
Aby odnajdować aktywów danych, które obejmują profilu danych, należy umieścić `has:tableDataProfiles` lub `has:columnsDataProfiles` jako jeden z wyszukiwanych terminów.

> [AZURE.NOTE] Wybieranie **Obejmują profilu danych** w narzędziu rejestracji źródła danych zawiera zarówno tabeli i informacje o profilu poziomie kolumny. Jednak interfejsu API wykaz danych umożliwia danych składniki majątku być zarejestrowany na tylko jeden zestaw dostępnych informacji w profilu.

## <a name="viewing-data-profile-information"></a>Wyświetlanie informacji o profilu danych

Po znalezieniu źródła odpowiednie dane z profilem, możesz wyświetlić szczegóły profilu danych. Aby wyświetlić profil danych, wybierz zasób danych i wybierz **Profil danych** w oknie portalu wykazu danych.

![](media\data-catalog-data-profile\data-catalog-view.png)

Profil danych w **Wykazie danych Azure** przedstawia tabel i kolumn profilu informacji, takich jak:

### <a name="object-data-profile"></a>Obiekt danych profilu

-   Liczba wierszy
-   Rozmiar tabeli
-   Po ostatniej aktualizacji obiektu

### <a name="column-data-profile"></a>Kolumna danych profilu

- Typ danych kolumny
- Liczba różnych wartości
- Liczba wierszy zawierających wartości NULL
- Minimum, maksimum, średnia i odchylenie standardowe dla wartości kolumny

## <a name="summary"></a>Podsumowanie
Dane profilowania zawiera statystyki i informacji na temat środków zarejestrowanych danych ułatwiającymi przydatności danych, aby rozwiązać problemy biznesowe. Oprócz adnotacje i dokumentowanie źródeł danych, profile danych można udostępnić użytkownikom szczegółowego zrozumienia danych.


## <a name="see-also"></a>Zobacz też
-   [Jak zarejestrować źródeł danych](data-catalog-how-to-register.md)
-   [Rozpoczynanie pracy z wykazem danych Azure](data-catalog-get-started.md)
