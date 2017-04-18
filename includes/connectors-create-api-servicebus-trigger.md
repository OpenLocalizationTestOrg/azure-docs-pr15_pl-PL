Poniżej przedstawiono sposób inicjowania przepływu pracy aplikacji logika po wysłaniu nowego elementu do kolejki Bus usługi za pomocą wyzwalacza **Usługi Bus - po odebraniu wiadomości w kolejce** .  

>[AZURE.NOTE]Pojawi się zaloguj się przy użyciu usługi Bus ciąg połączenia, jeśli nie zostały już utworzone połączenie Bus usługi.  

1. W polu wyszukiwania w Projektancie aplikacji logika wprowadź **bus usługi**. Następnie wybierz pozycję wyzwalacz **Bus usługi — po odebraniu wiadomości w kolejce** .  
![Obraz wyzwalacza Bus usługi 1](./media/connectors-create-api-servicebus/trigger-1.png)   
- Zostanie wyświetlone okno dialogowe **odebraniu wiadomości w kolejce** .  
![Obraz wyzwalacza Bus usługi 2](./media/connectors-create-api-servicebus/trigger-2.png)   
- Wprowadź nazwę kolejki usługi Bus potem wyzwalacza monitorowanie.   
![Obraz wyzwalacza Bus usługi 3](./media/connectors-create-api-servicebus/trigger-3.png)   

W tym momencie aplikacji logiczny został skonfigurowany z wyzwalacza. Po odebraniu nowego elementu w kolejce wybranego wyzwalacza rozpocznie Uruchom wyzwalaczy i akcje w ramach przepływu pracy.    
