<properties
   pageTitle="Wdrażanie StorSimple tablicy wirtualnych — należy w funkcji Hyper-V"
   description="Ten samouczek drugiego w tablicy Virtual StorSimple wdrożenia obejmuje inicjowania obsługi administracyjnej urządzenia wirtualnego w funkcji Hyper-V."
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
   ms.date="10/11/2016"
   ms.author="alkohli"/>

# <a name="deploy-storsimple-virtual-array---provision-a-virtual-array-in-hyper-v"></a>Wdrażanie wirtualnych tablicy StorSimple — obsługi administracyjnej wirtualnych tablicy w funkcji Hyper-V

![](./media/storsimple-ova-deploy2-provision-hyperv/hyperv4.png)

## <a name="overview"></a>Omówienie

Ten samouczek obsługi administracyjnej dotyczy wersji ogólnodostępną (GA) 2016 marca uruchomionego Microsoft Azure StorSimple wirtualnych tablic (nazywane także urządzeń wirtualnych lokalnego StorSimple lub StorSimple urządzeń wirtualnych). Ten samouczek w tym artykule opisano sposób inicjowania obsługi tablicę Virtual StorSimple hosta systemu funkcji Hyper-V w systemie Windows Server 2012 R2, Windows Server 2012 lub Windows Server 2008 R2. Ten artykuł dotyczy wdrożenia StorSimple tablic wirtualnych w portal Azure klasyczny, a także Microsoft Azure Government Cloud.

Będzie mieć uprawnienia administratora, aby obsługa administracyjna i skonfiguruj urządzenie wirtualne. Ustawienia inicjowania obsługi administracyjnej i początkowej może potrwać około 10 minut do wykonania.


## <a name="provisioning-prerequisites"></a>Wymagania wstępne dotyczące obsługi administracyjnej

Tu znajdziesz wymagania wstępne dotyczące obsługi administracyjnej urządzenie wirtualne w hosta systemem funkcji Hyper-V w systemie Windows Server 2012 R2, Windows Server 2012 lub Windows Server 2008 R2.

### <a name="for-the-storsimple-manager-service"></a>W usłudze Menedżer StorSimple

Zanim rozpoczniesz, upewnij się, że:

-   Wykonano wszystkie kroki w [Przygotowywanie portal wirtualne tablicy StorSimple](storsimple-ova-deploy1-portal-prep.md).

-   Obraz urządzenia wirtualnego funkcji Hyper-v pobrany z portalu Azure. Aby uzyskać więcej informacji, zobacz [Krok 3: Pobierz obraz urządzenia wirtualnego](storsimple-ova-deploy1-portal-prep.md#step-3-download-the-virtual-device-image).

    > [AZURE.IMPORTANT] Oprogramowanie na tablicy Virtual StorSimple można używać tylko w połączeniu z usługą Menedżera Storsimple.

### <a name="for-the-storsimple-virtual-device"></a>W przypadku urządzenia wirtualnego StorSimple

Przed wdrożeniem urządzenie wirtualne, upewnij się, że:

-   Użytkownik ma dostęp do hosta z systemem funkcji Hyper-V w systemie Windows Server 2008 R2 lub nowszy, które mogą być używane do postanowienia urządzenia.

-   System hosta jest w stanie przeznaczoną następujące zasoby do zapewniania obsługi wirtualnych urządzenia:

    -   Co najmniej 4 rdzenie.

    -   Co najmniej 8 GB pamięci RAM.

    -   Interfejs jednej sieci.

    -   500 GB dysku wirtualnego dla danych systemu.

### <a name="for-the-network-in-the-datacenter"></a>Sieci w obrębie centrum danych

Przed rozpoczęciem należy zapoznać się z wymaganiami sieci wdrażać urządzenie wirtualne StorSimple i odpowiednio skonfigurować sieć centrum danych. Aby uzyskać więcej informacji zobacz [wymagania sieci StorSimple wirtualnych tablicy](storsimple-ova-system-requirements.md#networking-requirements).

## <a name="step-by-step-provisioning"></a>Krok po kroku inicjowania obsługi administracyjnej

Aby obsługi administracyjnej i łączenie się z urządzeniem wirtualnych, należy wykonać następujące czynności:

1.  Upewnij się, że system hosta ma wystarczających zasobów, aby spełnić wymagania minimalne wirtualne urządzenie.

2.  Inicjowanie obsługi urządzenia wirtualnego w swojej monitor maszyny wirtualnej.

3.  Uruchom urządzenie wirtualne i uzyskać adres IP.

Każde z tych kroków omówiono w poniższych sekcjach.

## <a name="step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements"></a>Krok 1: Upewnij się, że system hosta spełnia wymagania minimalne urządzenia wirtualnego

Aby utworzyć urządzenie wirtualne, należy:

-   Rola funkcji Hyper-V zainstalowany w systemie Windows Server 2012 R2, Windows Server 2012 lub Windows Server 2008 R2 z dodatkiem SP1.

-   Menedżer funkcji Hyper-V firmy Microsoft na komputerze klienckim Microsoft Windows połączony z hostem.

Należy się upewnić, że używanego sprzętu (system hosta), na którym tworzysz wirtualne urządzenie jest w stanie przeznaczoną do wirtualnego urządzenia następujące zasoby:

- Co najmniej 4 rdzenie.
- Co najmniej 8 GB pamięci RAM.
- Interfejs jednej sieci.
- 500 GB dysku wirtualnego dla danych systemu.

## <a name="step-2-provision-a-virtual-device-in-hypervisor"></a>Krok 2: Inicjowanie obsługi urządzenia wirtualnego w monitor maszyny wirtualnej

Wykonaj poniższe czynności, aby zapewnić urządzenia w swojej monitor maszyny wirtualnej.

#### <a name="to-provision-a-virtual-device"></a>Inicjowanie obsługi urządzenie wirtualne

1.  Na hoście systemu Windows Server skopiuj obraz urządzenia wirtualnego na dysk lokalny. Jest to obraz (wirtualnego dysku twardego lub VHDX), pobranego za pośrednictwem portalu Azure. Zanotuj lokalizacji, do którego skopiowano obraz jako będziesz używać tego w dalszej części procedury.

2.  Otwórz **Menedżera serwera**. W prawym górnym rogu kliknij pozycję **Narzędzia** , a następnie wybierz pozycję **Menedżer funkcji Hyper-V**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image1.png)

    Jeśli korzystasz z systemu Windows Server 2008 R2, otwórz Menedżera funkcji Hyper-V. Kliknij pozycję Menedżer serwera **ról > funkcji Hyper-V > Menedżer funkcji Hyper-V**.

1.  W oknie **Menedżer funkcji Hyper-V**w okienku zakres kliknij prawym przyciskiem myszy węzeł system, aby otworzyć menu kontekstowe, a następnie kliknij **Nowy** > **maszyn wirtualnych**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image2.png)

1.  Na stronie **przed rozpoczęciem** Kreatora nowej maszyny wirtualnej kliknij przycisk **Dalej**.

1.  Na stronie **Określ nazwę i lokalizację** podaj **nazwę** dla swojego urządzenia wirtualnego. Kliknij przycisk **Dalej**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image4.png)

1.  Na stronie **Określanie Generowanie** wybierz typ obrazu urządzenia, a następnie kliknij przycisk **Dalej**. Ta strona nie są wyświetlane, jeśli korzystasz z systemu Windows Server 2008 R2.

    * Wybierz wartość **2 Generowanie** Jeśli pobrany obraz .vhdx dla systemu Windows Server 2012 lub nowszym.
    * Wybierz wartość **1 Generowanie** Jeśli pobrany obrazu VHD dla systemu Windows Server 2008 R2 lub nowszym.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image5.png)

1.  Na stronie **Przypisywanie pamięci** określić **pamięci uruchamiania** co najmniej **8192 MB**, nie włączyć pamięci dynamicznej, a następnie kliknij przycisk **Dalej**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image6.png)

1.  Na stronie **Konfigurowanie sieci** określ wirtualny przełącznik, który jest połączony z Internetem, a następnie kliknij przycisk **Dalej**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image7.png)

1.  Na stronie **Połącz wirtualnego dysku twardego** wybierz pozycję **Użyj istniejącej wirtualny dysk twardy**, określ lokalizację obrazu urządzenia wirtualnego (.vhdx lub VHD) i kliknij przycisk **Dalej**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image8m.png)

1.  Zapoznaj się z **podsumowania** , a następnie kliknij przycisk **Zakończ** , aby utworzyć maszyny wirtualnej. Ale nie przejść, jeszcze — nadal trzeba dodać kilka rdzenie Procesora i drugi dysk. 

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image9.png)

1.  Aby spełnić wymagania minimalne, konieczne będzie rdzenie 4. Aby dodać procesory wirtualne z zaznaczonym w oknie **Menedżer funkcji Hyper-V** w prawym okienku w obszarze lista **maszyn wirtualnych**systemu hosta Znajdź maszyny wirtualnej właśnie utworzony. Zaznacz i kliknij prawym przyciskiem myszy nazwę komputera i wybierz pozycję **Ustawienia**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image10.png)

1.  Na stronie **Ustawienia** w lewym okienku kliknij opcję **procesora**. W prawym okienku ustaw **Liczba procesorów wirtualnych** 4 (lub więcej). Kliknij przycisk **Zastosuj**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image11.png)

1.  Aby spełnić wymagania minimalne, również musisz dodać dyskiem wirtualnych danych 500 GB. Na stronie **Ustawienia** :

    1.  W okienku po lewej stronie wybierz **Kontroler SCSI**.
    2.  W okienku po prawej stronie wybierz **Dysk twardy** , a następnie kliknij przycisk **Dodaj**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image12.png)

1.  Na stronie **dysk twardy** wybierz opcję **wirtualnych dysk twardy** , a następnie kliknij przycisk **Nowy**. Zostanie uruchomiony **Kreator nowego wirtualnego dysku twardego**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image13.png)

1.  Na stronie **przed rozpoczęciem** Kreatora nowego wirtualnego dysku twardego kliknij przycisk **Dalej**.

1.  Na **stronie Format dysku wybierz pozycję**Zaakceptuj domyślna opcja formatu **VHDX** . Kliknij przycisk **Dalej**. Jeśli korzystasz z systemu Windows Server 2012 R2 lub Windows Server 2008 R2 nie widzisz tego ekranu.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image15.png)

1.  Na **stronie wybierz typ dysku**, Ustaw typ wirtualnego dysku twardego jako **dynamiczne rozwijanie** (zalecane). Jeśli wybierzesz **Stały rozmiar** dysku, będą również działać, ale być może trzeba będzie zaczekać od dłuższego czasu. Zaleca się, że nie używają opcji **Differencing** . Kliknij przycisk **Dalej**. Należy zauważyć, że **dynamiczne rozwijanie** domyślnego w systemie Windows Server 2012 R2 i Windows Server 2012. W systemie Windows Server 2008 R2 wartość domyślna to **Stały rozmiar**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image16.png)

1.  Na stronie **Określ nazwę i lokalizację** wprowadź **nazwę** , a także **lokalizację** (można przejść do innej) dysku danych. Kliknij przycisk **Dalej**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image17.png)

1.  Na stronie **Konfigurowanie dysku** wybierz opcję **Utwórz nowy pusty wirtualnego dysku twardego** i określić rozmiar jako **500 GB** (lub więcej). 500 GB jest wymaganie minimalne, możesz zawsze obsługi administracyjnej większy dysk. Uwaga nie można rozwinąć lub Zmniejsz dysku raz obsługi administracyjnej. Aby uzyskać więcej informacji na podstawie rozmiaru dysku udzielenia zapoznaj się z sekcją zmiany rozmiaru w dokumencie [najlepsze rozwiązania](storsimple-ova-best-practices.md#configuration-best-practices) . Kliknij przycisk **Dalej**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image18.png)

1.  Na stronie **Podsumowanie** Przejrzyj szczegóły dysku wirtualnego danych, a jeśli spełnione, kliknij przycisk **Zakończ** , aby utworzyć dysk. Kreator zostanie zamknięty i wirtualny dysk twardy, zostaną dodane na komputerze.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image19.png)

2.  Może zwrócić do strony **ustawień** . Kliknij **przycisk OK** , aby zamknąć stronę **Ustawienia** i powrócić do okna Menedżera funkcji Hyper-V.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image20.png)

## <a name="step-3-start-the-virtual-device-and-get-the-ip"></a>Krok 3: Rozpoczynanie wirtualne urządzenie i uzyskać IP

Wykonaj następujące czynności, aby uruchomić wirtualnego urządzenia i połączyć się z nim.

#### <a name="to-start-the-virtual-device"></a>Aby rozpocząć urządzenia wirtualnego

1.  Uruchom urządzenie wirtualne.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image21.png)

1.  Po uruchomieniu urządzenia wybierz urządzenie, kliknij prawym przyciskiem myszy, a następnie zaznacz pole wyboru **Połącz**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image22.png)

1.  Być może musisz zaczekać 5-10 minut w przypadku urządzenia będzie gotowa. Komunikat o stanie jest wyświetlany na konsoli w celu wskazania postępu. Gdy urządzenie jest gotowe, przejdź do **akcji**. Naciśnij klawisz `Ctrl + Alt + Delete` do zalogowania się do urządzenia wirtualnego. Użytkownik domyślny jest *StorSimpleAdmin* i hasło domyślne to *hasła1*.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image23.png)

1.  Ze względów bezpieczeństwa hasło administratora urządzenia wygasa przy pierwszym logowaniu. Możesz zostanie wyświetlony monit o zmianę hasła.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image24.png)

    Wprowadź hasło, która zawiera co najmniej 8 znaków. Hasło musi spełniać wymagania co najmniej 3 poza następujące wymagania 4: wielkie litery, małe litery, liczbowe i znaki specjalne. Wprowadź ponownie hasło, aby je potwierdzić. Użytkownik będzie powiadamiany, że hasło uległa zmianie.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image25.png)

1.  Po pomyślnym zmianie hasła, może być Uruchom ponownie urządzenie wirtualne. Czekaj na urządzeniu rozpocząć.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image26.png)

    Konsoli programu Windows PowerShell urządzenia będą wyświetlane razem z pasek postępu.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image27.png)

1.  Kroki od 6 do 8 mają zastosowanie tylko podczas uruchamiania systemu w środowisku nie DHCP. Jeśli pracujesz w środowisku DHCP, Pomiń te kroki i przejdź do kroku 9. Jeśli uruchomiono z urządzenia w środowisku DHCP nie zobaczysz poniższym ekranie.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image28m.png)

    Teraz należy skonfigurować sieć.

1.  Używanie `Get-HcsIpAddress` polecenia można wyświetlać listę interfejsów włączone na urządzeniu wirtualną. Jeśli urządzenie ma włączony interfejs jednej sieci, jest domyślna nazwa przypisane do tego interfejsu `Ethernet`.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image29m.png)

1.  Używanie `Set-HcsIpAddress` polecenia cmdlet, aby skonfigurować sieć. Poniżej przedstawiono przykład:

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image30.png)

1.  Po zakończeniu początkowej konfiguracji i uruchomieniu urządzenia w górę, pojawi się tekst transparentu urządzenia. Zanotuj adres IP i adres URL wyświetlany w polu Tekst transparentu do zarządzania urządzeniem. Ten adres IP użyje nawiązać połączenie z interfejsu użytkownika urządzenia wirtualnych sieci web i wykonaj ustawienia lokalne i rejestracji.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image31m.png)



1. (Opcjonalnie) W tym kroku należy wykonać tylko wtedy, gdy są wdrażanie urządzenia w chmurze dla instytucji rządowych. Teraz umożliwi tryb Stanów Zjednoczonych informacji przetwarzania FIPS (Federal Standard) na urządzeniu. Standard FIPS 140 definiuje cryptographic algorytmów zatwierdzone do użycia przez US Federal systemy komputerowe dla instytucji rządowych, dla ochrony ważnych danych.
    1. Aby włączyć tryb FIPS, uruchom następujące polecenie cmdlet:

        `Enter-HcsFIPSMode`

    2. Po włączeniu trybu FIPS tak, aby cryptographic reguły sprawdzania poprawności zostały zastosowane, uruchom ponownie urządzenie.

        > [AZURE.NOTE] Można włączyć lub wyłączyć tryb FIPS na urządzeniu. Naprzemienne urządzenia między trybem FIPS i innych niż FIPS nie jest obsługiwane.

Jeśli urządzenie nie spełnia wymagania minimalne konfiguracji, pojawi się komunikat o błędzie w polu Tekst transparentu (jak pokazano poniżej). Należy zmodyfikować konfigurację urządzenia, aby odpowiednie zasoby, aby spełnia minimalne wymagania. Następnie można uruchomić ponownie i połączyć się z urządzeniem. Wymagania minimalne konfiguracji w [Krok 1: Upewnij się, że system hosta spełnia wymagania minimalne urządzenia wirtualnego](#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements).

![](./media/storsimple-ova-deploy2-provision-hyperv/image32.png)

Jeśli inny błąd jest buźkę podczas początkowej konfiguracji korzystanie z lokalnego interfejsu użytkownika w sieci web, zapoznaj się z następujące kolejki w [Zarządzanie macierzy Virtual StorSimple korzystanie z lokalnego interfejsu użytkownika w sieci web](storsimple-ova-web-ui-admin.md).

-   Uruchom testy diagnostyczne do [Rozwiązywanie problemów z instalacją interfejs użytkownika sieci web](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).

-   [Pakiet dziennika Generuj i wyświetlanie pliki dziennika](storsimple-ova-web-ui-admin.md#generate-a-log-package).

![ikona klipu wideo](./media/storsimple-ova-deploy2-provision-hyperv/video_icon.png)  **wideo dostępne**

Obejrzyj klip wideo, aby zobaczyć, jak umożliwia obsługę tablicę Virtual StorSimple w funkcji Hyper-V.

> [AZURE.VIDEO create-a-storsimple-virtual-array]

## <a name="next-steps"></a>Następne kroki

-   [Konfigurowanie macierzy Virtual StorSimple jako serwer plików](storsimple-ova-deploy3-fs-setup.md)

-   [Konfigurowanie macierzy Virtual StorSimple jako serwerem](storsimple-ova-deploy3-iscsi-setup.md)
