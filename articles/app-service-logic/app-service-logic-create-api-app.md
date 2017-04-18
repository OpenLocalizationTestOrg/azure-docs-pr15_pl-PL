<properties 
    pageTitle="Tworzenie interfejsu API dla aplikacji warunków logicznych" 
    description="Tworzenie niestandardowego interfejsu API do użytku z logiki aplikacji" 
    authors="jeffhollan" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na" 
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/>
    
# <a name="creating-a-custom-api-to-use-with-logic-apps"></a>Tworzenie niestandardowego interfejsu API do użytku z logiki aplikacji

Jeśli chcesz rozszerzyć platformie logiki aplikacji, istnieje wiele sposobów, który można zadzwonić do interfejsów API lub systemów, które nie są dostępne w jednym z naszych wiele łączników w nowym polu.  Jeden z tych sposobów tworzenia aplikacji API możesz nawiązać połączenie aplikacji logika przepływu pracy.

## <a name="helpful-tools"></a>Przydatne narzędzia

Interfejsów API pod kątem aplikacji logiczny zalecamy Generowanie dokumentu [swagger](http://swagger.io) szczegółów operacji obsługiwane i parametrów dla interfejsu API usługi.  Istnieje wiele bibliotek (na przykład [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle)), które automatycznie generuje swagger dla Ciebie.  Za pomocą [TRex](https://github.com/nihaue/TRex) ułatwiające adnotowanie swagger do pracy z aplikacji logiczny (Wyświetl nazwy, typy właściwości itp.).  Dla niektórych próbek interfejsu API aplikacje utworzono dla aplikacji logika należy koniecznie zapoznaj się z naszą [repozytorium GitHub](http://github.com/logicappsio) lub w [blogu](http://aka.ms/logicappsblog).

## <a name="actions"></a>Akcje

Podstawowe Akcja dla aplikacji logika jest kontrolerze, który będzie zaakceptować żądanie HTTP i zwrócić odpowiedź (zazwyczaj 200).  Jednak istnieje różnych wzorców, które można wykonać, aby rozszerzyć akcji, w zależności od potrzeb.

Domyślnie aparat aplikacji logiczny będzie Przekroczono limit czasu żądania po 1 minucie.  Jednak możesz mieć do interfejsu API wykonać na akcje, które trwać dłużej i że masz aparat czekać na ukończenie, wykonując deseniu asynchroniczne albo webhook poniżej.

Standardowe czynności po prostu wpisz metoda żądania HTTP w swojej interfejsu API, który jest dostępne za pośrednictwem swagger.  Przykłady aplikacji interfejsu API, które współpracują z aplikacji logicznych w naszym [repozytorium GitHub](https://github.com/logicappsio)jest widoczny.  Poniżej przedstawiono sposoby wykonywania typowych wzorców z łącznika niestandardowego.

### <a name="long-running-actions---async-pattern"></a>Akcje długotrwałych — wzór asynchroniczne

Podczas uruchamiania krok długo lub zadania, najpierw należy wykonać jest upewnij się, że aparat wie, że nie przekroczono limit czasu. Trzeba również komunikowanie się za pomocą aparat jak będą wiedzieć, po zakończeniu pracy z zadaniem, a ponadto należy zwrócić odpowiednie dane do aparatu, więc można kontynuować przepływu pracy. Które za pośrednictwem interfejsu API można wykonać, wykonując poniższe przepływu. Te kroki są z punktu widzenia niestandardowego interfejsu API:

1. Po odebraniu żądania natychmiast powrócić odpowiedzi (aby praca jest wykonywana). Odpowiedź będzie `202 ACCEPTED` odpowiedzi, co aparat wiedzieć, masz dane, odrzucane ładunku, a teraz przetwarzania. 202 odpowiedzi powinien zawierać nagłówki następujące czynności: 
 * `location`Nagłówek (wymagany): jest to bezwzględną ścieżkę do adresu URL aplikacji logika umożliwia sprawdzanie stanu zadania.
 * `retry-after`(opcjonalnie, będzie domyślnie 20 do akcji). To liczbę sekund aparat czas oczekiwania przed sondowanie adres URL lokalizacji nagłówka, aby sprawdzić stan.

2. Po zaznaczeniu stan zadania wykonywane są następujące testy: 
 * Jeśli zadanie jest wykonane: zwracana `200 OK` odpowiedź ładunek odpowiedź.
 * Jeśli zadanie jest przetwarzana: zwracana innego `202 ACCEPTED` odpowiedzi z tym samym nagłówkami początkowej odpowiedzi

Ten wzorzec pozwala na uruchamianie bardzo długo zadań w wątku swojego niestandardowego interfejsu API, ale zachować aktywnego połączenia aktywności z aparatu logiki aplikacji, nie przekroczono limit czasu lub kontynuowanie przed zakończeniem pracy. Dodawanie tego elementu do aplikacji logiczny, jest należy zauważyć, że nie ma potrzeby wykonywać żadnych w swojej definicji aplikacji logicznych w dalszym ciągu ankieta i sprawdź stan. Jak aparat wykryje 202 odpowiedzi ZAAKCEPTOWANE z nagłówkiem prawidłową lokalizację, go będą przestrzegać deseniu asynchroniczne i przejdź do ankieta nagłówka lokalizacji, dopóki nie zostanie zwrócona innych niż 202.

Zobaczysz próbki tego wzorca w GitHub [tutaj](https://github.com/jeffhollan/LogicAppsAsyncResponseSample)

### <a name="webhook-actions"></a>Akcje Webhook

Podczas przepływu pracy możesz mieć aplikacji logiczny, Zatrzymaj wskaźnik myszy i poczekaj, aż "zwrotnego" kontynuować.  Jest to wywołanie zwrotne w postaci HTTP POST.  Aby zastosować tego wzorca, należy podać dwa punkty końcowe na kontrolerze: rozpocząć subskrypcję lub.

Na subskrypcji aplikacja logiczny utworzy i rejestrować zwrotnego adresu URL, którego z interfejsu API można przechowywać i zwrotnego z gotowych jako WPIS HTTP.  Wszelkie zawartości i nagłówki zostaną przekazane do aplikacji logika i mogą być używane w pozostałej części przepływu pracy.  Aparat aplikacji logika połączeń punktu Subskrybuj na wykonanie zaraz po jego trafienia tego kroku.

Jeśli Uruchom została anulowana, aparat aplikacji logika będzie nawiązywanie połączenia z punktem końcowym "Anuluj subskrypcję".  Z interfejsu API można następnie unregister zwrotnego adresu URL, stosownie do potrzeb.

Obecnie projektanta aplikacji logiczny nie obsługuje wykrywanie punkt końcowy webhook za pośrednictwem swagger, tak aby użyć tego typu akcji należy dodać akcję "Webhook" i określ adres URL, nagłówki i treści żądania.  Możesz użyć `@listCallbackUrl()` funkcji przepływu pracy w dowolnym z tych pól, aby przekazać w adresie URL zwrotnego.

Zobaczysz próbkę wzorzec webhook GitHub [tutaj](https://github.com/jeffhollan/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/WebhookTriggerController.cs)

## <a name="triggers"></a>Wyzwalaczy

Oprócz akcje może być swojego niestandardowego act interfejsu API wyzwalacza do aplikacji logicznych.  Istnieją dwa desenie, które można wykonać poniżej wyzwalać aplikacji logiczny:

### <a name="polling-triggers"></a>Ankieta wyzwalaczy

Wyzwalacze ankieta działa podobnie jak akcje długie asynchroniczne uruchomiony powyżej.  Aparat aplikacji logika nawiąże połączenie punkt końcowy wyzwalacza po upływie pewnego czasu, jaki upłynął (zależne od wersji 15 sekundach Premium 1 minuta standardu, a 1 godzina bezpłatnej).

Jeśli dane są dostępne, zwraca wyzwalacz `202 ACCEPTED` odpowiedzi, z `location` i `retry-after` nagłówka.  Jednak wyzwalaczy zaleca się `location` nagłówek zawiera parametr kwerendy `triggerState`.  To jest kilka identyfikator dla swojego interfejsu API wiedzieć podczas ostatniego aplikacji logika uruchamiane.  Jeśli danych jest dostępna, zwraca wyzwalacz `200 OK` odpowiedź ładunek zawartości.  Spowoduje to uruchomienie aplikacji logicznych.

Na przykład jeśli I została sondowania, aby sprawdzić, czy plik był dostępny można utworzyć wyzwalacz ankieta, który chcesz wykonaj następujące czynności:

* Jeśli otrzymano żądanie z triggerState nie zwróci API `202 ACCEPTED` z `location` nagłówek, który ma triggerState bieżącą godzinę i `retry-after` 15.
* Jeśli otrzymano żądanie z triggerState:
 * Sprawdź, jeśli wszystkie pliki zostały dodane po triggerState daty i godziny. 
  * W przypadku 1 plik zwrócić `200 OK` odpowiedź zawartości ładunek zwiększyć triggerState do daty/godziny pliku zwracane i ustawić `retry-after` 15.
  * W przypadku wielu plików mogę zwrócić 1 w czasie z `200 OK`, Zwiększ Moje triggerState w `location` nagłówek i ustaw `retry-after` 0.  Umożliwi aparat o dostępnych jest więcej danych i natychmiast będzie żądać go w `location` nagłówek określony.
  * Jeśli nie ma żadnych plików, zwracają `202 ACCEPTED` odpowiedź i pozostaw `location` triggerState takie same.  Ustawianie `retry-after` 15.

Zobaczysz próbki wyzwalacza ankieta w GitHub [tutaj](https://github.com/jeffhollan/LogicAppTriggersExample/tree/master/LogicAppTriggers)

### <a name="webhook-triggers"></a>Webhook wyzwalaczy

Wyzwalacze Webhook działa podobnie jak akcje Webhook powyżej.  Aparat logiki aplikacji nawiąże połączenie punkt końcowy "subskrypcji", gdy wyzwalacz webhook zostanie dodany i zapisane.  Z interfejsu API można zarejestrować adresu URL webhook i połączeń za pośrednictwem protokołu HTTP WPIS, gdy dane są dostępne.  Ładunek zawartości i nagłówki zostaną przekazane do uruchamiania aplikacji logicznych.

Kiedykolwiek usunięcie wyzwalacza webhook (aplikacja logiczny całkowicie lub po prostu wyzwalacza webhook), aparat zostaną nawiązywanie połączenia z adresem URL "Anuluj subskrypcję" miejsce, w którym można unregister z interfejsu API zwrotnego adresu URL i Zatrzymaj wszystkie procesy, stosownie do potrzeb.

Logika projektanta aplikacji nie obsługuje obecnie odkrywanie wyzwalacza webhook za pośrednictwem swagger, tak aby użyć tego typu akcji należy dodać wyzwalacza "Webhook" i określ adres URL, nagłówki i treści żądania.  Możesz użyć `@listCallbackUrl()` funkcji przepływu pracy w dowolnym z tych pól, aby przekazać w adresie URL zwrotnego.

Zobaczysz próbki wyzwalacza webhook w GitHub [tutaj](https://github.com/jeffhollan/LogicAppTriggersExample/tree/master/LogicAppTriggers)