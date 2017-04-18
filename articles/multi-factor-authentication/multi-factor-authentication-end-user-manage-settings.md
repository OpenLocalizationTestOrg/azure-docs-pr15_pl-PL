<properties
    pageTitle="Zarządzanie ustawieniami Weryfikacja dwuetapowa | Microsoft Azure"
    description="Zarządzanie korzystaniem uwierzytelnianie wieloskładnikowe Azure, włącznie ze zmianami swoje informacje kontaktowe i konfigurowanie urządzenia."
    services="multi-factor-authentication"
    keywords = "uwierzytelnianie wieloskładnikowe klienta, problem z uwierzytelnianiem, identyfikator korelacji"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="manage-your-settings-for-two-step-verification"></a>Zarządzanie ustawieniami na potrzeby weryfikacji dwuetapowej

Ten artykuł zawiera odpowiedzi na pytania dotyczące aktualizowania ustawień uwierzytelniania dwuetapowego weryfikacji lub wieloskładnikowe. Jeśli masz problemy z zalogowaniem się do swojego konta dotyczą [problemy z Weryfikacja dwuetapowa](multi-factor-authentication-end-user-troubleshoot.md) pomocy dotyczącej rozwiązywania problemów.


## <a name="where-to-find-the-settings-page"></a>Gdzie można znaleźć na stronie Ustawienia
W zależności od firmy konfiguracji uwierzytelnianie wieloskładnikowe Azure istnieje kilka miejsc, gdzie można zmienić ustawienia, takie jak Twój numer telefonu.

Jeśli administrator IT wysłane z określonego adresu URL lub kroki, aby zarządzać Weryfikacja dwuetapowa, wykonaj te instrukcje. W przeciwnym razie poniższe instrukcje powinna działać dla wszystkich innych. Jeśli wykonaj następujące kroki, ale nie widzisz te same opcje, oznacza to, że swojej pracy lub szkole dostosowania ich portalu. Poproś administratora łącza z portalem uwierzytelnianie wieloskładnikowe Azure.


1. Zaloguj się do [https://myapps.microsoft.com](https://myapps.microsoft.com)  
2. U góry wybierz **profil**.  
3. Wybierz pozycję **Weryfikacja dodatkowe zabezpieczenia**.  

    ![Moje aplikacje](./media/multi-factor-authentication-end-user-manage/myapps1.png)

4. Strona weryfikacji dodatkowe zabezpieczenia ładuje z ustawieniami.

    ![Proofup](./media/multi-factor-authentication-end-user-manage-myapps/proofup.png)


## <a name="i-want-to-change-my-phone-number-or-add-a-secondary-number"></a>Chcę zmienić mój numer telefonu lub dodać numer pomocniczej

Należy skonfigurować numer telefonu pomocniczej uwierzytelniania.  Ponieważ numeru telefonu podstawowego i urządzeń przenośnych aplikacji prawdopodobnie na tym samym telefonu, drugi numer jest jedynym sposobem można wrócić do swojego konta Jeśli Twój telefon zostanie utracony lub kradzież.

> [AZURE.NOTE]
> Jeśli nie mają dostęp do swojego numeru telefonu podstawowego i potrzebujesz pomocy do swojego konta, zobacz nasze tematy pomocy w [masz problemy z Weryfikacja dwuetapowa](multi-factor-authentication-end-user-troubleshoot.md).

**Aby zmienić swój numer telefonu podstawowego:**  

1. Na stronie Weryfikacja zwiększenia bezpieczeństwa zaznacz pole tekstowe z bieżącego numeru telefonu, a następnie ją edytować za pomocą nowego numeru telefonu.  
2. Wybierz przycisk **Zapisz**.  
3. Jest to numer używaną w opcjach preferowanej weryfikacji, należy sprawdzić nowy numer, przed zapisaniem go.  


**Aby dodać numer dodatkowy numer telefonu:**  

1. Na stronie Weryfikacja zwiększenia bezpieczeństwa zaznacz pole wyboru obok pozycji **telefonu alternatywny uwierzytelniania.**  
2. Wprowadź swój numer dodatkowy numer telefonu w polu tekstowym.  
3. Wybierz pozycję **Zapisz** i zmiany zostało zakończone.  


## <a name="how-do-i-clean-up-microsoft-authenticator-from-my-old-device-and-move-to-a-new-one"></a>Jak Oczyść Authenticator firmy Microsoft z urządzenia starych i przejście do nowego?
Gdy odinstalować aplikację na urządzeniu z systemem lub zresetuj urządzenie, nie powoduje usunięcia aktywacji na stronie tła. Należy używać z instrukcjami podanymi w [przenoszone do nowego urządzenia](multi-factor-authentication-microsoft-authenticator.md#how-to-move-to-the-new-microsoft-authenticator-app).

## <a name="next-steps"></a>Następne kroki
- Uzyskaj porady dotyczące rozwiązywania problemów i pomoc dotyczącą [problemów z Weryfikacja dwuetapowa](multi-factor-authentication-end-user-troubleshoot.md)
- Konfigurowanie [hasła aplikacji](multi-factor-authentication-end-user-app-passwords.md) żadnych aplikacji, które nie obsługują Weryfikacja dwuetapowa.
