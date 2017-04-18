<properties
pageTitle="Dodawanie bibliotek gałęzi podczas tworzenia klaster HDInsight | Azure"
description="Dowiedz się, jak dodać gałąź biblioteki (słoik pliki), z klastrem HDInsight podczas tworzenia klaster."
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
ms.date="09/20/2016"
ms.author="larryfr"/>

#<a name="add-hive-libraries-during-hdinsight-cluster-creation"></a>Dodawanie bibliotek gałęzi podczas tworzenia klaster HDInsight

Jeśli masz bibliotek, które często używanych z gałęzi na HDInsight ten dokument zawiera informacji na temat korzystania z akcji skrypt do wstępnie załadować bibliotek podczas tworzenia klaster. Dzięki temu bibliotek globalnie dostępnej w gałęzi (bez potrzeby ładowanie ich za pomocą [Dodawanie JAR](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) .)

##<a name="how-it-works"></a>Jak to działa

Podczas tworzenia klastrze, opcjonalnie można określić akcję skryptu uruchamianej skrypt na przez klaster, gdy jest tworzona. Skrypt w tym dokumencie akceptuje jeden parametr, czyli lokalizacji WASB, która zawiera biblioteki (przechowywane jako pliki jar), które będą ładowane wstępnie.

Podczas tworzenia klaster skrypt wylicza pliki, skopiowanie ich do `/usr/lib/customhivelibs/` katalogu w węzłach głowy i Pracownik, dodaniem ich do `hive.aux.jars.path` właściwości w `core-site.xml` pliku. Na podstawie Linux klastrów również aktualizacji `hive-env.sh` plik z lokalizacji plików.

> [AZURE.NOTE] Za pomocą akcji skryptu w tym artykule udostępnia biblioteki w następujących sytuacjach:
>
> * __Usługa HDInsight systemem Linux__ — podczas korzystania z __gałęzi wiersza polecenia__, __WebHCat__ (Templeton) i __HiveServer2__.
> * __Usługa HDInsight systemu Windows__ — podczas korzystania z __gałęzi wiersza polecenia__ i __WebHCat__ (Templeton).

##<a name="the-script"></a>Skrypt

__Lokalizacja skryptu__

W przypadku __klastrów z systemem Linux__: [https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh)

W przypadku __klastrów systemu Windows__: [https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)

__Wymagania dotyczące__

* Skrypty należy przeprowadzić __węzły głowy__ i __węzły pracownika__.

* Słoików, którą chcesz zainstalować muszą być przechowywane w magazynie obiektów Blob platformy Azure w __jeden kontener__. 

* Konto miejsca do magazynowania, zawierające Biblioteka słoik plików __musi__ być połączone z klaster HDInsight podczas tworzenia. Można to zrobić na dwa sposoby:

    * Jest w kontenerze w klastrze domyślne konto miejsca do magazynowania.
    
    * Jest w kontenerze w kontenerze połączone miejsca do magazynowania. Na przykład w portalu można użyć __Opcjonalnym__ __połączone konta przestrzeni dyskowej__ , aby dodać dodatkowe miejsce do magazynowania.

* Jako parametr Akcja skrypt należy określić ścieżkę WASB w kontenerze. Na przykład jeśli słoików są przechowywane w kontenerze o nazwie __bibliotekami__ na koncie miejsca do magazynowania o nazwie __mystorage__, parametr będzie __wasbs://libs@mystorage.blob.core.windows.net/__.

    > [AZURE.NOTE] Ten dokument założono utworzono już konto miejsca do magazynowania, blob kontenera i przekazać pliki do niego. 
    >
    > Jeśli nie utworzono konto miejsca do magazynowania, możesz to zrobić za pomocą [Azure portal](https://portal.azure.com). Następnie za pomocą narzędzi, takich jak [Eksplorator magazynu Azure](http://storageexplorer.com/) do tworzenia nowego kontenera w oknie konta i przekazywać pliki do niego.

##<a name="create-a-cluster-using-the-script"></a>Tworzenie klaster za pomocą skryptu

> [AZURE.NOTE] Poniższe kroki Utwórz nowy klaster HDInsight systemem Linux. Aby utworzyć nowy klaster systemu Windows, wybierz pozycję __Windows__ jako klaster OS podczas tworzenia klaster, a zamiast skrypt imprezie za pomocą skryptu systemu Windows (programu PowerShell).
> 
> Aby utworzyć klaster przy użyciu tego skryptu, można użyć programu PowerShell Azure lub HDInsight .NET SDK. Aby uzyskać więcej informacji na temat korzystania z tych metod zobacz [Dostosowywanie HDInsight klastrów z akcjami skrypt](hdinsight-hadoop-customize-cluster-linux.md).

1. Rozpoczynanie inicjowania obsługi administracyjnej klastrze, wykonując kroki opisane w [klastrów świadczenia usługi HDInsight na Linux](hdinsight-hadoop-provision-linux-clusters.md#portal), ale nie zostanie inicjowania obsługi administracyjnej.

2. Na karta **Opcjonalnym** wybierz **Akcje skryptu**i wprowadź informacje, jak pokazano poniżej:

    * __Nazwa__: Wprowadź przyjazną nazwę akcji skryptów.
    * __Identyfikator URI skrypt__: https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh
    * __Szef__: Zaznaczenie tego pola wyboru
    * __Pracownik__: Zaznacz tę opcję.
    * __ZOOKEEPER__: pozostaw to pole puste.
    * __Parametry__: Wprowadź adres WASB konta kontenera i miejsca do magazynowania, zawierającego słoików. Na przykład __wasbs://libs@mystorage.blob.core.windows.net/__.

3. U dołu **Akcje skrypt**Użyj przycisk **Wybierz** , aby zapisać konfigurację.

4. Na karta **Opcjonalnym** zaznacz __Połączone konta miejsca do magazynowania__ i wybierz łącze __Dodaj klucz miejsca do magazynowania__ . Wybierz konto miejsca do magazynowania, który zawiera słoików, a następnie użyj przycisków __Wybierz__ , aby zapisać ustawienia i wrócić karta __Opcjonalnym__ .

5. Przycisk **Zaznacz** w dolnej części karta **Opcjonalnym** umożliwia zapisywanie informacji opcjonalnym.

6. Kontynuuj inicjowania obsługi administracyjnej klaster, zgodnie z opisem w [klastrów świadczenia usługi HDInsight na Linux](hdinsight-hadoop-provision-linux-clusters.md#portal).

Po zakończeniu tworzenia klaster będzie mógł korzystać z słoików dodane do tego skryptu z gałęzi bez konieczności używania `ADD JAR` instrukcji.

##<a name="next-steps"></a>Następne kroki

W tym dokumencie zapoznaniu Dodawanie bibliotek gałęzi zawarte w plikach słoik do klastrów HDInsight podczas tworzenia klaster. Aby uzyskać więcej informacji o pracy z gałęzi zobacz [Używanie gałęzi z usługi HDInsight](hdinsight-use-hive.md)
