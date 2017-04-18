<properties
   pageTitle="Wprowadzenie wykazu Azure danych Lake analizy U-SQL | Azure"
   description="Wprowadzenie wykaz Azure danych Lake analizy U-SQL"
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="use-u-sql-catalog"></a>Za pomocą wykazu U SQL

Wykaz U SQL służy do struktury danych i kodu, może być udostępniane przez skrypty U-SQL. Wykaz umożliwia najwyższą wydajność możliwe z danymi w Lake danych Azure.

Każde konto Azure danych Lake analizy ma dokładnie jedną wykazu U SQL skojarzone z nim. Nie można usunąć katalogu U-SQL. Obecnie wykazy U SQL nie można udostępniać między kontami magazynu Lake danych.

Każdy wykaz U SQL zawiera bazy danych o nazwie **wzorca**. Nie można usunąć wzorzec bazy danych.  Każdego wykazu U SQL może zawierać więcej dodatkowych baz danych.

Baza danych U SQL zawiera:

- Zestawy — udostępnianie kodu .NET między skryptów U SQL.
- Wartości tabeli funkcji — Udostępnianie kodu U SQL między skryptów U SQL.
- Tabele — udostępnianie danych między skryptów U SQL.
- Schematy — udostępnianie schematy tabel między skryptów U SQL.

## <a name="manage-catalogs"></a>Zarządzanie katalogi
Każde konto Azure danych Lake analizy ma domyślne konto Azure magazynu Lake danych skojarzone z nim. To konto magazynu Lake danych jest określany jako konto domyślne magazynu Lake danych. Wykaz U SQL jest przechowywany w domyślnego konta magazynu Lake danych w folderze/Catalog. Nie usuwaj żadnych plików w folderze/Catalog.

### <a name="use-azure-portal"></a>Azure portal za pomocą

Zobacz [Zarządzanie analizy Lake danych za pomocą portalu](data-lake-analytics-manage-use-portal.md#view-u-sql-catalog)


### <a name="use-data-lake-tools-for-visual-studio"></a>Użyj narzędzia Lake danych dla programu Visual Studio.

Narzędzia Lake danych dla programu Visual Studio umożliwia zarządzanie wykazu.  Aby uzyskać więcej informacji o narzędziach zobacz [Narzędzia Lake danych przy użyciu programu Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

**Aby zarządzać wykazu**

1. Otwórz program Visual Studio i połącz Azure. Aby uzyskać instrukcje zobacz [Łączenie z Azure](data-lake-analytics-data-lake-tools-get-started.md#connect-to-azure).
1. Otwórz **Eksploratora serwera** , naciśnij klawisze **CTRL + ALT + S**.
2. Korzystając z **Eksploratora serwera**rozwiń **Azure**, rozwiń węzeł **Analizy Lake danych**, rozwiń konta analizy Lake danych, rozwiń węzeł **bazy danych**, a następnie rozwiń **wzorcowych**.



    - Aby dodać nową bazę danych, kliknij prawym przyciskiem myszy **bazę danych**, a następnie kliknij **Tworzenie bazy danych**.
    - Aby dodać nowy zestaw, kliknij prawym przyciskiem myszy **zespołów**, a następnie kliknij **Zestaw Zarejestruj**.
    - Aby dodać nowy schemat, kliknij prawym przyciskiem myszy **schematów**, a następnie kliknij "Tworzenie schematu **.
    - Aby dodać nową tabelę, kliknij prawym przyciskiem myszy **tabele**, a następnie kliknij "" Tworzenie tabeli **.
    - Aby dodać nową funkcję zwracające tabelę, zobacz [operatorów dla zadań analizy Lake danych zdefiniowane przez użytkownika można opracowywać U-SQL](data-lake-analytics-u-sql-develop-user-defined-operators.md).


![Przeglądanie katalogów Visual Studio U SQL](./media/data-lake-analytics-use-u-sql-catalog/data-lake-analytics-browse-catalogs.png)


## <a name="see-also"></a>Zobacz też

- Rozpoczynanie pracy
    - [Wprowadzenie do analizy Lake danych za pomocą portalu Azure](data-lake-analytics-get-started-portal.md)
    - [Wprowadzenie do analizy Lake danych przy użyciu programu PowerShell Azure](data-lake-analytics-get-started-powershell.md)
    - [Wprowadzenie do analizy Lake danych przy użyciu zestawu SDK .NET Azure](data-lake-analytics-get-started-net-sdk.md)
    - [Można opracowywać skrypty U SQL za pomocą narzędzia Lake danych dla programu Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
    - [Wprowadzenie do języka Azure danych Lake analizy U-SQL](data-lake-analytics-u-sql-get-started.md)

- U SQL i rozwój
    - [Wprowadzenie do języka Azure danych Lake analizy U-SQL](data-lake-analytics-u-sql-get-started.md)
    - [Użyj funkcji okna U SQL Azure danych Lake analizy zadań](data-lake-analytics-use-window-functions.md)
    - [Można opracowywać U SQL operatorów zdefiniowanych przez użytkownika dla zadań analizy Lake danych](data-lake-analytics-u-sql-develop-user-defined-operators.md)

- Zarządzanie
    - [Zarządzanie analizy Lake danych Azure za pomocą portalu Azure](data-lake-analytics-manage-use-portal.md)
    - [Zarządzanie analiz Lake danych Azure za pomocą programu PowerShell Azure](data-lake-analytics-manage-use-powershell.md)
    - [Monitorowanie i rozwiązywanie problemów z Azure danych Lake analizy zadań przy użyciu Azure portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

- Samouczek zakończenia do końca
    - [Interaktywne podręczniki przedstawiają analizy Lake danych Azure za pomocą](data-lake-analytics-use-interactive-tutorials.md)
    - [Analizowanie dzienniki witryny sieci Web przy użyciu analizy Lake danych Azure](data-lake-analytics-analyze-weblogs.md)
