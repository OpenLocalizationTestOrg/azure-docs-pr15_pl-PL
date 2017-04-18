W tym przykładzie I procedurach pokazano, jak za pomocą **Usługi SharePoint Online — po utworzeniu nowego elementu** wyzwalacza inicjowania przepływu pracy aplikacji logika po utworzeniu nowego elementu listy usługi SharePoint Online.

>[AZURE.NOTE]Zostanie wyświetlony monit informujący aby zalogować się do konta programu SharePoint, jeśli nie zostały już utworzone *połączenie* z usługi SharePoint Online.  

1. Wprowadzanie *programu sharepoint* w polu wyszukiwania w Projektancie aplikacji logiczny, a następnie wybierz wyzwalacza **Usługi SharePoint Online — po utworzeniu nowego elementu** .  
![SharePoint online wyzwalacza obrazu](./media/connectors-create-api-sharepointonline/trigger-1.png)  
- **Po utworzeniu nowego elementu** sterowania jest wyświetlany.  
![SharePoint online wyzwalacza obraz 2](./media/connectors-create-api-sharepointonline/trigger-2.png)   
- Wybierz **adres URL witryny**. To jest witryny programu SharePoint online, które chcesz monitorować dla nowych elementów wyzwalać przepływu pracy.  
![SharePoint online wyzwalacza obraz 3](./media/connectors-create-api-sharepointonline/trigger-3.png)   
- Wybierz **nazwę listy**. To jest lista w witrynie usługi SharePoint Online, które chcesz monitorować dla nowych elementów, wywołujących przepływu pracy.  
![SharePoint online wyzwalacza obraz 4](./media/connectors-create-api-sharepointonline/trigger-4.png)   

W tym momencie aplikacji logiczny został skonfigurowany z wyzwalacz, który rozpocznie Uruchom wyzwalaczy i akcje w ramach przepływu pracy. To nastąpi zawsze, gdy jest tworzony nowy element w wybranej listy usługi SharePoint Online.  