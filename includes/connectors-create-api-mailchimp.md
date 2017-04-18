### <a name="prerequisites"></a>Wymagania wstępne
- Konto [MailChimp](https://www.MailChimp.com/) 

Zanim będzie można używać konta MailChimp w aplikacji logiczny, musi zezwolić aplikacji logiki do łączenia się z kontem MailChimp. Na szczęście można to z zrobić w aplikacji logiczny na Azure Portal. 

Poniżej przedstawiono czynności, aby zezwolić aplikacji logiki do łączenia się z kontem MailChimp:

1. Aby utworzyć połączenie z MailChimp, w Projektancie logiki aplikacji, wybierz pozycję **Pokaż Microsoft zarządzanych interfejsy API** na liście rozwijanej, a następnie wprowadź *MailChimp* w polu wyszukiwania. Zaznacz wyzwalacz lub akcji będzie chcesz użyć:  
  ![MailChimp — krok 1](./media/connectors-create-api-mailchimp/mailchimp-1.png)
2. Jeśli nie utworzono dowolne połączenia MailChimp przed, zostanie wyświetlony monit informujący o podanie poświadczeń MailChimp. Te poświadczenia są używane do zezwolić aplikacji logika nawiązania połączenia, a dostęp do danych konta MailChimp:  
  ![MailChimp — krok 2](./media/connectors-create-api-mailchimp/mailchimp-2.png)
3. Wprowadź MailChimp nazwę użytkownika i hasło, aby zezwolić aplikacji logiczny:  
  ![MailChimp — krok 3](./media/connectors-create-api-mailchimp/mailchimp-3.png)   
4. Zwróć uwagę, połączenie została utworzona i teraz są bezpłatne kontynuowanie innych czynności w aplikacji logiczny:  
  ![MailChimp krok 4](./media/connectors-create-api-mailchimp/mailchimp-4.png)
