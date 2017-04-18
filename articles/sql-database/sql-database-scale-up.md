<properties
    pageTitle="Zmienianie poziomu warstwa i wydajności usługi bazy danych programu Azure SQL | Microsoft Azure"
    description="Zmiana poziomu usług i poziom wydajności bazy danych programu Azure SQL pokazano, jak skalowanie bazy danych SQL w górę lub w dół. Zmienianie poziomu cen bazy danych programu Azure SQL."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/12/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="change-the-service-tier-and-performance-level-pricing-tier-of-a-sql-database-using-the-azure-portal"></a>Zmienianie usługi warstwa i wydajności poziomu (poziomu ceny) bazy danych SQL za pomocą portalu Azure


> [AZURE.SELECTOR]
- [**Azure portal**](sql-database-scale-up.md)
- [Programu PowerShell](sql-database-scale-up-powershell.md)


Warstwy usługi i poziomy wydajności opis funkcji i zasobów dostępnych dla bazy danych SQL i może być aktualizowane jako potrzeb zmiany aplikacji. Aby uzyskać szczegółowe informacje zobacz [Warstwy usługi](sql-database-service-tiers.md).

Należy zauważyć, że zmiana poziomu usług i/lub poziom wydajności bazy danych tworzy replice oryginalnej bazy danych na nowy poziom wydajności, a następnie przełącza połączenia replice. W trakcie tego procesu są tracone żadne dane, ale podczas krótki moment, kiedy możemy przełączyć się do replice, połączenia z bazą danych są wyłączone, aby niektóre transakcje w lotów może zostać przywrócona. Tego okna zmienia się, ale trwa średnio poniżej 4 sekund, a w ponad 99% przypadków jest mniejsza niż 30 sekund. Bardzo rzadko zwłaszcza w przypadku dużej liczby transakcji w lotów w momencie połączenia są wyłączone, to okno może być dłuższy.  

Czas trwania całego procesu Skala up zależy od rozmiar i usług warstwie bazę danych przed i po zmianie. Na przykład 250 GB bazy danych, która jest zmiana do z lub w jednej warstwie standardowa usługa należy wykonać w ciągu 6 godzin. W bazie danych o tym samym rozmiarze, który jest zmieniając poziomy wydajności w warstwie usługi Premium należy wykonać w ciągu 3 godzin.


Skorzystaj z informacji w [warstwy usługi bazy danych SQL Azure i poziomy wydajności](sql-database-service-tiers.md) do określenia stopnia warstwa i wydajności usługi odpowiednie dla bazy danych SQL Azure.

- Na starszą wersję bazy danych, bazy danych powinien być mniejszy niż maksymalny rozmiar dozwolonych poziomu usługi docelowej. 
- Podczas uaktualniania bazy danych z [Replikacji Geo](sql-database-geo-replication-overview.md) włączone, należy najpierw uaktualnić pomocniczej bazy danych w warstwie odpowiedniej wydajności przed uaktualnieniem podstawowej bazy danych.
- Gdy obniżenie poziomu usług, należy najpierw zakończyć wszystkie relacje Geo replikacji. 
- Oferty usług przywracania są różne dla różnych poziomów usług. Jeśli możesz kontaktować mogą utracić możliwość Przywracanie do punktu w czasie lub dolnym okres przechowywania kopii zapasowej. Aby uzyskać więcej informacji zobacz [i przywracania kopii zapasowej bazy danych SQL Azure](sql-database-business-continuity.md).
- Zmienianie ceny Warstwa bazy danych nie zmienia rozmiar maksymalny bazy danych. Aby zmienić bazy danych maksymalny rozmiar użyj [Języku Transact-SQL (T-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) lub [programu PowerShell](https://msdn.microsoft.com/library/mt619433.aspx).
- Nowe właściwości bazy danych obowiązują dopiero po zakończeniu wprowadzania zmian.



**Aby zakończyć w tym artykule są potrzebne następujące elementy:**

- Baza danych Azure SQL. Jeśli nie masz bazy danych SQL, należy utworzyć jeden wykonując czynności opisane w tym artykule: [Tworzenie pierwszej bazy danych SQL Azure](sql-database-get-started.md).


## <a name="change-the-service-tier-and-performance-level-of-your-database"></a>Zmiana poziomu usług warstwowych i wydajności bazy danych


Otwórz karta Baza danych SQL dla bazy danych, który chcesz skalować w górę lub dół:

1.  W [portalu Azure](https://portal.azure.com), kliknij pozycję **więcej usług** > **bazy danych programu SQL**.
2.  Kliknij bazę danych, którą chcesz zmienić.
3.  Na karta **Baza danych SQL** kliknij **poziom ceny (skala DTUs)**:

    ![Cennik warstwy][1]

1.  Wybierz nowy poziom, a następnie kliknij przycisk **Wybierz**:

    Klikając przycisk **Wybierz** przesyła żądanie skali zmienić warstwie cennik. W zależności od rozmiaru bazy danych operacja skali może zająć trochę czasu, do wykonania (zobacz informacje w górnej części tego artykułu).

    > [AZURE.NOTE] Zmienianie ceny Warstwa bazy danych nie zmienia rozmiar maksymalny bazy danych. Aby zmienić bazy danych maksymalny rozmiar użyj [Języku Transact-SQL (T-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) lub [programu PowerShell](https://msdn.microsoft.com/library/mt619433.aspx).

    ![Wybieranie poziomu cen][2]

3.  Kliknij ikonę powiadomienia (mila) w prawym górnym rogu:

    ![powiadomienia][3]

    Kliknij tekst powiadomień, aby otworzyć okienko szczegółów, możesz wyświetlać stanu żądania.




## <a name="next-steps"></a>Następne kroki

- Zmienianie maksymalnego rozmiaru bazy danych przy użyciu [programu PowerShell](https://msdn.microsoft.com/library/mt619433.aspx)lub [(T-SQL) w języku Transact-SQL](https://msdn.microsoft.com/library/mt574871.aspx) .
- [Skalowanie i](sql-database-elastic-scale-get-started.md)
- [Nawiązywanie połączenia i kwerendy bazy danych SQL za pomocą SSMS](sql-database-connect-query-ssms.md)
- [Eksportowanie bazy danych programu Azure SQL](sql-database-export.md)

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Przegląd ciągłości działalności](sql-database-business-continuity.md)
- [Dokumentacja bazy danych SQL](https://azure.microsoft.com/documentation/services/sql-database/)


<!--Image references-->
[1]: ./media/sql-database-scale-up/new-tier.png
[2]: ./media/sql-database-scale-up/choose-tier.png
[3]: ./media/sql-database-scale-up/scale-notification.png
[4]: ./media/sql-database-scale-up/new-tier.png
