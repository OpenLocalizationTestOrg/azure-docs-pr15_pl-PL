<properties
    pageTitle="Wydajność i cena elastyczne puli bazy danych SQL"
    description="Informacje o cenach specyficzne dla pul elastyczne bazy danych."
    services="sql-database"
    documentationCenter=""
    authors="srinia"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="05/27/2016"
    ms.author="srinia"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="elastic-database-pool-billing-and-pricing-information"></a>Rozliczenia puli elastyczne bazy danych i informacje o cenach

Pule elastyczne bazy danych są wystawiona na następujące cechy:

- Elastyczne puli jest wystawiona po jego utworzeniu, nawet wtedy, gdy istnieje żadnych baz danych w puli.
- Elastyczne puli jest wystawiona co godzinę. Jest to taką samą częstotliwością pomiarowe dla poziomów wydajności jednej baz danych.
- Jeśli elastyczne puli jest zmieniony na nową wartość eDTUs, następnie puli jest nie dotyczy rozliczenie zgodnie z nową ilość eDTUS dopiero po zakończeniu operacji zmiany rozmiaru. Wynika to takim samym wzorcem jako zmiana poziomu wydajności autonomicznego baz danych.


- Cena puli elastyczne jest oparty na liczbę eDTUs puli. Cena puli elastyczne zależy od liczby i wykorzystania elastycznych baz danych znajdujące się w nim.
- Cena jest obliczana przez (liczba eDTUs puli) x (ceny jednostkowej eDTU).

Cena jednostkowa eDTU elastyczne puli jest wyższy niż ceny jednostkowe DTU autonomicznego bazy danych w tej samej warstwie usługi. Aby uzyskać szczegółowe informacje zobacz [ceny bazy danych SQL](https://azure.microsoft.com/pricing/details/sql-database/). 


Aby zrozumieć poziomów eDTUs i usług, zobacz [Opcje bazy danych SQL i wydajności](sql-database-service-tiers.md).

## <a name="next-steps"></a>Następne kroki

- [Tworzenie puli elastyczne bazy danych](sql-database-elastic-pool-create-portal.md)
- [Monitorowanie, zarządzanie i rozmiaru puli elastyczne bazy danych](sql-database-elastic-pool-manage-portal.md)
- [Opcje bazy danych SQL i wydajności: opis, jakie opcje są dostępne w każdej warstwie usługi](sql-database-service-tiers.md)
- [Skrypt programu PowerShell do identyfikowania baz danych odpowiednie dla puli elastyczne bazy danych](sql-database-elastic-pool-database-assessment-powershell.md)
