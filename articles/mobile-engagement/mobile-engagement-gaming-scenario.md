<properties 
    pageTitle="Azure implementacji zaangażowania Mobile aplikacji gier"
    description="Scenariusz aplikacji gier do wykonania zaangażowania Mobile Azure" 
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

#<a name="implement-mobile-engagement-with-gaming-app"></a>Implementowanie zaangażowania urządzeń przenośnych z aplikacją gier

## <a name="overview"></a>Omówienie

Uruchamiania gier została uruchomiona nowej aplikacji gier role play/strategii wędkarstwa podstawie. Gry został i rozpocząć pracę, 6 miesięcy. Gry jest duży sukcesu i ma miliony pliki do pobrania i przechowywania jest bardzo wysoki w porównaniu z innymi aplikacjami gier uruchamiania. Na spotkaniu kwartalnych Recenzja biorący udział w projekcie zgadza potrzebnych Zwiększ Średni przychód na użytkownika (ARPU). Pakiety w gry Premium są dostępne jako oferty specjalne. Te pakiety gier Zezwalanie użytkownikom na uaktualnianie wyglądu i wydajności ich wędkarstwa linii i przynętą lub tackles gry. Jednak sprzedaż pakietu jest bardzo niskie. Dlatego ich zdecydować, najpierw do analizowania obsługi klientów za pomocą narzędzia do analizy, a następnie opracowywaniu zaangażowania program, aby zwiększyć sprzedaży za pomocą zaawansowanych segmentacji.

Oparte na [Azure Mobile zaangażowania - przewodnika wprowadzenie z najlepszymi rozwiązaniami](mobile-engagement-getting-started-best-practices.md) tworzenia strategii zaangażowania.

##<a name="objectives-and-kpis"></a>Celów i wskaźników KPI

Odpowiedzialne za dla gry z tą osobą. Wszystkie zgadza się na jednym głównym celem - zwiększenie sprzedaży pakiet premium przez 15%. Tworzą firm kluczowych wskaźników wydajności (KPI) do środka i dysk ten cel

* Od poziomu gry te pakiety zakupionych?
* Co to jest zysk dla poszczególnych użytkowników, sesji tygodniu i miesięcznie?
* Co to są typy Ulubione zakupu?

1 część [Przewodnika wprowadzenie](mobile-engagement-getting-started-best-practices.md) wyjaśniono, jak definiowanie celów i wskaźników KPI. 

Ze wskaźnikami KPI firm teraz zdefiniowane Mobile Manager produktu tworzy zaangażowania kluczowych wskaźników wydajności do określenia nowego użytkownika trendów i przechowywania.

* Monitorowanie przechowywania i korzystanie z w następujących odstępach: codziennie, co 2 dni, tygodniowy, miesięczny i co trzy miesiące
* Zlicza aktywnego użytkownika
* Ocena aplikacji w sklepie

Na podstawie zaleceń od zespołu IT, następujące kluczowe wskaźniki wydajności techniczne zostały dodane do odpowiedzieć na następujące pytania:

* Co to jest Mój ścieżki użytkownika (stronę, do której jest odwiedzonej, ile użytkowników wydatki na jego)
* Liczba awarii lub usterek napotkał sesji
* Jakich wersji systemu operacyjnego uruchomionego moich użytkowników?
* Co to jest średni rozmiar ekranu dla moich użytkowników?
* Jakiego rodzaju łączność z Internetem mają moich użytkowników?

Dla każdego kluczowego wskaźnika wydajności Mobile Manager produktu określa dane potrzebuje i miejsce, w którym znajduje się w jej playbook.

## <a name="engagement-program-and-integration"></a>Program zaangażowania i integracji

Przed tworzenia programu zaawansowane zaangażowania, dyrektora projektu Mobile odpowiedzialnych za projekt powinien mieć głębokości objaśnienie, jak i produkty są używane przez użytkowników.

Po 3 miesiące dyrektora projektu Mobile zebrane za mało danych w celu poprawienia jego sprzedaż powiadomień wypychanych w aplikacji. Dowiaduje się on który:

* Pierwszym zakupie zwykle dzieje na poziomie 14 90% tych przypadkach zakupu jest nowe broni legendarnych $3.
* W tych przypadkach 80% użytkowników, którzy zakupieniu kontynuować produktu i wprowadzić więcej zakupy.
* Użytkownicy, którzy upłynęło poziomu 20, uruchom poświęcić więcej niż 10/ tydzień.
* Użytkownicy korzystają z nich kupić pakiety premium na poziomie 16, 24 i 32.

Dzięki tej analizy dyrektora projektu Mobile postanawia tworzenie konkretnych sekwencji powiadomień, aby zwiększyć w sprzedaży aplikacji. Należy utworzyć trzy sekwencji wypychanych, które on połączeń: Zapraszamy program, Program sprzedaży i nieaktywne Program. Więcej informacji można znaleźć w [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks)
    ![][1]

<!--Image references-->

[1]: ./media/mobile-engagement-game-scenario/notification-scenario.png

<!--Link references-->
