<properties
    pageTitle="Jak wysyłać powiadomienia o zaplanowanych | Microsoft Azure"
    description="W tym temacie opisano zaplanowane powiadomienia za pomocą koncentratorów powiadomienie Azure."
    services="notification-hubs"
    documentationCenter=".net"
    keywords="powiadomienia push, powiadomienia wypychane, planowanie powiadomienia wypychane"
    authors="ysxu"
    manager="erikre"
    editor=""/>
<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="how-to-send-scheduled-notifications"></a>Jak: Wysyłanie powiadomienia według harmonogramu


##<a name="overview"></a>Omówienie

Jeśli masz scenariusza, w którym chcesz wysłać powiadomienie w pewnym momencie w przyszłości, ale nie jest łatwym sposobem wznowić pracę w górę kodzie wewnętrznej, aby wysłać powiadomienie. Standardowa warstwa koncentratory powiadomienie obsługuje funkcję, która umożliwia zaplanowanie powiadomienia w górę do 7 dni w przyszłości.

Wysyłając powiadomienie, że po prostu użyć w SDK koncentratory powiadomienie klasy [ScheduledNotification](https://msdn.microsoft.com/library/microsoft.azure.notificationhubs.schedulednotification.aspx) jak pokazano w poniższym przykładzie:

    Notification notification = new AppleNotification("{\"aps\":{\"alert\":\"Happy birthday!\"}}");
    var scheduled = await hub.ScheduleNotificationAsync(notification, new DateTime(2014, 7, 19, 0, 0, 0));

Ponadto możesz anulować poprzednio zaplanowanym powiadomienie przy użyciu jego notificationId:

    await hub.CancelNotificationAsync(scheduled.ScheduledNotificationId);

Istnieją nie ograniczenia liczby powiadomienia według harmonogramu, które możesz wysłać.