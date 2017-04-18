<properties
pageTitle="Ograniczanie dostępu HDInsight do danych za pomocą udostępnionego podpisów programu Access"
description="Dowiedz się, jak ograniczyć HDInsight dostęp do danych zapisanych w Azure magazyn obiektów blob za pomocą podpisów dostępu do udostępnionych."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="10/11/2016"
ms.author="larryfr"/>

#<a name="use-azure-storage-shared-access-signatures-to-restrict-access-to-data-with-hdinsight"></a>Używanie podpisów udostępnione dostępu w programie Azure miejsca do magazynowania ograniczenia dostępu do danych z usługi HDInsight

Usługa HDInsight używa Azure magazyn obiektów blob do przechowywania danych. Podczas HDInsight musi mieć pełny dostęp do obiektów blob, używany jako domyślnego miejsca do magazynowania dla klaster, mogą ograniczyć uprawnienia do danych zapisanych w innych obiektów blob używana przez klaster. Na przykład można wprowadzić kilka danych tylko do odczytu. Można to zrobić za pomocą udostępnionego podpisów programu Access.

Podpisy dostępu do udostępnionej (SA) są funkcji konta Azure miejsca do magazynowania, która umożliwia ograniczanie dostępu do danych. Na przykład dostarczanie dostęp do danych tylko do odczytu. W tym dokumencie dowiesz się, jak używać skojarzeń zabezpieczeń umożliwiający dostęp tylko do listy do odczytu i do kontenera obiektów blob z usługi HDInsight.

##<a name="requirements"></a>Wymagania dotyczące

* Subskrypcję usługi Azure

* C# lub Python. Przykład kodu C# ma postać rozwiązania programu Visual Studio.

    * Program Visual Studio musi być wersji 2013 lub 2015 r.
    
    * Python musi być wersji 2.7 lub nowszej

* Usługa HDInsight systemem Linux klaster lub [Azure programu PowerShell] [ powershell] — Jeśli masz istniejące klaster systemem Linux umożliwia Ambari Dodaj podpis dostępu do udostępnionego z klastrem. W przeciwnym razie za pomocą programu PowerShell Azure utworzyć nowy klaster i dodać podpis dostępu do udostępnionego podczas tworzenia klaster.

* Przykładowe pliki [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature). To repozytorium obejmuje następujące czynności:

    * Projekt programu Visual Studio, które można utworzyć kontenera magazynu, zapisane zasady i skojarzenia zabezpieczeń do użytku z usługi HDInsight
    
    * Skrypt Python, który można utworzyć kontenera magazynu, zapisane zasady i skojarzenia zabezpieczeń do użytku z usługi HDInsight
    
    * Skrypt programu PowerShell, który można utworzyć nowy klaster HDInsight i skonfigurować go do używania skojarzeń zabezpieczeń.

##<a name="shared-access-signatures"></a>Podpisy udostępniania

Istnieją dwa rodzaje udostępnione podpisów programu Access:

* Ad hoc: czas rozpoczęcia, czas wygaśnięcia i uprawnienia dla skojarzeń zabezpieczeń są wszystkie określonego identyfikatora URI skojarzeń zabezpieczeń (lub dorozumianych, w przypadku której zostanie pominięty czas rozpoczęcia).

* Przechowywane zasady dostępu: zasadę dostępu przechowywane jest definiowana w kontenerze zasobów kontenera obiektów blob, tabeli, kolejki lub udziale plików - i może służyć do zarządzania ograniczenia dla jednego lub kilku podpisów udostępniania. Skojarzenia zabezpieczeń skojarzyć zasadę dostępu przechowywane, skojarzeń zabezpieczeń dziedziczy ograniczeń - czas rozpoczęcia, czas wygaśnięcia i uprawnienia - zdefiniowane zasady dostępu przechowywane.

Różnica między dwoma formularzami jest ważna w przypadku jeden scenariusz klucza: odwołania. Jest adres URL, aby każda osoba, która pobiera skojarzeń zabezpieczeń, używać niezależnie od tego, kto będzie zaczynać się wymagane. Jeśli skojarzenia zabezpieczeń jest opublikowany publicznie, mogą być używane przez wszystkich użytkowników w świecie. Skojarzenia zabezpieczeń, który jest rozkładana jest prawidłowy, aż jedną z czterech co się dzieje:

1. Czas wygaśnięcia określony na skojarzeń zabezpieczeń osiągnięciu.

2. Czas wygaśnięcia określony dla zasady dostępu przechowywaną odwołuje się skojarzeń zabezpieczeń osiągnięciu (jeśli odwołuje się zasadę dostępu przechowywane i określa czas wygaśnięcia). Nastąpi, ponieważ czasu lub zmodyfikowano zasady dostępu przechowywane, aby mieć czas wygaśnięcia w przeszłości, która jest jednym ze sposobów odwoływanie skojarzeń zabezpieczeń.

3. Zasady dostępu do przechowywanej odwołuje się skojarzeń zabezpieczeń zostanie usunięty, sposób aby odwołać skojarzeń zabezpieczeń. Należy zauważyć, że odtworzyć zasady przechowywane dostępu przy użyciu takiej samej nazwie, wszystkie istniejące tokeny skojarzeń zabezpieczeń ponownie będzie prawidłowa uprawnienia związane z tej zasady przechowywane programu access (przy założeniu, że który czas wygaśnięcia na skojarzeń zabezpieczeń nie minął). Jeśli mają zamiar odwoływanie skojarzeń zabezpieczeń, należy użyć pod inną nazwą, jeśli odtworzenie zasady dostępu z upływem czasu w przyszłości.

4. Klucz konta, który został użyty do utworzenia skojarzeń zabezpieczeń jest generowane. Zauważ, że w ten sposób spowoduje, że wszystkie składniki aplikacji przy użyciu tego klawisza konta kończy się niepowodzeniem do uwierzytelnienia, dopóki nie zostaną one zaktualizowane do za pomocą klawisza działające konto lub klucz nowo regenerowanej konta.

> [AZURE.IMPORTANT] Podpis udostępnienia URI jest skojarzony z klucz konta użytego do utworzenia podpisu, a skojarzonego przechowywane zasady dostępu (jeśli istnieją). Jeśli określono nie zasady dostępu przechowywane, jedynym sposobem odwoływanie podpisu udostępniania jest zmiana klucz konta. 

Zalecane jest zawsze używaj zasady dostępu przechowywane, dzięki czemu możesz odwołać podpisów lub przedłużenia daty wygaśnięcia, stosownie do potrzeb. Kroki opisane w tym dokumentu, użyj przechowywane zasady dostępu do generowania skojarzeń zabezpieczeń.

Aby uzyskać więcej informacji na udostępnionych podpisów programu Access zobacz [Opis modelu skojarzeń zabezpieczeń](../storage/storage-dotnet-shared-access-signature-part-1.md).

##<a name="create-a-stored-policy-and-generate-a-sas"></a>Tworzenie zasad przechowywanych oraz generowanie skojarzenia zabezpieczeń

Obecnie zapisane zasady należy utworzyć programowy. Można znaleźć C# i przykład Python Tworzenie skojarzeń zabezpieczeń i zasad przechowywanych w [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature).

###<a name="create-a-stored-policy-and-sas-using-c"></a>Tworzenie zasad przechowywanych i skojarzenia zabezpieczeń za pomocą C\#

1. Otwórz rozwiązanie w programie Visual Studio.

2. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy nad projektem __SASToken__ i wybierz polecenie __Właściwości__.

3. Wybierz pozycję __Ustawienia__ , a następnie dodaj wartości dla następujących pozycji:

    * StorageConnectionString: Parametry połączenia dla konta miejsca do magazynowania, które chcesz utworzyć zapisane zasady i skojarzenia zabezpieczeń na. Format powinien być `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey` miejsce, w którym `myaccount` to nazwa Twojego konta miejsca do magazynowania i `mykey` jest klucz konta miejsca do magazynowania.
    
    * ContainerName: Kontener na koncie miejsca do magazynowania, który chcesz ograniczyć dostęp do.
    
    * SASPolicyName: Nazwa ma być używana zapisane zasady, która zostanie utworzona.
    
    * FileToUpload: Ścieżka do pliku, który będzie można przekazywać do kontenera.
    
4. Uruchom projekt. Zostanie wyświetlone okno konsoli, a po wygenerowane skojarzeń zabezpieczeń będą wyświetlane informacje o podobnej do następującej:

        Container SAS token using stored access policy: sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14
        
    Zapisz tokenu zasad skojarzeń zabezpieczeń, jak należy to podczas kojarzenia konta miejsca do magazynowania z klaster HDInsight. Konieczne będzie również nazwę konta magazynu, a nazwa kontenera.
    
###<a name="create-a-stored-policy-and-sas-using-python"></a>Tworzenie zasad przechowywanych i przy użyciu Python skojarzenia zabezpieczeń

1. Otwórz plik SASToken.py i zmienić następujące wartości:

    * zasady\_Nazwa: Nazwa zapisane zasady, która zostanie utworzona.
    
    * Magazyn\_konta\_Nazwa: nazwę swojego konta miejsca do magazynowania.
    
    * Magazyn\_konta\_klucz: klucz konta miejsca do magazynowania.
    
    * Magazyn\_kontenera\_Nazwa: kontenera na koncie miejsca do magazynowania, który chcesz ograniczyć dostęp do.
    
    * przykład\_pliku\_ścieżka: ścieżka do pliku, które zostaną przekazane do kontenera

2. Uruchom skrypt. Po ukończeniu działania skryptu będzie wyświetlany token skojarzeń zabezpieczeń podobny do następującego:

        sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14
    
    Zapisz tokenu zasad skojarzeń zabezpieczeń, jak należy to podczas kojarzenia konta miejsca do magazynowania z klaster HDInsight. Konieczne będzie również nazwę konta magazynu, a nazwa kontenera.
    
##<a name="use-the-sas-with-hdinsight"></a>Używanie skojarzeń zabezpieczeń z usługi HDInsight

Podczas tworzenia klaster HDInsight, musisz określić konto podstawowy i opcjonalnie można określić konta dodatkowego miejsca do magazynowania. Obie te metody: Dodawanie miejsca do magazynowania wymaga pełnego dostępu do konta miejsca do magazynowania i kontenerów, które są używane.

Aby podpisu dostępu udostępnione można użyć w celu ograniczenia dostępu do kontenera, należy dodać wpisu niestandardowego do konfiguracji __witryny core__ klaster.

* W przypadku __systemu Windows__ lub __systemem Linux__ HDInsight klastrów możesz to zrobić podczas tworzenia klaster przy użyciu programu PowerShell.

* Dla klastrów HDInsight __systemem Linux__ możesz zmienić konfigurację po utworzeniu klaster przy użyciu Ambari.

###<a name="create-a-new-cluster-that-uses-the-sas"></a>Utwórz nowy klaster, która używa skojarzeń zabezpieczeń

Przykład tworzenia klaster HDInsight, która używa skojarzeń zabezpieczeń znajduje się w `CreateCluster` katalogu repozytorium. Aby użyć go, wykonaj następujące czynności:

1. Otwórz `CreateCluster\HDInsightSAS.ps1` plik w edytorze tekstów i modyfikowanie następujące wartości na początku dokumentu.

        # Replace 'mycluster' with the name of the cluster to be created
        $clusterName = 'mycluster'
        # Valid values are 'Linux' and 'Windows'
        $osType = 'Linux'
        # Replace 'myresourcegroup' with the name of the group to be created
        $resourceGroupName = 'myresourcegroup'
        # Replace with the Azure data center you want to the cluster to live in
        $location = 'North Europe'
        # Replace with the name of the default storage account to be created
        $defaultStorageAccountName = 'mystorageaccount'
        # Replace with the name of the SAS container created earlier
        $SASContainerName = 'sascontainer'
        # Replace with the name of the SAS storage account created earlier
        $SASStorageAccountName = 'sasaccount'
        # Replace with the SAS token generated earlier
        $SASToken = 'sastoken'
        # Set the number of worker nodes in the cluster
        $clusterSizeInNodes = 2
        
    Na przykład zmienić `'mycluster'` do nazwy klaster, którego chcesz utworzyć. Wartości skojarzeń zabezpieczeń powinny być zgodne wartości z powyższych czynności, podczas tworzenia konta miejsca do magazynowania i tokenu skojarzeń zabezpieczeń.
    
    Po zmianie wartości, aby zapisać plik.

1. Otwórz nowy wiersz Azure programu PowerShell. Jeśli nie zna Azure programu PowerShell lub nie została zainstalowana, zobacz [Instalowanie i konfigurowanie programu PowerShell Azure][powershell].

2. W wierszu uwierzytelnienia do subskrypcji usługi Azure należy wykonać następujące kroki:

        Login-AzureRmAccount
    
    Gdy zostanie wyświetlony monit, zaloguj się przy użyciu konta dla subskrypcji Azure.
    
    Jeśli logowanie nie jest skojarzone z wiele subskrypcji Azure, może być konieczne używanie `Select-AzureRmSubscription` do Wybierz subskrypcję, o których chcesz użyć.

2. W wierszu Zmienianie katalogów `CreateCluster` katalogu, w którym znajduje się plik HDInsightSAS.ps1. Następnie należy wykonać następujące kroki uruchamianie skryptu
        
        .\HDInsightSAS.ps1
    
    Jak skrypt zostanie uruchomiony, go można rejestrować dane wyjściowe do wiersza polecenia programu PowerShell podczas tworzenia konta grupy i miejsca do magazynowania zasobu. Następnie wyświetli pytanie o wprowadzenie użytkownika HTTP dla klastrów HDInsight. To jest konto użytkownika służące do bezpiecznego dostępu HTTP/s z klastrem.
    
    Jeśli tworzysz klastrze systemem Linux zostanie również wyświetlony monit o podanie SSH nazwę konta użytkownika i hasła. Służy do zdalnego logowania się z klastrem.
    
    > [AZURE.IMPORTANT] Po wyświetleniu monitu dla protokołu HTTP/s lub SSH nazwę użytkownika i hasło, należy podać hasło, która spełnia następujące warunki:
    >
    > - Musi zawierać co najmniej 10 znaków
    > - Musi zawierać co najmniej jedną cyfrę
    > - Musi zawierać co najmniej jeden znak niealfanumeryczne
    > - Musi zawierać co najmniej jedną górnym lub małe litery


Zajmie trochę czasu, zanim skrypt do wykonania, zwykle około 15 minut. Po zakończeniu skrypt bez błędów klaster został utworzony.

###<a name="update-an-existing-cluster-to-use-the-sas"></a>Aktualizowanie istniejącego klaster używać skojarzeń zabezpieczeń

Jeśli masz istniejącym klastrem systemem Linux, możesz dodać skojarzenia zabezpieczeń konfiguracji __witryny core__ , wykonując następujące czynności:

1. Otwórz web Ambari interfejsu użytkownika dla klaster. Adres dla tej strony jest https://YOURCLUSTERNAME.azurehdinsight.net. Po wyświetleniu monitu uwierzytelnienia z klastrem przy użyciu nazwy administratora (Administrator) i hasła użytych podczas tworzenia klaster.

2. Z lewej strony sieci web Ambari interfejsu użytkownika wybierz pozycję __HDFS__ , a następnie wybierz kartę __podawać__ na środku strony.

3. Wybierz kartę __Zaawansowane__ , a następnie przewiń do pozycji sekcji __witryny core niestandardowe__ .

4. Rozwiń sekcję __witryn core niestandardowe__ , a następnie przewiń do końca i kliknij łącze __Dodaj właściwość__ . Zastosuj następujące wartości dla pól __kluczy__ i __wartości__ :

    * __Klucz__: fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net
    
    * __Wartość__: skojarzeń zabezpieczeń zwracane przez aplikację C# lub Python wcześniej uruchomiono
    
    Zamień __CONTAINERNAME__ nazwa kontenera, używanego z C# lub skojarzenia zabezpieczeń aplikacji. Zamień nazwę konta magazynu używanego __STORAGEACCOUNTNAME__ .

5. Kliknij przycisk __Dodaj__ , aby zapisać ten klucz i wartość, a następnie kliknij przycisk __Zapisz__ , aby zapisać zmiany konfiguracji. Po wyświetleniu monitu, dodać opis zmian ("Dodawanie dostępu do magazynu skojarzeń zabezpieczeń" na przykład), a następnie kliknij przycisk __Zapisz__.

    Po zakończeniu zmiany, kliknij __przycisk OK__ .

    > [AZURE.IMPORTANT] Spowoduje to zapisanie zmian w konfiguracji, ale aby zmiany zostały uwzględnione należy ponownie uruchomić kilka usług.

6. W sieci web Ambari interfejsu użytkownika z listy po lewej stronie wybierz pozycję __HDFS__ , a następnie wybierz __Ponownie uruchom wszystko__ z listy rozwijanej __Akcje z__ listy po prawej stronie. Po wyświetleniu monitu wybierz pozycję __Włącz tryb konserwacji__ a następnie uruchom ponownie wszystkie __Conform wybierz pozycję".

    Powtórz tę procedurę dla wpisów MapReduce2 i PRZĘDZY na liście po lewej stronie.

7. Po tych ponownego uruchomienia, zaznacz każdy z nich i Wyłącz tryb konserwacji __Usługi akcje__ listy rozwijanej.

##<a name="test-restricted-access"></a>Testowanie ograniczenia dostępu

Aby sprawdzić, czy mają ograniczony dostęp, należy korzystać z następujących metod:

* W przypadku klastrów HDInsight __systemu Windows__ Połącz się z klastrem za pomocą pulpitu zdalnego. Aby uzyskać więcej informacji, zobacz [może połączyć się z usługą hdinsight za pomocą RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp) .

    Po połączeniu umożliwia ikonę __Hadoop wiersza polecenia__ na komputerze otwórz wiersz polecenia.

* Dla klastrów HDInsight __systemem Linux__ nawiązywanie połączenia z klastrem za pomocą SSH. Zobacz jedną z następujących czynności, aby uzyskać informacje na temat korzystania z systemem Linux klastrów SSH:

    * [Używanie SSH z systemem Linux Hadoop na HDInsight z Linux, OS X i Unix](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [Używanie SSH z systemem Linux Hadoop na HDInsight z systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
    
Po nawiązaniu połączenia z klastrem, wykonaj następujące czynności, aby sprawdzić możliwe tylko odczytu i lista elementów na koncie miejsca do magazynowania skojarzenia zabezpieczeń:

1. W wierszu polecenia wpisz następujące polecenie. Zamień __SASCONTAINER__ nazwę kontenera utworzone konta miejsca do magazynowania skojarzeń zabezpieczeń. Zastąp __SASACCOUNTNAME__ nazwę konta magazynu używane dla skojarzeń zabezpieczeń:

        hdfs dfs -ls wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/
    
    To zostanie wyświetlona zawartość kontenera, w którym powinien zawierać plik, który został przekazany utworzenia skojarzeń zabezpieczeń i kontener.
    
2. Sprawdź, czy można odczytać zawartość pliku należy wykonać następujące kroki. Zastąp __SASCONTAINER__ i __SASACCOUNTNAME__ , tak jak w poprzednim kroku. Zastąp __FILENAME__ nazwę pliku wyświetlane w poprzedniego polecenia:

        hdfs dfs -text wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME
        
    Spowoduje to wyświetlenie listy zawartość pliku.
    
3. Pobierz plik do lokalnego systemu plików należy wykonać następujące kroki:

        hdfs dfs -get wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME testfile.txt
    
    Spowoduje to pobranie pliku do lokalnego pliku o nazwie __plik_testowy.txt__.

4. Przekaż plik lokalny do nowego pliku o nazwie __testupload.txt__ w magazynie skojarzeń zabezpieczeń należy wykonać następujące kroki:

        hdfs dfs -put testfile.txt wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/testupload.txt
    
    Zostanie wyświetlony komunikat podobny do następującego:
    
        put: java.io.IOException
        
    Ten błąd występuje, ponieważ lokalizacji przechowywania jest odczytu + listy tylko. Dane są rozmieszczone w magazynie domyślne dla klaster, który jest zapisywalny należy wykonać następujące kroki:
    
        hdfs dfs -put testfile.txt wasbs:///testupload.txt
        
    Tym razem operacja powinien zakończyć się pomyślnie.
    
##<a name="troubleshooting"></a>Rozwiązywanie problemów

###<a name="a-task-was-canceled"></a>Zadanie zostało anulowane.

__Objawów__: podczas tworzenia klaster za pomocą skryptu programu PowerShell, może zostać wyświetlony następujący komunikat o błędzie:

    New-AzureRmHDInsightCluster : A task was canceled.
    At C:\Users\larryfr\Documents\GitHub\hdinsight-azure-storage-sas\CreateCluster\HDInsightSAS.ps1:62 char:5
    +     New-AzureRmHDInsightCluster `
    +     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (:) [New-AzureRmHDInsightCluster], CloudException
        + FullyQualifiedErrorId : Hyak.Common.CloudException,Microsoft.Azure.Commands.HDInsight.NewAzureHDInsightClusterCommand

__Przyczyna__: ten błąd może wystąpić, jeśli dla użytkownika admin/HTTP klaster lub (w przypadku systemem Linux klastrów) za pomocą hasła użytkownika SSH.

__Rozdzielczość__: Użyj hasła, która spełnia następujące warunki:

- Musi zawierać co najmniej 10 znaków
- Musi zawierać co najmniej jedną cyfrę
- Musi zawierać co najmniej jeden znak niealfanumeryczne
- Musi zawierać co najmniej jedną górnym lub małe litery

##<a name="next-steps"></a>Następne kroki

Teraz, gdy znasz jak dodać magazyn ograniczony dostęp do klaster HDInsight, Dowiedz się inne sposoby pracy z danymi w klastrze:

* [Gałąź za pomocą usługi HDInsight](hdinsight-use-hive.md)

* [Świnka korzystanie z usługi HDInsight](hdinsight-use-pig.md)

* [Używanie MapReduce z usługi HDInsight](hdinsight-use-mapreduce.md)

[powershell]: ../powershell-install-configure.md
