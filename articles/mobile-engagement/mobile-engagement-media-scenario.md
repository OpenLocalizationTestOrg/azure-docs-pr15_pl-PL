<properties 
    pageTitle="Azure implementacji zaangażowania Mobile aplikacji multimediów"
    description="Scenariusz aplikacji multimediów do wykonania zaangażowania Mobile Azure" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="08/19/2016"
    ms.author="piyushjo"/>

#<a name="implement-mobile-engagement-with-media-app"></a>Implementowanie zaangażowania urządzeń przenośnych przy użyciu aplikacji multimediów

## <a name="overview"></a>Omówienie

Jan jest menedżerem projektu urządzeń przenośnych firmy duży multimediów. Uruchomione on ostatnio nowej aplikacji, którą ma liczbę bardzo duży plik do pobrania. Ma on trafienie jego cele do pobrania, ale nadal jego zwracana na Investment(ROI) na użytkownika, który nie spełnia jego wymagania. 

Jan zidentyfikował już, dlaczego jego zwrotu z inwestycji jest zbyt niska. Użytkownicy często Zatrzymaj przy użyciu swoich aplikacji po tylko dwa tygodnie, a większość z nich nie wróci. Chce zwiększyć zachowania tej aplikacji.

Po początkowej niektóre, testowanie, ma on materiału po on uczestniczy jego użytkownikom powiadomień wypychanych mają zwykle nadal korzystać z tej aplikacji. Również użytkowników, które były nieaktywne często zwróci się do aplikacji w zależności od powiadomień, które on wysyła je. Jan postanawia inwestycji w niektórych rodzajów zaangażowania Program jego aplikacji używający kierowanie Zaawansowane z powiadomienia wypychane.

Jan odczytu ma ostatnio [Azure Mobile zaangażowania - przewodnika wprowadzenie z najlepszymi rozwiązaniami](mobile-engagement-getting-started-best-practices.md) i postanowił wdrażanie zaleceń z przewodnika.

##<a name="objectives-and-kpis"></a>Celów i wskaźników KPI

Odpowiedzialne za Jana aplikacji z tą osobą. Firm jest generowany z reklam, jak użytkownicy używają jego multimediów. Zwiększając zawartości zużyte na użytkownika Jana zwiększa jego przychodów. Zgodne na jednym głównym celem: Aby zwiększyć sprzedaż z ogłoszenia o 25%. Tworzą firm kluczowych wskaźników wydajności (KPI) do środka i dysk ten cel

* Liczba reklam kliknięte dla poszczególnych użytkowników
* Ile stron artykułów odwiedzonej (na użytkownika / sesji / tygodniowo / miesięcznie...)
* Co to są Ulubione kategorie

Na podstawie Jana spotkania z odpowiedzialne za zdefiniowano on jego firm kluczowych wskaźników wydajności. Wykonuje on część 1 [Azure Mobile zaangażowania - przewodnika wprowadzenie z najlepszymi rozwiązaniami](mobile-engagement-getting-started-best-practices.md). 

Następnie należy utworzyć następujące wskaźniki KPI zaangażowania, aby upewnić się, że osiągnięto cele:

* Monitorowanie przechowywania w następujących odstępach: dzienny, tygodniowy, co tydzień bi i miesięczny.
* Zlicza aktywni użytkownicy
* Ocena aplikacji w aplikacji są przechowywane

Na podstawie zaleceń od zespołu IT, następujące kluczowe wskaźniki wydajności techniczne zostały dodane do odpowiedzieć na następujące pytania:

* Co to jest Mój ścieżki użytkownika (wizyty stronę, do której ilu użytkowników czas spędzony na wykonywaniu go)
* Liczba awarii lub usterek napotkał sesji?
* Jakich wersji systemu operacyjnego uruchomionego moich użytkowników?
* Co to jest średni rozmiar ekranu dla moich użytkowników?
* Jakiego rodzaju połączenia z Internetem mają moich użytkowników?

Dla każdego kluczowego wskaźnika wydajności on klasyfikuje danych wymaganych i on rekordów w właściwego położenia jego playbook.

## <a name="engagement-program-and-integration"></a>Program zaangażowania i integracji

Po zakończeniu tej Jan definiowania jego kluczowych wskaźników wydajności przechodzi jego fazy strategii zaangażowania definiując 4 programy zaangażowania i ich celów:    ![][1]

Następnie Jan przechodzi szczegółowego przez określające powiadomień wypychanych dla każdego programu. Powiadomienia wypychane są definiowane przez pięć elementów:

1. Cel: co to jest celem powiadomienia
2. Jak celem będzie się z Tobą skontaktować
3. Element docelowy: kto otrzyma powiadomienie?
4. Zawartość: Co to jest treść i format powiadomienia (w App/Out: aplikacja)
5. Kiedy: co to jest zalecane moment wysłania powiadomienia wypychane

    ![][2]

Więcej informacji można znaleźć w [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks).

Zgodnie z częścią 2 oficjalny dokument Jan używa sekcji docelowej do definiowania ma do zbierania danych i zapisuje jego znacznik planem wspólnie z dział INFORMATYCZNY rozwiązanie. Po 1 tydzień wykonania i testowania akceptacji użytkownika Jana na końcu można uruchomić jego programów.

##<a name="program-results"></a>Program wyników

4 miesięcy później, Jan przegląda występy programów. Witaj programem a programem tygodniowy spotkania jego cele. Zmniejszy się liczba użytkownika z tylko jedną sesję, większej liczby funkcji w aplikacji są używane i dwukrotnie zawiera liczbę połączeń tygodniu.

**Nieaktywne Program** pomaga zrozumieć tendencji użytkownika Jana. Wydaje się, że 15% nieaktywne użytkowników, wróć do aplikacji. Jednak większość z nich nie loguj się aktywne więcej niż 1 miesiąc. Jan przewiduje potencjalne Optymalizacja sekwencji z dodatkowych powiadomienia i rozwijanie jego wybranej zawartości.

**Poznawanie programu** nie działać prawidłowo. Zwiększa krzyżowe sprzedaży, ale mało do osiągnięcia jego celów. Jan Określa, że adresat nie ma za mało danych, aby wprowadzić odpowiednie kierowanie i zaproponuj odpowiedniej zawartości. On zatrzymaniu tego programu i omówiono wysyłania "powiadomień wypychanych redakcyjny" z zaangażowania Mobile Azure. Jego dziennikarze już rozwiązanie CMS do wysyłania powiadomień wypychanych i nie chcesz zmieniać.

Jan decyduje się za pomocą interfejsu API osiągnięcia, czyli interfejsu API usługi REST protokołu HTTP, umożliwiający zarządzanie kampanii Reach bez konieczności używania interfejsu AZME w sieci Web. Z tej metody Jan może zbierać dane, on musi i zezwolić na jego autorom na korzystać rozwiązanie CMS.

Aby upewnić się, że funkcja działa poprawnie, Jan zwróci dział INFORMATYCZNY wzmożonej uwagi w następujących sprawach:

1. **Systemów operacyjnych** : wszystkie mają własne reguły do zarządzania zewnętrznymi powiadomienia wypychane, więc Jan decyduje o treści wyświetlić listę wszystkich przypadkach i sprawdzania, jeśli za pośrednictwem interfejsów API samodzielnie je.
Np.: System Android wypychanych umożliwia przeglądu, której nie ma litery z systemem iOS.

2. **Przedział czasu**: Jan chce interfejsu API, który przedział czasu i ustawianie punktu końcowego do kampanii. Chce zachować użytkowników z dowolnej sabotaż kłopotliwe środki powiadomienie.

3. **Kategorie**: Marketing zespołu przygotowanie szablonu dla każdego typu alertu. Jan zapytanie zespołowi Ustaw kategorie w interfejsie API.

Po niektórych testów Jan jest spełniony. Dzięki tym API dziennikarze może nadal wysyłać powiadomienia wypychane wraz z ich CMS i zaangażowania Mobile Azure zbiera dane funkcjonalne dla nich

Po tych 4 najpierw miesięcy, wyniki odzwierciedlają dobre ogólną wydajność i zapewnia ufności dla Jana i jego tablicy zwrotu z inwestycji na zwiększenie użytkownika na 15% i Sprzedaż mobilna reprezentują % 17,5 cala łącznej sprzedaży, wzrost % 7.5 tylko cztery miesiące.

<!--Image references-->
[1]: ./media/mobile-engagement-media-scenario/engagement-strategy.png
[2]: ./media/mobile-engagement-media-scenario/push-scenarios.png

<!--Link references-->
[Media Playbook link]: https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks
