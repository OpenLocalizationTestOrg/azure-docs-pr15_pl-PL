<properties
 pageTitle="Odwołanie polecenia cmdlet programu PowerShell harmonogramu"
 description="Odwołanie polecenia cmdlet programu PowerShell harmonogramu"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="scheduler-powershell-cmdlets-reference"></a>Odwołanie polecenia cmdlet programu PowerShell harmonogramu

W poniższej tabeli opisano i odwołuje się do strony odwołania każdego z głównych poleceń cmdlet w harmonogramie Azure.

Aby zainstalować Azure programu PowerShell i skojarzyć subskrypcji Azure, zobacz, [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md). 

Aby uzyskać więcej informacji na temat [polecenia cmdlet Menedżera zasobów Azure](https://msdn.microsoft.com/library/mt125356\(v=azure.200\).aspx)zobacz [Za pomocą programu Azure przy użyciu Menedżera zasobów Azure](../powershell-azure-resource-manager.md).

|Polecenie cmdlet|Opis polecenia cmdlet|
|---|---|
[Wyłącz AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490133\(v=azure.200\).aspx) |Wyłącza zbioru zadania. 
[Włącz AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490135\(v=azure.200\).aspx) |Udostępnia kolekcję zadania.
[Get-AzureRmSchedulerJob](https://msdn.microsoft.com/library/mt490125\(v=azure.200\).aspx) |Otrzymuje zadania harmonogramu.
[Get-AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490132\(v=azure.200\).aspx) |Pobiera zadania zbiorów.
[Get-AzureRmSchedulerJobHistory](https://msdn.microsoft.com/library/mt490126\(v=azure.200\).aspx) |Pobiera Historia zadania.
[Nowy AzureRmSchedulerHttpJob](https://msdn.microsoft.com/library/mt490136\(v=azure.200\).aspx) |Tworzy zadanie HTTP.
[Nowy AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490141\(v=azure.200\).aspx) |Tworzy zbiór zadania.
[Nowy AzureRmSchedulerServiceBusQueueJob](https://msdn.microsoft.com/library/mt490134\(v=azure.200\).aspx) |Tworzy zadanie kolejki bus usługi.
[Nowy AzureRmSchedulerServiceBusTopicJob](https://msdn.microsoft.com/library/mt490142\(v=azure.200\).aspx) |Tworzy zadanie tematu bus usługi.
[Nowy AzureRmSchedulerStorageQueueJob](https://msdn.microsoft.com/library/mt490127\(v=azure.200\).aspx) |Tworzy zadanie kolejki miejsca do magazynowania. 
[Usuń AzureRmSchedulerJob](https://msdn.microsoft.com/library/mt490140\(v=azure.200\).aspx) |Usuwa zadanie harmonogramu.  
[Usuń AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490131\(v=azure.200\).aspx) |Usuwa zbiór zadania. 
[Ustawianie AzureRmSchedulerHttpJob](https://msdn.microsoft.com/library/mt490130\(v=azure.200\).aspx) |Modyfikuje zadanie HTTP harmonogram.
[Ustawianie AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490129\(v=azure.200\).aspx) |Modyfikuje zbioru zadania. 
[Ustawianie AzureRmSchedulerServiceBusQueueJob](https://msdn.microsoft.com/library/mt490143\(v=azure.200\).aspx) |Modyfikuje zadanie kolejki bus usługi.  
[Ustawianie AzureRmSchedulerServiceBusTopicJob](https://msdn.microsoft.com/library/mt490137\(v=azure.200\).aspx) |Modyfikuje zadanie usługi bus tematu. 
[Ustawianie AzureRmSchedulerStorageQueueJob](https://msdn.microsoft.com/library/mt490128\(v=azure.200\).aspx) |Modyfikuje zadanie kolejki magazynu.   

Aby uzyskać więcej szczegółowych informacji można uruchomić dowolną z następujących poleceń cmdlet: 

```
Get-Help <cmdlet name> -Detailed
```
```
Get-Help <cmdlet name> -Examples
```
```
Get-Help <cmdlet name> -Full
```

## <a name="see-also"></a>Zobacz też


 [Co to jest harmonogram?](scheduler-intro.md)

 [Azure pojęcia harmonogram, terminologia i hierarchia jednostki](scheduler-concepts-terms.md)

 [Wprowadzenie do korzystania z harmonogramu w portalu Azure](scheduler-get-started-portal.md)

 [Plany i rozliczenia w harmonogramie Azure](scheduler-plans-billing.md)

 [Odwołanie interfejsu API usługi REST harmonogram Azure](https://msdn.microsoft.com/library/mt629143)

 [Azure Harmonogram dostępności i niezawodności](scheduler-high-availability-reliability.md)

 [Azure limity harmonogram, wartości domyślne i kody błędów](scheduler-limits-defaults-errors.md)

 [Azure uwierzytelniania ruchu wychodzącego harmonogramu](scheduler-outbound-authentication.md)
