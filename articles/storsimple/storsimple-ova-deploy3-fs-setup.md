<properties
   pageTitle="Wdrażanie StorSimple wirtualnych tablicy 3 — Konfigurowanie urządzenia wirtualnego jako serwera plików"
   description="Ten samouczek trzecim w tablicy Virtual StorSimple wdrożenia powoduje, że Konfigurowanie urządzenia wirtualnego jako serwer plików."
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
   ms.date="05/26/2016"
   ms.author="alkohli"/>

# <a name="deploy-storsimple-virtual-array---set-up-as-file-server"></a>Wdrażanie StorSimple tablicy wirtualnych - zestaw w górę jako serwera plików

![](./media/storsimple-ova-deploy3-fs-setup/fileserver4.png)

## <a name="introduction"></a>Wprowadzenie 

Ten artykuł dotyczy usługi Microsoft Azure StorSimple wirtualnych tablicy (nazywane także urządzenia wirtualnego StorSimple lokalnego lub urządzenia wirtualnego StorSimple) uruchomionego 2016 marca ogólnodostępną (GA) wersji. W tym artykule opisano, jak wykonywać początkowej konfiguracji, zarejestrować serwera plików StorSimple, ukończenia instalacji urządzenia i utworzyć i nawiązać udziałów SMB. To jest ostatni artykuł w serii samouczki wdrażania wymaganych do całkowicie wdrożenia wirtualnych macierzy jako serwera plików lub serwerem.

Proces instalacji i konfiguracji może zająć około 10 minut do wykonania.


## <a name="setup-prerequisites"></a>Wymagania wstępne dotyczące instalacji

Przed skonfigurowaniem i konfigurowanie urządzenia wirtualnego StorSimple, upewnij się, że:

-   Masz obsługi administracyjnej urządzenie wirtualne i podłączonych do niej zgodnie z [dostarczania tablicę Virtual StorSimple w funkcji Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) lub [przepisu tablicę Virtual StorSimple w VMware](storsimple-ova-deploy2-provision-vmware.md).

-   Masz klucz rejestru usługi przez usługę Menedżer StorSimple utworzone przez Ciebie do zarządzania urządzeniami virtual StorSimple. Aby uzyskać więcej informacji, zobacz [Krok 2: uzyskiwanie klucza rejestru usługi](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key) StorSimple wirtualnych tablicy.

-   Jeśli jest to urządzenie wirtualnych drugi lub późniejszy, które rejestrujesz się z istniejącą usługę Menedżer StorSimple, należy użyć klucza szyfrowania danych usługi. Ten klucz został wygenerowany po pierwszym urządzeniu został pomyślnie zarejestrowany z tej usługi. Utraconych ten klucz uzyskać macierzy Virtual StorSimple, zobacz [Uzyskiwanie klucza szyfrowania danych usługi](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) .

## <a name="step-by-step-setup"></a>Ustawienia krok po kroku

Instalowanie i konfigurowanie urządzenia wirtualnego StorSimple za pomocą następujących instrukcji krok po kroku.

## <a name="step-1-complete-the-local-web-ui-setup-and-register-your-device"></a>Krok 1: Kończenie ustawienia interfejsu użytkownika lokalnego sieci web i zarejestrować urządzenie 


#### <a name="to-complete-the-setup-and-register-the-device"></a>Aby zakończyć konfigurację i zarejestrować urządzenie

1.  Otwórz okno przeglądarki i łączenie do lokalnego interfejsu użytkownika w sieci web. Typ: 

    `https://<ip-address of network interface>`

    Za pomocą adresu URL połączenia ułatwień wymienionych w poprzednim kroku. Zostanie wyświetlony komunikat o błędzie informujący, że jest problem z certyfikatem zabezpieczeń witryny sieci Web. Kliknij przycisk **Kontynuuj, aby ta strona sieci Web**.

    ![](./media/storsimple-ova-deploy3-fs-setup/image2.png)

1.  Zaloguj się do interfejsu użytkownika wirtualnego urządzenia w sieci web jako **StorSimpleAdmin**. Wprowadź hasło administratora urządzenia, które zostały zmienione w kroku 3: rozpoczynanie urządzenia wirtualnego w [świadczenia tablicę Virtual StorSimple w funkcji Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) lub [świadczenia tablicę Virtual StorSimple w VMware](storsimple-ova-deploy2-provision-vmware.md).

    ![](./media/storsimple-ova-deploy3-fs-setup/image3.png)

1.  Nastąpi przekierowanie do strony **głównej** . Na tej stronie opisano różne ustawienia wymagane do skonfigurowania i zarejestrować urządzenie wirtualne usłudze Menedżer StorSimple. Zauważ, że **sieci ustawienia**, **Ustawienia serwera proxy w sieci Web**i **Ustawienia czasu** są opcjonalne. Tylko ustawienia wymagane są **Ustawienia urządzenia** i **chmury**.

    ![](./media/storsimple-ova-deploy3-fs-setup/image4.png)

1.  Na stronie **ustawień sieci** w obszarze **interfejsy**danych 0 zostanie automatycznie skonfigurowany dla Ciebie. Każdy interfejs sieci jest domyślnie automatycznie uzyskać adres IP (DHCP). W związku z tym adres IP, podsieci i bramy zostanie automatycznie przypisany (w przypadku IPv4 i IPv6).

    ![](./media/storsimple-ova-deploy3-fs-setup/image5.png)

    Jeśli podczas inicjowania obsługi administracyjnej urządzenia dodano więcej niż jeden interfejs sieci, można je skonfigurować w tym miejscu. Należy zauważyć, że można skonfigurować usługi sieciowej jako IPv4 wyłącznie lub jako IPv4 i IPv6. Tylko konfiguracje protokołu IPv6 nie są obsługiwane.

1.  Serwery DNS są wymagane, ponieważ są one używane podczas próby urządzenia można komunikować się z dostawcami usług do chmury miejsca do magazynowania lub, aby rozwiązać urządzenia według nazwy podczas konfigurowania jako serwer plików. Na stronie **ustawień sieci** **serwerów DNS**:

    1.  Podstawowy i pomocniczy serwer DNS zostanie automatycznie skonfigurowany. Jeśli wybierzesz Konfigurowanie adresów IP, możesz określić serwery DNS. Wysoką dostępność zalecamy skonfigurowanie głównego i pomocniczego serwera DNS.

    2.  Kliknij przycisk **Zastosuj**. To zastosuje i sprawdź poprawność ustawień sieci.

2.  Na stronie **Ustawienia urządzenia** :

    1.  Przypisz unikatową **nazwę** urządzenia. Ta nazwa może zawierać 1-15 znaków i może zawierać litery, cyfry i łączniki.

    2.  Kliknij ikonę **serwera plików** ![](./media/storsimple-ova-deploy3-fs-setup/image6.png) dla danego **typu** urządzenia, które są tworzone. Na serwerze plików umożliwia tworzenie folderów udostępnionych.

    3.  Urządzenie jest na serwerze plików, należy dołączyć urządzenie do domeny. Wprowadź **nazwę domeny**.

    1.  Kliknij przycisk **Zastosuj**.

2.  Zostanie wyświetlone okno dialogowe. Wprowadź swoje poświadczenia domeny w określonym formacie. Kliknij ikonę wyboru. Sprawdza się poświadczeń domeny. Zostanie wyświetlony komunikat o błędzie, jeśli poświadczenia są niepoprawne.

    ![](./media/storsimple-ova-deploy3-fs-setup/image7.png)

1.  Kliknij przycisk **Zastosuj**. To zastosuje i sprawdź poprawność ustawień urządzenia.

    ![](./media/storsimple-ova-deploy3-fs-setup/image8.png)

    > [AZURE.NOTE]
    > 
    > Upewnij się, virtual macierzy znajduje się w osobnym jednostka organizacyjna (OU) usługi Active Directory i nie obiekty zasad grupy (GPO) są stosowane do niego lub dziedziczone. Zasady grupy może zainstalować aplikacji, takich jak oprogramowanie antywirusowe na tablicy Virtual StorSimple. Instalowanie dodatkowego oprogramowania nie jest obsługiwane i może spowodować uszkodzenie danych. 

1.  (Opcjonalnie) skonfiguruj serwer proxy sieci web. Mimo że konfiguracji serwera proxy dla sieci web jest opcjonalne, należy pamiętać, że jeśli korzystasz z serwera proxy sieci web, można skonfigurować tylko go w tym miejscu.

    ![](./media/storsimple-ova-deploy3-fs-setup/image9.png)

    Na stronie **sieci Web serwera proxy** :

    1.  Podać **adres URL serwera proxy sieci Web** w następującym formacie: *http://&lt;adres IP hosta lub FDQN&gt;: numer portu*. Należy zauważyć, że adresy URL HTTPS nie są obsługiwane.

    2.  **Uwierzytelnianie** można określić jako **podstawowe** lub **Brak**.

    3.  Jeśli przy użyciu funkcji uwierzytelniania, konieczne będzie o podanie **nazwy użytkownika** i **hasła**.

    4.  Kliknij przycisk **Zastosuj**. To sprawdzić poprawność i zastosować ustawienia serwera proxy skonfigurowane sieci web.

1.  (Opcjonalnie) skonfiguruj ustawienia czasu dla swojego urządzenia, takie jak strefa czasowa i serwerami NTP głównego i pomocniczego. Serwery NTP są wymagane, ponieważ urządzenia, należy zsynchronizować czasu, aby mógł uwierzytelnić z dostawcami usług do chmury.

    ![](./media/storsimple-ova-deploy3-fs-setup/image10.png)

    Na stronie **Ustawienia czasu** :

    1.  Z listy rozwijanej wybierz **strefę czasową** na podstawie lokalizacji geograficznej w jakiej zostanie wdrożony urządzenia. Domyślna strefa czasowa dla danego urządzenia jest pliku PST. Urządzenie będzie używać tej strefie czasowej wszystkie operacje według harmonogramu.

    2.  Określanie **serwera NTP podstawowego** dla swojego urządzenia lub zaakceptuj wartość domyślną time.windows.com. Upewnij się, że sieć umożliwia NTP na przekazywanie ruchu z centrum danych do Internetu.

    3.  Opcjonalnie określ **serwer NTP pomocnicza** dla danego urządzenia.

    4.  Kliknij przycisk **Zastosuj**. To sprawdzić poprawność i stosowanie ustawień skonfigurowanym czasie.

1.  Konfigurowanie ustawień chmury dla danego urządzenia. W tym kroku możesz zakończyć konfigurację urządzeniem lokalnym i następnie zarejestrować urządzenia z usługą Menedżera StorSimple.

    1.  Wprowadzanie **klucza rejestru usługi** masz w [Krok 2: uzyskiwanie klucza rejestru usługi](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key) StorSimple wirtualnych tablicy.

    2.  Pomiń ten krok, jeśli jest to pierwszy urządzenia rejestrowanie przy użyciu tej usługi i przejdź do następnego kroku. Jest to pierwszy urządzenie, którego rejestrujesz się w tej usłudze, konieczne będzie podanie **klucza szyfrowania danych usługi**. Ten klawisz jest wymagane dla usługi klucza rejestru usługi rejestrowania dodatkowe urządzenia z usługą Menedżera StorSimple. Aby uzyskać więcej informacji zapoznaj się z uzyskania [klucza szyfrowania danych usługi](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) lokalnej sieci Web interfejsu użytkownika.

    3.  Kliknij przycisk **Zarejestruj**. Uruchom ponownie urządzenie. Może być konieczne Czekaj na 2 – 3 minuty przed zarejestrowaniem pomyślnie urządzenia. Po ponownym uruchomieniu urządzenia nastąpi przekierowanie do strony logowania strony.

        ![](./media/storsimple-ova-deploy3-fs-setup/image13.png)
    

1.  Wróć do portalu klasyczny Azure. Na stronie **urządzeń** Sprawdź, że urządzenie pomyślnie jest połączony z usługą, wyświetlając stan. Stan urządzenia powinna być **aktywna**.

![](./media/storsimple-ova-deploy3-fs-setup/image12.png)

## <a name="step-2-complete-the-required-device-setup"></a>Krok 2: Ukończenia instalacji wymagane urządzenie

Aby ukończyć konfigurację urządzenia urządzenia StorSimple, należy:

-   Wybierz konto miejsca do magazynowania chcesz skojarzyć urządzenia.

-   Wybierz pozycję Ustawienia szyfrowania danych, które są wysyłane do chmury.

Wykonaj następujące czynności w [portal Azure klasyczny](https://manage.windowsazure.com/) do ukończenia instalacji wymagane urządzenie.

#### <a name="to-complete-the-minimum-device-setup"></a>Aby zakończyć konfigurację urządzeń

1.  Na stronie **urządzeń** wybierz urządzenie, którego została właśnie utworzona. To urządzenie będzie wyświetlany jako **aktywna**. Kliknij strzałkę obok nazwy urządzenia, a następnie kliknij przycisk **Szybkie uruchamianie**.

2.  Kliknij pozycję **Ustawienia kompletne urządzenie** , aby uruchomić Kreatora konfiguracji urządzenia.

3.  W Kreatorze konfiguracji urządzeń na stronie **Ustawienia podstawowe** wykonaj następujące czynności:

    1.  Określ konto miejsca do magazynowania, może być używany z urządzeniem. Można wybrać istniejące konto miejsca do magazynowania w tej subskrypcji z listy rozwijanej lub określać **Dodaj więcej** , aby wybrać konta z innej subskrypcji.

    2.  Definiowanie ustawienia szyfrowania dla wszystkich danych podczas spoczynku (szyfrowanie AES), które będzie wysyłane do chmury. Aby zaszyfrować danych, zaznacz pole kombi, aby **włączyć klucza szyfrowania magazynu w chmurze**. Wprowadź chmury szyfrowania miejsca do magazynowania, 32 znaki. Ponownie wprowadź klucz, aby je potwierdzić. Klucz 256-bitowego AES zostanie użyty do szyfrowania kluczem zdefiniowane przez użytkownika.

    3.  Kliknij ikonę wyboru ![](./media/storsimple-ova-deploy3-fs-setup/image15.png).

        ![](./media/storsimple-ova-deploy3-fs-setup/image16.png)

Ustawienia teraz zostaną zaktualizowane. Po pomyślnym aktualizacji ustawienia przycisk Ustawienia kompletne urządzenie być wyszarzone. Może zwrócić na stronę **Szybki Start** urządzenia.

 ![](./media/storsimple-ova-deploy3-fs-setup/image17.png)


> [AZURE.NOTE]                                                              
>
> Wszystkie inne ustawienia urządzenia można zmodyfikować w dowolnym momencie po zalogowaniu się do strony **konfiguracji** .

## <a name="step-3-add-a-share"></a>Krok 3: Dodawanie udziału

W [portalu klasyczny Azure](https://manage.windowsazure.com/) Aby utworzyć udział, należy wykonać następujące czynności.

#### <a name="to-create-a-share"></a>Aby utworzyć udział

1.  Na stronie **Szybkie uruchamianie** urządzenia kliknij przycisk **Dodaj udziału**. Zostanie uruchomiony Kreator Udostępnij Dodawanie.

    ![](./media/storsimple-ova-deploy3-fs-setup/image17.png)

1.  Na stronie **Ustawienia podstawowe** wykonaj następujące czynności:

    1.  Określ unikatową nazwę swojego udziału. Nazwa musi być ciąg, który zawiera znaki 3 do 127.

    2.  (Opcjonalnie) Podaj opis udziału. Opis pomoże zidentyfikować właściciele Udostępnij.

    3.  Wybierz typ użycia udziału. Typ użycia może być **Tiered** lub **lokalnie przypięte**, z warstwowych są domyślnie. Obciążeń pracą wymagających lokalnego gwarancje, niska opóźnienia i większą wydajność wybierz udziału **lokalnie przypięte** . Dla wszystkich innych danych wybierz pozycję Udostępnij **Tiered** .

    Udostępnianie lokalnie przypięty grubo obsługi administracyjnej i gwarantuje, że dane podstawowe w udziale pozostaje lokalne na urządzeniu, a nie rozsypywać w chmurze. Udostępnianie warstwowych z drugiej strony jest znacznie obsługi administracyjnej. Po utworzeniu udziału warstwowych 10% obszaru jest włączona na warstwie lokalne i 90% obszaru jest obsługi administracyjnej w chmurze. Na przykład jeśli zainicjowano obsługę administracyjną głośność 1 TB, 100 GB czy znajdują się w lokalnej przestrzeni i 900 GB będzie używane w chmurze po poziomów danych. Z kolei oznacza to, że jeśli lokalnej przestrzeni brakować na tym urządzeniu, nie można dodawać warstwowych Udostępnij.

1.  Określ wielkość ustanawianie dla swojego udziału. Należy zauważyć, że określona pojemność powinien być mniejszy niż dostępne możliwości. Jeśli używasz udziału warstwowych, rozmiar Udostępnij powinien być między 500 GB i 20 TB. Do udziału lokalnie przypięty określ wielkość Udostępnij od 50 GB do 2 TB. Użyj możliwości dostępne jako przewodnik inicjowania obsługi udziału. Jeśli pojemność lokalne jest 0 GB, następnie będzie nie można inicjować obsługę lokalnych lub warstwowych udziałów.

    ![](./media/storsimple-ova-deploy3-fs-setup/image18.png)

1.  Kliknij ikonę strzałki ![](./media/storsimple-ova-deploy3-fs-setup/image19.png) aby przejść do następnej strony.

1.  Na stronie **Dodatkowe ustawienia** przypisać uprawnienia użytkownika lub grupę, do której będą uzyskiwać dostęp do tego udziału. Określ nazwę użytkownika lub grupy użytkowników w *<john@contoso.com>* format. Firma Microsoft zaleca używanie grupy użytkowników (zamiast pojedynczego użytkownika) umożliwia uprawnienia administratora uzyskać dostęp do tych akcji. Po przypisaniu uprawnień tutaj następnie służy Eksploratora Windows do modyfikowania tych uprawnień.

    ![](./media/storsimple-ova-deploy3-fs-setup/image20.png)

1.  Kliknij ikonę wyboru ![](./media/storsimple-ova-deploy3-fs-setup/image21.png). Udostępnianie zostanie utworzony przy użyciu określonych ustawień. Domyślnie monitorowanie i kopia zapasowa będą dostępne dla Udostępnij.

## <a name="step-4-connect-to-the-share"></a>Krok 4: Nawiązania połączenia z udziałem

Konieczne będzie teraz nawiązywanie połączenia z każdej, który został utworzony w poprzednim kroku. Te kroki należy wykonać na hoście systemu Windows Server.

#### <a name="to-connect-to-the-share"></a>Aby nawiązać połączenie z udziałem

1.  Naciśnij klawisz ![](./media/storsimple-ova-deploy3-fs-setup/image22.png) + R. W oknie Uruchamianie określ * \\ * jako ścieżkę, zamieniając *nazwę serwera* nazwę urządzenia, przydzielone do serwera plików. Kliknij **przycisk OK**.

    ![](./media/storsimple-ova-deploy3-fs-setup/image23.png)

2.  Spowoduje to otwarcie Eksploratora w górę. Teraz powinno być widoczne akcje utworzone jako foldery. Zaznacz, a następnie kliknij dwukrotnie Udostępnij (folder), aby wyświetlić zawartość.

    ![](./media/storsimple-ova-deploy3-fs-setup/image24.png)

3.  Teraz można dodać pliki do te akcje i wykonać kopię zapasową.

![ikona klipu wideo](./media/storsimple-ova-deploy3-fs-setup/video_icon.png) **wideo dostępne**

Obejrzyj klip wideo, aby zobaczyć, jak skonfigurować i zarejestrować tablicę Virtual StorSimple jako serwer plików.

> [AZURE.VIDEO configure-a-storsimple-virtual-array]

## <a name="next-steps"></a>Następne kroki

Informacje o sposobie korzystania z niego lokalne interfejsu użytkownika do [zarządzania macierzy Virtual StorSimple](storsimple-ova-web-ui-admin.md).
