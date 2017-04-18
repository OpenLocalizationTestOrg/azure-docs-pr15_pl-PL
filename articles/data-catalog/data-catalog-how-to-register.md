<properties
   pageTitle="Jak zarejestrować źródeł danych | Microsoft Azure"
   description="Artykule wyróżnianie jak zarejestrować źródeł danych za pomocą wykazu danych Azure, łącznie z polami metadanych wyodrębnionych podczas rejestrowania."
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


# <a name="how-to-register-data-sources"></a>Jak zarejestrować źródeł danych

## <a name="introduction"></a>Wprowadzenie
**Wykaz danych Microsoft Azure** to usługa w chmurze w pełni zarządzane, która pełni funkcję systemu rejestracji i system odnajdowania dla źródła danych przedsiębiorstwa. Innymi słowy **Azure wykazu danych** jest pomaga osobom odnajdowanie, opis i używanie źródeł danych i organizacji pomoc jeszcze lepiej korzystać ze swoich istniejących danych. I pierwszy krok do tworzenia źródła danych za pomocą **Wykazu danych Azure** odnajdowania zarejestrować tego źródła danych.
## <a name="registering-data-sources"></a>Rejestrowanie źródeł danych
Rejestracja jest proces wyodrębnianie metadanych ze źródła danych i kopiowanie danych w usłudze **Azure wykazu danych** . Dane pozostaną którym obecnie znajduje się i pozostaje w obszarze Sterowanie Administratorzy i zasad bieżącego systemu.

Aby zarejestrować źródła danych, po prostu uruchom narzędzie **Wykazu danych Azure** danych źródła rejestracji z portalu **Azure wykazu danych** . Zaloguj się przy użyciu swojej pracy lub konta służbowego (tych samych usługi Azure Active Directory poświadczeń używasz, aby zalogować się do portalu), a następnie wybierz źródło danych, które chcesz zarejestrować.
Zobacz samouczek [Wprowadzenie do wykazu danych usługi Azure](data-catalog-get-started.md) , aby uzyskać więcej szczegółowych informacji.

Po zarejestrowaniu źródła danych wykazu umożliwia śledzenie lokalizacji i indeksy metadanych, tak, aby użytkownicy mogli wyszukiwania, Przeglądaj i wykrywania źródła danych i następnie użyj lokalizacji, aby połączyć się z nim przy użyciu aplikacji lub narzędzia do siebie.

## <a name="sources-supported"></a>Obsługiwane źródła
Zajrzyj do [DSR wykazu danych](data-catalog-dsr.md) na liście obecnie obsługiwanych źródeł danych.
<br/>


## <a name="structural-metadata"></a>Strukturalne metadanych
Zapisując jest źródłem danych narzędzie do rejestracji będzie wyodrębnić informacje o strukturze obiektów, które można wybrać — jest to określone jako strukturalnych metadanych.

W przypadku wszystkich obiektów strukturalnych metadanych będzie zawierać lokalizację obiektu, aby użytkownicy, którzy odnajdowanie danych można połączyć się z obiektem w narzędziach klienta siebie za pomocą tych informacji. Inne metadane strukturalnych zawiera nazwy obiektu i typ i nazwa atrybutu/kolumny wpisz dane.

## <a name="descriptive-metadata"></a>Opisowy metadanych
Oprócz metadane strukturalnych core wyodrębnionych ze źródła danych narzędzie do rejestracji źródła danych również wyodrębnić także opisową metadanych. Dla usług SQL Server Analysis Services i SQL Server Reporting Services to jest pobierana z właściwości Opis udostępniane przez tych usług. Dla programu SQL Server wartości przy użyciu ms_description właściwość rozszerzona są wyodrębniane. W bazie danych programu Oracle narzędzie do rejestracji źródła danych będzie Wyodrębnij kolumnie Komentarze w widoku ALL_TAB_COMMENTS.

Oprócz opisu metadanych wyodrębnionych ze źródła danych można także wprowadzić opisowy metadanych za pomocą narzędzia rejestracji źródła danych. Użytkowników możesz dodać znaczniki i można określić ekspertów dla obiektów rejestrowana. Wszystkie te metadane opisowe jest kopiowana z usługą **Azure wykazu danych** oraz strukturalnych metadanych.

## <a name="including-previews"></a>W tym podglądu

Domyślnie tylko metadane jest wyodrębnionych z źródła danych i skopiować z usługą **Azure wykazu danych** , ale opis czy źródło danych jest często łatwiejsze sprawdzając przykładowych danych zawiera.

Narzędzie do rejestracji źródła danych **Wykazu danych Azure** umożliwia użytkownikom obejmują migawkę Podgląd danych w każdej tabeli i widoku, który jest zarejestrowany. Jeśli użytkownik zdecyduje się do uwzględnienia podglądy podczas rejestrowania, narzędzie do rejestracji będzie zawierać maksymalnie 20 rekordy z każdej tabeli i widoku. Tej migawki następnie jest kopiowana do katalogu wraz z metadanych strukturalne i opisu.


> [AZURE.NOTE]  Szerokie tabele z dużą liczbą kolumn może być mniej niż 20 rekordów zawarte w ich podgląd.


## <a name="including-data-profiles"></a>W tym profile danych

Taki sam sposób, w tym podglądy umożliwiają kontekstu przydatne dla użytkowników, wyszukując źródeł danych w **Wykazie danych Azure**, łącznie z profilu danych można również ułatwić opis źródeł danych odkryte.

Narzędzie do rejestracji źródła danych **Wykazu danych Azure** pozwala użytkownikom na dołączanie profilu danych dla każdej tabeli i widoku, który jest zarejestrowany. Jeśli użytkownik wybierze do uwzględnienia profilu danych podczas rejestrowania, narzędzie do rejestracji będzie zawierać zagregowanych statystyk dotyczących danych w każdej tabeli i widoku, w tym:

* Liczba wierszy i rozmiar danych w obiekcie
* Data ostatniej aktualizacji danych i schematu obiektu
* Liczba rekordów zawiera wartości null i unikatowych wartości dla kolumn
* Wartości minimum, maksimum, średnia i odchylenie standardowe dla kolumn

Statystyki te są kopiowane do wykazu wraz z metadanych strukturalne i opisu.

> [AZURE.NOTE]  Kolumny tekstu i daty nie będzie zawierać statystyki średnia lub odchylenie standardowe w swoim profilu danych.

## <a name="updating-registrations"></a>Aktualizowanie rejestracji

Rejestrowanie źródła danych, spowoduje to odnajdowania w **Azure wykazu danych** przy użyciu metadanych i opcjonalnie Podgląd wyodrębnionych podczas rejestrowania. Jeśli źródło danych musi zostać zaktualizowany w katalogu (na przykład schemat obiektu uległ zmianie, lub tabele pierwotnie wykluczone powinny być dołączone, czy użytkownik chce, aby zaktualizować dane zawarte w podglądy) Narzędzie do rejestracji źródła danych można uruchomić ponownie.

Ponowne rejestrowanie źródła danych już zarejestrowana wykonuje operację scalania "upsert": istniejące obiekty zostaną zaktualizowane, gdy zostanie utworzone nowe obiekty. Wszystkie metadane dostarczony przez użytkowników za pomocą portalu **Wykazu danych Azure** zostaną zachowane.

## <a name="summary"></a>Podsumowanie
Rejestrowanie źródła danych dzięki **Wykazowi danych Azure** ułatwia tego źródła danych rozpoznać i zrozumieć, kopiując strukturalne i opisową metadanych ze źródła danych w usłudze wykazu. Po zarejestrowaniu źródła danych, go można następnie można dodawać adnotacje, zarządzanych i odkryte, za pomocą portalu **Azure wykazu danych** .

## <a name="see-also"></a>Zobacz też
- [Rozpoczynanie pracy z wykazem danych Azure](data-catalog-get-started.md) samouczek krok po kroku szczegółowe informacje na temat sposobu rejestrowania źródła danych.
