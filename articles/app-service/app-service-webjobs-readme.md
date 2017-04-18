<properties
    pageTitle="WebJobs w usłudze Azure aplikacji"
    description="Dowiedz się, jak tworzyć WebJobs Uruchom testy tło, interakcje z usługami, takimi jak usługi Bus i magazynowania oraz tworzenia zaplanowanych zadań."
    services="app-service"
    documentationCenter=""
    authors="christopheranderson"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/10/2015"
    ms.author="chrande"/>

# <a name="using-webjobs-in-azure-app-service"></a>WebJobs przy użyciu Azure aplikacji usługi

Ten artykuł zawiera linki do dokumentacji zasobów dotyczących sposobu używania Azure WebJobs i Azure WebJobs SDK. Azure WebJobs stanowią łatwy sposób na uruchamianie skryptów lub programów jako procesy w tle na [Sieci Web usługi aplikacji](http://go.microsoft.com/fwlink/?LinkId=529714). Możesz przekazać i uruchom plik wykonywalny, takich jak jako cmd bat, exe (.NET), ps1, Pokaż, php, Kopiuj, js i słoik. Te programy uruchamiane jako WebJobs według harmonogramu (cron) albo w sposób ciągły.

WebJobs SDK ułatwia korzystania z magazynu Azure. WebJobs SDK ma powiązanie i system wyzwalacza, który współdziała z Microsoft Azure miejsca do magazynowania blob, kolejkach i tabele, a także kolejek Bus usług.

Tworzenie, wdrażania i zarządzania WebJobs jest bezproblemowa przy użyciu zintegrowanego narzędzia w programie Visual Studio. Tworzenie WebJobs przy użyciu szablonów, publikowanie i zarządzanie nimi (Uruchom/Zatrzymaj/monitor debugowanie) je.

Pulpit nawigacyjny WebJobs w portalu Azure zapewnia możliwości zarządzania zaawansowanych, przedstawiające pełną kontrolę nad realizacji WebJobs, łącznie z możliwością wywołania poszczególnych funkcji w WebJobs. Pulpit nawigacyjny wyświetla także obsługi funkcji i rejestrowanie danych wyjściowych.

[AZURE.INCLUDE [app-service-blueprint-webjobs](../../includes/app-service-blueprint-webjobs.md)]
