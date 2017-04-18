## <a name="download-install-and-register-the-azure-backup-agent"></a>Pobieranie, instalowanie i zarejestrować agent Azure Backup

Po utworzeniu magazynu kopii zapasowej Azure, należy zainstalować agenta na każdym komputery systemu Windows (Windows Server, klienta w systemie Windows, System Center Data Protection Manager serwera lub serwerze kopii zapasowej Azure) umożliwiającą wykonywanie kopii zapasowej danych i aplikacjom Azure.

1. Zaloguj się do [portalu zarządzania](https://manage.windowsazure.com/)

2. Kliknij pozycję **Usługi odzyskiwania**, a następnie wybierz magazynu kopii zapasowej, który chcesz zarejestrować na serwerze. Zostanie wyświetlona strona Szybki Start dla tego magazynu kopii zapasowej.

    ![Szybki start](./media/backup-install-agent/quickstart.png)

3. Na stronie Szybkie uruchamianie kliknij opcję **systemu Windows Server lub System Center Data Protection Manager lub klienta systemu Windows** w obszarze **Pobierz agenta**. Kliknij przycisk **Zapisz** , aby skopiować go do komputera lokalnego.

    ![Zapisywanie agenta](./media/backup-install-agent/agent.png)

4. Po zainstalowaniu agenta kliknij dwukrotnie MARSAgentInstaller.exe na uruchamianie instalacji agent Azure Backup. Wybierz folder instalacji i folder tymczasowy wymagane dla agenta. Lokalizacja pamięci podręcznej określony musi mieć wolnego miejsca, czyli co najmniej 5% danych kopii zapasowej.

5.  Jeśli korzystasz z serwera proxy do łączenia się z Internetem, na ekranie **konfiguracji serwera Proxy** , wprowadź szczegóły serwera proxy. Jeśli korzystasz z serwerem proxy z uwierzytelnianiem, wprowadź szczegóły nazwę i hasło użytkownika na tym ekranie.

6.  Agent Azure Backup instaluje program .NET Framework 4,5 i środowiska Windows PowerShell (Jeśli nie jest dostępna) w celu ukończenia instalacji.

7.  Po zainstalowaniu agenta kliknij przycisk **Kontynuuj, aby rejestracji** , aby kontynuować z przepływem pracy.

    ![Zarejestruj się](./media/backup-install-agent/register.png)

8. Na ekranie magazynu poświadczeń przejdź do i wybierz plik poświadczeń magazynu, który został wcześniej zostały pobrane.

    ![Magazyn poświadczeń](./media/backup-install-agent/vc.png)

    Plik magazynu poświadczeń jest prawidłowy tylko w przypadku 48 godzin (po pobraniu z portalu). Jeśli wystąpienia jakichkolwiek błędów w tym ekranu (np. "magazynu poświadczeń plik udostępniony wygasła"), zaloguj się do portalu Azure i ponownie pobrać plik magazynu poświadczeń.

    Upewnij się, że plik magazynu poświadczeń jest dostępna w miejscu, w którym mogą uzyskiwać dostęp do aplikacji do konfigurowania. W przypadku wystąpienia dostęp do powiązanych błędów, skopiuj plik magazynu poświadczeń do lokalizacji tymczasowej na tym komputerze i spróbuj jeszcze raz.

    W przypadku wystąpienia błędu nieprawidłowe magazynu poświadczeń (np. "nieprawidłowe magazynu poświadczeń dostarczonych") plik jest uszkodzony lub czy nie masz najnowsze poświadczeń skojarzonych z Usługa odzyskiwania. Powtórz operację po pobraniu nowego pliku magazynu poświadczeń w portalu. Ten błąd występuje zazwyczaj, jeśli użytkownik kliknie opcję **pobierania magazynu poświadczeń** w portalu Azure kolejno szybkie. W tym przypadku tylko drugi plik poświadczenie magazynu jest prawidłowy.

9. Na ekranie **Ustawienia szyfrowania** możesz wygenerować hasło lub podanie hasła (minimum 16 znaków). Pamiętaj zapisać hasło w bezpiecznym miejscu.

    ![Szyfrowanie](./media/backup-install-agent/encryption.png)

    > [AZURE.WARNING] Jeśli hasło jest utracone lub zapomniane; Microsoft nie pomoże w odzyskiwanie danych kopii zapasowej. Użytkownik końcowy właścicielem hasło szyfrowania, a nie ma wgląd hasło używane przez użytkownika końcowego. Zapisz plik w bezpiecznym miejscu, ponieważ jest wymagane podczas operacji odzyskiwania.

10. Po kliknięciu przycisku **Zakończ** komputer został pomyślnie zarejestrowany do magazyn i jest już rozpocząć wykonywanie kopii zapasowej Microsoft Azure.

11. Podczas używania autonomicznego kopia zapasowa Microsoft Azure można modyfikować ustawienia określony podczas przepływ pracy rejestracji, klikając opcję **Właściwości zmiany** w kopii zapasowej Azure mmc przystawki.

    ![Zmienianie właściwości](./media/backup-install-agent/change.png)

    Także podczas korzystania z Menedżera ochrony danych, można zmodyfikować ustawieniami podczas przepływ pracy rejestracji klikając polecenie **Konfiguruj** , wybierając **Online** na karcie **Zarządzanie** .

    ![Konfigurowanie Azure kopii zapasowej](./media/backup-install-agent/configure.png)
