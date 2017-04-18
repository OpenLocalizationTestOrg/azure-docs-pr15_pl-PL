<properties
    pageTitle="Przenoszenie danych do i z magazynu Azure | Microsoft Azure"
    description="W tym artykule omówiono różne metody przenoszenie danych do i z magazynu Azure."
    services="storage"
    documentationCenter=""
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="micurd"/>

# <a name="moving-data-to-and-from-azure-storage"></a>Przenoszenie danych do i z magazynu platformy Azure

Jeśli chcesz przenieść danych lokalnych do magazynu Azure (lub odwrotnie), ma na różne sposoby, w tym celu. Metody, która najbardziej Ci odpowiada zależy od rozwiązania. Ten artykuł zawiera krótkie omówienie różnych scenariuszach i odpowiednie oferty dla każdego z nich.

## <a name="building-applications"></a>Budowanie aplikacji

Jeśli tworzysz aplikację opracowywania przed interfejsu API usługi REST lub jeden z naszych wiele bibliotek klienta jest to doskonały sposób na przenoszenie danych do i z miejsca do magazynowania Azure.

Azure magazynowania obsługuje bibliotek klienta wzbogaconego .NET, iOS, Java, Android, Universal platformy systemu Windows (UWP), Xamarin, C++, Node.JS, PHP, dopiskiem i Python. Bibliotek klienta oferują zaawansowane funkcje, takie jak ponów próbę logicznych, rejestrowanie i równoległych przekazywania. Można także tworzyć bezpośrednio z interfejsu API usługi REST, który może być wywoływana przez dowolny język, który sprawia, że żądania HTTP/HTTPS.

Zobacz [Rozpoczynanie pracy z magazynem obiektów Blob platformy Azure](storage-dotnet-how-to-use-blobs.md) , aby dowiedzieć się więcej.

Ponadto Oferujemy także [Biblioteki przepływu Azure miejsca do magazynowania danych](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement) , która jest przeznaczona do wysokiej wydajności kopiowania danych z platformy Azure biblioteki. Zapoznaj się z naszą Biblioteka przepływ danych [dokumentacji](https://github.com/Azure/azure-storage-net-data-movement) , aby dowiedzieć się więcej. 

## <a name="quickly-viewinginteracting-with-your-data"></a>Szybkie wyświetlanie i interakcja z danymi

Jeśli chcesz łatwy sposób wyświetlania danych magazyn Azure mając także możliwość przekazywania i pobierania danych, rozważ, za pomocą Eksploratora magazynu Azure.

Zapoznaj się z naszą listę [Azure eksploratorów przestrzeni dyskowej](storage-explorers.md) , aby dowiedzieć się więcej.

## <a name="system-administration"></a>Administracja

Jeśli wymagają lub są sprawniej przy użyciu narzędzia wiersza polecenia (np. administratorów), Oto kilka opcji do przemyślenia:

### <a name="azcopy"></a>AzCopy

AzCopy jest przeznaczona do wysokiej wydajności kopiowania danych z magazynu Azure narzędzie wiersza polecenia systemu Windows. Możesz również skopiować danych w ramach konta miejsca do magazynowania lub między kontami innego miejsca do magazynowania.

Aby dowiedzieć się więcej, zobacz [Przenoszenie danych za pomocą narzędzia wiersza polecenia AzCopy](storage-use-azcopy.md) .

### <a name="azure-powershell"></a>Azure programu PowerShell

Azure PowerShell to modułu, który zawiera polecenia cmdlet zarządzania usług Azure. To opartym na zadaniu powłoka wiersza polecenia i języka skryptowego zaprojektowane specjalnie do administrowania systemem.

Zobacz [Przy użyciu programu PowerShell Azure z nośnikami Azure](storage-powershell-guide-full.md) , aby dowiedzieć się więcej.

### <a name="azure-cli"></a>Polecenie Azure

Polecenie Azure udostępnia zestaw Otwórz źródło między platformami polecenia dotyczące pracy z usług Azure. Polecenie Azure jest dostępna w systemie Windows, OSX i Linux.

Zobacz [za pomocą interfejsu wiersza polecenia Azure z nośnikami Azure](storage-azure-cli.md) , aby dowiedzieć się więcej.

## <a name="moving-large-amounts-of-data-with-a-slow-network"></a>Przenoszenie dużych ilości danych z wolno działającej sieci

Jedną z najważniejszych wyzwania związane z przenoszeniem dużych ilości danych jest czasu przeniesienia. Jeśli chcesz pobrać dane z magazynu w Azure bez martwiąc kosztami sieci lub pisania kodu Azure Importuj/Eksportuj to właściwe rozwiązanie.

Zobacz [Azure Importuj/Eksportuj](storage-import-export-service.md) , aby dowiedzieć się więcej.

## <a name="backing-up-your-data"></a>Wykonywanie kopii zapasowych danych

Jeśli chcesz po prostu wykonaj kopię zapasową danych w celu przechowywania Azure, Azure kopii zapasowej jest tak, aby przejść. To zaawansowane rozwiązanie do tworzenia kopii zapasowych danych lokalnych i maszyny wirtualne Azure.

Zobacz [Kopia zapasowa Azure](../backup/backup-introduction-to-azure-backup.md) , aby dowiedzieć się więcej.

## <a name="accessing-your-data-on-premises-and-from-the-cloud"></a>Uzyskiwanie dostępu do usługi danych lokalnych i z chmury

Rozwiązanie w razie potrzeby uzyskiwania dostępu do danych lokalnych i w chmurze, następnie należy użyć w Azure hybrydowych chmury masowej, StorSimple. To rozwiązanie składa się z urządzenia fizycznego StorSimple, że sposób inteligentny Sklepy często używane dane na twarde SSD, czasami używane dane dysków twardych i nieaktywny-kopii zapasowych i archiwizacji danych ilość miejsca do magazynowania Azure.

Zobacz [StorSimple](../storsimple/storsimple-overview.md) , aby dowiedzieć się więcej.

## <a name="recovering-your-data"></a>Odzyskiwanie danych

Jeśli masz obciążenia lokalnego i aplikacje, konieczne będzie rozwiązanie, które umożliwia firmie kontynuowanie pracy w przypadku awarii. Odzyskiwanie witryny Azure obsługuje replikacji, pracy awaryjnej i odzyskiwanie maszyn wirtualnych i serwerów fizycznych. Replikowane dane są przechowywane w magazynie Azure, co pozwala uniknąć dla pomocniczą na miejscu centrum danych.

Zobacz [Odzyskiwanie witryny Azure](../site-recovery/site-recovery-overview.md) , aby dowiedzieć się więcej.
