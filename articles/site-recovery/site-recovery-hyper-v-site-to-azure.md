<properties
    pageTitle="Powielić maszyn wirtualnych funkcji Hyper-V (bez VMM) Azure Odzyskiwanie witryny Azure za pomocą portalu Azure | Microsoft Azure"
    description="W tym artykule opisano jak wdrożyć serwer Azure Odzyskiwanie witryny, aby dodać akompaniament replikacji, pracy awaryjnej i odzyskiwania maszyny wirtualne funkcji Hyper-V lokalnego, które nie są zarządzane przez VMM Azure za pomocą portalu Azure"
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
    ms.workload="storage-backup-recovery"
    ms.date="09/19/2016"
    ms.author="raynew"/>


# <a name="replicate-hyper-v-virtual-machines-without-vmm-to-azure-using-azure-site-recovery-with-the-azure-portal"></a>Powielić maszyn wirtualnych funkcji Hyper-V (bez VMM) Azure Odzyskiwanie witryny Azure za pomocą portalu Azure

> [AZURE.SELECTOR]
- [Azure Portal](site-recovery-hyper-v-site-to-azure.md)
- [Klasyczny Azure](site-recovery-hyper-v-site-to-azure-classic.md)
- [PowerShell — Menedżera zasobów](site-recovery-deploy-with-powershell-resource-manager.md)



Odzyskiwanie Azure witryny — Zapraszamy! Użyj tego artykułu, jeśli dane mają być replikowane lokalnego funkcji Hyper-V wirtualnej maszyny tej **nie są** zarządzane w chmury System Centrum maszyn wirtualnych Manager (VMM) Azure. W tym artykule opisano sposób konfigurowania replikacji przy użyciu Odzyskiwanie witryny Azure w portalu Azure.

> [AZURE.NOTE] Azure ma dwa różne [modeli wdrażania](../resource-manager-deployment-model.md) służące do tworzenia i pracy z zasobami: Menedżer zasobów Azure i klasyczny. Azure też ma dwa portali — portalu klasyczny Azure obsługuje model klasyczny wdrożenia i portal Azure z obsługą obu modeli wdrożenia.

> Odzyskiwanie witryny Azure obsługuje odzyskiwania i migracji maszyn wirtualnych funkcji Hyper-V Azure. Kroki opisane w tym artykule identyczne stosowanie podczas konfigurowania replikacji Azure awarii lub migrowania maszyny wirtualne Azure

Azure Odzyskiwanie witryny w portalu Azure oferuje wiele nowych funkcji:

- Platformy Azure portalu, kopia zapasowa Azure i usługi Azure witryny odzyskiwania są łączone w jednym magazynu usługi odzyskiwania tak, aby skonfigurować i zarządzanie ciągłości i odzyskiwanie (BCDR) z jednej lokalizacji. Ujednolicony pulpitu nawigacyjnego umożliwia monitorowanie i zarządzanie operacji za pośrednictwem witryny lokalnej i Azure chmura publicznej.
- Użytkownicy z subskrypcją Azure obsługi administracyjnej z programem chmury rozwiązanie dostawcy (dostawcy) mogą teraz zarządzać operacji odzyskiwania witryny w portalu Azure.
- Odzyskiwanie witryny w portalu Azure można replikować maszyn z kontami miejsca do magazynowania Menedżera zasobów. Po awaryjnym przeniesieniu Odzyskiwanie witryny tworzony oparte na Menedżera zasobów maszyny wirtualne platformy Azure.
- Odzyskiwanie witryny nadal obsługuje replikacji kontach klasyczny miejsca do magazynowania i awaryjnego przeniesienia maszyny wirtualne przy użyciu modelu Klasyczny wdrożenia.


Po przeczytaniu ten wpis w artykule swoją opinię u dołu w sekcji komentarze Disqus. Zadawaj pytania techniczne na [Forum usługi odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="overview"></a>Omówienie


Organizacje muszą strategii BCDR, która określa sposób aplikacje, obciążenia i danych utrzymywania działa i jest dostępny podczas zaplanowane i niezaplanowane przestoje i odzyskiwanie do normalnych warunkach pracy tak szybko, jak to możliwe. Strategii BCDR zachować dane biznesowe, bezpiecznych i odzyskania i zapewnienia obciążenia przez cały czas dostępne po wystąpieniu awarii.

Odzyskiwanie witryny jest usługą Azure, która pozwala strategii BCDR przez orchestrating replikacji lokalnych serwerów fizycznych i maszyn wirtualnych w chmurze (Azure) lub pomocnicza centrum danych. W przypadku wystąpienia awarii w lokalizacji podstawowego po wymuszeniu przejęcia do lokalizacji pomocniczej, aby aplikacje i obciążenia dostępne. Powrót do swojej lokalizacji podstawowej może nie po powraca do normalnego działania. Dowiedz się więcej na [Co to jest Odzyskiwanie witryny Azure?](site-recovery-overview.md)

Ten artykuł zawiera wszystkie informacje, które musisz powielić lokalnego funkcji Hyper-V wirtualnej maszyny tej **nie są** zarządzane w chmury System Centrum maszyn wirtualnych Manager (VMM) Azure. Zawiera omówienie architektury informacji i wdrażania instrukcje dotyczące konfigurowania lokalne serwery, Azure zasad replikacji i planowaniu planowania. Po skonfigurowaniu infrastruktury można włączyć replikacji na komputerach, które mają być chronione i wykonywać trybie awaryjnym test, aby sprawdzić poprawność konfiguracji. Można również Migrowanie usługi maszyn wirtualnych Azure, wykonując pierwszego planowane trybie awaryjnym, a następnie wypełnij migracji.

## <a name="business-advantages"></a>Zalety firm

- Poza (Azure) pracy awaryjnej przewiduje biznesowych i aplikacje na maszyn wirtualnych funkcji Hyper-V.
- Udostępnia pojedynczej konsoli usługi odzyskiwania ustawień prostego i dotyczących zarządzania procesami replikacji, pracy awaryjnej i odzyskiwania.
- Pozwala na łatwe Uruchom praca awaryjna z infrastruktury lokalnego Azure i błędów zwrotne (Przywróć) od Azure do lokalnej witryny.
- Plany odzyskiwania można konfigurować z wieloma komputerami, tak, aby warstwowych aplikacji niepowodzenie nad razem.

## <a name="scenario-architecture"></a>Architektura scenariusz

Są to składniki scenariusza:

- **Host funkcji Hyper-V lub klaster**: Serwery hosta lokalnego funkcji Hyper-V lub grup. Uruchamianie maszyn wirtualnych chcesz chronić hostów funkcji Hyper-V są zbierane na logiczne witryny funkcji Hyper-V podczas wdrażania Odzyskiwanie witryny.
- **Dostawca odzyskiwania witryny Azure i agenta usług odzyskiwania**: podczas wdrażania zainstalowaniu dostawca odzyskiwania witryny Azure i agenta firmy Microsoft Azure odzyskiwania usług na serwerach hosta funkcji Hyper-V. Dostawca komunikuje się z Odzyskiwanie witryny Azure przez 443 HTTPS do replikacji aranżacji. Agent na serwerze hosta funkcji Hyper-V replikuje dane do magazynu Azure nad HTTPS 443 domyślnie.
- **Azure**: potrzebna subskrypcji usługi Azure, konto Azure miejsca do magazynowania danych sklepu replikowane oraz Azure wirtualną sieć tak, aby maszyny wirtualne Azure są połączone z siecią po przełączeniu.

![Architektura witryny funkcji Hyper-V](./media/site-recovery-hyper-v-site-to-azure/architecture.png)



## <a name="azure-prerequisites"></a>Wymagania wstępne dotyczące Azure

Oto, co będzie potrzebne platformy Azure wdrożenia tego scenariusza.

**Wymagania wstępne** | **Szczegóły**
--- | ---
**Konto Azure**| Musisz mieć konto [Microsoft Azure](http://azure.microsoft.com/) . Możesz rozpocząć z [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/). [Aby uzyskać więcej informacji](https://azure.microsoft.com/pricing/details/site-recovery/) na temat ceny Odzyskiwanie witryny.
**Azure miejsca do magazynowania** | Musisz mieć konto standardowego magazynu. Za pomocą konta miejsca do magazynowania LRS lub GRS. Zalecamy GRS tak, aby dane mechanizm awaria regionalne, lub jeśli nie można odzyskać obszaru podstawowego. Aby [uzyskać więcej informacji](../storage/storage-redundancy.md). Konto musi być w tym samym regionie jako magazynu usługi odzyskiwania.<br/><br/> Magazyn Premium nie jest obsługiwana.<br/><br/> Replikowane dane są przechowywane w magazynie Azure i maszyny wirtualne Azure są tworzone, gdy wystąpi awaryjne przeniesienie.<br/><br/> [Przeczytaj o](../storage/storage-introduction.md) Miejsce do magazynowania Azure.
**Sieć Azure** | Konieczne będzie Azure wirtualnej sieci, który maszyny wirtualne Azure będzie łączyć się, gdy wystąpi awaryjne przeniesienie. Azure wirtualną sieć musi być w tym samym regionie jako magazynu usługi odzyskiwania.

## <a name="on-premises-prerequisites"></a>Wymagania wstępne dotyczące lokalnego

Oto, czego potrzebujesz lokalnej.

**Wymagania wstępne** | **Szczegóły**
--- | ---
**Funkcji Hyper-V** | Co najmniej jeden lokalne serwery z systemem **Windows Server 2012 R2** z najnowszych aktualizacji i roli Hyper-V włączony lub **Microsoft Hyper-V Server 2012 R2**.<br/><br/>Serwer funkcji Hyper-V powinien zawierać co najmniej jeden maszyn wirtualnych.<br/><br/>Serwery funkcji Hyper-V powinny być połączone z Internetem, bezpośrednio lub za pośrednictwem serwera proxy.<br/><br/>Serwery funkcji Hyper-V powinny mieć poprawki wymienione w [KB2961977](https://support.microsoft.com/en-us/kb/2961977 "KB2961977") zainstalowany.
**Dostawca i agenta** | Podczas rozmieszczania Odzyskiwanie witryny Azure będzie instalowanie dostawcy odzyskiwania witryny Azure. Instalacja dostawcy zainstalować także agenta usługi Azure na poszczególnych funkcji Hyper-V serwerem maszyn wirtualnych, którą chcesz chronić. Wszystkie serwery funkcji Hyper-V w magazynu Odzyskiwanie witryny powinny mieć tej samej wersji dostawcy i agenta.<br/><br/>Dostawca należy nawiązać Odzyskiwanie witryny Azure przez Internet. Ruch mogą zostać wysłane bezpośrednio lub za pośrednictwem serwera proxy. Należy zauważyć, że serwera proxy HTTPS podstawie nie jest obsługiwana. Serwer proxy należy zezwolić na dostęp do: <br/><br/> *. hypervrecoverymanager.windowsazure.com <br/> <br/> *. accesscontrol.windows.net <br/><br/> *. backup.windowsazure.com <br/> <br/> *. blog.core.windows.net <br/><br/> *store.Core.Windows.NET <br/><br/> https://www.msftncsi.com/ncsi.txt<br/><br/>Jeśli masz reguły zapory oparte na adresie IP na serwerze, sprawdź zasady zezwalają na komunikację Azure. Musisz zezwolić [Zakresów adresów IP centrum danych Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) oraz portu HTTPS (443).<br/><br/>Zezwalaj na zakresy adresów IP dla Azure region subskrypcji i zachód USA.

## <a name="protected-machine-prerequisites"></a>Wymagania wstępne dotyczące chronionego komputera


**Wymagania wstępne** | **Szczegóły**
--- | ---
**Chroniony maszyny wirtualne** | Zanim po wymuszeniu przejęcia maszyny musisz upewnij się, że nazwę, która zostanie przypisany do maszyn wirtualnych Azure spełnia [wymagania wstępne dotyczące Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements). Po włączeniu replikacji maszyn wirtualnych, możesz zmienić nazwę.<br/><br/> Poszczególne pojemność dysku na komputerach chronionej nie powinny być więcej niż 1023 GB. Maszyny może zawierać maksymalnie 16 dysków (a więc do 16 TB).<br/><br/> Udostępniane gościa dysku, które nie są obsługiwane klastrów.<br/><br/> Jeśli źródło maszyn wirtualnych ma NIC zespołu jest konwertowany do pojedynczej NIC po przełączeniu Azure.<br/><br/>Ochrona maszyny wirtualne systemem Linux przy użyciu statycznego adresu IP nie jest obsługiwana.

## <a name="prepare-for-deployment"></a>Przygotowywanie do wdrożenia

Aby przygotować się do wdrożenia, które będą potrzebne do:

1. [Konfigurowanie Azure sieci](#set-up-an-azure-network) , w której maszyny wirtualne Azure zostaną umieszczone po są generowane po przełączeniu.
2. [Konfigurowanie konta usługi Azure miejsca do magazynowania](#set-up-an-azure-storage-account) dla zreplikowanej danych.
3. [Hosts Przygotuj funkcji Hyper-V](#prepare-the-hyper-v-hosts) , aby upewnić się, że będą uzyskiwać dostęp do odpowiednie adresy URL.

### <a name="set-up-an-azure-network"></a>Skonfiguruj sieć Azure

Skonfiguruj sieć Azure. Musisz to, tak aby maszyny wirtualne Azure utworzonych po pracy awaryjnej są połączone z siecią.

- Sieć powinna być w tym samym regionie, w którym będzie Wdrażanie magazynu usługi odzyskiwania.
- W zależności od modelu zasobu, który ma być używany dla nie powiodło się nad maszyny wirtualne Azure można konfigurować Azure sieci w [trybie Menedżer zasobów](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) lub w [trybie klasycznym](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).
- Zaleca się, że możesz skonfigurować sieć przed rozpoczęciem. Jeśli nie możesz musisz to zrobić podczas wdrażania Odzyskiwanie witryny.

> [AZURE.NOTE] [Migracja sieci](../resource-group-move-resources.md) wzdłuż grup zasobów w obrębie tej samej subskrypcji lub w subskrypcjach nie jest obsługiwane dla sieci używane do wdrażania Odzyskiwanie witryny.

### <a name="set-up-an-azure-storage-account"></a>Konfigurowanie konta usługi Azure miejsca do magazynowania

- Musisz mieć konto standardowego Azure magazynu do przechowywania danych replikować do Azure.
- W zależności od modelu zasobu, który ma być używany dla nie powiodło się nad maszyny wirtualne Azure skonfigurujesz konta w [trybie Menedżer zasobów](../storage/storage-create-storage-account.md) lub w [trybie klasycznym](../storage/storage-create-storage-account-classic-portal.md).
- Zaleca się Konfigurowanie konta magazynu przed rozpoczęciem. Jeśli nie możesz musisz to zrobić podczas wdrażania Odzyskiwanie witryny. Konta muszą być w tym samym regionie jako magazynu usługi odzyskiwania.

> [AZURE.NOTE] [Migracja kont miejsca do magazynowania](../resource-group-move-resources.md) w grup zasobów w obrębie tej samej subskrypcji lub w subskrypcjach nie jest obsługiwane w przypadku kont miejsca do magazynowania na potrzeby wdrażania Odzyskiwanie witryny.

### <a name="prepare-the-hyper-v-hosts"></a>Przygotowywanie hosts funkcji Hyper-V

- Upewnij się, że hosts funkcji Hyper-V spełniają [wymagania wstępne](#on-premises-prerequisites).

### <a name="create-a-recovery-services-vault"></a>Tworzenie magazynu usługi odzyskiwania

1. Zaloguj się do usługi [Azure portal](https://portal.azure.com).
2. Kliknij przycisk **Nowy** > **zarządzania** > **Wykonywanie kopii zapasowych i odzyskiwanie witryny (usługi OMS)**. Alternatywnie kliknij **Przeglądaj** > **Usługi odzyskiwania** magazynów > **Dodaj**.

    ![Nowe magazynu](./media/site-recovery-hyper-v-site-to-azure/new-vault3.png)

3. W polu **Nazwa** Określ przyjazną nazwę identyfikującą magazyn. Jeśli masz więcej niż jedną subskrypcję, wybierz jeden z nich.
4. [Tworzenie nowej grupy zasobów](../resource-group-template-deploy-portal.md) lub wybierz istniejący i określ Azure region. Komputery będzie można replikować do tego obszaru. Aby sprawdzić obsługiwanych regionów, zobacz dostępność geograficzne w [Azure Szczegóły ceny odzyskiwania witryny](https://azure.microsoft.com/pricing/details/site-recovery/)
4. Jeśli chcesz szybko uzyskać magazyn na pulpicie nawigacyjnym kliknij pozycję **Przypnij do pulpitu nawigacyjnego** , a następnie kliknij pozycję **Utwórz magazynu**.

    ![Nowe magazynu](./media/site-recovery-hyper-v-site-to-azure/new-vault-settings.png)

Nowy magazyn pojawią się na **pulpicie nawigacyjnym** > **wszystkie zasoby**i na głównym karta **magazynów usługi odzyskiwania** .

## <a name="getting-started"></a>Wprowadzenie

Odzyskiwanie witryny zawiera wprowadzenie do środowiska, który pomoże Ci wdrażanie tak szybko, jak to możliwe. Wprowadzenie do sprawdza wymagania wstępne i przeprowadzi Cię przez Odzyskiwanie witryny wdrożenia kroki w odpowiedniej kolejności.

Wprowadzenie, wybierz typ maszyn, który chcesz odtworzyć, a miejsce, w którym mają być replikowane w. Możesz skonfigurować lokalne serwery, konta magazynu platformy Azure i sieci. Tworzenie zasad replikacji i zaplanowanie wydajności. Po skonfigurowaniu infrastruktury umożliwia replikacji maszyny wirtualne. Można uruchomić praca awaryjna na określonych komputerach lub tworzenie planów odzyskiwania niepowodzenie na wielu komputerach.

Rozpocznij wprowadzenie, wybierając pozycję jak chcesz wdrożyć Odzyskiwanie witryny. Wprowadzenie do przepływów nieco zmienia się w zależności od potrzeb replikacji.



## <a name="step-1-choose-your-protection-goals"></a>Krok 1: Wybierz cele ochrony

Zaznacz dane mają być replikowane i miejsce, w którym mają być replikowane w.

1. W karta **magazynów usługi odzyskiwania** wybierz z magazynu, a następnie kliknij przycisk **Ustawienia**.
2. W obszarze **Ustawienia** > **Wprowadzenie** kliknij pozycję **Odzyskiwanie witryny** > **Krok 1: Przygotowywanie infrastruktury** > **cel ochrony**.

    ![Wybierz cele](./media/site-recovery-hyper-v-site-to-azure/choose-goals.png)

3. W **celu ochrony** zaznacz **Azure do**, a następnie wybierz pozycję **Tak, przy użyciu funkcji Hyper-V**. Wybierz pozycję **nie** , aby potwierdzić, że nie używasz VMM. Kliknij przycisk **OK**.

    ![Wybierz cele](./media/site-recovery-hyper-v-site-to-azure/choose-goals2.png)


## <a name="step-2-set-up-the-source-environment"></a>Krok 2: Konfigurowanie środowiska źródła

Konfigurowanie witryny funkcji Hyper-V, instalowanie dostawcy odzyskiwania witryny Azure i agenta usługi Azure odzyskiwania na funkcji Hyper-V hosts i zarejestrować hostów w magazyn.


1. Kliknij pozycję **Krok 2: Przygotowywanie infrastruktury** > **źródła**. Aby dodać nową witrynę funkcji Hyper-V jako kontenera dla funkcji Hyper-V hosts lub klastrów, kliknij przycisk **+ Witryna funkcji Hyper-V**.

    ![Ustawianie źródła](./media/site-recovery-hyper-v-site-to-azure/set-source1.png)

2. W **witrynie, tworzenie funkcji Hyper-V** karta Podaj nazwę dla witryny. Kliknij przycisk **OK**. Wybierz witrynę, która została właśnie utworzona.

    ![Ustawianie źródła](./media/site-recovery-hyper-v-site-to-azure/set-source2.png)

3. Kliknij przycisk **+ Hyper-V Server** Aby dodać serwer do witryny.
4. W polu **Dodaj serwer** > **Typ serwera** Sprawdź, czy **serwer funkcji Hyper-V** jest wyświetlane. Upewnij się, że serwer funkcji Hyper-V, z którym chcesz dodać spełnia [wymagania wstępne](#on-premises-prerequisites) i możliwość dostępu określone adresy URL.
4. Pobierz plik instalacyjny Azure witryny odzyskiwania dostawcy. Będzie uruchomić ten plik, aby zainstalować zarówno dostawcę, jak i agenta usługi odzyskiwania na każdym hoście funkcji Hyper-V.
5. Pobierz klucz rejestru. Musisz to, gdy jest uruchomiony Instalator. Klucz jest prawidłowy przez 5 dni po jego wygenerować.

    ![Ustawianie źródła](./media/site-recovery-hyper-v-site-to-azure/set-source3.png)

6. Uruchom dostawcę plik Instalatora na każdym hoście dodanych do witryny funkcji Hyper-V. Jeśli przeprowadzasz instalację w klastrze funkcji Hyper-V, uruchom Instalatora na każdym węźle klaster. Instalowanie i rejestrowanie poszczególnych funkcji Hyper-V węzłach zapewnia maszyn wirtualnych pozostaną chronione, nawet jeśli migrowania w węzłach.

### <a name="install-the-provider-and-agent"></a>Instalowanie dostawcy i agenta

1. Uruchom plik Instalatora dostawcy.
2. W **Witrynie Microsoft Update** można jednak aktualizacje tak, aby zainstalowanych aktualizacji dostawcy zgodnie z zasadami usługi Microsoft Update.
3. W **instalacji** zaakceptować lub modyfikowanie domyślnej lokalizacji instalacji dostawcy, a następnie kliknij przycisk **Zainstaluj**.
4. Na stronie **Ustawienia magazynu** kliknij przycisk **Przeglądaj** , aby wybrać pobranego pliku klucza magazynu. Określ subskrypcji Odzyskiwanie witryny Azure, nazwę magazynu i witryny funkcji Hyper-V, do której należy serwer funkcji Hyper-V.

    ![Rejestracja serwera](./media/site-recovery-hyper-v-site-to-azure/provider3.png)

5 w oknie **Ustawienia serwera Proxy** Określ, jak dostawcy, zainstalowanym na serwerze połączy się Odzyskiwanie witryny Azure przez internet.

- Jeśli chcesz, aby z dostawcą, aby połączyć bezpośrednio zaznacz pole wyboru **Połącz bezpośrednio bez serwera proxy**.
- Jeśli chcesz nawiązać połączenie z serwerem proxy, która jest obecnie skonfigurowana na serwerze zaznacz pole wyboru **Połącz z istniejących ustawień serwera proxy**.
- Serwer proxy istniejących wymaga uwierzytelniania, czy chcesz używać niestandardowego serwera proxy dla dostawcy wybierz połączenie **Nawiązywanie połączenia przy użyciu ustawień niestandardowych serwera proxy**.
- Jeśli korzystasz z serwera proxy niestandardowy, musisz określić adres, port i poświadczenia
- Jeśli korzystasz z serwera proxy tworzącej się, że adresy URL opisanego w [wymagania wstępne](#on-premises-prerequisites) są dozwolone przez niego.

    ![Internet](./media/site-recovery-hyper-v-site-to-azure/provider7.PNG)

6 po zakończeniu instalacji kliknij przycisk **Zarejestruj** , aby zarejestrować serwer w magazyn.

![Lokalizacja instalacji](./media/site-recovery-hyper-v-site-to-azure/provider2.png)

7 po rejestracji metadanych z funkcji Hyper-V server są pobierane przez Odzyskiwanie witryny Azure i serwer nie jest wyświetlany na stronie **Ustawienia** > **Infrastrukturą odzyskiwania witryny** > karta**Hosts funkcji Hyper-V** .


### <a name="command-line-installation"></a>Instalacja wiersza polecenia

Dostawca odzyskiwania witryny Azure i agenta można również zainstalować przy użyciu następującego polecenia. Ta metoda umożliwia instalowanie dostawcy na serwerze Core dla systemu Windows Server 2012 R2.

1. Pobierz klucz pliku i rejestracji instalacji dostawcy do folderu. Na przykład C:\ASR.
2. W wierszu polecenia z podwyższonym poziomem uprawnień Uruchom te polecenia, aby wyodrębnić Instalatora dostawcy:

            C:\Windows\System32> CD C:\ASR
            C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Uruchom to polecenie, aby zainstalować składniki:

            C:\ASR> setupdr.exe /i

4. Następnie uruchom te polecenia, aby zarejestrować serwer w Magazyn: CD C:\Program Files\Microsoft Azure witryny odzyskiwania Provider\
            C:\Program Files\Microsoft Azure witryny odzyskiwania dostawcy\> DRConfigurator.exe /r /Friendlyname <friendly name of the server> /poświadczeń<path of the credentials file>

<br/>
W przypadku gdy:

- **/Credentials** : obowiązkowe parametr, który określa lokalizację, w której znajduje się plik klucza rejestru  
- **/FriendlyName** : parametr obowiązkowe dla nazwy serwera hosta funkcji Hyper-V, który pojawia się w portalu Odzyskiwanie witryny Azure.
- **/proxyAddress** : parametr opcjonalny, który określa adres serwera proxy.
- **/proxyport** : parametr opcjonalny, który określa port serwera proxy.
- **/proxyUsername** : parametr opcjonalny określający nazwę użytkownika serwera Proxy (Jeśli serwer proxy wymaga uwierzytelniania).
- **/proxyPassword** : parametr opcjonalny, który określa hasło uwierzytelniania na serwerze proxy (Jeśli serwer proxy wymaga uwierzytelniania).


## <a name="step-3-set-up-the-target-environment"></a>Krok 3: Konfigurowanie środowiska docelowej

Określanie konta magazynu platformy Azure ma być używany dla replikacji i sieci Azure, do którego maszyny wirtualne Azure połączy się po przełączeniu.

1.  Kliknij pozycję **Przygotuj infrastruktury** > **docelowej** i wybierz subskrypcję Azure, którego chcesz użyć.
2.  Określ model wdrożenia, który ma być używany dla maszyny wirtualne po przełączeniu.
3.  Odzyskiwanie witryny sprawdza, czy masz kont zgodne Azure miejsca do magazynowania lub sieci.

    ![Miejsca do magazynowania](./media/site-recovery-hyper-v-site-to-azure/select-target.png)

4.  Jeśli nie utworzono konto miejsca do magazynowania i chcesz je utworzyć za pomocą Menedżera zasobów kliknij konto **+ miejsce** w tym tekście. Na karta **Tworzenie miejsca do magazynowania konta** Określ nazwę konta, typ, subskrypcji i lokalizacji. Konto powinno być w tym samym miejscu jako magazynu usługi odzyskiwania.

    ![Miejsca do magazynowania](./media/site-recovery-hyper-v-site-to-azure/gs-createstorage.png)

    Jeśli chcesz utworzyć konto miejsca do magazynowania przy użyciu modelu Klasyczny, wykonaj tę [w portalu Azure](../storage/storage-create-storage-account-classic-portal.md).

5.  Jeśli nie utworzono Azure sieci i chcesz je utworzyć za pomocą Menedżera zasobów kliknij **+ sieć** w tym tekście. Na karta **Tworzenie wirtualnych sieci** Określ nazwę sieciową, zakres adresów, szczegóły podsieci, subskrypcji i lokalizacji. Sieć powinna być w tym samym miejscu jako magazynu usługi odzyskiwania.

    ![Sieci](./media/site-recovery-hyper-v-site-to-azure/gs-createnetwork.png)

    Jeśli chcesz utworzyć sieć przy użyciu modelu Klasyczny, wykonaj tę [w portalu Azure](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).


## <a name="step-4-set-up-replication-settings"></a>Krok 4: Konfigurowanie ustawień replikacji

1. Aby utworzyć nową replikacją zasad kliknij pozycję **Przygotuj infrastruktury** > **Ustawień replikacji** > **+ Utwórz i skojarz**.

    ![Sieci](./media/site-recovery-hyper-v-site-to-azure/gs-replication.png)

2. **Tworzenie** i kojarzenie zasad określ nazwę zasady.
3. W polu **Kopiuj częstotliwość** Określ, jak często chcesz replikować różnicy danych po początkowej replikacji (co 30 sekund, 5 lub 15 minut).
4. W **odzyskiwania punktu przechowywania**określ w godzinach czasu okno przechowywania będzie dla każdego punktu odzyskiwania. Chroniony maszyn może zostać przywrócona do dowolnego punktu w oknie.
6. W **aplikacji spójne częstotliwość migawkę** określ częstotliwość (1 – 12 godzin) punktów odzyskiwania zawierające migawek spójnych aplikacji będzie utworzona. Funkcji Hyper-V używa dwóch typów migawek — standardowy migawkę zawierającego przyrostowe migawki całego maszyny wirtualnej i migawkę spójną z aplikacją, która pobiera migawkę w chwili dane aplikacji wewnątrz maszyny wirtualnej. Migawek spójnych aplikacji upewnij się, że aplikacje są spójna przy podejmowaniu migawki za pomocą usługi kopiowania woluminu w tle (VSS). Należy zauważyć, że po włączeniu migawek spójnych aplikacji wpłynie to na wydajność aplikacji uruchomionych maszyn wirtualnych źródła. Upewnij się, że ustawioną wartość jest mniejsza niż liczba punktów odzyskiwania dodatkowe, które można skonfigurować.
3. W **godzinę rozpoczęcia replikacji początkowy** Określ momentu rozpoczęcia replikacji początkowej. Replikacja odbywa się za pośrednictwem usługi przepustowości internetowej, warto zaplanować go poza zajęty godziny. Kliknij przycisk **OK**.

    ![Zasady replikacji](./media/site-recovery-hyper-v-site-to-azure/gs-replication2.png)

Podczas tworzenia nowych zasad automatycznie jest skojarzony z witryną funkcji Hyper-V. Kliknij **przycisk OK**. Możesz skojarzyć z wielu zasad replikacji w **ustawieniach**witryny funkcji Hyper-V (i maszyny wirtualne w nim) > **replikacji** > Nazwa zasady > **Kojarzenie witryny funkcji Hyper-V**.

## <a name="step-5-capacity-planning"></a>Krok 5: Planowanie wydajności

Teraz, gdy masz podstawowe usługi infrastruktury możesz skonfigurować możesz myśleć o planowaniu i wiadomo, czy potrzebujesz dodatkowych zasobów.

Odzyskiwanie witryny zawiera planowania pojemności ułatwiające przydzielanie zasobów do środowiska źródła, składniki odzyskiwania witryny, sieci i przechowywania. Terminarz może zostać uruchomiony w trybie szybkim dla ocen na podstawie średniej liczby maszyny wirtualne, dyski i miejsca do magazynowania lub w trybie szczegółowym, w której będzie wprowadzania dane na poziomie obciążenie pracą. Przed rozpoczęciem musisz:

- Gromadzenie informacji o środowisku replikacji, łącznie z maszyny wirtualne dysków za pośrednictwem SMS i miejsca do magazynowania na dysku.
- Szacowanie dziennego kursu Zmień (pochodząca), które będą dostępne dla zreplikowanej danych. Za pomocą [Planowania pojemności dla funkcji Hyper-V replice](https://www.microsoft.com/download/details.aspx?id=39057) ułatwiające wykonaj następujące czynności.

1.  Kliknij przycisk **Pobierz** , aby pobrać narzędzie, a następnie uruchom go. [Przeczytaj artykuł](site-recovery-capacity-planner.md) dostarczonej narzędzie.
2.  Po zakończeniu wybierz opcję **Tak** w **został uruchomiony planowania pojemności**?

    ![Planowanie pojemności](./media/site-recovery-hyper-v-site-to-azure/gs-capacity-planning.png)

### <a name="network-bandwidth-considerations"></a>Zagadnienia dotyczące przepustowości sieci

Narzędzie do planowania pojemności służy do obliczania przepustowości, potrzebnych dla replikacji (replikacji początkowej, a następnie delta). Aby sterować ilością wykorzystania przepustowości replikacji dostępnych jest kilka opcji:

- **Przepustowości**: ruch funkcji Hyper-V replikuje Azure przechodzi określonym hoście funkcji Hyper-V. Czy przepustowości na serwerze hosta.
- **Dostosować a przepustowości**: można wpływać na przepustowość dla replikacji przy użyciu kilka kluczach rejestru.

#### <a name="throttle-bandwidth"></a>Przepustowości

1. Otwieranie przystawki Microsoft Azure kopia zapasowa MMC na serwerze hosta funkcji Hyper-V. Domyślnie skrót kopia zapasowa Microsoft Azure jest dostępny na komputerze lub w C:\Program Files\Microsoft Azure odzyskiwania usług Agent\bin\wabadmin.
2. W polu przystawki kliknij pozycję **Zmień właściwości**.
3. Na karcie **Throttling** zaznacz pole wyboru **Włącz wykorzystania przepustowości internetowej ograniczania dla kopii zapasowych**, ustawianie limitów pracy i wartością pracy godziny. Prawidłowe zakresy są od 512 KB/s 102 MB/s na sekundę.

    ![Przepustowości](./media/site-recovery-hyper-v-site-to-azure/throttle2.png)

Za pomocą polecenia cmdlet [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) ustawić ograniczania. Oto przykładowa:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Zestaw OBMachineSetting NoThrottle** wskazuje, że nie ograniczania jest wymagane.


#### <a name="influence-network-bandwidth"></a>Mieć wpływ na przepustowość sieci

1. W rejestrze przejdź do **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
    - Wpływ na ruch przepustowości na dysku replikujących, zmień wartość **UploadThreadsPerVM**lub utworzyć klucza, jeśli nie istnieje.
    - Wpływ na przepustowość ruchu awarii z platformy Azure, zmień wartość **DownloadThreadsPerVM**.
2. Wartość domyślna to 4. W sieci "overprovisioned" należy zmienić klucze rejestru z wartościami domyślnymi. Wartość maksymalna to 32. Monitorowanie ruchu w celu zoptymalizowania wartość.

## <a name="step-6-enable-replication"></a>Krok 6: Włącz replikacji

Teraz włączyć replikacji w następujący sposób:

1. Kliknij pozycję **Krok 2: powielić aplikacji** > **źródła**. Po włączeniu replikacji po raz pierwszy będzie kliknij **+ replikacji** w magazynu, aby włączyć replikacji na dodatkowych komputerach.

    ![Włączanie replikacji](./media/site-recovery-hyper-v-site-to-azure/enable-replication.png)

2. W karta **źródła** > Wybierz witrynę funkcji Hyper-V. Kliknij przycisk **OK**.
3. W polu **obiekt docelowy** Wybierz subskrypcję magazynu, a model pracy awaryjnej, który ma być używany w Azure (Zarządzanie classic lub zasobu) po przełączeniu.
4. Wybierz konto miejsca do magazynowania, który ma być używany. Jeśli chcesz korzystać z konta innego miejsca do magazynowania niż masz użytkownik może [utworzyć](#set-up-an-azure-storage-account). Aby utworzyć miejsca do magazynowania konta przy użyciu modelu Menedżera zasobów kliknij pozycję **Utwórz nowy**. Jeśli chcesz utworzyć konto miejsca do magazynowania przy użyciu modelu Klasyczny, wykonaj tę [w portalu Azure](../storage/storage-create-storage-account-classic-portal.md). Kliknij przycisk **OK**.
5.  Wybierz pozycję Azure sieci i podsieci, do której maszyny wirtualne Azure połączy, gdy są one surowa w górę po przełączeniu. Wybierz pozycję **Konfiguruj teraz na wybranych komputerach** , aby zastosować ustawienie sieci do wszystkich wybranych komputerach ochrony. Wybierz pozycję **Konfiguruj później** , aby wybrać Azure sieć na komputer. Jeśli chcesz użyć innej sieci niż masz możesz można [utworzyć](#set-up-an-azure-network). Aby utworzyć sieci przy użyciu modelu Menedżera zasobów kliknij pozycję **Utwórz nowy**. Jeśli chcesz utworzyć sieć przy użyciu modelu Klasyczny, wykonaj tę [w portalu Azure](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). W razie potrzeby wybierz podsieć. Kliknij przycisk **OK**.

    ![Włączanie replikacji](./media/site-recovery-hyper-v-site-to-azure/enable-replication11.png)

6. W **środowisku maszyn wirtualnych** > **Wybierz maszyn wirtualnych** kliknij i wybierz na każdym komputerze, który chcesz odtworzyć. Możesz wybrać tylko komputerów, dla których można włączyć replikacji. Kliknij przycisk **OK**.

    ![Włączanie replikacji](./media/site-recovery-hyper-v-site-to-azure/enable-replication5.png)

11. W oknie dialogowym **Właściwości** > **Konfigurowanie właściwości**, wybierz system operacyjny dla wybranego maszyny wirtualne i dysku systemu operacyjnego. Sprawdź, czy Azure maszyn wirtualnych (Nazwa docelowej) spełnia [wymagania Azure maszyn wirtualnych](site-recovery-best-practices.md#azure-virtual-machine-requirements) i zmodyfikuj ją, jeśli musisz. Kliknij przycisk **OK**. Dodatkowe właściwości można ustawić później.

    ![Włączanie replikacji](./media/site-recovery-hyper-v-site-to-azure/enable-replication6.png)

12. W obszarze **Ustawienia replikacji** > **Konfiguruj ustawienia replikacji**, wybierz zasady replikacji mają być stosowane do chronionego maszyny wirtualne. Kliknij przycisk **OK**. Można modyfikować zasady replikacji w **ustawieniach** > **zasady replikacji** > Nazwa zasady > **Zmień ustawienia**. Zmiany będzie używana dla komputerów, na których są już replikacji i nowym komputerze.

    ![Włączanie replikacji](./media/site-recovery-hyper-v-site-to-azure/enable-replication7.png)

Możesz śledzić postęp zadania **Włącz ochronę** w **ustawieniach** > **zadania** > **zadań Odzyskiwanie witryny**. Po uruchomieniu zadania **Finalizowanie ochronę** komputera jest gotowy do przełączania awaryjnego.

### <a name="view-and-manage-vm-properties"></a>Wyświetlanie i zarządzanie właściwościami maszyn wirtualnych

Zaleca się sprawdzenie właściwości komputerem źródłowym.

1. Kliknij pozycję **Ustawienia** > **Chronionych elementów** > **Replikowane elementów** > i wybierz pozycję komputer.

    ![Włączanie replikacji](./media/site-recovery-hyper-v-site-to-azure/test-failover1.png)

2. W oknie dialogowym **Właściwości** możesz wyświetlać informacje o maszyn wirtualnych replikacji i pracy awaryjnej.

    ![Włączanie replikacji](./media/site-recovery-hyper-v-site-to-azure/test-failover2.png)

3. W **obliczeń i sieci** > **obliczyć właściwości** można określić rozmiar maszyn wirtualnych Azure nazwę i docelowej. Zmodyfikuj ją zgodnie z wymaganiami Azure, jeśli musisz. Można również wyświetlać i modyfikować informacji o sieci docelowej, podsieci i adres IP, który zostanie przypisany do maszyn wirtualnych Azure. Pamiętaj o następujących kwestiach:

    - Można ustawić docelowy adres IP. Jeśli nie podasz adres, nie powiodło się na komputerze użyje DHCP. Jeśli ustawisz adresu, który nie jest dostępne podczas pracy awaryjnej, tym przełączeniu nie powiedzie się. Ten sam adres IP docelowej może służyć do przełączania awaryjnego test, jeśli adres jest dostępna w sieci pracy awaryjnej testowego.
    - Liczba kart sieciowych zależy od rozmiaru zadanej maszyny wirtualnej docelowej:

        - Jeśli liczba kart sieciowych na komputerze źródła jest mniejsza niż lub równa argumentowi liczby kart niedozwolone dla rozmiaru komputera docelowego, docelowej będą mieć taką samą liczbę kart jako źródła.
        - Jeśli liczba kart maszyny wirtualnej źródła przekracza dozwolony dla rozmiaru docelowego, a następnie maksymalny rozmiar docelowej zostanie użyty numer.
        - Na przykład jeśli komputer źródłowy ma dwie karty sieciowe i rozmiar komputer docelowy obsługuje cztery, komputer docelowy będzie miał dwie karty. Jeśli komputer źródłowy ma dwie karty, ale rozmiar obsługiwane docelowy obsługuje tylko jeden komputer docelowy będzie miał tylko jedna karta.     
        - Jeśli maszyn wirtualnych zawiera wiele kart sieciowych ich będzie łączyć się tej samej sieci.

    ![Włączanie replikacji](./media/site-recovery-hyper-v-site-to-azure/test-failover4.png)

5.  W oknie **dysków** można wyświetlić systemu operacyjnego i dyski danych na maszyn wirtualnych, które będą replikowane.


## <a name="step-7-test-the-deployment"></a>Krok 7: Testowanie rozmieszczenia

Aby przetestować rozmieszczenie można uruchamiać trybie awaryjnym test dla jednego komputera wirtualnych lub planu odzyskiwania, który zawiera co najmniej jeden maszyn wirtualnych.


### <a name="prepare-for-test-failover"></a>Przygotowywanie się do przełączania awaryjnego test

- Do uruchamiania w trybie awaryjnym test zaleca się utworzenie nowej sieci Azure, która ma samodzielnie z sieci Azure produkcji (jest to domyślne zachowanie podczas tworzenia nowej sieci w Azure). [Aby uzyskać więcej informacji](site-recovery-failover.md#run-a-test-failover) o uruchamianiu praca awaryjna testowych.
- Aby uzyskać najlepszą wydajność, kiedy nie nad Azure, należy zainstalować agenta Azure na tym komputerze chroniony. Przekształca uruchamiania szybciej i pomaga w rozwiązywaniu problemów. Instalowanie agenta [Linux](https://github.com/Azure/WALinuxAgent) lub [systemu Windows](http://go.microsoft.com/fwlink/?LinkID=394789) .
- Aby w pełni przetestować wdrożenie musisz infrastruktury zreplikowanej komputer działa zgodnie z oczekiwaniami. Jeśli chcesz przetestować usługi Active Directory i systemie DNS możesz tworzenia maszyny wirtualnej jako kontrolera domeny z usługą DNS i powielić to Azure za pomocą Odzyskiwanie witryny Azure. Przeczytaj więcej o [test pracy awaryjnej zagadnienia związane z usługą Active Directory](site-recovery-active-directory.md#considerations-for-test-failover).
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

- Dodaj publiczny adres IP do karty Sieciowej skojarzone z maszyn wirtualnych Azure umożliwia RDP.
- Upewnij się, że nie ma żadnych zasad domeny, uniemożliwiające nawiązywanie połączenia przy użyciu publicznego adresu maszyny wirtualnej.
- Spróbuj połączyć. Jeśli nie możesz połączyć upewnij się, że działa maszyn wirtualnych. Aby uzyskać więcej porad dotyczących rozwiązywania problemów przeczytaj ten [artykuł](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

Jeśli chcesz uzyskać dostęp do maszyn wirtualnych Azure systemem Linux po przełączeniu przy użyciu powłoki bezpiecznego klienta (ssh), wykonaj następujące czynności:

**Na komputerze lokalnym przed przejęciem awaryjnym**:

- Upewnij się, że usługa bezpiecznego powłoki na maszyn wirtualnych Azure jest ustawiona na automatyczne uruchamianie na uruchamiania systemu.
- Upewnij się, że reguły zapory zezwalać na połączenia SSH.

**Na Azure maszyn wirtualnych po przełączeniu**:

- Muszą zasad grupy zabezpieczeń sieci na nie powiodło się nad maszyn wirtualnych i Azure podsieci, do którego jest podłączony do zezwalania na połączenia przychodzące do portu SSH.
- Czy można utworzyć publicznej punktu końcowego do zezwalania na połączenia przychodzące SSH port (port TCP 22 domyślnie).
- Jeśli maszyn wirtualnych jest dostępna za pośrednictwem połączenia VPN (trasy Express lub witryny do witryny sieci VPN) klienta mogą używane bezpośrednio z maszyn wirtualnych przez SSH.

### <a name="run-a-test-failover"></a>Uruchom w trybie awaryjnym test

Aby uruchomić test pracy awaryjnej następujące czynności:

1. Niepowodzenie w jednym maszyn wirtualnych w **ustawieniach** > **Replikowane elementów**, kliknij pozycję maszyn wirtualnych > **+ pracy awaryjnej Test**.

    ![Testowanie pracy awaryjnej](./media/site-recovery-hyper-v-site-to-azure/run-failover1.png)

2. Niepowodzenie nad planu odzyskiwania w **ustawieniach** > **Odzyskiwania plany**, kliknij prawym przyciskiem myszy odpowiedni plan > **Testowanie pracy awaryjnej**. Aby utworzyć odzyskiwania planowanie, [wykonaj następujące instrukcje](site-recovery-create-recovery-plans.md).

3. W **Pracy awaryjnej testowych** wybierz Azure sieci, do którego będzie można łączyć maszyny wirtualne Azure po awaria wystąpiła.

    ![Testowanie pracy awaryjnej](./media/site-recovery-hyper-v-site-to-azure/run-failover2.png)

4. Kliknij **przycisk OK** , aby rozpocząć tym przełączeniu. Możesz śledzić postęp, klikając pozycję maszyn wirtualnych, aby otworzyć jej properies lub stanowiska **Pracy awaryjnej testowych** w **ustawieniach** > **zadań Odzyskiwanie witryny**.
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


## <a name="failover"></a>Praca awaryjna

Po zakończeniu początkowej replikacji dla swojego urządzenia, w razie potrzeby można wywołać praca awaryjna. Odzyskiwanie witryny obsługuje różnego typu Praca awaryjna — przejęcia awaryjnego Test, pracy awaryjnej planowane i nieplanowanego przełączania awaryjnego.
[Aby uzyskać więcej informacji](site-recovery-failover.md) na temat różnych rodzajów praca awaryjna i szczegółowe opisy kiedy i jak wykonać poszczególne z nich.

> [AZURE.NOTE] W przypadku usługi zamiarem migracji maszyn wirtualnych do Azure, zalecamy używać [operacji planowana pracy awaryjnej](site-recovery-failover.md#run-a-planned-failover-primary-to-secondary) do migracji maszyn wirtualnych do Azure. Po uwierzytelnieniu migracją aplikacji platformy Azure za pomocą pracy awaryjnej test wykonaj czynności wymienione w obszarze [Wykonane migracji](#Complete-migration-of-your-virtual-machines-to-Azure) , aby przeprowadzić migrację maszyn wirtualnych. Nie musisz wykonywać Zatwierdź lub Usuń. Ukończono migracji ukończeniu migracji, powoduje usunięcie ochrony dla maszyny wirtualnej i przestaje Odzyskiwanie witryny Azure rozliczenia dla komputera.


###<a name="run-an-unplanned-failover"></a>Uruchamianie nieplanowanego przełączania awaryjnego

To należy wybrać podczas podstawowej witryny staje się niedostępne ze względu na nieoczekiwane zdarzenie, na przykład ataki awarii lub wirusami power. Ta procedura opisano sposób uruchamiania "nieplanowanego przełączania awaryjnego" plan odzyskiwania. Można też kliknąć można uruchamiać awaryjnego jednym komputerze wirtualnych na karcie maszyn wirtualnych. Zanim zaczniesz, upewnij się, że wszystkie maszyn wirtualnych chcesz zakończyć się niepowodzeniem na ukończenie replikacji początkowej.

1. Wybierz pozycję **odzyskiwania Plany > recoveryplan_name**.
2. Karta planu odzyskiwania, wybierz polecenie **Nieplanowanego przełączania awaryjnego**.
3. Na stronie **nieplanowanego przełączania awaryjnego** wybierz lokalizacji źródłowej i docelowej.
4. Wybierz **zamknięty maszyn wirtualnych i synchronizowanie najnowszych danych** Określ, czy Odzyskiwanie witryny należy spróbować zamknąć chronionego maszyn wirtualnych i zsynchronizować dane, tak aby najnowszą wersję pakietu danych nie powiedzie się nad.
5. Po przełączeniu maszyn wirtualnych znajdują się w Zatwierdź stanie oczekiwania.  Kliknij przycisk **Zatwierdź** , aby zatwierdzić tym przełączeniu.

[Dowiedz się więcej](site-recovery-failover.md#run-an-unplanned-failover)

## <a name="complete-migration-of-your-virtual-machines-to-azure"></a>Zakończenie migracji maszyn wirtualnych Azure


>[AZURE.NOTE] Następujące kroki dotyczą tylko wtedy, gdy są migracji maszyn wirtualnych Azure

1. Pełnienie planowanej pracy awaryjnej wymienione [poniżej](site-recovery-failover.md)
2. W **Ustawienia > replikowane elementów**, kliknij prawym przyciskiem myszy maszyny wirtualnej i wybierz polecenie **Wykonania migracji**

    ![completemigration](./media/site-recovery-hyper-v-site-to-azure/migrate.png)

2. Kliknij **przycisk OK** , aby przeprowadzić migrację. Możesz śledzić postęp, klikając na maszyn wirtualnych, aby otworzyć jego właściwości lub przy użyciu zadania wykonane migracji w **Ustawienia > zadań Odzyskiwanie witryny**.


## <a name="monitor-your-deployment"></a>Monitorowanie wdrożenia

Poniżej opisano, jak można monitorować ustawienia konfiguracji, status i kondycji rozmieszczania Odzyskiwanie witryny:

1. Kliknij nazwę magazynu, aby uzyskać dostęp do pulpitu nawigacyjnego **Essentials** . W tym pulpitu nawigacyjnego można Odzyskiwanie witryny zadania, stan replikacji plany naprawcze, Kondycja serwera i zdarzeń.  Możesz dostosować Essentials umożliwia wyświetlanie kafelków i układów, które są najbardziej użyteczne, tym stanu innych magazynów Odzyskiwanie witryny i kopia zapasowa.

    ![Podstawowe informacje dotyczące](./media/site-recovery-hyper-v-site-to-azure/essentials.png)

2. Na kafelku **kondycji** można monitorować serwery witryny, w których występuje problem i zdarzenia powstałe Odzyskiwanie witryny w ciągu ostatnich 24 godzin.
3. Możesz zarządzać i monitorować replikacji **Replikowane elementów**, **Plany odzyskiwania**, i Kafelki **Zadań Odzyskiwanie witryny** . Można przechodzić do zadań w **ustawieniach** -> **zadania** -> **Zadań Odzyskiwanie witryny**.
