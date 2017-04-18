<properties 
   pageTitle="Azure zaangażowania przenośnych Podręcznik - SDK rozwiązywania problemów" 
   description="Rozwiązywanie problemów z zestawu SDK integracji w zaangażowania Mobile Azure" 
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

# <a name="troubleshooting-guide-for-sdk-integration-issues"></a>Rozwiązywanie problemów z przewodnik w sprawie integracji SDK

Poniżej przedstawiono możliwych problemów, które mogą się pojawić w sposób zaangażowania Mobile Azure integruje się z aplikacji.

## <a name="sdk-issues-discovered-by-a-failure-in-another-area-of-your-application"></a>Problemy SDK odkryte awaria w innym miejscu aplikacji

### <a name="issue"></a>Problem
- Interfejs użytkownika Błąd zbioru danych (w analizy, monitorowanie, segmentacji lub pulpitów nawigacyjnych).
- Powiadomienia push błędy (umieszcza nie działa w aplikacji, poza aplikacji lub oba).
- Zaawansowana funkcja błędy (śledzenie, Geolokalizacja lub platformę, którą umieszcza określonych nie działają).
- Błędy interfejsu API (interfejsy API błędów często trybie cichym bez komunikatu o błędzie).
- Awarie usług (Brak zaangażowania Mobile Azure działa aplikacji).

### <a name="causes"></a>Powoduje, że

- Większość problemów, które należy rozwiązać z SDK zaangażowania Mobile Azure zostaną wykryte awaria w aplikacji (na przykład awarii zbioru danych interfejsu użytkownika, błąd wypychanych, zaawansowana funkcja błąd, błąd interfejsu API, awarii aplikacji lub awaria widocznej usługi).  
- Jeśli poszczególnych funkcji zaangażowania Mobile Azure nie ma doświadczenia w aplikacji przed, będzie konieczne wykonywanie integracja. 
- Jeśli poszczególnych funkcji zaangażowania Mobile Azure pracy i zatrzymane, może być konieczne uaktualnienie do ostatniej wersję z SDK zaangażowania Mobile Azure. Pamiętaj, że jest innej wersji pakietu SDK Azure Mobile zaangażowania dla każdej platformy obsługiwane przez Azure Mobile zaangażowania (Android, iOS, Windows i Windows Phone).

#### <a name="sdk-integration"></a>Integracja SDK

- Azure zaangażowania Mobile prawidłowo włączone SDK (analizy).
- Osiągnięcia prawidłowo zintegrowane SDK (w aplikacji i z umieszcza aplikacji).
- Certyfikat wygasła lub jest niepoprawny ZLECENIE a deweloperów (tylko system iOS).
- GCM lub ADM prawidłowo zintegrowane SDK (Android tylko — usługa określonych umieszcza).
- Śledzenie prawidłowo włączone SDK (Zainstaluj magazyn śledzenia).
- Opóźnieniem lokalizacji lub lokalizacji GPS prawidłowo zintegrowany zestaw SDK (Określanie wartości docelowych przez lokalizację geo).


**Zobacz też:**

- [Dokumentacja zestawu SDK - przewodniki integracji][Link 5] 
- [Podręcznik rozwiązywania problemów — Push][Link 23]

#### <a name="sdk-upgrade"></a>Uaktualnianie SDK

- Musisz uaktualnić SDK, aby rozwiązać problemy związane ze starszymi wersjami SDK (często powiązana z nowszych wersjach urządzenia z systemem operacyjnym).
- Odinstaluj wszystkie wcześniejsze wersje aplikacji z urządzenia i ponownie zainstaluj najnowszą wersję aplikacji, ponownie zarejestrować swój identyfikator urządzenia z zaangażowania Mobile Azure, elementy interfejsu użytkownika, aby potwierdzić, że urządzenie jest za pomocą najnowszej wersji aplikacji.

**Zobacz też:**

- [Dokumentacja zestawu SDK — informacje o wersji](http://go.microsoft.com/fwlink/?LinkId= 525554) 
- [Dokumentacja zestawu SDK - przewodniki uaktualnienia](http://go.microsoft.com/fwlink/?LinkId= 525554)

#### <a name="sdk-other"></a>Zestaw SDK innych

- Błędy w aplikacji pojawiają "AndroidManifest.xml" mogą powodować zaangażowania Mobile Azure nie będą działać (tylko system Android).
- Typowy problem z integracji SDK i użycie interfejsu API jest pomylić SDK i klucz interfejsu API.

**Zobacz też:**

- [Pojęcia — słownik][Link 6]

## <a name="advanced-coding-issues"></a>Zaawansowane kodowania problemów

### <a name="issue"></a>Problem
-  Platformy konkretnego kodu nie są bezpośrednio związane z zaangażowania Mobile Azure może powodować problemy z systemem iOS, Android i Windows Phone.

### <a name="causes"></a>Powoduje, że

- Wiele zaawansowane kodowania problemów z zaangażowania Mobile Azure spowodowane nieprawidłowo pisanych platformy kod nie są bezpośrednio związane z zaangażowania Mobile Azure. Konieczne będzie dokumentacji specyficzne dla platformy, które są opracowywania dla oprócz dokumentacji Azure Mobile zaangażowania (Android, iOS, sieci Web, Windows i Windows Phone).
- Konfigurowanie prawidłowo "kategorie", zapobiega łącze powiadomienie do innej lokalizacji wewnątrz lub poza aplikacji (tylko system Android). 
- To ustawienie nie zostanie "UIKit.framework" "opcjonalne" w kodzie iOS pokazuje "Błąd: nie odnaleziono symbol" i/lub ulega awarii podczas starszych urządzeń z systemem iOS (tylko system iOS).
- Wygasłe certyfikaty lub niepoprawnie przy użyciu wersji standardowego lub zlecenie certyfikatu, przyczyny problemów wypychanych (tylko system iOS).
- Istnieją pewne ograniczenia związane z platformy Azure Mobile zaangażowania nie mogą sterować (na przykład umieszcza działania Centrum systemu dla wylogowywanie się z aplikacji w systemie iOS i Android).
- Azure zaangażowania Mobile publikuje pełną listę opakowań wewnętrznych, używany przez zaangażowania Mobile Azure dla systemów iOS i Android dla odwołania. Należy pamiętać, że niektóre funkcje zaangażowania Mobile Azure są specyficzne dla platformy (Android, iOS, sieci Web, Windows i Windows Phone).

### <a name="see-also"></a>Zobacz też

 - [Podręcznik rozwiązywania problemów — Push][Link 23] 
 - [Dokumentacja zestawu SDK — informacje o wersji][Link 5]
 - [Dokumentacja zestawu SDK - uaktualnienia prowadnic][Link 5]

## <a name="application-crashes"></a>Awarie aplikacji

### <a name="issue"></a>Problem
- Aplikacja ulega awarii na urządzeniu przez użytkowników końcowych.

### <a name="causes"></a>Powoduje, że

- Informacje o awarii można wyświetlać w *Analizy interfejsu użytkownika* lub *Interfejsu API analizy*
- Można znaleźć identyfikator urządzenia urządzenia test, a następnie wykonaj czynności samej, powodujący awarie aplikacji dla użytkownika końcowego do identyfikowania przyczyny awarii usługi.
- Znane problemy dotyczące SDK zaangażowania Mobile Azure, które aplikacje się zawieszać czasami są rozpoznawane przez uaktualnienie do najnowszej wersji pakietu. Upewnij się sprawdzić informacje o wersji o swojej platformy przy badanie awarii.

### <a name="see-also"></a>Zobacz też

- [Dokumentacja zestawu SDK — informacje o wersji][Link 5]
- [Dokumentacja zestawu SDK - uaktualnienia prowadnic][Link 5]

## <a name="app-store-upload-failures"></a>Błędy przekazywania ze sklepu App

### <a name="issue"></a>Problem
- Błędy związane z przekazywanie najnowszej wersji aplikacji do firmy Apple, Google lub Sklep z systemu Windows.

### <a name="causes"></a>Powoduje, że

- Aplikacji są przechowywane czasami aplikacje blok z niektóre funkcje włączone (np. w sklepie Apple uniemożliwia korzystanie z IDFV w aplikacji w sklepie i magazynie GooglePlay zapobiega udostępniania informacji o aplikacji między aplikacjami). 
- Upewnij się, w przypadku trudności przekazywanie aplikacji do Sklepu możesz sprawdzić informacje o wersji o platformy i bieżącą wersję zestawu SDK.

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
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
 
