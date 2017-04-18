<properties
    pageTitle="Zaawansowane opcje raportowania dla systemu Android SDK zaangażowania Mobile Azure"
    description="Opisano sposób wykonywania zaawansowanych raportów do przechwytywania analizy dla systemu Android SDK zaangażowania Mobile Azure"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-reporting-with-engagement-on-android"></a>Zaawansowane zgłoszenie zaangażowania w systemie Android

> [AZURE.SELECTOR]
- [Uniwersalny systemu Windows](mobile-engagement-windows-store-integrate-engagement.md)
- [Program Silverlight Windows Phone](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-advanced-reporting.md)

W tym temacie opisano dodatkowe scenariusze raportowania w aplikacji Android. Te opcje można zastosować do utworzonych w samouczku [Wprowadzenie do](mobile-engagement-android-get-started.md) aplikacji.

## <a name="prerequisites"></a>Wymagania wstępne

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

Samouczek, wykonywane przez Ciebie był celowo bezpośredni i proste, ale są zaawansowane opcje, których można wybierać.

## <a name="modifying-your-activity-classes"></a>Modyfikowanie usługi `Activity` klasy

[Samouczek Wprowadzenie](mobile-engagement-android-get-started.md)wszystkie trzeba było zrobić zostały nawiązać z `*Activity` podklas dziedziczy odpowiednie `Engagement*Activity` klas. Na przykład jeśli starsze aktywności `ListActivity`, spowodowałby jej rozszerzenie `EngagementListActivity`.

> [AZURE.IMPORTANT] Podczas korzystania z `EngagementListActivity` lub `EngagementExpandableListActivity`, upewnij się, że dowolny wywołanie `requestWindowFeature(...);` nastąpi przed połączenie `super.onCreate(...);`, w przeciwnym razie wystąpienia awarii.

Możesz znaleźć te klasy w `src` folder i można skopiować je do projektu. Klasy są również **JavaDoc**.

## <a name="alternate-method-call-startactivity-and-endactivity-manually"></a>Alternatywne metody: połączenie `startActivity()` i `endActivity()` ręcznie

Jeśli nie można lub nie chcesz, aby przeciążeń usługi `Activity` klasy, możesz zamiast tego rozpoczęcia i zakończenia działań, dzwoniąc `EngagementAgent`w metod bezpośrednio.

> [AZURE.IMPORTANT] Android SDK nigdy nie wywołuje `endActivity()` metody, nawet wtedy, gdy aplikacja zostanie zamknięta (w systemie Android, aplikacje są nigdy nie zamknięte). Dlatego *zdecydowanie* zalecane połączenie jest `startActivity()` metoda `onResume` zwrotnego *wszystko* Twoich działań oraz `endActivity()` metody w `onPause()` zwrotnego *wszystko* działań. To jest jedynym sposobem upewnij się, że nie są przecieku sesji. Jeśli sesji jest przecieku, usługę zaangażowania nigdy nie odłącza od zaangażowania wewnętrznej bazy danych (ponieważ usługa połączono jak oczekuje sesji).

Oto przykład:

    public class MyActivity extends Some3rdPartyActivity
    {
      @Override
      protected void onResume()
      {
        super.onResume();
        String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at the end.
        EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
      }

      @Override
      protected void onPause()
      {
        super.onPause();
        EngagementAgent.getInstance(this).endActivity();
      }
    }

W tym przykładzie jest podobne do `EngagementActivity` zajęć i jego odmiany, których kod źródłowy znajduje się w `src` folder.

## <a name="using-applicationoncreate"></a>Przy użyciu Application.onCreate()

Cały kod umieścić w `Application.onCreate()` i w innej aplikacji zwrotne jest uruchamiany dla wszystkich aplikacji procesów, łącznie z usługą zaangażowania. Może mieć niepożądane efekty bocznym, jak przydziału pamięci niepotrzebne i wątkach proces zaangażowania zduplikowanych odbiorców emisji lub usług.

Jeśli zostanie zmieniona `Application.onCreate()`, zaleca się dodawania następujący fragment kodu na początku swojej `Application.onCreate()` funkcji:

     public void onCreate()
     {
       if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
         return;

       ... Your code...
     }

Można wykonywać te same kroki dla `Application.onTerminate()`, `Application.onLowMemory()`, i `Application.onConfigurationChanged(...)`.

Można również rozszerzyć `EngagementApplication` zamiast rozszerzenia `Application`: wywołanie zwrotne `Application.onCreate()` czy Sprawdź proces i połączeń `Application.onApplicationProcessCreate()` tylko jeśli bieżący proces nie jest uruchamiana usługa zaangażowania, same reguły są stosowane do innych zwrotne.

## <a name="tags-in-the-androidmanifestxml-file"></a>Znaczniki w pliku AndroidManifest.xml

Seryjny w pliku AndroidManifest.xml `android:label` atrybut pozwala wybrać nazwę usługi zaangażowania widoczny dla użytkowników końcowych na ekranie telefonu "Z usługami". Zaleca się ustawienie tego atrybutu na `"<Your application name>Service"` (na przykład `"AcmeFunGameService"`).

Określanie `android:process` atrybut gwarantuje, że usługa zaangażowania działa we własnym procesie (działa zaangażowania w tym samym procesem, jak aplikacja usługi wątku głównego i interfejsu użytkownika potencjalnie mniej odpowiada).

## <a name="building-with-proguard"></a>Tworzenie z ProGuard

Tworzenie pakietu aplikacji z ProGuard, należy zachować niektóre klasy. Można używać następujących wstawek konfiguracji:

    -keep public class * extends android.os.IInterface
    -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
    <methods>;
    }
