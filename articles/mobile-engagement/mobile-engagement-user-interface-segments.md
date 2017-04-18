<properties 
   pageTitle="Interfejs użytkownika Azure zaangażowania urządzeń przenośnych — segmentów" 
   description="Dowiedz się, jak tworzyć i zarządzać nimi segmentów użytkownikom identyfikować wzorce zastosowania przy użyciu zaangażowania Mobile Azure" 
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

# <a name="how-to-create-and-manage-segments-of-users-to-identify-usage-patterns"></a>Jak tworzyć i zarządzać nimi segmentów użytkownikom określenie wzorów użycia

W tym artykule opisano kartę **segmentów** portalu **Zaangażowania Mobile** . Monitorowanie i zarządzać swoimi aplikacjami urządzeń przenośnych za pomocą portalu **Zaangażowania Mobile** .

W sekcji segmentów interfejsu użytkownika umożliwia pracować nad segmentacji użytkowników na podstawie inaczej i analizy, który można uzyskać z poziomu aplikacji, a może też uzyskiwać dostęp za pośrednictwem interfejsu API segmentów. Segmenty najpierw są obliczane 24 godziny po są tworzone i są one przeliczane co 24 godziny, na podstawie najnowszych informacji analizy. Po segmentu jest obliczana, zostanie wyświetlona wykresu "Day do historii dnia" każdego dnia.


>[AZURE.NOTE] Wiele sekcji portalu **Zaangażowania Mobile** interfejsu użytkownika zawierają przycisk **Pokaż Pomoc** . Naciśnij ten przycisk, aby uzyskać więcej informacji kontekstowych o sekcji.

## <a name="create-segments"></a>Tworzenie segmentów
Możesz utworzyć segment na podstawie kryteriów do 10 w danym okresie w górę do 60 dni w przeszłości z sekcji analizy. Na przykład można utworzyć segment według osób, które mają wyświetlać określone strony lub wyszukiwane określoną zawartością w obrębie aplikacji w ciągu ostatnich 10 dni. Te informacje są dostępne w sekcji analizy. Tak można go utworzyć segment, a następnie skonfiguruj powiadomienia wypychane do tej grupy użytkowników, aby otrzymywać wróci do aplikacji. 
 
> Uwaga: Po segmentu została obliczona, nie można go edytować; można tylko sklonowany (skopiowanych) lub zniszczone (usunięte). Segment można było przeprowadzane w tej samej aplikacji (z tym samym identyfikator), a także można było przeprowadzane do innych aplikacji (z różnych identyfikator aplikacji). 
 
 ![segments1][35] 

## <a name="examples-segments"></a>Przykłady segmentów
 ![segments2][36]

Segmenty umożliwiają segmentu użytkowników końcowych aplikacji.
Segmentacji użytkowników jest ważne strategii marketingowych. Azure zaangażowania Mobile umożliwia pobieranie danych historycznych i tworzenie niestandardowego segmentów. To zaawansowane narzędzie umożliwia uzyskanie informacji o obsłudze klientów w aplikacji. Można łatwo analizowanie segmentów i używać segmentów jako wypychanych cele.
Typowe przypadków użycia jest, że chcesz wysłać wypychanych powiadomień pobudzanie użytkowników końcowych do klasyfikowania aplikacji w sklepie. Zamiast wysyłania powiadomień dla wszystkich użytkowników końcowych, możesz utworzyć segment określające tylko użytkownicy, którzy korzystają z aplikacji codziennie przez ostatni miesiąc i miały doskonałe środowiska użytkownika. Podczas wysyłania powiadomień wypychanych mniej, wysoce docelowej, możesz uzyskać lepszą zwrotu z inwestycji.
 
 ![segments3][37]

### <a name="segments-you-can-create-based-on-the-major-azure-mobile-engagement-elements"></a>Segmenty, które można tworzyć w oparciu o głównych elementach Azure zaangażowania Mobile:
- Zdarzeń: tworzenie segment elementów docelowych jeden zdarzenie określonej aplikacji, która się stało z więcej niż dwa razy w tygodniu. 
- Sesja: tworzenie segment użytkowników, którzy korzystają z aplikacji, ostatni tydzień więcej niż 5 godzin.
- Aktywność: tworzenie segment użytkowników, którzy korzystają z jednej strony lub zawartości więcej lub mniej niż 10 czasu ostatni miesiąc.
- Zadanie: tworzenie segment użytkowników, które zostały wykonane zadania więcej niż dwa razy dziennie.
- Awarie: tworzenie część wszystkich użytkowników, które miały awarię ostatni tydzień więcej niż 10 razy. (Może push tej części Przeprosiny lub nawet dywidendy!)
- Błąd: tworzenie segment użytkowników, które były błąd więcej niż 100 razy ostatnie 3 dni.
- Informacje o aplikacji: tworzenie segmentu, do którego jest przeznaczony dla niestandardowe informacje o aplikacji, które wystąpiły podczas ostatniego 25 dni.
 
 ![segments4][38]

Aby użyć części w optymalny sposób, musisz przeprowadzić niestandardowe integracji zestawu SDK w aplikacji z planem znakowania znaczników "Informacje o aplikacji".
Następnie przejdź do strony głównej interfejsu, wybierz aplikację i kliknij przycisk w sekcji "Segmentów".

1. Zaznacz sekcję "Segmentów".
2. "Nowy segment" kliknij przycisk, aby utworzyć nowy segment.

## <a name="real-life-example-create-a-simple-segment-based-on-session-information"></a>Rzeczywisty przykład życia: Tworzenie segment prostej na podstawie informacji "Sesji"
Utwórz część wszystkich użytkowników końcowych, które korzystają z aplikacji co najmniej 50 razy w ostatnim tygodniu. W tym miejscu znaleźć tylko użytkownicy końcowi, które poświęcony co najmniej 30 sekund w aplikacji sesji. To zostanie wyświetlona wszystkich użytkowników końcowych, którzy mają dodatnie doświadczenia w aplikacji. Następnie części utworzone mogą służyć do wypychanych powiadomień dla tych użytkowników końcowych, poproś ich, aby ocenić aplikacji w sklepie.
 
 ![segments5][39]

1. Nadaj nazwę segmentu, aby można było szybko znaleźć na liście "Części".
2. Kliknij przycisk "Utwórz".
 
 ![segments6][40]

Wybierz pozycję sesji.
 
 ![segments7][41]

1. Wybierz okres "Ostatni tydzień".
2. Kliknij przycisk Dalej.
 
 ![segments8][42]

1. Wybierz Operator, którego dotyczy na liście: =; ≥, ≤.
2. Wprowadź liczbę ma.
3. Wybierz wystąpienie ma. 
4. Kliknij przycisk Dalej.
W tym przykładzie jest ustawiona, więc w ostatnim tygodniu, pasujących użytkowników, których wprowadzone co najmniej 50 sesji.
 
 ![segments9][43]

Dla segmentacji sesji można określić czas trwania sesji jako kryterium.

1. Wybierz Operator z listy.
2. Podaj czas trwania sesji.
3. Kliknij przycisk Dalej.
W tym przykładzie komunikat, że na wszystkich sesji którego zostały podzielone na sekcji wystąpienie, zaznacz użytkowników, których poświęcony dłużej niż 30 sekund sesji.
 
 ![segments10][44]

Nazwa kryterium, aby go pobrać w pełną lejek, a następnie kliknij przycisk Zakończ.
 
 ![segments11][45]

Ukończono konfigurowanie kryterium, zostanie wyświetlona w lejek części.
Ponieważ segment na podstawie analizy danych, segmenty są obliczane raz dziennie.
W tym przykładzie 47,7% sumy użytkowników końcowych dopasowane kryterium. Należy je użytkowników, którzy miały dobry poziom obsługi, a zostanie może zapewnić wyższą klasyfikacji, jeśli push ich powiadomienie z prośbą aby ocenić aplikacji w sklepie.


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
 
