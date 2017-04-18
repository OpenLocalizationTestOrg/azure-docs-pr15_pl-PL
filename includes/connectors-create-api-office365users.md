### <a name="prerequisites"></a>Wymagania wstępne

- Konta usługi [Office 365 dla użytkowników](https://office365.com)  


Zanim będzie można używać konta usługi Office 365 dla użytkowników w aplikacji logiczny, musi zezwolić aplikacji logiki do nawiązania połączenia z kontem usługi Office 365 dla użytkowników. Na szczęście można można to zrobić z poziomu aplikacji logiczny na Azure Portal.  

Poniżej przedstawiono czynności, aby zezwolić aplikacji logiki do łączenia się z kontem usługi Office 365 użytkowników:  
1. Aby utworzyć połączenie z użytkownikiem usługi Office 365, w Projektancie aplikacji logiczny, wybierz pozycję **Pokaż Microsoft zarządzane interfejsy API** na liście rozwijanej, a następnie w polu wyszukiwania wprowadź *Użytkownicy usługi Office 365* . Zaznacz wyzwalacz lub akcji będzie chcesz użyć:  
![Tworzenie połączenia użytkowników usługi Office 365 — krok](./media/connectors-create-api-office365users/office365users-1.png)  
2. Jeśli nie utworzono dowolne połączenia użytkownicy usługi Office 365 przed, zostanie wyświetlony monit informujący o podanie poświadczeń użytkownicy usługi Office 365. Te poświadczenia są używane do zezwolić aplikacji logika nawiązania połączenia, a dostęp do danych konta usługi Office 365 dla użytkowników:  
![Tworzenie połączenia użytkowników usługi Office 365 — krok](./media/connectors-create-api-office365users/office365users-2.png)  
3. Wprowadź użytkownicy usługi Office 365, nazwę użytkownika i hasło, aby zezwolić aplikacji logiczny:  
 ![Tworzenie połączenia użytkowników usługi Office 365 — krok](./media/connectors-create-api-office365users/office365users-3.png)  
4. Zwróć uwagę, połączenie została utworzona i teraz są bezpłatne kontynuowanie innych czynności w aplikacji logiczny:  
![Tworzenie połączenia użytkowników usługi Office 365 — krok](./media/connectors-create-api-office365users/office365users-4.png)  
  