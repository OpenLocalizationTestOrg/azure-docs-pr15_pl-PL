<properties 
    pageTitle="Jak używać zaangażowania interfejsu API na uniwersalnego systemu Windows" 
    description="Jak używać zaangażowania interfejsu API na uniwersalnego systemu Windows"            
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-store" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="how-to-use-the-engagement-api-on-windows-universal"></a>Jak używać zaangażowania interfejsu API na uniwersalny systemu Windows

Ten dokument jest dodatkiem do dokumentu, [jak zintegrować zaangażowania na uniwersalny systemu Windows](mobile-engagement-windows-store-integrate-engagement.md): znajdują się w głębokości szczegóły dotyczące raportu statystyk aplikacji za pomocą interfejsu API zaangażowania.

Należy pamiętać, że jeśli chcesz tylko zaangażowania raportowania aplikacji sesje, działania, awarii i informacje techniczne, następnie Najłatwiejszym sposobem jest wyświetlanie wszystkich usługi `Page` klasy podrzędne dziedziczą `EngagementPage` zajęć.

Jeśli chcesz uzyskać więcej informacji, na przykład jeśli chcesz zgłosić zdarzenia określonej aplikacji, błędów i zadania, lub masz raportowania aplikacji działania w inny sposób niż to, realizowane w `EngagementPage` klasy, należy użyć interfejsu API zaangażowania.

Interfejs API zaangażowania jest dostarczany przez `EngagementAgent` zajęć. Można uzyskać dostęp do tych metod za pośrednictwem `EngagementAgent.Instance`.

Nawet jeśli nie została zainicjowana moduł agenta, każdego połączenia API jest odroczone, a zostanie wykonana ponownie po udostępnieniu agenta.

##<a name="engagement-concepts"></a>Pojęcia zaangażowania

Następujące elementy Uściślij typowych [Pojęcia zaangażowania Mobile](mobile-engagement-concepts.md) na platformie Windows uniwersalny.

### <a name="session-and-activity"></a>`Session`oraz`Activity`

*Działanie* jest zwykle skojarzony jedną stronę aplikacji, to znaczy *aktywności* uruchamianego po stronie zostanie wyświetlona i zatrzymuje się po zamknięciu strony: jest to wielkość liter w przypadku SDK zaangażowania jest zintegrowany przy użyciu `EngagementPage` zajęć.

Ale *działania* można sterować także ręcznie za pomocą interfejsu API zaangażowania. Umożliwia dzielenie danej strony w kilku części sub, aby uzyskać więcej informacji o korzystaniu z tej strony (na przykład dowiedzieć się, jak często i jak długo trwa okien dialogowych są używane w tej strony).

##<a name="reporting-activities"></a>Tworzenia raportów

### <a name="user-starts-a-new-activity"></a>Użytkownik uruchamia nowe działanie

#### <a name="reference"></a>Odwołania

            void StartActivity(string name, Dictionary<object, object> extras = null)

Musisz zadzwonić do `StartActivity()` każdej zmianie aktywności użytkownika. Pierwsze wywołanie tej funkcji zaczyna się nową sesję użytkownika.

> [AZURE.IMPORTANT] Zestaw SDK automatycznie wywołuje metodę EndActivity, gdy aplikacja zostanie zamknięta. Dlatego zdecydowanie zalecane jest wywołać metody StartActivity, gdy aktywności użytkownika zmian, a nigdy Wywołaj metodę EndActivity, ponieważ wywołanie tej metody wymusza bieżącej sesji do zakończenia.

#### <a name="example"></a>Przykład

            EngagementAgent.Instance.StartActivity("main", new Dictionary<object, object>() {{"example", "data"}});

### <a name="user-ends-his-current-activity"></a>Użytkownik kończy swoich bieżących działań.

#### <a name="reference"></a>Odwołania

            void EndActivity()

Kończy działanie i sesji. Ta metoda nie powinna wywołana, chyba że naprawdę znasz wykonywanej.

#### <a name="example"></a>Przykład

            EngagementAgent.Instance.EndActivity();

##<a name="reporting-jobs"></a>Raportowanie zadań

### <a name="start-a-job"></a>Rozpoczynanie pracy

#### <a name="reference"></a>Odwołania

            void StartJob(string name, Dictionary<object, object> extras = null)

Zadania umożliwia śledzenie zadań podaje w okresie.

#### <a name="example"></a>Przykład

            // An upload begins...
            
            // Set the extras
            var extras = new Dictionary<object, object>();
            extras.Add("title", "avatar");
            extras.Add("type", "image");
            
            EngagementAgent.Instance.StartJob("uploadData", extras);

### <a name="end-a-job"></a>Kończenie zadania

#### <a name="reference"></a>Odwołania

            void EndJob(string name)

Jak zadania śledzone przez zadanie zostało zakończone, należy wywołać metodę EndJob dla tego zadania, przez podanie nazwy zadania.

#### <a name="example"></a>Przykład

            // In the previous section, we started an upload tracking with a job
            // Then, the upload ends
            
            EngagementAgent.Instance.EndJob("uploadData");

##<a name="reporting-events"></a>Raportowanie zdarzeń

Istnieje trzy typy zdarzeń:

-   Zdarzenia autonomicznego
-   Zdarzeń sesji
-   Zdarzenia zadania

### <a name="standalone-events"></a>Zdarzenia autonomicznego

#### <a name="reference"></a>Odwołania

            void SendEvent(string name, Dictionary<object, object> extras = null)

Zdarzenia autonomicznego mogą występować poza kontekstem sesji.

#### <a name="example"></a>Przykład

            EngagementAgent.Instance.SendEvent("event", extra);

### <a name="session-events"></a>Zdarzeń sesji

#### <a name="reference"></a>Odwołania

            void SendSessionEvent(string name, Dictionary<object, object> extras = null)

Zdarzeń sesji zwykle są używane do raportu akcje wykonywane przez użytkownika podczas tej sesji.

#### <a name="example"></a>Przykład

**Bez danych:**

            EngagementAgent.Instance.SendSessionEvent("sessionEvent");
            
            // or
            
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", null);

**Z danymi:**

            Dictionary<object, object> extras = new Dictionary<object,object>();
            extras.Add("name", "data");
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", extras);

### <a name="job-events"></a>Zdarzenia zadania

#### <a name="reference"></a>Odwołania

            void SendJobEvent(string eventName, string jobName, Dictionary<object, object> extras = null)

Zdarzenia zadania są zazwyczaj używany do zgłaszania akcje wykonywane przez użytkownika podczas wykonywania zadania.

#### <a name="example"></a>Przykład

            EngagementAgent.Instance.SendJobEvent("eventName", "jobName", extras);

##<a name="reporting-errors"></a>Raportowanie błędów

Istnieją trzy rodzaje błędów:

-   Błędy autonomicznego
-   Błędy sesji
-   Błędy zadań

### <a name="standalone-errors"></a>Błędy autonomicznego

#### <a name="reference"></a>Odwołania

            void SendError(string name, Dictionary<object, object> extras = null)

W przeciwieństwie do sesji błędy mogą wystąpić błędy autonomicznego poza kontekstem sesji.

#### <a name="example"></a>Przykład

            EngagementAgent.Instance.SendError("errorName", extras);

### <a name="session-errors"></a>Błędy sesji

#### <a name="reference"></a>Odwołania

            void SendSessionError(string name, Dictionary<object, object> extras = null)

Błędy sesji zwykle są używane do raportować wpływające na ochronę użytkownika podczas tej sesji.

#### <a name="example"></a>Przykład

            EngagementAgent.Instance.SendSessionError("errorName", extra);

### <a name="job-errors"></a>Błędy zadań

#### <a name="reference"></a>Odwołania

            void SendJobError(string errorName, string jobName, Dictionary<object, object> extras = null)

Błędy może dotyczyć uruchomione zadanie, a nie jest powiązany bieżącej sesji użytkownika.

#### <a name="example"></a>Przykład

            EngagementAgent.Instance.SendJobError("errorName", "jobname", extra);

##<a name="reporting-crashes"></a>Raportowanie awarii

Agent oferuje dwie metody na zajęcie się awarii.

### <a name="send-an-exception"></a>Wysyłanie wyjątków

#### <a name="reference"></a>Odwołania

            void SendCrash(Exception e, bool terminateSession = false)

#### <a name="example"></a>Przykład

Możesz wysłać wyjątków w dowolnym momencie, dzwoniąc:

            EngagementAgent.Instance.SendCrash(aCatchedException);

Aby zakończyć sesję zaangażowania w tym samym czasie, niż przesyłanie awarii umożliwia także parametr opcjonalny. W tym celu należy połączeń:

            EngagementAgent.Instance.SendCrash(new Exception("example"), terminateSession: true);

Jeśli do sesji i zadań zostanie zamknięta tylko po wysłaniu awarii.

### <a name="send-an-unhandled-exception"></a>Wysyłanie powodu nieobsługiwanego wyjątku

#### <a name="reference"></a>Odwołania

            void SendCrash(Exception e)

Zaangażowania zapewnia metodę wysyłać nieobsługiwanego wyjątków, jeśli masz raportowania automatyczne **awarie** zaangażowania **wyłączone** . To jest szczególnie przydatne, gdy używana wewnątrz UnhandledException aplikacji zdarzeń.

Ta metoda będzie **zawsze** zakończyć sesję zaangażowania i zadania po wywołaniu.

#### <a name="example"></a>Przykład

Umożliwia go zaimplementować własnej obsługi UnhandledExceptionEventArgs. Na przykład dodać `Current_UnhandledException` metody `App.xaml.cs` pliku:

            // In your App.xaml.cs file
            
            // Code to execute on Unhandled Exceptions
            void Current_UnhandledException(object sender, UnhandledExceptionEventArgs e)
            {
               EngagementAgent.Instance.SendCrash(e.Exception,false);
            }

W App.xaml.cs w "{} App() publicznej" Dodaj:

            Application.Current.UnhandledException += Current_UnhandledException;

##<a name="device-id"></a>Identyfikator urządzenia

            String EngagementAgent.Instance.GetDeviceId()

Identyfikator urządzenia zaangażowania można uzyskać, dzwoniąc do tej metody.

##<a name="extras-parameters"></a>Dodatkowe parametry

Dowolne dane może zostać dołączony do zdarzenia, komunikat o błędzie, działania lub zadania. Dane te mogą być zorganizowane przy użyciu słownika. Klucze i wartości może być dowolnego typu.

Dodatkowe dane są szeregować, jeśli chcesz wstawić własny typ dodatki należy dodać umowy dotyczącej danych dla tego typu.

### <a name="example"></a>Przykład

Możemy utworzyć nową klasę "Osoby".

            using System.Runtime.Serialization;
            
            namespace Microsoft.Azure.Engagement
            {
              [DataContract]
              public class Person
              {
                public Person(string name, int age)
                {
                  Age = age;
                  Name = name;
                }
            
                // Properties
            
                [DataMember]
                public int Age
                {
                  get;
                  set;
                }
            
                [DataMember]
                public string Name
                {
                  get;
                  set; 
                }
              }
            }

Następnie zostanie dodana `Person` wystąpienie w celu dodatek.

            Person person = new Person("Engagement Haddock", 51);
            var extras = new Dictionary<object, object>();
            extras.Add("people", person);
            
            EngagementAgent.Instance.SendEvent("Event", extras);

> [AZURE.WARNING] Jeśli inne typy obiektów, upewnij się, że ich metody ToString() jest zaimplementowana zwraca ludzi ciąg czytelne.

### <a name="limits"></a>Ograniczenia

#### <a name="keys"></a>Klawisze

Każdy klucz w obiekcie musi być zgodna następujące wyrażenie:

`^[a-zA-Z][a-zA-Z_0-9]*$`

Oznacza to, że klawisze musi się zaczynać co najmniej jeden list, a po nim litery, cyfry i podkreślenia (\_).

#### <a name="size"></a>Rozmiar

Dodatki są ograniczone do **1024** znaków połączenia.

##<a name="reporting-application-information"></a>Raportowanie informacji o aplikacji

### <a name="reference"></a>Odwołania

            void SendAppInfo(Dictionary<object, object> appInfos)

Możesz ręcznie zgłosić, funkcja śledzenia informacji (lub innych aplikacji szczegółowych informacji) przy użyciu SendAppInfo().

Należy zauważyć, że te dane, które mogą być wysyłane stopniowo: tylko najnowsze wartość w polu klucz zostaną zachowane dla danego urządzenia. Jak dodatki zdarzenia, należy użyć słownika\<obiektu, obiekt\> do dołączenia danych.

### <a name="example"></a>Przykład

            Dictionary<object, object> appInfo = new Dictionary<object, object>()
              {
                {"birthdate", "1983-12-07"},
                {"gender", "female"}
              };
            
            EngagementAgent.Instance.SendAppInfo(appInfo);

### <a name="limits"></a>Ograniczenia

#### <a name="keys"></a>Klawisze

Każdy klucz w obiekcie musi być zgodna następujące wyrażenie:

`^[a-zA-Z][a-zA-Z_0-9]*$`

Oznacza to, że klawisze musi się zaczynać co najmniej jeden list, a po nim litery, cyfry i podkreślenia (\_).

#### <a name="size"></a>Rozmiar

Informacje o aplikacji jest ograniczone do **1024** znaków połączenia.

W poprzednim przykładzie JSON przesyłane do serwera jest 44 znaków:

            {"birthdate":"1983-12-07","gender":"female"}

##<a name="logging"></a>Rejestrowanie
###<a name="enable-logging"></a>Włączanie rejestrowania

Zestaw SDK można skonfigurować da dzienniki test w konsoli IDE.
Dzienniki te nie są aktywowane domyślnie. Aby dostosować tak, zaktualizuj właściwość `EngagementAgent.Instance.TestLogEnabled` do jednej z wartości dostępne na `EngagementTestLogLevel` wyliczenie, na przykład:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();
 
