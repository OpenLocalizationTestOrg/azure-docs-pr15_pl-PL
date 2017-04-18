<properties
    pageTitle="Informacje o wersji programu Hadoop składników na Azure HDInsight | Microsoft Azure"
    description="Najnowsze informacje o wersji i wersje składników Hadoop Azure HDInsight. Uzyskać porady dotyczące projektowania i szczegóły Hadoop, Burza Apache i HBase."
    services="hdinsight"
    documentationCenter=""
    editor="cgronlun"
    manager="jhubbard"
    authors="nitinme"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="nitinme"/>


# <a name="release-notes-for-hadoop-components-on-azure-hdinsight"></a>Informacje o wersji dla składników Hadoop w Azure HDInsight

## <a name="notes-for-10262016-release-of-r-server-on-hdinsight"></a>Notatki w wersji 2016-10-26 serwera R na HDInsight

- Został ulepszony serwera R w HDInsight klaster inicjowania obsługi administracyjnej.
- Serwer R na HDInsight jest teraz dostępny jako zwykłego HDInsight "Serwer R" klaster typu i nie jest już zainstalowana jako osobne aplikacja HDInsight. Węzeł krawędzi i R serwera plików binarnych teraz zainicjowano obsługę administracyjną w ramach wdrożenia klaster serwera R. Usprawnia szybkości i niezawodności inicjowania obsługi administracyjnej. Cennik modelu dla serwera R jest uaktualniany.
- Serwer R klaster typu cena teraz jest oparty na standardowy poziomu cen oraz cena opłata dodatkowa serwera R. Warstwa Premium zostanie zarezerwowane dla funkcji Premium dostępne różnych typów różnych klaster i nie będą używane dla typu klaster serwera R. Ta zmiana nie ma wpływu na ceny skutecznych serwera R, tylko zmienia jak opłat są prezentowane w zestawieniu. Wszystkich istniejących klastrów serwerów R będą nadal działać i szablony ARM będzie działać czasu oczekiwany powiadomienia. **Zalecane jest, jeśli do aktualizowania skryptów wdrożeń używać nowego szablonu ARM.**


## <a name="notes-for-08302016-release-of-r-server-on-hdinsight"></a>Notatki w wersji 2016-08-30 serwera R na HDInsight

Numery pełnej wersji dla klastrów systemem Linux HDInsight wdrożony w tej wersji:

|HDI |HDI klaster wersji   |HDP |Tworzenie HDP   |Tworzenie Ambari |
|----|----------------------|----|------------|-------------|
|3,2 |3.2.1000.0.8268980    |2.2 |2.2.9.1-19  |2.2.1.12-4   |
|3.3 |3.3.1000.0.8268980    |2.3 |2.3.3.1-25  |2.2.1.12-4   |
|3.4 |3.4.1000.0.8269383    |2,4 |2.4.2.4-5   |2.2.1.12-4   |

Numery pełnej wersji dla klastrów HDInsight systemu Windows wdrożony w tej wersji:

|HDI |HDI klaster wersji   |HDP |Tworzenie HDP     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.1033.2559206   |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.1033.2559206    |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.1033.2559206    |2.1 |2.1.16.0-2374 |
|3,2 |3.2.7.1033.2559206    |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.1033.2559206    |2.3 |2.3.3.1-25    |

## <a name="notes-for-08172016-release-of-r-server-on-hdinsight"></a>Notatki w wersji 2016-08-17 serwera R na HDInsight

- Serwer R 8.0.5 — głównie wersji poprawka błędu. Zobacz [Informacje o wersji serwera R](https://msdn.microsoft.com/microsoft-r/notes/r-server-notes) Aby uzyskać więcej informacji. 
- Pakiet AzureML w węźle krawędź — [Ten pakiet R](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) umożliwia modeli R zostanie opublikowany i zużyte jako usługi sieci web Azure ML.  Zobacz sekcję ["Operationalize modelu"](hdinsight-hadoop-r-server-overview.md#operationalize-a-model) nasze artykułu ["Omówienie z R Server na HDInsight"](hdinsight-hadoop-r-server-overview.md) , aby uzyskać więcej informacji.
- Zależności Linux [Górny 100 najpopularniejszych pakietów R](https://github.com/metacran/cranlogs) — te Linux zależności pakietów są wstępnie zainstalowany.  
- Opcji CRAN repo Dodawanie R pakiety do węzłów danych. Zobacz sekcję ["Zainstaluj R pakietów"](hdinsight-hadoop-r-server-get-started.md#install-r-packages) naszych artykułu ["Rozpocząć korzystanie z serwera R w HDInsight"](hdinsight-hadoop-r-server-get-started.md) , aby uzyskać więcej informacji.
- Większa niezawodność serwera R inicjowania obsługi administracyjnej, podczas tworzenia klastrów.


## <a name="notes-for-08012016-release-of-hdinsight"></a>Informacje o wersji 2016-08-01 HDInsight

Numery pełnej wersji dla klastrów systemem Linux HDInsight wdrożony w tej wersji:

|HDI |HDI klaster wersji   |HDP |Tworzenie HDP   |Tworzenie Ambari |
|----|----------------------|----|------------|-------------|
|3,2 |3.2.1000.0.8028416    |2.2 |2.2.9.1-19  |2.2.1.12-4   |
|3.3 |3.3.1000.0.8028416    |2.3 |2.3.3.1-25  |2.2.1.12-4   |
|3.4 |3.4.1000.0.8053402    |2,4 |2.4.2.4-5   |2.2.1.12-4   |

Numery pełnej wersji dla klastrów HDInsight systemu Windows wdrożony w tej wersji:

|HDI |HDI klaster wersji   |HDP |Tworzenie HDP     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.1005.2488842   |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.1005.2488842    |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.1005.2488842    |2.1 |2.1.16.0-2374 |
|3,2 |3.2.7.1005.2488842    |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.1005.2488842    |2.3 |2.3.3.1-25    |

Ta wersja zawiera następujące aktualizacje.

| Tytuł                                           | Opis                                          | Obszar ryzyko (na przykład usługi, części lub SDK) | Klaster typu (na przykład Spark, Hadoop, HBase lub Burza) | JIRA (jeśli dotyczy) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Zmiany dotyczące klastrów HDInsight 3.4 | Wartość domyślną dla następujących konfiguracji gałęzi są zmieniane w celu zwiększenia wydajności <ul><li>`hive.vectorized.execution.reduce.enabled=true`</li><li>`hive.tez.min.partition.factor=1f`</li><li>`hive.tez.max.partition.factor=3f`</li><li>`tez.shuffle-vertex-manager.min-src-fraction=0.9`</li><li>`tez.shuffle-vertex-manager.max-src-fraction=0.95`</li><li>`tez.runtime.shuffle.connect.timeout= 30000`</li></ul>| Usługa    | Wszystkie| N/D!|
| Następujące metody rozwiązania znajdują się w tej wersji | GAŁĄŹ 13632, HIVE-12897,HIVE-12907,HIVE-12908,HIVE-12988,HIVE-13510,HIVE-13572,HIVE-13716,HIVE-13726,HIVE-12505,HIVE-13632,HIVE-13661,HIVE-13705,HIVE-13743,HIVE-13810,HIVE-13857,HIVE-13902,HIVE-13911,HIVE-13933| Usługa    | Wszystkie| N/D!

## <a name="notes-for-07142016-release-of-hdinsight"></a>Informacje o wersji 2016-07-14 HDInsight

Numery pełnej wersji dla klastrów systemem Linux HDInsight wdrożony w tej wersji:

|HDI |HDI klaster wersji   |HDP |Tworzenie HDP   |Tworzenie Ambari |
|----|----------------------|----|------------|-------------|
|3,2 |3.2.1000.0.7932505    |2.2 |2.2.9.1-11  |2.2.1.12-2   |
|3.3 |3.3.1000.0.7932505    |2.3 |2.3.3.1-18  |2.2.1.12-2   |
|3.4 |3.4.1000.0.7933003    |2,4 |2.4.2.0     |2.2.1.12-2   |

Numery pełnej wersji dla klastrów HDInsight systemu Windows wdrożony w tej wersji:

|HDI |HDI klaster wersji   |HDP |Tworzenie HDP     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.989.2441725    |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.989.2441725     |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.989.2441725     |2.1 |2.1.16.0-2374 |
|3,2 |3.2.7.989.2441725     |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.989.2441725     |2.3 |2.3.3.1-21    |

## <a name="notes-for-07072016-release-of-hdinsight"></a>Informacje o wersji 2016-07-07 HDInsight

Numery pełnej wersji dla klastrów systemem Linux HDInsight wdrożony w tej wersji:

|HDI |HDI klaster wersji   |HDP |Tworzenie HDP   |
|----|----------------------|----|------------|
|3,2 |3.2.1000.0.7864996    |2.2 |2.2.9.1-11  |
|3.3 |3.3.1000.0.7864996    |2.3 |2.3.3.1-18  |
|3.4 |3.4.1000.0.7861906    |2,4 |2.4.2.0     |

Numery pełnej wersji dla klastrów HDInsight systemu Windows wdrożony w tej wersji:

|HDI |HDI klaster wersji   |HDP |Tworzenie HDP     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.977.2413853    |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.977.2413853     |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.977.2413853     |2.1 |2.1.16.0-2374 |
|3,2 |3.2.7.977.2413853     |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.977.2413853     |2.3 |2.3.3.1-21    |

Ta wersja zawiera następujące aktualizacje.

| Tytuł                                           | Opis                                          | Obszar ryzyko (na przykład usługi, części lub SDK) | Klaster typu (na przykład Spark, Hadoop, HBase lub Burza) | JIRA (jeśli dotyczy) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| [Narzędzia HDInsight do IntelliJ](hdinsight-apache-spark-intellij-tool-plugin.md) | Dodatek POMYSŁU IntelliJ dla klastrów HDInsight Spark jest teraz zintegrowany z Azure zestaw narzędzi dla IntelliJ. Ją obsługuje v2.9.1 Azure SDK, najnowszą SDK Java i zawiera wszystkie funkcje dostępne w autonomicznej dodatek HDInsight dla IntelliJ.| Narzędzia    | Spark| N/D!|
| [Narzędzia HDInsight dla programu Eclipse](hdinsight-apache-spark-eclipse-tool-plugin.md) | Azure zestaw narzędzi dla Zaćmienie obsługuje teraz klastrów HDInsight Spark. Umożliwia następujące funkcje. <ul><li>Tworzenie i zapisywanie aplikacji Spark łatwe w Scala i Java z klasą pierwszego tworzenia obsługę technologii IntelliSense, formatowanie automatyczne sprawdzanie błędów, itp.</li><li>Przetestuj aplikację Spark lokalnie.</li><li>Przesyłanie zadania do klastrów HDInsight Spark i pobrać wyniki.</li><li>Zaloguj się do Azure i uzyskać dostęp do wszystkich klastrów Spark skojarzone z subskrypcji Azure.</li><li>Przejdź wszystkie zasoby magazynu skojarzone klaster HDInsight Spark.</li></ul>| Narzędzia    | Spark| N/D!

Począwszy od tej wersji, zmieniono zasady poprawiania OS gościa dla klastrów HDInsight systemem Linux. Celem nowych zasad jest znacznie zmniejszyć liczbę ponownego uruchamiania z powodu poprawki. Nowych zasad będzie poprawki wirtualnych maszyn klastrów Linux każdej poniedziałek lub czwartek, zaczynając od 00 jest UTC w sposób rozłożonych w węzłach dowolnego dany klaster. Jednak dowolny danego maszyn wirtualnych tylko zostanie uruchomiony co najwyżej raz na 30 dni z powodu poprawki z systemem operacyjnym gościa. Ponadto ponownym dla nowo utworzonego klaster nie nastąpi wcześniej niż 30 dni od daty utworzenia klaster.

>[AZURE.NOTE] Te zmiany zostaną zastosowane tylko do nowo utworzonego klastrów równe lub większe niż ta wersja.

## <a name="notes-for-06062016-release-of-hdinsight"></a>Informacje o wersji 2016-06-06 HDInsight

Numery pełnej wersji dla klastrów HDInsight wdrożony w tej wersji:

|HDP    |Wersja HDI    |Wersja Spark  |Numer kompilacji Ambari    |Numer kompilacji HDP|
|-------|---------------|---------------|-----------------------|----------------|
|2.3    |3.3.1000.0.7702215|    1.5.2|  2.2.1.8-2|  2.3.3.1-18|
|2,4    |3.4.1000.0.7702224|    1.6.1|  2.2.1.8-2|  2.4.2.0|


Ta wersja zawiera następujące aktualizacje.

| Tytuł                                           | Opis                                          | Obszar ryzyko (na przykład usługi, części lub SDK) | Klaster typu (na przykład Spark, Hadoop, HBase lub Burza) | JIRA (jeśli dotyczy) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Spark na HDInsight jest zazwyczaj dostępny | Ta wersja dostarcza ulepszenia dostępność, skalowalność i wydajność, aby otworzyć źródła Spark Apache na HDInsight. <ul><li>Branży dostępność SLA 99,9%, dzięki czemu odpowiednie dla wymagających obciążenia przedsiębiorstwa.</li><li>Warstwa przechowywania skalowalna przy użyciu magazynu Lake danych Azure.</li><li>Narzędzia wydajności dla każdej fazy danych badania i rozwój. Jupyter notesów za pomocą niestandardowych jądra Spark włączać dane interakcyjne badań, integracja z pulpitów nawigacyjnych analizy Biznesowej, takich jak usługi Power BI, Tableau i Qlik będzie pasować do udostępniania szybkie danych i raportowania ciągły, dodatek IntelliJ jest opcja zaufanego długotrwałego kod artefaktu rozwoju i debugowania.</li></ul>| Usługa    | Spark| N/D!|
| Narzędzia HDInsight do IntelliJ | To jest dodatek POMYSŁU IntelliJ dla klastrów HDInsight Spark. Umożliwia następujące funkcje.<ul><li>Tworzenie i zapisywanie aplikacji Spark łatwe w Scala i Java z klasą pierwszego tworzenia obsługę technologii IntelliSense, formatowanie automatyczne sprawdzanie błędów, itp.</li><li>Przetestuj aplikację Spark lokalnie.</li><li>Przesyłanie zadania do klastrów HDInsight Spark i pobrać wyniki.</li><li>Zaloguj się do Azure i uzyskać dostęp do wszystkich klastrów Spark skojarzone z subskrypcji Azure.</li><li>Przejdź wszystkie zasoby magazynu skojarzone klaster HDInsight Spark.</li><li>Nawigowanie w obrębie historii zadania i informacje o zadaniu dla klaster HDInsight Spark.</li><li>Debugowanie Spark zadań za pomocą komputera stacjonarnego.</li></ul>| Narzędzia    | Spark| N/D!

## <a name="notes-for-05132016-release-of-hdinsight"></a>Informacje o wersji 2016-05-13 HDInsight

Numery pełnej wersji dla klastrów HDInsight wdrożony w tej wersji:

* Usługa HDInsight (Windows) 2.1.10.875.2159884 (HDP 1.3.12.0-01795 — bez zmian)
* Usługa HDInsight (Windows) 3.0.6.875.2159884 (HDP 2.0.13.0-2117 — bez zmian)
* Usługa HDInsight (Windows) 3.1.4.922.2266903 (HDP 2.1.15.0-2374 — bez zmian)
* Usługa HDInsight (Windows) 3.2.7.922.2266903 (HDP 2.2.9.1-11)
* Usługa HDInsight (Windows) 3.3.0.922.2266903 (HDP 2.3.3.1-18)
* Usługa HDInsight (Linux) 3.2.1000.0.7565644 (HDP 2.2.9.1-11)
* Usługa HDInsight (Linux) 3.3.1000.0.7565644 (HDP 2.3.3.1-18)
* Usługa HDInsight (Linux) 3.4.1000.0.7548380 (HDP 2.4.2.0)

Ta wersja zawiera następujące aktualizacje.

| Tytuł                                           | Opis                                          | Obszar ryzyko (na przykład usługi, części lub SDK) | Klaster typu (na przykład Spark, Hadoop, HBase lub Burza) | JIRA (jeśli dotyczy) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Wzbudzają wersja aktualizacji i inne poprawki | Ta wersja aktualizuje wersji Spark w klastrze HDInsight 1.6.1 i naprawia innych usterek| Usługa    | Spark| N/D!

## <a name="notes-for-04112016-release-of-hdinsight"></a>Informacje o wersji 2016-04-11 HDInsight

Numery pełnej wersji dla klastrów HDInsight wdrożony w tej wersji:

* Usługa HDInsight (Windows) 2.1.10.889.2191206 (HDP 1.3.12.0-01795 — bez zmian)
* Usługa HDInsight (Windows) 3.0.6.889.2191206 (HDP 2.0.13.0-2117 — bez zmian)
* Usługa HDInsight (Windows) 3.1.4.889.2191206 (HDP 2.1.15.0-2374 — bez zmian)
* Usługa HDInsight (Windows) 3.2.7.889.2191206 (HDP 2.2.9.1-10)
* Usługa HDInsight (Windows) 3.3.0.889.2191206 (HDP 2.3.3.1-16 — bez zmian)
* Usługa HDInsight (Linux) 3.2.1000.0.7339916 (HDP 2.2.9.1-10)
* Usługa HDInsight (Linux) 3.3.1000.0.7339916 (HDP 2.3.3.1-16)
* Usługa HDInsight (Linux) 3.4.1000.0.7338911 (HDP 2.4.1.1-3)
* ZESTAW SDK 1.5.8

Ta wersja zawiera następujące aktualizacje.

| Tytuł                                           | Opis                                          | Obszar ryzyko (na przykład usługi, części lub SDK) | Klaster typu (na przykład Hadoop, HBase lub Burza) | JIRA (jeśli dotyczy) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Uaktualnianie metastore niestandardowe problemy dla HDI 3.4 | Tworzenie klaster przestaną działać użycie niestandardowej metastore, która wcześniej została użyta w funkcjonalną wersję innej klaster HDInsight. Było ze względu na błąd uaktualniania skrypt, który ma teraz stałe| Tworzenie klaster    | Wszystkie | N/D!
| Odzyskiwanie po awarii Livy | Zapewnia elastyczność stan zadania dla dowolnego zadania przesłane przez Livy | Niezawodność | Spark w systemie Linux| N/D!
| Zawartość Jupyter HA | Umożliwia zapisywanie i ładowanie Jupyter zawartości notesu do i z konta magazynu skojarzone z klastrem. Aby uzyskać więcej informacji zobacz [jądra dostępne dla Jupyter notesów](hdinsight-apache-spark-jupyter-notebook-kernels.md).| Notesy | Spark w systemie Linux| N/D!
| Usuwanie hiveContext w notesach Jupter | Używanie `%%sql` Magiczna zamiast `%%hive` magiczną. SqlContext odpowiada hiveContext. Aby uzyskać więcej informacji zobacz [jądra dostępne dla Jupyter notesów](hdinsight-apache-spark-jupyter-notebook-kernels.md)| Notesy    | Klastrów Spark w systemie Linux| N/D!
| Oczekiwany starszych wersji Spark | Starsza wersja Spark 1.3.1 zostaną usunięte z usługi na 5-31 | Usługa | Klastrów Spark w systemie Windows | N/D!

## <a name="notes-for-03292016-release-of-hdinsight"></a>Informacje o wersji 2016-03-29 HDInsight

Numery pełnej wersji dla klastrów HDInsight wdrożony w tej wersji:

* Usługa HDInsight (Windows) 2.1.10.875.2159884 (HDP 1.3.12.0-01795 — bez zmian)
* Usługa HDInsight (Windows) 3.0.6.875.2159884 (HDP 2.0.13.0-2117 — bez zmian)
* Usługa HDInsight (Windows) 3.1.4.875.2159884 (HDP 2.1.15.0-2374 — bez zmian)
* Usługa HDInsight (Windows) 3.2.7.875.2159884 (HDP 2.2.9.1-7 — bez zmian)
* Usługa HDInsight (Windows) 3.3.0.875.2159884 (HDP 2.3.3.1-16)
* Usługa HDInsight (Linux) 3.2.1000.0.7193255 (HDP 2.2.9.1-8 — bez zmian)
* Usługa HDInsight (Linux) 3.3.1000.0.7193255 (HDP 2.3.3.1-7 — bez zmian)
* Usługa HDInsight (Linux) 3.4.1000.0.7195842 (HDP 2.4.1.0-327)
* ZESTAW SDK 1.5.8

Ta wersja zawiera następujące aktualizacje.

| Tytuł                                           | Opis                                          | Obszar ryzyko (na przykład usługi, części lub SDK) | Klaster typu (na przykład Hadoop, HBase lub Burza) | JIRA (jeśli dotyczy) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Dodano wersji HDInsight 3.4 i zaktualizowanych wersji HDP dla wszystkich klastrów HDInsight | W tej wersji możemy dodano v3.4 HDInsight (w oparciu o 2,4 HDP), a także zaktualizowany innych wersji HDP. 2,4 HDP informacje o wersji są dostępne [w tym miejscu](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html) i więcej informacji na temat HDInsight wersje mogą być znaleziony [w tym miejscu](hdinsight-component-versioning.md).| Usługa    | Wszystkich klastrów Linux| N/D!
| Usługa HDInsight Premium | Usługa HDInsight jest teraz dostępna w dwóch kategoriach - Standard i Premium. Usługa HDInsight Premium jest obecnie w wersji Preview i dostępne tylko w przypadku Hadoop i Spark klastrów Linux. Aby uzyskać więcej informacji, zobacz [poniżej](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium).| Usługa    | Hadoop i Spark w systemie Linux| N/D!
| Serwer Microsoft R | HDInsight Premium zawiera Microsoft R serwera, który może zostać dołączony do klastrów Hadoop i Spark na Linux. Aby uzyskać więcej informacji, zobacz [Omówienie R Server na HDInsight](hdinsight-hadoop-r-server-overview.md).| Usługa    | Hadoop i Spark w systemie Linux| N/D!
| Spark 1.6.0 | Klastrów HDInsight 3.4 zawiera teraz Spark 1.6.0| Usługa    | Klastrów Spark w systemie Linux| N/D!
| Ulepszenia notesu Jupyter | Jupyter notesy udostępnione do klastrów Spark zapewniają dodatkowe Spark jądra. Zawierają także ulepszenia, takie jak wykorzystanie %% Magia, automatyczne wizualizacji i integracji z bibliotekami wizualizacji Python (na przykład matplotlib). Aby uzyskać więcej informacji zobacz [jądra dostępne dla Jupyter notesów](hdinsight-apache-spark-jupyter-notebook-kernels.md). | Usługa | Klastrów Spark w systemie Linux | N/D!

## <a name="notes-for-03222016-release-of-hdinsight"></a>Informacje o wersji 2016-03-22 HDInsight

Numery pełnej wersji dla klastrów HDInsight wdrożony w tej wersji:

* Usługa HDInsight (Windows) 2.1.10.875.2159884 (HDP 1.3.12.0-01795 — bez zmian)
* Usługa HDInsight (Windows) 3.0.6.875.2159884 (HDP 2.0.13.0-2117 — bez zmian)
* Usługa HDInsight (Windows) 3.1.4.875.2159884 (HDP 2.1.15.0-2374 — bez zmian)
* Usługa HDInsight (Windows) 3.2.7.875.2159884 (HDP 2.2.9.1-7 — bez zmian)
* Usługa HDInsight (Windows) 3.3.0.875.2159884 (HDP 2.3.3.1-16)
* Usługa HDInsight (Linux) 3.2.1000.0.7193255 (HDP 2.2.9.1-8 — bez zmian)
* Usługa HDInsight (Linux) 3.3.1000.0.7193255 (HDP 2.3.3.1-7 — bez zmian)
* ZESTAW SDK 1.5.8

Ta wersja zawiera następujące aktualizacje.

| Tytuł                                           | Opis                                          | Obszar ryzyko (na przykład usługi, części lub SDK) | Klaster typu (na przykład Hadoop, HBase lub Burza) | JIRA (jeśli dotyczy) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Zaktualizowane wersje HDInsight dla wszystkich klastrów HDInsight | W tej wersji aktualizacji wersji HDInsight dla wszystkich klastrów HDInsight| Usługa    | Wszystkie| N/D!


## <a name="notes-for-03102016-release-of-hdinsight"></a>Informacje o wersji 2016-03-10 HDInsight

Numery pełnej wersji dla klastrów HDInsight wdrożony w tej wersji:

* Usługa HDInsight (Windows) 2.1.10.859.2123216 (HDP 1.3.12.0-01795 — bez zmian)
* Usługa HDInsight (Windows) 3.0.6.859.2123216 (HDP 2.0.13.0-2117 — bez zmian)
* Usługa HDInsight (Windows) 3.1.4.859.2123216 (HDP 2.1.15.0-2374 — bez zmian)
* Usługa HDInsight (Windows) 3.2.7.859.2123216 (HDP 2.2.9.1-7)
* Usługa HDInsight (Windows) 3.3.0.859.2123216 (HDP 2.3.3.1-5 — bez zmian)
* Usługa HDInsight (Linux) 3.2.1000.7076817 (HDP 2.2.9.1-8)
* Usługa HDInsight (Linux) 3.3.1000.7076817 (HDP 2.3.3.1-7)
* ZESTAW SDK 1.5.8

Ta wersja zawiera następujące aktualizacje.

| Tytuł                                           | Opis                                          | Obszar ryzyko (na przykład usługi, części lub SDK) | Klaster typu (na przykład Hadoop, HBase lub Burza) | JIRA (jeśli dotyczy) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Zaktualizowane wersje HDInsight dla wszystkich klastrów HDInsight | W tej wersji aktualizacji wersji HDInsight dla wszystkich klastrów HDInsight| Usługa    | Wszystkie| N/D!

## <a name="notes-for-01272016-release-of-hdinsight"></a>Informacje o wersji 2016-01-27 HDInsight

Numery pełnej wersji dla klastrów HDInsight wdrożony w tej wersji:

* Usługa HDInsight (Windows) 2.1.10.817.2028315 (HDP 1.3.12.0-01795 — bez zmian)
* Usługa HDInsight (Windows) 3.0.6.817.2028315 (HDP 2.0.13.0-2117 — bez zmian)
* Usługa HDInsight (Windows) 3.1.4.817.2028315 (HDP 2.1.15.0-2374 — bez zmian)
* Usługa HDInsight (Windows) 3.2.7.817.2028315 (HDP 2.2.9.1-1)
* Usługa HDInsight (Windows) 3.3.0.817.2028315 (HDP 2.3.3.1-5 — bez zmian)
* Usługa HDInsight (Linux) 3.2.1000.4072335 (HDP 2.2.9.1-1)
* Usługa HDInsight (Linux) 3.3.1000.4072335 (HDP 2.3.3.1-1)
* ZESTAW SDK 1.5.8

Ta wersja zawiera następujące aktualizacje.

| Tytuł                                           | Opis                                          | Obszar ryzyko (na przykład usługi, części lub SDK) | Klaster typu (na przykład Hadoop, HBase lub Burza) | JIRA (jeśli dotyczy) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Zaktualizowane wersje HDInsight dla wszystkich klastrów HDInsight | W tej wersji aktualizacji wersji HDInsight dla wszystkich klastrów HDInsight| Usługa    | Wszystkie| N/D!

## <a name="notes-for-12022015-release-of-hdinsight"></a>Informacje o wersji 2015-12-02 HDInsight

Numery pełnej wersji dla klastrów HDInsight wdrożony w tej wersji:

* Usługa HDInsight (Windows) 2.1.10.763.1931434 (HDP 1.3.12.0-01795 — bez zmian)
* Usługa HDInsight (Windows) 3.0.6.763.1931434 (HDP 2.0.13.0-2117 — bez zmian)
* Usługa HDInsight (Windows) 3.1.4.763.1931434 (HDP 2.1.15.0-2374 — bez zmian)
* Usługa HDInsight (Windows) 3.2.7.763.1931434 (HDP 2.2.7.1-34 — bez zmian)
* Usługa HDInsight (Windows) 3.3.1000.0 (HDP 2.3.3.1-5)
* Usługa HDInsight (Linux) 3.2.1000.0.6392801 (HDP 2.2.7.1-34 — bez zmian)
* Usługa HDInsight (Linux) 3.3.1000.0 (HDP 2.3.3.0-3039)
* ZESTAW SDK 1.5.8

Ta wersja zawiera następujące aktualizacje.

| Tytuł                                           | Opis                                          | Obszar ryzyko (na przykład usługi, części lub SDK) | Klaster typu (na przykład Hadoop, HBase lub Burza) | JIRA (jeśli dotyczy) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Dodano wersji HDInsight 3.3 i zaktualizowanych wersji HDP dla wszystkich klastrów HDInsight | W tej wersji możemy dodano v3.3 HDInsight (w oparciu o HDP 2.3), a także zaktualizowany innych wersji HDP. HDP 2.3 informacje o wersji są dostępne [w tym miejscu](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html) i więcej informacji na temat HDInsight wersje mogą być znaleziony [w tym miejscu](hdinsight-component-versioning.md).| Usługa    | Wszystkie| N/D!

## <a name="notes-for-11302015-release-of-hdinsight"></a>Informacje o wersji 2015-11-30 HDInsight

Numery pełnej wersji dla klastrów HDInsight wdrożony w tej wersji:

* Usługa HDInsight (Windows) 2.1.10.757.1923908 (HDP 1.3.12.0-01795 — bez zmian)
* Usługa HDInsight (Windows) 3.0.6.757.1923908 (HDP 2.0.13.0-2117 — bez zmian)
* Usługa HDInsight (Windows) 3.1.4.757.1923908 (HDP 2.1.15.0-2374 — bez zmian)
* Usługa HDInsight (Windows) 3.2.7.757.1923908 (HDP 2.2.7.1-34)
* Usługa HDInsight (Linux) 3.2.1000.0.6392801 (HDP 2.2.7.1-34)
* ZESTAW SDK 1.5.8

Ta wersja zawiera następujące aktualizacje.

| Tytuł                                           | Opis                                          | Obszar ryzyko (na przykład usługi, części lub SDK) | Klaster typu (na przykład Hadoop, HBase lub Burza) | JIRA (jeśli dotyczy) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Zaktualizowane wersje HDInsight dla wszystkich klastrów HDInsight i HDP wersji dla klastrów 3,2 HDInsight (Windows i Linux oraz) | W tej wersji zostały zaktualizowane HDInsight i HDP wersji | Usługa    | Wszystkie| N/D!


## <a name="notes-for-10272015-release-of-hdinsight"></a>Informacje o wersji 2015-10-27 HDInsight

Numery pełnej wersji dla klastrów HDInsight wdrożony w tej wersji:

* Usługa HDInsight (Windows) 2.1.10.726.1866228 (HDP 1.3.12.0-01795 — bez zmian)
* Usługa HDInsight (Windows) 3.0.6.726.1866228 (HDP 2.0.13.0-2117 — bez zmian)
* Usługa HDInsight (Windows) 3.1.4.726.1866228 (HDP 2.1.15.0-2374 — bez zmian)
* Usługa HDInsight (Windows) 3.2.7.726.1866228 (HDP 2.2.7.1-33)
* Usługa HDInsight (Linux) 3.2.1000.0.6035701 (HDP 2.2.7.1-33)
* ZESTAW SDK 1.5.8

Ta wersja zawiera następujące aktualizacje.

| Tytuł                                           | Opis                                          | Obszar ryzyko (na przykład usługi, części lub SDK) | Klaster typu (na przykład Hadoop, HBase lub Burza) | JIRA (jeśli dotyczy) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Zaktualizowane wersje HDInsight dla wszystkich klastrów HDInsight (Windows i Linux oraz) | W tej wersji zostały zaktualizowane HDInsight i HDP wersji | Usługa    | Wszystkie| N/D!
| Stały klastrów Spark Jupyter dla systemu Windows z klastrów wielkimi literami | Klastrów, które miały nazw DNS określonych wielkimi literami ma problemy z notesami Jupyter z powodu wyboru żądanie pochodzenia. Poprawka była zmiana nazwy DNS konfiguracji i Jupyter na małe litery. | Usługa    | Usługa HDInsight Spark (Windows)| N/D!


## <a name="notes-for-10202015-release-of-hdinsight"></a>Informacje o wersji 2015-10-20 HDInsight

Numery pełnej wersji dla klastrów HDInsight wdrożony w tej wersji:

* Usługa HDInsight 2.1.10.716.1846990 (Windows) (HDP 1.3.12.0-01795 — bez zmian)
* Usługa HDInsight 3.0.6.716.1846990 (Windows) (HDP 2.0.13.0-2117 — bez zmian)
* Usługa HDInsight 3.1.4.716.1846990 (Windows) (HDP 2.1.16.0-2374)
* Usługa HDInsight 3.2.7.716.1846990 (Windows) (HDP 2.2.7.1-0004)
* Usługa HDInsight 3.2.1000.0.5930166 (Linux) (HDP 2.2.7.1-0004)
* ZESTAW SDK 1.5.8

Ta wersja zawiera następujące aktualizacje.

| Tytuł                                           | Opis                                          | Obszar ryzyko (na przykład usługi, części lub SDK) | Klaster typu (na przykład Hadoop, HBase lub Burza) | JIRA (jeśli dotyczy) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Zmiana HDP 2.2 wersji HDP domyślnej | Wersja domyślna dla klastrów HDInsight systemu Windows zostanie zmieniona na HDP 2.2. Usługa HDInsight wersję 3,2 (HDP 2.2) został powszechnie dostępny w od luty 2015. Ta zmiana tylko przerzucony wersja klastrze domyślne, gdy nie została dokonana jawne zaznaczenia podczas inicjowania obsługi administracyjnej klaster przy użyciu Azure portal, poleceń cmdlet środowiska PowerShell lub zestawu SDK. | Usługa    | Wszystkie| N/D!                  |
|Zmiany w formacie nazwy maszyn wirtualnych do wdrażania wielu HDInsight na klastrów Linux w jednej wirtualnej sieci | Obsługa wdrażania wielu klastrów HDInsight Linux w jednej wirtualnej sieci jest dodawana w tej wersji. W ramach tego formatu nazw maszyn wirtualnych w klastrze zmienił się z headnode\*, workernode\* i zookeepernode\* do hn\*, dół\*i zk\* odpowiednio. Nie jest zaleca, aby zastosować bezpośrednia zależność formatu nazw maszyn wirtualnych, ponieważ jest to mogą ulec zmianie. Użyj "hostname -f" na komputerze lokalnym lub interfejsów API Ambari listy hosts i mapowanie składniki hosts. Można znaleźć więcej informacji na [https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/hosts.md](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/hosts.md) i [https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/host-components.md](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/host-components.md). | Usługa | Usługa HDInsight klastrów na Linux | N/D! |
| Zmiany w konfiguracji | W przypadku klastrów HDInsight 3.1 dostępne są teraz następujące ustawienia: <ul><li>tez.yarn.ATS.Enabled i yarn.log.server.url. Dzięki temu serwer osi czasu aplikacji i serwer dziennika można było używać się dzienników.</li></ul>W przypadku klastrów 3,2 HDInsight zostały zmodyfikowane następujące ustawienia: <ul><li>mapreduce.fileoutputcommitter.Algorithm.Version została ustawiona jako 2. Umożliwia korzystanie z wersji w wersji 2 z FileOutputCommitter.</li></ul> | Usługa | Wszystkie | N/D! |


## <a name="notes-for-09092015-release-of-hdinsight"></a>Informacje o wersji 2015-09-09 HDInsight

Numery pełnej wersji dla klastrów HDInsight wdrożony w tej wersji:

* Usługa HDInsight 2.1.10.675.1768697 (HDP 1.3.12.0-01795 — bez zmian)
* Usługa HDInsight 3.0.6.675.1768697 (HDP 2.0.13.0-2117 — bez zmian)
* Usługa HDInsight 3.1.4.675.1768697 (HDP 2.1.15.0-2334 — bez zmian)
* Usługa HDInsight 3.2.6.675.1768697 (HDP 2.2.6.1-0012 — bez zmian)
* ZESTAW SDK 1.5.8

Ta wersja zawiera następujące aktualizacje.

| Tytuł                                           | Opis                                          | Obszar ryzyko (na przykład usługi, części lub SDK) | Klaster typu (na przykład Hadoop, HBase lub Burza) | JIRA (jeśli dotyczy) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Zaktualizowane wersje HDInsight dla wszystkich klastrów HDInsight | W tej wersji zostały zaktualizowane wersje HDInsight | Usługa    | Wszystkie| N/D!                  |

## <a name="notes-for-07312015-release-of-hdinsight"></a>Informacje o wersji 2015-07-31 HDInsight

Numery pełnej wersji dla klastrów HDInsight wdrożony w tej wersji:

* Usługa HDInsight 2.1.10.640.1695824 (HDP 1.3.12.0-01795 — bez zmian)
* Usługa HDInsight 3.0.6.640.1695824 (HDP 2.0.13.0-2117 — bez zmian)
* Usługa HDInsight 3.1.4.640.1695824 (HDP 2.1.15.0-2334 — bez zmian)
* Usługa HDInsight 3.2.6.640.1695824 (HDP 2.2.6.1-0012 — bez zmian)
* ZESTAW SDK 1.5.8

Ta wersja zawiera następujące aktualizacje.

| Tytuł                                           | Opis                                          | Obszar ryzyko (na przykład usługi, części lub SDK) | Klaster typu (na przykład Hadoop, HBase lub Burza) | JIRA (jeśli dotyczy) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Naprawianie Spark klaster węzeł ponownie obrazów w przepływu pracy | Stałe błąd powodująca Spark węzłach do nie odzyskiwanie po ponownie utworzyć obraz | Usługa    | Spark| N/D!                  |


## <a name="notes-for-07312015-release-of-hdinsight"></a>Informacje o wersji 2015-07-31 HDInsight

Numery pełnej wersji dla klastrów HDInsight wdrożony w tej wersji:

* Usługa HDInsight 2.1.10.635.1684502 (HDP 1.3.12.0-01795 — bez zmian)
* Usługa HDInsight 3.0.6.635.1684502 (HDP 2.0.13.0-2117 — bez zmian)
* Usługa HDInsight 3.1.4.635.1684502 (HDP 2.1.15.0-2334 — bez zmian)
* Usługa HDInsight 3.2.6.635.1684502 (HDP 2.2.6.1-0012 — bez zmian)
* ZESTAW SDK 1.5.8

Ta wersja zawiera następujące aktualizacje.

| Tytuł                                           | Opis                                          | Obszar ryzyko (na przykład usługi, części lub SDK) | Klaster typu (na przykład Hadoop, HBase lub Burza) | JIRA (jeśli dotyczy) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Zaktualizowane wersje HDInsight dla wszystkich klastrów HDInsight | W tej wersji zostały zaktualizowane wersje HDInsight | Usługa    | Wszystkie| N/D!                  |


## <a name="notes-for-07072015-release-of-hdinsight"></a>Informacje o wersji 2015-07-07 HDInsight

Numery pełnej wersji dla klastrów HDInsight wdrożony w tej wersji:

* Usługa HDInsight 2.1.10.610.1630216 (HDP 1.3.12.0-01795 — bez zmian)
* Usługa HDInsight 3.0.6.610.1630216 (HDP 2.0.13.0-2117 — bez zmian)
* Usługa HDInsight 3.1.4.610.1630216 (HDP 2.1.15.0-2334 — bez zmian)
* Usługa HDInsight 3.2.4.610.1630216 (HDP 2.2.6.1-0012)
* ZESTAW SDK 1.5.8


Ta wersja zawiera następujące aktualizacje.

| Tytuł                                           | Opis                                          | Obszar ryzyko (na przykład usługi, części lub SDK) | Klaster typu (na przykład Hadoop, HBase lub Burza) | JIRA (jeśli dotyczy) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Zaktualizowane wersje HDP dla klastrów 3,2 HDInsight | W tej wersji HDInsight 3,2 wdraża HDP 2.2.6.1-0012 | Usługa    | Wszystkie                                                 | N/D!                  |


## <a name="notes-for-06262015-release-of-hdinsight"></a>Informacje o wersji 2015-06-26 HDInsight

Numery pełnej wersji dla klastrów HDInsight wdrożony w tej wersji:

* Usługa HDInsight 2.1.10.601.1610731 (HDP 1.3.12.0-01795 — bez zmian)
* Usługa HDInsight 3.0.6.601.1610731 (HDP 2.0.13.0-2117 — bez zmian)
* Usługa HDInsight 3.1.4.601.1610731 (HDP 2.1.15.0-2334 — bez zmian)
* Usługa HDInsight 3.2.4.601.1610731 (HDP 2.2.6.1-0011)
* ZESTAW SDK 1.5.8


Ta wersja zawiera następujące aktualizacje.

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Obszar ryzyko (na przykład usługi, części lub SDK)</p></th>
<th>Klaster typu (na przykład Hadoop, HBase lub Burza)</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>


<tr>
<td>Zaktualizowane wersje HDP dla klastrów 3,2 HDInsight</td>
<td>W tej wersji HDInsight 3,2 wdraża HDP punktach 2.2.6.1</td>
<td>Usługa</td>
<td>Wszystkie</td>
<td>N/D!</td>
</tr>

</table>

## <a name="notes-for-06182015-release-of-hdinsight"></a>Informacje o wersji 2015-06-18 HDInsight

Numery pełnej wersji dla klastrów HDInsight wdrożony w tej wersji:

* Usługa HDInsight 2.1.10.596.1601657 (HDP 1.3.12.0-01795 — bez zmian)
* Usługa HDInsight 3.0.6.596.1601657 (HDP 2.0.13.0-2117 — bez zmian)
* Usługa HDInsight 3.1.4.596.1601657 (HDP 2.1.15.0-2334)
* Usługa HDInsight 3.2.4.596.1601657 (HDP 2.2.6.1-0002)
* ZESTAW SDK 1.5.8


Ta wersja zawiera następujące aktualizacje.

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Obszar ryzyko (na przykład usługi, części lub SDK)</p></th>
<th>Klaster typu (na przykład Hadoop, HBase lub Burza)</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>


<tr>
<td>Dodatkowe porty HTTPS otwarty</td>
<td>Usługa w chmurze zostanie otwarty 5 portów 8001 do 8005 w klastrze np. na https://<clustername>.azurehdinsight.net:8001/. Żądania te adresy URL są uwierzytelnianie za pomocą ten sam mechanizm hasło uwierzytelnianie podstawowe jako na porcie 443. Te porty powiązać ten sam port na aktywne headnode. Akcje skryptu można nawiązać nasłuchują na następujące porty headnode i przekazać je do znajdującego się poza klastrem działu pomocy technicznej.</td>
<td>Usługa w chmurze</td>
<td>Wszystkie</td>
<td>N/D!</td>
</tr>

<tr>
<td>Przejściowymi problem losowo MapReduce 3,2 HDInsight</td>
<td>Rozwiązanie problemu warunku wyścigowe rzadkich, przerw w losowo zmniejszyć mapy na dużych klastrów uzyskując zdarzać zadania błędy. Aby uzyskać więcej informacji, zobacz <a href="https://issues.apache.org/jira/browse/MAPREDUCE-6361" target="_blank">MAPREDUCE 6361</a> .</td>
<td>Podstawowe Hadoop</td>
<td>Wszystkie</td>
<td><a href="https://issues.apache.org/jira/browse/MAPREDUCE-6361" target="_blank">MAPREDUCE 6361</a></td>
</tr>

<tr>
<td>Przechodzenie do najnowszej Java Azure SDK 2.2 dla HDInsight 3,2</td>
<td>Przenoszony do najnowszej wersji pakietu Azure SDK Java używane przez sterownik WASB. Najnowsze SDK zawiera kilka poprawek i informacje o wersji dla tego samego są dostępne na https://github.com/Azure/azure-storage-java/blob/master/ChangeLog.txt.</td>
<td>Podstawowe Hadoop</td>
<td>Wszystkie</td>
<td><a href="https://issues.apache.org/jira/browse/HADOOP-11959" target="_blank">HADOOP 11959</a></td>
</tr>

<tr>
<td>Przechodzenie do HDP 2.1.15 dla klastrów HDInsight 3.1</td>
<td>Informacje o wersji Hortonworks wersji są dostępne <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.15-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.15.html" target="_blank">w tym miejscu</a>.</td>
<td>HDP</td>
<td>Wszystkie</td>
<td>N/D!</td>
</tr>

</table>

## <a name="notes-for-06042015-release-of-hdinsight"></a>Informacje o wersji 2015-06-04 HDInsight

Numery pełnej wersji dla klastrów HDInsight wdrożony w tej wersji:

* Usługa HDInsight 2.1.10.583.1575584 (HDP 1.3.12.0-01795 — bez zmian)
* Usługa HDInsight 3.0.6.583.1575584 (HDP 2.0.13.0-2117 — bez zmian)
* Usługa HDInsight 3.1.3.583.1575584 (HDP 2.1.12.1-0003 — bez zmian)
* Usługa HDInsight 3.2.4.583.1575584 (HDP 2.2.6.1-1)
* ZESTAW SDK 1.5.8


Ta wersja zawiera następujące aktualizacje.

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Obszar ryzyko (na przykład usługi, części lub SDK)</p></th>
<th>Klaster typu (na przykład Hadoop, HBase lub Burza)</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>


<tr>
<td>Naprawianie 502 problemu Nieprawidłowa brama dla klastrów Burza</td>
<td>Ta wersja rozwiązuje błąd wpływu przesyłania zadania interfejsu API, które spowodowało działać po ponownym uruchomieniu witrynie sieci Web.</td>
<td>Usługa</td>
<td>Burza</td>
<td>N/D!</td>
</tr>

</table>

## <a name="notes-for-06012015-release-of-hdinsight"></a>Informacje o wersji 2015-06-01 HDInsight

Numery pełnej wersji dla klastrów HDInsight wdrożony w tej wersji:

* Usługa HDInsight 2.1.10.577.1563827 (HDP 1.3.12.0-01795 — bez zmian)
* Usługa HDInsight 3.0.6.577.1563827 (HDP 2.0.13.0-2117 — bez zmian)
* Usługa HDInsight 3.1.3.577.1563827 (HDP 2.1.12.1-0003 — bez zmian))
* Usługa HDInsight 3.2.4.577.1563827 (HDP 2.2.6.0-2800 — bez zmian)
* ZESTAW SDK 1.5.8


Ta wersja zawiera następujące aktualizacje.

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Obszar ryzyko (na przykład usługi, części lub SDK)</p></th>
<th>Klaster typu (na przykład Hadoop, HBase lub Burza)</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>


<tr>
<td>Różne poprawki</td>
<td>Ta wersja rozwiązuje błędy związane z klaster inicjowania obsługi administracyjnej.</td>
<td>Usługa</td>
<td>Wszystkie typy klaster</td>
<td>N/D!</td>
</tr>

</table>

## <a name="notes-for-05272015-release-of-hdinsight"></a>Informacje o wersji 2015-05-27 HDInsight

Numery pełnej wersji dla klastrów HDInsight wdrożony w tej wersji:

* Usługa HDInsight 3.2.4.570.1554102 (HDP 2.2.6.0-2800)
* Inne wersje klaster i SDK nie są wdrażane w ramach tej wersji.


Ta wersja zawiera następujące aktualizacje.

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Obszar ryzyko (na przykład usługi, części lub SDK)</p></th>
<th>Klaster typu (na przykład Hadoop, HBase lub Burza)</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>


<tr>
<td>Aktualizacja HDP 2.2</td>
<td>Ta wersja 3,2 HDInsight zawiera HDP 2.2.6 i udostępnia kilka ważnych poprawki HDInsight. Informacje o pełnej wersji jest dostępna w <a href="http://dev.hortonworks.com.s3.amazonaws.com/HDPDocuments/HDP2/HDP-2.2.6/HDP_RelNotes_v226/index.html">HDP 2.2.6 wersji</a>.</td>
<td>HDP</td>
<td>Wszystkie typy klaster</td>
<td>N/D!</td>
</tr>

<tr>
<td>Zmienianie domyślnej przędzy kontenera pamięci konfiguracji</td>
<td>W tej aktualizacji dostępną pamięć do kontenerów PRZĘDZY (yarn.nodemanager.resource.memory mb i yarn.scheduler.maximum alokacji mb), domyślne uruchomione przez Menedżera węzeł zostaje zwiększona 5632 MB. Wcześniej to zostało ograniczone do 4608MB, ale według różnych uruchomionych zadań, nowe wartości musisz oferować lepiej niezawodności i wydajności do większości zadań, w związku z tym jest lepsza domyślne. Normalny, jeśli użytkownik ma krytyczne zależności od tej konfiguracji pamięci, skonfiguruj ją jawnie podczas tworzenia klaster.</td>
<td>HDP</td>
<td>Wszystkie typy klaster</td>
<td>N/D!</td>
</tr>

<tr>
<td>Wartość domyślna spójności konfiguracji HBase i klastrów Burza</td>
<td>Ta aktualizacja przywraca Hbase i Burza klastrów, aby użyć tych samych wartości podawać PRZĘDZY jako klastrów Hadoop. Jest to zrobić dla spójności przez wszystkie typy klaster.</td>
<td>HDP</td>
<td>HBase, Burza</td>
<td>N/D!</td>
</tr>

</table>

## <a name="notes-for-05202015-release-of-hdinsight"></a>Informacje o wersji 2015-05-20 HDInsight

Numery pełnej wersji dla klastrów HDInsight wdrożony w tej wersji:

* Usługa HDInsight 2.1.10.564.1542093 (HDP 1.3.12.0-01795 — bez zmian)
* Usługa HDInsight 3.0.6.564.1542093 (HDP 2.0.13.0-2117 — bez zmian)
* Usługa HDInsight 3.1.3.564.1542093 (HDP 2.1.12.1-0003)
* Usługa HDInsight 3.2.4.564.1542093 (HDP 2.2.4.6-2)
* ZESTAW SDK 1.5.8

Ta wersja zawiera następujące aktualizacje.

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Obszar ryzyko (na przykład usługi, części lub SDK)</p></th>
<th>Klaster typu (na przykład Hadoop, HBase lub Burza)</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>


<tr>
<td>Obsługa EventHub SCP.NET</td>
<td>Pakiety zaktualizowanych klaster dla Burza HDInsight przenoszenie nowych funkcji SCP.NET. Teraz uzyskuje dostęp do nowych interfejsów API w Konstruktorze topologii, które ułatwiają za pomocą EventHubSpout lub Java Spouts. Należy zaktualizować klienta SCP.NET SDK pracę nowych klastrów umów zostały zaktualizowane. Szczegółowe informacje na temat nowych interfejsów API, zastosowania i udostępniania notatek (w tym poprawki) można znaleźć w pliku readme zawarte w pakiecie nuget SCP.NET.</td>
<td>Narzędzie w PORÓWNANIU z</td>
<td>Klastrów 3,2 HDInsight Burza</td>
<td>N/D!</td>
</tr>

<tr>
<td>Aktualizacja sterownika JDBC</td>
<td>Aktualizacja sterownika do wersji SQL Server obsługiwany sqljdbc_4.1.5605.100.</td>
<td>Metastore</td>
<td>Wszystkie</td>
<td>N/D!</td>
</tr>
</table>

## <a name="notes-for-04272015-release-of-hdinsight"></a>Informacje o wersji 2015-04-27 HDInsight

Numery pełnej wersji dla klastrów HDInsight wdrożony w tej wersji:

* Usługa HDInsight 2.1.10.537.1486660 (HDP 1.3.12.0-01795 — bez zmian)
* Usługa HDInsight 3.0.6.537.1486660 (HDP 2.0.13.0-2117 — bez zmian)
* Usługa HDInsight 3.1.3.537.1486660 (HDP 2.1.12.0-2329 — bez zmian)
* Usługa HDInsight 3.2.3.537.1486660 (HDP 2.2.2.2-4)
* ZESTAW SDK 1.5.8

Ta wersja zawiera następujące aktualizacje.

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Obszar ryzyko (na przykład usługi, części lub SDK)</p></th>
<th>Klaster typu (na przykład Hadoop, HBase lub Burza)</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>


<tr>
<td>Naprawianie współzależności DLL</td>
<td>Usuwa HDInsight zależność struktury testów jednostkowych.</td>
<td>ZESTAW SDK</td>
<td>Hadoop</td>
<td>N/D!</td>
</tr>

<tr>
<td>Poprawka błędu dla warunku wyścigowe</td>
<td>Klaster Utwórz żądanie teraz czeka na żądanie położenie są akceptowane przed ankieta stanu</td>
<td>ZESTAW SDK</td>
<td>Hadoop</td>
<td>N/D!</td>
</tr>
</table>

## <a name="notes-for-04142015-release-of-hdinsight"></a>Informacje o wersji 2015-04-14 HDInsight

Numery pełnej wersji dla klastrów HDInsight wdrożony w tej wersji:

* Usługa HDInsight 2.1.10.521.1453250 (HDP 1.3.12.0-01795 — bez zmian)
* Usługa HDInsight 3.0.6.521.1453250 (HDP 2.0.13.0-2117 — bez zmian)
* Usługa HDInsight 3.1.3.521.1453250 (HDP 2.1.12.0-2329 — bez zmian)
* Usługa HDInsight 3.2.3.525.1459730 (HDP 2.2.2.2-2)
* ZESTAW SDK 1.5.6

Ta wersja zawiera następujące aktualizacje.

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Obszar ryzyko (na przykład usługi, części lub SDK)</p></th>
<th>Klaster typu (na przykład Hadoop, HBase lub Burza)</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>


<tr>
<td>Tez poprawki</td>
<td>Rozwiązania dla Apache TEZ 2214 i TEZ 1923 znajdują się w tej wersji HDI 3,2. Potrzebne są one specjalnie dla niektórych kwerend gałęzi na Tez, które wymagają losowo dużo danych.
</td>
<td>HDP</td>
<td>Hadoop</td>
<td><a href="https://issues.apache.org/jira/browse/TEZ-2214">TEZ 2214</a></br><a href="https://issues.apache.org/jira/browse/TEZ-1923">TEZ 1923</a></td>
</tr>
</table>

## <a name="notes-for-04062015-release-of-hdinsight"></a>Informacje o wersji 2015-04-06 HDInsight

Numery pełnej wersji dla klastrów HDInsight wdrożony w tej wersji:

* Usługa HDInsight 2.1.10.521.1453250 (HDP 1.3.12.0-01795 — bez zmian)
* Usługa HDInsight 3.0.6.521.1453250 (HDP 2.0.13.0-2117 — bez zmian)
* Usługa HDInsight 3.1.3.521.1453250 (HDP 2.1.12.0-2329 — bez zmian)
* Usługa HDInsight 3.2.3.521.1453250 (HDP 2.2.2.2-1)
* ZESTAW SDK 1.5.6

Ta wersja zawiera następujące aktualizacje.

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Obszar ryzyko (na przykład usługi, części lub SDK)</p></th>
<th>Klaster typu (na przykład Hadoop, HBase lub Burza)</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>


<tr>
<td>Usługa HDInsight .NET SDK 1.5.6</td>
<td>Aktualizacje, aby usunąć niektóre wewnętrznych klasy dla HDInsight w systemie Linux.</td>
<td>ZESTAW SDK</td>
<td>Hadoop</td>
<td>N/D!</td>
</tr>

<tr>
<td>Biblioteka Avro 1.5.6</td>
<td>Dodano <b>KnownTypeAttribute</b> metody <b>GetAllKnownTypes</b>. Stały NullReferenceException typu ma wartość null dla metody GetAllKnownTypes.</td>
<td>ZESTAW SDK</td>
<td>Hadoop</td>
<td>N/D!</td>
</tr>

<tr>
<td>Poprawki</td>
<td>Różne poprawki z usługą</td>
<td>Usługa</td>
<td>Wszystkie</td>
<td>N/D!</td>
</tr>

</table>
<br>

## <a name="notes-for-04012015-release-of-hdinsight"></a>Informacje o wersji 2015-04-01 HDInsight

Numery pełnej wersji dla klastrów HDInsight wdrożony w tej wersji:

* Usługa HDInsight 2.1.10.513.1431705 (HDP 1.3.12.0-01795)
* Usługa HDInsight 3.0.6.513.1431705 (HDP 2.0.13.0-2117)
* Usługa HDInsight 3.1.3.513.1431705 (HDP 2.1.12.0-2329)
* Usługa HDInsight 3.2.3.513.1431705 (HDP 2.2.2.1-2600)
* ZESTAW SDK 1.5.5

Ta wersja zawiera następujące aktualizacje.

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Obszar ryzyko (na przykład usługi, części lub SDK)</p></th>
<th>Klaster typu (na przykład Hadoop, HBase lub Burza)</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>


<tr>
<td>Możliwość, aby włączyć lub wyłączyć zdalnego pulpitu poświadczenia na klastrów systemu Windows za pośrednictwem .NET SDK</td>
<td>Programistyczne obsługę włączania lub wyłączania RDP poświadczenia na klastrów systemu Windows.</td>
<td>ZESTAW SDK</td>
<td>Wszystkie</td>
<td>N/D!</td>
</tr>

<tr>
<td>Możliwość włączania pulpitu zdalnego poświadczenia klastrów podczas ich obsługi administracyjnej</td>
<td>Obsługa programowych Włączanie poświadczeń pulpitu zdalnego tworzenia klaster. Spowoduje to usunięcie proces dwuetapowy najpierw inicjowania obsługi administracyjnej klaster, a następnie włączać pulpitu zdalnego.</td>
<td>ZESTAW SDK</td>
<td>Wszystkie</td>
<td>N/D!</td>
</tr>

<tr>
<td>Uaktualnione Python do 2.7.8</td>
<td>Uaktualnione Python na klastrów HDInsight do Python 2.7.8, zawierający niektóre ważne zabezpieczeń rozwiązania HDInsight wersji 2.1, 3.0, 3.1 i 3,2</td>
<td>Usługa</td>
<td>Wszystkie</td>
<td>N/D!</td>
</tr>

<tr>
<td>Zmiana konfiguracji PRZĘDZY</td>
<td>Zmienione PRZĘDZY konfiguracji yarn.resourcemanager.max wykonane aplikacje do 1000 dla wszystkich typów klaster HDInsight wersji 3.1 i 3,2. Ta wartość kontroluje tylko na liście aplikacji wykonanych w Interfejsie użytkownika PRZĘDZY. Aby uzyskać informacje na temat aplikacji, które zostały przesłane przed na liście wyświetlane w interfejsie użytkownika aplikacji, można bezpośrednio przejdź na serwerze historii.</td>
<td>PRZĘDZA</td>
<td>Wszystkie</td>
<td>N/D!</td>
</tr>

<tr>
<td>Zmienianie rozmiaru węzłów w klastrze HBase</td>
<td>Klastrów HBase teraz zezwalanie na zmienianie rozmiaru węzły (górę i w dół) dla HDInsight wersji 3.1 i 3,2</td>
<td>Usługa</td>
<td>HBase</td>
<td>N/D!</td>
</tr>

<tr>
<td>Uaktualnianie JDBC</td>
<td>Sterownik SQL JDBC jest uaktualniany do wersji sqljdbc_4.0.2206.100 dla HDInsight wersji 3,2. Ta wersja zawiera ważne zabezpieczeń.</td>
<td>HDP</td>
<td>Wszystkie</td>
<td>N/D!</td>
</tr>

<tr>
<td>Aktualizacja konfiguracji maszyny wirtualnej Java</td>
<td>Zaktualizowane maszyny wirtualnej Java konfiguracji networkaddress.cache.ttl do 300 sekund z domyślnej wartości -1 dla HDInsight wersji 3.1 i 3,2. Konfiguracja steruje zasady buforowania dla wyszukiwania pomyślnego nazw z usługi nazw. Rozwiązuje ten błąd związany z wzrostu i zmniejszanie klastrów HBase.</td>
<td>Usługa</td>
<td>HBase</td>
<td>N/D!</td>
</tr>

<tr>
<td>Uaktualnianie do magazynowania Azure SDK dla języka Java 2.0</td>
<td>Usługa HDInsight wersji 3,2 jest uaktualniany do najnowszej wersji pakietu SDK miejsca do magazynowania Azure za pomocą języka Java. Ta strona zawiera kilka ważnych poprawki w bieżącym 0.6.0 wersji.</td>
<td>HDP</td>
<td>Wszystkie</td>
<td>N/D!</td>
</tr>

<tr>
<td>Uaktualnienie do kodu źródłowego Apache WASB</td>
<td>Usługa HDInsight wersji 3,2 teraz zastosowania najnowsze kodu sterownik systemu plików WASB z Apache Hadoop. W przypadku tej zmiany sterownik WASB teraz jest dostarczana w osobnym słoik. To jest czysto zmiany opakowania i nie zawiera wszelkie zmiany działanie sterownika WASB. Nazwa tego pliku SŁOIK jest hadoop-azure-2.6.0.jar.</td>
<td>HDP</td>
<td>Wszystkie</td>
<td>N/D!</td>
</tr>

<tr>
<td>JAR aktualizacji nazwy pliku w 3,2 HDInsight</td>
<td>Aktualizację do wersji 3,2 HDInsight zawiera kilka poprawki, a uaktualniono kilka wewnętrznych słoików spakowany jako część HDP. Należy zauważyć, że te pliki JAR wewnętrznych pakietu HDP, a nie do bezpośredniego użycia przez aplikacje klienta. Aplikacje należy pakiet własną wersję słoików tak, aby uaktualnienie słoików wewnętrznych HDP nie podziału aplikacji klienta.</td>
<td>HDP</td>
<td>Wszystkie</td>
<td>N/D!</td>
</tr>

</table>
<br>

## <a name="notes-for-03032015-release-of-hdinsight"></a>Informacje o wersji 2015-03-03 HDInsight

Numery pełnej wersji dla klastrów HDInsight wdrożony w tej wersji:

* Usługa HDInsight 2.1.10.488.1375841 (HDP 1.3.9.0-01351 — bez zmian)
* Usługa HDInsight 3.0.6.488.1375841 (HDP 2.0.9.0-2097 — bez zmian)
* Usługa HDInsight 3.1.3.488.1375841 (HDP 2.1.10.0-2290 — bez zmian)
* Usługa HDInsight 3.2.3.488.1375841 (HDP-2.2.10.0-2340 — bez zmian)
* Zestaw SDK 1.5.0 (bez zmian)

Ta wersja zawiera następującą aktualizację.

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Obszar ryzyko (na przykład usługi, części lub SDK)</p></th>
<th>Klaster typu (na przykład Hadoop, HBase lub Burza)</th>
<th>JIRA</th>
</tr>


<tr>
<td>Poprawa niezawodność</td>
<td>Wprowadzono poprawki, zezwalające na usługę, aby lepiej się skalować lepszą obciążenia dotyczące tworzenia klaster.</td>
<td>Usługa</td>
<td>Wszystkie</td>
<td>N/D!</td>
</tr>



</table>
<br>

## <a name="notes-for-02182015-release-of-hdinsight"></a>Informacje o wersji 2015-02-18 HDInsight

Numery pełnej wersji dla klastrów HDInsight wdrożony w tej wersji:

* Usługa HDInsight 2.1.10.471.1342507 (HDP 1.3.9.0-01351 — bez zmian)
* Usługa HDInsight 3.0.6.471.1342507 (HDP 2.0.9.0-2097 — bez zmian)
* Usługa HDInsight 3.1.3.471.1342507 (HDP 2.1.10.0-2290 — bez zmian)
* Usługa HDInsight 3.2.3.471.1342507 (HDP-2.2.10.0-2340)
* ZESTAW SDK 1.5.0

Ta wersja zawiera następujące aktualizacje.

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Obszar ryzyko (na przykład usługi, części lub SDK)</p></th>
<th>Klaster typu (na przykład Hadoop, HBase lub Burza)</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>


<tr>
<td>Usługa HDInsight 3,2 klastrów</td>
<td>Hadoop 2.6/HDP2.2 jest dostępna dla klastrów HDInsight 3,2. Zawiera najważniejsze aktualizacje do wszystkich składników Otwórz źródło. Aby uzyskać więcej informacji zobacz Co nowego w HDInsight i <a href ="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.0/HDP_2.2.0_Release_Notes_20141202_version/index.html" target="_blank">HDP 2.2.0.0 informacje o wersji</a>.</td>
<td>Oprogramowanie open source</td>
<td>Wszystkie</td>
<td>N/D!</td>
</tr>

<tr>
<td>Usługa HDinsight w systemie Linux (wersja Preview)</td>
<td>Klastrów mogą być rozmieszczone uruchomionych Ubuntu Linux. Aby uzyskać więcej informacji zobacz wprowadzenie do usługi HDInsight na Linux.</td>
<td>Usługa</td>
<td>Hadoop</td>
<td>N/D!</td>
</tr>

<tr>
<td>Dostępność Burza ogólne</td>
<td>Burza Apache klastrów są powszechnie dostępne. Aby uzyskać więcej informacji Zobacz Rozpoczynanie pracy przy użyciu Burza w HDInsight.</td>
<td>Usługa</td>
<td>Burza</td>
<td>N/D!</td>
</tr>

<tr>
<td>Rozmiary maszyn wirtualnych</td>
<td>Usługa Azure HDInsight jest dostępna na więcej typów maszyn wirtualnych i rozmiarów. Usługa HDInsight mogą korzystać od A2 do rozmiarów A7 wbudowany celach ogólne; Węzły serii D, które funkcji półprzewodnikowe dyski SSD i szybsze procesory 60 procent; rozmiary A8 i A9, które mają InfiniBand obsługę sieci fast oraz.</td>
<td>Usługa</td>
<td>Wszystkie</td>
<td>N/D!</td>
</tr>

<tr>
<td>Klaster skalowania</td>
<td>Możesz zmienić liczby węzłów danych dla uruchomionego klaster HDInsight bez konieczności ponownie utworzyć lub usunąć. Obecnie tylko kwerenda Hadoop i Burza Apache typy klaster mają taką możliwość, ale obsługa HBase Apache typ klaster wkrótce polega na wykonaniu. Aby uzyskać więcej informacji zobacz Zarządzanie HDInsight klastrów.</td>
<td>Usługa</td>
<td>Hadoop, Burza</td>
<td>N/D!</td>
</tr>

<tr>
<td>Narzędzia Visual Studio</td>
<td>Oprócz wykonania narzędzia dla Burza Apache narzędzia dla Apache gałęzi w programie Visual Studio została zaktualizowana do uwzględnienia Kończenie instrukcji, lokalne sprawdzania poprawności i ulepszona obsługa debugowania. Aby uzyskać więcej informacji zobacz wprowadzenie do narzędzia Hadoop HDInsight programu Visual Studio.</td>
<td>Narzędzie</td>
<td>Hadoop</td>
<td>N/D!</td>
</tr>

<tr>
<td>Łącznik Hadoop dla DocumentDB</td>
<td>Łącznik Hadoop dla DocumentDB umożliwiają wykonywanie złożonych agregacji, analizy i operacje nad dokumentami JSON mniej schematu przechowywanych w różnych zbiorach DocumentDB lub wielu kont bazy danych. Aby uzyskać więcej informacji i samouczek Zobacz Hadoop uruchamianie zadania za pomocą DocumentDB i HDInsight.</td>
<td>Usługa</td>
<td>Hadoop</td>
<td>N/D!</td>
</tr>

<tr>
<td>Poprawki</td>
<td>Wprowadzono różne nieznacznej poprawki usługi HDInsight. Bez zmian zachowanie skierowaną klienta są planowane.</td>
<td>Usługa</td>
<td>Wszystkie</td>
<td>N/D!</td>
</tr>

</table>
<br>

## <a name="notes-for-02062015-release-of-hdinsight"></a>Informacje o wersji 2015-02-06 HDInsight

Numery pełnej wersji dla klastrów HDInsight wdrożony w tej wersji:

* Usługa HDInsight 2.1.10.463.1325367 (HDP 1.3.9.0-01351 — bez zmian)
* Usługa HDInsight 3.0.6.463.1325367 (HDP 2.0.9.0-2097 — bez zmian)
* Usługa HDInsight 3.1.2.463.1325367 (HDP 2.1.10.0-2290)
* ZESTAW SDK N/D!

Ta wersja zawiera następujące aktualizacje.

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Obszar ryzyko (na przykład usługi, części lub SDK)</p></th>
<th>Klaster typu (na przykład Hadoop, HBase lub Burza)</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>


<tr>
<td>Poprawki</td>
<td>Wprowadzono różne nieznacznej poprawki usługi HDInsight. Bez zmian zachowanie skierowaną klienta są planowane.</td>
<td>Usługa</td>
<td>Wszystkie</td>
<td>N/D!</td>
</tr>

<tr>
<td>Aktualizacja konserwacji HDP 2.1</td>
<td>Usługa HDInsight 3.1 jest aktualizowana do wdrożenia HDP 2.1.10.0. Aby uzyskać więcej informacji zobacz <a href ="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.10/bk_releasenotes_hdp_2.1/content/ch_relnotes-HDP-2.1.10.html" target="_blank">informacje o wersji HDP-2.1.10</a>. </td>
<td>Oprogramowanie open source</td>
<td>Wszystkie</td>
<td>N/D!</td>
</tr>

<tr>
<td>Aktualizacje binarne HDP</td>
<td>Istnieje kilka plików JAR w HBase, dla których plik nazw zostały zaktualizowane. Te pliki JAR są używane wewnętrznie HBase, więc nie jest planowane konieczne zależność na nazwy plików tych SŁOIK. W tym:
<ul>
<li>./lib/jetty-6.1.26.hwx.JAR</li>
<li>./lib/jetty-sslengine-6.1.26.hwx.JAR</li>
<li>./lib/jetty-Util-6.1.26.hwx.JAR</li>
</ul>
</td>
<td>Oprogramowanie open source</td>
<td>HBase</td>
<td>N/D!</td>
</tr>

</table>
<br>

## <a name="notes-for-1292015-release-of-hdinsight"></a>Informacje o wersji 2015-1-29 HDInsight

Numery pełnej wersji dla klastrów HDInsight wdrożony w tej wersji:

* Usługa HDInsight 2.1.10.455.1309616 (HDP 1.3.9.0-01351 — bez zmian)
* Usługa HDInsight 3.0.6.455.1309616 (HDP 2.0.9.0-2097 — bez zmian)
* Usługa HDInsight 3.1.2.455.1309616 (HDP 2.1.9.0-2196 — bez zmian)
* ZESTAW SDK N/D!

Ta wersja zawiera następującą aktualizację.

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Obszar ryzyko (na przykład usługi, części lub SDK)</p></th>
<th>Klaster typu (na przykład Hadoop, HBase lub Burza)</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>


<tr>

<td>Poprawki</td>
<td>Wprowadzono kilka ważnych poprawki zwiększające niezawodność klastrów HDInsight podczas uaktualniania Azure.</td>
<td>Usługa</td>
<td>Wszystkie</td>
<td>N/D!</td>
</tr>



</table>
<br>

## <a name="notes-for-152015-release-of-hdinsight"></a>Informacje o wersji 2015-1-5 HDInsight

Numery pełnej wersji dla klastrów HDInsight wdrożony w tej wersji:

* Usługa HDInsight 2.1.10.420.1246118 (HDP 1.3.9.0-01351 — bez zmian)
* Usługa HDInsight 3.0.6.420.1246118 (HDP 2.0.9.0-2097 — bez zmian)
* Usługa HDInsight 3.1.2.420.1246118 (HDP 2.1.9.0-2196 — bez zmian)


Ta wersja zawiera następujące aktualizacje.

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Składnik</th>
<th>Typ klaster</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>


<tr>
<td>Próbki do analizy trendu Twitter i zalecenia oparte na Mahout filmu</td>
<td><p>W tej wersji konsolę kwerendy HDInsight zawiera dwie dodatkowe próbki:</p>

<p><b>Analizy trendu Twitter</b><br>
Publiczne interfejsy API dostarczony przez podobnych Twitter witryn są przydatne źródła danych do analizowania i opis trendów popularne. W tym samouczku Dowiedz się, jak za pomocą gałęzi uzyskać listy użytkowników Twitter, które wysłane większość tweety, zawierających określony wyraz. </p>

<p><b>Zalecenie mahout filmu</b><br>
Apache Mahout to komputer Apache Hadoop nauki biblioteki. Mahout zawiera algorytmów przetwarzania danych (na przykład filtrowania, klasyfikacji i klaster). W tym samouczku umożliwia generowanie zalecenia dotyczące filmu na filmy, które miały znajomych aparat rekomendacji.</p></td>
<td>Konsola kwerendy</td>
<td>Hadoop</td>
<td>N/D!</td>
</tr>

<tr>
<td>Zmienić wartość domyślną dla gałęzi konfiguracji: hive.auto.convert.join.noconditionaltask.size</td>
<td><p>Ta konfiguracja rozmiar dotyczy map automatycznie przekonwertowanego sprzężenia. Wartość stanowi sumę rozmiary tabel, które mogą być konwertowane na mapy skrótu, które przebiegają w pamięci. W poprzednich wersji ta wartość podwyższona z wartością domyślną 10 MB do 128 MB. Jednak nową wartość 128 MB był przyczyną zadania niepowodzenie ukończenia braku pamięci. Ta wersja zostanie zmieniona wartość domyślna powrót do 10 MB. Klienci nadal można zastąpić tę wartość podczas tworzenia klaster danej ich kwerend i rozmiarów tabeli. Aby uzyskać więcej informacji na temat tego ustawienia i zastąpić zobacz <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.0.2/ds_Hive/optimize-joins.html#JoinOptimization-OptimizeAutoJoinConversion" target="_blank">Optymalizacja automatyczne dołączanie do konwersji</a> w dokumentacji Hortonworks. </p></td>
<td>Gałąź</td>
<td>Hadoop, Hbase</td>
<td>N/D!</td>
</tr>

</table>
<br>

## <a name="notes-for-12232014-release-of-hdinsight"></a>Informacje o wersji 2014-12-23 HDInsight

Numery pełnej wersji dla klastrów HDInsight wdrożony w tej wersji to:

* Usługa HDInsight 2.1.10.420.1246783 (wersja HDP bez zmian)
* Usługa HDInsight 3.0.6.420.1246783 (wersja HDP bez zmian)
* Usługa HDInsight 3.1.1.420.1246783 (wersja HDP bez zmian)

Ta wersja zawiera następującą aktualizację.

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Składnik</th>
<th>Typ klaster</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>


<tr>
<td>Klaster przerw tworzenia awarii z powodu nadmierne obciążenie</td>
<td><p>Ulepszone algorytm pobierania pakietów HDP podczas tworzenia klaster umożliwia bardziej rozbudowany obsługi błędów z powodu nadmierne obciążenie.</p></td>
<td>Usługa</td>
<td>Hadoop, Hbase, Burza</td>
<td>N/D!</td>
</tr>



</table>
<br>

## <a name="notes-for-12182014-release-of-hdinsight"></a>Informacje o wersji 2014-12-18 HDInsight

Ta wersja zawiera następujące aktualizacji składnika.

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Składnik</th>
<th>Typ klaster</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>

<tr>
<td><a href = "hdinsight-hadoop-customize-cluster.md" target="_blank">Dostosowywanie klaster Avalability ogólne</a></td>
<td><p>Dostosowywanie umożliwia którą można dostosowywać klastrów Azure HDInsight z projektami, które są dostępne z ekosystemie Apache Hadoop. Za pomocą tej nowej funkcji możesz wypróbować i wdrażanie projektów Hadoop Azure HDInsight. Ta opcja jest włączona za pomocą funkcji **Akcja skrypt** , który można zmodyfikować klastrów Hadoop w dowolny sposób za pomocą niestandardowych skryptów. To dostosowanie jest dostępna we wszystkich typach klastrów HDInsight, takich jak Hadoop, HBase i Burza. Wykazać możliwości tej funkcji, możemy mieć opisano proces instalacji popularne <a href = "hdinsight-hadoop-spark-install.md" target="_blank">Spark</a>, <a href = "hdinsight-hadoop-r-scripts.md" target="_blank">R</a>, <a href = "hdinsight-hadoop-solr-install.md" target="_blank">Solr</a>i moduły <a href = "hdinsight-hadoop-giraph-install.md" target="_blank">Giraph</a> . Ta wersja dodaje również możliwości dla klientów określić akcję ich skryptu niestandardowego za pośrednictwem portalu Azure zawiera wskazówki i najważniejsze wskazówki dotyczące sposobu tworzenia skryptu niestandardowego okienka akcji przy użyciu metody Pomocnik i zawiera wskazówki dotyczące metod testowania Akcja skrypt. </p></td>
<td>Dostępność funkcji ogólne</td>
<td>Wszystkie</td>
<td>N/D!</td>
</tr>


</table>
<br>

## <a name="notes-for-12052014-release-of-hdinsight"></a>Informacje o wersji 2014-12-05 HDInsight

Numery pełnej wersji dla klastrów HDInsight wdrożony w tej wersji to:

* Usługa HDInsight 2.1.9.406.1221105 (HDP 1.3.9.0-01351)
* Usługa HDInsight 3.0.5.406.1221105 (HDP 2.0.9.0-2097)
* Usługa HDInsight 3.1.1.406.1221105 (HDP 2.1.9.0-2196)
* Usługa HDInsight SDK n/d!

Ta wersja zawiera następujące aktualizacje składników.

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Składnik</th>
<th>Typ klaster</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>

<tr>
<td>Poprawka błędu: przejściowymi wystąpił błąd podczas dodawania dużej liczby partycje do tabeli w DDL gałęzi </td>
<td><p>Jeśli podczas dodawania wielu partycje na tabelę programu Hive występuje błąd przejściowymi połączenia z bazą danych metastore gałęzi, mogą nie być DDL gałęzi. Następująca instrukcja będą widoczne w dzienniku błędów gałęzi, jeśli wystąpi ten błąd: </p><p>"Błąd [główny]: ql. Sterownik (SessionState.java:printError(547)) - nie powiodło się: błąd wykonania, kod zwrotny 1 z org.apache.hadoop.hive.ql.exec.DDLTask. MetaException (message:java.lang.RuntimeException: commitTransaction została wywołana, ale openTransactionCalls = 0. Prawdopodobnie oznacza, że są niedopasowane połączenia do openTransaction-commitTransaction)"</p></td>
<td>Gałąź</td>
<td>Hadoop, Hbase</td>
<td>GAŁĄŹ-482 (jest to wewnętrzny JIRA, więc nie mogą być podawane zewnętrznie. Zauważyć odszukanie.)</td>
</tr>

<tr>
<td>Poprawka błędu: rzadkie zawiesza się w konsoli kwerendy HDInsight</td>
<td>W takim przypadku następującą instrukcję są widoczne w dzienniku WebHCat WebHCat uruchamianie zadania: <p>"org.apache.hive.hcatalog.templeton.CatchallExceptionMapper | org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.yarn.exceptions.YarnRuntimeException): nie można załadować pliku historii {wasb adres url z plikiem historii} "</p></td>
<td>WebHCat</td>
<td>Hadoop</td>
<td>GAŁĄŹ-482 (jest to wewnętrzny JIRA, więc nie mogą być podawane zewnętrznie. Zauważyć odszukanie.)</td>
</tr>

<tr>
<td>Poprawka błędu: rzadkie kolekcji w opóźnienie związane z kwerendami Hbase</td>
<td>W takim przypadku użytkownicy będą wyprzedzeniem opóźnienie związane z kwerendami Hbase rzadkie kolekcji 3 sekundy. </td>
<td>Klaster HDInsight bramy</td>
<td>HBase</td>
<td>N/D!</td>
</tr>

<tr>
<td>HDP JAR zmiany nazw plików</td>
<td>Aby uzyskać HDI klaster w wersji 3.0, dostępne są kilka zmian w wewnętrznych plików JAR zainstalowanych przez HDP. Molo 6.1.26.jar zamieniono molo 6.1.26.hwx.jar. Molo — narzędzie 6.1.26.jar zamieniono molo — narzędzie 6.1.26.hwx.jar. Zastosuj te zmiany do projektów Hadoop, Mahout, WebHCat i Oozie.</td>
<td>Hadoop, Mahout, WebHCat, Oozie</td>
<td>Hadoop, HBase</td>
<td>N/D!</td>
</tr>

</table>
<br>


## <a name="notes-for-11212014-release-of-hdinsight"></a>Informacje o wersji 2014-11-21 HDInsight

Numery pełnej wersji dla klastrów HDInsight wdrożony w tej wersji to:

* Usługa HDInsight 2.1.9.382.1169709 (bez zmian z 2014-11-14)
* Usługa HDInsight 3.0.5.382.1169709 (bez zmian z 2014-11-14)
* Usługa HDInsight 3.1.1.382.1169709 (bez zmian z 2014-11-14)
* Usługa HDINsight SDK 1.4.0

Ta wersja zawiera następujące aktualizacje składników.

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Składnik</th>
<th>Typ klaster</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>

<tr>
<td>Uzyskiwanie dostępu do Dzienniki aplikacji</td>
<td>Możliwość wyliczyć programowy aplikacji uruchamianych na klastrów i pobrać odpowiednie Dzienniki aplikacji lub kontenera, aby ułatwić, problematyczny aplikacje.</td>
<td>ZESTAW SDK</td>
<td>Hadoop</td>
<td>N/D!</td>
</tr>

<tr>
<td>Możliwość określania nazwa regionu w IHdInsightClient.DeleteCluster </td>
<td>SDK HDInsight platformy Azure umożliwia Określ nazwę regionu, korzystając z **DeleteCluster**. Dzięki temu odblokować klienci, którzy używano dwa zasoby o takiej samej nazwie w różnych regionów i była nie można usunąć jedną z nich.</td>
<td>ZESTAW SDK</td>
<td>Wszystkie</td>
<td>N/D!</td>
</tr>

<tr>
<td>ClusterDetails.DeploymentId</td>
<td>Obiekt **ClusterDetails** zwraca pola **DeploymentID** , która reprezentuje unikatowy identyfikator klaster. Jest ona zagwarantować być unikatowe w obrębie próby utworzenia klaster o tych samych nazwach.</td>
<td>ZESTAW SDK</td>
<td>Wszystkie</td>
<td>N/D!</td>
</tr>
</table>
<br>

## <a name="notes-for-11142014-release-of-hdinsight"></a>Informacje o wersji 2014-11-14 HDInsight

Numery pełnej wersji dla klastrów HDInsight wdrożony w tej wersji to:

* Usługa HDInsight 2.1.9.382.1169709
* Usługa HDInsight 3.0.5.382.1169709
* Usługa HDInsight 3.1.1.382.1169709

Ta wersja zawiera następujące nowe funkcje, aktualizacje składników i poprawki.

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Składnik</th>
<th>Typ klaster</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>

<tr>
<td>Akcja skrypt (wersja Preview)</td>
<td>Podgląd funkcja dostosowywania klaster, która umożliwia zmianę Hadoop klastrów w dowolny sposób za pomocą niestandardowych skryptów. Za pomocą tej funkcji Użytkownicy mogą eksperymentować i wdrażanie projektów, które są dostępne w ekosystemie Apache Hadoop do klastrów Azure HDInsight. Ta funkcja dostosowywania jest dostępna we wszystkich typach klastrów HDInsight, takich jak Hadoop, HBase i Burza.</td>
<td>Nowa funkcja</td>
<td>Wszystkie</td>
<td>N/D!</td>
</tr>

<tr>
<td>Wbudowane zadania Azure analizy dziennika witryn sieci Web i miejsca do magazynowania</td>
<td>Konsola kwerendy HDInsight ma wprowadzenie do galerii, obsługującego rozwiązania, które działają w danych lub w przykładowych danych.
<p>**Rozwiązania, które pracować z danymi**:<br>
Firma Microsoft została utworzona zadania niektóre z najbardziej typowych scenariuszy analizy danych, które zapewniają punkt wyjścia do tworzenia własnych rozwiązań. Za pomocą danych z zadaniem, aby zobaczyć, jak działa. Gdy skończysz, wpisz co wiesz już utworzyć rozwiązanie, które po zadaniu gotowych w modelu.</p>
<p>**Rozwiązania, które pracować nad przykładowych danych**:<br>
Dowiedz się, jak pracować z usługi HDInsight przez Instruktaż niektóre podstawowe scenariusze (na przykład analizowanie dzienniki sieci web i czujnik danych). Dowiesz się, jak za pomocą usługi HDInsight do analizowania danych w takich i jak możesz połączyć inne aplikacje i usługi do tych danych. Wizualizowanie danych za pomocą połączenia do programu Microsoft Excel zawiera przykład możliwości tej metody.</p></td>
<td>Konsola kwerendy</td>
<td>Hadoop</td>
<td>N/D!</td>
</tr>

<tr>
<td>Poprawka przeciek pamięci w Templeton</td>
<td>Przeciek pamięci w Templeton, którego dotyczy klientów, którzy sprzedał długotrwałe uruchamianie klastrów lub zostały przesyłanie 100s zadania wezwania, drugi został rozwiązany. Manifestowane jako Templeton 5xx błędów i obejść problem został ponownie uruchomić usługę. Obejście nie jest już potrzebne.</td>
<td>Templeton</td>
<td>Wszystkie</td>
<td>https://issues.apache.org/jira/Browse/HADOOP-11248</td>
</tr>
</table>
<br>


**Uwaga**: celu zademonstrowania nowe funkcje udostępnione przez dostosowanie klaster, procedur przy użyciu akcji skrypt, aby zainstalować moduły iskrowym i R w klastrze został umieszczony. Aby uzyskać więcej informacji zobacz:

* [Instalowanie i używanie Spark 1.0 na klastrów HDInsight](hdinsight-hadoop-spark-install.md)
* [Instalowanie i używanie R na klastrów HDInsight Hadoop](hdinsight-hadoop-r-scripts.md)




## <a name="notes-for-11072014-release-of-hdinsight"></a>Informacje o wersji 2014-11-07 HDInsight

Numery pełnej wersji dla klastrów HDInsight, które wdrożony w tej wersji to:

* Usługa HDInsight 2.1 2.1.9.374.1153876
* Usługa HDInsight 3.0 3.0.5.374.1153876
* Usługa HDInsight 3.1 3.1.1.374.1153876

Ta wersja zawiera następujące aktualizacje składników.

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Składnik</th>
<th>Typ klaster</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>

<tr>
<td>HDP 2.1.7</td>
<td>Ta wersja jest oparty na Hortonworks platformy danych (HDP) 2.1.7. Aby uzyskać więcej informacji zobacz <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html" target="_blank">Informacje o wersji dla HDP 2.1.7</a>.</td>
<td>HDP</td>
<td>Wszystkie</td>
<td>N/D!</td>
</tr>

<tr>
<td>Przędza osi czasu serwera</td>
<td>Domyślnie został włączony PRZĘDZY serwera osi czasu (nazywane także ogólnego Historia serwer aplikacji). Oś czasu serwera zawiera ogólne informacje o aplikacjach złożonym (na przykład identyfikator aplikacji, nazwę aplikacji, stan aplikacji, czas przesyłania aplikacji i czas zakończenia aplikacji).

Informacje o aplikacji można pobrać z węzła głównego po zalogowaniu się do identyfikatora URI http://headnodehost:8188 lub uruchamiając polecenie PRZĘDZY: przędzy aplikacji — lista appStates — wszystkie.

Te informacje można również można pobrać zdalnie, jeśli interfejsu API usługi REST w https://{ClusterDnsName}. azurehdinsight.NET/ws/V1/applicationhistory/.

Aby uzyskać bardziej szczegółowe informacje zobacz <a href="http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">PRZĘDZY osi czasu serwera</a>.</td>
<td>Usługi, PRZĘDZY</td>
<td>Hadoop, HBase</td>
<td>N/D!</td>
</tr>

<tr>
<td>Identyfikator rozmieszczania klaster</td>
<td>Począwszy od wersji 1.3.3.1.5426.29232 SDK, użytkownicy mogą uzyskiwać dostęp Unikatowy identyfikator każdej klaster, który jest wydawany HDInsight. Dzięki temu klienci ustalanie unikatowych wystąpień klastrów, gdy wynikowej nazwy DNS wszystkich Tworzenie lub usuwanie scenariuszy.</td>
<td>ZESTAW SDK</td>
<td>Wszystkie</td>
<td>N/D!</td>
</tr>
</table>
<br>

**Uwaga**: ten błąd, który można zapisać numer pełną wersję z pojawiać się w portalu lub zwracane przez zestawu SDK lub środowiska Windows PowerShell został rozwiązany w tej wersji.

## <a name="notes-for-10152014-release"></a>Informacje o wersji 2014-10-15

Ta wersja poprawki ustawiony przeciek pamięci w Templeton, którego dotyczy intensywnie użytkowników Templeton. W niektórych przypadkach użytkownicy, którzy intensywnie wykonywane Templeton chcesz zobaczyć błędy manifestowane jako 500 kody błędów, ponieważ żądania czy nie ma za mało pamięci, aby uruchomić. Obejścia tego problemu było ponownie uruchomić usługę Templeton. Ten problem został rozwiązany.


## <a name="notes-for-1072014-release"></a>Informacje o wersji 2014-10-7

* Podczas korzystania z końcowego Ambari "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}", polu *host_name* zwraca w pełni kwalifikowaną nazwę domeny (FQDN) węzła zamiast tylko nazwa hosta. Na przykład, zamiast powrócić "**headnode0**", otrzymujesz Kwalifikowaną "**headnode0. {} .Net .azurehdinsight ClusterDNS}**". Ta zmiana została wymagane do ułatwienia scenariusze, w którym wielu typów klaster (na przykład HBase i Hadoop) mogą być rozmieszczone w jednej wirtualnej sieci. Dzieje się tak, na przykład, używając HBase jako platformy wewnętrznej dla Hadoop.

* Utworzono nowe ustawienia pamięci wdrażania domyślne klaster HDInsight. Odpowiednio poprzedniego domyślnych ustawień pamięci nie został uwzględniony orientacji dla liczby rdzeni Procesora wdrażania. Nowe ustawienia pamięci powinien zawierać lepiej domyślne (zgodnie z zaleceniami Hortonworks). Aby zmienić te, można znaleźć w dokumentacji zestawu SDK o zmienianiu konfiguracji klaster. Nowe ustawienia pamięci, które są używane przez klaster HDInsight domyślne 4 Procesora core (kontener 8) są wyszczególnione w poniższej tabeli. (Wartości używany przed wydaniem są także dostarczane parenthetically.)

<table border="1">
<tr><th>Składnik</th><th>Przydzielanie pamięci</th></tr>
<tr><td> Alokacja yarn.Scheduler.minimum</td><td>768 MB (wcześniej 512 MB)</td></tr>
<tr><td> Alokacja yarn.Scheduler.Maximum</td><td>6144 MB (bez zmian)</td></tr>
<tr><td>yarn.nodemanager.Resource.Memory</td><td>6144 MB (bez zmian)</td></tr>
<tr><td>mapreduce.map.Memory</td><td>768 MB (wcześniej 512 MB)</td></tr>
<tr><td>mapreduce.map.Java.opts</td><td>Opcje Xmx512m = (wcześniej - Xmx410m)</td></tr>
<tr><td>mapreduce.Reduce.Memory</td><td>1536 MB (wcześniej 1024 MB)</td></tr>
<tr><td>mapreduce.Reduce.Java.opts</td><td>Opcje Xmx1024m = (wcześniej — Xmx819m)</td></tr>
<tr><td>yarn.App.mapreduce.AM.Resource</td><td>768 MB (wcześniej 1024 MB)</td></tr>
<tr><td>yarn.App.mapreduce.AM.Command</td><td>Opcje Xmx512m = (wcześniej - Xmx819m)</td></tr>
<tr><td>mapreduce.Task.IO.sort</td><td>256 MB (wcześniej 200 MB)</td></tr>
<tr><td>tez.AM.Resource.Memory</td><td>1536 MB (bez zmian)</td></tr>

</table><br>

Aby uzyskać więcej informacji o ustawieniach konfiguracji pamięci używane przez PRZĘDZY i MapReduce na platformie danych Hortonworks jest używana przez usługi HDInsight zobacz [Określanie ustawienia konfiguracji pamięci HDP](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1-latest/bk_installing_manually_book/content/rpm-chap1-11.html). Hortonworks wyposażony w narzędziu do obliczania ustawienia pamięci pisane z wielkiej litery.

Dotyczące Azure programu PowerShell i HDInsight SDK komunikat o błędzie: "*klaster nie jest skonfigurowany do dostępu usług HTTP*":

* Ten błąd występuje znany [problem ze zgodnością](https://social.msdn.microsoft.com/Forums/azure/a7de016d-8de1-4385-b89e-d2e7a1a9d927/hdinsight-powershellsdk-error-cluster-is-not-configured-for-http-services-access?forum=hdinsight) , które mogą wystąpić ze względu na różnice w wersji HDInsight SDK lub Azure programu PowerShell i wersji programu klaster. Klastrów utworzony na 8-15 lub nowszy obsługują nowe możliwości obsługi administracyjnej do wirtualnych sieci. Ale ta funkcja nie jest poprawnie interpretowany przez starsze wersje programu HDInsight SDK lub Azure programu PowerShell. Wynikiem funkcji jest błąd w niektórych operacji przesyłania zadań. Użycie interfejsy API SDK HDInsight lub poleceń cmdlet środowiska PowerShell Azure (**Użyj AzureRmHDInsightCluster** lub **AzureRmHDInsightHiveJob Wywołaj**), aby przesłać zadania może zakończyć się niepowodzeniem tych operacji z powodu błędu "*klaster <clustername> nie jest skonfigurowany do dostępu usług HTTP*." Lub (w zależności od operacji), może zostać wyświetlony inne komunikaty o błędach, takich jak "*nie można połączyć z klastrem*".

* Te problemy zgodności można rozwiązać w najnowsze wersje HDInsight SDK oraz Azure programu PowerShell. Zaleca się aktualizowanie HDInsight SDK wersji 1.3.1.6 lub nowszym i Azure narzędzi programu PowerShell do wersji 0.8.8 lub nowszej. Możesz uzyskać dostęp do najnowszych SDK HDInsight z [](http://nuget.codeplex.com/wikipage?title=Getting%20Started) i narzędzi programu PowerShell Azure, jak [zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).



## <a name="notes-for-9122014-release-of-hdinsight-31"></a>Informacje o wersji HDInsight 3.1 2014-9-12

* Ta wersja jest oparty na Hortonworks platformy danych (HDP) 2.1.5. Aby uzyskać listę błędy usunięte w tej wersji zobacz stronę [stała w tej wersji](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.5/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.5-fixed.html) w witrynie Hortonworks.
* W folderze biblioteki świnka pliku "avro-mapred-1.7.4.jar" została zmieniona na "avro-mapred-1.7.4-hadoop2.jar." Zawartość tego pliku zawiera pomocniczych poprawka błędu nierozdzielające. Zaleca się, że klienci nie wprowadzaj bezpośrednia zależność na nazwę pliku SŁOIK, aby uniknąć podziału zmian nazw plików.


## <a name="notes-for-8212014-release"></a>Informacje o wersji 2014-8-21

* Firma Microsoft dodajesz następującej konfiguracji WebHCat (gałąź 7155), który określa domyślny limit pamięci dla zadania kontroler Templeton do 1 GB. (Wartość domyślna poprzedniego był 512 MB).

     templeton.mapper.Memory.MB (= 1024)

    * Tę zmianę dotyczy następujący komunikat o błędzie, który był uruchamiany niektóre kwerendy gałęzi w ze względu na dolnym limity pamięci: "Kontener jest uruchomiona poza limity pamięci fizycznej".
    * Aby powrócić do starego ustawienia domyślne, można ustawić wartość ta konfiguracja 512 przy użyciu programu PowerShell Azure w czasie tworzenia klaster przy użyciu następującego polecenia:

        Dodawanie AzureRmHDInsightConfigValues-Core@{"templeton.mapper.memory.mb"="512";}


* Nazwa hosta roli zookeeper została zmieniona na *zookeeper*. Dotyczy to rozpoznawanie nazw w klastrze, ale nie wywiera wpływu na zewnętrznych pozostałych interfejsów API. Jeśli masz składników korzystających z nazwą hosta *zookeepernode* , musisz zaktualizuje ich tak, aby użyć nową nazwę. Nowe nazwy trzy węzły zookeeper są następujące:
    * zookeeper0
    * zookeeper1
    * zookeeper2
* Diagram obsługi wersję HBase jest aktualizowana. Dla produkcji HBase obciążenia jest obsługiwane tylko HDInsight wersji 3.1 (HBase wersję 0,98). W wersji 3.0 (który był dostępny do podglądu) nie jest obsługiwane przenoszony do przodu.

## <a name="notes-about-clusters-created-prior-to-8152014"></a>Uwagi dotyczące klastrów utworzone przed 2014-8-15

Komunikat o błędzie Azure programu PowerShell lub HDInsight SDK "klaster <clustername> nie jest skonfigurowany do dostępu usług HTTP" (lub w zależności od operacji, inne komunikaty o błędach takich jak: "Nie można połączyć z klastrem") mogą wystąpić ze względu na wersję różnią Azure programu PowerShell lub HDInsight SDK i klastrze. Klastrów utworzony na 8-15 lub nowszy obsługują nowe możliwości obsługi administracyjnej do wirtualnych sieci. Ta funkcja nie jest poprawnie interpretowane przez starsze wersje programu Azure PowerShell lub SDK HDInsight, co spowoduje błędy operacji przesyłania zadań. Użycie polecenia cmdlet interfejsy API SDK HDInsight lub Azure programu PowerShell (na przykład użyj AzureRmHDInsightCluster lub AzureRmHDInsightHiveJob Wywołaj) przesyłać zadania te operacje może zakończyć się niepowodzeniem z jednym z opisem komunikaty o błędach.

Te problemy zgodności można rozwiązać w najnowsze wersje HDInsight SDK oraz Azure programu PowerShell. Zaleca się aktualizowanie HDInsight SDK wersji 1.3.1.6 lub nowszym i Azure narzędzi programu PowerShell do wersji 0.8.8 lub nowszej. Możesz uzyskać dostęp do najnowszych SDK HDInsight z [NuGet][nuget-link]. Dostęp do narzędzia programu PowerShell Azure za pomocą [Instalatora platformy sieci Web firmy Microsoft][webpi-link].


## <a name="notes-for-7282014-release"></a>Informacje o wersji 2014-7-28

* **Usługa HDInsight dostępne w nowe obszary**: Firma Microsoft rozwinięty obecności geograficznych HDInsight do trzech regionów. Usługa HDInsight klientów można tworzyć klastrów w następujących regionach:
    * Wschodnioazjatyckie
    * Ameryka Północna centralnej w Stanach Zjednoczonych
    * Południowej centralnej Stany Zjednoczone
* Usługa HDInsight wersję 1.6 (HDP 1.1 i Hadoop 1.0.3) i HDInsight wersji 2.1 (HDP1.3 i Hadoop 1.2) są usuwane z portalu Azure. Nadal można tworzyć klastrów Hadoop dla tych wersji, przy użyciu polecenia cmdlet programu PowerShell Azure [Nowy AzureRmHDInsightCluster](http://msdn.microsoft.com/library/dn593744.aspx) lub przy użyciu [Zestawu SDK HDInsight](http://msdn.microsoft.com/library/azure/dn469975.aspx). Zajrzyj do strony [przechowywania wersji składnika HDInsight](hdinsight-component-versioning.md) , aby uzyskać więcej informacji.
* Zmiany Hortonworks danych platformy (HDP) w tej wersji:

<table border="1">
<tr><th>HDP</th><th>Zmiany</th></tr>
<tr><td>HDP 1.3-HDI 2.1</td><td>Bez zmian</td></tr>
<tr><td>HDP 2.0-HDI 3.0</td><td>Bez zmian</td></tr>
<tr><td>HDP 2.1-HDI 3.1</td><td>zookeeper: [3.4.5.2.1.3.0-1948] -> [3.4.5.2.1.3.2-0002]</td></tr>


</table><br>

## <a name="notes-for-6242014-release"></a>Informacje o wersji 2014-6-24

Ta wersja zawiera ulepszenia z usługą HDInsight:

* **Dostępność HDP 2.1**: 3.1 HDInsight, (zawierającej HDP 2.1) jest zazwyczaj dostępny i jest to wersja domyślnego dla nowych klastrów.
* **HBase — ulepszenia portal Azure**: udostępniamy klastrów HBase w podglądzie. Możesz utworzyć klastrów HBase z portalu za pomocą kilku kliknięć. 

Z HBase można tworzyć różne obciążenia w czasie rzeczywistym na HDInsight, pochodzących z takich witryn interakcyjnych współdziałać z dużych zestawów danych do usług przechowywania danych czujnik i telemetrycznego z milionów punktów końcowych. Następnym krokiem jest analizowanie danych w tych obciążeń pracą z zadaniami Hadoop i jest to możliwe w HDInsight za pośrednictwem programu PowerShell Azure i na pulpicie nawigacyjnym klaster gałęzi.

### <a name="apache-mahout-preinstalled-on-hdinsight-31"></a>Mahout Apache preinstalowane na HDInsight 3.1

 [Mahout](http://hortonworks.com/hadoop/mahout/) jest zainstalowany na klastrów HDInsight 3.1 Hadoop, więc można uruchamiać zadania Mahout bez konieczności kolejne klastrze konfiguracji. Na przykład można zdalnego w klastrze Hadoop za pomocą protokołu RDP (Remote Desktop) i bez dodatkowych czynności, można uruchomić następujące polecenie Witaj świecie Mahout:

        mahout org.apache.mahout.classifier.df.tools.Describe -p /user/hdp/glass.data -f /user/hdp/glass.info -d I 9 N L  

        mahout org.apache.mahout.classifier.df.BreimanExample -d /user/hdp/glass.data -ds /user/hdp/glass.info -i 10 -t 100

Dokładniejszy opis tej procedury zapoznaj się z dokumentacją, na [Przykład Breiman](https://mahout.apache.org/users/classification/breiman-example.html) w witrynie sieci Web Apache Mahout.


### <a name="hive-queries-can-use-tez-in-hdinsight-31"></a>Gałąź kwerendy można używać Tez w HDInsight 3.1

Gałąź 0,13 cala jest dostępna w HDInsight 3.1 i jest możliwe uruchamianie zapytań za pomocą Tez, którego można użyć do znacznych ulepszeń dotyczących wydajności.
Tez nie jest Włącz domyślnie gałęzi kwerend. Aby użyć go, możesz wybrać opcję w. Możesz włączyć Tez, uruchamiając następujące wstawkę kodu:

        set hive.execution.engine=tez;
        select sc_status, count(*), histogram_numeric(sc_bytes,5) from website_logs_orc_local group by sc_status;

Hortonworks opublikowała szczegółowego podziału gałęzi ulepszenia wydajności kwerendy z Tez dostarczona w stawek standardowych. Aby uzyskać szczegółowe informacje zobacz [wzorców Apache gałęzi 13 dla Hadoop przedsiębiorstwa](http://hortonworks.com/blog/benchmarking-apache-hive-13-enterprise-hadoop/).

Aby uzyskać więcej informacji o korzystaniu z Tez gałęzi zobacz [gałęzi na Tez](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez).

###<a name="global-availability"></a>Dostępność globalny
W wersji HDInsight na Hadoop 2.2, firma Microsoft udostępniła HDInsight w miejsce, w którym znajduje się Azure wszystkich głównych regionów geograficznych. W szczególności zachód Europa i Azji Południowo centrach danych zostały wprowadzone online. Dzięki temu klienci zlokalizować klastrów w centrum danych, który jest Zamknij i potencjalnie strefę podobne wymagania dotyczące zgodności.


###<a name="dos--donts-between-cluster-versions"></a>Zadania i między wersjami klaster

**Używany z klastrem HDInsight 3.1 metastores Oozie są nie zgodne z klastrów HDInsight 2.1 i nie można używać z tą wersją poprzedniego**.

Niestandardowe Oozie metastore bazy danych używany z klastrem HDInsight 3.1 nie można użyć ponownie z klastrem HDInsight 2.1. Dotyczy to nawet wtedy, gdy metastore pochodzi z klastrem HDInsight 2.1. W tym scenariuszu nie jest obsługiwana, ponieważ schemat metastore otrzymuje uaktualniony, gdy używana z klastrem HDInsight 3.1, więc nie jest zgodny z metastore wymagane przez klastrów HDInsight 2.1. Każda próba ponowne używanie metastore Oozie używany z klastrem HDInsight 3.1 spowoduje, że klaster HDInsight 2.1 bezużyteczny.

**Nie można udostępniać Oozie metastores między klastrów.**

Oozie metastores zostaną dołączone do określonej klastrów, a nie mogą być udostępniane przez klastrów.

###<a name="breaking-changes"></a>Przerywanie zmian

**Prefiks składni**: tylko "wasbs: / /" składnia jest obsługiwana w HDInsight 3.1 i klastrów 3.0. Starszy "asv: / /" składnia jest obsługiwana w HDInsight 2.1 i 1,6 klastrów, ale nie jest obsługiwana w HDInsight 3.1 lub klastrów 3.0. Oznacza to, że przesłane do 3.1 HDInsight lub 3.0 klaster zadania korzystające z jawnie "asv: / /" składni zakończy się niepowodzeniem. "Wasbs: / /" składni należy użyć. Ponadto zadania przekazane do dowolnego 3.1 HDInsight lub 3.0 klastrów, które zostały utworzone przy użyciu istniejącego metastore, który zawiera jawne odwołania do zasobów za pomocą "asv: / /" składni zakończy się niepowodzeniem. Te metastores muszą być ponownie utworzony przy użyciu "wasbs: / /" składni zasoby adres.


**Porty**: porty używane przez usługę HDInsight zostały zmienione. Numery portów, które były używane były w zakresie portów tymczasowych systemu operacyjnego Windows. Porty są przydzielane automatycznie z wstępnie zdefiniowanego zakresu tymczasowych dla krótkotrwałe Internet protocol komunikacji. Nowy zestaw dozwolonych numery portów usługi Hortonworks danych platformy (HDP) wykraczają poza zakres w ten sposób, aby uniknąć konfliktów, które mogą wystąpić były używane przez usługi uruchomionych węzła głównego występują. Nowe numery portów nie powinien powodować zmiany podziału. Liczby używane są następujące:

 **Usługa HDInsight 1.6 (HDP 1.1)**
<table border="1">
<tr><th>Nazwa</th><th>Wartość</th></tr>
<tr><td>DFS.http.address</td><td>namenodehost:30070</td></tr>
<tr><td>DFS.datanode.address</td><td>0.0.0.0:30010</td></tr>
<tr><td>DFS.datanode.http.address</td><td>0.0.0.0:30075</td></tr>
<tr><td>DFS.datanode.IPC.address</td><td>0.0.0.0:30020</td></tr>
<tr><td>DFS.Secondary.http.address</td><td>0.0.0.0:30090</td></tr>
<tr><td>mapred.job.Tracker.http.address</td><td>jobtrackerhost:30030</td></tr>
<tr><td>mapred.Task.Tracker.http.address</td><td>0.0.0.0:30060</td></tr>
<tr><td>mapreduce.history.Server.http.address</td><td>0.0.0.0:31111</td></tr>
<tr><td>templeton.port</td><td>30111</td></tr>
</table><br>

 **Usługa HDInsight 3.1 i 3.0 (HDP 2.1 i 2.0)**
<table border="1">
<tr><th>Nazwa</th><th>Wartość</th></tr>
<tr><td>adres DFS.namenode.http</td><td>namenodehost:30070</td></tr>
<tr><td>adres DFS.namenode.HTTPS</td><td>headnodehost:30470</td></tr>
<tr><td>DFS.datanode.address</td><td>0.0.0.0:30010</td></tr>
<tr><td>DFS.datanode.http.address</td><td>0.0.0.0:30075</td></tr>
<tr><td>DFS.datanode.IPC.address</td><td>0.0.0.0:30020</td></tr>
<tr><td>adres DFS.namenode.Secondary.http</td><td>0.0.0.0:30090</td></tr>
<tr><td>yarn.nodemanager.webapp.address</td><td>0.0.0.0:30060</td></tr>
<tr><td>templeton.port</td><td>30111</td></tr>
</table><br>

###<a name="dependencies"></a>Zależności

Dodano następujących zależności w HDInsight 3.x (HDP2.x):

* guice servlet
* podstawowe optiq
* javax.inject
* Aktywacja
* jsr305
* geronimo-jaspic_1.0_spec
* lip — slf4j
* xmlbuilder języka Java
* który
* kompilatora Commons:
* Interfejs api jdo
* math3 Commons:
* paranamer
* jaxb impl
* stringtemplate
* eigenbase xom
* Jersey servlet
* wykonywalna Commons:
* Interfejs api jaxb
* Molo wszystkie serwera
* janino
* xercesImpl
* optiq avatica
* jta
* właściwości eigenbase
* wszystkie groovy
* podstawowe hamcrest
* Poczta
* linq4j
* jpam
* Jersey klienta
* aopalliance
* geronimo-annotation_1.0_spec
* Uruchamianie który
* Jersey guice
* interfejsy API XML
* Interfejs api stax
* ASM commons
* ASM drzewa
* wadl
* geronimo-jta_1.1_spec
* guice
* wszystkie leveldbjni
* prędkości
* zrzucenia
* szałowe java
* wszystkie molo
* dbcp Commons:

Istnieje już następujących zależności w HDInsight 3.x (HDP2.x):

* jdeb
* kfs
* sqlline
* Bluszcz
* aspectjrt
* JSON
* podstawowe
* Interfejs api jdo2
* avro mapred
* Wzmacniacz datanucleus
* JSP
* Commons rejestrowanie api
* Funkcje matematyczne Commons:
* JavaEWAH
* aspectjtools
* javolution
* hdfsproxy
* hbase
* szałowe

###<a name="version-changes"></a>Zmiany wersji

Wprowadzono następujące zmiany wersję między HDInsight 2.x (HDP1.x) i HDInsight 3.x (HDP2.x):

* Podstawowe metryki: [2.1.2] -> [3.0.0]
* derbynet: [10.4.2.0] -> [10.10.1.1]
* datanucleus: ["rdbms-3.0.8'] -> [" rdbms-3.2.9']
* jasper kompilatora: [5.5.12] -> [5.5.23]
* log4j: ["1.2.15", "1.2.16"] -> ["1.2.16", "1.2.17"]
* derbyclient: [10.4.2.0] -> [10.10.1.1]
* httpcore: [4.2.4] -> [4.2.5]
* hsqldb: [1.8.0.10] -> [2.0.0]
* jets3t: [0.6.1] -> [0.9.0]
* protobuf java: [2.4.1] -> [2.5.0]
* DOM: [10.4.2.0] -> [10.10.1.1]
* jasper: ["środowisko uruchomieniowe-5.5.12'] -> [" środowisko uruchomieniowe-5.5.23']
* demon Commons: [1.0.1] -> [1.0.13]
* podstawowe datanucleus: [3.0.9] -> [3.2.10]
* datanucleus-interfejsu api-jdo: [3.0.7] -> [3.2.6]
* zookeeper: [3.4.5.1.3.9.0-01320] -> [3.4.5.2.1.3.0-1948]
* bonecp: [0.7.1.RELEASE] -> ["
* 0.8.0.RELEASE "]


### <a name="drivers"></a>Sterowniki
Sterownik Java Connnectivity bazy danych (JDBC) dla programu SQL Server jest używane wewnętrznie HDInsight i nie jest używana do działania zewnętrzne. Jeśli chcesz nawiązać połączenie HDInsight za pomocą połączenia ODBC (Open Database), użyj sterownika ODBC gałęzi firmy Microsoft. Aby uzyskać więcej informacji zobacz [Połączyć program Excel z usługą hdinsight za pomocą sterownika ODBC gałęzi firmy Microsoft](hdinsight-connect-excel-hive-odbc-driver.md).


### <a name="bug-fixes"></a>Poprawki

W tej wersji możemy odświeżeniu następujących wersji usługi HDInsight z kilku poprawki:

* Usługa HDInsight 2.1 (HDP 1.3)
* Usługa HDInsight 3.0 (HDP 2.0)
* Usługa HDInsight 3.1 (HDP 2.1)


## <a name="hortonworks-release-notes"></a>Informacje o wersji Hortonworks

Informacje o wersji dla platformy danych Hortonworks (HDPs) są używane przez HDInsight wersję klastrów są dostępne w następujących miejscach:

* Usługa HDInsight wersji 3.1 stosuje rozkład Hadoop, oparty na [Hortonworks danych platformy 2.1.7][hdp-2-1-7]. Jest to domyślne klaster Hadoop utworzenia za pomocą portalu Azure po 2014-11-7. Usługa HDInsight 3.1 klastrów utworzonych przed 2014-11-7 były oparte na [Platformie danych Hortonworks 2.1.1][hdp-2-1-1]

* Usługa HDInsight wersji 3.0 używa rozkładu Hadoop, oparty na [Hortonworks danych platformy 2.0][hdp-2-0-8].

* Usługa HDInsight wersji 2.1 używa rozkładu Hadoop, oparty na [Hortonworks danych platformy 1.3][hdp-1-3-0].

* Usługa HDInsight wersji 1.6 używa rozkładu Hadoop, oparty na [Hortonworks danych platformy 1.1][hdp-1-1-0].

[hdp-2-1-7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-Win-1.1/bk_releasenotes_HDP-Win/content/ch_relnotes-hdp-win-1.1.0_1.html

[nuget-link]: https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.HDInsight/

[webpi-link]: http://go.microsoft.com/?linkid=9811175&clcid=0x409

[hdinsight-install-spark]: ../hdinsight-hadoop-spark-install/
[hdinsight-r-scripts]: ../hdinsight-hadoop-r-scripts/
 
