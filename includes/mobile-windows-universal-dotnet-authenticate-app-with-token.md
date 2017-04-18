
1. W pliku projektu MainPage.xaml.cs Dodaj następujące instrukcje **za pomocą** :

        using System.Linq;      
        using Windows.Security.Credentials;

2. Zastąp metodę **AuthenticateAsync** następujący kod:

        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;

            // This sample uses the Facebook provider.
            var provider = MobileServiceAuthenticationProvider.Facebook;

            // Use the PasswordVault to securely store and access credentials.
            PasswordVault vault = new PasswordVault();
            PasswordCredential credential = null;

            try
            {
                // Try to get an existing credential from the vault.
                credential = vault.FindAllByResource(provider.ToString()).FirstOrDefault();
            }
            catch (Exception)
            {
                // When there is no matching resource an error occurs, which we ignore.
            }

            if (credential != null)
            {
                // Create a user from the stored credentials.
                user = new MobileServiceUser(credential.UserName);
                credential.RetrievePassword();
                user.MobileServiceAuthenticationToken = credential.Password;

                // Set the user from the stored credentials.
                App.MobileService.CurrentUser = user;

                // Consider adding a check to determine if the token is 
                // expired, as shown in this post: http://aka.ms/jww5vp.

                success = true;
                message = string.Format("Cached credentials for user - {0}", user.UserId);
            }
            else
            {
                try
                {
                    // Login with the identity provider.
                    user = await App.MobileService
                        .LoginAsync(provider);

                    // Create and store the user credentials.
                    credential = new PasswordCredential(provider.ToString(),
                        user.UserId, user.MobileServiceAuthenticationToken);
                    vault.Add(credential);

                    success = true;
                    message = string.Format("You are now logged in - {0}", user.UserId);
                }
                catch (MobileServiceInvalidOperationException)
                {
                    message = "You must log in. Login Required";
                }
            }
            
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();

            return success;
        }

    W tej wersji programu **AuthenticateAsync**aplikacji próbuje uzyskać dostęp do usługi za pomocą poświadczeń przechowywanych w **PasswordVault** . Zwykłe logowania również jest wykonywane po nie przechowywanych poświadczeń.

    >[AZURE.NOTE]Buforowany token może wygasła i po uwierzytelnieniu wygaśnięcia tokenu może również wystąpić, gdy aplikacja jest używany. Aby dowiedzieć się, jak ustalić, jeśli wygasła tokenu, zobacz [Sprawdź tokenów uwierzytelniania wygasły](http://aka.ms/jww5vp). Rozwiązania do obsługi autoryzacji błędów związanych z wygasania tokenów Zobacz wpis [buforowanie i obsługi wygasłe tokeny w usługach Mobile Azure zarządzanych SDK](http://blogs.msdn.com/b/carlosfigueira/archive/2014/03/13/caching-and-handling-expired-tokens-in-azure-mobile-services-managed-sdk.aspx). 

3. Ponownie uruchom aplikację dwa razy.

    Należy zauważyć, że przy pierwszym uruchamianiu, logowania z dostawcą ponownie jest wymagana. Jednak w drugim ponownym uruchomieniu komputera są używane poświadczenia z pamięci podręcznej i logowania jest pomijane. 
