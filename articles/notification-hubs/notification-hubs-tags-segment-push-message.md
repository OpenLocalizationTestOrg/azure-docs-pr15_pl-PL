<properties
    pageTitle="Routing i wyrażeń znacznika"
    description="W tym temacie wyjaśniono wyrażenia routingu i znacznika dla koncentratorów Azure powiadomienie."
    services="notification-hubs"
    documentationCenter=".net"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="routing-and-tag-expressions"></a>Wyrażenia routingu i znaczników

##<a name="overview"></a>Omówienie

Znacznik wyrażeń umożliwia docelowej zestawów określonych urządzeń lub bardziej szczegółowo rejestracji podczas wysyłania powiadomienia wypychane za pośrednictwem koncentratory powiadomienie.


## <a name="targeting-specific-registrations"></a>Kierowanie określonych rejestracji

Jedynym sposobem docelowej określonych powiadomienie o rejestracji ma znaczniki można skojarzyć z nimi skierować te znaczniki. Zgodnie z opisem w [Zarządzaniu rejestracji](notification-hubs-push-notification-registration-management.md), aby otrzymywać powiadomienia wypychane aplikacja ma zarejestrować uchwyt urządzenia koncentratora powiadomienie. Po utworzeniu rejestracji na koncentratora powiadomienie wewnętrznej bazy danych aplikacji można wysyłać powiadomienia wypychane do niej.
Wewnętrznej bazy danych aplikacji można wybrać rejestracji do miejsca docelowego z określonym powiadomienie w następujący sposób:

1. **Emisja**: wszystkich rejestracji w Centrum powiadomienie otrzyma powiadomienie.
2. **Znacznik**: wszystkich rejestracji, które zawierają określony znacznik otrzyma powiadomienie.
3. **Wyrażenie etykiety**: wszystkich rejestracji, którego zestaw znaczników odpowiadać określone wyrażenie otrzyma powiadomienie.

## <a name="tags"></a>Znaczniki

Znacznik może być dowolny ciąg znaków do 120, zawierające alfanumeryczny i następujące znaki niealfanumeryczne: "_" ‘@’, "#", ".",":", "-". W poniższym przykładzie pokazano aplikacja, z której możesz odbierać wyskakującego powiadomienia o grupach określonych muzyki. W tym scenariuszu w prosty sposób rozsyłania powiadomienia jest rejestracji etykiety przy użyciu tagów reprezentujące różne pasma, jak pokazano na poniższej ilustracji.

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags.png)

Na tym obrazie wiadomości oznaczonych **Beatles** osiągnie tylko tabletu zarejestrowana z tagiem **Beatles**.

Aby uzyskać więcej informacji o tworzeniu rejestracji dla znaczników zobacz [Zarządzanie rejestracji](notification-hubs-push-notification-registration-management.md).

Powiadomienia można wysłać do znaczników przy użyciu metody powiadomienia Wyślij `Microsoft.Azure.NotificationHubs.NotificationHubClient` klasy w [Microsoft Azure powiadomienie koncentratory](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) SDK. Umożliwia także Node.js lub API pozostałych powiadomienia Push.  Oto przykład przy użyciu zestawu SDK.


    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You requested a Beatles notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Beatles");

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You requested a Wailers notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Wailers");




Znaczniki nie musi być wstępnie ustanawianie i może odnosić się do wielu pojęcia specyficzne dla aplikacji. Na przykład użytkownicy tej aplikacji przykład można komentarz grup i chce się otrzymywać toasts, nie tylko dla komentarzy w swoich ulubionych grup, ale również dla wszystkich komentarzy z ich znajomych, bez względu na to grupy, na którym są komentowania. Na poniższej ilustracji przedstawiono przykład w tym scenariuszu:



![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags2.png)

Na tym obrazie Alicja chce aktualizacje Beatles i Robert chce aktualizacje Wailers. Roman jest również zainteresować marek na komentarze i marek znajdują się w chcą Wailers. Gdy powiadomienie zostanie wysłane dla osoby marek komentowanie Beatles, zarówno Alicja i Robert otrzymywać go.

Czy kodowanie wielu problemów w znaczniki (na przykład "band_Beatles" lub "follows_Charlie"), znaczniki są proste ciągi i nie właściwości z wartościami. Rejestracja jest takie samo tylko na obecność lub brak konkretnego znacznika.

Samouczek pełnego krok po kroku dotyczące wysyłania do grup odsetki za pomocą znaczników zobacz [Przerywanie wiadomości](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md).


## <a name="using-tags-to-target-users"></a>Przy użyciu tagów użytkowników docelowych

Innym sposobem używania znaczników jest zidentyfikowanie wszystkich urządzeń danego użytkownika. Rejestracji może być oznakowanie przy znacznika, który zawiera identyfikator użytkownika, jak pokazano na poniższej ilustracji:


![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags3.png)

Na tym obrazie uid:Alice wiadomości oznakowane osiągnie wszystkich uid:Alice oznakowane rejestracji; w związku z tym wszystkich urządzeń Alicji.


##<a name="tag-expressions"></a>Znacznik wyrażeń

Istnieją przypadki, w których powiadomienie ma kierowania zestawu rejestracji opisanej nie przez jeden znacznik, ale przez wyrażenie logiczne na znaczniki.

Należy rozważyć aplikację sports wysyłające przypomnienia dla wszystkich osób w Boston o gry między ustawą Sarbanes-Oxley czerwony i Cardinals. Jeśli aplikacja klienta rejestruje znaczników dotyczących zainteresowanie zespołów i lokalizacji, powiadomienie należy przeznaczone dla wszystkich osób w Boston, kto jest zainteresować ustawą Sarbanes-Oxley w kolorze czerwonym lub Cardinals. Z następujące wyrażenie logiczne może być wyrażona za pomocą tego warunku:

    (follows_RedSox || follows_Cardinals) && location_Boston


![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags4.png)

Wyrażenia znacznika może zawierać wszystkich operatorów logicznych, takich jak i (& &), lub (|), a nie (!). Mogą także zawierać nawiasów. Wyrażenia znacznika są ograniczone do 20 znaczniki, jeśli zawierają one tylko ORs; w przeciwnym razie są ograniczone do 6 znaczniki.

Oto przykład wysyłania powiadomień przy użyciu wyrażeń znacznika przy użyciu zestawu SDK.


    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    String userTag = "(location_Boston && !follows_Cardinals)"; 

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You want info on the Red Socks</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You want info on the Red Socks</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);
