<properties
   pageTitle="Interfejs użytkownika Azure zaangażowania urządzeń przenośnych — analizy"
   description="Dowiedz się, jak do analizowania danych historycznych aplikację za pomocą zaangażowania Mobile Azure"
   services="mobile-engagement"
   documentationCenter=""
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

# <a name="how-to-analyze-historical-data-about-your-application"></a>Jak do analizowania danych historycznych aplikacji

W tym artykule opisano kartę **analizy** portalu **Zaangażowania Mobile** . Monitorowanie i zarządzać swoimi aplikacjami urządzeń przenośnych za pomocą portalu **Zaangażowania Mobile** . Należy zauważyć, że aby rozpocząć korzystanie z portalu należy najpierw utworzyć konto **Azure Mobile zaangażowania** .


Sekcja analizy interfejsu użytkownika zawiera zagregowane informacje na temat aplikacji na podstawie danych historycznych, która jest aktualizowana co 24 godziny. Informacje są wyświetlane na różnych pulpity nawigacyjne składający się z linii i pasek-kołowe, siatki i mapy. Dane można także pobrać jako pliki CSV. Większość tych informacji jest dostępna w czasie rzeczywistym w sekcji Monitor interfejsu użytkownika, a może też uzyskiwać z interfejsu API analizy.

>[AZURE.NOTE] Wiele sekcji portalu **Zaangażowania Mobile** interfejsu użytkownika zawierają przycisk **Pokaż Pomoc** . Naciśnij ten przycisk, aby uzyskać więcej informacji kontekstowych o sekcji.

## <a name="standard-and-custom-analytics"></a>Analizy standardowych i niestandardowych

Azure zaangażowania Mobile udostępnia zestaw podstawowe standardowe analityczny informacji na temat aplikacji, które można przedstawić graficznie — jak integracja aplikacji z zestawu SDK. Azure zaangażowania Mobile również umożliwia uzyskanie dodatkowych analizy niestandardowe informacje, które mają o zachowaniu do użytkowników końcowych. Możesz to zrobić, tworząc plan znacznika "Znaczniki niestandardowe (aplikacja info)", utworzone na podstawie **Ustawienia** tak, aby zaangażowania Mobile Azure może zbierać te dodatkowe dane dla Ciebie.



## <a name="analytics"></a>Analizy
- Pulpit nawigacyjny: Zawiera ogólne informacje o nowych i uaktywnia użytkowników i ich trendów.
- Użytkownicy: Użytkownicy są oznaczane ich identyfikator urządzenia: ten identyfikator jest unikatowy dla każdego urządzenia (jeden nowego użytkownika jest faktycznie jednego nowego urządzenia). Użytkownik jest traktowana jako nowy w określonym interwale czasu, jeśli dana przeprowadził jego pierwszej sesji w tym przedziale czasu. Użytkownik jest traktowane jako przechowywane, jeśli ma on wykonać co najmniej jednej sesji w ciągu ostatnich 7 dni. Aktywni użytkownicy korzystają wprowadzone co najmniej jednej sesji w danym okresie. Można sortować według, co miesiąc, co tydzień, codziennie lub co godzinę okresów. Wszystkie wykresy wyglądają podobnie, lecz pozwalają Filtruj według różne funkcje, takie jak wersję aplikacji, a następnie Sortuj według przedziału czasu. Standardowe informacje zgromadzone przez integrowanie zestawu SDK zawiera następujące: aktywni użytkownicy, nowego użytkownika, liczba sesji, długość każdego sesji, techniczne informacje o kraju, lokalnych, lokalizację, carrier języka, urządzenia, sprzętowego, sieci (Wi-Fi), wersji aplikacji i SDK, używane przez klientów. Te informacje można wyświetlać w czasie rzeczywistym z sekcji monitor.

> Uwaga: Przedziału czasu jest na podstawie daty od użytkowników ustawień urządzenia, dzięki czemu użytkownika, którego telefon ma nieprawidłowe daty można wyświetlane w danym okresie problem.

- Przechowywanie: Użytkownik jest traktowane jako przechowywane w określonym interwale czasu, jeśli dana przeprowadził jego pierwszej sesji w tym przedziale czasu. Do godziny, dni, tygodni lub miesięcy, można zmienić interwałów, w których zliczane są zachowywane użytkowników (i nowych użytkowników). Analizy przechowywania użytkownika jest tworzona na bieżąco stado. Kohorty to zbiór nowych użytkowników wykryte za dany okres (to znaczy zestaw użytkownikom wykonywanie ich pierwszej sesji w tym okresie). Firma Microsoft korzysta z stado 1 dzień, dzień 2, 4-dniowego, 7-dniowego lub 1 miesiąc. Mieć kohorty, każdego dnia 1, 2-dzień, 4-dniowego, 7 dni lub 1 miesiąca i Azure zaangażowania przenośnych oblicza zestaw wszystkich użytkowników, którzy należą do kohorty i są nadal aktywne (to znaczy zestaw użytkowników, którzy wykonać co najmniej jednej sesji w okresie). Wersja kohorty nosi nazwę tego zestawu użytkowników. (Azure zaangażowania Mobile możesz wyświetlić liczbę użytkowników nadal korzystasz z aplikacji, ale tylko sklepu określonej platformy umożliwiają poznanie liczbę użytkowników programu iTunes aplikacji — na przykład GooglePlay, ze Sklepu Windows, odinstalować itp.).
- Sesje: Jeden korzystanie z aplikacji przez użytkownika. Sesje są generowane z sekwencji czynności wykonywane przez użytkowników (działanie jest zwykle skojarzony zastosowania po jednym ekranie aplikacji, ale to może się różnić w zależności od sposobu zestawu SDK został zintegrowany w aplikacji). Użytkownik w danej chwili można wykonać tylko jedno działanie: sesji zaczyna się zaraz po rozpoczęciu jego pierwsze działanie użytkownika i przestaje po jego zakończeniu swojej ostatniego działania. Jeśli użytkownik pozostaje więcej niż kilka sekund bez wykonywania czynność, jego kolejność czynności jest podzielony na dwóch różnych sesji.
- Działania: Nazwy każdego ekranu w aplikacji, a długość użytkowników wydatki na poszczególnych ekranach. Działania są opcja analityczny niestandardowego, odpowiadające znaczniki "informacje o aplikacji", które skonfigurowano własnych aplikacji:
- Ścieżka użytkownika: Pokazano, jak użytkownicy nawigować działania aplikacji (ekranów). Czy Przesuń suwak, aby dostosować poziom szczegółów. Niebieski węzły reprezentują działania aplikacji. Ich rozmiar jest proporcjonalny do czasu poświęconego użytkowników w nim. Węzły białe reprezentują sesji rozpoczęcia i zakończenia. Węzły czerwone reprezentują awarii. Łącza oznaczają przejścia między działania aplikacji (lub działania i awarii). Kliknij łącze, aby wyświetlić etykietka narzędzia, zawierająca więcej informacji na temat danych lub węzeł: czas spędzony w konkretnym ekranie liczba przejścia i procent przejścia z działania źródła do działania miejsca docelowego. (---60%---> B oznacza, że użytkownicy na aktywność A przechodzi do działania B 60% czas.) Wykres można uporządkować, zgodnie z oczekiwaniami wyjaśnienie położenia jest zapisywana za każdym razem, gdy wprowadzisz zmiany. Można wyświetlić lub ukryć awarię, aby rozjaśnić na wykresie.
- Zdarzenia: Określone akcje wykonywane przez użytkownika w aplikacji. Rozkład zdarzeń jest wyświetlany jako liczba zdarzeń dla poszczególnych użytkowników sesji. Zdarzenia reprezentuje błyskawiczne akcji, na przykład kliknięcie przycisku lub odbiór powiadomienia. (Znaczenie zdarzeń zależy od sposobu został zintegrowany zestaw SDK w aplikacji.) Zdarzenia mogą wystąpić podczas sesji lub zadanie lub może być autonomiczne.
- Zadania: Podobne do zdarzenia, z wyjątkiem skoncentrowanie się na długości akcji. Na przykład zadania można powiedzieć informacje techniczne dotyczące jak długo trwa zawartości załadować lub wywołanie usługi sieci web. Można również pokazywać, jak długo trwa użytkownikowi wypełnianie formularzy, Utwórz konto lub dokonać zakupu. Zadanie reprezentuje czasu trwania zadania, na przykład czasu trwania zadania pobierania lub czas transparentu jest wyświetlany na ekranie. (Znaczenie zadań zależy od sposobu został zintegrowany zestaw SDK w aplikacji.) Zadania są zwykle skojarzony zadania w tle, które są wykonywane poza zakres sesji (to znaczy bez aktywności użytkownika).
- Technicals: Informacje techniczne dotyczące urządzeń użytkowników aplikacji można śledzić, takie jak ustawienia regionalne, przewoźnika, sieci, urządzenia, sprzętowego i ekranu rozmiar użytkowników urządzeń i wersji aplikacji i wersji SDK używane w aplikacji.
- Błędy: Informacje techniczne błędy wewnątrz aplikacji, których nie może ulec awarii aplikacji. Komunikat o błędzie oznacza problem błyskawicznych, na przykład awarii sieci lub nieprawidłowe manipulowania. (Znaczenie zdarzeń zależy od sposobu został zintegrowany zestaw SDK w aplikacji.) Komunikat o błędzie może wystąpić podczas sesji lub zadanie lub może być autonomiczne.
- Awarie: Informacje o błędach, które powodować awarię aplikacji. Awarię jest nieoczekiwany warunek miejsce, w którym aplikacja przestanie odpowiadać jego oczekiwanych funkcji i muszą być zatrzymane. Awarię jest zwykle z powodu błędu w aplikacji.

![Analytics2][11]

## <a name="accessing-the-retention-overview"></a>Uzyskiwanie dostępu do przechowywania — omówienie
![Analytics3][12]

Omówienie przechowywania dzieli się na środku do kilku kart, każdy z artykułem przez pewien okres przechowywania. Okres przechowywania 2-dniowego jest widoczny w przykładzie. Inne karty przedstawiają okresów przechowywania 4- i dzień 7.

## <a name="understanding-the-retention-overview-cards"></a>Opis karty omówienie przechowywania
![Analytics4][13]

### <a name="each-card-is-composed-of-3-main-parts"></a>Każda karta składa się z 3 główne części:
1. 1: kohorty i okresu
2. 2 – 4: przechowywania dla bieżącego okresu
3. 5: wykres przebiegu w czasie historii

### <a name="here-is-detailed-information-about-each-element"></a>Poniżej przedstawiono szczegółowe informacje na temat poszczególnych elementów:
1.    Kohorty i okresu: ten nagłówek zawiera typ kohorty. W tym miejscu "2 dni" oznacza, że będzie przyjrzymy się zachowanie użytkowników przekracza 2 dni, użytkowników, które otrzymano w okresie 2 dni i czy działają w blokach następujące 2 dni. W powyższym przykładzie są uwzględnione aktywności użytkowników między 21 i 22 listopada.
2.    Dzięki temu stopa przechowywania na 21 i 22 listopada dla użytkowników w 19 i 20 listopada. W tym miejscu było 1 aktywnym użytkownikiem między 21 i 22, przez 3, które zostały nowych użytkowników między 19th i 20.
3.    Ten wizualnej zawiera te same informacje, co powyżej przedstawione w formie graficznej. (Trzecia koła jest od numeru 33%). Kolor udostępnia dodatkowe informacje: zielony wskazuje, ta liczba rośnie z poprzedniego obliczeń. Żółty oznacza, że stabilny i czerwony oznacza zmniejszenie.
4.    Ta wartość oznacza wartości używane do obliczeń.
5.    To jest wykres przebiegu w czasie historii przechowywania wartości. Umożliwia wyświetlanie wartości w przeszłości mają szeroki obraz jak utworzony na.


## <a name="see-also"></a>Zobacz też

- [Pojęcia][Link 6]
- [Usługa Przewodnik rozwiązywania problemów][Link 24]

<!--Image references-->
[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png
[2]: ./media/mobile-engagement-user-interface-home/home1.png
[3]: ./media/mobile-engagement-user-interface-home/home2.png
[4]: ./media/mobile-engagement-user-interface-home/home3.png
[5]: ./media/mobile-engagement-user-interface-home/home4.png
[6]: ./media/mobile-engagement-user-interface-home/home5.png
[7]: ./media/mobile-engagement-user-interface-my-account/myaccount1.png
[8]: ./media/mobile-engagement-user-interface-my-account/myaccount2.png
[9]: ./media/mobile-engagement-user-interface-my-account/myaccount3.png
[10]: ./media/mobile-engagement-user-interface-analytics/analytics1.png
[11]: ./media/mobile-engagement-user-interface-analytics/analytics2.png
[12]: ./media/mobile-engagement-user-interface-analytics/analytics3.png
[13]: ./media/mobile-engagement-user-interface-analytics/analytics4.png
[14]: ./media/mobile-engagement-user-interface-monitor/monitor1.png
[15]: ./media/mobile-engagement-user-interface-monitor/monitor2.png
[16]: ./media/mobile-engagement-user-interface-monitor/monitor3.png
[17]: ./media/mobile-engagement-user-interface-monitor/monitor4.png
[18]: ./media/mobile-engagement-user-interface-reach/reach1.png
[19]: ./media/mobile-engagement-user-interface-reach/reach2.png
[20]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign1.png
[21]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign2.png
[22]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign3.png
[23]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign4.png
[24]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign5.png
[25]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign6.png
[26]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign7.png
[27]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign8.png
[28]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign9.png
[29]: ./media/mobile-engagement-user-interface-reach-criterion/Reach-Criterion1.png
[30]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content1.png
[31]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content2.png
[32]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content3.png
[33]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content4.png
[34]: ./media/mobile-engagement-user-interface-dashboard/dashboard1.png
[35]: ./media/mobile-engagement-user-interface-segments/segments1.png
[36]: ./media/mobile-engagement-user-interface-segments/segments2.png
[37]: ./media/mobile-engagement-user-interface-segments/segments3.png
[38]: ./media/mobile-engagement-user-interface-segments/segments4.png
[39]: ./media/mobile-engagement-user-interface-segments/segments5.png
[40]: ./media/mobile-engagement-user-interface-segments/segments6.png
[41]: ./media/mobile-engagement-user-interface-segments/segments7.png
[42]: ./media/mobile-engagement-user-interface-segments/segments8.png
[43]: ./media/mobile-engagement-user-interface-segments/segments9.png
[44]: ./media/mobile-engagement-user-interface-segments/segments10.png
[45]: ./media/mobile-engagement-user-interface-segments/segments11.png
[46]: ./media/mobile-engagement-user-interface-settings/settings1.png
[47]: ./media/mobile-engagement-user-interface-settings/settings2.png
[48]: ./media/mobile-engagement-user-interface-settings/settings3.png
[49]: ./media/mobile-engagement-user-interface-settings/settings4.png
[50]: ./media/mobile-engagement-user-interface-settings/settings5.png
[51]: ./media/mobile-engagement-user-interface-settings/settings6.png
[52]: ./media/mobile-engagement-user-interface-settings/settings7.png
[53]: ./media/mobile-engagement-user-interface-settings/settings8.png
[54]: ./media/mobile-engagement-user-interface-settings/settings9.png
[55]: ./media/mobile-engagement-user-interface-settings/settings10.png
[56]: ./media/mobile-engagement-user-interface-settings/settings11.png
[57]: ./media/mobile-engagement-user-interface-settings/settings12.png
[58]: ./media/mobile-engagement-user-interface-settings/settings13.png

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md
