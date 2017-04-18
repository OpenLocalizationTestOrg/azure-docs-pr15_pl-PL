<properties 
   pageTitle="Automatyzowanie DR dla udziały plików na StorSimple przy użyciu Odzyskiwanie witryny Azure | Microsoft Azure"
   description="W tym artykule opisano czynności i najlepsze rozwiązania dotyczące tworzenia rozwiązanie odzyskiwania po awarii udziały plików obsługiwanych StorSimple ilość miejsca do magazynowania."
   services="storsimple"
   documentationCenter="NA"
   authors="vidarmsft"
   manager="syadav"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/16/2016"
   ms.author="vidarmsft" />

# <a name="automated-disaster-recovery-solution-using-azure-site-recovery-for-file-shares-hosted-on-storsimple"></a>Automatyczne rozwiązanie awarii przy użyciu Odzyskiwanie witryny Azure dla udziały plików obsługiwanych na StorSimple

## <a name="overview"></a>Omówienie

Microsoft Azure StorSimple jest rozwiązaniem miejsca do magazynowania chmury hybrydowe, która dotyczy złożoności niestrukturalne danych często skojarzone z udziałach plików. StorSimple używa magazynu w chmurze jako rozszerzenie lokalnego rozwiązania i automatycznie poziomów danych wszystkich lokalnego przechowywania i magazynu w chmurze. Zintegrowane ochrona danych z lokalnych i w chmurze migawek, eliminuje konieczność sprawling infrastruktury miejsca do magazynowania.

[Odzyskiwanie witryny Azure](../site-recovery/site-recovery-overview.md) jest oparte na platformie Azure usługa, która zapewnia możliwości odzyskiwania (DR) awarii przy orchestrating replikacji, pracy awaryjnej i odzyskiwanie maszyn wirtualnych. Odzyskiwanie witryny Azure obsługuje wiele technologii replikacji spójne replikować, ochronę i bezproblemowo kończą się niepowodzeniem w środowisku maszyn wirtualnych systemu i aplikacji do publicznego i prywatnego lub hostowanej chmury.

Przy użyciu Odzyskiwanie witryny Azure, replikacji maszyn wirtualnych i StorSimple chmury migawek, można chronić środowisko serwera całego pliku. W przypadku przerwania jednym kliknięciem umożliwia przybliżenie Azure usługi udziały plików w trybie online w ciągu kilku minut.

Ten dokument w szczegółowo wyjaśniono sposób można utworzyć rozwiązanie odzyskiwania danych dla swojego udziały plików obsługiwanych StorSimple ilość miejsca do magazynowania i wykonywać planowanego, niezaplanowane i testowanie pracy awaryjnej za pomocą planu odzyskiwania jednym kliknięciem. W zasadzie pokazano, jak można zmodyfikować Plan odzyskiwania magazynu usługi Azure Odzyskiwanie witryny umożliwiające praca awaryjna StorSimple podczas scenariusze awarii. Ponadto opisano obsługiwanych konfiguracji i wymagania wstępne. Ten dokument przyjęto założenie, że znasz podstawy architektury Odzyskiwanie witryny Azure i StorSimple.

## <a name="supported-azure-site-recovery-deployment-options"></a>Obsługiwane opcje wdrażania Odzyskiwanie witryny Azure

Klienci mogą wdrażanie serwerów plików jako serwerów fizycznych lub wirtualnych maszyn uruchomionych dla funkcji Hyper-V lub VMware, a następnie utwórz udziały plików z wielkości carved Brak miejsca StorSimple. Azure Odzyskiwanie witryny można chronić fizycznych i wirtualnych wdrożeń pomocniczej witryny lub Azure. W tym dokumencie opisano szczegóły rozwiązanie DR z Azure jako witryny odzyskiwania serwera plików, które maszyn wirtualnych znajdujących się na funkcji Hyper-V i udziały plików StorSimple ilość miejsca do magazynowania. Inne scenariusze, w których serwer plików maszyn wirtualnych jest na maszyny VMware lub fizyczny komputer może nastąpić podobnie.

## <a name="prerequisites"></a>Wymagania wstępne

Implementowanie rozwiązania odzyskiwania po jednym kliknięciem awarii, używaną Odzyskiwanie witryny Azure na potrzeby udziały plików obsługiwanych w magazynie StorSimple występują następujące wymagania:

-   Serwer Windows Server 2012 R2 plik, który maszyn wirtualnych znajdujących się na funkcji Hyper-V lub VMware lub fizycznie komputera lokalnego

-   StorSimple miejsca do magazynowania urządzenia lokalne zarejestrowany za pomocą Menedżera Azure StorSimple

-   StorSimple chmury urządzenia utworzone w Menedżerze Azure StorSimple (to mogą być przechowywane w stanie zamykanie)

-   Udziały plików obsługiwanych ilości skonfigurowane na urządzeniu magazynującym StorSimple

-   [Odzyskiwanie witryny azure usług magazynu](../site-recovery/site-recovery-vmm-to-vmm.md) utworzone w subskrypcji Microsoft Azure

Ponadto Azure w przypadku witryny odzyskiwania, uruchom [Narzędzie do oceny gotowości maszyn wirtualnych Azure](http://azure.microsoft.com/downloads/vm-readiness-assessment/) na maszyny wirtualne, aby upewnić się, że są one zgodne z usługami maszyny wirtualne Azure i odzyskiwanie witryny Azure.

Aby uniknąć opóźnienie problemów (które mogą spowodować zwiększenie kosztów), upewnij się, tworzenia StorSimple chmury urządzenia, konto automatyzacji i przechowywanie konta lub kont w tym samym regionie.

## <a name="enable-dr-for-storsimple-file-shares"></a>Włączanie DR dla StorSimple udziały plików  

Każdy składnik lokalnego środowiska musi być chronione umożliwiające dokończenie replikacji i odzyskiwania. W tej sekcji opisano sposoby:

-   Konfigurowanie replikacji usługi Active Directory i systemie DNS (opcjonalnie)

-   Aby włączyć ochronę serwera plików maszyn wirtualnych za pomocą Odzyskiwanie witryny Azure

-   Włącz ochronę wielkości StorSimple

-   Konfigurowanie sieci

### <a name="set-up-active-directory-and-dns-replication-optional"></a>Konfigurowanie replikacji usługi Active Directory i systemie DNS (opcjonalnie)

Jeśli chcesz chronić komputerów z systemem DNS i usługi Active Directory, tak aby były dostępne w witrynie DR, należy jawnie ich ochrony (dzięki temu serwery plików są dostępne po błędów przez z uwierzytelnianiem). Dostępne są dwie opcje zalecane, zależnie od stopnia złożoności środowiska lokalnego klienta.

#### <a name="option-1"></a>Opcja 1

Jeśli odbiorca ma małą liczbę aplikacji, pojedynczy kontroler domeny dla całej lokalnej witryny, a awarii na całą witrynę, a następnie zaleca się używanie replikacji Odzyskiwanie witryny Azure replikacji komputera kontrolera domeny w witrynie pomocniczą (dotyczy zarówno dla witryny do witryny i witryny do Azure).

#### <a name="option-2"></a>Opcja 2

Jeśli odbiorcy wiele aplikacji, jest uruchomiony las usługi Active Directory, a także będzie braku przez kilka aplikacji naraz, a następnie zalecamy konfigurowania dodatkowego kontrolera domeny w witrynie DR (pomocniczej witryny lub Azure).

Można znaleźć [rozwiązanie automatycznego DR dla usługi Active Directory i systemie DNS za pomocą Odzyskiwanie witryny Azure](../site-recovery/site-recovery-active-directory.md) instrukcje podczas udostępniania kontrolera domeny w witrynie DR. Dla pozostała część tego dokumentu będzie przyjęto założenie, że kontrolera domeny jest dostępna w witrynie DR.

### <a name="use-azure-site-recovery-to-enable-protection-of-the-file-server-vm"></a>Aby włączyć ochronę serwera plików maszyn wirtualnych za pomocą Odzyskiwanie witryny Azure

Ten krok wymaga przygotowania środowiska lokalnego serwera plików, tworzenie i przygotowywanie magazynu Odzyskiwanie witryny Azure i włącz ochronę pliku maszyn wirtualnych.

#### <a name="to-prepare-the-on-premises-file-server-environment"></a>Aby przygotować środowisko serwera lokalnego pliku

1.  Ustaw **powiadamiaj nigdy** **Kontrola konta użytkownika** . Jest to wymagane, tak aby skryptów automatyzacji Azure umożliwia łączenie elementów docelowych iSCSI po błędów od podstaw, odzyskiwanie witryny Azure.

    1.  Naciśnij klawisze Windows + Q i wyszukiwanie **Kontrola konta użytkownika**.

    2.  Wybierz **Ustawienia Zmień Kontrola konta użytkownika**.

    3.  Przeciągnij pasek w dół w kierunku **Powiadamiaj nigdy**.

    4.  Kliknij **przycisk OK** , a następnie wybierz pozycję **Tak,** po wyświetleniu monitu.

        ![](./media/storsimple-dr-using-asr/image1.png)

1.  Instalowanie agenta maszyn wirtualnych na każdej z serwera plików maszyny wirtualne. Jest to wymagane, aby uruchomić skrypty Azure automatyzacji na nie powiodło się nad maszyny wirtualne.

    1.  [Pobierz agenta](http://aka.ms/vmagentwin) na `C:\\Users\\<username>\\Downloads`.

    2.  Otwieranie programu Windows PowerShell w trybie administratora (Uruchom jako Administrator), a następnie wprowadź następujące polecenie, aby przejść do lokalizacji pobierania:

        `cd C:\\Users\\<username>\\Downloads\\WindowsAzureVmAgent.2.6.1198.718.rd\_art\_stable.150415-1739.fre.msi`

        > [AZURE.NOTE] Nazwa pliku może się zmienić w zależności od wersji.

1.  Kliknij przycisk **Dalej**.

2.  Zaakceptuj **Warunki umowy** , a następnie kliknij przycisk **Dalej**.

3.  Kliknij przycisk **Zakończ**.


1.  Tworzenie udziały plików za pomocą wielkości carved Brak miejsca StorSimple. Aby uzyskać więcej informacji zobacz [Używanie usługi Menedżera StorSimple Zarządzanie wielkości](storsimple-manage-volumes.md).

    1.  Na swojej lokalnej maszyny wirtualne naciśnij klawisze Windows + Q i wyszukiwanie **iSCSI**.

    2.  Wybierz pozycję **inicjatorach**.

    3.  Wybierz kartę **Konfiguracja** i skopiuj nazwę inicjator.

    4.  Zaloguj się do [portalu klasyczny Azure](https://manage.windowsazure.com/).

    5.  Wybierz kartę **StorSimple** , a następnie wybierz usługę Menedżer StorSimple zawierającą urządzenia fizycznego.

    6.  Tworzenie służące głośności, a następnie utwórz woluminów. (Tych wielkości są dla każdej plik na serwerze plików maszyny wirtualne). Skopiuj nazwę Inicjator i nadaj odpowiednią nazwę rekordów kontrolki programu Access, podczas tworzenia wielkość.

    7.  Wybierz kartę **Konfiguruj** i zanotuj w adres IP urządzenia.

    8.  W swojej lokalnej maszyny wirtualne Przejdź ponownie do **inicjatorach** i wprowadź IP w sekcji szybkie połączenie. Kliknij przycisk **Szybkie połączenie** (urządzenie powinno być teraz połączone).

    9.  Otwórz Portal Azure zarządzania, a następnie wybierz kartę **wielkości i urządzeń** . Kliknij przycisk **Konfiguruj automatyczne**. Głośność, która została właśnie utworzona powinien być wyświetlany.

    10. W portalu, wybierz kartę **urządzenia** , a następnie wybierz **utworzyć nowe urządzenie wirtualną.** (To urządzenie wirtualna zostanie użyty, jeśli występuje w trybie awaryjnym). Ten nowy urządzenia wirtualnego można przechowywać w trybie offline w celu uniknięcia dodatkowych kosztów. Przełączanie urządzenia wirtualnego do trybu offline, przejdź do sekcji **maszyn wirtualnych** w portalu i zamknij go.

    11. Wróć do maszyny wirtualne lokalnego i otwórz zarządzania dysku (naciśnij klawisze Windows + X i wybierz pozycję **Zarządzanie dysku**).

    12. Można zauważyć pewne dodatkowe dyski (w zależności od liczby ilości, które zostały utworzone). Kliknij prawym przyciskiem myszy pierwszą, wybierz **Dysk zainicjować**i wybierz **przycisk OK**. Kliknij prawym przyciskiem myszy sekcję **Unallocated** , wybierz pozycję **Nowy prosty**, przypisać go literę i zakończyć działanie kreatora.

    13. Powtórz krok l dla wszystkich dysków. Można teraz zobaczyć wszystkie dyski na **Ten komputer** w Eksploratorze Windows.

    14. Za pomocą roli usługi przechowywania plików i tworzyć udziały plików tych ilości.

#### <a name="to-create-and-prepare-an-azure-site-recovery-vault"></a>Aby utworzyć i przygotować magazynu Odzyskiwanie witryny Azure

Zapoznaj się z [dokumentacją Odzyskiwanie witryny Azure](../site-recovery/site-recovery-hyper-v-site-to-azure.md) rozpocząć pracę z Odzyskiwanie witryny Azure przed zastosowaniem ochrony serwera plików maszyn wirtualnych.

#### <a name="to-enable-protection"></a>Aby włączyć ochronę

1.  Rozłączanie iSCSI target(s) z lokalnego maszyny wirtualne, które mają być chronione za pomocą Azure Odzyskiwanie witryny:

    1.  Naciśnij klawisze Windows + Q i wyszukiwanie **iSCSI**.

    2.  Wybierz pozycję **Konfigurowanie inicjatorach**.

    3.  Odłącz urządzenie StorSimple, którą wcześniej połączenie. Alternatywnie możesz przełączać się z serwera plików przez kilka minut po włączenie ochrony.

    > [AZURE.NOTE] Spowoduje to udziałów plików, aby być tymczasowo niedostępne

1.  [Włącz ochronę maszyn wirtualnych](../site-recovery/site-recovery-hyper-v-site-to-azure.md##step-6-enable-replication) serwera plików maszyn wirtualnych z portalu Odzyskiwanie witryny Azure.

2.  Po początkowej synchronizacji, możesz ponownie docelowej. Przejdź do inicjatorach, wybierz urządzenie, StorSimple i kliknij przycisk **Połącz**.

3.  Po zakończeniu synchronizacji i stan maszyn wirtualnych jest **chronione**, zaznacz maszyn wirtualnych, wybierz kartę **Konfiguruj** i odpowiednio aktualizować sieci maszyn wirtualnych (jest to sieci, w której nie powiodło się nad VM(s) będą częścią). Jeśli sieć nie jest widoczna, oznacza to, że synchronizacja nadal trwa.

### <a name="enable-protection-of-storsimple-volumes"></a>Włącz ochronę wielkości StorSimple

Jeśli nie wybrano opcję **Włącz kopii zapasowej domyślnych dla tego woluminu** dla wielkość StorSimple, przejdź do **Kopii zapasowej zasady** w usłudze Menedżer StorSimple i tworzenie kopii zapasowej odpowiednich zasad dla wszystkich wielkość. Zalecane ustawienie częstotliwości wykonywania kopii zapasowych do celu punktu odzyskiwania (RPO), który chcesz wyświetlić dla aplikacji.

### <a name="configure-the-network"></a>Konfigurowanie sieci

Na serwerze plików maszyn wirtualnych ustawień sieci w Odzyskiwanie witryny Azure tak skonfigurować program maszyn wirtualnych sieci zostaną dołączone do odpowiedniej sieci DR po przełączeniu.

Możesz wybrać maszyn wirtualnych w **Chmurze VMM** lub **Grupa ochrona** do skonfigurowania ustawień sieci, jak pokazano na poniższej ilustracji.

![](./media/storsimple-dr-using-asr/image2.png)

## <a name="create-a-recovery-plan"></a>Tworzenie planu odzyskiwania

Możesz utworzyć plan odzyskiwania w automatycznego odzyskiwania systemu, aby zautomatyzować awaryjnego procesu udziałach plików. Jeśli występuje zakłócenia w pracy, możesz zabrać ze sobą udziały plików w górę w ciągu kilku minut za pomocą jednego kliknięcia. Aby włączyć ten automatyzacji, konieczne będzie konto Azure automatyzacji.

#### <a name="to-create-the-account"></a>Aby utworzyć konto

1.  Przejdź do portalu klasyczny Azure i przejdź do sekcji **automatyzacji** .

1.  Tworzenie nowego konta automatyzacji. Pozostaw je w tym samym geo/regionu w którym zostały utworzone konta miejsca do magazynowania i StorSimple chmury urządzenia.

2.  Kliknij przycisk **Nowy** &gt; **Aplikacji usług** &gt; **automatyzacji** &gt; **działań aranżacji** &gt; **Z galerii** , aby zaimportować wszystkie wymagane runbooks do konta automatyzacji.

    ![](./media/storsimple-dr-using-asr/image3.png)

1.  Dodaj następujące runbooks w okienku **Awarii** w galerii:

    -   Niepowodzenie nad StorSimple głośność kontenerów

    -   Oczyść wielkości StorSimple po pracy awaryjnej Test (TFO)

    -   Wielkości instalacji na urządzeniu StorSimple po przełączeniu

    -   Rozpoczynanie urządzenia wirtualnego StorSimple

    -   Odinstalowywanie rozszerzenia skryptu niestandardowego w maszyn wirtualnych Azure

        ![](./media/storsimple-dr-using-asr/image4.png)


1.  Publikowanie wszystkich skryptów, wybierając działań aranżacji konta automatyzacji i przejść do karty **autora** . Po wykonaniu tego kroku na karcie **Runbooks** będzie wyglądać następująco:

     ![](./media/storsimple-dr-using-asr/image5.png)

1.  W oknie konta automatyzacji przejdź na kartę **zasoby** , kliknij przycisk **Dodaj ustawienie** &gt; **Dodaj poświadczenia**i Dodaj poświadczenia Azure — nazwa elementu AzureCredential.

    Użyj poświadczeń systemu Windows PowerShell. Należy to poświadczeń, która zawiera identyfikator organizacji nazwy użytkownika i hasła, z dostępem do tej subskrypcji Azure i wyłączone uwierzytelnianie wieloskładnikowe. Jest to wymagane do uwierzytelnienia w imieniu użytkownika podczas pracy awaryjnej i uzupełnić wielkość serwera plików w witrynie DR.

1.  Na koncie automatyzacji wybierz kartę **zasoby** , a następnie kliknij przycisk **Dodaj ustawienie** &gt; **zmiennej Dodaj** i dodaj następujące zmienne. Istnieje możliwość szyfrowanie te zasoby. Zmienne są specyficzne dla planu odzyskiwania. Jeśli do odzyskiwania (który ma zostać utworzony w następnym kroku) nazwa jest TestPlan, a następnie zmiennych powinny być TestPlan StorSimRegKey, TestPlan AzureSubscriptionName i tak dalej.

    -   *RecoveryPlanName* **-StorSimRegKey**: klucz rejestru dla usługi Menedżera StorSimple.

    -   *RecoveryPlanName* **-AzureSubscriptionName**: nazwę Azure subskrypcji.

    -   *RecoveryPlanName* **-ResourceName**: Nazwa zasobu StorSimple, który urządzenia StorSimple.

    -   *RecoveryPlanName* **-Nazwa_urządzenia**: urządzenie, które ma być nie powiodło się nad.

    -   *RecoveryPlanName* **-TargetDeviceName**: urządzenia chmury StorSimple, w którym mają się nie powiodło się nad kontenerów.

    -   *RecoveryPlanName* **-VolumeContainers**: przecinkami ciąg kontenerów głośność urządzenia, które muszą być nie powiodło się; na przykład volcon1, volcon2, volcon3.

    -   RecoveryPlanName**- TargetDeviceDnsName**: nazwę urządzenia (ten znajduje się w sekcji **maszyn wirtualnych** : Nazwa usługi jest taka sama jak nazwa DNS).

    -   *RecoveryPlanName* **-StorageAccountName**: nazwę konta magazynu, w którym będą przechowywane skrypt (który ma uruchamiać na nie powiodło się nad maszyn wirtualnych). Może to być dowolnego miejsca do magazynowania konta, które ma miejsce do przechowywania skrypt tymczasowo.

    -   *RecoveryPlanName* **-StorageAccountKey**: klucz dostępu do konta powyżej miejsca do magazynowania.

    -   *RecoveryPlanName* **-ScriptContainer**: nazwa kontenera, w którym skrypt będą przechowywane w chmurze. Jeśli kontener nie istnieje, zostanie on utworzony.

    -   *RecoveryPlanName* **-VMGUIDS**: po ochrona maszyny, odzyskiwanie witryny Azure przypisuje każdej maszyn wirtualnych Unikatowy identyfikator, który zawiera szczegółowe informacje o niepowodzeniu nad maszyn wirtualnych. Aby uzyskać VMGUID, wybierz kartę **Usługi odzyskiwania** , a następnie kliknij **Element chroniony** &gt; **Ochrony** &gt; **maszyny** &gt; **Właściwości**. Jeśli masz wiele maszyny wirtualne, Dodaj identyfikatory GUID jako ciąg rozdzielanych przecinkami.

    -   *RecoveryPlanName* **-AutomationAccountName** — nazwę konta automatyzacji, w którym została dodana runbooks i zasoby.

    Na przykład jeśli nazwa planu odzyskiwania jest fileServerpredayRP, następnie tabulatory **aktywów** powinna wyglądać następująco po dodaniu wszystkich trwałych.

    ![](./media/storsimple-dr-using-asr/image6.png)


1.  Przejdź do sekcji **Usługi odzyskiwania** i wybierz pozycję magazynu Odzyskiwanie witryny Azure, który został utworzony wcześniej.

2.  Wybierz kartę **Plany odzyskiwania** i utworzenie nowego planu odzyskiwania w następujący sposób:

    .  Określ nazwę i wybierz odpowiednią **Grupę ochrony**.

    b.  Wybierz maszyny wirtualne z grupy ochrony, który chcesz uwzględnić w planie odzyskiwania.

    c.  Po wykonaniu odzyskiwania tworzenia planu, zaznacz je, aby otworzyć widok dostosowywania planu odzyskiwania.

    d.  Wybierz pozycję **Zamknij wszystkie grupy**, kliknij **skrypt**i wybierz pozycję **Dodaj skrypt podstawowego po stronie wszystkie grupy zamknięcia systemu**.

    e.  Wybierz konto automatyzacji (w którym zostanie dodany runbooks), a następnie wybierz pozycję działań aranżacji **Niepowodzenie przez StorSimple-obrót-kontenerów** .

    f.  Kliknij pozycję **grupy 1: rozpoczynanie**, wybierz pozycję **maszyn wirtualnych**i dodawanie maszyny wirtualne, które mają być chronione w planie odzyskiwania.

    g.  Kliknij pozycję **grupy 1: rozpoczynanie**, wybierz **skrypt**i dodaj następujące skrypty w kolejności jako kroki **od 1 grupy** .

    - Rozpoczęcie-StorSimple-wirtualnych-urządzenia działań aranżacji
    - Niepowodzenie działań aranżacji przez StorSimple obrót kontenerów
    - Działań aranżacji instalacji wielkości po-przypadku awarii
    - Odinstalowywanie niestandardowe skryptu rozszerzenie działań aranżacji

1.  Dodawanie ręcznego akcji po powyższym skryptów 4 w tym samym **grupy 1: kroki po** sekcji. Ta akcja jest punkt, w którym można sprawdzić, że wszystko działa poprawnie. Ta akcja musi zostać dodany tylko jako część pracy awaryjnej test (dlatego tylko wybierz **Testowanie pracy awaryjnej** pole).

2.  Po Akcja ręczne dodawanie skryptu oczyszczanie, za pomocą tej samej procedury, który był używany przez inne runbooks. Zapisywanie planu odzyskiwania.

    > [AZURE.NOTE] Podczas uruchamiania trybie awaryjnym test, należy sprawdzić wszystko na ręczne czynności, ponieważ wielkość StorSimple, które miały został sklonowany na tym urządzeniu docelowej zostaną usunięte w ramach oczyszczanie po zakończeniu operacji ręcznego.

    ![](./media/storsimple-dr-using-asr/image7.png)

## <a name="perform-a-test-failover"></a>Wykonywanie trybie awaryjnym test

Zapoznaj się z przewodnik companion [Rozwiązanie Active Directory DR](../site-recovery/site-recovery-active-directory.md) dla zagadnień dotyczących określonej w usłudze Active Directory, w tym przełączeniu test. Ustawienia lokalne nie zakłóceń w ogóle, gdy wystąpi awaryjne przeniesienie test. Wielkość StorSimple, które zostały dołączone do lokalnego maszyn wirtualnych są sklonowany na urządzeniu chmury StorSimple Azure. Maszyn wirtualnych do celów testowych zostanie przeniesiony w górę w Azure i wielkość sklonowanym zostaną dołączone do maszyn wirtualnych.

#### <a name="to-perform-the-test-failover"></a>Aby wykonać tym przełączeniu test

1.  W portalu klasyczny Azure wybierz z magazynu odzyskiwania witryny.

1.  Kliknij utworzone dla serwera plików maszyn wirtualnych plan odzyskiwania.

2.  Kliknij przycisk **Testuj pracy awaryjnej**.

3.  Wybierz pozycję wirtualną sieć, aby rozpocząć test awaryjnego procesu.

    ![](./media/storsimple-dr-using-asr/image8.png)

1.  Gdy pomocniczej środowiska znajduje się w górę, można wykonywać z reguły sprawdzania poprawności.

2.  Po zakończeniu sprawdzaniu poprawności kliknij przycisk **Sprawdzanie poprawności wykonane**. Środowisko pracy awaryjnej testowe zostaną wyczyszczone i będzie można ukończyć tej operacji TFO.

## <a name="perform-an-unplanned-failover"></a>Wykonywanie nieplanowanego przełączania awaryjnego

Podczas nieplanowanego przełączania awaryjnego wielkość StorSimple są zainicjowana urządzenia wirtualnego replice maszyn wirtualnych zostanie przeniesione na Azure i wielkość zostaną dołączone do maszyn wirtualnych.

#### <a name="to-perform-an-unplanned-failover"></a>Aby wykonać nieplanowanego przełączania awaryjnego

1.  W portalu klasyczny Azure wybierz z magazynu odzyskiwania witryny.

1.  Kliknij utworzone dla serwera plików maszyn wirtualnych plan odzyskiwania.

2.  Kliknij pozycję **pracy awaryjnej** , a następnie wybierz pozycję **Nieplanowanego przełączania awaryjnego**.

    ![](./media/storsimple-dr-using-asr/image9.png)

1.  Wybierz pozycję sieci docelowej, a następnie kliknij ikonę wyboru ✓, aby rozpocząć awaryjnego procesu.

## <a name="perform-a-planned-failover"></a>Wykonywanie planowanych trybie awaryjnym

Podczas planowanej przejścia awaryjnego w lokalnym plików serwera, który maszyn wirtualnych jest prawidłowe zamknięcie i chmurę pobrania kopii zapasowej migawki wielkości na urządzeniu StorSimple. Wielkość StorSimple są zainicjowana urządzenia wirtualnego, replice maszyn wirtualnych zostanie przeniesiony na Azure i wielkość zostaną dołączone do maszyn wirtualnych.

#### <a name="to-perform-a-planned-failover"></a>Aby wykonać planowanej trybie awaryjnym

1.  W portalu klasyczny Azure wybierz z magazynu odzyskiwania witryny.

1.  Kliknij utworzone dla serwera plików maszyn wirtualnych plan odzyskiwania.

2.  Kliknij pozycję **pracy awaryjnej** , a następnie wybierz pozycję **Planowana pracy awaryjnej**.

3.  Wybierz pozycję sieci docelowej, a następnie kliknij ikonę wyboru ✓, aby rozpocząć awaryjnego procesu.

## <a name="perform-a-failback"></a>Wykonywanie awarii

Podczas awarii StorSimple głośność kontenerów są nie na powrót do urządzenia fizycznego po ich kopii zapasowej.

#### <a name="to-perform-a-failback"></a>Aby wykonać awarii

1.  W portalu klasyczny Azure wybierz z magazynu odzyskiwania witryny.

1.  Kliknij utworzone dla serwera plików maszyn wirtualnych plan odzyskiwania.

2.  Kliknij pozycję **pracy awaryjnej** i wybierz **pracy awaryjnej planowane** lub **nieplanowanego przełączania awaryjnego**.

3.  Kliknij przycisk **Zmień kierunek**.

4.  Wybierz odpowiednie dane synchronizacji i opcje tworzenia maszyn wirtualnych.

5.  Kliknij ikonę wyboru ✓, aby rozpocząć proces awarii.

    ![](./media/storsimple-dr-using-asr/image10.png)

## <a name="best-practices"></a>Najważniejsze wskazówki

### <a name="capacity-planning-and-readiness-assessment"></a>Wydajność ocena planowania i gotowości


#### <a name="hyper-v-site"></a>Witryna funkcji Hyper-V

Za pomocą [narzędzi planowania pojemności użytkownika](http://www.microsoft.com/download/details.aspx?id=39057) do projektowania serwera, miejsca do magazynowania i infrastruktury sieciowej dla środowiska replice funkcji Hyper-V.

#### <a name="azure"></a>Azure

[Narzędzie do oceny gotowości maszyn wirtualnych Azure](http://azure.microsoft.com/downloads/vm-readiness-assessment/) można uruchamiać na maszyny wirtualne, aby upewnić się, że są one zgodne z maszyny wirtualne Azure i usługi odzyskiwania witryny Azure. Narzędzie do oceny gotowości sprawdza konfiguracji maszyn wirtualnych i ostrzega, gdy konfiguracji są niezgodne z Azure. Na przykład emituje Ostrzeżenie Jeśli dysk C: jest większy niż 127 GB.


Planowanie pojemności składa się z co najmniej dwóch ważnych procesów:

-   Mapowanie lokalnego maszyny wirtualne funkcji Hyper-V rozmiary Azure maszyn wirtualnych (na przykład A6, A7 A8 i A9).

-   Określanie wymagane przepustowości internetowej.

## <a name="limitations"></a>Ograniczenia

- Obecnie tylko 1 StorSimple urządzenia może być nie powiodło się nad (na jednym urządzeniu chmury StorSimple). Scenariusz na serwerze plików, który obejmuje kilka urządzeń StorSimple nie jest jeszcze obsługiwane.

- Jeśli zostanie wyświetlony komunikat o błędzie podczas włączania ochrony dla maszyny, upewnij się, że rozłączeniu obiektów docelowych iSCSI.

- Wszystkie kontenerów głośność, które zostały zgrupowane razem ze względu na kopii zapasowej zasad, obejmujące wielu kontenerów głośności nie powiedzie się nad razem.

- Wszystkie wielkość w kontenerach głośność, wybrana nie powiedzie się nad.

- Ilości, które w sumie dają więcej niż 64 TB nie można nie można nad, ponieważ maksymalnej pojemności jednego urządzenia chmury StorSimple jest 64 TB.

- Jeśli planowana nieplanowanego przełączania awaryjnego kończy się niepowodzeniem i maszyny wirtualne są tworzone w Azure, następnie nie wyczyścić maszyny wirtualne. Zamiast tego awarii. Jeśli usuniesz maszyny wirtualne następnie maszyny wirtualne lokalnego nie można włączyć ponownie.

- Po przełączeniu Jeśli nie jesteś w stanie zobaczyć wielkość, przejdź do maszyny wirtualne, otwieranie zarządzania dysku, skanowania dysków i dostosowania ich online.

- W niektórych przypadkach litery dysków w witrynie DR mogą być inne niż litery lokalnego. W takim przypadku należy ręcznie usunąć ten problem, po zakończeniu tym przełączeniu.

- Uwierzytelnianie wieloskładnikowe powinna być wyłączona dla Azure poświadczenia wprowadzone na koncie automatyzacji jako środka trwałego. Jeśli uwierzytelnianie nie jest wyłączone, skrypty nie będą mogli automatycznie uruchamiać i planu odzyskiwania zakończy się niepowodzeniem.

- Limit czasu pracy awaryjnej zadania: skrypt StorSimple limit czasu będzie, jeśli awaryjnego przeniesienia kontenerów głośność trwa dłużej niż limit Odzyskiwanie witryny Azure na skrypt (obecnie 120 minut).

- Przekroczono limit czasu zadania wykonywania kopii zapasowej: Jeśli kopii zapasowej wielkości trwa dłużej niż limit Odzyskiwanie witryny Azure na skrypt (obecnie 120 minut) limitu czasu skryptu StorSimple.
 
    > [AZURE.IMPORTANT] Ręczne uruchamianie kopii zapasowej z portalu Azure, a następnie ponownie uruchom planu odzyskiwania.

- Klonowanie limit czasu zadania: Jeśli klonowanie wielkości trwa dłużej niż limit Odzyskiwanie witryny Azure na skrypt (obecnie 120 minut) limitu czasu skryptu StorSimple.

- Czas błąd synchronizacji: StorSimple skrypty błędy się informacją kopie zapasowe były nie powiedzie, mimo że kopia zapasowa zostanie przeprowadzone pomyślnie w portalu. Możliwe przyczyny to może być, że urządzenie StorSimple czasu może być zsynchronizowany z bieżącą godzinę w strefie czasowej.
 
    > [AZURE.IMPORTANT] Synchronizowanie czasu urządzenia z bieżącą godzinę w strefie czasowej.

- Błąd pracy awaryjnej urządzenia: skrypt StorSimple może zakończyć się niepowodzeniem, jeśli istnieje pracy awaryjnej urządzenia, gdy jest uruchomiony planu odzyskiwania.
    
    > [AZURE.IMPORTANT] Po zakończeniu tym przełączeniu urządzenie, uruchom ponownie planu odzyskiwania.

## <a name="summary"></a>Podsumowanie

Za pomocą Azure Odzyskiwanie witryny, możesz utworzyć planu odzyskiwania wykonania danych automatyczną serwera plików maszyn wirtualnych konieczności udziały plików obsługiwanych StorSimple ilość miejsca do magazynowania. Inicjowanie konwersacji tym przełączeniu w ciągu kilku sekund z dowolnego miejsca w przypadku przerwy w pracy i pobieranie aplikacji pracę w ciągu kilku minut.
