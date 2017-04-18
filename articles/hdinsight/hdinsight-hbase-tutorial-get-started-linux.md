<properties
    pageTitle="Samouczek HBase: rozpoczynanie pracy z systemem Linux HBase klastrów w Hadoop | Microsoft Azure"
    description="Skorzystać z tego samouczka HBase, aby rozpocząć korzystanie z Hadoop w HDInsight Apache HBase. Tworzenie tabel przy użyciu powłoki HBase i ich kwerendy przy użyciu gałęzi."
    keywords="Apache hbase, hbase, hbase powłoki, samouczek hbase"
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
    ms.topic="get-started-article"
    ms.date="10/19/2016"
    ms.author="jgao"/>



# <a name="hbase-tutorial-get-started-using-apache-hbase-with-linux-based-hadoop-in-hdinsight"></a>HBase samouczek: wprowadzenie do korzystania z systemem Linux Hadoop w HDInsight Apache HBase 

[AZURE.INCLUDE [hbase-selector](../../includes/hdinsight-hbase-selector.md)]

Dowiedz się, jak utworzyć klaster HBase w HDInsight, tworzyć tabele HBase i tabele kwerendy przy użyciu gałęzi. Aby uzyskać ogólne informacje HBase zobacz [Omówienie HDInsight HBase][hdinsight-hbase-overview].

Informacje w tym dokumencie są specyficzne dla klastrów HDInsight systemem Linux. Aby uzyskać informacje dotyczące klastrów opartych na systemie Windows umożliwia przełączenie selektor tabulatorów w górnej części strony.

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

##<a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego samouczka HBase musi mieć następujące czynności:

- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- [Zabezpieczanie Shell(SSH)](hdinsight-hadoop-linux-use-ssh-unix.md). 
- [zawinięcie](http://curl.haxx.se/download.html).

### <a name="access-control-requirements"></a>Wymagania dotyczące kontroli dostępu

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-hbase-cluster"></a>Utwórz klaster HBase

W poniższej procedurze użyto szablonu Menedżera zasobów Azure utworzyć klaster w wersji 3.4 systemem Linux HBase i zależne domyślnego konta magazynu platformy Azure. Aby zrozumieć parametry używane w procedurze i innych metod tworzenia klaster, zobacz [klastrów opartych na tworzenie Linux Hadoop w HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

1. Kliknij, aby otworzyć szablon w portalu Azure następujące obraz. Szablon znajduje się w kontenerze publicznej obiektów blob. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-cluster-in-hdinsight.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Z karta **wdrożenia niestandardowe** wprowadź następujące informacje:

    - **Subskrypcja**: Wybierz subskrypcję Azure, którego będzie można używać do tworzenia klaster.
    - **Grupa zasobów**: Tworzenie nowej grupy zarządzania zasobami Azure lub użyj istniejącego wzornika.
    - **Lokalizacja**: Określ lokalizację grupy zasobów. 
    - **NazwaKlastra**: Wprowadź nazwę klaster HBase, który ma zostać utworzony.
    - **Klaster nazwę logowania i hasło**: domyślna nazwa logowania to **administratora**.
    - **SSH nazwy użytkownika i hasła**: domyślna nazwa użytkownika to **sshuser**.  Można zmienić jej nazwę.
     
    Inne parametry są opcjonalne.  
    
    Każdy klaster ma zależność konta magazyn obiektów Blob platformy Azure. Po usunięciu klastrze dane są zachowywane w oknie konta miejsca do magazynowania. Nazwę konta magazynu domyślnego klaster jest nazwą klaster z dodanym "store". Jest ustalony w sekcji Szablon zmiennych.
        
3. Wybierz pozycję **zgodę na warunki umowy podanych powyżej**, a następnie kliknij **zakupu**. Aby utworzyć klaster trwa około 20 minut.


>[AZURE.NOTE] Po usunięciu klaster HBase możesz utworzyć inny klaster HBase przy użyciu samej domyślnego kontenera obiektów blob. Nowy klaster użyje tabele HBase utworzonego w grupie oryginalnej. Aby uniknąć niespójności, zaleca się wyłączenie tabel HBase, przed usunięciem klaster.

## <a name="create-tables-and-insert-data"></a>Tworzenie tabel i wstawianie danych

SSH umożliwia nawiązywanie połączenia z klastrów HBase, a następnie użyj powłoki HBase tworzyć tabele HBase, wstawianie danych i dane kwerendy. Aby uzyskać informacje na temat korzystania z SSH zobacz [Używanie SSH z systemem Linux Hadoop na HDInsight z Linux, Unix lub OS X](hdinsight-hadoop-linux-use-ssh-unix.md) i [Użyj SSH z systemem Linux Hadoop na HDInsight z systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md).
 

Większość osób dane są wyświetlane w formacie tabelarycznym:

![Dane tabelaryczne HDInsight HBase][img-hbase-sample-data-tabular]

W HBase, który jest implementacją BigTable tych samych danych wygląda:

![Usługa HDInsight HBase bigtable danych][img-hbase-sample-data-bigtable]

Będzie bardziej zrozumiałe po zakończeniu następną procedurę.  


**Aby użyć powłoki HBase**

1. Z SSH uruchom następujące polecenie:

        hbase shell

4. Tworzenie HBase z rodziny dwie kolumny:

        create 'Contacts', 'Personal', 'Office'
        list
5. Wstawianie danych:

        put 'Contacts', '1000', 'Personal:Name', 'John Dole'
        put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
        put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
        put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
        scan 'Contacts'

    ![Usługa hdinsight hadoop hbase powłoki][img-hbase-shell]

6. Uzyskiwanie pojedynczy wiersz

        get 'Contacts', '1000'

    Ten sam efekt zostanie wyświetlona jako za pomocą polecenia skanowania, ponieważ jest tylko jeden wiersz.

    Aby uzyskać więcej informacji na temat schematu tabeli HBase zobacz [Wprowadzenie do projektowania schematu HBase][hbase-schema]. Uzyskać więcej poleceń HBase, zobacz [Przewodnik po Apache HBase][hbase-quick-start].

6. Zakończ powłoki

        exit



**Aby zbiorczo ładowania danych do tabeli HBase kontaktów**

HBase zawiera kilka metod ładowanie danych do tabel.  Aby uzyskać więcej informacji zobacz [ładowanie zbiorcze](http://hbase.apache.org/book.html#arch.bulk.load).


Przykładowy plik danych przekazaniu do kontenera publicznej obiektów blob *wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt*.  Zawartość pliku danych jest:

    8396    Calvin Raji     230-555-0191    230-555-0191    5415 San Gabriel Dr.
    16600   Karen Wu        646-555-0113    230-555-0192    9265 La Paz
    4324    Karl Xie        508-555-0163    230-555-0193    4912 La Vuelta
    16891   Jonn Jackson    674-555-0110    230-555-0194    40 Ellis St.
    3273    Miguel Miller   397-555-0155    230-555-0195    6696 Anchor Drive
    3588    Osa Agbonile    592-555-0152    230-555-0196    1873 Lion Circle
    10272   Julia Lee       870-555-0110    230-555-0197    3148 Rose Street
    4868    Jose Hayes      599-555-0171    230-555-0198    793 Crawford Street
    4761    Caleb Alexander 670-555-0141    230-555-0199    4775 Kentucky Dr.
    16443   Terry Chander   998-555-0171    230-555-0200    771 Northridge Drive

Można utworzyć plik tekstowy i przekaż plik do konta miejsca do magazynowania, jeśli chcesz. Aby uzyskać instrukcje, zobacz [przekazywanie danych dla zadań Hadoop w HDInsight][hdinsight-upload-data].

> [AZURE.NOTE] Ta procedura używa tabeli HBase kontaktów utworzonego w ostatniej procedury.

1. Z SSH, uruchom następujące polecenie, aby przekształcić pliku danych do StoreFiles i sklepu na ścieżkę względną określony przez Dimporttsv.bulk.output:.  Jeśli jesteś w powłoce HBase, aby zakończyć za pomocą polecenie Zakończ.

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name,Personal:Phone,Office:Phone,Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

4. Uruchom następujące polecenie, aby przekazać dane z /example/data/storeDataFileOutput do tabeli HBase:

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts

5. Można otworzyć powłoki HBase i listy zawartość tabeli przy użyciu polecenia skanowania.



## <a name="use-hive-to-query-hbase"></a>Gałąź za pomocą kwerendy HBase

Dane w tabelach HBase mogą kwerendy przy użyciu gałęzi. W tej sekcji tworzy tabelę programu Hive mapy do tabeli HBase i używa jej do kwerendy danych w tabeli HBase.

1. Otwórz **Kit**i połącz się z klastrem.  Zapoznaj się z instrukcjami w poprzedniej procedurze.
2. Otwórz powłokę gałęzi.

       hive
3. Uruchom następujący skrypt HiveQL, aby utworzyć tabelę gałęzi mapy do tabeli HBase. Upewnij się, że utworzono przykładowej tabeli, do których odwołuje się wcześniej w tym samouczku przy użyciu powłoki HBase przed uruchomieniem zasad zachowania.

        CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
        STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
        WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
        TBLPROPERTIES ('hbase.table.name' = 'Contacts');

2. Uruchom następujący skrypt HiveQL w celu pobrania danych w tabeli HBase:

        SELECT count(*) FROM hbasecontacts;

## <a name="use-hbase-rest-apis-using-curl"></a>Używanie interfejsów API pozostałych HBase przy użyciu zwinięcie

> [AZURE.NOTE] Podczas korzystania z WebHCat zwinięcie i wszelkie inne metody komunikacji pozostałych, musi uwierzytelnić żądania, podając nazwy użytkownika i hasła administratora klastrów HDInsight. Należy również użyć nazwy klaster w ramach programu identyfikator URI (Uniform Resource) służy do wysyłania żądania na serwerze.
>
> Polecenia w tej sekcji Zamień **nazwę użytkownika** z użytkownikiem w celu uwierzytelnienia z klastrem i zastąpić **hasło** hasło do konta użytkownika. Zamień **NAZWAKLASTRA** nazwę klaster.
>
> Interfejsu API usługi REST jest zabezpieczone za pomocą [uwierzytelniania podstawowego](http://en.wikipedia.org/wiki/Basic_access_authentication). Zawsze należy żądania za pomocą protokołu HTTP bezpiecznego (HTTPS) upewnij się, że poświadczenia są bezpieczne wysyłane na serwerze.

1. W wierszu polecenia wpisz następujące polecenie aby sprawdzić, czy możesz połączyć się ze klaster HDInsight:

        curl -u <UserName>:<Password> \
        -G https://<ClusterName>.azurehdinsight.net/templeton/v1/status

    Powinien zostać wyświetlony odpowiedzi podobny do następującego:

        {"status":"ok","version":"v1"}

    Parametry używane w tym poleceniu są następujące:

    * **-u** - nazwę użytkownika i hasło używane do uwierzytelnienia wezwanie.
    * **-G** - wskazuje, że jest żądania GET.

2. Użyj następującego polecenia, aby wyświetlić listę istniejących tabel HBase:

        curl -u <UserName>:<Password> \
        -G https://<ClusterName>.azurehdinsight.net/hbaserest/

3. Użyj następującego polecenia, aby utworzyć nową tabelę HBase z dwóch rodzin kolumny:

        curl -u <UserName>:<Password> \
        -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/schema" \
        -H "Accept: application/json" \
        -H "Content-Type: application/json" \
        -d "{\"@name\":\"Contact1\",\"ColumnSchema\":[{\"name\":\"Personal\"},{\"name\":\"Office\"}]}" \
        -v

    Schemat jest dostępna w formacie JSon.

4. Wstawianie danych przy użyciu następującego polecenia:

        curl -u <UserName>:<Password> \
        -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/false-row-key" \
        -H "Accept: application/json" \
        -H "Content-Type: application/json" \
        -d "{\"Row\":{\"key\":\"MTAwMA==\",\"Cell\":{\"column\":\"UGVyc29uYWw6TmFtZQ==\", \"$\":\"Sm9obiBEb2xl\"}}}" \
        -v

    Należy base64 kodowanie wartości określone w pracy z wersją -d.  W exmaple:

    - MTAwMA ==: 1000.
    - UGVyc29uYWw6TmFtZQ ==: Personal: Nazwa
    - Sm9obiBEb2xl: Dole Jan

    [FAŁSZ wiersza klucza](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single) umożliwia wstawianie wielu wartości (wsadowej).

5. Aby uzyskać wiersza, użyj następującego polecenia:

        curl -u <UserName>:<Password> \
        -X GET "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/1000" \
        -H "Accept: application/json" \
        -v

Aby uzyskać więcej informacji o pozostałej HBase zobacz [Przewodnik po HBase Apache](https://hbase.apache.org/book.html#_rest).

## <a name="check-cluster-status"></a>Sprawdzanie stanu klaster

HBase w HDInsight jest dostarczany z interfejs sieci Web dla monitorowania klastrów. Za pomocą Interfejsu sieci Web, możesz przejąć statystyki lub informacje dotyczące regionów.

Można także SSH do tunelowania żądań lokalnych, takich jak żądania sieci web do klastrów HDInsight. Żądanie następnie będą kierowane do żądanego zasobu, tak jakby były rozpoczęte na głowy węzła HDInsight. Aby uzyskać więcej informacji zobacz [Używanie SSH z systemem Linux Hadoop na HDInsight z systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md#tunnel).

**Aby ustalić SSH tunneling sesji**

1. Otwórz **Kit**.  
2. Jeśli po utworzeniu konta użytkownika w trakcie procesu tworzenia udostępniane klawisza SSH, należy wykonać następujące czynności, aby zaznaczyć klucz prywatny używany podczas uwierzytelniania w grupie:

    W **kategorii**rozwiń **połączenie**, rozwiń **SSH**, a następnie wybierz pozycję **Auth**. Na koniec kliknij przycisk **Przeglądaj** i wybierz plik .ppk, który zawiera klucz prywatny.

3. W **kategorii**kliknij pozycję **sesji**.
4. Podstawowe opcje na ekranie Kit sesji wprowadź następujące wartości:

    - **Nazwa hosta**: adres SSH HDInsight pola server w polu Nazwa hosta (lub adres IP). Adres SSH jest nazwą klaster, następnie **-ssh.azurehdinsight.net**. Na przykład *mycluster ssh.azurehdinsight.net*.
    - **Portu**: 22. Ssh portu podstawowego headnode jest 22.  
5. W sekcji **kategorii** po lewej stronie okna dialogowego rozwiń **połączenie**, rozwiń **SSH**, a następnie kliknij **tuneli**.
6. Wprowadź następujące informacje na temat opcji sterowanie SSH portu Przekazywanie formularza:

    - **Port źródłowy** — port klienta, który chcesz przesłać dalej. Na przykład 9876.
    - **Dynamiczne** — umożliwia dynamiczne serwera proxy SOCKS routing.
7. Kliknij przycisk **Dodaj** , aby dodać ustawienia.
8. Kliknij przycisk **Otwórz** w dolnej części okna dialogowego otworzyć połączenia SSH.
9. Gdy zostanie wyświetlony monit, zaloguj się na serwerze przy użyciu konta SSH. Spowoduje to ustanowić sesję SSH i Włącz tunelem.

**Aby znaleźć nazwę FQDN opiekunowie przy użyciu Ambari**

1. Przejdź do https://<ClusterName>.azurehdinsight.net/.
2. Wprowadź poświadczenia konta użytkownika klaster dwa razy.
3. Z menu po lewej stronie kliknij pozycję **zookeeper**.
4. Kliknij jeden z trzech łącza **ZooKeeper Server** na liście podsumowanie.
5. Kopiowanie **Nazwa hosta**. Na przykład zk0-CLUSTERNAME.xxxxxxxxxxxxxxxxxxxx.cx.internal.cloudapp.net.

**Aby skonfigurować program Klient (Firefox) i sprawdź stan klaster**

1. Otwórz program Firefox.
2. Kliknij przycisk **Otwórz** .
3. Kliknij pozycję **Opcje**.
4. Kliknij pozycję **Zaawansowane**, kliknij pozycję **sieć**, a następnie kliknij przycisk **Ustawienia**.
5. Wybierz pozycję **Konfiguracja ręczna serwera proxy**.
6. Wprowadź następujące wartości:

    - **Socks hosta**: host lokalny
    - **Port**: używać tego samego portu skonfigurowane w SSH Kit tunelowania.  Na przykład 9876.
    - **SOCKS v5**: (zaznaczone)
    - **Zdalny DNS**: (zaznaczone)
7. Kliknij **przycisk OK** , aby zapisać zmiany.
8. Przejdź do http://&lt;FQDN ZooKeeper >: 60010-wzorzec status.

W klastrze szybkiej znajdziesz łącza do bieżącej HBase wzorca węzła aktywnego obsługujący interfejs sieci Web.

##<a name="delete-the-cluster"></a>Usuwanie klaster

Aby uniknąć niespójności, zaleca się wyłączenie tabel HBase, przed usunięciem klaster.

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Następne kroki

W tym samouczku HBase HDInsight wiesz, jak utworzyć klaster HBase i tworzenie tabel i przeglądania danych w tych tabelach z powłoki HBase. Wiesz również, jak używać gałęzi kwerendy na danych w tabelach HBase i jak używać HBase C# pozostałych API do tworzenia tabeli HBase i pobierania danych z tabeli.

Aby uzyskać więcej informacji, zobacz:

- [Omówienie HDInsight HBase][hdinsight-hbase-overview]: HBase jest Apache, Otwórz źródło NoSQL baza danych oparty na Hadoop zawierającego dostępie i spójność znaczący dla dużych ilości danych niestrukturalne i semistrukturalne.


[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hbase-reference]: http://hbase.apache.org/book.html#importtsv
[hbase-schema]: http://0b4af6cdc2f0c5998459-c0245c5c937c5dedcca3f1764ecc9b2f.r43.cf2.rackcdn.com/9353-login1210_khurana.pdf
[hbase-quick-start]: http://hbase.apache.org/book.html#quickstart





[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-versions]: hdinsight-component-versioning.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-bigtable.png
