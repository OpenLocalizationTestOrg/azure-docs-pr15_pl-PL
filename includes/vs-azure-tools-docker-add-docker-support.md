1. W oknie programu Visual Studio **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projektu, a następnie wybierz pozycję **Dodaj > Obsługa Docker** z menu kontekstowego.

    ![Dodawanie menu kontekstowego Docker pomocy technicznej](media/vs-azure-tools-docker-add-docker-support/docker-support-context-menu.png)

1. Dodawanie obsługi Docker do Core ASP.NET projektu sieci web powoduje dodanie kilku plików związanych z Docker dodawanych do projektu, w tym redagowania Docker plików, skrypty środowiska Windows PowerShell wdrożenia i Docker właściwości plików. 

    ![Pliki docker dodane do programu project](media/vs-azure-tools-docker-add-docker-support/docker-files-added.png)
    
> [AZURE.NOTE]Jeśli używasz [Docker dla Windows w wersji Beta](https://beta.docker.com), otwórz Properties\Docker.props, usunąć wartość domyślna i uruchom ponownie programu Visual Studio dla wartości zostały wprowadzone.
> 
> ```
> <DockerMachineName Condition="'$(DockerMachineName)'=='' "></DockerMachineName>
> ```
