<properties
    pageTitle="Skrypt opracowywania akcji z systemem Linux HDInsight | Microsoft Azure"
    description="Jak dostosować klastrów systemem Linux HDInsight z akcją skrypt. Skrypt akcje służą do dostosowywania klastrów Azure HDInsight Określanie ustawień konfiguracji klaster lub instalując dodatkowych usług, narzędzi lub innego oprogramowania w klastrze. "
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="larryfr"/>

# <a name="script-action-development-with-hdinsight"></a>Opracowywanie akcji skryptów z usługi HDInsight

Akcje skryptu służą do dostosowywania klastrów Azure HDInsight Określanie ustawień konfiguracji klaster lub instalując dodatkowych usług, narzędzi lub innego oprogramowania w klastrze. Za pomocą akcji skryptu podczas tworzenia klaster lub w klastrze uruchomione.

> [AZURE.NOTE] Informacje w tym dokumencie są specyficzne dla klastrów HDInsight systemem Linux. Uzyskać informacji na temat korzystania z akcji skryptu z systemem Windows klastrów zobacz [opracowywanie akcji skryptów z usługi HDInsight (Windows)](hdinsight-hadoop-script-actions.md).

## <a name="what-are-script-actions"></a>Co to są akcje skryptu?

Akcje skryptu są skrypty imprezie Azure działa w węzłach dokonać konfiguracji zmian lub instalowanie oprogramowania. Akcja skrypt jest wykonywana jako główny, a zawiera pełne prawa dostępu do przez klaster.

Akcje skryptu można stosować za pomocą następujących metod:

| Umożliwia zastosowanie skrypt... | Podczas klaster tworzenia... | W klastrze uruchomione... |
| ----- |:-----:|:-----:|
| Azure Portal | ✓ | ✓ |
| Azure programu PowerShell | ✓ | ✓ |
| Polecenie Azure | &nbsp; | ✓ |
| Usługa HDInsight .NET SDK | ✓ | ✓ |
| Azure szablonu Menedżera zasobów | ✓ | &nbsp; |

Aby uzyskać więcej informacji na temat korzystania z tych metod do zastosowania akcje skrypt zobacz [klastrów Dostosowywanie HDInsight za pomocą skryptu akcje](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="bestPracticeScripting"></a>Najważniejsze wskazówki dotyczące opracowywania skryptu

Podczas opracowywania skryptu niestandardowego dla klastrów HDInsight, istnieje kilka najważniejszych wskazówek, aby pamiętać:

- [Docelowej wersji Hadoop](#bPS1)
- [Docelowej wersji systemu operacyjnego](#bps10)
- [Stałe łącza do zasobów skryptu](#bPS2)
- [Użyj wstępnie skompilowany zasobów](#bPS4)
- [Upewnij się, że skrypt dostosowywania klaster jest idempotent](#bPS3)
- [Upewnij się, szybkiej architektury klaster](#bPS5)
- [Konfigurowanie niestandardowych składników umożliwia magazyn obiektów Blob platformy Azure](#bPS6)
- [Zapisywanie informacji do STDOUT i STDERR.](#bPS7)
- [Zapisywanie plików w formacie ASCII z końców wysuwu wiersza](#bps8)
- [Odzyskiwanie z błędami przejściowych za pomocą ponów próbę logicznych](#bps9)

> [AZURE.IMPORTANT] Akcje skryptu należy wykonać w ciągu 60 minut lub zostaną przekroczenia limitu czasu. Podczas inicjowania obsługi administracyjnej węzeł, skrypt jest uruchomione równocześnie z innymi procesami instalacja i konfiguracja. Konkurencji dla zasobów, takich jak przepustowość Procesora czasu lub sieci może spowodować skrypt umożliwiający potrwać dłużej niż w środowisku rozwoju.

### <a name="bPS1"></a>Docelowej wersji Hadoop

Różne wersje HDInsight używają różnych wersji usługi Hadoop i zainstalowane składniki. Jeśli skrypt oczekuje określonej wersji usługi lub składnik, skrypt należy używać tylko w wersji HDInsight, zawierający wymagane składniki. Możesz znaleźć informacji na temat wersje składnika uwzględnione w systemie HDInsight przy użyciu [HDInsight składnik wersji](hdinsight-component-versioning.md) dokumentu.

###<a name="bps10"></a>Docelowa wersja systemu operacyjnego

Usługa HDInsight systemem Linux jest oparty na rozkład Ubuntu Linux. Różne wersje HDInsight oparte na różnych wersji Ubuntu, który może dokonać zachowanie skrypt. Na przykład HDInsight 3.4 lub nowszy na podstawie wersji Ubuntu, które używają Upstart. Wersja 3.5 jest oparty na 16.04 Ubuntu, która używa Systemd. Systemd oraz Upstart są oparte na różne polecenia, więc skrypt powinny być zapisywane do pracy z obu.

Inna ważna różnica między HDInsight 3.4 i 3.5 jest `JAVA_HOME` teraz punkty języka Java 8.

Możesz sprawdzić wersję systemu operacyjnego przy użyciu `lsb_release`. Następujące fragmenty kodu skryptu zainstaluj odpowiednią wartość odcienia pokazano, jak ustalić, czy skrypt działa na Ubuntu 14 lub 16:

    OS_VERSION=$(lsb_release -sr)
    if [[ $OS_VERSION == 14* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
        HUE_TARFILE=hue-binaries-14-04.tgz
    elif [[ $OS_VERSION == 16* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
        HUE_TARFILE=hue-binaries-16-04.tgz
    fi
    ...
    if [[ $OS_VERSION == 16* ]]; then
        echo "Using systemd configuration"
        systemctl daemon-reload
        systemctl stop webwasb.service    
        systemctl start webwasb.service
    else
        echo "Using upstart configuration"
        initctl reload-configuration
        stop webwasb
        start webwasb
    fi
    ...
    if [[ $OS_VERSION == 14* ]]; then
        export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
    elif [[ $OS_VERSION == 16* ]]; then
        export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
    fi

Można znaleźć pełny skrypt, który zawiera te wstawki u https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh.

Wersję Ubuntu jest używana przez usługi HDInsight zobacz dokument [wersja składnika HDInsight](hdinsight-component-versioning.md) .

Aby poznać różnice między Systemd i Upstart, zobacz [Systemd Upstart użytkowników](https://wiki.ubuntu.com/SystemdForUpstartUsers).

### <a name="bPS2"></a>Stałe łącza do zasobów skryptu

Należy upewnić się, że skrypty i zasoby używane przez skrypt są dostępne w czasie klaster i że wersje tych plików nie zmieniają się na czas trwania. Te zasoby są wymagane, jeśli nowe węzły są dodawane do klaster podczas skalowania operacji.

Najlepszym rozwiązaniem jest pobieranie i zarchiwizowanie wszystkiego w konto Azure miejsca do magazynowania w subskrypcji.

> [AZURE.IMPORTANT] Konto miejsca do magazynowania, używane musi być domyślne konto miejsca do magazynowania dla klaster lub kontenerem publicznej, tylko do odczytu na inne konto miejsca do magazynowania.

Na przykład próbki dostarczane przez firmę Microsoft są przechowywane w [https://hdiconfigactions.blob.core.windows.net/](https://hdiconfigactions.blob.core.windows.net/) konto miejsca do magazynowania, które jest obsługiwane przez zespół HDInsight kontenera publicznej, tylko do odczytu.

### <a name="bPS4"></a>Użyj wstępnie skompilowany zasobów

Aby skrócić czas potrzebny na uruchamianie skryptu, unikaj operacji, które można skompilować zasoby z kodu źródłowego. Zamiast tego wstępnie skompilować zasobów i przechowywanie wersji binarne w magazynie obiektów Blob platformy Azure tak, aby je szybko można pobrać z klastrem z skrypt.

### <a name="bPS3"></a>Upewnij się, że skrypt dostosowywania klaster jest idempotent

Skrypty muszą być zaprojektowane jako idempotent w znaczeniu, że jeśli skrypt jest uruchomiono wielokrotnie, powinny zapewnić, że klaster są zwracane do niezmienionym za każdym razem, gdy jest uruchomiony.

Na przykład jeśli skryptu niestandardowego instaluje aplikację pod /usr/local/bin na jego pierwszego uruchamiania, a następnie przy każdym uruchomieniu kolejnych skrypt należy sprawdzić, czy aplikacja już istnieje w określonej lokalizacji /usr/local/bin, przed kontynuowaniem innych czynności w skrypt.

### <a name="bPS5"></a>Upewnij się, szybkiej architektury klaster

Systemem Linux klastrów HDInsight zawiera dwa węzły głowy, które są aktywne w klastrze i skrypt, które akcje są wykonano dla obu węzłów. Jeżeli składniki, które można zainstalować tylko jeden węzeł głowy, należy zaprojektować skrypt, który będzie tylko zainstalować składnik w jednym z dwóch głowy węzłów w klastrze.

> [AZURE.IMPORTANT] Domyślne usługi zainstalowano jako część HDInsight mają się nie powieść nad między dwa węzły głowy stosownie do potrzeb, jednak ta funkcja nie jest używane niestandardowe składniki zainstalowane za pomocą skryptu na komputerze. Jeśli potrzebujesz składniki zainstalowane za pomocą akcji skrypt, aby był dostępny podczas wysoce, należy zaimplementować własny mechanizm pracy awaryjnej, która korzysta z dwóch dostępne węzły głowy.

### <a name="bPS6"></a>Konfigurowanie niestandardowych składników umożliwia magazyn obiektów Blob platformy Azure

Składniki, które można zainstalować na klaster może być domyślnej konfiguracji, która używa magazynu System plików Distributed Hadoop (HDFS). Usługa HDInsight używa magazyn obiektów Blob platformy Azure (WASB) jako domyślnego magazynu. Dzięki temu system plików zgodne HDFS, który będzie nadal występował danych, nawet jeśli klaster zostanie usunięty. Należy skonfigurować składniki zainstalować używać WASB zamiast HDFS.

Na przykład poniższa skopiowanie pliku giraph examples.jar z lokalnego systemu plików do WASB:

    hadoop fs -copyFromLocal /usr/hdp/current/giraph/giraph-examples.jar /example/jars/

### <a name="bPS7"></a>Zapisywanie informacji do STDOUT i STDERR.

Informacje zapisane STDOUT i STDERR podczas wykonywania skryptu jest rejestrowane i można wyświetlać za pomocą sieci web Ambari interfejsu użytkownika.

> [AZURE.NOTE] Ambari tylko będą dostępne, jeśli klaster jest tworzony pomyślnie. Jeśli można używać podczas tworzenia klaster akcji skryptu i utworzenie nie powiedzie się, zobacz sekcji rozwiązywania problemów, [Dostosowywanie HDInsight klastrów przy użyciu akcji skryptu](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) dla inne sposoby uzyskiwania dostępu do zarejestrowane informacje.

Większość narzędzi i pakietów instalacyjnych już zapisze informacje STDOUT i STDERR, jednak chcesz dodać dodatkowe rejestrowanie. Aby wysłać tekst do użycia STDOUT `echo`. Na przykład:

        echo "Getting ready to install Foo"

Domyślnie `echo` wyśle ciąg stdout. Aby skierować ją na STDERR, Dodaj `>&2` przed `echo`. Na przykład:

        >&2 echo "An error occurred installing Foo"

Informacje są wysyłane do STDOUT (1, co jest domyślnym, dlatego nie wymieniono tutaj,) to przekierowuje stderr (2). Aby uzyskać więcej informacji dotyczących przekierowywania Jo zobacz [http://www.tldp.org/LDP/abs/html/io-redirection.html](http://www.tldp.org/LDP/abs/html/io-redirection.html).

Aby uzyskać więcej informacji na wyświetlanie informacji o zarejestrowane przez akcje skrypt zobacz [klastrów Dostosowywanie HDInsight przy użyciu akcji skryptu](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting)

###<a name="bps8"></a>Zapisywanie plików w formacie ASCII z końców wysuwu wiersza

Skryptów imprezie powinny być przechowywane jako ASCII format z liniami zamykane przez wysuwu wiersza. Jeśli pliki są przechowywane jako UTF-8, który może zawierać znak porządku bajtów na początku pliku lub z końców programu CRLF, czyli wspólne dla systemu Windows edytory, skrypt zakończy się niepowodzeniem z błędami podobny do następującego:

    $'\r': command not found
    line 1: #!/usr/bin/env: No such file or directory

###<a name="bps9"></a>Odzyskiwanie z błędami przejściowych za pomocą ponów próbę logicznych

Pobieranie plików instalacji pakietów przy użyciu stanie get lub inne akcje, których dane są przesyłane przez internet, akcja może zakończyć się niepowodzeniem z powodu przejściowych błędów sieci. Na przykład zdalnego zasobu, które komunikują się z może być podczas awarii do węzła kopii zapasowej.

Aby wprowadzić skrypt mechanizm przejściowych błędy, można zaimplementować ponów próbę logicznych. Poniżej przedstawiono przykład funkcji uruchomisz dowolnego polecenia przekazywane do niego i (Jeśli to polecenie nie powiedzie się,) Ponów próbę maksymalnie trzy razy. Zostanie on oczekiwania przez dwie sekundy między kolejnymi próbami.

    #retry
    MAXATTEMPTS=3

    retry() {
        local -r CMD="$@"
        local -i ATTMEPTNUM=1
        local -i RETRYINTERVAL=2

        until $CMD
        do
            if (( ATTMEPTNUM == MAXATTEMPTS ))
            then
                    echo "Attempt $ATTMEPTNUM failed. no more attempts left."
                    return 1
            else
                    echo "Attempt $ATTMEPTNUM failed! Retrying in $RETRYINTERVAL seconds..."
                    sleep $(( RETRYINTERVAL ))
                    ATTMEPTNUM=$ATTMEPTNUM+1
            fi
        done
    }

Poniżej przedstawiono przykłady używania tej funkcji.

    retry ls -ltr foo

    retry wget -O ./tmpfile.sh https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh

## <a name="helpermethods"></a>Pomocnik metod niestandardowych skryptów

skrypt akcji Pomocnik metody są narzędzia, które można używać podczas pisania własnych skryptów. Te są definiowane w [https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh)i mogą być zawarte w skryptów za pomocą następujących:

    # Import the helper method module.
    wget -O /tmp/HDInsightUtilities-v01.sh -q https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh && source /tmp/HDInsightUtilities-v01.sh && rm -f /tmp/HDInsightUtilities-v01.sh

Dzięki temu następujące wątków dostępne do użytku za pomocą skryptu:

| Pomocnik zastosowania | Opis |
| ------------ | ----------- |
| `download_file SOURCEURL DESTFILEPATH [OVERWRITE]` | Pobieranie pliku z źródłowy adres URL do określonej ścieżce pliku. Domyślnie go nie zastąpi istniejący plik. |
| `untar_file TARFILE DESTDIR` | Wyodrębnia plik tar (za pomocą `-xf`,) do katalogu docelowego. |
| `test_is_headnode` | Jeśli uruchomiono na węzła głównego klaster zwraca wartość 1; w przeciwnym razie 0. |
| `test_is_datanode` | Jeśli bieżący węzeł jest węzeł danych (pracownik), zwraca wartość 1; w przeciwnym razie 0. |
| `test_is_first_datanode` | Jeśli bieżący węzeł jest pierwszym węźle danych (pracownik) (o nazwie workernode0), zwraca wartość 1; w przeciwnym razie 0. |
| `get_headnodes` | Zwraca w pełni kwalifikowaną nazwę domeny z headnodes w klastrze. Nazwy są rozdzielane przecinkami. W przypadku błędu, zwracany jest pusty ciąg. |
| `get_primary_headnode` | Otrzymuje w pełni kwalifikowana nazwa domeny podstawowej headnode. W przypadku błędu, zwracany jest pusty ciąg. |
| `get_secondary_headnode` | Otrzymuje w pełni kwalifikowaną nazwę domeny headnode pomocniczą. W przypadku błędu, zwracany jest pusty ciąg. |
| `get_primary_headnode_number` | Otrzymuje sufiks liczbową headnode podstawowego. W przypadku błędu, zwracany jest pusty ciąg. |
| `get_secondary_headnode_number` | Otrzymuje liczbowe sufiks pomocniczej headnode. W przypadku błędu, zwracany jest pusty ciąg. |

## <a name="commonusage"></a>Typowe zastosowania wzorce

Ta sekcja zawiera wskazówki dotyczące wykonywania niektórych typowych upodobania, które mogą pojawić się podczas zapisywania własnego skryptu niestandardowego.

### <a name="passing-parameters-to-a-script"></a>Przekazywanie parametrów do skryptu

W niektórych przypadkach skrypt może wymagać parametry. Na przykład może być konieczne hasło administratora dla klaster w celu pobierania informacji z interfejsu API usługi REST Ambari.

Parametry przekazywane do skryptu są nazywane _pozycyjne parametry_i są przypisane do `$1` jako pierwszy parametr `$2` dla drugiego, a więc na. `$0`zawiera nazwę samego skryptu.

Wartości, które zostały przekazane do skryptu jako parametry powinny być ujęte w pojedynczy cudzysłów ('), aby przekazana wartość jest traktowana jako literał i specjalne traktowanie nie jest nadana dołączone znaki, takie jak "!".

### <a name="setting-environment-variables"></a>Ustawienia środowiska zmiennych

Ustawianie zmiennej środowiska odbywa się w następujących czynności:

    VARIABLENAME=value

Gdzie VARIABLENAME jest nazwą zmiennej. Aby uzyskać dostęp do zmiennej później, należy użyć `$VARIABLENAME`. Na przykład przypisanie wartości podane przez parametr pozycyjne jako zmiennej środowiska o nazwie HASŁA, możesz użyć następujących czynności:

    PASSWORD=$1

Dostęp do informacji można używać `$PASSWORD`.

Zmienne środowiska w skrypt istnieć tylko w zakresie skrypt. W niektórych przypadkach może być konieczne dodawanie system środowiska wielu zmiennych, które będzie umieszczony po zakończeniu skrypt. Zazwyczaj jest to, dzięki czemu użytkowników łączących się z klastrem za pośrednictwem SSH można użyć składniki zainstalowane przez skrypt. Można to zrobić, dodając zmiennej środowiska `/etc/environment`. Na przykład poniższy dodaje __HADOOP\_Telefonicznej\_DIR__:

    echo "HADOOP_CONF_DIR=/etc/hadoop/conf" | sudo tee -a /etc/environment

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a>Dostęp do lokalizacji przechowywania niestandardowych skryptów

Dowolnego skryptów używane w celu dostosowania klastrze musi znajdować się w domyślne konto miejsca do magazynowania dla klastrów lub, jeśli na inne konto miejsca do magazynowania, w kontenerze publicznej tylko do odczytu. Jeśli skrypt uzyskuje dostęp do zasobów znajdujących się w innym miejscu są również muszą być publicznie (co najmniej publicznej tylko do odczytu). Na przykład można pobrać plik za pomocą klaster `download_file`.

Przechowywanie plik na konto Azure miejsca do magazynowania dostępnego przez klaster (na przykład domyślnego konta miejsca do magazynowania), podaj szybki dostęp, tak jak tego miejsca do magazynowania w ramach Azure sieci.

### <a name="checking-the-operating-system-version"></a>Sprawdzanie wersji systemu operacyjnego

Różne wersje HDInsight zależne od określonych wersjach programu Ubuntu. Czasami może występować różnic między wersjami systemu operacyjnego, które można należy sprawdzić za pomocą skryptu. Na przykład może być konieczne instalowanie binarnego, który jest związany z wersji Ubuntu.

Aby sprawdzić wersję systemu operacyjnego, użyj `lsb_release`. Na przykład poniższa przedstawia sposób odwołanie pliku tar różne w zależności od wersji systemu operacyjnego:

    OS_VERSION=$(lsb_release -sr)
    if [[ $OS_VERSION == 14* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
        HUE_TARFILE=hue-binaries-14-04.tgz
    elif [[ $OS_VERSION == 16* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
        HUE_TARFILE=hue-binaries-16-04.tgz
    fi

## <a name="deployScript"></a>Lista kontrolna wdrażania akcję skryptu

Poniżej przedstawiono czynności, które możemy sporządzonymi podczas przygotowań do zastosowania tych skryptów:

- Umieszczanie plików, które zawierają skrypty niestandardowe w miejsce, w którym jest dostępny w węzłach podczas wdrażania. Może to być dowolny domyślnych lub kolejne konta miejsca do magazynowania określony w momencie wdrażania klastrów lub innych kontenera publicznie miejsca do magazynowania.

- Dodawanie kontroli na skrypty, aby upewnić się, że są wykonywane impotently, tak, aby skrypt mogą być wykonywane wielokrotnie w tym samym węźle.

- Za pomocą /tmp katalogu plików tymczasowych zachować pobrane pliki używane przez skrypty i wyczyść je w górę po wykonaniu skryptów.

- W przypadku gdy ustawienia poziomu systemu operacyjnego lub pliki konfiguracji usługi Hadoop zostały zmienione, można ponownie uruchomić usługi HDInsight, dlatego mogą odebrać dowolnego ustawienia poziomu systemu operacyjnego, na przykład zmienne środowiska w skryptów.

## <a name="runScriptAction"></a>Jak uruchomić akcję skryptu

Skrypt akcje służą do dostosowywania klastrów HDInsight za pomocą portalu Azure Azure programu PowerShell, szablony Azure Menedżera zasobów (ARM) lub HDInsight .NET SDK. Aby uzyskać instrukcje zobacz [jak za pomocą skryptu akcji](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="sampleScripts"></a>Przykłady skryptu niestandardowego

Firma Microsoft udostępnia przykładowe skrypty, aby zainstalować składniki w klastrze HDInsight. Przykładowe skrypty i instrukcje dotyczące sposobu korzystania z nich są dostępne w poniższych łączy:

- [Instalowanie i używanie odcień na klastrów HDInsight](hdinsight-hadoop-hue-linux.md)
- [Instalowanie i używanie R na klastrów HDInsight Hadoop](hdinsight-hadoop-r-scripts-linux.md)
- [Instalowanie i używanie Solr na klastrów HDInsight](hdinsight-hadoop-solr-install-linux.md)
- [Instalowanie i używanie Giraph na klastrów HDInsight](hdinsight-hadoop-giraph-install-linux.md)  

> [AZURE.NOTE] Dokumenty, połączone powyżej są specyficzne dla klastrów HDInsight systemem Linux. Dotyczące skryptów, które współpracują z usługi HDInsight systemu Windows zobacz [opracowywanie akcji skryptów z usługi HDInsight (Windows)](hdinsight-hadoop-script-actions.md) lub skorzystaj z łączy dostępnych w górnej części każdej artykuł.

##<a name="troubleshooting"></a>Rozwiązywanie problemów

Błędy, które można napotkać podczas korzystania z skryptów, które masz są następujące:

__Error__: `$'\r': command not found`. Czasami następuje `syntax error: unexpected end of file`.

_Przyczyna_: ten błąd występuje, gdy linie skryptu kończy się CRLF. Systemy UNIX oczekiwać tylko wysuwu wiersza jako koniec wiersza.

Ten problem najczęściej występuje, gdy skrypt został utworzony w środowisku systemu Windows, tak jak CRLF linii zakończenie dla wielu edytory tekstu w systemie Windows.

_Rozdzielczość_: Jeśli jest to opcję w edytorze tekstu, wybierz format systemu Unix lub wysuwu wiersza na koniec wiersza. W systemie Unix może również użyć następujących poleceń, aby zmienić CRLF wysuwu wiersza:

> [AZURE.NOTE] Następujące polecenia są przybliżeniu, w tym ich należy zmienić końców CRLF na wysuwu wiersza. Wybierz jeden zgodnie z narzędzi dostępnych w systemie.

| Polecenie | Notatki |
| ------- | ----- |
| `unix2dos -b INFILE` | Oryginalny plik będzie kopii zapasowej z. Rozszerzenie BAK |
| `tr -d '\r' < INFILE > OUTFILE` | PLIKWYJŚCIOWY będzie zawierać wersję z tylko zakończenia wysuwu wiersza |
| `perl -pi -e 's/\r\n/\n/g' INFILE` | Plik zostanie zmodyfikowany bezpośrednio bez tworzenia nowego pliku |
| ```sed 's/$'"/`echo \\\r`/" INFILE > OUTFILE``` | PLIKWYJŚCIOWY będzie zawierać wersję z tylko zakończenia wysuwu wiersza.

__Error__: `line 1: #!/usr/bin/env: No such file or directory`.

_Przyczyna_: ten błąd występuje, gdy skrypt został zapisany jako UTF-8 z znacznik kolejności bajtów (BOM).

_Rozdzielczość_: Zapisz plik jako ASCII, lub UTF-8 bez BOM. Może również utworzyć nowy plik bez BOM za pomocą następującego polecenia na komputerze z systemem Unix lub Linux:

    awk 'NR==1{sub(/^\xef\xbb\xbf/,"")}{print}' INFILE > OUTFILE

Powyższe polecenia Zamień __PLIKWEJŚCIOWY__ plik zawierający BOM. __PLIKWYJŚCIOWY__ powinny być nową nazwę pliku, który będzie zawierał skrypt bez BOM.

## <a name="seeAlso"></a>Następne kroki

* Dowiedz się, jak [dostosować HDInsight klastrów przy użyciu akcji skryptu](hdinsight-hadoop-customize-cluster-linux.md)

* Dowiedz się więcej o tworzeniu aplikacji .NET HDInsight za pomocą [odwołania HDInsight .NET SDK](https://msdn.microsoft.com/library/mt271028.aspx)

* Aby dowiedzieć się, jak używać pozostałych do działania zarządzania na klastrów HDInsight za pomocą [Interfejsu API usługi REST HDInsight](https://msdn.microsoft.com/library/azure/mt622197.aspx) .
