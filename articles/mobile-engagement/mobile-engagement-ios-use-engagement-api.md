<properties
    pageTitle="Jak korzystać z interfejsu API zaangażowania na iOS"
    description="IOS najnowszą SDK — jak za pomocą interfejsu API zaangażowania systemie iOS"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />


#<a name="how-to-use-the-engagement-api-on-ios"></a>Jak korzystać z interfejsu API zaangażowania na iOS

Ten dokument jest dodatkiem do dokumentu jak zintegrować zaangażowania systemie iOS: znajdują się w głębokości szczegóły dotyczące raportu statystyk aplikacji za pomocą interfejsu API zaangażowania.

Należy pamiętać, że jeśli chcesz tylko zaangażowania raportowania aplikacji sesje, działania, awarii i informacje techniczne, następnie Najłatwiejszym sposobem jest aby wszystkie niestandardowe `UIViewController` obiekty dziedziczyć odpowiadający mu `EngagementViewController` zajęć.

Jeśli chcesz uzyskać więcej informacji, na przykład jeśli chcesz zgłosić zdarzenia określonej aplikacji, błędów i zadania, lub masz raportowania aplikacji działania w inny sposób niż to, realizowane w `EngagementViewController` klasy, należy użyć interfejsu API zaangażowania.

Interfejs API zaangażowania jest dostarczany przez `EngagementAgent` zajęć. Wystąpienia tej klasy mogą być pobierane, dzwoniąc `[EngagementAgent shared]` metoda statyczna (należy zauważyć, że `EngagementAgent` obiektu, zwracany jest pojedyncza).

Przed wezwań interfejsu API `EngagementAgent` obiekt musi zostać zainicjowany przez wywołanie metody`[EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];`

##<a name="engagement-concepts"></a>Pojęcia zaangażowania

Następujące elementy Uściślij typowych [Pojęcia zaangażowania Mobile](mobile-engagement-concepts.md) platformy systemu iOS.

### <a name="session-and-activity"></a>`Session`oraz`Activity`

*Działanie* jest zwykle skojarzony jeden ekran aplikacji, to znaczy *aktywności* uruchamianego po ekranie zostanie wyświetlona i zatrzymuje się po zamknięciu ekranu: jest to wielkość liter w przypadku SDK zaangażowania jest zintegrowany przy użyciu `EngagementViewController` klasy.

Ale *działania* można sterować także ręcznie za pomocą interfejsu API zaangażowania. Dzięki temu dzielenie danego ekranu w kilku części sub, aby uzyskać więcej informacji na temat zastosowania tego ekranu (na przykład do znanych częstotliwości i jak długo trwa okien dialogowych są używane wewnątrz tego ekranu).

##<a name="reporting-activities"></a>Tworzenia raportów

### <a name="user-starts-a-new-activity"></a>Użytkownik uruchamia nowe działanie

            [[EngagementAgent shared] startActivity:@"MyUserActivity" extras:nil];

Musisz zadzwonić do `startActivity()` każdej zmianie aktywności użytkownika. Pierwsze wywołanie tej funkcji zaczyna się nową sesję użytkownika.

### <a name="user-ends-his-current-activity"></a>Użytkownik kończy swoich bieżących działań.

            [[EngagementAgent shared] endActivity];

> [AZURE.WARNING] Możesz powinien **nigdy** wywołanie tej funkcji przez siebie, chyba że chcesz podzielić jeden stosowania aplikację na kilka sesji: połączenia do tej funkcji może zakończyć natychmiast bieżącej sesji tak, kolejne połączenia do `startActivity()` czy uruchomić nową sesję. Ta funkcja jest automatycznie wywołana przez zestaw SDK, gdy aplikacja jest zamknięty.

##<a name="reporting-events"></a>Raportowanie zdarzeń

### <a name="session-events"></a>Zdarzeń sesji

Zdarzeń sesji zwykle są używane do raportu akcje wykonywane przez użytkownika podczas tej sesji.

**Przykład bez dodatkowych danych:**

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:nil];
            ...
       }
       [...]
    }

**Przykład: dodatkowe dane:**

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        NSMutableDictionary* extras = [NSMutableDictionary dictionary];
        [extras setObject:[NSNumber numberWithInt:toInterfaceOrientation] forKey:@"to_orientation_id"];
        [extras setObject:[NSNumber numberWithDouble:duration] forKey:@"duration"];
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:extras];
            ...
       }
       [...]
    }

### <a name="standalone-events"></a>Zdarzenia autonomicznego

W przeciwieństwie do zdarzeń sesji zdarzeń autonomicznego mogą być używane poza kontekstem sesji.

**Przykład:**

    [[EngagementAgent shared] sendEvent:@"received_notification" extras:nil];

##<a name="reporting-errors"></a>Raportowanie błędów

### <a name="session-errors"></a>Błędy sesji

Błędy sesji zwykle są używane do raportować wpływające na ochronę użytkownika podczas tej sesji.

**Przykład:**

    /** The user has entered invalid data in a form */
    @implementation MyViewController {
      [...]
      -(void)onMyFormSubmitted:(MyForm*)form {
        [...]
        /* The user has entered an invalid email address */
        [[EngagementAgent shared] sendSessionError:@"sign_up_email" extras:nil]
        [...]
      }
      [...]
    }

### <a name="standalone-errors"></a>Błędy autonomicznego

W przeciwieństwie do sesji błędy błędy autonomicznego mogą być używane poza kontekstem sesji.

**Przykład:**

    [[EngagementAgent shared] sendError:@"something_failed" extras:nil];

##<a name="reporting-jobs"></a>Raportowanie zadań

**Przykład:**

Załóżmy, że chcesz zgłosić czas trwania procesu logowania:

    [...]
    -(void)signIn
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      [... sign in ...]

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    }
    [...]

### <a name="report-errors-during-a-job"></a>Raportowanie błędów podczas wykonywania zadania

Błędy może dotyczyć uruchomione zadanie, a nie jest powiązany bieżącej sesji użytkownika.

**Przykład:**

Załóżmy, że chcesz zgłosić błąd podczas procesu logowania:

    [...]
    -(void)signin
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      BOOL success = NO;
      while (!success) {
        /* Try to sign in */
        NSError* error = nil;
        [self trySigin:&error];
        success = error == nil;

        /* If an error occured report it */
        if(!success)
        {
          [[EngagementAgent shared] sendJobError:@"sign_in_error"
                         jobName:@"sign_in"
                          extras:[NSDictionary dictionaryWithObject:[error localizedDescription] forKey:@"error"]];

          /* Retry after a moment */
          [NSThread sleepForTimeInterval:20];
        }
      }

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    };
    [...]

### <a name="events-during-a-job"></a>Zdarzenia podczas wykonywania zadania

Zdarzenia może dotyczyć uruchomione zadanie, a nie jest powiązany bieżącej sesji użytkownika.

**Przykład:**

Załóżmy, że mamy sieci społecznościowej, firma Microsoft korzysta zadanie do raportu całkowity czas, w którym użytkownik jest połączony z serwerem. Użytkownik może odbierać wiadomości z jego znajomych, to zdarzenie zadania.

    [...]
    - (void) signin
    {
      [...Sign in code...]
      [[EngagementAgent shared] startJob:@"connection" extras:nil];
    }
    [...]
    - (void) signout
    {
      [...Sign out code...]
      [[EngagementAgent shared] endJob:@"connection"];
    }
    [...]
    - (void) onMessageReceived
    {
      [...Notify user...]
      [[EngagementAgent shared] sendJobEvent:@"connection" jobName:@"message_received" extras:nil];
    }
    [...]

##<a name="extra-parameters"></a>Dodatkowe parametry

Dowolne dane może zostać dołączony do zdarzenia, błędy, działania i zadania.

Te dane mogą być zorganizowane, używa klasy NSDictionary w systemie iOS.

Należy zauważyć, że dodatki może zawierać `arrays(NSArray, NSMutableArray)`, `numbers(NSNumber class)`, `strings(NSString, NSMutableString)`, `urls(NSURL)`, `data(NSData, NSMutableData)` lub innych `NSDictionary` wystąpienia.

> [AZURE.NOTE] Dodatkowy parametr jest seryjny w formacie JSON. Jeśli chcesz przekazać różnych obiektów niż opisane powyżej, należy zaimplementować poniższej metody w klasie:
>
             -(NSString*)JSONRepresentation;
>
> Metoda nie powinna zwracać reprezentację JSON obiektu.

### <a name="example"></a>Przykład

    NSMutableDictionary* extras = [NSMutableDictionary dictionaryWithCapacity:2];
    [extras setObject:[NSNumber numberWithInt:123] forKey:@"video_id"];
    [extras setObject:@"http://foobar.com/blog" forKey:@"ref_click"];
    [[EngagementAgent shared] sendEvent:@"video_clicked" extras:extras];

### <a name="limits"></a>Ograniczenia

#### <a name="keys"></a>Klawisze

Każdy klucz w `NSDictionary` musi odpowiadać na następujące wyrażenie:

`^[a-zA-Z][a-zA-Z_0-9]*`

Oznacza to, że klawisze musi się zaczynać co najmniej jeden list, a po nim litery, cyfry i podkreślenia (\_).

#### <a name="size"></a>Rozmiar

Dodatki są ograniczone do **1024** znaków połączenia (raz zakodowany w formacie JSON przez agenta zaangażowania).

W poprzednim przykładzie JSON przesyłane do serwera jest 58 znaków:

    {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

##<a name="reporting-application-information"></a>Raportowanie informacji o aplikacji

Możesz ręcznie zgłosić śledzenie informacji (lub innych aplikacji szczegółowych informacji) za pomocą `sendAppInfo:` funkcji.

Należy zauważyć, że te informacje, które mogą być wysyłane stopniowo: tylko najnowsze wartość w polu klucz zostaną zachowane dla danego urządzenia.

Dodatki zdarzenia, takie jak `NSDictionary` klasa jest używana do abstrakcyjne informacje o aplikacji, należy zauważyć, że tablicami lub podrzędny słowniki jest traktowana jako ciągi prostym (za pomocą szeregowania JSON).

**Przykład:**

    NSMutableDictionary* appInfo = [NSMutableDictionary dictionaryWithCapacity:2];
    [appInfo setObject:@"female" forKey:@"gender"];
    [appInfo setObject:@"1983-12-07" forKey:@"birthdate"]; // December 7th 1983
    [[EngagementAgent shared] sendAppInfo:appInfo];

### <a name="limits"></a>Ograniczenia

#### <a name="keys"></a>Klawisze

Każdy klucz w `NSDictionary` musi odpowiadać na następujące wyrażenie:

`^[a-zA-Z][a-zA-Z_0-9]*`

Oznacza to, że klawisze musi się zaczynać co najmniej jeden list, a po nim litery, cyfry i podkreślenia (\_).

#### <a name="size"></a>Rozmiar

Informacje o aplikacji są ograniczone do **1024** znaków połączenia (raz zakodowany w formacie JSON przez agenta zaangażowania).

W poprzednim przykładzie JSON przesyłane do serwera jest 44 znaków:

    {"birthdate":"1983-12-07","gender":"female"}
