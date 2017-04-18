<properties 
   pageTitle="Interfejs użytkownika Azure zaangażowania urządzeń przenośnych — dostęp do zawartości" 
   description="Dowiedz się, jak zarządzać unikatową zawartość różnych rodzajów kampanii powiadomień wypychanych w zaangażowania Mobile Azure" 
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

# <a name="how-to-manage-the-unique-content-of-the-different-types-of-push-notification-campaigns"></a>Jak zarządzać unikatową zawartość różnych typów kampanii powiadomień wypychanych
 
Sekcja zawartość nowej kampanii reach umożliwia modyfikować zawartość do ogłoszenia, ankiety, umieszcza danych i Kafelki (tylko dla Windows Phone). Ustawienie zawartości kampanii wypychanych jest zależna od typu kampanii. 
 
### <a name="content-types"></a>Typy zawartości:
- Ogłoszenia
- Ankiety
- Umieszcza danych
- Kafelki (tylko dla Windows Phone)
 
## <a name="content-of-announcements"></a>Zawartość anonsów
 ![Dostęp do Content1][30] 

### <a name="choose-the-type-of-your-announcement"></a>Wybierz typ usługi ogłoszenia:
-    Powiadomienie o tylko: jest proste powiadomienie standardowe. Oznacza to, że jeśli użytkownik kliknie je, nie dodatkowych widok będzie wyświetlany, ale wystąpi akcji skojarzonych z nim.
-    Zawiadomienie o tekstu: to powiadomienie, które uczestniczy użytkownik miał spojrzenie na widok tekstu.
-    Zawiadomienie o sieci Web: to powiadomienie, które uczestniczy użytkownik miał przyjrzeć się widoku sieci web.

### <a name="see-also"></a>Zobacz też
- [Osiągnięcia — jak procedur - ogłoszenia][Link 3] 

### <a name="about-web-view-announcements"></a>Informacje o ogłoszenia widoku sieci Web:
Wystąpienia wzór "{identyfikator urządzenia}" kodu HTML lub kodu JavaScript podane tutaj zostaną automatycznie zastąpione identyfikator urządzenia wyświetlanie powiadomienia. Jest to łatwy sposób pobierania zaangażowania Mobile Azure identyfikatory urządzenia w usłudze zewnętrznej strony sieci web hostowana w biurze Wstecz.
Jeśli chcesz utworzyć widok pełnoekranowy sieci web (bez domyślnej akcji i wyjście przycisków udostępniamy) można używać następujących funkcji z kodu JavaScript anons widoku sieci web: 

-    wykonywanie akcji zawiadomienie o: ReachContent.actionContent()
-    Kończenie z anonsów: ReachContent.exitContent()
 
### <a name="choose-your-action"></a>Wybieranie czynności:

### <a name="about-action-urls"></a>Informacje o adresy URL akcji:
Dowolny adres URL, który może być interpretowany przez system operacyjny urządzenia docelowego może służyć jako adres URL akcji.
Dowolny dedykowany adres URL, który może obsługiwać aplikacji (np. Aby pokazać użytkownikom przechodzenie do określonego ekranu) można także jako adres URL akcji.
Każde wystąpienie deseniu {identyfikator urządzenia} automatycznie zastępuje identyfikator urządzenia wykonywania czynności. To można łatwo pobrać identyfikatorów urządzenia zaangażowania Mobile Azure za pośrednictwem usługi zewnętrzne sieci web hostowana w biurze Wstecz.

- **Android + iOS akcje**
    - Otwórz stronę sieci web
    - http://\[domeny w przypadku witryny sieci web\] 
    - Przykład: http://www.azure.com
    - Wysyłanie wiadomości e-mail
    - mailto:\[e-mail poczty e-mail adresata\]?subject =\[temat\]& treści =\[wiadomości\] 
    - Example:mailto:foo@example.com?subject=Greetings%20from%20Azure%20Mobile%20Engagement!&body=Good%20stuff!
    - Wyślij wiadomość SMS
    - wiadomości SMS:\[numer telefonu\] 
    - Przykład: sms:2125551212
    - Wybierz numer telefonu
    - Tel:\[numer telefonu\] 
    - Przykład: tel:2125551212
- **Android czynności**
    - Pobierz aplikację na sklepu Play
    - Market://details?id=\[pakietu aplikacji\] 
    - Przykład: market://details?id=com.microsoft.office.word
    - Rozpocznij wyszukiwanie zlokalizowane geo
    - Geo:0, 0?q =\[kwerendy wyszukiwania\] 
    - Przykład: geo:0, 0? q = starbucks, Paryż
- **tylko akcje iOS**
    - Pobieranie aplikacji w sklepie App Store
    - /id /app/ [Nazwa aplikacji] http://iTunes.Apple.com/ [country] [identyfikator aplikacji]? mt = 8 
    - Przykład: http://itunes.apple.com/fr/app/briquet-virtuel/id430154748?mt=8
    - Akcje systemu Windows
    - Otwórz stronę sieci web
    - http://\[domeny w przypadku witryny sieci web\] 
    - Przykład: http://www.azure.com
    - Wysyłanie wiadomości e-mail
    - mailto:\[e-mail poczty e-mail adresata\]?subject =\[temat\]& treści =\[wiadomości\] 
    - Example:mailto:foo@example.com?subject=Greetings%20from%20Azure%20Mobile%20Engagement!&body=Good%20stuff!
    - Wysyłanie wiadomości SMS za (aplikacja ze sklepu Skype wymagane)
    - wiadomości SMS:\[numer telefonu\] 
    - Przykład: sms:2125551212
    - Wybierz numer telefonu (aplikacja ze sklepu Skype wymagane)
    - Tel:\[numer telefonu\] 
    - Przykład: tel:2125551212
    - Pobierz aplikację na sklepu Play
    - MS-systemu windows — ze Sklepu: PDP?PFN =\[identyfikator pakietu aplikacji\] 
    - Przykład: ms-systemu windows — ze Sklepu: strony szczegółów projektu? PFN = 4d91298a-07cb-40fb-aecc-4cb5615d53c1
    - Rozpocznij wyszukiwanie bingmaps
    - bingmaps:?q =\[kwerendy wyszukiwania\] 
    - Przykład: bingmaps:? q = starbucks, Paryż
    - Używanie niestandardowego schematu
    - \[niestandardowy schemat\]://\[parametry niestandardowego schematu\] 
    - Przykład: myCustomProtocol://myCustomParams
    - Używanie danych pakietu (aplikacja ze sklepu dla numeru wewnętrznego, przeczytaj wymagane)
    - \[folder\]\[danych\]. \[rozszerzenia\] 
    - Example:myfolderdata.txt
 
### <a name="build-a-tracking-url"></a>Tworzenie adresu URL śledzenia:
-    Zobacz sekcję "Ustawienia" <UI Documentation> Aby uzyskać instrukcje dotyczące tworzenia śledzenia adresu URL, który umożliwia użytkownikom pobrać jeden z innych aplikacji.
 
### <a name="define-the-texts-of-your-announcement"></a>Definiowanie teksty cały anons
Wprowadź tytuł, zawartość i przycisk teksty cały anons. Można kierować odbiorców kampanii przyszłą na podstawie opinii reach: jak użytkownicy odpowiedzi kampanii. Określania docelowych odbiorców mogą być oparte na opinii: czy kampanii został właśnie przeniesiony, odpowiedzi, actioned lub zakończone.

### <a name="see-also"></a>Zobacz też
- [Nowe kryterium wypychanych dokumentacji - Reach - interfejs użytkownika][Link 28]

## <a name="content-of-polls"></a>Zawartość ankiety
![Dostęp do Content2][31] wpisz tytuł, opis i przycisk teksty cały anons. Następnie Dodaj pytania i możliwości odpowiedzi na pytania.
Można kierować odbiorców kampanii przyszłą na podstawie opinii reach: jak użytkownicy odpowiedzi kampanii. Określania docelowych odbiorców mogą być oparte na czy kampanii został właśnie przeniesiony, odpowiedzi, actioned lub zakończone. Określania docelowych odbiorców mogą być również oparte na opinię odpowiedzi ankiety, gdzie pytania i odpowiedzi wybór są używane jako kryteriów.

### <a name="see-also"></a>Zobacz też
- [Nowe kryterium wypychanych dokumentacji - Reach - interfejs użytkownika][Link 28]
 
## <a name="content-of-data-pushes"></a>Umieszcza zawartość danych
![Dostęp do Content3][32] 

### <a name="choose-the-type-of-your-data"></a>Wybierz typ danych:
- Tekst
- Dane binarne
- Dane Base64

### <a name="define-the-content-of-your-data"></a>Definiowanie zawartości danych
- Jeśli wybrano push danych tekstowych, skopiuj i wklej tekst w polu "zawartości".
- W przypadku wybrania danych binarnych lub base64, użyj przycisku "Przekaż plik" Aby przekazać plik.
- Można kierować odbiorców kampanii przyszłą na podstawie opinii reach: jak użytkownicy odpowiedzi kampanii. Określania docelowych odbiorców mogą być oparte na czy kampanii został właśnie przeniesiony, odpowiedzi, actioned lub zakończone.

### <a name="see-also"></a>Zobacz też
- [Nowe kryterium wypychanych dokumentacji - Reach - interfejs użytkownika][Link 28]

## <a name="content-of-tiles-windows-phone-only"></a>Zawartość kafelków (tylko dla Windows Phone)
![Dostęp do Content4][33]

### <a name="define-the-content-of-your-tile"></a>Definiowanie zawartości kafelka
Ładunek kafelków jest tekst, który ma być wyświetlana w kafelka aplikacji na urządzeniach Windows Phone.
Wypychanych kafelków jest wersja Microsoft wypychanych powiadomień usługi (MPNS) natywnych wypychanych dla Windows Phone. Typ kafelka wypychanych jest to jedyny typ wypychanych, który nie ma odpowiedzi, a więc odbiorców kampanii przyszłych nie można utworzyć na stronie wyników kampanii wypychanych kafelków. 

### <a name="see-also"></a>Zobacz też
- [Interfejs API dokumentacji - Reach API — natywne Push][Link 4]

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
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md
 
