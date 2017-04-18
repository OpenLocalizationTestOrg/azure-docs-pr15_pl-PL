<properties
   pageTitle="Skalowanie klastrze tkaninie usługi lub pomniejszyć | Microsoft Azure"
   description="Skalowanie klastrze tkaninie usługi lub pomniejszyć zgodnie z żądanie, ustawiając automatyczne skalowanie reguły dla każdego zestawu węzeł typu/m Skala. Dodawanie lub usuwanie węzły do klastrów tkaninie usługi"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="chackdan"/>


# <a name="scale-a-service-fabric-cluster-in-or-out-using-auto-scale-rules"></a>Skalowanie klastrze tkaninie usługi lub pomniejszyć przy użyciu reguł automatyczne skalowanie

Zestawy skali maszyn wirtualnych są zasobu Azure obliczeń, który umożliwia wdrażanie i zarządzanie zbiorem maszyn wirtualnych jako zestaw. Każdego typu węzeł, która jest zdefiniowana w klastrze tkaninie usługi jest skonfigurowana jako oddzielny zestaw skali maszyn wirtualnych. Każdego typu węzła można następnie skalować w lub się niezależnie od tego, mają różne zestawy otwarte porty i nie może mieć inną pojemność metryki. Dowiedz się więcej o go w dokumencie [nodetypes tkaninie usługi](service-fabric-cluster-nodetypes.md) . Ponieważ tkaninie usługi typy węzłów w klastrze zostały wprowadzone skali maszyn wirtualnych zestawy w wewnętrznej bazie danych, należy skonfigurować reguły automatyczne skalowanie dla każdego zestawu węzeł typu/m Skala.

>[AZURE.NOTE] Twoja subskrypcja może zawierać za mało rdzenie, aby dodać nowe maszyny wirtualne, które tworzą dany klaster. Brak sprawdzania poprawności modelu obecnie jest, więc zostanie wyświetlony błąd czasu wdrożenia, jeśli są trafienie wymienionych limitów przydziału.

## <a name="choose-the-node-typevm-scale-set-to-scale"></a>Wybrana skala typ/m węzeł ustaw skalowanie

Obecnie nie jest możliwe do określania zasad automatyczne skalowanie zestawy skali maszyn wirtualnych za pomocą portalu, więc nam typy węzła listy, a następnie dodaj reguły automatyczne skalowanie do nich za pomocą Azure programu PowerShell (1.0 +).

Aby uzyskać listę zestawu skali maszyn wirtualnych, które składają się klaster, uruchom następujące polecenia cmdlet:

```powershell
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Compute/VirtualMachineScaleSets

Get-AzureRmVmss -ResourceGroupName <RGname> -VMScaleSetName <VM Scale Set name>
```

## <a name="set-auto-scale-rules-for-the-node-typevm-scale-set"></a>Ustawianie reguł automatyczne skalowanie w zestawie węzłów typu/m skali

Jeśli klaster zawiera wiele typów węzeł, następnie powtórzyć dla każdej skali typy/m węzeł powoduje chcesz skalować (lub pomniejszyć). Brać pod uwagę liczby węzłów należy przed skonfigurowaniem automatyczne skalowanie. Minimalna liczba węzłów, które musi być typu główny węzeł wynika wybrany poziom niezawodności. Dowiedz się więcej na temat [poziomów niezawodności](service-fabric-cluster-capacity.md).

>[AZURE.NOTE]  Skalowania główny węzeł wpisz na wartość mniejszą niż minimalna liczba wybierz klaster nietrwałe lub przesuń go w dół. Może to spowodować utratę danych dla aplikacji i usług systemowych.

Funkcja Automatyczne skalowanie nie jest obecnie prowadzone przez obciążenia, które aplikacje mogą raportowania na tkaninie usługi. Dlatego obecnie skali automatyczne Ci czysto wynika wydajność liczniki, które są wysyłane przez każdą maszyn wirtualnych skalowanie wystąpienia zestawu.  

Wykonaj te instrukcje dotyczące [konfigurowania automatycznego skali dla każdego zestawu skali maszyn wirtualnych](../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md).

>[AZURE.NOTE] W skali w dół scenariusza chyba że typu węzeł poziomu wytrzymałości złoty lub Silver musisz zadzwonić do polecenia [cmdlet Usuń ServiceFabricNodeState](https://msdn.microsoft.com/library/azure/mt125993.aspx) o nazwie odpowiedni węzeł.

## <a name="manually-add-vms-to-a-node-typevm-scale-set"></a>Ręczne dodawanie maszyny wirtualne do zestawu węzłów typu/m skali

Wykonaj próbki i instrukcje w [galerii szablonów szybki start](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) , aby zmienić liczbę maszyny wirtualne w każdym typ węzła. 

>[AZURE.NOTE] Dodawanie maszyny wirtualne trwa tak nie będzie dodatki należy chwilową. Dlatego planowanie stoisz również w czasie, aby umożliwić ponad 10 minut, zanim możliwości maszyn wirtualnych jest dostępna dla repliki / usługi wystąpienia punkty umieszczenie.

## <a name="manually-remove-vms-from-the-primary-node-typevm-scale-set"></a>Ręczne usuwanie maszyny wirtualne z zestawu skali główny węzeł typu/m

>[AZURE.NOTE] Usługi systemu tkaninie usługi są uruchamiane w polu wpisz podstawowy węzeł w klastrze. Aby nigdy nie należy zamknąć lub przeskalować liczbę wystąpień w tym typy węzłów poniżej poziomu niezawodności gwarantuje. Zapoznaj się z [szczegółowych informacji na temat poziomów niezawodności tutaj](service-fabric-cluster-capacity.md). 

Należy wykonać następujące czynności jednego maszyn wirtualnych wystąpienia naraz. Dzięki temu usługi systemowe (i stanowe usług) można zamknąć bezpiecznie na wystąpienie maszyn wirtualnych, które są usuwane i nowych replik utworzonych w innych węzłach.

1. Uruchom [ServiceFabricNode Wyłącz](https://msdn.microsoft.com/library/mt125852.aspx) z zamiarem "RemoveNode" Wyłącz węzeł zamierzasz usunąć (najwyższe wystąpienia tego typu węzeł).

2. Uruchom [Get-ServiceFabricNode](https://msdn.microsoft.com/library/mt125856.aspx) , aby upewnić się, że węzeł faktycznie przeszła na wyłączone. W przeciwnym razie poczekaj, aż węzeł jest wyłączona. Nie można Pospiesz się ten krok.

2. Wykonaj podane próbki i instrukcje w [galerii szablonów szybki start](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) , aby zmienić liczbę pośrednictwem SMS, w tym typ węzła. Wystąpienia usunięte jest najwyższe wystąpienie maszyn wirtualnych. 

3. Powtórz kroki od 1 do 3, stosownie do potrzeb, ale nigdy nie przeskalować liczbę wystąpień typów główny węzeł mniejsze niż poziom niezawodności gwarantuje w dół. Zapoznaj się z [szczegółowych informacji na temat poziomów niezawodności tutaj](service-fabric-cluster-capacity.md). 

## <a name="manually-remove-vms-from-the-non-primary-node-typevm-scale-set"></a>Ręczne usuwanie maszyny wirtualne z zestawu skali typ/m węzeł inne niż podstawowe

>[AZURE.NOTE] Stanowe usługi należy niektórych liczby węzłów maksymalnie zawsze należy zachować dostępność i zachować stan usługi. W bardzo minimalne konieczne liczby węzłów równe statystykę docelowej replice zestawu partition/usługi. 

Potrzebujesz wykonaj następujące kroki jednego maszyn wirtualnych wystąpienia naraz. Dzięki temu usługi systemowe (i stanowe usług) można zamknąć bezpiecznie w wystąpieniu maszyn wirtualnych, które są usuwane, a gdzie jeszcze utworzona nowych replik.

1. Uruchom [ServiceFabricNode Wyłącz](https://msdn.microsoft.com/library/mt125852.aspx) z zamiarem "RemoveNode", aby wyłączyć węzeł zamierzasz usunąć (najwyższe wystąpienia tego typu węzeł).

2. Uruchom [Get-ServiceFabricNode](https://msdn.microsoft.com/library/mt125856.aspx) , aby upewnić się, że węzeł faktycznie przeszła na wyłączone. W przeciwnym razie poczekaj, aż do przeprowadzenia wyłączeniu węzła. Nie można Pospiesz się ten krok.

2. Wykonaj podane próbki i instrukcje w [galerii szablonów szybki start](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) , aby zmienić liczbę pośrednictwem SMS, w tym typ węzła. Spowoduje to usunięcie teraz najwyższe wystąpienie maszyn wirtualnych. 

3. Powtórz kroki od 1 do 3, stosownie do potrzeb, ale nigdy nie przeskalować liczbę wystąpień typów główny węzeł mniejsze niż poziom niezawodności gwarantuje w dół. Zapoznaj się z [szczegółowych informacji na temat poziomów niezawodności tutaj](service-fabric-cluster-capacity.md).

## <a name="behaviors-you-may-observe-in-service-fabric-explorer"></a>Zachowanie, które mogą obserwować w Eksploratorze tkaninie usługi

Gdy rozbudowy klastrze Eksploratora tkaninie usługi zostaną zastosowane liczby węzłów (maszyn wirtualnych Skala Ustaw wystąpień), które są częścią klaster.  Jednak podczas skalowania klaster w dół możesz Zobacz wystąpienia usunięte węzeł/m wyświetlane w stanie nieprawidłowe, chyba że połączeń [cmd ServiceFabricNodeState Usuń](https://msdn.microsoft.com/library/mt125993.aspx) o nazwie odpowiedni węzeł.   

Oto opis tego zachowania.

Węzły w Eksploratorze tkaninie usługi są odbicie jakich usług systemu tkaninie usługi (FM specjalnie) zna liczby węzłów klastrze sprzedał/występują. Skalowanie skali maszyn wirtualnych określone maszyn wirtualnych został usunięty, ale usługa systemowa UKF nadal uznaje, że węzeł (który został mapowany maszyn wirtualnych, który został usunięty) będzie wróć. Dlatego Eksploratora tkaninie usługi są nadal wyświetlane węzła (chociaż stan kondycji może być błąd lub nieznany).

Aby upewnić się, że węzeł zostaną usunięte po usunięciu maszyny, masz dwie opcje:

1) Wybieranie poziomu wytrzymałości złoty lub Silver (dostępne wkrótce) dla typów węzeł w klastrze, który umożliwia integracji infrastruktury. Która następnie automatycznie usunie węzłów z naszych stanu usług (FM) systemu podczas skalowania w dół.
Odwołują się do [szczegółowych informacji na temat poziomów wytrzymałości](service-fabric-cluster-capacity.md)

2) Po wystąpieniu maszyn wirtualnych skalowania w dół, których trzeba zadzwonić polecenia [cmdlet ServiceFabricNodeState Usuń](https://msdn.microsoft.com/library/mt125993.aspx).

>[AZURE.NOTE] Klastrów tkaninie usługi wymagają określonej liczbie węzły wynosi cały czas w celu zapewnienia dostępności i zachować state - określane jako "zachowaniu kworum". Tak jest zazwyczaj niebezpiecznych zamknąć wszystkie komputery w klastrze, chyba że najpierw wykonaniu [pełnej kopii zapasowej według stanu](service-fabric-reliable-services-backup-restore.md).

## <a name="next-steps"></a>Następne kroki
Przeczytaj poniższe czynności, aby także informacje na temat Planowanie pojemności klaster, uaktualniania klastrze i podziału usług:

- [Planowanie możliwości klaster](service-fabric-cluster-capacity.md)
- [Klaster uaktualnienia](service-fabric-cluster-upgrade.md)
- [Partycją stanowe usługi związane z maksymalną skalę](service-fabric-concepts-partitioning.md)

<!--Image references-->
[BrowseServiceFabricClusterResource]: ./media/service-fabric-cluster-scale-up-down/BrowseServiceFabricClusterResource.png
[ClusterResources]: ./media/service-fabric-cluster-scale-up-down/ClusterResources.png
