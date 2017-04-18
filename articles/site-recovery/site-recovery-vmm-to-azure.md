<properties
    pageTitle="Powielić maszyn wirtualnych funkcji Hyper-V w chmury VMM Azure Odzyskiwanie witryny za pomocą Azure portal | Microsoft Azure"
    description="W tym artykule opisano jak wdrożyć serwer Azure Odzyskiwanie witryny, aby dodać akompaniament replikacji, pracy awaryjnej i odzyskiwania maszyny wirtualne funkcji Hyper-V w chmury VMM Azure za pomocą portalu Azure"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/16/2016"
    ms.author="raynew"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-azure-site-recovery-with-the-azure-portal--microsoft-azure"></a>Powielić maszyn wirtualnych funkcji Hyper-V w chmury VMM Azure Odzyskiwanie witryny Azure za pomocą portalu Azure | Microsoft Azure

> [AZURE.SELECTOR]
- [Azure portal](site-recovery-vmm-to-azure.md)
- [Klasyczny Azure](site-recovery-vmm-to-azure-classic.md)
- [Menedżer zasobów programu PowerShell](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Klasyczny programu PowerShell](site-recovery-deploy-with-powershell.md)

Odzyskiwanie Azure witryny — Zapraszamy! Użyj tego artykułu, aby odtworzyć funkcji Hyper-V w lokalnym środowisku maszyn wirtualnych systemu zarządzania w chmury System Center Virtual Machine Manager (VMM) Azure za pomocą Odzyskiwanie witryny Azure w portalu Azure.

> [AZURE.NOTE]Azure ma dwa różne [modeli wdrażania](../resource-manager-deployment-model
> ) służące do tworzenia i pracy z zasobami: Menedżer zasobów Azure i klasyczny. Azure też ma dwa portali — portalu klasyczny Azure obsługuje model klasyczny wdrożenia i portal Azure z obsługą obu modeli wdrożenia.


Azure Odzyskiwanie witryny w portalu Azure udostępnia kilka nowych funkcji:

- W portalu Azure usługi Azure wykonywanie kopii zapasowych i odzyskiwanie witryny Azure są łączone w pojedynczy magazynu usługi odzyskiwania, dzięki czemu będzie można skonfigurować i zarządzanie ciągłości i odzyskiwanie (BCDR) z jednej lokalizacji. Ujednolicony pulpit nawigacyjny pozwala na monitorowanie i zarządzanie operacji za pośrednictwem witryny lokalnej i Azure chmura publicznej.
- Użytkownicy z subskrypcją Azure obsługi administracyjnej z programem chmury rozwiązanie dostawcy (dostawcy) mogą teraz zarządzać operacji odzyskiwania witryny w portalu Azure.
- Odzyskiwanie witryny w portalu Azure można replikować maszyn do Menedżera zasobów Azure miejsca do magazynowania kont. Po awaryjnym przeniesieniu Odzyskiwanie witryny tworzony oparte na Menedżera zasobów maszyny wirtualne platformy Azure.
- Odzyskiwanie witryny nadal obsługuje replikacji z kontami klasyczny miejsca do magazynowania. Po awaryjnym przeniesieniu Odzyskiwanie witryny tworzony maszyny wirtualne przy użyciu modelu Klasyczny.


Po zapoznaniu się w tym artykule, publikować komentarze u dołu w komentarzach Disqus. Zadawaj pytania techniczne na [Forum usługi odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Omówienie

Organizacje muszą strategii BCDR, która określa sposób aplikacje, obciążenia i danych utrzymywania działa i jest dostępny podczas zaplanowane i niezaplanowane przestoje i odzyskiwanie do normalnych warunkach pracy tak szybko, jak to możliwe. Strategii BCDR należy zachować dane biznesowe, bezpiecznych i odzyskania i upewnij się, obciążenia pozostają nieprzerwanie dostępne, gdy wystąpi awarii.

Odzyskiwanie witryny jest usługą Azure, która pozwala strategii BCDR orchestrating replikacji lokalnych serwerów fizycznych i maszyn wirtualnych w chmurze (Azure) lub pomocniczej centrum danych. W przypadku wystąpienia awarii w lokalizacji podstawowego nie na pomocniczym lokalizacji, aby aplikacje i obciążenia dostępne. Możesz zakończyć się niepowodzeniem powrót do swojej lokalizacji podstawowej podczas powraca do normalnego działania. Dowiedz się więcej na [Co to jest Azure witryny odzyskiwanie?](site-recovery-overview.md)

Ten artykuł zawiera wszystkie informacje, które chcesz odtworzyć maszyny wirtualne funkcji Hyper-V w chmury VMM Azure lokalnego. Zawiera omówienie architektury informacji i wdrażania instrukcje dotyczące konfigurowania Azure, lokalne serwery ustawień replikacji i planowaniu planowania. Po skonfigurowaniu infrastruktury można włączyć replikacji na komputerach, który chcesz chronić i sprawdź działania tej pracy awaryjnej.


## <a name="business-advantages"></a>Zalety firm

- Odzyskiwanie witryny znajdują się poza ochronę biznesowych i aplikacje na maszyny wirtualne funkcji Hyper-V.
- Portal usługi odzyskiwania miejsce na jednym Konfigurowanie, zarządzanie i monitorowanie replikacji, pracy awaryjnej i odzyskiwania.
- Można łatwo uruchamiać praca awaryjna z infrastruktury lokalnego Azure i powrotu (Przywróć) z platformy Azure do serwerów głównych funkcji Hyper-V w witrynie lokalnej.
- Plany odzyskiwania można konfigurować z wieloma komputerami, tak, aby warstwowych aplikacji niepowodzenie nad razem.


## <a name="scenario-architecture"></a>Architektura scenariusz


Są to składniki scenariusza:

- **Serwer VMM**: lokalnego serwera VMM z co najmniej jeden chmury.
- **Host funkcji Hyper-V lub klaster**: Serwery hosta funkcji Hyper-V lub klastrów zarządzania w VMM chmury.
- **Dostawca odzyskiwania witryny Azure i agenta usług odzyskiwania**: podczas wdrażania zainstalowaniu dostawcy odzyskiwania witryny Azure na serwerze VMM i agenta firmy Microsoft Azure odzyskiwania usług na serwerach hosta funkcji Hyper-V. Dostawca na serwerze VMM komunikuje się z Odzyskiwanie witryny przez 443 HTTPS do replikacji aranżacji. Agent na serwerze hosta funkcji Hyper-V replikuje dane do magazynu Azure nad HTTPS 443 domyślnie.
- **Azure**: potrzebna subskrypcji usługi Azure, konto Azure miejsca do magazynowania danych sklepu replikowane oraz Azure wirtualną sieć tak, aby maszyny wirtualne Azure są połączone z siecią po przełączeniu.

![Topologia E2A](./media/site-recovery-vmm-to-azure/architecture.png)


## <a name="azure-prerequisites"></a>Wymagania wstępne dotyczące Azure

Oto, co należy platformy Azure wdrażanie w tym scenariuszu.

**Wymagania wstępne** | **Szczegóły**
--- | ---
**Konto Azure**| Wymagane jest konto [Microsoft Azure](http://azure.microsoft.com/) . Możesz rozpocząć z [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/). [Aby uzyskać więcej informacji](https://azure.microsoft.com/pricing/details/site-recovery/) na temat ceny Odzyskiwanie witryny.
**Azure miejsca do magazynowania** | Potrzebne konto standardowego magazynu Azure, aby zapisać zreplikowanej dane. Za pomocą konta miejsca do magazynowania LRS lub GRS. Zalecamy GRS tak, aby dane mechanizm awaria regionalne, lub jeśli nie można odzyskać obszaru podstawowego. Aby [uzyskać więcej informacji](../storage/storage-redundancy.md). Konto musi być w tym samym regionie jako magazynu usługi odzyskiwania.<br/><br/>Magazyn Premium nie jest obsługiwana.<br/><br/> Replikowane dane są przechowywane w magazynie Azure i maszyny wirtualne Azure są tworzone, gdy wystąpi awaryjne przeniesienie. <br/><br/> [Przeczytaj o](../storage/storage-introduction.md) Miejsce do magazynowania Azure.
**Sieć Azure** | Potrzebujesz Azure wirtualnej sieci, który maszyny wirtualne Azure łączyć się, gdy wystąpi awaryjne przeniesienie. Sieć musi być w tym samym regionie jako magazynu usługi odzyskiwania.

## <a name="on-premises-prerequisites"></a>Wymagania wstępne dotyczące lokalnego

Poniżej opisano, co jest potrzebne w lokalnym

**Wymagania wstępne** | **Szczegóły**
--- | ---
**VMM**| Co najmniej jeden VMM serwery mają zainstalowany w systemie Centrum 2012 R2. Każdy serwer VMM powinien mieć co najmniej jeden chmury skonfigurowane. Chmurę powinien zawierać:<br/><br/> Jedną lub więcej grup hosta VMM.<br/><br/> Jeden lub więcej serwerów głównych funkcji Hyper-V lub klastrów w każdej grupie hosta.<br/><br/>[Aby uzyskać więcej informacji](http://www.server-log.com/blog/2011/8/26/vmm-2012-and-the-clouds.html) o konfigurowaniu VMM chmury.
**Funkcji Hyper-V** | Serwery hosta funkcji Hyper-V musi działać co najmniej **Windows Server 2012 R2** z roli Hyper-V lub **Microsoft funkcji Hyper-V Server 2012 R2** i zainstalowano najnowsze aktualizacje.<br/><br/> Serwer funkcji Hyper-V powinien zawierać co najmniej jeden maszyny wirtualne.<br/><br/> Serwer hosta funkcji Hyper-V lub klaster, który zawiera dane mają być replikowane maszyny wirtualne muszą być zarządzane w chmurze VMM.<br/><br/>Serwery funkcji Hyper-V powinny być połączone z Internetem, bezpośrednio lub za pośrednictwem serwera proxy.<br/><br/>Serwery funkcji Hyper-V powinny mieć poprawki wymienione w artykule [2961977](https://support.microsoft.com/kb/2961977) zainstalowany.<br/><br/>Serwery hosta funkcji Hyper-V potrzebny dostęp do Internetu dla replikacji danych Azure.
**Dostawca i agenta** | Podczas rozmieszczania Odzyskiwanie witryny Azure będzie instalowanie dostawcy odzyskiwania witryny Azure na serwerze VMM i agenta usługi odzyskiwania na hosts funkcji Hyper-V. Dostawca i agenta wymaga nawiązania Azure bezpośrednio w Internecie lub za pośrednictwem serwera proxy. Należy zauważyć, że serwer proxy oparte na HTTPS nie jest obsługiwane. Serwer proxy na serwerze VMM i funkcji Hyper-V hosts należy zezwolić na dostęp do: <br/><br/> *. hypervrecoverymanager.windowsazure.com <br/> <br/> *. accesscontrol.windows.net <br/><br/> *. backup.windowsazure.com <br/> <br/> *. blob.core.windows.net <br/><br/> *. store.core.windows.net<br/><br/>Jeśli masz reguły zapory oparte na adresie IP na serwerze VMM, sprawdź zasady zezwalają na komunikację Azure. Należy zezwolić [Zakresów adresów IP centrum danych Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) oraz portu HTTPS (443).<br/><br/>Zezwalaj na zakresy adresów IP dla Azure region subskrypcji i zachód USA.<br/><br/>W dodatku. Serwer proxy na serwerze VMM musi mieć dostęp do https://www.msftncsi.com/ncsi.txt


## <a name="protected-machine-prerequisites"></a>Wymagania wstępne dotyczące chronionego komputera


**Wymagania wstępne** | **Szczegóły**
--- | ---
**Chroniony maszyny wirtualne** | Zanim nie zostanie przez maszyny, upewnij się, że nazwę, która jest przypisana do maszyn wirtualnych Azure spełnia [wymagania wstępne dotyczące Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements). Po włączeniu replikacji maszyn wirtualnych, możesz zmienić nazwę. <br/><br/> Poszczególne pojemność dysku na komputerach chronionej nie powinny być więcej niż 1023 GB. Maszyny może zawierać maksymalnie 16 dysków (a więc do 16 TB).<br/><br/> Udostępniane gościa dysku, które nie są obsługiwane klastrów.<br/><br/> Unified Extensible sprzętowego Interface (UEFI) / Extensible Interface(EFI) sprzętowego uruchamiania nie jest obsługiwane.<br/><br/> Jeśli źródło maszyn wirtualnych ma NIC zespołu jest konwertowany do pojedynczej NIC po przełączeniu Azure.<br/><br/>Ochrona maszyny wirtualne systemem Linux przy użyciu statycznego adresu IP nie jest obsługiwana.

## <a name="prepare-for-deployment"></a>Przygotowywanie do wdrożenia

Aby przygotować się do wdrożenia, które należy:

1. [Konfigurowanie Azure sieci](#set-up-an-azure-network) , w której maszyny wirtualne Azure zostaną umieszczone po przełączeniu.
2. [Konfigurowanie konta usługi Azure miejsca do magazynowania](#set-up-an-azure-storage-account) dla zreplikowanej danych.
4. [Przygotuj na serwerze VMM](#prepare-the-vmm-server) wdrożenia Odzyskiwanie witryny.
5. [Przygotowywanie do mapowania sieci](#prepare-for-network-mapping). Konfigurowanie sieci, dzięki czemu można skonfigurować mapowanie sieci podczas wdrażania Odzyskiwanie witryny.

### <a name="set-up-an-azure-network"></a>Skonfiguruj sieć Azure

Potrzebujesz sieć Azure tak, aby maszyny wirtualne Azure utworzonych po pracy awaryjnej będzie się z nim połączyć.

- Sieć powinna być w tym samym regionie, w którym Wdrażanie magazynu usługi odzyskiwania.
- W zależności od modelu zasobu, który ma być używany dla nie powiodło się nad maszyny wirtualne Azure można konfigurować Azure sieci w [trybie Menedżer zasobów](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) lub w [trybie klasycznym](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).
- Zaleca się, że możesz skonfigurować sieć przed rozpoczęciem. Jeśli Cię nie musisz to zrobić podczas wdrażania Odzyskiwanie witryny.

> [AZURE.NOTE] [Migracja sieci](../resource-group-move-resources.md) wzdłuż grup zasobów w obrębie tej samej subskrypcji lub w subskrypcjach nie jest obsługiwane dla sieci używane do wdrażania Odzyskiwanie witryny.


### <a name="set-up-an-azure-storage-account"></a>Konfigurowanie konta usługi Azure miejsca do magazynowania

- Musisz mieć konto standardowego Azure magazynu do przechowywania danych replikować do Azure. Konto musi być w tym samym regionie jako magazynu usługi odzyskiwania.
- W zależności od modelu zasobu, który ma być używany dla nie powiodło się nad pośrednictwem Azure SMS możesz skonfigurować konto w [trybie Menedżer zasobów](../storage/storage-create-storage-account.md) lub w [trybie klasycznym](../storage/storage-create-storage-account-classic-portal.md).
- Zaleca się Konfigurowanie konta przed rozpoczęciem. Jeśli Cię nie musisz to zrobić podczas wdrażania Odzyskiwanie witryny.

> [AZURE.NOTE] [Migracja kont miejsca do magazynowania](../resource-group-move-resources.md) w grup zasobów w obrębie tej samej subskrypcji lub w subskrypcjach nie jest obsługiwane w przypadku kont miejsca do magazynowania na potrzeby wdrażania Odzyskiwanie witryny.

### <a name="prepare-the-vmm-server"></a>Przygotuj serwer VMM

- Upewnij się, że serwer VMM spełnia [wymagania wstępne](#on-premises-prerequisites).
- Podczas rozmieszczania Odzyskiwanie witryny można określić, że wszystkie chmury na serwerze VMM powinna być dostępna w portalu Azure. Jeśli chcesz tylko określonych chmury pojawiały się w portalu, możesz włączyć to ustawienie w chmurze w konsoli administracyjnej VMM.


### <a name="prepare-for-network-mapping"></a>Przygotowywanie do mapowania sieci

Musisz skonfigurować mapowanie sieci podczas wdrażania Odzyskiwanie witryny. Mapowanie sieci mapy między źródła VMM maszyn wirtualnych sieci i docelowych Azure sieciach, zapewniając następujące czynności:

- Maszyn, których nie powiodło się nad w tej samej sieci można połączyć ze sobą, nawet jeśli nie jest niepowodzenia nad w taki sam sposób lub tego samego planu odzyskiwania.
- Jeśli brama sieci jest skonfigurowana w celu Azure sieci, Azure maszyn wirtualnych można nawiązać maszyn wirtualnych lokalnego.
- Aby skonfigurować sieć poniżej mapowanie jest co jest potrzebne do przygotowania:

    - Upewnij się, że maszyny wirtualne na serwerze hosta funkcji Hyper-V źródła jest połączony z siecią VMM maszyn wirtualnych. Sieci należy połączyć z siecią logiczną, która jest skojarzony z chmury.
    - Azure sieci, jak opisano [powyżej](#set-up-an-azure-network)

- [Aby uzyskać więcej informacji](site-recovery-network-mapping.md) o sposobie działania mapowanie sieci.


## <a name="create-a-recovery-services-vault"></a>Tworzenie magazynu usługi odzyskiwania

1. Zaloguj się do [portalu Azure](https://portal.azure.com).
2. Kliknij przycisk **Nowy** > **zarządzania** > **usługi odzyskiwania**. Alternatywnie kliknij **Przeglądaj** > **Usługi odzyskiwania** magazynów > **Dodaj**.

    ![Nowe magazynu](./media/site-recovery-vmm-to-azure/new-vault3.png)

3. W polu **Nazwa**Określ przyjazną nazwę identyfikującą magazyn. Jeśli masz więcej niż jedną subskrypcję, wybierz jeden z nich.
4. [Tworzenie grupy zasobów](../resource-group-template-deploy-portal.md), lub wybierz istniejący. Określ Azure region. Komputery będzie można replikować do tego obszaru. Aby sprawdzić obsługiwanych regionów, zobacz dostępność geograficzne w [Azure Szczegóły ceny odzyskiwania witryny](https://azure.microsoft.com/pricing/details/site-recovery/)
4. Jeśli chcesz szybko uzyskać magazyn z pulpitu nawigacyjnego, kliknij polecenie **Przypnij do pulpitu nawigacyjnego** > **Tworzenie magazynu**.

    ![Nowe magazynu](./media/site-recovery-vmm-to-azure/new-vault-settings.png)

Nowy magazyn pojawi się na **pulpicie nawigacyjnym** > **wszystkie zasoby**i na głównym karta **magazynów usługi odzyskiwania** .

## <a name="getting-started"></a>Wprowadzenie

Odzyskiwanie witryny zawiera wprowadzenie do środowiska, który pomoże Ci wdrażanie tak szybko, jak to możliwe. Wprowadzenie do sprawdza wymagania wstępne i przeprowadzi Cię przez Odzyskiwanie witryny wdrożenia kroki w odpowiedniej kolejności.

Wprowadzenie, wybierz typ maszyn, który chcesz odtworzyć, a miejsce, w którym mają być replikowane w. Możesz skonfigurować lokalne serwery, konta magazynu platformy Azure i sieci. Tworzenie zasad replikacji i zaplanowanie wydajności. Po skonfigurowaniu infrastruktury umożliwia replikacji maszyny wirtualne. Można uruchomić praca awaryjna na określonych komputerach lub tworzenie planów odzyskiwania niepowodzenie na wielu komputerach.

Rozpocznij wprowadzenie, wybierając pozycję jak chcesz wdrożyć Odzyskiwanie witryny. Wprowadzenie do przepływów nieco zmienia się w zależności od potrzeb replikacji.



## <a name="step-1-choose-your-protection-goals"></a>Krok 1: Wybierz cele ochrony

Zaznacz dane mają być replikowane i miejsce, w którym mają być replikowane w.

1. W karta **magazynów usługi odzyskiwania** wybierz z magazynu, a następnie kliknij przycisk **Ustawienia**.
2. **Wprowadzenie** kliknij pozycję **Odzyskiwanie witryny** > **Krok 1: Przygotowywanie infrastruktury** > **cel ochrony**.

    ![Wybierz cele](./media/site-recovery-vmm-to-azure/choose-goals.png)

3. W **celu ochrony** zaznacz **Azure do**, a następnie wybierz pozycję **Tak, przy użyciu funkcji Hyper-V**. Wybierz pozycję **Tak,** aby potwierdzić, że używasz VMM hosts funkcji Hyper-V i witryny odzyskiwania do zarządzania. Kliknij przycisk **OK**.

    ![Wybierz cele](./media/site-recovery-vmm-to-azure/choose-goals2.png)



## <a name="step-2-set-up-the-source-environment"></a>Krok 2: Konfigurowanie środowiska źródła

Instalowanie dostawcy odzyskiwania witryny Azure na serwerze VMM i zarejestrować serwer w magazyn. Instalowanie agenta usługi Azure odzyskiwania na hosts funkcji Hyper-V.

1. Kliknij pozycję **Krok 2: Przygotowywanie infrastruktury** > **źródła**.

    ![Ustawianie źródła](./media/site-recovery-vmm-to-azure/set-source1.png)

2. W **źródle przygotowywanie** kliknij **+ VMM** Aby dodać serwer VMM.

    ![Ustawianie źródła](./media/site-recovery-vmm-to-azure/set-source2.png)

3. W Sprawdź karta **Dodaj serwer** , czy **serwer VMM Centrum systemu** pojawia się **Typ serwera** , a na serwerze VMM spełnia [wymagania wstępne i wymagania dotyczące adresu URL](#on-premises-prerequisites).
4. Pobierz plik instalacyjny Azure witryny odzyskiwania dostawcy.
5. Pobierz klucz rejestru. Potrzebujesz to po uruchomieniu Instalatora. Klucz jest prawidłowy przez pięć dni po jego wygenerować.

    ![Ustawianie źródła](./media/site-recovery-vmm-to-azure/set-source3.png)

6. Instalowanie dostawcy odzyskiwania witryny Azure na serwerze VMM.


### <a name="set-up-the-azure-site-recovery-provider"></a>Konfigurowanie dostawcy odzyskiwania witryny Azure

1.  Uruchom plik Instalatora dostawcy.
2. W **Witrynie Microsoft Update** można jednak aktualizacje tak, aby zainstalowanych aktualizacji dostawcy zgodnie z zasadami usługi Microsoft Update.
3. W **instalacji**zaakceptować lub modyfikowanie domyślnej lokalizacji instalacji dostawcy, a następnie kliknij przycisk **Zainstaluj**.

    ![Lokalizacja instalacji](./media/site-recovery-vmm-to-azure/provider2.png)

4. Po zakończeniu instalacji kliknij przycisk **Zarejestruj** , aby zarejestrować serwer VMM w magazyn.
5. Na stronie **Ustawienia magazynu** kliknij przycisk **Przeglądaj** , aby wybrać plik klucza magazynu. Określ subskrypcji Azure Odzyskiwanie witryny i nazwę magazynu.

    ![Rejestracja serwera](./media/site-recovery-vmm-to-azure/provider10.PNG)

6. W **Połączenie internetowe**Określ, jak dostawcy na serwerze VMM połączy się Odzyskiwanie witryny przez internet.

    - Jeśli chcesz, aby połączyć bezpośrednio dostawcy zaznacz pole wyboru **Połącz bezpośrednio do Odzyskiwanie witryny Azure bez serwera proxy**.
    - Serwer proxy istniejących wymaga uwierzytelniania, czy ma być używany wybierz niestandardowy serwera proxy, **Nawiązywanie połączenia z Odzyskiwanie witryny Azure za pomocą serwera proxy**.
    - Jeśli korzystasz z niestandardowej serwera proxy, określ adres, port i poświadczenia.
    - Jeśli korzystasz z serwera proxy mają powinna już dozwolone adresy URL opisanego w [wymagania wstępne](#on-premises-prerequisites).
    - Jeśli korzystasz z niestandardowej serwera proxy konta VMM RunAs (DRAProxyAccount) jest tworzona automatycznie przy użyciu poświadczeń określonego serwera proxy. Konfigurowanie serwera proxy, aby to konto można uwierzytelnić pomyślnie. W konsoli VMM można modyfikować ustawienia konta VMM RunAs. W przypadku **Ustawienia**, rozwiń węzeł **Zabezpieczenia** > **Uruchomić jako konta**, a następnie zmodyfikuj hasła dla DRAProxyAccount. Musisz ponownie uruchomić usługę VMM, tak aby to ustawienie jest stosowane.

    ![Internet](./media/site-recovery-vmm-to-azure/provider13.PNG)

7. Zaakceptuj lub zmodyfikuj lokalizację certyfikatu SSL, który jest generowany automatycznie do szyfrowania danych. Ten certyfikat jest używany po włączeniu szyfrowania danych składnika chmura Azure chronione w portalu Odzyskiwanie witryny Azure. Zabezpieczyć ten certyfikat. Po uruchomieniu trybie awaryjnym Azure musisz go, aby odszyfrować, jeśli została włączona funkcja szyfrowania danych.


8. W polu **Nazwa serwera**Określ przyjazną nazwę identyfikującą serwer VMM w magazyn. W konfiguracji klaster Określ nazwę roli klaster VMM.
9. Włącz **metadanych chmury synchronizacji** , jeśli chcesz zsynchronizować z magazynu metadanych dla wszystkich chmur na serwerze VMM. Ta akcja musi tylko raz wystąpić na wszystkich serwerach. Jeśli nie chcesz zsynchronizować wszystkie chmury, można pozostawić niezaznaczone to ustawienie i synchronizowanie każdego chmury pojedynczo w chmurze właściwości w konsoli VMM. Kliknij przycisk **Zarejestruj** , aby ukończyć proces.

    ![Rejestracja serwera](./media/site-recovery-vmm-to-azure/provider16.PNG)

10. Zostanie uruchomiony rejestracji. Po zakończeniu rejestracji serwera jest wyświetlany na stronie **Ustawienia** > karta**Serwery** w magazyn.


#### <a name="command-line-installation-for-the-azure-site-recovery-provider"></a>Instalacja wiersza polecenia dla dostawcy odzyskiwania witryny Azure

Dostawca odzyskiwania witryny Azure można zainstalować z poziomu wiersza polecenia. Ta metoda umożliwia instalowanie dostawcy na serwerze Core dla systemu Windows Server 2012 R2.

1. Pobierz klucz pliku i rejestracji instalacji dostawcy do folderu. Na przykład C:\ASR.
2. W wierszu polecenia z podwyższonym poziomem uprawnień Uruchom te polecenia, aby wyodrębnić Instalatora dostawcy:

            C:\Windows\System32> CD C:\ASR
            C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Uruchom to polecenie, aby zainstalować składniki:

            C:\ASR> setupdr.exe /i

4. Następnie uruchom te polecenia, aby zarejestrować serwer w Magazyn:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>       

W przypadku gdy:

- **/Credentials**: obowiązkowe parametr, który określa, w którym znajduje się plik klucza rejestru.  
- **/FriendlyName**: parametr obowiązkowe dla nazwy serwera hosta funkcji Hyper-V, który pojawia się w portalu Odzyskiwanie witryny Azure.
- - **/EncryptionEnabled**: parametr opcjonalny, gdy masz replikacji maszyny wirtualne funkcji Hyper-V w VMM chmury Azure. Określ, czy chcesz zaszyfrować maszyn wirtualnych platformy Azure (w pozostałych szyfrowanie). Upewnij się, że nazwa pliku ma rozszerzenie **pfx** . Szyfrowanie jest domyślnie wyłączona.
- **/proxyAddress**: parametr opcjonalny, który określa adres serwera proxy.
- **/proxyport**: parametr opcjonalny, który określa port serwera proxy.
- **/proxyUsername**: parametr opcjonalny określający nazwę użytkownika serwera proxy (Jeśli serwer proxy wymaga uwierzytelniania).
- **/proxyPassword**: parametr opcjonalny, który określa hasło do uwierzytelniania serwera proxy (Jeśli serwer proxy wymaga uwierzytelniania).


### <a name="install-the-azure-recovery-services-agent-on-hyper-v-hosts"></a>Instalowanie agenta usługi Azure odzyskiwania na hosts funkcji Hyper-V

1. Po skonfigurowaniu dostawcę należy pobrać plik instalacji agenta usługi Azure odzyskiwania. Uruchom Instalatora na wszystkich serwerach funkcji Hyper-V w chmurze VMM.

    ![Witryny funkcji Hyper-V](./media/site-recovery-vmm-to-azure/hyperv-agent1.png)

2. Na stronie **Sprawdź wymagania wstępne** kliknij przycisk **Dalej**. Wymagania wstępne dotyczące wszelkie brakujące zostanie zainstalowany automatycznie.

    ![Wymagania wstępne dotyczące odzyskiwania usług agenta](./media/site-recovery-vmm-to-azure/hyperv-agent2.png)

3. Na stronie **Ustawienia instalacji** Zaakceptuj lub modyfikowanie lokalizacji instalacji i lokalizacja pamięci podręcznej. Możesz skonfigurować pamięci podręcznej na dysku, który ma co najmniej 5 GB miejsca do magazynowania, ale zaleca się dysk pamięci podręcznej z najmniej 600 GB wolnego miejsca. Następnie kliknij przycisk **Zainstaluj**.
4. Po zakończeniu instalacji kliknij przycisk **Zamknij** , aby zakończyć.

    ![Agent MARS rejestru](./media/site-recovery-vmm-to-azure/hyperv-agent3.png)

#### <a name="command-line-installation-for-azure-site-recovery-services-agent"></a>Instalacja wiersza polecenia dla agenta usługi odzyskiwania witryny Azure

Agent usługi Microsoft Azure odzyskiwania można zainstalować z wiersza polecenia przy użyciu następującego polecenia:

     marsagentinstaller.exe /q /nu

#### <a name="set-up-internet-proxy-access-to-site-recovery-from-hyper-v-hosts"></a>Konfigurowanie dostępu do serwera proxy internet do Odzyskiwanie witryny z hostów funkcji Hyper-V

Agent usługi odzyskiwania uruchomionych hosts funkcji Hyper-V musi dostęp do Internetu Azure replikacji maszyn wirtualnych. Jeśli masz dostęp do Internetu za pośrednictwem serwera proxy, skonfiguruj ją w następujący sposób:

1. Otwieranie przystawki Microsoft Azure kopia zapasowa MMC na hoście funkcji Hyper-V. Domyślnie skrót kopia zapasowa Microsoft Azure jest dostępny na komputerze lub w C:\Program Files\Microsoft Azure odzyskiwania usług Agent\bin\wabadmin.
2. W polu przystawki kliknij pozycję **Zmień właściwości**.
3. Na karcie **Konfiguracji serwera Proxy** Określ informacje o serwerze proxy.

    ![Agent MARS rejestru](./media/site-recovery-vmm-to-azure/mars-proxy.png)

4. Upewnij się, że agent może osiągnąć adresy URL opisanego w [wymagania wstępne](#on-premises-prerequisites).


## <a name="step-3-set-up-the-target-environment"></a>Krok 3: Konfigurowanie środowiska docelowej

Określanie konta magazynu platformy Azure ma być używany dla replikacji i sieci Azure, do którego maszyny wirtualne Azure połączy się po przełączeniu.

1.  Kliknij pozycję **Przygotuj infrastruktury** > **docelowej** i wybierz Azure subskrypcji, którego chcesz użyć.
2.  Określ model wdrożenia, który ma być używany dla maszyny wirtualne po przełączeniu.
3.  Odzyskiwanie witryny sprawdza, czy masz kont zgodne Azure miejsca do magazynowania lub sieci.

    ![Miejsca do magazynowania](./media/site-recovery-vmm-to-azure/compatible-storage.png)

4.  Jeśli nie utworzono konto miejsca do magazynowania i chcesz je utworzyć za pomocą Menedżera zasobów, kliknij pozycję **+ konto miejsca do magazynowania** w tym tekście.  Na karta **Tworzenie miejsca do magazynowania konta** Określ nazwę konta, typ, subskrypcji i lokalizacji. Konto powinno być w tym samym miejscu jako magazynu usługi odzyskiwania.

    ![Miejsca do magazynowania](./media/site-recovery-vmm-to-azure/gs-createstorage.png)

    Należy zauważyć, że:

    - Jeśli chcesz utworzyć konto miejsca do magazynowania przy użyciu modelu Klasyczny, można to zrobić w portalu Azure. [Dowiedz się więcej](../storage/storage-create-storage-account-classic-portal.md)
    - Jeśli używasz konta miejsca do magazynowania premium zreplikowanej danych, musisz skonfigurować konto dodatkowego miejsca do magazynowania standardowy do przechowywania dzienniki replikacji, które trwających zmian w danych lokalnych.

4.  Jeśli nie utworzono Azure sieci i chcesz je utworzyć za pomocą Menedżera zasobów kliknij **+ sieć** w tym tekście. Na karta **Tworzenie wirtualnych sieci** Określ nazwę sieciową, zakres adresów, szczegóły podsieci, subskrypcji i lokalizacji. Sieć powinna być w tym samym miejscu jako magazynu usługi odzyskiwania.

    ![Sieci](./media/site-recovery-vmm-to-azure/gs-createnetwork.png)

    Jeśli chcesz utworzyć sieć przy użyciu modelu Klasyczny będzie to zrobić w portalu Azure. Aby [uzyskać więcej informacji](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

### <a name="configure-network-mapping"></a>Konfigurowanie mapowanie sieci

- [Odczyt](#prepare-for-network-mapping) wykonuje krótkie omówienie jakie mapowanie sieci. [Przeczytaj ten artykuł](site-recovery-network-mapping.md) , aby uzyskać więcej informacji na temat objaśnienie.
- Upewnij się, że maszyn wirtualnych na serwerze VMM są podłączone do sieci maszyn wirtualnych i czy został utworzony co najmniej jeden Azure wirtualnej sieci. Wiele maszyn wirtualnych sieci mogą być mapowane na jednej sieci Azure.

Konfigurowanie mapowania w następujący sposób:

1. W obszarze **Ustawienia** > **Infrastrukturą odzyskiwania witryny** > **mapowania sieci** > **Mapowanie sieci**, kliknij ikonę **+ Mapowanie sieci** .

    ![Mapowanie sieci](./media/site-recovery-vmm-to-azure/network-mapping1.png)

2. **Dodaj mapowanie sieci** wybierz serwer źródłowy VMM i **Azure** jako miejsce docelowe.
3. Sprawdź subskrypcję i model wdrażania po przełączeniu.
4. **Sieć źródłowa**wybierz sieć maszyn wirtualnych lokalnego źródła, którą chcesz zamapować na liście skojarzonych z serwerem VMM.
5. W **sieci docelowej**zaznacz Azure sieci, w których replice maszyny wirtualne Azure zostaną umieszczone po są generowane. Kliknij przycisk **OK**.

    ![Mapowanie sieci](./media/site-recovery-vmm-to-azure/network-mapping2.png)

Oto, co się dzieje, gdy zaczyna się mapowanie sieci:

- Istniejące maszyny wirtualne sieci maszyn wirtualnych źródła są połączone z siecią docelową po rozpoczęciu mapowania. Nowe maszyny wirtualne połączony z siecią maszyn wirtualnych źródła są podłączone do sieci Azure zamapowanych podczas replikacji.
- Po zmodyfikowaniu istniejącego mapowania sieci maszyn wirtualnych replice zostaną połączone za pomocą nowych ustawień.
- Jeśli jeden z tych podsieci ma taką samą nazwę jak podsieci, na którym znajduje się maszyny wirtualnej źródła sieci docelowej zawiera wiele podsieci, następnie maszyny wirtualnej replice będzie połączony z tej podsieci docelowej po przełączeniu.
- W przypadku nie podsieć docelowej z odpowiednia nazwa maszyny wirtualnej zostanie połączony z pierwszą podsieć w sieci.



## <a name="step-4-set-up-replication-settings"></a>Krok 4: Konfigurowanie ustawień replikacji


1. Aby utworzyć nową zasadę replikacji, kliknij pozycję **Przygotuj infrastruktury** > **Ustawień replikacji** > **+ Utwórz i skojarz**.

    ![Sieci](./media/site-recovery-vmm-to-azure/gs-replication.png)

2. **Tworzenie i kojarzenie zasady**Określ nazwę zasady.
3. W, **Skopiuj częstotliwość**Określ, jak często chcesz replikować różnicy danych po początkowej replikacji (co 30 sekund, 5 lub 15 minut).
4. W **odzyskiwania punktu przechowywania**określ w godzinach czasu okno przechowywania będzie dla każdego punktu odzyskiwania. Chroniony maszyn może zostać przywrócona do dowolnego punktu w oknie.
6. **Spójne aplikacji częstotliwość migawkę**Określ, jak często (1 – 12 godzin) punktów odzyskiwania zawierające migawek spójnych aplikacji będzie utworzona. Funkcji Hyper-V używa dwóch typów migawek — standardowy migawkę zawierającego przyrostowe migawki całego maszyny wirtualnej i migawkę spójną z aplikacją, która pobiera migawkę w chwili dane aplikacji wewnątrz maszyny wirtualnej. Migawek spójnych aplikacji upewnij się, że aplikacje są spójna przy podejmowaniu migawki za pomocą usługi kopiowania woluminu w tle (VSS). Należy zauważyć, że po włączeniu migawek spójnych aplikacji wpłynie to na wydajność aplikacji uruchomionych maszyn wirtualnych źródła. Upewnij się, że ustawioną wartość jest mniejsza niż liczba punktów odzyskiwania dodatkowe, które można skonfigurować.
3. W polu **czas rozpoczęcia replikacji początkowy**wskazują momentu rozpoczęcia replikacji początkowej. Replikacja odbywa się za pośrednictwem usługi przepustowości internetowej, warto zaplanować go poza zajęty godziny.
5. **Szyfruj dane przechowywane w Azure**Określ, czy szyfrowanie pozostałe dane w magazynie Azure. Kliknij przycisk **OK**.

    ![Zasady replikacji](./media/site-recovery-vmm-to-azure/gs-replication2.png)

6. Podczas tworzenia nowych zasad automatycznie jest skojarzony z chmurą VMM. Kliknij **przycisk OK**. Dodatkowe chmury VMM (i maszyny wirtualne w nich) można skojarzyć z tymi zasadami replikacji w **ustawieniach** > **replikacji** > Nazwa zasady > **Skojarzyć VMM w chmurze**.

    ![Zasady replikacji](./media/site-recovery-vmm-to-azure/policy-associate.png)

## <a name="step-5-capacity-planning"></a>Krok 5: Planowanie wydajności

Teraz, gdy masz podstawowe usługi infrastruktury możesz skonfigurować możesz myśleć o planowaniu i wiadomo, czy potrzebujesz dodatkowych zasobów.

Odzyskiwanie witryny zawiera planowania pojemności ułatwiające przydzielanie zasobów do środowiska źródła, składniki odzyskiwania witryny, sieci i przechowywania. Terminarz może zostać uruchomiony w trybie szybkim dla ocen na podstawie średniej liczby maszyny wirtualne, dyski i miejsca do magazynowania lub w trybie szczegółowym, w której będzie wprowadzania dane na poziomie obciążenie pracą. Przed rozpoczęciem musisz:

- Gromadzenie informacji o środowisku replikacji, łącznie z maszyny wirtualne dysków za pośrednictwem SMS i miejsca do magazynowania na dysku.
- Szacowanie dziennego kursu Zmień (pochodząca), które będą dostępne dla zreplikowanej danych. Za pomocą [planowania pojemności dla funkcji Hyper-V replice](https://www.microsoft.com/download/details.aspx?id=39057) ułatwiające wykonaj następujące czynności.

1.  Kliknij przycisk **Pobierz** , aby pobrać narzędzie, a następnie uruchom go. [Przeczytaj artykuł](site-recovery-capacity-planner.md) dostarczonej narzędzie.
2.  Po zakończeniu wybierz opcję **Tak** w **został uruchomiony planowania pojemności**?

    ![Planowanie pojemności](./media/site-recovery-vmm-to-azure/gs-capacity-planning.png)

### <a name="network-bandwidth-considerations"></a>Zagadnienia dotyczące przepustowości sieci

Narzędzie do planowania pojemności służy do obliczania przepustowości, potrzebnych dla replikacji (replikacji początkowej, a następnie delta). Aby sterować ilością wykorzystania przepustowości replikacji dostępnych jest kilka opcji:

- **Przepustowości**: ruch funkcji Hyper-V replikuje na pomocniczym witryny przechodzi określonym hoście funkcji Hyper-V. Czy przepustowości na serwerze hosta.
- **Dostosować a przepustowości**: można wpływać na przepustowość dla replikacji przy użyciu kilka kluczach rejestru.

#### <a name="throttle-bandwidth"></a>Przepustowości

1. Otwieranie przystawki Microsoft Azure kopia zapasowa MMC na serwerze hosta funkcji Hyper-V. Domyślnie skrót kopia zapasowa Microsoft Azure jest dostępny na komputerze lub w C:\Program Files\Microsoft Azure odzyskiwania usług Agent\bin\wabadmin.
2. W polu przystawki kliknij pozycję **Zmień właściwości**.
3. Na karcie **Throttling** zaznacz pole wyboru **Włącz wykorzystania przepustowości internetowej ograniczania dla kopii zapasowych**, ustawianie limitów pracy i wartością pracy godziny. Prawidłowe zakresy są od 512 KB/s 102 MB/s na sekundę.

    ![Przepustowości](./media/site-recovery-vmm-to-azure/throttle2.png)

Za pomocą polecenia cmdlet [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) ustawić ograniczania. Oto przykładowa:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Zestaw OBMachineSetting NoThrottle** wskazuje, że nie ograniczania jest wymagane.


#### <a name="influence-network-bandwidth"></a>Mieć wpływ na przepustowość sieci

Wartość rejestru **UploadThreadsPerVM** określa liczbę wątków, które są używane do przesyłania danych (początkowy lub zmiana replikacji) dysku. Wyższa wartość zwiększa przepustowość sieci replikacji. Wartość rejestru **DownloadThreadsPerVM** określa liczbę wątków używanych przesyłania danych podczas awarii.

1. W rejestrze przejdź do **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.

    - Modyfikowanie wartości **UploadThreadsPerVM** (lub utworzyć klucza, jeśli nie istnieje) wątków sterowania replikacji dysku.
    - Modyfikowanie wartości **DownloadThreadsPerVM** (lub utworzyć klucza, jeśli nie istnieje) wątków sterowania na potrzeby ruchu awarii z Azure.
2. Wartość domyślna to 4. W sieci "overprovisioned" należy zmienić klucze rejestru z wartościami domyślnymi. Wartość maksymalna to 32. Monitorowanie ruchu w celu zoptymalizowania wartość.

## <a name="step-6-enable-replication"></a>Krok 6: Włącz replikacji

Teraz włączyć replikacji w następujący sposób:

1. Kliknij pozycję **Krok 2: powielić aplikacji** > **źródła**. Po włączeniu replikacji po raz pierwszy kliknij **+ replikacji** w magazynu, aby włączyć replikacji na dodatkowych komputerach.

    ![Włączanie replikacji](./media/site-recovery-vmm-to-azure/enable-replication1.png)

2. W karta **źródła** > Wybierz serwer VMM oraz chmury, w którym znajdują się hosts funkcji Hyper-V. Kliknij przycisk **OK**.

    ![Włączanie replikacji](./media/site-recovery-vmm-to-azure/enable-replication-source.png)

3. W **docelowej** Wybierz subskrypcję, model wdrożenia wpis w przypadku awarii i konto miejsca do magazynowania, którego używasz zreplikowanej danych.

    ![Włączanie replikacji](./media/site-recovery-vmm-to-azure/enable-replication-target.png)

4. Wybierz konto miejsca do magazynowania, który ma być używany. Jeśli chcesz korzystać z konta innego miejsca do magazynowania niż masz użytkownik może [utworzyć](#set-up-an-azure-storage-account). Aby utworzyć miejsca do magazynowania konta przy użyciu modelu Menedżera zasobów kliknij pozycję **Utwórz nowy**. Jeśli chcesz utworzyć konto miejsca do magazynowania przy użyciu modelu Klasyczny, jak tego [w portalu Azure](../storage/storage-create-storage-account-classic-portal.md). Kliknij przycisk **OK**.
5. Wybierz pozycję Azure sieci i podsieci, do której maszyny wirtualne Azure połączy, gdy są one surowa w górę po przełączeniu. Wybierz pozycję **Konfiguruj teraz na wybranych komputerach** , aby zastosować ustawienie sieci do wszystkich wybranych komputerach ochrony. Wybierz pozycję **Konfiguruj później** , aby wybrać Azure sieć na komputer. Jeśli chcesz użyć innej sieci od tych masz użytkownik może [utworzyć](#set-up-an-azure-network). Aby utworzyć sieci przy użyciu modelu Menedżera zasobów kliknij pozycję **Utwórz nowy**. Jeśli chcesz utworzyć sieć przy użyciu modelu Klasyczny, wykonaj tę [w portalu Azure](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). W razie potrzeby wybierz podsieć. Kliknij przycisk **OK**.
6. W **środowisku maszyn wirtualnych** > **Wybierz maszyn wirtualnych** kliknij i wybierz na każdym komputerze, który chcesz odtworzyć. Możesz wybrać tylko komputerów, dla których można włączyć replikacji. Kliknij przycisk **OK**.

    ![Włączanie replikacji](./media/site-recovery-vmm-to-azure/enable-replication5.png)

5. W oknie dialogowym **Właściwości** > **Konfigurowanie właściwości**, wybierz pozycję system operacyjny dla wybranego maszyny wirtualne i dysk z systemem operacyjnym. Kliknij przycisk **OK**. Dodatkowe właściwości można ustawić później.

    ![Włączanie replikacji](./media/site-recovery-vmm-to-azure/enable-replication6.png)


12. W obszarze **Ustawienia replikacji** > **Konfiguruj ustawienia replikacji**, wybierz zasady replikacji mają być stosowane do chronionego maszyny wirtualne. Kliknij przycisk **OK**. Można modyfikować zasady replikacji w **ustawieniach** > **zasady replikacji** > Nazwa zasady > **Zmień ustawienia**. Zmiany są używane dla komputerów, na których są już replikacji i nowym komputerze.

    ![Włączanie replikacji](./media/site-recovery-vmm-to-azure/enable-replication7.png)

Możesz śledzić postęp zadania **Włącz ochronę** w **ustawieniach** > **zadania** > **zadań Odzyskiwanie witryny**. Po uruchomieniu zadania **Finalizowanie ochronę** komputera jest gotowy do przełączania awaryjnego.

### <a name="view-and-manage-vm-properties"></a>Wyświetlanie i zarządzanie właściwościami maszyn wirtualnych

Zaleca się sprawdzenie właściwości komputerem źródłowym. Należy pamiętać, że nazwa maszyn wirtualnych Azure powinny być zgodne z [wymaganiami Azure maszyn wirtualnych](site-recovery-best-practices.md#azure-virtual-machine-requirements).

1. Kliknij pozycję **Ustawienia** > **Chronionych elementów** > **Replikowane elementów** > i wybierz pozycję komputer, aby wyświetlić jego szczegóły.

    ![Włączanie replikacji](./media/site-recovery-vmm-to-azure/vm-essentials.png)

2. W oknie dialogowym **Właściwości** możesz wyświetlać informacje o maszyn wirtualnych replikacji i pracy awaryjnej.

    ![Włączanie replikacji](./media/site-recovery-vmm-to-azure/test-failover2.png)

3. W **obliczeń i sieci** > **obliczyć właściwości** można określić rozmiar maszyn wirtualnych Azure nazwę i docelowej. Zmodyfikuj ją zgodnie z [wymaganiami Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements) , jeśli musisz. Można również wyświetlać i modyfikować informacji o sieci docelowej, podsieci i adres IP, który jest przypisana do maszyn wirtualnych Azure. Pamiętaj o następujących kwestiach:

    - Można ustawić docelowy adres IP. Jeśli nie podasz adres, nie powiodło się na komputerze użyje DHCP. Jeśli ustawisz adresu, który nie jest dostępne podczas pracy awaryjnej, tym przełączeniu nie powiedzie się. Ten sam adres IP docelowej może służyć do przełączania awaryjnego test, jeśli adres jest dostępna w sieci pracy awaryjnej testowego.
    - Liczba kart sieciowych zależy od rozmiaru zadanej maszyny wirtualnej docelowej:

        - Jeśli liczba kart sieciowych na komputerze źródła jest mniejsza niż lub równa argumentowi liczby kart niedozwolone dla rozmiaru komputera docelowego, docelowej będą mieć taką samą liczbę kart jako źródła.
        - Jeśli liczba kart maszyny wirtualnej źródła przekracza dozwolony rozmiar docelowej, maksymalny rozmiar docelowej zostanie użyty numer.
        - Na przykład jeśli komputer źródłowy ma dwie karty sieciowe i rozmiar komputer docelowy obsługuje cztery, komputer docelowy będzie miał dwie karty. Jeśli komputer źródłowy ma dwie karty, ale rozmiar obsługiwane docelowy obsługuje tylko jeden komputer docelowy będzie miał tylko jedna karta.     
        - Jeśli maszyn wirtualnych zawiera wiele kart sieciowych ich będzie łączyć się tej samej sieci.

    ![Włączanie replikacji](./media/site-recovery-vmm-to-azure/test-failover4.png)

5.  W oknie **dysków** można wyświetlić systemu operacyjnego i dyski danych na maszyn wirtualnych, które będą replikowane.



## <a name="step-7-test-your-deployment"></a>Krok 7: Testowanie wdrożenia

Aby przetestować rozmieszczenie można uruchamiać trybie awaryjnym test dla jednego komputera wirtualnych lub planu odzyskiwania, który zawiera co najmniej jeden maszyn wirtualnych.


### <a name="prepare-for-failover"></a>Przygotowywanie się do przełączania awaryjnego

- Do uruchamiania w trybie awaryjnym test zaleca się utworzenie nowej sieci Azure, która ma samodzielnie z sieci Azure produkcji (jest to domyślne zachowanie podczas tworzenia nowej sieci w Azure). [Aby uzyskać więcej informacji](site-recovery-failover.md#run-a-test-failover) o uruchamianiu praca awaryjna testowych.
- Aby uzyskać najlepszą wydajność, kiedy nie nad Azure, należy zainstalować agenta Azure na tym komputerze chroniony. Przekształca uruchamiania szybciej i pomaga w rozwiązywaniu problemów. Instalowanie agenta [Linux](https://github.com/Azure/WALinuxAgent) lub [systemu Windows](http://go.microsoft.com/fwlink/?LinkID=394789) .
- Aby w pełni przetestować wdrożenie należy infrastruktury zreplikowanej komputer działa zgodnie z oczekiwaniami. Jeśli chcesz przetestować usługi Active Directory i systemie DNS możesz tworzenia maszyny wirtualnej jako kontrolera domeny z usługą DNS i powielić to Azure za pomocą Odzyskiwanie witryny Azure. Przeczytaj więcej o [test pracy awaryjnej zagadnienia związane z usługą Active Directory](site-recovery-active-directory.md#considerations-for-test-failover).
- Jeśli chcesz uruchomić nieplanowanego przełączania awaryjnego zamiast trybie awaryjnym test Pamiętaj o następujących kwestiach:

    - Jeśli to możliwe należy wyłączać podstawowego maszyn przed uruchomieniem nieplanowanego przełączania awaryjnego. Dzięki temu, że nie masz źródłowa i replice komputerów z systemem w tym samym czasie.
    - Po uruchomieniu nieplanowanego przełączania awaryjnego przestaje replikacji danych z podstawowego komputerów dzięki różnicy wszelkie dane nie będą przekazywane po rozpoczęciu nieplanowanego przełączania awaryjnego. Ponadto w przypadku wystąpienia nieplanowanego przełączania awaryjnego plan odzyskiwania będzie działać aż do wykonania, nawet jeśli wystąpi błąd.

### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Przygotowywanie nawiązywania połączenia z maszyny wirtualne Azure po przełączeniu

Jeśli chcesz nawiązać połączenie z maszyny wirtualne Azure za pomocą RDP po przełączeniu, upewnij się, możesz wykonać następujące czynności:

**Na komputerze lokalnym przed przejęciem awaryjnym**:

- Dostęp za pośrednictwem Internetu Włącz RDP, upewnij się, że TCP i UDP reguły są dodawane do **publicznej**oraz upewnij się, że RDP jest dozwolona w **Zapora systemu Windows** -> **dozwolone aplikacje i funkcje** dla wszystkich profilów.
- Dostęp za pośrednictwem połączenia witryny do witryny Włącz RDP na komputerze i upewnij się, że RDP jest dozwolona w **Zapora systemu Windows** -> **dozwolone aplikacje i funkcje** sieci **domeny** i **prywatne** .
- [Agent maszyn wirtualnych Azure](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) zainstalować na komputerze lokalnym.
- Upewnij się, że system operacyjny SAN zasady są ustawione na OnlineAll. [Dowiedz się więcej]( https://support.microsoft.com/kb/3031135)
- Wyłącz usługi IPSec przed uruchomieniem tym przełączeniu.

**Na Azure maszyn wirtualnych po przełączeniu**:

- Dodaj publicznej punkt końcowy dla protokołu RDP (port 3389) i określ poświadczenia logowania.
- Upewnij się, że nie ma żadnych zasad domeny, uniemożliwiające nawiązywanie połączenia przy użyciu publicznego adresu maszyny wirtualnej.
- Spróbuj połączyć. Jeśli nie możesz połączyć, upewnij się, że działa maszyn wirtualnych. Aby uzyskać więcej porad dotyczących rozwiązywania problemów przeczytaj ten [artykuł](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

Jeśli chcesz uzyskać dostęp do maszyn wirtualnych Azure systemem Linux po przełączeniu przy użyciu powłoki bezpiecznego klienta (ssh), wykonaj następujące czynności:

**Na komputerze lokalnym przed przejęciem awaryjnym**:

- Upewnij się, że usługa bezpiecznego powłoki na maszyn wirtualnych Azure jest ustawiona na automatyczne uruchamianie na uruchamiania systemu.
- Upewnij się, że reguły zapory zezwalać na połączenia SSH.

**Na Azure maszyn wirtualnych po przełączeniu**:

- Muszą zasad grupy zabezpieczeń sieci na nie powiodło się nad maszyn wirtualnych i Azure podsieci, do którego jest podłączony do zezwalania na połączenia przychodzące do portu SSH.
- Czy można utworzyć publicznej punktu końcowego do zezwalania na połączenia przychodzące SSH port (port TCP 22 domyślnie).
- Jeśli maszyn wirtualnych jest dostępna za pośrednictwem połączenia VPN (trasy Express lub witryny sieci VPN) klienta można używać bezpośrednio z maszyn wirtualnych przez SSH.


### <a name="run-a-test-failover"></a>Uruchom w trybie awaryjnym test

Aby uruchomić test pracy awaryjnej następujące czynności:

1. Niepowodzenie w jednym maszyn wirtualnych w **ustawieniach** > **Replikowane elementów**, kliknij pozycję maszyn wirtualnych > **+ pracy awaryjnej Test**.
2. Niepowodzenie nad planu odzyskiwania w **ustawieniach** > **Odzyskiwania plany**, kliknij prawym przyciskiem myszy odpowiedni plan > **Testowanie pracy awaryjnej**. Aby utworzyć odzyskiwania planowanie, [wykonaj następujące instrukcje](site-recovery-create-recovery-plans.md).

3. W **Pracy awaryjnej testowych** wybierz Azure sieci, do której maszyny wirtualne Azure podłączyć po awaria wystąpiła.
4. Kliknij **przycisk OK** , aby rozpocząć tym przełączeniu. Możesz śledzić postęp, klikając pozycję maszyn wirtualnych, aby otworzyć jego właściwości lub stanowiska **Pracy awaryjnej Test** w **ustawieniach** > **zadań Odzyskiwanie witryny**.
5. Po tym przełączeniu osiągnie fazy **badania wykonane** , wykonaj następujące czynności:

    1. Wyświetlanie maszyny wirtualnej replice Azure portal. Upewnij się, że maszyna wirtualna zostanie pomyślnie uruchomiona.
    2. Jeśli do maszyn wirtualnych programu access z sieci lokalnej może inicjować połączenie pulpitu zdalnego maszyn wirtualnych.
    3. Kliknij przycisk **Zakończ test** do pracy nad nią.
    4. Kliknij **Notes** , aby nagrać, a wszelkie uwagi skojarzone z tym przełączeniu test.
    5. Kliknij pozycję **tym przełączeniu test zostało zakończone**. Oczyść środowisku testowym automatycznego Wyłącz i Usuń maszyny wirtualnej test.
    6. Na tym etapie są usuwane wszystkie elementy lub maszyny wirtualne utworzone automatycznie przez Odzyskiwanie witryny w tym przełączeniu test. Dodatkowych elementów utworzonych przez siebie do przełączania awaryjnego test nie są usuwane.

    > [AZURE.NOTE] Jeśli w trybie awaryjnym test trwa dłużej niż dwa tygodnie jest uzupełniona życie.

6. Po zakończeniu tym przełączeniu należy także widoczne w replice Azure komputera są wyświetlane w portalu Azure > **maszyn wirtualnych**. Należy upewnić się, że maszyn wirtualnych jest odpowiedni rozmiar, który jest połączony z siecią właściwe i że działa.
7. Jeśli zostanie [przygotowany połączeń po przełączeniu](#prepare-to-connect-to-Azure-VMs-after-failover) można nawiązać maszyn wirtualnych Azure.


## <a name="monitor-your-deployment"></a>Monitorowanie wdrożenia

Poniżej opisano, jak można monitorować ustawienia konfiguracji, status i kondycji rozmieszczania Odzyskiwanie witryny:

1. Kliknij nazwę magazynu, aby uzyskać dostęp do pulpitu nawigacyjnego **Essentials** . W tym pulpitu nawigacyjnego można Odzyskiwanie witryny zadania, stan replikacji plany naprawcze, Kondycja serwera i zdarzeń.  Możesz dostosować Essentials umożliwia wyświetlanie kafelków i układów, które są najbardziej użyteczne, tym stanu innych magazynów Odzyskiwanie witryny i kopia zapasowa.

    ![Podstawowe informacje dotyczące](./media/site-recovery-vmm-to-azure/essentials.png)

2. Na kafelku **kondycji** można monitorować serwerów witryny (VMM lub konfiguracji serwera), w których występuje problem i zdarzenia powstałe Odzyskiwanie witryny w ciągu ostatnich 24 godzin.
3. Możesz zarządzać i monitorować replikacji **Replikowane elementów**, **Plany odzyskiwania**, i Kafelki **Zadań Odzyskiwanie witryny** . Można przechodzić do zadań w **ustawieniach** -> **zadania** -> **Zadań Odzyskiwanie witryny**.


## <a name="next-steps"></a>Następne kroki

Po wdrożenia jest skonfigurowany i działa, [Dowiedz się więcej](site-recovery-failover.md) na temat różnych typów pracy awaryjnej.
