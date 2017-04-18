<properties
   pageTitle="Rozwiązywanie problemów ze zgodnością bazy danych programu SQL Server przed migracją do bazy danych SQL | Microsoft Azure"
   description="Microsoft Azure SQL Database, migracja bazy danych, informacje o zgodności, Kreator migracji SQL Azure, SSDT"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-migrate"
   ms.date="08/24/2016"
   ms.author="carlrab"/>

# <a name="migrate-a-sql-server-database-to-azure-sql-database-using-sql-server-data-tools-for-visual-studio"></a>Migrowanie bazy danych programu SQL Server bazą danych SQL Azure za pomocą programu SQL Server Data Tools for Visual Studio 

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Uaktualnianie Advisor](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

W tym artykule można Dowiedz się, jak wykrywanie i rozwiązywanie problemów ze zgodnością bazy danych programu SQL Server przy użyciu programu SQL Server Data Tools for Visual Studio przed migracją do bazy danych SQL Azure.

## <a name="using-sql-server-data-tools-for-visual-studio"></a>Za pomocą programu SQL Server Data Tools for Visual Studio

Zaimportować schemat bazy danych w projekcie bazy danych programu Visual Studio do analizy za pomocą programu SQL Server Data Tools for Visual Studio ("SSDT"). Aby przeprowadzić analizę, określić platforma docelowa projektu jako wersji 12 bazy danych SQL, a następnie skonstruuj projektu. Jeśli kompilacja zakończyło się pomyślnie, baza danych jest zgodne. Jeśli kompilacja nie powiedzie się, można rozwiązać błędy SSDT (lub jeden z innych narzędzi omówione w tym temacie). Po projektu tworzy się pomyślnie, możesz opublikować go jako kopię źródłowej bazy danych. Można następnie skopiuj dane z bazy danych źródłowych z bazą danych SQL Azure w wersji 12 zgodne za pomocą funkcji Porównaj dane w SSDT. Następnie należy przeprowadzić migrację tej zaktualizowanej bazy danych. Aby użyć tej opcji, Pobierz [najnowszą wersję SSDT](https://msdn.microsoft.com/library/mt204009.aspx).

  ![Diagram migracji VSSSDT](./media/sql-database-cloud-migrate/03VSSSDTDiagram.png)

  > [AZURE.NOTE] Jeśli wymagane jest tylko do schematu migracji, schemat można publikować bezpośrednio z programu Visual Studio bezpośrednio do bazy danych SQL Azure. Ta metoda schemat bazy danych wymaga zmiany nie są obsługiwane przez Kreatora migracji samodzielnie.

## <a name="detecting-compatibility-issues-using-sql-server-data-tools-for-visual-studio"></a>Wykrywanie problemy ze zgodnością przy użyciu programu SQL Server Data Tools for Visual Studio
   
1.  Otwórz **Eksploratora obiektów programu SQL Server** w programie Visual Studio. Nawiązywanie połączenia z wystąpienie programu SQL Server zawierającego bazę danych migrowanych za pomocą **Dodawanie programu SQL Server** . Zlokalizuj bazę danych w Eksploratorze obiektów, kliknij prawym przyciskiem myszy bazę danych i wybierz polecenie **Utwórz nowy projekt...**     
    
    ![Nowy projekt](./media/sql-database-migrate-visualstudio-ssdt/02MigrateSSDT.png)    
   
2.  Konfigurowanie ustawień importowania, aby **zaimportować tylko obiekty z zakresu aplikacji**. Wyczyść pole wyboru opcji importowania następujących: odwołanie do logowania, uprawnień i ustawień bazy danych.    

    ![tekst alternatywny](./media/sql-database-migrate-visualstudio-ssdt/03MigrateSSDT.png)    

3.  Kliknij przycisk **Rozpocznij** Importowanie bazy danych i Utwórz projekt zawierający plik skryptu T-SQL dla każdego obiektu w bazie danych. Pliki skryptów są umieszczane w folderach w projekcie.    

    ![tekst alternatywny](./media/sql-database-migrate-visualstudio-ssdt/04MigrateSSDT.png)    

4.  W Eksploratorze rozwiązań programu Visual Studio kliknij prawym przyciskiem myszy projekt bazy danych, a następnie wybierz pozycję Właściwości. Na stronie **Ustawienia projektu** skonfigurować docelowej platformy Microsoft Azure SQL bazy danych w wersji 12.    
    
    ![tekst alternatywny](./media/sql-database-migrate-visualstudio-ssdt/05MigrateSSDT.png)    
    
5.  Kliknij prawym przyciskiem myszy projektu, a następnie wybierz pozycję **Tworzenie** skompilować projektu.    
    
    ![tekst alternatywny](./media/sql-database-migrate-visualstudio-ssdt/06MigrateSSDT.png)    
    
6.  **Lista błędów** zawiera każdego niezgodności. W tym przypadku nazwa użytkownika NT\Usługa jest niezgodny. Ponieważ jest zgodna, możesz go komentarz lub go usunąć (i adresów do skutków usunięcia tego logowania i roli z rozwiązania bazy danych).     
    
    ![tekst alternatywny](./media/sql-database-migrate-visualstudio-ssdt/07MigrateSSDT.png)    
    
## <a name="fixing-compatibility-issues-using-sql-server-data-tools-for-visual-studio"></a>Rozwiązywanie problemów ze zgodnością przy użyciu programu SQL Server Data Tools for Visual Studio

1.  Kliknij dwukrotnie pierwszy skrypt, aby otworzyć skrypt w oknie kwerendy i komentarz się skrypt, a następnie wykonać skrypt.     
    ![tekst alternatywny](./media/sql-database-migrate-visualstudio-ssdt/08MigrateSSDT.png)

2.  Powtórz tę procedurę dla każdego skryptu zawierające niezgodności pozostało nie błędu.    
    ![tekst alternatywny](./media/sql-database-migrate-visualstudio-ssdt/09MigrateSSDT.png)
    
3.  W przypadku błędów bazy danych kliknij prawym przyciskiem myszy projektu, a następnie wybierz pozycję **Publikuj**. Kopia bazy danych źródłowych jest wbudowany i opublikowany (zdecydowanie zaleca się użycie kopii, co najmniej początkowo).     
 - Przed opublikowaniem, w zależności od wersji programu SQL Server źródła (starszej niż SQL Server 2014), może być konieczne Resetowanie platformy docelowej projektu w celu umożliwienia wdrożenia.     
 - Jeśli są migrowane starszej bazy danych programu SQL Server, nie wprowadzać funkcji do projektu, które nie są obsługiwane w źródle programu SQL Server do migracji bazy danych do nowszej wersji programu SQL Server.     

        ![alt text](./media/sql-database-migrate-visualstudio-ssdt/10MigrateSSDT.png)    
    
        ![alt text](./media/sql-database-migrate-visualstudio-ssdt/11MigrateSSDT.png)    
        
4.  W Eksploratorze obiektów serwera SQL kliknij prawym przyciskiem myszy źródłowej bazy danych, a następnie kliknij przycisk **Porównania danych**. Porównanie programu project w oryginalnej bazie danych ułatwia zrozumieć, jakie zmiany wprowadzono przez kreatora. Wybierz wersję programu SQL Azure w wersji 12 bazy danych, a następnie kliknij przycisk **Zakończ**.    
    
    ![tekst alternatywny](./media/sql-database-migrate-visualstudio-ssdt/12MigrateSSDT.png)    
    
    ![tekst alternatywny](./media/sql-database-migrate-visualstudio-ssdt/13MigrateSSDT.png)    

5.  Przeglądaj różnice wykryte, a następnie kliknij pozycję **Aktualizacji docelowej** przeprowadzić migrację danych ze źródłowej bazy danych do bazy danych SQL Azure w wersji 12.     
    
    ![tekst alternatywny](./media/sql-database-migrate-visualstudio-ssdt/14MigrateSSDT.png)    
    
6.  Wybierz metodę wdrożenia. Zobacz [Migrowanie zgodne bazy danych programu SQL Server do bazy danych SQL.](sql-database-cloud-migrate.md)  

## <a name="next-steps"></a>Następne kroki

- [Najnowsza wersja SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Najnowsza wersja programu SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Baza danych SQL w wersji 12](sql-database-v12-whats-new.md)
- [Transact-SQL częściowo lub nieobsługiwane funkcje](sql-database-transact-sql-information.md)
- [Migrowanie bazy danych serwera SQL za pomocą Asystenta migracji programu SQL Server](http://blogs.msdn.com/b/ssma/)
