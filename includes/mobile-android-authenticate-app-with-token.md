
Poprzednim przykładzie pokazano standardowe logowania, co wymaga klientowi skontaktuj się z zarówno dostawcy tożsamości i wewnętrznej bazy danych usługi Azure każdym uruchomieniu aplikacji. Nie tylko ta metoda jest nieefektywne, można uruchamiać w sprawach związanych z zastosowania należy spróbować wielu klientów uruchomić aplikację w tym samym czasie. Lepszym rozwiązaniem jest pamięci podręcznej token autoryzacji zwrócony przez usługę Azure i spróbuj użyć pierwsze przed użyciem oparte na dostawcy logowania. 

>[AZURE.NOTE]Może buforować token wystawiony przez usługi Azure niezależnie od tego, czy korzystasz z zarządzaniem klienta lub zarządzane usługi uwierzytelniania wewnętrznej bazy danych. Samouczku zarządzane usługi uwierzytelniania.


1. Otwórz plik ToDoActivity.java i Dodaj poniższych instrukcji importu:

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

2. Dodaj następujące elementy do `ToDoActivity` zajęć.

        public static final String SHAREDPREFFILE = "temp"; 
        public static final String USERIDPREF = "uid";  
        public static final String TOKENPREF = "tkn";   


3. W pliku ToDoActivity.java, Dodaj następujące definicję `cacheUserToken` metody.
 
        private void cacheUserToken(MobileServiceUser user)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            Editor editor = prefs.edit();
            editor.putString(USERIDPREF, user.getUserId());
            editor.putString(TOKENPREF, user.getAuthenticationToken());
            editor.commit();
        }   
  
    Ta metoda przechowuje identyfikator użytkownika i token w pliku preferencji oznaczonego jako prywatne. Należy chronić dostęp do pamięci podręcznej, tak, aby innych aplikacji na urządzenia nie ma dostępu do tokenu, ponieważ Preferencje jest w trybie piaskownicy dla aplikacji. Jednak jeśli ktoś uzyskuje dostęp do urządzenia, jest możliwe, że mogą one uzyskać dostęp do pamięci podręcznej tokenów w inny sposób. 

    >[AZURE.NOTE]Token z szyfrowania można zabezpieczyć, jeśli token dostępu do danych jest traktowany jako bardzo ważne i osoby mogą uzyskać dostęp do urządzenia. Jednak całkowicie bezpieczne rozwiązanie wykracza poza zakres tego samouczka i zależne od określonych wymagań dotyczących zabezpieczeń.


4. W pliku ToDoActivity.java, Dodaj następujące definicję `loadUserTokenCache` metody.

        private boolean loadUserTokenCache(MobileServiceClient client)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            String userId = prefs.getString(USERIDPREF, null); 
            if (userId == null)
                return false;
            String token = prefs.getString(TOKENPREF, null); 
            if (token == null)
                return false;
                
            MobileServiceUser user = new MobileServiceUser(userId);
            user.setAuthenticationToken(token);
            client.setCurrentUser(user);
                
            return true;
        }



5. W pliku *ToDoActivity.java* Zamień `authenticate` metody przy użyciu następującej metody, który używa tokenów pamięci podręcznej. Zmienianie dostawca logowania, jeśli chcesz użyć konta innego niż Google.

        private void authenticate() {
            // We first try to load a token cache if one exists.
            if (loadUserTokenCache(mClient))
            {
                createTable();
            }
            // If we failed to load a token cache, login and create a token cache
            else
            {
                // Login using the Google provider.    
                ListenableFuture<MobileServiceUser> mLogin = mClient.login(MobileServiceAuthenticationProvider.Google);
        
                Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                    @Override
                    public void onFailure(Throwable exc) {
                        createAndShowDialog("You must log in. Login Required", "Error");
                    }           
                    @Override
                    public void onSuccess(MobileServiceUser user) {
                        createAndShowDialog(String.format(
                                "You are now logged in - %1$2s",
                                user.getUserId()), "Success");
                        cacheUserToken(mClient.getCurrentUser());
                        createTable();  
                    }
                });
            }
        }

6. Tworzenie aplikacji i badania uwierzytelnianie przy użyciu prawidłowego konta. Uruchom co najmniej dwa razy. Przy pierwszym uruchomieniu powinien otrzymać monit się zalogować i utworzyć pamięci podręcznej tokenów. Po wykonaniu tej każdej serii spróbuje załadować pamięć podręczną tokenów uwierzytelniania, a nie powinno być wymagane do logowania.



