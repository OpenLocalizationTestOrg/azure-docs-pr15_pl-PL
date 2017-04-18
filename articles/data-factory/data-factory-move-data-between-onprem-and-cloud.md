<properties 
    pageTitle="Przenoszenie danych — brama zarządzania danymi | Microsoft Azure"
    description="Konfigurowanie bramy danymi przenoszenie danych między lokalnym a chmurą. Przenoszenie danych za pomocą bramy zarządzania danymi w Azure danych Factory." 
    keywords="Brama danymi, integracji danych, przenoszenie danych, poświadczeń bramy"
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="jingwang"/>

# <a name="move-data-between-on-premises-sources-and-the-cloud-with-data-management-gateway"></a>Przenoszenie danych między lokalnych źródeł i chmura z bramy zarządzania danymi
Ten artykuł zawiera omówienie integracji danych między lokalnego zbioru danych i chmury magazynów danych przy użyciu danych Factory. Opiera się na artykuł [Działania przepływu danych](data-factory-data-movement-activities.md) i innych danych factory podstawowych pojęć artykułów: [zestawy danych](data-factory-create-datasets.md) i [procesy](data-factory-create-pipelines.md). 

## <a name="data-management-gateway"></a>Brama zarządzania danymi
Należy zainstalować bramę zarządzania danymi na komputerze lokalnym umożliwiające przenoszenie danych z lokalnego magazynu danych. Brama można zainstalować na tym samym komputerze do przechowywania danych lub na innym komputerze, jak bramy można nawiązać magazynu danych. 

> [AZURE.IMPORTANT] Zobacz artykuł [Bramy zarządzania danymi](data-factory-data-management-gateway.md) szczegółowe informacje na temat bramy zarządzania danymi.   

Następujące instruktażu przedstawiono sposób tworzenia factory danych z procesu, który umożliwia przeniesienie danych z bazy danych **Programu SQL Server** lokalnego z magazynem obiektów blob platformy Azure. W ramach Instruktaż zainstalować i skonfigurować bramę zarządzania danymi na komputerze. 

## <a name="walkthrough-copy-on-premises-data-to-cloud"></a>Przewodnik: kopiowanie danych lokalnych do chmury
  
## <a name="create-data-factory"></a>Tworzenie factory danych
W tym kroku umożliwia Azure portal utworzyć wystąpienia Azure Factory danych o nazwie **ADFTutorialOnPremDF**. 

1.  Zaloguj się do [portalu Azure](https://portal.azure.com). 
2.  Kliknij pozycję **+ Nowy**, kliknij pozycję **analizy + analizy**, a następnie kliknij przycisk **Factory danych**.

    ![Nowy -> DataFactory](./media/data-factory-move-data-between-onprem-and-cloud/NewDataFactoryMenu.png)  
2. W karta **nowego factory danych** wprowadź **ADFTutorialOnPremDF** dla nazwy.

    ![Dodawanie do Startboard](./media/data-factory-move-data-between-onprem-and-cloud/OnPremNewDataFactoryAddToStartboard.png)

    > [AZURE.IMPORTANT] 
    > Nazwa fabryki Azure danych musi być globalnie unikatowa. Jeśli zostanie wyświetlony komunikat o błędzie: **factory danych o nazwie "ADFTutorialOnPremDF" nie jest dostępny**, Zmień nazwę fabryki danych (na przykład yournameADFTutorialOnPremDF) i spróbuj ponownie utworzyć. Użyj tej nazwy zamiast ADFTutorialOnPremDF podczas wykonywania pozostałe kroki opisane w tym samouczku.
    > 
    > Nazwa fabryki danych może być zarejestrowana w przyszłości i w związku z tym stają się publicznie widoczne nazwy **DNS** .
3. Wybierz miejsce, w którym chcesz factory danych do utworzenia **Azure subskrypcji** . 
4.  Wybieranie istniejącej **grupy zasobów** lub Utwórz nową grupę zasobów. Samouczek, należy utworzyć grupę zasobów o nazwie: **ADFTutorialResourceGroup**. 
5.  Kliknij przycisk **Utwórz** na karta **factory nowych danych** .

    > [AZURE.IMPORTANT] Aby utworzyć wystąpienia Factory danych, musi być członkiem roli [Współautora Factory dane](../active-directory/role-based-access-built-in-roles.md/#data-factory-contributor) na poziomie grupy subskrypcji i zasobów. 
11. Po zakończeniu tworzenia możesz zobaczyć karta **Factory danych** , jak pokazano na poniższej ilustracji:

    ![Strona główna Factory danych](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDataFactoryHomePage.png)

## <a name="create-gateway"></a>Tworzenie bramy
5. Karta **Factory danych** kliknij **Autor i wdrażanie** kafelków, aby uruchomić **Edytor** dla factory danych.

    ![Tworzenie i wdrażanie kafelków](./media/data-factory-move-data-between-onprem-and-cloud/author-deploy-tile.png) 
6.  W edytorze Factory danych, kliknij przycisk **... Więcej** na pasku narzędzi, a następnie kliknij pozycję **Nowa brama danych**. Można także w widoku drzewa kliknij prawym przyciskiem myszy **Bram danych** i kliknij przycisk **Nowa brama danych**. 

    ![Nowa brama danych na pasku narzędzi](./media/data-factory-move-data-between-onprem-and-cloud/NewDataGateway.png)
2. W karta **Tworzenie** wprowadzić **adftutorialgateway** **nazwę**i kliknij **przycisk OK**.    

    ![Tworzenie karta bramy](./media/data-factory-move-data-between-onprem-and-cloud/OnPremCreateGatewayBlade.png)
3. W karta **konfiguracji** kliknij przycisk **Zainstaluj bezpośrednio na tym komputerze**. Ta akcja do pobrania pakiet instalacyjny bramy, instaluje konfiguruje i rejestruje bramy na tym komputerze.  

    > [AZURE.NOTE] 
    > Za pomocą programu Internet Explorer lub przeglądarki sieci web zgodne ClickOnce firmy Microsoft.
    > 
    > Jeśli używasz przeglądarki Chrome, przejdź do [Sklep internetowy Chrome](https://chrome.google.com/webstore/)wyszukiwanie przy użyciu słów kluczowych "ClickOnce", wybierz jedną z rozszerzeniem ClickOnce i zainstaluj ją. 
    >  
    > Zrób to samo dla przeglądarki Firefox (Zainstaluj dodatek). Kliknij przycisk **Otwórz Menu** na pasku narzędzi (**trzy linie poziome** w prawym górnym rogu), kliknij pozycję **Dodatki**, wyszukiwanie przy użyciu słów kluczowych "ClickOnce", wybierz jedną z rozszerzeniem ClickOnce i zainstaluj go.    

    ![Brama — Konfigurowanie karta](./media/data-factory-move-data-between-onprem-and-cloud/OnPremGatewayConfigureBlade.png)

    W ten sposób jest najprostszym sposobem (jednym kliknięciem) pobieranie, instalowanie, konfigurowanie i zarejestruj bramę w jednym kroku pojedynczy. Możesz sprawdzić, czy aplikacja **Menedżer konfiguracji bramy zarządzania danymi firmy Microsoft** jest zainstalowana na komputerze. Możesz również znaleźć wykonywalny **ConfigManager.exe** w folderze: **C:\Program Files\Microsoft danych zarządzania Gateway\2.0\Shared**.

    Można również pobieranie i instalowanie bramy ręcznie za pomocą łącza w tym karta i zarejestruj, używając klawisza wyświetlane w polu tekstowym **Nowy klucz** .
    
    Zobacz artykuł [Bramy zarządzania danymi](data-factory-data-management-gateway.md) szczegółowe informacje o bramy.

    >[AZURE.NOTE] Musisz być administratorem na komputerze lokalnym, aby zainstalować i skonfigurować bramę zarządzania danymi pomyślnie. Możesz dodać dodatkowych użytkowników do lokalnej grupy **Użytkowników bramy zarządzania danymi** w systemie Windows. Członkowie tej grupy mogą umożliwia skonfigurowanie bramy narzędzie Menedżer konfiguracji bramy zarządzania danymi. 

5. Poczekaj kilka minut i poczekaj, aż zostanie wyświetlony następujący komunikat powiadomienia:

    ![Instalacja powiodła się bramy](./media/data-factory-move-data-between-onprem-and-cloud/gateway-install-success.png) 
6. Uruchom aplikację **Menedżer konfiguracji bramy zarządzania danymi** na komputerze. W oknie **Wyszukiwanie** wpisz **Bramy zarządzania danymi** , aby uzyskać dostęp do tego narzędzia. Możesz również znaleźć wykonywalny **ConfigManager.exe** w folderze: **C:\Program Files\Microsoft danych zarządzania Gateway\2.0\Shared** 

    ![Menedżer konfiguracji bramy](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDMGConfigurationManager.png)
6. Upewnij się, że jest wyświetlany `adftutorialgateway is connected to the cloud service` wiadomości. Pasek dołu stanu wyświetla **Połączono z usług w chmurze** oraz **zielony znacznik wyboru**.

    Na karcie **Narzędzia główne** możesz wykonać następujące czynności: 
    - **Zarejestruj** bramę przy użyciu klucza z portalu Azure za pomocą przycisku Rejestruj. 
    - **Zatrzymywanie** usługi hosta bramy zarządzania danymi na komputerze bramy. 
    - **Planowanie aktualizacji** musi być zainstalowany w określonym czasie dnia. 
    - Sprawdzanie, kiedy brama została **Data ostatniej aktualizacji**.
    - Określ czas, w którym można zainstalować aktualizację do bramy. 

8. Przejdź na kartę **Ustawienia** . Certyfikat określony w sekcji **certyfikat** jest używany do Szyfrowanie/odszyfrowywanie poświadczenia dla lokalnego magazynu danych przez użytkownika w portalu. (opcjonalnie) Kliknij przycisk **Zmień** , aby użyć własnego certyfikatu. Domyślnie bramy używa certyfikat, który jest generowane automatycznie przez usługę Factory danych.

    ![Konfiguracji certyfikat bramy](./media/data-factory-move-data-between-onprem-and-cloud/gateway-certificate.png)

    Możesz również wykonać następujące akcje na karcie **Ustawienia** : 
    - Wyświetlanie i eksportowanie certyfikatu używany przez bramę.
    - Zmienianie punktu końcowego HTTPS używane przez bramę.    
    - Ustaw serwer proxy HTTP, może być używany przez bramę.   
9. (opcjonalnie) Przejdź do karty **Narzędzia diagnostyczne** , zaznacz opcję **Włącz rejestrowanie** , jeśli chcesz włączyć rejestrowanie, które umożliwiają rozwiązywanie problemów związanych z bramą. Rejestrowanie informacji można znaleźć w **Podglądzie zdarzeń** w obszarze **Dzienniki aplikacji i usług** -> węzeł**Bramy zarządzania danymi** . 

    ![Karta Narzędzia diagnostyczne](./media/data-factory-move-data-between-onprem-and-cloud/diagnostics-tab.png)

    Na karcie **Narzędzia diagnostyczne** można również wykonać następujące czynności: 
    
    - Użyj sekcji **Testuj połączenie** ze źródłem danych lokalnych przy użyciu bramy.
    - Kliknij pozycję **Wyświetl dzienniki** , aby wyświetlić dziennik bramy zarządzania danymi w oknie Podgląd zdarzeń. 
    - Kliknij pozycję **Wyślij dzienniki** przekazywanie pliku zip z dzienników ostatnich siedmiu dni do firmy Microsoft, aby ułatwić rozwiązywanie problemów związanych z problemów związanych z. 
10. W sekcji **Testuj połączenie** na karcie **Narzędzia diagnostyczne** wybierz **SQL** dla typu magazynu danych, wprowadź nazwę serwera bazy danych, nazwę bazy danych, określ typ uwierzytelniania, wprowadź nazwę użytkownika i hasło i kliknij pozycję **Przetestuj** , aby sprawdzić, czy bramy nawiązać połączenie z bazą danych. 
11. Przełączanie w przeglądarce sieci web, a w **Azure portal**, karta **konfiguracji** , a następnie na karta **Nowa brama danych** , kliknij przycisk **OK** .
6. Powinien zostać wyświetlony **adftutorialgateway** w obszarze **Bram danych** w widoku drzewa po lewej stronie.  Po kliknięciu go powinna być widoczna skojarzone JSON. 
    

## <a name="create-linked-services"></a>Tworzenie połączonych usług 
W tym kroku utworzyć dwie usługi połączone: **AzureStorageLinkedService** i **SqlServerLinkedService**. **SqlServerLinkedService** łączy bazy danych programu SQL Server w środowisku lokalnym i przechowywania obiektów blob platformy Azure łącza powiązanych z **AzureStorageLinkedService** factory danych. Możesz utworzyć potok w dalszej części tego instruktażu, który kopiuje dane z bazy danych programu SQL Server lokalnego w magazynie obiektów blob platformy Azure. 

#### <a name="add-a-linked-service-to-an-on-premises-sql-server-database"></a>Dodawanie usługi połączone z bazą danych programu SQL Server lokalnego
1.  W oknie **Edytor Factory danych**na pasku narzędzi kliknij pozycję **Przechowywanie nowe dane** i wybierz **Programu SQL Server**. 

    ![Nowe usługi SQL Server połączone](./media/data-factory-move-data-between-onprem-and-cloud/NewSQLServer.png) 
3.  W **Edytorze JSON** po prawej stronie wykonaj następujące czynności: 
    1. Określ **adftutorialgateway** **gatewayName**. 
    2. W elemencie **connectionString**wykonaj następujące czynności: 
        1. **Nazwa_serwera**wprowadź nazwę serwera, który obsługuje bazę danych programu SQL Server.
        2. **Databasename**wprowadź nazwę bazy danych.
        3. Kliknij pozycję **Szyfruj** przycisku na pasku narzędzi. To do pobrania i uruchamia aplikację Menedżer poświadczeń.
        
            ![Aplikacja Menedżer poświadczeń](./media/data-factory-move-data-between-onprem-and-cloud/credentials-manager-application.png)
        5. W oknie dialogowym **Ustawianie poświadczeń** Określ typ uwierzytelniania, nazwę użytkownika i hasło, a następnie kliknij **przycisk OK**. Jeśli połączenie zostanie nawiązane, zaszyfrowane poświadczenia są przechowywane w formacie JSON i zamyka okno dialogowe. 
        6. Zamknij kartę puste przeglądarki, która uruchomić okno dialogowe, jeśli nie zostanie automatycznie zamknięty i wrócić do karty Portal Azure. 
  
            Na komputerze bramy te poświadczenia są **szyfrowane** za pomocą certyfikatu usługi Factory danych należą. Jeśli chcesz używać certyfikatu, który jest skojarzony z bramy zarządzania danymi w zamian, zobacz [Ustawianie poświadczeń bezpieczne](#set-credentials-and-security).    
    1.  Kliknij pozycję **Rozmieść** na pasku poleceń do wdrożenia usługi SQL Server połączone. Powinien zostać wyświetlony usługi połączone w widoku drzewa. 
        
        ![Usługi SQL Server połączone w widoku drzewa](./media/data-factory-move-data-between-onprem-and-cloud/sql-linked-service-in-tree-view.png)  

#### <a name="add-a-linked-service-for-an-azure-storage-account"></a>Dodawanie usługi połączone konta magazynu platformy Azure
 
1. W oknie **Edytor Factory danych**kliknij **nowe dane przechowywane** na pasku poleceń, a następnie kliknij przycisk **Azure miejsca do magazynowania**.
2. Wprowadź nazwę konta magazynu platformy Azure **nazwę konta**.
3. Wprowadź klucz dla Twojego konta Azure miejsca do magazynowania dla **klucz konta**.
4. Kliknij pozycję **Deploy** wdrożenia **AzureStorageLinkedService**.
   
 
## <a name="create-datasets"></a>Tworzenie zestawów danych
W tym kroku utworzyć dane wejściowe i wyjściowe zestawy danych, które reprezentują dane wejściowe i wyjściowe dla operacji kopiowania (baza danych programu SQL Server lokalnego = > magazyn obiektów blob platformy Azure). Przed utworzeniem zestawy danych, wykonaj następujące czynności (uzyskać szczegółowe instrukcje poniżej listy):

- Tworzenie tabeli o nazwie **emp** w bazie danych serwera SQL, które zostały dodane jako powiązanych z fabryki danych i wstawić kilka przykładowych wpisy do tabeli.
- Tworzenie kontenera obiektów blob o nazwie **adftutorial** w dodanego jako usługi połączone do fabryki danych konta magazyn obiektów blob platformy Azure.

### <a name="prepare-on-premises-sql-server-for-the-tutorial"></a>Przygotowanie lokalnego programu SQL Server samouczka

1. W bazie danych, określonej dla programu SQL Server w lokalnej usługi połączone (**SqlServerLinkedService**), skorzystaj z tego skryptu SQL Aby utworzyć tabelę **emp** w bazie danych.

        CREATE TABLE dbo.emp
        (
            ID int IDENTITY(1,1) NOT NULL, 
            FirstName varchar(50),
            LastName varchar(50),
            CONSTRAINT PK_emp PRIMARY KEY (ID)
        )
        GO 
2. Wstawianie kilka przykładowych do tabeli: 

        INSERT INTO emp VALUES ('John', 'Doe')
        INSERT INTO emp VALUES ('Jane', 'Doe')

### <a name="create-input-dataset"></a>Tworzenie zestawu wprowadzania danych

1. W **Edytorze Factory danych**kliknij przycisk **... Więcej**, kliknij przycisk **Nowy zestaw danych** na pasku poleceń i kliknij przycisk **Tabela programu SQL Server**. 
2.  Zastąp JSON w okienku po prawej stronie następujący tekst:
        
            {       
                "name": "EmpOnPremSQLTable",
                "properties": {
                    "type": "SqlServerTable",
                    "linkedServiceName": "SqlServerLinkedService",
                    "typeProperties": {
                        "tableName": "emp"
                    },
                    "external": true,
                    "availability": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "policy": {
                        "externalData": {
                            "retryInterval": "00:01:00",
                            "retryTimeout": "00:10:00",
                            "maximumRetry": 3
                        }
                    }
                }
            }    

    Zwróć uwagę następujące punkty: 

    - **Typ** jest ustawiony na **SqlServerTable**.
    - **tableName** ustawiono **emp**.
    - **linkedServiceName** jest ustawiona na **SqlServerLinkedService** (utworzono tej usługi połączone wcześniej w tym instruktażu.).
    - Do wprowadzania zestaw danych nie jest generowany przez innego planowana w Azure Factory danych należy ustawić **zewnętrznych** **wartość PRAWDA**. Go oznacza, że dane wejściowe jest wyprodukowano z usługą Azure Factory danych zewnętrznych. Opcjonalnie można określić wszelkie zasady danych zewnętrznych za pomocą elementu **externalData** w sekcji **zasady** .    

    Aby uzyskać szczegółowe informacje o właściwościach JSON, zobacz [Przenoszenie danych z programu SQL Server](data-factory-sqlserver-connector.md) .
2. Kliknij pozycję **Rozmieść** na pasku poleceń do wdrożenia zestawu danych.  


### <a name="create-output-dataset"></a>Tworzenie zestawu danych wyjściowych danych

1.  W **Edytorze Factory danych**kliknij przycisk **Nowy zestaw danych** na pasku poleceń, a następnie kliknij przycisk **Magazyn obiektów Blob platformy Azure**.
2.  Zastąp JSON w okienku po prawej stronie następujący tekst: 

            {
                "name": "OutputBlobTable",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "AzureStorageLinkedService",
                    "typeProperties": {
                        "folderPath": "adftutorial/outfromonpremdf",
                        "format": {
                            "type": "TextFormat",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Hour",
                        "interval": 1
                    }
                }
            }
  
    Zwróć uwagę następujące punkty: 
    
    - **Typ** jest ustawiony na **AzureBlob**.
    - **linkedServiceName** jest ustawiona na **AzureStorageLinkedService** (utworzono tej usługi połączone w kroku 2).
    - **ścieżkafolderu** jest ustawiona na **adftutorial-outfromonpremdf** , gdzie outfromonpremdf jest folderem w kontenerze adftutorial. Tworzenie kontenera **adftutorial** , jeśli jeszcze nie istnieje. 
    - **Dostępność** jest ustawiona na **co godzinę** (**Częstotliwość** **godzinę** i **interwału** równa **1**).  Usługa danych Factory generuje wycinek wynik co godzinę w tabeli **emp** bazy danych SQL Azure. 

    Jeśli nie określisz **nazwę pliku** dla **tabeli wyników**, wygenerowane pliki w **ścieżkafolderu** są nazywane w następującym formacie: danych. <Guid>txt (na przykład:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).

    Aby ustawić **ścieżkafolderu** i **nazwę pliku** dynamicznie na podstawie **SliceStart** czasu, należy użyć właściwości partitionedBy. W poniższym przykładzie ścieżkafolderu została użyta rok, miesiąc i dzień z SliceStart (godzina rozpoczęcia wycinka przetwarzane) i godzinę od SliceStart używa nazwy pliku. Na przykład wycinek jest wyprodukowane 2014 r-10-20T08:00:00, nazwa_folderu jest ustawiona na wikidatagateway/wikisampledataout/2014-10-20 i nazwę pliku jest ustawiona na 08.csv. 

        "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
        "fileName": "{Hour}.csv",
        "partitionedBy": 
        [
            { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
            { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
            { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
            { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
        ],

 
    Aby uzyskać szczegółowe informacje o właściwościach JSON, zobacz [Przenoszenie danych z magazynem obiektów Blob platformy Azure](data-factory-azure-blob-connector.md) .
2.  Kliknij pozycję **Rozmieść** na pasku poleceń do wdrożenia zestawu danych. Upewnij się, czy jest widoczny zarówno zestawów danych w widoku drzewa.  

## <a name="create-pipeline"></a>Tworzenie procesu
W tym kroku możesz utworzyć **potok** z jednej **Kopii aktywności** używającej **EmpOnPremSQLTable** jako danych wejściowych i **OutputBlobTable** jako wynik.

1.  W edytorze Factory danych kliknij **... Więcej**i kliknij pozycję **Nowy potoku**. 
2.  Zastąp JSON w okienku po prawej stronie następujący tekst: 
    
            {
                "name": "ADFTutorialPipelineOnPrem",
                "properties": {
                "description": "This pipeline has one Copy activity that copies data from an on-prem SQL to Azure blob",
                "activities": [
                {
                    "name": "CopyFromSQLtoBlob",
                    "description": "Copy data from on-prem SQL server to blob",
                    "type": "Copy",
                    "inputs": [
                    {
                        "name": "EmpOnPremSQLTable"
                    }
                    ],
                    "outputs": [
                    {
                        "name": "OutputBlobTable"
                      }
                    ],
                    "typeProperties": {
                      "source": {
                        "type": "SqlSource",
                        "sqlReaderQuery": "select * from emp"
                      },
                      "sink": {
                        "type": "BlobSink"
                      }
                    },
                    "Policy": {
                      "concurrency": 1,
                      "executionPriorityOrder": "NewestFirst",
                      "style": "StartOfInterval",
                      "retry": 0,
                      "timeout": "01:00:00"
                    }
                  }
                ],
                "start": "2016-07-05T00:00:00Z",
                "end": "2016-07-06T00:00:00Z",
                "isPaused": false
              }
            }

    > [AZURE.IMPORTANT]
    > Wartość właściwości **rozpocząć** należy zastąpić bieżącą wartość dnia i **zakończenia** z następnego dnia.

    Zwróć uwagę następujące punkty:
 
    - W sekcji działania jest tylko działanie, którego **Typ** jest ustawiona na **kopii**.
    - **Danych wejściowych** dla działania jest ustawiona na **EmpOnPremSQLTable** , a **wynik** działania jest ustawiona na **OutputBlobTable**.
    - W sekcji **typeProperties** **SqlSource** jest określony jako **Typ źródła** i **BlobSink **jest określony jako **Typ sink**.
    - Kwerenda SQL `select * from emp` jest określona dla właściwości **sqlReaderQuery** **SqlSource**.

     Oba Uruchom i dat zakończenia musi być w [formacie ISO](http://en.wikipedia.org/wiki/ISO_8601). Na przykład: 2014-10-14T16:32:41Z. Czas **zakończenia** jest opcjonalne, ale firma Microsoft korzysta w tym samouczku. 
    
    Jeśli nie określisz wartości dla właściwości **zakończenia** , jest obliczana jako "**start + 48 godzin**". Aby uruchomić proces czas nieokreślony, określ **9-9-9999** jako wartość właściwości **zakończenia** . 
    
    Definiowania czas trwania, w jakiej fragmenty danych są przetwarzane na podstawie właściwości **dostępności** , zdefiniowane dla każdego zestawu danych Azure danych Factory.
    
    W tym przykładzie istnieje 24 fragmenty danych podczas każdego wycinek powstaje co godzinę.     
2. Kliknij pozycję **Rozmieść** na pasku poleceń do wdrożenia zestawu danych (tabeli jest prostokątnym zestawu danych). Upewnij się, że proces są widoczne w widoku drzewa w węźle **procesy** .  
5. Teraz kliknij przycisk **X** , dwa razy, aby zamknąć karty, aby wrócić do karta **Factory danych** dla **ADFTutorialOnPremDF**.

**Gratulacje!** Pomyślnie utworzony factory Azure danych, usługi połączone, zestawy danych i potok i zaplanowane proces.

#### <a name="view-the-data-factory-in-a-diagram-view"></a>Wyświetlanie factory danych w widoku diagramu 
1. W **portalu Azure**kliknij **diagramu** na stronie głównej factory danych **ADFTutorialOnPremDF** . :

    ![Łącza diagramu](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramLink.png)

2. Powinien zostać wyświetlony diagram podobne na poniższej ilustracji:

    ![Widok diagramu](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramView.png)

    Możesz powiększyć, pomniejszyć, powiększenie 100%, powiększenie do dopasowania, automatycznie umieść potoki i zestawy danych i wyświetlić informacje dotyczące elementów nadrzędnych (wyróżnia powyżej i poniżej elementy wybranych elementów).  Dwukrotne kliknięcie obiektu (wejścia i wyjścia zestawu danych lub potoku), aby wyświetlić właściwości. 

## <a name="monitor-pipeline"></a>Potok monitora
W tym kroku umożliwia Azure portal monitorowania, co się dzieje w factory Azure danych. Za pomocą poleceń cmdlet monitorowania zestawy danych i procesy. Aby uzyskać szczegółowe informacje dotyczące monitorowania zobacz [Monitorowanie i zarządzanie procesy](data-factory-monitor-manage-pipelines.md).

5. Na diagramie kliknij dwukrotnie **EmpOnPremSQLTable**.  

    ![Wycinki EmpOnPremSQLTable](./media/data-factory-move-data-between-onprem-and-cloud/OnPremSQLTableSlicesBlade.png)

6. Zwróć uwagę, że wszystkie dane plasterki w górę są w stanie **gotowości** ponieważ czas trwania planowana (godzina rozpoczęcia do czasu zakończenia) jest w przeszłości. Należy również ponieważ umieszczono dane w bazie danych programu SQL Server, a jest zawsze. Upewnij się, że wycinków nie są wyświetlane w sekcji **wycinków Problem** u dołu. Aby wyświetlić wszystkie wycinki, kliknij pozycję **Zobacz więcej** u dołu listy wycinków. 
7. Teraz w karta **zestawy danych** , kliknij przycisk **OutputBlobTable**.

    ![Wycinki OputputBlobTable](./media/data-factory-move-data-between-onprem-and-cloud/OutputBlobTableSlicesBlade.png)
9. Kliknij dowolny wycinek danych z listy i powinna być widoczna karta **Wycinek** . Widać, że trwa aktywności wycinek. Zobacz tylko jedno działanie zazwyczaj uruchamianie.  

    ![Karta wycinek danych](./media/data-factory-move-data-between-onprem-and-cloud/DataSlice.png)

    Jeśli wycinek nie jest w stanie **gotowości** , zobaczysz nadrzędny wycinki, które nie są gotowe i blokują bieżący fragment wykonywanie na liście **nadrzędny wycinki, które są nie jest gotowy** .

10. Kliknij przycisk **Uruchom aktywności** z listy u dołu, aby zobaczyć **działanie uruchamianie szczegóły**.

    ![Karta Szczegóły uruchamianie aktywności](./media/data-factory-move-data-between-onprem-and-cloud/ActivityRunDetailsBlade.png)

    Czy informacje takie jak przepustowość, czas trwania i bramy używany do przesyłania danych. 
11. Kliknij przycisk **X** , aby zamknąć wszystkie karty dopiero po 
12. wrócić do karta Narzędzia główne dla **ADFTutorialOnPremDF**.
14. (opcjonalnie) Kliknij **procesy**, kliknij pozycję **ADFTutorialOnPremDF**i przechodzenie do za pomocą tabel wejściowych (**zużyto**) lub dane wyjściowe zestawów danych (**produkcji**).
15. Aby sprawdzić, czy obiektów blob plik jest tworzony dla każdej godziny za pomocą narzędzi, takich jak korzystanie z narzędzi takich jak [Miejsca do magazynowania w Eksploratorze](http://storageexplorer.com/) .

    ![Eksplorator magazynu platformy Azure](./media/data-factory-move-data-between-onprem-and-cloud/OnPremAzureStorageExplorer.png)

## <a name="next-steps"></a>Następne kroki

- Zobacz artykuł [Bramy zarządzania danymi](data-factory-data-management-gateway.md) szczegółowe informacje o bramy zarządzania danymi.
- Zobacz [Kopiowanie danych z obiektów Blob platformy Azure SQL Azure](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) , aby informacje na temat kopiowania działanie służy do przenoszenia danych z magazynu danych źródłowych do magazynu danych sink. 
