Możesz utworzyć kolejek Azure miejsca do magazynowania przy użyciu programu Visual Studio **Server Explorer**.

![Blob Eksploratora serwera][Image1]

1. W menu **Widok** wybierz pozycję **Eksplorator serwera**.
2. W Eksploratorze Server rozwiń węzeł **Azure** za subskrypcję, rozwiń węzeł **miejsca do magazynowania** i węzeł dla konta miejsca do magazynowania, które określone w usłudze magazyn Azure połączone.
3. Wybierz węzeł **kolejek** i wybierz pozycję **Utwórz kolejkę** z menu kontekstowego.
4. Wprowadź nazwę kontenera, a następnie kliknij **przycisk OK**.   

Domyślnie nowego kontenera jest prywatna, a następnie wprowadź klucz dostępu przestrzeni dyskowej, aby pobrać obiektów blob z tego kontenera. Jeśli chcesz pliki w kontenerze publicznie, wybierz kontener w **Eksploratorze serwera** i naciśnij klawisz `F4` Aby wyświetlić okno **Właściwości** . Skonfiguruj **publiczną odczyt** do **obiektów Blob**. Każda osoba w Internecie Zobacz obiektów blob w kontenerze publicznej, ale można modyfikować lub je usunąć tylko wtedy, gdy masz klucz odpowiedni dostęp.


[Image1]: ./media/vs-create-blob-container-in-server-explorer/vs-storage-create-blob-containers-in-Server-Explorer.png