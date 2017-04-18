<properties
    pageTitle="Koncentratory Azure powiadomienie — często zadawane pytania"
    description="Często zadawane pytania dotyczące projektowania i implementowania rozwiązań koncentratory powiadomień"
    services="notification-hubs"
    documentationCenter="mobile"
    authors="ysxu"
    manager="erikre"
    keywords="powiadomienia wypychane, powiadomienia wypychane, powiadomienia wypychane iOS, powiadomienia wypychane android, wypychanych ios, android wypychanych"
    editor="" />

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="yuaxu" />

#<a name="push-notifications-with-azure-notification-hubs---frequently-asked-questions"></a>Powiadomienia push powiadomienia z koncentratorów Azure powiadomienie — często zadawane pytania

##<a name="general"></a>Ogólne
###<a name="1---what-is-the-price-model-for-notification-hubs"></a>1. co to jest model ceny dla koncentratorów powiadomienia?
Powiadomienie o koncentratory jest oferowane w trzech:

* **Wolny** — uzyskiwanie umieszcza 1 milion na subskrypcję miesięcznie.
* **Podstawowe** — uzyskiwanie 10 milionów umieszcza na subskrypcję miesięcznie planu bazowego, przy użyciu opcji wzrostu przydziału.
* **Standardowe** — się umieszcza 10 milionów na subskrypcję miesięcznie planu bazowego, z przydziału Zwiększ opcje, a także capabilties telemetrycznego sformatowany.

Najnowsze szczegóły można znaleźć na stronie [Ceny koncentratory powiadomienie] . Ceny nawiązano na poziomie subskrypcji i jest oparty na liczbę inicjacjami powiadomienia wypychane, więc nie ma znaczenia, ile koncentratorów nazw lub powiadomienie o został utworzony w subskrypcji usługi Azure.

**Wolny** warstwa jest oferowane w celu rozwoju z gwarantowana Umowa dotycząca poziomu usług. Ten poziom może być dobry punkt wyjścia dla osób, które mają być Poznawanie możliwości powiadomienia wypychane za pośrednictwem koncentratory powiadomienie Azure, może nie być to najlepszy wybór dla średnich i dużych aplikacji.

**Podstawowe** & **Standardowy** poziomów dostępnych zastosowania produkcji z następujące cechy włączona *tylko standardu poziomu*:

- *Sformatowany telemetrycznego* - oferty koncentratory powiadomienie o wiele możliwości eksportowania danych telemetrycznych, a także wypychanych powiadomień informacji o rejestracji do wyświetlania w trybie offline i analizy.
- *Wiele dzierżawy* — idealne rozwiązanie w przypadku tworzenia aplikacji dla urządzeń przenośnych za pomocą powiadomień koncentratory do obsługi wielu dzierżaw. Umożliwia ustawianie poświadczeń usługi wypychanych powiadomień (PNS) na poziomie nazw powiadomień Centrum dla aplikacji, a następnie można oddzielnie różnic dzierżaw dostarczanie im poszczególnych koncentratorów w tym typowych nazw. Włącza ułatwienia konserwacji, gdy zachowanie klawiszy skojarzenia zabezpieczeń do wysyłanie i odbieranie powiadomień wypychanych z koncentratorów powiadomienie rozdzielone dla każdego dzierżawy zapewnienie pokrywające się nie dzierżawy krzyżowe.
- *Zaplanowane wypychanych* — umożliwia planowanie powiadomienia wypychane, które zostaną następnie umieszczone w kolejce i wysyłany.
- *Importowanie zbiorcze* — umożliwia importowanie rejestracji zbiorczo.

###<a name="2---what-is-the-notification-hubs-sla"></a>2. co to jest SLA koncentratory powiadomienia?
Dla poziomów **podstawowe** i **Standardowy** koncentratory powiadomienie możemy zagwarantować, że co najmniej 99,9% czasu, poprawnie skonfigurowanego aplikacje będą będzie możliwość wysyłania powiadomień wypychanych lub wykonywania operacji zarządzania rejestracji w odniesieniu do koncentratora powiadomienie wdrożony w warstwie obsługiwane. Aby dowiedzieć się więcej na temat naszych Umowa dotycząca poziomu usług, odwiedź strony [SLA koncentratory powiadomienie] .

> [AZURE.NOTE] Istnieje żadnych gwarancji Umowa dotycząca poziomu usług dla nóg między usługą powiadomienie platformy i urządzenia, ponieważ koncentratory powiadomienie zależą od dostawców zewnętrznych platformy do przeprowadzania powiadomień wypychanych na urządzeniu.

###<a name="3---which-customers-are-using-notification-hubs"></a>3. którzy używają koncentratory powiadomienia?
Udostępniamy dużej liczby klientów koncentratory powiadomień za pomocą kilku godne uwagi z nich poniżej:

* Sochi 2014 — 100s odsetek grupy, 3 + milionów urządzeń, 150 + milionów powiadomienie wysyłane w 2 tygodni. [CaseStudy - Sochi]
* Skanska - [CaseStudy - Skanska]
* Czas Seattle - [CaseStudy - razy Seattle]
* Mural.LY - [CaseStudy - Mural.ly]
* 7Digital - [CaseStudy - 7Digital]
* Aplikacje usługi Bing — 10s milionów urządzeń, wysyłanie 3 miliony powiadomienia/dzień.

###<a name="4-how-do-i-upgrade-or-downgrade-my-notification-hubs-to-change-my-service-tier"></a>4. jak uaktualnić lub starszą wersję Moje koncentratory powiadomienie zmienić moje warstwa usług?
Przejdź do [Klasycznej Portal Azure], kliknij Bus usługi, a następnie kliknij na danym obszarze nazw następnie Twoim Centrum powiadomienie. Na karcie Skala można zmienić z poziomu usługi koncentratory powiadomienie.

![](./media/notification-hubs-faq/notification-hubs-classic-portal-scale.png)

##<a name="design--development"></a>Projektowanie i rozwój
###<a name="1---which-server-side-platforms-do-you-support"></a>1. które platformy po stronie serwera obsługuje?
Firma Microsoft udostępnia SDK oraz [pełną próbki] dla .NET, języka Java, PHP, Python, Node.js tak, aby wewnętrznej bazy danych aplikacji można skonfigurować do przekazywania koncentratory powiadomień przy użyciu jednej z tych platform. Interfejsy API koncentratory powiadomień są oparte na pozostałych interfejsów pozwala wybrać się bezpośrednio do tych, zamiast tego Jeśli nie chcesz dodać dodatkowe zależności. Więcej informacji można znaleźć na stronie [NH - interfejsy API pozostałych] .

###<a name="2---which-client-platforms-do-you-support"></a>2. które klienta platformy obsługiwane?
Obsługujemy wysyłania powiadomień wypychanych [systemem Apple iOS](notification-hubs-ios-apple-push-notification-apns-get-started.md), [Android](notification-hubs-android-push-notification-google-gcm-get-started.md), [Uniwersalny systemu Windows](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md), [Windows Phone](notification-hubs-windows-mobile-push-notifications-mpns.md), [Kindle](notification-hubs-kindle-amazon-adm-push-notification.md), [Android Chin (przez Baidu)](notification-hubs-baidu-china-android-notifications-get-started.md), Xamarin ([iOS](xamarin-notification-hubs-ios-push-notification-apns-get-started.md) & [Android](xamarin-notification-hubs-push-notifications-android-gcm.md)), [Safari](https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSafari) i [Chrome aplikacji](notification-hubs-chrome-push-notifications-get-started.md) platformy. Aby uzyskać pełną listę Rozpoczynanie pracy — samouczki działania wysyłania powiadomień wypychanych na tych platformach odwiedź stronę [NH — wprowadzenie samouczki dotyczące] .

###<a name="3---do-you-support-smsemailweb-notifications"></a>3. obsługuje powiadomienia SMS i E-mail i sieci web?
Wysyłanie powiadomienia do aplikacji dla urządzeń przenośnych za pomocą platformy wymienione powyżej przede wszystkim zaprojektowano koncentratory powiadomienie. Firma Microsoft nie jeszcze udostępnia możliwość wysyłania wiadomości e-mail lub alerty SMS; jednak platformy innej firmy, które zapewniają tych funkcji można zintegrować wraz z koncentratorów powiadomienie wysyłania powiadomień wypychanych natywnej przy użyciu [Aplikacji Mobile Azure].

Powiadomienie o koncentratory nie zawierają również wypychanych w przeglądarce powiadomienie o dostarczeniu usługi z gotowych do. Klienci mogą wybrać do wykonania tych przy użyciu SignalR u góry platformy obsługiwane po stronie serwera. Jeśli szukasz Wysyłanie powiadomienia do aplikacji przeglądarki w trybie piaskownicy Chrome, zapoznaj się z [aplikacji Chrome samouczka].

###<a name="4---what-is-the-relation-between-azure-mobile-apps-and-azure-notification-hubs-and-when-do-i-use-what"></a>4. co to jest stosunek aplikacji Mobile Azure i koncentratory powiadomienie Azure i kiedy co używać?
Jeśli masz istniejące wewnętrznej bazy danych aplikacji dla urządzeń przenośnych i chcesz dodać możliwość wysyłania powiadomień wypychanych można użyć koncentratory powiadomienie Azure. Jeśli chcesz skonfigurować do wewnętrznej bazy danych aplikacji dla urządzeń przenośnych od podstaw następnie należy rozważyć przy użyciu aplikacji Mobile Azure. Aplikację Mobile Azure automatycznie inicjuje Centrum powiadomienie o możliwość wysyłania powiadomień wypychanych łatwo z wewnętrznej bazy danych aplikacji dla urządzeń przenośnych. Cennik dla aplikacji Mobile Azure zawiera podstawowej opłaty koncentratora powiadomienie i płacisz tylko po przejściu poza umieszcza uwzględniane. Więcej informacji na temat kosztów są dostępne na stronie [Aplikacji usługi ceny] .

###<a name="5---how-many-devices-can-i-support-if-i-send-push-notifications-via-notification-hubs"></a>5. jak wielu urządzeń strony można obsługiwać czy w przypadku wysłania powiadomienia wypychane za pośrednictwem koncentratory powiadomienia?
Zajrzyj do strony [Ceny koncentratory powiadomienie] szczegółowe informacje na temat liczby obsługiwanych urządzeń.

Dla niektórych scenariuszy, jeśli potrzebujesz pomocy technicznej dla więcej niż 10 000 000 zarejestrowanych urządzeń, należy bezpośrednio [skontaktować się z nami](https://azure.microsoft.com/overview/contact-us/) i my pomoże Ci skalowanie rozwiązania.

###<a name="6---how-many-push-notifications-can-i-send-out"></a>6. liczby powiadomienia push można wysłać?
W zależności od wybranej warstwie Azure będzie automatycznie rozbudowy zależności od liczby powiadomienia ułożony w systemie.

>[AZURE.NOTE] Koszt użycia ogólnego przejść w górę podstawie liczby powiadomienia wypychane obsługiwanej. Upewnij się, są właściwie istniejących limitów warstwa opisane na stronie [Ceny koncentratory powiadomienie] .

Istniejących klientów używają koncentratory powiadomień do wysyłania powiadomień wypychanych miliony codziennie. Nie musisz wykonywać żadnych specjalnych przeskalować do wypychania osiągnięcia powiadomienia o ile korzystasz z koncentratorów powiadomienie Azure.

###<a name="7---how-long-does-it-take-for-sent-push-notifications-to-reach-my-device"></a>7. jak długo trwa dla wysyłane wypychanych powiadomień do osiągnięcia Moje urządzenie?
Azure koncentratory powiadomienia jest w stanie procesu co najmniej **1 mln powiadomień wypychanych wysyła minuty** w zwykłym za pomocą scenariusza, gdzie przychodzących Załaduj zasadzie spójne i nie jest spikey charakter. Ten kurs mogą się różnić w zależności od liczby znaczników, rodzaj wysyła przychodzących i inne czynniki zewnętrzne.

W czasie szacowany czas dostawy: usługa jest w stanie obliczyć elementów docelowych na platformy i rozsyłania wiadomości usługi dostarczania powiadomień wypychanych odpowiednich oparte na wyrażeniach zarejestrowanych znaczniki/znacznik. W tym miejscu na spoczywa usług powiadomienia Push (PNS), aby wysłać powiadomienie na urządzeniu.

Obwodowym układzie Nerwowym nie daje gwarancji dowolnego SLA dostarczania powiadomienia; jednak zwykle większość powiadomienia wypychane są dostarczane do urządzenia docelowe w ciągu kilku minut (zazwyczaj w granicach 10 minut) od momentu, gdy są one są wysyłane do naszej platformy. Może istnieć kilka wartości odstających, które może trwać dłużej.

>[AZURE.NOTE] Azure koncentratory powiadomienie ma zasady w miejscu, aby usunąć wszelkie powiadomienia wypychane, których nie można było jej dostarczyć do obwodowym układzie Nerwowym w 30 minut. To opóźnienie może się zdarzyć z kilku powodów, aby uzyskać najbardziej często ponieważ obwodowym układzie Nerwowym jest ograniczania aplikacji.

###<a name="8---is-there-any-latency-guarantee"></a>8. występuje dowolny gwarancji opóźnienie?
Ze względu na charakter powiadomień wypychanych (są dostarczane przez zewnętrznych, specyficzne dla platformy Usługa powiadomień Push) nie jest gwarantowana opóźnienie. Zazwyczaj większość powiadomienia wypychane dostarczenie w ciągu kilku minut.

###<a name="9---what-are-the-considerations-i-need-to-take-into-account-when-designing-a-solution-with-namespaces-and-notification-hubs"></a>9. co to są zagadnienia, które należy wziąć pod uwagę podczas projektowania rozwiązanie z nazw i koncentratory powiadomienie?

####<a name="mobile-appenvironment"></a>Urządzeń przenośnych aplikacji i środowiska

* Należy jednym centrum powiadomień dla aplikacji dla urządzeń przenośnych, dla środowiska.
* Scenariusz z wieloma dzierżawy każdego dzierżawy powinien mieć osobnym Centrum.
* Nigdy nie muszą mieć tego samego koncentratora powiadomienie między środowiskami badań i produkcji, jak to może powodować problemy wzdłuż linii podczas wysyłania powiadomień. np. Apple oferuje piaskownicy i Push produkcji punkty końcowe z każdym o osobnych poświadczeń.
* Domyślnie możesz wysłać powiadomienia test z urządzeniami zarejestrowanych przez Azure Portal lub Azure zintegrowanym składnikiem w programie Visual Studio. Próg wynosi 10 urządzeń losowo wybranych z puli rejestracji.

>[AZURE.NOTE] Jeśli pierwotnie skonfigurowany certyfikat piaskownicy Apple koncentratora, a następnie ponownie skonfigurowany do używania certyfikatu produkcji firmy Apple, stare tokeny urządzenia czy stają się nieprawidłowe przy użyciu nowego certyfikatu i powodować umieszcza się niepowodzeniem. Najlepiej oddzielić produkcji i przetestuj środowiskach i używanie koncentratory różne dla różnych środowiskach.

####<a name="pns-credentials"></a>Poświadczenia obwodowym układzie Nerwowym

Podczas rejestrowania aplikacji dla urządzeń przenośnych z platformy dzięki portalowi deweloperów (na przykład Apple lub Google itp) następnie możesz uzyskać aplikacji tokeny identyfikator i zabezpieczenia, które należy przekazywać usług powiadomień wypychanych platformy, aby można było wysyłać powiadomienia wypychane do urządzeń wewnętrznej bazy danych aplikacji. Tych tokenów zabezpieczających, które mogą być w postaci certyfikaty (np Apple iOS lub Windows Phone) lub klucze zabezpieczeń (Google Android itp systemu Windows) muszą być skonfigurowane w koncentratory powiadomienie. Odbywa się zwykle na poziomie Centrum powiadomienie, ale można również wykonać na poziomie nazw scenariusz z wieloma dzierżawy.

####<a name="namespaces"></a>Przestrzenie nazw

Przestrzenie nazw może służyć do grupowania wdrożenia.  Również można go symbolizującą wszystkich koncentratorach powiadomień dla wszystkich dzierżaw samej aplikacji w scenariuszu wielu dzierżawy.

####<a name="geo-distribution"></a>Rozkład Geo

Rozkład Geo nie jest zawsze krytyczne w scenariuszach powiadomień wypychanych. Należy zauważyć, czy różne wypychanych powiadomień usługi (APN, GCM itp), które ostatecznie dostarczanie powiadomień wypychanych urządzenia, nie są równomiernie distributed.

Jeśli masz aplikację, która jest używana w dowolnym miejscu na świecie, możesz utworzyć kilka koncentratorów w różnych obszarach nazw skorzystać dostępność usługi koncentratory powiadomienie o różnych regionów Azure na całym świecie.

>[AZURE.NOTE] Zwiększa koszt zarządzania — zwłaszcza wokół rejestracji, aby to naprawdę nie jest zalecane i należy wykonać tylko jeśli jest jawnie wymagane.

###<a name="10--should-i-do-registrations-from-the-app-backend-or-directly-through-client-devices"></a>10. zrobić rejestracji z wewnętrznej bazy danych aplikacji lub bezpośrednio za pomocą klienta urządzenia?
Rejestracji z wewnętrznej bazy danych aplikacji są przydatne, gdy trzeba zrobić uwierzytelnianie klienta na podstawie przed utworzeniem rejestracji lub w przypadku gdy dostępne znaczniki, które zostały utworzone lub zmodyfikowane przez wewnętrznej bazy danych aplikacji według logikę aplikacji. Aby uzyskać szczegółowe informacje, czy więcej informacji na stronach [wskazówki rejestracji wewnętrznej bazy danych — 2] i [wskazówki dotyczące rejestracji wewnętrznej bazy danych] .

###<a name="11--what-is-the-push-notification-delivery-security-model"></a>11. co to jest model zabezpieczeń dostarczenia powiadomienia push?
Azure koncentratory powiadomień za pomocą [Udostępnionego podpis programu Access (SA)](../storage/storage-dotnet-shared-access-signature-part-1.md)— podstawie model zabezpieczeń. Za pomocą tokeny skojarzeń zabezpieczeń na poziomie głównym nazw lub na poziomie koncentratory powiadomienie szczegółowego. Tych tokenów skojarzeń zabezpieczeń można ustawić przy użyciu reguł autoryzacji różnych np uprawnienia Wyślij wiadomości, odsłuchać uprawnienia powiadomienie itp. Szczegółowe informacje są dostępne w dokumencie [model zabezpieczeń NH] .

###<a name="12--how-should-i-handle-sensitive-payload-in-the-push-notifications"></a>12. jak obsługiwać poufnych ładunek powiadomienia wypychane?
Wszystkie powiadomienia są dostarczane do urządzenia przez platformy wypychanych powiadomień usług (obwodowym układzie Nerwowym). Gdy nadawcy powiadomi koncentratory powiadomienie Azure następnie możemy procesu i przekazać powiadomienie do odpowiednich obwodowym układzie Nerwowym.

Wszystkie połączenia od nadawcy Azure koncentratory powiadomień, a następnie obwodowym układzie Nerwowym za pomocą HTTPS.

>[AZURE.NOTE] Azure koncentratory powiadomienia nie rejestruje ładunku wiadomości w dowolny sposób.

Wysyłanie ładunki poufne jednak zaleca się deseń bezpiecznego Push miejsce, w którym nadawcy zapewnia "ping" powiadomienie o identyfikatorze wiadomości na urządzeniu, bez ładunku poufne i aplikacji na tym urządzeniu odbiera ten ładunek, jest możliwość bezpiecznego interfejsu API bezpośrednio do pobrania treść wiadomości. Przewodnik na temat zaimplementowania deseniem opisanych powyżej jest dostępna na stronie [NH - bezpiecznego Push samouczka] .

##<a name="operations"></a>Operacje
###<a name="1---what-is-the-disaster-recovery-dr-story"></a>1. co to jest opowieści odzyskiwania danych (DR)?
Firma Microsoft udostępnia metadanych odzyskiwanie zapotrzebowania na naszej stronie (nazwa Centrum powiadomienie, parametry połączenia i innych ważnych informacji). Wyzwolenia scenariusz DR danych rejestracji jest **tylko część** infrastruktury koncentratory powiadomienie, które mogą zostać utracone. Należy wdrożyć rozwiązanie do ponownego wypełnienia te dane do nowego odzyskiwania po Centrum.

- *Step1* — tworzenie koncentratora podrzędnego powiadomienie w różnych centrum danych. Można go utworzyć w czasie rzeczywistym w momencie zdarzenia DR lub można utworzyć z get Przejdź. Nie należy duża różnica którą opcję wybrać, ponieważ powiadomień Centrum obsługi administracyjnej jest procesem stosunkowo szybkie kolejności kilka sekund. O jeden od początku będzie zabezpieczyć przed z zdarzenia DR wpływające na ochronę z funkcjami zarządzania, dlatego zdecydowanie zalecane opcji.

- *Kroku2* - wodzianem pomocniczej Centrum powiadomienie o rejestracji z podstawowego Centrum powiadomienie. Spróbuj Obsługa rejestracji na obu koncentratory i próby synchronizowania ich w czasie rzeczywistym podczas rejestracji pochodzą w — zwykle działające nie również ze względu na właściwych tendencji rejestracji tak, aby wygasało na stronie obwodowym układzie Nerwowym nie jest zalecane. Powiadomienie koncentratory wyczyść je jako otrzymane obwodowym układzie Nerwowym opinii na temat rejestracji wygasła lub jest nieprawidłowy.  

Zalecenie jest użycie wewnętrznej bazy danych aplikacji, których:

- Zachowuje określony zestaw rejestracji u końca, dzięki czemu możliwości Wstawianie zbiorczo do koncentratora pomocniczej powiadomienia w przypadku DR

**LUB**

- Pobiera zwykłego zrzut rejestracji z poziomu Centrum podstawowego jako kopii zapasowej, a następnie oznacza zbiorcze Włóż do NH pomocniczą.

>[AZURE.NOTE] Funkcja eksportu/importu rejestracji dostępne w warstwie standardowy opisano w dokumencie [Rejestracji eksportu/importu] .

Jeśli nie masz wewnętrznej bazie danych, następnie podczas uruchamiania aplikacji w dowolnej z urządzenia, wykona nowa rejestracja w Centrum pomocniczej powiadomień, a pomocniczej powiadomień Centrum będzie mieć wszystkich urządzeń aktywne zarejestrowane.

Wadą podwyższonego to, że będzie przedziału czasu, gdy urządzeń, gdzie nie jeszcze otwarte aplikacje nie będziesz otrzymywać powiadomień.

###<a name="2---is-there-any-audit-log-capability"></a>2. występuje dowolny możliwości dziennika inspekcji?
Wszystkie operacje zarządzania koncentratory powiadomienie przejdź do pozycji Dzienniki operacji, które są dostępne w [Klasycznym Portal Azure].

##<a name="monitoring--troubleshooting"></a>Monitorowanie i rozwiązywanie problemów
###<a name="1---what-troubleshooting-capabilities-are-available"></a>1. jakie możliwości rozwiązywania problemów są dostępne?
Azure koncentratory powiadomienie zapewniają kilka funkcji zrobić typowych rozwiązywania problemów, szczególnie w najbardziej typowy scenariusz wokół porzucone powiadomienia. Wyświetlanie informacji o w naszym [NH — Rozwiązywanie problemów z] dokument.

###<a name="2---what-telemetry-features-are-available"></a>2. jakie funkcje telemetrycznego są dostępne?
Wyświetlanie danych telemetrycznych w [Klasycznym Portal Azure]Azure umożliwia koncentratory powiadomienie. Szczegóły dostępne wskaźniki są dostępne na stronie [NH - metryki] .

>[AZURE.NOTE] Powiadomienia pomyślnego tylko oznacza, że powiadomienia wypychane zostały dostarczone do zewnętrznych Usługa powiadomień Push (APN dla firmy Apple, GCM Google itd). Jest obwodowym układzie Nerwowym do przeprowadzania powiadomienie do urządzenia docelowego. Zazwyczaj obwodowym układzie Nerwowym nie są uwidaczniane metryki dostarczania stronom trzecim.  

Firma Microsoft udostępnia także możliwość eksportowania danych telemetrycznych (w **standardowej** warstwa). Zobacz [NH - Przykładowe metryki] , aby uzyskać szczegółowe informacje.

[Portal Azure klasyczny]: https://manage.windowsazure.com
[Powiadomienie o koncentratory ceny]: http://azure.microsoft.com/pricing/details/notification-hubs/
[Powiadomienie o koncentratory Umowa dotycząca poziomu usług]: http://azure.microsoft.com/support/legal/sla/
[CaseStudy - Sochi]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=7942
[CaseStudy - Skanska]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=5847
[CaseStudy - Seattle godzin]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=8354
[CaseStudy - Mural.ly]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=11592
[CaseStudy - 7Digital]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=3684
[NH - interfejsy API REST]: https://msdn.microsoft.com/library/azure/dn530746.aspx
[NH — samouczki z wprowadzeniem]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
[Samouczek aplikacje Chrome]: http://azure.microsoft.com/documentation/articles/notification-hubs-chrome-get-started/
[Mobile Services Pricing]: http://azure.microsoft.com/pricing/details/mobile-services/
[Wskazówki dotyczące rejestracji wewnętrznej bazy danych]: https://msdn.microsoft.com/library/azure/dn743807.aspx
[Wskazówki dotyczące rejestracji wewnętrznej bazy danych - 2]: https://msdn.microsoft.com/library/azure/dn530747.aspx
[Model zabezpieczeń NH]: https://msdn.microsoft.com/library/azure/dn495373.aspx
[NH - samouczek bezpiecznego Push]: http://azure.microsoft.com/documentation/articles/notification-hubs-aspnet-backend-ios-secure-push/
[NH — Rozwiązywanie problemów]: http://azure.microsoft.com/documentation/articles/notification-hubs-diagnosing/
[NH - metryki]: https://msdn.microsoft.com/library/dn458822.aspx
[NH - Przykładowe metryki]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel
[Rejestracji eksportu/importu]: https://msdn.microsoft.com/library/dn790624.aspx
[Azure Portal]: https://portal.azure.com
[Przykłady wykonania]: https://github.com/Azure/azure-notificationhubs-samples
[Azure aplikacji dla urządzeń przenośnych]: https://azure.microsoft.com/en-us/services/app-service/mobile/
[Cennik aplikacji usługi]: https://azure.microsoft.com/en-us/pricing/details/app-service/
