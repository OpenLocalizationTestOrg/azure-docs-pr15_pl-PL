<properties
    pageTitle="Zarządzanie Bus usługi Azure za pomocą automatyzacji Azure | Microsoft Azure"
    description="Dowiedz się, jak zarządzać Bus usługi Azure za pomocą usługi Azure automatyzacji."
    services="service-bus, automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="magoedte;csand"/>

# <a name="managing-azure-service-bus-using-azure-automation"></a>Zarządzanie Bus usługi Azure za pomocą automatyzacji Azure

Ten przewodnik podstawowe informacje na temat usług Azure Automation Services i jak on używany w celu uproszczenia zarządzania Bus usługi Azure.

## <a name="what-is-azure-automation"></a>Co to jest Azure automatyzacji?

[Automatyzacja Azure](../automation/automation-intro.md) jest usługą Azure dotyczące uproszczenia zarządzania chmurze przez proces automatyzacji i konfiguracji pożądany stan. Przy użyciu automatyzacji Azure ręcznego, powtarzać, długim i podatne na błędy zadania można zautomatyzować można zwiększyć niezawodność, wydajność i wartość czasu dla Twojej organizacji.

Automatyzacja Azure udostępnia aparat wykonania wysoce niezawodne, wysokiej dostępności przepływu pracy, który skale stosownie do potrzeb. W automatyzacji Azure procesów może być kopać ręcznie, systemów 3rd firm lub zaplanowanymi interwałami tak, aby zadania wystąpić dokładnie w razie potrzeby.

Zmniejszyć operacyjne i zwolnić IT i personelu DevOps skoncentrować się na pracy, które dodaje wartości biznesowej, przenosząc zadań zarządzania chmury mógł być automatycznie uruchamiany przez automatyzacji Azure.

## <a name="how-can-azure-automation-help-manage-azure-service-bus"></a>Jak automatyzacji Azure ułatwia zarządzanie Bus usługi Azure?

Możesz zarządzać Bus usługi przy użyciu automatyzacji Azure za pomocą [Interfejsu API usługi REST Bus usługi](https://msdn.microsoft.com/library/azure/mt639375.aspx). W automatyzacji Azure można uruchomić skrypty środowiska PowerShell do wykonywania wielu zadań Bus usługi przy użyciu interfejsu API usługi REST. Można także skojarzyć te wywołania interfejsu API usługi REST w automatyzacji Azure z poleceń cmdlet dla innych usług Azure, do automatyzowania zadań złożonych 3 systemów firmy i usług Azure.

Oto kilka przykładów przy użyciu programu PowerShell do zarządzania Bus usługi Azure:

* [Niestandardowych poleceń cmdlet środowiska PowerShell do zarządzania kolejek Bus usługi Azure](https://blogs.technet.microsoft.com/uktechnet/2014/12/04/sample-of-custom-powershell-cmdlets-to-manage-azure-servicebus-queues)
* [Jak utworzyć Bus usługi kolejek, tematy i subskrypcji za pomocą skryptu programu PowerShell](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [Tworzenie nazw Bus usługi Azure przy użyciu programu PowerShell](http://buildazure.com/2015/09/24/create-azure-service-bus-namespaces-using-powershell-and-x-plat-cli/)

Praca z usługi Azure bus w runbooks automatyzacji modułu programu PowerShell można pobrać z [Galerii programu PowerShell](https://www.powershellgallery.com/packages/AzureServiceBusCreation/1.0).


## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już podstawy automatyzacji Azure i jak może być używana do zarządzania Bus usługi Azure, skorzystaj z poniższych łączy, aby dowiedzieć się więcej na temat automatyzacji Azure.

* Azure automatyzacji [wprowadzenie samouczka](../automation/automation-first-runbook-graphical.md)
* Zobacz jak [zarządzać Bus usługi przy użyciu programu PowerShell](service-bus-powershell-how-to-provision.md)
