<properties
    pageTitle="Monitorowanie miejsca do magazynowania w pamięci XTP | Microsoft Azure"
    description="Szacowanie i monitorowanie miejsca do magazynowania w pamięci XTP korzystać, zdolności; Naprawianie błędu zdolności 41823"
    services="sql-database"
    documentationCenter=""
    authors="jodebrui"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="jodebrui"/>


# <a name="monitor-in-memory-oltp-storage"></a>Monitorowanie magazynowania OLTP w pamięci

Używając [OLTP w pamięci](sql-database-in-memory.md), dane w tabelach zoptymalizowane pamięci i zmienne tabeli znajdują się w magazynie OLTP w pamięci. Każdej warstwie usługi Premium jest maksymalny rozmiar miejsca do magazynowania w pamięci OLTP, które podano w [artykule warstwy usługi bazy danych SQL](sql-database-service-tiers.md#service-tiers-for-single-databases). Po przekroczeniu tego limitu wstawiać i aktualizować może zostać uruchomiony operacje niepowodzeniu (błąd 41823). W tym momencie będzie potrzebny dowolnego usunięcie danych do odzyskania pamięci i uaktualnianie warstwie wydajności bazy danych.

## <a name="determine-whether-data-will-fit-within-the-in-memory-storage-cap"></a>Określanie, czy dane będą pasować zakończenie przechowywania w pamięci

Określanie zakończenie miejsca do magazynowania: można znaleźć w [artykule warstwy usługi bazy danych SQL](sql-database-service-tiers.md#service-tiers-for-single-databases) caps miejsca do magazynowania różnych poziomów usługi Premium.

Szacowanie wymagania dotyczące pamięci działa zoptymalizowane pamięci tabela ma taki sam sposób dla programu SQL Server, ponieważ w bazie danych SQL Azure. Potrwać kilka minut, aby zapoznać się z tym tematem w [witrynie MSDN](https://msdn.microsoft.com/library/dn282389.aspx).

Zauważ, że tabela i zmiennych wiersze tabeli, a także indeksy, są wliczane do użytkownika maksymalny rozmiar danych. Ponadto ALTER TABLE muszą miejsca, aby utworzyć nową wersję całą tabelę i jego indeksy.

## <a name="monitoring-and-alerting"></a>Monitorowanie i alerty

Można monitorować użycie miejsca do magazynowania w pamięci jako procent [Caps miejsca do magazynowania dla swojego poziomu wydajności](sql-database-service-tiers.md#service-tiers-for-single-databases) w [portal](https://portal.azure.com/)Azure: 

- Na karta bazy danych zlokalizuj pole wykorzystania zasobów i kliknij pozycję Edytuj.
- Następnie wybierz pozycję Metryka `In-Memory OLTP Storage percentage`.
- Aby dodać alert, kliknij w polu wykorzystania zasobów, aby otworzyć metryczne karta, a następnie wybierz polecenie Dodaj alert.

Lub Pokaż wykorzystanie miejsca do magazynowania w pamięci za pomocą następującej kwerendy:

    SELECT xtp_storage_percent FROM sys.dm_db_resource_stats


## <a name="correct-out-of-memory-situations---error-41823"></a>Poprawianie sytuacjach brakiem pamięci - błąd 41823

Uruchamianie wyników brakiem pamięci w operacje WSTAWIANIA, aktualizacji i Utwórz niepowodzeniem z powodu błędu 41823.

Komunikat o błędzie 41823 wskazuje, że tabele zoptymalizowane pamięci i zmienne tabeli został przekroczony maksymalny rozmiar.

Aby rozwiązać ten problem, albo:


- Usuwanie danych z tabel zoptymalizowane pamięci potencjalnie Odciążanie danych do tabel tradycyjny, oparte na dysku. lub,
- Uaktualnij warstwa usług do jednego z wystarczającą ilość miejsca do magazynowania w pamięci dla danych, które należy zachować w tabelach zoptymalizowane pamięci.

## <a name="next-steps"></a>Następne kroki
Dodatkowe zasoby dotyczące o [monitorowania bazy danych SQL Azure za pomocą dynamiczne zarządzanie widokami](sql-database-monitoring-with-dmvs.md)
