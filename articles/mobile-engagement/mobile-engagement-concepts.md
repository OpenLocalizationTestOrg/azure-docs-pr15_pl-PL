<properties
    pageTitle="Pojęcia zaangażowania Mobile | Microsoft Azure"
    description="Azure pojęcia zaangażowania Mobile"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="azure-mobile-engagement-concepts"></a>Azure pojęcia zaangażowania Mobile

Zaangażowania Mobile definiuje pojęcia kilka typowych wszystkich obsługiwanych platformach. W tym artykule opisano tych koncepcji.

Ten artykuł jest dobrym rozpoczęcia, jeśli jesteś nowym użytkownikiem zaangażowania Mobile. Upewnij się również do zapoznaj się z dokumentacją specyficzne dla platformy, którego używasz, jak będzie uściślić pojęcia opisane w tym artykule z więcej szczegółów i przykłady także możliwe ograniczenia.

## <a name="devices-and-users"></a>Urządzenia i użytkowników
Telefon komórkowy zaangażowania identyfikuje użytkowników za pomocą generuje unikatowy identyfikator dla każdego urządzenia. Ten identyfikator jest nazywany identyfikator urządzenia (lub `deviceid`). Jest generowany w taki sposób, aby wszystkie uruchomione aplikacje tego samego urządzenia miały ten sam identyfikator urządzenia.

Domyślnie oznacza, że jedno urządzenie, do której będzie należał dokładnie jeden użytkownik uważa zaangażowania Mobile, i w związku z tym użytkowników i urządzeń są równoważne pojęcia.

## <a name="sessions-and-activities"></a>Sesje i działania
Sesja jest jednym korzystanie z aplikacji przez użytkownika, od momentu użytkownik rozpoczyna się korzystania z niego, poczekaj, aż stopnie użytkownika.

Czynność jest jednym z zastosowań danej podrzędny część aplikacji wykonywane przez użytkowników (zwykle jest to ekranu, ale może być cokolwiek odpowiednie do aplikacji).

Użytkownika można wykonać tylko jedno działanie naraz.

Działanie jest identyfikowany przez nazwę (ograniczone do 64 znaków) i opcjonalnie można osadzić kilka dodatkowych danych (w limit 1024 bajtów).

Sesje są obliczane automatycznie z sekwencji czynności wykonywane przez użytkowników. Sesji zostanie uruchomiony, gdy użytkownik rozpoczyna się pierwszy działalność i przestaje po jego zakończeniu swojej ostatniego działania. Oznacza to, że sesji nie trzeba jawnie uruchomiona lub zatrzymana. Zamiast tego działania są wyraźnie uruchomiona lub zatrzymana. Jeśli działanie nie jest zgłoszony, jest zgłaszany żadnej sesji.

## <a name="events"></a>Zdarzenia
Zdarzenia są używane do raportu błyskawiczne akcje (na przykład naciśniętym przyciskiem lub artykuły przeczytać przez użytkowników).

Zdarzenie może być związany z bieżącej sesji do uruchomionego zadania lub może być zdarzeniem autonomicznego.

Zdarzenie jest identyfikowany przez nazwę (ograniczone do 64 znaków) i opcjonalnie można osadzić kilka dodatkowych danych (w limit 1024 bajtów).

## <a name="error"></a>Błąd
Błędy są używane do zgłaszać problemy poprawnie wykryte przez aplikację (na przykład akcje użytkownika niepoprawne lub błędy połączenia interfejsu API).

Komunikat o błędzie może być związany z bieżącej sesji do uruchomionego zadania lub może być błąd autonomicznego.

Komunikat o błędzie jest identyfikowany przez nazwę (ograniczone do 64 znaków) i opcjonalnie można osadzić kilka dodatkowych danych (w limit 1024 bajtów).

## <a name="job"></a>Zadanie
Zadania są używane do raportu akcje trwające (takich jak czas trwania interfejsu API, Wyświetl czasu reklam, czas trwania zadania w tle lub czasu trwania działania użytkownika).

Zadanie nie jest powiązany z sesją, ponieważ mogą być wykonywane zadania w tle, bez interakcji użytkownika.

Zadanie jest identyfikowany przez nazwę (ograniczone do 64 znaków) i opcjonalnie można osadzić kilka dodatkowych danych (w limit 1024 bajtów).

## <a name="crash"></a>Awarii
Awarie są automatycznie wydawane przez SDK zaangażowania Mobile raportowania błędów aplikacji, której problemów nie wykryto przez aplikację był awaria.

## <a name="application-information"></a>Informacje o aplikacji
Informacje o aplikacji (lub informacje o aplikacji) służy do użytkowników, oznacza to, że tag skojarzyć niektóre dane do użytkowników aplikacji (metoda ta przypomina pliki cookie sieci web, z wyjątkiem, że informacje o aplikacji jest przechowywany po stronie serwera na platformie Azure Mobile zaangażowania).

Informacje o aplikacji można rejestrować za pomocą interfejsu API zestawu SDK zaangażowania Mobile lub przy użyciu platformy zaangażowania Mobile interfejsu API urządzenia.

Informacje o aplikacji są skojarzone z urządzeniem para klucz/wartość. Klucz jest nazwą informacje o aplikacji (ograniczone do 64 ASCII litery [a-zA-Z], cyfry [0-9] i podkreślenia [_]). Wartość (ograniczone do 1024 znaków) może być ciąg, liczba całkowita, daty (RRRR MM-dd) lub wartość logiczną (PRAWDA lub FAŁSZ).

Dowolna liczba informacje o aplikacji może być skojarzone z urządzeniem w granicach zdefiniowanych przez telefon komórkowy zaangażowania cennik. Dla jednej klucz zaangażowania Mobile tylko rejestruje informacje o najnowszych zestawu wartości (nie Historia). Ustawianie lub zmienianie wartość informacje o aplikacji wymusza zaangażowania Mobile ponownie oceń odbiorców kryteriami ustawionymi na te informacje o aplikacji (jeśli istnieje), co oznacza tych informacji aplikacji może służyć do wyzwalanie umieszcza w czasie rzeczywistym.

## <a name="extra-data"></a>Dodatkowe dane
Dodatkowe dane (lub dodatki) to niektóre dane dowolnego może zostać dołączony do zdarzenia, błędy, działania i zadania.

Dodatki mają strukturę podobnie do obiektów JSON: są one drzewa par klucz wartość. Klucze są ograniczone do 64 ASCII litery [a-zA-Z], [0-9] liczb i podkreślenia [_]) i całkowity rozmiar dodatki jest ograniczona do 1024 znaków (raz zakodowany w formacie JSON przez telefon komórkowy SDK zaangażowania).

Całego drzewa par klucz wartość są przechowywane jako obiekt JSON. Jednak rozłożony bezpośrednio dostępności niektóre zaawansowane funkcje, takie jak segmentów jest tylko pierwszy poziom kluczy i wartości (na przykład można łatwo zdefiniować segmentu o nazwie "SciFi wentylatory", wprowadzone wszystkich użytkowników o wysyłane co najmniej 10 razy zdarzenia o nazwie "content_viewed" dodatkowe klucza "content_type" ustawiony na wartość "scifi" w ostatnim miesiącu). Dlatego zaleca wysyłania tylko dodatki składa się z proste listy par klucz wartość używanie skalarne wartości (na przykład ciągi, daty, liczby całkowite lub wartość logiczna).

## <a name="next-steps"></a>Następne kroki

- [Omówienie uniwersalny SDK systemu Windows Azure Mobile zaangażowania](mobile-engagement-windows-store-sdk-overview.md)
- [Omówienie systemu Windows Phone Silverlight SDK zaangażowania Mobile Azure](mobile-engagement-windows-phone-sdk-overview.md)
- [iOS SDK dla zaangażowania Mobile Azure](mobile-engagement-ios-sdk-overview.md)
- [Android SDK dla Azure zaangażowania urządzeń przenośnych](mobile-engagement-android-sdk-overview.md)
