## <a name="using-vault-credentials-to-authenticate-with-the-azure-backup-service"></a>Przy użyciu magazynu poświadczeń do uwierzytelniania za pomocą usługi Azure kopii zapasowej

Lokalnego serwera (klient systemu Windows lub Windows Server lub Data Protection Manager serwera) wymaga uwierzytelniania z kopii zapasowej magazynu przed jego tworzenia kopii zapasowych danych Azure. Uwierzytelnianie uzyskuje się za pomocą "magazynu poświadczeń". Koncepcja magazynu poświadczeń jest podobne do pojęcia "Ustawienia publikowania" plik, który jest używany w programie PowerShell Azure.

### <a name="what-is-the-vault-credential-file"></a>Co to jest plik magazynu poświadczeń?

Plik magazynu poświadczeń jest certyfikat wygenerowany przez portal dla każdej kopii zapasowej magazynu. Portalu wysyła następnie klucz publiczny do usługi kontroli dostępu (ACS). Klucz prywatny certyfikatu staje się dostępny dla użytkowników w ramach przepływu pracy, której wartość jest podawana jako dane wejściowe w przepływie pracy rejestracji komputera. To uwierzytelnianie komputera wysyłanie kopii zapasowych danych do określonych magazynu w usłudze Azure kopii zapasowej.

Poświadczenia magazynu jest używana tylko podczas przepływ pracy rejestracji. Jest odpowiedzialność użytkownika, aby upewnić się, że nie odczyta plików magazynu poświadczeń. Jeśli wypada w ręce każdy użytkownik rozpoczęcie pliku magazynu poświadczeń może służyć do rejestrowania innych komputerów przed tym samym magazynu. Jednak podczas danych kopii zapasowej jest zaszyfrowany przy użyciu hasła, który należy do klienta, nie złamane istniejące dane kopii zapasowej. Aby zmniejszyć ten problem, magazynu poświadczenia są ustawione tak, aby wygasało w 48hrs. Możesz pobrać magazynu poświadczeń kopii zapasowej magazynu dowolną liczbę razy — ale podczas rejestracji przepływu pracy dotyczy tylko najnowsze magazynu poświadczeń pliku.

### <a name="download-the-vault-credential-file"></a>Pobierz plik magazynu poświadczeń

Plik magazynu poświadczeń są pobierane za pośrednictwem bezpiecznego kanału w portalu Azure. Usługa Azure kopii zapasowej nie rozpoznaje klucz prywatny certyfikatu i klucz prywatny nie jest zachowywane w portalu lub usługi. Wykonaj następujące czynności, aby pobrać plik magazynu poświadczeń na komputer lokalny.

1.  Zaloguj się do [portalu zarządzania](https://manage.windowsazure.com/)
2.  Kliknij **Usługi odzyskiwania** w okienku nawigacji po lewej stronie i wybierz pozycję magazynu kopii zapasowej, które zostały utworzone. Kliknij ikonę chmury, aby przejść do widoku Szybki Start magazynu kopii zapasowej.

    ![Szybki podgląd](./media/backup-download-credentials/quickview.png)

3.  Na stronie Szybkie uruchamianie kliknij pozycję **Pobierz magazynu poświadczeń**. Portalu generuje do magazynu poświadczeń pliku, który jest udostępnienie do pobrania.

    ![Plik do pobrania](./media/backup-download-credentials/downloadvc.png)

4.  Portalu wygeneruje poświadczenie magazynu przy użyciu kombinacji nazwy magazynu i bieżącą datę. Kliknij przycisk **Zapisz** , aby pobrać magazynu poświadczeń do folderu pobierania lokalnego konta lub wybierz polecenie Zapisz jako w menu Zapisz, aby określić lokalizację magazynu poświadczeń.

### <a name="note"></a>Uwaga
- Upewnij się, że poświadczenia magazynu jest zapisany w miejscu, w których można uzyskać dostęp z komputera. Jeśli jest on przechowywany w pliku Udostępnij/SMB, sprawdź uprawnienia dostępu.
- Plik magazynu poświadczeń jest używany tylko podczas przepływ pracy rejestracji.
- Plik magazynu poświadczeń Dezaktualizuje się po 48hrs, którą można pobrać z portalu.
- Zapoznaj się z kopii zapasowej Azure [często zadawane pytania dotyczące](../articles/backup/backup-azure-backup-faq.md) wszelkie pytania w przepływie pracy.
