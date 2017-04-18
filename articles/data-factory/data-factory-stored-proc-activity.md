<properties 
    pageTitle="Działania procedury przechowywane programu SQL Server" 
    description="Dowiedz się, jak za pomocą aktywności przechowywane procedury programu SQL Server do wywołania procedura składowana bazy danych SQL Azure lub magazynu danych SQL Azure z poziomu procesu Factory danych." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/30/2016" 
    ms.author="spelluru"/>

# <a name="sql-server-stored-procedure-activity"></a>Działania procedury przechowywane programu SQL Server
> [AZURE.SELECTOR]
[Gałąź](data-factory-hive-activity.md)  
[Świnka](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Przesyłanie strumieniowe Hadoop](data-factory-hadoop-streaming-activity.md)
[Maszynowego uczenia](data-factory-azure-ml-batch-execution-activity.md) 
[Procedura przechowywana](data-factory-stored-proc-activity.md)
[Analizy Lake danych U-SQL](data-factory-usql-activity.md)
[.NET niestandardowe](data-factory-use-custom-activities.md)

Działanie procedura składowana programu SQL Server w danych Factory [Planowana](data-factory-create-pipelines.md) umożliwia wywołania procedury przechowywanej w jednym z następujących magazynów: 


- Baza danych SQL Azure 
- Magazyn danych Azure SQL  
- Baza danych programu SQL Server w przedsiębiorstwie lub maszyn wirtualnych Azure. Należy zainstalować bramę zarządzania danymi na tym samym komputerze obsługującym bazę danych lub na komputerze osobnych, aby uniknąć uczestniczących w zawodach dla zasobów w bazie danych. Brama zarządzania danymi to oprogramowanie, który łączy lokalnych danych źródła danych źródeł hosed w maszyny wirtualne Azure z usługami w chmurze w sposób bezpieczny i zarządzanie nimi. Zobacz artykuł [Przenoszenie danych między wdrożeniem lokalnym a chmurą](data-factory-move-data-between-onprem-and-cloud.md) szczegółowe informacje na temat bramy zarządzania danymi. 

W tym artykule opiera się na artykuł [działania przekształcania danych](data-factory-data-transformation-activities.md) , w którym przedstawiono ogólne omówienie przekształcenie danych i działania transformacja obsługiwane.

## <a name="walkthrough"></a>Instruktaż

### <a name="sample-table-and-stored-procedure"></a>Przykładowa tabela i procedura składowana
1. Tworzenie w poniższej **tabeli** w bazie danych SQL Azure za pomocą programu SQL Server Management Studio lub innego narzędzia, które masz doświadczenia w. Kolumna datetimestamp zawiera datę i godzinę wygenerowania odpowiadający mu identyfikator. 

        CREATE TABLE dbo.sampletable
        (
            Id uniqueidentifier,
            datetimestamp nvarchar(127)
        )
        GO

        CREATE CLUSTERED INDEX ClusteredID ON dbo.sampletable(Id);
        GO

    Identyfikator jest unikatowy zidentyfikował i kolumna datetimestamp zawiera datę i godzinę wygenerowania odpowiadający mu identyfikator.
    ![Przykładowe dane](./media/data-factory-stored-proc-activity/sample-data.png)

    > [AZURE.NOTE] W tym przykładzie użyto bazy danych SQL Azure, ale działa w taki sam sposób dla magazynu danych SQL Azure i bazy danych programu SQL Server. 
2. Tworzenie poniższej **procedury składowanej** wstawiany danych w celu **sampletable**.

        CREATE PROCEDURE sp_sample @DateTime nvarchar(127)
        AS
        
        BEGIN
            INSERT INTO [sampletable]
            VALUES (newid(), @DateTime)
        END

    > [AZURE.IMPORTANT] **Nazwa** i **obudowy** parametru (DateTime w tym przykładzie) musi być zgodne z parametr w potoku/działaniu JSON. W definicji procedura składowana, upewnij się, że **@** jest używany jako prefiks parametru.
    
### <a name="create-a-data-factory"></a>Tworzenie factory danych  
4. Zaloguj się do [portalu Azure](https://portal.azure.com/). 
5. W menu po lewej stronie kliknij pozycję **Nowy** , kliknij pozycję **analizy + analizy**, a następnie kliknij przycisk **Factory danych**.
    
    ![Nowe factory danych](media/data-factory-stored-proc-activity/new-data-factory.png)   
4.  W karta **nowego factory danych** wprowadź **SProcDF** dla nazwy. Azure nazwy Factory danych są **globalnie unikatowe**. Należy nazwa fabryki danych przy użyciu Twojej nazwy, aby włączyć utworzenia fabryki prefiksu.

    ![Nowe factory danych](media/data-factory-stored-proc-activity/new-data-factory-blade.png)      
3.  Wybierz **subskrypcję Azure**. 
4.  **Grupa zasobów**wykonaj jedną z następujących czynności: 
    1.  Kliknij przycisk **Utwórz nowy** i wprowadź nazwę dla grupy zasobów.
    2.  Kliknij pozycję **Użyj istniejącego** i wybierz istniejącej grupy zasobów.  
5.  Wybierz **lokalizację** factory danych.
6.  Wybierz pozycję **Przypnij do pulpitu nawigacyjnego** , dzięki której będziesz widzieć factory danych na pulpicie nawigacyjnym po następnym zalogowaniu się. 
6.  Kliknij przycisk **Utwórz** na karta **factory nowych danych** .
6.  Zobacz factory danych tworzonych w **pulpicie nawigacyjnym** portalu Azure. Po factory danych został utworzony pomyślnie, zostanie wyświetlona strona factory danych, prezentującej zawartość fabryki danych.
    ![Strona główna Factory danych](media/data-factory-stored-proc-activity/data-factory-home-page.png)

### <a name="create-an-azure-sql-linked-service"></a>Tworzenie usługi połączone SQL Azure  
Po utworzeniu factory danych, możesz utworzyć usługa SQL Azure połączone, która łączy do fabryki danych bazy danych SQL Azure. Ta baza danych zawiera sampletable tabeli i sp_sample procedura składowana.

7.  Kliknij pozycję **Autor i wdrażanie** na karta **Factory danych** dla **SProcDF** uruchomić Edytor Factory danych.
2.  Kliknij przycisk **nowe dane przechowywane** na pasku poleceń i wybierz pozycję **Bazy danych SQL Azure**. Skrypt JSON związane z tworzeniem usługa SQL Azure połączone w edytorze powinny być widoczne. 

    ![Nowy magazyn danych](media/data-factory-stored-proc-activity/new-data-store.png)
4. W obszarze skrypt JSON wprowadzić następujące zmiany: 
    1. Zamienianie ** &lt;nazwa_serwera&gt; ** o nazwie serwera bazy danych SQL Azure.
    2. Zamienianie ** &lt;databasename&gt; ** z bazą danych, w którym został utworzony w tabeli i procedura składowana.
    3. Zamienianie ** &lt; username@servername ** z konta użytkownika, który ma dostęp do bazy danych.
    4. Zamienianie ** &lt;hasło&gt; ** przy użyciu hasła dla konta użytkownika. 

    ![Nowy magazyn danych](media/data-factory-stored-proc-activity/azure-sql-linked-service.png)
5. Kliknij pozycję **Rozmieść** na pasku poleceń do wdrożenia usługi połączone. Upewnij się, że jest wyświetlany AzureSqlLinkedService w widoku drzewa po lewej stronie. 

    ![Widok drzewa za pomocą usługi połączone](media/data-factory-stored-proc-activity/tree-view.png)

### <a name="create-an-output-dataset"></a>Utwórz zestaw danych wyjściowych
6. Kliknij przycisk **... Więcej** na pasku narzędzi kliknij pozycję **Nowy zestaw danych**, a następnie kliknij pozycję **Azure SQL**. **Nowy zestaw danych** na pasku poleceń i zaznacz opcję **Azure SQL**.

    ![Widok drzewa za pomocą usługi połączone](media/data-factory-stored-proc-activity/new-dataset.png)
7. Kopiowanie i wklejanie następujące skryptu JSON w edytorze JSON.

        {               
            "name": "sprocsampleout",
            "properties": {
                "type": "AzureSqlTable",
                "linkedServiceName": "AzureSqlLinkedService",
                "typeProperties": {
                    "tableName": "sampletable"
                },
                "availability": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
        }
7. Kliknij pozycję **Rozmieść** na pasku poleceń do wdrożenia zestawu danych. Upewnij się, że jest wyświetlany zestaw danych w widoku drzewa. 

    ![Widok drzewa z usługami połączone](media/data-factory-stored-proc-activity/tree-view-2.png)

### <a name="create-a-pipeline-with-sqlserverstoredprocedure-activity"></a>Utworzyć potok z działaniem SqlServerStoredProcedure
Teraz tworzenie potok z działaniem SqlServerStoredProcedure.
 
9. Kliknij przycisk **... Więcej** polecenie paska, a następnie kliknij przycisk **Nowy potoku**. 
9. Kopiowanie i wklejanie poniższy fragment JSON. Ustaw **storedProcedureName** **sp_sample**. Nazwa i obudowy parametru **daty i godziny** musi odpowiadać nazwę i obudowy parametru w definicji procedura składowana.  

        {
            "name": "SprocActivitySamplePipeline",
            "properties": {
                "activities": [
                    {
                        "type": "SqlServerStoredProcedure",
                        "typeProperties": {
                            "storedProcedureName": "sp_sample",
                            "storedProcedureParameters": {
                                "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                            }
                        },
                        "outputs": [
                            {
                                "name": "sprocsampleout"
                            }
                        ],
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        },
                        "name": "SprocActivitySample"
                    }
                ],
                "start": "2016-08-02T00:00:00Z",
                "end": "2016-08-02T05:00:00Z",
                "isPaused": false
            }
        }

    Jeśli potrzebujesz pomyślnie null parametru, użyj następującej składni: "parametr1": wartość null (pisany wyłącznie małymi literami). 
9. Kliknij pozycję **Rozmieść** na pasku narzędzi, aby wdrożyć proces.  

### <a name="monitor-the-pipeline"></a>Monitorowanie planowanej sprzedaży

6. Kliknij przycisk **X** , aby zamknąć Edytor Factory danych karty i przejść z powrotem do karta Factory danych, a następnie kliknij pozycję **Diagram**.

    ![Diagram kafelków](media/data-factory-stored-proc-activity/data-factory-diagram-tile.png)
7. W **Widoku diagramu**zobaczysz omówienie procesy i zestawy danych używane w tym samouczku. 

    ![Diagram kafelków](media/data-factory-stored-proc-activity/data-factory-diagram-view.png)
8. W widoku diagramu kliknij dwukrotnie zestawu danych **sprocsampleout**. Zobacz wycinków gotowa do drukowania. Powinny być pięć wycinki, ponieważ wycinek powstaje dla każdej godziny między czas rozpoczęcia i czas zakończenia w formacie JSON.

    ![Diagram kafelków](media/data-factory-stored-proc-activity/data-factory-slices.png) 
10. Gdy wycinek jest **gotowa** do drukowania, uruchamianie, * *Zaznacz* sampletable** zapytanie w bazie danych Azure SQL, aby zweryfikować, że wstawieniu danych w tabeli przez procedury składowanej.

    ![Dane wyjściowe](./media/data-factory-stored-proc-activity/output.png)

    Zobacz [Monitor proces](data-factory-monitor-manage-pipelines.md) szczegółowe informacje dotyczące monitorowania procesy Azure danych Factory.  

> [AZURE.NOTE] W tym przykładzie SprocActivitySample ma nie danych wejściowych. Jeśli chcesz rowerowy tego działania z działaniem powyżej (przetwarzanie poprzednich), wyjściowe działania nadrzędnego może być używany jako wartości wejściowych w tym działaniu. W takim przypadku to działanie nie wykonać, aż zostanie zakończone przed aktywności i wyjściowe nadrzędny działań są dostępne (w stan Gotowe). Nie można używać danych wejściowych bezpośrednio jako parametr do działania procedura składowana

## <a name="json-format"></a>JSON format
    {
        "name": "SQLSPROCActivity",
        "description": "description", 
        "type": "SqlServerStoredProcedure",
        "inputs":  [ { "name": "inputtable"  } ],
        "outputs":  [ { "name": "outputtable" } ],
        "typeProperties":
        {
            "storedProcedureName": "<name of the stored procedure>",
            "storedProcedureParameters":  
            {
                "param1": "param1Value"
                …
            }
        }
    }

## <a name="json-properties"></a>Właściwości JSON

Właściwość | Opis | Wymagane
-------- | ----------- | --------
Nazwa | Nazwa działania | Tak
Opis | Opis działania do czego służy | Brak
Typ | SqlServerStoredProcedure | Tak
wartości wejściowych | Opcjonalnie. Jeśli użytkownik określi zestaw danych wejściowych, muszą być dostępne (w stan "Gotowe") dla działania procedury składowanej do uruchomienia. Wprowadzania zestaw danych nie zużyte w procedura przechowywana jako parametru. Przed rozpoczęciem działań procedura składowana Sprawdź zależności jest jedynie. | Brak
Wyświetla | Należy określić zestaw wynik działania procedury składowanej. Dane wyjściowe zestawu danych określa **Harmonogram** wykonania procedury składowanej (co godzinę, co tydzień, co miesiąc, itp.). <br/><br/>Dane wyjściowe zestawu danych, należy użyć **usługi połączone** , która odwołuje się do bazy danych SQL Azure lub magazynu danych SQL Azure lub bazy danych programu SQL Server w której ma zostać procedura składowana, aby uruchomić. <br/><br/>Dane wyjściowe zestawu danych może służyć jako sposób przekazywania wynik procedury składowanej dla kolejnych przetwarzania przez innego działania ([łączenia działania](data-factory-scheduling-and-execution.md#chaining-activities)) w potoku. Jednak Factory danych nie automatycznie zapisuje dane wyjściowe procedura składowana do tego zestawu danych. Jest procedura przechowywana zapisuje tabeli SQL, wskazującego na dane wyjściowe zestawu danych. <br/><br/>W niektórych przypadkach dane wyjściowe zestaw danych może być **fikcyjna zestawu danych**, który jest używany tylko do określić harmonogram uruchamiania działania procedury składowanej. | Tak
storedProcedureName | Określ nazwę procedury przechowywanej w bazie danych Azure SQL lub magazynu danych SQL Azure, który jest reprezentowany przez usługę połączone, która korzysta z tabeli wyników. | Tak
storedProcedureParameters | Określ wartości parametrów procedura składowana. Jeśli musisz przekazać wartości zerowej parametru, użyj następującej składni: "parametr1": wartość null (wszystkie małe litery). Zobacz poniższy przykład, aby uzyskać informacje o korzystaniu z tej właściwości.| Brak

## <a name="passing-a-static-value"></a>Przekazywanie wartość statyczną 
Teraz przejdźmy Rozważ dodanie innej kolumny o nazwie "Scenariusz" w tabeli zawierającej wartość statyczną o nazwie "Przykładowy dokument".

![Przykładowe dane 2](./media/data-factory-stored-proc-activity/sample-data-2.png)

    CREATE PROCEDURE sp_sample @DateTime nvarchar(127) , @Scenario nvarchar(127)
    
    AS
    
    BEGIN
        INSERT INTO [sampletable]
        VALUES (newid(), @DateTime, @Scenario)
    END

Teraz przekazać parametr scenariusz oraz wartość z działania procedury składowanej. W sekcji typeProperties powyższego przykładu wygląda wstawkę kodu następujące czynności:

    "typeProperties":
    {
        "storedProcedureName": "sp_sample",
        "storedProcedureParameters": 
        {
            "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
            "Scenario": "Document sample"
        }
    }

