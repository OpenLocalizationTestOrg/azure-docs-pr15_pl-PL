### <a name="prerequisites"></a>Wymagania wstępne
- Konto Twitter 

Zanim będzie można używać konta Twitter w aplikacji logiczny, możesz zezwolić aplikacji logiki do łączenia się z kontem w serwisie Twitter. Na szczęście można można to zrobić z poziomu aplikacji logiczny na Azure Portal. 

Poniżej przedstawiono czynności, aby zezwolić aplikacji logiki do łączenia się z kontem w serwisie Twitter:

1. Aby utworzyć połączenie z serwisem Twitter, w Projektancie aplikacji logiczny, wybierz pozycję **Pokaż Microsoft zarządzanych interfejsy API** na liście rozwijanej a następnie w polu wyszukiwania wprowadź *serwisu Twitter* . Zaznacz wyzwalacza lub akcji będzie chcesz użyć:  
  ![Obraz połączenia Twitter 0](./media/connectors-create-api-twitter/twitter-0.png)
2. Jeśli nie utworzono dowolne połączenia Twitter przed, zostanie wyświetlony monit informujący o podanie poświadczeń Twitter. Te poświadczenia są używane do zezwolić aplikacji logika nawiązania połączenia i uzyskiwać dostęp do danych konta Twitter:  
  ![Obraz połączenia Twitter 1](./media/connectors-create-api-twitter/twitter-1.png)  
3. Wprowadź Twitter nazwę użytkownika i hasło, aby zezwolić aplikacji logiczny:  
  ![Obraz połączenia Twitter 2](./media/connectors-create-api-twitter/twitter-2.png)  
4. Potwierdzenie Twojej zgody:  
  ![Obraz połączenia Twitter 3](./media/connectors-create-api-twitter/twitter-3.png)  
6. Zwróć uwagę, połączenie została utworzona i teraz są bezpłatne kontynuowanie innych czynności w aplikacji logiczny:  
  ![Obraz połączenia Twitter 4](./media/connectors-create-api-twitter/twitter-4.png)