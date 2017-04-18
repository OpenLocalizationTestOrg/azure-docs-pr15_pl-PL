<properties 
   pageTitle="Interfejs użytkownika Azure zaangażowania urządzeń przenośnych — Moje konto" 
   description="Dowiedz się, jak zarządzać urządzeniami test i profilu konta przy użyciu zaangażowania Mobile Azure" 
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

# <a name="how-to-manage-your-account-profile-and-test-devices"></a>Jak zarządzać urządzeniami test i profilu konta
 
Ten artykuł zawiera opis strony **głównej** portalu **Zaangażowania Mobile** . Monitorowanie i zarządzać swoimi aplikacjami urządzeń przenośnych za pomocą portalu **Zaangażowania Mobile** . 
 
Aby przejść do strony **Moje konto** , kliknij przycisk konta w górnej części strony.

W sekcji Moje konto interfejsu użytkownika to miejsce, w którym można wyświetlać i zmiany ustawień skojarzonych z Twoim kontem, w tym ustawienia profilu i przetestować identyfikatory urządzenia. Te ustawienia zawierać elementy, które są dostępne za pośrednictwem interfejsu API urządzenia.

![MyAccount1][7]  

## <a name="profile"></a>Profil:
Można wyświetlić lub zmienić żadnych ustawień konta, jak pokazano poniżej. Możesz także zmienić innego uprawnień użytkowników do korzystania z aplikacji na podstawie ich adresu e-mail z [dla użytkowników domowych](mobile-engagement-user-interface-home.md).

![MyAccount2][8]  

## <a name="devices"></a>Urządzenia:
Wyświetlanie, dodawanie i usuwanie testowanie urządzenia identyfikatory urządzeń test, które umożliwia testowanie **osiągnięcia** lub **wypychanych** kampanii. Kontekstowe instrukcje dotyczące znajdowanie Identyfikatora urządzenia urządzeń dla każdej platformy (iOS, Android, Windows Phone itp.) są wyświetlane po kliknięciu pozycji "Nowe urządzenie". 
 
![MyAccount3][9]  
 
Aby użyć Push interfejsu API lub interfejsu API urządzenia, musisz znać użytkowników Unikatowy identyfikator urządzenia (parametr identyfikator urządzenia). Istnieje kilka sposobów na pobranie ich:
 
1. Za pomocą funkcji "Pobierz" interfejsu API urządzenia Aby uzyskać pełną listę identyfikatorów urządzenia, z sieci wewnętrznej bazy danych.
2. W przypadku aplikacji zestawu SDK umożliwia wykonywanie. (W systemie Android, wywołaj getDeviceID() klasy agenta, a następnie na iOS, znajdziesz właściwość identyfikator urządzenia klasy agenta).
3. Z anonsów Reach Jeśli adres URL akcji skojarzonych z anons zawiera wzór {identyfikator urządzenia} go zostaną automatycznie zastąpione identyfikator urządzenia powodujące akcji.
http://<example>.com/registeruser?deviceid = {identyfikator urządzenia} & otherparam = myparamdata zostaną zastąpione: http://<example>.com/registeruser?deviceid = XXXXXXXXXXXXXXXX & otherparam = myparamdata 
4. Z anonsów web Reach Jeśli kodu HTML anons zawiera wzór {identyfikator urządzenia} go zostaną automatycznie zastąpione identyfikator urządzenia wyświetlanie zawiadomienie o sieci web.
Oto mój identyfikator urządzenia: {identyfikator urządzenia} zostaną zastąpione: w tym miejscu jest mój identyfikator urządzenia: XXXXXXXXXXXXXXXX
5.  Otwieranie aplikacji na urządzeniu i wykonać zdarzenie w swojej aplikacji, które zostały oznakowane.
Na "Interfejsu użytkownika — zdarzenia aplikacji — Monitor — - szczegóły" Znajdź zdarzenia wykonywane na liście.
Kliknij, aby to zdarzenie w monitorze.
Identyfikator urządzenia powinien znajdować się na liście urządzeń, które jest wykonywane to zdarzenie.
Następnie możesz skopiować ten identyfikator urządzenia i zarejestrować ją w "Urządzeń interfejsu użytkownika — Moje konto — — nowego urządzenia — wybierz posiadanej platformy urządzenia".
>(Należy pamiętać, że po wyłączeniu IDFA dla systemu iOS identyfikator urządzenia mogą się zmieniać w czasie Jeśli Odinstaluj i ponownie zainstaluj aplikację).

##<a name="troubleshooting-guide"></a>Rozwiązywanie problemów z przewodnika
-  [Podręcznik rozwiązywania problemów — usługi][Link 24]

## <a name="see-also"></a>Zobacz też
-  [Dokumentacja interfejsu użytkownika — strona główna][Link 13]


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


 
 
