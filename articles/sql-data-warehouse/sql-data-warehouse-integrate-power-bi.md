<properties
   pageTitle="Używanie usługi Power BI z magazynu danych SQL | Microsoft Azure"
   description="Porady dotyczące korzystania z usługi Power BI z magazynu danych SQL Azure dla opracowania rozwiązań."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/31/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="use-power-bi-with-sql-data-warehouse"></a>Używanie usługi Power BI z magazynu danych SQL
Jako z bazy danych SQL Azure SQL danych magazynu Direct Connect umożliwia użytkownikowi wykorzystać możliwości logiczne przekazywanie razem z pakietem możliwości analityczne programu Power BI.  Z nawiązywanie bezpośredniego połączenia kwerendy są wysyłane do magazynu danych SQL Azure w czasie rzeczywistym, jak Eksplorowanie danych.  Połączone, skali magazynu danych SQL umożliwia użytkownikom tworzyć dynamiczne raporty w minut przed terabajtów danych.  Ponadto wprowadzenie przycisku Otwórz w usłudze Power BI umożliwia użytkownikom bezpośrednio z usługi Power BI bez zbieranie informacji z innych części Azure ich magazynu danych SQL.

Podczas używania nawiązywanie bezpośredniego połączenia należy notatki:

+ Określ nazwę serwera w pełni kwalifikowana podczas nawiązywania połączenia (poniżej podano więcej szczegółów)
+ Upewnij się, że reguły zapory bazy danych są skonfigurowane do "Zezwalaj na dostęp do usług Azure".
+ Każda akcja, takich jak zaznaczenie kolumny lub Dodawanie filtru bezpośrednio zwrócą magazynu danych
+ Kafelki są odświeżane około na 15 minut (odświeżania nie musi być planowane)
+ Pytania i odpowiedzi nie jest dostępna dla nawiązywanie bezpośredniego połączenia zestawy danych
+ Zmiany schematu nie są odczytywane automatycznie
+ Wszystkie kwerendy nawiązywanie bezpośredniego połączenia limit czasu będzie 2 minuty

Te ograniczenia i notatek może zmienić w dalszym ciągu do poprawy środowiska. Procedura łączenia wyszczególniono poniżej.  

## <a name="using-the-open-in-power-bi-button"></a>Za pomocą przycisku "Otwórz w usłudze Power BI"
Najłatwiejszym sposobem przechodzenie między magazynu danych SQL i usługi Power BI jest przy użyciu przycisku Otwórz w usłudze Power BI. Kliknięcie tego przycisku pozwala na bezproblemowe rozpocząć tworzenie nowych pulpitów nawigacyjnych w usłudze Power BI.  

1.  Aby rozpocząć przejdź do wystąpienia magazynu danych SQL w portalu klasyczny Azure.
2.  Kliknij przycisk "Otwórz w usłudze Power BI".
3.  Jeśli nie możemy możesz logować się bezpośrednio lub jeśli nie masz konta usługi Power BI, będzie konieczne logowania.  
4.  Nastąpi przekierowanie do strony połączenia magazynu danych SQL, na podstawie informacji z magazynu danych SQL wstępnie wypełnione.
5.  Po wprowadzeniu poświadczeń można będzie w pełni połączony z magazynu danych SQL.

## <a name="connecting-through-the-power-bi-portal"></a>Nawiązywanie połączenia za pośrednictwem portalu usługi Power BI
Oprócz przy użyciu przycisku Otwórz w usłudze Power BI, użytkownicy mogą również łączyć do ich magazynu danych SQL za pośrednictwem portalu Power BI.

1.  Kliknij przycisk "Pobierz dane" u dołu okienka nawigacji.
2.  Zaznacz "Bazy danych".
3.  Raz na stronie bazy danych wybierz "Skład danych SQL Azure", a następnie kliknij przycisk "Połącz".
4.  Wprowadź niezbędnych informacji o połączeniu.  Nazwa serwera, a nazwa bazy danych można znaleźć w Azure Portal.
5.  Będzie kierowany do strony głównej usługi Power Bi i po utworzeniu nowego wpisu w obszarze "Zestawy danych" z połączenia pojawi się z nazwą wystąpienia.  
6.   Możesz kliknąć nowy zestaw danych Eksplorowanie wszystkie tabele i widoki w bazie danych. Zaznaczenie kolumny będzie wysyłać kwerendy do źródła dynamicznego tworzenia wizualizację. Te elementy wizualne można zapisać w nowym raporcie i ponownie przypięta do pulpitu nawigacyjnego.

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse development overview]:  ./sql-data-warehouse-overview-develop/
[SQL Data Warehouse integration overview]:  ./sql-data-warehouse-overview-integration/

<!--MSDN references-->

<!--Other Web references-->
