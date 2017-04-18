Teraz, gdy dodano wyzwalacz, jego czas zrobić coś sekcja z danymi, które jest generowany przez wyzwalacz. Wykonaj poniższe czynności, aby dodać akcję **Usługi SharePoint Online — Tworzenie pliku** . Ta akcja utworzy plik w usłudze SharePoint Online za każdym razem nowego elementu wyzwalacza. 

Aby skonfigurować to akcji, należy podać następujące informacje. Udostępnia te informacje, można zauważyć, że jest łatwe w użyciu danych wygenerowane przez wyzwalacz jako danych wejściowych dla niektórych właściwości dla nowego pliku:

|Tworzenie właściwości pliku|Opis|
|---|---|
|Adres URL witryny|To jest adres URL witryny usługi SharePoint Online, w którym chcesz utworzyć nowy plik. Wybierz witrynę z listy prezentowane.|
|Ścieżka folderu|Jest to folder (adresem URL wybrany w poprzednim kroku), gdzie umieścić nowy plik. Odszukaj i wybierz folder.|
|Nazwa pliku|Jest to nazwa tworzony plik.|
|Zawartość pliku|Wartości, które zostaną zapisane w pliku.|

1. Wybierz pozycję **+ Nowy krok** Aby dodać akcję.  
![SharePoint online akcji obraz 1](./media/connectors-create-api-sharepointonline/action-1.png)  
- Wybierz łącze **Dodaj akcję** . Ten zostanie otwarty pola wyszukiwania, gdzie możesz wyszukać żadnych działań możesz chcesz wykonać. W tym przykładzie akcje programu SharePoint są zainteresowania.    
![SharePoint online akcji obraz 2](./media/connectors-create-api-sharepointonline/action-2.png)    
- Wprowadź *programu sharepoint* , aby wyszukać działania związane z programu SharePoint.
- Wybierz pozycję **SharePoint Online — Tworzenie pliku** jako akcję do wykonania.   **Uwaga**: pojawi się monit Aby autoryzować aplikacji logiki do uzyskiwania dostępu do konta programu SharePoint, jeśli wcześniej nie zostały utworzone połączenie usługi SharePoint Online.    
![SharePoint online action obrazu 3](./media/connectors-create-api-sharepointonline/action-3.png)    
- Przycisk **Utwórz plik** zostanie otwarty.   
![SharePoint online action obrazu 4](./media/connectors-create-api-sharepointonline/action-4.png)     
- Zaznacz **Adres URL witryny** , a następnie przejdź do witryny, w której chcesz utworzyć plik.     
![SharePoint online action obrazu 5](./media/connectors-create-api-sharepointonline/action-5.png)  
- Zaznacz **ścieżkę folderu** , a następnie znajdź folder, w którym zostaną umieszczone nowy plik.  
![SharePoint online action obrazu 6](./media/connectors-create-api-sharepointonline/action-6.png)  
- Zaznacz kontrolkę, **nazwę pliku** i wprowadź nazwę pliku, który chcesz utworzyć. W tym miejscu można wprowadzić nazwę pliku bezpośrednio lub możesz użyć dowolnej właściwości wyzwalacza utworzonej wcześniej. W tym celu wybierz opcję Właściwości z listy **dane wyjściowe po utworzeniu nowego elementu**. Ta lista jest wyświetlania tylko po zaznaczeniu sterowania **nazwę pliku** . W tym walkthough jako nazwę pliku tworzone przez **Usługi SharePoint Online — Tworzenie pliku** akcji wybrano ID (identyfikator nowego elementu listy).    
![SharePoint online action obrazu 7](./media/connectors-create-api-sharepointonline/action-7.png)  
- Zaznacz kontrolkę **zawartości pliku** , a następnie wprowadź wartości, które zostaną zapisane w pliku, który zostanie utworzony. Zawartość pliku Zwróć uwagę, że możesz użyć dowolnej właściwości wyzwalacza utworzonej wcześniej. Po prostu wybierz właściwości z listy prezentowane. Można także wprowadzić tekst **zawartości pliku** bezpośrednio w formancie. W tym przykładzie wybrane niektóre właściwości i dodać spacje i łącznik między każdej właściwości.        
![SharePoint online action obrazu 8](./media/connectors-create-api-sharepointonline/action-8.png)  
- Zapisanie zmian do przepływu pracy  
- Gratulacje, teraz masz aplikację w pełni funkcjonalny logiczny, która zostanie wywołana podczas dodawania nowego elementu do listy usługi SharePoint Online. Aplikacja automatycznie utworzy plik za pomocą niektórych właściwości z nowego elementu listy.  Możesz go teraz przetestować przez utworzenie nowego elementu na liście programu SharePoint. 
