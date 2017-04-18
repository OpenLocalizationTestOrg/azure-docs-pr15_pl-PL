Teraz, gdy dodano wyzwalacz, jego czas zrobić coś sekcja z danymi, które jest generowany przez wyzwalacz. Wykonaj poniższe czynności, aby dodać akcję **SFTP — folder Wyodrębnij** . Ta akcja będzie pobierającego zawartość pliku, jeśli są spełnione określone warunki. 

Aby skonfigurować to akcji, należy podać następujące informacje. Można zauważyć, że jest łatwe w użyciu danych wygenerowane przez wyzwalacz jako danych wejściowych dla niektórych właściwości dla nowego pliku:

|SFTP - Wyodrębnij właściwości folderu|Opis|
|---|---|
|Ścieżka pliku archiwum źródła|Jest to ścieżka pliku wyodrębniania. Możesz z wcześniejszych akcji wybierz jedną z tokenów lub przeglądanie serwera SFTP w celu odnalezienia ścieżki pliku.|
|Ścieżka folderu docelowego|Jest to ścieżka rozmieszczenie wyodrębnione pliki. Wybierz jedną z tokenów z wcześniejszych akcji jako ścieżki docelowej lub przejść na serwerze SFTP i wybierz ścieżkę.|
|Czy zastąpić?|Wskazuje, jeśli plik o takiej samej nazwie jak plik wyodrębnionej znajduje się w ścieżka folderu docelowego, jeśli istniejący plik powinny być zastąpione, czy nie.|

Zaczynamy dodanie akcji, aby wyodrębnić pliki, jeśli zdefiniowany wcześniej warunek ma *wartość PRAWDA*. 

1. Wybierz pozycję **Dodaj akcję**.        
![Obraz warunku akcji SFTP 6](./media/connectors-create-api-sftp/condition-6.png)   
- Wybierz akcję, **SFTP — folder Wyodrębnij**      
![Obraz warunku akcji SFTP 7](./media/connectors-create-api-sftp/condition-7.png)   
- Wybierz **ścieżkę pliku archiwum źródła**              
![Obraz warunku akcji SFTP 9](./media/connectors-create-api-sftp/condition-9.png)   
- Wybierz pozycję token **ścieżki pliku** . Oznacza to, że ścieżka pliku, który znaleziono wyzwalacz będzie używany jako ścieżka pliku archiwum źródła.           
![Obraz warunku akcji SFTP 10](./media/connectors-create-api-sftp/condition-10.png)   
- Wybierz pozycję **ścieżka folderu docelowego**           
![Obraz warunku akcji SFTP 11](./media/connectors-create-api-sftp/condition-11.png)   
- Wybierz pozycję token **ścieżki pliku** . Oznacza to, że ścieżka pliku, którego odnaleźć wyzwalacz będzie używany jako ścieżka docelowa dla wyodrębnionych plików.   
- Wprowadź *\ExtractedFile* w formancie **ścieżka folderu docelowego** . Zrób to po prostu po token ścieżka pliku w kontrolce ścieżkę folderu docelowego.         
![Obraz warunku akcji SFTP 12](./media/connectors-create-api-sftp/condition-12.png)   
- Wprowadź *wartość PRAWDA* w **Zastąp?* sterowania, aby wskazać, że powinny być zastąpione istniejące pliki, jeśli mają taką samą nazwę jak wyodrębnione pliki.      
![Obraz warunku akcji SFTP 13](./media/connectors-create-api-sftp/condition-13.png)   
- Zapisanie zmian do przepływu pracy  
