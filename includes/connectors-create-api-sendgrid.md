### <a name="prerequisites"></a>Wymagania wstępne
- Konto [SendGrid](https://www.SendGrid.com/) 

Zanim będzie można używać konta SendGrid w aplikacji logiczny, możesz zezwolić aplikacji logiki do łączenia się z kontem SendGrid. Na szczęście można to z zrobić w aplikacji logiczny na Azure Portal. 

Poniżej przedstawiono czynności, aby zezwolić aplikacji logiki do łączenia się z kontem SendGrid:

1. Aby utworzyć połączenie z SendGrid, w Projektancie logiki aplikacji, wybierz pozycję **Pokaż Microsoft zarządzane interfejsy API** na liście rozwijanej, a następnie wprowadź *SendGrid* w polu wyszukiwania. Zaznacz wyzwalacz lub akcji będzie chcesz użyć:  
  ![SendGrid — krok 1](./media/connectors-create-api-sendgrid/sendgrid-1.png)
2. Jeśli nie utworzono wszystkie połączenia SendGrid przed, zostanie wyświetlony monit informujący o podanie poświadczeń SendGrid. Te poświadczenia są używane do zezwolić aplikacji logika nawiązywania połączenia z i uzyskać dostęp do swojego konta SendGrid danych:  
  ![SendGrid — krok 2](./media/connectors-create-api-sendgrid/sendgrid-2.png)
3. Zwróć uwagę, połączenie została utworzona i teraz są bezpłatne kontynuowanie innych czynności w aplikacji logiczny:  
  ![SendGrid — krok 3](./media/connectors-create-api-sendgrid/sendgrid-3.png)   
