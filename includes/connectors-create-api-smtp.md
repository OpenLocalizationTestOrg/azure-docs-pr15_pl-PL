### <a name="prerequisites"></a>Wymagania wstępne

- Konto [usługi SMTP](https://wikipedia.org/wiki/Simple_Mail_Transfer_Protocol)  


Zanim będzie można używać konta SMTP w aplikacji logiczny, możesz zezwolić aplikacji logiki do łączenia się z kontem usługi SMTP. Na szczęście można można to zrobić z poziomu aplikacji logiczny na Azure Portal.  

Poniżej przedstawiono czynności, aby zezwolić aplikacji logiki do łączenia się z kontem usługi SMTP:  
1. Aby utworzyć połączenie z SMTP, w Projektancie aplikacji logiczny, wybierz pozycję **Pokaż Microsoft zarządzane interfejsy API** na liście rozwijanej, a następnie w polu wyszukiwania wprowadź *SMTP* . Zaznacz wyzwalacz lub akcji będzie chcesz użyć:  
![](./media/connectors-create-api-smtp/smtp-1.png)  
2. Jeśli nie utworzono dowolne połączenia SMTP przed, zostanie wyświetlony monit informujący o podanie poświadczeń SMTP. Te poświadczenia są używane do zezwolić aplikacji logika nawiązywania połączenia z i uzyskać dostęp do danych konta SMTP:  
![](./media/connectors-create-api-smtp/smtp-2.png)  
3. Zwróć uwagę, połączenie została utworzona i teraz są bezpłatne kontynuowanie innych czynności w aplikacji logiczny:  
 ![](./media/connectors-create-api-smtp/smtp-3.png)  

