<properties
   pageTitle="Rozwiązywanie problemów za pomocą śledzenia zdarzeń | Microsoft Azure"
   description="Najczęstsze problemy napotykane podczas wdrażania usługi na tkaninie usługi Azure firmy Microsoft."
   services="service-fabric"
   documentationCenter=".net"
   authors="mattrowmsft"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/31/2016"
   ms.author="mattrow"/>


# <a name="troubleshoot-common-issues-when-you-deploy-services-on-azure-service-fabric"></a>Rozwiązywanie typowych problemów podczas wdrażania usługi na tkaninie usługi Azure

Jeśli korzystasz z usług na komputerze dewelopera, jest łatwe w użyciu [Narzędzia debugowania Visual Studio](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). W przypadku zdalnego klastrów [Raporty dotyczące kondycji](service-fabric-view-entities-aggregated-health.md) są zawsze dobre miejsce do rozpoczęcia. Najprostszym sposobem uzyskiwać dostęp do tych raportów jest za pomocą programu PowerShell lub [SFX](service-fabric-visualizing-your-cluster.md). W tym artykule założono, że debugowania klaster zdalny i masz już podstawy, jak używać jednej z tych narzędzi.

##<a name="application-crash"></a>Awarie aplikacji
"Partition znajduje się poniżej docelowe replice lub wystąpienie liczby" raport jest dobrym wskaźnikiem, który powoduje awarię usługi. Aby dowiedzieć się, gdzie awarii usługi trwa nieco więcej dochodzenia. Po uruchomieniu usługi w większej skali znajomego najlepsze będzie zestaw śledzenia dobrze przemyślany.  Sugerujemy, spróbuj [Diagnostyki Azure](service-fabric-diagnostics-how-to-setup-wad.md) zbierania tych śledzenia i wyświetlania i wyszukiwanie śledzenia przy użyciu rozwiązanie, takich jak [Elastyczne wyszukiwania](service-fabric-diagnostic-how-to-use-elasticsearch.md) .

![Kondycja partycją SFX](./media/service-fabric-diagnostics-troubleshoot-common-scenarios/crashNewApp.png)

###<a name="during-service-or-actor-initialization"></a>Podczas inicjowania usługi lub aktor
Wyjątki od przed zainicjowano typ usługi spowoduje, że proces awarie. Dla tych typów awarii dziennik zdarzeń aplikacji zostanie wyświetlona błąd z tej usługi.
Są to najczęściej używane wyjątków, aby zobaczyć przed zainicjowaniu usługi.

***System.IO.FileNotFoundException***

Ten błąd jest zazwyczaj z powodu braku zależności zestawu. Sprawdź właściwość ma parametru CopyLocal Visual Studio lub w pamięci podręcznej zestawów globalnych dla węzła.

***System.Runtime.InteropServices.COMException***
 *w System.Fabric.Interop.NativeRuntime+IFabricRuntime.RegisterStatefulServiceFactory (IntPtr, IFabricStatefulServiceFactory)*
 
 Oznacza to, że nazwa typu usługi zarejestrowanych nie odpowiada manifestu usługi.

[Diagnostyka Azure](service-fabric-diagnostics-how-to-setup-wad.md) można skonfigurować automatyczne przekazywanie wszystkich węzły w dzienniku zdarzeń aplikacji.

###<a name="runasync-or-onactivateasync"></a>RunAsync() lub OnActivateAsync()
W przypadku awarii podczas inicjowania lub wykonania aktora lub typ usługi zarejestrowanych, wyjątek będą podlegać tkaninie usługi Azure. Można wyświetlić te od dostawców źródła zdarzeń szczegółowe w sekcji "Następne kroki".

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej na temat istniejących diagnostyki dostarczony przez tkaninie usługi:

* [Niezawodne diagnostyki uczestników](service-fabric-reliable-actors-diagnostics.md)
* [Niezawodne diagnostyki usług](service-fabric-reliable-services-diagnostics.md)
