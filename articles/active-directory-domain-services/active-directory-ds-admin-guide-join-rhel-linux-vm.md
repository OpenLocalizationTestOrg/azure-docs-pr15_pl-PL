<properties
    pageTitle="Azure Active Directory Domain Services: Dołączanie do maszyny RHEL z domeną zarządzanych | Microsoft Azure"
    description="Dołączanie do maszyny wirtualnej Red funkcję Enterprise Linux do usług domenowych AD Azure"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/02/2016"
    ms.author="maheshu"/>

# <a name="join-a-red-hat-enterprise-linux-7-virtual-machine-to-a-managed-domain"></a>Dołączanie do maszyny wirtualnej czerwony funkcję Enterprise Linux 7 z domeną zarządzanych
W tym artykule pokazano, jak dołączyć do maszyny wirtualnej czerwony funkcję Enterprise Linux (RHEL) 7 w domenie usług domenowych AD Azure zarządzane.

## <a name="provision-a-red-hat-enterprise-linux-virtual-machine"></a>Inicjowanie obsługi maszyny wirtualnej Red funkcję Enterprise Linux
Wykonaj poniższe czynności, aby zapewnić maszyny wirtualnej RHEL 7 za pomocą portalu Azure.

1. Zaloguj się do [portalu Azure](https://portal.azure.com).

    ![Azure portalu pulpitu nawigacyjnego](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-dashboard.png)

2. W lewym okienku kliknij pozycję **Nowy** , a następnie wpisz **Czerwony funkcję** na pasku wyszukiwania, jak pokazano w poniższej zrzut ekranu. W wynikach wyszukiwania zostaną wyświetlone tylko zapisy Red funkcję Enterprise Linux. Kliknij **czerwony funkcję Enterprise Linux 7.2**.

    ![Wybierz pozycję RHEL w wynikach](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-find-rhel-image.png)

3. Obraz czerwonego funkcję Enterprise Linux 7.2 należy lista wyników wyszukiwania w okienku **wszystkie elementy** . Kliknij **czerwony funkcję Enterprise Linux 7.2** , aby wyświetlić więcej informacji na temat obraz maszyn wirtualnych.

    ![Wybierz pozycję RHEL w wynikach](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-rhel-image.png)

4. W okienku **czerwony funkcję Enterprise Linux 7.2** znajdą więcej informacji na temat obraz maszyn wirtualnych. Na liście rozwijanej **Wybierz model wdrożenia** wybierz pozycję **Klasyczny**. Następnie kliknij przycisk **Utwórz** .

    ![Wyświetlanie szczegółów obrazu](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-clicked.png)

5. W okienku **Tworzenie maszyn wirtualnych** wprowadź **Nazwę hosta** dla nowej maszyny wirtualnej. W polu **Nazwa użytkownika** i **hasło**, także określić nazwę użytkownika administratora lokalnego. Można też użyć klawisza SSH do uwierzytelnienia użytkownika administratora lokalnego. Wybrać **Cennik warstwa** maszyny wirtualnej.

    ![Tworzenie maszyn wirtualnych — podstawowe informacje szczegółowe](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-basic-details.png)

6. Kliknij pozycję **opcjonalnym**. W okienku **opcjonalne konfiguracji** kliknij **sieć**.

    ![Tworzenie maszyn wirtualnych — Konfigurowanie wirtualnej sieci](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-configure-vnet.png)

7. Spowoduje to wyświetlenie okienka zatytułowany **sieci**. W okienku **sieci** kliknij pozycję **Wirtualną sieć** zaznacz wirtualnej sieci, do którego powinny zostać wdrożone maszyn wirtualnych Linux. Spowoduje to otwarcie okienka **Wirtualnej sieci** . W okienku **Wirtualnej sieci** wybierz opcję **Użyj istniejącej sieci wirtualnej** . Następnie wybierz pozycję wirtualnej sieci, w której Azure AD Domain Services jest dostępny. W tym przykładzie możemy wybierz wirtualną sieć "MyPreviewVNet".

    ![Tworzenie maszyn wirtualnych — wybór wirtualnej sieci](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-select-vnet.png)

8. W okienku **opcjonalne konfiguracji** kliknij przycisk **OK** .

    ![Tworzenie maszyn wirtualnych — wirtualnej sieci zaznaczone](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-vnet-selected.png)

9. Teraz możesz przystąpić do tworzenia maszyny wirtualnej. W okienku **Tworzenie maszyn wirtualnych** kliknij przycisk **Utwórz** .

    ![Tworzenie maszyn wirtualnych - gotowe](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm.png)

10. Nazwa powinna rozpoczynać się wdrożenia nowej maszyny wirtualnej na podstawie obrazu RHEL 7.2.

  ![Tworzenie maszyn wirtualnych - Wdrażanie rozpoczęte](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployment-started.png)

11. Po kilku minutach maszyny wirtualnej należy wdrożyć pomyślnie i gotowa do użycia.

  ![Tworzenie maszyn wirtualnych - wdrożony](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployed.png)



## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a>Łączenie zdalnie nowo ustanawianie maszyn wirtualnych Linux
Maszyny wirtualnej RHEL 7.2 zainicjowano platformy Azure. Następnego zadania jest nawiązywanie połączeń zdalnych maszyn wirtualnych.

**Łączenie maszyn wirtualnych RHEL 7.2** Postępuj zgodnie z instrukcjami w artykule [jak zalogować się do maszyny wirtualnej systemem Linux](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md).

Pozostałe kroki założono, że możesz połączyć się z komputerem wirtualnych RHEL za pomocą klienta SSH Kit. Aby uzyskać więcej informacji zobacz [Pobieranie Kit strony](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

1. Otwórz program Kit.

2. Wprowadź **Nazwę hosta** dla nowo utworzonego maszyny wirtualnej RHEL. W tym przykładzie naszych maszyn wirtualnych ma nazwę hosta "contoso-rhel.cloudapp .net". Jeśli nie masz pewności co do nazwy hosta usługi maszyn wirtualnych, zapoznaj się z pulpitu nawigacyjnego maszyn wirtualnych w portalu Azure.

    ![Łączenie Kit](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-connect.png)

3. Zaloguj się przy użyciu poświadczeń administratora lokalnego określonej podczas tworzenia maszyny wirtualnej maszyny wirtualnej. W tym przykładzie użyliśmy lokalnego konta administratora "mahesh".

    ![Kit logowania](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-login.png)


## <a name="install-required-packages-on-the-linux-virtual-machine"></a>Wymagane pakiety zostaną zainstalowane na komputerze wirtualnych Linux
Po nawiązaniu połączenia maszyn wirtualnych następnego zadania jest zainstalować pakiety wymagane dla sprzężenia domeny na komputerze wirtualnych. Wykonaj następujące czynności:

1. **Zainstalować realmd:** Pakiet realmd służy do dołączania do domeny. W swojej Kit terminal wpisz następujące polecenie:

    realmd instalacji yum sudo

    ![Instalowanie realmd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-realmd.png)

    Po kilku minutach pakietu realmd należy uzyskać zainstalowany na komputerze wirtualnych.

    ![realmd zainstalowane](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-installed.png)

3. **Zainstalować sssd:** Pakiet realmd zależy od sssd do wykonywania operacji dołączania do domeny. W swojej Kit terminal wpisz następujące polecenie:

    sssd instalacji yum sudo

    ![Instalowanie sssd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-sssd.png)

    Po kilku minutach pakietu sssd należy uzyskać zainstalowany na komputerze wirtualnych.

    ![realmd zainstalowane](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-sssd-installed.png)

4. **Instalowanie kerberos:** W swojej Kit terminal wpisz następujące polecenie:

    sudo yum instalacji roboczej krb5 krb5-bibliotekami

    ![Instalowanie kerberos](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-kerberos.png)

    Po kilku minutach pakietu realmd należy uzyskać zainstalowany na komputerze wirtualnych.

    ![Zainstalowano Kerberos](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kerberos-installed.png)


## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>Dołączanie do maszyny wirtualnej Linux do domeny zarządzanych
Wymagane pakiety są zainstalowane na komputerze wirtualnych Linux, następnego zadania jest do połączenia maszyny wirtualnej zarządzanych domeny.

1. Odkrywanie AAD domenie usług domenowych zarządzane. W swojej Kit terminal wpisz następujące polecenie:

    Poznawanie obszaru sudo CONTOSO100.COM

    ![Poznawanie obszaru](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-discover.png)

    W przypadku **poznawanie obszaru** nie można odnaleźć zarządzanych domeny, upewnij się, że domena jest dostępne z maszyny wirtualnej (spróbuj ping). Również upewnij się, że maszyny wirtualnej faktycznie został wdrożony do tej samej sieci wirtualnej, w którym zarządzanych domena jest dostępna.

2. Inicjowanie kerberos. W swojej Kit terminal wpisz następujące polecenie. Upewnij się, że określić użytkownik, który należy do grupy "Administratorzy AAD kontrolera domeny". Tylko ci użytkownicy mogą dołączyć do komputerów zarządzanych domenę.

    kinitbob@CONTOSO100.COM

    ![Kinit](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kinit.png)

    Zagwarantować, że Określ nazwę domeny wielkimi literami, kinit jeszcze nie powiedzie się.

3. Dołączanie komputera do domeny. W swojej Kit terminal wpisz następujące polecenie. Określ ten sam użytkownik, określonej w poprzednim kroku (kinit).

    Dołącz obszaru sudo — pełne CONTOSO100.COM -U'bob@CONTOSO100.COM'

    ![Dołączanie do obszaru](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-join.png)

Czy jest wyświetlany komunikat ("pomyślnie zarejestrowany machine w obszarze") po komputera pomyślnie jest dołączony do domeny zarządzane.


## <a name="verify-domain-join"></a>Weryfikowanie domeny sprzężenia
Można szybko sprawdzić, czy komputer został pomyślnie dołączony do zarządzanych domeny. Nawiązywanie połączenia z nowo domeny sprzężone RHEL maszyn wirtualnych przy użyciu SSH i konta domeny, a następnie zaznacz, aby sprawdzić, czy konto użytkownika został rozwiązany poprawnie.

1. W swojej Kit terminal, wpisz następujące polecenie, aby nawiązać połączenie nowo domeny sprzężone RHEL maszyn wirtualnych przy użyciu SSH. Za pomocą konta domeny, które należy do domeny zarządzanych (na przykład 'bob@CONTOSO100.COM' w tym przypadku.)

    SSH -l bob@CONTOSO100.COM contoso rhel.cloudapp.net

2. W swojej Kit terminal wpisz następujące polecenie, aby wyświetlić, jeśli katalogu macierzystego został zainicjowany poprawnie.

    pwd

3. W swojej Kit terminal wpisz następujące polecenie, aby wyświetlić, jeśli członkostwa w grupach są rozpoznawane poprawnie.

    Identyfikator

Przykładowe dane wyjściowe tych poleceń są następujące:

![Weryfikowanie domeny sprzężenia](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-verify-domain-join.png)


## <a name="troubleshooting-domain-join"></a>Rozwiązywanie problemów z Dołączanie domeny
Zapoznaj się z artykułem [Dołączanie domeny Rozwiązywanie problemów](active-directory-ds-admin-guide-join-windows-vm.md#troubleshooting-domain-join) .


## <a name="related-content"></a>Zawartość pokrewna
- [Azure usługach domenowych AD - przewodnik wprowadzenie](./active-directory-ds-getting-started.md)

- [Dołączanie do maszyny wirtualnej systemu Windows Server w domenie usług domenowych AD Azure zarządzanych](active-directory-ds-admin-guide-join-windows-vm.md)

- [Jak zalogować się do maszyny wirtualnej systemem Linux](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md).

- [Instalowanie Kerberos](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Managing_Smart_Cards/installing-kerberos.html)

- [Czerwone funkcję Enterprise Linux 7 - przewodnik integracji systemu Windows](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)
