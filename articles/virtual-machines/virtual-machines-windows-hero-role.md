<properties
    pageTitle="Instalowanie programu IIS na swojej pierwszej maszyn wirtualnych systemu Windows | Microsoft Azure"
    description="Poeksperymentować z pierwszego komputera wirtualnych przez zainstalowanie usług IIS i otwieranie portu 80 za pomocą portalu Azure."
    keywords=""
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="cynthn"/>

# <a name="experiment-with-installing-a-role-on-your-windows-vm"></a>Eksperymentować instalowania roli na swojej maszyn wirtualnych systemu Windows
    
Po pierwszym wirtualnych komputera (maszyn wirtualnych) pracę, można przenieść instalowanie oprogramowania i usług. Ten samouczek chwilę za pomocą Menedżera serwera na maszyn wirtualnych systemu Windows Server instalacji programu IIS. Następnie zostanie utworzony grupy zabezpieczeń sieci (NSG) otwórz port 80 na ruch programu IIS za pomocą portalu Azure. 

Jeśli utworzono nie jeszcze do pierwszej maszyn wirtualnych, należy wrócisz do [tworzenia komputera pierwszego wirtualnych w portalu Azure](virtual-machines-windows-hero-tutorial.md) przed kontynuowaniem tego samouczka.

## <a name="make-sure-the-vm-is-running"></a>Upewnij się, że działa maszyn wirtualnych

1. Otwórz [Azure portal](https://portal.azure.com).
2. W menu Centrum kliknij **maszyn wirtualnych**. Z listy wybierz maszyny wirtualnej.
3. Jeśli stan jest **zatrzymania (Deallocated)**, kliknij przycisk **Start** na karta **podstawowe** części maszyn wirtualnych. Jeśli jest wyświetlany stan **uruchomiony**, na można przenosić do następnego kroku.

## <a name="connect-to-the-virtual-machine-and-sign-in"></a>Łączenie maszyn wirtualnych i logowanie

1.  W menu Centrum kliknij **maszyn wirtualnych**. Z listy wybierz maszyny wirtualnej.

3. Na karta maszyny wirtualnej kliknij przycisk **Połącz**. Tworzy, a do pobrania pliku Remote Desktop Protocol (RDP pliku), podobnie jak skrótu połączyć się z komputerem. Można zapisać plik na pulpicie w celu ułatwienia dostępu. **Otwórz** ten plik Aby nawiązać połączenie z maszyn wirtualnych.

    ![Zrzut ekranu przedstawiający, jak połączyć się z maszyn wirtualnych portal Azure](./media/virtual-machines-windows-hero-tutorial/connect.png)

4. Zostanie wyświetlone ostrzeżenie, który .rdp jest nieznany wydawcy. Jest to normalne. W oknie pulpitu zdalnego kliknij pozycję **Połącz** , aby kontynuować.

    ![Zrzut ekranu przedstawiający ostrzeżenia o nieznanych programu publisher](./media/virtual-machines-windows-hero-tutorial/rdp-warn.png)

5. W oknie zabezpieczeń systemu Windows wpisz nazwę użytkownika i hasło lokalnego konta utworzone przez Ciebie podczas tworzenia maszyn wirtualnych. Nazwa użytkownika jest wprowadzona jako *vmname*& #92; *Nazwa użytkownika*, kliknij **przycisk OK**.

    ![Zrzut ekranu: wprowadzanie nazwy maszyn wirtualnych, nazwę użytkownika i hasło](./media/virtual-machines-windows-hero-tutorial/credentials.png)
    
6.  Zostanie wyświetlone ostrzeżenie nie może zweryfikować certyfikatu. Jest to normalne. Kliknij pozycję, **Tak** Aby zweryfikować tożsamość maszyny wirtualnej i zakończenie logowania.

    ![Zrzut ekranu przedstawiający komunikat stykają się weryfikowanie tożsamości maszyn wirtualnych](./media/virtual-machines-windows-hero-tutorial/cert-warning.png)


Jeśli możesz wystąpią problemy podczas próby nawiązania połączenia, zobacz [Rozwiązywanie problemów z pulpitu zdalnego połączenia do systemu Windows Azure maszyny wirtualnej](virtual-machines-windows-troubleshoot-rdp-connection.md).


## <a name="install-iis-on-your-vm"></a>Instalowanie programu IIS na swojej maszyn wirtualnych

Teraz, gdy użytkownik jest zalogowany do maszyn wirtualnych, możemy zainstalować rola serwera, dlatego możesz poeksperymentować więcej.

1. Otwórz **Menedżera serwera** , jeśli nie jest jeszcze otwarte. Kliknij **Start** menu, a następnie kliknij pozycję **Menedżer serwera**.
2. W **Menedżerze serwera**wybierz pozycję **Lokalnego serwera** w okienku po lewej stronie. 
3. W menu wybierz pozycję **Zarządzaj** > **Dodaj role i funkcje**.
4. W Kreatorze funkcje, na stronie **Typ instalacji** i Dodaj role wybierz **Instalacja oparta na rolach lub funkcji**, a następnie kliknij przycisk **Dalej**.

    ![Zrzut ekranu przedstawiający kartę Dodaj role i funkcje kreatora w obszarze Typ instalacji](./media/virtual-machines-windows-hero-tutorial/role-wizard.png)

5. Zaznacz maszyn wirtualnych z puli serwera, a następnie kliknij przycisk **Dalej**.
6. Na stronie **Role serwerów** wybierz **Serwer sieci Web (IIS)**.

    ![Zrzut ekranu przedstawiający kartę Dodaj role i Kreatora funkcji dla ról serwera](./media/virtual-machines-windows-hero-tutorial/add-iis.png)

7. W oknie podręcznym o dodawaniu funkcje wymagane dla usług IIS upewnij się, że jest zaznaczona opcja **zawiera narzędzia do zarządzania** , a następnie kliknij przycisk **Dodaj funkcje**. Gdy zamyka okno, kliknij przycisk **Dalej** w kreatorze.

    ![Zrzut ekranu przedstawiający podręcznym o potwierdzenie dodania roli usług IIS](./media/virtual-machines-windows-hero-tutorial/confirm-add-feature.png)

8. Na stronie funkcje kliknij przycisk **Dalej**.
9. Na stronie **Rola serwera sieci Web (IIS)** kliknij przycisk **Dalej**. 
10. Na stronie **Usługi ról** kliknij przycisk **Dalej**. 
11. Na stronie **potwierdzenia** kliknij przycisk **Zainstaluj**. 
12. Po zakończeniu instalacji kliknij przycisk **Zamknij** kreatora.



## <a name="open-port-80"></a>Otwórz port 80 

Aby do maszyn wirtualnych zaakceptować ruchu przychodzącego przez port 80 musisz dodać regułę ruchu przychodzącego do grupy zabezpieczeń sieci. 

1. Otwórz [Azure portal](https://portal.azure.com).
2. W **środowisku maszyn wirtualnych systemu** wybierz maszyn wirtualnych utworzone przez Ciebie.
3. W obszarze Ustawienia maszyn wirtualnych wybierz **interfejsów sieciowych** , a następnie wybierz istniejący sieciowej.

    ![Zrzut ekranu przedstawiający ustawienie maszyn wirtualnych interfejsów sieciowych](./media/virtual-machines-windows-hero-tutorial/network-interface.png)

4. **Podstawowe informacje dotyczące** interfejsu sieci kliknij **grupę zabezpieczeń sieci**.

    ![Zrzut ekranu przedstawiający sekcji Essentials dla interfejsu sieciowego](./media/virtual-machines-windows-hero-tutorial/select-nsg.png)

5. W karta **Essentials** dla NSG należy użyć jednej istniejącej domyślne Reguła ruchu przychodzącego dla **domyślnego Zezwalaj rdp** , dzięki czemu można zalogować się do maszyn wirtualnych. Należy dodać inną regułę ruchu przychodzącego dla zezwalanie na ruch programu IIS. Kliknij przycisk **Reguły przychodzące zabezpieczeń**.

    ![Zrzut ekranu przedstawiający sekcji Essentials dla NSG](./media/virtual-machines-windows-hero-tutorial/inbound.png)

6. W **ruchu przychodzącego reguły zabezpieczeń**kliknij przycisk **Dodaj**.

    ![Zrzut ekranu przedstawiający przycisk Dodaj regułę zabezpieczeń](./media/virtual-machines-windows-hero-tutorial/add-rule.png)

7. W **ruchu przychodzącego reguły zabezpieczeń**kliknij przycisk **Dodaj**. Wpisz **80** w zakresie portów i upewnij się, że zaznaczono pola wyboru **Zezwalaj** . Gdy skończysz, kliknij **przycisk OK**.

    ![Zrzut ekranu przedstawiający przycisk Dodaj regułę zabezpieczeń](./media/virtual-machines-windows-hero-tutorial/port-80.png)
 
Aby uzyskać więcej informacji na temat NSGs, reguły przychodzące i wychodzące, zobacz [Zezwalaj na zewnętrznym dostęp do Twojej maszyn wirtualnych za pomocą portalu Azure](virtual-machines-windows-nsg-quickstart-portal.md)
 
## <a name="connect-to-the-default-iis-website"></a>Nawiązywanie połączenia z usług IIS domyślna witryna sieci Web

1. W portalu usługi Azure kliknij **maszyn wirtualnych** , a następnie wybierz z maszyn wirtualnych.
2. Karta **podstawowe informacje dotyczące** skopiuj swój **publiczny adres IP**.

    ![Zrzut ekranu przedstawiający, gdzie znaleźć publiczny adres IP swojego maszyn wirtualnych](./media/virtual-machines-windows-hero-tutorial/ipaddress.png)

2. Otwórz przeglądarkę i w pasku adresu wpisz publiczny adres IP, w następujący sposób: http://<publicIPaddress> i kliknij pozycję **Enter** , aby przejść do tego adresu.
3. Przeglądarki należy otwierać domyślnej strony sieci web usług IIS. Wygląda mniej więcej tak:

    ![Zrzut ekranu przedstawiający, jak wygląda domyślnej strony usług IIS w przeglądarce](./media/virtual-machines-windows-hero-tutorial/iis-default.png)

    

## <a name="next-steps"></a>Następne kroki

- Możesz również eksperymentować [Dołączanie dysku danych](virtual-machines-windows-attach-disk-portal.md) na komputerze wirtualnych. Dyski danych zapewniają więcej miejsca do magazynowania dla komputera wirtualnych.
