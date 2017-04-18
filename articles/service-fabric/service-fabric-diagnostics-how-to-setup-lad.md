<properties
   pageTitle="Zbieranie dzienników przy użyciu zestawu narzędzi Diagnostyka Linux Azure | Microsoft Azure"
   description="W tym artykule opisano, jak skonfigurować diagnostyki Azure do zbierania dzienników od klastrze Linux tkaninie usługa działa w Azure."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/28/2016"
   ms.author="subramar"/>


# <a name="collect-logs-by-using-azure-diagnostics"></a>Zbieranie dzienników przy użyciu zestawu narzędzi Diagnostyka Azure

> [AZURE.SELECTOR]
- [Systemu Windows](service-fabric-diagnostics-how-to-setup-wad.md)
- [Linux](service-fabric-diagnostics-how-to-setup-lad.md)

Gdy korzystasz z klastrem tkaninie usługi Azure jest dobrym pomysłem jest zbieranie dzienników ze wszystkich węzłów w centralnej lokalizacji. O dziennikach w centralnej lokalizacji ułatwia analizowanie i rozwiązywanie problemów, czy są one w usług, aplikacji lub klastrem. Jednym ze sposobów przekazywanie i zbieranie dzienników ma rozszerzenie diagnostyki Azure przekazywanie dzienników do magazynu Azure za pomocą. Można czytać zdarzenia z magazynu i umieść je w produkcie, takich jak [Elastyczne wyszukiwania](service-fabric-diagnostic-how-to-use-elasticsearch.md) lub inne rozwiązanie analizowania dziennika.

## <a name="log-sources-that-you-might-want-to-collect"></a>Dziennik źródeł, które mają być zbierane
- **Dzienniki usługi tkaninie**: emisji z platformy za pośrednictwem [LTTng](http://lttng.org) i przekazać do swojego konta miejsca do magazynowania. Dzienniki można zdarzenia operacyjne lub obsługi zdarzeń, które emituje platformy. Dzienniki te są przechowywane w lokalizacji, w której manifeście klaster. (Aby uzyskać szczegółowe informacje o koncie miejsca do magazynowania, wyszukaj znacznika **AzureTableWinFabETWQueryable** i poszukaj **StoreConnectionString**).
- **Zdarzenia aplikacji**: emisji z kodu tej usługi. Możesz użyć każdego rozwiązania rejestrowania zapisuje pliki dziennika tekstowych — na przykład LTTng. Aby uzyskać więcej informacji zobacz dokumentację LTTng na śledzenie aplikacji.  


## <a name="deploy-the-diagnostics-extension"></a>Wdrażanie rozszerzenia diagnostyki
Zbieranie dzienników pierwszym krokiem jest wdrażanie rozszerzenia diagnostyki na każdej maszyny wirtualne w klastrze tkaninie usługi. Rozszerzenie diagnostyki zbieranie dzienników na poszczególnych maszyn wirtualnych i przekazuje je do konta miejsca do magazynowania, określonym przez użytkownika. Czynności różnią się w zależności od tego, czy używane Azure portal lub Azure Menedżera zasobów.

Aby wdrożyć rozszerzenia diagnostyki maszyny wirtualne w klastrze jako część tworzenia klaster, ustaw **diagnostyki** **na**. Po utworzeniu klaster, nie można zmienić to ustawienie, za pomocą portalu.

Skonfiguruj diagnostyki Azure Linux (LAD) do gromadzić pliki i umieść je do swojego konta miejsca do magazynowania. Ten proces jest wyjaśnione jako Scenariusz 3 ("przekazywanie plików dziennika") w artykule [Przy użyciu LAD do monitorowania i diagnozowanie maszyny wirtualne Linux](../virtual-machines/virtual-machines-linux-classic-diagnostic-extension.md). Wykonując ten proces uzyskuje dostęp do śledzenia. Możesz przekazać śledzenia do Wizualizator wybranych przez użytkownika.

Można także wdrożyć rozszerzenia diagnostyki za pomocą Menedżera zasobów Azure. Proces przypomina dla systemu Windows i Linux i opisano dla klastrów systemu Windows w [sposób zbierania dzienników Diagnostyka Azure](service-fabric-diagnostics-how-to-setup-wad.md).

Umożliwia także pakiet zarządzania operacji, zgodnie z opisem w [Operacji zarządzania pakietu dziennika analizy z systemem Linux](https://blogs.technet.microsoft.com/hybridcloud/2016/01/28/operations-management-suite-log-analytics-with-linux/).

Po zakończeniu tej konfiguracji agenta LAD monitoruje określonej dzien. Gdy nowy wiersz jest dołączany do pliku, tworzy wpis syslog, na który są wysyłane do przechowywania określony.


## <a name="next-steps"></a>Następne kroki
Aby dowiedzieć się bardziej szczegółowo zdarzenia, jakie należy przejrzeć podczas rozwiązywania problemów, zobacz [Dokumentacja LTTng](http://lttng.org/docs) i [Przy użyciu LAD](../virtual-machines/virtual-machines-linux-classic-diagnostic-extension.md).
