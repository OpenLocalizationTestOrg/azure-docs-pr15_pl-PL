<properties 
   pageTitle="Interfejs użytkownika Azure zaangażowania urządzeń przenośnych — Reach kryterium" 
   description="Dowiedz się, jak wysyłanie kampanii wypychanych do podzbiór Wybierz użytkowników za pomocą zaangażowania Mobile Azure za pomocą kryteriów określania docelowych" 
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


# <a name="how-to-use-targeting-criteria-to-send-push-campaigns-to-a-select-subset-of-your-users"></a>Jak za pomocą kryteriów określania docelowych wysyłanie kampanii wypychanych do wybierz część wszystkich użytkowników

Użyciu funkcji określania docelowych odbiorców według określonych kryteriów przy użyciu przycisku "Nowe kryteria" jest jednym z najbardziej zaawansowanych pojęć w zaangażowania Mobile Azure ułatwia wysyłania odpowiednie powiadomienia wypychane że klienci będą odpowiadać zamiast spamowi Wszyscy. Możesz ograniczyć odbiorców na podstawie kryteriów standardowych i zasymulowania umieszcza, aby ustalić, ile osób otrzyma powiadomienie.

**Zobacz też:**

- [Interfejs użytkownika dokumentacji - Reach - nowej wypychanych kampanii][Link 27]

## <a name="audience-criteria-can-include"></a>Kryteria odbiorców mogą być następujące:
- **Technicals:** Można kierować oparte na tych samych informacji technicznych widoczne w sekcji Monitor i analizy. **Zobacz też:** [Dokumentacja interfejsu użytkownika — analizy] [ Link 15], [Dokumentację interfejsu użytkownika — monitorowanie][Link 16]
- **Lokalizacji:** Aplikacje, które używają "czasie rzeczywistym raportowania lokalizacji" z ogrodzenia Geo umożliwia Geo lokalizacji jako kryterium docelowych odbiorców z lokalizacji GPS. Połączenia "Lokalizacji opóźnieniem raportowania" również docelowych odbiorców z lokalizacji telefonu komórkowego ("czasie rzeczywistym raportowania lokalizacji" i "Opóźnieniem obszar lokalizacji raportowania" należy aktywować z zestawu SDK). **Zobacz też:** [Dokumentacja zestawu SDK — iOS - integracji] [ Link 5], [Integracja — Android — dokumentacji SDK][Link 5]
- **Osiągnięcia opinii:** Można kierować odbiorców na ich podstawie poprzedniego powiadomienia reach za pośrednictwem opinii reach anonsy, ankiet i umieszcza danych. Pozwala to lepiej docelowej odbiorców po dwóch lub trzech osiągnięcia kampanii niż po raz pierwszy. Można również umożliwia za odfiltrowywania użytkowników, którzy już odebrane powiadomienia o podobnej zawartości, ustawiając kampanii nie były wysyłane do użytkowników, którzy już odebrane określonej kampanii poprzedniego. Można nawet wykluczanie użytkowników osób, które znajdują się określonej kampanii, który jest nadal aktywny odbieranie umieszcza nowy. **Zobacz też:** [Dokumentacja interfejsu użytkownika — osiągnięcia — Push zawartości][Link 29]
- **Instalowanie śledzenia:** Możesz śledzić informacje dotyczące zainstalowanego użytkowników aplikacji. **Zobacz też:** [Dokumentacja interfejsu użytkownika — ustawienia][Link 20]
- **Profilu użytkownika:** Można kierować na podstawie standardowe informacje o użytkowniku, a może docelowej według informacje niestandardową aplikację, które zostały utworzone. Dotyczy to również użytkowników, którzy są obecnie zalogowany i użytkowników, którzy mają odpowiedzi na pytania określone, którego zażądano je w aplikacji zamiast tylko jak ich odpowiedziano do poprzedniego kampanii. Wszystkie swoje informacje aplikacji zdefiniowane dla pokazu aplikację w górę na liście.
- Segmentów: Możesz także docelowej oparte na segmenty, które zostały utworzone na podstawie określonego użytkownika zachowań zawierających wiele kryteriów. Wszystkie segmentów zdefiniowanych dla aplikacji są wyświetlane na tej liście. **Zobacz też:** [Dokumentacja interfejsu użytkownika — segmentów][Link 18]
- **Informacje o aplikacji:** Znaczniki niestandardowe informacje o aplikacji można tworzyć w obszarze "Ustawienia" do śledzenia zachowanie użytkownika. **Zobacz też:** [Dokumentacja interfejsu użytkownika — ustawienia][Link 20]

## <a name="example"></a>Przykład: 
Jeśli chcesz przekazać ogłoszenie tylko do podzbioru użytkowników wykonanych akcji w aplikacji zakupu.

1. Przejdź do strony Ustawienia aplikacji, wybierz menu "Informacje o aplikacji" i wybierz opcję "Nowe informacje o aplikacji"
2. Zarejestruj nowe informacje o aplikacji logicznych o nazwie "inAppPurchase"
3. Ustawianie jako aplikacji ustawić te informacje o aplikacji na "prawda", gdy użytkownik wykona pomyślnie zakupu w aplikacji (przy użyciu sendAppInfo ("inAppPurchase",...) funkcja)
4. Jeśli nie chcesz, aby to zrobić za pomocą aplikacji, należy go z sieci wewnętrznej bazy danych przy użyciu urządzeń interfejsu API)
5. Następnie wystarczy utworzyć usługi anons z kryterium ograniczenie uczestnikom użytkownicy posiadający "inAppPurchase" ustawiony na "wartość true")
 
> Uwaga: Kierowanie na podstawie kryteriów innych niż znaczniki informacje o aplikacji wymaga zaangażowania Mobile Azure do gromadzenie informacji z urządzenia użytkownika przed wysłaniem naciśnięcie i może powodować opóźnienia. Umieszcza konfiguracji złożonych wypychanych również może opóźnić opcji (na przykład aktualizowanie znaczki). Przy użyciu kampanii "ujęcie" z interfejsu API wypychanych jest bezwzględne najszybszy metoda push zaangażowania Mobile Azure. Używanie tylko znaczniki informacje o aplikacji jako kryteriów wypychanych kampanii Reach (albo z interfejsu API osiągnięcia lub interfejsu użytkownika) jest następnej metody najszybszy, ponieważ znaczniki informacje o aplikacji są przechowywane na serwerze. Przy użyciu innych kryteriów określania docelowych kampanii wypychanych jest metoda push najbardziej elastyczne, ale najniższej wartości, ponieważ zaangażowania Mobile Azure ma kwerendy urządzenia, aby móc wysyłać kampanii.
 
![Dostęp do Criterion1][29] 

## <a name="criterion-options-apply-to"></a>Opcje kryterium mają zastosowanie do:
- **Technicals**     
- Nazwa sprzętowego: Nazwa sprzętowego
- Wersja oprogramowania układowego: wersję oprogramowania układowego
- Model urządzenia: modelu urządzenia
- Producenta urządzenia: producenta urządzenia
- Wersja aplikacji: wersja aplikacji
- Nazwa przewoźnika: Nazwa przewoźnika, zdefiniowane
- Kraj Carrier: country przewoźnika, zdefiniowane
- Typ sieci: typ sieci
- Ustawienia regionalne: ustawienia regionalne
- Rozmiar ekranu: rozmiar ekranu
- **Lokalizacja**      
- Ostatni znany obszar: miejscowości kraju i regionu,
- Czasie rzeczywistym geo ogrodzenia: Lista POIs (nazwa, akcje), cykliczne GROT (nazwa, szerokość, długość geograficzna, Radius metry)
- **Osiągnięcia opinii**     
- Zawiadomienie o opinii: anons, opinii
- Ankieta opinii: ankiety, opinii
- Ankieta opinii odpowiedzi: ankieta opinii odpowiedzi, pytanie, wybór
- Dane wypychanych opinii: wypychanych danych, opinii
- **Instalowanie śledzenia**     
- Magazyn: Magazyn, zdefiniowane
- Źródła: Nie zdefiniowano źródło
- **Profil użytkownika**     
- Płeć: mężczyzny lub kobiety, zdefiniowane
- Data urodzenia: operator, datę zdefiniowane
- Wyraź zgodę na: PRAWDA lub FAŁSZ, nieokreślone
- **Informacje o aplikacji**      
- Ciąg: Ciąg zdefiniowane
- Daty: operator, datę nie zdefiniowano
- Całkowitą: operator, liczba, nie zdefiniowano
- Wartość logiczna: wartość PRAWDA lub FAŁSZ, zdefiniowane
- **Części**    
- Nazwa segmentów (z listy rozwijanej) wykluczeń (użytkownicy docelowej, które nie są częścią tej części).

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
 
