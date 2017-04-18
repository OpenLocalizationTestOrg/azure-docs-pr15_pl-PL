<properties 
   pageTitle="Azure zaangażowania przenośnych Podręcznik — usługa rozwiązywania problemów" 
   description="Rozwiązywanie problemów z przewodniki dla Azure zaangażowania urządzeń przenośnych" 
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

# <a name="troubleshooting-guide-for-service-issues"></a>Rozwiązywanie problemów z przewodnika występują problemy z usługami

Poniżej przedstawiono możliwych problemów, które mogą wystąpić z działania zaangażowania Mobile Azure.

## <a name="service-outages"></a>Usługa dostawie

### <a name="issue"></a>Problem
- Problemy, które ma być spowodowany Azure Mobile zaangażowania usługi dostawie.

### <a name="causes"></a>Powoduje, że
- Problemy, które ma być spowodowany Azure Mobile zaangażowania usługi dostawie może być spowodowany kilka różnych problemów:
    - Są one odizolowane problemy, które pierwotnie są wyświetlane systemowych do wszystkich zaangażowania Mobile Azure
    - Znane problemy spowodowane przez serwer dostawie (nie zawsze pokazuje stan serwera):
    - Planowanie opóźnienia, określanie wartości docelowych błędy, znaczek aktualizacji problemy, Zatrzymaj statystyki zbierania wypychanych przestaje działać, interfejsu API pracy tabulatora, nowych aplikacji lub użytkowników nie można utworzyć, błędy DNS i błędy Przekroczono limit czasu w interfejsu użytkownika, interfejs API lub aplikacji na urządzeniu.
    - Chmura współzależności dostawie [Stan usługi Azure](http://status.azure.com/)
    - Dostawie zależności usług (obwodowym układzie Nerwowym) powiadomienia push
    - Sklep dostawie

1) Aby test, aby sprawdzić, czy problem jest systemowe można przetestować tę samą funkcję z innego
   
   - Azure zaangażowania Mobile zintegrowanych aplikacji
   - Testowanie urządzenia
   - Testowanie urządzenia z systemem operacyjnym wersji
   - Kampanii
   - Konta administratora
   - Przeglądarki (IE, Firefox, Chrome, itp.)
   - Komputer

2) Aby sprawdzić, czy problem dotyczy tylko interfejsu użytkownika lub interfejsu API:

   - Przetestować tę samą funkcję z interfejsu użytkownika zaangażowania Mobile Azure i API zaangażowania Mobile Azure.

3) Aby sprawdzić, czy problem dotyczy sieci telefon komórkowy:

   - Testowanie podczas połączenia z Internetem za pośrednictwem sieci Wi-Fi i nawiązaniu połączenia przez sieć komórkowa 3G.
   - Upewnij się, że Zapora nie blokuje adresów IP zaangażowania Mobile Azure lub porty.

4) Aby sprawdzić, czy problem dotyczy urządzenia:

   - Sprawdzenia, czy urządzenie jest możliwość łączenia się zaangażowania Mobile Azure z innej aplikacji zintegrowane zaangażowania Mobile Azure.
   - Test, który istnieje możliwość wygenerowania zdarzenia, zadań i awarii za pomocą telefonu, którą można wyświetlić w interfejsie użytkownika zaangażowania Mobile Azure. 
   - Sprawdzenia, czy możesz wysłać powiadomienia wypychane z interfejsu użytkownika zaangażowania Mobile Azure urządzenia oparte na jej identyfikator urządzenia. 

5) Aby sprawdzić, czy problem dotyczy aplikacji:

   - Instalowanie i testowanie aplikacji emulatora zamiast z fizycznie urządzenia:
   
6) Aby sprawdzić, czy problem dotyczy uaktualnienia systemu operacyjnego do użytkowników końcowych urządzeń, które wymagają aktualizacji SDK rozwiązania:

   - Testowanie aplikacji na różnych urządzeniach z różnymi wersjami systemu operacyjnego.
   - Upewnij się, że używasz najnowszej wersji zestawu SDK.
 
## <a name="connectivity-and-incorrect-information-issues"></a>Łączność i problemów niepoprawne informacje

### <a name="issue"></a>Problem
- Logowanie do interfejsu użytkownika zaangażowania Mobile Azure problemy.
- Błędy połączeń z API zaangażowania Mobile Azure.
- Problemy z przekazywaniem znaczniki informacje o aplikacji przy użyciu interfejsu API urządzenia.
- Problemów z pobieraniem dzienniki lub wyeksportowane dane z zaangażowania Mobile Azure.
- Niepoprawne informacje wyświetlane w interfejsie użytkownika zaangażowania Mobile Azure.
- Niepoprawne informacje wyświetlane w dziennikach zaangażowania Mobile Azure.

### <a name="causes"></a>Powoduje, że
* Upewnij się, że Twoje konto użytkownika ma wystarczających uprawnień do wykonania zadania.
* Upewnij się, że problem dotyczy tylko nie jednego komputera lub sieci lokalnej.
* Upewnij się, że nie zawiera usługi Azure Mobile zaangażowania zgłoszone dostawie.
* Upewnij się, plików znacznika informacje o aplikacji wykonaj wszystkie te reguły:
    - Użyj tylko zestaw znaków UTF8 (zestawu znaków ANSI nie jest obsługiwany).
    - Użyj przecinka "," jako znak separatora (możesz otworzyć usługę do żądania Aby zmienić znak separatora CSV z przecinkiem "," do innego znaków, takich jak średnik ";").
    - "True" i "false", użyj wszystkie małe litery dla wartości logicznych.
    - Za pomocą pliku, która jest mniejsza niż maksymalny rozmiar pliku 35 MB.
 
