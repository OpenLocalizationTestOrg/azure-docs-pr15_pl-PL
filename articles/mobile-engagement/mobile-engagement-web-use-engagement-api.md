<properties
    pageTitle="Azure zaangażowania przenośnych Web interfejsy API SDK | Microsoft Azure"
    description="Najnowsze aktualizacje i procedury SDK sieci Web dla zaangażowania Mobile Azure"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="web"
    ms.devlang="js"
    ms.topic="article"
    ms.date="06/07/2016"
    ms.author="piyushjo" />

# <a name="use-the-azure-mobile-engagement-api-in-a-web-application"></a>W aplikacji sieci web za pomocą interfejsu API zaangażowania Mobile Azure

Ten dokument jest dodatkiem do dokumentu, który zawiera informację, jak [zintegrować zaangażowania Mobile w aplikacji sieci web](mobile-engagement-web-integrate-engagement.md). Umożliwia szczegółowo szczegóły dotyczące raportu statystyk aplikacji za pomocą interfejsu API zaangażowania Mobile Azure.

Interfejs API zaangażowania Mobile jest dostarczany przez `engagement.agent` obiektu. Domyślne Azure Mobile zaangażowania Web SDK alias jest `engagement`. Można zmienić tego aliasu z konfiguracji SDK.

## <a name="mobile-engagement-concepts"></a>Pojęcia zaangażowania Mobile

Następujące elementy Uściślij typowych [pojęcia zaangażowania Mobile](mobile-engagement-concepts.md) platformy sieci web.

### <a name="session-and-activity"></a>`Session`oraz`Activity`

Jeśli użytkownik pozostaje bezczynne więcej niż kilka sekund między dwa działania, użytkownika kolejność czynności jest podzielony na dwóch różnych sesji. Te kilka sekund są nazywane limit czasu sesji.

Aplikacji sieci web nie zadeklarować zakończenia działań użytkownika przez siebie (dzwoniąc `engagement.agent.endActivity` funkcja), na serwerze zaangażowania Mobile automatycznie wygaśnie sesję użytkownika w ciągu trzech minut po zamknięciu stronie aplikacji. Jest to limit czasu sesji serwera.

### `Crash`

Automatyczne raportów nie przechwycony wyjątków JavaScript nie są tworzone domyślnie. Jednak możesz zgłosić awarię ręcznie za pomocą `sendCrash` (zobacz sekcję na raportowanie awarii), funkcja.

## <a name="reporting-activities"></a>Tworzenia raportów

Raportowanie na aktywności użytkownika zawiera po uruchomieniu nowe działanie, a gdy użytkownik zakończy bieżących działań.

### <a name="user-starts-a-new-activity"></a>Użytkownik uruchamia nowe działanie

    engagement.agent.startActivity("MyUserActivity");

Musisz zadzwonić do `startActivity()` zmian w każdej aktywnością użytkownika czasu. Pierwsze wywołanie tej funkcji zaczyna się nową sesję użytkownika.

### <a name="user-ends-the-current-activity"></a>Użytkownik kończy bieżących działań.

    engagement.agent.endActivity();

Musisz zadzwonić do `endActivity()` co najmniej raz, kiedy użytkownik kończy ich ostatniego działania. Informuje SDK Web zaangażowania Mobile, że użytkownik jest obecnie bezczynne i że sesję użytkownika musi zostać zamknięty po wygaśnięciu limit czasu sesji. Jeśli zostanie wywołana `startActivity()` przed upływem limit czasu sesji, po prostu wznowienia sesji.

Ponieważ istnieje zaufanego składania po zamknięciu okna Nawigator, często jest trudne lub niemożliwe efektywnej zakończenia działań użytkownika w środowisku sieci web. Dlatego na serwerze zaangażowania Mobile automatycznie wygaśnie sesję użytkownika w ciągu trzech minut po zamknięciu stronie aplikacji.

## <a name="reporting-events"></a>Raportowanie zdarzeń

Raportowanie o zdarzeniach obejmuje zdarzeń sesji i zdarzeń autonomicznego.

### <a name="session-events"></a>Zdarzeń sesji

Zdarzeń sesji zwykle są używane do raportu akcje wykonywane przez użytkownika podczas sesji użytkownika.

**Przykład bez dodatkowych danych:**

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login');
      // [...]
    }

**Przykład: dodatkowe dane:**

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login', {user: 'alice'});
      // [...]
    }

### <a name="standalone-events"></a>Zdarzenia autonomicznego

W przeciwieństwie do zdarzeń sesji zdarzeń autonomicznego mogą występować poza kontekstem sesji.

W tym, za pomocą ``engagement.agent.sendEvent`` zamiast ``engagement.agent.sendSessionEvent``.

## <a name="reporting-errors"></a>Raportowanie błędów

Raportowania błędów obejmuje błędy sesji i autonomicznego błędy.

### <a name="session-errors"></a>Błędy sesji

Błędy sesji zazwyczaj są używane do raportowania błędów, które mają wpływ na użytkownika podczas sesji użytkownika.

**Przykład bez dodatkowych danych:**

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short');
      }
      // [...]
    }

**Przykład: dodatkowe dane:**

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short', {length: 4});
      }
      // [...]
    }

### <a name="standalone-errors"></a>Błędy autonomicznego

W przeciwieństwie do sesji błędy mogą wystąpić błędy autonomicznego poza kontekstem sesji.

W tym, za pomocą `engagement.agent.sendError` zamiast `engagement.agent.sendSessionError`.

## <a name="reporting-jobs"></a>Raportowanie zadań

Raportowanie na obejmuje zadania raportowania błędów i zdarzeń występujących podczas wykonywania zadania i raportowanie awarii.

**Przykład:**

Jeśli chcesz monitorować żądanie AJAX użyje następujące czynności:

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
      // [...]
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-errors-during-a-job"></a>Raportowanie błędów podczas wykonywania zadania

Błędy może dotyczyć uruchomionego zadania zamiast bieżącej sesji użytkownika.

**Przykład:**

Jeśli chcesz zgłosić błąd, jeśli żądanie AJAX kończy się niepowodzeniem:

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
        // [...]
        if (xhr.status == 0 || xhr.status >= 400) {
          engagement.agent.sendJobError('publish_xhr', 'publish', {status: xhr.status, statusText: xhr.statusText});
        }
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-events-during-a-job"></a>Raportowanie zdarzeń podczas wykonywania zadania

Zdarzenia może być związany z uruchomionego zadania zamiast bieżącej sesji użytkownika podziękowania dla `engagement.agent.sendJobEvent` funkcji.

Ta funkcja działa dokładnie tak jak `engagement.agent.sendJobError`.

### <a name="reporting-crashes"></a>Raportowanie awarii

Używanie `sendCrash` funkcji do raportu powoduje awarię ręcznie.

`crashid` Argument jest ciąg, który identyfikuje typ awarii.
`crash` Argument jest zazwyczaj śledzenia stosu dotyczące awarii w postaci ciągu.

    engagement.agent.sendCrash(crashid, crash);

## <a name="extra-parameters"></a>Dodatkowe parametry

Dowolne dane można dołączać do zdarzenia, błąd, aktywności lub zadania.

Dane można dowolnego obiektu JSON (ale nie tablicy lub typ pierwotny).

**Przykład:**

    var extras = {"video_id": 123, "ref_click": "http://foobar.com/blog"};
    engagement.agent.sendEvent("video_clicked", extras);

### <a name="limits"></a>Ograniczenia

Ograniczenia, które dotyczą dodatkowe parametry znajdują się w zakresie wyrażeń regularnych kluczy, typów wartości i rozmiar.

#### <a name="keys"></a>Klawisze

Każdy klucz w obiekcie musi być zgodna następujące wyrażenie:

    ^[a-zA-Z][a-zA-Z_0-9]*

Oznacza to, że klucze musi rozpoczynać się od litery co najmniej jeden, a po nim litery, cyfry lub znaki podkreślenia (\_).

#### <a name="values"></a>Wartości

Wartości są ograniczone do ciąg, liczba i typów logicznych.

#### <a name="size"></a>Rozmiar

Dodatki są ograniczone do 1024 znaków połączenia (po SDK Web zaangażowania Mobile kodowane w formacie JSON).

## <a name="reporting-application-information"></a>Raportowanie informacji o aplikacji

Możesz ręcznie zgłosić śledzenie informacji (lub inne informacje specyficzne dla aplikacji) za pomocą `sendAppInfo()` funkcji.

Zauważ, że te informacje, które mogą być wysyłane stopniowo. Tylko ostatnia wartość dla określonego klucza zostaną zachowane dla konkretnego urządzenia.

Jak dodatki zdarzenia umożliwia dowolnego obiektu JSON abstrakcyjne informacje o aplikacji. Należy zauważyć, że tablicami lub obiekty podrzędne są traktowane jako ciągi prostym (za pomocą szeregowania JSON).

**Przykład:**

Oto przykładowy kod do wysyłania płeć użytkownika i datę urodzenia:

    var appInfos = {"birthdate":"1983-12-07","gender":"female"};
    engagement.agent.sendAppInfo(appInfos);

### <a name="limits"></a>Ograniczenia

Ograniczenia, które dotyczą informacje o aplikacji znajdują się w zakresie wyrażeń regularnych kluczy i rozmiar.

#### <a name="keys"></a>Klawisze

Każdy klucz w obiekcie musi być zgodna następujące wyrażenie:

    ^[a-zA-Z][a-zA-Z_0-9]*

Oznacza to, że klucze musi rozpoczynać się od litery co najmniej jeden, a po nim litery, cyfry lub znaki podkreślenia (\_).

#### <a name="size"></a>Rozmiar

Informacje o aplikacji jest ograniczone do 1024 znaków połączenia (po SDK Web zaangażowania Mobile kodowane w formacie JSON).

W powyższym przykładzie JSON przesyłane do serwera jest 44 znaków:

    {"birthdate":"1983-12-07","gender":"female"}
