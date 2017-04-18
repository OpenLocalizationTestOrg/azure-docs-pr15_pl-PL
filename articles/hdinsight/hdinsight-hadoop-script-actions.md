<properties
    pageTitle="Skrypt opracowywania akcji z usługi HDInsight | Microsoft Azure"
    description="Dowiedz się, jak dostosować klastrów Hadoop z akcją skrypt. Akcja skrypt może służyć do instalowania dodatkowego oprogramowania uruchamiania w klastrze Hadoop lub do zmiany konfiguracji aplikacji zainstalowanych w klastrze."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
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

# <a name="develop-script-action-scripts-for-hdinsight-windows-based-clusters"></a>Projektowania skryptów skrypt akcji dla klastrów HDInsight systemu Windows

Dowiedz się, jak napisać skrypty skrypt akcji dla HDInsight. Aby uzyskać informacje na temat korzystania z akcji skryptu skryptów zobacz [Dostosowywanie HDInsight klastrów przy użyciu akcji skryptu](hdinsight-hadoop-customize-cluster.md). W tym samym artykule napisane dla klastrów systemem Linux HDInsight zobacz [Skrypty można opracowywać Akcja skrypt do HDInsight](hdinsight-hadoop-script-actions-linux.md).

> [AZURE.NOTE] Informacje w tym dokumencie są specyficzne dla klastrów HDInsight systemu Windows. Uzyskać informacji na temat korzystania z akcji skryptu z systemem Windows klastrów zobacz [opracowywanie akcji skryptów z usługi HDInsight (Linux)](hdinsight-hadoop-script-actions-linux.md).


Akcja skrypt może służyć do instalowania dodatkowego oprogramowania uruchamiania w klastrze Hadoop lub do zmiany konfiguracji aplikacji zainstalowanych w klastrze. Akcje skryptu są skrypty uruchamiane w węzłach podczas rozmieszczania klastrów HDInsight i są wykonywane po węzły w klastrze zakończyć konfigurację HDInsight. Akcja skrypt jest wykonywane w obszarze uprawnienia konta administratora systemu, a także pełne prawa dostępu do przez klaster. Każdy klaster może być udostępniana wykaz akcji skrypt do wykonania w kolejności, w jakiej są określane.

> [AZURE.NOTE] Jeśli występuje następujący komunikat o błędzie:
>
>     System.Management.Automation.CommandNotFoundException; ExceptionMessage : The term 'Save-HDIFile' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.
> To, że nie włączono metody Pomocnik.  Zobacz [metody Pomocnik dla niestandardowych skryptów](hdinsight-hadoop-script-actions.md#helper-methods-for-custom-scripts).

## <a name="sample-scripts"></a>Przykładowe skrypty

Do tworzenia HDInsight klastrów w systemie operacyjnym Windows, akcja skrypt jest skrypt programu PowerShell Azure. Oto przykładowy skrypt do skonfigurowania plików konfiguracji witryny:

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    param (
        [parameter(Mandatory)][string] $ConfigFileName,
        [parameter(Mandatory)][string] $Name,
        [parameter(Mandatory)][string] $Value,
        [parameter()][string] $Description
    )

    if (!$Description) {
        $Description = ""
    }

    $hdiConfigFiles = @{
        "hive-site.xml" = "$env:HIVE_HOME\conf\hive-site.xml";
        "core-site.xml" = "$env:HADOOP_HOME\etc\hadoop\core-site.xml";
        "hdfs-site.xml" = "$env:HADOOP_HOME\etc\hadoop\hdfs-site.xml";
        "mapred-site.xml" = "$env:HADOOP_HOME\etc\hadoop\mapred-site.xml";
        "yarn-site.xml" = "$env:HADOOP_HOME\etc\hadoop\yarn-site.xml"
    }

    if (!($hdiConfigFiles[$ConfigFileName])) {
        Write-HDILog "Unable to configure $ConfigFileName because it is not part of the HDI configuration files."
        return
    }

    [xml]$configFile = Get-Content $hdiConfigFiles[$ConfigFileName]

    $existingproperty = $configFile.configuration.property | where {$_.Name -eq $Name}

    if ($existingproperty) {
        $existingproperty.Value = $Value
        $existingproperty.Description = $Description
    } else {
        $newproperty = @($configFile.configuration.property)[0].Clone()
        $newproperty.Name = $Name
        $newproperty.Value = $Value
        $newproperty.Description = $Description
        $configFile.configuration.AppendChild($newproperty)
    }

    $configFile.Save($hdiConfigFiles[$ConfigFileName])

    Write-HDILog "$configFileName has been configured."

Skrypt trwa cztery parametry, nazwy pliku konfiguracji, właściwość do zmodyfikowania wartości, które chcesz skonfigurować i opis. Na przykład:

    hive-site.xml hive.metastore.client.socket.timeout 90

Tych parametrów ustawia wartość hive.metastore.client.socket.timeout 90 w pliku site.xml gałęzi.  Wartością domyślną jest 60 sekund.

Przykładowy skrypt znajduje się w [https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1](https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1).

Usługa HDInsight zawiera kilka skryptów zainstalować dodatkowe składniki na klastrów HDInsight:

Nazwa | Skrypt
----- | -----
**Instalowanie Spark** | https://hdiconfigactions.blob.Core.Windows.NET/sparkconfigactionv03/Spark-Installer-v03.ps1. Zobacz [Instalowanie i używanie wzmóc dotyczących klastrów HDInsight][hdinsight-install-spark].
**Instalowanie R** | https://hdiconfigactions.blob.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1. Zobacz [Instalowanie i używanie R na klastrów HDInsight][hdinsight-r-scripts].
**Instalowanie Solr** | https://hdiconfigactions.blob.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1. Zobacz [Instalowanie i używanie klastrów Solr na HDInsight](hdinsight-hadoop-solr-install.md).
- **Instalowanie Giraph** | https://hdiconfigactions.blob.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1. Zobacz [Instalowanie i używanie klastrów Giraph na HDInsight](hdinsight-hadoop-giraph-install.md).

Akcja skrypt można wdrożyć za pomocą HDInsight .NET SDK lub portal Azure usługi Azure programu PowerShell.  Aby uzyskać więcej informacji, zobacz [Dostosowywanie HDInsight klastrów przy użyciu akcji skryptu][hdinsight-cluster-customize].

> [AZURE.NOTE] Przykładowe skrypty działa tylko w przypadku usługi HDInsight klaster wersji 3.1 lub powyżej. Aby uzyskać więcej informacji w wersjach klaster HDInsight zobacz [HDInsight klaster wersji](hdinsight-component-versioning.md).





## <a name="helper-methods-for-custom-scripts"></a>Pomocnik metod niestandardowych skryptów

Skrypt akcji Pomocnik metody są narzędzia, które można używać podczas pisania własnych skryptów. Te są definiowane w [https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1](https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1)i mogą być zawarte w skryptów za pomocą następujących:

    # Download config action module from a well-known directory.
    $CONFIGACTIONURI = "https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1";
    $CONFIGACTIONMODULE = "C:\apps\dist\HDInsightUtilities.psm1";
    $webclient = New-Object System.Net.WebClient;
    $webclient.DownloadFile($CONFIGACTIONURI, $CONFIGACTIONMODULE);

    # (TIP) Import config action helper method module to make writing config action easy.
    if (Test-Path ($CONFIGACTIONMODULE))
    {
        Import-Module $CONFIGACTIONMODULE;
    }
    else
    {
        Write-Output "Failed to load HDInsightUtilities module, exiting ...";
        exit;
    }

Poniżej przedstawiono metody Pomocnik, które zostały udostępnione przez tego skryptu:

Pomocnik metody | Opis
-------------- | -----------
**Zapisz HDIFile** | Jak pobrać plik z określoną identyfikator URI (Uniform Resource) do lokalizacji na dysku lokalnym, który jest skojarzony z węzeł maszyn wirtualnych Azure przypisane do klaster.
**Rozwijanie HDIZippedFile** | Rozpakuj plik spakowany.
**Wywołania HDICmdScript** | Za pomocą skryptu z cmd.exe.
**Wpisywanie HDILog** | Zapisywanie danych wyjściowych z skryptu niestandardowego na potrzeby akcji skryptów.
**Get usług** | Pobierz listę usług uruchomione na komputerze, w której zostanie wykonany.
**Get-Service** | Z nazwą określonej usługi jako danych wejściowych uzyskać szczegółowe informacje dotyczące określonej usługi (nazwa usługi, procesu ID, stan, itp.) na komputerze, na którym wykonuje skrypt.
**Get-HDIServices** | Pobierz listę usługi HDInsight uruchomione na komputerze, w której zostanie wykonany.
**Get-HDIService** | Przy użyciu określonej nazwy usługi HDInsight jako danych wejściowych, uzyskać szczegółowe informacje dotyczące określonej usługi (nazwa usługi, procesu ID, stan, itp.) na komputerze, na którym wykonuje skrypt.
**Get-ServicesRunning** | Pobierz listę usług, które są uruchomione na komputerze, na którym wykonuje skrypt.
**Get-ServiceRunning** | Sprawdź, czy określonej usługi (według nazwy) działa na komputerze, na którym wykonuje skrypt.
**Get-HDIServicesRunning** | Pobierz listę usługi HDInsight uruchomione na komputerze, w której zostanie wykonany.
**Get-HDIServiceRunning** | Sprawdź, czy określonej usługi HDInsight (według nazwy) działa na komputerze, na którym wykonuje skrypt.
**Get-HDIHadoopVersion** | Pobranie wersji Hadoop zainstalowany na komputerze, w której zostanie wykonany.
**Test IsHDIHeadNode** | Sprawdź, czy komputer, gdzie wykonuje skrypt ma węzła głównego.
**Test IsActiveHDIHeadNode** | Sprawdź, czy komputer, gdzie wykonuje skrypt jest aktywne węzła głównego.
**Test IsHDIDataNode** | Sprawdź, czy komputer, gdzie wykonuje skrypt jest węzeł danych.
**Edytuj HDIConfigFile** | Edytowanie plików konfiguracji gałęzi site.xml, site.xml podstawowych, hdfs site.xml, mapred site.xml lub site.xml przędzy.


## <a name="best-practices-for-script-development"></a>Najważniejsze wskazówki dotyczące opracowywania skryptu

Podczas opracowywania skryptu niestandardowego dla klastrów HDInsight, istnieje kilka najważniejszych wskazówek, aby pamiętać:

- Wersja Hadoop

    HDInsight wersji 3.1 (Hadoop 2,4) oraz powyżej pomocy technicznej przy użyciu akcji skrypt do zainstalowania niestandardowych składników w klastrze. W niestandardowy skrypt należy użyć metody Pomocnik **Get-HDIHadoopVersion** , aby sprawdzić wersję Hadoop przed kontynuowaniem wykonywania innych zadań skryptu.


- Stałe łącza do zasobów skryptu

    Użytkownicy powinni dopilnować, wszystkie skrypty i inne artefakty używane w dostosowanie klastrze pozostają dostępne w czasie klaster i że wersje tych plików nie zmieniają się na czas trwania. Te zasoby są wymagane, jeśli wymagane jest ponowne do obrazowania węzłów w klastrze. Najlepszym rozwiązaniem jest pobieranie i zarchiwizowanie wszystkiego na koncie miejsca do magazynowania, który steruje użytkownika. Może to być domyślne konto miejsca do magazynowania lub na dowolnej stronie kolejne konta miejsca do magazynowania określony w momencie wdrażania dla klastrów niestandardowe.
    W iskrowym i R dostosowania próbki klaster pod warunkiem w dokumentacji, na przykład wprowadzono lokalną kopię zasobów na tym koncie miejsca do magazynowania: https://hdiconfigactions.blob.core.windows.net/.


- Upewnij się, że skrypt dostosowywania klaster jest idempotent

    Należy się spodziewać węzłów klaster HDInsight konieczności ponownego obrazowanych czasie trwania klaster. Skrypt dostosowywania klaster jest uruchamiany, gdy klaster jest ponownie obrazowanych. Ten skrypt muszą być zaprojektowane jako idempotent w znaczeniu, że po ponownie do obrazowania skrypt powinno zwrócenia klaster do tego samego dostosowania Województwo, z którego znajdował się zaraz po skrypt został uruchomiony po raz pierwszy, gdy klaster został utworzony. Na przykład jeśli skryptu niestandardowego zainstalowana aplikacja u D:\AppLocation na jego pierwszego uruchamiania, a następnie w każdej serii kolejnych, po ponownym do obrazowania skrypt należy sprawdzić, czy istnieje aplikacja w lokalizacji D:\AppLocation, przed kontynuowaniem innych czynności w skrypt.


- Instalowanie niestandardowych składników w optymalnej lokalizacji

    W przypadku ponownego obrazowanych węzłach C:\ zasobów dysk i dysku systemowym D:\ mogą być ponownie sformatowany, co powoduje utratę danych i aplikacje, które zostały zainstalowane na tych dysków. To również może wystąpić, jeśli węzeł Azure maszyn wirtualnych (maszyn wirtualnych), który jest częścią klastrze ulegnie awarii i są zastępowane przez nowy węzeł. Składniki można zainstalować na dysku D:\ lub w lokalizacji C:\apps w klastrze. Innych lokalizacji na dysku C:\ są zarezerwowane. Określ lokalizację, w której aplikacji lub biblioteki mają zostać zainstalowane w klastrze skryptu dostosowania.


- Upewnij się, szybkiej architektury klaster

    Usługa HDInsight zawiera architektura pasywne aktywne wysoką dostępność, w którym węzła głównego jest w trybie aktywnym (w miejsce, w którym są uruchomione usługi HDInsight) i innych węzła głównego jest w stanie wstrzymania (w którym HDInsight usług nie działają). Węzły przełączać aktywne oraz w stronie biernej, jeśli są przerywane usługi HDInsight. Jeśli akcja skrypt służy do instalowania usługi na obu węzłach głowy wysokiej dostępności, Uwaga mechanizmu pracy awaryjnej HDInsight nie będzie mógł automatycznie kończą się niepowodzeniem przy użyciu tych usług zainstalowane przez użytkownika. Dlatego użytkownik zainstalował usług w węzłach głowy HDInsight, które mają być bardzo dostępne musisz mieć własne mechanizmu pracy awaryjnej, jeśli w trybie pasywne aktywne lub być w trybie aktywny aktywny.

    Polecenie HDInsight skrypt akcji jest uruchamiane na obu węzłach głowy po roli głowy węzeł jest określony jako wartość parametru *ClusterRoleCollection* . Dlatego podczas projektowania skryptu niestandardowego należy się upewnić, że skrypt jest świadomy tego ustawienia. Czy nie wystąpiły problemy, gdzie te same usługi są zainstalowane i pracę w obu węzłów głowy i są one umieszczane uczestniczących w zawodach ze sobą. Ponadto należy pamiętać, że dane zostaną utracone podczas ponownego tworzenia obrazu, aby oprogramowania zainstalowanego przy użyciu akcji skrypt musi być mechanizm do takich wydarzeń. Aplikacje należy przeznaczona do pracy z wysokiej dostępności dane, które są rozmieszczane wielu węzłów. Należy zauważyć, że maksymalnie 1-5 węzłów w klastrze można ponownie obrazami w tym samym czasie.


- Konfigurowanie niestandardowych składników umożliwia magazyn obiektów Blob platformy Azure

    Niestandardowe składniki, które można zainstalować na przez klaster może być domyślnej konfiguracji korzystania z magazynu System plików Distributed Hadoop (HDFS). Należy zmienić konfigurację, aby użyć magazyn obiektów Blob platformy Azure. Na klaster ponownie utworzyć obraz system plików HDFS otrzymuje sformatowane i zostaną utracone dane, które są przechowywane. Zamiast tego korzystanie z magazynem obiektów Blob platformy Azure zapewnia dane zostaną zachowane.

## <a name="common-usage-patterns"></a>Typowe zastosowania wzorce

Ta sekcja zawiera wskazówki dotyczące wykonywania niektórych typowych upodobania, które mogą pojawić się podczas zapisywania własnego skryptu niestandardowego.

### <a name="configure-environment-variables"></a>Konfigurowanie środowiska zmienne

Często w opracowywanie akcji skryptów, będzie uważa konieczności ustawiania zmiennych środowiska. Na przykład najbardziej prawdopodobną wykorzystania jest podczas pobierania pliku binarnym z witryny zewnętrznej, zainstaluj ją w grupie i Dodaj lokalizację, w którym zainstalowano usługi zmiennej środowiska "PATH". Poniższy fragment pokazano, jak ustawić zmienne środowiska w obszarze skrypt niestandardowy.

    Write-HDILog "Starting environment variable setting at: $(Get-Date)";
    [Environment]::SetEnvironmentVariable('MDS_RUNNER_CUSTOM_CLUSTER', 'true', 'Machine');

Zasad zachowania ustawia zmienną środowiska **MDS_RUNNER_CUSTOM_CLUSTER** wartość "true", a także ustawia zakres tej zmiennej być komputera. W czasie należy pamiętać, że zmienne środowiska są ustawione na odpowiedni zakres — komputera lub użytkownika. Można znaleźć [tutaj] [ 1] uzyskać więcej informacji na temat ustawiania zmiennych środowiska.

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a>Dostęp do lokalizacji przechowywania niestandardowych skryptów

Dowolnego skryptów używane w celu dostosowania klaster musi być w klastrze domyślne konto miejsca do magazynowania lub w publicznej kontenera tylko do odczytu na inne konto miejsca do magazynowania. Jeśli skrypt uzyskuje dostęp do zasobów znajdujących się w innym miejscu te muszą być publicznie (co najmniej publicznej tylko do odczytu). Na przykład można uzyskać dostępu do pliku, a następnie zapisz go za pomocą polecenia SaveFile HDI.

    Save-HDIFile -SrcUri 'https://somestorageaccount.blob.core.windows.net/somecontainer/some-file.jar' -DestFile 'C:\apps\dist\hadoop-2.4.0.2.1.9.0-2196\share\hadoop\mapreduce\some-file.jar'

W tym przykładzie należy się upewnić, że kontenerze 'somecontainer' na koncie miejsca do magazynowania 'somestorageaccount' jest dostępny publicznie. W przeciwnym razie skrypt zostać zgłoszony wyjątek nie odnaleziono i kończą się niepowodzeniem.

### <a name="pass-parameters-to-the-add-azurermhdinsightscriptaction-cmdlet"></a>Przekazywanie parametrów do polecenia cmdlet Dodaj AzureRmHDInsightScriptAction

Aby przekazać wiele parametrów do polecenia cmdlet Dodaj AzureRmHDInsightScriptAction, musisz sformatować wartość ciągu zawiera wszystkie parametry skrypt. Na przykład:

    "-CertifcateUri wasbs:///abc.pfx -CertificatePassword 123456 -InstallFolderName MyFolder"

lub

    $parameters = '-Parameters "{0};{1};{2}"' -f $CertificateName,$certUriWithSasToken,$CertificatePassword


### <a name="throw-exception-for-failed-cluster-deployment"></a>Zostać zgłoszony wyjątek wdrożenia klaster nie powiodło się

Jeśli chcesz uzyskać dokładne powiadamiany o fakt, że klaster dostosowywania nie powiodła się zgodnie z oczekiwaniami, ważne jest, aby zostać zgłoszony wyjątek i się nie powieść tworzenia klaster. Na przykład można przetwarzać pliku, jeśli istnieje i obsługę w przypadku błędu, gdy plik nie istnieje. Czy to zapewnić skrypt zamknięciu bezpiecznie i stan klaster poprawnie jest znany. Poniższy fragment przedstawiono przykładowy sposób osiągnąć ten cel:

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    exit
    }

W tym wstawek Jeśli plik nie został znaleziony, jego doprowadziłaby do Państwie miejsce, w którym skrypt faktycznie zamknięciu bezpiecznie po wydrukowaniu komunikat o błędzie i klaster osiągnie stan uruchomiony przy założeniu, że jej "" pomyślnie procesu dostosowania klaster. Jeśli chcesz otrzymywać powiadomienia dokładnie w taki sposób, że klaster dostosowywania zasadniczo nie powiodła się zgodnie z oczekiwaniami ze względu na brakującego pliku, należy więcej zostać zgłoszony wyjątek i się nie powieść kroku dostosowywania klaster. Aby osiągnąć ten cel należy użyć zamiast tego następujące przykładowe wstawkę kodu.

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    throw
    }


## <a name="checklist-for-deploying-a-script-action"></a>Lista kontrolna wdrażania akcję skryptu
Poniżej przedstawiono czynności, które możemy sporządzonymi podczas przygotowań do zastosowania tych skryptów:

1. Umieszczanie plików, które zawierają skrypty niestandardowe w miejsce, w którym jest dostępny w węzłach podczas wdrażania. Może to być dowolny domyślnych lub kolejne konta miejsca do magazynowania określony w momencie wdrażania klastrów lub innych kontenera publicznie miejsca do magazynowania.
2. Dodawanie kontroli na skrypty, aby upewnić się, że są one wykonywane idempotently, tak, aby skrypt mogą być wykonywane wielokrotnie w tym samym węźle.
3. Polecenia cmdlet programu PowerShell Azure **Dane wyjściowe zapisu** umożliwia drukowanie STDOUT, jak również STDERR. Nie należy używać **Hosta zapisu**.
4. Za pomocą tymczasowego folderu plików, takie jak $env: TEMP, aby zachować pobrany plik używane przez skrypty i wyczyść je w górę po wykonaniu skryptów.
5. Instalowanie oprogramowania niestandardowe tylko na D:\ lub C:\apps. Nie należy używać innych lokalizacji na dysku C: są zarezerwowane. Należy zauważyć, że instalowania plików na dysk C: poza C:\apps folder może spowodować błędy instalacji podczas ponownego obrazy węzła.
6. W przypadku gdy ustawienia poziomu systemu operacyjnego lub pliki konfiguracji usługi Hadoop zostały zmienione, można ponownie uruchomić usługi HDInsight, dlatego mogą odebrać dowolnego ustawienia poziomu systemu operacyjnego, na przykład zmienne środowiska w skryptów.

## <a name="debug-custom-scripts"></a>Debugowanie skryptów niestandardowych

Dzienniki błędów skryptu są przechowywane razem z innych danych wyjściowych na koncie miejsca do magazynowania domyślne określone dla klastrów podczas jej tworzenia. Dzienniki znajdują się w tabeli o nazwie *u < \cluster-name-fragment >< \time-stamp > setuplog*. Są to zagregowane dzienniki, których rekordy z wszystkie węzły (węzła głównego i węzły pracownika), na których skrypt uruchamiania w klastrze.
Łatwy sposób Sprawdź dzienniki jest korzystanie z narzędzi HDInsight programu Visual Studio. Do zainstalowania narzędzia, zobacz [wprowadzenie](hdinsight-hadoop-visual-studio-tools-get-started.md#install-hdinsight-tools-for-visual-studio) do narzędzia Visual Studio Hadoop HDInsight

**Aby sprawdzić dziennika za pomocą programu Visual Studio**

1. Otwórz program Visual Studio.
2. Kliknij pozycję **Widok**, a następnie kliknij **Server Explorer**.
3. Kliknij prawym przyciskiem myszy "Azure", kliknij pozycję Połącz z programem **Microsoft Azure subskrypcji**, a następnie wprowadź poświadczenia.
4. Rozwiń **miejsca do magazynowania**, rozwiń konto Azure magazynowania używane jako domyślnego systemu plików, rozwiń **tabele**, a następnie kliknij dwukrotnie nazwę tabeli.


Możesz również zdalnego w węzłach, aby wyświetlić STDOUT i STDERR dla niestandardowych skryptów. Dzienniki w każdym węźle dotyczą tylko tego węzła i są rejestrowane w **C:\HDInsightLogs\DeploymentAgent.log**. Te pliki dziennika rekord wszystkie dane wyjściowe skrypt niestandardowy. Przykład wstawek dziennika akcji skryptu Spark wygląda następująco:

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand; Details : BEGIN: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Starting Spark installation at: 09/04/2014 21:46:02 Done with Spark installation at: 09/04/2014 21:46:38;

    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand;
    Details : END: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;


W tym dzienniku jest jasne, że została wykonana akcja skrypt Spark na maszyn wirtualnych, o nazwie HEADNODE0 i że bez wyjątków zostały zgłoszone podczas wykonywania.

W przypadku, gdy wystąpi błąd wykonania, wynik opisujące go też być zawarte w tym pliku dziennika. Informacje zawarte w tych dziennikach powinny być pomocne w debugowanie skryptu problemów, które mogą się pojawić.


## <a name="see-also"></a>Zobacz też

- [Dostosowywanie przy użyciu akcji skryptu klastrów HDInsight][hdinsight-cluster-customize]
- [Instalowanie i używanie Spark na klastrów HDInsight][hdinsight-install-spark]
- [Instalowanie i używanie R na klastrów HDInsight][hdinsight-r-scripts]
- [Instalowanie i używanie klastrów Solr na HDInsight](hdinsight-hadoop-solr-install.md).
- [Instalowanie i używanie klastrów Giraph na HDInsight](hdinsight-hadoop-giraph-install.md).

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-r-scripts]: hdinsight-hadoop-r-scripts.md
[powershell-install-configure]: install-configure-powershell.md

<!--Reference links in article-->
[1]: https://msdn.microsoft.com/library/96xafkes(v=vs.110).aspx
