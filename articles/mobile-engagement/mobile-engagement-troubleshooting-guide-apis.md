<properties 
   pageTitle="Azure zaangażowania przenośnych Podręcznik - interfejsy API rozwiązywania problemów" 
   description="Rozwiązywanie problemów z przewodniki dla Azure zaangażowania urządzeń przenośnych — interfejsy API" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="10/04/2016"
   ms.author="piyushjo"/>

# <a name="troubleshooting-guide-for-api-issues"></a>Podręcznik interfejsu API problemów dotyczących rozwiązywania problemów

Poniżej przedstawiono możliwe problemy, które mogą wystąpić z interakcja Administratorzy z zaangażowania Mobile Azure za pośrednictwem interfejsów API.

## <a name="syntax-issues"></a>Problemy dotyczące składni

### <a name="issue"></a>Problem
- Występują błędy składniowe za pomocą interfejsu API (lub nieoczekiwane działanie).

### <a name="causes"></a>Powoduje, że

- Problemy dotyczące składni:
    - Upewnij się, Sprawdź składnię określonego interfejsu API używasz, aby potwierdzić, że ta opcja jest dostępna.
    - Typowy problem z użycie interfejsu API jest pomylić API osiągnięcia oraz (większości zadań powinny być wykonywane z interfejsem API osiągnięcia zamiast interfejsu API Push) interfejsu API Push. 
    - Inny typowych problem dotyczący integracji SDK i użycie interfejsu API jest pomylić SDK i klucz interfejsu API.
    - Skryptów łączących się za pośrednictwem interfejsów API trzeba będzie wysłać danych co najmniej 10 minut lub połączenie zostanie limitu czasu (zwłaszcza w przypadku często skryptów interfejsu API Monitor wykrywanie danych). Aby zapobiec limity czasu, masz skrypt wysłać polecenie ping XMPP 10 minut warto sesję aktywności na serwerze.

### <a name="see-also"></a>Zobacz też
 
- [Interfejs API dokumentacji][Link 4]
- [Informacje o protokole XMPP]( http://xmpp.org/extensions/xep-0199.html)
 
## <a name="unable-to-use-the-api-to-perform-the-same-action-available-in-the-azure-mobile-engagement-ui"></a>Nie można wykonać tę samą czynność dostępne w interfejsie użytkownika zaangażowania Mobile Azure za pomocą interfejsu API

### <a name="issue"></a>Problem
- Akcję, która działa w interfejsie użytkownika zaangażowania Mobile Azure nie działa z powiązanych interfejsu API zaangażowania Mobile Azure.

### <a name="causes"></a>Powoduje, że

- Potwierdzenie, że można wykonywać tego samego działania z interfejsu użytkownika zaangażowania Mobile Azure zawiera poprawnie zintegrowanego ta funkcja zaangażowania Mobile Azure z zestawu SDK.

### <a name="see-also"></a>Zobacz też
 
- [Dokumentacja interfejsu użytkownika][Link 1]
 
## <a name="error-messages"></a>Komunikaty o błędach

### <a name="issue"></a>Problem
- Korzystanie z interfejsu API wyświetlane w czasie rzeczywistym, czy w dziennikach kodów błędów.

### <a name="causes"></a>Powoduje, że

- Poniżej przedstawiono listę złożone typowych liczb kody stanu interfejsu API dla odwołania i wstępny rozwiązywania problemów:

        200        Success.
        200        Account updated: device registered, associated, updated, or removed from the current account.
        200        Returns a list of projects as a JSON object or an authentication token generated and returned in the response’s body.
        201        Account created.
        400        Invalid parameter or validation exception (check payload for details). The parameters provided to the API or service are invalid. In this case, the HTTP response will embed more details. Make sure to test for the MIME type of the response as the payload can either be plain text or a JSON object.
        401        Authentication error. No user is currently authenticated or connected (check the AppID and SDK key).
        402        Billing lock. The application is either off its quotas or is currently in a bad billing state.
        403        The application is not enabled or the specific API is disabled for this application.
        403        Unauthorized access to the project or application, invalid access key (the key must match the one provided when created).
        403        Campaign specific errors: campaign must be finished (or has already been activated), the suspend action can only be performed on an scheduled campaign, cannot finish a campaign that is not currently “in progress”, campaign must be “in progress” and the campaign’s property named, manual Push must be set to true.
        403        The email address is already associated to another account (a super user for instance). No authentication token will be generated.
        404        Application, device, campaign, or project identifier not found.
        404        Query parameter is invalid JSON or has a field with an unexpected value.
        404        The email address is not associated with an account. Please create or update the account first.
        405        Invalid HTTP method (GET, POST, etc.) or trying to edit a read only segment (i.e. add or update or delete a criterion). A segment becomes read only after it has been computed for the first time.
        409        Name already associated to a different device ID or campaign.
        413        Too many device identifiers (current limit is 1,000), POST URL encoded entity is over 2MB, or the period is too large to be displayed (the server didn’t retrieve the analytics because the user request is for a period that is too large).
        503        Analytics not available yet (the requested information is not computed yet for an application).
        504        The server was not able to handle your request in a reasonable time (if you make multiple calls to an API very quickly, try to make one call at a time and spread the calls out over time).

### <a name="see-also"></a>Zobacz też

- [Interfejs API dokumentacji — szczegółowe błędy w każdej określonego interfejsu API][Link 4]
 
## <a name="silent-failures"></a>Błędy cichy

### <a name="issue"></a>Problem
- Interfejs API akcji zakończy się niepowodzeniem z komunikatem o błędzie wyświetlany w czasie wykonywania lub w dziennikach.

### <a name="causes"></a>Powoduje, że

- Wiele elementów zostanie wyłączona w interfejsie użytkownika zaangażowania Mobile Azure, jeśli nie są poprawnie, zintegrowanego będzie mogą nieznacznie gorzej z interfejsu API, ale pamiętaj, tak przetestować tę samą funkcję w interfejsie użytkownika, aby sprawdzić, czy działa.
- Azure zaangażowania Mobile i wiele zaawansowanych funkcji zaangażowania Mobile Azure próbujesz użyć, musisz pojedynczo integracji aplikacji z zestawu SDK jako osobnych kroków zanim będzie można ich używać.

### <a name="see-also"></a>Zobacz też

- [Podręcznik rozwiązywania problemów — SDK][Link 25]
 
<!--Link references-->
[Link 1]: mobile-engagement-user-interface-home.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/en-us/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/en-us/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/en-us/pricing/details/mobile-engagement/
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
 
