<properties
   pageTitle="Używanie danych Lake sklepu .NET SDK do tworzenia aplikacji | Microsoft Azure"
   description="Używanie Azure danych Lake sklepu .NET SDK do tworzenia aplikacji"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-net-sdk"></a>Rozpoczynanie pracy z magazynu Lake danych Azure przy użyciu zestawu SDK .NET

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [Programu PowerShell](data-lake-store-get-started-powershell.md)
- [ZESTAW SDK PROGRAMU .NET](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [INTERFEJSU API USŁUGI REST](data-lake-store-get-started-rest-api.md)
- [Polecenie Azure](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Dowiedz się, jak używać [Azure danych Lake sklepu .NET SDK](https://msdn.microsoft.com/library/mt581387.aspx) do wykonywania podstawowych operacji, takich jak tworzyć foldery, przekazywanie i pobieranie plików danych itp. Aby uzyskać więcej informacji na temat Lake danych zobacz [Magazynu Lake danych Azure](data-lake-store-overview.md).

## <a name="prerequisites"></a>Wymagania wstępne

* **Visual Studio 2013 lub 2015 r**. Poniższe instrukcje za pomocą programu Visual Studio 2015 r.

* **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/pricing/free-trial/).

* **Konto azure magazynu Lake danych**. Aby uzyskać instrukcje dotyczące tworzenia konta zobacz [Rozpoczynanie pracy z magazynu Lake danych Azure](data-lake-store-get-started-portal.md)

* **Tworzenie aplikacji usługi Azure Active Directory**. Za pomocą aplikacji Azure AD służą do uwierzytelniania aplikacji magazynu Lake danych z usługą Azure Active Directory. Istnieją różne metody do uwierzytelniania Azure AD, które są **uwierzytelniania użytkowników końcowych** lub **do usługi**. Instrukcje i uzyskać więcej informacji na temat uwierzytelniania Zobacz [uwierzytelniania z magazynu Lake danych za pomocą usługi Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="create-a-net-application"></a>Tworzenie aplikacji programu .NET

1. Otwórz program Visual Studio i utworzyć aplikację konsoli.

2. W menu **plik** kliknij polecenie **Nowy**, a następnie kliknij **Projekt**.

3. Z **Nowego projektu**wpisz lub wybierz następujące wartości:

  	| Właściwość | Wartość                       |
  	|----------|-----------------------------|
  	| Kategoria | Szablony i Visual C# i systemu Windows |
  	| Szablon | Aplikacja konsoli         |
  	| Nazwa     | CreateADLApplication        |

4. Kliknij przycisk **OK** , aby utworzyć projekt.

5. Dodawanie pakietów Nuget do projektu.

    1. Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań i kliknij pozycję **Zarządzaj pakiety NuGet**.
    2. Na karcie **Menedżer pakietów Nuget** upewnij się, że **pakiet źródła** jest ustawiona na **nuget.org** i jest zaznaczone pole wyboru tej **obejmują wstępną** .
    3. Wyszukiwanie i zainstalować pakiety NuGet następujące czynności:

        * `Microsoft.Azure.Management.DataLake.Store`— W tym samouczku v0.12.5 Podgląd.
        * `Microsoft.Azure.Management.DataLake.StoreUploader`— W tym samouczku v0.10.6 Podgląd.
        * `Microsoft.Rest.ClientRuntime.Azure.Authentication`— W tym samouczku v2.2.8 Podgląd.

        ![Dodawanie źródła Nuget] (./media/data-lake-store-get-started-net-sdk/ADL.Install.Nuget.Package.png "Utwórz nowe konto Azure Lake danych")

    4. Zamknij **Menedżera pakietów Nuget**.

6. Otwórz **Plik Program.cs**, usuń istniejący kod, a następnie dołączyć następujące instrukcje, aby dodać odwołania do nazw.

        using System;
        using System.Threading;
        
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.StoreUploader;

7. Deklarować zmienne, tak jak pokazano poniżej i podaj wartości dla nazwy magazynu Lake danych i nazwa grupy zasobów, który już istnieje. Upewnij się również, lokalną ścieżkę i nazwę pliku, który podanych tutaj musi istnieć na komputerze. Dodaj następujący fragment kodu za deklaracje przestrzeni nazw.

        namespace SdkSample
        {
            class Program
            {
                private static DataLakeStoreAccountManagementClient _adlsClient;
                private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;
                
                private static string _adlsAccountName;
                private static string _resourceGroupName;
                private static string _location;
                private static string _subId;

                
                private static void Main(string[] args)
                {
                    _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with the name of your existing Data Lake Store account.
                    _resourceGroupName = "<RESOURCE-GROUP-NAME>"; // TODO: Replace this value with the name of the resource group containing your Data Lake Store account.
                    _location = "East US 2";
                    _subId = "<SUBSCRIPTION-ID>";
                    
                    string localFolderPath = @"C:\local_path\"; // TODO: Make sure this exists and can be overwritten.
                    string localFilePath = localFolderPath + "file.txt"; // TODO: Make sure this exists and can be overwritten.
                    string remoteFolderPath = "/data_lake_path/";
                    string remoteFilePath = remoteFolderPath + "file.txt";
                }
            }
        }

W pozostałych sekcjach tego artykułu widać sposobu używania dostępne metody .NET do wykonywania operacji, takich jak uwierzytelnianie, Przekaż plik itp.

## <a name="authentication"></a>Uwierzytelnianie

### <a name="if-you-are-using-end-user-authentication-recommended-for-this-tutorial"></a>Jeśli korzystasz z uwierzytelniania użytkownika końcowego (zalecane dla tego samouczka)

Za pomocą tego istniejącej Azure AD "Native Client" aplikacji; jeden jest dostarczany poniżej. Ułatwiające wykonywanie tego samouczka szybciej, zaleca się, że korzystasz z tej metody.

    // User login via interactive popup
    // Use the client ID of an existing AAD "Native Client" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "common"; // Replace this string with the user's Azure Active Directory tenant ID or domain name, if needed.
    var nativeClientApp_clientId = "1950a258-227b-4e31-a9cf-717495945fc2";
    var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientApp_clientId, new Uri("urn:ietf:wg:oauth:2.0:oob"));
    var creds = UserTokenProvider.LoginWithPromptAsync(domain, activeDirectoryClientSettings).Result;

Kilka rzeczy, które należy wiedzieć o tym wstawek powyżej.

* Ułatwiające wykonywanie samouczka szybciej, używa tej wstawkę i Azure AD Identyfikatora domeny i klienta, który jest domyślnie dostępne dla wszystkich subskrypcji Azure. Tak, można **za pomocą tego wstawek jako-znajduje się w aplikacji**.
* Jednak jeśli chcesz używać własnej identyfikator klienta Azure AD, jak domeny i aplikacji, należy utworzyć aplikację natywnych Azure AD i następnie użyć domeny Azure AD, identyfikator klienta i przekierować URI aplikacji utworzonej. Aby uzyskać instrukcje, zobacz temat [Tworzenie aplikacji usługi Active Directory](../resource-group-create-service-principal-portal.md#create-an-active-directory-application) .

>[AZURE.NOTE] Z instrukcjami powyżej łącza są Azure AD aplikacji sieci web. Jednak czynności są identyczne nawet wtedy, gdy wybierzesz zamiast tego utworzyć aplikację klientami. 

### <a name="if-you-are-using-service-to-service-authentication-with-client-secret"></a>Jeśli korzystasz z usługi do uwierzytelniania przy użyciu hasła klienta 

Poniższy fragment służą do uwierzytelniania aplikacji innych niż interakcyjnej przy użyciu klienta klawisz dla kapitału aplikacji / usługi / poufne. Za pomocą tego istniejącą [Azure AD "aplikację sieci Web" aplikacji](../resource-group-create-service-principal-portal.md).

    // Service principal / appplication authentication with client secret / key
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientSecret = "<AAD-application-client-secret>";
    var clientCredential = new ClientCredential(webApp_clientId, clientSecret);
    var creds = ApplicationTokenProvider.LoginSilentAsync(domain, clientCredential).Result;

### <a name="if-you-are-using-service-to-service-authentication-with-certificate"></a>Jeśli korzystasz z certyfikatem uwierzytelniania do usługi

Jak trzecią możliwość, poniższy fragment może służyć do innych niż interakcyjnej uwierzytelniania aplikacji przy użyciu certyfikatu dla kapitału aplikacji / usługi. Za pomocą tego istniejącą [Azure AD "aplikację sieci Web" aplikacji](../resource-group-create-service-principal-portal.md).

    // Service principal / application authentication with certificate
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    System.Security.Cryptography.X509Certificates.X509Certificate2 clientCert = <AAD-application-client-certificate>
    var clientAssertionCertificate = new ClientAssertionCertificate(webApp_clientId, clientCert);
    var creds = ApplicationTokenProvider.LoginSilentWithCertificateAsync(domain, clientAssertionCertificate).Result;

## <a name="create-client-objects"></a>Tworzenie klienta obiektów

Poniższy fragment tworzy klienta konto i plików magazynu Lake danych obiektów, które są używane do wykonania żądania usługi.

    // Create client objects and set the subscription ID
    _adlsClient = new DataLakeStoreAccountManagementClient(creds);
    _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(creds);

    _adlsClient.SubscriptionId = _subId;

## <a name="list-all-data-lake-store-accounts-within-a-subscription"></a>Lista wszystkich kont magazynu Lake danych w ramach subskrypcji

Poniższy fragment zawiera listę wszystkich kont magazynu Lake danych w ramach danej subskrypcji Azure.

    // List all ADLS accounts within the subscription
    public static List<DataLakeStoreAccount> ListAdlStoreAccounts()
    {
        var response = _adlsClient.Account.List();
        var accounts = new List<DataLakeStoreAccount>(response);
        
        while (response.NextPageLink != null)
        {
            response = _adlsClient.Account.ListNext(response.NextPageLink);
            accounts.AddRange(response);
        }
        
        return accounts;
    }

## <a name="create-a-directory"></a>Tworzenie katalogu

Poniższej wstawek `CreateDirectory` metodę, która umożliwia tworzenie katalogu w konta magazynu Lake danych.

    // Create a directory
    public static void CreateDirectory(string path)
    {
        _adlsFileSystemClient.FileSystem.Mkdirs(_adlsAccountName, path);
    }

## <a name="upload-a-file"></a>Przekazywanie pliku

Poniższej wstawek `UploadFile` metodę, która umożliwia przekazywanie plików do konta magazynu Lake danych.

    // Upload a file
    public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
    {
        var parameters = new UploadParameters(srcFilePath, destFilePath, _adlsAccountName, isOverwrite: force);
        var frontend = new DataLakeStoreFrontEndAdapter(_adlsAccountName, _adlsFileSystemClient);
        var uploader = new DataLakeStoreUploader(parameters, frontend);
        uploader.Execute();
    }

`DataLakeStoreUploader`obsługuje cykliczne przekazywania i pobierania między ścieżka do pliku lokalnego i ścieżkę do pliku magazynu Lake danych.    

## <a name="get-file-or-directory-info"></a>Uzyskiwanie informacji o pliku lub katalogu

Poniższej wstawkę `GetItemInfo` metodę, która umożliwia pobieranie informacji o pliku lub katalogu, które są dostępne w magazynie Lake danych. 

    // Get file or directory info
    public static FileStatusProperties GetItemInfo(string path)
    {
        return _adlsFileSystemClient.FileSystem.GetFileStatus(_adlsAccountName, path).FileStatus;
    }

## <a name="list-file-or-directories"></a>Lista plików lub katalogów

Poniższej wstawek `ListItem` metodę, która służy do wyświetlania listy plików i katalogów z konta magazynu Lake danych.

    // List files and directories
    public static List<FileStatusProperties> ListItems(string directoryPath)
    {
        return _adlsFileSystemClient.FileSystem.ListFileStatus(_adlsAccountName, directoryPath).FileStatuses.FileStatus.ToList();
    }

## <a name="concatenate-files"></a>ZŁĄCZ.teksty plików

Poniższej wstawek `ConcatenateFiles` metody, używanym do łączenia plików. 

    // Concatenate files
    public static void ConcatenateFiles(string[] srcFilePaths, string destFilePath)
    {
        _adlsFileSystemClient.FileSystem.Concat(_adlsAccountName, destFilePath, srcFilePaths);
    }

## <a name="append-to-a-file"></a>Dołączanie do pliku

Poniższej wstawek `AppendToFile` metody, którego używasz, dołączanie danych do pliku przechowywanego na koncie magazynu Lake danych.

    // Append to file
    public static void AppendToFile(string path, string content)
    {
        var stream = new MemoryStream(Encoding.UTF8.GetBytes(content));
        
        _adlsFileSystemClient.FileSystem.Append(_adlsAccountName, path, stream);
    }

## <a name="download-a-file"></a>Pobieranie pliku

Poniższej wstawek `DownloadFile` metoda, służąca do pobrania pliku z konta magazynu Lake danych.

    // Download file
    public static void DownloadFile(string srcPath, string destPath)
    {
        var stream = _adlsFileSystemClient.FileSystem.Open(_adlsAccountName, srcPath);
        var fileStream = new FileStream(destPath, FileMode.Create);
        
        stream.CopyTo(fileStream);
        fileStream.Close();
        stream.Close();
    }

## <a name="next-steps"></a>Następne kroki

- [Zabezpieczanie danych w magazynie Lake danych](data-lake-store-secure-data.md)
- [Używanie analizy Lake Azure danych z magazynu Lake danych](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Usługa Azure HDInsight za pomocą magazynu Lake danych](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Odwołanie SDK .NET magazynu Lake danych](https://msdn.microsoft.com/library/mt581387.aspx)
- [Odwołanie pozostałych magazynu Lake danych](https://msdn.microsoft.com/library/mt693424.aspx)
