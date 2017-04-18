<properties
    pageTitle="Logowanie odwołania kafelków projektanta widoku analizy | Microsoft Azure"
    description="Projektant widoków w dzienniku analizy umożliwia tworzenie niestandardowych widoków w konsoli usługi OMS, które zawierają różne wizualizacje danych w repozytorium usługi OMS. Ten artykuł zawiera odwołanie ustawienia dla każdego kafelków dostępny do użytku w sekcji Widoki niestandardowe."
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
    ms.date="09/27/2016"
    ms.author="bwren"/>

# <a name="log-analytics-view-designer-tile-reference"></a>Odwołania kafelków projektanta widoku analizy dziennika
Projektant widoków w dzienniku analizy umożliwia tworzenie niestandardowych widoków w konsoli usługi OMS, które zawierają różne wizualizacje danych w repozytorium usługi OMS. Ten artykuł zawiera odwołanie ustawienia dla każdego kafelków dostępny do użytku w sekcji Widoki niestandardowe.

Inne artykuły, które są dostępne dla projektanta widoku są:

- [Projektant widoków](log-analytics-view-designer.md) — omówienie Projektant widoków i procedur dotyczących tworzenia i edytowania widoków niestandardowych.
- [Odwołanie do wizualizacji części](log-analytics-view-designer-parts.md) — odwołanie ustawienia dla każdego kafelków dostępny do użytku w sekcji Widoki niestandardowe. 


W poniższej tabeli wymieniono różne rodzaje Kafelki dostępnych w Projektancie widoku.  Poniżej opisano każdy typ kafelka w szczegóły i ich właściwości.

| Kafelków | Opis |
|:--|:--|
| [Liczba](#number-tile) | Jeden numer pokazujący liczbę rekordów z kwerendy. |
| [Dwóch liczb.](#two-numbers-tile) | Dwie liczby pojedynczej przedstawiający liczby rekordów z dwóch różnych zapytań. |
| [Pierścieniowy](#donut-tile) | Wykres pierścieniowy na podstawie zapytania z wartością podsumowania w Centrum. |
| [Wykres liniowy i objaśnienia](#line-chart-amp-callout-tile) | Wykres liniowy, na podstawie zapytania i objaśnienia z wartością podsumowania. |
| [Wykres liniowy](#line-chart-tile) | Wykres liniowy, na podstawie zapytania. |
| [Dwie osie czasu](#two-timelines-tile) | Wykres kolumnowy z dwóch serie, które każdy na podstawie oddzielnych zapytania. |



## <a name="number-tile"></a>Liczba kafelków

Kafelek **numer** Wyświetla liczbę pojedynczy pokazujący liczbę rekordów z kwerendy dziennika i etykiety.

![Liczba kafelków](media/log-analytics-view-designer/tile-number.png)

| Ustawienie | Opis |
|:--|:--|
| Nazwa        | Tekst wyświetlany w górnej części fragmentu. |
| Opis | Tekst do wyświetlenia w obszarze Nazwa kafelka.    |
| **Kafelków** |
| Legendy | Tekst do wyświetlenia w obszarze wartości. |
| Kwerendy | Kwerendy.  Zostanie wyświetlona liczba rekordów zwróconych przez kwerendę. |
| **Zaawansowane** |  **> Weryfikacja przepływu danych** |
| Włączone | Zaznaczenie weryfikacji przepływu danych powinna być włączona dla kafelka.  Udostępnia alternatywny wiadomości, jeśli dane nie są dostępne dla kafelka.  Jest to zazwyczaj używane do dostarczania wiadomości podczas okresu tymczasowy po zainstalowaniu widoku i danych jest dostępna. |
| Kwerendy | Kwerendy, uruchomić, aby sprawdzić, czy jest dostępna w widoku danych.  Jeśli kwerenda zwraca żadnych wyników, zamiast wartości z głównego kwerendy zostanie wyświetlony komunikat z. |
| Komunikat | Komunikat wyświetlany, jeśli kwerenda weryfikacji przepływu danych zwraca żadnych danych.  Jeśli podasz żaden komunikat jest wyświetlany *Wykonywania oceny* . |
| **Interwał** |
| Czas trwania | Okres od daty bieżącej interwału czasu kwerendy.  Na przykład jeśli określono **7 dni** , następnie kwerenda jest ograniczone do rekordów utworzonych na podstawie 7 dni do daty bieżącej. |
| Przesunięcie danych zakończenia | Opcjonalnie przesunięcie od bieżącego danych do użycia w przedział czasu głównym kwerendy.  Na przykład jeśli **dzień -1** jest używane **Przesunięcie daty zakończenia** i **7 dni** na potrzeby **czasu trwania**, następnie kwerendy jest ograniczone do rekordów utworzonych na podstawie 8 dni do wczoraj. |

## <a name="two-numbers-tile"></a>Kafelka dwóch liczb.

Kafelków **Dwie liczby** są wyświetlane dwie liczby pokazujący statystykę rekordy z dwóch różnych dziennika kwerend i etykietę dla każdej z nich.

![Kafelka dwóch liczb.](media/log-analytics-view-designer/tile-two-numbers.png)

| Ustawienie | Opis |
|:--|:--|
| Nazwa        | Tekst wyświetlany w górnej części fragmentu. |
| Opis | Tekst do wyświetlenia w obszarze Nazwa kafelka.    |
| **Pierwszy fragment** |
| Legendy | Tekst do wyświetlenia w obszarze wartości. |
| Kwerendy | Kwerendy.  Zostanie wyświetlona liczba rekordów zwróconych przez kwerendę. |
| **Druga kafelków** |
| Legendy | Tekst do wyświetlenia w obszarze wartości. |
| Kwerendy | Kwerendy.  Zostanie wyświetlona liczba rekordów zwróconych przez kwerendę. |
| **Zaawansowane** | **> Weryfikacja przepływu danych** |
| Włączone | Zaznaczenie weryfikacji przepływu danych powinna być włączona dla kafelka.  Udostępnia alternatywny wiadomości, jeśli dane nie są dostępne dla kafelka.  Jest to zazwyczaj używane do dostarczania wiadomości podczas okresu tymczasowy po zainstalowaniu widoku i danych jest dostępna. |
| Kwerendy | Kwerendy, uruchomić, aby sprawdzić, czy jest dostępna w widoku danych.  Jeśli kwerenda zwraca żadnych wyników, zamiast wartości z głównego kwerendy zostanie wyświetlony komunikat z. |
| Komunikat | Komunikat wyświetlany, jeśli kwerenda weryfikacji przepływu danych zwraca żadnych danych.  Jeśli podasz żaden komunikat jest wyświetlany *Wykonywania oceny* . |
| **Interwał** |
| Czas trwania | Okres od daty bieżącej interwału czasu kwerendy.  Na przykład jeśli określono **7 dni** , następnie kwerenda jest ograniczone do rekordów utworzonych na podstawie 7 dni do daty bieżącej. |
| Przesunięcie danych zakończenia | Opcjonalnie przesunięcie od bieżącego danych do użycia w przedział czasu głównym kwerendy.  Na przykład jeśli **dzień -1** jest używane **Przesunięcie daty zakończenia** i **7 dni** na potrzeby **czasu trwania**, następnie kwerendy jest ograniczone do rekordów utworzonych na podstawie 8 dni do wczoraj. |

## <a name="donut-tile"></a>Pierścień kafelków

Kafelków **pierścieniowego** Wyświetla liczbę pojedynczy podsumowane z wartości kolumny w kwerendzie dziennika.  Pierścień wyświetla wyniki górny trzy rekordy.

![Pierścień kafelków](media/log-analytics-view-designer/tile-donut.png)

| Ustawienie | Opis |
|:--|:--|
| Nazwa        | Tekst wyświetlany w górnej części fragmentu. |
| Opis | Tekst do wyświetlenia w obszarze Nazwa kafelka.    |
| **Pierścieniowy** |
| Kwerendy | Kwerendy dla pierścieniowego.  Pierwszej właściwości powinny być wartość tekstową i drugi właściwość wartość liczbową.  Jest to zazwyczaj kwerendę, która używa **miarę** słowo kluczowe do podsumowania wyników. |
| **Pierścieniowy** | **> Centrum** |
| Tekst | Tekst do wyświetlenia w obszarze wartości wewnątrz pierścieniowego. |
| Operacja | Operacja do wykonania na właściwości wartości do podsumowania na pojedynczą wartość.<br><br>— Suma: Dodawanie wartości wszystkie rekordy z wartością właściwości.<br>— Procent: Procent równy określonemu procentowi wartości z rekordami z wartością właściwości w porównaniu z równy określonemu procentowi wartości wszystkich rekordów. |
| Wartości wyników używana w operacji Centrum | Opcjonalnie kliknij znak plus, aby dodać jedną lub kilka wartości.  Wyniki kwerendy będą ograniczone do rekordów zawierających wartości właściwości, które można określić.  Jeśli wartości nie zostaną dodane, niż wszystkie rekordy znajdują się w kwerendzie. |
| **Pierścieniowy** | **> Dodatkowe opcje** |
| Kolory | Kolor, który ma być wyświetlane dla każdej z trzech właściwości górnego.  Jeśli chcesz określić alternatywny kolory określone wartości właściwości, należy użyć, zaawansowane mapowanie koloru. |
| Mapowanie zaawansowane kolorów | Wyświetla kolor dla określonej wartości.  Jeśli wartość zadanej znajduje się w trzy pierwsze, alternatywny kolor zostanie wyświetlona zamiast kolorów standardowych.  Jeśli właściwość nie jest trzy pierwsze, kolor nie jest wyświetlane. |
| **Zaawansowane** | **> Weryfikacja przepływu danych** |
| Włączone | Zaznaczenie weryfikacji przepływu danych powinna być włączona dla kafelka.  Udostępnia alternatywny wiadomości, jeśli dane nie są dostępne dla kafelka.  Jest to zazwyczaj używane do dostarczania wiadomości podczas okresu tymczasowy po zainstalowaniu widoku i danych jest dostępna. |
| Kwerendy | Kwerendy, uruchomić, aby sprawdzić, czy jest dostępna w widoku danych.  Jeśli kwerenda zwraca żadnych wyników, zamiast wartości z głównego kwerendy zostanie wyświetlony komunikat z. |
| Komunikat | Komunikat wyświetlany, jeśli kwerenda weryfikacji przepływu danych zwraca żadnych danych.  Jeśli podasz żaden komunikat jest wyświetlany *Wykonywania oceny* . |
| **Interwał** |
| Czas trwania | Okres od daty bieżącej interwału czasu kwerendy.  Na przykład jeśli określono **7 dni** , następnie kwerenda jest ograniczone do rekordów utworzonych na podstawie 7 dni do daty bieżącej. |
| Przesunięcie danych zakończenia | Opcjonalnie przesunięcie od bieżącego danych do użycia w przedział czasu głównym kwerendy.  Na przykład jeśli **dzień -1** jest używane **Przesunięcie daty zakończenia** i **7 dni** na potrzeby **czasu trwania**, następnie kwerendy jest ograniczone do rekordów utworzonych na podstawie 8 dni do wczoraj. |

## <a name="line-chart-tile"></a>Linia wykresu kafelków

Fragment **wykresu liniowego** Wyświetla wykres liniowy z wielu serii z kwerendy dziennika w czasie.  

![Wykres liniowy i objaśnienia kafelków](media/log-analytics-view-designer/tile-line-chart.png)

| Ustawienie | Opis |
|:--|:--|
| Nazwa        | Tekst wyświetlany w górnej części fragmentu. |
| Opis | Tekst do wyświetlenia w obszarze Nazwa kafelka.    |
| **Wykres liniowy** |  
| Kwerendy | Kwerendy wykresu liniowego.  Pierwszej właściwości powinny być wartość tekstową i drugi właściwość wartość liczbową.  Jest to zazwyczaj kwerendę, która używa **miarę** słowo kluczowe do podsumowania wyników.  Jeśli w kwerendzie użyto słowo kluczowe **Interwał** osi wykresu użyje tego przedziału czasu.  Jeśli kwerenda nie zawiera słowo kluczowe **Interwał** godzinowe interwały są używane na osi x. |
| **Wykres liniowy** | **> Oś Y** |
| Używanie Skala logarytmiczna | Zaznacz, aby użyć Skala logarytmiczna dla osi y. |
| Jednostki | Określ jednostki wartości zwrócone przez kwerendę.  Te informacje służy do wyświetlania etykiet, na wykresie wskazujące typy wartości i opcjonalnie do konwertowania wartości.  **Typ jednostki** określa kategorię jednostki i definiuje wartości **Bieżącego typu jednostki** , które są dostępne.  Wybranie wartości w **przekonwertować** wartości liczbowe są konwertowane w z typu **Bieżącej jednostki** **Konwertowanie** typu. |
| Etykiety niestandardowej | Tekst do wyświetlenia dla osi Y obok etykiety typ jednostki.  Jeśli określono bez etykiety, jest wyświetlany tylko typ jednostki. |
| **Zaawansowane** | **> Weryfikacja przepływu danych** |
| Włączone | Zaznaczenie weryfikacji przepływu danych powinna być włączona dla kafelka.  Udostępnia alternatywny wiadomości, jeśli dane nie są dostępne dla kafelka.  Jest to zazwyczaj używane do dostarczania wiadomości podczas okresu tymczasowy po zainstalowaniu widoku i danych jest dostępna. |
| Kwerendy | Kwerendy, uruchomić, aby sprawdzić, czy jest dostępna w widoku danych.  Jeśli kwerenda zwraca żadnych wyników, zamiast wartości z głównego kwerendy zostanie wyświetlony komunikat z. |
| Komunikat | Komunikat wyświetlany, jeśli kwerenda weryfikacji przepływu danych zwraca żadnych danych.  Jeśli podasz żaden komunikat jest wyświetlany *Wykonywania oceny* . |
| **Interwał** |
| Czas trwania | Okres od daty bieżącej interwału czasu kwerendy.  Na przykład jeśli określono **7 dni** , następnie kwerenda jest ograniczone do rekordów utworzonych na podstawie 7 dni do daty bieżącej. |
| Przesunięcie danych zakończenia | Opcjonalnie przesunięcie od bieżącego danych do użycia w przedział czasu głównym kwerendy.  Na przykład jeśli **dzień -1** jest używane **Przesunięcie daty zakończenia** i **7 dni** na potrzeby **czasu trwania**, następnie kwerendy jest ograniczone do rekordów utworzonych na podstawie 8 dni do wczoraj. |


## <a name="line-chart--callout-tile"></a>Linia wykresu i objaśnienia kafelków

Fragment **wykresu liniowego i objaśnienia** Wyświetla wykres liniowy z wielu serii z kwerendy dziennika przez godzinę i objaśnienia z wartością podsumowane.  

![Wykres liniowy i objaśnienia kafelków](media/log-analytics-view-designer/tile-line-chart-callout.png)

| Ustawienie | Opis |
|:--|:--|
| Nazwa        | Tekst wyświetlany w górnej części fragmentu. |
| Opis | Tekst do wyświetlenia w obszarze Nazwa kafelka.    |
| **Wykres liniowy** |  
| Kwerendy | Kwerendy wykresu liniowego.  Pierwszej właściwości powinny być wartość tekstową i drugi właściwość wartość liczbową.  Jest to zazwyczaj kwerendę, która używa **miarę** słowo kluczowe do podsumowania wyników.  Jeśli w kwerendzie użyto słowo kluczowe **Interwał** osi wykresu użyje tego przedziału czasu.  Jeśli kwerenda nie zawiera słowo kluczowe **Interwał** godzinowe interwały są używane na osi x. |
| **Wykres liniowy** | **> Objaśnienie** |
| Objaśnienia | Tekst tytułu wyświetlany powyżej wartości objaśnienie. |
| Nazwa serii | Wartość właściwości dla serii, aby użyć wartości objaśnienie.  Jeśli podano bez serii są używane wszystkie rekordy z kwerendy. |
| Operacja | Operacja do wykonania na właściwości wartości do podsumowania na pojedynczą wartość objaśnienia.<br>-Średnia: Średniej wartości ze wszystkich rekordów.<br><br>-Liczba: Liczba wszystkie rekordy zwrócone przez kwerendę.<br>-Ostatniej próbki: Wartość od ostatniej interwał zawartych w wykresie.<br>-Max: Wartość maksymalna z interwałów zawartych w wykresie.<br>-Min: Wartość minimalna z interwałów zawartych w wykresie.<br>— Suma: Suma wartości ze wszystkich rekordów. |
| **Wykres liniowy** | **> Oś Y** |
| Używanie Skala logarytmiczna | Zaznacz, aby użyć Skala logarytmiczna dla osi y. |
| Jednostki | Określ jednostki wartości zwrócone przez kwerendę.  Te informacje służy do wyświetlania etykiet, na wykresie wskazujące typy wartości i opcjonalnie do konwertowania wartości.  **Typ jednostki** określa kategorię jednostki i definiuje wartości **Bieżącego typu jednostki** , które są dostępne.  Wybranie wartości w **przekonwertować** wartości liczbowe są konwertowane w z typu **Bieżącej jednostki** **Konwertowanie** typu. |
| Etykiety niestandardowej | Tekst do wyświetlenia dla osi Y obok etykiety typ jednostki.  Jeśli określono bez etykiety, jest wyświetlany tylko typ jednostki. |
| **Zaawansowane** | **> Weryfikacja przepływu danych** |
| Włączone | Zaznaczenie weryfikacji przepływu danych powinna być włączona dla kafelka.  Udostępnia alternatywny wiadomości, jeśli dane nie są dostępne dla kafelka.  Jest to zazwyczaj używane do dostarczania wiadomości podczas okresu tymczasowy po zainstalowaniu widoku i danych jest dostępna. |
| Kwerendy | Kwerendy, uruchomić, aby sprawdzić, czy jest dostępna w widoku danych.  Jeśli kwerenda zwraca żadnych wyników, zamiast wartości z głównego kwerendy zostanie wyświetlony komunikat z. |
| Komunikat | Komunikat wyświetlany, jeśli kwerenda weryfikacji przepływu danych zwraca żadnych danych.  Jeśli podasz żaden komunikat jest wyświetlany *Wykonywania oceny* . |
| **Interwał** |
| Czas trwania | Okres od daty bieżącej interwału czasu kwerendy.  Na przykład jeśli określono **7 dni** , następnie kwerenda jest ograniczone do rekordów utworzonych na podstawie 7 dni do daty bieżącej. |
| Przesunięcie danych zakończenia | Opcjonalnie przesunięcie od bieżącego danych do użycia w przedział czasu głównym kwerendy.  Na przykład jeśli **dzień -1** jest używane **Przesunięcie daty zakończenia** i **7 dni** na potrzeby **czasu trwania**, następnie kwerendy jest ograniczone do rekordów utworzonych na podstawie 8 dni do wczoraj. |

## <a name="two-timelines-tile"></a>Fragment dwie osie czasu

Fragment **dwie osie czasu** wyświetla wyniki dwóch zapytań dziennika czasie jako wykres kolumnowy.  Objaśnienie jest wyświetlany dla każdej serii.  

![Fragment dwie osie czasu](media/log-analytics-view-designer/tile-two-timelines.png)

| Ustawienie | Opis |
|:--|:--|
| Nazwa        | Tekst wyświetlany w górnej części fragmentu. |
| Opis | Tekst do wyświetlenia w obszarze Nazwa kafelka.    |
| Pierwszy wykres   
| Legendy | Tekst do wyświetlenia w obszarze objaśnień dla pierwszej serii.
| Kolor | Kolor używany dla kolumn w pierwszej serii.
| Wykres zapytania | Kwerendy dla pierwszej serii.  Liczba rekordów w każdym przedziale czasu będą reprezentowane przez kolumn wykresu.
| Operacja | Operacja do wykonania na właściwości wartości do podsumowania na pojedynczą wartość objaśnienia.<br><br>-Średnia: Średniej wartości ze wszystkich rekordów.<br>-Liczba: Liczba wszystkie rekordy zwrócone przez kwerendę.<br>-Ostatniej próbki: Wartość od ostatniej interwał uwzględnione na wykresie.<br>-Max: Wartość maksymalna z interwałów zawartych w wykresie.
| **Drugi wykres** |
| Legendy | Tekst do wyświetlenia w obszarze objaśnienia do drugiej serii.
| Kolor | Kolor używany dla kolumn w drugiej serii.
| Wykres zapytania | Kwerendy dla drugiej serii.  Liczba rekordów w każdym przedziale czasu będą reprezentowane przez kolumn wykresu.
| Operacja | Operacja do wykonania na właściwości wartości do podsumowania na pojedynczą wartość objaśnienia.<br><br>-Średnia: Średniej wartości ze wszystkich rekordów.<br>-Liczba: Liczba wszystkie rekordy zwrócone przez kwerendę.<br>-Ostatniej próbki: Wartość od ostatniej interwał zawartych w wykresie.<br>-Max: Wartość maksymalna z interwałów zawartych w wykresie. |
| **Zaawansowane** | **> Weryfikacja przepływu danych** |
| Włączone | Zaznaczenie weryfikacji przepływu danych powinna być włączona dla kafelka.  Udostępnia alternatywny wiadomości, jeśli dane nie są dostępne dla kafelka.  Jest to zazwyczaj używane do dostarczania wiadomości podczas okresu tymczasowy po zainstalowaniu widoku i danych jest dostępna. |
| Kwerendy | Kwerendy, uruchomić, aby sprawdzić, czy jest dostępna w widoku danych.  Jeśli kwerenda zwraca żadnych wyników, zamiast wartości z głównego kwerendy zostanie wyświetlony komunikat z. |
| Komunikat | Komunikat wyświetlany, jeśli kwerenda weryfikacji przepływu danych zwraca żadnych danych.  Jeśli podasz żaden komunikat jest wyświetlany *Wykonywania oceny* . |
| **Interwał** |
| Czas trwania | Okres od daty bieżącej interwału czasu kwerendy.  Na przykład jeśli określono **7 dni** , następnie kwerenda jest ograniczone do rekordów utworzonych na podstawie 7 dni do daty bieżącej. |
| Przesunięcie danych zakończenia | Opcjonalnie przesunięcie od bieżącego danych do użycia w przedział czasu głównym kwerendy.  Na przykład jeśli **dzień -1** jest używane **Przesunięcie daty zakończenia** i **7 dni** na potrzeby **czasu trwania**, następnie kwerendy jest ograniczone do rekordów utworzonych na podstawie 8 dni do wczoraj. |

## <a name="next-steps"></a>Następne kroki

- Informacje na temat [wyszukiwania dziennika](log-analytics-log-searches.md) do obsługi kwerend w kafelki.
- Dodawanie [Składników wizualizacji](log-analytics-view-designer-parts.md) do widoku niestandardowym.