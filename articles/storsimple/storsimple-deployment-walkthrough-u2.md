<properties 
   pageTitle="Wdrażanie urządzenia StorSimple (Aktualizuj 2) | Microsoft Azure"
   description="W tym artykule opisano czynności i najlepsze rozwiązania dotyczące rozmieszczania StorSimple aktualizacji 2 urządzeń i usług."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/24/2016"
   ms.author="alkohli" />

# <a name="deploy-your-on-premises-storsimple-device-update-2"></a>Wdrażanie urządzenia StorSimple lokalnego (Aktualizuj 2)

> [AZURE.SELECTOR]
- [Aktualizacja 2 i nowszy](../articles/storsimple/storsimple-deployment-walkthrough-u2.md)
- [Aktualizacja 1](../articles/storsimple/storsimple-deployment-walkthrough-u1.md)
- [GA wersji](../articles/storsimple/storsimple-deployment-walkthrough.md)

## <a name="overview"></a>Omówienie

Zapraszamy do wdrożenia urządzenia Microsoft Azure StorSimple. Następujące samouczki wdrażania dotyczą StorSimple 8000 serii aktualizacji 2. Tę serię samouczków zawiera lista kontrolna konfiguracji, wymagania wstępne dotyczące konfiguracji i konfiguracji szczegółowe instrukcje dotyczące urządzenia StorSimple.

Informacje znajdujące się w tych samouczkach założono, że masz przejrzeć ostrożności i rozpakowane, racked i kablem urządzenia StorSimple. Jeśli nadal potrzebujesz do tych zadań, Rozpocznij od przeglądania [ostrożności](storsimple-safety.md). Postępuj zgodnie instrukcjami określonego urządzenia Rozpakowywanie, stojaków instalacji i kabla urządzenia.

- [Rozpakowywanie, stojaków instalacji i kabla usługi 8100](storsimple-8100-hardware-installation.md)
- [Rozpakowywanie, stojaków instalacji i kabla usługi 8600](storsimple-8600-hardware-installation.md)

Konieczne będzie uprawnienia administratora, aby ukończyć proces instalacji i konfiguracji. Zalecamy zapoznanie się lista kontrolna konfiguracji przed rozpoczęciem. Procesu konfigurowania i wdrażania może zająć trochę czasu, aby zakończyć.

> [AZURE.NOTE] StorSimple informacje na temat wdrażania opublikowanych w witrynie sieci Web Microsoft Azure dotyczy tylko urządzenia serii StorSimple 8000. Aby uzyskać pełne informacje o urządzeniach serii 7000, przejdź do: [http://onlinehelp.storsimple.com/](http://onlinehelp.storsimple.com). Dla 7000 serii informacje na temat wdrażania zobacz [StorSimple System Przewodnik Szybki Start](http://onlinehelp.storsimple.com/111_Appliance/).

## <a name="deployment-steps"></a>Kroków wdrażania

Wykonaj poniższe czynności wymagane do skonfigurowania urządzenia StorSimple i łączenie się z usługą Menedżera StorSimple. Oprócz kroki wymagane są czynności opcjonalne i procedur, które może być konieczne podczas wdrażania. Instrukcje krok po kroku wdrażania wskazują, gdy należy wykonać te czynności opcjonalne.


| Krok                                                                                   | Opis                                                                                                                                                   |
|----------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **WYMAGANIA WSTĘPNE**                                                                      | Te muszą zostać ukończone w celu wdrożenia zbliżających przygotowania.                                                                                        |
| [Lista kontrolna konfiguracji wdrożenia](#deployment-configuration-checklist)                                                     | Użyj tej listy kontrolnej do zbierania i rejestrowania informacji przed i podczas wdrożenia.                                                                       |
| [Wymagania wstępne dotyczące rozmieszczania](#deployment-prerequisites)                                                               | Te Sprawdź poprawność środowiska jest gotowy do wdrożenia.                                                                                                     |
|                                                                                        |                                                                                                                                                               |
| **KROK PO KROKU WDRAŻANIA**                                                                   | Poniższe czynności są wymagane do wdrożenia urządzeniu StorSimple produkcji.                                                                                      |
| [Krok 1: Tworzenie nowej usługi](#step-1-create-a-new-service)                                                         | Konfigurowanie zarządzania chmury i miejsca do magazynowania dla swojego urządzenia StorSimple. *Pomiń ten krok, jeśli istniejąca usługa dla innych urządzeń StorSimple*.                |
| [Krok 2: Uzyskiwanie klucza rejestru usługi](#step-2-get-the-service-registration-key)                                               | Użyj tego klucza do rejestrowania i podłączyć urządzenie StorSimple za pomocą usługi zarządzania.                                                                         |
| [Krok 3: Konfigurowanie i zarejestrować urządzenie przy użyciu programu Windows PowerShell dla StorSimple](#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple)    | Podłącz urządzenie do sieci i zarejestrować ją w Azure do ukończenia instalacji przy użyciu usługi zarządzania.                                            |
| [Krok 4: Kończenie konfigurowania urządzeń](#step-4-complete-minimum-device-setupd)</br>[Opcjonalnie: Aktualizowanie urządzenia StorSimple](#scan-for-and-apply-updates)      | Aby zakończyć konfigurację urządzenia i Włącz jej do udostępniania miejsca do magazynowania za pomocą usługi zarządzania.                                                                      |
| [Krok 5: Tworzenie kontenera głośności](#step-5-create-a-volume-container)                                                      | Tworzenie kontenera do świadczenia wielkości. Kontener głośności ma konto miejsca do magazynowania, przepustowości i ustawienia szyfrowania dla wszystkich wielkość zawarte w niej.    |
| [Krok 6: Tworzenie woluminu](#step-6-create-a-volume)                                                                | Inicjowanie obsługi woluminów miejsca do magazynowania na urządzeniu StorSimple dla serwerów.                                                                                        |
| [Krok 7: Instalacji, zainicjować i formatowanie woluminu](#step-7-mount-initialize-and-format-a-volume)</br>[Opcjonalnie: Konfigurowanie MPIO](storsimple-configure-mpio-windows-server.md)            | Podłącz serwery do magazynu iSCSI dostarczony przez urządzenie. Opcjonalnie można skonfigurować MPIO, aby upewnić się, że serwery przeszkadzają łącza, sieci i interfejsu błąd.                                                                                                                                                              |
| [Krok 8: Sporządzanie kopii zapasowej](#step-8-take-a-backup)                                                                  | Konfigurowanie zasad kopii zapasowej do ochrony danych                                                                                                                 |
|                                                                                        |                                                                                                                                                               |
| **INNE PROCEDURY**                                                                   | Może być konieczne odwołują się do tych procedur, jak wdrażanie rozwiązania.                                                                                        |
| [Konfigurowanie nowego konta miejsca do magazynowania w usłudze](#configure-a-new-storage-account-for-the-service)                                      |                                                                                                                                                               |
| [Nawiązywanie połączenia z konsoli szeregowego urządzenia za pomocą Kit](#use-putty-to-connect-to-the-device-serial-console)                                    |                                                                                                                                                               |
| [Uzyskiwanie IQN hosta Windows Server](#get-the-iqn-of-a-windows-server-host)                                                   |                                                                                                                                                               |
| [Ręcznie utworzyć kopię zapasową](#create-a-manual-backup)                                                                 | 


## <a name="deployment-configuration-checklist"></a>Lista kontrolna konfiguracji wdrożenia

Przed wdrożeniem urządzenia, będzie konieczne zbieranie informacji w celu skonfigurowania oprogramowania na urządzeniu StorSimple. Przygotowywanie niektóre z tych informacji wyprzedzeniem pomoże usprawnienie procesu wdrażania urządzenia StorSimple w środowisku. Pobierz i zanotuj szczegóły konfiguracji, jak wdrożyć urządzenia za pomocą następującej listy kontrolnej.

- [Pobierz StorSimple Lista kontrolna konfiguracji wdrożenia](http://www.microsoft.com/download/details.aspx?id=49159)


## <a name="deployment-prerequisites"></a>Wymagania wstępne dotyczące rozmieszczania

W poniższych sekcjach opisano wymagania wstępne dotyczące konfiguracji usługi Menedżera StorSimple i urządzenia StorSimple.

### <a name="for-the-storsimple-manager-service"></a>W usłudze Menedżer StorSimple

Zanim rozpoczniesz, upewnij się, że:

- Masz konto Microsoft przy użyciu poświadczeń programu access.

- Masz konto Microsoft Azure miejsca do magazynowania przy użyciu poświadczeń programu access.

- Subskrypcji usługi Microsoft Azure włączono usługę Menedżer StorSimple. [Licencja Enterprise Agreement](https://azure.microsoft.com/pricing/enterprise-agreement/)należy zakupić subskrypcję.

- Masz dostęp do oprogramowania emulacji końcowych, takich jak Kit.

### <a name="for-the-device-in-the-datacenter"></a>W przypadku urządzenia w obrębie centrum danych

Przed rozpoczęciem konfigurowania urządzenia, upewnij się, że urządzenie jest w pełni rozpakowane, umieszczony na stojaków i pełni kablem power, sieci i szeregowego access zgodnie z opisem w:

-  [Rozpakowywanie, stojaków instalacji i kabla urządzenia 8100](storsimple-8100-hardware-installation.md)
-  [Rozpakowywanie, stojaków instalacji i kabla urządzenia 8600](storsimple-8600-hardware-installation.md)


### <a name="for-the-network-in-the-datacenter"></a>Sieci w obrębie centrum danych

Zanim rozpoczniesz, upewnij się, że:

- Porty w zaporze centrum danych są otwierane umożliwiający iSCSI i ruch zgodnie z opisem w [sieci wymagania dla swojego urządzenia StorSimple](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device)w chmurze.


## <a name="step-by-step-deployment"></a>Krok po kroku wdrażania

Wdrożenia urządzeniu StorSimple w obrębie centrum danych, wykonaj następujące instrukcje krok po kroku.

## <a name="step-1-create-a-new-service"></a>Krok 1: Tworzenie nowej usługi

Usługa Menedżera StorSimple można zarządzać wielu urządzeń StorSimple. Wykonaj poniższe czynności, aby utworzyć nowe wystąpienie usługi Menedżera StorSimple.

[AZURE.INCLUDE [storsimple-create-new-service](../../includes/storsimple-create-new-service.md)]

> [AZURE.IMPORTANT] Jeśli nie została włączona automatyczne tworzenie konta miejsca do magazynowania z usługą, należy utworzyć co najmniej jedno konto miejsca do magazynowania, po pomyślnym utworzeniu usługi. To konto miejsca do magazynowania zostanie użyty podczas tworzenia kontenera głośność. 
>
> * Jeśli nie utworzył automatycznie konto miejsca do magazynowania, przejdź do [konfiguracji nowego konta miejsca do magazynowania dla usługi](#configure-a-new-storage-account-for-the-service) , aby uzyskać szczegółowe instrukcje. 
> * Jeśli włączone automatyczne tworzenie konta miejsca do magazynowania, przejdź do [Krok 2: uzyskiwanie klucza rejestru usługi](#step-2-get-the-service-registration-key).

## <a name="step-2-get-the-service-registration-key"></a>Krok 2: Uzyskiwanie klucza rejestru usługi

Po usługa Menedżera StorSimple jest i rozpocząć pracę, może być konieczne uzyskanie klucza rejestru usługi. Ten klucz służy do rejestrowania i podłączyć urządzenie StorSimple z usługą.

W portalu zarządzania, należy wykonać następujące czynności.

[AZURE.INCLUDE [storsimple-get-service-registration-key](../../includes/storsimple-get-service-registration-key.md)]


## <a name="step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple"></a>Krok 3: Konfigurowanie i zarejestrować urządzenie przy użyciu programu Windows PowerShell dla StorSimple

Użyj programu Windows PowerShell dla StorSimple, aby ukończyć początkowej konfiguracji urządzenia StorSimple, zgodnie z opisem w poniższej procedurze. Należy wykonać ten krok, za pomocą oprogramowania do emulacji końcowych. Aby uzyskać więcej informacji zobacz [Używanie Kit nawiązywania połączenia z konsoli szeregowego urządzenia](#use-putty-to-connect-to-the-device-serial-console).

[AZURE.INCLUDE [storsimple-configure-and-register-device-u1](../../includes/storsimple-configure-and-register-device-u1.md)]

## <a name="step-4-complete-minimum-device-setup"></a>Krok 4: Kończenie konfigurowania urządzeń

Konfiguracja urządzenia minimalne urządzenia StorSimple jest wymagane: 

- Konfigurowanie pomocniczego serwera DNS.
- Włącz iSCSI na co najmniej jeden interfejs sieciowy.
- Przypisywanie stałe adresy IP do obu kontrolerów.

W portalu zarządzania w celu zakończenia konfiguracji urządzeń, należy wykonać następujące czynności.

[AZURE.INCLUDE [storsimple-complete-minimum-device-setup](../../includes/storsimple-complete-minimum-device-setup-u1.md)]

## <a name="step-5-create-a-volume-container"></a>Krok 5: Tworzenie kontenera głośności

Kontener głośności ma konto miejsca do magazynowania, przepustowości i ustawienia szyfrowania dla wszystkich wielkość zawarte w niej. Należy utworzyć kontener głośność przed rozpoczęciem inicjowania obsługi administracyjnej wielkości na urządzeniu StorSimple. 

W portalu zarządzania, aby utworzyć kontener głośności, należy wykonać następujące czynności.

[AZURE.INCLUDE [storsimple-create-volume-container](../../includes/storsimple-create-volume-container.md)]

## <a name="step-6-create-a-volume"></a>Krok 6: Tworzenie woluminu

Po utworzeniu kontenera głośności umożliwia obsługę woluminu magazynu na tym urządzeniu StorSimple dla serwerów. W portalu zarządzania do utworzenia woluminu, należy wykonać następujące czynności.

> [AZURE.IMPORTANT] Menedżer StorSimple może utworzyć zarówno zubożonym, jak i w pełni ustanawianie wielkości. Nie można jednak tworzyć częściowo ustanawianie wielkości. 

[AZURE.INCLUDE [storsimple-create-volume](../../includes/storsimple-create-volume-u2.md)]

## <a name="step-7-mount-initialize-and-format-a-volume"></a>Krok 7: Instalacji, zainicjować i formatowanie woluminu

Poniższe kroki są wykonywane na hoście systemu Windows Server. 


> [AZURE.IMPORTANT]

> - Wysoką dostępność rozwiązania StorSimple zaleca się skonfigurować MPIO na serwerach hosta (opcjonalnie) przed skonfigurowaniem iSCSI. Konfiguracja MPIO na serwerach hosta będziesz mieć pewność, że serwery przeszkadzają łącza, siecią lub awaria interfejsu.

> - MPIO i iSCSI instalacja i konfiguracja instrukcje dotyczące hosta Windows Server przejdź do [Skonfigurowania MPIO dla swojego urządzenia StorSimple](storsimple-configure-mpio-windows-server.md). Obejmuje to także kroki, aby zainstalować, zainicjować i formatować StorSimple.

> - MPIO i iSCSI instalacja i konfiguracja instrukcje dotyczące hosta Linux przejdź do [Skonfigurowania MPIO dla hosta StorSimple Linux](storsimple-configure-mpio-on-linux.md)

Jeśli jednak nie chcesz skonfigurować MPIO, wykonaj następujące czynności do instalacji, inicjowania i formatować usługi StorSimple na hoście systemu Windows Server.

[AZURE.INCLUDE [storsimple-mount-initialize-format-volume](../../includes/storsimple-mount-initialize-format-volume.md)]

## <a name="step-8-take-a-backup"></a>Krok 8: Sporządzanie kopii zapasowej

Wykonywanie kopii zapasowych zapewniają ochronę w chwili wielkości i poprawić odzyskiwanie danych minimalizując czas przywracania. Możesz wykonać dwa typy kopii zapasowych na urządzeniu StorSimple: lokalne migawek i migawek chmury. Każdy z tych typów kopii zapasowej może być **Zaplanowane** lub **ręcznie**. 

W portalu zarządzania, aby utworzyć kopię zapasową według harmonogramu, należy wykonać następujące czynności.

[AZURE.INCLUDE [storsimple-take-backup](../../includes/storsimple-take-backup.md)]

Możesz wykonać ręczne kopii zapasowej w dowolnym momencie. Aby uzyskać procedury przejdź do artykułu [Tworzenie ręczną kopię zapasową](#create-a-manual-backup). 

## <a name="configure-a-new-storage-account-for-the-service"></a>Konfigurowanie nowego konta miejsca do magazynowania w usłudze

To krok opcjonalny, który należy wykonać tylko wtedy, gdy nie została włączona automatyczne tworzenie konta miejsca do magazynowania z usługą. Aby utworzyć kontener głośność StorSimple jest wymagane konto Microsoft Azure miejsca do magazynowania.

Jeśli musisz utworzyć konto Azure miejsca do magazynowania w innym regionie, zobacz [Temat miejsca do magazynowania konta usługi Azure](../storage/storage-create-storage-account.md) instrukcje krok po kroku.

W portalu zarządzania na stronie **Menedżera StorSimple usługi** , należy wykonać następujące czynności.

[AZURE.INCLUDE [storsimple-configure-new-storage-account-u1](../../includes/storsimple-configure-new-storage-account-u1.md)]


## <a name="use-putty-to-connect-to-the-device-serial-console"></a>Nawiązywanie połączenia z konsoli szeregowego urządzenia za pomocą Kit

Aby połączyć się z programu Windows PowerShell dla StorSimple, musisz korzystać z oprogramowania emulacji końcowych, takich jak Kit. Podczas uzyskiwania dostępu do urządzenia bezpośrednio za pomocą konsoli szeregowego lub otwierając sesji telnet z komputera zdalnego, możesz użyć Kit.

[AZURE.INCLUDE [Use PuTTY to connect to the device serial console](../../includes/storsimple-use-putty.md)]


## <a name="scan-for-and-apply-updates"></a>Skanowanie w poszukiwaniu i zastosować aktualizacje

Aktualizowanie urządzenia może potrwać kilka godzin. Wykonaj poniższe czynności, aby odszukać i zastosować aktualizacje na urządzeniu.
<!--can take 1-4 hours--> 

<!--If you have a gateway configured on a network interface other than Data 0, you will need to disable Data 2 and Data 3 network interfaces before installing the update. Go to **Devices > Configure** and disable Data 2 and Data 3 interfaces. You should re-enable these interfaces after the device is updated.-->

#### <a name="to-update-your-device"></a>Aby zaktualizować urządzenia

1.  Na stronie **Szybkie uruchamianie** urządzenia kliknij pozycję **urządzenia**. Wybierz urządzenie, fizycznej, kliknij pozycję **Konserwacja** , a następnie kliknij **Skanowanie aktualizacji**.  

2.  Utworzono zadanie do skanowania w poszukiwaniu dostępnych aktualizacji. Jeśli są dostępne aktualizacje, **Skanowanie aktualizacji** zmienia się **Zainstaluj aktualizacje**. Kliknij pozycję **Zainstaluj aktualizacje**. 

3.  Zadanie aktualizacji zostanie utworzona. Monitorowanie stanu aktualizację, przechodząc do **zadania**.

    > [AZURE.NOTE] Po uruchomieniu zadanie aktualizacji natychmiast wyświetla stan jako 50 procent. Zmiany statusu do 100 procent dopiero po zakończeniu zadania aktualizacji. Istnieje nie stan w czasie rzeczywistym proces aktualizacji.

4.  Po pomyślnym zaktualizowaniu urządzenia, włączyć interfejsy danych 2 i 3 danych, jeśli zostały one wyłączone.

<!-- In step 2, you may be requested to disable Data 2 and Data 3 prior to installing the updates. You must disable these network interfaces or the updates may fail.-->

## <a name="get-the-iqn-of-a-windows-server-host"></a>Uzyskiwanie IQN hosta Windows Server

Wykonaj następujące czynności, aby uzyskać iSCSI kwalifikowana nazwa (IQN) hosta systemu Windows z systemem Windows Server® 2012.

[AZURE.INCLUDE [Create a manual backup](../../includes/storsimple-get-iqn.md)]

## <a name="create-a-manual-backup"></a>Ręcznie utworzyć kopię zapasową

W portalu zarządzania w celu utworzenia kopii zapasowej ręczne na żądanie dla jednego woluminu na urządzeniu StorSimple, należy wykonać następujące czynności.

[AZURE.INCLUDE [Create a manual backup](../../includes/storsimple-create-manual-backup.md)]


## <a name="next-steps"></a>Następne kroki

- Konfigurowanie [urządzenia wirtualnego](storsimple-virtual-device-u2.md).

- Za pomocą [Menedżera StorSimple usługi](storsimple-manager-service-administration.md) Zarządzanie Twoim urządzeniem StorSimple.
 
