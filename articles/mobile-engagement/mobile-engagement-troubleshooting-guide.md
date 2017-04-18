<properties 
   pageTitle="Przewodniki rozwiązywania problemów Azure zaangażowania urządzeń przenośnych" 
   description="Rozwiązywanie problemów z przewodnika dotyczącego Azure zaangażowania urządzeń przenośnych" 
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

# <a name="azure-mobile-engagement---troubleshooting-guide"></a>Azure zaangażowania urządzeń przenośnych — Podręcznik rozwiązywania problemów

## <a name="introduction"></a>Wprowadzenie
Ten przewodnik rozwiązywania problemów pomoże zrozumieć, że przyczyn niektóre często widoczna problemów i będzie umożliwiają rozwiązywanie problemów z własne. 

## <a name="general"></a>Ogólne

Ogólnie zawsze należy następujące czynności:

1. Upewnij się, że zostało już wszystkie kroki wymagane do integracji, zgodnie z opisem w naszym [samouczki wprowadzenie](mobile-engagement-windows-store-dotnet-get-started.md)
2. Używasz najnowszej wersji platformy SDK. 
3. Test zarówno na urządzeniu z systemem rzeczywisty i emulatora, ponieważ niektóre problemy dotyczą tylko emulatora. 
4. Nie są naciśnięcie wszelkie ograniczenia i limity z zaangażowania Mobile opisanych [tutaj](../azure-subscription-service-limits.md)
5. Jeśli jesteś nie może nawiązać połączenia z usługi zaangażowania Mobile wewnętrznej bazy danych lub oglądanie danych nie jest ładowana stale następnie upewnić, że nie zdarzeń trwających usługi, zaznaczając pole wyboru [poniżej](https://azure.microsoft.com/status/)

## <a name="monitor-issues"></a>Problemy "Monitor"

### <a name="i-am-not-seeing-my-device-showing-up-on-the-monitor-tab"></a>Urządzenie widoczne na karcie Monitor nie jest widoczny
Na karcie Monitor wyświetlana urządzeń podłączonych do posiadanej platformy zaangażowania Mobile w czasie rzeczywistym. Debugowania na emulatora i urządzenia, należy zobacz co najmniej jednej sesji w tym miejscu. Jeśli aplikacja została rozdzielona, zostanie wyświetlona miernik sesje odzwierciedlenia urządzeń, które są połączone z platformy w czasie rzeczywistym. 

Jeśli nie widzisz urządzenia na karcie Monitor jest prawdopodobnie problem integracji SDK. Oto kilka typowych czynności, aby podjąć w celu rozwiązania:

1. Upewnij się, że używasz prawidłowych parametrów połączenia w aplikacji dla urządzeń przenośnych i pochodzi z klawiszy SDK i nie sekcji klawiszy interfejsu API. Parametry połączenia łączy do aplikacji dla urządzeń przenośnych wystąpienie aplikacji zaangażowania Mobile, w której urządzenia będą widoczne na karcie Monitor. 
2. Platformy systemu Windows — Jeśli zastępuje stronę `OnNavigatedTo` metody, upewnij się, że połączenie `base.OnNavigatedTo(e)`.
3. Jeśli zaangażowania Mobile są integrowanie istniejącej aplikacji dla urządzeń przenośnych, a następnie można również zagwarantować, że nie ma żadnych kroków sprawdzając czynności integracji zaawansowane [poniżej](mobile-engagement-windows-store-integrate-engagement.md)
4. Upewnij się, że co najmniej jeden ekranie i działania są wysyłane przez zastąpienie strona z EngagementActivity w zależności od platformę, którą pracujesz, zgodnie z opisem w [samouczkach wprowadzenie](mobile-engagement-windows-store-dotnet-get-started.md).

### <a name="i-am-seeing-the-monitor-tab-showing-a-session-even-when-i-have-disconnected-or-closed-my-app-emulator"></a>Jest wyświetlany na karcie Monitor przedstawiający sesji nawet gdy jest odłączona lub zamknięty Moja aplikacja-emulatora. 
Jeśli jesteś jedyną osobą połączenie z platformą na tym etapie i emulatora jest używana, aby otworzyć aplikację następnie to prawdopodobnie z powodu Osobliwości emulatora. Ogólnie należy się upewnić, że możesz wrócić do ekranu głównego emulatora sesji aplikacji odłączyć pomyślnie. Ponadto na platformie Windows podczas debugowania za pomocą programu Visual Studio, może być konieczne zagwarantować, że w programie Visual Studio, można przejść na pasek menu **Zdarzenia czasu trwania** i wybierz polecenie **wstrzymania** naprawdę zamknąć sesję. Aby uzyskać szczegółowe informacje, zobacz [Samouczek systemu Windows](mobile-engagement-windows-store-dotnet-get-started.md) . 

## <a name="analytics-issues"></a>Problemy dotyczące "Analizy"

### <a name="i-am-not-seeing-any-data-refreshed-data-on-analytics-tab"></a>I nie są widoczne wszystkie dane / odświeżania danych na karcie analiza 
Analizy danych jest obliczana ponownie regularnie i może potrwać maksymalnie przez 24 godziny na odświeżanie. Nie te dane czasu rzeczywistego i zostanie wyświetlona jego odświeżone w tym 24-godzinnego przedziału czasu.
Upewnij się, mimo że wysyłasz co najmniej jeden ekran lub czynność do wewnętrznej bazy danych platformy o albo nadrzędnych co najmniej jedną stronę z `EngagementActivity` lub `SendActivity` explcitly. 

### <a name="i-am-seeing-incorrectly-captured-datetime-for-a-device-on-the-analytics-tab"></a>Niepoprawnie przechwycony Data/Godzina na urządzeniu jest wyświetlany na karcie analiza
Termin analizy jest oparty na datę w obszarze Ustawienia urządzenia użytkowników. Aby upewnić się, że urządzenie ma daty poprawnie ustawiona jako. 

## <a name="segment-issues"></a>Problemy "Części"

### <a name="i-created-a-segment-and-it-is-showing-up-as-greyed-out-or-not-showing-any-data"></a>Po utworzeniu segmentu i jest wyświetlana jako wyszarzony lub nie pokazuje wszystkie dane
Tworzenie części nie jest w czasie rzeczywistym w momencie. W tym samym czasie jest obliczana jako pole jest agregowane analizy danych i może potrwać maksymalnie przez 24 godziny. Należy sprawdzić ponownie później, ale w międzyczasie należy upewnić się do aplikacji dla urządzeń przenośnych faktycznie wysyłają danych na podstawie której są tworzące segmentów. Np. Jeśli zdarzenie powiedzieć "foo" nie jest wysyłane przez każde urządzenie przenośne, a następnie nie być jakiekolwiek dane segmentu dla segmentu utworzone za pomocą EventName = foo jako kryterium. Należy także sprawdzić, czy usługi integracji SDK zapewnienie Twojej aplikacji dla urządzeń przenośnych poprawnie wysyłania danych. 

## <a name="reach-or-push-notifications-issues"></a>Problemy związane z "Zasięg" i powiadomienia Push

### <a name="my-push-messages-are-not-being-delivered"></a>Nie są dostarczane wiadomości push 

1. Spróbuj Wysyłanie powiadomienia do testowanie urządzenia, aby upewnić się, że wszystkie składniki - aplikacji dla urządzeń przenośnych, SDK i usługi są poprawnie połączone i stanie dostarczyć powiadomienia wypychane. 
2. Zawsze wysyłaj najłatwiejszym "powiadomienie w nowym oknie aplikacji" najpierw przy użyciu kampanii, która nie jest zaplanowane i ani każdego z określonych kryteriów odbiorców. Ponownie to aby udowodnić, czy łączności powiadomień działa poprawnie. 
3. Jeśli występują problemy z przedstawiania powiadomienia w aplikacji również jest pierwszym krokiem warto wypróbować najpierw wysyłania powiadomień w nowym oknie aplikacji. 
4. Upewnij się, "natywnych Push" jest poprawnie skonfigurowany dla twojej aplikacji dla urządzeń przenośnych. W zależności od platformy albo obejmuje się klawiszy (Android, systemu Windows) lub certyfikaty (system iOS). Zobacz [interfejs użytkownika — ustawienia](mobile-engagement-user-interface-settings.md)
5. Wylogowywanie się z aplikacji, którą powiadomienia może również być zablokowana przez użytkownika za pomocą urządzeń przenośnych systemu operacyjnego tak upewnij się, że nie jest to wielkość liter. 
6. Upewnij się, że nie ustawiasz opcję *Ignoruj odbiorców wypychanych będą wysyłane do użytkowników za pośrednictwem interfejsu API programu* w sekcji **kampanii** kampanii Reach ponieważ temu będziesz mieć pewność, że tylko być wysyłane powiadomienia wypychane za pośrednictwem interfejsów API. 
7. Upewnij się, oba urządzenia, połączonych za pomocą operatora sieci Wi-Fi i telefonu aby wyeliminować połączenie sieciowe jako źródła możliwych problemów testujesz kampanii push.
8. Upewnij się, system Data/Godzina na urządzeniu emulatora jest poprawny, ponieważ dowolnego urządzenia zsynchronizowany zakłóca także możliwość wypychanych powiadomień usługi przeprowadzania powiadomienia. 

Więcej platformy poniższe instrukcje dotyczące rozwiązywania problemów:

1. **iOS** 

    - Upewnij się, że certyfikaty są prawidłowe i pozostały dla systemu iOS powiadomienia Push. 
    - Upewnij się, że poprawnie konfigurujesz *świadectwo* w aplikacji Mobile zaangażowania. 
    - Upewnij się, że testujesz na *rzeczywistą, fizycznie urządzenie.* IOS simulator nie może przetworzyć wypychanych wiadomości.
    - Upewnij się, że identyfikator pakietu jest poprawnie skonfigurowany w aplikacji dla urządzeń przenośnych. Zapoznaj się z instrukcjami [poniżej](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)
    - Podczas testowania, rozkład "Ad Hoc" w profilu obsługi administracyjnej urządzeń przenośnych. Nie można otrzymać powiadomienie, jeśli aplikacji jest skompilowany za pomocą "Debugowanie"

2. **Android**

    - Upewnij się, że został określony prawidłowy numer projektu w aplikacji mobilnej AndroidManifest.xml pliku, której następuje znak \n. 
    
            <meta-data android:name="engagement:gcm:sender" android:value="************\n" />
        
    - Upewnij się, że nie są widoczne lub nieprawidłowo skonfigurowane uprawnienia w pliku Android pojawiają 
    - Należy się upewnić, że jest numer projektu, który chcesz dodać do aplikacji klienckich z tego samego konta miejsce, w którym masz klucz serwera GCM. Wszelkie niezgodności między tymi dwoma uniemożliwi usługi umieszcza będzie. 
    - W przypadku otrzymania powiadomienia systemowe, ale nie w aplikacji, a następnie przeglądanie [Określ ikonę sekcji powiadomienia](mobile-engagement-android-get-started.md) jako może nie wpisano odpowiednią ikonę pliku Android pojawiają. 
    - Jeśli wysyłasz powiadomienie BigPicture, a następnie upewnij się, że jeśli masz obrazu zewnętrznego serwerów, a następnie muszą mieć możliwość obsługi protokołu HTTP "Pobierz" i "Szef".

3. **Systemu Windows**
    
    - Upewnij się, że skojarzony aplikacji z prawidłową aplikacji ze Sklepu Windows. W programie Visual Studio — należy kliknij prawym przyciskiem myszy projektu i wybierz opcję "Skojarzyć z sklep" i wybierz aplikację, którą utworzono w Sklepie Windows. Tej aplikacji ze Sklepu Windows powinna być taka sama, jedną, z którym masz poświadczenia natywnych wypychanych, aby skonfigurować w portalu zaangażowania Mobile.
    - W przypadku otrzymania powiadomień wypychanych w nowym oknie aplikacji, ale nie w aplikacji powiadomienia z `EngagementOverlay` integracji następnie upewnij się, istnieje element główny siatki na stronie. Przy użyciu EngagementOverlay pierwszego elementu "Siatki" znajdzie się w pliku xaml, aby dodać dwa web widoków na stronie. Jeśli chcesz zlokalizować miejsce, w którym można ustawić widoku sieci web, można zdefiniować siatkę o nazwie "EngagementGrid", tak jak to jednak należy upewnić się, że jest wystarczające wysokość i szerokość dla sieci web kolejnych dwóch widoków, które zostanie wyświetlona powiadomienie i następujące powiadomienie jako powiadomienie w aplikacji app:
        
            <Grid x:Name="EngagementGrid"></Grid>

### <a name="i-created-a-push-notificationannouncement-campaign-and-even-after-it-sent-me-the-notification-it-is-showing-as-active-what-does-it-mean"></a>Po utworzeniu wypychanych powiadomień-zawiadomienie o/kampanii i nawet, po jej wysłaniu mi powiadomienie, jest wyświetlany jako "Aktywna". Co to znaczy? 
**Kampania** utworzony w zaangażowania Mobile nosi nazwę, ponieważ jest długotrwałych znaczenia powiadomień wypychanych jako nowe urządzenia połączenia do posiadanej platformy urządzeń przenośnych zaangażowania, zostaną automatycznie wysłane powiadomienie, które można skonfigurować, dopóki spełniają kryterium ustawiane w kampanii. To nie jednego konfiguracji zrzut pojedyncze powiadomienia. Musisz ręcznie kliknij przycisk **Zakończ** , aby zakończyć kampanii go nie są wysyłane dodatkowo powiadomienia. 

### <a name="i-created-a-push-campaign-and-i-am-receiving-notifications-successfully-however-whenever-i-open-up-the-app-i-get-the-same-notification-even-when-i-had-actioned-it-before"></a>Po utworzeniu kampanii wypychanych i odbiera powiadomienia pomyślnie jednak po otwarciu aplikacji w górę, otrzymuję powiadomienie samej nawet wtedy, gdy użytkownik miał actioned go przed? 
Jest to prawdopodobne podczas testowania i jeśli używasz emulatory lub niektóre przetestować framework, takich jak TestFlight. Co się dzieje, Oto, że w każdej aplikacji uruchomić wystąpienie jest podczas uzyskiwania nowy identyfikator urządzenia i wysyłanie go do naszych wewnętrznej bazy danych, który powoduje platformy zaangażowania Mobile traktowania jej jako nowego urządzenia i wysyłanie powiadomienia. 

## <a name="getting-support"></a>Uzyskiwanie pomocy technicznej

Jeśli nie możesz rozwiązać ten problem samodzielnie możesz wykonywać następujące czynności:

1. Wyszukaj problemu istniejących wątków na forum zdarzeń StackOverflow i [forum w witrynie MSDN](https://social.msdn.microsoft.com/Forums/windows/en-US/home?forum=azuremobileengagement) i jeśli nie następnie zadać pytanie. 
2. Jeśli znajdziesz funkcji Brak następnie dodać głos żądania naszych [możliwości forum](https://feedback.azure.com/forums/285737-mobile-engagement/) w witrynie
3. Jeśli masz otwarty pomocy technicznej firmy Microsoft pomocy technicznej zdarzenia, dostarczając na następujące szczegóły: 
    - Identyfikator subskrypcji Azure
    - Platformy (np. iOS, Android itp)
    - Identyfikator aplikacji
    - Identyfikator kampanii (w przypadku problemów powiadomień wypychanych)
    - Identyfikator urządzenia
    - Wersja SDK zaangażowania Mobile (np. v2.1.0 SDK systemu Android)
    - Informacje o błędzie z komunikat o błędzie dokładnie i scenariusz
