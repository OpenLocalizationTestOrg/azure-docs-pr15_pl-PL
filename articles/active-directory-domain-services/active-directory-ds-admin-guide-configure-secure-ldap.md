<properties
    pageTitle="Konfigurowanie bezpiecznego protokołu LDAP (LDAPS) w usługach domenowych AD Azure | Microsoft Azure"
    description="Konfigurowanie bezpiecznego protokołu LDAP (LDAPS) dla domeny zarządzanych usług domenowych AD Azure"
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
    ms.date="09/21/2016"
    ms.author="maheshu"/>

# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Konfigurowanie bezpiecznego protokołu LDAP (LDAPS) dla domeny zarządzanych usług domenowych AD Azure
W tym artykule pokazano, jak można włączyć bezpiecznego Lightweight Directory programu Access Protocol (protokół) (LDAPS) dla swojej domeny zarządzanych usług domenowych AD Azure. Bezpiecznego protokołu LDAP jest nazywany "LDAP Lightweight Directory Access Protocol () na warstwy SSL (Secure Sockets) i zabezpieczeń TLS (Transport Layer)".

## <a name="before-you-begin"></a>Przed rozpoczęciem
Aby wykonać zadania, wymienione w tym artykule, należy następująco:

1. Ważna **Subskrypcja Azure**.

2. Usługi **katalogowej Azure AD** -, albo zsynchronizowane z katalogu lokalnego lub w katalogu tylko do chmury.

3. **Usługi Azure AD domeny** musi być włączona dla katalogu Azure AD. Jeśli nie zostało to zrobione, wykonaj wszystkie zadania opisane w [Przewodnik wprowadzający dla](./active-directory-ds-getting-started.md).

4. **Certyfikat może być używany w celu umożliwienia bezpiecznego protokołu LDAP**.
    - **Zalecane** — uzyskiwanie certyfikatu z urzędu certyfikacji przedsiębiorstwa lub publiczny urząd certyfikacji. Ta opcja konfiguracji jest bezpieczniejsze.
    - Alternatywnie można też utworzyć [certyfikat z podpisem własnym](#task-1---obtain-a-certificate-for-secure-ldap) jak pokazano w dalszej części tego artykułu.

<br>

### <a name="requirements-for-the-secure-ldap-certificate"></a>Wymagania dotyczące zabezpieczeń certyfikatu LDAP
Uzyskiwanie certyfikat na tych wskazówek, przed włączeniem bezpiecznego protokołu LDAP. Wystąpienia awarii przy próbie Włączanie bezpiecznego protokołu LDAP zarządzanych domeny z certyfikatem nieprawidłowy niepoprawne.

1. **Zaufane wystawcy** - certyfikat musi wystawiony przez urząd zaufane na komputerach, które trzeba połączyć się z domeną przy użyciu bezpiecznego protokołu LDAP. Ten urząd może być urząd certyfikacji przedsiębiorstwa Twojej organizacji lub publiczny urząd certyfikacji zaufany przez tych komputerów.

2. **Czas użytkowania** - certyfikat musi być prawidłowa dla co najmniej następnego 3 do 6 miesięcy. Bezpieczny dostęp LDAP z domeną zarządzanych zostanie zerwane po wygaśnięciu certyfikatu.

3. **Nazwa** - nazwa certyfikatu musi być symbol wieloznaczny zarządzanych domeny. Na przykład jeśli domeny ma nazwę "contoso100.com", nazwy podmiotu certyfikatu musi być "*. contoso100.com". Skonfiguruj nazwę DNS (alternatywna nazwa tematu) do tej nazwy symboli wieloznacznych.

3. **Użycie klucza** - certyfikat musi być skonfigurowany dla następujących używa - szyfrowanie klucza i podpisy cyfrowe.

4. **Cel certyfikatu** - certyfikat musi być używany do uwierzytelniania serwera protokołu SSL.

<br>

## <a name="task-1---obtain-a-certificate-for-secure-ldap"></a>Zadanie 1 — uzyskiwanie certyfikatu dla bezpiecznego protokołu LDAP
Pierwsze zadanie polega na uzyskiwanie certyfikatu na potrzeby bezpiecznego dostępu LDAP do zarządzanych domeny. Dostępne są dwie opcje:

- Uzyskiwanie certyfikatu przez urząd certyfikacji. Urząd może być certyfikacji przedsiębiorstwa Twojej organizacji lub publiczny urząd certyfikacji.

- Tworzenie certyfikatu z podpisem własnym.


### <a name="option-a-recommended---obtain-a-secure-ldap-certificate-from-a-certification-authority"></a>Opcja (zalecane) — uzyskiwanie certyfikatu bezpiecznego LDAP przez urząd certyfikacji
Jeśli w organizacji wdrożono enterprise infrastruktury publicznych (), musisz uzyskać certyfikat z urząd certyfikacji (CA) dla swojej organizacji. Jeśli Twoja organizacja uzyskuje jego certyfikaty z publiczny urząd certyfikacji, musisz uzyskać bezpieczny certyfikatu LDAP z tym publiczny urząd certyfikacji.

Podczas żądania certyfikatu, upewnij się, czy postępujesz wymagania przedstawione w [Wymaganie bezpiecznego certyfikatu LDAP](#requirements-for-the-secure-ldap-certificate).

> [AZURE.NOTE] Komputery, które trzeba połączyć się z domeną zarządzane za pomocą bezpiecznego protokołu LDAP musi ufać wystawcy certyfikatu LDAPS.


### <a name="option-b---create-a-self-signed-certificate-for-secure-ldap"></a>Opcja B — Tworzenie certyfikatu z podpisem własnym dla bezpiecznego protokołu LDAP
Można utworzyć certyfikat z podpisem własnym dla bezpiecznego protokołu LDAP, jeśli:

- certyfikaty w Twojej organizacji nie są wydawane przez urząd certyfikacji przedsiębiorstwa lub
- nie będzie używany certyfikat z publiczny urząd certyfikacji.

**Tworzenie certyfikatu z podpisem własnym przy użyciu programu PowerShell**

Na komputerze z systemem Windows otwórz nowe okno programu PowerShell jako **Administrator** , a następnie wpisz następujące polecenia, aby utworzyć nowy certyfikat z podpisem własnym.

    $lifetime=Get-Date

    New-SelfSignedCertificate -Subject *.contoso100.com -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment -Type SSLServerAuthentication -DnsName *.contoso100.com

W poprzednim przykładzie Zamień "contoso100.com" z nazwą domeny DNS domeny zarządzanych usług domenowych AD Azure.

![Wybierz katalog Azure AD](./media/active-directory-domain-services-admin-guide/secure-ldap-powershell-create-self-signed-cert.png)

Nowo utworzony certyfikat z podpisem własnym jest umieszczany w magazynie certyfikatów na komputerze lokalnym.


## <a name="task-2---export-the-secure-ldap-certificate-to-a-pfx-file"></a>Świadectwo zadania 2 — Eksport bezpiecznego protokołu LDAP. Plik PFX
Przed rozpoczęciem tego zadania upewnij się, że otrzymali bezpiecznego świadectwo LDAP z urząd certyfikacji lub publiczny urząd certyfikacji, lub utworzono certyfikat z podpisem własnym.

Wykonaj następujące czynności, aby wyeksportować certyfikat LDAPS. Plik PFX.

1. Naciśnij przycisk **Start** , a następnie wpisz **R**. W oknie dialogowym **Uruchamianie** wpisz **mmc** , a następnie kliknij **przycisk OK**.

    ![Uruchamianie konsoli MMC](./media/active-directory-domain-services-admin-guide/secure-ldap-start-run.png)

2. W monicie **Kontrola konta użytkownika** kliknij przycisk **Tak,** Aby uruchomić konsoli MMC (Microsoft Management Console) jako administrator.

3. W menu **plik** kliknij polecenie **Dodaj lub usuń przyciągania-w...**.

    ![Dodawanie przystawki do konsoli MMC](./media/active-directory-domain-services-admin-guide/secure-ldap-add-snapin.png)

4. W oknie dialogowym **Dodawanie lub usuwanie przystawki** , wybierz pozycję przystawki **Certyfikaty** , a następnie kliknij pozycję **Dodaj >** przycisk.

    ![Dodawanie przystawki Certyfikaty do konsoli MMC](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin.png)

5. W kreatorze **przystawki Certyfikaty** zaznacz **konto** , a następnie kliknij przycisk **Dalej**.

    ![Dodawanie przystawki Certyfikaty dla konta komputera](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-computer-account.png)

6. Na stronie **Wybieranie komputera** wybierz **komputer lokalny: (komputer konsoli pracuje)** i kliknij przycisk **Zakończ**.

    ![Dodawanie przystawki Certyfikaty — wybierz ikonę komputer](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-local-computer.png)

7. W oknie dialogowym **Dodawanie lub usuwanie przystawki** kliknij **przycisk OK** , aby dodać przystawki Certyfikaty do konsoli MMC.

    ![Dodawanie przystawki Certyfikaty do konsoli MMC - gotowe](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin-done.png)

8. W oknie konsoli MMC kliknij, aby rozwinąć **Katalog główny konsoli**. Powinien zostać wyświetlony przystawki Certyfikaty załadowane. Kliknij pozycję **Certyfikaty (komputer lokalny)** , aby rozwinąć. Kliknij, aby rozwinąć węzeł **osobistej** następuje węzeł **Certyfikaty** .

    ![Magazynie certyfikatów osobistych Otwórz](./media/active-directory-domain-services-admin-guide/secure-ldap-open-personal-store.png)

9. Powinien zostać wyświetlony utworzonego certyfikatu z podpisem własnym. Można sprawdzić właściwości certyfikatu, aby upewnić się, że odcisku palca odpowiada zgłoszone w systemie windows PowerShell, podczas tworzenia certyfikatu.

10. Wybierz certyfikat z podpisem własnym, a następnie **kliknij prawym przyciskiem myszy**. W menu podręcznym wybierz **Wszystkie zadania** , a następnie wybierz pozycję **Eksportuj**.

    ![Eksportowanie certyfikatu](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert.png)

11. W oknie dialogowym **Kreatora eksportu certyfikatów**kliknij przycisk **Dalej**.

    ![Kreator eksportu certyfikatu](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert-wizard.png)

12. Na stronie **Eksportowanie klucza prywatnego** wybierz pozycję **Tak, Eksportuj klucz prywatny**, a następnie kliknij przycisk **Dalej**.

    ![Eksportowanie certyfikatu klucz prywatny](./media/active-directory-domain-services-admin-guide/secure-ldap-export-private-key.png)

    > [AZURE.WARNING] Należy wyeksportować klucz prywatny wraz z certyfikatu. Jeśli podasz PFX, który nie zawiera klucz prywatny certyfikatu, włączanie bezpiecznego protokołu LDAP zarządzanych domeny nie powiedzie się.

13. Na stronie **Format pliku eksportu** wybierz **Wymiana informacji osobistych - PKCS #12 (. PFX)** jako format pliku dla eksportowanego certyfikatu.

    ![Eksportowanie certyfikatu formatu pliku](./media/active-directory-domain-services-admin-guide/secure-ldap-export-to-pfx.png)

    > [AZURE.NOTE] Tylko. Format pliku PFX jest obsługiwany. Nie wyeksportowany certyfikat. Format pliku CER.

14. Na **stronie,** wybierz opcję **hasło** i wpisz w polu Hasło ochrony. Plik PFX. Pamiętaj to hasło, ponieważ będzie potrzebna do wykonywania następnego zadania. Kliknij przycisk **Dalej** , aby kontynuować.

    ![Hasło dla eksportowanie certyfikatu ](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-password.png)

    > [AZURE.NOTE] Zanotuj to hasło. Potrzebujesz podczas włączania bezpiecznego protokołu LDAP dla tej domeny zarządzanych w [3 zadania — Włączanie bezpiecznego protokołu LDAP dla zarządzanych domeny](#task-3---enable-secure-ldap-for-the-managed-domain)

15. Na stronie **Eksport pliku** Określ nazwę pliku i lokalizację, w której chcesz wyeksportować certyfikat.

    ![Ścieżka dla eksportowanie certyfikatu](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-path.png)

16. Na następnej stronie kliknij przycisk **Zakończ** , aby wyeksportować certyfikat do pliku PFX. Okno dialogowe potwierdzenia powinny być widoczne po wyeksportowaniu certyfikatu.

    ![Wyeksportuj certyfikat gotowe](./media/active-directory-domain-services-admin-guide/secure-ldap-exported-as-pfx.png)


## <a name="task-3---enable-secure-ldap-for-the-managed-domain"></a>Zadanie 3 — Włączanie bezpiecznego protokołu LDAP dla zarządzanych domeny
Aby włączyć bezpiecznego protokołu LDAP, wykonaj następujące czynności konfiguracyjne:

1. Przejdź do **[portalu klasyczny Azure](https://manage.windowsazure.com)**.

2. Wybierz węzeł **Usługi Active Directory** w okienku po lewej stronie.

3. Wybierz katalog Azure AD (także nazywane "dzierżawy"), dla których włączono usług domenowych AD Azure.

    ![Wybierz katalog Azure AD](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. Kliknij kartę **Konfiguruj** .

    ![Konfigurowanie karty katalogu](./media/active-directory-domain-services-getting-started/configure-tab.png)

5. Przewiń do sekcji **usług domenowych**. Powinna być widoczna opcja zatytułowany **Bezpiecznego protokołu LDAP (LDAPS)** , jak pokazano w poniższej zrzut ekranu:

    ![Sekcja konfiguracji usług domeny](./media/active-directory-domain-services-admin-guide/secure-ldap-start.png)

6. Kliknij przycisk **Konfiguruj certyfikat...** , aby wyświetlić okno dialogowe **Konfigurowanie certyfikatu bezpiecznego protokołu LDAP** .

    ![Konfigurowanie certyfikatu dla bezpiecznego protokołu LDAP](./media/active-directory-domain-services-admin-guide/secure-ldap-configure-cert-page.png)

7. Kliknij ikonę folderu poniżej **Certyfikatu z pliku PFX** Określ plik PFX, który zawiera certyfikat, który chcesz używać bezpiecznego dostępu LDAP do zarządzanych domeny. Również wprowadź hasło ustawione podczas eksportowania certyfikatu do pliku PFX. Następnie kliknij przycisk Gotowe, u dołu.

    ![Określ bezpieczny plik LDAP PFX oraz hasło](./media/active-directory-domain-services-admin-guide/secure-ldap-specify-pfx.png)

8. Sekcja **usług domenowych** na karcie **Konfigurowanie** uzyskiwanie wyszarzony i znajdują się w **Oczekiwanie...** stanu przez kilka minut. W tym okresie certyfikat LDAPS został zweryfikowany dokładności i bezpiecznego protokołu LDAP jest konfigurowany zarządzanych domeny.

    ![Bezpieczny protokół LDAP - stanie oczekiwania](./media/active-directory-domain-services-admin-guide/secure-ldap-pending-state.png)

    > [AZURE.NOTE] Trwa około 10 do 15 minut umożliwiające bezpiecznego protokołu LDAP zarządzanych domeny. Jeśli dostarczonych bezpiecznego certyfikat LDAP nie ma odpowiednika wymagane kryteria, bezpiecznego protokołu LDAP nie jest włączone dla katalogu i zostanie wyświetlony błąd. Jeśli na przykład nazwa domeny jest nieprawidłowa, certyfikat wygasł lub wygaśnie wkrótce itp.

9. Jeśli bezpiecznego protokołu LDAP pomyślnie jest włączona dla domeny zarządzanych **Oczekiwanie...** wiadomości powinien zniknąć. Odcisk palca certyfikatu wyświetlonego powinna być widoczna.

    ![Bezpieczny włączony protokół LDAP](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled.png)

<br>


## <a name="task-4---enable-secure-ldap-access-over-the-internet"></a>Zadanie 4 — Włączanie bezpiecznego dostępu LDAP przez internet
**Opcjonalnie zadania** — Jeśli nie planujesz do uzyskiwania dostępu do domeny zarządzane przez internet przy użyciu LDAPS, pominąć to zadanie konfiguracji.

Przed rozpoczęciem tego zadania upewnij się, że wykonano kroki opisane w [3 zadania](#task-3---enable-secure-ldap-for-the-managed-domain).

1. Powinna być widoczna opcja **Włącz bezpiecznego LDAP dostęp przez INTERNET** w **usługach domenowych** części na stronie **Konfigurowanie** . Ta opcja jest ustawiona na **nie** w domyślnie, ponieważ dostęp do Internetu do domeny zarządzane przez bezpiecznego protokołu LDAP jest domyślnie wyłączona.

    ![Bezpieczny włączony protokół LDAP](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled2.png)

2. Przełącznik **Włączanie dostępu bezpiecznego protokołu LDAP w Internecie** na wartość **Tak**. Kliknij przycisk **ZAPISZ** na dole panelu.
    ![Bezpieczny włączony protokół LDAP](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access.png)

3. Sekcja **usług domenowych** na karcie **Konfigurowanie** uzyskiwanie wyszarzony i znajdują się w **Oczekiwanie...** stanu przez kilka minut. Po pewnym czasie jest włączony dostęp do Internetu z domeną zarządzane przez bezpiecznego protokołu LDAP.

    ![Bezpieczny protokół LDAP - stanie oczekiwania](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access-pending-state.png)

    > [AZURE.NOTE] Aby włączyć dostęp do Internetu przez bezpiecznego protokołu LDAP dla swojej domeny zarządzanych trwa około 10 minut.

4. Po pomyślnym włączeniu bezpiecznego dostępu LDAP z domeną zarządzane przez internet **Oczekiwanie...** wiadomości powinien zniknąć. Powinien zostać wyświetlony zewnętrzny adres IP, który może być używany do uzyskiwania dostępu do katalogu nad LDAPS w polu **IP adres dla LDAPS dostępu zewnętrznego**.

    ![Bezpieczny włączony protokół LDAP](./media/active-directory-domain-services-admin-guide/secure-ldap-internet-access-enabled.png)

<br>

## <a name="task-5---configure-dns-to-access-the-managed-domain-from-the-internet"></a>Zadania 5 - konfigurowania systemu DNS, aby uzyskać dostęp do zarządzanych domeny z Internetu
**Opcjonalnie zadania** — Jeśli nie planujesz do uzyskiwania dostępu do domeny zarządzane przez internet przy użyciu LDAPS, pominąć to zadanie konfiguracji.

Przed rozpoczęciem tego zadania upewnij się, że wykonano kroki opisane w [4 zadania](#task-4---enable-secure-ldap-access-over-the-internet).

Po włączeniu bezpiecznego dostępu LDAP przez internet zarządzanych domeny, musisz zaktualizować DNS, tak aby komputerów klienckich można znaleźć tej domeny zarządzanych. Na koniec zadania 4 zewnętrzny adres IP jest wyświetlany na karcie **Konfigurowanie** **IP adres dla LDAPS dostępu zewnętrznego**.

Skonfigurować zewnętrznego dostawcy DNS, tak aby nazwy DNS domeny zarządzanych (na przykład "contoso100.com") wskazuje ten zewnętrzny adres IP. W naszym przykładzie trzeba utworzyć następujący wpis DNS:

    contoso100.com  -> 52.165.38.113

To wszystko — możesz już przystąpić do połączyć się z domeną zarządzane przez internet przy użyciu bezpiecznego protokołu LDAP.

> [AZURE.WARNING] Należy pamiętać, że komputery klienckie musi ufać wystawcy certyfikatu LDAPS należy mógł pomyślnie połączyć się z domeną zarządzane za pomocą LDAPS. Jeśli korzystasz z urzędu certyfikacji przedsiębiorstwa lub publicznie zaufany urząd certyfikacji, nie musisz wykonywać żadnych dodatkowych czynności, ponieważ te wystawców certyfikatów zaufania komputerów klienckich. Jeśli używasz certyfikatu z podpisem własnym, należy zainstalować publicznej część certyfikatu z podpisem własnym w magazynie certyfikatów zaufanych na komputerze klienckim.

<br>

## <a name="related-content"></a>Zawartość pokrewna

- [Administrowanie Azure AD domenie usług domenowych zarządzanych](active-directory-ds-admin-guide-administer-domain.md)
