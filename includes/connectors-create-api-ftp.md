### <a name="prerequisites"></a>Wymagania wstępne

- Konta [FTP](https://wikipedia.org/wiki/File_Transfer_Protocol)  


Zanim będzie można używać konta FTP w aplikacji logiczny, możesz zezwolić aplikacji logiki do łączenia się z kontem FTP. Na szczęście można to z zrobić w aplikacji logiczny na Azure Portal.  

Poniżej przedstawiono czynności, aby zezwolić aplikacji logiki do łączenia się z kontem FTP:  
1. Aby utworzyć połączenie z FTP, w Projektancie logiki aplikacji, wybierz pozycję **Pokaż Microsoft zarządzane interfejsy API** na liście rozwijanej, a następnie w polu wyszukiwania wprowadź *FTP* . Zaznacz wyzwalacz lub akcji będzie chcesz użyć:  
![Tworzenie połączenia FTP — krok](./media/connectors-create-api-ftp/ftp-1.png)  
2. Jeśli nie utworzono wszystkie połączenia FTP przed, zostanie wyświetlony monit informujący o podanie poświadczeń FTP. Te poświadczenia są używane do zezwolić aplikacji logika nawiązywania połączenia z i uzyskać dostęp do danych konta FTP:  
![Tworzenie połączenia FTP — krok](./media/connectors-create-api-ftp/ftp-2.png)  
3. Zwróć uwagę, połączenie została utworzona i teraz są bezpłatne kontynuowanie innych czynności w aplikacji logiczny:  
 ![Tworzenie połączenia FTP — krok](./media/connectors-create-api-ftp/ftp-3.png)  
