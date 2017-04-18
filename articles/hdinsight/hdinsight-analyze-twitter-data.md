<properties
    pageTitle="Analizowanie danych Twitter za Hadoop w HDInsight | Microsoft Azure"
    description="Dowiedz się, jak za pomocą gałęzi do analizowania danych w serwisie Twitter na Hadoop w HDInsight, aby znaleźć częstotliwość zastosowania danego wyrazu."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>

# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a>Analizowanie danych Twitter przy użyciu gałąź w HDInsight

Społecznościowych witryn sieci Web to jeden z głównych siły kierowania do wdrażania duży danych. Publicznej interfejsy API dostarczony przez podobnych Twitter witryn są przydatne źródła danych do analizowania i opis trendów popularne. W tym samouczku będzie uzyskać tweety za pomocą Twitter streaming interfejsu API, a następnie użyj gałęzi Apache Azure HDInsight w celu uzyskania listy użytkowników Twitter wysyłane większość tweety, zawierających określony wyraz.

> [AZURE.NOTE] Kroki opisane w tym dokumencie wymagają klastrze HDInsight systemu Windows. Dla określonych czynności, aby klastrze systemem Linux zobacz [Analizowanie serwisu Twitter danych przy użyciu gałąź w HDInsight (Linux)](hdinsight-analyze-twitter-data-linux.md).


##<a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego samouczka, musisz mieć następujące czynności:

- **Pracy** z programem PowerShell Azure zainstalowaniu i skonfigurowaniu. 

    Aby wykonać skrypty środowiska Windows PowerShell, możesz uruchomić Azure programu PowerShell jako administrator i ustawić zasady wykonywania *RemoteSigned*. Zobacz [skrypty środowiska Windows PowerShell Uruchom][powershell-script].

    Przed uruchomieniem skrypty środowiska Windows PowerShell, upewnij się, że są podłączone do subskrypcji usługi Azure za pomocą następującego polecenia cmdlet:

        Login-AzureRmAccount

    Jeśli masz wiele subskrypcji Azure, użyj następującego polecenia cmdlet ustawić bieżącej subskrypcji:

        Select-AzureRmSubscription -SubscriptionID <Azure Subscription ID>

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Klaster HDInsight Azure**. Aby uzyskać instrukcje dotyczące klaster inicjowania obsługi administracyjnej, zobacz [Wprowadzenie do korzystania z usługi HDInsight] [ hdinsight-get-started] lub [klastrów świadczenia usługi HDInsight] [hdinsight-provision]. W dalszej części samouczka konieczne będzie jego nazwy.

W poniższej tabeli wymieniono pliki używane w tym samouczku:

Pliki|Opis
---|---
/Tutorials/Twitter/Data/tweets.txt|Dane źródłowe dla zadania gałęzi.
/Tutorials/Twitter/Output|Folder wyjściowy dla zadania gałęzi. Domyślnej nazwy pliku wyjściowego zadania gałąź jest **000000_0**.
Tutorials/Twitter/Twitter.hql|Plik skryptu HiveQL.
/Tutorials/Twitter/JobStatus|Stan zadania Hadoop.


##<a name="get-twitter-feed"></a>Uzyskiwanie Twitter kanału

W tym samouczku użyjesz, [serwisu Twitter przesyłanie strumieniowe interfejsy API][twitter-streaming-api]. Szczególne serwisu Twitter streaming interfejsu API będzie używany jest [statusy/filtru][twitter-statuses-filter].

>[AZURE.NOTE] Plik zawierający 10 000 tweety i plik skryptu gałęzi (objęte w następnej sekcji) zostały przekazane w kontenerze obiektów Blob publicznej. Jeśli ma być używany do przekazywania plików, możesz pominąć tę sekcję.

[Tweety dane](https://dev.twitter.com/docs/platform-objects/tweets) są przechowywane w formacie notacji obiektu JavaScript (JSON), który zawiera złożonej strukturze zagnieżdżonych. Zamiast liniami wiele kodu przy użyciu konwencjonalny język programowania, można przekształcić ten zagnieżdżonych struktury do tabelę programu Hive, tak, aby go można wyszukiwać według języka SQL (Structured Query) — języka o nazwie HiveQL, takich jak.

Twitter używa OAuth w celu zapewnienia dostępu autoryzowanych do jego interfejs API. OAuth jest protokołem uwierzytelniania, którego użytkownicy mogą zatwierdzać aplikacje do działania w imieniu tej osoby bez konieczności udostępniania swoje hasło. Więcej informacji można znaleźć w [oauth.net](http://oauth.net/) lub doskonałe [dla początkujących OAuth](http://hueniverse.com/oauth/) z Hueniverse.

Pierwszy krok, aby użyć OAuth jest utworzenie nowej aplikacji w witrynie serwisu Twitter Deweloper.

**Aby utworzyć aplikację Twitter**

1. Zaloguj się do [https://apps.twitter.com/](https://apps.twitter.com/). Kliknij łącze **Utwórz konto teraz** , jeśli nie masz konta Twitter.
2. Kliknij przycisk **Utwórz nową aplikację**.
3. Wprowadź **nazwę**, **Opis** **witryny sieci Web**. Można uzupełnić adresu URL w polu **witryny sieci Web** . W poniższej tabeli przedstawiono kilka przykładowych wartości do używania:

    Pole|Wartość
    ---|---
    Nazwa|MyHDInsightApp
    Opis|MyHDInsightApp
    Witryna sieci Web|http://www.myhdinsightapp.com

4. Sprawdzanie **Tak, zgoda**, a następnie kliknij **Tworzenie aplikacji Twitter**.
5. Kliknij kartę **uprawnienia** . Domyślne uprawnienia jest **tylko do odczytu**. Jest to wystarczające dla tego samouczka.
6. Kliknij kartę **klawiszy i tokeny dostępu** .
7. Kliknij pozycję **Utwórz moje token dostępu**.
8. Kliknij przycisk **Testuj OAuth** w prawym górnym rogu strony.
9. Zanotuj **klucz**, **hasła dla klientów indywidualnych**, **token dostępu**i **tajny token dostępu**. Wartości muszą w dalszej części samouczka.

W tym samouczku będzie nawiązywanie połączeń usługi sieci web za pomocą programu Windows PowerShell. Aby .NET C# próbki, zobacz [Analiza w czasie rzeczywistym upodobania Twitter z HBase w HDInsight][hdinsight-hbase-twitter-sentiment]. Popularne narzędzie do nawiązywania połączeń usługi sieci web jest [*zawinięcie*][curl]. Zwinięcie można pobrać z [tutaj][curl-download].

>[AZURE.NOTE] Użycie polecenia zwinięcie w systemie Windows, użyj cudzysłowów zamiast apostrofy dla wartości opcji.

**Aby uzyskać tweety**

1. Otwórz Windows PowerShell zintegrowane środowisko skryptów (ISE). (Na ekranie startowym systemu Windows 8, wpisz **PowerShell_ISE** i kliknij pozycję **Windows PowerShell ISE**. Zobacz [Uruchamianie programu Windows PowerShell w systemie Windows 8 i Windows][powershell-start].)

2. Skopiuj poniższy skrypt do okienka Skrypt:

        #region - variables and constants
        $clusterName = "<HDInsightClusterName>" # Enter the HDInsight cluster name

        # Enter the OAuth information for your Twitter application
        $oauth_consumer_key = "<TwitterAppConsumerKey>";
        $oauth_consumer_secret = "<TwitterAppConsumerSecret>";
        $oauth_token = "<TwitterAppAccessToken>";
        $oauth_token_secret = "<TwitterAppAccessTokenSecret>";

        $destBlobName = "tutorials/twitter/data/tweets.txt" # This script saves the tweets into this blob.

        $trackString = "Azure, Cloud, HDInsight" # This script gets the tweets containing these keywords.
        $track = [System.Uri]::EscapeDataString($trackString);
        $lineMax = 10000  # The script will get this number of tweets. It is about 3 minutes every 100 lines.
        #endregion

        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        Login-AzureRmAccount
        #endregion

        #region - Create a block blob object for writing tweets into Blob storage
        Write-Host "Get the default storage account name and Blob container name using the cluster name ..." -ForegroundColor Green
        $myCluster = Get-AzureRmHDInsightCluster -Name $clusterName
        $resourceGroupName = $myCluster.ResourceGroup
        $storageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
        $containerName = $myCluster.DefaultStorageContainer
        Write-Host "`tThe storage account name is $storageAccountName." -ForegroundColor Yellow
        Write-Host "`tThe blob container name is $containerName." -ForegroundColor Yellow

        Write-Host "Define the Azure storage connection string ..." -ForegroundColor Green
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$storageAccountName;AccountKey=$storageAccountKey"
        Write-Host "`tThe connection string is $storageConnectionString." -ForegroundColor Yellow

        Write-Host "Create block blob object ..." -ForegroundColor Green
        $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
        $storageClient = $storageAccount.CreateCloudBlobClient();
        $storageContainer = $storageClient.GetContainerReference($containerName)
        $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
        #end region

        # region - Format OAuth strings
        Write-Host "Format oauth strings ..." -ForegroundColor Green
        $oauth_nonce = [System.Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes([System.DateTime]::Now.Ticks.ToString()));
        $ts = [System.DateTime]::UtcNow - [System.DateTime]::ParseExact("01/01/1970", "dd/MM/yyyy", $null)
        $oauth_timestamp = [System.Convert]::ToInt64($ts.TotalSeconds).ToString();

        $signature = "POST&";
        $signature += [System.Uri]::EscapeDataString("https://stream.twitter.com/1.1/statuses/filter.json") + "&";
        $signature += [System.Uri]::EscapeDataString("oauth_consumer_key=" + $oauth_consumer_key + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_nonce=" + $oauth_nonce + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_signature_method=HMAC-SHA1&");
        $signature += [System.Uri]::EscapeDataString("oauth_timestamp=" + $oauth_timestamp + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_token=" + $oauth_token + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_version=1.0&");
        $signature += [System.Uri]::EscapeDataString("track=" + $track);

        $signature_key = [System.Uri]::EscapeDataString($oauth_consumer_secret) + "&" + [System.Uri]::EscapeDataString($oauth_token_secret);

        $hmacsha1 = new-object System.Security.Cryptography.HMACSHA1;
        $hmacsha1.Key = [System.Text.Encoding]::ASCII.GetBytes($signature_key);
        $oauth_signature = [System.Convert]::ToBase64String($hmacsha1.ComputeHash([System.Text.Encoding]::ASCII.GetBytes($signature)));

        $oauth_authorization = 'OAuth ';
        $oauth_authorization += 'oauth_consumer_key="' + [System.Uri]::EscapeDataString($oauth_consumer_key) + '",';
        $oauth_authorization += 'oauth_nonce="' + [System.Uri]::EscapeDataString($oauth_nonce) + '",';
        $oauth_authorization += 'oauth_signature="' + [System.Uri]::EscapeDataString($oauth_signature) + '",';
        $oauth_authorization += 'oauth_signature_method="HMAC-SHA1",'
        $oauth_authorization += 'oauth_timestamp="' + [System.Uri]::EscapeDataString($oauth_timestamp) + '",'
        $oauth_authorization += 'oauth_token="' + [System.Uri]::EscapeDataString($oauth_token) + '",';
        $oauth_authorization += 'oauth_version="1.0"';

        $post_body = [System.Text.Encoding]::ASCII.GetBytes("track=" + $track);
        #endregion

        #region - Read tweets
        Write-Host "Create HTTP web request ..." -ForegroundColor Green
        [System.Net.HttpWebRequest] $request = [System.Net.WebRequest]::Create("https://stream.twitter.com/1.1/statuses/filter.json");
        $request.Method = "POST";
        $request.Headers.Add("Authorization", $oauth_authorization);
        $request.ContentType = "application/x-www-form-urlencoded";
        $body = $request.GetRequestStream();

        $body.write($post_body, 0, $post_body.length);
        $body.flush();
        $body.close();
        $response = $request.GetResponse() ;

        Write-Host "Start stream reading ..." -ForegroundColor Green

        Write-Host "Define a MemoryStream and a StreamWriter for writing ..." -ForegroundColor Green
        $memStream = New-Object System.IO.MemoryStream
        $writeStream = New-Object System.IO.StreamWriter $memStream

        $sReader = New-Object System.IO.StreamReader($response.GetResponseStream())

        $inrec = $sReader.ReadLine()
        $count = 0
        while (($inrec -ne $null) -and ($count -le $lineMax))
        {
            if ($inrec -ne "")
            {
                Write-Host "`n`t $count tweets received." -ForegroundColor Yellow

                $writeStream.WriteLine($inrec)
                $count ++
            }

            $inrec=$sReader.ReadLine()
        }
        #endregion

        #region - Write tweets to Blob storage
        Write-Host "Write to the destination blob ..." -ForegroundColor Green
        $writeStream.Flush()
        $memStream.Seek(0, "Begin")
        $destBlob.UploadFromStream($memStream)

        $sReader.close()
        #endregion

        Write-Host "Completed!" -ForegroundColor Green

3. Ustawianie pierwszego pięciu do ośmiu zmiennych w skrypt:


    Zmienna|Opis
    ---|---
    $clusterName|Jest to nazwa klaster HDInsight, w którym chcesz uruchomić aplikację.
    $oauth_consumer_key|Jest aplikacji Twitter **klucz** , który możesz zapisanym wcześniej podczas tworzenia aplikacji Twitter.
    $oauth_consumer_secret|Jest to aplikacji Twitter **hasła dla klientów indywidualnych** , które zapisanej wcześniej.
    $oauth_token|To jest aplikacja Twitter **token dostępu** , które zapisanej wcześniej.
    $oauth_token_secret|To jest aplikacja Twitter **tajny token dostępu** , które zapisanej wcześniej.
    $destBlobName|Jest to nazwa obiektów blob dane wyjściowe. Wartość domyślna to **tutorials/twitter/data/tweets.txt**. Jeśli zmienisz wartość domyślna, należy odpowiednio zaktualizować skrypty środowiska Windows PowerShell.
    $trackString|Usługa sieci web zwróci tweety związane z tych słów kluczowych. Wartość domyślna to **Azure, chmury, HDInsight**. Jeśli zmienisz wartość domyślna zostanie odpowiednio zaktualizowany skrypty środowiska Windows PowerShell.
    $lineMax|Wartość określa liczbę tweety odczyta skrypt. Czytanie 100 tweety trwa około trzech minut. Można ustawić większą liczbę, ale zajmie więcej czasu, aby pobrać.

5. Naciśnij klawisz **F5** , aby uruchomić skrypt. Jeśli wystąpią problemy, aby obejść ten problem, zaznacz wszystkie wiersze, a następnie naciśnij klawisz **F8**.
6. Zapewniają "Wykonano!" na koniec danych wyjściowych. Komunikaty o błędach będą wyświetlane w kolorze czerwonym.

Jako procedurę sprawdzania poprawności możesz sprawdzić w pliku docelowym **/tutorials/twitter/data/tweets.txt**w magazynie obiektów Blob platformy Azure za pomocą Eksploratora magazynu Azure lub Azure programu PowerShell. Aby uzyskać przykładowy skrypt programu Windows PowerShell do wyświetlania listy plików, zobacz [Magazyn obiektów Blob korzystanie z usługi HDInsight][hdinsight-storage-powershell].



##<a name="create-hiveql-script"></a>Tworzenie skryptu HiveQL

Przy użyciu programu PowerShell Azure, można uruchomić instrukcje z wieloma HiveQL jeden pojedynczo lub pakowanie w pliku skryptu instrukcji HiveQL. W tym samouczku utworzysz skrypt HiveQL. Plik skryptu należy przekazać magazynem obiektów Blob platformy Azure. W następnej sekcji uruchomi plik skryptu przy użyciu programu PowerShell Azure.

>[AZURE.NOTE] Plik skryptu gałęzi i plik zawierający 10 000 tweety zostały przekazane w kontenerze obiektów Blob publicznej. Jeśli ma być używany do przekazywania plików, możesz pominąć tę sekcję.

Skrypt HiveQL będzie wykonywać następujące czynności:

1. **Tabelę tweets_raw** w przypadku tabeli już istnieje.
2. **Tworzenie tabeli gałąź tweets_raw**. Ta gałąź strukturalnych Tabela tymczasowa przechowywane dane na potrzeby dalszego, przekształcanie i ładowanie przetwarzanie (ETL). Uzyskać informacji na temat partycje, zobacz [Samouczek gałęzi][apache-hive-tutorial].  
3. **Załaduj dane** z folderu źródłowego /tutorials/twitter/data. Duże tweety zestawu danych w formacie JSON zagnieżdżonych występują zostały przetworzone na tymczasowe strukturę tabeli gałęzi.
3. **Tabelę tweety** w przypadku tabeli już istnieje.
4. **Tworzenie tabeli tweety**. Przed kwerendy można przed tweety zestawu danych przy użyciu gałęzi, należy uruchomić inny proces ETL. Ten proces ETL definiuje schemat tabeli bardziej szczegółowe dane, które są przechowywane w tabeli "twitter_raw".  
5. **Wstawianie tabeli Zastąp**. Ten złożonym gałęzi będzie rozpocząć zestaw długich zadań MapReduce przez klaster Hadoop. W zależności od swojego zestawu danych i rozmiar klaster może to potrwać około 10 minut.
6. **Wstawianie zastąpić katalogu**. Uruchamianie kwerendy i wyjściowych zestawu danych w pliku. Ta kwerenda zwróci listę użytkowników Twitter, których wysłane większość tweety, zawierających wyraz "Azure".

**Aby utworzyć skrypt gałęzi i przekaż go do Azure**

1. Otwórz program Windows PowerShell ISE.
2. Skopiuj poniższy skrypt do okienka Skrypt:

        #region - variables and constants
        $clusterName = "<Existing HDInsight Cluster Name>" # Enter your HDInsight cluster name
        $subscriptionID = "<Azure Subscription ID>"
        
        $sourceDataPath = "/tutorials/twitter/data"
        $outputPath = "/tutorials/twitter/output"
        $hqlScriptFile = "tutorials/twitter/twitter.hql"
        
        $hqlStatements = @"
        set hive.exec.dynamic.partition = true;
        set hive.exec.dynamic.partition.mode = nonstrict;
        
        DROP TABLE tweets_raw;
        CREATE EXTERNAL TABLE tweets_raw (
            json_response STRING
        )
        STORED AS TEXTFILE LOCATION '$sourceDataPath';
        
        DROP TABLE tweets;
        CREATE TABLE tweets
        (
            id BIGINT,
            created_at STRING,
            created_at_date STRING,
            created_at_year STRING,
            created_at_month STRING,
            created_at_day STRING,
            created_at_time STRING,
            in_reply_to_user_id_str STRING,
            text STRING,
            contributors STRING,
            retweeted STRING,
            truncated STRING,
            coordinates STRING,
            source STRING,
            retweet_count INT,
            url STRING,
            hashtags array<STRING>,
            user_mentions array<STRING>,
            first_hashtag STRING,
            first_user_mention STRING,
            screen_name STRING,
            name STRING,
            followers_count INT,
            listed_count INT,
            friends_count INT,
            lang STRING,
            user_location STRING,
            time_zone STRING,
            profile_image_url STRING,
            json_response STRING
        );
        
        FROM tweets_raw
        INSERT OVERWRITE TABLE tweets
        SELECT
            cast(get_json_object(json_response, '$.id_str') as BIGINT),
            get_json_object(json_response, '$.created_at'),
            concat(substr (get_json_object(json_response, '$.created_at'),1,10),' ',
            substr (get_json_object(json_response, '$.created_at'),27,4)),
            substr (get_json_object(json_response, '$.created_at'),27,4),
            case substr (get_json_object(json_response, '$.created_at'),5,3)
                when "Jan" then "01"
                when "Feb" then "02"
                when "Mar" then "03"
                when "Apr" then "04"
                when "May" then "05"
                when "Jun" then "06"
                when "Jul" then "07"
                when "Aug" then "08"
                when "Sep" then "09"
                when "Oct" then "10"
                when "Nov" then "11"
                when "Dec" then "12" end,
            substr (get_json_object(json_response, '$.created_at'),9,2),
            substr (get_json_object(json_response, '$.created_at'),12,8),
            get_json_object(json_response, '$.in_reply_to_user_id_str'),
            get_json_object(json_response, '$.text'),
            get_json_object(json_response, '$.contributors'),
            get_json_object(json_response, '$.retweeted'),
            get_json_object(json_response, '$.truncated'),
            get_json_object(json_response, '$.coordinates'),
            get_json_object(json_response, '$.source'),
            cast (get_json_object(json_response, '$.retweet_count') as INT),
            get_json_object(json_response, '$.entities.display_url'),
            array(
                trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[1].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[2].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[3].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[4].text')))),
            array(
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[1].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[2].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[3].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[4].screen_name')))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
            get_json_object(json_response, '$.user.screen_name'),
            get_json_object(json_response, '$.user.name'),
            cast (get_json_object(json_response, '$.user.followers_count') as INT),
            cast (get_json_object(json_response, '$.user.listed_count') as INT),
            cast (get_json_object(json_response, '$.user.friends_count') as INT),
            get_json_object(json_response, '$.user.lang'),
            get_json_object(json_response, '$.user.location'),
            get_json_object(json_response, '$.user.time_zone'),
            get_json_object(json_response, '$.user.profile_image_url'),
            json_response
        WHERE (length(json_response) > 500);
        
        INSERT OVERWRITE DIRECTORY '$outputPath'
        SELECT name, screen_name, count(1) as cc
            FROM tweets
            WHERE text like "%Azure%"
            GROUP BY name,screen_name
            ORDER BY cc DESC LIMIT 10;
        "@
        #endregion
        
        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        
        Try{
            Get-AzureRmSubscription
        }
        Catch{
            Login-AzureRmAccount
        }
        
        Select-AzureRmSubscription -SubscriptionId $subscriptionID
        
        #endregion
        
        #region - Create a block blob object for writing the Hive script file
        Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
        $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroupName = $myCluster.ResourceGroup
        $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
        $defaultBlobContainerName = $myCluster.DefaultStorageContainer
        Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
        Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow
        
        Write-Host "Define the connection string ..." -ForegroundColor Green
        $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
        $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"
        
        Write-Host "Create block blob objects referencing the hql script file" -ForegroundColor Green
        $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
        $storageClient = $storageAccount.CreateCloudBlobClient();
        $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
        $hqlScriptBlob = $storageContainer.GetBlockBlobReference($hqlScriptFile)
        
        Write-Host "Define a MemoryStream and a StreamWriter for writing ... " -ForegroundColor Green
        $memStream = New-Object System.IO.MemoryStream
        $writeStream = New-Object System.IO.StreamWriter $memStream
        $writeStream.Writeline($hqlStatements)
        #endregion
        
        #region - Write the Hive script file to Blob storage
        Write-Host "Write to the destination blob ... " -ForegroundColor Green
        $writeStream.Flush()
        $memStream.Seek(0, "Begin")
        $hqlScriptBlob.UploadFromStream($memStream)
        #endregion
        
        Write-Host "Completed!" -ForegroundColor Green

        

4. Ustawianie pierwszych dwóch zmiennych w skrypt:

    Zmienna|Opis
    ---|---
    $clusterName|Wprowadź nazwę klaster HDInsight, w którym chcesz uruchomić aplikację.
    $subscriptionID|Wprowadź swój identyfikator Azure subskrypcji.
    $sourceDataPath|Miejsce, w którym kwerend gałęzi odczyta dane z lokalizacji przechowywania obiektów Blob platformy Azure. Nie musisz zmienić tej zmiennej.
    $outputPath|Miejsce przechowywania obiektów Blob platformy Azure miejsce, w którym kwerend gałęzi będzie wyjściowy wyniki. Nie musisz zmienić tej zmiennej.
    $hqlScriptFile|Lokalizacja i nazwa pliku skryptu HiveQL. Nie musisz zmienić tej zmiennej.

5. Naciśnij klawisz **F5** , aby uruchomić skrypt. Jeśli wystąpią problemy, aby obejść ten problem, zaznacz wszystkie wiersze, a następnie naciśnij klawisz **F8**.
6. Zapewniają "Wykonano!" na koniec danych wyjściowych. Komunikaty o błędach będą wyświetlane w kolorze czerwonym.

Jako procedurę sprawdzania poprawności możesz sprawdzić w pliku docelowym **/tutorials/twitter/twitter.hql**w magazynie obiektów Blob platformy Azure za pomocą Eksploratora magazynu Azure lub Azure programu PowerShell. Aby uzyskać przykładowy skrypt programu Windows PowerShell do wyświetlania listy plików, zobacz [Magazyn obiektów Blob korzystanie z usługi HDInsight][hdinsight-storage-powershell].  


##<a name="process-twitter-data-by-using-hive"></a>Proces Twitter danych przy użyciu gałęzi

Zakończono wszystkich prac. Teraz możesz wywołać skrypt gałęzi i sprawdzić wyniki.

### <a name="submit-a-hive-job"></a>Prześlij zadanie gałęzi

Użyj tego skryptu programu Windows PowerShell, aby uruchomić skrypt gałęzi. Należy ustawić pierwszy zmiennej.

>[AZURE.NOTE] Aby użyć tweety i skrypt HiveQL przekazany w dwie ostatnie sekcje, ustaw $hqlScriptFile "/ tutorials/twitter/twitter.hql". Aby korzystać z nich, które zostały przekazane do publicznej obiektów blob usługi, ustaw $hqlScriptFile "wasbs://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql".

    #region variables and constants
    $clusterName = "<Existing Azure HDInsight Cluster Name>"
    $httpUserName = "admin"
    $httpUserPassword = "<HDInsight Cluster HTTP User Password>"
    
    #use one of the following
    $hqlScriptFile = "wasbs://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql"
    $hqlScriptFile = "/tutorials/twitter/twitter.hql"
    
    $statusFolder = "/tutorials/twitter/jobstatus"
    #endregion
    
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    
    
    #region - Invoke Hive
    Write-Host "Invoke Hive ... " -ForegroundColor Green
    
    # Create the HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpUserPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)
    
    Use-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName -HttpCredential $httpCredential 
    $response = Invoke-AzureRmHDInsightHiveJob -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -file $hqlScriptFile -StatusFolder $statusFolder #-OutVariable $outVariable
    
    Write-Host "Display the standard error log ... " -ForegroundColor Green
    $jobID = ($response | Select-String job_ | Select-Object -First 1) -replace ‘\s*$’ -replace ‘.*\s’
    Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $jobID -DefaultContainer $defaultBlobContainerName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -HttpCredential $httpCredential
    #endregion

### <a name="check-the-results"></a>Sprawdź wyniki

Użyj tego skryptu programu Windows PowerShell, aby sprawdzić wynik zadania gałęzi. Będzie konieczne ustawienie pierwszych dwóch zmiennych.

    #region variables and constants
    $clusterName = "<Existing Azure HDInsight Cluster Name>"
    
    $blob = "tutorials/twitter/output/000000_0" # The name of the blob to be downloaded.
    #engregion
    
    #region - Create an Azure storage context object
    Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    
    Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow
    
    Write-Host "Create a context object ... " -ForegroundColor Green
    $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey  
    #endregion
    
    #region - Download blob and display blob
    Write-Host "Download the blob ..." -ForegroundColor Green
    cd $HOME
    Get-AzureStorageBlobContent -Container $defaultBlobContainerName -Blob $blob -Context $storageContext -Force
    
    Write-Host "Display the output ..." -ForegroundColor Green
    Write-Host "==================================" -ForegroundColor Green
    cat "./$blob"
    Write-Host "==================================" -ForegroundColor Green
    #end region

> [AZURE.NOTE] Tabelę programu Hive używa \001 jako ogranicznik pola. Ogranicznik nie jest widoczny w wyniku kwerendy.

Po wyniki analizy zostały umieszczone w magazynie obiektów Blob platformy Azure, można eksportować dane z serwerem bazy danych i SQL Azure SQL, wyeksportuj dane do programu Excel przy użyciu dodatku Power Query lub łączenie aplikacji do danych za pomocą sterownika ODBC gałęzi. Aby uzyskać więcej informacji, zobacz [Sqoop korzystanie z usługi HDInsight][hdinsight-use-sqoop], [danych opóźnienia lotów analiza przy użyciu HDInsight][hdinsight-analyze-flight-delay-data], [Połączyć program Excel z usługą hdinsight za pomocą dodatku Power Query][hdinsight-power-query]i [Połączyć program Excel z usługą hdinsight za pomocą sterownika ODBC gałęzi Microsoft][hdinsight-hive-odbc].

##<a name="next-steps"></a>Następne kroki

W tym samouczku możemy umieścić jak zestaw JSON niestrukturalne umożliwia przekształcenie strukturalnych tabelę programu Hive kwerendy, eksplorowanie i analizowanie danych z Twitter przy użyciu HDInsight Azure. Aby uzyskać więcej informacji, zobacz:

- [Wprowadzenie do usługi HDInsight][hdinsight-get-started]
- [Analizowanie w czasie rzeczywistym upodobania Twitter z HBase w HDInsight][hdinsight-hbase-twitter-sentiment]
- [Analizowanie danych opóźnienia lotów przy użyciu usługi HDInsight][hdinsight-analyze-flight-delay-data]
- [Łączenie programu Excel z usługą hdinsight za pomocą dodatku Power Query][hdinsight-power-query]
- [Nawiązywanie połączenia programu Excel z usługą hdinsight za pomocą sterownika ODBC gałęzi firmy Microsoft][hdinsight-hive-odbc]
- [Używanie Sqoop z usługi HDInsight][hdinsight-use-sqoop]

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176961.aspx


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]: ../hdinsight-hadoop-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-odbc-driver.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
