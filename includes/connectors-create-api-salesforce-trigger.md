W tym samouczek dowiesz się, jak za pomocą wyzwalacza **usług Salesforce — po utworzeniu obiektu** inicjowania przepływu pracy aplikacji logika po utworzeniu nowego potencjalnego klienta z usług Salesforce.

>[AZURE.NOTE]Zostanie wyświetlony monit informujący aby zalogować się do konta usługi Salesforce, jeśli nie została jeszcze utworzona *połączenia* usług Salesforce.  

1. Wprowadzanie *usług salesforce* w polu wyszukiwania w Projektancie aplikacji logiczny, a następnie wybierz wyzwalacza **usług Salesforce — po utworzeniu obiektu** .  
![Obraz wyzwalacza usług SalesForce 1](./media/connectors-create-api-salesforce/trigger-1.png)   
- **Po utworzeniu obiektu** sterowania jest wyświetlany.  
![Obraz wyzwalacza usług SalesForce 2](./media/connectors-create-api-salesforce/trigger-2.png)   
- Wybierz **Typ obiektu** , a następnie wybierz pozycję *potencjalnego klienta* z listy obiektów. W tym kroku są wskazującą, tworzysz wyzwalacz, który będzie wyświetlane powiadomienie o aplikacji logika przy każdym utworzeniu nowego potencjalnego klienta w usług Salesforce.   
![Obraz wyzwalacza usług SalesForce 3](./media/connectors-create-api-salesforce/trigger-3.png)   
- To wszystko. Została utworzona wyzwalacza. Jednak należy utworzyć co najmniej jedną akcję, aby nawiązać to aplikacji logika prawidłowe.    
![Obraz wyzwalacza usług SalesForce 4](./media/connectors-create-api-salesforce/trigger-4.png)   

W tym momencie aplikacji logiczny został skonfigurowany z wyzwalacz, który rozpocznie Uruchom wyzwalaczy i akcje w ramach przepływu pracy po utworzeniu nowego elementu w swojej usługi Salesforce.  