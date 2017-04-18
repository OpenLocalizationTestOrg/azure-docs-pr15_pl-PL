
1. W **Programie Project Explorer** w Android Studio Otwórz plik ToDoActivity.java i Dodaj poniższych instrukcji importu.

        import java.util.concurrent.ExecutionException;
        import java.util.concurrent.atomic.AtomicBoolean;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceAuthenticationProvider;
        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceUser;

2. Dodaj następujące metody klasy **ToDoActivity** : 
    
        private void authenticate() {
            // Login using the Google provider.
            
            ListenableFuture<MobileServiceUser> mLogin = mClient.login(MobileServiceAuthenticationProvider.Google);
    
            Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                @Override
                public void onFailure(Throwable exc) {
                    createAndShowDialog((Exception) exc, "Error");
                }           
                @Override
                public void onSuccess(MobileServiceUser user) {
                    createAndShowDialog(String.format(
                            "You are now logged in - %1$2s",
                            user.getUserId()), "Success");
                    createTable();  
                }
            });     
        }


    Spowoduje to utworzenie nowej metody, aby obsługiwać ten proces uwierzytelniania. Użytkownik jest uwierzytelniony przy użyciu logowania Google. Okno dialogowe zostanie wyświetlone zawierającym identyfikator uwierzytelnionego użytkownika. Nie można kontynuować bez dodatnia uwierzytelniania.

    > [AZURE.NOTE] Jeśli korzystasz z dostawcą tożsamości innych niż Google, zmień wartość przekazywana do powyższej metody **logowania** do jednej z następujących czynności: _MicrosoftAccount_, _Facebook_, _serwisu Twitter_lub _windowsazureactivedirectory_.

3. W przypadku metody **onCreate** Dodaj poniższy wiersz kodu po kodzie, który tworzy `MobileServiceClient` obiektu.

        authenticate();

    To połączenie zostanie uruchomiony proces uwierzytelniania.

4. Przenoszenie pozostały kod po `authenticate();` w metodę **onCreate** nowa metoda **createTable** , która wygląda następująco:

        private void createTable() {
    
            // Get the table instance to use.
            mToDoTable = mClient.getTable(ToDoItem.class);
    
            mTextNewToDo = (EditText) findViewById(R.id.textNewToDo);
    
            // Create an adapter to bind the items with the view.
            mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);
    
            // Load the items from Azure.
            refreshItemsFromTable();
        }

9. Z menu **uruchamiania** następnie kliknij polecenie **Uruchom aplikację** Uruchom aplikację i zaloguj się przy użyciu dostawcy tożsamości wybranym. 

    Po pomyślnym zalogowany aplikacji powinny być uruchamiane bez błędów, a powinno być możliwe do wykonywania kwerend usługi wewnętrznej bazy danych i wprowadzić zmiany do danych.