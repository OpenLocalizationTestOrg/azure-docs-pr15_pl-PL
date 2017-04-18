<properties
    pageTitle="Co należy zrobić w przypadku awarii magazyn Azure | Microsoft Azure"
    description="Co należy zrobić w przypadku awarii magazyn Azure"
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>


# <a name="what-to-do-if-an-azure-storage-outage-occurs"></a>Co zrobić, jeśli występuje awaria magazyn Azure

Firma Microsoft pracujemy nad trwałym upewnij się, że naszych usług są zawsze dostępne. Czasami wymusza poza naszych wpływ sterowania nam w sposób spowodować dostawie niezaplanowane usługi w jednej lub większej liczbie regionów. Ułatwiające obsługę tych wystąpień rzadkich, firma Microsoft udostępnia poniższe wskazówki wysokiego poziomu usług Azure miejsca do magazynowania.

## <a name="how-to-prepare"></a>Jak przygotować 

Jest krytyczne dla każdego klienta do przygotowania planu odzyskiwania własnych danych. Starań, aby odzyskać awaria miejsca do magazynowania zwykle obejmuje zarówno personel, jak i procedury automatycznego Aby ponownie aktywować aplikacji w stanie działa. Można znaleźć w dokumentacji Azure poniżej, aby utworzyć własny planu odzyskiwania danych:

-   [Odzyskiwanie i wysoką dostępność dla aplikacji Azure](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)

-   [Wytyczne techniczne elastyczność Azure](../resiliency/resiliency-technical-guidance.md)

-   [Usługa Azure Odzyskiwanie witryny](https://azure.microsoft.com/services/site-recovery/)

-   [Azure replikacji miejsca do magazynowania](storage-redundancy.md)

-   [Usługa Azure kopii zapasowej](https://azure.microsoft.com/services/backup/)

## <a name="how-to-detect"></a>Jak wykryć 

Zalecany sposób, aby sprawdzić stan usługi Azure jest subskrybowanie [Pulpit nawigacyjny kondycji usługi Azure](https://azure.microsoft.com/status/).

## <a name="what-to-do-if-a-storage-outage-occurs"></a>Co zrobić, jeśli występuje awaria miejsca do magazynowania

Jeśli co najmniej jednej usługi miejsca do magazynowania jest tymczasowo niedostępny w jednej lub większej liczbie regionów, dostępne są dwie opcje dla Ciebie. Jeśli jest to wymagane natychmiastowy dostęp do danych, należy rozważyć opcję 2.

### <a name="option-1-wait-for-recovery"></a>Opcja 1: Poczekaj, aż odzyskiwania

W tym przypadku jest wymagane żadne działanie ze strony użytkownika. Pracujemy nad dokładnie przywrócenie dostępność usługi Azure. Można monitorować stan usługi w [Pulpit nawigacyjny kondycji usługi Azure](https://azure.microsoft.com/status/).

### <a name="option-2-copy-data-from-secondary"></a>Opcja 2: Skopiuj dane z pomocniczej

Jeśli wybierzesz [odczytu zbędne geo miejsca do magazynowania (Pomoc Zdalna GRS)](storage-redundancy.md#read-access-geo-redundant-storage) (zalecane) dla kont usługi przechowywania będzie zawierał dostęp do odczytu danych z obszaru pomocniczą. Za pomocą narzędzi, takich jak [AzCopy](storage-use-azcopy.md), [Azure programu PowerShell](storage-powershell-guide-full.md)i [przenoszenia danych Azure biblioteki](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/) , skopiuj dane z pomocniczym regionu do innego konta miejsca do magazynowania w regionie unimpacted, a następnie kliknij polecenie aplikacji do tego konta miejsca do magazynowania dla obu odczytywanie i zapisywanie dostępności.

## <a name="what-to-expect-if-a-storage-failover-occurs"></a>Czego można oczekiwać, jeśli występuje trybie awaryjnym miejsca do magazynowania

Jeśli zdecydujesz [zbędnych Geo przestrzeni dyskowej (GRS)](storage-redundancy.md#geo-redundant-storage) lub [odczytu zbędne geo miejsca do magazynowania (Pomoc Zdalna GRS)](storage-redundancy.md#read-access-geo-redundant-storage) (zalecane), Magazyn Azure powoduje, że dane trwałe w dwóch regionów (głównego i pomocniczego). W obu regionów magazyn Azure stale przechowuje wielu replikach danych.

Gdy regionalne danych wpływa na obszar podstawowego, firma Microsoft będzie próbowała przywrócenia usługi w danym regionie. Zależne od rodzaju danych i jego skutki, w niektórych przypadkach rzadkich firma Microsoft może okazać się niemożliwe Przywracanie obszaru podstawowego. W tym momencie możemy wykona geo trybie awaryjnym. Replikacja danych między region jest proces asynchroniczny, które obejmują opóźnienie, dzięki czemu jest możliwe, że zmiany, które nie zostały replikowane do obszaru pomocniczej mogą zostać utracone. Kwerendy można ["ostatniej synchronizacji Time" konta miejsca do magazynowania](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/) , aby uzyskać szczegółowe informacje o stanie replikacji.

Oto kilka punktów dotyczące obsługi przypadku awarii geo miejsca do magazynowania:

-   Miejsca do magazynowania przypadku awarii geo tylko zostanie wyzwolony przez zespół magazyn Azure — nie jest wymagana żadna akcja klienta.

-   Istniejące punkty końcowe usługi miejsca do magazynowania, dla obiektów blob, tabele, kolejkach i plików pozostaną niezmienione po przełączeniu; wpis DNS muszą zostać zaktualizowane, aby przełączyć się z obszaru podstawowego obszaru pomocniczą.

-   Przed i w przypadku awarii geo nie mieć dostęp do zapisu do swojego konta miejsca do magazynowania z powodu wpływu danych, ale nadal uzyskasz z pomocniczej Jeśli skonfigurowano konto miejsca do magazynowania jako GRS pomoc Zdalna.

-   Po zakończeniu przypadku awarii geo i zmiany DNS propagowane, będzie można wznowić do odczytu i zapisu do swojego konta miejsca do magazynowania. Kwerendy można ["ostatniej Geo pracy awaryjnej Time" konta miejsca do magazynowania](https://msdn.microsoft.com/library/azure/ee460802.aspx) , aby uzyskać więcej szczegółowych informacji.

-   Po przełączeniu konta miejsca do magazynowania będzie działać w pełni, ale o stanie "ograniczone" jego jest faktycznie hostowana w regionie autonomicznego z nie możliwości geo replikacji. Zmniejszeniu tego ryzyka, możemy przywrócić oryginalny region podstawowego i wykonaj awarii geo, aby przywrócić stan pierwotny. W przypadku odzyskać oryginalny podstawowy region, możemy będą przydzielane innego regionu pomocniczej.
Aby uzyskać więcej informacji na infrastrukturę replikacji geo magazyn Azure przeczytaj artykuł w blogu zespołu miejsca do magazynowania o [opcji nadmiarowości i GRS pomoc Zdalna](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/).

##<a name="best-practices-for-protecting-your-data"></a>Najważniejsze wskazówki dotyczące ochrony danych

Istnieje kilka metod zalecane do tworzenia kopii zapasowych danych miejsca do magazynowania na bieżąco.

-   Dyski maszyn wirtualnych — za pomocą [usługi Azure kopii zapasowej](https://azure.microsoft.com/services/backup/) do tworzenia kopii zapasowych maszyn wirtualnych dysków Azure maszyn wirtualnych.

-   Blokowanie blob — tworzenie [migawki](https://msdn.microsoft.com/library/azure/hh488361.aspx) poszczególnych obiektów blob blok lub kopiowanie obiektów blob na inny nośnik kont w innym regionie przy użyciu [AzCopy](storage-use-azcopy.md), [Azure programu PowerShell](storage-powershell-guide-full.md)lub [bibliotece Azure przenoszenia danych](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/).

-   Tabele — umożliwia [AzCopy](storage-use-azcopy.md) eksportowania danych tabeli na inne konto miejsca do magazynowania w innym regionie.

-   Pliki — użyj [AzCopy](storage-use-azcopy.md) lub [Azure programu PowerShell](storage-powershell-guide-full.md) , aby skopiować pliki z innym kontem miejsca do magazynowania w innym regionie.
