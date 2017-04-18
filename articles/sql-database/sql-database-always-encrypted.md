<properties
    pageTitle="Zawsze szyfrowane: Ochrona poufnych danych w bazie danych SQL Azure za pomocą szyfrowanie bazy danych | Microsoft Azure"
    description="Ochrona ważnych danych w bazie danych SQL w minutach."
    keywords="szyfrowanie danych, szyfrowania sql, szyfrowanie bazy danych, dane poufne, zawsze szyfrowane"
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

# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-the-windows-certificate-store"></a>Zawsze szyfrowane: Ochrony ważnych danych w bazie danych SQL i przechowywania kluczy szyfrowania w magazynie certyfikatów systemu Windows

> [AZURE.SELECTOR]
- [Azure klucza magazynu](sql-database-always-encrypted-azure-key-vault.md)
- [Magazyn certyfikatów systemu Windows](sql-database-always-encrypted.md)


W tym artykule pokazano, jak do zabezpieczenia ważnych danych w bazie danych SQL przez szyfrowanie bazy danych przy użyciu [Zawsze szyfrowane kreatora](https://msdn.microsoft.com/library/mt459280.aspx) programu [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx). On również pokazano, jak przechowywanie kluczy szyfrowania w magazynie certyfikatów systemu Windows.

Zawsze zaszyfrowana to nowa technologia szyfrowania danych w bazie danych SQL Azure i SQL Server, który ułatwia ochronę danych poufnych spoczynku na serwerze, podczas przemieszczania między klientem a serwerem, a w trakcie danych jest używany, zapewnianie poufne dane nigdy nie jest wyświetlana jako zwykły tekst wewnątrz systemu bazy danych. Gdy dane są szyfrowane, tylko w aplikacjach klienckich lub serwerach aplikacji, które mają dostęp do kluczy dostęp do danych w postaci zwykłego tekstu. Aby uzyskać szczegółowe informacje zobacz [Zawsze szyfrowane (aparat bazy danych)](https://msdn.microsoft.com/library/mt163865.aspx).

Po skonfigurowaniu bazy danych zawsze szyfrowane, spowoduje utworzenie aplikacji klienckiej w C# z programem Visual Studio do pracy z zaszyfrowane dane.

Wykonaj kroki opisane w tym artykule, aby dowiedzieć się, jak skonfigurować zawsze szyfrowane dla bazy danych programu Azure SQL. W tym artykule dowiesz się, jak wykonywać następujące zadania:

- Tworzenie [Zawsze klawiszy szyfrowane](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3)za pomocą Kreatora zawsze szyfrowane w SSMS.
    - Tworzenie [Kolumny klucza (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).
    - Tworzenie [kolumny klucza szyfrowania (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).
- Tworzenie tabeli bazy danych i szyfrowanie kolumn.
- Tworzenie aplikacji, która powoduje wstawienie, wybiera i wyświetla dane z kolumn zaszyfrowane.

## <a name="prerequisites"></a>Wymagania wstępne

Ten samouczek musisz:

- Konto Azure i subskrypcji. Jeśli nie masz jedną Załóż [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).
- [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) wersji 13.0.700.242 lub nowszej.
- [.NET framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) lub nowszego (na komputerze klienckim).
- [Programu visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).



## <a name="create-a-blank-sql-database"></a>Tworzenie pustej bazy danych SQL
1. Zaloguj się do [portalu Azure](https://portal.azure.com/).
2. Kliknij przycisk **Nowy** > **danych + miejsca do magazynowania** > **bazy danych SQL**.
3. Tworzenie **pustej** bazy danych o nazwie **Klinika** na serwerze nowym lub istniejącym. Aby uzyskać szczegółowe instrukcje dotyczące tworzenia bazy danych w portalu Azure zobacz [Tworzenie bazy danych SQL w minutach](sql-database-get-started.md).

    ![Tworzenie pustej bazy danych](./media/sql-database-always-encrypted/create-database.png)

W dalszej części samouczka należy parametry połączenia. Po utworzeniu bazy danych, przejdź do nowej bazy danych Klinika i skopiuj parametry połączenia. Parametry połączenia można uzyskać w dowolnym momencie, ale łatwo skopiować go, gdy jesteś w portalu Azure.

1. Kliknij pozycję **bazy danych programu SQL** > **Klinika** > **Pokaż parametry połączenia bazy danych**.
2. Skopiuj parametry połączenia dla **ADO.NET**.

    ![Skopiuj parametry połączenia](./media/sql-database-always-encrypted/connection-strings.png)


## <a name="connect-to-the-database-with-ssms"></a>Nawiązywanie połączenia z bazą danych przy użyciu SSMS

Otwórz SSMS i połączyć się z serwerem z bazą danych Klinika.


1. Otwórz SSMS. (Kliknij pozycję **Połącz** > **Aparat bazy danych** , aby otworzyć okno **połączenie z serwerem** , jeśli nie jest otwarta).
2. Wprowadź nazwę serwera i poświadczenia. Nazwa serwera znajduje się na karta bazy danych SQL, a w parametrach połączenia skopiowany wcześniej. Wpisz pełną nazwę serwera w tym *database.windows.net*.

    ![Skopiuj parametry połączenia](./media/sql-database-always-encrypted/ssms-connect.png)

Jeśli zostanie otwarte okno **Nowej reguły zapory** , zaloguj się do Azure i umożliwić SSMS Tworzenie nowej reguły zapory dla Ciebie.


## <a name="create-a-table"></a>Tworzenie tabeli

W tej sekcji utworzy tabelę do przechowywania danych pacjentów. Są to wstępnie — normalny tabeli skonfiguruje szyfrowania w następnej sekcji.

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

SSMS udostępnia kreatora przez skonfigurowanie CMK, CEK i zaszyfrowane kolumn można skonfigurować łatwo zawsze szyfrowane.

1. Rozwiń węzeł **bazy danych** > **Klinika** > **tabel**.
2. Kliknij prawym przyciskiem myszy tabelę **pacjentów** , a następnie wybierz pozycję **Szyfrowanie kolumn** , aby otworzyć Kreatora zawsze szyfrowane:

    ![Szyfrowanie kolumn](./media/sql-database-always-encrypted/encrypt-columns.png)

Kreator zawsze szyfrowane zawiera następujące sekcje: **Zaznaczenie kolumny**, **Konfiguracji głównego** (CMK), **Sprawdzanie poprawności**i **podsumowania**.

### <a name="column-selection"></a>Zaznaczenie kolumny ###

Kliknij przycisk **Dalej** na stronie **wprowadzenie** , aby otworzyć stronę **Zaznaczenie kolumny** . Na tej stronie, możesz wybrać kolumny, które chcesz zaszyfrować, [Typ szyfrowania i jakie kolumny klucza szyfrowania (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) korzystać.

Zaszyfruj **ubezpieczenia społecznego** i **DataUrodzenia** informacje dotyczące poszczególnych pacjentów. Kolumna **ubezpieczenia społecznego** użyje deterministyczne szyfrowania, który obsługuje odnośniki równości, sprzężeń i Grupuj według. Kolumna **DataUrodzenia** użyje te szyfrowania, która nie obsługuje operacji.

Ustaw **Typ szyfrowania** w kolumnie **ubezpieczenia społecznego** **Deterministic** i kolumnę **DataUrodzenia** **Randomized**. Kliknij przycisk **Dalej**.

![Szyfrowanie kolumn](./media/sql-database-always-encrypted/column-selection.png)

### <a name="master-key-configuration"></a>Konfiguracja klucza głównego###

Strona **Konfiguracja głównego** to miejsce, w którym Konfigurowanie usługi CMK i wybierz dostawcę magazynie kluczy przechowywania CMK. Obecnie mogą zawierać CMK w magazynie certyfikatów systemu Windows, Azure klucza magazynu lub sprzęt zabezpieczeń moduły. Ten samouczek pokazano, jak przechowywanie kluczy w magazynie certyfikatów systemu Windows.

Sprawdź, czy **Magazyn certyfikatów systemu Windows** jest zaznaczone, a następnie kliknij przycisk **Dalej**.

![Konfiguracja klucza głównego](./media/sql-database-always-encrypted/master-key-configuration.png)


### <a name="validation"></a>Sprawdzanie poprawności###

Można teraz szyfrowanie kolumny lub zapisać skrypt programu PowerShell, aby uruchomić później. Ten samouczek wybierz pozycję **Kontynuuj, aby zakończyć teraz** , a następnie kliknij przycisk **Dalej**.

### <a name="summary"></a>Podsumowanie###

Upewnij się, że ustawienia są wszystkie Popraw i kliknij przycisk **Zakończ** , aby zakończyć konfigurację dla zawsze szyfrowane.

![Podsumowanie](./media/sql-database-always-encrypted/summary.png)


### <a name="verify-the-wizards-actions"></a>Sprawdź akcji kreatora

Po zakończeniu działania Kreatora bazy danych jest skonfigurowana dla zawsze szyfrowane. Kreator wykonać następujące czynności:

- Utworzony CMK.
- Utworzony CEK.
- Skonfigurowane kolumny wybrane dla szyfrowania. Tabela **pacjentów** obecnie nie ma żadnych danych, ale wszystkie istniejące dane w zaznaczonych kolumn są teraz szyfrowane.

Tworzenie kluczy w SSMS, można sprawdzić, przechodząc do **Klinika** > **zabezpieczeń** > **Zawsze szyfrowane klawiszy**. Można teraz zobaczyć nowe klucze wygenerowanych przez kreatora dla Ciebie.


## <a name="create-a-client-application-that-works-with-the-encrypted-data"></a>Tworzenie aplikacji klienckiej, która działa z zaszyfrowane dane

Teraz, gdy skonfigurowano zawsze szyfrowane można utworzyć aplikację, która wykonuje *wstawia* i *zaznacza* zaszyfrowanych kolumn. Aby pomyślnie uruchomić przykładowej aplikacji, należy uruchomić go na tym samym komputerze miejsce, w którym został uruchomiony Kreator zawsze szyfrowane. Aby uruchomić aplikację na innym komputerze, należy wdrożyć własnych certyfikatów zawsze szyfrowane na komputerze aplikację klienta.  

> [AZURE.IMPORTANT] Podczas przekazywania danych zwykłego tekstu na serwerze przy użyciu kolumn zawsze szyfrowane aplikacji należy użyć [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) obiektów. Przekazywanie literałów bez użycia obiektów SqlParameter spowoduje wyjątek.


1. Otwórz program Visual Studio i tworzenie nowej aplikacji konsoli C#. Upewnij się, że projekt ustawiono **.NET Framework** 4.6 lub nowszy.
2. Nazwa projektu **AlwaysEncryptedConsoleApp** , a następnie kliknij **przycisk OK**.

![Nowa aplikacja konsoli](./media/sql-database-always-encrypted/console-app.png)



## <a name="modify-your-connection-string-to-enable-always-encrypted"></a>Modyfikowanie ciąg połączenia umożliwiające zawsze szyfrowane

W tej sekcji wyjaśniono, jak włączyć zawsze szyfrowane w ciągu połączenia bazy danych. Należy zmodyfikować aplikację konsoli właśnie utworzonego w następnej sekcji "Zawsze szyfrowane przykładowej konsoli aplikacji".


Aby włączyć zawsze szyfrowane, należy dodać słowo kluczowe **Ustawienie szyfrowania kolumny** do ciągu połączenia i ustawianie go na **włączone**.

Można tak ustawić bezpośrednio w parametrach połączenia lub można ustawić go przy użyciu [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx). Przykładowa aplikacja w następnej sekcji pokazano, jak używać **SqlConnectionStringBuilder**.

> [AZURE.NOTE] To jest tylko zmiana wymagane w określonej aplikacji klienckiej zawsze szyfrowane. Jeśli masz istniejącą aplikację, której są magazynowane zewnętrznie jej parametrów połączenia (oznacza to, że w pliku konfiguracji), można włączyć zawsze szyfrowane bez zmieniania kodu.


### <a name="enable-always-encrypted-in-the-connection-string"></a>Włączanie zawsze szyfrowane w parametrach połączenia

Dodaj następujące słowo kluczowe do ciągu połączenia:

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-a-sqlconnectionstringbuilder"></a>Włącz zawsze szyfrowane z SqlConnectionStringBuilder

Poniższy kod przedstawia włączania zawsze szyfrowane przez ustawienie [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) na [włączone](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;



## <a name="always-encrypted-sample-console-application"></a>Zawsze szyfrowane aplikacji konsoli próbki

W przykładzie pokazano, jak:

- Modyfikowanie ciąg połączenia umożliwiające zawsze szyfrowane.
- Wstawianie danych do zaszyfrowanych kolumn.
- Wybierz rekord, filtrując dla określonej wartości w kolumnie zaszyfrowane.

Zamień zawartość **Plik Program.cs** poniższy kod. Zamień parametry połączenia dla zmiennej globalnej connectionString w wierszu bezpośrednio nad metody główne swojego prawidłowe parametry połączenia z portalem Azure. To jest tylko zmiany, które należy wprowadzić kod.

Uruchom aplikację, aby zobaczyć zawsze szyfrowane w działaniu.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Data;
    using System.Data.SqlClient;

    namespace AlwaysEncryptedConsoleApp
    {
    class Program
    {
        // Update this line with your Clinic database connection string from the Azure portal.
        static string connectionString = @"Replace with your connection string";

        static void Main(string[] args)
        {
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

            InsertPatient(new Patient() {
                SSN = "999-99-0001", FirstName = "Orlando", LastName = "Gee", BirthDate = DateTime.Parse("01/04/1964") });
            InsertPatient(new Patient() {
                SSN = "999-99-0002", FirstName = "Keith", LastName = "Harris", BirthDate = DateTime.Parse("06/20/1977") });
            InsertPatient(new Patient() {
                SSN = "999-99-0003", FirstName = "Donna", LastName = "Carreras", BirthDate = DateTime.Parse("02/09/1973") });
            InsertPatient(new Patient() {
                SSN = "999-99-0004", FirstName = "Janet", LastName = "Gates", BirthDate = DateTime.Parse("08/31/1985") });
            InsertPatient(new Patient() {
                SSN = "999-99-0005", FirstName = "Lucy", LastName = "Harrington", BirthDate = DateTime.Parse("05/06/1993") });


            // Fetch and display all patients.
            Console.WriteLine(Environment.NewLine + "All the records currently in the Patients table:");

            foreach (Patient patient in SelectAllPatients())
            {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now let's locate records by searching the encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that the user entered 11 characters.
            // In production be sure to check all user input and use the best validation for your specific application.
            do
            {
                Console.WriteLine("Please enter a valid SSN (ex. 123-45-6789):");
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

Można szybko sprawdzić, czy dane na serwerze jest zaszyfrowany przez badania danych **pacjentów** z SSMS. (Użyj bieżące połączenie gdzie ustawienie szyfrowania kolumn nie jest jeszcze włączone).

Uruchom poniższe zapytanie w bazie danych Klinika.

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

Można zobaczyć zaszyfrowanych kolumn nie zawierają żadnych danych w postaci zwykłego tekstu.

   ![Nowa aplikacja konsoli](./media/sql-database-always-encrypted/ssms-encrypted.png)


Aby uzyskać dostęp do danych w postaci zwykłego tekstu za pomocą SSMS, możesz dodać **ustawienie szyfrowania kolumny = włączone** parametr w celu połączenia.

1. W SSMS kliknij prawym przyciskiem myszy serwera w **Eksploratorze obiektów**, a następnie kliknij **Rozłącz**.
2. Kliknij pozycję **Połącz** > **Aparat bazy danych** , aby otworzyć okno **połączenie z serwerem** , a następnie kliknij pozycję **Opcje**.
3. Kliknij przycisk **Dodatkowe parametry połączenia** i typ **ustawienie szyfrowania kolumny = włączone**.

    ![Nowa aplikacja konsoli](./media/sql-database-always-encrypted/ssms-connection-parameter.png)

4. Uruchom poniższe zapytanie w bazie danych **Klinika** .

        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

     Można teraz zobaczyć dane zwykłego tekstu w kolumnach zaszyfrowane.


    ![Nowa aplikacja konsoli](./media/sql-database-always-encrypted/ssms-plaintext.png)



> [AZURE.NOTE] Jeśli łączysz się z SSMS (lub dowolnego klienta) z innego komputera, nie będą mieć dostępu do kluczy szyfrowania i nie będzie można odszyfrować danych.



## <a name="next-steps"></a>Następne kroki
Po utworzeniu bazy danych, która korzysta z zawsze szyfrowane, można wykonać następujące czynności:

- Uruchom w tym przykładzie na innym komputerze. Nie będzie on mieć dostęp do kluczy szyfrowania, nie będą mieć dostępu do danych w postaci zwykłego tekstu i nie będzie działać pomyślnie.
- [Obróć i oczyścić kluczy](https://msdn.microsoft.com/library/mt607048.aspx).
- [Migrowanie danych, który jest już zaszyfrowanych za pomocą zawsze szyfrowane](https://msdn.microsoft.com/library/mt621539.aspx).
- [Wdrażanie zawsze szyfrowane certyfikaty na innych komputerach klienckich](https://msdn.microsoft.com/library/mt723359.aspx#Anchor_1) (zobacz sekcję "Co certyfikatów dostępnych do aplikacji i użytkowników").

## <a name="related-information"></a>Informacje pokrewne

- [Zawsze szyfrowane (do opracowania klienta)](https://msdn.microsoft.com/library/mt147923.aspx)
- [Szyfrowanie danych przezroczystości](https://msdn.microsoft.com/library/bb934049.aspx)
- [Program SQL Server szyfrowania](https://msdn.microsoft.com/library/bb510663.aspx)
- [Kreator zawsze zaszyfrowanych](https://msdn.microsoft.com/library/mt459280.aspx)
- [Blogu zawsze zaszyfrowanych](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)
