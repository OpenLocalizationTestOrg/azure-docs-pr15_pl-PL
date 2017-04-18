<properties
   pageTitle="Zwróć uwagę, 1 wycofywanie gościnny system operacyjny rodziny | Microsoft Azure"
   description="Informacje na temat kiedy się stało wycofywanie Azure gościa OS rodziny 1 i jak ustalić, jeśli występuje"
   services="cloud-services"
   documentationCenter="na"
   authors="raiye"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="10/24/2016"
   ms.author="raiye"/>



# <a name="guest-os-family-1-retirement-notice"></a>Powiadomienie o wycofywanie 1 rodziny systemu operacyjnego gościa

Wycofywanie 1 rodziny OS najpierw zostało ogłoszone 1 czerwca 2013.

**2 marzec 2014 r.** System operacyjny gościa Azure (z systemem operacyjnym gościa) rodziny 1.x, na którym jest oparty na systemie operacyjnym Windows Server 2008, urzędowo została wycofana. Wszystkie próby wdrażanie nowych usług i uaktualnianie istniejących usług przy użyciu 1 rodziny nie powiedzie się komunikat o błędzie informujący o tym, 1 rodziny systemu operacyjnego gościa została wycofana.

**3 listopada 2014 r.** Rozszerzona obsługa 1 rodziny systemu operacyjnego gościa zakończone i jest w pełni wycofana. Wszystkie usługi nadal na 1 rodzina będzie mieć negatywny wpływ. Firma Microsoft może przestać tych usług w dowolnym momencie. Nie jest gwarantowana, które usług będzie nadal działać, dopóki nie zostanie ręcznie uaktualniona je samodzielnie.

Jeśli masz dodatkowe pytania, odwiedź [Fora usługi w chmurze](http://social.msdn.microsoft.com/Forums/home?forum=windowsazuredevelopment&filter=alltypes&sort=lastpostdesc) lub [kontakt z pomocą techniczną Azure](https://azure.microsoft.com/support/options/).




## <a name="are-you-affected"></a>Możesz występuje?

Usług w chmurze występuje, jeśli ma zastosowanie jednego z następujących czynności:

1. Użytkownik ma wartość "osFamily ="1"jawnie wymienione w pliku ServiceConfiguration.cscfg dla usługi w chmurze.
2. Nie ma wartość osFamily jawnie wymienione w pliku ServiceConfiguration.cscfg dla usługi w chmurze. Obecnie system używa wartości domyślnej "1" w tym przypadku.
3. Azure portalu klasyczny Wyświetla wartość rodziny systemu operacyjnego gościa jako "Windows Server 2008".

Aby dowiedzieć się, które z usługami w chmurze są uruchomione które rodziny systemu operacyjnego, można uruchamiać skrypt poniżej w programie PowerShell Azure, ale najpierw [skonfigurować Azure programu PowerShell](../powershell-install-configure.md) . Dodatkowe szczegóły dotyczące skrypt, zobacz [Azure gościa OS rodziny 1 zakończenia życia: 2014 czerwca](http://blogs.msdn.com/b/ryberry/archive/2014/04/02/azure-guest-os-family-1-end-of-life-june-2014.aspx). 

```Powershell
foreach($subscription in Get-AzureSubscription) {
    Select-AzureSubscription -SubscriptionName $subscription.SubscriptionName

    $deployments=get-azureService | get-azureDeployment -ErrorAction Ignore | where {$_.SdkVersion -NE ""}

    $deployments | ft @{Name="SubscriptionName";Expression={$subscription.SubscriptionName}}, ServiceName, SdkVersion, Slot, @{Name="osFamily";Expression={(select-xml -content $_.configuration -xpath "/ns:ServiceConfiguration/@osFamily" -namespace $namespace).node.value }}, osVersion, Status, URL
}
```

Usług w chmurze wpłynie implementacja wycofywanie OS rodziny 1 Jeśli kolumna osFamily w wyjściowe skrypt jest pusta lub zawiera wartość "1".

## <a name="recommendations-if-you-are-affected"></a>Zalecenia, jeśli to dotyczy

Zaleca się migrowanie poszczególnych ról usługi w chmurze do jednego z obsługiwanych rodzin z systemem operacyjnym gościa:

**System operacyjny gościa 4.x rodziny** -Windows Server 2012 R2 *(zalecane)*

1. Upewnij się, że aplikacja korzysta SDK 2.1 lub nowszej .NET framework 4.0, 4,5 lub 4.5.1.
2. Atrybut osFamily na "4" w pliku ServiceConfiguration.cscfg i ponownie wdróż usługi w chmurze.


**System operacyjny gościa rodziny 3.x** -Windows Server 2012

1. Upewnij się, że aplikacja korzysta SDK 1.8 lub nowszy z .NET framework 4.0 lub 4,5.
2. Atrybut osFamily do "3" w pliku ServiceConfiguration.cscfg i ponownie wdróż usługi w chmurze.


**System operacyjny gościa 2.x rodziny** -Windows Server 2008 R2

1. Upewnij się, że aplikacja korzysta SDK 1.3 i z wyższymi z .NET framework 3.5 lub 4.0.
2. Atrybut osFamily do "2" w pliku ServiceConfiguration.cscfg i ponownie wdróż usługi w chmurze.


## <a name="extended-support-for-guest-os-family-1-ended-nov-3-2014"></a>Rozszerzona obsługa 1 rodziny systemu operacyjnego gościa zakończone 3 lis 2014 r.
Usługi cloud services na system operacyjny gościa rodzina 1 nie są już obsługiwane. Przeprowadź migrację wyłączyć rodzinie 1 tak szybko, jak to możliwe, aby uniknąć awarii usługi.  

## <a name="next-steps"></a>Następne kroki
Przejrzyj najnowsze [wydanie system operacyjny gościa](cloud-services-guestos-update-matrix.md).
