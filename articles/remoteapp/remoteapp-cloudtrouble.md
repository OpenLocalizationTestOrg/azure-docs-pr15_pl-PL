
<properties
    pageTitle="Rozwiązywanie problemów z RemoteApp chmury zbiory — tworzenie | Microsoft Azure"
    description="Dowiedz się, jak rozwiązywać problemy tworzenia zbioru RemoteApp z chmury"
    services="remoteapp"
    documentationCenter=""
    authors="vkbucha"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="troubleshoot-creating-remoteapp-cloud-collections"></a>Rozwiązywanie problemów z tworzenia zbiorów chmury RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Jeśli masz problemy tworzenia zbioru chmury, zapoznaj się z następujących informacji.

## <a name="your-image-is-invalid"></a>Obraz jest nieprawidłowy ##
Jeśli zobaczysz komunikat podobny do "GoldImageInvalid" podczas oczekiwania Azure do zapewniania obsługi kolekcji, oznacza to, że obraz szablonu nie spełnia [określonych wymagań obrazu](remoteapp-imagereqs.md). Tak Przejdź przeczytaj tych [wymagań](remoteapp-imagereqs.md), rozwiązywanie obrazu i spróbuj utworzyć kolekcji ponownie.

## <a name="common-errors-seen-in-the-azure-management-portal"></a>Typowe błędy w portalu zarządzania Azure

    DNS server could not be reached
    ProvisioningTimeout

Kolekcje w chmurze często błędów podczas tworzenia ze względu na możesz używają niestandardowych obrazów.  Jeśli widzisz jedną z powyższych błędów i używasz obraz niestandardowy, aby utworzyć zbiór, sprawdź następujące elementy:

- Upewnij się, że obraz niestandardowy, które zostały przekazane spełnia wymagania obrazu.
- Najczęściej używane typowych problemów to, że obraz nie został poprawnie przetworzonej przez program Sysprep.  
- Sprawdź obrazu można uruchomić w ramach funkcji Hyper-V lub spróbuj utworzyć maszyn wirtualnych IAAS bezpośrednio w subskrypcji usługi Azure za pomocą obrazu. Jeśli maszyn wirtualnych nie powiedzie się uruchomić i nie zaczynają się, następnie zazwyczaj oznacza, że obraz niestandardowy nie został poprawnie przygotowany.  Sprawdź obraz niestandardowy został utworzony, wykonując jak utworzenie niestandardowego szablonu dla RemoteApp

Jeśli korzystasz z jednego z obrazów Microsoft dołączone do subskrypcji, spróbuj utworzyć zbiór ponownie. Jeśli problem będzie nadal występował, skontaktuj się pomocy technicznej firmy Microsoft.

    PlatformImageTrialModeOnly

Jeśli zostanie wyświetlony ten błąd zazwyczaj oznacza to, która została uaktualniona do płatnej konta, ale chcesz użyć obrazu firmę Microsoft, który jest prawidłowy tylko w trybie wersji próbnej usługi. W takim przypadku spróbuj utworzyć kolekcji chmury ponownie, ale pamiętaj określić prawidłowy obraz.
