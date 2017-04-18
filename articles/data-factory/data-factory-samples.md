<properties     
    pageTitle="Azure danych Factory - próbki" 
    description="Szczegółowe informacje na temat próbki dostarczane z usługą Azure danych Factory." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="shlo"/>

# <a name="azure-data-factory---samples"></a>Azure danych Factory - próbki

## <a name="samples-on-github"></a>Próbki GitHub
[Repozytorium GitHub Azure-DataFactory](https://github.com/azure/azure-datafactory) zawiera kilka próbek, które ułatwiają szybkie zwiększać z usługą Azure Factory danych (lub) modyfikowanie skryptów i używać go w aplikacji własnych. Samples\JSON folder zawiera wstawki JSON dla typowych scenariuszy.

| Przykładowe | Opis |
| :----- | :---------- | 
| [Przewodnik po ADF](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFWalkthrough) | W tym przykładzie przedstawiono Instruktaż zakończenia do końca przetwarzania Włączanie danych z plików dziennika w analizy za pomocą Azure danych fabrycznych pliki dziennika. <br/><br/>W tym instruktażu proces Factory danych zbiera dzienniki próbki, procesy i wzbogaca dane z dzienników z danych źródłowych i przekształceń danych do oceny skuteczność kampanii marketingowej, który został ostatnio uruchomiony. |
| [Przykłady JSON](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON) | W tym przykładzie przedstawiono przykłady JSON dla typowych scenariuszy. | 
| [Przykładowe Downloader danych http](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample) | Ten przykładowy pokazy pobieranie danych z punktu końcowego HTTP magazynem obiektów Blob platformy Azure za pomocą niestandardowym działaniem .NET. |
| [Krzyżowe przykładowe netto działania domeny aplikacji kropka](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) | W tym przykładzie umożliwia tworzenie niestandardowego działania .NET, która nie jest ograniczona do wersji zestawu używanych przez uruchamianie ADF (na przykład WindowsAzure.Storage v4.3.0 Newtonsoft.Json v6.0.x, itp.). |
| [Uruchamianie skryptu R](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample) |  W tym przykładzie zawiera niestandardowym działaniem Factory danych, który może służyć do wywołania RScript.exe. W tym przykładzie działa tylko z własnych klaster HDInsight (nie z wybieraniem), który jest już zainstalowany R nad nim. |
| [Wywoływanie zadań Spark w klastrze HDInsight Hadoop](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/Spark) | W tym przykładzie pokazano, jak za pomocą aktywności MapReduce wywołania programu Spark. Spark program kopiuje tylko dane z jeden kontener obiektów Blob platformy Azure innemu użytkownikowi. |
| [Analiza Twitter przy użyciu Azure maszynowego uczenia partii wyników aktywności](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-AzureMLBatchScoringActivity) | W tym przykładzie pokazano, jak za pomocą AzureMLBatchScoringActivity wywołania model Azure maszynowego uczenia wykonującego twitter upodobania analiza wyników przewidywania itp. |
| [Analiza Twitter przy użyciu niestandardowego działania](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-CustomC%23Activity) |  W tym przykładzie pokazano, jak za pomocą niestandardowym działaniem .NET wywołania modelu Azure maszynowego uczenia, który wykonuje twitter upodobania analiza wyników przewidywania itp. |
| [Procesy parametryczne szkoleniowe Azure komputera](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ParameterizedPipelinesForAzureML/) | Przykład zawiera kompleksowe C# kodu do wdrożenia procesy N wyników i przeszkolenie każdego z parametrem innego regionu, gdzie listą regionów pochodzi z pliku parameters.txt, który znajduje się w tym przykładzie. | 
| [Odświeżanie danych odniesienia dla zadań analizy strumieniu Azure](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs) |  W tym przykładzie pokazano sposób przy użyciu Azure Factory danych i analizy strumieniu Azure wykonać kwerendy z danych źródłowych i Konfigurowanie odświeżania danych odwołania zgodnie z harmonogramem. |
| [Potok hybrydowego z lokalnego Hortonworks Hadoop](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HybridPipelineWithOnPremisesHortonworksHadoop) | W przykładzie użyto klastrze Hadoop lokalnego docelowej obliczeń dla wykonywania zadań w Factory danych tak, jak chcesz dodać innych celów obliczeń, jak HDInsight podstawie Hadoop klaster w chmurze. |
| [Narzędzie do konwersji JSON](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSONConversionTool) | To narzędzie umożliwia konwertowanie JSONs wersji przed 2015-07-01-Podgląd, aby najnowsze lub 2015-07-01-Podgląd (ustawienie domyślne). |  
| [U SQL przykładowy plik wprowadzania danych](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/U-SQL%20Sample%20Input%20File) |  Ten plik jest przykładowy plik używane przez działanie U-SQL. | 

## <a name="azure-resource-manager-templates"></a>Azure szablony Menedżera zasobów
Możesz znaleźć szablonach Azure Menedżera zasobów dla fabryki danych na Github. 

| Szablon | Opis |
| -------- | ----------- | 
| [Skopiuj z magazynem obiektów Blob Azure bazą danych SQL Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy) | Wdrażanie ten szablon utworzy factory Azure danych z potok kopiowania danych z magazynu obiektów blob platformy Azure określoną bazą danych Azure SQL |    
| [Skopiuj z usług Salesforce z magazynem obiektów Blob platformy Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy) | Wdrażanie ten szablon utworzy factory Azure danych z potok kopiuje dane z określonego konta usług Salesforce z magazynem obiektów blob platformy Azure. |    
| [Przekształcanie danych, uruchamiając skrypt gałęzi w klastrze Azure HDInsight](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation) | Wdrażanie ten szablon utworzy factory Azure danych z potok przekształca dane, uruchamiając przykładowy skrypt gałęzi w klastrze Azure HDInsight Hadoop. | 


## <a name="samples-in-azure-portal"></a>Przykłady w Azure portal
Za pomocą kafelków **procesy przykładowy** na stronie głównej firmie danych wdrożenia potoki próbki i ich skojarzone jednostek (zestawy danych i usługi połączone), w firmie danych. 

1. Utwórz factory danych lub Otwórz istniejący factory danych. Zobacz [Wprowadzenie do Factory danych Azure](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md#CreateDataFactory) instrukcje tworzenie factory danych.
2. Na karta **FACTORY danych** dla factory danych kliknij Kafelek **procesy próbki** .

    ![Przykładowe procesy kafelków](./media/data-factory-samples/SamplePipelinesTile.png)

2. W karta **przykładowych procesy** kliknij **próbki** , którą chcesz wdrożyć. 
    
    ![Karta procesy przykładowy](./media/data-factory-samples/SampleTile.png)

3. Określ ustawienia konfiguracji dla próbki. Na przykład usługi klucz konta i nazwę konta Azure miejsca do magazynowania, nazwa serwera Azure SQL, bazy danych, identyfikator użytkownika i hasło, itp. 

    ![Przykładowa karta](./media/data-factory-samples/SampleBlade.png)

4. Po wykonaniu Określanie ustawień konfiguracji, kliknij przycisk **Utwórz** , aby utworzyć i wdrażanie procesy próbki i usług i tabele połączone używane przez procesy.
5. Zobaczysz stanu rozmieszczania na kafelku przykładowe kliknięte wcześniej na karta **procesy próbki** .

    ![Stan wdrażania](./media/data-factory-samples/DeploymentStatus.png)

6. Po wyświetleniu komunikatu **pomyślnie rozmieszczania** na kafelku dla próbki, zamknij karta **procesy próbki** .  
5. Na karta **FACTORY danych** widoczne, że usługi połączone, zestawy danych i procesy są dodawane do firmie danych.  

    ![Karta Factory danych](./media/data-factory-samples/DataFactoryBladeAfter.png)
   
## <a name="samples-in-visual-studio"></a>Przykłady w programie Visual Studio

### <a name="prerequisites"></a>Wymagania wstępne

Musi być zainstalowane na komputerze następujące elementy: 

- Visual Studio 2013 lub Visual Studio 2015 r.
- Pobierz Azure zestaw SDK programu Visual Studio 2013 lub program Visual Studio 2015 r. Przejdź do [Strony pobierania Azure](https://azure.microsoft.com/downloads/) , a następnie kliknij przycisk **2013 w PORÓWNANIU z** lub **2015 w PORÓWNANIU z** w sekcji **.NET** .
- Pobierz najnowszą wtyczki Azure Factory danych dla programu Visual Studio: [2013 w PORÓWNANIU z](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) lub [w PORÓWNANIU z 2015 r](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Jeśli korzystasz z programu Visual Studio 2013, możesz także zaktualizować wtyczkę, wykonując następujące czynności: W menu kliknij pozycję **Narzędzia** -> **rozszerzenia i aktualizacje** -> **Online** -> **Galerii Visual Studio** -> **Microsoft Azure danych Factory Tools for Visual Studio** -> **aktualizacji**.

### <a name="use-data-factory-templates"></a>Korzystanie z szablonów Factory danych

1. W menu kliknij pozycję **plik** , wskaż polecenie **Nowy**, a następnie kliknij pozycję **Projekt**. 
2. W oknie dialogowym **Nowy projekt** wykonaj następujące czynności: 
    1. Wybierz pozycję **DataFactory** w obszarze **Szablony**. 
    2. W okienku po prawej stronie wybierz pozycję **Szablony Factory danych** . 
    3. Wprowadź **nazwę** projektu. 
    4. Wybierz **lokalizację** dla projektu. 
    5. Kliknij **przycisk OK**. 

    ![Okno dialogowe nowego projektu](./media/data-factory-samples/vs-new-project-adf-templates.png)
6. W oknie dialogowym **Szablony Factory danych** Wybierz poniższy przykładowy szablon w sekcji **Szablony przypadków użycia** , a następnie kliknij przycisk **Dalej**. Poniższe kroki szczegółową przy użyciu szablonu **Profilowania odbiorców** . Czynności są podobne do innych próbek. 

    ![Okno dialogowe Factory szablonów danych](./media/data-factory-samples/vs-data-factory-templates-dialog.png) 
7. W oknie dialogowym **Konfiguracja Factory danych** kliknij przycisk **Dalej** na stronie **Podstawy Factory danych** .
8. Na stronie **Konfigurowanie factory danych** wykonaj następujące czynności: 
    1. Wybierz polecenie **Utwórz nowy Factory danych**. Można też zaznaczyć **Używanie istniejących danych factory**.
    2. Wprowadź **nazwę** dla factory danych.
    3. Wybierz **Azure subskrypcji** , w której ma zostać factory danych ma zostać utworzony. 
    4. Wybierz pozycję **Grupa zasobów** dla factory danych.
    5. Zaznacz **Zachód USA**, **USA wschodniego**lub **Północnej Europe** dla danego **regionu**.
    6. Kliknij przycisk **Dalej**. 
9. Na stronie **Konfigurowanie dane są przechowywane** określenie istniejącej **bazy danych Azure SQL** i **konto Azure miejsca do magazynowania** (lub) tworzenie bazy danych i miejsca do magazynowania, a następnie kliknij przycisk Dalej. 
10. Na stronie **Konfigurowanie obliczyć** wybierz ustawienia domyślne, a następnie kliknij przycisk **Dalej**. 
11. Na stronie **Podsumowanie** przejrzeć wszystkie ustawienia, a następnie kliknij przycisk **Dalej**. 
12. Na stronie **Stan wdrażania** poczekaj na zakończenie rozmieszczenia, a następnie kliknij przycisk **Zakończ**.
13. Kliknij prawym przyciskiem myszy projektu w Eksploratorze rozwiązań, a następnie kliknij pozycję **Publikuj**. 
19. Jeśli zostanie wyświetlone okno dialogowe **Zaloguj się do swojego konta Microsoft** , wprowadź poświadczenia konta, które ma Azure subskrypcję, a następnie kliknij przycisk **Zaloguj**.
20. Powinien zostać wyświetlony następujące okno dialogowe:

    ![Okno dialogowe publikowanie](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)

21. Na stronie **Konfigurowanie factory danych** wykonaj następujące czynności: 
    1. Upewnij się, ta opcja **Użyj istniejących danych factory** .
    2. Wybierz pozycję **factory danych** miał wybierz podczas korzystania z szablonu. 
    6. Kliknij przycisk **Dalej** , aby przejść na stronę **Publikowanie elementów** . (Naciśnij klawisz **TAB** , aby przenieść z pola Nazwa, jeśli jest wyłączona, przycisk **Dalej** ). 
23. Na stronie **Publikowanie elementów** upewnij się, że wszystkie fabryki danych jednostki są zaznaczone, a następnie kliknij przycisk **Dalej** , aby przejść na stronę **podsumowania** .     
24. Zapoznaj się z podsumowaniem i kliknij przycisk **Dalej** uruchomienie procesu wdrażania i wyświetlanie **Stanu rozmieszczania**.
25. Na stronie **Stanu rozmieszczania** powinna być widoczna stan procesu wdrażania. Po zakończeniu rozmieszczania, kliknij przycisk Zakończ. 

Szczegółowe informacje na temat autora podmioty Factory danych za pomocą programu Visual Studio i opublikowanie ich w Azure, zobacz [Tworzenie pierwszej factory danych (Visual Studio)](data-factory-build-your-first-pipeline-using-vs.md) .          