### <a name="prerequisites"></a>Wymagania wstępne
- Konto [źródła](https://products.office.com/Project/project-online-with-project-for-office-365) 

Zanim będzie można używać źródła konta w aplikacji logiczny, możesz zezwolić aplikacji logiki do łączenia się z kontem źródła. Na szczęście można można to zrobić z poziomu aplikacji logiczny na Azure Portal. 

Poniżej przedstawiono czynności, aby zezwolić aplikacji logiki do łączenia się z kontem źródła:

1. Aby utworzyć połączenie źródła, w Projektancie logiki aplikacji, wybierz pozycję **Pokaż Microsoft zarządzane interfejsy API** na liście rozwijanej, a następnie w polu wyszukiwania wprowadź *źródła* . Zaznacz wyzwalacz lub akcji będzie chcesz użyć:  
  ![Źródła — krok 1](./media/connectors-create-api-projectonline/projectonline-1.png)
2. Jeśli nie utworzono dowolne połączenia źródła przed, zostanie wyświetlony monit informujący o podanie poświadczeń źródła. Te poświadczenia są używane do Autoryzuj aplikacji logika nawiązania połączenia i uzyskać dostęp do swojego konta źródła danych:  
  ![Źródła — krok 2](./media/connectors-create-api-projectonline/projectonline-2.png)
3. Wprowadź źródła nazwę użytkownika i hasło, aby zezwolić aplikacji logiczny:  
  ![Źródła — krok 3](./media/connectors-create-api-projectonline/projectonline-3.png)   
4. Zwróć uwagę, połączenie została utworzona i teraz są bezpłatne kontynuowanie innych czynności w aplikacji logiczny:  
  ![Źródła krok 4](./media/connectors-create-api-projectonline/projectonline-4.png)   
