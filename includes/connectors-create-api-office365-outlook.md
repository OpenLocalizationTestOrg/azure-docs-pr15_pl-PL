#### <a name="prerequisites"></a>Wymagania wstępne
- Konto Azure; można utworzyć [bezpłatne konto](https://azure.microsoft.com/free)
- Konto [usługi Office 365](https://office365.com)  

Przed rozpoczęciem korzystania z konta usługi Office 365 w aplikacji logiczny, Autoryzuj aplikacji logiki do łączenia się z kontem usługi Office 365. Możesz to zrobić łatwo w logiczny aplikacji na Azure portal.  

Zezwolić aplikacji logiki do łączenia się z kontem usługi Office 365, wykonując następujące czynności:

1. Tworzenie aplikacji logicznych. W Projektancie logiki aplikacji wybierz pozycję **Pokaż Microsoft zarządzane interfejsy API** na liście rozwijanej, a następnie wprowadź "office 365" w polu wyszukiwania. Wybierz jedną z wyzwalaczami lub akcje:  
    ![Krok tworzenia połączenie usługi Office 365](./media/connectors-create-api-office365-outlook/office365-sendemail.png)  

2. Jeśli nie utworzono jeszcze wszystkie połączenia do usługi Office 365, zostanie wyświetlony monit o Zaloguj się przy użyciu swoich poświadczeń usługi Office 365:  
    ![Krok tworzenia połączenie usługi Office 365](./media/connectors-create-api-office365-outlook/office365-signin.png)  

3. Wybierz pozycję **Zaloguj się**i wprowadź nazwę użytkownika i hasło. Wybierz pozycję **Zaloguj się**:  
    ![Krok tworzenia połączenie usługi Office 365](./media/connectors-create-api-office365-outlook/office365-usernamepassword.png)

    Te poświadczenia są używane do zezwolić aplikacji logika nawiązywanie i uzyskiwania dostępu do konta usługi Office 365. 

4. Zwróć uwagę, że utworzono połączenie. Teraz kontynuować innych czynności w aplikacji logiczny:   
    ![Krok tworzenia połączenie usługi Office 365](./media/connectors-create-api-office365-outlook/office365-sendemailproperties.png)  
  