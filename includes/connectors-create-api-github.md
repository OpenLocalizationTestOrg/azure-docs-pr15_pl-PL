### <a name="prerequisites"></a>Wymagania wstępne
- Konto [GitHub](http://GitHub.com) 

Zanim będzie można używać konta GitHub w aplikacji logiczny, możesz zezwolić aplikacji logiki do łączenia się z kontem GitHub. Na szczęście można można to zrobić z poziomu aplikacji logiczny na Azure Portal. 

Poniżej przedstawiono czynności, aby zezwolić aplikacji logiki do łączenia się z kontem GitHub:

1. Aby utworzyć połączenie z GitHub, w Projektancie logiki aplikacji, wybierz pozycję **Pokaż Microsoft zarządzane interfejsy API** na liście rozwijanej, a następnie wprowadź *GitHub* w polu wyszukiwania. Zaznacz wyzwalacz lub akcji będzie chcesz użyć:  
  ![](./media/connectors-create-api-github/github-1.png)
2. Jeśli nie utworzono wszystkie połączenia GitHub przed, zostanie wyświetlony monit informujący o podanie poświadczeń GitHub. Te poświadczenia są używane do zezwolić aplikacji logika nawiązania połączenia, a dostęp do danych konta GitHub:  
  ![](./media/connectors-create-api-github/github-2.png)
3. Wprowadź GitHub nazwę użytkownika i hasło, aby zezwolić aplikacji logiczny:  
  ![](./media/connectors-create-api-github/github-3.png)   
4. Potwierdzenie zamiaru:  
  ![](./media/connectors-create-api-github/github-4.png)   
5. Zwróć uwagę, że połączenia został utworzony w portalu. Teraz można przystąpić do tworzenia aplikacji logiki i używania GitHub w niej:   
  ![](./media/connectors-create-api-github/github-5.png)   
