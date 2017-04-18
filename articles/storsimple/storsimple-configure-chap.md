<properties 
   pageTitle="Konfigurowanie CHAP dla swojego urządzenia StorSimple | Microsoft Azure"
   description="W tym artykule opisano sposób konfigurowania uwierzytelniania Protocol (CHAP) na urządzeniu StorSimple."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="configure-chap-for-your-storsimple-device"></a>Konfigurowanie CHAP dla swojego urządzenia StorSimple

Ten samouczek opisano sposób konfigurowania CHAP dla swojego urządzenia StorSimple. Procedury wyszczególnione w tym artykule dotyczą serii StorSimple 8000, a także StorSimple 1200 urządzenia.

CHAP oznacza protokół uwierzytelniania uzgadniania wezwanie. Jest schematu uwierzytelniania używane przez serwery do sprawdzania tożsamości klientów zdalnych. Weryfikacja jest oparty na udostępnionych hasła lub hasło. CHAP może być jednokierunkowa (jednokierunkowe) lub wzajemnej (dwukierunkowe). Jednokierunkowe CHAP jest, gdy inicjator uwierzytelnia docelowej. CHAP wzajemnego lub odwrotnej, wymaga z drugiej strony docelowej uwierzytelnienia Inicjator i następnie inicjator uwierzytelnić docelowej. Inicjator może być realizowane bez uwierzytelniania docelowej. Jednak docelowej może być realizowane tylko wtedy, gdy inicjator uwierzytelniania również jest zaimplementowana. 

Zgodnie z zaleceniami dotyczącymi zalecamy użycie CHAP w celu zwiększenia zabezpieczeń iSCSI.

>[AZURE.NOTE] Należy pamiętać, że IPSEC nie jest obecnie obsługiwane na urządzeniach StorSimple.

Ustawienia CHAP na tym urządzeniu StorSimple można skonfigurować w następujący sposób:

- Uwierzytelnianie jednokierunkowe lub jednokierunkowe

- Uwierzytelnianie wzajemnego lub odwrotnej lub dwukierunkowym

W każdym z tych przypadków portal dla urządzenia i oprogramowania inicjator iSCSI serwera musi skonfigurować. Szczegółowe informacje na temat tej konfiguracji opisanych w następujących samouczka.

## <a name="unidirectional-or-one-way-authentication"></a>Uwierzytelnianie jednokierunkowe lub jednokierunkowe

W przypadku uwierzytelniania jednokierunkowe docelowej uwierzytelnia inicjator. Uwierzytelnianie wymaga skonfigurować ustawienia inicjator CHAP na tym urządzeniu StorSimple i iSCSI inicjator oprogramowania na hoście. Szczegółowe procedury StorSimple urządzenia i hosta systemu Windows opisane dalej.

#### <a name="to-configure-your-device-for-one-way-authentication"></a>Aby skonfigurować urządzenia w celu uwierzytelnianie jednokierunkowe

1. W portalu klasyczny Azure, na stronie **urządzenia** kliknij kartę **Konfiguruj** .

    ![CHAP inicjator](./media/storsimple-configure-chap/IC740943.png)

2. Przewiń w dół na tej stronie, a następnie w sekcji **CHAP inicjator** :
                                                    
    1. Podaj nazwę użytkownika dla swojego inicjator CHAP.

    2. Wprowadź hasło dla usługi inicjator CHAP.

         > [AZURE.IMPORTANT] Nazwa użytkownika CHAP musi zawierać mniej niż 233 znaki. Hasło CHAP musi zawierać od 12 do 16 znaków. Wystąpił błąd uwierzytelniania na hoście systemu Windows spowoduje dłużej nazwa użytkownika lub hasło.
    
    3. Potwierdź hasło.

4. Kliknij przycisk **Zapisz**. Pojawi się komunikat z potwierdzeniem. Kliknij **przycisk OK** , aby zapisać zmiany.

#### <a name="to-configure-one-way-authentication-on-the-windows-host-server"></a>Aby skonfigurować uwierzytelnianie jednokierunkowe w systemie Windows server hosta

1. W systemie Windows server hosta Uruchom inicjator iSCSI.

2. W oknie **Właściwości inicjator iSCSI** należy wykonać następujące czynności:
                                                    
    1. Kliknij kartę **odnajdowania** .

        ![właściwości inicjator iSCSI](./media/storsimple-configure-chap/IC740944.png)

    2. Kliknij przycisk **Odkryj Portal**.

3. W oknie dialogowym **Odkrywanie Portal docelowej** :
                                                    
    1. Określ adres IP urządzenia.

    3. Kliknij pozycję **Zaawansowane**.

        ![Poznawanie portal docelowej](./media/storsimple-configure-chap/IC740945.png)

4. W oknie dialogowym **Ustawienia zaawansowane** :
                                                    
    1. Zaznacz pole wyboru **Włącz protokół CHAP Zaloguj** .

    2. W polu **Nazwa** wprowadź nazwę użytkownika, określone dla CHAP inicjator w portalu klasyczny.

    3. W polu **hasło docelowej** wprowadź hasło, określone dla CHAP inicjator w portalu klasyczny.

    4. Kliknij **przycisk OK**.

        ![Zaawansowane ustawienia ogólne](./media/storsimple-configure-chap/IC740946.png)

5. Na karcie **cele** okna **iSCSI inicjator właściwości** stan urządzenia powinien być wyświetlany jako **połączony**. Jeśli korzystasz z urządzenia StorSimple 1200, następnie każdego woluminu zostanie zainstalowany jako obiekt docelowy iSCSI tak jak pokazano poniżej. W związku z tym kroki od 3 do 4 należy powtórzyć dla każdego woluminu.

    ![Wielkości zainstalowanym jako osobne elementów docelowych](./media/storsimple-configure-chap/chap4.png)

    > [AZURE.IMPORTANT] Jeśli zmienisz nazwę iSCSI, Nowa nazwa będzie używana dla nowych sesjach. Nowe ustawienia nie są używane do istniejących sesji do momentu wylogowania i zaloguj ponownie.

Aby uzyskać więcej informacji o konfigurowaniu CHAP na serwerze hosta systemu Windows przejdź do [sekcji Uwagi dodatkowe](#additional-considerations).


## <a name="bidirectional-or-mutual-authentication"></a>Uwierzytelnianie dwukierunkowe lub wzajemnej

W przypadku uwierzytelniania dwukierunkowy docelowej uwierzytelnia inicjator, a następnie inicjator uwierzytelnia docelowej. W tym celu użytkownikowi na konfigurowanie ustawień inicjator CHAP, a także odwrotnej ustawienia CHAP oprogramowania inicjator urządzenia i iSCSI na hoście. W poniższych procedurach opisano czynności, aby skonfigurować wzajemnego uwierzytelniania na tym urządzeniu, a na hoście systemu Windows.

#### <a name="to-configure-your-device-for-mutual-authentication"></a>Aby skonfigurować urządzenia w celu wzajemnego uwierzytelniania

1. W portalu klasyczny Azure, na stronie **urządzenia** kliknij kartę **Konfiguruj** .

    ![CHAP docelowej](./media/storsimple-configure-chap/IC740948.png)

2. Przewiń w dół na tej stronie, a następnie w sekcji **Docelowej CHAP** :
                                                    
    1. Podaj **odwrócić CHAP nazwę użytkownika** dla danego urządzenia.

    2. Wprowadź **hasło odwrócić CHAP** dla danego urządzenia.

    3. Potwierdź hasło.

3. W sekcji **CHAP inicjator** :
                                                
    1. Podaj **nazwę użytkownika** dla danego urządzenia.

    1. Podanie **hasła** dla danego urządzenia.

    3. Potwierdź hasło.

4. Kliknij przycisk **Zapisz**. Pojawi się komunikat z potwierdzeniem. Kliknij **przycisk OK** , aby zapisać zmiany.

#### <a name="to-configure-bidirectional-authentication-on-the-windows-host-server"></a>Aby skonfigurować dwukierunkowe uwierzytelnianie na serwerze hosta systemu Windows

1. W systemie Windows server hosta Uruchom inicjator iSCSI.

2. W oknie **iSCSI inicjator właściwości** kliknij kartę **Konfiguracja** .

3. Kliknij pozycję **CHAP**.

4. W oknie dialogowym **iSCSI inicjator wzajemnego CHAP hasła** :
                                                    
    1. Wpisz **Odwrócić CHAP hasło** skonfigurowane w portalu klasyczny Azure.

    2. Kliknij **przycisk OK**.

        ![tajny wzajemnego protokołu CHAP inicjator iSCSI](./media/storsimple-configure-chap/IC740949.png)

5. Kliknij kartę **elementów docelowych** .

6. Kliknij przycisk **Połącz** . 

7. W oknie dialogowym **Łączenie do docelowej** kliknij przycisk **Zaawansowane**.

8. W oknie dialogowym **Właściwości zaawansowane** :
                                                    
    1. Zaznacz pole wyboru **Włącz protokół CHAP Zaloguj** .

    2. W polu **Nazwa** wprowadź nazwę użytkownika, określone dla CHAP inicjator w portalu klasyczny.

    3. W polu **hasło docelowej** wprowadź hasło, określone dla CHAP inicjator w portalu klasyczny.

    4. Zaznacz pole wyboru **Wykonaj wzajemnego uwierzytelniania** .

        ![Ustawienia zaawansowane wzajemnego uwierzytelniania](./media/storsimple-configure-chap/IC740950.png)

    5. Kliknij **przycisk OK** , aby zakończyć konfigurację CHAP
     
Aby uzyskać więcej informacji o konfigurowaniu CHAP na serwerze hosta systemu Windows przejdź do [sekcji Uwagi dodatkowe](#additional-considerations).

## <a name="additional-considerations"></a>Uwagi dodatkowe

Funkcja **Szybkie połączenie** nie obsługuje połączenia, które mają CHAP włączone. Po włączeniu CHAP, upewnij się, że używasz przycisk **Połącz** , który jest dostępny na karcie **elementów docelowych** , aby połączyć się do wartości docelowej.

![Nawiązywanie połączenia z docelowej](./media/storsimple-configure-chap/IC740947.png)

W oknie dialogowym **Łączenie do miejsca docelowego** , które są prezentowane zaznacz pole wyboru **Dodaj to połączenie do listy ulubionych elementów docelowych** . Dzięki temu, że każdorazowo po ponownym uruchomieniu komputera próby przywrócenia połączenia do ulubionych obiektów docelowych iSCSI.

## <a name="errors-during-configuration"></a>Błędy podczas konfigurowania

Jeśli konfiguracji CHAP jest niepoprawny, następnie najprawdopodobniej będzie wyświetlony komunikat o błędzie **błąd uwierzytelniania** .

## <a name="verification-of-chap-configuration"></a>Weryfikacja konfiguracji CHAP

Można sprawdzić, czy CHAP jest używany przez wykonanie następujących kroków.

#### <a name="to-verify-your-chap-configuration"></a>Aby sprawdzić konfigurację CHAP

1. Kliknij przycisk **Ulubione elementów docelowych**.

2. Wybierz element docelowy, dla których włączono uwierzytelniania.

3. Kliknij przycisk **Szczegóły**.

    ![Ulubione obiekty docelowe iSCSI inicjator właściwości](./media/storsimple-configure-chap/IC740951.png)

4. W oknie dialogowym **Szczegóły docelowej Ulubione** Uwaga wpis w polu **uwierzytelniania** . Jeśli konfiguracja zakończyło się pomyślnie, należy powiedzieć, **CHAP**.

    ![Szczegóły ulubiony obiekt docelowy](./media/storsimple-configure-chap/IC740952.png)

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej o [zabezpieczeniach StorSimple](storsimple-security.md).

- Dowiedz się więcej o [korzystaniu z usługi Menedżera StorSimple administrowania urządzenia StorSimple](storsimple-manager-service-administration.md).
