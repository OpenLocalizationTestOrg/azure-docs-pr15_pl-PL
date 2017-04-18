<properties
    pageTitle="Skopiuj dane z magazynem obiektów Blob do bazy danych SQL | Microsoft Azure"
    description="Ten samouczek pokazano, jak za pomocą kopiowania aktywności w potoku Factory danych Azure skopiować dane z magazynem obiektów Blob do bazy danych SQL."
    Keywords="obiektów blob sql, magazyn obiektów blob kopiowanie danych"
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
    ms.date="09/26/2016"
    ms.author="spelluru"/>

# <a name="copy-data-from-blob-storage-to-sql-database-using-data-factory"></a>Skopiuj dane z magazynem obiektów Blob SQL bazy danych przy użyciu danych Factory 
> [AZURE.SELECTOR]
- [Omówienie i wymagania wstępne](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Kreator kopii](data-factory-copy-data-wizard-tutorial.md)
- [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Programu Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [Programu PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure szablonu Menedżera zasobów](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [INTERFEJSU API USŁUGI REST](data-factory-copy-activity-tutorial-using-rest-api.md)
- [INTERFEJS API PROGRAMU .NET](data-factory-copy-activity-tutorial-using-dotnet-api.md)


W tym samouczku możesz utworzyć factory danych z potok, aby skopiować dane z magazynem obiektów Blob do bazy danych SQL.

Działanie kopii wykonuje przenoszenia danych w Azure danych Factory. Jest on obsługiwany przez ogólnie dostępna usługa, która można kopiować danych między różnych magazynów w sposób bezpieczna, niezawodne i skalowalna. Zobacz artykuł [Działania przepływu danych](data-factory-data-movement-activities.md) szczegółowe informacje na temat działania Kopiuj.  

> [AZURE.NOTE] Szczegółowe omówienie usługi Factory danych zobacz artykuł [Wprowadzenie do fabryki danych Azure](data-factory-introduction.md) .

##<a name="prerequisites-for-the-tutorial"></a>Wymagania wstępne dotyczące przerabiania samouczka
Przed rozpoczęciem tego samouczka, musisz mieć następujące czynności:

- **Azure subskrypcji**.  Jeśli nie masz subskrypcji, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut. Zobacz artykuł [Bezpłatna wersja próbna](http://azure.microsoft.com/pricing/free-trial/) , aby uzyskać szczegółowe informacje.
- **Konto azure miejsca do magazynowania**. Magazyn obiektów blob za pomocą przechowywania danych **źródłowych** w tym samouczku. Jeśli nie masz konta usługi Azure miejsca do magazynowania, zobacz artykuł [Tworzenie konta miejsca do magazynowania](../storage/storage-create-storage-account.md#create-a-storage-account) dla czynności, aby utworzyć.
- **Baza danych SQL azure**. W tym samouczku za pomocą bazy danych programu Azure SQL przechowywania danych **miejsca docelowego** . Jeśli nie masz bazy danych programu Azure SQL, używanej w samouczku, zobacz [jak utworzyć i skonfigurować bazy danych SQL Azure](../sql-database/sql-database-get-started.md) go utworzyć.
- **SQL Server 2012-2014 lub Visual Studio 2013**. Program SQL Server Management Studio lub Visual Studio do tworzenia przykładowej bazy danych i wyświetlania danych wyników w bazie danych.  

## <a name="collect-blob-storage-account-name-and-key"></a>Zbieranie nazwę konta magazynu obiektów blob i klawiszy 
Potrzebujesz nazwę konta i klucz konta swojego konta Azure przestrzeni dyskowej, aby przerobić ten samouczek. Zanotuj **nazwę konta** i **klucz konta** dla Twojego konta Azure miejsca do magazynowania.

1. Zaloguj się do [portalu Azure](https://portal.azure.com/).
2. W lewym menu kliknij polecenie **więcej usług** i wybierz pozycję **Konta miejsca do magazynowania**.

    ![Przeglądanie - kont miejsca do magazynowania](media\data-factory-copy-data-from-azure-blob-storage-to-sql-database\browse-storage-accounts.png)
3. W karta **Miejsca do magazynowania konta** wybierz **konto Azure miejsca do magazynowania** , które ma być używany w tym samouczku.
4. Wybierz łącze **klawisze dostępu** , w obszarze **Ustawienia**.
5.  Kliknij przycisk **Kopiuj** (obraz) obok pola tekstowego **nazwę konta magazynu** i Zapisz i wklej go innym (na przykład: w pliku tekstowym).
6. Powtórz poprzedni krok do kopiowania lub zanotuj w **klucz1**.
    
    ![Klawisz dostępu miejsca do magazynowania](media\data-factory-copy-data-from-azure-blob-storage-to-sql-database\storage-access-key.png)
7. Zamknij wszystkie karty, klikając pozycję **X**.

## <a name="collect-sql-server-database-user-names"></a>Zbieranie bazy danych programu SQL server, nazwy użytkowników
Potrzebujesz nazwy użytkownika, aby przerobić ten samouczek, bazy danych i serwera Azure SQL. Zanotuj nazwy **serwera**, **bazy danych**i **użytkownika** bazy danych Azure SQL.

1. W **portalu Azure**po lewej stronie kliknij pozycję **więcej usług** i wybierz **baz danych programu SQL**.
2. W **Karta bazy danych SQL**wybierz **bazę danych** do użycia w tym samouczku. Zanotuj **nazwę bazy danych**.  
3. W karta **bazy danych SQL** w obszarze **Ustawienia**kliknij pozycję **Właściwości** .
4. Zanotuj wartości dla **Nazwy serwera** i **Logowanie do administratora serwera**.
5. Zamknij wszystkie karty, klikając pozycję **X**.

## <a name="allow-azure-services-to-access-sql-server"></a>Zezwalaj na usług Azure uzyskać dostęp do programu SQL server 
Upewnij się, **że ustawienie **Zezwalaj na dostęp do usług Azure** włączona dla serwera Azure SQL, aby usługi Factory danych można uzyskać dostęp do serwera Azure SQL** . Aby sprawdzić i włącz to ustawienie, wykonaj następujące czynności:

1. Kliknij pozycję Centrum **więcej usług** po lewej stronie, a następnie kliknij pozycję **serwery SQL Server**.
2. Zaznacz serwera, a następnie w obszarze **Ustawienia**kliknij pozycję **Zapora** . 
4. W karta **ustawienia zapory** dla **Zezwalaj na dostęp do usług Azure**kliknij przycisk **Dalej** .
5. Zamknij wszystkie karty, klikając pozycję **X**.

## <a name="prepare-blob-storage-and-sql-database"></a>Przygotuj magazyn obiektów Blob i baza danych SQL 
Teraz przygotować z magazynem obiektów blob Azure i baza danych Azure SQL samouczka, wykonując następujące czynności:  

1. Uruchom Notatnik, wklej następujący tekst i zapisz go jako **emp.txt** do folderu **C:\ADFGetStarted** na dysku twardym.

        John, Doe
        Jane, Doe

2. Aby utworzyć kontener **adftutorial** i przekazać plik **emp.txt** do kontenera za pomocą narzędzi, takich jak [Eksplorator magazynu Azure](https://azurestorageexplorer.codeplex.com/) .

    ![Eksplorator magazynu Azure. Skopiuj dane z magazynem obiektów Blob do bazy danych SQL](./media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/getstarted-storage-explorer.png)
3. Skorzystaj z tego skryptu SQL, aby utworzyć tabelę **emp** w bazie danych SQL Azure.  


        CREATE TABLE dbo.emp
        (
            ID int IDENTITY(1,1) NOT NULL,
            FirstName varchar(50),
            LastName varchar(50),
        )
        GO

        CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);

    **Jeśli program SQL Server 2012 2014-zainstalowany na Twoim komputerze:** postępuj zgodnie z instrukcjami [Krok 2: nawiązywanie połączenia z bazą danych SQL systemu zarządzania bazy danych SQL Azure za pomocą programu SQL Server Management Studio](../sql-database/sql-database-manage-azure-ssms.md#Step2) artykułu, aby połączyć się z serwerem Azure SQL i uruchamianie skryptu SQL. W tym artykule używa [Klasyczny portal Azure](http://manage.windowsazure.com), nie [Nowy portal Azure](https://portal.azure.com), aby skonfigurować zapory dla serwera Azure SQL.

    Jeśli klient nie może uzyskać dostęp do serwera Azure SQL, musisz skonfigurować zapory dla serwera Azure SQL umożliwić dostęp z komputera (adres IP). Zobacz [Ten artykuł](../sql-database/sql-database-configure-firewall-settings.md) , aby czynności, aby skonfigurować zapory dla serwera Azure SQL.

Ukończono wymagania wstępne. Możesz utworzyć fabryki danych przy użyciu jednej z następujących metod. Kliknij jedną z kart u góry lub poniższe łącza do wykonywania samouczka.     

- [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Programu Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [Programu PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [INTERFEJSU API USŁUGI REST](data-factory-copy-activity-tutorial-using-rest-api.md)
- [INTERFEJS API .NET](data-factory-copy-activity-tutorial-using-dotnet-api.md)
- [Kreator kopii](data-factory-copy-data-wizard-tutorial.md)
