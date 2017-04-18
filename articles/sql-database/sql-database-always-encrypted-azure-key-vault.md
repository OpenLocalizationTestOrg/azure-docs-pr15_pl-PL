<properties
    pageTitle="Zawsze szyfrowane: Ochrona poufnych danych w bazie danych SQL Azure za pomocą szyfrowanie bazy danych | Microsoft Azure"
    description="Ochrona ważnych danych w bazie danych SQL w minutach."
    keywords="szyfrowanie danych klucza szyfrowania w chmurze szyfrowania"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="sstein"/>

# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-azure-key-vault"></a>Zawsze szyfrowane: Ochrony ważnych danych w bazie danych SQL i przechowywanie kluczy szyfrowania w Azure klucza magazynu

> [AZURE.SELECTOR]
- [Azure klucza magazynu](sql-database-always-encrypted-azure-key-vault.md)
- [Magazyn certyfikatów systemu Windows](sql-database-always-encrypted.md)


W tym artykule pokazano, jak do zabezpieczenia ważnych danych w bazie danych SQL przez szyfrowanie danych przy użyciu [Kreatora zawsze szyfrowane](https://msdn.microsoft.com/library/mt459280.aspx) w [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx). Zawiera instrukcje, które przedstawiono, jak przechowywanie każdego klucza szyfrowania w Azure klucza magazynu.

Zawsze szyfrowane jest nowa technologia szyfrowania danych w bazie danych SQL Azure i SQL Server, które ułatwia ochronę danych poufnych spoczynku na serwerze, podczas przepływu między klientem a serwerem, gdy danych jest używany. Zawsze zaszyfrowana gwarantuje, że dane poufne nigdy nie jest wyświetlany jako zwykły tekst wewnątrz systemu bazy danych. Po skonfigurowaniu szyfrowania danych tylko w aplikacjach klienckich lub serwerach aplikacji, które mają dostęp do kluczy dostęp do danych w postaci zwykłego tekstu. Aby uzyskać szczegółowe informacje zobacz [Zawsze szyfrowane (aparat bazy danych)](https://msdn.microsoft.com/library/mt163865.aspx).


Po skonfigurowaniu bazy danych zawsze szyfrowane spowoduje utworzenie aplikacji klienckiej w C# z programem Visual Studio do pracy z zaszyfrowane dane.

Wykonaj czynności opisane w tym artykule i Dowiedz się, jak skonfigurować zawsze szyfrowane dla bazy danych programu Azure SQL. W tym artykule dowiesz się, jak wykonywać następujące zadania:

- Tworzenie [klawiszy zawsze szyfrowane](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3)za pomocą Kreatora zawsze szyfrowane w SSMS.
    - Tworzenie [kolumny klucza głównego (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).
    - Tworzenie [kolumny klucza szyfrowania (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).
- Tworzenie tabeli bazy danych i szyfrowanie kolumn.
- Tworzenie aplikacji, która powoduje wstawienie, wybiera i wyświetla dane z kolumn zaszyfrowane.


## <a name="prerequisites"></a>Wymagania wstępne

Ten samouczek musisz:

- Konto Azure i subskrypcji. Jeśli nie masz jedną Załóż [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).
- [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) wersji 13.0.700.242 lub nowszej.
- [.NET framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) lub nowszego (na komputerze klienckim).
- [Programu visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
- [Azure programu PowerShell](../powershell-install-configure.md), wersji 1.0 lub nowszej. Typ **(Get-Module azure - ListAvailable). Wersja** do sprawdzenia, która wersja programu PowerShell działa.



## <a name="enable-your-client-application-to-access-the-sql-database-service"></a>Włączanie aplikacji klienckiej dostęp do usługi bazy danych SQL

Należy włączyć aplikację klienta dostęp do bazy danych SQL usługi konfigurując wymagane uwierzytelniania i uzyskiwanie *ClientId* i *hasło* , potrzebne do uwierzytelniania aplikacji poniższy kod.

1. Otwórz [portal Azure klasyczny](http://manage.windowsazure.com).
2. Wybierz pozycję **Usługi Active Directory** , a następnie kliknij pozycję wystąpienia usługi Active Directory, które będą używane aplikacji.
3. Kliknij pozycję **aplikacje**, a następnie kliknij przycisk **Dodaj**.
4. Wpisz nazwę dla aplikacji (na przykład: *myClientApp*), a następnie wybierz **APLIKACJĘ sieci WEB**i kliknij strzałkę, aby kontynuować.
5. W polu **Adres URL logowania na** i **Aplikacji identyfikator URI** można wpisać prawidłowy adres URL (na przykład *http://myClientApp*) i Kontynuuj.
6. Kliknij przycisk **Konfiguruj**.
7. Kopiowanie swój **identyfikator klienta**. (Będą potrzebne tę wartość w kodzie później.)
8. W sekcji **kluczy** wybierz **1 rok** z listy rozwijanej **Wybierz czas trwania** . (Zostanie skopiowany klucz, kiedy zostanie zapisany w kroku 14.)
11. Przewiń w dół i kliknij pozycję **Dodaj aplikację**.
12. Pozostaw **Pokaż** zestaw **Aplikacji firmy Microsoft** , a następnie wybierz pozycję **Zarządzanie usługą Microsoft Azure**. Kliknij znacznik wyboru, aby kontynuować.
13. **Zarządzanie usługą Azure dostęp** wybierz z listy rozwijanej **Delegowane uprawnienia** .
14. Kliknij przycisk **ZAPISZ**.
15. Po zakończeniu zapisz skopiuj wartość klucza w sekcji **klawiszy** . (Będą potrzebne tę wartość w kodzie później.)



## <a name="create-a-key-vault-to-store-your-keys"></a>Tworzenie klucza magazynu do przechowywania kluczy

Teraz, gdy skonfigurowano aplikacji klienckich i masz identyfikator klienta, nadszedł czas na tworzenie klucza magazynu i konfigurowanie jego zasady dostępu, aby aplikacja dostępem hasła magazynu (klawisze zawsze szyfrowane). *Tworzenie*, *Pobieranie* *listy*, *logowania*, *Sprawdź*, *wrapKey*i *unwrapKey* uprawnienia są wymagane do tworzenia nowego klucza głównego kolumny i konfigurowaniu szyfrowania z programu SQL Server Management Studio.

Można szybko utworzyć klucza magazynu, uruchamiając skrypt następujące. Szczegółowy opis tych poleceń cmdlet i więcej informacji na temat tworzenia i konfigurowania magazynu klucza zobacz [Rozpoczynanie pracy z magazynu klucza Azure](../Key-Vault/key-vault-get-started.md).



    $subscriptionName = '<your Azure subscription name>'
    $userPrincipalName = '<username@domain.com>'
    $clientId = '<client ID that you copied in step 7 above>'
    $resourceGroupName = '<resource group name>'
    $location = '<datacenter location>'
    $vaultName = 'AeKeyVault'


    Login-AzureRmAccount
    $subscriptionId = (Get-AzureRmSubscription -SubscriptionName $subscriptionName).SubscriptionId
    Set-AzureRmContext -SubscriptionId $subscriptionId

    New-AzureRmResourceGroup –Name $resourceGroupName –Location $location
    New-AzureRmKeyVault -VaultName $vaultName -ResourceGroupName $resourceGroupName -Location $location

    Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName -ResourceGroupName $resourceGroupName -PermissionsToKeys create,get,wrapKey,unwrapKey,sign,verify,list -UserPrincipalName $userPrincipalName
    Set-AzureRmKeyVaultAccessPolicy  -VaultName $vaultName  -ResourceGroupName $resourceGroupName -ServicePrincipalName $clientId -PermissionsToKeys get,wrapKey,unwrapKey,sign,verify,list




## <a name="create-a-blank-sql-database"></a>Tworzenie pustej bazy danych SQL
1. Zaloguj się do [portalu Azure](https://portal.azure.com/).
2. Przejdź do **nowej** > **danych + miejsca do magazynowania** > **bazy danych SQL**.
3. Tworzenie **pustej** bazy danych o nazwie **Klinika** na serwerze nowym lub istniejącym. Aby uzyskać szczegółowe informacje na temat tworzenia bazy danych w portalu Azure zobacz [Tworzenie bazy danych SQL w minutach](sql-database-get-started.md).

    ![Tworzenie pustej bazy danych](./media/sql-database-always-encrypted-azure-key-vault/create-database.png)

Konieczne będzie połączenie string w dalszej części samouczka, aby po utworzeniu bazy danych, przejdź do nowej bazy danych Klinika i skopiuj parametry połączenia. Parametry połączenia można uzyskać w dowolnym momencie, ale łatwo skopiować go w portalu Azure.

1. Przejdź do **bazy danych programu SQL** > **Klinika** > **Pokaż parametry połączenia bazy danych**.
2. Skopiuj parametry połączenia dla **ADO.NET**.

    ![Skopiuj parametry połączenia](./media/sql-database-always-encrypted-azure-key-vault/connection-strings.png)


## <a name="connect-to-the-database-with-ssms"></a>Nawiązywanie połączenia z bazą danych przy użyciu SSMS

Otwórz SSMS i połączyć się z serwerem z bazą danych Klinika.


1. Otwórz SSMS. (Przejdź do pozycji **Połącz** > **Aparat bazy danych** , aby otworzyć okno **połączenie z serwerem** , jeśli nie jest otwarty.)
2. Wprowadź nazwę serwera i poświadczenia. Nazwa serwera znajduje się na karta bazy danych SQL, a w parametrach połączenia skopiowany wcześniej. Wpisz pełną nazwę serwera, łącznie z *database.windows.net*.

    ![Skopiuj parametry połączenia](./media/sql-database-always-encrypted-azure-key-vault/ssms-connect.png)

Jeśli zostanie otwarte okno **Nowej reguły zapory** , zaloguj się do Azure i umożliwić SSMS Tworzenie nowej reguły zapory dla Ciebie.


## <a name="create-a-table"></a>Tworzenie tabeli

W tej sekcji utworzy tabelę do przechowywania danych pacjentów. Nie jest wstępnie szyfrowane — skonfigurujesz szyfrowania w następnej sekcji.

1. Rozwiń węzeł **bazy danych**.
1. Kliknij prawym przyciskiem myszy **Klinika** bazy danych, a następnie kliknij przycisk **Nowe zapytanie**.
2. Wklej następujące języku Transact-SQL (T-SQL) w nowym oknie kwerendy i **Wykonywanie** go.


        CREATE TABLE [dbo].[Patients](
         [PatientId] [int] IDENTITY(1,1),
         [SSN] [char](11) NOT NULL,
         [FirstName] [nvarchar](50) NULL,
         [LastName] [nvarchar](50) NULL,
         [MiddleName] [nvarchar](50) NULL,
         [StreetAddress] [nvarchar](50) NULL,
         [City] [nvarchar](50) NULL,
         [ZipCode] [char](5) NULL,
         [State] [char](2) NULL,
         [BirthDate] [date] NOT NULL
         PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY] );
         GO


## <a name="encrypt-columns-configure-always-encrypted"></a>Szyfrowanie kolumn (Konfigurowanie zawsze szyfrowane)

SSMS zawiera kreatora, który pomoże Ci w prosty sposób konfigurować zawsze szyfrowane przez skonfigurowanie kolumny klucza, kolumny klucza szyfrowania i zaszyfrowane kolumny dla Ciebie.

1. Rozwiń węzeł **bazy danych** > **Klinika** > **tabel**.
2. Kliknij prawym przyciskiem myszy tabelę **pacjentów** , a następnie wybierz pozycję **Szyfrowanie kolumn** , aby otworzyć Kreatora zawsze szyfrowane:

    ![Szyfrowanie kolumn](./media/sql-database-always-encrypted-azure-key-vault/encrypt-columns.png)

Kreator zawsze szyfrowane zawiera następujące sekcje: **Zaznaczenie kolumny**, **Klucza konfiguracji** **sprawdzania poprawności**i **podsumowania**.

### <a name="column-selection"></a>Zaznaczenie kolumny##

Kliknij przycisk **Dalej** na stronie **wprowadzenie** , aby otworzyć stronę **Zaznaczenie kolumny** . Na tej stronie, możesz wybrać kolumny, które chcesz zaszyfrować, [Typ szyfrowania i jakie kolumny klucza szyfrowania (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) korzystać.

Zaszyfruj **ubezpieczenia społecznego** i **DataUrodzenia** informacje dotyczące poszczególnych pacjentów. Kolumna ubezpieczenia społecznego użyje deterministyczne szyfrowania, który obsługuje odnośniki równości, sprzężeń i Grupuj według. Kolumna DataUrodzenia użyje te szyfrowania, która nie obsługuje operacji.

Ustaw **Typ szyfrowania** w kolumnie ubezpieczenia społecznego **Deterministic** i kolumnę DataUrodzenia **Randomized**. Kliknij przycisk **Dalej**.

![Szyfrowanie kolumn](./media/sql-database-always-encrypted-azure-key-vault/column-selection.png)

### <a name="master-key-configuration"></a>Konfiguracja klucza głównego###

Strona **Konfiguracja głównego** to miejsce, w którym Konfigurowanie usługi CMK i wybierz dostawcę magazynie kluczy przechowywania CMK. Obecnie mogą zawierać CMK w magazynie certyfikatów systemu Windows, Azure klucza magazynu lub sprzęt zabezpieczeń moduły.

Ten samouczek pokazano, jak przechowywanie kluczy w Azure klucza magazynu.

1.     Wybierz pozycję **Azure klucza magazynu**.
1.     Z listy rozwijanej wybierz odpowiednie magazynu kluczy.
1.     Kliknij przycisk **Dalej**.

![Konfiguracja klucza głównego](./media/sql-database-always-encrypted-azure-key-vault/master-key-configuration.png)


### <a name="validation"></a>Sprawdzanie poprawności###

Można teraz szyfrowanie kolumny lub zapisać skrypt programu PowerShell, aby uruchomić później. Ten samouczek wybierz pozycję **Kontynuuj, aby zakończyć teraz** , a następnie kliknij przycisk **Dalej**.

### <a name="summary"></a>Podsumowanie ###

Upewnij się, że ustawienia są wszystkie Popraw i kliknij przycisk **Zakończ** , aby zakończyć konfigurację dla zawsze szyfrowane.


![Podsumowanie](./media/sql-database-always-encrypted-azure-key-vault/summary.png)


### <a name="verify-the-wizards-actions"></a>Sprawdź akcji kreatora

Po zakończeniu działania Kreatora bazy danych jest skonfigurowana dla zawsze szyfrowane. Kreator wykonać następujące czynności:

- Utworzony klucz główny kolumny i przechowywanym w Azure klucza magazynu.
- Tworzenia klucza szyfrowania kolumny i przechowywanym w Azure klucza magazynu.
- Skonfigurowane kolumny wybrane dla szyfrowania. Tabela pacjentów obecnie nie ma danych, ale wszystkie istniejące dane w zaznaczonych kolumn są teraz szyfrowane.

Po rozwinięciu **Klinika**, można sprawdzić tworzenia kluczy w SSMS > **zabezpieczeń** > **Zawsze szyfrowane klawiszy**.


## <a name="create-a-client-application-that-works-with-the-encrypted-data"></a>Tworzenie aplikacji klienckiej, która działa z zaszyfrowane dane

Teraz, gdy skonfigurowano zawsze szyfrowane można utworzyć aplikację, która wykonuje *wstawia* i *zaznacza* zaszyfrowanych kolumn.  

> [AZURE.IMPORTANT] Podczas przekazywania danych zwykłego tekstu na serwerze przy użyciu kolumn zawsze szyfrowane aplikacji należy użyć [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) obiektów. Przekazywanie literałów bez użycia obiektów SqlParameter spowoduje wyjątek.

1. Otwórz program Visual Studio i tworzenie nowej aplikacji konsoli C#. Upewnij się, że projekt ustawiono **.NET Framework** 4.6 lub nowszy.
2. Nazwa projektu **AlwaysEncryptedConsoleAKVApp** , a następnie kliknij **przycisk OK**.
![Nowa aplikacja konsoli](./media/sql-database-always-encrypted-azure-key-vault/console-app.png)
3. Instalowanie następujące pakiety NuGet, przechodząc do pozycji **Narzędzia** > **Menedżera pakietów NuGet** > **Konsoli Menedżera pakietów**.

Uruchom następujące dwa wiersze kodu w konsoli Menedżera pakietów.

    Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory



## <a name="modify-your-connection-string-to-enable-always-encrypted"></a>Modyfikowanie ciąg połączenia umożliwiające zawsze szyfrowane

W tej sekcji wyjaśniono, jak włączyć zawsze szyfrowane w ciągu połączenia bazy danych.


Aby włączyć zawsze szyfrowane, należy dodać słowo kluczowe **Ustawienie szyfrowania kolumny** do ciągu połączenia i ustawianie go na **włączone**.

Można tak ustawić bezpośrednio w parametrach połączenia lub można ustawić go przy użyciu [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx). Przykładowa aplikacja w następnej sekcji pokazano, jak używać **SqlConnectionStringBuilder**.



### <a name="enable-always-encrypted-in-the-connection-string"></a>Włączanie zawsze szyfrowane w parametrach połączenia

Dodaj następujące słowo kluczowe do ciągu połączenia.

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-sqlconnectionstringbuilder"></a>Włącz zawsze szyfrowane z SqlConnectionStringBuilder

Poniższy kod przedstawia włączania zawsze szyfrowane przez ustawienie [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) na [włączone](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;

## <a name="register-the-azure-key-vault-provider"></a>Dostawca Azure magazynu klucza rejestru

Poniższy kod pokazano, jak zarejestrować dostawcy magazynu klucza Azure za pomocą sterownika ADO.NET.

    private static ClientCredential _clientCredential;

    static void InitializeAzureKeyVaultProvider()
    {
       _clientCredential = new ClientCredential(clientId, clientSecret);

       SqlColumnEncryptionAzureKeyVaultProvider azureKeyVaultProvider =
          new SqlColumnEncryptionAzureKeyVaultProvider(GetToken);

       Dictionary<string, SqlColumnEncryptionKeyStoreProvider> providers =
          new Dictionary<string, SqlColumnEncryptionKeyStoreProvider>();

       providers.Add(SqlColumnEncryptionAzureKeyVaultProvider.ProviderName, azureKeyVaultProvider);
       SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers);
    }



## <a name="always-encrypted-sample-console-application"></a>Zawsze szyfrowane aplikacji konsoli próbki

W przykładzie pokazano, jak:

- Modyfikowanie ciąg połączenia umożliwiające zawsze szyfrowane.
- Zarejestruj Azure klucza magazynu jako dostawca magazynie kluczy aplikacji.  
- Wstawianie danych do zaszyfrowanych kolumn.
- Wybierz rekord, filtrując dla określonej wartości w kolumnie zaszyfrowane.

Zamień zawartość **Plik Program.cs** poniższy kod. Zastąp ciąg połączenia dla zmiennej globalnej connectionString w wierszu bezpośrednio poprzedzającej metody główne z Twojej prawidłowe parametry połączenia z portalem Azure. To jest tylko zmiany, które należy wprowadzić kod.

Uruchom aplikację, aby zobaczyć zawsze szyfrowane w działaniu.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Data;
    using System.Data.SqlClient;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider;

    namespace AlwaysEncryptedConsoleAKVApp
    {
    class Program
    {
        // Update this line with your Clinic database connection string from the Azure portal.
        static string connectionString = @"<connection string from the portal>";
        static string clientId = @"<client id from step 7 above>";
        static string clientSecret = "<key from step 13 above>";


        static void Main(string[] args)
        {
            InitializeAzureKeyVaultProvider();

            Console.WriteLine("Signed in as: " + _clientCredential.ClientId);

            Console.WriteLine("Original connection string copied from the Azure portal:");
            Console.WriteLine(connectionString);

            // Create a SqlConnectionStringBuilder.
            SqlConnectionStringBuilder connStringBuilder =
                new SqlConnectionStringBuilder(connectionString);

            // Enable Always Encrypted for the connection.
            // This is the only change specific to Always Encrypted
            connStringBuilder.ColumnEncryptionSetting =
                SqlConnectionColumnEncryptionSetting.Enabled;

            Console.WriteLine(Environment.NewLine + "Updated connection string with Always Encrypted enabled:");
            Console.WriteLine(connStringBuilder.ConnectionString);

            // Update the connection string with a password supplied at runtime.
            Console.WriteLine(Environment.NewLine + "Enter server password:");
            connStringBuilder.Password = Console.ReadLine();


            // Assign the updated connection string to our global variable.
            connectionString = connStringBuilder.ConnectionString;


            // Delete all records to restart this demo app.
            ResetPatientsTable();

            // Add sample data to the Patients table.
            Console.Write(Environment.NewLine + "Adding sample patient data to the database...");

            InsertPatient(new Patient()
            {
                SSN = "999-99-0001",
                FirstName = "Orlando",
                LastName = "Gee",
                BirthDate = DateTime.Parse("01/04/1964")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0002",
                FirstName = "Keith",
                LastName = "Harris",
                BirthDate = DateTime.Parse("06/20/1977")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0003",
                FirstName = "Donna",
                LastName = "Carreras",
                BirthDate = DateTime.Parse("02/09/1973")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0004",
                FirstName = "Janet",
                LastName = "Gates",
                BirthDate = DateTime.Parse("08/31/1985")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0005",
                FirstName = "Lucy",
                LastName = "Harrington",
                BirthDate = DateTime.Parse("05/06/1993")
            });


            // Fetch and display all patients.
            Console.WriteLine(Environment.NewLine + "All the records currently in the Patients table:");

            foreach (Patient patient in SelectAllPatients())
            {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now lets locate records by searching the encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that the user entered 11 characters.
            // In production be sure to check all user input and use the best validation for your specific application.
            do
            {
                Console.WriteLine("Please enter a valid SSN (ex. 999-99-0003):");
                ssn = Console.ReadLine();
            } while (ssn.Length != 11);

            // The example allows duplicate SSN entries so we will return all records
            // that match the provided value and store the results in selectedPatients.
            Patient selectedPatient = SelectPatientBySSN(ssn);

            // Check if any records were returned and display our query results.
            if (selectedPatient != null)
            {
                Console.WriteLine("Patient found with SSN = " + ssn);
                Console.WriteLine(selectedPatient.FirstName + " " + selectedPatient.LastName + "\tSSN: "
                    + selectedPatient.SSN + "\tBirthdate: " + selectedPatient.BirthDate);
            }
            else
            {
                Console.WriteLine("No patients found with SSN = " + ssn);
            }

            Console.WriteLine("Press Enter to exit...");
            Console.ReadLine();
        }


        private static ClientCredential _clientCredential;

        static void InitializeAzureKeyVaultProvider()
        {

            _clientCredential = new ClientCredential(clientId, clientSecret);

            SqlColumnEncryptionAzureKeyVaultProvider azureKeyVaultProvider =
              new SqlColumnEncryptionAzureKeyVaultProvider(GetToken);

            Dictionary<string, SqlColumnEncryptionKeyStoreProvider> providers =
              new Dictionary<string, SqlColumnEncryptionKeyStoreProvider>();

            providers.Add(SqlColumnEncryptionAzureKeyVaultProvider.ProviderName, azureKeyVaultProvider);
            SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers);
        }

        public async static Task<string> GetToken(string authority, string resource, string scope)
        {
            var authContext = new AuthenticationContext(authority);
            AuthenticationResult result = await authContext.AcquireTokenAsync(resource, _clientCredential);

            if (result == null)
                throw new InvalidOperationException("Failed to obtain the access token");
            return result.AccessToken;
        }

        static int InsertPatient(Patient newPatient)
        {
            int returnValue = 0;

            string sqlCmdText = @"INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate])
     VALUES (@SSN, @FirstName, @LastName, @BirthDate);";

            SqlCommand sqlCmd = new SqlCommand(sqlCmdText);


            SqlParameter paramSSN = new SqlParameter(@"@SSN", newPatient.SSN);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            SqlParameter paramFirstName = new SqlParameter(@"@FirstName", newPatient.FirstName);
            paramFirstName.DbType = DbType.String;
            paramFirstName.Direction = ParameterDirection.Input;

            SqlParameter paramLastName = new SqlParameter(@"@LastName", newPatient.LastName);
            paramLastName.DbType = DbType.String;
            paramLastName.Direction = ParameterDirection.Input;

            SqlParameter paramBirthDate = new SqlParameter(@"@BirthDate", newPatient.BirthDate);
            paramBirthDate.SqlDbType = SqlDbType.Date;
            paramBirthDate.Direction = ParameterDirection.Input;

            sqlCmd.Parameters.Add(paramSSN);
            sqlCmd.Parameters.Add(paramFirstName);
            sqlCmd.Parameters.Add(paramLastName);
            sqlCmd.Parameters.Add(paramBirthDate);

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();
                }
                catch (Exception ex)
                {
                    returnValue = 1;
                    Console.WriteLine("The following error was encountered: ");
                    Console.WriteLine(ex.Message);
                    Console.WriteLine(Environment.NewLine + "Press Enter key to exit");
                    Console.ReadLine();
                    Environment.Exit(0);
                }
            }
            return returnValue;
        }


        static List<Patient> SelectAllPatients()
        {
            List<Patient> patients = new List<Patient>();


            SqlCommand sqlCmd = new SqlCommand(
              "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients]",
                new SqlConnection(connectionString));


            using (sqlCmd.Connection = new SqlConnection(connectionString))

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patients.Add(new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            });
                        }
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }

            return patients;
        }


        static Patient SelectPatientBySSN(string ssn)
        {
            Patient patient = new Patient();

            SqlCommand sqlCmd = new SqlCommand(
                "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN]=@SSN",
                new SqlConnection(connectionString));

            SqlParameter paramSSN = new SqlParameter(@"@SSN", ssn);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            sqlCmd.Parameters.Add(paramSSN);


            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patient = new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            };
                        }
                    }
                    else
                    {
                        patient = null;
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }
            return patient;
        }


        // This method simply deletes all records in the Patients table to reset our demo.
        static int ResetPatientsTable()
        {
            int returnValue = 0;

            SqlCommand sqlCmd = new SqlCommand("DELETE FROM Patients");
            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();

                }
                catch (Exception ex)
                {
                    returnValue = 1;
                }
            }
            return returnValue;
        }
    }

    class Patient
    {
        public string SSN { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public DateTime BirthDate { get; set; }
    }
    }



## <a name="verify-that-the-data-is-encrypted"></a>Sprawdź, czy dane są szyfrowane

Można szybko sprawdzić, czy dane na serwerze jest zaszyfrowany przez badania danych pacjentów z SSMS (za pomocą bieżące połączenie gdzie **Ustawienie szyfrowania kolumn** nie jest jeszcze włączone).

Uruchom poniższe zapytanie w bazie danych Klinika.

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

Można zobaczyć zaszyfrowanych kolumn nie zawierają żadnych danych w postaci zwykłego tekstu.

   ![Nowa aplikacja konsoli](./media/sql-database-always-encrypted-azure-key-vault/ssms-encrypted.png)


Aby uzyskać dostęp do danych w postaci zwykłego tekstu za pomocą SSMS, możesz dodać *ustawienie szyfrowania kolumny = włączone* parametr w celu połączenia.

1. SSMS kliknij prawym przyciskiem myszy serwera w **Eksploratorze obiektów** i wybierz polecenie **Rozłącz**.
2. Kliknij pozycję **Połącz** > **Aparat bazy danych** , aby otworzyć okno **połączenie z serwerem** , a następnie kliknij pozycję **Opcje**.
3. Kliknij przycisk **Dodatkowe parametry połączenia** i typ **ustawienie szyfrowania kolumny = włączone**.

    ![Nowa aplikacja konsoli](./media/sql-database-always-encrypted-azure-key-vault/ssms-connection-parameter.png)

4. Uruchom poniższe zapytanie w bazie danych Klinika.

        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

     Można teraz zobaczyć dane zwykłego tekstu w kolumnach zaszyfrowane.


    ![Nowa aplikacja konsoli](./media/sql-database-always-encrypted-azure-key-vault/ssms-plaintext.png)


## <a name="next-steps"></a>Następne kroki
Po utworzeniu bazy danych, która korzysta z zawsze szyfrowane, można wykonać następujące czynności:

- [Obróć i oczyścić kluczy](https://msdn.microsoft.com/library/mt607048.aspx).
- [Migrowanie danych, który jest już zaszyfrowanych za pomocą zawsze szyfrowane](https://msdn.microsoft.com/library/mt621539.aspx).


## <a name="related-information"></a>Informacje pokrewne

- [Zawsze szyfrowane (do opracowania klienta)](https://msdn.microsoft.com/library/mt147923.aspx)
- [Szyfrowanie danych przezroczystości](https://msdn.microsoft.com/library/bb934049.aspx)
- [Szyfrowanie programu SQL Server](https://msdn.microsoft.com/library/bb510663.aspx)
- [Zawsze szyfrowane Kreatora](https://msdn.microsoft.com/library/mt459280.aspx)
- [Blogu zawsze zaszyfrowanych](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)
