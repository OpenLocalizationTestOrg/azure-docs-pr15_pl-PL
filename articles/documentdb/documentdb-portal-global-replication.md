<properties
    pageTitle="Replikacja bazy danych globalnym DocumentDB | Microsoft Azure"
    description="Dowiedz się, jak zarządzać globalnej replikacji konta DocumentDB przez Azure portal."
    services="documentdb"
    keywords="Globalne bazy danych, replikacji"
    documentationCenter=""
    authors="mimig1"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="mimig"/>

# <a name="how-to-perform-documentdb-global-database-replication-using-the-azure-portal"></a>Jak przeprowadzić DocumentDB replikacji globalnej bazy danych za pomocą portalu Azure

Dowiedz się, jak korzystać z portalu Azure replikacji danych w wielu regionach globalnej dostępność danych w Azure DocumentDB.

Aby dowiedzieć się, jak globalnej bazie danych replikacji działa w DocumentDB, zobacz [dane Rozłóż globalnie DocumentDB](documentdb-distribute-data-globally.md). Aby dowiedzieć się, jak programowo wykonywania replikacji globalnej bazy danych zobacz [rozwijających się z kontami DocumentDB wielu region](documentdb-developing-with-multiple-regions.md).

> [AZURE.NOTE] Globalny rozkład DocumentDB baz danych jest zazwyczaj dostępny i automatycznie włączone dla wszystkich nowo utworzonych kont DocumentDB. Pracujemy nad włączyć globalnego rozkładu dla wszystkich istniejących kont, ale w międzyczasie ma być globalnym rozkładu włączone dla swojego konta, należy [kontakt z pomocą techniczną](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) i firma Microsoft będzie ją włączyć można teraz.

## <a id="addregion"></a>Dodawanie regionów globalnej bazie danych

DocumentDB jest dostępna w większości [Azure regionów] [azureregions]. Po wybraniu domyślny poziom spójności dla Twojego konta bazy danych, możesz skojarzyć jednego lub kilku regionów (zależnie od wyboru domyślnego spójności dystrybucyjną poziomu i globalnej potrzeb).

1. W [portalu Azure](https://portal.azure.com/)w Jumpbar kliknij polecenie **Konta DocumentDB**.
2. W karta **DocumentDB konta** wybierz konto bazy danych, aby zmodyfikować.
3. W karta konta kliknij **globalnie replikacji danych** z menu.
4. W karta **globalnie replikacji danych** Wybierz regiony, aby dodać lub usunąć i kliknij przycisk **Zapisz**. Istnieje koszt do dodawania regionów, zobacz [ceny strony](https://azure.microsoft.com/pricing/details/documentdb/) lub artykuł [danych Rozłóż globalnie DocumentDB](documentdb-distribute-data-globally.md) uzyskać więcej informacji.

    ![Kliknij pozycję regionów na mapie, aby dodać lub usunąć ich][1]

### <a name="selecting-global-database-regions"></a>Wybieranie regionów globalnej bazie danych

Konfigurując dwóch lub większej liczbie regionów, zaleca się zaznaczone regionów według pary region opisanego w [firm ciągłości i awarii odzyskiwania (BCDR): regiony sparowane Azure]  [ bcdr] artykuł.

W szczególności konfigurując do wielu regionów, upewnij się wybrać tę samą liczbę regionów (+/-1 dla stron parzystych i nawet) z każdej z kolumn iloczynów region. Na przykład jeśli chcesz wdrożyć na cztery obszary Stanów Zjednoczonych, możesz wybrać dwóch regionów USA z kolumnie po lewej stronie i dwie z prawej strony. Tak, następujące będzie odpowiedni zestaw: zachód USA, USA wschodniego Północna centralnej nam i Południowej centralnej nam.

Te wskazówki jest należy uwzględnić podczas tylko dwóch regionów są skonfigurowane dla scenariuszy odzyskiwania po awarii. Więcej niż dwóch regionów, zgodnie z zaleceniami ten jest zalecane, ale nie krytycznych, jak niektóre z wybranych regionach zgodny połączenie.

<!---
## <a id="selectwriteregion"></a>Select the write region

While all regions associated with your DocumentDB database account can serve reads (both, single item as well as multi-item paginated reads) and queries, only one region can actively receive the write (insert, upsert, replace, delete) requests. To set the active write region, do the following  


1. In the **DocumentDB Account** blade, select the database account to modify.
2. In the account blade, if the **All Settings** blade is not already opened, click **All Settings**.
3. In the **All Settings** blade, click **Write Region Priority**.
    ![Change the write region under DocumentDB Account > Settings > Add/Remove Regions][2]
4. Click and drag regions to order the list of regions. The first region in the list of regions is the active write region.
    ![Change the write region by reordering the region list under DocumentDB Account > Settings > Change Write Regions][3]
-->

## <a id="next"></a>Następne kroki

Dowiedz się, jak zarządzać spójności konta globalne zreplikowanej, czytając [poziomy spójności w DocumentDB](documentdb-consistency-levels.md).

Aby dowiedzieć się, jak globalnej bazie danych replikacji działa w DocumentDB, zobacz [dane Rozłóż globalnie DocumentDB](documentdb-distribute-data-globally.md). Aby dowiedzieć się, jak programowo replikacji danych w wielu regionów zobacz [rozwijających się z kontami DocumentDB wielu region](documentdb-developing-with-multiple-regions.md).

<!--Image references-->
[1]: ./media/documentdb-portal-global-replication/documentdb-add-region.png
[2]: ./media/documentdb-portal-global-replication/documentdb_change_write_region-1.png
[3]: ./media/documentdb-portal-global-replication/documentdb_change_write_region-2.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: https://azure.microsoft.com/documentation/articles/documentdb-consistency-levels/
[azureregions]: https://azure.microsoft.com/en-us/regions/#services
[offers]: https://azure.microsoft.com/en-us/pricing/details/documentdb/
