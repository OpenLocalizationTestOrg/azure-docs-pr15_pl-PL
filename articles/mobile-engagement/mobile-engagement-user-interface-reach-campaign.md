<properties 
   pageTitle="Interfejs użytkownika Azure zaangażowania urządzeń przenośnych — Reach kampanii" 
   description="Laern sposób tworzyć i zarządzać nimi powiadomień wypychanych dla kampanii przy użyciu zaangażowania Mobile Azure" 
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


# <a name="how-to-create-and-manage-push-notification-campaigns"></a>Jak tworzyć i zarządzać nimi kampanii powiadomień wypychanych
Dostęp do części interfejsu użytkownika umożliwia tworzenie nowej kampanii wypychanych przy użyciu formuły złożone, dostarczając wszystkie informacje, które chcesz wysłać powiadomienia push. Opcje kampanii wypychanych się nieco różnić w zależności od typów kampanii cztery: anonsy, ankiety, umieszcza danych oraz Kafelki (tylko dla Windows Phone).

### <a name="option-applies-to"></a>Opcja dotyczy:
- Języki: Wszystkie (anonsy, ankiety, danych umieszcza, Kafelki)
- Kampania: Wszystkie (anonsy, ankiety, danych umieszcza, Kafelki)
- Powiadomienie: Anonsy, ankiet
- Zawartość: Unikatowe dla każdego typu kampanii
- Odbiorcy: Wszystkie (anonsy, ankiety, danych umieszcza, Kafelki)
- Przedział czasu: anonsy, ankiety, kafelków
- Test: Wszystkie (anonsy, ankiety, danych umieszcza, Kafelki)
 
![Dostęp do Campaign1][20]

## <a name="languages"></a>Języki
Menu rozwijane języków umożliwia wysyłanie różnych wersji usługi wypychanych do urządzenia, które są ustawione na używanie różnych języków. Domyślnie wszystkie urządzenia otrzymają samej wypychanych niezależnie od tego, jakie są ustawione za pomocą języka. Użytkownicy z ustawionym językiem innego urządzenia będą otrzymywać wersji domyślny język naciśnięcie. Wiele opcji kampanii wypychanych umożliwiają określenie alternatywny zawartości dla każdego z dodatkowych języków, które można wybrać. 
 
![Dostęp do Campaign2][21]

### <a name="language-differences-apply-to"></a>Różnice w języku odnoszą się do:
- Języki: Unikatowe języków można wybrać oprócz języka domyślnego
- Kampania: Takie Same dla wszystkich języków
- Powiadomienie: Unikatowa dla każdego języka, oprócz języka domyślnego
- Zawartość: Unikatowa dla każdego języka, oprócz języka domyślnego
- Odbiorcy: Mogą być filtrowane według kryterium osobnych języka
- Przedział czasu: samego dla wszystkich języków
- Test: Mogą zostać wysłane do każdego języka, w czasie
 
### <a name="supported-languages"></a>Obsługiwane języki:
- Arabski (ar) 
- Bułgarski (bg) 
- Kataloński (ca) 
- Chiński (zh) 
- Chorwacki (h) 
- Czech (CS) 
- Duński (da) 
- Holenderski (Holandia) 
- Angielski (en) 
- Fiński (fi) 
- Francuski (fr) 
- Niemiecki (de) 
- Grecki (e1) 
- Hebrajskiego (he) 
- Hindi (Witam) 
- Węgierski (Węgry) 
- Indonezyjski (id) 
- Włoski (go) 
- Japoński (ja) 
- Koreański (ko) 
- Łotewski (lv) 
- Litewski (lt) 
- Malajski (macrolanguage) (ms) 
- Norweski (Bokmål) (Uwaga) 
- Polski (pl) 
- Portugalski (pt) 
- Rumuński (ro) 
- Rosyjski (ru) 
- Serbski (sr) 
- Słowacki (sk) 
- Słoweński (sl) 
- Hiszpański (Hiszpania) 
- Szwedzki (OHR) 
- Tagalog (tl) 
- Tajski (ty) 
- Turecki (tr) 
- Ukraiński (Zjednoczone Królestwo) 
- Wietnamski (IV) 
 
## <a name="campaign"></a>Kampanii
Sekcja kampanii ustawić nazwę i Kategoria kampanii oraz jak, jeśli zamierzasz zignorować sekcji odbiorcy kampanii wypychanych i zamiast tego wysłać kampanii za pośrednictwem interfejsu API osiągnięcia (i niektórych elementów z niski poziom Push interfejsu API). Czy można używać kategorii za pomocą szablonu powiadomieniem niestandardowym powiadomienia w aplikacji sterowania na podstawie wstępnie zdefiniowanych ustawień. Można uzyskać listę istniejących "kategorii" przy użyciu interfejsu API osiągnięcia.

> Ostrzeżenie: Jeśli korzystasz z opcją "Odbiorców Ignoruj wypychanych będą wysyłane do użytkowników za pośrednictwem interfejsu API" w sekcji "Kampanii" kampanii Reach kampanii nie zostanie automatycznie wysłany, będzie konieczne ręczne Wyślij go przy użyciu interfejsu API osiągnięcia.
 
![Dostęp do Campaign3][22]
 
### <a name="option-applies-to"></a>Opcja dotyczy:
- Nazwa: wszystkie
- Kategoria: Anonsy, ankiet
- Ignoruj odbiorców, wypychanych będą wysyłane do użytkowników za pośrednictwem interfejsu API: wszystkie
 
## <a name="notification"></a>Powiadomienie
W sekcji powiadomienia umożliwia ustawianie podstawowych ustawień między innymi wypychanych: tytuł naciśnięcie, wiadomości, obraz w aplikacji lub jest dismissible. Wiele ustawień powiadomień są specyficzne dla platformy urządzenia. Możesz określić, czy usługi wypychanych będą wysyłane "w aplikacji" lub "poza aplikację" lub obie. (Należy pamiętać, że użytkownicy mogą "Wyraź zgodę na" lub "rezygnacji" "wylogowywanie się z aplikacji" umieszcza w systemie operacyjnym poziomu na swoich urządzeniach i zaangażowania Mobile Azure nie będą mogli zastąpić to ustawienie. Również należy pamiętać, że interfejs API osiągnięcia obsługuje "w aplikacji" i "w nowym aplikacji" umieszcza. Interfejs API wypychanych może służyć za obsługę "poza aplikację" umieszcza.) Umieszcza można dostosować za pomocą obrazów lub zawartość HTML, w tym głębokości łączy do łączenia spoza aplikacji lub do innej lokalizacji w aplikacji (Android SDK 2.1.0 lub nowszym kategorie konwersji wymagane). Możesz zmienić ikonę lub iOS znaczek i przesyłać zawartość tekstu lub sieci web (podręcznego html zawartości, adres URL łącza do innej lokalizacji wewnątrz lub poza aplikacji). Można również wprowadzić dzwonienia urządzeń z systemem Android lub wibracje z naciśnięcie. (Należy pamiętać, że konieczne będzie odpowiedniego pliku, aby pierścienia lub wibracje urządzenia pojawiają uprawnienia SDK w systemie Android.) Nie jest obecnie nie industry standard rozmiarów Android "przeglądu", ponieważ rozmiarów ekranu różnią się na wszystkich urządzeniach, ale obrazy 400 x 100 pracować nad niemal każde rozmiar ekranu.

### <a name="delivery-types"></a>Typy odbiorcy:
-    Wylogowywanie się z aplikacji tylko: otrzymają powiadomienie, gdy użytkownik nie korzysta z aplikacji.
- W nowym oknie aplikacji tylko powiadomienia o wymaga certyfikatu z jabłoń i Google (certyfikat APN lub GCM).
- W aplikacji tylko: powiadomienie jest wyświetlane tylko wtedy, gdy aplikacja jest uruchomiona.
- Powiadomienie jest używany system dostarczania Capptain do osiągnięcia użytkownika. W pełni można dostosować układ i wyświetlanie usługi wypychanych.
- W dowolnym momencie: Ta opcja zapewnia wysyłania powiadomień, które albo aplikacja jest uruchomiona lub nie.

 
![Dostęp do Campaign4][23]

### <a name="option-applies-to"></a>Opcja dotyczy:
- Powiadomienie: Anonsy, ankiet
 
## <a name="content"></a>Zawartość
Za pomocą sekcji zawartości do modyfikowania zawartości do ogłoszenia, ankiety, umieszcza danych oraz Kafelki (tylko dla Windows Phone). Ustawienie zawartości kampanii wypychanych jest zależna od typu kampanii. 

### <a name="see-also"></a>Zobacz też
- [Dokumentacja interfejsu użytkownika — osiągnięcia — Push zawartości][Link 29]
 
![Dostęp do Campaign5][24]

## <a name="audience"></a>Grupy odbiorców
Za pomocą sekcji odbiorcy do definiowania standardowe listy elementów, aby ograniczyć kampanii limity kampanii w oparciu o kryteria niestandardowe. Standardowy zestaw opcje służące do ograniczania odbiorców umożliwia push do nowych i starych lub tylko wypychanych macierzystych użytkowników. Można także ustawić przydziału, aby ograniczyć liczbę użytkowników, którzy otrzymają naciśnięcie. Możesz ręcznie edytować wyrażenie filtrowania kampanii do uwzględnienia kryterium jeden lub więcej użytkowników docelowych. Można ręcznie wpisać wyrażenie odbiorców. Wyrażenia takiego jawnie należy zdefiniować relacje między kryteria. Kryterium opisano przez identyfikator musi się zaczynać się wielką literą, która nie może zawierać spacji. Relacja między kryteria można przedstawić za pomocą "i", "lub", "nie" operatorów jak "(",")". Przykład: "Criterion1 lub (Criterion1 i nie Criterion2)".

> Uwaga: Dużej liczby odbiorców zawarte w kampanii, po stronie serwera kierowanie skanowania może być wolno, szczególnie w przypadku próby uruchomienia wielu kampanii w tym samym czasie.

- Jeśli to możliwe tylko jednej kampanii w chwili uruchomić.
- Co najwyżej tylko cztery kampanii w chwili uruchomić.
- Push tylko do aktywni użytkownicy (pole wyboru "uczestniczyć tylko użytkownicy, którzy mogą się z Tobą za pomocą natywnego wypychanych" i "Uczestniczyć tylko aktywni użytkownicy") tak, aby tylko do użytkowników, którzy nadal zainstalowaną aplikację i używać go muszą być przeszukiwane.
Po zdefiniowano odbiorców, możesz użyć przycisku simulate, aby dowiedzieć się, ile użytkownicy otrzymają ten wypychanych. Spowoduje to obliczyć liczbę wybranych użytkowników potencjalnie odwołuje odbiorców (jest szacowana na podstawie próbki losowe użytkowników). Należy pamiętać, użytkownicy, którzy mają odinstalować aplikację w ramach tej grupy odbiorców są również, że nie można się z Tobą.

### <a name="see-also"></a>Zobacz też
- [Nowe kryterium wypychanych dokumentacji - Reach - interfejs użytkownika][Link 28]

![Dostęp do Campaign6][25]

### <a name="edit-expression"></a>Edytowanie wyrażenia
![Dostęp do Campaign7][26]
 
### <a name="limit-your-audience-option-applies-to"></a>Limit dotyczy opcjach odbiorców:
- Prowadzenie tylko niektóre użytkowników: wszystkie (ogłoszenia, ankiety, umieszcza danych, Kafelki)
- Prowadzenie tylko użytkownicy: wszystkie (ogłoszeń, ankiety, umieszcza danych, Kafelki)
- Prowadzenie tylko nowych użytkowników: wszystkie (ogłoszenia, ankiety, umieszcza danych, Kafelki)
- Prowadzenie tylko użytkownicy bezczynne: anonsy, ankiety, Kafelki
- Prowadzenie tylko aktywnych użytkowników: wszystkie (ogłoszenia, ankiety, umieszcza danych, Kafelki)
- Prowadzenie tylko użytkownicy, którzy mogą się z Tobą za pomocą natywnego Push: anonsy, ankiet
 
## <a name="time-frame"></a>Przedział czasu
Za pomocą sekcji przedziału czasu można określić, kiedy będą wysyłane naciśnięcie lub puste przedział czasu od razu zacząć kampanii. Należy pamiętać, że przy użyciu strefy czasowej użytkowników końcowych może zostać uruchomiony kampanii dzień wcześniej niż oczekiwano dla użytkowników końcowych w Azji i wysyłanie małych partii umieszcza naraz, aż wszystkie strefy czasowe w świecie odpowiadać przedziału czasu Ustaw kampanii. Przy użyciu strefy czasowej użytkownicy końcowi mogą powodować opóźnienia w kampanii ponieważ ma żądania czas od telefonu przed rozpoczęciem naciśnięcie.

> Uwaga: Kampanii bez daty zakończenia może buforować umieszcza lokalnie i nadal je wyświetlić po ręcznie wykonania kampanii. Aby uniknąć tego problemu, określonego godziny zakończenia kampanii.

### <a name="see-also"></a>Zobacz też
- [Osiągnięcia — jak procedur — planowania][Link 3] 
 
![Dostęp do Campaign8][27]

### <a name="settings-apply-to"></a>Ustawienia mają zastosowanie do:
- Przedział czasu: anonsy, ankiety, kafelków
 
## <a name="test"></a>Test
Za pomocą sekcji testowych do wysłania tego wypychanych na urządzeniu testowych przed zapisaniem kampanii. Jeśli skonfigurowano wszelkie niestandardowe języków dla kampanii, możesz przetestować naciśnięcie w każdym języku. Możesz skonfigurować testowanie urządzenia z "Moje konto".
> Uwaga: Nie po stronie serwera, które dane są rejestrowane, korzystając z przycisku próba "" umieszcza, dane są tylko rejestrowane rzeczywistą wypychanych kampanii.

### <a name="see-also"></a>Zobacz też
- [Dokumentacja interfejsu użytkownika — Moje konto][Link 14]
 
![Dostęp do Campaign9][28]

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
 
