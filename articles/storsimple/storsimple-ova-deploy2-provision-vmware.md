<properties
   pageTitle="Wdrażanie StorSimple tablicy wirtualnych - postanowień VMware"
   description="Ten samouczek drugiego w tablicy Virtual StorSimple wdrożenia serii obejmuje inicjowania obsługi administracyjnej urządzenie wirtualne w VMware."
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
   ms.date="04/12/2016"
   ms.author="alkohli"/>


# <a name="deploy-storsimple-virtual-array---provision-a-virtual-array-in-vmware"></a>Wdrażanie wirtualnych tablicy StorSimple — obsługi administracyjnej wirtualnych macierzy w VMware

![](./media/storsimple-ova-deploy2-provision-vmware/vmware4.png)

## <a name="overview"></a>Omówienie 
Ten samouczek obsługi administracyjnej dotyczy wersji ogólnodostępną (GA) 2016 marca uruchomionego StorSimple tablic wirtualnych (nazywane także urządzeń wirtualnych lokalnego StorSimple lub StorSimple urządzeń wirtualnych). Ten samouczek opisano sposób obsługi administracyjnej i łączenie tablicy Virtual StorSimple na komputerze hosta z systemem VMware ESXi 5.5 i powyżej. Ten artykuł dotyczy wdrożenia StorSimple tablic wirtualnych w portal Azure klasyczny, a także Microsoft Azure Government Cloud.

Będzie mieć uprawnienia administratora, aby obsługa administracyjna i połącz urządzenie wirtualne. Ustawienia inicjowania obsługi administracyjnej i początkowej może potrwać około 10 minut do wykonania.


## <a name="provisioning-prerequisites"></a>Wymagania wstępne dotyczące obsługi administracyjnej

Tu znajdziesz wymagania wstępne dotyczące obsługi administracyjnej urządzenie wirtualne na komputerze hosta z systemem VMware ESXi 5.5 i powyżej.

### <a name="for-the-storsimple-manager-service"></a>W usłudze Menedżer StorSimple

Zanim rozpoczniesz, upewnij się, że:

-   Wykonano wszystkie kroki w [Przygotowywanie portal wirtualne tablicy StorSimple](storsimple-ova-deploy1-portal-prep.md).

-   Obraz urządzenia wirtualnego dla VMware pobrany z portalu Azure. Aby uzyskać więcej informacji, zobacz [Krok 3: Pobierz obraz urządzenia wirtualnego](storsimple-ova-deploy1-portal-prep.md#step-3-download-the-virtual-device-image).

### <a name="for-the-storsimple-virtual-device"></a>W przypadku urządzenia wirtualnego StorSimple 

Przed wdrożeniem urządzenie wirtualne, upewnij się, że:

-   Użytkownik ma dostęp do hosta z systemem funkcji Hyper-V (2008 R2 lub nowszy) który może być używany do postanowienia urządzenia.

-   System hosta jest w stanie przeznaczoną następujące zasoby do zapewniania obsługi wirtualnych urządzenia:

    -   Co najmniej 4 rdzenie.

    -   Co najmniej 8 GB pamięci RAM.

    -   Interfejs jednej sieci.

    -   500 GB dysku wirtualnego dla danych systemu.

### <a name="for-the-network-in-datacenter"></a>Dla sieci w centrum danych 

Zanim rozpoczniesz, upewnij się, że:

-   Masz przejrzeć wymagania sieciowe wdrożenia urządzenie wirtualne StorSimple i skonfigurować sieć centrum danych zgodnie z wymaganiami. Aby uzyskać więcej informacji zobacz [wymagania systemowe StorSimple wirtualnych tablicy](storsimple-ova-system-requirements.md).

## <a name="step-by-step-provisioning"></a>Krok po kroku inicjowania obsługi administracyjnej 

Aby obsługi administracyjnej i łączenie się z urządzeniem wirtualnych, należy wykonać następujące czynności:

1.  Upewnij się, że system hosta ma wystarczających zasobów, aby spełnić wymagania minimalne wirtualne urządzenie.

2.  Inicjowanie obsługi urządzenia wirtualnego w swojej monitor maszyny wirtualnej.

3.  Uruchom urządzenie wirtualne i uzyskać adres IP.

## <a name="step-1-ensure-host-system-meets-minimum-virtual-device-requirements"></a>Krok 1: Upewnij się, że system hosta spełnia wymagania minimalne urządzenia wirtualnego

Aby utworzyć urządzenie wirtualne, należy:

-   Dostęp do hosta z systemem VMware ESXi Server 5.5 oraz powyżej.

-   VMware vSphere klienta w systemie zarządzania hosta ESXi.

    -   Co najmniej 4 rdzenie.

    -   Co najmniej 8 GB pamięci RAM.

    -   Jeden interfejs sieciowy połączony z siecią do routingu ruchu internetowego. Minimalne przepustowości internetowej powinny być 5 MB/s, aby umożliwić optymalne pracy urządzenie.

    -   500 GB dysku wirtualnego dla danych.

## <a name="step-2-provision-a-virtual-device-in-hypervisor"></a>Krok 2: Inicjowanie obsługi urządzenia wirtualnego w monitor maszyny wirtualnej

Wykonaj poniższe czynności, aby zapewnić urządzenie wirtualne w swojej monitor maszyny wirtualnej.

1.  Skopiuj obraz urządzenia wirtualnego w systemie. To jest obraz, który został pobrany za pośrednictwem portalu klasyczny Azure. 
    1.  Upewnij się, że jest najnowsza pliku obrazu, który został pobrany. Jeśli obraz został pobrany wcześniej, pobierz go ponownie, aby upewnić się, że masz najnowszą obrazu. Najnowsze obraz ma dwa pliki (zamiast jednej).
    2.  Zanotuj lokalizacji, do którego skopiowano obraz jako będziesz używać tego w dalszej części procedury.

2.  Zaloguj się do serwera ESXi przy użyciu klienta vSphere. Konieczne będzie mieć uprawnienia administratora, aby utworzyć maszyny wirtualnej.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image1.png)

1.  W kliencie vSphere w sekcji zapasów w okienku po lewej stronie wybierz serwer ESXi.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image2.png)

1.  Zostanie najpierw przekazać VMDK na serwerze ESXi. Przejdź na kartę **Konfiguracja** w okienku po prawej stronie. W obszarze **sprzęt**wybierz **miejsca do magazynowania**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image3.png)

1.  W prawym okienku w obszarze **Datastores**wybierz miejsce, w którym chcesz przekazać VMDK magazynu danych. Magazynu danych musi mieć wystarczającą ilość miejsca na dyskach systemu operacyjnego i danych.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image4.png)

1.  Kliknij prawym przyciskiem myszy i wybierz pozycję **Przeglądaj magazynu danych**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image5.png)

1.  Zostanie wyświetlone okno **Przeglądarki magazynu danych** .

    ![](./media/storsimple-ova-deploy2-provision-vmware/image6.png)

1.  Na pasku narzędzi kliknij ![](./media/storsimple-ova-deploy2-provision-vmware/image7.png) ikonę, aby utworzyć nowy folder. Określ nazwę folderu i zapisz go. Ta nazwa folderu będzie używać później, podczas tworzenia maszyny wirtualnej (zalecane najlepiej). Kliknij **przycisk OK**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image8.png)

1.  Nowy folder zostanie wyświetlony w lewym okienku okna **Przeglądarki magazynu danych**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image9.png)

1.  Kliknij ikonę Upload ![](./media/storsimple-ova-deploy2-provision-vmware/image10.png) i wybierz pozycję **Przekaż plik**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image11.png)

1.  Teraz należy przeglądać i wskaż pobrane pliki VMDK. Będą dwa pliki. Wybierz plik do przekazania.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image12m.png)

1.  Kliknij przycisk **Otwórz**. Teraz rozpocznie przekazywania pliku VMDK do określonego magazynu danych. Może potrwać kilka minut plik do przekazania.


1.  Po zakończeniu przekazywania zostanie wyświetlona pliku w magazynie danych w folderze, który został utworzony. 

    ![](./media/storsimple-ova-deploy2-provision-vmware/image14.png)

    Teraz należy przekazać drugiego pliku VMDK z tym samym magazynem danych.

1.  Wróć do okna vSphere klienta. ESXi wybranego serwera kliknij prawym przyciskiem myszy i wybierz pozycję **Nowa maszyna wirtualna**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image15.png)

1.  Zostanie wyświetlone okno **Tworzenie nowych maszyn wirtualnych** . Na stronie **konfiguracji** wybierz opcję **niestandardowe** . Kliknij przycisk **Dalej**.
    ![](./media/storsimple-ova-deploy2-provision-vmware/image16.png)

2.  Na stronie **Nazwa i lokalizacja** Określ nazwę komputera wirtualnych. Ta nazwa powinna pasować nazwę folderu (zalecane najważniejsze wskazówki) określone wcześniej w kroku 8.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image17.png)

1.  Na stronie **Magazyn** wybierz magazynu danych, którego chcesz użyć do obsługi administracyjnej usługi maszyn wirtualnych.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image18.png)

1.  Na stronie **Wersji maszyn wirtualnych** wybierz **wersji maszyn wirtualnych: 8**. Należy zauważyć, że w wersji 8-11 są obsługiwane.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image19.png)

1.  Na stronie **System operacyjny gościa** wybierz pozycję **System operacyjny gościa** jako **systemu Windows**. Dla **wersji**z listy rozwijanej wybierz **systemu Microsoft Windows Server 2012 (wersja 64-bitowa)**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image20.png)

1.  Na stronie **procesory** Dostosuj **liczby wirtualnych sockets** i **Liczba rdzeni w jednym wirtualnych socket** tak, aby **Całkowita liczba rdzeni** jest 4 (lub więcej). Kliknij przycisk **Dalej**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image21.png)

1.  Na stronie **pamięci** Określ 8 GB (lub więcej) pamięci RAM. Kliknij przycisk **Dalej**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image22.png)

1.  Na stronie **sieci** Określ liczbę interfejsów sieciowych. Wymaganie minimalne jest jednego interfejsu sieciowego.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image23.png)

1.  Na stronie **Kontroler SCSI** Zaakceptuj domyślne **skojarzenia zabezpieczeń logiczny LSI kontrolerze**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image24.png)

1.  Na stronie **Wybierz dysk** wybierz pozycję **Użyj istniejącego dysku wirtualnego**. Kliknij przycisk **Dalej**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image25.png)

1.  Na stronie **Wybierz istniejący dysk** w obszarze **Ścieżka pliku dysku**, kliknij przycisk **Przeglądaj**. Spowoduje to otwarcie okna dialogowego **Przeglądanie Datastores** . Przejdź do lokalizacji, w której przekazane VMDK. Zostanie wyświetlona tylko jeden plik z magazynu danych jako dwa pliki, które początkowo przekazane zostały scalone. Zaznacz plik, a następnie kliknij **przycisk OK**. Kliknij przycisk **Dalej**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image26.png)

1.  Na stronie **Zaawansowane opcje** Zaakceptuj domyślne, a następnie kliknij przycisk **Dalej**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image27.png)

1.  Na stronie **gotowy do wykonania** Przejrzyj wszystkie ustawienia skojarzone z nowym komputerze wirtualnych. Sprawdzanie, **Edytuj ustawienia maszyn wirtualnych przed zakończeniem**. Kliknij przycisk **Kontynuuj**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image28.png)

1.  Na stronie **Właściwości maszyn wirtualnych** na karcie **sprzętu** Znajdź urządzenie. Wybierz **Nowy dysk twardy**. Kliknij przycisk **Dodaj**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image29.png)

1.  Spowoduje to wyświetlenie okna **Dodawanie sprzętu** . Na stronie **Typu urządzenia** w obszarze **Wybierz typ urządzenia, które chcesz dodać**wybierz **dysk twardy** , a następnie kliknij przycisk **Dalej**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image30.png)

1.  Na stronie **Wybierz dysk** wybierz pozycję **Utwórz nowy wirtualny dysk**. Kliknij przycisk **Dalej**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image31.png)

1.  Na stronie **Tworzenie dysku** Zmień **Rozmiar** 500 GB (lub więcej). W obszarze **Dysku inicjowania obsługi administracyjnej**zaznacz **Cienki świadczenia**. Kliknij przycisk **Dalej**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image32.png)

1.  Na stronie **Zaawansowane opcje** Zaakceptuj domyślne.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image33.png)

1.  Na stronie **gotowy do wykonania** Przejrzyj opcje dysku. Kliknij przycisk **Zakończ**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image34.png)

1.  Teraz zwróci do strony właściwości maszyn wirtualnych. Nowy dysk twardy jest dodawana do komputera wirtualnych. Kliknij przycisk **Zakończ**.
  
    ![](./media/storsimple-ova-deploy2-provision-vmware/image35.png)

2.  Z komputera wirtualnych zaznaczone w okienku po prawej stronie przejdź na kartę **Podsumowanie** . Przejrzyj ustawienia komputera wirtualnych.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image36.png)

Komputer wirtualnych teraz zainicjowano obsługę administracyjną. Następnym krokiem jest zasilania na tym komputerze i uzyskać adres IP.

## <a name="step-3-start-the-virtual-device-and-get-the-ip"></a>Krok 3: Rozpoczynanie wirtualne urządzenie i uzyskać IP

Wykonaj następujące czynności, aby uruchomić wirtualnego urządzenia i połączyć się z nim.

#### <a name="to-start-the-virtual-device"></a>Aby rozpocząć urządzenia wirtualnego

1.  Uruchom urządzenie wirtualne. W vSphere Menedżer konfiguracji w okienku po lewej stronie wybierz urządzenie, a następnie kliknij prawym przyciskiem myszy, aby wyświetlić menu kontekstowe. Wybierz pozycję **Power** , a następnie wybierz pozycję **Włącz**. Czy to power na tym komputerze wirtualnych. W dolnym okienku **Ostatnie zadania** klienta vSphere można wyświetlić stan.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image37.png)

1.  Zadania konfiguracyjne może potrwać kilka minut do wykonania. Po uruchomieniu urządzenia, przejdź na kartę **konsoli** . Wyślij Ctrl + Alt + Delete, aby zalogować się do urządzenia. Można również wskazać umieść kursor w oknie konsoli i naciśnij klawisze Ctrl + Alt + Insert. Użytkownik domyślny jest *StorSimpleAdmin* i hasło domyślne to *hasła1*.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image38.png)

1.  Ze względów bezpieczeństwa hasło administratora urządzenia wygasa przy pierwszym logowaniu. Możesz zostanie wyświetlony monit o zmianę hasła.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image39.png)

1.  Wprowadź hasło, która zawiera co najmniej 8 znaków. Hasło musi zawierać 3 z 4 następujące wymagania: wielkie litery, małe litery, liczbowe i znaki specjalne. Wprowadź ponownie hasło, aby je potwierdzić. Użytkownik będzie powiadamiany, że hasło uległa zmianie.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image40.png)

1.  Po pomyślnym zmianie hasła, może być Uruchom ponownie urządzenie wirtualne. Poczekaj, aż ponownego uruchomienia, aby zakończyć. Konsoli programu Windows PowerShell urządzenia mogą być wyświetlane razem z pasek postępu.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image41.png)

1.  Kroki od 6 do 8 mają zastosowanie tylko podczas uruchamiania systemu w środowisku nie DHCP. Jeśli pracujesz w środowisku DHCP, Pomiń te kroki i przejdź do kroku 9. Jeśli uruchomiono z urządzenia w środowisku DHCP nie zobaczysz poniższym ekranie. 

    ![](./media/storsimple-ova-deploy2-provision-vmware/image42m.png)

    Teraz należy skonfigurować sieć.

1.  Używanie `Get-HcsIpAddress` polecenia można wyświetlać listę interfejsów włączone na urządzeniu wirtualną. Jeśli urządzenie ma włączony interfejs jednej sieci, jest domyślna nazwa przypisane do tego interfejsu `Ethernet`.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image43m.png)

1.  Używanie `Set-HcsIpAddress` polecenia cmdlet, aby skonfigurować sieć. Poniżej przedstawiono przykład:


    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-ova-deploy2-provision-vmware/image44.png)

1.  Po zakończeniu początkowej konfiguracji i uruchomieniu urządzenia w górę, pojawi się tekst transparentu urządzenia. Zanotuj adres IP i adres URL wyświetlany w polu Tekst transparentu do zarządzania urządzeniem. Ten adres IP użyje nawiązać połączenie z interfejsu użytkownika urządzenia wirtualnych sieci web i wykonaj ustawienia lokalne i rejestracji.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image45.png)


1. (Opcjonalnie) W tym kroku należy wykonać tylko wtedy, gdy są wdrażanie urządzenia w chmurze dla instytucji rządowych. Teraz umożliwi tryb Stanów Zjednoczonych informacji przetwarzania FIPS (Federal Standard) na urządzeniu. Standard FIPS 140 definiuje cryptographic algorytmów zatwierdzone do użycia przez US Federal systemy komputerowe dla instytucji rządowych, dla ochrony ważnych danych.
    1. Aby włączyć tryb FIPS, uruchom następujące polecenie cmdlet:
        
        `Enter-HcsFIPSMode`

    2. Po włączeniu trybu FIPS tak, aby cryptographic reguły sprawdzania poprawności zostały zastosowane, uruchom ponownie urządzenie.

        > [AZURE.NOTE] Można włączyć lub wyłączyć tryb FIPS na urządzeniu. Naprzemienne urządzenia między trybem FIPS i innych niż FIPS nie jest obsługiwane.


Jeśli urządzenie nie spełnia wymagania minimalne konfiguracji, pojawi się komunikat o błędzie w polu Tekst transparentu (jak pokazano poniżej). Należy zmodyfikować konfigurację urządzenia, aby odpowiednie zasoby, aby spełnia minimalne wymagania. Następnie można uruchomić ponownie i połączyć się z urządzeniem. Wymagania minimalne konfiguracji w [Krok 1: Upewnij się, że system hosta spełnia wymagania minimalne urządzenia wirtualnego](#step-1-ensure-host-system-meets-minimum-virtual-device-requirements).

![](./media/storsimple-ova-deploy2-provision-vmware/image46.png)

Jeśli inny błąd jest buźkę podczas początkowej konfiguracji korzystanie z lokalnego interfejsu użytkownika w sieci web, zapoznaj się z następujące kolejki w [Zarządzanie macierzy Virtual StorSimple korzystanie z lokalnego interfejsu użytkownika w sieci web](storsimple-ova-web-ui-admin.md).

-   Uruchom testy diagnostyczne do [Rozwiązywanie problemów z instalacją interfejs użytkownika sieci web](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).

-   [Pakiet dziennika Generuj i wyświetlanie pliki dziennika](storsimple-ova-web-ui-admin.md#generate-a-log-package).

## <a name="next-steps"></a>Następne kroki

-   [Konfigurowanie macierzy Virtual StorSimple jako serwer plików](storsimple-ova-deploy3-fs-setup.md)

-   [Konfigurowanie macierzy Virtual StorSimple jako serwerem](storsimple-ova-deploy3-iscsi-setup.md)

