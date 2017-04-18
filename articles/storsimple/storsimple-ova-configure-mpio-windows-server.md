<properties 
   pageTitle="Konfigurowanie MPIO na hoście wirtualnych tablicy StorSimple | Microsoft Azure"
   description="W tym artykule opisano sposób konfigurowania wielościeżkowego wejścia/wyjścia (MPIO) dla swojej StorSimple wirtualnych tablicy połączony z hostem z systemem Windows Server 2012 R2."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/04/2016"
   ms.author="alkohli" />

# <a name="configure-multipath-io-on-windows-server-host-for-the-storsimple-virtual-array"></a>Konfigurowanie wielościeżkowym na hoście systemu Windows Server w tabeli wirtualnych StorSimple

## <a name="overview"></a>Omówienie

Ten artykuł zawiera opis sposobu instalowania funkcji wielościeżkowym (MPIO) na hoście systemu Windows Server, stosowanie określonych ustawień konfiguracyjnych tylko do StorSimple ilości, a następnie sprawdź MPIO do przedstawienia wielkości StorSimple. W procedurze przyjęto, że macierzy wirtualnych StorSimple 1200 dwa interfejsy sieci jest podłączony do hosta Windows Server z dwóch interfejsów. Informacje zawarte w tym artykule dotyczy tylko wirtualnych tablicy. Aby uzyskać informacje na urządzeniach serii StorSimple 8000 przejdź do [Skonfigurowania MPIO StorSimple hosta](storsimple-configure-mpio-windows-server.md). 

Funkcja MPIO w Windows Server ułatwia tworzenie wysokiej dostępności, odporność na uszkodzenia magazynu konfiguracji. MPIO używa nadmiarowe składniki ścieżek fizycznych-karty, kable i przełączniki — do tworzenia ścieżek logicznych między serwerem i na urządzeniu magazynującym. Po awarii składnika ścieżki logiczne kończy się niepowodzeniem, powodując logiczny wielu ścieżek używa ścieżki alternatywnej we/wy tak, aby aplikacje można nadal dostęp do danych. Ponadto w zależności od konfiguracji, MPIO również zwiększyć wydajność przez ponownie równoważenia obciążenia przez te ścieżki. Aby uzyskać więcej informacji zobacz [Omówienie MPIO](https://technet.microsoft.com/library/cc725907.aspx "Omówienie MPIO i funkcje").  

Dostępność wysoki rozwiązanie StorSimple skonfiguruj MPIO na hosts Windows Server podłączonym do macierzy wirtualnych StorSimple 1200 (nazywane także urządzenie wirtualne lokalnego). Serwery hosta przeszkadzają następnie łącze, siecią lub awaria interfejsu. 

Należy wykonać następujące czynności, aby skonfigurować MPIO: 

- Wymagania wstępne dotyczące konfiguracji

- Krok 1: Instalowanie MPIO na hoście systemu Windows Server

- Krok 2: Konfigurowanie MPIO do przedstawienia wielkości StorSimple

- Krok 3: StorSimple instalacji wielkości na hoście

W poniższych sekcjach omówiono wszystkich powyższe czynności.


## <a name="prerequisites"></a>Wymagania wstępne

W tej sekcji opisano wymagania wstępne dotyczące konfiguracji hosta Windows Server i wirtualnych macierzy.

### <a name="on-windows-server-host"></a>Na hoście systemu Windows Server

-  Upewnij się, że hosta Windows Server ma 2 interfejsy włączone.


### <a name="on-storsimple-virtual-array"></a>Wirtualna tablicy StorSimple

- Wirtualna tablicy należy skonfigurować jako serwerem. Aby uzyskać więcej informacji, zobacz [Konfigurowanie tablic wirtualnych jako serwerem](storsimple-ova-deploy3-iscsi-setup.md). Jeden lub więcej interfejsów powinna być włączona na tablicy.   

- Interfejsy na wirtualnej macierzy powinny być dostępne na hoście systemu Windows Server.

- Jeden lub więcej ilości powinny być tworzone w macierzy Virtual StorSimple. Aby uzyskać więcej informacji, zobacz [Dodawanie woluminu](storsimple-ova-deploy3-iscsi-setup.md#step-3-add-a-volume) na macierzy wirtualnych StorSimple 1200. W tej procedurze utworzonych wielkości 3 (2 lokalnie przypięte i 1 warstwowych objętość jak pokazano poniżej) wirtualnej tablicy.
    
    ![mpio0](./media/storsimple-ova-configure-mpio-windows-server/mpio0.png)

### <a name="hardware-configuration-for-storsimple-virtual-array"></a>Konfiguracja sprzętu StorSimple wirtualnych tablicy

Na poniższym rysunku przedstawiono konfiguracji sprzętowej wysokiej dostępności i równoważenia obciążenia wielu ścieżek hosta Windows Server i StorSimple tablicy wirtualnej używane w tej procedurze.  

![Konfiguracja sprzętu MPIO](./media/storsimple-ova-configure-mpio-windows-server/1200hardwareconfig.png)

Jak pokazano na kolejnej ilustracji:

- Macierzy virtual StorSimple obsługi administracyjnej na funkcji Hyper-V jest urządzeniem active jeden węzeł skonfigurowanego jako serwerem.

- Dwa interfejsy wirtualną sieć są włączone w macierzy. W lokalnym web interfejsu użytkownika programu virtual tablicy 1200 upewnij się, że dwa interfejsy sieciowe są włączone, przechodząc do **Ustawień sieci** , tak jak pokazano poniżej:

    ![Interfejsy włączone 1200](./media/storsimple-ova-configure-mpio-windows-server/mpio9.png)
    
    Uwaga adresy IP protokołu IPv4 interfejsów sieciowych enabled (Ethernet, 2 Ethernet domyślnie) i zapisywanie do późniejszego użycia na hoście.

- Dwa interfejsy sieciowe są włączone na swoim hoście systemu Windows Server. Jeśli podłączonych interfejsów dla hosta i Tablica są w tej samej podsieci, będą ścieżek 4 dostępne. To miało miejsce w tej procedurze. Jednak w przypadku każdego interfejsu sieciowego w interfejsie tablicy i hosta w innej podsieci IP (i nie routingu), następnie tylko 2 ścieżki będą dostępne.

## <a name="step-1-install-mpio-on-the-windows-server-host"></a>Krok 1: Instalowanie MPIO na hoście systemu Windows Server

MPIO jest opcjonalną funkcją w systemie Windows Server, a nie jest zainstalowany domyślnie. Powinna zostać zainstalowana jako funkcji za pomocą Menedżera serwera. Aby zainstalować tę funkcję na hoście systemu Windows Server, wykonaj poniższą procedurę.

[AZURE.INCLUDE [storsimple-install-mpio-windows-server-host](../../includes/storsimple-install-mpio-windows-server-host.md)]


## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>Krok 2: Konfigurowanie MPIO do przedstawienia wielkości StorSimple

MPIO musi być skonfigurowany do identyfikowania ilości StorSimple. Aby skonfigurować MPIO rozpoznanie wielkości StorSimple, wykonaj następujące czynności.

[AZURE.INCLUDE [storsimple-configure-mpio-volumes](../../includes/storsimple-configure-mpio-volumes.md)]

## <a name="step-3-mount-storsimple-volumes-on-the-host"></a>Krok 3: StorSimple instalacji wielkości na hoście

Po skonfigurowaniu MPIO w systemie Windows Server woluminy utworzone na tablicy StorSimple można zainstalować, a następnie można sporządzanie zaletą MPIO nadmiarowości. Aby zainstalować woluminu, należy wykonać następujące czynności.

#### <a name="to-mount-volumes-on-the-host"></a>Aby zainstalować wielkości na hoście

1. Otwórz okno **Właściwości inicjator iSCSI** na hoście systemu Windows Server. Kliknij pozycję **Menedżer serwera > pulpit nawigacyjny > Narzędzia > inicjator iSCSI**.
2. W oknie dialogowym **iSCSI inicjator właściwości** kliknij kartę odnajdowanie, a następnie kliknij **Wykrywania portalu docelowej**.
3. W oknie dialogowym **Odkrywanie portalu docelowej** wykonaj następujące czynności:
    
    - Wprowadź adres IP pierwszego enabled sieciowej na macierzy virtual StorSimple. Domyślnie będzie **Ethernet**. 
    - Kliknij **przycisk OK** , aby powrócić do okna dialogowego **Właściwości inicjator iSCSI** .

    >[AZURE.IMPORTANT] **Jeśli korzystasz z sieci prywatnej połączeń iSCSI, wprowadź adres IP portu danych, który jest podłączony do sieci prywatnej.**

4. Powtórz kroki 2 i 3 dla drugiego sieciowej (na przykład Ethernet 2) w macierzy. 

5. W oknie dialogowym **iSCSI inicjator właściwości** wybierz kartę **elementów docelowych** . Każda powierzchnia głośność programu virtual tablicy powinny być widoczne jako obiekt docelowy w obszarze **Wykryte obiekty docelowe**. W tym przypadku czy wykryte trzech elementów docelowych (odpowiadające trzech wielkości).

    ![mpio1](./media/storsimple-ova-configure-mpio-windows-server/mpio1.png)

6. Kliknij przycisk **Połącz** , aby ustanowić sesji iSCSI z macierzy StorSimple. Zostanie wyświetlone okno dialogowe **Łączenie do miejsca docelowego** . Zaznacz pole wyboru **Włącz obsługę wielu ścieżek** . Kliknij pozycję **Zaawansowane**.

    ![mpio2](./media/storsimple-ova-configure-mpio-windows-server/mpio2.png)

8. W oknie dialogowym **Ustawienia zaawansowane** wykonaj następujące czynności:                                       
    -    Na liście rozwijanej **Karty lokalnej** wybierz **Inicjator iSCSI firmy Microsoft**.
    -    Na liście rozwijanej **IP inicjator** wybierz adres IP hosta.
    -    Na liście **Docelowej portalu** IP listy rozwijanej wybierz IP interfejsu tablicy.
    -    Kliknij **przycisk OK** , aby powrócić do okna dialogowego **Właściwości inicjator iSCSI** .

    ![mpio3](./media/storsimple-ova-configure-mpio-windows-server/mpio3.png)

9. Kliknij pozycję **Właściwości**. 

    ![mpio4](./media/storsimple-ova-configure-mpio-windows-server/mpio4.png)
10. W oknie dialogowym **Właściwości** kliknij przycisk **Dodaj sesję**.

    ![mpio5](./media/storsimple-ova-configure-mpio-windows-server/mpio5.png)

10. W oknie dialogowym **Łączenie do miejsca docelowego** zaznacz pole wyboru **Włącz obsługę wielu ścieżek** . Kliknij pozycję **Zaawansowane**.
11. W oknie dialogowym **Ustawienia zaawansowane** :                                        
    -  Na liście rozwijanej **karty lokalnej** wybierz inicjator iSCSI firmy Microsoft.
    -  Na liście rozwijanej **IP inicjator** zaznacz adres IP odpowiadający host. W tym przypadku w jednym sieciowej na hoście łączysz dwa interfejsy na tablicy. W związku z tym ten interfejs jest taki sam jak dla pierwszej sesji.
    -  Na liście rozwijanej **Target IP portalu** zaznacz adres IP drugi interfejs danych włączony w tablicy.
    -  Kliknij **przycisk OK** , aby powrócić do okna dialogowego właściwości inicjator iSCSI. Drugiej sesji dodano do elementu docelowego.

        ![mpio11](./media/storsimple-ova-configure-mpio-windows-server/mpio11.png)

    - Po dodaniu odpowiedniej sesji (ścieżki), w oknie dialogowym **Właściwości inicjator iSCSI** , wybierz element docelowy, a następnie kliknij polecenie **Właściwości**. Na karcie Sesje okna dialogowego **Właściwości** Uwaga cztery sesji identyfikatorów, które odpowiadają Permutacje możliwe ścieżki. Aby anulować sesji, zaznacz pole wyboru obok pozycji identyfikator sesji, a następnie kliknij **Rozłącz**.
 
    - Aby wyświetlić urządzenia przedstawione w sesji, wybierz kartę **urządzenia** . Aby skonfigurować zasady MPIO dla wybranego urządzenia, kliknij pozycję **MPIO**. **
    -  Szczegóły** zostanie wyświetlone okno dialogowe. Na **MPIO** Karta, możesz wybrać odpowiedni **zasady równoważenia obciążenia** Ustawienia. Możesz także wyświetlić **aktywny** lub **wpisz ścieżkę wstrzymania **.

10. Powtórz kroki 8-11, aby dodać dodatkowe sesje (ścieżki) do elementu docelowego. Z dwóch interfejsów na hoście i dwoma wirtualnych tablicy możesz dodać sumę cztery sesji dla każdego elementu docelowego. 

    ![mpio14](./media/storsimple-ova-configure-mpio-windows-server/mpio14.png)

11. Konieczne będzie Powtórz te czynności dla każdego woluminu (pobierający jako element docelowy).

    ![mpio15](./media/storsimple-ova-configure-mpio-windows-server/mpio15.png)

12. Otwórz program **Zarządzanie komputerem** , przechodząc do **Menedżer serwera > pulpit nawigacyjny > Zarządzanie komputerem**. W okienku po lewej stronie kliknij **miejsca do magazynowania > Zarządzanie dysku**. Woluminy utworzone na tablicy virtual StorSimple, których są widoczne dla tego hosta będzie wyświetlany w obszarze **Zarządzanie dysku** jako nowe dyski.

13. Inicjowanie dysku i Utwórz nowy. W trakcie procesu format wybierz rozmiar jednostki przydziału (Australia) 64 KB. Powtórz ten proces dla dostępne wielkość.

    ![Zarządzanie dysku](./media/storsimple-ova-configure-mpio-windows-server/mpio20.png)

14. W obszarze **Zarządzanie dysku**kliknij prawym przyciskiem myszy **dysku** i wybierz pozycję **Właściwości**.

15. W oknie dialogowym **Właściwości urządzenia dysku wiele ścieżek** kliknij kartę **MPIO** .

    ![Właściwości dysku](./media/storsimple-ova-configure-mpio-windows-server/mpio21.png)

16. W sekcji **Nazwa DSM** kliknij pozycję **Szczegóły** i sprawdź, że parametry są ustawione domyślne parametry. Domyślne parametry są następujące:

    - Ścieżka Sprawdź okresu = 30
    - Liczba prób = 3
    - Chroniona nazwa pochodzenia usuwanie okresu = 20
    - Interwał ponawiania próby = 1
    - Sprawdź ścieżkę włączone = niezaznaczone.

    >[AZURE.NOTE] **Nie należy modyfikować domyślne parametry.**


## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o [korzystaniu z usługi Menedżera StorSimple administrowania macierzy Virtual StorSimple](storsimple-ova-manager-service-administration.md).
 
