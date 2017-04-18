### <a name="prerequisites"></a>Wymagania wstępne

- Konto [SFTP](https://en.wikipedia.org/wiki/SSH_File_Transfer_Protocol)  


Zanim będzie można używać konta SFTP w aplikacji logiczny, możesz zezwolić aplikacji logiki do łączenia się z kontem SFTP. Na szczęście można można to zrobić z poziomu aplikacji logiczny na Azure Portal.  

Poniżej przedstawiono czynności, aby zezwolić aplikacji logiki do łączenia się z kontem SFTP:  
1. Aby utworzyć połączenie z SFTP w Projektancie aplikacji logiczny, wybierz pozycję **Pokaż Microsoft zarządzane interfejsy API** na liście rozwijanej, a następnie w polu wyszukiwania wprowadź *SFTP* . Wybierz wyzwalacz **SFTP — podczas dodawania lub modyfikowania pliku** :  
![Obraz online połączenia SFTP 1](./media/connectors-create-api-sftp/sftp-1.png)  
2. Jeśli nie utworzono dowolne połączenia SFTP przed, zostanie wyświetlony monit informujący o podanie poświadczeń SFTP. Te poświadczenia są używane do Autoryzuj aplikacji logika nawiązywania połączenia z i uzyskać dostęp do swojego konta SFTP danych:  
![Obraz połączenia w trybie online SFTP 2](./media/connectors-create-api-sftp/sftp-2.png)  
3. Zwróć uwagę, połączenie została utworzona i teraz są bezpłatne kontynuowanie innych czynności w aplikacji logiczny:   
 ![Obraz online połączenia SFTP 3](./media/connectors-create-api-sftp/sftp-3.png) 
