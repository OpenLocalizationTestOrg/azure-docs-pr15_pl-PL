<properties 
    pageTitle="Powiadomienie Azure koncentratory — wskazówki dotyczące diagnostyki" 
    description="Wytyczne dotyczące diagnozowania typowych problemów z koncentratorów powiadomienie Azure." 
    services="notification-hubs" 
    documentationCenter="Mobile" 
    authors="ysxu" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="NA" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs---diagnosis-guidelines"></a>Azure koncentratory powiadomienie — wskazówki dotyczące diagnostyki

##<a name="overview"></a>Omówienie

Jedną z często zadawane pytania, zadający koncentratory powiadomienie Azure klientów jest jak ustalanie, dlaczego nie widzą powiadomienie wysyłane z ich wewnętrznej bazy danych aplikacji są wyświetlane na urządzeniu klienta — miejsce, w którym i dlaczego powiadomienia zostały usunięte i jak rozwiązać ten problem. W tym artykule będzie możemy przejść przez różnych powodów, dlaczego powiadomienia mogą uzyskać wpuszczony lub nie kończą na urządzeniach. Możemy również będzie wyglądać za pomocą metody można przeanalizować i ustalanie głównej przyczyny. 

Przede wszystkim niezwykle ważne jest zrozumieć, jak koncentratory powiadomienie Azure umieszcza powiadomień do urządzenia.
![][0]

W formie przepływu powiadomienie typowe Wyślij wiadomość jest wysyłana z **wewnętrznej bazy danych aplikacji** do **Azure powiadomień Centrum (NH)** , która z kolei część przetwarzania na wszystkich rejestracji uwzględnieniem skonfigurowane znaczniki i wyrażeń znaczników w celu ustalenia "elementów docelowych", to znaczy wszystkich rejestracji, które chcesz otrzymywać powiadomienia push. Te rejestracji może obejmować przez dowolne lub wszystkie z naszych platformy obsługiwane — iOS, Google, systemu Windows, Windows Phone Kindle i Baidu dla Android w Chinach. Po ustaleniu celów NH, a następnie umieszcza powiadomień, podzielić na wiele partii rejestracji platformy urządzeń określonych **Usługa wypychanych powiadomień (PNS)** — przykład APN dla firmy Apple, GCM Google itd. NH uwierzytelnia z odpowiednich obwodowym układzie Nerwowym na podstawie poświadczeń ustawiane w portalu klasyczny Azure na stronie Konfigurowanie Centrum powiadomienie. Obwodowym układzie Nerwowym przekazuje powiadomienia do odpowiednich **o urządzeniach klienckich**. Jest to platformy zalecany sposób przedstawiania powiadomienia wypychane i należy zauważyć, że ostatnim etapie powiadomienie o dostarczeniu odbywa się między platformy obwodowym układzie Nerwowym i urządzenia. W związku z tym mamy cztery główne składniki - *Klient*, *wewnętrznej bazy danych aplikacji* *Azure powiadomienie koncentratory (NH)* i *Usługi wypychanych powiadomień (PNS)* i dowolną z następujących mogą spowodować, powiadomienia o wprowadzenie inicjały. Więcej informacji na temat tej architektury jest dostępna na [Przegląd koncentratory powiadomienie].

Błąd do przeprowadzania powiadomienia może wystąpić podczas początkowej test przemieszczania faza, które mogą wskazywać problem z konfiguracją lub może wystąpić w miejsce, w którym wszystkie lub niektóre z powiadomienia o jego może być wprowadzenie porzucone wskazująca szczegółowego aplikacji lub wiadomości problem deseniu produkcji. W sekcji poniżej firma Microsoft będzie wyglądać w różnych scenariuszach porzucone powiadomienia od typowych rzadkich rodzaju, niektóre z nich może się okazać oczywiste i inne nie tak wiele. 

##<a name="azure-notifications-hub-mis-configuration"></a>Azure powiadomień Centrum niewłaściwa konfiguracja 

Azure koncentratory powiadomienie musi uwierzytelnienia bramy w kontekście dewelopera aplikacji, aby umożliwić pomyślne Wysyłanie powiadomienia do odpowiednich obwodowym układzie Nerwowym. Jest to możliwe przez dewelopera Tworzenie konta Deweloper odpowiednich platformie (Google, Apple itp systemu Windows) i następnie rejestrowania ich aplikacji, gdzie one uzyskać poświadczenia, które muszą być skonfigurowane w portalu w sekcji Konfiguracja koncentratory powiadomienie. Jeśli nie powiadomienia są za pośrednictwem, najpierw należy aby upewnić się, że poprawne poświadczenia są skonfigurowane w Centrum powiadomienie pasujących je z aplikacją utworzone na podstawie swojego konta określonego dewelopera platformy. Będą przydatne naszych [Wprowadzenie samouczki dotyczące] przekroczy ten proces w sposób krok po kroku. Oto niektóre typowe konfiguracje niewłaściwa:

1. **Ogólne**
 
    ) upewnij się, że nazwy Centrum powiadomienie (bez literówki) jest równa:

    - Gdy rejestrujesz się w kliencie 
    - Gdy wysyłasz powiadomienia z wewnętrznej bazy danych,  
    - Miejsce, w którym skonfigurowano poświadczeń obwodowym układzie Nerwowym i 
    - Którego poświadczenia skojarzeń zabezpieczeń skonfigurowano na kliencie i wewnętrznej bazy danych. 
        
    b) upewnij się, że są poprawne ciągów konfiguracji skojarzeń zabezpieczeń na kliencie i wewnętrznej bazy danych aplikacji. Jako zasada należy używać **DefaultListenSharedAccessSignature** na kliencie i **DefaultFullSharedAccessSignature** do wewnętrznej bazy danych aplikacji, (co daje uprawnienia, aby można było wysyłać powiadomienie NH)

2. **Konfiguracja usługi wypychanych powiadomień firmy Apple (APN)**
 
    Należy zachować dwa różne koncentratory — jedną dla produkcji i innego testowania cel. Oznacza to, przekazywanie certyfikat, który zamierzasz korzystać w środowisku piaskownicy do koncentratora oddzielnych i certyfikat, który zamierzasz użyć w produkcji osobnym Centrum. Nie należy przekazać do tego samego koncentratora różne typy certyfikatów może powodować błędy powiadomienie wiersz w dół. Jeśli okaże się, w miejscu, gdzie przypadkowo przekazano różnego rodzaju certyfikat do tego samego koncentratora, zaleca się do usunięcia koncentratora i rozpoczęcie pracy od nowa. Jeśli z jakiegoś powodu nie jest można usunąć Centrum następnie co najmniej, możesz usunąć wszystkich istniejących rejestracji z poziomu Centrum. 

3. **Konfiguracja usługi Google Cloud wiadomości (GCM)** 

    ) upewnij się, że jest włączana "Google Cloud wiadomości dla Android" w obszarze Projekt chmury. 
    
    ![][2]
    
    b) upewnij się, że utworzyć klucz"serwer" podczas uzyskiwania poświadczeń, które NH zostaną użyte do uwierzytelnienia z GCM. 
    
    ![][3]
    
    c) upewnij się, że masz skonfigurowane "Identyfikator projektu", na komputerze klienckim, który jest całkowicie liczbowa jednostki, który można uzyskać na pulpicie nawigacyjnym:
    
    ![][1]

##<a name="application-issues"></a>Problemy dotyczące aplikacji

1) **Tagi / znakowanie wyrażeń**

Jeśli segmentu odbiorców za pomocą znaczników lub znacznik wyrażeń, zawsze jest możliwe, że wysyłając powiadomienie, jest żadne miejsce docelowe nie znaleziono oparte na wyrażeniach znaczniki i znacznika, który określasz w połączeniach Wyślij. Warto przejrzeć usługi rejestracji, aby upewnić się, że są znaczniki, które odpowiadają podczas wysyłania powiadomień, a następnie sprawdź potwierdzenie dostarczenia tylko od klientów z tych rejestracji. Np. Jeśli wysyłasz powiadomienie znacznikiem "Sports" wszystkich, które zostały wykonane rejestracji z NH powiedzieć znacznika "Polityka", nie będzie można wysłać do dowolnego urządzenia. Złożonych przypadkach może spowodować wyrażeń znacznika, gdzie możesz tylko zarejestrowana "Znacznika A" lub "B znacznika", ale podczas wysyłania powiadomień są kierowanie "Znacznika & & znacznika B". W poniższej sekcji własny diagnozowanie porady istnieją sposoby, w którym można przeglądać rejestracji wraz z znaczniki, które mają. 

2) **Problemy z szablonu**

Jeśli są używane szablony a następnie upewnij się, że są zgodnie z wytycznymi, opisem w [szablonie wskazówki]. 

3) **Nieprawidłowe rejestracji**

Przy założeniu Centrum powiadomienie zostało skonfigurowane poprawnie i wszystkie znaczniki i znacznika wyrażenia użyto poprawnie uzyskując Znajdź prawidłowych elementów docelowych do których muszą być wysyłane powiadomienia o jego, NH uruchamiana, wyłączanie partie przetwarzanie równolegle - każdej partii wysyłania wiadomości do zestawu rejestracji. 

> [AZURE.NOTE] Ponieważ czynności przetwarzania równolegle, firma Microsoft nie daje gwarancji kolejność, w którym będą dostarczane powiadomienia. 

Azure powiadomień Centrum jest zoptymalizowane pod kątem model dostarczenia wiadomości "raz w większości". Oznacza to, możemy próba do powtarzania tak, aby powiadomienia nie są dostarczane więcej niż jeden raz do urządzenia. Aby zapewnić możemy przejrzeć rejestracji i upewnij się, że tego tylko jedną wiadomość jest wysyłana na identyfikator urządzenia przed faktycznie wiadomość jest wysyłana do obwodowym układzie Nerwowym. Każda partia jest wysyłana do obwodowym układzie Nerwowym, która z kolei jest akceptowania i sprawdzanie poprawności rejestracji, jest to możliwe, że obwodowym układzie Nerwowym wykryje komunikat o błędzie z jednej lub większej liczby rejestracji w ramach partii zwraca błąd Azure NH i zatrzymuje przetwarzanie w związku z tym całkowite usunięcie tej partii. To jest szczególnie z APN używający protokołu strumienia TCP. Mimo że firma Microsoft są zoptymalizowane dla w większości po odbiorcy, w tym przypadku możemy usunąć faulting rejestracji z naszych bazy danych, a następnie ponów próbę dostarczanie powiadomień dla pozostałych urządzeń w tej partii.

Uzyskiwać informacje o błędzie próba dostarczenia nie powiodło się przed rejestracji za pomocą API pozostałe koncentratory powiadomienie Azure: [na telemetrycznego wiadomości: uzyskiwanie powiadomień wiadomości telemetrycznego](https://msdn.microsoft.com/library/azure/mt608135.aspx) i [Obwodowym układzie Nerwowym opinie](https://msdn.microsoft.com/library/azure/mt705560.aspx). Zobacz [SendRESTExample](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/SendRestExample) na przykład kod.

##<a name="pns-issues"></a>Problemy z obwodowym układzie Nerwowym

Po otrzymaniu wiadomości z powiadomieniem o przy odpowiednich obwodowym układzie Nerwowym jest jej odpowiedzialność do przeprowadzania powiadomienie na urządzeniu. Azure koncentratory powiadomień znajduje się poza obrazów i nie ma kontroli po lub jeśli powiadomienie ma być wydana na urządzeniu. Ponieważ usługi powiadomień platformy są bardzo rozbudowane, powiadomienia zwykle do osiągnięcia urządzeń w ciągu kilku sekund z obwodowym układzie Nerwowym. Jeśli ograniczania opcji obwodowym układzie Nerwowym jednak koncentratory powiadomienie Azure stosowane wykładniczego wycofywania strategii i jeśli obwodowym układzie Nerwowym pozostaje niedostępne dla 30 minut następnie mamy zasady w miejscu, aby wygasało i upuść trwale tych wiadomości. 

Jeśli próba dostarczenia powiadomienia obwodowym układzie Nerwowym, ale urządzenie jest w trybie offline, powiadomienie jest przechowywane przez obwodowym układzie Nerwowym ograniczony okres czasu, a następnie dostarczane na urządzeniu, gdy będzie dostępny. Znajduje się tylko jedno powiadomienie ostatnio używane dla określonej aplikacji. Jeśli wiele powiadomienia są wysyłane, gdy urządzenie jest w trybie offline, każdy powiadomienie o nowej powoduje, że wcześniejsze powiadomienie do odrzucenia. To zachowanie pozostawienie tylko najnowsze powiadomienie nazywane tych powiadomień w APN i zwijanie w GCM (który korzysta z klucza zwijania). Jeśli urządzenie pozostaje w trybie offline przez dłuższy czas, powiadomienia zapisanymi są dla niego zostaną odrzucone. Źródło — [wskazówki APN] & [GCM wskazówki]

Z Azure koncentratory powiadomienie — można przekazać klucz coalescing za pośrednictwem nagłówek HTTP za pomocą Typowa `SendNotification` interfejsu API (np .NET SDK — `SendNotificationAsync`) które bierze również nagłówki HTTP, które są przekazywane jako ma odpowiednich obwodowym układzie Nerwowym. 

##<a name="self-diagnose-tips"></a>Własny diagnozowanie porady

W tym miejscu możemy będzie sprawdzenie różnych drogi do diagnozowania i główne powodować jakichkolwiek problemów z Centrum powiadomień:

###<a name="verify-credentials"></a>Sprawdź poświadczenia

1. **Dzięki portalowi deweloperów obwodowym układzie Nerwowym**

    Sprawdź je w odpowiednich obwodowym układzie Nerwowym Deweloper portalu (APN, GCM, WNS itp) przy użyciu naszych [Wprowadzenie samouczki dotyczące].

2. **Portal Azure klasyczny**

    Przejdź do karty konfigurowanie przeglądać i zgodnie z otrzymanymi z portalem Deweloper obwodowym układzie Nerwowym poświadczeń. 

    ![][4]

###<a name="verify-registrations"></a>Weryfikowanie rejestracji

1. **Programu Visual Studio**

    Jeśli korzystasz z programu Visual Studio rozwoju można łączyć się Microsoft Azure i wyświetlanie i zarządzanie więcej usług Azure, w tym powiadomienia Centrum "Eksploratora serwera". Jest to przede wszystkim przydatne dla deweloperów i w środowisku. 

    ![][9]

    Można wyświetlać i zarządzać wszystkich rejestracji w Twoim Centrum, podzielonych właściwego platformy natywnych lub szablonu rejestracji, wszystkie znaczniki, obwodowym układzie Nerwowym identyfikator, identyfikator rejestracji oraz datę wygaśnięcia. Można również edytować rejestracji w czasie rzeczywistym - przydaje się, że, jeśli chcesz edytować dowolne znaczniki. 

    ![][8]
 
    > [AZURE.NOTE] Funkcje programu Visual Studio Edytowanie rejestracji można używać tylko podczas deweloperów test z ograniczoną liczbą rejestracji. W razie konieczności rozwiązać rejestracji zbiorczo, należy rozważyć możliwość korzystania z funkcji rejestracji eksportu/importu opisanych tutaj - [Eksportu/importu rejestracji] (https://msdn.microsoft.com/library/dn790624.aspx)

2. **Eksplorator Bus usługi**

    Wielu klientów za pomocą Eksploratora opisanych tutaj - [ServiceBus Explorer] wyświetlania i zarządzania nimi ich Centrum powiadomienie ServiceBus. Jest udostępniany przez firmę code.microsoft.com - [Kod ServiceBus Eksplorator] projektu Otwórz źródło

###<a name="verify-message-notifications"></a>Sprawdź powiadomień o wiadomościach

1. **Portal Azure klasyczny**

    Możesz na karcie "Debugowanie" na wysyłanie powiadomienia test dla klientów bez wymagających dowolnego wewnętrznej bazy danych usługi w górę i uruchamiania. 

    ![][7]

2. **Programu Visual Studio**

    Możesz też wysyłać powiadomienia test z comforts programu Visual Studio:

    ![][10]

    Więcej w funkcji Eksploratora Visual Studio powiadomień Centrum Azure tutaj - 
    
    - [Omówienie Eksploratora w PORÓWNANIU z serwera]
    - [Wpis w blogu Eksploratora serwera w PORÓWNANIU z-1]
    - [Wpis w blogu Eksploratora serwera w PORÓWNANIU z-2]

###<a name="debug-failed-notifications-review-notification-outcome"></a>Debugowanie powiadomienia o niepowodzeniu / Przejrzyj wyniki powiadomień

**Właściwość EnableTestSend**

Podczas wysyłania powiadomień za pomocą powiadomień koncentratory początkowo go tylko otrzymuje umieszczone w kolejce dla NH czynność przetwarzania ustalanie ich elementów docelowych i ewentualnie NH przesyła go do obwodowym układzie Nerwowym. Oznacza to, że podczas używania interfejsu API usługi REST lub na dowolnej stronie klienta SDK pomyślnego return połączeń Wyślij tylko oznacza, że wiadomość została pomyślnie umieszczona w kolejce w górę z Centrum powiadomienie. Nie udostępnia wgląd co się dzieje podczas NH ostatecznie masz wysłać wiadomość do obwodowym układzie Nerwowym. Jeśli do powiadomienia nie jest trafiać do urządzenia klienta, istnieje możliwość po NH próba dostarczenia wiadomości do obwodowym układzie Nerwowym, wystąpił błąd, na przykład rozmiar ładunku przekracza maksymalną dozwoloną przez obwodowym układzie Nerwowym lub poświadczenia skonfigurowane w NH są nieprawidłowe itp. Aby uzyskać wgląd błędy obwodowym układzie Nerwowym, możemy wprowadziły właściwość o nazwie [funkcji EnableTestSend]. Ta właściwość jest włączany automatycznie po wysłaniu wiadomości testowe z portalu lub klienta programu Visual Studio i w związku z tym umożliwia wyświetlenie szczegółowych informacji debugowania. Za pomocą tego za pośrednictwem interfejsów API w powyższym przykładzie .NET SDK miejsce, w którym będzie dostępny i zostanie dodany do wszystkich SDK klienta po pewnym czasie. Aby użyć to połączenie pozostałych, po prostu dołączyć parametru ciągu kwerendy o nazwie "test" na końcu połączeń Wyślij np. 

    https://mynamespace.servicebus.windows.net/mynotificationhub/messages?api-version=2013-10&test

*Przykład (.NET SDK)*
 
Załóżmy, że używasz .NET SDK wysłać natywnych wyskakującego powiadomienia:

    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName);
    var result = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(result.State);
 
`result.State`zostanie po prostu stanu `Enqueued` na końcu wykonanie bez wgląd co się stało z usługi wypychanych. Teraz możesz wykorzystać `EnableTestSend` właściwość logiczna podczas inicjowania `NotificationHubClient` i można uzyskać szczegółowe informacje o błędach obwodowym układzie Nerwowym napotkał podczas wysyłania powiadomień. Wyślij połączenie tutaj potrwa dodatkowe zwrócić, ponieważ jest to zwracanie tylko po NH został dostarczony powiadomienie do obwodowym układzie Nerwowym, aby określić wyniki. 
 
    bool enableTestSend = true;
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName, enableTestSend);
    
    var outcome = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(outcome.State);
    
    foreach (RegistrationResult result in outcome.Results)
    {
        Console.WriteLine(result.ApplicationPlatform + "\n" + result.RegistrationId + "\n" + result.Outcome);
    }

*Przykładowe wyniki*

    DetailedStateAvailable
    windows
    7619785862101227384-7840974832647865618-3
    The Token obtained from the Token Provider is wrong
 
Ten komunikat oznacza albo nieprawidłowe poświadczenia są skonfigurowane w Centrum powiadomienie lub wystąpił problem z rejestracji koncentratora oraz kursu zalecane jest usuwanie tej rejestracji i poinformuj klienta, utwórz ją ponownie przed wysłaniem wiadomości. 
 
> [AZURE.NOTE] Zauważ, że użycie tej właściwości jest intensywnie ograniczenie i dlatego tylko w należy użyć to środowisko deweloperów/testowanie z ograniczonym zestawem rejestracji. Tylko wyślemy powiadomienia debugowania do 10 urządzeń. Ponadto mamy limit przetwarzania debugowania wysyła się 10 na minutę. 

###<a name="review-telemetry"></a>Przeglądanie danych telemetrycznych 

1. **Używanie Portal Azure klasyczny**

    Portal umożliwia Obejrzyj krótki przegląd wszystkich działań na Twoim Centrum powiadomienie. 
    
    ) z poziomu karty "pulpitu nawigacyjnego" można wyświetlać zagregowany widok rejestracji, powiadomienia, jak również błędów na platformie. 
    
    ![][5]
    
    b) na karcie "Monitor" do szczegółowego podglądu szczególnie błędy określonych obwodowym układzie Nerwowym zwracane po NH próbuje Wysyłanie powiadomienia do obwodowym układzie Nerwowym możesz również dodać wiele innych platform określonej metryki. 
    
    ![][6]
    
    c) należy rozpoczynać przeglądu **Wiadomości przychodzących**, **Operacji rejestracji** **Pomyślnego powiadomienia** i przejdź do na kartę platformy, aby przejrzeć obwodowym układzie Nerwowym konkretnych błędów. 
    
    d) Jeśli Centrum powiadomienie nieprawidłowo skonfigurowane przy użyciu ustawień uwierzytelniania, a następnie zostanie wyświetlony błąd uwierzytelniania obwodowym układzie Nerwowym. To jest dobrym wskaźnikiem, aby sprawdzić poświadczenia obwodowym układzie Nerwowym. 

2) **Programowy dostęp**

W tym miejscu — więcej informacji 

- [Dostęp programistyczny telemetrycznego]
- [Telemetrycznego dostępu za pośrednictwem interfejsów API przykładowe] 

> [AZURE.NOTE] Funkcje związane z kilku telemetrycznego takich jak **Rejestracji eksportu/importu** **Telemetrycznego dostępu za pośrednictwem interfejsów API** itp są dostępne tylko w standardowej warstwy. Podczas próby korzystania z tych funkcji, jeśli znajdujesz się w wolnym lub warstwa podstawowe następnie zostanie wyświetlony komunikat wyjątku w tym celu podczas korzystania z zestawu SDK oraz HTTP 403 (Dostęp zabroniony) podczas ich używania bezpośrednio z pozostałych interfejsy API. Upewnij się, że zostały przeniesione do standardowego poziomu za pośrednictwem klasyczny Portal Azure.  

<!-- IMAGES -->
[0]: ./media/notification-hubs-diagnosing/Architecture.png
[1]: ./media/notification-hubs-diagnosing/GCMConfigure.png
[2]: ./media/notification-hubs-diagnosing/GCMEnable.png
[3]: ./media/notification-hubs-diagnosing/GCMServerKey.png
[4]: ./media/notification-hubs-diagnosing/PortalConfigure.png
[5]: ./media/notification-hubs-diagnosing/PortalDashboard.png
[6]: ./media/notification-hubs-diagnosing/PortalMonitoring.png
[7]: ./media/notification-hubs-diagnosing/PortalTestNotification.png
[8]: ./media/notification-hubs-diagnosing/VSRegistrations.png
[9]: ./media/notification-hubs-diagnosing/VSServerExplorer.png
[10]: ./media/notification-hubs-diagnosing/VSTestNotification.png
 
<!-- LINKS -->
[Omówienie koncentratory powiadomień]: notification-hubs-push-notification-overview.md
[Samouczki z wprowadzeniem]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Wskazówki dotyczące szablonu]: https://msdn.microsoft.com/library/dn530748.aspx 
[Wskazówki APN]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW4
[Wskazówki GCM]: http://developer.android.com/google/gcm/adv.html
[Eksportu/importu rejestracji]: http://msdn.microsoft.com/library/dn790624.aspx
[Eksplorator ServiceBus]: http://msdn.microsoft.com/library/dn530751.aspx
[Kod ServiceBus Eksploratora]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Explorer-f2abca5a
[Omówienie Eksploratora w PORÓWNANIU z serwera]: http://msdn.microsoft.com/library/windows/apps/xaml/dn792122.aspx 
[Wpis w blogu Eksploratora serwera w PORÓWNANIU z-1]: http://azure.microsoft.com/blog/2014/04/09/deep-dive-visual-studio-2013-update-2-rc-and-azure-sdk-2-3/#NotificationHubs 
[Wpis w blogu Eksploratora serwera w PORÓWNANIU z-2]: http://azure.microsoft.com/blog/2014/08/04/announcing-release-of-visual-studio-2013-update-3-and-azure-sdk-2-4/ 
[Funkcja EnableTestSend]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx
[Dostęp programistyczny telemetrycznego]: http://msdn.microsoft.com/library/azure/dn458823.aspx
[Telemetrycznego dostępu za pośrednictwem interfejsów API próbki]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel

 