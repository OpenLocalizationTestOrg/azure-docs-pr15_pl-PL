<properties
   pageTitle="Można opracowywać aplikacje za pomocą danych Lake sklepu Java SDK | Microsoft Azure"
   description="Używanie Azure Data Lake sklepu Java SDK do tworzenia aplikacji"
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
   ms.date="10/17/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-java"></a>Rozpoczynanie pracy z magazynu Lake danych Azure za pomocą języka Java

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [Programu PowerShell](data-lake-store-get-started-powershell.md)
- [ZESTAW SDK PROGRAMU .NET](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [INTERFEJSU API USŁUGI REST](data-lake-store-get-started-rest-api.md)
- [Polecenie Azure](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Dowiedz się, jak używać Azure Data Lake sklepu Java SDK do wykonywania podstawowych operacji, takich jak tworzyć foldery, przekazywanie i pobieranie plików danych itp. Aby uzyskać więcej informacji na temat Lake danych zobacz [Magazynu Lake danych Azure](data-lake-store-overview.md).

Masz dostęp do dokumentów interfejsu API języka Java zestawu SDK Azure danych Lake magazynu w [interfejsu API języka Java sklepu Azure danych Lake dokumenty](https://azure.github.io/azure-data-lake-store-java/javadoc/).

## <a name="prerequisites"></a>Wymagania wstępne

* Java Development Kit (JDK 7 lub nowszym, przy użyciu języka Java wersji 1.7 lub nowszej)
* Konto Azure magazynu Lake danych. Postępuj zgodnie z instrukcjami na [Rozpoczynanie pracy z magazynu Lake danych Azure za pomocą portalu Azure](data-lake-store-get-started-portal.md).
* [Środowiska maven](https://maven.apache.org/install.html). Ten samouczek używa środowiska Maven zależności kompilacji i project. Chociaż można tworzyć bez użycia systemu kompilacji, takich jak środowiska Maven lub Gradle, te upewnij systemów jest znacznie ułatwia zarządzanie zależności.
* (Opcjonalnie) I IDE, takie jak [Ogólny obraz IntelliJ](https://www.jetbrains.com/idea/download/) lub [Zaćmienie](https://www.eclipse.org/downloads/) lub podobne.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Jak uwierzytelniania za pomocą usługi Azure Active Directory

W tym samouczku będziemy używać hasła klienta aplikacji Azure AD do pobierania token usługi Azure Active Directory (uwierzytelnianie do usługi). Aby utworzyć obiekt klienta magazynu Lake danych do wykonywania operacji pliku i katalogu operacji firma Microsoft korzysta z ten token. Aby uzyskać instrukcje dotyczące uwierzytelniania z magazynu Lake danych Azure za pomocą hasła klienta możemy wykonaj następujące czynności wysokiego poziomu:

1. Tworzenie aplikacji sieci web Azure AD
2. Pobieranie identyfikator klienta, tajny klienta i token końcowy Azure AD aplikacji sieci web.
3. Konfigurowanie dostępu dla aplikacji sieci web Azure AD na magazynu Lake danych plik/folder, w którym chcesz uzyskać dostęp z poziomu aplikacji Java do tworzonego.

Aby uzyskać instrukcje, jak wykonać te kroki zobacz [Tworzenie aplikacji usługi Active Directory](data-lake-store-authenticate-using-active-directory.md#create-an-active-directory-application).

Azure Active Directory zawiera inne opcje, a także w celu pobrania token. Możesz wybrać z wielu różnych mechanizmy odpowiednio do scenariusza, na przykład aplikacja działa w przeglądarce, aplikacja distributed jako aplikacji klasycznej lub aplikacja serwera z lokalnego lub w Azure maszyn wirtualnych. Możesz również wybrać z różnych rodzajów poświadczeń, takich jak haseł, certyfikatów, uwierzytelniania 2 itd. Ponadto usługi Azure Active Directory umożliwia synchronizowanie użytkowników usługi Active Directory w lokalnej z chmurą. Aby uzyskać szczegółowe informacje zobacz [Scenariusze uwierzytelniania dla usługi Azure Active Directory](../active-directory/active-directory-authentication-scenarios.md). 

## <a name="create-a-java-application"></a>Tworzenie aplikacji Java

Kod przykładowe dostępne [na GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/) przeprowadzi użytkownika przez proces tworzenia plików w sklepie łączenia plików, pobierania pliku i usuń niektóre pliki w magazynie. W tej sekcji tego artykułu pomagają głównym fragmenty kodu.

1. Tworzenie projektu środowiska Maven przy użyciu [mvn archetype](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) z wiersza polecenia lub IDE. Aby uzyskać instrukcje dotyczące tworzenia projektu Java przy użyciu IntelliJ, zobacz [poniżej](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html). Aby uzyskać instrukcje dotyczące tworzenia projektu przy użyciu Zaćmienie, zobacz [poniżej](http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm). 

2. Dodawanie następujących zależności do pliku **pom.xml** środowiska Maven. Dodaj poniższy fragment tekstu między ** \<FREESPACE >** znacznika i ** \</Projekt >** znacznika:

        <dependencies>
          <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-data-lake-store-sdk</artifactId>
            <version>2.0.4-SNAPSHOT</version>
          </dependency>
          <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-nop</artifactId>
            <version>1.7.21</version>
          </dependency>
        </dependencies>

    Pierwszy zależności jest użycie SDK sklepu Lake danych (`azure-datalake-store`) z repozytorium środowiska maven. Druga współzależności (`slf4j-nop`) można określić, które framework rejestrowania dla tej aplikacji. Data Lake sklepu SDK używa elewację [slf4j](http://www.slf4j.org/) rejestrowania, które pozwala wybrać RAM popularne rejestrowania, takie jak log4j, Java rejestrowania logback itd., lub Brak rejestrowania. Na przykład możemy wyłączy rejestrowanie, w związku z tym firma Microsoft korzysta z powiązania **slf4j nop** . Aby użyć innych opcji rejestrowania w aplikacji, zobacz [poniżej](http://www.slf4j.org/manual.html#projectDep).

### <a name="add-the-application-code"></a>Dodawanie kodu aplikacji

Istnieją trzy główne elementy do kodu.

1. Uzyskać tokenu usługi Azure Active Directory

2. Umożliwia utworzenie klienta magazynu Lake danych tokenu.

3. Za pomocą klienta usługi magazynu Lake danych do wykonywania operacji.

#### <a name="step-1-obtain-an-azure-active-directory-token"></a>Krok 1: Uzyskać tokenu usługi Azure Active Directory.

SDK sklepu Lake danych zapewnia wygodny metod, które pozwalają uzyskać tokenów zabezpieczających, aby porozmawiać z konta magazynu Lake danych. Jednak zestawu SDK nie przesądza, że można używać tylko tych metod. Możesz użyć innych sposobów uzyskania token także, jak w przypadku [Azure Active Directory SDK](https://github.com/AzureAD/azure-activedirectory-library-for-java)lub niestandardowy kod.

Aby uzyskać tokenu dla aplikacji Active Directory w sieci Web utworzonej wcześniej za pomocą SDK sklepu Lake danych, za pomocą metody statyczne w `AzureADAuthenticator` zajęć. Zamień **Wypełniania-miejsca** na rzeczywiste wartości dla aplikacji sieci Web Azure Active Directory.

    private static String clientId = "FILL-IN-HERE";
    private static String authTokenEndpoint = "FILL-IN-HERE";
    private static String clientKey = "FILL-IN-HERE";

    AzureADToken token = AzureADAuthenticator.getTokenUsingClientCreds(authTokenEndpoint, clientId, clientKey);

#### <a name="step-2-create-an-azure-data-lake-store-client-adlstoreclient-object"></a>Krok 2: Tworzenie obiektu magazynu Lake danych Azure klienta (ADLStoreClient)

Tworzenie obiektu [ADLStoreClient](https://azure.github.io/azure-data-lake-store-java/javadoc/) wymaga Określ nazwę konta magazynu Lake danych i token usługi Azure Active Directory, który został wygenerowany w ostatnim kroku. Należy zauważyć, że nazwę konta magazynu Lake danych powinien być w pełni kwalifikowaną nazwę domeny. Na przykład Zamień **Wypełniania — tutaj** podobną do **mydatalakestore.azuredatalakestore.net**.

    private static String accountFQDN = "FILL-IN-HERE";  // full account FQDN, not just the account name
    ADLStoreClient client = ADLStoreClient.createClient(accountFQDN, token);

### <a name="step-3-use-the-adlstoreclient-to-perform-file-and-directory-operations"></a>Krok 3: Użyj ADLStoreClient do wykonywania operacji plików i katalogów

Poniższy kod zawiera wstawki przykład kilka typowych operacji. Możesz obejrzeć pełnego [interfejsu API zestawu SDK Java danych Lake magazynu dokumentów](https://azure.github.io/azure-data-lake-store-java/javadoc/) obiektu **ADLStoreClient** Aby wyświetlić inne czynności.
 
Należy zauważyć, że pliki są odczytywać i zapisane w przy użyciu standardowych strumieni Java. Oznacza to, że możesz warstwy dowolną strumieni Java u góry strumienie magazynu Lake danych do korzystania ze standardowych funkcji języka Java (np Drukuj strumieni sformatowane dane wyjściowe, lub na dowolnej stronie strumienie kompresji lub szyfrowania, aby uzyskać dodatkowe funkcje na górze itp.).

    // set file permission
    client.setPermission(filename, "744");

    // append to file
    stream = client.getAppendStream(filename);
    stream.write(getSampleContent());
    stream.close();

    // Read File
    InputStream in = client.getReadStream(filename);
    byte[] b = new byte[64000];
    while (in.read(b) != -1) {
        System.out.write(b);
    }
    in.close();

    // concatenate the two files into one
    List<String> fileList = Arrays.asList("/a/b/c.txt", "/a/b/d.txt");
    client.concatenateFiles("/a/b/f.txt", fileList);

    //rename the file
    client.rename("/a/b/f.txt", "/a/b/g.txt");

    // list directory contents
    List<DirectoryEntry> list = client.enumerateDirectory("/a/b", 2000);
    System.out.println("Directory listing for directory /a/b:");
    for (DirectoryEntry entry : list) {
        printDirectoryInfo(entry);
    }

    // delete directory along with all the subdirectories and files in it
    client.deleteRecursive("/a");

#### <a name="step-4-build-and-run-the-application"></a>Krok 4: Tworzenie i uruchamianie aplikacji

1. Aby uruchomić z poziomu IDE, Znajdź i kliknij przycisk **Uruchom** . Aby uruchomić ze środowiska Maven, użyj [wykonywalna: wykonywalna](http://www.mojohaus.org/exec-maven-plugin/exec-mojo.html).

2. Da słoik autonomicznej, które można uruchamiać z wiersza polecenia tworzenia słoju za pomocą wszystkie zależności dostępny za pomocą [wtyczki zestawu środowiska Maven](http://maven.apache.org/plugins/maven-assembly-plugin/usage.html). Pom.xml w [przykładzie kodu źródłowego na github](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started/blob/master/pom.xml) zawiera przykładową jak to zrobić.


## <a name="next-steps"></a>Następne kroki

- [Zabezpieczanie danych w magazynie Lake danych](data-lake-store-secure-data.md)
- [Używanie analizy Lake Azure danych z magazynu Lake danych](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Usługa Azure HDInsight za pomocą magazynu Lake danych](data-lake-store-hdinsight-hadoop-use-portal.md)
