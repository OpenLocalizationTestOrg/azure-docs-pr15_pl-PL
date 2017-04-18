<properties
    pageTitle="Uaktualnianie do wersji 12 bazy danych SQL Azure za pomocą portalu Azure | Microsoft Azure"
    description="Wyjaśniono, jak przeprowadzić uaktualnienie do wersji 12 bazy danych SQL Azure wraz ze uaktualnianie baz danych sieci Web i małych firm oraz serwer V11 Migrowanie bazy danych bezpośrednio w puli elastyczne bazy danych przy użyciu Azure portal uaktualnić."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="08/08/2016"
    ms.author="sstein"/>


# <a name="upgrade-to-azure-sql-database-v12-using-the-azure-portal"></a>Uaktualnianie do wersji 12 bazy danych SQL Azure za pomocą portalu Azure


> [AZURE.SELECTOR]
- [Azure portal](sql-database-upgrade-server-portal.md)
- [Programu PowerShell](sql-database-upgrade-server-powershell.md)


Baza danych SQL w wersji 12 jest najnowsza wersja, zaleca się uaktualnienie istniejące serwery do wersji 12 bazy danych SQL.
Bazy danych SQL w wersji 12 ma wiele [zalet w porównaniu z poprzednią wersję](sql-database-v12-whats-new.md) w tym:

- Bardziej zgodny z programem SQL Server.
- Ulepszone premium wydajność i nowe poziomy wydajności.
- [Pule elastyczne bazy danych](sql-database-elastic-pool.md).

Ten artykuł zawiera wskazówki dotyczące uaktualniania baz danych i istniejące serwery V11 bazy danych SQL do wersji 12 bazy danych SQL.

Podczas uaktualniania do wersji 12 będzie Uaktualnij żadnej bazy danych sieci Web i małych firm do nowego poziomu usług, więc ze wskazówkami dotyczącymi uaktualniania baz danych sieci Web i małych firm są zawarte.

Ponadto Migrowanie do [puli elastyczne bazy danych](sql-database-elastic-pool.md) może być kosztów wydajniejsza niż uaktualnienie do wydajności poszczególnych poziomów (ceny poziomów) dla pojedynczej baz danych. Pule również uprościć zarządzanie bazą danych, ponieważ potrzebne do zarządzania ustawieniami wydajności puli, a nie oddzielnie Zarządzanie poziomami wydajności pojedyncze bazy danych. Jeśli masz baz danych na wielu serwerach warto rozważyć przeniesienie ich do tego samego serwera i skorzystać z wprowadzaniem ich w puli. Możesz łatwo [Automatyczne Migrowanie bazy danych z serwerami V11 bezpośrednio do pul elastyczne bazy danych przy użyciu programu PowerShell](sql-database-upgrade-server-powershell.md). Umożliwia także portalu przeprowadzić migrację V11 baz danych do puli, ale w portalu wcześniej trzeba serwera w wersji 12, aby utworzyć pulę. Wskazówki znajdują się w dalszej części tego artykułu, aby utworzyć puli po uaktualnieniu serwera, jeśli masz [baz danych, które mogą korzystać z puli](sql-database-elastic-pool-guidance.md).

Należy zauważyć, że baz danych będzie działać w trybie online i działają podczas całego procesu uaktualniania. W tym czasie rzeczywisty przejścia na nowy poziom wydajności, tymczasowe usuwanie połączenia z bazą danych może się zdarzyć, bardzo mały czasu trwania, które jest zwykle około 90 sekund, ale może być możliwie 5 minut. Jeśli aplikacja ma [przejściowych błędów obsługi zakończeń połączenia](sql-database-connectivity-issues.md) następnie wystarczy chronić się przed przerwane połączenia na końcu uaktualniania.

Uaktualnianie do wersji 12 bazy danych SQL nie można cofnąć. Po uaktualnieniu serwera nie można przywrócić do V11.

Po uaktualnieniu do wersji 12, [zalecenia dotyczące poziomu usług](sql-database-service-tier-advisor.md) i [Zagadnienia dotyczące wydajności pula elastyczne](sql-database-elastic-pool-guidance.md) nie natychmiast będą dostępne do czasu usługa ma czasu na oszacowanie z obciążeń pracą na nowym serwerze. Historia rekomendacji serwera V11 nie dotyczy serwerów wersji 12, nie jest zachowywana.

## <a name="prepare-to-upgrade"></a>Przygotowywanie do uaktualnienia

- **Uaktualnianie wszystkie bazy danych sieci Web i małych firm**: zobacz [Uaktualnianie wszystkie bazy danych sieci Web i małych firm](sql-database-upgrade-server-portal.md#upgrade-all-web-and-business-databases) sekcji poniżej lub zobacz [Monitor i zarządzanie nimi puli elastyczne bazy danych (programu PowerShell)](sql-database-elastic-pool-manage-powershell.md).
- **Recenzja i zawiesić Geo replikacji**: Jeśli bazy danych Azure SQL jest skonfigurowany do replikacji Geo jego bieżącej konfiguracji i [zatrzymywanie replikacji Geo](sql-database-geo-replication-portal.md#remove-secondary-database)należy dokumentu. Po zakończeniu uaktualniania skonfigurowanie bazy danych dla replikacji Geo.
- **Otwórz następujące porty, jeśli masz klientów na maszyn wirtualnych Azure**: Jeśli program klient nawiązuje połączenie bazy danych SQL w wersji 12 Klient uruchomiony na Azure maszyn wirtualnych (maszyn wirtualnych), należy otworzyć zakresy portów 11000 11999 i 14000-14999 na maszyn wirtualnych. Aby uzyskać szczegółowe informacje zobacz [porty dla wersji 12 bazy danych SQL](sql-database-develop-direct-route-ports-adonet-v12.md).



## <a name="start-the-upgrade"></a>Rozpoczynanie uaktualnienia

1. Podczas przeglądania [Azure portal](https://portal.azure.com/) na serwerze chcesz uaktualnić, wybierając pozycję **PRZEGLĄDAJ** > **SQL Server**i Wybieranie serwera 2.0 chcesz uaktualnić.
2. Wybierz **Najnowszą aktualizację bazy danych SQL**, a następnie wybierz pozycję **uaktualnienie tego serwera**.

      ![Uaktualnianie serwera][1]

3. Uaktualnienie serwera do najnowszej aktualizacji bazy danych SQL jest stałych i bez. Aby potwierdzić uaktualnienie, wpisz nazwę serwera, a następnie kliknij **przycisk OK**.

## <a name="upgrade-all-web-and-business-databases"></a>Uaktualnianie wszystkie bazy danych sieci Web i małych firm

Jeśli serwer ma żadnej bazy danych sieci Web lub firm można je uaktualnić. Podczas uaktualniania do wersji 12 bazy danych SQL zostanie zaktualizowany wszystkie bazy danych sieci Web i małych firm do nowego poziomu usług.    

Aby pomocne w przypadku uaktualniania, usługa bazy danych SQL zaleca odpowiednią usługę warstwy i wydajności poziomu (poziomu cen) dla każdej bazy danych. Usługa zaleca warstwie najważniejsze uruchamiania obciążenie pracą z istniejącą bazą danych analizując historycznych zastosowania dla bazy danych.

3. W karta **uaktualnić ten serwer** zaznacz każdej bazy danych, aby zapoznać się z i wybierz pozycję polecane ceny poziomu przeprowadzić uaktualnienie do. Zawsze można wyszukiwać dostępny cennik warstwy i wybierz ten, który najlepiej pasuje środowiska.


     ![bazy danych][2]


7. Po kliknięciu pozycji sugerowane Warstwa możesz zostanie wyświetlona karta **Wybierz z poziomu cen** miejsce, w którym można wybierz poziom, a następnie kliknij przycisk **Wybierz** , aby zmienić tego poziomu. Wybierz nowy poziom dla każdej bazy danych sieci Web lub firm.

    Należy zauważyć, jest to, że bazy danych SQL nie jest zablokowany do dowolnego określonego warstwa lub wydajności poziomie usługi, tak jak wymagania zmiany bazy danych można łatwo zmienić między warstwy dostępne usługi i poziomy wydajności. W rzeczywistości podstawowej, standardowe i baz danych programu SQL Premium są wystawiona przez godzinę, a użytkownik ma możliwość skalowania każdej bazy danych w górę lub w dół 4 razy w ciągu 24 godzin.

    ![zalecenia][6]


Gdy kwalifikujesz się wszystkie bazy danych na serwerze można przystąpić do Rozpoczynanie uaktualnienia

## <a name="confirm-the-upgrade"></a>Potwierdzenie aktualizacji

3. W przypadku wszystkich baz danych na serwerze uaktualnić należy **Typ Nazwa serwera** do weryfikacji, że chcesz przeprowadzić uaktualnienie, a następnie kliknij **przycisk OK**.

    ![Sprawdź uaktualnienia][3]


4. Uaktualnianie i wyświetlenie w toku powiadomienie. Proces uaktualniania została zainicjowana. W zależności od danych określonych baz danych uaktualniania do wersji 12 może zająć trochę czasu. W tym czasie wszystkich baz danych na serwerze pozostanie w trybie online, ale serwera i akcje zarządzania bazy danych zostanie ograniczony.

    ![uaktualnienie w toku][4]

    W momencie rzeczywisty przejścia do nowego wydajności poziom tymczasowe usuwanie połączenia z bazą danych może się zdarzyć, bardzo mały czas trwania (zwykle mierzony w sekundach). Jeśli aplikacja ma obsługi błędów przejściowych (ponów próbę logicznych) dla zakończenia połączenia, a następnie wystarczy chronić się przed przerwane połączenia na końcu uaktualniania.

5. Po zakończeniu procesu uaktualniania karta **Najnowszą aktualizację** wyświetli **włączone**.

    ![W wersji 12 jest włączona][5]  

## <a name="move-your-databases-into-an-elastic-database-pool"></a>Przenoszenie baz danych do puli elastyczne bazy danych

W [portalu Azure](https://portal.azure.com/) przejdź do serwera w wersji 12, a następnie kliknij polecenie **Dodaj pulę**.

- lub -

Jeśli zobaczysz komunikat, **kliknij tutaj, aby wyświetlić pul zalecane elastyczne bazy danych dla tego serwera**, kliknij go, aby łatwo tworzyć puli przeznaczonego do swojego serwera bazy danych. Aby uzyskać szczegółowe informacje zobacz [Wydajność i cena zagadnienia związane z puli elastyczne bazy danych](sql-database-elastic-pool-guidance.md).

![Dodawanie puli na serwerze][7]

Postępuj zgodnie z instrukcjami w artykule [Tworzenie puli elastyczne bazy danych](sql-database-elastic-pool.md) , aby zakończyć tworzenie puli użytkownika.

## <a name="monitor-databases-after-upgrading-to-sql-database-v12"></a>Monitorowanie baz danych po uaktualnieniu do wersji 12 bazy danych SQL

>[AZURE.IMPORTANT] Uaktualnienie do najnowszej wersji z SQL Server Management Studio (SSMS) aby można było korzystać z nowych możliwości wersji 12. [Pobierz program SQL Server Management Studio] (https://msdn.microsoft.com/library/mt238290.aspx).

Po uaktualnieniu, aktywne monitorowanie bazy danych, aby upewnić się, że aplikacji działa w odpowiedniej wydajności, a następnie Zoptymalizuj ustawienia zależnie od potrzeb.

Oprócz monitorowania pojedyncze bazy danych, można monitorować pul elastyczne bazy danych [Monitor, zarządzać i rozmiaru puli elastyczne bazy danych za pomocą portalu Azure](sql-database-elastic-pool-manage-portal.md) lub przy użyciu [programu PowerShell](sql-database-elastic-pool-manage-powershell.md).


**Danymi zużycie zasobów:** Podstawowe, Standard i Premium bazach danych dla danych zużycie zasobów jest dostępne za pośrednictwem [sys.dm_ db_ resource_stats](http://msdn.microsoft.com/library/azure/dn800981.aspx) DMV w bazie danych użytkownika. Ten widok DMV znajdują się w czasie rzeczywistym zużycie informacje w 15 szczegółowości drugiego o poprzedniej godziny pracy. DTU przez wartość procentową dla interwału jest obliczana jako zużycie maksymalnym procencie wymiarów Procesora, Jo i dziennika. Oto kwerendy do obliczenia średniej zużycie procent DTU przez godzinę ostatniej:

    SELECT end_time
         , (SELECT Max(v)
             FROM (VALUES (avg_cpu_percent)
                         , (avg_data_io_percent)
                         , (avg_log_write_percent)
           ) AS value(v)) AS [avg_DTU_percent]
    FROM sys.dm_db_resource_stats
    ORDER BY end_time DESC;

Dodatkowe informacje monitorowania:

- [Wskazówki dotyczące wydajności bazy danych SQL azure dla pojedynczego baz danych](http://msdn.microsoft.com/library/azure/dn369873.aspx).
- [Ceny i wydajności zagadnienia związane z puli elastyczne bazy danych](sql-database-elastic-pool-guidance.md).
- [Monitorowanie korzystanie z widoków dynamiczne zarządzanie bazą danych SQL Azure](sql-database-monitoring-with-dmvs.md)




**Alertów:** Konfigurowanie alertów "o" w portalu Azure powiadomienia, gdy zbliża się zużycie DTU bazy danych w uaktualnionym niektórych wysokiego poziomu. Alerty bazy danych można skonfigurować w programie portal Azure w różne wskaźniki, takie jak DTU, Procesora, Jo i dziennika. Przejdź do bazy danych i zaznacz **reguły alertów** w karta **Ustawienia** .

Na przykład możesz skonfigurować alert e-mail na "DTU procent" Jeśli średniej wartości procentowej DTU przekracza 75% w ciągu ostatnich 5 minut. Zapoznaj się z [odbieranie alertów](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) , aby dowiedzieć się więcej na temat sposobu konfigurowania alertów.





## <a name="next-steps"></a>Następne kroki

- [Sprawdź, czy puli zalecenia i tworzenie puli](sql-database-elastic-pool-create-portal.md).
- [Zmienianie poziomu warstwa i wydajności usługi bazy danych](sql-database-scale-up.md).



## <a name="related-links"></a>Łącza pokrewne

- [Co nowego w wersji 12 bazy danych SQL](sql-database-v12-whats-new.md)
- [Planowanie i przygotowywanie przeprowadzić uaktualnienie do wersji 12 bazy danych SQL](sql-database-v12-plan-prepare-upgrade.md)


<!--Image references-->
[1]: ./media/sql-database-upgrade-server-portal/latest-sql-database-update.png
[2]: ./media/sql-database-upgrade-server-portal/upgrade-server2.png
[3]: ./media/sql-database-upgrade-server-portal/upgrade-server3.png
[4]: ./media/sql-database-upgrade-server-portal/online-during-upgrade.png
[5]: ./media/sql-database-upgrade-server-portal/enabled.png
[6]: ./media/sql-database-upgrade-server-portal/recommendations.png
[7]: ./media/sql-database-upgrade-server-portal/new-elastic-pool.png
