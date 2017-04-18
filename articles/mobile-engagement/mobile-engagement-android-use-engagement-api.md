<properties
    pageTitle="Jak używać zaangażowania interfejsu API w systemie Android"
    description="Najnowsze SDK systemu Android — jak za pomocą zaangażowania interfejsu API w systemie Android"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="piyushjo;ricksal" />

#<a name="how-to-use-the-engagement-api-on-android"></a>Jak używać zaangażowania interfejsu API w systemie Android

Ten dokument jest dodatkiem do dokumentu, [Opcje zaawansowane raportowania dla systemu Android SDK zaangażowania Mobile](mobile-engagement-android-advanced-reporting.md). Zapewnia on głębokości szczegóły dotyczące raportu statystyk aplikacji za pomocą interfejsu API zaangażowania.

Należy pamiętać, że jeśli chcesz tylko zaangażowania raportowania aplikacji sesje, działania, awarii i informacje techniczne, następnie Najłatwiejszym sposobem jest wyświetlanie wszystkich usługi `Activity` klasy podrzędne dziedziczą odpowiadający mu `EngagementActivity` zajęć.

Jeśli chcesz uzyskać więcej informacji, na przykład jeśli chcesz zgłosić zdarzenia określonej aplikacji, błędów i zadania, lub masz raportowania aplikacji działania w inny sposób niż to, realizowane w `EngagementActivity` klasy, należy użyć interfejsu API zaangażowania.

Interfejs API zaangażowania jest dostarczany przez `EngagementAgent` zajęć. Wystąpienia tej klasy mogą być pobierane, dzwoniąc `EngagementAgent.getInstance(Context)` metoda statyczna (należy zauważyć, że `EngagementAgent` obiektu, zwracany jest pojedyncza).

##<a name="engagement-concepts"></a>Pojęcia zaangażowania

Następujące elementy Uściślij typowych [Mobile zaangażowania pojęcia](mobile-engagement-concepts.md), platformy systemu Android.

### <a name="session-and-activity"></a>`Session`oraz`Activity`

Jeśli użytkownik pozostaje więcej niż kilka sekund bezczynności między dwa *działania*, następnie jego sekwencji *czynności* podzielono na dwa różne *Sesje*. Te kilka sekund są nazywane "limit czasu sesji".

*Działanie* jest zwykle skojarzony jeden ekran aplikacji, to znaczy *aktywności* uruchamianego po ekranie zostanie wyświetlona i zatrzymuje się po zamknięciu ekranu: jest to wielkość liter w przypadku SDK zaangażowania jest zintegrowany przy użyciu `EngagementActivity` klasy.

Ale *działania* można sterować także ręcznie za pomocą interfejsu API zaangażowania. Dzięki temu dzielenie danego ekranu w kilku części sub, aby uzyskać więcej informacji na temat zastosowania tego ekranu (na przykład do znanych częstotliwości i jak długo trwa okien dialogowych są używane wewnątrz tego ekranu).

##<a name="reporting-activities"></a>Tworzenia raportów

> [AZURE.IMPORTANT] Nie musisz raportu działań, takich jak opisane w tej sekcji, jeśli korzystasz z `EngagementActivity` zajęć i jego odmiany zgodnie z opisem w temacie jak zintegrować zaangażowania w dokumencie Android.

### <a name="user-starts-a-new-activity"></a>Użytkownik uruchamia nowe działanie

            EngagementAgent.getInstance(this).startActivity(this, "MyUserActivity", null);
            // Passing the current activity is required for Reach to display in-app notifications, passing null will postpone such announcements and polls.

Musisz zadzwonić do `startActivity()` każdej zmianie aktywności użytkownika. Pierwsze wywołanie tej funkcji zaczyna się nową sesję użytkownika.

Najlepiej połączeń ta funkcja znajduje się na każdej czynności `onResume` zwrotnego.

### <a name="user-ends-his-current-activity"></a>Użytkownik kończy swoich bieżących działań.

            EngagementAgent.getInstance(this).endActivity();

Musisz zadzwonić do `endActivity()` co najmniej raz, gdy użytkownik kończy swojej ostatniego działania. To SDK zaangażowania informuje, że użytkownik jest obecnie bezczynne i wygaśnie sesję użytkownika muszą być zamknięte po limit czasu sesji (Jeśli zostanie wywołana `startActivity()` przed upływem limit czasu sesji, po prostu wznowienia sesji).

Najlepiej połączeń ta funkcja znajduje się na każdej czynności `onPause` zwrotnego.

##<a name="reporting-events"></a>Raportowanie zdarzeń

### <a name="session-events"></a>Zdarzeń sesji

Zdarzeń sesji zwykle są używane do raportu akcje wykonywane przez użytkownika podczas tej sesji.

**Przykład bez dodatkowych danych:**

            public MyActivity extends EngagementActivity {
               [...]
               @Override
               public boolean onPrepareOptionsMenu(Menu menu) {
                  getEngagementAgent().sendSessionEvent("menu_shown", null);
               }
               [...]
            }

**Przykład: dodatkowe dane:**

            public MyActivity extends EngagementActivity {
              [...]
              @Override
              public boolean onMenuItemSelected(int featureId, MenuItem item) {
                Bundle extras = new Bundle();
                extras.putInt("id", item.getItemId());
                getEngagementAgent().sendSessionEvent("menu_selected", extras);
              }
              [...]
            }

### <a name="standalone-events"></a>Zdarzenia autonomicznego

W przeciwieństwie do zdarzeń sesji zdarzeń autonomicznego mogą występować poza kontekstem sesji.

**Przykład:**

Załóżmy, że chcesz zgłaszania zdarzeń występujących podczas emisji odbiorcy zostanie wywołana:

            /** Triggered by Intent.ACTION_BATTERY_LOW */
            public BatteryLowReceiver extends BroadcastReceiver {
              [...]
              @Override
              public void onReceive(Context context, Intent intent) {
                EngagementAgent.getInstance(context).sendEvent("battery_low", null);
              }
              [...]
            }

##<a name="reporting-errors"></a>Raportowanie błędów

### <a name="session-errors"></a>Błędy sesji

Błędy sesji zwykle są używane do raportować wpływające na ochronę użytkownika podczas tej sesji.

**Przykład:**

            /** The user has entered invalid data in a form */
            public MyActivity extends EngagementActivity {
              [...]
              public void onMyFormSubmitted(MyForm form) {
                [...]
                /* The user has entered an invalid email address */
                getEngagementAgent().sendSessionError("sign_up_email", null);
                [...]
              }
              [...]
            }

### <a name="standalone-errors"></a>Błędy autonomicznego

W przeciwieństwie do sesji błędy mogą wystąpić błędy autonomicznego poza kontekstem sesji.

**Przykład:**

W poniższym przykładzie pokazano, jak zgłaszać błąd, gdy pamięć staje się mała na telefonie, gdy jest uruchomiony proces aplikacji.

            public MyApplication extends EngagementApplication {

              @Override
              protected void onApplicationProcessLowMemory() {
                EngagementAgent.getInstance(this).sendError("low_memory", null);
              }
            }

##<a name="reporting-jobs"></a>Raportowanie zadań

### <a name="example"></a>Przykład

Załóżmy, że chcesz zgłosić czas trwania procesu logowania:

            [...]
            public void signIn(Context context, ...) {

              /* We need an Android context to call the Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has started */
              engagementAgent.startJob("sign_in", null);

              [... sign in ...]

              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="report-errors-during-a-job"></a>Raportowanie błędów podczas wykonywania zadania

Błędy może dotyczyć uruchomione zadanie, a nie jest powiązany bieżącej sesji użytkownika.

**Przykład:**

Załóżmy, że chcesz zgłosić błąd podczas możesz zalogować procesu:

[...] ekran logowania void publicznej (kontekst kontekst,...) {

              /* We need an Android context to call the Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has been started */
              engagementAgent.startJob("sign_in", null);

              /* Try to sign in */
              while(true)
                try {
                  trySignin();
                  break;
                }
                catch(Exception e) {
                  /* Report the error to Engagement */
                  engagementAgent.sendJobError("sign_in_error", "sign_in", null);

                  /* Retry after a moment */
                  sleep(2000);
                }
              [...]
              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="reporting-events-during-a-job"></a>Raportowanie zdarzeń podczas wykonywania zadania

Zdarzenia może dotyczyć uruchomione zadanie, a nie jest powiązany bieżącej sesji użytkownika.

**Przykład:**

Załóżmy, że mamy sieci społecznościowej, firma Microsoft korzysta zadanie do raportu całkowity czas, w którym użytkownik jest połączony z serwerem. Użytkownik może pozostawanie w kontakcie w tle, nawet gdy jest on przy użyciu innej aplikacji lub telefon jest Śpiący, dlatego nie sesji.

Użytkownik może odbierać wiadomości z jego znajomych, to zdarzenie zadania.

            [...]
            public void signin(Context context, ...) {
              [...Sign in code...]
              EngagementAgent.getInstance(context).startJob("connection", null);
            }
            [...]
            public void signout(Context context) {
              [...Sign out code...]
              EngagementAgent.getInstance(context).endJob("connection");
            }
            [...]
            public void onMessageReceived(Context context) {
              [...Notify in status bar...]
              EngagementAgent.getInstance(context).sendJobEvent("message_received", "connection", null);
            }
            [...]

##<a name="extra-parameters"></a>Dodatkowe parametry

Dowolne dane może zostać dołączony do zdarzenia, błędy, działania i zadania.

Te dane mogą być zorganizowane, to wybiera klasy pakiet dla systemu Android (w rzeczywistości działa podobnie jak dodatkowe parametry w opcji systemu Android). Należy zauważyć, że pakiet może zawierać tablicami lub innego wystąpienia pakietu.

> [AZURE.IMPORTANT] Jeśli umieszczasz w parcelable lub można parametrów, upewnij się, ich `toString()` metoda jest zaimplementowana zwraca ciąg zrozumiałą. Można klas, zawierające pola nie przejściowych, które nie są można spowoduje awarię Android po nawiąże połączenie`bundle.putSerializable("key",value);`

> [AZURE.WARNING] Rzadkie tablice w dodatkowe parametry nie są obsługiwane, oznacza to, że nie można szeregować w postaci tablicy. Należy je przekonwertować do tablic standardowy, przed użyciem go w dodatkowe parametry.

### <a name="example"></a>Przykład

            Bundle extras = new Bundle();
            extras.putString("video_id", 123);
            extras.putString("ref_click", "http://foobar.com/blog");
            EngagementAgent.getInstance(context).sendEvent("video_clicked", extras);

### <a name="limits"></a>Ograniczenia

#### <a name="keys"></a>Klawisze

Każdy klucz w `Bundle` musi odpowiadać na następujące wyrażenie:

`^[a-zA-Z][a-zA-Z_0-9]*`

Oznacza to, że klawisze musi się zaczynać co najmniej jeden list, a po nim litery, cyfry i podkreślenia (\_).

#### <a name="size"></a>Rozmiar

Dodatki są ograniczone do **1024** znaków połączenia (raz zakodowany w formacie JSON przez usługę zaangażowania).

W poprzednim przykładzie JSON przesyłane do serwera jest 58 znaków:

            {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

##<a name="reporting-application-information"></a>Raportowanie informacji o aplikacji

Możesz ręcznie zgłosić śledzenie informacji (lub innych aplikacji szczegółowych informacji) za pomocą `sendAppInfo()` funkcji.

Należy zauważyć, że te informacje, które mogą być wysyłane stopniowo: tylko najnowsze wartość w polu klucz zostaną zachowane dla danego urządzenia.

Jak dodatki zdarzenia klasy pakietu jest używany do abstrakcyjne informacje o aplikacji, należy zauważyć, że tablicami lub podrzędny wiązki jest traktowana jako ciągi płaskie (za pomocą szeregowania JSON).

### <a name="example"></a>Przykład

Oto przykładowy kod, aby wysłać płeć użytkownika i DataUrodzenia:

            Bundle appInfo = new Bundle();
            appInfo.putString("status", "premium");
            appInfo.putString("expiration", "2016-12-07"); // December 7th 2016
            EngagementAgent.getInstance(context).sendAppInfo(appInfo);

### <a name="limits"></a>Ograniczenia

#### <a name="keys"></a>Klawisze

Każdy klucz w `Bundle` musi odpowiadać na następujące wyrażenie:

`^[a-zA-Z][a-zA-Z_0-9]*`

Oznacza to, że klawisze musi się zaczynać co najmniej jeden list, a po nim litery, cyfry i podkreślenia (\_).

#### <a name="size"></a>Rozmiar

Informacje o aplikacji są ograniczone do **1024** znaków połączenia (raz zakodowany w formacie JSON przez usługę zaangażowania).

W poprzednim przykładzie JSON przesyłane do serwera jest 44 znaków:

            {"expiration":"2016-12-07","status":"premium"}
