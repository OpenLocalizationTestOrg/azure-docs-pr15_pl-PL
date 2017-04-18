<properties
    pageTitle="Azure Active Directory Domain Services: Dołączanie do maszyn wirtualnych systemu Windows Server z domeną zarządzanych | Microsoft Azure"
    description="Dołączanie do maszyny wirtualnej Windows Server do usług domenowych AD Azure"
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

# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain"></a>Dołączanie do maszyny wirtualnej Windows Server z domeną zarządzanych

> [AZURE.SELECTOR]
- [Portal Azure klasyczny - systemu Windows](active-directory-ds-admin-guide-join-windows-vm.md)
- [PowerShell — systemu Windows](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)

<br>

W tym artykule pokazano, jak dołączyć do maszyny wirtualnej systemem Windows Server 2012 R2 w domenie zarządzanych usług domenowych AD Azure za pomocą portalu klasyczny Azure.


## <a name="step-1-create-the-windows-server-virtual-machine"></a>Krok 1: Tworzenie maszyny wirtualnej systemu Windows Server
Postępuj zgodnie z instrukcjami przedstawionych w samouczku [Tworzenie wirtualnych komputera z systemem Windows w portalu klasyczny Azure](../virtual-machines/virtual-machines-windows-classic-tutorial.md) . Jest ważne, aby zapewnić nowo utworzonej maszyn wirtualnych jest dołączony do tej samej sieci wirtualnych, w której są włączone usługi Azure AD domeny. Opcja "Szybkie tworzenie" nie pozwalają na połączenia wirtualnej sieci maszyny wirtualnej. W związku z tym należy użyć opcji "Z galerii" do tworzenia maszyny wirtualnej.

Wykonaj poniższe czynności, aby utworzyć dołączony do wirtualnej sieci, w której został włączony usług domenowych AD Azure maszyny wirtualnej systemu Windows.

1. W portalu klasyczny Azure, na pasku poleceń w dolnej części okna kliknij przycisk **Nowy**.

2. W obszarze **obliczenia**kliknij **maszyn wirtualnych**, a następnie kliknij pozycję **Z galerii**.

3. Pierwszy ekran umożliwia **Wybieranie obrazu z** komputera wirtualnych z listy dostępnych obrazów. Wybierz odpowiedni obraz.

    ![Wybieranie obrazu](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-image.png)

4. Drugi ekran umożliwia wybierz nazwę komputera, rozmiar i nazwę administratora i hasło. Korzystanie z poziomu i rozmiar wymagany, aby uruchomić aplikację lub Obciążenie pracą. Nazwa użytkownika, którego wybierz w tym miejscu jest użytkownika lokalnego administratora na komputerze. Nie wprowadzaj tutaj poświadczenia konta użytkownika domeny.

    ![Konfigurowanie maszyn wirtualnych](./media/active-directory-domain-services-admin-guide/create-windows-vm-config.png)

5. Trzecim ekranie umożliwia konfigurowanie zasobów dla sieci, magazynowania i dostępności. Upewnij się, że zaznaczasz wirtualnej sieci, w której są włączone usługi Azure AD domeny z listy rozwijanej **Region i koligacji grupy i wirtualnej sieci** . Określ **Nazwę DNS usługi Cloud** odpowiednio dla maszyny wirtualnej.

    ![Wybierz pozycję wirtualną sieć dla maszyn wirtualnych](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-vnet.png)

    > [AZURE.WARNING]
    Upewnij się, dołączanie do maszyny wirtualnej do tej samej sieci wirtualnej, w którym został włączony usług domenowych AD Azure. W wyniku maszyny wirtualnej można wyświetlić domeny i wykonywać zadania, takie jak dołączania do domeny. Jeśli chcesz utworzyć maszyny wirtualnej w różnych wirtualnej sieci nawiązać połączenia wirtualnej sieci wirtualnej sieci, w której został włączony usług domenowych AD Azure.

6. Czwarty ekran pozwala zainstalować agenta maszyn wirtualnych i skonfigurować niektóre dostępne rozszerzenia.

    ![Gotowe](./media/active-directory-domain-services-admin-guide/create-windows-vm-done.png)

7. Po utworzeniu maszyny wirtualnej portalu klasyczny Lista nowa maszyna wirtualna w węźle **maszyn wirtualnych** . Zarówno maszyn wirtualnych i usługa w chmurze są uruchamiane automatycznie, a jego stan jest wyświetlany jako **uruchomiony**.

    ![Wirtualna maszyna jest i rozpocząć pracę](./media/active-directory-domain-services-admin-guide/create-windows-vm-running.png)


## <a name="step-2-connect-to-the-windows-server-virtual-machine-using-the-local-administrator-account"></a>Krok 2: Łączenie maszyn wirtualnych systemu Windows Server przy użyciu lokalnego konta administratora
Teraz możemy Połącz nowo utworzony maszyn wirtualnych systemu Windows Server, aby dołączyć go do domeny. Użyj poświadczeń administratora lokalnego, określonym przez Ciebie podczas tworzenia maszyny wirtualnej, aby połączyć się z nim.

Wykonaj poniższe czynności, aby połączyć się z komputerem wirtualnych.

1. Przejdź do węzła **maszyn wirtualnych** w portalu klasyczny. Zaznacz maszyny wirtualnej utworzone w kroku 1, a następnie kliknij przycisk **Połącz** na pasku poleceń w dolnej części okna.

    ![Nawiązywanie połączenia z maszyn wirtualnych systemu Windows](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. Portalu klasycznym wyświetli monit o otwarcie lub zapisanie pliku z rozszerzeniem ".rdp", który jest używany do łączenia maszyn wirtualnych. Kliknij, aby otworzyć plik po zakończeniu pobierania.

3. Po wyświetleniu monitu logowania wprowadź **poświadczenia administratora lokalnego**, określonej podczas tworzenia maszyny wirtualnej. Na przykład możemy wykorzystano "localhost\mahesh" w tym przykładzie.

W tym momencie możesz powinny być rejestrowane nowo utworzonych maszyn wirtualnych systemu Windows przy użyciu poświadczeń administratora lokalnego. Następnym krokiem jest dołączyć do maszyny wirtualnej do domeny.


## <a name="step-3-join-the-windows-server-virtual-machine-to-the-aad-ds-managed-domain"></a>Krok 3: Dołączanie do maszyny wirtualnej Windows Server do usługi Katalogowej AAD domeny zarządzanych
Wykonaj poniższe czynności, aby dołączyć maszyny wirtualnej Windows Server do domeny zarządzanych AAD DS.

1. Nawiązywanie połączenia z systemu Windows Server, jak pokazano w kroku 2. Na ekranie startowym Otwórz **Menedżera serwera**.

2. Kliknij pozycję **Lokalny serwer** w okienku po lewej stronie okna Menedżer serwera.

    ![Uruchamianie Menedżera serwera maszyn wirtualnych](./media/active-directory-domain-services-admin-guide/join-domain-server-manager.png)

3. W sekcji **Właściwości** , kliknij pozycję **Grupa robocza** . Na stronie właściwości **Właściwości systemu** kliknij przycisk **Zmień** , aby dołączyć do domeny.

    ![Strona właściwości systemu](./media/active-directory-domain-services-admin-guide/join-domain-system-properties.png)

4. Określ nazwę domeny usługi Azure AD domeny zarządzane domeny w polu tekstowym **domeny** i kliknij **przycisk OK**.

    ![Określanie domeny, która ma być połączone](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-domain.png)

5. Zostanie wyświetlony monit o podanie poświadczeń, aby dołączyć do domeny. Upewnij się, możesz **określić poświadczenia dla użytkownika należącego do administratorów kontrolera domeny AAD** grupy. Tylko członkowie tej grupy uprawnień do połączenia komputerów zarządzanych domeny.

    ![Określ poświadczenia Dołączanie domeny](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-credentials.png)

6. Można określić poświadczeń w jeden z następujących sposobów:

    - Format głównej nazwy użytkownika: określenie sufiksu głównej nazwy użytkownika dla konta użytkownika, zgodnie z konfiguracją w Azure AD. W tym przykładzie jest sufiksu głównej nazwy użytkownika użytkownika "Robert" 'bob@domainservicespreview.onmicrosoft.com'.

    - SAMAccountName format: Określ nazwę konta, w formacie SAMAccountName. W tym przykładzie "Robert" użytkownik musi wprowadzić "CONTOSO100\bob".

        > [AZURE.NOTE] **Firma Microsoft zaleca używanie formatu UPN w celu określenia poświadczeń.** Atrybuty SAMAccountName może być generowane automatycznie jeśli prefiks głównej nazwy użytkownika dla użytkownika jest zbyt długi (na przykład "joereallylongnameuser"). Jeśli wielu użytkowników ma ten sam prefiks głównej nazwy użytkownika (na przykład "Robert") w dzierżawie usługi Azure AD, ich format SAMAccountName może być generowane automatycznie przez usługę. W tych przypadkach formatu UPN można prawidłowo zalogować się do domeny.

7. Po Dołączanie domeny zakończyło się pomyślnie, zostanie wyświetlony poniższy komunikat przyjmując należy do domeny. Uruchom ponownie komputer wirtualnych na zakończenie operacji dołączania do domeny.

    ![Domeny — Zapraszamy!](./media/active-directory-domain-services-admin-guide/join-domain-done.png)


## <a name="troubleshooting-domain-join"></a>Rozwiązywanie problemów z Dołączanie domeny
### <a name="connectivity-issues"></a>Problemy z łącznością
Jeśli maszyny wirtualnej nie może znaleźć domenę, zobacz następujące czynności rozwiązywania problemów:

- Upewnij się, że maszyny wirtualnej jest podłączony do tej samej sieci wirtualnej, który został włączony usług domenowych w. W przeciwnym razie maszyny wirtualnej nie może połączyć się z domeną i dlatego nie można dołączyć do domeny.

- Jeśli maszyny wirtualnej jest podłączone do innego wirtualną sieć, upewnij się, że ten wirtualnej sieci jest połączony wirtualnej sieci, w której został włączony usług domenowych.

- Spróbuj ping domeny używają tej nazwy domeny zarządzanych domeny (na przykład "ping contoso100.com"). Jeśli nie możesz to zrobić, spróbuj ping adresów IP dla domeny wyświetlane na stronie, jeśli włączono usług domenowych AD Azure (na przykład "ping 10.0.0.4"). Jeśli jest możliwe sprawdzić, czy adres IP, ale nie domenę, DNS może być niepoprawnie skonfigurowana. Może nie skonfigurowano adresów IP domeny jako serwery DNS sieci wirtualnej.

- Spróbuj opróżnianie pamięci podręcznej rozpoznawania nazw DNS na komputerze wirtualnych ("ipconfig/flushdns").

Jeśli pojawi się okno dialogowe z prośbą o podanie poświadczeń w celu dołączenia do domeny, nie masz problemy z łącznością.


### <a name="credentials-related-issues"></a>Problemy związane z poświadczeniami
Zapoznaj się z następujące kroki, jeśli występują problemy z poświadczeniami i nie można dołączyć do domeny.

- Spróbuj użyć formatu UPN, aby określić poświadczenia. Atrybuty SAMAccountName dla Twojego konta może być generowane automatycznie, jeśli jest wielu użytkowników prefiksem głównej nazwy użytkownika w dzierżawie usługi lub swojego Prefiks głównej nazwy użytkownika jest zbyt długa. W związku z tym format SAMAccountName dla Twojego konta może różnić się od czego się spodziewać lub w swojej domeny lokalnej za pomocą.

- Spróbuj użyć poświadczeń konta użytkownika należącego do grupy "Administratorzy AAD kontrolera domeny" do połączenia komputerów zarządzanych domeny.

- Upewnij się, że masz [włączony synchronizacji haseł](active-directory-ds-getting-started-password-sync.md) zgodnie z instrukcjami podanymi w podręczniku Wprowadzenie.

- Upewnij się, że używasz UPN użytkownika zgodnie z konfiguracją w Azure AD (na przykład 'bob@domainservicespreview.onmicrosoft.com') do zalogowania się.

- Upewnij się, że masz czas potrzebny długo na synchronizacji haseł do wykonania określonych w podręczniku Wprowadzenie.


## <a name="related-content"></a>Zawartość pokrewna

- [Azure usługach domenowych AD - przewodnik wprowadzenie](./active-directory-ds-getting-started.md)

- [Administrowanie Azure AD domenie usług domenowych zarządzanych](./active-directory-ds-admin-guide-administer-domain.md)
