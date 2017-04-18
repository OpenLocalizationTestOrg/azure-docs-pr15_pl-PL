#### <a name="prerequisites"></a>Wymagania wstępne
- Konto Azure; można utworzyć [bezpłatne konto](https://azure.microsoft.com/free)
- Konto [usługi OneDrive](https://www.microsoft.com/store/apps/onedrive/9wzdncrfj1p3) 

Zanim będzie można używać konta usługi OneDrive w aplikacji logiczny, zezwolić aplikacji logiki do łączenia się z kontem usługi OneDrive.  Możesz to zrobić łatwo w logiczny aplikacji portalu Azure. 

Zezwolić aplikacji logiki do łączenia się z kontem usługi OneDrive, wykonując następujące czynności:

1. Tworzenie aplikacji logicznych. W Projektancie logiki aplikacji wybierz pozycję **Pokaż Microsoft zarządzane interfejsy API** na liście rozwijanej, a następnie wprowadź "onedrive" w polu wyszukiwania. Wybierz jedną z wyzwalaczami lub akcje:  
  ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. Jeśli nie utworzono jeszcze wszystkie połączenia do usługi OneDrive, zostanie wyświetlony monit o Zaloguj się przy użyciu swoich poświadczeń usługi OneDrive:  
  ![](./media/connectors-create-api-onedrive/onedrive-2.png)
3. Wybierz pozycję **Zaloguj się**i wprowadź nazwę użytkownika i hasło. Wybierz pozycję **Zaloguj się**:  
  ![](./media/connectors-create-api-onedrive/onedrive-3.png)   

    Te poświadczenia są używane do zezwolić aplikacji logika nawiązywanie i uzyskać dostęp do danych na koncie usługi OneDrive. 
4. Wybierz pozycję **Tak,** Aby autoryzować aplikację logiczny, aby korzystać z konta usługi OneDrive:  
  ![](./media/connectors-create-api-onedrive/onedrive-4.png)   
5. Zwróć uwagę, że utworzono połączenie. Teraz kontynuować innych czynności w aplikacji logiczny:  
  ![](./media/connectors-create-api-onedrive/onedrive-5.png)
