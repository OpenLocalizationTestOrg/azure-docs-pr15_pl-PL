<properties 
pageTitle="Obsługi zdarzeń cyklu świadczenia usługi w chmurze | Microsoft Azure" 
description="Dowiedz się, jak można metody cyklu życia rolę usługi w chmurze w .NET" 
services="cloud-services" 
documentationCenter=".net" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="09/06/2016" 
ms.author="adegeo"/>

# <a name="customize-the-lifecycle-of-a-web-or-worker-role-in-net"></a>Dostosowywanie do zarządzania cyklem życia roli sieci Web lub pracownika w .NET

Po utworzeniu roli Pracownik rozszerzanie klasy [RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx) , która zawiera metody pominięcia, które umożliwiają reagowanie na zdarzenia cyklu życia. Dla ról w sieci web tej klasy jest opcjonalne, trzeba go używać reagować na zdarzenia czasu trwania.

## <a name="extend-the-roleentrypoint-class"></a>Rozszerzenie klasy RoleEntryPoint

Klasy [RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx) zawiera metody, które są nazywane przez Azure po jego **uruchamia**, **uruchamia**lub **zatrzymuje** roli sieci web lub pracownika. Opcjonalnie można zastąpić te metody zarządzania inicjowanie roli, roli zamknięcia sekwencji lub wątku wykonywania roli. 

Rozszerzanie **RoleEntryPoint**, należy pamiętać o następujących zachowań metod:

-   Metody [OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) i [OnStop](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) zwracać wartość logiczną, co zwraca **wartość false** z następujących metod.

     Jeśli kod zwraca **wartość false**, roli nieprawidłowo zakończenia procesu, bez obaw o dowolnej zamykania, który może być w miejscu. Ogólnie należy unikać zwraca **wartość false** z metody **OnStart** .
     
-   Każdy nie przechwycony wyjątek w przeciążeń metodę **RoleEntryPoint** jest traktowana jako nieobsługiwany wyjątek.

     Jeśli wystąpi wyjątek w jednej z metod cyklu życia, Azure będzie podwyższanie zdarzenia [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) , a następnie zakończenia procesu. Po swojej roli podjęciu trybu offline, zostanie uruchomiony ponownie przez Azure. W przypadku wystąpienia nieobsługiwanego wyjątku [Zatrzymywanie](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.stopping.aspx) wywołaniu nie zdarzenia, a metoda **OnStop** nie jest wywoływana.

Jeśli Twoja rola nie rozpocznie się lub jest odtwarzanie między inicjowania, zajęty, a stany zatrzymania, kodzie może Zgłaszanie wyjątku nieobsługiwanego w jednej z każdym uruchomieniu roli zdarzenia cyklu życia. W tym przypadku użyć zdarzenia [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) , aby ustalić przyczynę wyjątku i obsługiwać go odpowiednio. Twoja rola może również zwracać z [uruchomić](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metodę, która powoduje, że roli go ponownie. Aby uzyskać więcej informacji na temat stanów wdrażania zobacz [Typowe problemy którego przyczyna ról do Kosza](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md).

> [AZURE.NOTE] Jeśli korzystasz z **Narzędzia Azure dla programu Microsoft Visual Studio** do tworzenia aplikacji, szablony projektów roli automatycznie rozszerzenia klasy **RoleEntryPoint** , w plikach *WebRole.cs* i *WorkerRole.cs* .

## <a name="onstart-method"></a>Metoda OnStart

Metoda **OnStart** jest wywoływana, gdy wystąpienia roli tryb online przez Azure. Gdy wykonuje kod OnStart wystąpienie roli został oznaczony jako **zajęty** i nie zewnętrznych ruch będzie przekierowywany do niego przez usługę równoważenia obciążenia. Ta metoda do wykonywania pracy inicjowania, takich jak implementacji obsługi zdarzeń i uruchamianie [Diagnostyki Azure](cloud-services-how-to-monitor.md)można zastąpić.

Jeśli **OnStart** zwraca **wartość PRAWDA**, pomyślnie zainicjowano wystąpienia i Azure wywołuje metodę **RoleEntryPoint.Run** . Jeśli **OnStart** zwraca **wartość false**, roli kończy się natychmiast, bez wykonywania dowolnej sekwencji planowanego wyłączenia.

W poniższym przykładzie pokazano, jak zastąpić metodę **OnStart** . Ta metoda konfiguruje i uruchamia diagnostyczne monitor, gdy wystąpienie roli zaczyna się i konfiguruje przeniesienia rejestrowania danych do konta miejsca do magazynowania:

```csharp
public override bool OnStart()
{
    var config = DiagnosticMonitor.GetDefaultInitialConfiguration();

    config.DiagnosticInfrastructureLogs.ScheduledTransferLogLevelFilter = LogLevel.Error;
    config.DiagnosticInfrastructureLogs.ScheduledTransferPeriod = TimeSpan.FromMinutes(5);

    DiagnosticMonitor.Start("DiagnosticsConnectionString", config);

    return true;
}
```

## <a name="onstop-method"></a>Metoda OnStop

Metoda **OnStop** jest wywoływana po wystąpieniu roli podjęciu offline przez Azure i przed zamknięciu procesu. Ta metoda połączenie kodu wymaganego do roli wystąpienia do zamknięty można zastąpić.

> [AZURE.IMPORTANT] Kod uruchomiony w metodzie **OnStop** ma przez ograniczony czas, aby zakończyć, gdy jest wywoływana ze względów innych niż zamknięcia zainicjowany przez użytkownika. Po upływie tego czasu, proces zostanie zakończone, więc należy upewnić się, kodu w polu może metody **OnStop** Szybkie uruchamianie lub zaakceptować nie działa do zakończenia. Metoda **OnStop** jest wywoływana po **zatrzymywania** wywołaniu zdarzenia.


## <a name="run-method"></a>Metoda Run

Można zastąpić metody **Run** do wykonania wątku długotrwałe wystąpienia roli.

Zastępowanie metody **Run** nie jest wymagana. Domyślna implementacja zaczyna się wątku, który jest w stanie uśpienia zawsze. Jeśli zastąpić metody **Run** , czas nieokreślony zablokować kodu. Jeśli zwraca metody **Run** , roli zostanie automatycznie bezpiecznie odtworzony; innymi słowy Azure podnosi **Zatrzymywanie** zdarzeń i wywołuje metodę **OnStop** tak, aby usługi sekwencji zamknięcia mogą być wykonywane przed roli do trybu offline.


### <a name="implementing-the-aspnet-lifecycle-methods-for-a-web-role"></a>Implementacji metody cyklu życia ról w sieci web programu ASP.NET

Metody cyklu życia ASP.NET oprócz określone przez klasę **RoleEntryPoint** umożliwia zarządzanie inicjowanie i zamknięcia sekwencji dla ról w sieci web. Może to być przydatne do celów zgodności, jeśli są przenoszenia istniejącą aplikację ASP.NET Azure. Metody cyklu życia programu ASP.NET zostaną wywołane wewnątrz metody **RoleEntryPoint** . **Aplikacji\_rozpocząć** metoda jest wywoływana po zakończeniu metody **RoleEntryPoint.OnStart** . **Aplikacji\_zakończenia** metoda jest wywoływana przed wywoływana jest metoda **RoleEntryPoint.OnStop** .

## <a name="next-steps"></a>Następne kroki
Dowiedz się, jak [utworzyć pakiet z chmury](cloud-services-model-and-package.md).