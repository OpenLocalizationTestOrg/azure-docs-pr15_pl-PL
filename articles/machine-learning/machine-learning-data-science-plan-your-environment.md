<properties
    pageTitle="Jak scenariusze zidentyfikowanie i zaplanowanie zaawansowane przetwarzanie analizy danych | Microsoft Azure"
    description="Planowanie pod kątem zaawansowanej analizy, ustalając szereg pytań klucza."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="bradsev" />


# <a name="how-to-identify-scenarios-and-plan-for-advanced-analytics-data-processing"></a>Sposób identyfikowania scenariusze i Planowanie przetwarzania zaawansowanej analizy danych

Jakie zasoby należy zaplanować mają być uwzględniane podczas konfigurowania środowiska do zaawansowanej analizy przetwarzania w zestawie danych? W tym artykule sugerowanie szereg pytań ułatwiających identyfikowanie zadań i zasobów odpowiednie rozwiązania. Kolejność czynności wysokiego poziomu przewidywanych analizy przedstawiono w [Co to jest proces nauki danych zespołu (TDSP)?](data-science-process-overview.md). Każdy z tych kroków wymaga określonych zasobów dla zadań dotyczących określonego rozwiązania. Podstawowe pytania do identyfikowania rozwiązania dotyczą danych logistyce, cechy jakości zestawy danych, narzędzia i języki, aby wykonać analizy.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="logistic-questions-data-locations-and-movement"></a>Pytania logistyczne: lokalizacje danych i przepływu
Logistyczne pytania dotyczą lokalizację **źródła danych**, **docelowego miejsca docelowego** platformy Azure i wymagania dotyczące przenoszenia dane, włącznie z harmonogramem, kwota i zasobach. Dane może być konieczne można przenieść kilka razy podczas procesu analizy. Typowy scenariusz jest przeniesienie danych lokalnych w niektórych formularz miejsca do magazynowania, Azure, a następnie do komputera nauki Studio.

1. **Co to jest źródło danych?** Jest to lokalne lub w chmurze? Na przykład:
    - Dane są publicznie dostępne pod adresem HTTP.
    - Dane znajdują się w lokalizacji pliku sieć lokalna.
    - Dane znajdują się w bazie danych programu SQL Server.
    - Dane są przechowywane w kontenerze Azure miejsca do magazynowania

2. **Co to jest Azure miejsce docelowe?** Miejsce, w którym usługa musi być przetwarzania lub modelowania? Na przykład:
    - Magazyn obiektów Blob platformy Azure
    - Bazy danych platformy SQL Azure
    - Program SQL Server na Azure maszyn wirtualnych
    - Usługa HDInsight (Hadoop Azure) lub gałęzi tabel
    - Nauka Azure komputera
    - Instalację Azure wirtualnych dyskach twardych.

3. **Jak zamierzasz przenieść dane?** W poniższych tematach opisano procedury i zasobów dostępnych dla mogły zjeść tej ostatniej i ładowanie danych na wiele różnych przechowywania i przetwarzania środowiskach.

    -  [Ładowanie danych do środowiska miejsca do magazynowania dla analizy](machine-learning-data-science-ingest-data.md)
    -  [Importowanie danych szkolenie w Azure maszynowego uczenia Studio z różnych źródeł danych](machine-learning-data-science-import-data.md).

4. **Czy dane należy przenieść regularnego lub zmodyfikowane podczas migracji?** Warto rozważyć użycie Azure Factory danych (ADF), gdy dane trzeba stale zmigrować, szczególnie jeśli scenariuszy hybrydowego, który uzyskuje dostęp do obu w wersji lokalnej i polega zasoby chmury lub po danych jest wykonany lub musi można modyfikować lub zostały dodane do niego w trakcie migrowane reguł biznesowych. Aby uzyskać więcej informacji zobacz [Przenoszenie danych z lokalnego programu SQL server do platformy SQL Azure z Factory danych Azure](machine-learning-data-science-move-sql-azure-adf.md)

5. **Ilość danych jest mają zostać przeniesione do Azure?** Zestawy danych, które są bardzo duże może przekroczyć pojemności pewnych środowiskach. Na przykład zobacz dyskusji limity rozmiarów dla komputera Studio nauki w następnej sekcji. W takich przypadkach przykładowych danych może być stosowane podczas analizy. Aby uzyskać szczegółowe informacje dotyczące próby szczegółów zestawu danych w różnych środowiskach Azure zobacz [przykładowych danych w procesie nauki danych zespołu](machine-learning-data-science-sample-data.md).


## <a name="data-characteristics-questions-type-format-and-size"></a>Pytania cechy danych: typ, format i rozmiar
Te pytania stanowią podstawę planowania z magazynu i przetwarzania środowisk, z których każdy będzie są odpowiednie dla różnych typów danych i każdy z nich ma pewne ograniczenia.

1. **Co to są typy danych?** Na przykład:
    - Numeryczne
    - Kategorii
    - Ciągi
    - Binarny

2. **Sposób formatowania danych?** Na przykład:
    - Rozdzielany przecinkami (CSV) lub tabulatorami (TSV) płaska plików
    - Skompresowany lub nieskompresowane
    - Obiektów blob platformy Azure
    - Gałąź Hadoop tabel
    - Tabele programu SQL Server

2. **Jak duże jest danych?**
    - Mały: mniej niż 2GB
    - Średni: Większa niż 2GB i mniejsza niż 10
    - Duży: Ponad 10GB

Na przykład wykonać środowiska Azure maszynowego uczenia Studio:

- Aby uzyskać listę formatów danych i typów obsługiwanych przez Azure maszynowego uczenia Studio, zobacz sekcję [danych formatów i typy danych obsługiwane](machine-learning-data-science-import-data.md#data-formats-and-data-types-supported) .
- Aby uzyskać informacji na temat ograniczeń danych Azure maszynowego uczenia Studio, zobacz **wielkość zestawu danych można dla mojej modułów?** sekcji [Importowanie i eksportowanie danych szkoleniowe na komputerze](machine-learning-faq.md#machine-learning-studio-questions)

Dla informacji na temat ograniczeń dotyczących innych usług Azure używanego podczas analizy zobacz [subskrypcji Azure i limity dotyczące usługi, przydziały oraz ograniczenia](../azure-subscription-service-limits.md).

## <a name="data-quality-questions-exploration-and-pre-processing"></a>Pytania dotyczące jakości danych: badań i wstępne przetwarzanie

1. **Co należy wiedzieć o danych?** Eksplorowanie danych, gdy potrzebne do uzyskania opis jego podstawowe cechy. Co wzorców lub trendy go dowody, co to jest wartości odstających lub wartości nie są widoczne. Ten krok jest ważna w przypadku określania wstępne przetwarzanie potrzeb, opracowywania hipotez, które mogą proponowanie najbardziej odpowiednie właściwości lub wpisz analizy i opracowywania planów zbierania dodatkowych danych. Statystyki opisowe wyliczanie i rysowanie wizualizacji są przydatne technik kontroli danych. Aby uzyskać szczegółowe informacje o sposobach eksplorowania zestawu danych w różnych środowiskach Azure zobacz [Eksplorowanie danych w procesie nauki danych zespołu](machine-learning-data-science-explore-data.md).

2. **Czy dane wymaga wstępne przetwarzanie lub czyszczenie?**
Wstępne przetwarzanie i oczyszczania danych są ważne zadania, które zwykle należy przeprowadzić przed zestawu danych można skutecznie szkoleniowe na komputerze. Dane często jest naprawienie i mało wiarygodnych i może brakować wartości. Przy użyciu takich danych modelowania może spowodować błąd wyników. Aby uzyskać opis zobacz [nauki zadań w celu przygotowywanie danych dla rozszerzonego komputera](machine-learning-data-science-prepare-data.md).

## <a name="tools-and-languages-questions"></a>Pytania dotyczące narzędzia i języki
Istnieje wiele opcji w tym miejscu, w zależności od jakich języków i środowiska projektowania lub narzędzia potrzebne lub są najbardziej conformable przy użyciu.

1. **Jakie języki woli używać na potrzeby analizy?**  
    - R
    - Python
    - SQL

2. **Z jakich narzędzi należy używać na potrzeby analizy danych?**
    - [Microsoft Azure programu Powershell](powershell-install-configure.md) — język skryptów, używane do administrowania Azure zasobów w języku skryptów.
    - [Azure maszynowego uczenia Studio](machine-learning-what-is-ml-studio/)
    - [Obrót analizy](http://www.revolutionanalytics.com/revolution-r-open)
    - [RStudio](http://www.rstudio.com)
    - [Narzędzia Python programu Visual Studio](http://microsoft.github.io/PTVS/)
    - [Anaconda](https://www.continuum.io/why-anaconda)
    - [Jupyter notesów](http://jupyter.org/)
    - [Microsoft Power BI](http://powerbi.microsoft.com)


## <a name="identify-your-advanced-analytics-scenario"></a>Identyfikowanie rozwiązania zaawansowanej analizy
Gdy masz odpowiedzi na pytania w poprzedniej sekcji, możesz przystąpić do określenia, który scenariusz najlepiej pasuje do Twojej sprawy. Przykładowe scenariusze zostały opisane w [scenariusze zaawansowanej analizy w Azure maszynowego uczenia](machine-learning-data-science-plan-sample-scenarios.md).
