<properties 
   pageTitle="Azure zaangażowania przenośnych Podręcznik - wypychanych Reach rozwiązywania problemów" 
   description="Rozwiązywanie problemów z użytkownika interakcji i powiadomień w zaangażowania Mobile Azure" 
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

# <a name="troubleshooting-guide-for-push-and-reach-issues"></a>Rozwiązywanie problemów z przewodnika dotyczącego Push i osiągnięcia problemów

Poniżej przedstawiono możliwe problemy, które mogą się pojawić w sposób zaangażowania Mobile Azure przesyła informacje do użytkowników.
 
## <a name="push-failures"></a>Powiadomienia push błędy

### <a name="issue"></a>Problem
- Umieszcza nie działa (w aplikacji, poza aplikacji lub oba).

### <a name="causes"></a>Powoduje, że
- Wielokrotnie awarii wypychanych jest wskazanie, że zaangażowania Mobile Azure, Reach lub inną funkcją zaawansowane zatrudnienia Mobile Azure nie jest poprawnie zintegrowany lub że uaktualnienie jest wymagane w zestawie SDK naprawy znany problem w nowej platformy systemu operacyjnego lub urządzenia.
- Testowanie tylko wypychanych w aplikacji i po prostu naciśnij z aplikacji, aby określić, czy jest coś problem w aplikacji lub z aplikacji.
- Przeprowadź test z interfejsu użytkownika i API w ramach rozwiązywania problemów, aby sprawdzić, jakie dodatkowe informacje o błędzie jest dostępna obu tych miejscach.
- Wylogowywanie się z aplikacji umieszcza nie będą działać, o ile zarówno zaangażowania Mobile Azure i Reach zostały zintegrowane w zestawie SDK.
- Umieszcza nie będzie działać, certyfikaty nie są prawidłowe lub używają ZLECENIE a deweloperów poprawnie (tylko system iOS). (**Uwaga:** "poza aplikację" powiadomienia wypychane nie można dostarczać w systemie iOS, jeśli masz rozwoju (deweloperów) i wersji produkcji (ZLECENIE) zainstalowane na tym samym urządzeniu, ponieważ tokenu zabezpieczającego skojarzone z certyfikatu aplikacji może być unieważniane przez firmy Apple. Aby rozwiązać ten problem, odinstaluj deweloperów i ZLECENIE wersje aplikacji i ponownie zainstalować tylko jedną wersję na urządzeniu.)
- Wylogowywanie się z aplikacji wypychanych liczone są obsługiwane inaczej w różnych platform (iOS zawiera informacje o mniej niż Android wyłączenie natywnych umieszcza na urządzeniu, API mogą dostarczyć więcej informacji niż interfejs użytkownika na statystykę push).
- Wylogowywanie się z aplikacji umieszcza może zostać zablokowana przez klientów na poziomie systemu operacyjnego (iOS i Android).
- Wylogowywanie się z aplikacji umieszcza będą wyświetlane jako wyłączona w interfejsie użytkownika zaangażowania Mobile Azure, jeśli nie są poprawnie zintegrowanego, ale mogą nie działać w trybie cichym z interfejsu API.
- W aplikacji umieszcza nie będą działać, o ile zarówno zaangażowania Mobile Azure i Reach zostały zintegrowane w zestawie SDK.
- Umieszcza GCM i ADM nie będą działać, o ile zaangażowania Mobile Azure i określonego serwera zostały zintegrowane w SDK (tylko system Android).
- W aplikacji i z aplikacji umieszcza należy przetestować oddzielnie, aby ustalić, czy jest wypychanych lub Reach problem.
- W aplikacji, którą umieszcza wymagają aplikacji Otwórz, aby otrzymać.
- W aplikacji umieszcza są często instalację, aby filtrować według tag informacje o aplikacji opcjonalnych lub rezygnacji.
- Jeśli za pomocą kategorii niestandardowej w Reach wyświetlania powiadomień w aplikacji, należy wykonać poprawne cyklu powiadomienie, inaczej nie można deklarować powiadomienie gdy użytkownik je zamknąć.
- Jeśli rozpoczynasz kampanii z Brak daty zakończenia i urządzenie odebranych w aplikacji powiadomienie, ale ma nie wyświetlania go jeszcze nadal będą użytkownika otrzymywać powiadomienie przy następnym zalogowaniu do aplikacji, nawet jeśli ręcznie zakończyć kampanii.
- W przypadku problemów z interfejsem API Push upewnij się, że naprawdę chcesz (ponieważ interfejsu API osiągnięcia jest używana częściej) za pomocą interfejsu API Push zamiast interfejsu API osiągnięcia i że nie są mylące parametrów "ładunek" i "powiadamiający".
- Testowanie kampanii wypychanych z obu połączonych za pomocą sieci Wi-Fi i 3G, aby wyeliminować połączenie sieciowe jako źródła możliwych problemów z urządzeniem.

## <a name="push-testing"></a>Testowanie powiadomienia push

### <a name="issue"></a>Problem
- Umieszcza mogą być wysyłane do konkretnego urządzenia według identyfikatorem urządzenia.

### <a name="causes"></a>Powoduje, że

- Testowanie urządzenia są ustawienia inaczej dla każdej z tych platform, ale powoduje zdarzenia w aplikacji na urządzeniu test i szukasz swój identyfikator urządzenia w portalu powinna działać znaleźć swój identyfikator urządzenia dla wszystkich platform.
- Testowanie urządzenia działają inaczej z IDFA a IDFV (tylko system iOS).


## <a name="push-customization"></a>Dostosowywanie push

### <a name="issue"></a>Problem
- Zaawansowane wypychanych zawartości elementu nie będzie działać (identyfikatora, zadzwoń, wibracje, obraz, itd.).
- Łącza z umieszcza nie działa (poza aplikacji w aplikacji do witryny sieci Web, do określonej lokalizacji w aplikacji).
- Statystyki wypychanych wskazują, że wypychane nie została wysłana do dowolną liczbę osób, zgodnie z oczekiwaniami, (za dużo lub za mało).
- Zduplikowane i otrzymano dwa razy.
- Nie można zarejestrować testowanie urządzenia dla Azure Mobile zaangażowania umieszcza (z własnych zlecenie lub deweloperów aplikacji).

### <a name="causes"></a>Powoduje, że

- Aby utworzyć łącze do określonej lokalizacji w aplikacji wymaga "kategorie" (tylko system Android).
- Głębokości schematy łączenia aby przekierować użytkowników do innej lokalizacji po kliknięciu przycisku powiadomień wypychanych muszą być utworzone w i zarządzane przez system operacyjny urządzenia nie przez telefon komórkowy zaangażowania i aplikacji bezpośrednio. (**Uwaga:** się aplikacji powiadomienia nie można połączyć bezpośrednio do w lokalizacjach aplikacji z systemem iOS jak w przypadku systemu Android.)
- Serwery obrazu zewnętrznego, konieczne będzie mógł korzystać z protokołu HTTP "GET" i "Głowy" do przeglądu umieszcza pracy (tylko system Android).
- W kodzie można wyłączyć agenta zaangażowania Mobile Azure, po otwarciu klawiatury i kod ponownie aktywować agenta zaangażowania Mobile Azure po zamknięciu klawiatury, tak aby klawiatury nie wpływają na wygląd usługi powiadomień (tylko system iOS).
- Niektóre elementy nie działają w symulacji test, ale tylko rzeczywistą kampanii (identyfikatora, zadzwoń, wibracje, obraz, itd.).
- Nie danych po stronie serwera jest rejestrowany, gdy "Testowanie" umieszcza za pomocą przycisku. Dane są rejestrowane tylko wypychanych rzeczywistą kampanii.
- Aby wyizolować problem, rozwiązywanie problemów z: test, symulacji, a rzeczywistą kampanii, ponieważ każdy działają nieco inaczej.
- Czas, który usługi "w aplikacji" i "dowolny time" kampanie zaplanowane może mieć wpływ liczb dostarczenia ponieważ kampanii będą dostarczane tylko do użytkowników, którzy są "w aplikacji", gdy jest uruchomiony kampanii (i użytkowników, którzy mają ich urządzenia ustawienia powiadomień "poza aplikację").
- Różnice między sposób obsługi Android oraz iOS poza powiadomienia aplikacji utrudnia bezpośrednio porównywanie wypychanych danych statystycznych dotyczących Android oraz iOS wersję aplikacji. Android znajdziesz więcej informacji poziomu powiadomienie z systemem operacyjnym niż iOS. Android raportów, gdy odebrane, kliknięciu lub usunięte w Centrum powiadomienie natywnych powiadomienie, ale iOS nie zgłasza te informacje, o ile nie zostanie kliknięty powiadomienie. 
- Przyczyna głównym "barki" liczb różnią się od innego niż "dostarczona" numery dla osiągnięcia kampanii jest że "w aplikacji" i "poza aplikację" powiadomienia zliczane są inaczej. "W aplikacji" powiadomienia są obsługiwane przez telefon komórkowy zaangażowania, ale "poza aplikację" powiadomienia są obsługiwane przez Centrum powiadomienie w systemie operacyjnym urządzenia.

## <a name="push-targeting"></a>Kierowanie powiadomienia push

### <a name="issue"></a>Problem
- Wbudowana kierowanie nie działa zgodnie z oczekiwaniami.
- Kierowanie znacznika informacje o aplikacji nie działa zgodnie z oczekiwaniami.
- Kierowanie Geo lokalizacji nie działa zgodnie z oczekiwaniami.
- Opcje językowe nie działają zgodnie z oczekiwaniami.

### <a name="causes"></a>Powoduje, że

- Upewnij się, że zostały przekazane znaczniki informacje o aplikacji za pośrednictwem interfejsu użytkownika zaangażowania Mobile Azure lub interfejsu API.
- Ograniczanie szybkości wypychanych lub przydział wypychanych na poziomie aplikacji lub ograniczanie odbiorców na poziomie kampanii zapobiec osoby otrzymywania określonych wypychanych, nawet jeśli spełniają podane kryteria określania docelowych. 
- Ustawienie "Język" jest inny niż kierowanie według kraju lub ustawienia regionalne, które jest również inne niż kierowanie na podstawie lokalizacji Geo na podstawie lokalizacji telefonu lub GPS lokalizacji.
- Wiadomość w języku"domyślne" jest wysyłana do dowolnego klienta, który nie ma urządzenia ustaw jedną zadanej języki alternatywne.


## <a name="push-scheduling"></a>Powiadomienia push planowania

### <a name="issue"></a>Problem
- Planowanie wypychane nie działa zgodnie z oczekiwaniami (wysyłane za wcześniejsze lub opóźnione).

### <a name="causes"></a>Powoduje, że

- Strefy czasowe mogą problemy dotyczące planowania, zwłaszcza w przypadku, gdy przy użyciu strefy czasowej przez użytkowników końcowych.
- Funkcje zaawansowane wypychanych może opóźnić umieszcza.
- Kierowanie według telefonu, którego ustawienia (zamiast znaczników informacje o aplikacji) może opóźnić umieszcza od zaangażowania Mobile Azure może być konieczne zażądać danych w czasie rzeczywistym telefonu przed wysłaniem wypychania.
- Kampanie utworzone bez daty końcowej przechowywać naciśnięcie lokalnie na urządzeniu i pokazać przy następnym otwarciu aplikacji nawet wtedy, gdy kampanii ręcznie zostanie zakończone.
- Uruchamianie w tym samym czasie więcej niż jednej kampanii może potrwać dłużej skanowanie bazy użytkowników (spróbuj tylko rozpoczynać jednej kampanii jednocześnie maksymalnie czterech, również docelowej tylko dla aktywnego użytkowników tak, aby użytkownicy nie muszą być przeszukiwane).
- Opcja "Odbiorców Ignoruj wypychanych będą wysyłane do użytkowników za pośrednictwem interfejsu API" w sekcji "Kampanii" kampanii Reach kampanii nie zostanie automatycznie wysłany, będzie konieczne ręczne Wyślij go przy użyciu interfejsu API osiągnięcia.
- Jeśli za pomocą kategorii niestandardowej w Reach wyświetlania powiadomień w aplikacji, należy wykonać poprawne cyklu powiadomienie, inaczej nie można deklarować powiadomienie gdy użytkownik je zamknąć.

 
