<properties
    pageTitle="Przenoszenie baz danych między serwerami między subskrypcje i się Azure."
    description="Szybkie kroki, aby skopiować, przenieść, a następnie przeprowadzić migrację danych i baz danych w bazie danych SQL Azure."
    services="sql-database"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="move-databases-between-servers-between-subscriptions-and-in-and-out-of-azure"></a>Przenoszenie baz danych między serwerami między subskrypcje i się Azure

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]
##<a name="to-move-a-database-to-a-different-server-in-the-same-subscription"></a>Aby przenieść bazy danych na innym serwerze w tej samej subskrypcji
- W [Azure Portal](https://portal.azure.com)kliknij **baz danych programu SQL**, wybierz bazę danych z listy, a następnie kliknij polecenie **Kopiuj**. Aby uzyskać więcej szczegółów, zobacz [Kopiowanie bazy danych programu Azure SQL](sql-database-copy.md) .

## <a name="to-move-a-database-between-subscriptions"></a>Przenoszenie bazy danych między subskrypcji
- W [Azure Portal](https://portal.azure.com)kliknij pozycję **serwery SQL** , a następnie wybierz serwer, który obsługuje bazy danych z listy. Kliknij przycisk **Przenieś**, a następnie wybierz zasoby, aby przenieść i subskrypcji, aby przejść do.

## <a name="to-migrate-a-sql-database-into-azure"></a>Aby przeprowadzić migrację z bazą danych SQL do Azure
- Ustalić zgodności bazy danych, a następnie wybierz Metoda migracji prawo, w zależności od potrzeb. Wykonaj wskazówki i opcje migracji [bazy danych programu SQL Server](sql-database-cloud-migrate.md).

## <a name="to-create-a-copy-of-a-database-for-use-outside-of-azure"></a>Aby utworzyć kopię bazy danych do użytku poza Azure
- [Eksportowanie pliku PLECAK.](sql-database-export.md)
