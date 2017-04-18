<properties
    pageTitle="Zaloguj się Projektant widoków analizy | Microsoft Azure"
    description="Projektant widoków w dzienniku analizy umożliwia tworzenie niestandardowych widoków w konsoli usługi OMS, które zawierają różne wizualizacje danych w repozytorium usługi OMS. Ten artykuł zawiera odwołanie ustawienia dla każdej z nich części wizualizacji, dostępny do użytku w sekcji Widoki niestandardowe."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="bwren"/>

# <a name="log-analytics-view-designer-visualization-part-reference"></a>Odwołanie do części wizualizacji Projektant widoków analizy dziennika
Projektant widoków w dzienniku analizy umożliwia tworzenie niestandardowych widoków w konsoli usługi OMS, które zawierają różne wizualizacje danych z repozytorium usługi OMS. Ten artykuł zawiera odwołanie ustawienia dla każdej z nich części wizualizacji, dostępny do użytku w sekcji Widoki niestandardowe.

Inne artykuły, które są dostępne dla projektanta widoku są:

- [Projektant widoków](log-analytics-view-designer.md) — omówienie Projektant widoków i procedur dotyczących tworzenia i edytowania widoków niestandardowych.
- [Odwołanie kafelku](log-analytics-view-designer-tiles.md) — odwołanie ustawienia dla każdego kafelków dostępny do użytku w sekcji Widoki niestandardowe. 

W poniższej tabeli opisano różne typy dostępnych w Projektancie widoku kafelków.  Poniżej opisano każdy typ kafelka w szczegóły i ich właściwości.

| Typ widoku | Opis |
|:--|:--|
| [Listy kwerend](#list-of-queries-part) | Wyświetla listę dziennika kwerend wyszukiwania.  Użytkownik może kliknąć dla każdej kwerendy, aby wyświetlić jego wyniki.  |
| [Liczba i listy](#number-amp-list-part) | Nagłówek zawiera pojedynczy pokazujący liczbę rekordów z kwerendy wyszukiwania dziennika.  Lista wyświetla górny dziesięć wyniki kwerendy z Wykres wskazujący względne wartości liczbowe kolumny lub jego zmian w czasie. |
| [Dwa numery i listy](#two-numbers-amp-list-part) | Nagłówek ma dwie liczby pokazujący statystykę rekordy z osobnych dziennika kwerend wyszukiwania.  Lista wyświetla górny dziesięć wyniki kwerendy z Wykres wskazujący względne wartości liczbowe kolumny lub jego zmian w czasie. |
| [Listy i pierścieniowy](#donut-amp-list-part) | Nagłówek przedstawia jeden numer podsumowane z wartości kolumny w kwerendzie dziennika.  Pierścień wyświetla wyniki górny trzy rekordy. |
| [Dwie osie czasu i listy](#two-timelines-amp-list-part) | Nagłówek służy do wyświetlania wyników dwóch zapytań dziennika czasie jako wykres kolumnowy z objaśnieniem wyświetlanie jeden numer podsumowane z wartości kolumny w kwerendzie dziennika.  Lista wyświetla górny dziesięć wyniki kwerendy z Wykres wskazujący względne wartości liczbowe kolumny lub jego zmian w czasie. |   
| [Informacje o](#information-part) | Nagłówek zawiera tekst statyczny i opcjonalnie łącze.  Lista wyświetla jednego lub kilku elementów z tytuł i tekst statyczny. |
| [Wykres liniowy, objaśnienia i listy](#line-chart-callout-amp-list-part) | Nagłówek przedstawia wykresu liniowego z wielu serii z kwerendy dziennika przez godzinę i objaśnienia z wartością podsumowane.  Lista wyświetla górny dziesięć wyniki kwerendy z Wykres wskazujący względne wartości liczbowe kolumny lub jego zmian w czasie. |
| [Wykres liniowy i listy](#line-chart-amp-list-part) | Nagłówek przedstawia wykresu liniowego z wielu serii z kwerendy dziennika w czasie.  Lista wyświetla górny dziesięć wyniki kwerendy z Wykres wskazujący względne wartości liczbowe kolumny lub jego zmian w czasie. |
| [Stos części wykresy linii](#stack-of-line-charts-part) | W czasie są wyświetlane trzy osobne wykresy z wielu serii z kwerendy dziennika. |


## <a name="list-of-queries-part"></a>Lista części kwerendy

Wyświetla listę dziennika kwerend wyszukiwania.  Użytkownik może kliknąć dla każdej kwerendy, aby wyświetlić jego wyniki.  Widok będzie zawierał pojedynczą kwerendę domyślnie, a kliknięcie **+ kwerendy** Dodawanie dodatkowych kwerend.

![Lista Widok kwerendy](media/log-analytics-view-designer/view-list-queries.png)

| Ustawienie | Opis |
|:--|:--|
| **Ogólne** |
| Tytuł | Tekst wyświetlany w górnej części widoku. |
| Nowa grupa | Wybierz, aby utworzyć nową grupę w widoku, zaczynając od bieżącego widoku. |
| Wstępnie wybrane filtry | Lista właściwości, które należy uwzględnić w okienku po lewej stronie filtr, gdy użytkownik wybierze kwerendy przecinkami. |
| Tryb renderowania | Widok początkowy wyświetlany po wybraniu zapytania.  Użytkownik może zaznaczyć wszystkie dostępne widoki po otwarciu kwerendy. |
| **Kwerendy** |
| Kwerendy wyszukiwania | Kwerendy. |
| Przyjazną nazwę | Opisowa nazwa kwerendy w celu wyświetlenia dla użytkownika. |


## <a name="number--list-part"></a>Listy i numer strony

Nagłówek zawiera pojedynczy pokazujący liczbę rekordów z kwerendy wyszukiwania dziennika.  Lista wyświetla górny dziesięć wyniki kwerendy z Wykres wskazujący względne wartości liczbowe kolumny lub jego zmian w czasie.


![Lista Widok kwerendy](media/log-analytics-view-designer/view-number-list.png)

| Ustawienie | Opis |
|:--|:--|
| **Ogólne** |
| Tytuł grupy | Tekst wyświetlany w górnej części widoku. |
| Nowa grupa | Wybierz, aby utworzyć nową grupę w widoku, zaczynając od bieżącego widoku. |
| Ikona | Plik obrazu, aby wyświetlić obok wynik w nagłówku.
| Użyj ikony | Umożliwia wyświetlanie ikony. |
| **Tytuł** |
| Legendy | Tekst wyświetlany w górnej części nagłówka. |
| Kwerendy | Kwerendy nagłówka.  Zostanie wyświetlona liczba rekordów zwróconych przez kwerendę. |
| **Lista** |
| Kwerendy | Kwerendy na liście.  Zostaną wyświetlone dwa pierwsze właściwości dla pierwszych dziesięć rekordów w wynikach.  Pierwszej właściwości powinny być wartość tekstową i drugi właściwość wartość liczbową.  Słupki są tworzone automatycznie na podstawie wartości względne kolumny liczbowe.<br><br>Sortowanie rekordów na liście za pomocą polecenia sortowania w kwerendzie.  Użytkownik może kliknąć, aby wyświetlić całą, aby uruchomić kwerendę i zwraca wszystkie rekordy. |
| Ukrywanie wykresu | Wybierz, aby wyłączyć na wykresie po prawej stronie kolumny. |
| Włączanie wykresów przebiegu w czasie | Wybierz, aby wyświetlić wykres przebiegu w czasie zamiast poziomy pasek.  Aby uzyskać szczegółowe informacje, zobacz [Typowe ustawienia](#sparklines) . |
| Kolor | Kolor słupków bądź wykresów przebiegu w czasie. |
| Nazwa i wartość separatora | Pojedynczy znak ogranicznika, jeśli chcesz przeanalizować właściwości tekstu do wielu wartości.  Aby uzyskać szczegółowe informacje, zobacz [Typowe ustawienia](#name-value-separator) . |
| Kwerendy nawigacji | Kwerendy, aby uruchomić, gdy użytkownik wybierze element na liście.  Aby uzyskać szczegółowe informacje, zobacz [Typowe ustawienia](#navigation-query) . |
| **Lista** | **> Tytuły kolumn** |
| Nazwa | Tekst wyświetlany w górnej części pierwszej kolumny listy. |
| Wartość | Tekst wyświetlany w górnej części drugiej kolumny listy. |
| **Lista** | **> Progi** |
| Włączanie progi | Zaznacz, aby włączyć progi.  Aby uzyskać szczegółowe informacje, zobacz [Typowe ustawienia](#thresholds) . |


## <a name="two-numbers--list-part"></a>Część listy i dwóch liczb.

Nagłówek ma dwie liczby pokazujący statystykę rekordy z osobnych dziennika kwerend wyszukiwania.  Lista wyświetla górny dziesięć wyniki kwerendy z Wykres wskazujący względne wartości liczbowe kolumny lub jego zmian w czasie.

![Dwóch liczb i widoku listy](media/log-analytics-view-designer/view-two-numbers-list.png)

| Ustawienie | Opis |
|:--|:--|
| **Ogólne** |
| Tytuł grupy | Tekst wyświetlany w górnej części widoku. |
| Nowa grupa | Wybierz, aby utworzyć nową grupę w widoku, zaczynając od bieżącego widoku. |
| Ikona | Plik obrazu, aby wyświetlić obok wynik w nagłówku.
| Użyj ikony | Umożliwia wyświetlanie ikony. |
| **Tytuł** |
| Legendy | Tekst wyświetlany w górnej części nagłówka. |
| Kwerendy | Kwerendy nagłówka.  Zostanie wyświetlona liczba rekordów zwróconych przez kwerendę. |
| **Lista** |
| Kwerendy | Kwerendy na liście.  Zostaną wyświetlone dwa pierwsze właściwości dla pierwszych dziesięć rekordów w wynikach.  Pierwszej właściwości powinny być wartość tekstową i drugi właściwość wartość liczbową.  Słupki są tworzone automatycznie na podstawie wartości względne kolumny liczbowe.<br><br>Sortowanie rekordów na liście za pomocą polecenia sortowania w kwerendzie.  Użytkownik może kliknąć, aby wyświetlić całą, aby uruchomić kwerendę i zwraca wszystkie rekordy. |
| Ukrywanie wykresu | Wybierz, aby wyłączyć na wykresie po prawej stronie kolumny. |
| Włączanie wykresów przebiegu w czasie | Wybierz, aby wyświetlić wykres przebiegu w czasie zamiast poziomy pasek.  Aby uzyskać szczegółowe informacje, zobacz [Typowe ustawienia](#sparklines) . |
| Kolor | Kolor słupków bądź wykresów przebiegu w czasie. |
| Operacja | Operacja w celu wykonywania dla wykresu przebiegu w czasie.  Aby uzyskać szczegółowe informacje, zobacz [Typowe ustawienia](#sparklines) . |
| Nazwa i wartość separatora | Pojedynczy znak ogranicznika, jeśli chcesz przeanalizować właściwości tekstu do wielu wartości.  Aby uzyskać szczegółowe informacje, zobacz [Typowe ustawienia](#name-value-separator) . |
| Kwerendy nawigacji | Kwerendy, aby uruchomić, gdy użytkownik wybierze element na liście.  Aby uzyskać szczegółowe informacje, zobacz [Typowe ustawienia](#navigation-query) . |
| **Lista** | **> Tytuły kolumn** |
| Nazwa | Tekst wyświetlany w górnej części pierwszej kolumny listy. |
| Wartość | Tekst wyświetlany w górnej części drugiej kolumny listy. |
| **Lista** | **> Progi** |
| Włączanie progi | Zaznacz, aby włączyć progi.  Aby uzyskać szczegółowe informacje, zobacz [Typowe ustawienia](#thresholds) . |

## <a name="donut--list-part"></a>Część listy i pierścieniowy

Nagłówek przedstawia jeden numer podsumowane z wartości kolumny w kwerendzie dziennika.  Pierścień wyświetla wyniki górny trzy rekordy.

![Widok listy i pierścieniowy](media/log-analytics-view-designer/view-donut-list.png)

| Ustawienie | Opis |
|:--|:--|
| **Ogólne** |
| Tytuł grupy | Tekst wyświetlany w górnej części fragmentu. |
| Nowa grupa | Wybierz, aby utworzyć nową grupę w widoku, zaczynając od bieżącego widoku. |
| Ikona | Plik obrazu, aby wyświetlić obok wynik w nagłówku. |
| Użyj ikony | Umożliwia wyświetlanie ikony. |
| **Nagłówek** |
| Tytuł | Tekst wyświetlany w górnej części nagłówka.
| Pomocniczą | Tekst do wyświetlenia w obszarze tytuł u góry nagłówka.
| **Pierścieniowy** |
| Kwerendy | Kwerendy dla pierścieniowego.  Pierwszej właściwości powinny być wartość tekstową i drugi właściwość wartość liczbową. |
| **Pierścieniowy** |  **> Centrum** |
| Tekst | Tekst do wyświetlenia w obszarze wartości wewnątrz pierścieniowego. |
| Operacja | Operacja do wykonania na właściwości wartości do podsumowania na pojedynczą wartość.<br><br>— Suma: Dodawanie wartości wszystkich rekordów.<br>— Procent: Procent rekordy zwrócone przez wartości z **wartości wyników używana w operacji centrum** całkowita liczba rekordów w kwerendzie. |
| Wartości wyników używana w operacji Centrum | Opcjonalnie kliknij znak plus, aby dodać jedną lub kilka wartości.  Wyniki kwerendy będą ograniczone do rekordów zawierających wartości właściwości, które można określić.  Jeśli wartości nie zostaną dodane, wszystkie rekordy znajdują się w kwerendzie. |
| **Dodatkowe opcje** | **> Kolorów** |
| Kolor 1<br>Kolor 2<br>Kolor 3 | Wybierz kolor dla każdego z wartościami wyświetlanymi w pierścieniowego. |
| **Dodatkowe opcje** | **> Mapowanie kolorów zaawansowane** |
| Wartość pola | Wpisz nazwę pola, aby wyświetlić go jako inny kolor, jeśli będzie obejmować pierścieniowego. |
| Kolor | Wybierz odpowiedni kolor w polu unikatowy. |
| **Lista** |
| Kwerendy | Kwerendy na liście.  Zostanie wyświetlona liczba rekordów zwróconych przez kwerendę. |
| Ukrywanie wykresu | Wybierz, aby wyłączyć na wykresie po prawej stronie kolumny. |
| Włączanie wykresów przebiegu w czasie | Wybierz, aby wyświetlić wykres przebiegu w czasie zamiast poziomy pasek.  Aby uzyskać szczegółowe informacje, zobacz [Typowe ustawienia](#sparklines) . |
| Kolor | Kolor słupków bądź wykresów przebiegu w czasie. |
| Operacja | Operacja w celu wykonywania dla wykresu przebiegu w czasie.  Aby uzyskać szczegółowe informacje, zobacz [Typowe ustawienia](#sparklines) . |
| Nazwa i wartość separatora | Pojedynczy znak ogranicznika, jeśli chcesz przeanalizować właściwości tekstu do wielu wartości.  Aby uzyskać szczegółowe informacje, zobacz [Typowe ustawienia](#name-value-separator) . |
| Kwerendy nawigacji | Kwerendy, aby uruchomić, gdy użytkownik wybierze element na liście.  Aby uzyskać szczegółowe informacje, zobacz [Typowe ustawienia](#navigation-query) . |
| **Lista** | **> Tytuły kolumn** |
| Nazwa | Tekst wyświetlany w górnej części pierwszej kolumny listy. |
| Wartość | Tekst wyświetlany w górnej części drugiej kolumny listy. |
| **Lista** | **> Progi** |
| Włączanie progi | Zaznacz, aby włączyć progi.  Aby uzyskać szczegółowe informacje, zobacz [Typowe ustawienia](#thresholds) . |

## <a name="two-timelines--list-part"></a>Dwie części osi czasu i listy

Nagłówek służy do wyświetlania wyników dwóch zapytań dziennika czasie jako wykres kolumnowy z objaśnieniem wyświetlanie jeden numer podsumowane z wartości kolumny w kwerendzie dziennika.  Lista wyświetla górny dziesięć wyniki kwerendy z Wykres wskazujący względne wartości liczbowe kolumny lub jego zmian w czasie.

![Wyświetlanie dwóch osi czasu i listy](media/log-analytics-view-designer/view-two-timelines-list.png)

| Ustawienie | Opis |
|:--|:--|
| **Ogólne** |
| Tytuł grupy | Tekst wyświetlany w górnej części fragmentu. |
| Nowa grupa | Wybierz, aby utworzyć nową grupę w widoku, zaczynając od bieżącego widoku. |
| Ikona | Plik obrazu, aby wyświetlić obok wynik w nagłówku. |
| Użyj ikony | Umożliwia wyświetlanie ikony. |
| **Najpierw wykresu<br>drugi wykres** |
| Legendy | Tekst do wyświetlenia w obszarze objaśnień dla pierwszej serii. |
| Kolor | Kolor używany dla kolumn w serii. |
| Kwerendy | Kwerendy dla pierwszej serii.  Liczba rekordów w każdym przedziale czasu będą reprezentowane przez kolumn wykresu. |
| Operacja | Operacja do wykonania na właściwości wartości do podsumowania na pojedynczą wartość objaśnienia.<br><br>— Suma: Suma wartości ze wszystkich rekordów.<br>-Średnia: Średniej wartości ze wszystkich rekordów.<br>-Ostatniej próbki: Wartość od ostatniej interwał zawartych w wykresie.<br>-Pierwszej próbie: Wartość od pierwszego interwał zawartych w wykresie.<br>-Liczba: Liczba wszystkie rekordy zwrócone przez kwerendę.|
| **Lista** |
| Kwerendy | Kwerendy na liście.  Zostanie wyświetlona liczba rekordów zwróconych przez kwerendę. |
| Ukrywanie wykresu | Wybierz, aby wyłączyć na wykresie po prawej stronie kolumny. |
| Włączanie wykresów przebiegu w czasie | Wybierz, aby wyświetlić wykres przebiegu w czasie zamiast poziomy pasek.  Aby uzyskać szczegółowe informacje, zobacz [Typowe ustawienia](#sparklines) . |
| Kolor | Kolor słupków bądź wykresów przebiegu w czasie. |
| Operacja | Operacja w celu wykonywania dla wykresu przebiegu w czasie.  Aby uzyskać szczegółowe informacje, zobacz [Typowe ustawienia](#sparklines) . |
| Kwerendy nawigacji | Kwerendy, aby uruchomić, gdy użytkownik wybierze element na liście.  Aby uzyskać szczegółowe informacje, zobacz [Typowe ustawienia](#navigation-query) . |
| **Lista** | **> Tytuły kolumn** |
| Nazwa | Tekst wyświetlany w górnej części pierwszej kolumny listy. |
| Wartość | Tekst wyświetlany w górnej części drugiej kolumny listy. |
| **Lista** | **> Progi** |
| Włączanie progi | Zaznacz, aby włączyć progi.  Aby uzyskać szczegółowe informacje, zobacz [Typowe ustawienia](#thresholds) . |

## <a name="information-part"></a>Część informacji

Nagłówek zawiera tekst statyczny i opcjonalnie łącze.  Lista wyświetla jednego lub kilku elementów z tytuł i tekst statyczny.

![Wyświetlanie informacji](media/log-analytics-view-designer/view-information.png)

| Ustawienie | Opis |
|:--|:--|
| **Ogólne** |
| Tytuł grupy | Tekst wyświetlany w górnej części fragmentu. |
| Nowa grupa | Wybierz, aby utworzyć nową grupę w widoku, zaczynając od bieżącego widoku. |
| Kolor | Kolor tła nagłówka. |
| **Nagłówek** |
| Obraz | Plik obrazu do wyświetlenia w nagłówku. |
| Etykieta | Tekst do wyświetlenia w nagłówku. |
| **Nagłówek** | **> Łącze** |
| Etykieta | Tekst łącza. |
| Adres URL | Adres URL łącza. |
| **Elementy informacji** |
| Tytuł | Tekst do wyświetlenia tytuł każdego elementu. |
| Zawartość | Tekst do wyświetlenia dla każdego elementu. |


## <a name="line-chart-callout--list-part"></a>Wykres liniowy, objaśnienia i listy

Nagłówek przedstawia wykresu liniowego z wielu serii z kwerendy dziennika przez godzinę i objaśnienia z wartością podsumowane.  Lista wyświetla górny dziesięć wyniki kwerendy z Wykres wskazujący względne wartości liczbowe kolumny lub jego zmian w czasie.

![Wykres liniowy, objaśnienia i widoku listy](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Ustawienie | Opis |
|:--|:--|
| **Ogólne** |
| Tytuł grupy | Tekst wyświetlany w górnej części fragmentu. |
| Nowa grupa | Wybierz, aby utworzyć nową grupę w widoku, zaczynając od bieżącego widoku. |
| Ikona | Plik obrazu, aby wyświetlić obok wynik w nagłówku. |
| Użyj ikony | Umożliwia wyświetlanie ikony. |
| **Nagłówek** |
| Tytuł | Tekst wyświetlany w górnej części nagłówka. |
| Pomocniczą | Tekst do wyświetlenia w obszarze tytuł u góry nagłówka. |
| **Wykres liniowy** |
| Kwerendy | Kwerendy wykresu liniowego.  Pierwszej właściwości powinny być wartość tekstową i drugi właściwość wartość liczbową.  Jest to zazwyczaj kwerendę, która używa **miarę** słowo kluczowe do podsumowania wyników.  Jeśli w kwerendzie użyto słowo kluczowe **Interwał** osi wykresu użyje tego przedziału czasu.  Jeśli kwerenda nie zawiera słowo kluczowe **interwału** godzinowe interwały są używane na osi x. |
| **Wykres liniowy** | **> Objaśnienie** |
| Tytuł objaśnienia | Tekst do wyświetlenia powyżej wartości objaśnienie. |
| Nazwa serii | Wartość właściwości dla serii, aby użyć wartości objaśnienie.  Jeśli podano bez serii są używane wszystkie rekordy z kwerendy. |
| Operacja | Operacja do wykonania na właściwości wartości do podsumowania na pojedynczą wartość objaśnienia.<br><br>-Średnia: Średniej wartości ze wszystkich rekordów.<br>-Zliczanie liczby wszystkie rekordy zwrócone przez kwerendę.<br>-Ostatniej próbki: Wartość od ostatniej interwał zawartych w wykresie.<br>-Max: Wartość maksymalna z interwałów zawartych w wykresie.<br>-Min: Wartość minimalna z interwałów uwzględnione na wykresie.<br>— Suma: Suma wartości ze wszystkich rekordów. |
| **Wykres liniowy** | **> Oś Y** |
| Używanie Skala logarytmiczna | Zaznacz, aby użyć Skala logarytmiczna dla osi y. |
| Jednostki | Określ jednostki wartości zwrócone przez kwerendę.  Te informacje służy do wyświetlania etykiet, na wykresie wskazujące typy wartości i opcjonalnie do konwertowania wartości.  Typ jednostki określa kategorię jednostki i definiuje wartości bieżącego typu jednostki, które są dostępne.  W przypadku wybrania wartości w przekonwertować, a następnie wartości liczbowe są konwertowane z typu bieżącej jednostki do konwersji wpisywanie. |
| Etykiety niestandardowej | Tekst do wyświetlenia dla osi Y obok etykiety typ jednostki.  Jeśli określono bez etykiety, jest wyświetlany tylko typ jednostki. |
| **Lista** |
| Kwerendy | Kwerendy na liście.  Zostanie wyświetlona liczba rekordów zwróconych przez kwerendę. |
| Ukrywanie wykresu | Wybierz, aby wyłączyć na wykresie po prawej stronie kolumny. |
| Włączanie wykresów przebiegu w czasie | Wybierz, aby wyświetlić wykres przebiegu w czasie zamiast poziomy pasek.  Aby uzyskać szczegółowe informacje, zobacz [Typowe ustawienia](#sparklines) . |
| Kolor | Kolor słupków bądź wykresów przebiegu w czasie. |
| Operacja | Operacja w celu wykonywania dla wykresu przebiegu w czasie.  Aby uzyskać szczegółowe informacje, zobacz [Typowe ustawienia](#sparklines) . |
| Nazwa i wartość separatora | Pojedynczy znak ogranicznika, jeśli chcesz przeanalizować właściwości tekstu do wielu wartości.  Aby uzyskać szczegółowe informacje, zobacz [Typowe ustawienia](#name-value-separator) . |
| Kwerendy nawigacji | Kwerendy, aby uruchomić, gdy użytkownik wybierze element na liście.  Aby uzyskać szczegółowe informacje, zobacz [Typowe ustawienia](#navigation-query) . |
| **Lista** | **> Tytuły kolumn** |
| Nazwa | Tekst wyświetlany w górnej części pierwszej kolumny listy. |
| Wartość | Tekst wyświetlany w górnej części drugiej kolumny listy. |
| **Lista** | **> Progi** |
| Włączanie progi | Zaznacz, aby włączyć progi.  Aby uzyskać szczegółowe informacje, zobacz [Typowe ustawienia](#thresholds) . |

## <a name="line-chart--list-part"></a>Linia wykresu i listy części

Nagłówek przedstawia wykresu liniowego z wielu serii z kwerendy dziennika w czasie.  Lista wyświetla górny dziesięć wyniki kwerendy z Wykres wskazujący względne wartości liczbowe kolumny lub jego zmian w czasie.

![Widok wykresu i lista wiersza](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Ustawienie | Opis |
|:--|:--|
| **Ogólne** |
| Tytuł grupy | Tekst wyświetlany w górnej części fragmentu. |
| Nowa grupa | Wybierz, aby utworzyć nową grupę w widoku, zaczynając od bieżącego widoku. |
| Ikona | Plik obrazu, aby wyświetlić obok wynik w nagłówku. |
| Użyj ikony | Umożliwia wyświetlanie ikony. |
| **Nagłówek** |
| Tytuł | Tekst wyświetlany w górnej części nagłówka. |
| Pomocniczą | Tekst do wyświetlenia w obszarze tytuł u góry nagłówka. |
| **Wykres liniowy** |
| Kwerendy | Kwerendy wykresu liniowego.  Pierwszej właściwości powinny być wartość tekstową i drugi właściwość wartość liczbową.  Jest to zazwyczaj kwerendę, która używa **miarę** słowo kluczowe do podsumowania wyników.  Jeśli w kwerendzie użyto słowo kluczowe **Interwał** osi wykresu użyje tego przedziału czasu.  Jeśli kwerenda nie zawiera słowo kluczowe **Interwał** godzinowe interwały są używane na osi x. |
| **Wykres liniowy** | **> Oś Y** |
| Używanie Skala logarytmiczna | Zaznacz, aby użyć Skala logarytmiczna dla osi y. |
| Jednostki | Określ jednostki wartości zwrócone przez kwerendę.  Te informacje służy do wyświetlania etykiet, na wykresie wskazujące typy wartości i opcjonalnie do konwertowania wartości.  Typ jednostki określa kategorię jednostki i definiuje wartości bieżącego typu jednostki, które są dostępne.  W przypadku wybrania wartości w przekonwertować, a następnie wartości liczbowe są konwertowane z typu bieżącej jednostki do konwersji, aby wpisać. |
| Etykiety niestandardowej | Tekst do wyświetlenia dla osi Y obok etykiety typ jednostki.  Jeśli określono bez etykiety, jest wyświetlany tylko typ jednostki. |
| **Lista** |
| Kwerendy | Kwerendy na liście.  Zostanie wyświetlona liczba rekordów zwróconych przez kwerendę. |
| Ukrywanie wykresu | Wybierz, aby wyłączyć na wykresie po prawej stronie kolumny. |
| Włączanie wykresów przebiegu w czasie | Wybierz, aby wyświetlić wykres przebiegu w czasie zamiast poziomy pasek.  Aby uzyskać szczegółowe informacje, zobacz [Typowe ustawienia](#sparklines) . |
| Kolor | Kolor słupków bądź wykresów przebiegu w czasie. |
| Operacja | Operacja w celu wykonywania dla wykresu przebiegu w czasie.  Aby uzyskać szczegółowe informacje, zobacz [Typowe ustawienia](#sparklines) . |
| Nazwa i wartość separatora | Pojedynczy znak ogranicznika, jeśli chcesz przeanalizować właściwości tekstu do wielu wartości.  Aby uzyskać szczegółowe informacje, zobacz [Typowe ustawienia](#name-value-separator) . |
| Kwerendy nawigacji | Kwerendy, aby uruchomić, gdy użytkownik wybierze element na liście.  Aby uzyskać szczegółowe informacje, zobacz [Typowe ustawienia](#navigation-query) . |
| **Lista** | **> Tytuły kolumn** |
| Nazwa | Tekst wyświetlany w górnej części pierwszej kolumny listy. |
| Wartość | Tekst wyświetlany w górnej części drugiej kolumny listy. |
| **Lista** | **> Progi** |
| Włączanie progi | Zaznacz, aby włączyć progi.  Aby uzyskać szczegółowe informacje, zobacz [Typowe ustawienia](#thresholds) . |

## <a name="stack-of-line-charts-part"></a>Stos części wykresy linii

W czasie są wyświetlane trzy osobne wykresy z wielu serii z kwerendy dziennika.

![Stos wykresy liniowe](media/log-analytics-view-designer/view-stack-line-charts.png)

| Ustawienie | Opis |
|:--|:--|
| **Ogólne** |
| Tytuł grupy | Tekst wyświetlany w górnej części fragmentu. |
| Nowa grupa | Wybierz, aby utworzyć nową grupę w widoku, zaczynając od bieżącego widoku. |
| Ikona | Plik obrazu, aby wyświetlić obok wynik w nagłówku. |
| **Wykres 1<br>wykresu 2<br>wykresu 3** | **> Nagłówka** |
| Tytuł | Tekst wyświetlany w górnej części wykresu. |
| Pomocniczą | Tekst do wyświetlenia w obszarze tytuł w górnej części wykresu. |
| **Wykres 1<br>wykresu 2<br>wykresu 3** | **Wykres liniowy** |
| Kwerendy | Kwerendy wykresu liniowego.  Pierwszej właściwości powinny być wartość tekstową i drugi właściwość wartość liczbową.  Jest to zazwyczaj kwerendę, która używa **miarę** słowo kluczowe do podsumowania wyników.  Jeśli w kwerendzie użyto słowo kluczowe **Interwał** osi wykresu użyje tego przedziału czasu.  Jeśli kwerenda nie zawiera słowo kluczowe **Interwał** godzinowe interwały są używane na osi x. |
| **Wykres** | **> Oś Y** |
| Używanie Skala logarytmiczna | Zaznacz, aby użyć Skala logarytmiczna dla osi y. |
| Jednostki | Określ jednostki wartości zwrócone przez kwerendę.  Te informacje służy do wyświetlania etykiet, na wykresie wskazujące typy wartości i opcjonalnie do konwertowania wartości.  Typ jednostki określa kategorię jednostki i definiuje wartości bieżącego typu jednostki, które są dostępne.  W przypadku wybrania wartości w przekonwertować, a następnie wartości liczbowe są konwertowane z typu bieżącej jednostki do konwersji, aby wpisać. |
| Etykiety niestandardowej | Tekst do wyświetlenia dla osi Y obok etykiety typ jednostki.  Jeśli określono bez etykiety, jest wyświetlany tylko typ jednostki. |

## <a name="common-settings"></a>Typowe ustawienia
W poniższych sekcjach opisano ustawienia wspólne dla kilku części wizualizacji.

### <a name="name-value-separator">Nazwa i wartość separatora</a>
Pojedynczy znak ogranicznika, jeśli chcesz przeanalizować właściwości tekstu z kwerendy listy do wielu wartości.  Jeśli użytkownik określi ogranicznika, można udostępnić nazwy dla każdego pola, oddzielając je tego samego ogranicznika w polu Nazwa.

Na przykład można rozważyć właściwość o nazwie *lokalizacji* zawierającego wartości, takie jak *41 Redmond konstrukcyjnych* i *Building12 Międzyzdrojach*.  Można określić — nazwy i Separator wartość i *Budowania miast* dla nazwy.  Czy to przeanalizować każdej wartości w dwóch właściwości o nazwie *miasta* i *konstrukcyjnych*. 

### <a name="navigation-query">Kwerendy nawigacji</a>
Kwerendy, aby uruchomić, gdy użytkownik wybierze element na liście.  *{Zaznaczony element}* umożliwia dołączył składnię elementu, który użytkownik wybrał.

Na przykład jeśli zapytanie zawiera kolumnę o nazwie *komputera* i kwerenda nawigacji jest *{zaznaczonego elementu}*, kwerendy takie jak *komputera = "Mój komputer"* zostanie uruchomione gdy użytkownik wybrał komputera.  Jeśli kwerenda nawigacji jest *Typ = zdarzenie {zaznaczonego elementu}* następnie kwerendę *Typ = komputer zdarzenia = "Mój komputer"* zostanie uruchomione.

### <a name="sparklines">Wykresy przebiegu w czasie</a>
Wykres przebiegu w czasie jest wykresu liniowego małych, który przedstawia wartość wpisu listy w czasie.  Dla części wizualizacji z listą można wybrać, czy mają być wyświetlane poziomy pasek względne wartości liczbowe kolumny lub wykres przebiegu w czasie wskazująca jego wartości w czasie.

W poniższej tabeli opisano ustawienia wykresu przebiegu w czasie.

| Ustawienie | Opis |
|:--|:--|
| Włączanie wykresów przebiegu w czasie | Wybierz, aby wyświetlić wykres przebiegu w czasie zamiast poziomy pasek. |
| Operacja | Jeśli włączono wykresów przebiegu w czasie, to operację do wykonania na każdej właściwości na liście w celu obliczania wartości dla wykresu przebiegu w czasie.<br><br>-Ostatniej próbki: Ostatnia wartość serii przedziale czasu.<br>-Max: Wartość maksymalna serii przedziale czasu.<br>-Min: Wartość minimalna serii przedziale czasu.<br>— Suma: Suma wartości dla serii przedziale czasu.<br>— Podsumowanie: Używa tego samego polecenia miary jako kwerendę w nagłówku. |

### <a name="thresholds">Progi</a>
Progi umożliwiają wyświetlanie kolorowe ikony obok każdego elementu na liście, które zapewnia szybki wizualnej elementów, które przekraczają określoną wartość lub mieszczą się w określonym zakresie.  Na przykład można wyświetlić zieloną ikonę dla elementów za pomocą dopuszczalne wartości, żółty, jeśli wartość jest wewnątrz zakresu, który wskazuje ostrzeżenie i czerwony, jeśli go jest większa niż wartość błędu.

Po włączeniu progów dla części, musisz określić progi jeden lub więcej.  Jeśli wartość elementu jest większa niż wartość progowa i mniejsza niż wartość progowa dalej, ten kolor jest używany.  Jeśli element jest większa niż wartość progowa następnie najwyższe, ten kolor jest ustawiona.   

Każdy zestaw progu ma jedną próg o wartości **domyślne**.  Jest to kolor, jeśli nie innych wartości.  Można dodawać lub usuwać progi, klikając pozycję **+** lub przycisk **x** .

W poniższej tabeli opisano ustawienia progów.

| Ustawienie | Opis |
|:--|:--|
| Włączanie progi | Wybierz, aby wyświetlić kolorową ikonę z lewej strony każdego wartość wskazująca swoją kondycję względem określonej progi. |
| Nazwa | Nazwę identyfikującą wartość progowa. |
| Progowa. | Wartość progu.  Kolor najwyższe wartości progowej przekroczenia wartość elementu, którą ustawiony jest kolor kondycji dla każdego elementu listy.  Istnieje jeden próg domyślny jest kolor, jeśli nie wartości progowe. |
| Kolor | Kolor dla wartość progowa. |


## <a name="next-steps"></a>Następne kroki

- Informacje na temat [wyszukiwania dziennika](log-analytics-log-searches.md) do obsługi kwerend w częściach wizualizacji.