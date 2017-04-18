<properties 
   pageTitle="Cennik warstwa zalecenia dotyczące bazy danych SQL Azure" 
   description="Podczas zmieniania ceny poziomów w portalu Azure ceny zalecenia warstwa są pod warunkiem, że zalecane warstwie, która najlepiej nadaje się do uruchamiania obciążenie pracą istniejącej bazy danych SQL Azure w. Cennik poziomów opisuje poziom warstwa i wydajności usługi z bazą danych SQL." 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="08/08/2016"
   ms.author="sstein"/>

# <a name="sql-database-pricing-tier-recommendations"></a>Zalecenia dotyczące warstwa cennik bazy danych SQL

 Cennik zalecenia warstwa Zaproponuj warstwa i wydajności poziomu usług najlepiej nadaje się do uruchamiania obciążenie pracą baza istniejących danych Azure SQL.

> [AZURE.NOTE] Cennik zalecenia warstwa są tylko dostępne w bazach danych sieci Web i małych firm i pule elastyczne bazy danych — i dostępne tylko w [Azure portal](https://portal.azure.com/).


Pobieranie ceny zalecenia warstwa podczas następujących zadań:

- [Zmienianie usługi warstwa i wydajności poziomu (poziomu cen) z bazą danych SQL](sql-database-scale-up.md)
- [Uaktualnianie Azure SQL server do wersji 12](sql-database-upgrade-server-portal.md)
- Przejdź do swojego serwera w wersji 12. Zobacz [bazy danych SQL ceny zalecenia warstwy](sql-database-service-tier-advisor.md).
- [Tworzenie puli elastyczne bazy danych](sql-database-elastic-pool.md#elastic-database-pool-pricing-tier-recommendations)





## <a name="overview"></a>Omówienie

Usługa bazy danych SQL analizuje Bieżąca wydajność i wymagań funkcji według oceny użycie zasobu historycznych bazy danych SQL. Ponadto warstwa minimalne usług dopuszczalne jest określana na podstawie rozmiaru bazy danych i funkcji włączonych [ciągłości](sql-database-business-continuity.md) . 

Analizy te informacje i usługi warstwy i wydajności poziom, który najlepiej nadaje się do uruchamiania typowych zadań bazy danych i zaleca się z zachowaniem jest bieżący zestaw funkcji.

- Usługa sprawdza poprzedniego 15-30 dni danych historycznych (użycie zasobu, rozmiar bazy danych i działanie bazy danych) i wykonuje porównanie rzeczywisty ograniczenia warstwy jest obecnie dostępna usługi i poziomy wydajności i ilości zasobów używanych.
- Analizy danych w 15 sekund i każdego interwału zestaw wyników jest przydzielone do istniejącego poziomu usług i poziom wydajności, który najlepiej nadaje się do obsługi obciążenia tego zestawu wyników.
- Te 15 drugiego próbki następnie są agregowane do większych analizy 15-30 dni i wydajność i warstwa poziomu usług optymalnie mogą obsługiwać 95% historycznych obciążenie pracą jest zalecane.

### <a name="recommendations"></a>Zalecenia

W zależności od zastosowania bazy danych, są obecnie 2 kategorie zaleceń, które mogą wystąpić:


| Zalecenia | Opis |
| :--- | :--- |
| Uaktualnianie | Uaktualnianie do nowego poziomu. |
| Niedostępne | Bazy danych wymaga minimalne obciążenie pracą lub 35 dni działalności. Jest za mało danych, aby zapewnić prawidłowe rekomendacji. |

## <a name="getting-pricing-tier-recommendations"></a>Wprowadzenie ceny zalecenia warstwy

Pobierz ceny zalecenia Warstwa przez wybranie istniejącej bazy danych sieci Web i firm, kliknij polecenie **wszystkie ustawienia**, a następnie **Warstwa ceny (skala DTUs)**. (Cennik warstwa są również dostępne podczas możesz [uaktualnić Azure SQL server do wersji 12](sql-database-upgrade-server-portal.md).)

1. Zaloguj się do [portalu Azure](https://portal.azure.com/).
2. Kliknij przycisk **PRZEGLĄDAJ,** > **bazy danych programu SQL**.
4. W karta **baz danych programu SQL** kliknij bazę danych, którą chcesz wyświetlić zalecenia dotyczące:

    ![Wybierz bazę danych][1]

5. Na karta bazy danych wybierz pozycję **wszystkie ustawienia** , a następnie wybierz **poziom ceny (skala DTUs)**.


7. **Zalecane ceny poziomów** Otwórz miejsce, w którym można kliknij warstwie sugerowane a następnie kliknij przycisk **Wybierz** , aby zmienić tego poziomu.

    ![Utwórz konto w usłudze podglądu][4]

8. Opcjonalnie kliknij pozycję **informacje dotyczące sposobu użycia widoku** , aby otworzyć karta **Ceny Szczegóły rekomendacji warstwy** , gdzie można przeglądać warstwie zalecane dla bazy danych, funkcja porównanie bieżącej i zalecane poziomy i wykres analizy użycia zasobów historycznych.

    ![Utwórz konto w usłudze podglądu][5]



## <a name="summary"></a>Podsumowanie

Cennik zalecenia warstwa zapewniają automatyczną obsługę zbieranie danych telemetrycznych dla każdej bazy danych SQL i zalecenie najlepsze usługi warstwy i wydajności poziomu połączenie na podstawie potrzeb rzeczywistej wydajności i wymagań funkcji bazy danych. Aby sprawdzić ceny warstwa zalecenia dotyczące żadnej bazy danych sieci Web i małych firm karta Ustawienia kliknąć polecenie **Warstwa ceny (skala DTUs)** .



## <a name="next-steps"></a>Następne kroki

W zależności od szczegóły konkretnej bazy danych zazwyczaj przeprowadzania uaktualnienia i zmiany nie wystąpić natychmiast. Portalu zapewni powiadomienia jako przejścia do bazy danych do jego nowego poziomu lub stanu uaktualniania można monitorować za pomocą kwerend wysyłanych [sys.dm_operation_status (bazy danych SQL Azure)](https://msdn.microsoft.com/library/dn270022.aspx) widok wzorca bazy danych programu SQL Server bazy danych.


<!--Image references-->
[1]: ./media/sql-database-service-tier-advisor/select-database.png
[4]: ./media/sql-database-service-tier-advisor/choose-pricing-tier.png
[5]: ./media/sql-database-service-tier-advisor/usage-details.png


 
