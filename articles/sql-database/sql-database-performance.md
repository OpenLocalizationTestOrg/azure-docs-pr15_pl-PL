<properties 
   pageTitle="Wgląd wydajności bazy danych Azure SQL | Microsoft Azure" 
   description="Baza danych SQL Azure udostępnia narzędzia wydajności ułatwiające określanie obszarów, które można zwiększyć wydajność bieżącej kwerendy." 
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
   ms.date="07/19/2016"
   ms.author="sstein"/>

# <a name="sql-database-performance-insight"></a>Wgląd wydajności bazy danych SQL

Baza danych SQL Azure udostępnia narzędzia wydajności ułatwiające identyfikowanie i zwiększyć wydajność programu baz danych, dostarczając inteligentnego akcje dostosowywania i zalecenia. 

1. Przejdź do bazy danych w [Azure Portal](http://portal.azure.com) i kliknij polecenie **wszystkie ustawienia** > **wydajności **  >  **Przegląd** , aby otworzyć stronę **wydajności** . 


2. Kliknij przycisk **zalecenia** , aby otworzyć [Advisor bazy danych SQL](#sql-database-advisor), a następnie kliknij przycisk **kwerendy** otworzyć [Wglądu wyników kwerendy](#query-performance-insight).

    ![Widok wydajności](./media/sql-database-performance/entries.png)



## <a name="performance-overview"></a>Omówienie wydajności

Kliknięcie **Omówienie** lub na kafelku **wydajności** spowoduje przejście do pulpitu nawigacyjnego wydajności bazy danych. Ten widok zawiera podsumowanie wydajność bazy danych i umożliwia dostosowywanie wydajności i rozwiązywania problemów. 

![Wydajność](./media/sql-database-performance/performance.png)

- Kafelków **zalecenia** udostępnia podział Dostosowywanie zalecenia dotyczące bazy danych (3 pierwszych rekomendacje są widoczne jeśli istnieje więcej). Klikając tym kafelku przejście do **Advisor bazy danych SQL**. 
- Kafelków **aktywności dostrajania** zawiera podsumowanie trwających i ukończone, dostosowywanie akcji dla bazy danych, które zapewnia szybki wgląd do historii dostrajania aktywności. Klikając tym kafelku przejście do pełnego widoku historii dostrajania bazy danych.
- Fragment **automatycznego dostrajania** pokazano konfigurację automatycznego dostrajania bazy danych (które dostrajania akcje są skonfigurowane automatycznie przypisywane do bazy danych). Kliknięcie tym kafelku powoduje otwarcie okna dialogowego konfiguracji automatyzacji.
- Kafelków **kwerendy bazy danych** zawiera podsumowanie wydajności kwerend dla bazy danych (ogólna DTU użycia i góry pochłania zasoby kwerend). Klikając tym kafelku przejście do **Wglądu wyników kwerendy**.



## <a name="sql-database-advisor"></a>Advisor bazy danych SQL


[Advisor bazy danych SQL](sql-database-advisor.md) zawiera inteligentnego określania zalecenia, których można usprawnić działanie bazy danych. 

- Zalecenia dotyczące której indeksy do tworzenia i upuść (i możliwość zastosowania zalecenia indeks automatycznie bez jakiejkolwiek interakcji z użytkownikiem i automatycznie wycofywanie zalecenia, które mają negatywny wpływ na wydajność).
- Zalecenia dotyczące problemów schematu są oznaczone w bazie danych.
- Zalecenia dotyczące kwerend mogą korzystać z kwerend parametrycznych.




## <a name="query-performance-insight"></a>Kwerendy wydajności wglądu

[Kwerendy wydajności wglądu](sql-database-query-performance.md) umożliwia poświęcać mniej czasu na rozwiązywanie problemów z wydajnością bazy danych, dostarczając:

- Szczegółowego wgląd zużycie zasobów (DTU) do bazy danych. 
- Górny Procesora przez inne kwerendy, które mogą być potencjalnie dostosowanych do zwiększona wydajność. 
- Możliwość przechodzić do szczegółów kwerendy. 


## <a name="additional-resources"></a>Dodatkowe zasoby

- [Azure SQL Database wydajności wskazówki dotyczące pojedynczego baz danych](sql-database-performance-guidance.md)
- [Kiedy ma być użyty puli elastyczne bazy danych?](sql-database-elastic-pool-guidance.md)