
<properties
    pageTitle="Odzyskiwanie witryny Azure: Maszyn wirtualnych skopiowanych funkcji Hyper-V na serwerze VMM | Microsoft Azure"
    description="Ten artykuł zawiera opis sposobu replikacji maszyn wirtualnych funkcji Hyper-V, gdy masz tylko jeden serwer VMM."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="backup-recovery"
    ms.date="08/24/2016"
    ms.author="raynew"/>

#  <a name="replicate-hyper-v-virtual-machines-on-a-single-vmm-server"></a>Replikacja maszyn wirtualnych funkcji Hyper-V na serwerze VMM

W tym artykule opisano sposób odtworzyć znajduje się na serwerze hosta funkcji Hyper-V w chmurze VMM, gdy masz tylko jeden serwer VMM we wdrożeniu programu maszyn wirtualnych funkcji Hyper-V.

Azure ma dwa różne [modeli wdrażania](../resource-manager-deployment-model.md) służące do tworzenia i pracy z zasobami: Menedżer zasobów Azure i klasyczny. Azure też ma dwa portali — portalu klasyczny Azure obsługuje model klasyczny wdrożenia i portal Azure z obsługą obu modeli wdrożenia. Ten artykuł zawiera instrukcje dotyczące konfigurowania replikacji w portalu Azure.


Jeśli masz pytania po przeczytaniu w tym artykule publikowanie ich w komentarzach Disqus u dołu tego artykułu lub na [forum usługi Azure odzyskiwania](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Omówienie

Jeśli dane mają być replikowane funkcji Hyper-V maszyny wirtualne znajdujące się w funkcji Hyper-V hostów VMM i masz tylko jeden serwer VMM, można [replikować Azure](site-recovery-vmm-to-azure.md), lub między chmury na serwerze VMM pojedynczy.

Zaleca się, że replikacji Azure ponieważ awaryjnego i odzyskiwania nie są Bezproblemowa podczas replikacji między chmury i liczba ręczne są wymagane. Jeśli chcesz odtworzyć przy użyciu tylko serwer VMM, można wykonaj następujące czynności:

- Replikować z serwerem VMM pojedynczego autonomicznego
- Powielić z jednego serwera VMM wdrożony w klastrze rozciągnięty systemu Windows


## <a name="replicate-across-sites-with-a-single-standalone-vmm-server"></a>Odtworzyć w witrynach z serwerem VMM pojedynczego autonomicznego

![Serwera wirtualnego VMM autonomicznego](./media/site-recovery-single-vmm/single-vmm-standalone.png)

W tym scenariuszu wdrażanie jednym serwerze VMM jako maszyny wirtualnej w podstawowej witryny, a odtworzyć ten maszyn wirtualnych do pomocniczej witryny przy użyciu Odzyskiwanie witryny i replice funkcji Hyper-V.

1. Konfigurowanie VMM na maszyny funkcji Hyper-V. Sugerujemy zapoznawanie zainstalować wystąpienie programu SQL Server używane przez VMM na tym samym maszyn wirtualnych pozwala zaoszczędzić czas. Jeśli chcesz używać zdalnego wystąpienia programu SQL Server i awaria występuje, trzeba przywrócić to wystąpienie, zanim będzie można odzyskać VMM.
2. Upewnij się, że serwer VMM ma co najmniej dwa chmury skonfigurowane. Jedna chmura zawiera maszyny wirtualne mają być replikowane i innych chmury służy jako lokalizacji pomocniczej. Powinny mieć chmurze, która zawiera maszyny wirtualne, mają być chronione:

    - Co najmniej jeden VMM hosta grup zawierających jeden lub więcej serwerów hosta funkcji Hyper-V w każdej grupie hosta.
    - Co najmniej jeden maszyny wirtualnej funkcji Hyper-V na wszystkich serwerach hosta funkcji Hyper-V.

3. Tworzenie magazynu usługi odzyskiwania, generowanie Pobierz klucz rejestru magazynu i zarejestrować serwer VMM w magazyn. Podczas rejestrowania na serwerze VMM zainstalować dostawcę Azure witryny odzyskiwania.
4. Konfigurowanie jeden lub więcej chmury na maszyn wirtualnych VMM i Dodaj hosts funkcji Hyper-V, aby te chmury.
3. Konfigurowanie ustawień ochrony dla chmury. Możesz określić nazwę jednego serwera VMM jako lokalizacji źródłowej i docelowej. Aby skonfigurować mapowania sieci, można mapować maszyn wirtualnych sieci chmury z pośrednictwem SMS, mają być chronione, do sieci maszyn wirtualnych w chmurze replikacji.
4. Włączanie replikacji początkowej dla maszyny wirtualne mają być chronione przez sieć, ponieważ obie chmury znajdują się na tym samym serwerze.
4. W konsoli Menedżera funkcji Hyper-V Włącz replice funkcji Hyper-V na hoście funkcji Hyper-V, która zawiera maszyn wirtualnych VMM i replikacji na maszyn wirtualnych. Upewnij się, że nie możesz dodać VMM maszyn wirtualnych do dowolnego chmury, które są chronione Odzyskiwanie witryny. Dzięki temu, że ustawienia funkcji Hyper-V replice nie są zastępowane przez Odzyskiwanie witryny.
5. Jeśli chcesz utworzyć plany odzyskiwania, możesz określić ten sam serwer VMM dla źródłowej i docelowej.

Postępuj zgodnie z instrukcjami [w tym artykule](site-recovery-vmm-to-vmm.md) , aby utworzyć magazynu, zarejestrować serwer i skonfigurować ochrony.

### <a name="what-to-do-in-an-outage"></a>Co należy zrobić w przypadku awarii

Jeśli wystąpi awaria wykonane i potrzebne do działania z pomocniczej witryny, wykonaj następujące czynności:

1.  W konsoli Menedżera funkcji Hyper-V w witrynie pomocniczej Uruchom nieplanowanego przełączania awaryjnego niepowodzenie nad maszyn wirtualnych VMM z podstawowych do pomocniczej witryny.
2.  Zweryfikuj maszyn wirtualnych VMM i rozpocząć pracę w witrynie pomocniczej.
3.  W magazynu usługi odzyskiwania uruchamiać nieplanowanego przełączania awaryjnego niepowodzenie przez obciążenie pracą maszyny wirtualne z serwera podstawowego do chmury pomocniczą. Aby wykonać nieplanowanego przełączania awaryjnego z pośrednictwem SMS, Zatwierdź tym przełączeniu lub Wybieranie punktu odzyskiwania różnych wymaganą.
4.  Po zakończeniu nieplanowanego przełączania awaryjnego użytkownicy mogą uzyskiwać dostęp obciążenie pracą zasobów w witrynie pomocniczą.

Podstawowej witryny normalnego działania ponownie, wykonaj następujące czynności:

1.  W konsoli Menedżera funkcji Hyper-V umożliwia odwrotnej replikacji Głosowa VMM, aby rozpocząć replikacji go z pomocniczej na podstawową.
2.  W konsoli Menedżera funkcji Hyper-V Uruchom planowane awarię na powrócić maszyn wirtualnych VMM do podstawowej witryny. Przekaż przełączanie awaryjne do jego wykonaniem. Następnie należy włączyć odwrotnej replikacji zacząć replikowane VMM z podstawowego na pomocniczym.
3.  Magazynu usługi odzyskiwania włączyć odwrotnej replikacji obciążenie pracą maszyny wirtualne zacząć replikacji ich z pomocniczej na podstawową.
4.  W magazynu usługi odzyskiwania uruchamiać planowane awarię na powrócić obciążenie pracą maszyny wirtualne do podstawowej witryny. Przekaż przełączanie awaryjne do jego wykonaniem. Następnie należy włączyć odwrotnej replikacji zacząć replikacji obciążenie pracą maszyny wirtualne z podstawowego do pomocniczą.



## <a name="replicate-across-sites-with-a-single-vmm-server-in-a-stretched-cluster"></a>Odtworzyć w witrynach z jednego serwera VMM w klastrze rozciągnięty

![Grupowany serwera wirtualnego VMM](./media/site-recovery-single-vmm/single-vmm-cluster.png)

Zamiast wdrażanie serwera VMM autonomicznego jako maszyn wirtualnych, w której replikuje na pomocniczym witryny, można udostępnić VMM wysoce wdrażając go jako maszyn wirtualnych w klastrze pracy awaryjnej systemu Windows. Dzięki temu odporność obciążenie pracą i ochrony przed awarii sprzętu. Aby wdrożyć Odzyskiwanie witryny maszyn wirtualnych VMM powinny zostać wdrożone w klastrze rozciąganie w witrynach geograficznie. Aby to zrobić:

1. Instalowanie VMM na maszyny wirtualnej w klastrze pracy awaryjnej systemu Windows, a następnie wybierz opcję, aby uruchomić na serwerze jako wysoce dostępne podczas instalacji.
2. Wystąpienie programu SQL Server jest używana przez VMM powinny być replikowane z grupy dostępności (AlwaysOn) programu SQL Server, aby była ona replice bazy danych w witrynie pomocniczą.
3. Postępuj zgodnie z instrukcjami [w tym artykule](site-recovery-vmm-to-vmm.md) , aby utworzyć magazynu, zarejestrować serwer i skonfigurować ochrony. Musisz zarejestrować każdego serwera VMM w klastrze w magazynu usługi odzyskiwania. W tym celu należy zainstalować dostawcę na aktywny węzeł, a zarejestrować serwer VMM. Następnie zainstalować dostawcę na innych węzłach.

### <a name="what-to-do-in-an-outage"></a>Co należy zrobić w przypadku awarii

W przypadku wystąpienia awarii serwera VMM jej odpowiednią bazę danych programu SQL Server nie powiodło się nad i uzyskać do nich dostęp z pomocniczej witryny.


## <a name="next-steps"></a>Następne kroki

[Aby uzyskać więcej informacji](site-recovery-vmm-to-vmm.md) na temat szczegółowe rozmieszczania Odzyskiwanie witryny dla VMM VMM replikacji.
