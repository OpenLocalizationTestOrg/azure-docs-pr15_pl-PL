<properties
    pageTitle="Wyłącz rozciąganie bazy danych i przywracanie danych zdalnych | Microsoft Azure"
    description="Dowiedz się, jak wyłączyć rozciąganie bazy danych dla tabeli i opcjonalnie zwracać dane zdalne."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/05/2016"
    ms.author="douglasl"/>

# <a name="disable-stretch-database-and-bring-back-remote-data"></a>Wyłącz rozciąganie bazy danych i przywracanie danych zdalnych

Aby wyłączyć rozciąganie bazy danych dla tabeli, wybierz pozycję **Rozciągnij** dla tabeli w programie SQL Server Management Studio. Następnie wybierz jedną z następujących opcji.

-   Wyłącz **| Przenoszenie danych z powrotem z Azure**. Kopiowanie zdalnych danych dla tabeli z platformy Azure z powrotem do programu SQL Server, następnie wyłącz rozciąganie bazy danych dla tabeli. Operacja wiąże się z kosztami przekazania danych i nie można anulować.

-   Wyłącz **| Pozostawić dane w Azure**. Wyłączanie rozciąganie bazy danych dla tabeli.  Opuszczenie zdalnych danych dla tabeli w Azure.

Można również użyć Transact\-kod SQL, aby wyłączyć rozciąganie bazy danych do tabeli lub bazy danych.

Po wyłączeniu rozciąganie bazy danych dla tabeli, stopnie migracji danych i wyników kwerendy zawiera już wyniki z tabeli zdalnej.

Jeśli chcesz po prostu Zatrzymaj wskaźnik myszy migracji danych, zobacz [Wstrzymywanie i wznawianie rozciąganie bazy danych](sql-server-stretch-database-pause.md).

>   [AZURE.NOTE] Wyłączanie rozciąganie bazy danych do tabeli lub bazy danych nie powoduje usunięcia obiektu zdalnego. Aby usunąć tabeli zdalnej lub zdalna baza danych, musisz upuść ją za pomocą portalu zarządzania Azure. Zdalny obiektów w dalszym ciągu ponoszenia kosztów Azure, dopóki nie zostaną usunięte. Aby uzyskać więcej informacji zobacz [Ceny bazy danych rozciąganie programu SQL Server](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

## <a name="disable-stretch-database-for-a-table"></a>Wyłączanie rozciąganie bazy danych dla tabeli

### <a name="use-sql-server-management-studio-to-disable-stretch-database-for-a-table"></a>Wyłączanie funkcji rozciąganie bazy danych dla tabeli przy użyciu programu SQL Server Management Studio

1.  W programie SQL Server Management Studio w Eksploratorze obiektów zaznacz tabelę, dla którego chcesz wyłączyć rozciąganie bazy danych.

2.  Prawo\-kliknij i wybierz pozycję **Rozciągnij**, a następnie wybierz jedną z następujących opcji.

    -   Wyłącz **| Przenoszenie danych z powrotem z Azure**. Kopiowanie zdalnych danych dla tabeli z platformy Azure z powrotem do programu SQL Server, następnie wyłącz rozciąganie bazy danych dla tabeli. Nie można anulować tego polecenia.

        >   [AZURE.NOTE] Kopiowanie zdalnych danych dla tabeli z platformy Azure Wstecz, aby program SQL Server wiąże się z kosztami przekazania danych. Aby uzyskać więcej informacji zobacz [Szczegóły ceny przeniesienia danych](https://azure.microsoft.com/pricing/details/data-transfers/).

        Po wszystkich skopiowanych danych zdalnych z platformy Azure z powrotem do programu SQL Server, rozciąganie jest wyłączone dla tabeli.

    -   Wyłącz **| Pozostawić dane w Azure**. Wyłączanie rozciąganie bazy danych dla tabeli.  Opuszczenie zdalnych danych dla tabeli w Azure.

    >   [AZURE.NOTE] Wyłączanie rozciąganie bazy danych dla tabeli nie powoduje usunięcia dane zdalne lub tabeli zdalnej. Aby usunąć tabelę zdalnego, musisz upuść ją za pomocą portalu zarządzania Azure. Tabeli zdalnej w dalszym ciągu ponoszenia kosztów Azure, dopóki nie zostaną usunięte. Aby uzyskać więcej informacji zobacz [Ceny bazy danych rozciąganie programu SQL Server](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

### <a name="use-transact-sql-to-disable-stretch-database-for-a-table"></a>Używanie Transact\-kod SQL, aby wyłączyć rozciąganie bazy danych dla tabeli

-   Aby wyłączyć rozciąganie dla tabeli i skopiuj zdalnego danych dla tabeli z platformy Azure z powrotem do programu SQL Server, uruchom następujące polecenie. Po wszystkich skopiowanych danych zdalnych z platformy Azure z powrotem do programu SQL Server, rozciąganie jest wyłączone dla tabeli.

    Nie można anulować tego polecenia.

    ```tsql
    USE <Stretch-enabled database name>;
    GO
    ALTER TABLE <Stretch-enabled table name>  
       SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ;
    GO
    ```
    >   [AZURE.NOTE] Kopiowanie zdalnych danych dla tabeli z platformy Azure Wstecz, aby program SQL Server wiąże się z kosztami przekazania danych. Aby uzyskać więcej informacji zobacz [Szczegóły ceny przeniesienia danych](https://azure.microsoft.com/pricing/details/data-transfers/).

-   Aby wyłączyć rozciąganie dla tabeli i dane zdalne zrezygnować, uruchom następujące polecenie.

    ```tsql
    ALTER TABLE <table_name>
       SET ( REMOTE_DATA_ARCHIVE = OFF_WITHOUT_DATA_RECOVERY ( MIGRATION_STATE = PAUSED ) ) ;
    ```

>   [AZURE.NOTE] Wyłączanie rozciąganie bazy danych dla tabeli nie powoduje usunięcia dane zdalne lub tabeli zdalnej. Aby usunąć tabelę zdalnego, musisz upuść ją za pomocą portalu zarządzania Azure. Tabeli zdalnej w dalszym ciągu ponoszenia kosztów Azure, dopóki nie zostaną usunięte. Aby uzyskać więcej informacji zobacz [Ceny bazy danych rozciąganie programu SQL Server](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

## <a name="disable-stretch-database-for-a-database"></a>Wyłączanie rozciąganie bazy danych w bazie danych
Przed rozciąganie bazy danych można wyłączyć dla bazy danych, musisz wyłączyć rozciąganie bazy danych na poszczególnych rozciąganie\-włączone tabel w bazie danych.

### <a name="use-sql-server-management-studio-to-disable-stretch-database-for-a-database"></a>Wyłączanie funkcji rozciąganie bazy danych dla bazy danych przy użyciu programu SQL Server Management Studio

1.  W programie SQL Server Management Studio w Eksploratorze obiektów wybierz bazę danych, dla którego chcesz wyłączyć rozciąganie bazy danych.

2.  Prawo\-kliknij i wybierz **zadania**, a następnie wybierz pozycję **Rozciągnij**, a następnie zaznacz pole wyboru **Wyłącz**.

>   [AZURE.NOTE] Wyłączanie rozciąganie bazy danych dla bazy danych nie powoduje usunięcia zdalna baza danych. Jeśli chcesz usunąć ze zdalną bazą danych, musisz upuść ją za pomocą portalu zarządzania Azure. Zdalna baza danych jest nadal ponoszenia kosztów Azure, dopóki nie zostaną usunięte. Aby uzyskać więcej informacji zobacz [Ceny bazy danych rozciąganie programu SQL Server](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

### <a name="use-transact-sql-to-disable-stretch-database-for-a-database"></a>Używanie Transact\-kod SQL, aby wyłączyć rozciąganie bazy danych w bazie danych
Uruchom następujące polecenie.

```tsql
ALTER DATABASE <database name>
    SET REMOTE_DATA_ARCHIVE = OFF ;
```

>   [AZURE.NOTE] Wyłączanie rozciąganie bazy danych dla bazy danych nie powoduje usunięcia zdalna baza danych. Jeśli chcesz usunąć ze zdalną bazą danych, musisz upuść ją za pomocą portalu zarządzania Azure. Zdalna baza danych jest nadal ponoszenia kosztów Azure, dopóki nie zostaną usunięte. Aby uzyskać więcej informacji zobacz [Ceny bazy danych rozciąganie programu SQL Server](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

## <a name="see-also"></a>Zobacz też

[Instrukcja ALTER DATABASE — opcje SET (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx)

[Wstrzymywanie i wznawianie rozciąganie bazy danych](sql-server-stretch-database-pause.md)
