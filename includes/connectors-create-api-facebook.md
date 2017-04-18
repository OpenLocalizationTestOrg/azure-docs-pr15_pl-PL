### <a name="prerequisites"></a>Wymagania wstępne
- Konto w [serwisie Facebook](https://www.facebook.com/) 

Zanim będzie można używać z kontem w serwisie Facebook w aplikacji logiczny, możesz zezwolić aplikacji logiki do łączenia się z kontem w serwisie Facebook. Na szczęście można można to zrobić z poziomu aplikacji logiczny na Azure Portal. 

Poniżej przedstawiono czynności, aby zezwolić aplikacji logiki do łączenia się z kontem w serwisie Facebook:

1. Tworzenie połączenia z serwisem Facebook, w Projektancie aplikacji logiczny, na liście rozwijanej wybierz pozycję **Pokaż Microsoft zarządzane interfejsy API** , a następnie w polu wyszukiwania wprowadź *Facebook* . Zaznacz wyzwalacz lub akcji będzie chcesz użyć:  
  ![Facebook — krok 1](./media/connectors-create-api-facebook/facebook-1.png)
2. Jeśli nie utworzono wszystkie połączenia z serwisem Facebook przed, zostanie wyświetlony monit informujący o podanie poświadczeń Facebook. Te poświadczenia są używane do Autoryzuj aplikacji logika nawiązania połączenia, a dostęp do danych konta serwisu Facebook:  
  ![Facebook — krok 2](./media/connectors-create-api-facebook/facebook-2.png)
3. Wprowadź Facebook nazwę użytkownika i hasło, aby zezwolić aplikacji logiczny:  
  ![Facebook — krok 3](./media/connectors-create-api-facebook/facebook-3.png)   
4. Zwróć uwagę, połączenie została utworzona i teraz są bezpłatne kontynuowanie innych czynności w aplikacji logiczny:  
  ![Facebook krok 4](./media/connectors-create-api-facebook/facebook-4.png)   
