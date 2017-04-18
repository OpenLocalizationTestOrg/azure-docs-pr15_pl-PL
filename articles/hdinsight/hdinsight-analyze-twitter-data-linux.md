<properties
    pageTitle="Analizowanie danych Twitter za Apache gałęzi na HDInsight | Microsoft Azure"
    description="Dowiedz się, jak używać Python do przechowywania Tweety, które zawierają określone słowa kluczowe, a następnie za pomocą gałąź i Hadoop na HDInsight nieprzetworzonych danych TWitter umożliwia przekształcenie można wyszukiwać tabelę programu Hive."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="larryfr"/>

# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a>Analizowanie danych Twitter przy użyciu gałąź w HDInsight

W tym dokumencie zostanie wyświetlony tweety przy użyciu Twitter streaming interfejsu API, a następnie użyj Apache gałęzi w klastrze systemem Linux HDInsight do procesu JSON sformatowane dane. Wynik będzie listy użytkowników Twitter, których wysłane większość tweety, zawierających określony wyraz.

> [AZURE.NOTE] Chociaż poszczególne części tego dokumentu mogą być używane z klastrów HDInsight systemu Windows (Python, na przykład) kilku etapach na podstawie przy użyciu klastrze HDInsight systemem Linux. Określone instrukcje dotyczące do klastrów opartych na systemie Windows zobacz [Analiza serwisu Twitter danych przy użyciu gałąź w HDInsight](hdinsight-analyze-twitter-data.md).

###<a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego samouczka, musisz mieć następujące czynności:

- __Klaster systemem Linux HDInsight Azure__. Aby uzyskać informacje na temat tworzenia klastrze zobacz [Rozpoczynanie pracy z systemem Linux HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) instrukcje dotyczące tworzenia klastrze.

- __SSH klienta__. Aby uzyskać więcej informacji na temat korzystania z SSH z systemem Linux HDInsight zobacz następujące artykuły:

    * [Używanie SSH z systemem Linux Hadoop na HDInsight z Linux, Unix lub systemu OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Używanie SSH z systemem Linux Hadoop na HDInsight z systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

- __Python__ i [pip](https://pypi.python.org/pypi/pip)

##<a name="get-a-twitter-feed"></a>Uzyskiwanie Twitter, kanału informacyjnego

Twitter pozwala na pobieranie [danych dla każdego tweet](https://dev.twitter.com/docs/platform-objects/tweets) jako dokumentu notacji obiektu JavaScript (JSON) za pomocą interfejsu API usługi REST. [OAuth](http://oauth.net) jest wymagany do uwierzytelniania API. Musisz też utworzyć _Aplikację serwisu Twitter_ , która zawiera ustawienia umożliwiające dostęp do API.

###<a name="create-a-twitter-application"></a>Tworzenie aplikacji Twitter

1. Za pomocą przeglądarki sieci web Zaloguj się do [https://apps.twitter.com/](https://apps.twitter.com/). Kliknij łącze **Utwórz konto teraz** , jeśli nie masz konta Twitter.
2. Kliknij przycisk **Utwórz nową aplikację**.
3. Wprowadź **nazwę**, **Opis** **witryny sieci Web**. Można uzupełnić adresu URL w polu **witryny sieci Web** . W poniższej tabeli przedstawiono kilka przykładowych wartości do używania:

  	| Pole | Wartość |
  	|:----- |:----- |
  	| Nazwa  | MyHDInsightApp |
  	| Opis | MyHDInsightApp |
  	| Witryna sieci Web | http://www.myhdinsightapp.com |
    
4. Sprawdzanie **Tak, zgoda**, a następnie kliknij **Tworzenie aplikacji Twitter**.
5. Kliknij kartę **uprawnienia** . Domyślne uprawnienia jest **tylko do odczytu**. Jest to wystarczające dla tego samouczka.
6. Kliknij kartę **klawiszy i tokeny dostępu** .
7. Kliknij pozycję **Utwórz moje token dostępu**.
8. Kliknij przycisk **Testuj OAuth** w prawym górnym rogu strony.
9. Zanotuj **klucz**, **hasła dla klientów indywidualnych**, **token dostępu**i **tajny token dostępu**. Wartości jest potrzebna później.

>[AZURE.NOTE] Użycie polecenia zwinięcie w systemie Windows, użyj cudzysłowów zamiast apostrofy dla wartości opcji.

###<a name="download-tweets"></a>Pobierz tweety

Poniższy kod Python pobrać 10 000 tweety Twitter, a następnie zapisać je w pliku o nazwie __tweets.txt__.

> [AZURE.NOTE] W klastrze HDInsight wykonywane są następujące czynności, ponieważ Python jest już zainstalowany.

1. Nawiązywanie połączenia z klastrem HDInsight SSH:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
        
    Jeżeli używasz hasła do zabezpieczenia konta użytkownika SSH, wyświetli się monit o wprowadź je. Jeśli został użyty klucz publiczny, może być konieczne używanie `-i` parametru do określenia pasujące klucz prywatny. Na przykład `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.
        
    Aby uzyskać więcej informacji na temat korzystania z SSH z systemem Linux HDInsight zobacz następujące artykuły:
    
    * [Używanie SSH z systemem Linux Hadoop na HDInsight z Linux, Unix lub systemu OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Używanie SSH z systemem Linux Hadoop na HDInsight z systemu Windows](hdinsight-hadoop-linux-use-ssh-windows)
    
2. Domyślnie narzędzie __pip__ nie jest zainstalowany na węzła głównego HDInsight. Instalowanie należy wykonać następujące kroki, a następnie zaktualizuj to narzędzie:

        sudo apt-get install python-pip
        sudo pip install --upgrade pip

3. Kod, aby pobrać tweety oparte na [Tweepy](http://www.tweepy.org/) i [Progressbar](https://pypi.python.org/pypi/progressbar/2.2). Aby zainstalować te, wpisz następujące polecenie:

        sudo apt-get install python-dev libffi-dev libssl-dev
        sudo apt-get remove python-openssl
        sudo pip install tweepy progressbar pyOpenSSL requests[security]
        
    > [AZURE.NOTE] Bitów o usuwaniu openssl python, instalowania deweloperów python, deweloperów libffi, deweloperów libssl, pyOpenSSL i żądania [zabezpieczenia] jest uniknąć ostrzeżenia InsecurePlatform podczas nawiązywania połączenia z Python Twitter za pośrednictwem protokołu SSL.
    >
    > Tweepy v3.2.0 jest używany w celu uniknięcia [komunikat o błędzie](https://github.com/tweepy/tweepy/issues/576) , które mogą wystąpić podczas przetwarzania tweety.

4. Umożliwia utworzenie nowego pliku o nazwie __gettweets.py__następujące polecenie:

        nano gettweets.py

5. Użyj następujących jako zawartość pliku __gettweets.py__ . Zamień symbol zastępczy informacje dotyczące __dla klientów indywidualnych\_tajne__, __dla klientów indywidualnych\_klucza__, __access-\_token__, i __dostępu\_token\_tajne__ na podstawie informacji z aplikacji Twitter.

        #!/usr/bin/python

        from tweepy import Stream, OAuthHandler
        from tweepy.streaming import StreamListener
        from progressbar import ProgressBar, Percentage, Bar
        import json
        import sys

        #Twitter app information
        consumer_secret='Your consumer secret'
        consumer_key='Your consumer key'
        access_token='Your access token'
        access_token_secret='Your access token secret'

        #The number of tweets we want to get
        max_tweets=10000

        #Create the listener class that will receive and save tweets
        class listener(StreamListener):
            #On init, set the counter to zero and create a progress bar
            def __init__(self, api=None):
                self.num_tweets = 0
                self.pbar = ProgressBar(widgets=[Percentage(), Bar()], maxval=max_tweets).start()

            #When data is received, do this
            def on_data(self, data):
                #Append the tweet to the 'tweets.txt' file
                with open('tweets.txt', 'a') as tweet_file:
                    tweet_file.write(data)
                    #Increment the number of tweets
                    self.num_tweets += 1
                    #Check to see if we have hit max_tweets and exit if so
                    if self.num_tweets >= max_tweets:
                        self.pbar.finish()
                        sys.exit(0)
                    else:
                        #increment the progress bar
                        self.pbar.update(self.num_tweets)
                return True

            #Handle any errors that may occur
            def on_error(self, status):
                print status

        #Get the OAuth token
        auth = OAuthHandler(consumer_key, consumer_secret)
        auth.set_access_token(access_token, access_token_secret)
        #Use the listener class for stream processing
        twitterStream = Stream(auth, listener())
        #Filter for these topics
        twitterStream.filter(track=["azure","cloud","hdinsight"])

6. Zapisz plik za pomocą __klawiszy Ctrl + X__, a następnie __Y__ .

7. Użyj następującego polecenia Uruchom plik i pobierania tweety:

        python gettweets.py

    Wskaźnik postępu należy są wyświetlane i 100% są traktowane jako tweety są pobierane i do zapisania pliku.

    > [AZURE.NOTE] Jeśli trwa bardzo długo pasek postępu, aby przechodzić do należy zmienić filtr, aby śledzić popularnych tematy; w przypadku dużej ilości tweety filtrowanego na temat może bardzo szybko uzyskać tweety 10000, potrzebne.

###<a name="upload-the-data"></a>Przekazywanie danych

Aby przekazać dane do WASB (Rozproszony system plików używane przez HDInsight), należy użyć następujących poleceń:

    hdfs dfs -mkdir -p /tutorials/twitter/data
    hdfs dfs -put tweets.txt /tutorials/twitter/data/tweets.txt

Dane są przechowywane w lokalizacji dostępnej dla wszystkich węzłów w klastrze.

##<a name="run-the-hiveql-job"></a>Uruchamianie zadania HiveQL


1. Aby utworzyć plik zawierający instrukcje HiveQL, użyj następującego polecenia:

        nano twitter.hql
    
    Zawartość pliku, należy użyć następującej składni:

        set hive.exec.dynamic.partition = true;
        set hive.exec.dynamic.partition.mode = nonstrict;
        -- Drop table, if it exists
        DROP TABLE tweets_raw;
        -- Create it, pointing toward the tweets logged from Twitter
        CREATE EXTERNAL TABLE tweets_raw (
            json_response STRING
        )
        STORED AS TEXTFILE LOCATION '/tutorials/twitter/data';
        -- Drop and recreate the destination table
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
        -- Select tweets from the imported data, parse the JSON,
        -- and insert into the tweets table
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
        
        
3. Naciśnij __klawisze Ctrl + X__, a następnie naciśnij klawisz __T__ , aby zapisać plik.

4. Użyj następującego polecenia do uruchamiania HiveQL zawarte w pliku:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -i twitter.hql
        
    To będzie ładowanie powłoki gałęzi, uruchom HiveQL w pliku __twitter.hql__ , a następnie zwrócenie `jdbc:hive2//localhost:10001/>` wiersza.
    
5. W wierszu beeline Sprawdź, czy możesz zaznaczyć dane z tabeli __tweety__ utworzone przez HiveQL w pliku __twitter.hql__ należy wykonać następujące kroki:
        
        SELECT name, screen_name, count(1) as cc
            FROM tweets
            WHERE text like "%Azure%"
            GROUP BY name,screen_name
            ORDER BY cc DESC LIMIT 10;

    To może zwrócić maksymalnie 10 tweety, zawierających wyraz __Azure__ w treści wiadomości.

##<a name="next-steps"></a>Następne kroki

W tym samouczku możemy umieścić jak zestaw JSON niestrukturalne umożliwia przekształcenie strukturalnych tabelę programu Hive kwerendy, eksplorowanie i analizowanie danych z Twitter przy użyciu usługi HDInsight Azure. Aby uzyskać więcej informacji, zobacz:

- [Wprowadzenie do usługi HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
- [Analizowanie danych opóźnienia lotów przy użyciu usługi HDInsight](hdinsight-analyze-flight-delay-data-linux.md)

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter
