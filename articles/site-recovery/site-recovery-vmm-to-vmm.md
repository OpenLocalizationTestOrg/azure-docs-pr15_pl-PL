<properties
    pageTitle="Powielić maszyn wirtualnych funkcji Hyper-V w chmury VMM do pomocniczej witryny VMM za pomocą portalu Azure | Microsoft Azure"
    description="W tym artykule opisano, jak wdrożyć serwer Azure Odzyskiwanie witryny, aby dodać akompaniament replikacji, pracy awaryjnej i odzyskiwania maszyny wirtualne funkcji Hyper-V w chmury VMM do pomocniczej witryny VMM za pomocą portalu Azure."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="raynew"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site-using-the-azure-portal"></a>Powielić maszyn wirtualnych funkcji Hyper-V w chmury VMM do pomocniczej witryny VMM za pomocą portalu Azure

> [AZURE.SELECTOR]
- [Azure portal](site-recovery-vmm-to-vmm.md)
- [Klasyczny portalu](site-recovery-vmm-to-vmm-classic.md)
- [PowerShell — Menedżera zasobów](site-recovery-vmm-to-vmm-powershell-resource-manager.md)

Odzyskiwanie Azure witryny — Zapraszamy! Ten artykuł, aby odtworzyć funkcji Hyper-V w lokalnym środowisku maszyn wirtualnych systemu zarządzania w chmury System Center Virtual Machine Manager (VMM) do pomocniczej witryny do użycia. W tym artykule opisano sposób konfigurowania replikacji przy użyciu Odzyskiwanie witryny Azure w portalu Azure.

> [AZURE.NOTE] Azure ma dwa różne [modeli wdrażania](../resource-manager-deployment-model.md) służące do tworzenia i pracy z zasobami: Menedżer zasobów Azure i klasyczny. Azure też ma dwa portali — portalu klasyczny Azure obsługuje model klasyczny wdrożenia i portal Azure z obsługą obu modeli wdrożenia.


Azure Odzyskiwanie witryny w portalu Azure udostępnia kilka nowych funkcji:

- Platformy Azure portal Azure kopii zapasowej i usługi Azure witryny odzyskiwania są łączone w jednym magazynu usługi odzyskiwania, dzięki czemu będzie można skonfigurować i zarządzanie ciągłości i odzyskiwanie (BCDR) z jednej lokalizacji. Ujednolicony pulpit nawigacyjny pozwala na monitorowanie i zarządzanie operacji za pośrednictwem witryny lokalnej i Azure chmura publicznej.
- Użytkownicy z subskrypcją Azure obsługi administracyjnej z programem chmury rozwiązanie dostawcy (dostawcy) mogą teraz zarządzać operacji odzyskiwania witryny w portalu Azure.


Po zapoznaniu się w tym artykule, publikować komentarze u dołu w komentarzach Disqus. Zadawaj pytania techniczne na [Forum usługi odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="overview"></a>Omówienie

Organizacje muszą strategii BCDR, która określa sposób aplikacje, obciążenia i danych utrzymywania działa i jest dostępny podczas zaplanowane i niezaplanowane przestoje i odzyskiwanie do normalnych warunkach pracy tak szybko, jak to możliwe. Strategii BCDR należy zachować dane biznesowe, bezpiecznych i odzyskania i upewnij się, obciążenia pozostają nieprzerwanie dostępne, gdy wystąpi awarii.

Odzyskiwanie witryny jest usługą Azure, która pozwala strategii BCDR przez orchestrating replikacji lokalnych serwerów fizycznych i maszyn wirtualnych w chmurze (Azure) lub pomocnicza centrum danych. W przypadku wystąpienia awarii w lokalizacji podstawowego nie na pomocniczym lokalizacji, aby aplikacje i obciążenia dostępne. Możesz zakończyć się niepowodzeniem powrót do swojej lokalizacji podstawowej podczas powraca do normalnego działania. Dowiedz się więcej na [Co to jest Odzyskiwanie witryny Azure?](site-recovery-overview.md)

Ten artykuł zawiera wszystkie informacje, które chcesz odtworzyć maszyny wirtualne funkcji Hyper-V w chmury VMM do pomocniczej witryny VMM lokalnego. Zawiera omówienie architektury informacji i wdrażania instrukcje dotyczące konfigurowania lokalne serwery, ustawień replikacji i planowaniu planowania. Po skonfigurowaniu infrastruktury można włączyć replikacji na komputerach, który chcesz chronić i sprawdź działania tej pracy awaryjnej.

## <a name="business-advantages"></a>Zalety firm

- Odzyskiwanie witryny zapewnia ochronę biznesowych i aplikacje na maszyny wirtualne funkcji Hyper-V za replikowane ich na serwerze pomocniczym funkcji Hyper-V.
- Portal usługi odzyskiwania miejsce na jednym Konfigurowanie, zarządzanie i monitorowanie replikacji, pracy awaryjnej i odzyskiwania.
- Łatwe uruchamianie praca awaryjna można uruchamiać z infrastruktury podstawowego lokalnego dodatkowego i powrotu (Przywróć) z pomocniczej witryny do podstawowej.
- Plany odzyskiwania można konfigurować z wieloma komputerami, tak, aby warstwowych aplikacji niepowodzenie nad razem.

## <a name="scenario-architecture"></a>Architektura scenariusz

Są to składniki scenariusza:

- **Podstawowej witryny**: W podstawowej witryny jest jedną lub więcej funkcji Hyper-V Serwery hosta uruchamianie maszyn wirtualnych źródła, który chcesz odtworzyć. Tych serwerach podstawowy host znajdują się w chmurze prywatne VMM.
- **Pomocniczy witryny**: W witrynie pomocniczej są serwery hosta jeden lub więcej funkcji Hyper-V uruchamianie maszyn wirtualnych docelowej, do których powielić podstawowego maszyny wirtualne. Te serwery hosta znajdują się w chmurze prywatne VMM. Chmura może być na serwerze podstawowym (jeśli istnieje tylko jeden serwer VMM) lub na serwerze VMM pomocniczym.
- **Dostawca**: podczas wdrażania Odzyskiwanie witryny dostawca odzyskiwania witryny Azure należy zainstalować na serwerach VMM, a rejestrowanie tych serwerów w magazynu usługi odzyskiwania. Dostawca na serwerze VMM komunikuje się z Odzyskiwanie witryny przez 443 HTTPS do replikacji aranżacji. Dane replikacji między serwerami hosta funkcji Hyper-V głównego i pomocniczego. Replikowane dane pozostaną w lokalnej witryny i sieci i nie są wysyłane do Azure. Dowiedz się więcej o [prywatności](#privacy-information-for-site-recovery).

![Topologia E2E](./media/site-recovery-vmm-to-vmm/architecture.png)

### <a name="data-privacy-overview"></a>Omówienie ochrony prywatności danych

W poniższej tabeli podsumowano sposób przechowywania danych w tym scenariuszu:
****
Akcja | **Szczegóły** | **Dane zebrane** | **Użyj** | **Wymagane**
--- | --- | --- | --- | ---
**Rejestracja** | Zarejestruj się z serwerem VMM w magazynu usługi odzyskiwania. Jeśli chcesz później unregister serwera, możesz to zrobić, usuwając informacje o serwerze z portalu Azure. | Po zarejestrowaniu serwer VMM Odzyskiwanie witryny zbiera procesów i przesyła metadane dotyczące serwera VMM i nazwy chmur VMM wykryte przez Odzyskiwanie witryny. | Dane służy do identyfikowania i komunikować się z odpowiedniego serwera VMM i konfigurowanie ustawień dla odpowiednich chmur VMM. | Ta funkcja jest wymagane. Jeśli nie chcesz, aby wysłać te informacje do Odzyskiwanie witryny nie należy używać usługi Odzyskiwanie witryny.
**Włączanie replikacji** | Dostawca odzyskiwania Azure witryny jest zainstalowana na serwerze VMM i jest kanał komunikacji z usługą Odzyskiwanie witryny. Dostawca jest obsługiwany w procesie VMM biblioteki dołączanej (dynamicznie DLL). Po zainstalowaniu dostawcy w konsoli administratora VMM otrzymuje włączyć funkcję "Odzyskiwania centrum danych". Nowych i istniejących maszyny wirtualne można włączyć tę funkcję, aby włączyć ochronę maszyny. | Z tą właściwość do Odzyskiwanie witryny dostawca wysyła nazwy i Identyfikatora maszyn wirtualnych.  Replikacja jest włączona przez system Windows Server 2012 lub Windows Server 2012 R2 Hyper-V replice. Dane maszyn wirtualnych otrzymuje replikowane z jednego hosta funkcji Hyper-V do innego (zwykle znajduje się w centrum danych, różnych "odzyskiwania"). | Odzyskiwanie witryny używa metadanych wypełnianie informacji maszyn wirtualnych w portalu Azure. | Ta funkcja jest podstawowe częścią usługi i nie można wyłączyć. Jeśli nie chcesz wysyłać te informacje, nie włączyć ochronę Odzyskiwanie witryny maszyny wirtualne. Należy zauważyć, że wszystkie dane wysyłane przez dostawcę do Odzyskiwanie witryny jest wysyłana za pomocą protokołu HTTPS.
**Planowanie odzyskiwania** | Plany odzyskiwania ułatwiają tworzenie planu aranżacji centrum danych odzyskiwania. Można określić kolejność, w której maszyny wirtualne lub grupę maszyn wirtualnych powinny być uruchamiane w witrynie odzyskiwania. Można również określić skrypty automatyczną ma być akcją wykonywania lub dowolną ręczny jakie należy podjąć, w tym czasie odzyskiwania dla każdego maszyn wirtualnych. Pracy awaryjnej zwykle zostanie wywołana na poziomie planu odzyskiwania skoordynowanego odzyskiwania. | Odzyskiwanie witryny zbiera, przetwarza i przesyła metadanych dla planu odzyskiwania, w tym metadanych maszyn wirtualnych i metadanych skryptów automatyzacji i notatki ręczne akcji. | Metadane służy do tworzenia planu odzyskiwania w portalu Azure. | Ta funkcja jest podstawowe częścią usługi i nie można wyłączyć. Jeśli nie chcesz wysłać te informacje do Odzyskiwanie witryny, nie należy tworzyć plany odzyskiwania.
**Mapowanie sieci** | Mapy sieci informacji z centrum danych podstawowych centrum danych odzyskiwania. Gdy maszyny wirtualne odzyskuje się w witrynie odzyskiwania mapowanie sieci pomaga w ustalaniu łączność sieciowa. | Odzyskiwanie witryny zbiera, przetwarza i przesyła metadane sieci logicznych dla każdej witryny (podstawowego i centrum danych). | Metadane będzie używany do wypełniania ustawień sieci, tak aby można było mapować informacje o sieci. | Ta funkcja jest podstawowe częścią usługi i nie można wyłączyć. Jeśli nie chcesz wysłać te informacje do Odzyskiwanie witryny, nie używaj mapowanie sieci.
**Pracy awaryjnej (planowana niezaplanowane test)** | Pracy awaryjnej nie powiedzie się nad maszyny wirtualne z jednym centrum danych zarządzanych VMM do drugiego. Akcja pracy awaryjnej zostanie wywołana ręcznie w portalu Azure. | Dostawca na serwerze VMM jest powiadamiany o zdarzeniu pracy awaryjnej przez Odzyskiwanie witryny i uruchamia akcję pracy awaryjnej na hoście funkcji Hyper-V za pośrednictwem interfejsów VMM. Rzeczywisty awaryjnego przeniesienia maszyny jest z jednego hosta funkcji Hyper-V do innego i obsłużone przez system Windows Server 2012 lub Windows Server 2012 R2 Hyper-V replice. Po zakończeniu pracy awaryjnej dostawcy na serwerze VMM w centrum danych odzyskiwania przesyła informacje sukcesu do Odzyskiwanie witryny. | Odzyskiwanie witryny korzysta z informacji wysyłane do wypełnienia kolumny stanu pracy awaryjnej informacji akcji w portalu Azure. | Ta funkcja jest podstawowe częścią usługi i nie można wyłączyć. Jeśli nie chcesz wysłać te informacje do Odzyskiwanie witryny, nie używaj pracy awaryjnej.


## <a name="azure-prerequisites"></a>Wymagania wstępne dotyczące Azure

Oto, co należy platformy Azure wdrażanie w tym scenariuszu:

**Wymagania wstępne** | **Szczegóły**
--- | ---
**Azure**| Wymagane jest konto [Microsoft Azure](http://azure.microsoft.com/) . Możesz rozpocząć z [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/). [Aby uzyskać więcej informacji](https://azure.microsoft.com/pricing/details/site-recovery/) na temat ceny Odzyskiwanie witryny.


## <a name="on-premises-prerequisites"></a>Wymagania wstępne dotyczące lokalnego

Oto, co jest potrzebne w witrynach głównego i pomocniczego lokalnego wdrożenia w tym scenariuszu:

**Wymagania wstępne** | **Szczegóły**
--- | ---
**VMM** | Zalecamy zapoznanie wdrożyć serwer VMM w podstawowej witryny i serwer VMM w witrynie pomocniczą.<br/><br/> Możesz także [replikować między chmury na serwerze VMM](site-recovery-single-vmm.md). W tym celu potrzebne są co najmniej dwa chmury skonfigurowany na serwerze VMM.<br/><br/> Serwery VMM powinna działać co najmniej systemu Centrum 2012 z dodatkiem SP1 z najnowszymi aktualizacjami.<br/><br/> Każdy serwer VMM musi mieć na jednym lub więcej chmury skonfigurowane i wszystkich chmur muszą mieć profil zdolności funkcji Hyper-V, ustaw. <br/><br/>Chmury musi zawierać jedną lub więcej grup hosta VMM.<br/><br/>Dowiedz się więcej o konfigurowaniu chmury VMM [Konfigurowanie tkaninie VMM w chmurze](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), a [Instruktaż: tworzenie prywatnych chmury za pomocą systemu Centrum 2012 z dodatkiem SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).<br/><br/> Serwery VMM potrzebny dostęp do Internetu.
**Funkcji Hyper-V** | Serwery funkcji Hyper-V musi działać co najmniej systemu Windows Server 2012 z roli Hyper-V i zainstalowano najnowsze aktualizacje.<br/><br/> Serwer funkcji Hyper-V powinien zawierać co najmniej jeden maszyny wirtualne.<br/><br/>  Serwery hosta funkcji Hyper-V powinien znajdować się w grupach hosta w chmury VMM głównego i pomocniczego.<br/><br/> Jeśli używasz funkcji Hyper-V w klastrze w systemie Windows Server 2012 R2 należy zainstalować [aktualizacji 2961977](https://support.microsoft.com/kb/2961977)<br/><br/> Jeśli używasz funkcji Hyper-V w klastrze w systemie Windows Server 2012, należy zauważyć, że tego brokera klaster nie jest tworzone automatycznie, jeśli masz statyczne klaster oparty na adresie IP. Musisz ręcznie skonfigurować brokera klaster. [Dowiedz się więcej](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).
**Dostawcy** | Podczas wdrażania Odzyskiwanie witryny dostawca odzyskiwania witryny Azure zainstalowanie na serwerach VMM. Dostawca komunikuje się z Odzyskiwanie witryny przez 443 HTTPS, aby dodać akompaniament replikacji. Dane replikacji między serwerami funkcji Hyper-V głównego i pomocniczego w sieci LAN lub połączenie VPN.<br/><br/> Dostawca na serwerze VMM musi mieć dostęp do następujących adresów URL: *. hypervrecoverymanager.windowsazure.com; *. AccessControl.Windows.NET; *. backup.windowsazure.com; *. blob.Core.Windows.NET; *. store.core.windows.net.<br/><br/> Ponadto zezwolić na komunikację zapory z serwerów VMM [zakresów adresów IP centrum danych Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) i umożliwić protokołu HTTPS (443).

## <a name="prepare-for-deployment"></a>Przygotowywanie do wdrożenia

Aby przygotować się do wdrożenia, które należy:

1. [Przygotuj na serwerze VMM](#prepare-the-vmm-server) wdrożenia Odzyskiwanie witryny.
2. [Przygotowywanie do mapowania sieci](#prepare-for-network-mapping). Konfigurowanie sieci, dzięki czemu można skonfigurować mapowanie sieci.

### <a name="prepare-the-vmm-server"></a>Przygotuj serwer VMM

Upewnij się, że serwer VMM spełnia [wymagania wstępne](#on-premises-prerequisites) i można uzyskać dostęp do listy adresów URL.


### <a name="prepare-for-network-mapping"></a>Przygotowywanie do mapowania sieci

Sieć mapy mapowania między sieciami maszyn wirtualnych VMM na serwerach VMM głównego i pomocniczego, aby:

- Optymalnie umieść po przełączeniu maszyny wirtualne replice pomocniczej hosts funkcji Hyper-V.
- Połącz replice maszyny wirtualne do odpowiednich maszyn wirtualnych sieci.
- Jeśli nie skonfigurowano sieci mapowanie replice maszyny wirtualne nie można połączyć dowolna sieć, po przełączeniu.
- Jeśli chcesz skonfigurować sieć mapowania podczas odzyskiwania witryny wdrożenia tutaj to co należy:

    - Upewnij się, że maszyny wirtualne na serwerze hosta funkcji Hyper-V źródła jest połączony z siecią VMM maszyn wirtualnych. Sieci należy połączyć z siecią logiczną, która jest skojarzony z chmury.
    - Sprawdź, czy pomocniczej chmury służące do odzyskiwania ma odpowiednie maszyn wirtualnych sieci skonfigurowanej. Czy sieć maszyn wirtualnych powinna być połączona z siecią logiczną, która jest skojarzony z chmurą pomocniczą.

- [Aby uzyskać więcej informacji](site-recovery-network-mapping.md) o sposobie działania mapowanie sieci.

## <a name="prepare-for-deployment-with-a-single-vmm-server"></a>Przygotowywanie do wdrożenia z jednego serwera VMM

Jeśli masz tylko jeden serwer VMM maszyny wirtualne można odtworzyć w hosts funkcji Hyper-V w chmurze VMM [Azure](site-recovery-vmm-to-azure.md) lub pomocnicza chmurze VMM. Zaleca się, że pierwszą opcję ponieważ replikacji między chmury nie jest bezproblemowa, ale musisz to zrobić w tym miejscu jest, co należy zrobić:

1. **Konfigurowanie VMM na maszyny funkcji Hyper-V**. W trakcie firma Microsoft zaleca się przekazać wystąpienie programu SQL Server używane przez VMM na tym samym maszyn wirtualnych. Zaoszczędzić czas, jak tylko jeden maszyn wirtualnych ma zostać utworzone. Jeśli chcesz używać zdalnego wystąpienia programu SQL Server i awaria występuje, trzeba przywrócić to wystąpienie, zanim będzie można odzyskać VMM.
2. **Upewnij się na serwerze VMM zawiera co najmniej dwa chmury skonfigurowane**. Jedna chmura będzie zawierać maszyny wirtualne mają być replikowane i innych chmury będzie służyć jako lokalizacji pomocniczej. Chmura zawierający maszyny wirtualne, który chcesz chronić powinny spełniać [wymagania wstępne](#on-premises-prerequisites).
3. Konfigurowanie witryny odzyskiwania zgodnie z opisem w tym artykule. Tworzenie i zarejestrować serwer VMM w magazyn, konfigurowanie zasad replikacji i włączanie replikacji. Należy określić, że replikacji początkowej odbywa się w sieci.
4. Po skonfigurowaniu mapowanie sieci należy mapować maszyn wirtualnych sieci podstawowego chmury maszyn wirtualnych sieci pomocniczej chmury.
5. W konsoli Menedżera funkcji Hyper-V Włącz replice funkcji Hyper-V na hoście funkcji Hyper-V, która zawiera maszyn wirtualnych VMM i replikacji na maszyn wirtualnych. Upewnij się, że nie możesz dodać maszyny wirtualnej VMM do chmury, które są chronione Odzyskiwanie witryny, aby upewnić się, że ustawienia funkcji Hyper-V replice nie są zastępowane przez Odzyskiwanie witryny.
6. Jeśli tworzysz odzyskiwania planów do przełączania awaryjnego program ten sam serwer VMM dla źródłowej i docelowej.
7. Niepowodzenie nad i odzyskiwanie w następujący sposób:

    - Ręczne nie nad maszyn wirtualnych VMM do pomocniczej witryny za pomocą Menedżera funkcji Hyper-V z planowanej trybie awaryjnym.
    - Niepowodzenie przez maszyny wirtualne.
    - Po podstawowej, które odzyskane maszyn wirtualnych VMM Zaloguj się do portalu Azure -> usługi odzyskiwania magazynu i uruchom nieplanowanego przełączania awaryjnego maszyny wirtualne z pomocniczej witryny do podstawowej witryny.
    - Po zakończeniu nieplanowanego przełączania awaryjnego wszystkie zasoby są dostępne w witrynie podstawowego ponownie.


### <a name="create-a-recovery-services-vault"></a>Tworzenie magazynu usługi odzyskiwania

1. Zaloguj się do usługi [Azure portal](https://portal.azure.com).
2. Kliknij przycisk **Nowy** > **zarządzania** > **usługi odzyskiwania**. Alternatywnie kliknij **Przeglądaj** > **Usługi odzyskiwania** magazynów > **Dodaj**.

    ![Nowe magazynu](./media/site-recovery-vmm-to-vmm/new-vault3.png)

3. W polu **Nazwa** Określ przyjazną nazwę identyfikującą magazyn. Jeśli masz więcej niż jedną subskrypcję, wybierz jeden z nich.
4. [Tworzenie nowej grupy zasobów](../resource-group-template-deploy-portal.md) lub wybierz istniejący i określ Azure region. Komputery będzie można replikować do tego obszaru. Aby sprawdzić obsługiwanych regionów, zobacz dostępność geograficzne w [Azure Szczegóły ceny odzyskiwania witryny](https://azure.microsoft.com/pricing/details/site-recovery/)
4. Jeśli chcesz szybko uzyskać magazyn na pulpicie nawigacyjnym kliknij pozycję **Przypnij do pulpitu nawigacyjnego** > **Tworzenie magazynu**.

    ![Nowe magazynu](./media/site-recovery-vmm-to-vmm/new-vault-settings.png)

Nowy magazyn pojawią się na **pulpicie nawigacyjnym** > **wszystkie zasoby**i na głównym karta **magazynów usługi odzyskiwania** .




## <a name="getting-started"></a>Wprowadzenie

Odzyskiwanie witryny zawiera wprowadzenie do środowiska, który pomoże Ci wdrażanie tak szybko, jak to możliwe. Wprowadzenie do sprawdza wymagania wstępne i przeprowadzi Cię przez Odzyskiwanie witryny wdrożenia kroki w odpowiedniej kolejności.

Wprowadzenie, wybierz typ maszyn, który chcesz odtworzyć, a miejsce, w którym mają być replikowane w. Konfigurowanie lokalne serwery, Utwórz zasady replikacji i zaplanowanie wydajności. Po skonfigurowaniu infrastruktury umożliwia replikacji maszyny wirtualne. Można uruchomić praca awaryjna na określonych komputerach lub tworzenie planów odzyskiwania niepowodzenie na wielu komputerach.

Rozpocznij wprowadzenie, wybierając pozycję jak chcesz wdrożyć Odzyskiwanie witryny. Wprowadzenie do przepływów nieco zmienia się w zależności od potrzeb replikacji.

## <a name="step-1-choose-your-protection-goal"></a>Krok 1: Wybierz celem ochrony

Zaznacz dane mają być replikowane i miejsce, w którym mają być replikowane w.

1. W karta **magazynów usługi odzyskiwania** wybierz z magazynu, a następnie kliknij przycisk **Ustawienia**.
2. W obszarze **Ustawienia** > **Wprowadzenie** kliknij pozycję **Odzyskiwanie witryny** > **Krok 1: Przygotowywanie infrastruktury** > **cel ochrony**.
3. W **celu ochrony** wybierz **do odzyskiwania witryny**, a następnie wybierz pozycję **Tak, przy użyciu funkcji Hyper-V**.
4. Wybierz pozycję **Tak,** aby wskazać, że używasz VMM do zarządzania hosts funkcji Hyper-V i wybierz opcję **Tak** , jeśli serwer pomocniczy VMM. Jeśli jest instalowany replikacji między chmury na serwerze VMM klikniesz przycisk **nie**. Kliknij przycisk **OK**.

    ![Wybierz cele](./media/site-recovery-vmm-to-vmm/choose-goals.png)


## <a name="step-2-set-up-the-source-environment"></a>Krok 2: Konfigurowanie środowiska źródła

Instalowanie dostawcy odzyskiwania witryny Azure na serwerach VMM i rejestrowanie serwerów w magazyn.

1. Kliknij pozycję **Krok 2: Przygotowywanie infrastruktury** > **źródła**.

    ![Ustawianie źródła](./media/site-recovery-vmm-to-vmm/goals-source.png)

2. W **źródle przygotowywanie** kliknij **+ VMM** Aby dodać serwer VMM.

    ![Ustawianie źródła](./media/site-recovery-vmm-to-vmm/set-source1.png)

2. W Sprawdź karta **Dodaj serwer** , czy **serwer VMM Centrum systemu** pojawia się **Typ serwera** , a na serwerze VMM spełnia [wymagania wstępne i wymagania dotyczące adresu URL](#on-premises-prerequisites).
4. Pobierz plik instalacyjny Azure witryny odzyskiwania dostawcy.
5. Pobierz klucz rejestru. Potrzebujesz to po uruchomieniu Instalatora. Klucz jest prawidłowy przez 5 dni po jego wygenerować.

    ![Ustawianie źródła](./media/site-recovery-vmm-to-vmm/set-source3.png)

6. Instalowanie dostawcy odzyskiwania witryny Azure na serwerze VMM.

> [AZURE.NOTE] Nie musisz jawnie zainstalować nic na serwerach hosta funkcji Hyper-V.


### <a name="set-up-the-azure-site-recovery-provider"></a>Konfigurowanie dostawcy odzyskiwania witryny Azure

1. Uruchom plik Instalatora na wszystkich serwerach VMM dostawcy. Jeśli VMM zostanie wdrożony w klastrze i instalujesz dostawcę po raz pierwszy zainstalować go na aktywny węzeł i zakończyć instalację, aby zarejestrować serwer VMM w magazyn. Następnie zainstalować dostawcę na innych węzłach. Węzłach powinna działać tej samej wersji dostawcy.
2. Instalator uruchamia kilka testy prerequirement i żądania uprawnień, aby zatrzymać usługę VMM. Usługa VMM zostanie automatycznie uruchomiona, po zakończeniu instalacji. Jeśli przeprowadzasz instalację w klastrze VMM zostanie wyświetlony monit, aby zatrzymać roli klaster.

2.  W **Witrynie Microsoft Update** można jednak aktualizacje tak, aby zainstalowanych aktualizacji dostawcy zgodnie z zasadami usługi Microsoft Update.
3. W **instalacji** zaakceptować lub modyfikowanie domyślnej lokalizacji instalacji dostawcy, a następnie kliknij przycisk **Zainstaluj**.

    ![Lokalizacja instalacji](./media/site-recovery-vmm-to-vmm/provider-location.png)

3. Po zakończeniu instalacji kliknij przycisk **Zarejestruj** , aby zarejestrować serwer w magazyn.

    ![Lokalizacja instalacji](./media/site-recovery-vmm-to-vmm/provider-register.png)

9. W polu **Nazwa magazynu**Sprawdź nazwę magazynu, w którym zostanie zarejestrowany serwera. Kliknij przycisk *Dalej*.

    ![Rejestracja serwera](./media/site-recovery-vmm-to-vmm-classic/vaultcred.PNG)

7. W oknie **Połączenie internetowe** określ jak dostawcy na serwerze VMM łączy się z Internetem. Wybierz pozycję **Połącz z istniejących ustawień serwera proxy** , aby korzystać z domyślnych ustawień połączenia internetowego skonfigurowany na serwerze.

    ![Ustawienia internetowej](./media/site-recovery-vmm-to-vmm-classic/proxydetails.PNG)

    - Jeśli chcesz używać niestandardowego serwera proxy został on skonfigurowany przed rozpoczęciem instalacji dostawcy. Podczas konfigurowania ustawień serwera proxy niestandardowy, aby sprawdzić połączenie serwera proxy spowoduje uruchomienie test.
    - Używanie niestandardowej serwera proxy, czy serwer proxy domyślne wymaga uwierzytelniania, należy wprowadzić szczegóły serwera proxy, w tym adres serwera proxy oraz portu.
    - Po adresy URL powinien być dostępny na serwerze VMM i hostów funkcji Hyper-v
        - *. hypervrecoverymanager.windowsazure.com
        - *. accesscontrol.windows.net
        - *. backup.windowsazure.com
        - *. blob.core.windows.net
        - *. store.core.windows.net
    - Zezwalaj opisanych w [Zakresów adresów IP centrum danych Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) i HTTPS protocol (443) adresów IP. Trzeba biała lista zakresów adresów IP Azure regionu, który ma zostać użycia i US Zachód.
    - Jeśli korzystasz z niestandardowej serwera proxy konta VMM RunAs (DRAProxyAccount) jest tworzona automatycznie przy użyciu poświadczeń określonego serwera proxy. Konfigurowanie serwera proxy, aby to konto można uwierzytelnić pomyślnie. W konsoli VMM można modyfikować ustawienia konta VMM RunAs. W tym celu należy otworzyć obszar roboczy **ustawień** , rozwiń węzeł **Zabezpieczenia**, kliknij polecenie **Uruchom jako konta**, a następnie zmodyfikuj hasła dla DRAProxyAccount. Musisz ponownie uruchomić usługę VMM, tak aby to ustawienie jest stosowane.


8. W polu **Klucz rejestru**wybierz klucz, który pobrane z Odzyskiwanie witryny Azure i skopiować na serwerze VMM.


10.  Ustawienie szyfrowania jest używane tylko w przypadku, gdy masz replikacji maszyny wirtualne funkcji Hyper-V w chmury VMM Azure. Jeśli masz replikacji do pomocniczej witryny nie służy.

11.  W polu **Nazwa serwera**Określ przyjazną nazwę identyfikującą serwer VMM w magazyn. W konfiguracji klaster Określ nazwę VMM klaster roli.
12.  W **metadanych chmury Synchronizuj** wybierz czy chcesz zsynchronizować z magazynu metadanych dla wszystkich chmur na serwerze VMM. Ta akcja musi tylko raz wystąpić na wszystkich serwerach. Jeśli nie chcesz zsynchronizować wszystkie chmury, można pozostawić niezaznaczone to ustawienie i synchronizowanie każdego chmury pojedynczo w chmurze właściwości w konsoli VMM.

13.  Kliknij przycisk **Dalej** , aby ukończyć proces. Po zarejestrowaniu metadanych na serwerze VMM jest pobierana przez Odzyskiwanie witryny Azure. Serwer jest wyświetlany na karcie **Serwery VMM** na stronie **Serwery** magazyn.

    ![Serwer](./media/site-recovery-vmm-to-vmm-classic/provider13.PNG)

11. Gdy serwer jest dostępny w konsoli Odzyskiwanie witryny w **źródle** > **Przygotuj źródła** wybierz serwer VMM, a następnie wybierz pozycję chmury, w którym znajduje się hosta funkcji Hyper-V. Kliknij przycisk **OK**.

#### <a name="command-line-installation"></a>Instalacja wiersza polecenia

Dostawca odzyskiwania witryny Azure można zainstalować z poziomu wiersza polecenia. Ta metoda umożliwia instalowanie dostawcy na serwerze Core dla systemu Windows Server 2012 R2.

1. Pobierz klucz pliku i rejestracji instalacji dostawcy do folderu. Na przykład C:\ASR.
2. Zatrzymaj usługę System Center Virtual Machine Manager.
3. W wierszu polecenia z podwyższonym poziomem uprawnień Uruchom te polecenia, aby wyodrębnić Instalatora dostawcy:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q

4. Uruchom to polecenie, aby zainstalować dostawcy:

        C:\ASR> setupdr.exe /i

5. Następnie uruchom te polecenia, aby zarejestrować serwer w Magazyn:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>     

Gdzie są parametry:

 - **/Credentials**: obowiązkowe parametr, który określa lokalizację, w której znajduje się plik klucza rejestru  
 - **/FriendlyName**: parametr obowiązkowe dla nazwy serwera hosta funkcji Hyper-V, który jest wyświetlana w portalu Azure Odzyskiwanie witryny.
 - **/EncryptionEnabled**: parametr opcjonalny, używaną tylko podczas replikacji z VMM Azure.
 - **/proxyAddress**: parametr opcjonalny, który określa adres serwera proxy.
 - **/proxyport**: parametr opcjonalny, który określa port serwera proxy.
 - **/proxyUsername**: parametr opcjonalny określający nazwę użytkownika serwera Proxy (Jeśli serwer proxy wymaga uwierzytelniania).
 - **/proxyPassword**: parametr opcjonalny, który określa hasło uwierzytelniania na serwerze proxy (Jeśli serwer proxy wymaga uwierzytelniania).  

## <a name="step-3-set-up-the-target-environment"></a>Krok 3: Konfigurowanie środowiska docelowej

Zaznacz docelowy VMM serwer i chmury.

1. Kliknij pozycję **Przygotuj infrastruktury** > **docelowej** i wybierz serwer VMM docelowy, którego chcesz użyć.
2.  Chmury na serwerze, które są synchronizowane z Odzyskiwanie witryny będą wyświetlane. Zaznacz w chmurze docelowej.

    ![Docelowy](./media/site-recovery-vmm-to-vmm/target-vmm.png)

## <a name="step-4-set-up-replication-settings"></a>Krok 4: Konfigurowanie ustawień replikacji

1. Aby utworzyć nową replikacją zasad kliknij pozycję **Przygotuj infrastruktury** > **Ustawień replikacji** > **+ Utwórz i skojarz**.

    ![Sieci](./media/site-recovery-vmm-to-vmm/gs-replication.png)

2. **Tworzenie** i kojarzenie zasad określ nazwę zasady. Typ źródłowej i docelowej powinien być **Funkcji Hyper-V**.
3. W **wersji hosta funkcji Hyper-V** zaznacz na hoście działa system operacyjny.

    > [AZURE.NOTE] W chmurze VMM może zawierać hosts funkcji Hyper-V z różnymi wersjami (obsługiwane) systemu Windows Server, ale zasady replikacji są stosowane hosty z usługą tego samego systemu operacyjnego. Jeśli masz hosts z więcej niż jednego systemu operacyjnego, następnie utwórz osobne replikacji zasady.

4. W **Typ uwierzytelniania** i **port uwierzytelniania** Określ, jak ruch uwierzytelnieniu się między serwerami hosta podstawowego i odzyskiwania funkcji Hyper-V. Wybierz **certyfikat** , chyba że działa środowisko Kerberos. Odzyskiwanie witryny Azure automatycznie skonfiguruje certyfikatów do uwierzytelniania HTTPS. Nie musisz niczego robić ręcznie. Domyślnie port 8083 i 8084 (w przypadku certyfikaty) zostanie otwarty w Zaporze systemu Windows na serwerach hosta funkcji Hyper-V. Jeśli wybierzesz **Kerberos**, bilet Kerberos będzie używana do wzajemnego uwierzytelniania serwerów hosta. Zauważ, że to ustawienie ma zastosowanie tylko dla funkcji Hyper-V hosta serwery mają zainstalowany w systemie Windows Server 2012 R2.
3. W polu **Kopiuj częstotliwość** Określ, jak często chcesz replikować różnicy danych po początkowej replikacji (co 30 sekund, 5 lub 15 minut).
4. W **odzyskiwania punktu przechowywania**określ w godzinach czasu okno przechowywania będzie dla każdego punktu odzyskiwania. Chroniony maszyn może zostać przywrócona do dowolnego punktu w oknie.
6. W **aplikacji spójne częstotliwość migawkę** określ częstotliwość (1 – 12 godzin) punktów odzyskiwania zawierające migawek spójnych aplikacji będzie utworzona. Funkcji Hyper-V używa dwóch typów migawek — standardowy migawkę zawierającego przyrostowe migawki całego maszyny wirtualnej i migawkę spójną z aplikacją, która pobiera migawkę w chwili dane aplikacji wewnątrz maszyny wirtualnej. Migawek spójnych aplikacji upewnij się, że aplikacje są spójna przy podejmowaniu migawki za pomocą usługi kopiowania woluminu w tle (VSS). Należy zauważyć, że po włączeniu migawek spójnych aplikacji wpłynie to na wydajność aplikacji uruchomionych maszyn wirtualnych źródła. Upewnij się, że ustawioną wartość jest mniejsza niż liczba punktów odzyskiwania dodatkowe, które można skonfigurować.
7. W **kompresji przesyłania danych** Określ, czy być kompresowany zreplikowanej dane, które są przenoszone.
8. Wybierz pozycję **Usuń replice maszyn wirtualnych** , aby określić, że maszyny wirtualnej replice należy usunąć wyłączenie ochrony przed źródła maszyn wirtualnych. Jeśli to ustawienie jest włączone, po wyłączeniu ochrony dla źródła maszyn wirtualnych zostanie usunięty z konsoli odzyskiwania witryn, ustawienia Odzyskiwanie witryny VMM zostaną usunięte z konsoli VMM i replice zostanie usunięty.
3. Metody **replikacji początkowy** Jeśli masz replikacji przez sieć Określ, czy uruchomić replikacji początkowej i Zaplanuj termin jego. Aby zapisać przepustowość sieci można planować poza zajęty godziny. Kliknij przycisk **OK**.

    ![Zasady replikacji](./media/site-recovery-vmm-to-vmm/gs-replication2.png)

6. Podczas tworzenia nowych zasad automatycznie jest skojarzony z chmurą VMM. W **zasady replikacji** kliknij **przycisk OK**. Dodatkowe chmury VMM (i maszyny wirtualne w nich) można skojarzyć z tymi zasadami replikacji w **ustawieniach** > **replikacji** > Nazwa zasady > **Skojarzyć VMM w chmurze**.

    ![Zasady replikacji](./media/site-recovery-vmm-to-vmm/policy-associate.png)

### <a name="prepare-for-offline-initial-replication"></a>Przygotowywanie do trybu offline replikacji początkowej

Jak replikacji offline kopii danych początkowych. Aby przygotować to w następujący sposób:

- Na serwerze źródłowym będzie Określ lokalizacji, z której Eksport danych ma być wykonywana. Pełna kontrola przypisywanie uprawnień NTFS i udostępnianie z usługą VMM na ścieżce eksportu. Na serwerze docelowym będzie Określ lokalizacji, z której nastąpi importowania danych. Przypisywanie takie same uprawnienia na następującą ścieżkę importu.
- Jeśli ścieżka importu lub eksportu jest udostępniony, przypisz administratora, użytkownicy zaawansowani, Operator wydruku lub Operator serwera członkostwa w grupach dla konta usługi VMM na komputerze zdalnym, na którym udostępnionego znajduje się.
- Jeśli dodawanie hostów na ścieżce importowania i eksportowania przy użyciu dowolnego konta Uruchom jako przypisywanie odczytu i zapisu rachunku Uruchom jako VMM.
- Importowanie i eksportowanie akcji nie powinien znajdować się na dowolnym komputerze używany jako serwer hosta funkcji Hyper-V, ponieważ konfiguracja sprzężenia zwrotnego nie jest obsługiwana przez funkcji Hyper-V.
- W usłudze Active Directory na poszczególnych funkcji Hyper-V hosta serwera, który zawiera maszyn wirtualnych, który chcesz chronić, włączanie i konfigurowanie delegowanie ograniczeń za zaufany komputerów zdalnych, na których ścieżki importowania i eksportowania znajdują się, w następujący sposób:
    1. Na kontrolerze domeny otwórz **Użytkownicy usługi Active Directory i komputery**.
    2. W drzewie konsoli kliknij **nazwa_domeny** > **komputerach**.
    3. Kliknij prawym przyciskiem myszy nazwę serwera funkcji Hyper-V > **Właściwości**.
    4. Na karcie **Delegowanie** kliknij przycisk **Ufaj temu komputerowi w delegowanie tylko do określonych usług**.
    5. Kliknij opcję **Użyj dowolnego protokołu uwierzytelniania**.
    6. Kliknij przycisk **Dodaj** > **użytkowników i komputerów**.
    7. Wpisz nazwę komputera obsługującego ścieżki eksportu > **OK**. Z listy dostępnych usług, przytrzymaj naciśnięty klawisz CTRL, a następnie kliknij pozycję **cifs** > **przycisk OK**. Powtórz nazwę komputera obsługującego ścieżka importu. Powtórz stosownie do potrzeb dodatkowe serwery hosta funkcji Hyper-V.


### <a name="configure-network-mapping"></a>Konfigurowanie mapowanie sieci

Konfigurowanie sieci mapowania między sieciami źródłowej i docelowej.

- Czy [odczytu](#prepare-for-network-mapping) dla krótkie omówienie jakie mapowanie sieci. W dodatku [więcej](site-recovery-network-mapping.md) Aby uzyskać więcej informacji na temat objaśnienie.
- Sprawdź, czy maszyn wirtualnych na serwerach VMM połączenie z siecią maszyn wirtualnych.

Konfigurowanie mapowania w następujący sposób:

1. **Ustawienia** > **Infrastrukturą odzyskiwania witryny** > **Mapowanie sieci** > **mapowania sieci** kliknij pozycję **+ Mapowanie sieci**.

    ![Mapowanie sieci](./media/site-recovery-vmm-to-azure/network-mapping1.png)

2. Na karcie **Mapowanie sieci Dodaj** wybierz źródło i docelowych VMM serwerów. Maszyn wirtualnych sieci skojarzonych z serwerami VMM są pobierane.
3. W **sieci źródła**wybierz sieci, który ma być używany na liście maszyn wirtualnych sieci skojarzone z podstawowy serwer VMM.
6. W **sieci docelowej** wybierz sieci, który ma być używany na serwerze VMM pomocniczej. Kliknij przycisk **OK**.

    ![Mapowanie sieci](./media/site-recovery-vmm-to-vmm/network-mapping2.png)

Oto, co się dzieje, gdy zaczyna się mapowanie sieci:

- Wszelkie istniejące maszyn wirtualnych replice, które odpowiadają źródła maszyn wirtualnych sieci będzie połączony z siecią maszyn wirtualnych docelową.
- Nowe maszyn wirtualnych, które są połączone z siecią maszyn wirtualnych źródło będzie połączony z siecią docelową mapowane po replikacji.
- Zmodyfikowanie istniejącego mapowania z nową sieć maszyn wirtualnych replice zostaną połączone za pomocą nowych ustawień.
- Jeśli jeden z tych podsieci ma taką samą nazwę jak podsieci, na którym znajduje się maszyny wirtualnej źródła sieci docelowej zawiera wiele podsieci, następnie maszyny wirtualnej replice będzie połączony z tej podsieci docelowej po przełączeniu. W przypadku nie podsieć docelowej z odpowiednia nazwa maszyny wirtualnej zostanie połączony z pierwszą podsieć w sieci.

### <a name="configure-storage-mapping"></a>Skonfiguruj mapowanie miejsca do magazynowania
Domyślnie podczas replikacji maszyny wirtualnej na serwerze hosta funkcji Hyper-V źródło na serwer host docelowy funkcji Hyper-V zreplikowanej dane są przechowywane w lokalizacji domyślnej wskazaną dla hosta docelowego funkcji Hyper-V w Menedżerze funkcji Hyper-V. Mieć większą kontrolę nad którym zreplikowanej dane są przechowywane można skonfigurować mapowania miejsca do magazynowania<br/><br/> Aby skonfigurować mapowania miejsca do magazynowania, musisz skonfigurować klasyfikacje miejsca do magazynowania w źródle i docelowych VMM serwerów, przed rozpoczęciem wdrażania. Mapowanie miejsca do magazynowania za pośrednictwem nowy portal Azure nie jest obecnie obsługiwane. Jednak może być włączona przy użyciu programu Powershell. Aby [uzyskać więcej informacji](site-recovery-vmm-to-vmm-powershell-resource-manager.md#step-6-configure-storage-mapping).

## <a name="step-5-capacity-planning"></a>Krok 5: Planowanie wydajności

Teraz, gdy masz podstawowe usługi infrastruktury możesz skonfigurować możesz myśleć o planowaniu i wiadomo, czy potrzebujesz dodatkowych zasobów.

Odzyskiwanie witryny zawiera planowania pojemności na podstawie programu Excel, ułatwiające przydzielanie zasobów do środowiska źródła, składniki odzyskiwania witryny, sieci i przechowywania. Terminarz może zostać uruchomiony w trybie szybkim dla ocen na podstawie średniej liczby maszyny wirtualne, dyski i miejsca do magazynowania lub w trybie szczegółowym, w której będzie wprowadzania dane na poziomie obciążenie pracą. Przed rozpoczęciem musisz:

- Gromadzenie informacji o środowisku replikacji, łącznie z maszyny wirtualne dysków za pośrednictwem SMS i miejsca do magazynowania na dysku.
- Szacowanie dziennego kursu Zmień (pochodząca), które będą dostępne dla danych zreplikowanej różnicy. Za pomocą [planowania pojemności dla funkcji Hyper-V replice](https://www.microsoft.com/download/details.aspx?id=39057) ułatwiające wykonaj następujące czynności.

1.  Kliknij przycisk **Pobierz** , aby pobrać narzędzie, a następnie uruchom go. [Przeczytaj artykuł](site-recovery-capacity-planner.md) dostarczonej narzędzie.
2.  Po zakończeniu wybierz opcję **Tak** w **został uruchomiony planowania pojemności**?

    ![Planowanie pojemności](./media/site-recovery-vmm-to-azure/gs-capacity-planning.png)

### <a name="control-bandwidth"></a>Kontrola przepustowości

Po zgromadzonych informacji replikacji różnicy w czasie rzeczywistym przy użyciu replice funkcji Hyper-V terminarz wydajność, narzędzi planowania pojemności na podstawie programu Excel pomaga obliczanie przepustowości, potrzebnych dla replikacji (początkowy i delta). Aby sterować ilością przepustowość replikacji można skonfigurować zasady NetQos za pomocą zasad grupy lub środowiska Windows PowerShell. Istnieje kilka sposobów, wykonaj następujące czynności:

**Programu PowerShell** | **Szczegóły**
--- | ---
**Nowy — NetQosPolicy "QoS do miejsca docelowego podsieci"** | Ograniczyć ruch z systemem Windows Server 2012 R2 do pomocniczej podsieci hosta funkcji Hyper-V. Użyj, jeśli różnią się z głównego i pomocniczego podsieci.
**Nowy - NetQosPolicy "QoS do portu docelowego"** | Ograniczyć ruch z systemem Windows Server 2012 R2 portu docelowego hosta funkcji Hyper-V.
**Nowy - NetQosPolicy "Ograniczenia ruchu z VMM"** | Ograniczenia ruchu z vmms.exe. Spowoduje to ograniczania ruchu funkcji Hyper-V i Live ruchu migracji. Możesz dopasować protokołów IP i porty, aby dostosować.

Można użyć ustawienia grubości przepustowości lub ograniczyć ruch bitów na pomocniczym. Jeśli korzystasz z klastrem musisz to zrobić na wszystkich węzła. Aby uzyskać więcej informacji, zobacz:


- Blog Thomas Maurer [ograniczania](http://www.thomasmaurer.ch/2013/12/throttling-hyper-v-replica-traffic/) ruchu replice funkcji Hyper-V
- Dodatkowe informacje na temat [polecenia cmdlet New-NetQosPolicy](https://technet.microsoft.com/library/hh967468.aspx).


## <a name="step-6-enable-replication"></a>Krok 6: Włącz replikacji

Teraz włączyć replikacji w następujący sposób:

1. Kliknij pozycję **Krok 2: powielić aplikacji** > **źródła**. Po włączeniu replikacji po raz pierwszy, możesz kliknąć **+ replikacji** w magazynu, aby włączyć replikacji na dodatkowych komputerach.

    ![Włączanie replikacji](./media/site-recovery-vmm-to-vmm/enable-replication1.png)

2. W karta **źródła** > Wybierz serwer VMM oraz chmury, w którym znajdują się dane mają być replikowane hostów funkcji Hyper-V. Kliknij przycisk **OK**.

    ![Włączanie replikacji](./media/site-recovery-vmm-to-vmm/enable-replication2.png)

3. W oknie karta **docelowej** Sprawdź serwer VMM pomocniczy i chmury.
4. W **środowisku maszyn wirtualnych systemu** wybierz maszyny wirtualne, mają być chronione z listy.

    ![Włącz ochronę maszyn wirtualnych](./media/site-recovery-vmm-to-vmm/enable-replication5.png)

Możesz śledzić postęp akcji **Włącz ochronę** w obszarze Ustawienia > **zadania** > **zadań Odzyskiwanie witryny**. Po uruchomieniu zadania **Finalizowanie ochrony** maszyny wirtualnej jest gotowy do przełączania awaryjnego.


>[AZURE.NOTE] Można także włączyć ochronę maszyn wirtualnych w konsoli VMM. Kliknij pozycję **Włącz ochronę** na pasku narzędzi w oknie dialogowym właściwości maszyn wirtualnych > karta **Odzyskiwanie witryny Azure** .

Po włączeniu replikacji dla maszyn wirtualnych w **ustawieniach**można wyświetlać właściwości > **Replikowane elementów** > Nazwa maszyn wirtualnych. Na pulpicie nawigacyjnym **Essentials** widać informacji na temat zasad replikacji dla maszyn wirtualnych i jego stan. Aby uzyskać więcej informacji, kliknij pozycję **Właściwości** .

### <a name="onboard-existing-virtual-machines"></a>Wbudowany istniejących maszyn wirtualnych

Jeśli masz istniejące maszyn wirtualnych w VMM, które są replikowane przy użyciu funkcji Hyper-V replice możesz podłączone ich ochrony Odzyskiwanie witryny Azure w następujący sposób:

1. Upewnij się, że funkcji Hyper-V serwera obsługującego istniejących maszyn wirtualnych znajduje się w chmurze podstawowego i że serwer funkcji Hyper-V obsługujący maszyny wirtualnej replice znajduje się w chmurze pomocniczą.
2. Upewnij się, że skonfigurowano zasady replikacji podstawowego chmury VMM.
2. Włączanie replikacji podstawowego maszyny wirtualnej. Azure Odzyskiwanie witryny i VMM będziesz mieć pewność, że zostanie wykryta samego hosta replice i maszyn wirtualnych i odzyskiwanie witryny Azure będzie ponowne używanie przywrócić replikacji przy użyciu określonych ustawień.


## <a name="step-7-test-your-deployment"></a>Krok 7: Testowanie wdrożenia

Aby przetestować wdrożenie można uruchomić trybie awaryjnym test dla jednego komputera wirtualnych lub tworzenie planu odzyskiwania, który zawiera co najmniej jeden maszyn wirtualnych.

### <a name="prepare-for-failover"></a>Przygotowywanie się do przełączania awaryjnego

- Aby w pełni przetestować wdrożenie należy infrastruktury zreplikowanej komputer działa zgodnie z oczekiwaniami. Jeśli chcesz przetestować usługi Active Directory i systemie DNS możesz tworzenia maszyny wirtualnej jako kontrolera domeny z usługą DNS i powielić to Azure za pomocą Odzyskiwanie witryny Azure. Przeczytaj więcej o [test pracy awaryjnej zagadnienia związane z usługą Active Directory](site-recovery-active-directory.md#considerations-for-test-failover).
- Z instrukcjami podanymi w tym artykule opisano, jak przeprowadzić trybie awaryjnym test nie sieci. Ta opcja spowoduje przetestowanie, że maszyn wirtualnych przełącza, ale nie Przetestuj ustawienia sieciowe dla maszyn wirtualnych. [Aby uzyskać więcej informacji](site-recovery-failover.md#run-a-test-failover) na temat innych opcji.
- Jeśli chcesz uruchomić nieplanowanego przełączania awaryjnego zamiast trybie awaryjnym test Pamiętaj o następujących kwestiach:

    - Jeśli to możliwe należy wyłączać podstawowego maszyn przed uruchomieniem nieplanowanego przełączania awaryjnego. Dzięki temu, że nie masz źródłowa i replice komputerów z systemem w tym samym czasie.
    - Po uruchomieniu nieplanowanego przełączania awaryjnego przestaje replikacji danych z podstawowego komputerów dzięki różnicy wszelkie dane nie będą przekazywane po rozpoczęciu nieplanowanego przełączania awaryjnego. Ponadto w przypadku wystąpienia nieplanowanego przełączania awaryjnego plan odzyskiwania będzie działać aż do wykonania, nawet jeśli wystąpi błąd.




### <a name="run-a-test-failover"></a>Uruchom w trybie awaryjnym test

1. Niepowodzenie w jednym maszyn wirtualnych w **ustawieniach** > **Replikowane elementów**, kliknij pozycję maszyn wirtualnych > **+ pracy awaryjnej Test**.

    ![Testowanie pracy awaryjnej](./media/site-recovery-vmm-to-vmm/test-failover.png)

2. Niepowodzenie nad planu odzyskiwania w **ustawieniach** > **Odzyskiwania plany**, kliknij prawym przyciskiem myszy odpowiedni plan > **Testowanie pracy awaryjnej**. Aby utworzyć odzyskiwania planowanie, [wykonaj następujące instrukcje](site-recovery-create-recovery-plans.md).
2. W **Pracy awaryjnej Test**wybierz opcję **Brak**. Po wybraniu tej opcji przetestować maszyn wirtualnych przełącza zgodnie z oczekiwaniami, ale zreplikowanej maszyn wirtualnych nie będzie podłączony do każdej sieci.

    ![Testowanie pracy awaryjnej](./media/site-recovery-vmm-to-vmm/test-failover1.png)

3. Kliknij **przycisk OK** , aby rozpocząć tym przełączeniu. Możesz śledzić postęp, klikając pozycję maszyn wirtualnych, aby otworzyć jego właściwości lub stanowiska **Pracy awaryjnej Test** w **ustawieniach** > **zadania** > **zadań Odzyskiwanie witryny**.
4. Zadanie pracy awaryjnej osiągnie fazy **badania wykonane** , wykonaj następujące czynności:

    -  Wyświetlanie replice maszyn wirtualnych w chmurze VMM pomocniczą.
    -  Kliknij przycisk **Zakończ test** aby zakończyć tym przełączeniu test.
    -  Kliknij **Notes** , aby nagrać, a wszelkie uwagi skojarzone z tym przełączeniu test.

5. Test maszyny wirtualnej zostanie utworzony na tym samym hoście jako host, na którym istnieje maszyny wirtualnej replice. Dodaje się w chmurze samej, w którym znajduje się maszyny wirtualnej replice.
6. Po zweryfikowaniu rozpoczynających się maszyny wirtualne pomyślnie kliknij **tym przełączeniu test zostało zakończone**. Na tym etapie są usuwane wszelkie elementy utworzone automatycznie przez Odzyskiwanie witryny w tym przełączeniu test.  

    > [AZURE.NOTE] Jeśli w trybie awaryjnym test trwa dłużej niż dwa tygodnie jest uzupełniona życie.



### <a name="update-dns-with-the-replica-vm-ip-address"></a>Aktualizacja DNS z replice adres IP maszyn wirtualnych

Po przełączeniu replice maszyn wirtualnych nie może być taki sam adres IP, jak podstawowy maszyny wirtualnej.

- Maszyn wirtualnych zaktualizuje serwera DNS, które używają po rozpoczęciu.
- Można też ręcznie zaktualizować DNS w następujący sposób:

#### <a name="retrieve-the-ip-address"></a>Pobieranie adresu IP

Uruchom ten przykładowy skrypt, aby pobrać adresu IP.

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

#### <a name="update-dns"></a>Aktualizacja DNS

Uruchom ten przykładowy skrypt, aby zaktualizować system DNS, określając adres IP, który można pobrać za pomocą przykładowy skrypt powyżej.

        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord

## <a name="next-steps"></a>Następne kroki

Po wdrożenia jest skonfigurowany i działa, [Dowiedz się więcej](site-recovery-failover.md) na temat różnych typów praca awaryjna.
