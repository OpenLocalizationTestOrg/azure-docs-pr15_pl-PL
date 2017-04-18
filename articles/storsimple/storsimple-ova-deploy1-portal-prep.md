<properties
   pageTitle="Wdrażanie StorSimple wirtualnych tablicy 1 - Przygotowanie portalu"
   description="Pierwszy samouczka, aby wdrażanie StorSimple tablicy wirtualnej obejmuje Przygotowywanie się do portalu"
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/24/2016"
   ms.author="alkohli"/>

# <a name="deploy-storsimple-virtual-array---prepare-the-portal"></a>Wdrażanie StorSimple tablicy wirtualnych — przygotowywanie portalu

![](./media/storsimple-ova-deploy1-portal-prep/getstarted4.png)

## <a name="overview"></a>Omówienie

Ten artykuł dotyczy usługi Microsoft Azure StorSimple wirtualnych tablicy (nazywane także urządzenia wirtualnego StorSimple lokalnego lub urządzenia wirtualnego StorSimple) uruchomionego 2016 marca ogólnodostępną (GA) wersji. Jest to pierwszego artykułu w serii samouczki wdrażania wymaganych do całkowicie wdrożenia wirtualnych macierzy jako serwera plików lub serwerem. Ten artykuł zawiera opis przygotowania wymaganych do tworzenia i konfigurowania usługi Menedżera StorSimple przed inicjowania obsługi administracyjnej wirtualnych tablicy. Ten artykuł jest również połączona się lista kontrolna konfiguracji wdrażania, a także konfiguracji wymagania wstępne.

Konieczne będzie uprawnienia administratora, aby ukończyć proces instalacji i konfiguracji. Zalecamy zapoznanie się lista kontrolna konfiguracji wdrażania przed rozpoczęciem. Przygotowanie portalu może potrwać mniej niż 10 minut.

Informacje zawarte w tym artykule dotyczą wdrażania StorSimple tablic wirtualnych w portal Azure klasyczny, a także Microsoft Azure Government Cloud.

### <a name="get-started"></a>Rozpoczynanie pracy

Przepływ wdrażania składa się z przygotowanie portalu, inicjowania obsługi administracyjnej tablicę wirtualnych w środowisku wirtualizowanym i zakończeniu instalacji. Aby rozpocząć pracę z wdrożenia tablicy Virtual StorSimple jako serwera plików lub serwerem, konieczne będzie dotyczyć następujące zasoby tabelaryczne (artykułów i klipy wideo).

#### <a name="deployment-articles"></a>Wdrażania

Zapoznaj się z następującymi artykułami w określonej kolejności wdrożenia macierzy Virtual StorSimple.

| **#** | **W tym kroku**                          | **Spowoduje to zrobić...**                                                         | **Za pomocą tych dokumentów.**|
|------|-------------------------------------------|--------------------------------------------------------------------------------|------------------------|
|1.   | **Konfigurowanie portalu klasyczny Azure**       | Tworzenie i konfigurowanie usługi Menedżera StorSimple przed inicjowania obsługi administracyjnej urządzenie wirtualne StorSimple.  |[Przygotowywanie portalu](storsimple-ova-deploy1-portal-prep.md)|
|2.   | **Inicjowanie obsługi wirtualnych tablicy**           | Funkcji Hyper-v obsługi administracyjnej i połącz urządzenie wirtualne StorSimple hosta systemu funkcji Hyper-V w systemie Windows Server 2012 R2, Windows Server 2012 lub Windows Server 2008 R2. <br></br> <br></br> Aby uzyskać VMware obsługi administracyjnej i łączenie się z StorSimple lokalnej wirtualną urządzeniem na komputerze hosta z systemem VMware ESXi 5.5 i powyżej.<br></br>| [Inicjowanie obsługi wirtualnych tablicy w funkcji Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) <br></br> <br></br> [Inicjowanie obsługi wirtualnych macierzy w VMware](storsimple-ova-deploy2-provision-vmware.md)|
|3.    | **Konfigurowanie wirtualnego tablicy**              | Dla serwera plików wykonywanie początkowej konfiguracji, rejestrowanie serwera plików StorSimple i ukończenia instalacji na urządzeniu. Następnie można dodawać udziałów SMB. <br></br> <br></br> Dla z serwerem wykonywanie początkowej konfiguracji, zarejestruj się z serwerem StorSimple i ukończenia instalacji na urządzeniu. Następnie można dodawać wielkości iSCSI.| [Konfigurowanie wirtualnego tablicy jako serwera plików](storsimple-ova-deploy3-fs-setup.md)<br></br> <br></br>[Konfigurowanie wirtualnego tablicy jako serwerem](storsimple-ova-deploy3-iscsi-setup.md)|

#### <a name="deployment-videos"></a>Klipy wideo wdrażania

| **Aby wykonać ten krok...** |  **Obejrzyj ten klip wideo.**|
|----------------|-------------|
| Instrukcje krok po kroku można rozpocząć pracę z tabeli wirtualnej StorSimple. | [Wprowadzenie do tablicy wirtualnych StorSimple](https://azure.microsoft.com/documentation/videos/get-started-with-the-storsimple-virtual-array/)|
| Instrukcje krok po kroku do zapewniania obsługi tablicę Virtual StorSimple w funkcji Hyper-V.|[Tworzenie macierzy wirtualnych StorSimple](https://azure.microsoft.com/documentation/videos/create-a-storsimple-virtual-array/) |
|Instrukcje krok po kroku, konfigurować i rejestrować tablicę wirtualnych StorSimple|[Konfigurowanie tablicę wirtualnych StorSimple](https://azure.microsoft.com/documentation/videos/configure-a-storsimple-virtual-array/)|
|Instrukcje krok po kroku, aby utworzyć akcji, wykonywanie kopii zapasowej akcji i przywracania danych w tablicę Virtual StorSimple pełnienia roli serwera plików|[Za pomocą tablicy wirtualnych StorSimple](https://azure.microsoft.com/documentation/videos/use-the-storsimple-virtual-array/)|
|Instrukcje krok po kroku awaryjnego i awarii odzyskiwania tablicę wirtualnych StorSimple|[Odzyskiwanie w tablicy wirtualnych StorSimple](https://azure.microsoft.com/documentation/videos/storsimple-virtual-array-disaster-recovery/)

Teraz można rozpocząć konfigurowanie portalu klasyczny Azure.

## <a name="configuration-checklist"></a>Lista kontrolna konfiguracji

Lista kontrolna konfiguracji zawiera informacje, które należy zebrać przed skonfigurowaniem oprogramowania na urządzeniu StorSimple. Przygotowywanie te informacje wyprzedzeniem pomoże usprawnienie procesu wdrażania urządzenia StorSimple w środowisku. Zależnie od tego, czy urządzenia wirtualnego StorSimple zostanie wdrożony jako serwera plików lub serwerem, konieczne będzie jedną z następujących list kontrolnych.

-   Pobierz [Lista kontrolna konfiguracji serwera StorSimple wirtualnych tablicy pliku](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayFileServerConfigurationChecklist.pdf).

-   Pobierz [iSCSI tablicy Virtual StorSimple Lista kontrolna konfiguracji serwera](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayiSCSIServerConfigurationChecklist.pdf).

## <a name="prerequisites"></a>Wymagania wstępne

Tu znajdziesz wymagania wstępne dotyczące konfiguracji usługi Menedżera StorSimple, urządzenia wirtualnego StorSimple i sieci centrum danych.

### <a name="for-the-storsimple-manager-service"></a>W usłudze Menedżer StorSimple

Zanim rozpoczniesz, upewnij się, że:

-   Masz konto Microsoft przy użyciu poświadczeń programu access.

-   Masz konto Microsoft Azure miejsca do magazynowania przy użyciu poświadczeń programu access.

-   Subskrypcji usługi Microsoft Azure powinna być włączona dla usługi Menedżera StorSimple.

### <a name="for-the-storsimple-virtual-device"></a>W przypadku urządzenia wirtualnego StorSimple

Przed wdrożeniem urządzenie wirtualne, upewnij się, że:

-   Użytkownik ma dostęp do hosta funkcji Hyper-V w systemie Windows Server 2008 R2 lub nowszy z systemem lub VMware (ESXi 5,5 lub nowszy), które mogą być używane do postanowienia urządzenia.

-   System hosta jest w stanie przeznaczoną następujące zasoby do zapewniania obsługi wirtualnych urządzenia:

    -   Co najmniej 4 rdzenie.

    -   Co najmniej 8 GB pamięci RAM.

    -   Interfejs jednej sieci.

    -   500 GB dysku wirtualnego dla danych systemu.

### <a name="for-the-datacenter-network"></a>Sieci centrum danych

Zanim rozpoczniesz, upewnij się, że:

-   Sieć w centrum danych jest skonfigurowana zgodnie z wymaganiami sieci dla swojego urządzenia StorSimple. Aby uzyskać więcej informacji zobacz [Wymagania systemowe tablicy Virtual StorSimple](storsimple-ova-system-requirements.md).

-   Urządzenie wirtualne StorSimple ma dedykowane 5 przepustowości internetowej MB/s (lub więcej) dostępne przez cały czas. Przepustowość nie powinien być współużytkowany z innych aplikacji.

## <a name="step-by-step-preparation"></a>Przygotowanie krok po kroku

Aby przygotować się do portalu usługi Menedżera StorSimple, wykonaj następujące instrukcje krok po kroku.

## <a name="step-1-create-a-new-service"></a>Krok 1: Tworzenie nowej usługi

Jedno wystąpienie usługi Menedżera StorSimple można zarządzać wielu urządzeń StorSimple 1200. Wykonaj poniższe czynności, aby utworzyć nowe wystąpienie usługi Menedżera StorSimple. Jeśli masz istniejącą usługę Menedżer StorSimple do zarządzania urządzeniami 1200, Pomiń ten krok i przejdź do [kroku2: uzyskiwanie klucza rejestru usługi](#step-2-get-the-service-registration-key).

[AZURE.INCLUDE [storsimple-ova-create-new-service](../../includes/storsimple-ova-create-new-service.md)]

> [AZURE.IMPORTANT]
>
> Jeśli nie została włączona automatyczne tworzenie konta miejsca do magazynowania z usługą, należy utworzyć co najmniej jedno konto miejsca do magazynowania, po pomyślnym utworzeniu usługi.
>

> - Jeśli nie utworzył automatycznie konto miejsca do magazynowania, przejdź do [konfiguracji nowego konta miejsca do magazynowania dla usługi](#optional-step-configure-a-new-storage-account-for-the-service) , aby uzyskać szczegółowe instrukcje.
>

> - Jeśli włączone automatyczne tworzenie konta miejsca do magazynowania, przejdź do [Krok 2: uzyskiwanie klucza rejestru usługi](#step-2-get-the-service-registration-key).


## <a name="step-2-get-the-service-registration-key"></a>Krok 2: Uzyskiwanie klucza rejestru usługi


Po usługa Menedżera StorSimple jest i rozpocząć pracę, może być konieczne uzyskanie klucza rejestru usługi. Ten klucz służy do rejestrowania i podłączyć urządzenie StorSimple z usługą.

W [portalu klasyczny Azure](https://manage.windowsazure.com/), należy wykonać następujące czynności.


[AZURE.INCLUDE [storsimple-ova-get-service-registration-key](../../includes/storsimple-ova-get-service-registration-key.md)]

> [AZURE.NOTE]
>
> Klucz rejestru usługi służy do rejestrowania wszystkich urządzeń Menedżera StorSimple, które chcesz zarejestrować z usługą Menedżera StorSimple.

## <a name="step-3-download-the-virtual-device-image"></a>Krok 3: Pobierz obraz urządzenia wirtualnego

Po uzyskaniu klucza rejestru usługi, należy pobrać obraz odpowiedniego urządzenia wirtualnego do zapewniania obsługi urządzenia wirtualnego w systemie hosta. Obrazy urządzenia wirtualnego są zależne od systemu operacyjnego, którą można pobrać na stronie Szybkie uruchamianie w portalu klasyczny Azure.

> [AZURE.IMPORTANT] Oprogramowanie na tablicy Virtual StorSimple można używać tylko w połączeniu z usługą Menedżera Storsimple.


W [portalu klasyczny Azure](https://manage.windowsazure.com/), należy wykonać następujące czynności.

#### <a name="to-get-the-virtual-device-image"></a>Aby uzyskać obraz urządzenia wirtualnego

1.  Na stronie **Menedżera StorSimple usługi** kliknij pozycję Usługa, która została utworzona. Zostanie otwarta strona **Początkowa szybkie** . (Można kliknąć ikonę szybki start ![](./media/storsimple-ova-deploy1-portal-prep/image8.png) uzyskać dostęp do strony **Szybki Start** w dowolnej chwili.)

1.  Kliknij łącze odpowiadające do obrazu, który chcesz pobrać z Microsoft Download Center. Pliki są około 4,8 GB.

    -   VHDX funkcji Hyper-v w systemie Windows Server 2012 lub nowszym

    -   Wirtualnego dysku twardego funkcji Hyper-v w systemie Windows Server 2008 R2 lub w nowszej wersji

    -   VMDK VMWare ESXi 5.5 lub nowszym

2.  Pobierz i rozpakuj plik na dysku lokalnym, co Zapisz miejsce, w którym znajduje się plik rozpakowane.

![ikona klipu wideo](./media/storsimple-ova-deploy1-portal-prep/video_icon.png) **wideo dostępne**

Obejrzyj klip wideo instrukcje krok po kroku można rozpocząć pracę z tabeli wirtualnej StorSimple.

> [AZURE.VIDEO get-started-with-the-storsimple-virtual-array]



## <a name="optional-step-configure-a-new-storage-account-for-the-service"></a>Krok opcjonalny: Konfigurowanie nowego konta miejsca do magazynowania w usłudze

To krok opcjonalny, który należy wykonać tylko wtedy, gdy nie została włączona automatyczne tworzenie konta miejsca do magazynowania z usługą.

Jeśli musisz utworzyć konto Azure miejsca do magazynowania w innym regionie, zobacz, [jak utworzyć konto miejsca do magazynowania](storage-create-storage-account.md#create-a-storage-account) instrukcje krok po kroku.

W [portalu klasyczny Azure](https://manage.windowsazure.com/) na stronie usługi Menedżera StorSimple dodać istniejące konto Microsoft Azure miejsca do magazynowania, należy wykonać następujące czynności.

#### <a name="to-add-a-storage-account"></a>Aby dodać konto miejsca do magazynowania

1.  W usłudze Menedżer StorSimple strona początkowa wybierz pozycję usługi i kliknij go dwukrotnie. Zostanie otwarta strona **Początkowa szybkie** . Zaznacz stronę **Konfigurowanie** .

2.  Kliknij **konto, dodawanie i edytowanie miejsca do magazynowania**. W oknie dialogowym **Dodawanie/edytowanie konta miejsca do magazynowania** wykonaj następujące czynności:

    1.  Kliknij przycisk **Dodaj nowy**.

    1.  Podaj nazwę dla Twojego konta miejsca do magazynowania.

    1.  Wprowadź podstawowy **Klawisz dostępu** do konta Microsoft Azure miejsca do magazynowania.

    1.  Wybierz **Tryb Enable SSL** , aby utworzyć bezpiecznego kanału komunikacji sieci między urządzeniem przenośnym a chmury. Wyczyść pole wyboru **Włącz SSL tryb** tylko wtedy, gdy pracujesz w chmurze prywatne.

    1.  Kliknij ikonę wyboru ![](./media/storsimple-ova-deploy1-portal-prep/image7.png). Po pomyślnym utworzeniu konta miejsca do magazynowania, otrzymasz powiadomienie.

        ![](./media/storsimple-ova-deploy1-portal-prep/image11.png)

1.  Konto nowo utworzonego magazynowania będą wyświetlane na stronie **Konfigurowanie** konta **miejsca do magazynowania**. Kliknij przycisk **Zapisz** zapisywania rachunku nowo utworzonego miejsca do magazynowania. Kliknij przycisk **OK** , gdy zostanie wyświetlony monit o potwierdzenie.


## <a name="next-step"></a>Następny krok

Następnym krokiem jest do zapewniania obsługi maszyny wirtualnej dla swojego urządzenia wirtualnego StorSimple. W zależności od używanego systemu operacyjnego hosta Zobacz szczegółowe instrukcje:

-   [Inicjowanie obsługi tablicę Virtual StorSimple w funkcji Hyper-V](storsimple-ova-deploy2-provision-hyperv.md)

-   [Inicjowanie obsługi tablicę Virtual StorSimple w VMware](storsimple-ova-deploy2-provision-vmware.md)
