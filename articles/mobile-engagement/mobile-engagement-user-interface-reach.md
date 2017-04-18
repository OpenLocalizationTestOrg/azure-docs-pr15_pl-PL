<properties 
   pageTitle="Interfejs użytkownika Azure zaangażowania urządzeń przenośnych — Reach" 
   description="Dowiedz się, jak docieranie do użytkowników aplikacji do powiadomienia wypychane przy użyciu zaangażowania Mobile Azure" 
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


# <a name="how-to-reach-out-to-the-users-of-your-application-with-push-notifications"></a>Jak docieranie do użytkowników aplikacji przy użyciu powiadomienia wypychane

W tym artykule opisano kartę **osiągnięcia** portalu **Zaangażowania Mobile** . Monitorowanie i zarządzać swoimi aplikacjami urządzeń przenośnych za pomocą portalu **Zaangażowania Mobile** . Należy zauważyć, że aby rozpocząć korzystanie z portalu należy najpierw utworzyć konto **Azure Mobile zaangażowania** . Aby uzyskać więcej informacji zobacz [Tworzenie konta usługi Azure Mobile zaangażowania](mobile-engagement-create.md).

Dostęp do części interfejsu użytkownika jest wypychanych kampanii narzędzie do zarządzania, w którym można utworzyć Edytuj aktywowanie zakończenie monitor i uzyskiwanie statystyki kampanii powiadomień wypychanych i funkcje, które są dostępne za pośrednictwem interfejsu API osiągnięcia (i niektórych elementów niski poziom Push interfejsu API). Należy pamiętać, czy korzystasz z za pośrednictwem interfejsów API lub interfejsu użytkownika, trzeba będzie integracja zarówno zaangażowania Mobile Azure i Reach do aplikacji dla poszczególnych platform z SDK, zanim będzie można używać osiągnięcia kampanii.

>[AZURE.NOTE] Wiele sekcji portalu **Zaangażowania Mobile** interfejsu użytkownika zawierają przycisk **Pokaż Pomoc** . Naciśnij ten przycisk, aby uzyskać więcej informacji kontekstowych o sekcji.

## <a name="four-types-of-push-notifications"></a>Cztery typy powiadomień wypychanych
1.    Anonsy - możliwe do wysyłania wiadomości reklamami użytkownikom, którzy przekierować je do innej lokalizacji w aplikacji lub, aby wysłać je do strony sieci Web lub poza aplikacji ze sklepu. 
2.    Ankiety — umożliwia gromadzenie informacji z użytkowników końcowych przez ich pytania.
3.    Umieszcza danych — umożliwia wysyłanie pliku danych binarnych lub base64. Informacje zawarte w wypychanych dane są wysyłane do aplikacji, aby zmodyfikować bieżące środowisko użytkownika w aplikacji. Aplikacja musi mieć możliwość przetwarzania danych w wypychanych danych.

## <a name="campaign-details"></a>Szczegółowe informacje o kampanii

Można edytować, klonowanie, usuwanie i aktywować kampanii, które nie zostały aktywowane jeszcze przez umieszczenie wskaźnika myszy na ich nazwy lub można kliknąć, aby je otworzyć. Czy klonowanie kampanii, które już zostały aktywowane przez umieszczenie wskaźnika myszy na ich nazwy lub można kliknąć, aby je otworzyć. Jednak nie można zmienić kampanii, gdy zostało już aktywowane.
 
![Reach1][18]

## <a name="reach-feedback"></a>Osiągnięcia opinii

Polecenie **Statystyki** , aby wyświetlić szczegóły kampanii Reach. **Prosty** widok zawiera wizualną reprezentację w formie wykresu słupkowego kolumny o zaistniałej sytuacji po aktywowaniu kampanii. Widok **Zaawansowane** szczegółowe bardziej szczegółowe informacje na temat kampanii wypychanych. Szczegóły te nie będą dostępne, jeśli wysyłasz kampanii testowej to znaczy wypychania wysyłane do urządzenia test. Poniżej opisano, jak należy interpretować następujące szczegóły:

1. **Pushed** - określa liczbę wiadomości przypisany do urządzenia. Ten numer zależy od określonej podczas tworzenia kampanii wypychanych odbiorcy docelowi. Jeśli nie określisz dowolnego docelowych odbiorców, następnie ten wypychanych będą wysyłane do wszystkich zarejestrowanych urządzeń. Podobnie jak wszystkie inne usługi wypychanych firma Microsoft nie powiadomienia wypychane bezpośrednio do urządzenia, ale zamiast tego przekazać je do odpowiednich platformy określonych usług powiadomień wypychanych (obwodowym układzie Nerwowym - APN-GCM-WNS) tak, aby zapewnia powiadomienia o jego do urządzenia. 

2.  **Dostarczone** - określa liczbę wiadomości, które pomyślnie dostarczony przez obwodowym układzie Nerwowym na urządzeniu i potwierdzone jako odebranych przez telefon komórkowy zaangażowania SDK. 
        
    *Powody, dla których dostarczone liczba jest mniejsza niż liczba barki:*
    
    1. Jeśli użytkownik ma odinstalować aplikację z urządzenia, ale obwodowym układzie Nerwowym nie wiesz w czasie wyślemy naciśnięcie do obwodowym układzie Nerwowym wiadomość zostanie usunięty.
    2. Jeśli urządzenie ma aplikacji, ale urządzenia się pracuje w trybie offline przez dłuższy czas, obwodowym układzie Nerwowym zakończy się niepowodzeniem na dostarczenie wiadomości do urządzenia. 
    3. Jeśli dostarczenie wiadomości do urządzenia, ale SDK zaangażowania Mobile w aplikacji nie rozpoznaje zawartość wiadomości, jej pomija tę wiadomość. Może się to zdarzyć, jeśli dostosowanie powiadomienie w aplikacji generuje wyjątek, który firma Microsoft efektywnej w SDK i upuszczanie wiadomości. To również może wystąpić, jeśli aplikację na urządzeniu jest przy użyciu wersji SDK zaangażowania Mobile, która nie jest w stanie zrozumieć nowszą wersją wypychanych wiadomości wysłanych z platformy, ale jest to tylko wtedy, gdy aplikacja została uaktualniona po powiadomienie zostało wysłane z platformy usługi. Na karcie **Zaawansowane** informuje, ile wiadomości zostały usunięte. 
    4. Na urządzeniach z systemem iOS wiadomości czasami nie dostarczenie Jeśli albo urządzenie jest włączone baterii lub aplikacji jest używające dużo power podczas przetwarzania zdalnego powiadomienia. Jest to ograniczenie urządzeń z systemem iOS.   

3.  **Wyświetlane** - określa liczbę wiadomości, które pomyślnie są wyświetlane na ekranie aplikacji na urządzeniu w formularzu powiadomienia wypychane i w nowym oknie-z aplikacji systemu w Centrum powiadomienie lub powiadomienie w aplikacji w aplikacji dla urządzeń przenośnych.  Na karcie **Zaawansowane** informuje o ile były powiadomienia systemowe i ile były powiadomienia w aplikacji. 
    
    *Powody, dla których wyświetlane liczba jest mniejsza niż liczba dostarczone (oczekiwanie mają być wyświetlane)*
    
    1. Gdyby kampanii powiadomienie datę zakończenia nad nim następnie jest możliwe, że została dostarczona powiadomienie, ale po otrzymaniu podczas otwierania i wyświetlania go do użytkownika aplikacji, jej już wygasła, nie została nigdy nie będzie wyświetlana.   
    2. Jeśli powiadomienie jest powiadomienie w aplikacji, a następnie powiadomienie jest wyświetlane tylko wtedy, gdy użytkownika aplikacji zostanie otwarta aplikacja. W przypadku której użytkownik aplikacji nie otworzy aplikację zestawu SDK zgłasza, że powiadomienie została dostarczona, ale nie są jeszcze wyświetlone dopóki nie zostanie otwarty w aplikacji. 
    2. Jeśli powiadomienie jest powiadomienie w aplikacji i mają być wyświetlane na określonym aktywności i ekranu, a następnie także powiadomienie jest informowany dostarczona, ale jeszcze nie dostarczona do użytkownika skonfigurowane otwarcie aplikacji na ekranie określone. 
    
4.  **Interakcji użytkownika** — określa liczbę wiadomości, które użytkownika aplikacji ma interakcji z i będzie zawierać wiadomości, które są actioned lub opuszczenia. 

    - *Użytkownik aplikacji może akcji powiadomienie w jeden z następujących sposobów:*
            
        1. W przypadku powiadomienie powiadomienie system limit z aplikacji lub powiadomienie w aplikacji wysyłane tylko do powiadomień, a następnie kliknięciu aplikacji w powiadomieniu.
        2. W przypadku powiadomienie powiadomienie w aplikacji z tekstem lub widoku strony sieci web lub ankiety aplikacji użytkownik kliknie przycisk akcji w powiadomieniu.
        3. W przypadku powiadomienie powiadomienie w aplikacji przy użyciu widoku sieci web następnie aplikacji użytkownik kliknie adres URL sieci web tylko w widoku [Android]
    
    - *Użytkownik aplikacji wyjść powiadomienie w jeden z następujących sposobów:*
    
        1. Kliknięcie przycisku Zamknij w powiadomieniu bezpośrednio. 
        2. Przesuwając szybko z dala od komputera lub usuwanie powiadomienia. 
        3. Powiadomienia w aplikacji z zawartości tekstu i sieci web i ankiety zazwyczaj są wyświetlane użytkownikowi aplikacji w dwóch krokach. Najpierw widzą powiadomienie i po kliknięciu go widzą zawartość kolejnych ankietę tekstu i sieci web. Użytkownika aplikacji można zamknąć powiadomienie w jedną z następujących czynności i szczegółów w widoku Zaawansowane rejestruje to. 

5.  **Actioned** - określa liczbę wiadomości, które zostały jawnie actioned przez użytkownika aplikacji. Jest to najbardziej interesujące numer, jak to informuje, ilu użytkowników aplikacji zostały zainteresowanych przez wiadomość, którą przypisany w powiadomieniu. 
 
> [AZURE.NOTE] W systemie iOS i okien otwieranie platform, jeśli użytkownik ma aplikację i kampanii został kampanii "W dowolnym momencie", a następnie jest możliwe, że oba wylogowywanie się z aplikacji i powiadomienia w aplikacji są wyświetlane w tym samym czasie. Może to powodować wyższymi niż dostarczone zestawienia wyświetlane. Jeśli interakcji użytkownika lub akcje powiadomienie, a następnie nawet liczba interakcji użytkownika/Actioned może być większy niż dostarczone. 


![Reach2][19]

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
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md
 
