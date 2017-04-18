<properties
    pageTitle="Monitorowanie wydajności bazy danych w bazie danych SQL Azure | Microsoft Azure"
    description="Więcej informacji na temat opcji monitorowania bazy danych przy użyciu narzędzia Azure i dynamiczne zarządzanie widoków."
    keywords="Monitorowanie wydajności bazy danych w chmurze bazy danych"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="09/27/2016"
    ms.author="carlrab"/>

# <a name="monitoring-database-performance-in-azure-sql-database"></a>Monitorowanie wydajności bazy danych w bazie danych SQL Azure
Monitorowanie wydajności bazy danych SQL platformy Azure zaczyna się monitorowanie wykorzystania zasobów względem poziom wydajności bazy danych, które możesz wybrać. Monitorowanie pomaga określić, czy ma pojemność nadmiarowego bazy danych, czy występują problemy, ponieważ zasoby są wyczerpane, a następnie określ, czy nadszedł czas, aby dostosować poziom wydajności i [warstwa usług](sql-database-service-tiers.md) bazy danych. Można monitorować bazy danych przy użyciu narzędzi graficznych w [Azure portal](https://portal.azure.com) lub SQL [dynamiczne zarządzanie widokami](https://msdn.microsoft.com/library/ms188754.aspx).

## <a name="monitor-databases-using-the-azure-portal"></a>Monitorowanie bazy danych za pomocą portalu Azure

W [portalu Azure](https://portal.azure.com/)można monitorować użycie pojedynczej bazy danych, wybierając bazy danych i klikając wykres **monitorowania** . Spowoduje to wyświetlenie **metryki** okno, w którym można zmienić, klikając przycisk **Edytuj wykres** . Dodaj następujące wskaźniki:

- Procesor procent
- Procent DTU
- Procent Jo danych
- Procent rozmiaru bazy danych

Po dodaniu tych metryki można wyświetlać je na wykresie **monitorowania** bardziej szczegółowo w oknie **metryki** . Wszystkie cztery metryki Pokaż procent średnie wykorzystanie względem **DTU** bazy danych. Zobacz artykuł [warstwy usługi](sql-database-service-tiers.md) , aby uzyskać szczegółowe informacje o DTUs.

![Usługa warstwa monitorowanie wydajności bazy danych.](./media/sql-database-service-tiers/sqldb_service_tier_monitoring.png)

Można również skonfigurować alerty na miar wydajności. Kliknij przycisk **Dodaj alertu** w oknie **metryki** . Wykonaj kreatora, aby skonfigurować alert. Istnieje możliwość alert o metryki przekroczeniu określonej wartości progowej lub jeśli Metryka przypada poniżej określonej wartości progowej.

Na przykład jeśli obciążenie pracą w bazie danych, aby powiększyć, możesz skonfigurować alert e-mail przy każdym bazy danych osiągnie 80% w dowolnej z pomiarów wydajności. Umożliwia to jako wczesnego ostrzeżenia wiadomo, gdy trzeba przełączyć się do następnego wyższy poziom wydajności.

Wskaźniki wydajności pomaga ustalić, czy można przekonwertować na starszą wersję na niższy poziom wydajności. Przyjęto założenie, jest używany Standard S2 bazy danych i wszystkie wskaźniki pokazać, że baza danych średnio nie korzysta z więcej niż 10% w dowolnym momencie. Prawdopodobnie działanie bazy danych w standardowej S1. Należy jednak pamiętać o obciążeń zwiększyć lub zmieniają się przed podjęciem decyzji, aby przenieść na niższy poziom wydajności.

## <a name="monitor-databases-using-dmvs"></a>Monitorowanie bazy danych przy użyciu DMVs

Dostępne widoki systemowe są także samej metryk, które są dostępne w portalu: [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) w logiczne **wzorca** bazy danych serwera i [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) w bazie danych użytkownika. Użyj **sys.resource_stats** , jeśli chcesz monitorować dane mniej szczegółowy przez okres dłuższy czas. Jeśli potrzebujesz bardziej szczegółowe dane w mniejszym przedziale czasu można monitorować za pomocą **sys.dm_db_resource_stats** . Aby uzyskać więcej informacji zobacz [Wskazówki wydajności bazy danych SQL Azure](sql-database-performance-guidance.md#monitoring-resource-use-with-sysresourcestats).

>[AZURE.NOTE] **sys.dm_db_resource_stats** pusty przypadku zwraca zestaw wyników używane w sieci Web i Business edition bazy danych, które są wycofana.

Puli elastyczne bazy danych można monitorować pojedyncze bazy danych w puli techniki opisane w tej sekcji. Ale można również monitorować puli jako całość. Aby uzyskać informacje, zobacz [Monitor i zarządzanie nimi puli elastyczne bazy danych](sql-database-elastic-pool-manage-portal.md).
