<properties
    pageTitle="Aplikacja Microsoft Authenticator dla telefonów komórkowych | Microsoft Azure"
    description="Dowiedz się, jak przeprowadzić uaktualnienie do najnowszej wersji uwierzytelnienia Azure."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="kgremban"/>

# <a name="microsoft-authenticator"></a>Microsoft uwierzytelnienia

Aplikacja Microsoft Authenticator zapewnia dodatkowy poziom zabezpieczeń na koncie Azure (na przykład bsimon@contoso.onmicrosoft.com), swoje lokalnie służbowym kontem (na przykład bsimon@contoso.com), lub konto Microsoft (na przykład bsimon@outlook.com).

Aplikacja działa w jeden z dwóch sposobów:

- **Powiadomienie**. Aplikacja może pomóc w zapobieganie nieupoważnionemu dostępowi do konta i zatrzymywania fałszywe transakcje przez naciśnięcie powiadomienia na telefonie smartphone lub tablecie. Po prostu Wyświetl powiadomienie, a jeśli jest godny zaufania, wybierz pozycję **Weryfikuj**. W przeciwnym razie możesz wybrać **Odmów**. Aby dowiedzieć się, jak odmowy odbierania powiadomień Zobacz jak używać funkcji oszustwa raportu i Odmów uwierzytelniania wieloskładnikowego.

- **Hasło kod weryfikacyjny**. Aplikacja umożliwia jako token oprogramowania Generuj kod weryfikacyjny OAuth. Wprowadź kod dostępne za pomocą aplikacji do ekranu logowania oraz nazwę użytkownika i hasło, po wyświetleniu monitu. Kod weryfikacyjny zawiera drugą formę uwierzytelniania.

W wersji aplikacji Microsoft Authenticator starą aplikację uwierzytelnienia Azure jest zastępowany.  Aplikacja uwierzytelnienia Azure będą nadal działać, ale jeśli zdecydujesz się na przejście do nowej aplikacji Microsoft Authenticator, w tym artykule mogą pomóc.  

## <a name="install-the-app"></a>Instalowanie aplikacji

Aplikacja Microsoft Authenticator jest dostępna dla [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)i [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

## <a name="add-accounts-to-the-app"></a>Dodawanie konta do aplikacji

Dla każdego konta, które chcesz dodać do aplikacji Authenticator firmy Microsoft użyj jednej z poniższych procedur.

### <a name="add-an-account-to-the-app-by-using-the-qr-code-scanner"></a>Dodawanie konta do aplikacji przy użyciu skanera kod QR

1. Przejdź do ekranu ustawienia zabezpieczeń weryfikacji.  Aby uzyskać informacje na temat uzyskiwania dostępu do tego ekranu zobacz [Zmienianie ustawień zabezpieczeń](multi-factor-authentication-end-user-manage-settings.md).

2. Wybierz pozycję **Konfigurowanie**.

    ![Przycisk Konfiguruj na ekranie Ustawienia Weryfikacja zabezpieczeń](./media/multi-factor-authentication-azure-authenticator/azureauthe.png)

    Spowoduje to wyświetlenie ekranu kod QR.

    ![Ekran, który zawiera kod QR](./media/multi-factor-authentication-azure-authenticator/barcode2.png)

3. Otwórz aplikację Microsoft uwierzytelnienia. Na ekranie **konta** wybierz pozycję **+**, a następnie określ, czy chcesz dodać konto służbowe.

    ![Ekran konta z znak plus](./media/multi-factor-authentication-azure-authenticator/addaccount3.png)

    ![Ekran określania konto służbowe](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan.png)

4. Zeskanuj kod QR, za pomocą aparatu fotograficznego, a następnie wybierz pozycję **Gotowe** , aby zamknąć ekran kod QR.

    ![Ekran skanowania kod QR](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan2.png)

    Jeśli kamera nie działa poprawnie, możesz ręcznie wprowadzić kod QR i adres URL. Aby uzyskać więcej informacji zobacz [Dodawanie konta do aplikacji ręcznie](#add-an-account-to-the-app-manually).

5. Poczekaj chwilę na konto zostało aktywowane. Po zakończeniu aktywacji wybierz **kontakt ja**.  To wyśle odbiorcy powiadomienie lub kod weryfikacyjny telefonu.  Wybierz pozycję **Sprawdź**.

    ![Ekran miejsce, w którym zaznacz opcję Weryfikuj, aby się zalogować](./media/multi-factor-authentication-end-user-first-time-mobile-app/verify.png)

6. Jeśli firma wymaga numeru PIN zatwierdzania weryfikacji logowania, wprowadź go.

    ![Pole służące do wprowadzania numeru PIN](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan3.png)

7. Po zakończeniu wprowadzania numeru PIN, wybierz pozycję **Zamknij**. W tym momencie weryfikacji powinien zakończyć się pomyślnie.
8. Zalecamy, wprowadź swój numer telefonu komórkowego, w przypadku, gdy utracisz dostęp do aplikacji. Określ swój kraj z listy rozwijanej, a następnie wprowadź numer swojego telefonu komórkowego w polu obok nazwy kraju. Wybierz przycisk **Dalej**.
9. W tym momencie skonfigurowano metodę kontaktu. Nadszedł czas, aby skonfigurować hasła aplikacji dla aplikacji innych niż przeglądarki, takich jak program Outlook 2010 lub starszym. Jeśli nie używasz te aplikacje, wybierz pozycję **Gotowe**. W przeciwnym razie przejdź do następnego kroku.

    ![Ekran tworzenia hasła](./media/multi-factor-authentication-end-user-first-time-mobile-app/step4.png)

10. Jeśli korzystasz z aplikacji innych niż przeglądarki, skopiuj hasło aplikacji przedstawionych i wkleić hasło aplikacji. Instrukcje dotyczące poszczególnych aplikacji, takich jak program Outlook i programu Lync Zobacz jak zmienić hasło w wiadomości e-mail do hasło aplikacji i jak zmienić hasło w aplikacji hasło aplikacji.
11. Wybierz pozycję **Gotowe**.

Powinien zostać wyświetlony nowego konta, na ekranie **konta** .

![Ekran konta](./media/multi-factor-authentication-azure-authenticator/accounts.png)

### <a name="add-an-account-to-the-app-manually"></a>Ręczne dodawanie konta do aplikacji

1. Przejdź do ekranu ustawienia zabezpieczeń weryfikacji.  Aby uzyskać informacje na temat uzyskiwania dostępu do tego ekranu zobacz [Zmienianie ustawień zabezpieczeń](multi-factor-authentication-end-user-manage-settings.md).

2. Wybierz pozycję **Konfigurowanie**.

    ![Przycisk Konfiguruj na ekranie Ustawienia Weryfikacja zabezpieczeń](./media/multi-factor-authentication-azure-authenticator/azureauthe.png)

    Spowoduje to wyświetlenie ekranu kod QR.  Zwróć uwagę na kod i adres URL.

    ![Ekran, który zawiera kod QR i adresu URL](./media/multi-factor-authentication-azure-authenticator/barcode2.png)

3. Otwórz aplikację Microsoft uwierzytelnienia. Na ekranie **konta** wybierz pozycję **+**, a następnie określ, czy chcesz dodać konto służbowe.

    ![Ekran konta z znak plus](./media/multi-factor-authentication-azure-authenticator/addaccount3.png)

    ![Ekran określania konto służbowe](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan.png)

4. Skanera wybierz pozycję **ręczne wprowadzenie kodu**.

    ![Ekran skanowania kod QR](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan2.png)

5. Wprowadź kod i adres URL w odpowiednich polach w aplikacji.

    ![Ekran wprowadzenie kodu oraz adres URL](./media/multi-factor-authentication-azure-authenticator/manual.png)

    ![Ekran wprowadzenie kodu oraz adres URL](./media/multi-factor-authentication-end-user-first-time-mobile-app/addaccount2.png)

6. Poczekaj chwilę na konto zostało aktywowane. Po zakończeniu aktywacji, zaznacz **kontakt ja**. To wyśle odbiorcy powiadomienie lub kod weryfikacyjny telefonu. Wybierz pozycję **Sprawdź**.

Powinien zostać wyświetlony nowego konta, na ekranie **konta** .

![Ekran konta](./media/multi-factor-authentication-azure-authenticator/accounts.png)

### <a name="add-an-account-to-the-app-by-using-touch-id"></a>Dodawanie konta do aplikacji przy użyciu Identyfikatora dotyku

Aplikacja Microsoft Authenticator systemie iOS obsługuje identyfikatora dotykowym.  Uwierzytelnianie wieloskładnikowe Azure umożliwia organizacjom Wymagaj numeru PIN dla urządzeń. Identyfikatorem dotknij iOS użytkowników, nie musisz wprowadzić kod PIN. Zamiast tego można zeskanować ich odcisku palca i wybierz pozycję **Zatwierdź**.

Konfigurowanie dotknij identyfikator z Authenticator Microsoft jest proste. Wykonywanie wezwanie normalny weryfikacji za pomocą numeru PIN. Jeśli urządzenie obsługuje identyfikator dotyku, Authenticator Microsoft skonfiguruje go automatycznie dla tego konta.

![Weryfikacja konfiguracji identyfikator dotyku](./media/multi-factor-authentication-azure-authenticator/touchid1.png)

Od tego miejsca, gdy jest wymagane do weryfikacji z logowaniem, wybierz powiadomień wypychanych otrzymanej oraz skanowanie usługi odcisku palca zamiast wprowadzać numeru PIN.

![Powiadomienia wypychane](./media/multi-factor-authentication-azure-authenticator/touchid2.png)

## <a name="uninstall-the-old-azure-authentication-app"></a>Odinstaluj starą aplikację uwierzytelniania Azure

Po dodaniu wszystkich kont do nowej aplikacji, możesz odinstalować starą aplikację za pomocą telefonu.

## <a name="delete-an-account"></a>Usuwanie konta

Aby usunąć konto z aplikacji Microsoft Authenticator, zaznacz konto, a następnie wybierz polecenie **Usuń**.

![Przycisk Usuń](./media/multi-factor-authentication-azure-authenticator/remove.png)
