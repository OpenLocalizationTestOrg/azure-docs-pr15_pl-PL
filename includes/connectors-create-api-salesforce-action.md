Teraz, gdy dodano warunku, jego czas zrobić coś sekcja z danymi, które jest generowany przez wyzwalacza. Wykonaj poniższe czynności, aby dodać akcję **usług Salesforce — Pobierz obiekt** . Ta akcja otrzymają danych zawsze, gdy jest tworzony nowy potencjalny klient. Będzie również dodać drugą akcję, która będzie używać danych z usług Salesforce - Get Akcja obiektu do wysyłania wiadomości e-mail za pomocą łącznika w usłudze Office 365.  

Aby skonfigurować to akcji, należy podać następujące informacje. Można zauważyć jest łatwe w użyciu danych wygenerowane przez wyzwalacz jako danych wejściowych dla niektórych właściwości dla nowego pliku:

|Tworzenie właściwości pliku|Opis|
|---|---|
|Typ obiektu|Jest to typ usługi Salesforce obiekt, który Cię interesuje. Przykłady potencjalnego klienta, konto itp.|
|Identyfikator obiektu|Jest to identyfikator obiektu.|


1. Wybierz łącze **Dodaj akcję** . Ten zostanie otwarty pola wyszukiwania, gdzie możesz wyszukać żadnych działań możesz chcesz wykonać. W tym przykładzie są akcje usług Salesforce zainteresowania.      
![Obraz akcji usług SalesForce 1](./media/connectors-create-api-salesforce/action-1.png)  
- Wprowadź *usług salesforce* do wyszukiwania działania związane z usług salesforce.
- Wybierz pozycję **usługi Salesforce — Pobierz obiekt** jako akcję do wykonania.   **Uwaga**: pojawi się monit Aby autoryzować aplikacji logiki do uzyskiwania dostępu do konta usługi Salesforce, jeśli jeszcze nie tego wcześniej.    
![Obraz akcji usług SalesForce 2](./media/connectors-create-api-salesforce/action-2.png)    
- Zostanie wyświetlona sterowania **pobrać obiektu** .  
- Wybierz pozycję *prowadzić* jako typ obiektu.
- Zaznacz kontrolkę **Identyfikatora obiektu** .
- Wybierz pozycję **...** , aby rozwinąć listę stacji tokeny, które mogą być używane jako dane wejściowe do akcji.       
![Obraz działania usługi SalesForce 3](./media/connectors-create-api-salesforce/action-3.png)    
- Zostanie wyświetlona sterującego **Identyfikator potencjalnego klienta** .   
![Obraz akcji usług SalesForce 4](./media/connectors-create-api-salesforce/action-4.png)     
- Należy zauważyć, że token Identyfikatora potencjalny klient jest teraz w kontrolce identyfikator obiektu wskazująca Akcja obiektu Get wyszuka potencjalnego klienta z Identyfikatorem, który jest równe identyfikator potencjalny klient potencjalny klient, którego dotyczy ta aplikacja logicznych.  
![Obraz akcji usług SalesForce 5](./media/connectors-create-api-salesforce/action-5.png)  
- Zapisz swoją pracę. To wszystko, Akcja obiektu Get zostały dodane do aplikacji logicznych. Kontrolę nad obiekt Get powinna wyglądać następująco:    
![Obraz akcji usług SalesForce 6](./media/connectors-create-api-salesforce/action-6.png)  

Teraz, gdy dodano akcji uzyskiwania potencjalnego klienta, można zrobić coś interesujące z nowo utworzonego potencjalnego klienta. W przedsiębiorstwie można wysyłać wiadomości e-mail, aby powiadomić listy dystrybucyjnej utworzenia nowego potencjalnego klienta. Można użyć łącznika w usłudze Office 365, aby wysłać wiadomość e-mail z niektórych istotnych informacji z nowego obiektu potencjalnego klienta w usług Salesforce.  

1. Wybierz pozycję **Dodaj akcję** , a następnie wprowadź *adres e-mail* w formancie wyszukiwania. To filtruje akcje do tych, które są związane z wysyłaniem i odbieraniem wiadomości e-mail.  
- Wybierz element listy **Office 365 Outlook - wysyłanie wiadomości e-mail** . Jeśli jeszcze tego nie utworzone *połączenie* z kontem usługi Office 365, wyświetli się monit o wprowadź poświadczenia usługi Office 365, aby go utworzyć teraz. Po wykonaniu tych czynności, zostanie wyświetlona kontrolkę **Wysyłanie wiadomości e-mail** .        
![Obraz akcji usług SalesForce 7](./media/connectors-create-api-salesforce/action-7.png)  
- Wpisz adres e-mail, którą chcesz wysyłać wiadomości e-mail do w formancie **do** .
-  W formancie **temat** wprowadź *Nowy lider utworzone* — a następnie wybierz token *firmy* . Spowoduje to wyświetlenie pola *firmy* nowy potencjalny klient utworzony w usług Salesforce.  
-  W formancie **treści** można wybrać dowolny z tokenów z nowego obiektu potencjalnego klienta i można też wprowadzić tekst, niezależnie od chcesz wyświetlić w treści wiadomości e-mail. Oto przykład:  
![Obraz akcji usług SalesForce 8](./media/connectors-create-api-salesforce/action-8.png)   
- Zapisywanie przepływu pracy.  

To wszystko. Logika aplikacji zostało zakończone.  

Teraz możesz przetestować aplikacji logika: w usług Salesforce, Utwórz nowy potencjalnego klienta, który spełnia warunku została utworzona.  Jeśli zostały wykonane w pełni ten samouczek, po prostu Tworzenie potencjalnego klienta przy użyciu adresu e-mail, który zawiera *amazon.com* w nim. Po kilku sekundach powinno być wyzwalane aplikacji logiki i wyniki mogą wyglądać podobnie do następującej:  
![Obraz akcji usług SalesForce 9](./media/connectors-create-api-salesforce/action-9.png)  

