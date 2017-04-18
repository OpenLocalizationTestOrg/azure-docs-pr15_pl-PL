Możesz dodać wyzwalacza.

1. Wprowadź *sftp* w polu wyszukiwania w Projektancie aplikacji logiczny, a następnie wybierz pozycję wyzwalacz **SFTP — podczas dodawania lub modyfikowania pliku**   
![Obraz wyzwalacza SFTP 1](./media/connectors-create-api-sftp/trigger-1.png)  
- Otwiera kontroli **podczas dodawania lub modyfikowania pliku**  
![Obraz wyzwalacza SFTP 2](./media/connectors-create-api-sftp/trigger-2.png)  
- Wybierz pozycję **...** znajdującej się po prawej stronie formantu. Spowoduje to otwarcie folderu kontrolki selektora  
![Obraz wyzwalacza SFTP 3](./media/connectors-create-api-sftp/action-1.png)  
- Wybierz pozycję **SFTP** w celu wybrania folderu głównego jako folderu monitorowanie dla nowych lub zmienionych plików. Zwróć uwagę, że folder główny zostanie wyświetlona w formancie **folderu** .  
![Obraz wyzwalacza SFTP 4](./media/connectors-create-api-sftp/action-2.png)   

W tym momencie aplikacji logiczny został skonfigurowany z wyzwalacz, który rozpocznie Uruchom wyzwalaczy i akcje w ramach przepływu pracy po modyfikacji lub utworzony w określonym folderze SFTP pliku. 

>[AZURE.NOTE]Dla aplikacji logiki do działać musi zawierać co najmniej jeden wyzwalacz i akcji. Wykonaj kroki opisane w następnej sekcji, aby dodać akcję.  