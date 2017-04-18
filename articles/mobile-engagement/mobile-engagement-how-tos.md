<properties 
   pageTitle="Interfejs użytkownika Azure zaangażowania urządzeń przenośnych — Reach jak"
   description="Omówienie interfejsu użytkownika dla Azure zaangażowania urządzeń przenośnych" 
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

# <a name="how-to-get-started-using-and-managing-pushes-to-reach-out-to-your-end-users"></a>Jak rozpocząć pracę przy użyciu i zarządzanie nim umieszcza się skontaktować dla użytkowników końcowych

Po zestawu SDK jest w pełni zintegrowany z aplikacji, można rozpocząć za pomocą sekcji Reach interfejsu użytkownika umożliwiającego powiadomień wypychanych dla użytkowników aplikacji.  

## <a name="do-your-first-push-notification-campaign"></a>Wykonaj pierwsze kampanii powiadomień wypychanych
-    Upewnij się, że zasięg jest zintegrowany z aplikacji z zestawu SDK. 
-    Wybieranie aplikacji
 
![First1][1]

-    Przejdź do sekcji "Reach" i kliknij pozycję "zawiadomienie o"
 
![First2][2]

-    Utwórz nową kampanię i nadaj mu nazwę
 
 ![First3][3]

-    Wybierz, jak powiadomienie powinna zostać dostarczona, jak w aplikacji tylko
 
![First4][4]

-    Utwórz wiadomość, którą chcesz push
 
![First5][5]

-    Tytuł może tworzyć w powiadomieniu (opcjonalnie).
-    Wpisz treść wiadomości push.
-    Możesz przekazać obraz. Należy pamiętać, że rozmiar pliku nie może przekraczać 32 768 bajtów.
-    Masz również możliwość dalszego wybierz opcje, ale dla zespołu tego samouczka, należy sprawdzić, które później.

-    Wybierz typ zawartości tylko jako powiadomienie
 
![First6][6]

-    Tworzenie kampanii wypychanych i pojawi się na liście kampanii.
 
![First7][7]

## <a name="test-your-push-notification-campaign"></a>Testowanie kampanii powiadomień wypychanych
![Test1][8]

-    Zarejestruj się w urządzeniu.
-    Kliknij pole wyboru urządzenie, którego chcesz przesunąć.
-    Kliknij przycisk "Test", aby wysłać naciśnięcie do urządzenia.
 
![Test2][9]

-    Aktywowanie kampanii
 
![Test3][10]

-    Teraz, gdy użytkownik utworzył kampanii wystarczy aktywowania go dla powiadomienia, aby zostać przeniesiony do użytkowników.
 
## <a name="send-personalized-pushes"></a>Wysyłanie spersonalizowanych umieszcza
-    W tym przykładzie tworzy wypychanych, gdzie kodu niestandardowego rabatu zawarta powiadomień push.
 
![Personalize1][11]

Działa personalizacji, zamieniając znacznik przez z tag informacje o aplikacji tak, konieczne będzie upewnij się, że użytkownik ma odpowiednie aplikacji info definiowana jako pierwsza. W tym przykładzie docelowej użytkownicy będą mieli tag informacje o aplikacji o nazwie rebate_code zdefiniowane.
Jak pokazano powyżej zawartości powiadomień wypychanych zawiera $ znacznik {rebate_code}, które będzie wskazywać, że ma zostać zastąpiona rzeczywistej zawartości znacznika informacje o aplikacji.

> Ostrzeżenie: Jeśli znacznik informacje o aplikacji nie jest zdefiniowane dla użytkownika, użytkownik nie otrzyma naciśnięcie.

-    Wynik
 
![Personalize2][12]

### <a name="you-can-further-personalize-the-text-your-notification"></a>Można jeszcze bardziej spersonalizować tekst usługi powiadomień
![Personalize3][13]

-    W tym tytuł powiadomienia
-    I zawartość wiadomości.
-    Wybierz typ powiadomienia (Widok tekstu lub widoku strony sieci Web)
 
![Personalize4][14]

### <a name="the-body-of-an-announcement-may-also-be-personalized-with"></a>Treść powiadomienia mogą być również spersonalizowane na podstawie:
-    Adres URL akcji, czy chcesz dostosować strona początkowa
-    Tytuł,
-    Treść wiadomości.
 
 
## <a name="differentiate-your-push-notification-in-or-out-of-app"></a>Rozróżnienie i powiadomień Push (w lub poza aplikacji)
-    Wybierz typ powiadomienia wypychane, wybierz aplikację, przejdź do sekcji "Osiągnięcia", wybierz lub utworzysz kampanii wypychanych i przejdź do sekcji "Powiadomienie".
 
-    Kliknij na "dostawy".
-    Kliknij pole wyboru "Ogranicz działania", gdy chcesz powiadomienie występuje na określone działania (ekranów).

![Differentiate1][15]

### <a name="out-of-app-only-delivery-mode"></a>Dostawy "Się tylko aplikacji"
![Differentiate2][16]

"Się tylko aplikacji" dostawy zapewnia powiadomienia wypychane, gdy aplikacja zostanie zamknięta. Jest to standardowy wypychanych powiadomień.
Gdy wybierzesz "się tylko aplikacji", trzeba mieć już podany certyfikaty z platformę, na której aplikacji jest oparty na (APN lub GCM).

### <a name="see-also"></a>Zobacz też
-  [Usługa powiadomień — certyfikatów Push firmy Apple](http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9), usługa Google Cloud wiadomości — certyfikat] (http://developer.android.com/google/gcm/index.html) 

### <a name="in-app-only-delivery-mode"></a>"w aplikacji tylko" dostawy
![Differentiate3][17]

Dostawy "W aplikacji tylko" zawiera powiadomienia wypychane, gdy aplikacja jest uruchomiona.
W ramach tego powiadomienia nie musisz przejść przez system APN i GCM.
System dostarczania w aplikacji służy do osiągnięcia użytkowników końcowych.
Możesz w pełni dostosować powiadomienie i zdecyduj, w którym będą wyświetlane aktywności (ekran) powiadomienie.

### <a name="anytime-delivery-mode"></a>"W dowolnym momencie" dostawy
Możesz wybrać dostawy "W dowolnym momencie", gwarantuje, możesz nawiązywać połączenia użytkownika końcowego, czy aplikacja jest uruchomiona lub nie.
Po wybraniu opcji "W dowolnym momencie" trzeba mieć już podany certyfikaty z platformę, na której aplikacji jest rozwijania (APN lub GCM). 
 
## <a name="schedule-a-push-campaign"></a>Planowanie kampanii Push
### <a name="plan-to-start-a-campaign"></a>Plan, aby rozpocząć kampanii
![Shedule1][18]

To 21 z marca i masz ogłoszenie nawiązać i być to planowane dla 22 z marca północy. Nie trzeba otrzymywać przed interfejsu zrobić wypychanych! Można planować z wyprzedzeniem dokładnie powiadomienia zostaną wysyłane minutę.
-    Wyczyść pozycję "Brak" pole wyboru i wybierz czas rozpoczęcia 
-    Wybierz datę i godzinę ma być uruchamiany kampanii push.

### <a name="plan-to-end-a-campaign"></a>Plan, aby zakończyć kampanii
![Shedule2][19]

Chcesz kampanii Aby zatrzymać na 25 z marca godzinie 3:00, ale wiadomo, że nie będzie on zawierał to zrobić.
Nie trzeba otrzymywać przed interfejs do push! Można planować z wyprzedzeniem dokładnie przestanie kampanii minutę.
-    "Brak" kliknij pole wyboru lub wybierz czas zakończenia
-    Wybierz datę i godzinę, można dokończyć kampanii push.

### <a name="end-a-campaign-manually"></a>Kończenie kampanii ręcznie
![Shedule3][20]

Domyślnie "Brak" są zaznaczone pola wyboru.
Oznacza to, że kampanii rozpocznie się zaraz po należy go aktywować w sekcji reach i zakończy się po przestanie w sekcji reach.
 
> Uwaga: Kampanii utworzona bez daty zakończenia przechowywać naciśnięcie lokalnie na urządzeniu, a wyświetleniem przy następnym otwarciu aplikacji nawet wtedy, gdy kampanii ręcznie zostanie zakończone.

## <a name="enhance-a-push-notification-with-a-text-view"></a>Ulepszanie powiadomienia wypychane za pomocą widoku tekstu
### <a name="what-is-a-text-view"></a>Co to jest widok tekstu?
![TextView1][21]

Wyświetlanie tekstu jest okno podręczne z zawartością tekstu. Ta podręczna pojawia się po powiadomień wypychanych kliknie użytkownika końcowego.
Wyświetlanie tekstu umożliwia prezentowanie zawartości do użytkownika końcowego. Jest to także możliwość przedstawienia połączenia akcji, takich jak przechodzenie do strony zawierającej aplikacji, przekierowywania do sklepu, Otwieranie strony sieci web, wysyłanie wiadomości e-mail uruchomienie wyszukiwania zlokalizowane geo itp.

### <a name="example-text-view"></a>Przykład: Widok tekstu
-    Tworzenie kampanii powiadomień wypychanych w sekcji "Reach" i nadaj nazwę kampanii
 
![TextView2][22]

-    Napisz wiadomość, która będzie wyświetlana w powiadomieniu.
-    Wybierz typ zawartości zawiadomienie o "tekst"
 
![TextView3][23]

> Uwaga: po naciśnięciu widoku tekst zawsze pochodzi z powiadomienie najpierw. 

- Definiowanie tekstu (po dokonaniu wyboru zawiadomienie o tekst pojawi się sekcji podrzędny umożliwia definiowanie tekst, który ma być wyświetlany.)
 
![TextView4][24]

-    Wpisz tytuł, który będzie wyświetlany w górnej części wiadomości.
-    Wpisz zawartość głównym widoku tekstu.
-    Napisz zawartości, który będzie wyświetlany na przycisku akcji (przycisk akcji umożliwia aplikacji podejmowanie określonych akcji, takich jak otwierania strony stosowania przekierowywanie do sklepu App store lub dowolnego rodzaju źródła, które pozwalają).
-    Pisanie zawartości, który będzie wyświetlany na przycisku Zakończ (po kliknięciu przycisku Zakończ, widok tekstu znikną.)
 
-    Tworzenie kampanii powiadomień wypychanych i pojawi się na liście kampanii.
 
![TextView5][25]

-    Aktywowanie kampanii powiadomień wypychanych wysyłanie widok tekstu do użytkowników.
 
![TextView6][26]

-    Wynik
 
![TextView7][27]

-    Użytkownik otrzymuje powiadomienie i kliknij go.
-    Wyświetlanie tekstu jest wyświetlany jako podręczny Zezwalanie użytkownikowi na interakcję go.

## <a name="enhance-a-push-notification-with-a-web-view"></a>Ulepszanie powiadomienia wypychane z widoku sieci Web
### <a name="what-is-a-web-view"></a>Co to jest widok sieci Web?
![WebView1][28]

Widok sieci web jest okno podręczne z zawartością sieci web. Ta podręczna pojawia się, gdy użytkownik końcowy kliknie powiadomień push.
Widok sieci web umożliwia ma więcej interakcji z użytkownika końcowego.
Jest to także możliwość przedstawienia wywołania akcji, takich jak przekierowanie do sklepu App Store otwierania strony sieci web, wysyłanie wiadomości e-mail, uruchomienie wyszukiwania zlokalizowane geo itd...

### <a name="example-web-view"></a>Przykład: Widok sieci Web
-    Tworzenie kampanii wypychanych w sekcji "Reach" i nadaj nazwę kampanii.
 
![WebView2][29]

-    Napisz wiadomość, która będzie wyświetlana w powiadomieniu.
-    Wybierz typ zawartości zawiadomienie o jako "web"
 
![WebView3][30]

### <a name="about-announcement-types"></a>Zawiadomienie o typy: — informacje
- Powiadomienie o tylko: jest proste powiadomienie standardowe. Oznacza to, że jeśli użytkownik kliknie je, nie dodatkowych widok będzie wyświetlany, ale wystąpi akcji skojarzonych z nim.
- Zawiadomienie o tekstu: to powiadomienie, które uczestniczy użytkownik miał spojrzenie na widok tekstu.
- Zawiadomienie o sieci Web: to powiadomienie, które uczestniczy użytkownik miał przyjrzeć się widoku sieci web.
Wybierz zawartość "Zawiadomienie o sieci Web".

> Uwaga: Po naciśnięciu widoku sieci web zawsze pochodzi z powiadomienie najpierw.

- Definiowanie zawartości sieci web (po dokonaniu wyboru zawiadomienie o zawartości sieci web, pojawi się podsekcji, co umożliwia definiowanie wyświetlania zawartości sieci web, które mają być wyświetlane.)

 
![WebView4][31]

-    Wpisz tytuł, który będzie wyświetlany w górnej części wiadomości (opcjonalnie).
-    Wpisz swój kod HTML.
-    Kliknij przycisk Tryb przełączanie edition i zobacz, jak wygląda autorskie.
-    Napisz zawartości, który będzie wyświetlany na przycisku akcji (przycisk akcji umożliwia aplikacji podejmowanie określonych akcji, takich jak otwierania strony stosowania przekierowanie do sklepu lub dowolnego rodzaju źródła, które pozwalają).
-    Pisanie zawartości, który będzie wyświetlany na przycisku Zakończ (po kliknięciu przycisku Zakończ, widok sieci web znikną).
 
-    Wynik
 
![WebView5][32]

-    Użytkownik otrzyma powiadomienie i kliknij go.
-    Wyświetlanie tekstu jest wyświetlany jako podręczny Zezwalanie użytkownikowi na interakcję go.

<!--Image references-->
[1]: ./media/mobile-engagement-how-tos/First1.png
[2]: ./media/mobile-engagement-how-tos/First2.png
[3]: ./media/mobile-engagement-how-tos/First3.png
[4]: ./media/mobile-engagement-how-tos/First4.png
[5]: ./media/mobile-engagement-how-tos/First5.png
[6]: ./media/mobile-engagement-how-tos/First6.png
[7]: ./media/mobile-engagement-how-tos/First7.png
[8]: ./media/mobile-engagement-how-tos/Test1.png
[9]: ./media/mobile-engagement-how-tos/Test2.png
[10]: ./media/mobile-engagement-how-tos/Test3.png
[11]: ./media/mobile-engagement-how-tos/Personalize1.png
[12]: ./media/mobile-engagement-how-tos/Personalize2.png
[13]: ./media/mobile-engagement-how-tos/Personalize3.png
[14]: ./media/mobile-engagement-how-tos/Personalize4.png
[15]: ./media/mobile-engagement-how-tos/Differentiate1.png
[16]: ./media/mobile-engagement-how-tos/Differentiate2.png
[17]: ./media/mobile-engagement-how-tos/Differentiate3.png
[18]: ./media/mobile-engagement-how-tos/Schedule1.png
[19]: ./media/mobile-engagement-how-tos/Schedule2.png
[20]: ./media/mobile-engagement-how-tos/Schedule3.png
[21]: ./media/mobile-engagement-how-tos/TextView1.png
[22]: ./media/mobile-engagement-how-tos/TextView2.png
[23]: ./media/mobile-engagement-how-tos/TextView3.png
[24]: ./media/mobile-engagement-how-tos/TextView4.png
[25]: ./media/mobile-engagement-how-tos/TextView5.png
[26]: ./media/mobile-engagement-how-tos/TextView6.png
[27]: ./media/mobile-engagement-how-tos/TextView7.png
[28]: ./media/mobile-engagement-how-tos/WebView1.png
[29]: ./media/mobile-engagement-how-tos/WebView2.png
[30]: ./media/mobile-engagement-how-tos/WebView3.png
[31]: ./media/mobile-engagement-how-tos/WebView4.png
[32]: ./media/mobile-engagement-how-tos/WebView5.png

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
 
