<properties 
   pageTitle="Konfigurowanie MPIO dla swojego urządzenia StorSimple | Microsoft Azure"
   description="W tym artykule opisano sposób konfigurowania wielościeżkowego wejścia/wyjścia (MPIO) dla danego urządzenia StorSimple połączony z systemem Windows Server 2012 R2 hosta."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="configure-multipath-io-for-your-storsimple-device"></a>Konfigurowanie wielościeżkowym dla swojego urządzenia StorSimple

Microsoft utworzono Obsługa funkcji wielościeżkowego wejścia/wyjścia (MPIO) w systemie Windows Server do pomocy kompilacji wysokiej dostępności, odporność na uszkodzenia SAN konfiguracji. MPIO używa nadmiarowe składniki ścieżek fizycznych-karty, kable i przełączniki — do tworzenia ścieżek logicznych między serwerem i na urządzeniu magazynującym. Po awarii składnika ścieżki logiczne kończy się niepowodzeniem, powodując logiczny wielu ścieżek używa ścieżki alternatywnej we/wy tak, aby aplikacje można nadal dostęp do danych. Ponadto w zależności od konfiguracji, MPIO również zwiększyć wydajność przez ponownie równoważenia obciążenia przez te ścieżki. Aby uzyskać więcej informacji zobacz [Omówienie MPIO](https://technet.microsoft.com/library/cc725907.aspx "Omówienie MPIO i funkcje").  

Dostępność wysoki rozwiązanie StorSimple MPIO należy skonfigurować na swoim urządzeniu StorSimple. Po zainstalowaniu MPIO na serwerach hosta z systemem Windows Server 2012 R2 serwery następnie przeszkadzają łącza, siecią lub awaria interfejsu. 

MPIO jest opcjonalną funkcją w systemie Windows Server, a nie jest zainstalowany domyślnie. Powinna zostać zainstalowana jako funkcji za pomocą Menedżera serwera. W tym temacie opisano działania, jakie należy wykonać, aby zainstalować i za pomocą funkcji MPIO na hoście z systemem Windows Server 2012 R2 i połączenie z urządzeniem fizycznie StorSimple.

>[AZURE.NOTE] **Ta procedura dotyczy tylko seria StorSimple 8000. MPIO nie jest obecnie obsługiwane urządzenia wirtualnego StorSimple.**

Należy wykonać następujące czynności, aby skonfigurować MPIO na urządzeniu StorSimple:

- Krok 1: Instalowanie MPIO na hoście systemu Windows Server

- Krok 2: Konfigurowanie MPIO do przedstawienia wielkości StorSimple

- Krok 3: StorSimple instalacji wielkości na hoście

- Krok 4: Konfigurowanie MPIO wysokiej dostępności i równoważenia obciążenia

W poniższych sekcjach omówiono wszystkich powyższe czynności.

## <a name="step-1-install-mpio-on-the-windows-server-host"></a>Krok 1: Instalowanie MPIO na hoście systemu Windows Server

Aby zainstalować tę funkcję na hoście systemu Windows Server, wykonaj poniższą procedurę.

#### <a name="to-install-mpio-on-the-host"></a>Aby zainstalować MPIO na hoście

1. Otwórz Menedżera serwera na hoście systemu Windows Server. Domyślnie Menedżer serwera zostanie uruchomiony, gdy członek grupy Administratorzy zaloguje się na komputerze z systemem Windows Server 2012 R2 lub Windows Server 2012. Jeśli Menedżer serwera nie jest jeszcze otwarta, kliknij pozycję **Start > Menedżer serwera**.
![Menedżer serwera](./media/storsimple-configure-mpio-windows-server/IC740997.png)
2. Kliknij pozycję **Menedżer serwera > pulpit nawigacyjny > Dodawanie ról i funkcji**. Zostanie uruchomiony Kreator **dodawania ról i funkcje** .
![Dodawanie ról i funkcji Kreator 1](./media/storsimple-configure-mpio-windows-server/IC740998.png)
3. W kreatorze **Dodawanie ról i funkcji** wykonaj następujące czynności:

    - Na stronie **przed rozpoczęciem** kliknij przycisk **Dalej**.
    - Na stronie **Wybierz typ instalacji** Zaakceptuj domyślne ustawienia instalacja **oparta na rolach lub funkcji** . Kliknij przycisk **Dalej**. ![Dodawanie ról i Kreatora funkcji 2](./media/storsimple-configure-mpio-windows-server/IC740999.png)
    - Na stronie **Wybierz serwer docelowy** wybierz **Wybierz serwer z puli serwera**. Należy można automatycznie wykryć serwera hosta. Kliknij przycisk **Dalej**.
    - Na stronie **Wybierz role serwerów** kliknij przycisk **Dalej**.
    - Na stronie **Wybieranie funkcji** wybierz **Wielościeżkowym**, a następnie kliknij przycisk **Dalej**. ![Dodawanie ról i Kreatora funkcji 5](./media/storsimple-configure-mpio-windows-server/IC741000.png)
    - Na stronie **Potwierdzanie opcji instalacji** Potwierdź wybór, a następnie wybierz pozycję **Uruchom ponownie docelowym serwerze automatycznie, jeśli jest to wymagane**, tak jak pokazano poniżej. Kliknij przycisk **Zainstaluj**. ![Dodawanie ról i Kreatora funkcji 8](./media/storsimple-configure-mpio-windows-server/IC741001.png)
    - Po zakończeniu instalacji, otrzymywać powiadomienia. Kliknij przycisk **Zamknij** , aby zamknąć kreatora. ![Dodawanie ról i Kreatora funkcji 9](./media/storsimple-configure-mpio-windows-server/IC741002.png)

## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>Krok 2: Konfigurowanie MPIO do przedstawienia wielkości StorSimple

MPIO musi być skonfigurowany do identyfikowania ilości StorSimple. Aby skonfigurować MPIO rozpoznanie wielkości StorSimple, wykonaj następujące czynności.

#### <a name="to-configure-mpio-for-storsimple-volumes"></a>Aby skonfigurować MPIO do przedstawienia wielkości StorSimple

1. Otwórz **MPIO konfiguracji**. Kliknij pozycję **Menedżer serwera > pulpit nawigacyjny > Narzędzia > MPIO**.

2. W oknie dialogowym **Właściwości MPIO** wybierz kartę **Wykrywanie obsługi wielu ścieżek** .

3. Wybierz pozycję **Dodaj obsługę urządzeń iSCSI**, a następnie kliknij przycisk **Dodaj**.  
![Właściwości MPIO wykrywania wielu ścieżek](./media/storsimple-configure-mpio-windows-server/IC741003.png)

4. Należy ponownie uruchomić serwer po wyświetleniu monitu.
5. W oknie dialogowym **Właściwości MPIO** kliknij kartę **Urządzenia MPIO** . Kliknij przycisk **Dodaj**.
    </br>![MPIO właściwości MPIO urządzeń](./media/storsimple-configure-mpio-windows-server/IC741004.png)
6. W oknie dialogowym **Dodawanie pomocy technicznej MPIO** w obszarze **Identyfikator sprzętu urządzenia**wprowadź numer seryjny urządzenia. Numer seryjny urządzenia można uzyskać dostęp do usługi Menedżera StorSimple i przechodzenie do **urządzeń > pulpitu nawigacyjnego**. Numer seryjny urządzenia jest wyświetlana w **Skrócie szybkie** okienko pulpitu nawigacyjnego urządzenia po prawej stronie.
    </br>![Dodawanie obsługi MPIO](./media/storsimple-configure-mpio-windows-server/IC741005.png)
7. Należy ponownie uruchomić serwer po wyświetleniu monitu.

## <a name="step-3-mount-storsimple-volumes-on-the-host"></a>Krok 3: StorSimple instalacji wielkości na hoście

Po skonfigurowaniu MPIO w systemie Windows Server woluminy utworzone na urządzeniu StorSimple można zainstalować, a następnie korzystać z MPIO dla nadmiarowości. Aby zainstalować woluminu, należy wykonać następujące czynności.

#### <a name="to-mount-volumes-on-the-host"></a>Aby zainstalować wielkości na hoście

1. Otwórz okno **Właściwości inicjator iSCSI** na hoście systemu Windows Server. Kliknij pozycję **Menedżer serwera > pulpit nawigacyjny > Narzędzia > inicjator iSCSI**.
2. W oknie dialogowym **iSCSI inicjator właściwości** kliknij kartę odnajdowanie, a następnie kliknij **Wykrywania portalu docelowej**.
3. W oknie dialogowym **Odkrywanie portalu docelowej** wykonaj następujące czynności:
    
    - Wprowadź adres IP portu danych urządzenia StorSimple (na przykład wprowadź dane 0).
    - Kliknij **przycisk OK** , aby powrócić do okna dialogowego **Właściwości inicjator iSCSI** .

    >[AZURE.IMPORTANT] **Jeśli korzystasz z sieci prywatnej połączeń iSCSI, wprowadź adres IP portu danych, który jest podłączony do sieci prywatnej.**

4. Powtórz kroki 2 i 3 dla drugiego sieciowej (na przykład 1 danych) na urządzeniu. Należy pamiętać, że tych interfejsów powinna być włączona na potrzeby iSCSI. Aby uzyskać więcej informacji na ten temat, zobacz [interfejsy Modyfikuj](storsimple-modify-device-config.md#modify-network-interfaces).
5. W oknie dialogowym **iSCSI inicjator właściwości** wybierz kartę **elementów docelowych** . Powinien zostać wyświetlony docelowej urządzenia StorSimple IQN obszarze **Wykryte obiekty docelowe**.
 ![Karta elementów docelowych właściwości inicjator iSCSI](./media/storsimple-configure-mpio-windows-server/IC741007.png)
6. Kliknij przycisk **Połącz** , aby ustanowić sesji iSCSI z urządzeniem StorSimple. Zostanie wyświetlone okno dialogowe **Łączenie do miejsca docelowego** .

7. W oknie dialogowym **Łączenie do miejsca docelowego** zaznacz pole wyboru **Włącz obsługę wielu ścieżek** . Kliknij pozycję **Zaawansowane**.

8. W oknie dialogowym **Ustawienia zaawansowane** wykonaj następujące czynności:                                       
    -    Na liście rozwijanej **Karty lokalnej** wybierz **Inicjator iSCSI firmy Microsoft**.
    -    Na liście rozwijanej **IP inicjator** wybierz adres IP hosta.
    -    Na liście **Docelowej portalu** IP listy rozwijanej wybierz IP interfejsu urządzenia.
    -    Kliknij **przycisk OK** , aby powrócić do okna dialogowego **Właściwości inicjator iSCSI** .

9. Kliknij pozycję **Właściwości**. W oknie dialogowym **Właściwości** kliknij przycisk **Dodaj sesję**.
10. W oknie dialogowym **Łączenie do miejsca docelowego** zaznacz pole wyboru **Włącz obsługę wielu ścieżek** . Kliknij pozycję **Zaawansowane**.
11. W oknie dialogowym **Ustawienia zaawansowane** :                                        
    -  Na liście rozwijanej **karty lokalnej** wybierz inicjator iSCSI firmy Microsoft.
    -  Na liście rozwijanej **IP inicjator** zaznacz adres IP odpowiadający host. W tym przypadku w jednym sieciowej na hoście łączysz dwa interfejsy na tym urządzeniu. W związku z tym ten interfejs jest taki sam jak dla pierwszej sesji.
    -  Na liście rozwijanej **Target IP portalu** zaznacz adres IP drugi interfejs danych włączone na tym urządzeniu.
    -  Kliknij **przycisk OK** , aby powrócić do okna dialogowego właściwości inicjator iSCSI. Drugiej sesji dodano do elementu docelowego.

12. Otwórz program **Zarządzanie komputerem** , przechodząc do **Menedżer serwera > pulpit nawigacyjny > Zarządzanie komputerem**. W okienku po lewej stronie kliknij **miejsca do magazynowania > Zarządzanie dysku**. Woluminy utworzone na urządzeniu StorSimple, które będą widoczne dla tego hosta będzie wyświetlany w obszarze **Zarządzanie dysku** jako nowe dyski.

13. Inicjowanie dysku i Utwórz nowy. W trakcie procesu format wybierz rozmiar bloku 64 KB.
![Zarządzanie dysku](./media/storsimple-configure-mpio-windows-server/IC741008.png)
14. W obszarze **Zarządzanie dysku**kliknij prawym przyciskiem myszy **dysku** i wybierz pozycję **Właściwości**.
15. W modelu StorSimple ### okno dialogowe **Właściwości urządzenia dysku wiele ścieżek** kliknij kartę **MPIO** .
![StorSimple 8100 DeviceProp wiele ścieżek dysku.](./media/storsimple-configure-mpio-windows-server/IC741009.png)

16. W sekcji **Nazwa DSM** kliknij pozycję **Szczegóły** i sprawdź, że parametry są ustawione domyślne parametry. Domyślne parametry są następujące:

    - Ścieżka Sprawdź okresu = 30
    - Liczba prób = 3
    - Chroniona nazwa pochodzenia usuwanie okresu = 20
    - Interwał ponawiania próby = 1
    - Sprawdź ścieżkę włączone = niezaznaczone.


>[AZURE.NOTE] **Nie należy modyfikować domyślne parametry.**

## <a name="step-4-configure-mpio-for-high-availability-and-load-balancing"></a>Krok 4: Konfigurowanie MPIO wysokiej dostępności i równoważenia obciążenia

Dla wiele ścieżek opartych na wysoką dostępność i równoważenia obciążenia, wiele sesji można ręcznie dodać Aby zadeklarować różnych ścieżek dostępne. Na przykład jeśli host ma dwa interfejsy połączony z SAN urządzenie ma dwa interfejsy połączony z SAN, a następnie potrzebne cztery sesje skonfigurowano permutacji właściwej ścieżki (tylko dwie sesje będą wymagane każdego danych i interfejsem hosta jest w innej podsieci IP, ale nie jest routingu).

>[AZURE.IMPORTANT] **Zaleca się, że użytkownik nie mieszać 1 GbE i 10 GbE interfejsów. Jeśli używasz dwóch interfejsów obu interfejsów powinna być identyczne typu.**

Poniższa procedura zawiera opis sposobu dodawania sesje po urządzenia StorSimple z dwóch interfejsów jest połączony z hostem przy użyciu dwóch interfejsów.

### <a name="to-configure-mpio-for-high-availability-and-load-balancing"></a>Aby skonfigurować MPIO wysoką dostępność i równoważenia obciążenia

1. Odnajdywanie docelowej: w oknie dialogowym **iSCSI inicjator właściwości** na karcie **odnajdowania** kliknij polecenie **Odkryj portalu**.
2. W oknie dialogowym **Łączenie do miejsca docelowego** wprowadź adres IP jednego z interfejsów sieciowych urządzenia.
3. Kliknij **przycisk OK** , aby powrócić do okna dialogowego **Właściwości inicjator iSCSI** .

4. W oknie dialogowym **iSCSI inicjator właściwości** wybierz kartę **elementów docelowych** , wyróżnij odkryte docelowej, a następnie kliknij **Połącz**. Zostanie wyświetlone okno dialogowe **Łączenie do miejsca docelowego** .

5. W oknie dialogowym **Łączenie do miejsca docelowego** :
    
    - Pozostaw domyślne wybranej wartości docelowej ustawienie dla **tego połączenia Dodaj** do listy ulubionych elementów docelowych. Spowoduje to urządzenie automatycznie próbę ponownego uruchomienia połączenia, przy każdym uruchomieniu tego komputera.
    - Zaznacz pole wyboru **Włącz obsługę wielu ścieżek** .
    - Kliknij pozycję **Zaawansowane**.

6. W oknie dialogowym **Ustawienia zaawansowane** :
    - Na liście rozwijanej **Karty lokalnej** wybierz **Inicjator iSCSI firmy Microsoft**.
    - Na liście rozwijanej **IP inicjator** wybierz adres IP hosta.
    - Na liście rozwijanej **Target IP portalu** zaznacz adres IP interfejsu danych włączone na tym urządzeniu.
    - Kliknij **przycisk OK** , aby powrócić do okna dialogowego właściwości inicjator iSCSI.

7. Kliknij pozycję **Właściwości**, a następnie w oknie dialogowym **Właściwości** kliknij pozycję **Dodaj sesji**.

8. W oknie dialogowym **Łączenie do miejsca docelowego** zaznacz pole wyboru **Włącz obsługę wielu ścieżek** , a następnie kliknij pozycję **Zaawansowane**.

9. W oknie dialogowym **Ustawienia zaawansowane** :
    1. Na liście rozwijanej **karty lokalnej** wybierz **Inicjator iSCSI firmy Microsoft**.
    2. Na liście rozwijanej **IP inicjator** zaznacz adres IP odpowiadający drugi interfejs na hoście.
    3. Na liście rozwijanej **Target IP portalu** zaznacz adres IP drugi interfejs danych włączone na tym urządzeniu.
    4. Kliknij **przycisk OK** , aby powrócić do okna dialogowego **Właściwości inicjator iSCSI** . Drugiej sesji zostało dodane do elementu docelowego.

10. Powtórz kroki 8-10, aby dodać dodatkowe sesje (ścieżki) do elementu docelowego. Z dwóch interfejsów na hoście i dwoma na tym urządzeniu możesz dodać sumę cztery sesji.

11. Po dodaniu odpowiedniej sesji (ścieżki), w oknie dialogowym **Właściwości inicjator iSCSI** , wybierz element docelowy, a następnie kliknij polecenie **Właściwości**. Na karcie Sesje okna dialogowego **Właściwości** Uwaga cztery sesji identyfikatorów, które odpowiadają Permutacje możliwe ścieżki. Aby anulować sesji, zaznacz pole wyboru obok pozycji identyfikator sesji, a następnie kliknij **Rozłącz**.

12. Aby wyświetlić urządzenia przedstawione w sesji, wybierz kartę **urządzenia** . Aby skonfigurować zasady MPIO dla wybranego urządzenia, kliknij pozycję **MPIO**. Zostanie wyświetlone okno dialogowe **Szczegóły urządzenia** . Na karcie **MPIO** można wybrać odpowiednie ustawienia **Zasad równoważenia obciążenia** . Możesz także wyświetlić typ ścieżki **aktywnej** lub **wstrzymania** .

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o [korzystaniu z usługi Menedżera StorSimple, aby zmodyfikować konfigurację urządzeń StorSimple](storsimple-modify-device-config.md).
 
