
   * Zaloguj się na konto Azure przez wprowadzenie poświadczeń.

     Ta metoda jest szybsze i łatwiejsze, ale jeśli ta metoda nie będzie można wyświetlić bazy danych SQL Azure lub usługi Mobile w oknie **Eksploratora serwera** .

     W **Eksploratorze serwera**kliknij przycisk **Połącz z Azure** . Alternatywny jest kliknij prawym przyciskiem myszy węzeł **Azure** , a następnie w menu kontekstowym kliknij polecenie **Połącz Azure** .

   * Instalowanie certyfikatu zarządzania, który umożliwia dostęp do swojego konta.

     W **Eksploratorze Server**kliknij prawym przyciskiem myszy węzeł **Azure** , a następnie w menu kontekstowym kliknij polecenie **Zarządzaj subskrypcjami** . W oknie dialogowym **Zarządzaj subskrypcjami Azure** kliknij kartę **Certyfikaty** , a następnie kliknij przycisk **Importuj**. Postępuj zgodnie z instrukcjami, aby pobrać, a następnie zaimportować plik subskrypcji (zwanych również pliku *.publishsettings* ) dla Twojego konta Azure.

     > [AZURE.NOTE] Pobierz plik subskrypcji do folderu spoza Twojej katalogów kodu źródła (na przykład w folderze pobrane), a następnie usuń je po zakończeniu importowania. Złośliwy użytkownik uzyskuje dostęp do pliku subskrypcji można edytować, tworzenie i usuwanie usługi Azure.

    Aby uzyskać więcej informacji zobacz [jak połączyć Azure z programu Visual Studio](http://go.microsoft.com/fwlink/?LinkId=324796).
