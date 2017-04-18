#### <a name="prerequisites"></a>Wymagania wstępne
- Konto Azure; można utworzyć [bezpłatne konto](https://azure.microsoft.com/free)
- Konta programu [Dynamics CRM Online](https://www.microsoft.com/en-us/dynamics/crm-free-trial-overview.aspx) 

Przed użyciem konta Dynamics w aplikacji logiczny, zezwolić aplikacji logiki do łączenia się z kontem CRM Online. Możesz to zrobić łatwo w logiczny aplikacji portalu Azure. 

Zezwolić aplikacji logiki do łączenia się z kontem CRM Online, wykonaj następujące czynności:

1. Tworzenie aplikacji logicznych. W Projektancie logiki aplikacji wybierz pozycję **Pokaż Microsoft zarządzane interfejsy API** na liście rozwijanej, a następnie wprowadź "dynamics" w polu wyszukiwania. Wybierz jedną z wyzwalaczami lub akcje:  
  ![](./media/connectors-create-api-crmonline/dynamics-triggers.png)
2. Jeśli nie utworzono jeszcze wszystkie połączenia Dynamics, zostanie wyświetlony monit o Zaloguj się przy użyciu swoich poświadczeń Dynamics:  
  ![](./media/connectors-create-api-crmonline/dynamics-signin.png)
3. Wybierz pozycję **Zaloguj się**i wprowadź nazwę użytkownika i hasło. Wybierz pozycję **Zaloguj**. 

    Te poświadczenia są używane do zezwolić aplikacji logika nawiązywanie i uzyskać dostęp do danych na swoim koncie Dynamics. 
4. Zwróć uwagę, że utworzono połączenie. Teraz kontynuować innych czynności w aplikacji logiczny:  
  ![](./media/connectors-create-api-crmonline/dynamics-properties.png)
