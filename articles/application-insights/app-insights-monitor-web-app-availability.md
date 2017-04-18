<properties
    pageTitle="Monitorowanie dostępności i czas reakcji dowolnej witryny sieci web | Microsoft Azure"
    description="Konfigurowanie badania wniosków aplikacji sieci web. Jeśli witryna sieci Web jest niedostępny lub odpowiada powoli, otrzymywać alerty."
    services="application-insights"
    documentationCenter=""
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/07/2016"
    ms.author="awills"/>

# <a name="monitor-availability-and-responsiveness-of-any-web-site"></a>Monitorowanie dostępności i czas reakcji dowolnej witryny sieci web

Po Twojej aplikacji sieci web lub witrynie sieci web po wdrożeniu do dowolnego serwera, możesz skonfigurować testy sieci web, monitorowanie jego dostępności i czas reakcji. [Visual Studio aplikacji wniosków](app-insights-overview.md) wysyła żądania sieci web do aplikacji w regularnych odstępach z punktów na świecie. Ostrzega użytkownika w przypadku aplikacji nie odpowiedzieć lub odpowiada powoli.

![Przykład test sieci Web](./media/app-insights-monitor-web-app-availability/appinsights-10webtestresult.png)

Możesz skonfigurować testy sieci web dla protokołu HTTP lub HTTPS końcowego, który jest dostępne za pośrednictwem publicznego Internetu.

Istnieją dwa rodzaje testów sieci web:

* [Testowanie adresu URL ping](#create): prosty test, którą można utworzyć w portalu Azure.
* [Testowanie sieci web wielu kroku](#multi-step-web-tests): tworzenie programu Visual Studio Ultimate lub Enterprise programu Visual Studio i przekazać do portalu.

Możesz utworzyć maksymalnie 10 testy sieci web na zasób aplikacji.

## <a name="create"></a>1. Utwórz zasób dla raportów programu test

Pomiń ten krok, jeśli skonfigurowano już [zasób aplikacji wniosków] [ start] dla tej aplikacji i chcesz wyświetlić raporty dostępność w tym samym miejscu.

Utwórz konto [Microsoft](http://azure.com)Azure, przejdź do [portalu Azure](https://portal.azure.com)i utworzyć zasób wniosków aplikacji.

![Nowy > wniosków aplikacji](./media/app-insights-monitor-web-app-availability/11-new-app.png)

Kliknij pozycję **wszystkie zasoby** , aby otworzyć karta Przegląd nowego zasobu.

## <a name="setup"></a>2. Utwórz test ping adresu URL

Zasób aplikacji wniosków poszukaj kafelka dostępności. Kliknij, aby otworzyć karta testy sieci Web dla aplikacji, a następnie dodaj test sieci web.

![Wypełnianie co najmniej adres URL witryny sieci Web](./media/app-insights-monitor-web-app-availability/13-availability.png)

- **Adres URL** musi być widoczna z publicznego Internetu. Tak, na przykład można uruchamiać bazy danych nieco może zawierać ciągu kwerendy & #151; Jeśli adres URL jest rozpoznawana jako przekierowanie, możemy po nim maksymalnie 10 przekierowania.
- **Analizowanie żądania zależne**: obrazy, skrypty, pliki stylów i inne zasoby strony wymagane są częścią badania i czas reakcji zarejestrowanych zawiera tych godzinach. Test zakończy się niepowodzeniem, jeśli te zasoby nie może pomyślnie pobrane w ciągu limitu czasu dla całego badania.
- **Włączanie ponowne próby**: gdy test nie powiedzie się, próba jest ponawiana po krótki odstęp. Błąd jest zgłaszany tylko wtedy, gdy trzech kolejnych próbach kończą się niepowodzeniem. Kolejne testy następnie są wykonywane w częstotliwość badania normalny. Ponów próbę jest tymczasowo zawieszone do następnego sukcesu. Ta reguła jest stosowana niezależnie w każdej lokalizacji test. (Zaleca się to ustawienie. Przeciętnie około 80% błędy znikną Ponów.)
- **Testowanie częstotliwość**: Określa, jak często test jest uruchamiany w każdej lokalizacji test. Z częstotliwością pięć minut i lokalizacji pięć test witryny jest sprawdzany średnio co minutę.
- **Test lokalizacje** to argument miejsca, z którym naszych serwerów wysyłają żądania sieci web do adresu URL. Wybierz pozycję więcej niż jedno, tak, aby w witrynie sieci Web można odróżnić problemów sieciowych. Możesz wybrać maksymalnie 16 lokalizacji.

- **Kryteria sukcesu**:

    **Limit czasu test**: zmniejszyć tę wartość, aby otrzymywać alerty o wolne odpowiedzi. Test jest liczony jako błąd, jeśli odpowiedzi z witryny nie zostały odebrane w ciągu tego okresu. W przypadku wybrania **przeanalizować żądania zależne**, następnie wszystkie obrazy, pliki stylu, skrypty i inne zasoby zależne musi otrzymano w ciągu tego okresu.

    **Odpowiedź HTTP**: kodu stanu zwrócona, który jest liczony jako sukcesu. 200 jest kod, który wskazuje, zwrócenia normalna strona sieci web.

    **Uwzględnij zawartość**: ciągu, takie jak "Zapraszamy!" Testowanie firma Microsoft, że pojawi się w każdej odpowiedzi. Musi to być ciągu zwykły, bez symboli wieloznacznych. Pamiętaj, że jeśli zmiany zawartości strony trzeba je zaktualizować.


- **Alerty** są domyślnie wysłała występują błędy w trzech miejscach niż pięć minut. Błąd w jednym miejscu jest prawdopodobnie problem z siecią, a nie problem z witryny. Możesz zmienić progu się więcej lub mniej ważne a można też zmienić osób, które powinny być wysłane wiadomości e-mail.

    Możesz skonfigurować [webhook](../monitoring-and-diagnostics/insights-webhooks-alerts.md) wywoływana, gdy powstaje alertu. (Ale nie przechodzą należy zauważyć, że obecnie parametry zapytania jako właściwości).

### <a name="test-more-urls"></a>Testowanie więcej adresów URL

Dodawanie kolejnych testów. Na przykład, a także testowanie strony głównej można upewnij się, że baza danych działa, testując adresu URL wyszukiwania.


## <a name="monitor"></a>3. Zobacz sieci web wyniki testu

Po 1-2 minuty wyniki są wyświetlane w karta Test sieci Web.

![Podsumowanie wyników na karta Narzędzia główne](./media/app-insights-monitor-web-app-availability/14-availSummary.png)

Kliknij dowolny słupek na wykresie podsumowania, aby uzyskać bardziej szczegółowe informacje dotyczące tego okresu.

Te wykresy łączenie wyników dla wszystkie testy sieci web tej aplikacji.


## <a name="failures"></a>Jeśli widzisz błędy

Kliknij pozycję czerwona kropka.

![Kliknij pozycję czerwona kropka](./media/app-insights-monitor-web-app-availability/14-availRedDot.png)

Lub, przewiń w dół, a następnie kliknij przycisk test napisem mniej niż 100% sukcesu.

![Kliknij pozycję określone webtest](./media/app-insights-monitor-web-app-availability/15-webTestList.png)

Wyniki otwarte test.

![Kliknij pozycję określone webtest](./media/app-insights-monitor-web-app-availability/16-1test.png)

Test jest uruchamiany z kilku lokalizacjach & #151, wybierz jedną z nich, której wyniki są mniej niż 100%.

![Kliknij pozycję określone webtest](./media/app-insights-monitor-web-app-availability/17-availViewDetails.png)


Przewiń w dół do **nie powiodło się badania** i wybierz wyniku.

Kliknij wynik, aby ocenianie go w portalu i sprawdź, dlaczego nie powiodło się.

![Webtest wynik uruchomienia](./media/app-insights-monitor-web-app-availability/18-availDetails.png)


Można też kliknąć można pobrać plik wyników i sprawdzić w programie Visual Studio.


*Wydaje się przycisk OK, ale zgłoszone jako błąd?* Zaznacz wszystkie obrazy, skryptów, arkusze stylów i innych plików załadowane przez stronę. Dowolnej z nich nie powiedzie się, test jest zgłoszone jako nie powiodła się, nawet w przypadku ładowania strony głównej html OK.



## <a name="multi-step-web-tests"></a>Testy kroku wielu sieci web

Można monitorować scenariusza, który wymaga sekwencji adresów URL. Na przykład jeśli są monitorowania sprzedaży witryny sieci Web, możesz przetestować dodawania elementów do zakupów poprawnie koszyka działa.

Aby utworzyć test wielu krok, zarejestrować tego scenariusza przy użyciu programu Visual Studio, a następnie przekaż nagrania do wniosków aplikacji. Wnioski aplikacji odtwarzaniem tego scenariusza w określonych odstępach i potwierdza odpowiedzi.

Należy zauważyć, że nie można używać kodowane funkcje w testy: czynności scenariusz musi znajdować się jako skrypt w pliku .webtest.

#### <a name="1-record-a-scenario"></a>1. rekord scenariusz

Rejestrowanie sesji sieci web za pomocą programu Visual Studio Enterprise lub Ultimate.

1. Tworzenie projektu badania wydajności sieci web.

    ![W programie Visual Studio Tworzenie projektu na podstawie szablonu wydajności sieci Web i badanie.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-create.png)

2. Otwórz plik .webtest i rozpocząć rejestrowanie.

    ![Otwórz plik .webtest i kliknij przycisk Nagraj.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-start.png)

3. Wykonaj akcje użytkownika ma być zasymulowania w swojej test: otwieranie witryny sieci Web, dodać produkt do koszyka i tak dalej. Następnie Zatrzymaj usługi test.

    ![Sieci web przetestować rejestratora zostanie uruchomiona w programie Internet Explorer.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-record.png)

    Nie wprowadzaj scenariuszu długa. Istnieje ograniczenie kroków 100 i 2 minuty.

4. Edytowanie test, aby:
 - Dodawanie reguły sprawdzania poprawności, aby sprawdzić otrzymanej kody tekstu i odpowiedzi.
 - Usuń wszelkie nadmiar interakcji. Można też usunąć zależne żądania dla obrazów i do ad lub śledzenie witryn.

    Pamiętaj, że można edytować tylko testowy skrypt — nie można dodać kodu niestandardowego lub zadzwoń inne testy sieci web. Nie można wstawić pętli w teście. Możesz użyć wtyczki test standardowych sieci web.

5. Uruchamianie testu w programie Visual Studio, aby upewnić się, że działa on.

    Runner test sieci web zostanie otwarty w przeglądarce sieci web i powtarza akcje, które zostało zarejestrowane. Upewnij się, że działa on zgodnie z oczekiwaniami.

    ![W programie Visual Studio Otwórz plik .webtest, a następnie kliknij pozycję Uruchom.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-run.png)


#### <a name="2-upload-the-web-test-to-application-insights"></a>2. przekazywanie test sieci web do wniosków aplikacji

1. Utwórz nowe badania sieci web w portalu wniosków aplikacji.

    ![Na karta testy sieci web wybierz przycisk Dodaj.](./media/app-insights-monitor-web-app-availability/16-another-test.png)

2. Wybierz przycisk Testuj wielu kroku i przekaż plik .webtest.

    ![Zaznaczanie wielu krok webtest.](./media/app-insights-monitor-web-app-availability/appinsights-71webtestUpload.png)

    Ustawianie lokalizacji test, częstotliwość i parametry alertu w taki sam sposób, jak w przypadku próby ping.

Wyświetlanie wyników test i wszystkie błędy w taki sam sposób, jak w przypadku testów adres url pojedynczego.

Typowe przyczyny błąd jest badanie trwa zbyt długo. Musi uruchomić więcej niż dwie minuty.

Pamiętaj, że wszystkie zasoby strony należy załadować poprawnie w teście pomyślnie tym skryptów, arkusze stylów, obrazów i tak dalej.

Uwaga test sieci web muszą być całkowicie zawarte w pliku .webtest: nie można używać funkcji zakodowany w teście.


### <a name="plugging-time-and-random-numbers-into-your-multi-step-test"></a>Podłączyć do swojego test wielu kroku godziny i liczb losowych

Załóżmy, że testowania narzędzi, który pobiera dane zależne od czasu, takich jak zasobów z zewnętrznego źródła. Po zarejestrowaniu test usługi sieci web, należy użyć określonym czasie, ale Ustaw jako parametrów badania, czas rozpoczęcia i zakończenia.

![Testowanie sieci web z parametrami.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-parameters.png)

Po uruchomieniu test, potem zakończenia zawsze do chwili obecnej i czas rozpoczęcia powinny być 15 minut wcześniej.

Dodatki Test sieci Web umożliwia definiowanie parametrów razy.

1. Dodawanie wtyczki test sieci web dla każdej wartości parametru zmiennych, które mają. Na pasku narzędzi test sieci web wybierz pozycję **Dodaj dodatek Testowanie sieci Web**.

    ![Dodawanie wtyczki Test sieci Web i wybierz przycisk typu.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugins.png)

    W tym przykładzie używamy dwa wystąpienia wtyczka czasu daty. Jedno wystąpienie dotyczy "15 minut wcześniej", a drugi "teraz".

2. Otwórz okno właściwości każdego dodatku. Nadaj plikowi nazwę i ustaw ją na wartość Użyj bieżącej godziny. Dla jednej z nich, ustaw Dodawanie minut = -15.

    ![Ustawianie nazwy, użyj bieżącej godziny i minuty dodać.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugin-parameters.png)

3. W sieci web, przetestuj parametry, odwołanie do wtyczki nazwy za pomocą {{nazwa wtyczki}}.

    ![W parametrze test za pomocą {{nazwa wtyczki}}.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugin-name.png)

Teraz przekazać do badania do portalu. Używa wartości dynamiczne przy każdym uruchomieniu test.

## <a name="dealing-with-sign-in"></a>Sposób postępowania z logowaniem

Jeśli użytkownicy zalogować się do aplikacji, masz różne opcje symulacji logowania, aby mógł testować strony logowania. Podejście, którego używasz, zależy od typu zabezpieczeń za pomocą aplikacji.

We wszystkich przypadkach należy utworzyć konto w aplikacji tylko do celów testowania. Jeśli to możliwe należy ograniczyć uprawnienia to konto testowe, aby istnieje możliwość testów web wpływających na rzeczywisty użytkownik.

### <a name="simple-username-and-password"></a>Prosta nazwy użytkownika i hasła

Rekord test sieci web w zwykły sposób. Najpierw usuń pliki cookie.

### <a name="saml-authentication"></a>Uwierzytelnianie SAML

Za pomocą wtyczki SAML, dostępnej badań sieci web.

### <a name="client-secret"></a>Tajny klienta

Jeśli w aplikacji jest trasa logowania, która wymaga tajny klienta, użyj tej trasy. Azure Active Directory (AAD) jest przykładem usługa, która zapewnia klientem tajne logowania. W AAD tajny klienta jest kluczem aplikacji.

Oto przykładowy test sieci web aplikacji Azure web za pomocą klawisza aplikacji:

![Przykładowe tajny klienta](./media/app-insights-monitor-web-app-availability/110.png)

1. Pobierz token z AAD przy użyciu hasła klienta (AppKey).
2. Wyodrębnianie token okaziciela z odpowiedzią.
3. Nawiązywanie połączenia przy użyciu token okaziciela w nagłówku autoryzacji interfejsu API.

Upewnij się, że testowanie sieci web jest klientem rzeczywisty - oznacza to, że ma własny aplikacji w AAD - i użyj jego clientId + appkey. Usługi badany również ma własny aplikacji w AAD: identyfikator URI ta aplikacja jest widoczny w teście sieci web w polu "zasobu".

### <a name="open-authentication"></a>Otwieranie uwierzytelniania

Przykład otwórz uwierzytelniania jest zalogować się za pomocą konta Microsoft lub Google. Wiele aplikacje, które używają OAuth zapewniają klienta zamiast tajne, do pierwszej osoby powinny wskazywać na badanie tej możliwości.

Jeśli do badania, musisz zalogować się przy użyciu OAuth, ogólne rozwiązaniem jest:

 * Użyj narzędzia, takie jak Fiddler Sprawdź komunikacji między przeglądarki sieci web, witryna uwierzytelniania i aplikacji.
 * Wykonaj co najmniej dwa rejestrowaniu przy użyciu różnych komputerach lub przeglądarek, lub w długich odstępach (umożliwiające tokenów wygasało).
 * Przez porównanie różnych sesji, określ token przekazywane ponownie z witryny uwierzytelniania, co wartość przekazywana następnie do swojego serwera aplikacji po logowania.
 * Rekord test sieci web przy użyciu programu Visual Studio.
 * Definiowanie parametrów tokenów, ustawienie parametru po podłączeniu tokenu z uwierzytelnianiem i korzystania z niego w kwerendzie do witryny.
 (Visual Studio próbuje Definiowanie parametrów test, ale nie poprawnie Definiowanie parametrów tokeny).


## <a name="edit"></a>Edytowanie lub wyłącz test

Otwórz poszczególnych test, aby edytować lub je wyłączyć.

![Edytowanie lub wyłącz test sieci web](./media/app-insights-monitor-web-app-availability/19-availEdit.png)

Może chcesz wyłączyć testy sieci web podczas wykonywania konserwacji na tej usługi.

## <a name="performance-tests"></a>Testy wydajności

Test ładowania może zostać uruchomiony w witrynie sieci Web. Jak badania dostępności możesz wysłać prostych żądań lub wielu kroku żądania z naszych punktów na świecie. W przeciwieństwie do testu dostępność zostaną wysłane wezwania wiele, symulowanie wielu użytkowników jednocześnie.

Korzystając z karta Przegląd Otwórz **Ustawienia** **Testów wydajności**. Po utworzeniu test możesz przystąpić do łączenia lub Utwórz konto programu Visual Studio Team Services.

Po zakończeniu testów są wyświetlane czas reakcji i stawki sukcesu.


## <a name="automation"></a>Automatyzacji

* [Skryptów programu PowerShell Użyj skonfigurować test sieci web](https://azure.microsoft.com/blog/creating-a-web-test-alert-programmatically-with-application-insights/) automatycznie.
* Konfigurowanie [webhook](../monitoring-and-diagnostics/insights-webhooks-alerts.md) wywoływana, gdy powstaje alertu.

## <a name="questions-problems"></a>Masz pytania? Problemy?

* *Można zadzwonić kodu testu mojej sieci web?*

    Wartość nie. Procedura badania musi być w pliku .webtest. I nie połączenia inne testy sieci web lub użyć pętli. Ale istnieje kilka wtyczek, które mogą okazać się pomocne.

* *Protokół HTTPS jest obsługiwana?*

    Obsługujemy TLS 1.1 i TLS 1.2.

* *Czy istnieje różnica między "testy sieci web" i "dostępność testy"?*

    Firma Microsoft korzysta z są używane zamiennie dwóch warunków.

* *Chcę używać sprawdza dostępności na naszym serwerze wewnętrznym, uruchamianego za zaporą.*

    Konfigurowanie zapory umożliwiające żądania od [czynników testowanie adresów IP w sieci Web](app-insights-ip-addresses.md#availability).

* *Test kroku wielu sieci web nie można przekazać*

    Istnieje limit rozmiaru 300 k.

    Pętle nie są obsługiwane.

    Odwołania do innych testów sieci web nie są obsługiwane.

    Źródła danych nie są obsługiwane.


* *Moje test wielu krok nie wykonania*

    Istnieje ograniczenie liczby 100 żądania na test.

    Test jest zablokowany, jeśli jest uruchomione dłużej niż dwie minuty.

* *Jak uruchomić test certyfikaty klienta*

    Firma Microsoft nie obsługuje, bardzo nam przykro.


## <a name="video"></a>Klip wideo

> [AZURE.VIDEO monitoring-availability-with-application-insights]

## <a name="next"></a>Następne kroki

[Dzienniki diagnostyczne wyszukiwania][diagnostic]

[Rozwiązywanie problemów][qna]

[Adresy IP agenci test sieci web](app-insights-ip-addresses.md)


<!--Link references-->

[azure-availability]: ../insights-create-web-tests.md
[diagnostic]: app-insights-diagnostic-search.md
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md
