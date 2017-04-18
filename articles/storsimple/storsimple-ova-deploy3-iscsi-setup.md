<properties 
   pageTitle="Konfiguracji serwera iSCSI tablicy Virtual StorSimple | Microsoft Azure"
   description="Opisano, jak wykonywać początkowej konfiguracji, zarejestruj się z serwerem StorSimple i ukończenia instalacji na urządzeniu."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="07/18/2016"
   ms.author="alkohli" />


# <a name="deploy-storsimple-virtual-array--set-up-your-virtual-device-as-an-iscsi-server"></a>Wdrażanie macierz wirtualnych StorSimple — Konfigurowanie urządzenia wirtualnego jako serwerem

![przebieg procesu konfiguracji iSCSI](./media/storsimple-ova-deploy3-iscsi-setup/iscsi4.png)

## <a name="overview"></a>Omówienie

Ten samouczek wdrożenia dotyczy wersji ogólnodostępną (GA) 2016 marca uruchomionego programu Microsoft Azure StorSimple wirtualnych tablicy (nazywane także StorSimple urządzenia wirtualnego lokalnego lub urządzenia wirtualnego StorSimple). Ten samouczek opisano, jak wykonywać początkowej konfiguracji, zarejestruj się z serwerem StorSimple, ukończenia instalacji na urządzeniu, a następnie utwórz, instalacji, zainicjować i formatować na serwerem swojego urządzenia wirtualnego StorSimple. Informacje o ustawieniach StorSimple w tym artykule dotyczy tylko programu tablic wirtualnych StorSimple. 

Procedury opisane tutaj potrwać około 30 minut na 1 godzinę do wykonania. Informacje zawarte w tym artykule dotyczy tylko programu tablic wirtualnych StorSimple.

## <a name="setup-prerequisites"></a>Wymagania wstępne dotyczące instalacji

Przed skonfigurowaniem i konfigurowanie urządzenia wirtualnego StorSimple, upewnij się, że:

- Masz obsługi administracyjnej urządzenie wirtualne i podłączonych do niej, zgodnie z opisem w [Wdrażanie StorSimple wirtualnych tablicy — Obsługa administracyjna wirtualnych tablicy w funkcji Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) lub [Wdrażanie StorSimple wirtualnych tablicy — Obsługa administracyjna wirtualnych macierzy w VMware](storsimple-ova-deploy2-provision-vmware.md).

- Masz klucz rejestru usługi przez usługę Menedżer StorSimple utworzone przez Ciebie do zarządzania urządzeniami virtual StorSimple. Aby uzyskać więcej informacji, zobacz **Krok 2: uzyskiwanie klucza rejestru usługi** w [Wdrażanie StorSimple wirtualnych tablicy - Przygotowywanie się do portalu](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key).

- Jeśli jest to urządzenie wirtualnych drugi lub późniejszy, które rejestrujesz się z istniejącą usługę Menedżer StorSimple, należy użyć klucza szyfrowania danych usługi. Ten klucz został wygenerowany po pierwszym urządzeniu został pomyślnie zarejestrowany z tej usługi. Utraconych ten klucz zobacz **Uzyskiwanie klucza szyfrowania danych usługi** [Użyj interfejs sieci Web do zarządzania macierzy Virtual StorSimple](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key).

## <a name="step-by-step-setup"></a>Ustawienia krok po kroku 

Instalowanie i konfigurowanie urządzenia wirtualnego StorSimple za pomocą następujących instrukcji krok po kroku:

-  [Krok 1: Kończenie ustawienia interfejsu użytkownika lokalnego sieci web i zarejestrować urządzenie](#step-1-complete-the-local-web-ui-setup-and-register-your-device)
-  [Krok 2: Ukończenia instalacji wymagane urządzenie](#step-2-complete-the-required-device-setup)
-  [Krok 3: Dodawanie głośności](#step-3-add-a-volume)
-  [Krok 4: Instalacji, zainicjować i formatowanie woluminu](#step-4-mount-initialize-and-format-a-volume)  

## <a name="step-1-complete-the-local-web-ui-setup-and-register-your-device"></a>Krok 1: Kończenie ustawienia interfejsu użytkownika lokalnego sieci web i zarejestrować urządzenie 

#### <a name="to-complete-the-setup-and-register-the-device"></a>Aby zakończyć konfigurację i zarejestrować urządzenie

1. Otwórz okno przeglądarki i łączenie interfejsu użytkownika w sieci Web, wpisując:

    `https://<ip-address of network interface>`

    Za pomocą adresu URL połączenia ułatwień wymienionych w poprzednim kroku. Zostanie wyświetlony komunikat o błędzie powiadomienie, że występuje problem z certyfikatem zabezpieczeń witryny sieci Web. Kliknij przycisk **Kontynuuj, aby ta strona sieci web**.

    ![Błąd certyfikatu zabezpieczeń](./media/storsimple-ova-deploy3-iscsi-setup/image3.png)

2. Zaloguj się do interfejsu użytkownika wirtualnego urządzenia w sieci web jako **StorSimpleAdmin**. Wprowadź hasło administratora urządzenia, które zostały zmienione w kroku 3: rozpoczynanie urządzenia wirtualnego w [Wdrażanie StorSimple wirtualnych tablicy — Obsługa administracyjna urządzenia wirtualnego w funkcji Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) lub [Wdrażanie StorSimple wirtualnych tablicy — Obsługa administracyjna urządzenie wirtualne w VMware](storsimple-ova-deploy2-provision-vmware.md).

    ![Strona logowania](./media/storsimple-ova-deploy3-iscsi-setup/image4.png)

3. Nastąpi przekierowanie do strony **głównej** . Na tej stronie opisano różne ustawienia wymagane do skonfigurowania i zarejestrować urządzenie wirtualne usłudze Menedżer StorSimple. Zauważ, że **sieci ustawienia**, **Ustawienia serwera proxy w sieci Web**i **Ustawienia czasu** są opcjonalne. Tylko ustawienia wymagane są **Ustawienia urządzenia** i **chmury**.

    ![Strona główna](./media/storsimple-ova-deploy3-iscsi-setup/image5.png)

4. Na stronie **ustawień sieci** w obszarze **interfejsy**danych 0 zostanie automatycznie skonfigurowany dla Ciebie. Każdy interfejs sieci jest domyślnie automatycznie uzyskać adres IP (DHCP). W związku z tym adres IP, podsieci i brama zostanie automatycznie przypisany (w przypadku IPv4 i IPv6).

    Podczas planowania wdrożenia urządzenia jako serwerem (Aby zainicjować obsługę magazynowania bloku), zaleca się wyłączyć opcję **automatycznie adres IP uzyskiwanie** i konfigurowanie adresów IP.

    ![Strona ustawień sieci](./media/storsimple-ova-deploy3-iscsi-setup/image6.png)

    Jeśli podczas inicjowania obsługi administracyjnej urządzenia dodano więcej niż jeden interfejs sieci, można je skonfigurować w tym miejscu. Należy zauważyć, że można skonfigurować usługi sieciowej jako IPv4 wyłącznie lub jako IPv4 i IPv6. Tylko konfiguracje protokołu IPv6 nie są obsługiwane.

5. Serwery DNS są wymagane, ponieważ są one używane, gdy urządzenie próbuje można komunikować się z dostawcami usług do chmury miejsca do magazynowania lub, aby rozwiązać urządzenia według nazwy, jeśli jest skonfigurowany jako serwer plików. Na stronie **ustawień sieci** **serwerów DNS**:

    1. Podstawowy i pomocniczy serwer DNS zostanie automatycznie skonfigurowany. Jeśli wybierzesz Konfigurowanie adresów IP, możesz określić serwery DNS. Wysoką dostępność zalecamy skonfigurowanie głównego i pomocniczego serwera DNS.

    2. Kliknij przycisk **Zastosuj**. To zastosuje i sprawdź poprawność ustawień sieci.

6. Na stronie **Ustawienia urządzenia** :

    1. Przypisz unikatową **nazwę** urządzenia. Ta nazwa może zawierać 1-15 znaków i może zawierać litery, cyfry i łączniki.

    2. Kliknij ikonę **serwerem** ![ikona serwera iSCSI](./media/storsimple-ova-deploy3-iscsi-setup/image7.png) dla danego **typu** urządzenia, które są tworzone. Serwerem pozwoli na obsługę blok magazynowania.

    3. Określ, czy mają być domeny to urządzenie. Jeśli urządzenie jest serwerem, dołączanie do domeny jest opcjonalna. Jeśli zdecydujesz się nie dołączaj do domeny z serwerem, kliknij przycisk **Zastosuj**, poczekaj, aż ustawienia można stosować, a następnie przejdź do następnego kroku.

        Jeśli chcesz dołączyć do urządzenia do domeny. Wprowadź **nazwę domeny**, a następnie kliknij przycisk **Zastosuj**.

        > [AZURE.NOTE] Jeśli z serwerem dołączania do domeny, upewnij się, virtual macierzy znajduje się w osobnym jednostka organizacyjna (OU) dla usługi Microsoft Azure Active Directory, a nie obiekty zasad grupy (GPO) są stosowane do niego.

    5. Zostanie wyświetlone okno dialogowe. Wprowadź swoje poświadczenia domeny w określonym formacie. Kliknij ikonę wyboru ![Zaznacz ikony](./media/storsimple-ova-deploy3-iscsi-setup/image15.png). Sprawdza się poświadczeń domeny. Zostanie wyświetlony komunikat o błędzie, jeśli poświadczenia są niepoprawne.

        ![poświadczenia](./media/storsimple-ova-deploy3-iscsi-setup/image8.png)

    6. Kliknij przycisk **Zastosuj**. To zastosuje i sprawdź poprawność ustawień urządzenia.
 
7. (Opcjonalnie) skonfiguruj serwer proxy sieci web. Mimo że konfiguracji serwera proxy dla sieci web jest opcjonalne, należy pamiętać, że jeśli korzystasz z serwera proxy sieci web, można skonfigurować tylko go w tym miejscu.

    ![Konfigurowanie serwera proxy sieci web](./media/storsimple-ova-deploy3-iscsi-setup/image9.png)

    Na stronie **sieci Web serwera proxy** :

    1. Podać **adres URL serwera proxy sieci Web** w następującym formacie: *http://host-IP adres* lub *numer FDQN:Port*. Należy zauważyć, że adresy URL HTTPS nie są obsługiwane.

    2. **Uwierzytelnianie** można określić jako **podstawowe** lub **Brak**.

    3. Jeśli korzystasz z uwierzytelniania, konieczne będzie o podanie **nazwy użytkownika** i **hasła**.

    4. Kliknij przycisk **Zastosuj**. To sprawdzić poprawność i zastosować ustawienia serwera proxy skonfigurowane sieci web.
 
8. (Opcjonalnie) skonfiguruj ustawienia czasu dla swojego urządzenia, takie jak strefa czasowa i serwerami NTP głównego i pomocniczego. Serwery NTP są wymagane, ponieważ urządzenia, należy zsynchronizować czasu, aby mógł uwierzytelnić z dostawcami usług do chmury.

    ![Ustawienia czasu](./media/storsimple-ova-deploy3-iscsi-setup/image10.png)

    Na stronie **Ustawienia czasu** :

    1. Z listy rozwijanej wybierz **strefę czasową** na podstawie lokalizacji geograficznej w jakiej zostanie wdrożony urządzenia. Domyślna strefa czasowa dla danego urządzenia jest pliku PST. Urządzenie będzie używać tej strefie czasowej wszystkie operacje według harmonogramu.

    2. Określanie **serwera NTP podstawowego** dla swojego urządzenia lub zaakceptuj wartość domyślną time.windows.com. Upewnij się, że sieć umożliwia NTP na przekazywanie ruchu z centrum danych do Internetu.

    3. Opcjonalnie określ **serwer NTP pomocnicza** dla danego urządzenia.

    4. Kliknij przycisk **Zastosuj**. To sprawdzić poprawność i stosowanie ustawień skonfigurowanym czasie.

9. Konfigurowanie ustawień chmury dla danego urządzenia. W tym kroku możesz zakończyć konfigurację urządzeniem lokalnym i następnie zarejestrować urządzenia z usługą Menedżera StorSimple.

    1. Wprowadzanie **klucza rejestru usługi** masz w **Krok 2: uzyskiwanie klucza rejestru usługi** w [Wdrażanie StorSimple wirtualnych tablicy - Przygotowywanie się do portalu](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key).

    2. Jest to pierwszy urządzenie, którego rejestrujesz się w tej usłudze, konieczne będzie podanie **klucza szyfrowania danych usługi**. Ten klawisz jest wymagane dla usługi klucza rejestru usługi rejestrowania dodatkowe urządzenia z usługą Menedżera StorSimple. Aby uzyskać więcej informacji można znaleźć uzyskanie [klucza szyfrowania danych usługi](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) lokalne sieci Web interfejsu użytkownika.

    3. Kliknij przycisk **Zarejestruj**. Uruchom ponownie urządzenie. Może być konieczne Czekaj na 2 – 3 minuty przed zarejestrowaniem pomyślnie urządzenia. Po ponownym uruchomieniu urządzenia nastąpi przekierowanie do strony logowania strony.

       ![Zarejestruj się w urządzeniu](./media/storsimple-ova-deploy3-iscsi-setup/image11.png)

10. Wróć do portalu klasyczny Azure. Na stronie **urządzeń** Sprawdź, że urządzenie pomyślnie jest połączony z usługą, wyświetlając stan. Stan urządzenia powinna być **aktywna**.

    ![Strona urządzenia](./media/storsimple-ova-deploy3-iscsi-setup/image12.png)

## <a name="step-2-complete-the-required-device-setup"></a>Krok 2: Ukończenia instalacji wymagane urządzenie

Aby ukończyć konfigurację urządzenia urządzenia StorSimple, należy:

- Wybierz konto miejsca do magazynowania chcesz skojarzyć urządzenia.

- Wybierz pozycję Ustawienia szyfrowania danych, które są wysyłane do chmury.

W portalu klasyczny Azure do ukończenia instalacji wymagane urządzenie, należy wykonać następujące czynności.

#### <a name="to-complete-the-minimum-device-setup"></a>Aby zakończyć konfigurację urządzeń

1. Na stronie **urządzeń** wybierz urządzenie, która została właśnie utworzona. To urządzenie będzie wyświetlany jako **aktywna**. Kliknij strzałkę obok nazwy urządzenia, a następnie kliknij przycisk **Szybkie uruchamianie**.

    ![Strona urządzenia](./media/storsimple-ova-deploy3-iscsi-setup/image13.png)

2. Kliknij pozycję **Ustawienia kompletne urządzenie** , aby uruchomić Kreatora konfiguracji urządzenia.

    ![Konfigurowanie urządzenia Kreatora](./media/storsimple-ova-deploy3-iscsi-setup/image14.png)

3. W Kreatorze konfiguracji urządzeń na stronie **Ustawienia podstawowe** wykonaj następujące czynności:

   1. Określ konto miejsca do magazynowania, może być używany z urządzeniem. W tej subskrypcji z listy rozwijanej można wybrać istniejące konto miejsca do magazynowania lub można określić **Dodaj więcej** , aby wybrać konta z innej subskrypcji.

   2. Definiowanie ustawienia szyfrowania dla wszystkich danych w pozostałych, które będzie wysyłane do chmury. (StorSimple używa szyfrowania AES-256). Aby zaszyfrować danych, zaznacz pole wyboru **Włącz chmury miejsca do magazynowania szyfrowania** . Wprowadź chmury szyfrowania miejsca do magazynowania, 32 znaki. Ponownie wprowadź klucz, aby je potwierdzić.

   3. Kliknij ikonę wyboru ![Zaznacz ikony](./media/storsimple-ova-deploy3-iscsi-setup/image15.png).

    ![Ustawienia podstawowe](./media/storsimple-ova-deploy3-iscsi-setup/image16.png)

    Ustawienia teraz zostaną zaktualizowane. Po pomyślnym aktualizacji ustawienia przycisk Ustawienia wykonane urządzenia będą niedostępne. Może zwrócić na stronę **Szybki Start** urządzenia.                                                        

>[AZURE.NOTE]Wszystkie inne ustawienia urządzenia można zmodyfikować w dowolnym momencie po zalogowaniu się do strony **konfiguracji** .

## <a name="step-3-add-a-volume"></a>Krok 3: Dodawanie głośności

W portalu klasyczny Azure do utworzenia woluminu, należy wykonać następujące czynności.

#### <a name="to-create-a-volume"></a>Do utworzenia woluminu

1. Na stronie **Szybkie uruchamianie** urządzenia kliknij przycisk **Dodaj woluminu**. Zostanie uruchomiony Kreator głośność Dodawanie.

2. W oknie Dodawanie kreatora głośność, w obszarze **Ustawienia podstawowe**wykonaj następujące czynności:

    1. Określ unikatową nazwę głośność. Nazwa musi być ciąg, który zawiera znaki 3 do 127.

    2. Podaj opis dla woluminu. Opis pomoże zidentyfikować właściciele głośność.

    3. Wybierz typ użycia dla woluminu. Typ użycia może być **Tiered głośność** lub **lokalnie przypięte głośności.** (**Głośność Tiered** to ustawienie domyślne). Dla obciążeń pracą wymagających lokalnego gwarancje, niska opóźnienia i większą wydajność wybierz **lokalnie przypięte** **Głośność**. Dla wszystkich innych danych wybierz **Tiered** **Głośność**.

        Lokalnie przypięty głośność grubo obsługi administracyjnej i gwarantuje, że dane podstawowe wielkości pozostaje na tym urządzeniu, a nie rozsypywać w chmurze. Jeśli tworzysz lokalnie przypięty głośność, urządzenie sprawdza wolnego miejsca na lokalne warstw do zapewniania obsługi głośności żądanym rozmiarze. Tworzenie woluminu lokalnie przypięty może wymagać przechodzeniu istniejące dane z urządzenia w chmurze, a czas potrzebny do tworzenia wielkość może być długa. Całkowity czas zależy od rozmiaru ustanawianie głośność, dostępna przepustowość sieci i danych na urządzeniu.

        Z drugiej strony warstwowych głośności znacznie obsługi administracyjnej i można tworzyć bardzo szybko. Po utworzeniu woluminu warstwowych około 10% obszaru jest włączona na warstwie lokalne i 90% obszaru jest obsługi administracyjnej w chmurze. Na przykład jeśli zainicjowano obsługę administracyjną głośność 1 TB, 100 GB czy znajdują się w lokalnej przestrzeni i 900 GB będzie używane w chmurze po poziomów danych. To z kolei oznacza to, że jeśli brakować lokalnej przestrzeni na urządzeniu, nie można dodawać warstwowych Udostępnij (ponieważ nie będą dostępne 10%).

    4. Określ wielkość ustanawianie dla głośność. Należy zauważyć, że określona pojemność powinien być mniejszy niż dostępne możliwości. Jeśli tworzysz warstwowych głośność rozmiar powinien być między 500 GB i 5 TB. Obrót lokalnie przypiętych Określ rozmiar woluminu między 50 GB i 500. Używanie możliwości dostępne jako przewodnik inicjowania obsługi administracyjnej woluminu. Jeśli pojemność lokalnym będzie 0 GB, użytkownik nie jest dozwolony do zapewniania obsługi lokalnie przypięty lub woluminu warstwowych.

        ![Ustawienia podstawowe](./media/storsimple-ova-deploy3-iscsi-setup/image17.png)

    5. Kliknij ikonę strzałki ![Ikona strzałki](./media/storsimple-ova-deploy3-iscsi-setup/image18.png) Aby przejść do następnej strony.

3. Na stronie **Dodatkowe ustawienia** Dodaj nowy rekord kontroli dostępu (awaryjnego):

    1. Wprowadź **nazwę** dla swojego awaryjnego.

    2. W obszarze **Nazwa inicjator iSCSI**, wprowadź iSCSI kwalifikowana nazwa (IQN) danego hosta systemu Windows. Jeśli nie masz IQN, przejdź do [dodatku o: Get IQN hosta Windows Server](#appendix-a-get-the-iqn-of-a-windows-server-host).

    3. Zaleca się włączenie kopii zapasowej domyślne, zaznaczając pole wyboru **Włącz kopii zapasowej domyślnych dla tego woluminu** . Wykonywanie kopii zapasowych domyślnej utworzy zasady, które wykonuje w 22:30 każdego dnia (urządzenia czas) i tworzy migawkę chmury tego woluminu.

        ![dodatkowe ustawienia](./media/storsimple-ova-deploy3-iscsi-setup/image19.png)

    4. Kliknij ikonę wyboru ![Zaznacz ikony](./media/storsimple-ova-deploy3-iscsi-setup/image15.png). Zostanie uruchomiony zadania tworzenia głośność. Pojawi się komunikat podobny do następującego.

        ![informacje o postępie wiadomości](./media/storsimple-ova-deploy3-iscsi-setup/image20.png)

        Głośność zostanie utworzony przy użyciu określonych ustawień. Domyślnie monitorowanie i kopia zapasowa będą dostępne dla głośność.

    5. Aby potwierdzić, że głośność został pomyślnie utworzony, przejdź do strony **wielkości** . Powinien zostać wyświetlony wielkość na liście.

        ![](./media/storsimple-ova-deploy3-iscsi-setup/image21.png)

## <a name="step-4-mount-initialize-and-format-a-volume"></a>Krok 4: Instalacji, zainicjować i formatowanie woluminu

Wykonaj poniższe czynności, aby zainstalować, zainicjować i formatować usługi StorSimple na hoście systemu Windows Server.

#### <a name="to-mount-initialize-and-format-a-volume"></a>Aby zainstalować, inicjowania i formatowanie woluminu

1. Rozpocznij inicjatorach firmy Microsoft.

2. W oknie **iSCSI inicjator właściwości** na karcie **odnajdowania** kliknij przycisk **Odkryj portalu**.

    ![Poznawanie portalu](./media/storsimple-ova-deploy3-iscsi-setup/image22.png)

3. W oknie dialogowym **Odkrywanie portalu docelowej** podanie adresu IP interfejsu sieci iSCSI, a następnie kliknij **przycisk OK**.

    ![Adres IP](./media/storsimple-ova-deploy3-iscsi-setup/image23.png)

4. W oknie **iSCSI inicjator właściwości** na karcie **cele** Znajdź **Discovered elementów docelowych**. (Każdego woluminu będą docelowej odkryte.) Stan urządzenia powinien być wyświetlany jako **nieaktywny**.

    ![wykrytych elementów docelowych](./media/storsimple-ova-deploy3-iscsi-setup/image24.png)

5. Wybierz urządzenia, a następnie kliknij przycisk **Połącz**. Po podłączeniu urządzenia zmienić stan na **połączony**. (Aby uzyskać więcej informacji o używaniu inicjatorach firmy Microsoft, zobacz [Instalowanie i konfigurowanie programu Microsoft inicjator iSCSI] [1].

    ![Wybierz urządzenia](./media/storsimple-ova-deploy3-iscsi-setup/image25.png)

6. Na hoście systemu Windows naciśnij klawisz Windows Logo + X, a następnie kliknij polecenie **Uruchom**.

7. W oknie dialogowym **Uruchamianie** wpisz **Diskmgmt.msc**. Kliknij przycisk **OK**, a zostanie wyświetlone okno dialogowe **Zarządzanie dysku** . Okienku po prawej stronie zostanie wyświetlona wielkość na hoście.

8. W oknie **Zarządzanie dysku** wielkość zainstalowanego pojawi, jak pokazano na poniższej ilustracji. Kliknij prawym przyciskiem myszy wielkość odkryte (kliknij nazwę dysku), a następnie kliknij pozycję **Online**.

    ![Zarządzanie dysku](./media/storsimple-ova-deploy3-iscsi-setup/image26.png)

9. Kliknij prawym przyciskiem myszy i wybierz pozycję **Inicjowania dysku**.

    ![Inicjowanie dysk 1](./media/storsimple-ova-deploy3-iscsi-setup/image27.png)

10. W oknie dialogowym Wybierz dyski do zainicjowania, a następnie kliknij **przycisk OK**.

    ![Inicjowanie dysku 2](./media/storsimple-ova-deploy3-iscsi-setup/image28.png)

11. Zostanie uruchomiony Kreator prostych nowy. Wybierz rozmiar dysku, a następnie kliknij przycisk **Dalej**.

    ![Kreator nowego woluminu 1](./media/storsimple-ova-deploy3-iscsi-setup/image29.png)

12. Przypisać literę do woluminu, a następnie kliknij przycisk **Dalej**.

    ![Kreator nowego woluminu 2](./media/storsimple-ova-deploy3-iscsi-setup/image30.png)

13. Wprowadź odpowiednie parametry, aby sformatować głośność. **W systemie Windows Server jest obsługiwana tylko NTFS.** Ustaw Australia 64 KB. Podać etykietę głośność. Jest zalecane dobrym rozwiązaniem dla tej nazwy być taka sama, jak nazwa woluminu podanych na urządzeniu virtual StorSimple. Kliknij przycisk **Dalej**.

    ![Kreator nowego woluminu 3](./media/storsimple-ova-deploy3-iscsi-setup/image31.png)

14. Sprawdź wartości głośność, a następnie kliknij przycisk **Zakończ**.

    ![Kreator nowego woluminu 4](./media/storsimple-ova-deploy3-iscsi-setup/image32.png)

    Wielkość będzie wyświetlany jako **Online** na stronie **Zarządzanie dysku** .

    ![ilości w trybie online](./media/storsimple-ova-deploy3-iscsi-setup/image33.png)

## <a name="next-steps"></a>Następne kroki

Informacje o sposobie korzystania z niego lokalne interfejsu użytkownika do [zarządzania macierzy Virtual StorSimple](storsimple-ova-web-ui-admin.md).

## <a name="appendix-a-get-the-iqn-of-a-windows-server-host"></a>Dodatek A: Pobierz IQN hosta Windows Server

Wykonaj następujące czynności, aby uzyskać iSCSI kwalifikowana nazwa (IQN) hosta systemu Windows z systemem Windows Server 2012.

#### <a name="to-get-the-iqn-of-a-windows-host"></a>Aby uzyskać IQN hosta systemu Windows

1. Rozpocznij inicjatorach Microsoft na hoście systemu Windows.

2. W oknie **iSCSI inicjator właściwości** na karcie **Konfiguracja** zaznacz i skopiuj parametry z pola **Nazwa inicjator** .

    ![właściwości inicjator iSCSI](./media/storsimple-ova-deploy3-iscsi-setup/image34.png)

2. Zapisz ten ciąg.

<!--Reference link-->
[1]: https://technet.microsoft.com/library/ee338480(WS.10).aspx



