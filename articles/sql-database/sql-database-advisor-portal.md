<properties 
   pageTitle="Azure Advisor bazy danych SQL za pomocą portalu Azure | Microsoft Azure" 
   description="Advisor bazy danych SQL Azure Azure portal umożliwia przeglądanie i wdrażanie zaleceń istniejące bazy danych SQL, który można zwiększyć wydajność bieżącej kwerendy." 
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
   ms.date="09/30/2016"
   ms.author="sstein"/>

# <a name="sql-database-advisor-using-the-azure-portal"></a>Za pomocą portalu Azure Advisor bazy danych SQL

> [AZURE.SELECTOR]
- [Omówienie Advisor bazy danych SQL](sql-database-advisor.md)
- [Portal](sql-database-advisor-portal.md)

Advisor bazy danych SQL Azure Azure portal umożliwia przeglądanie i wdrażanie zaleceń istniejące bazy danych SQL, który można zwiększyć wydajność bieżącej kwerendy.

## <a name="viewing-recommendations"></a>Wyświetlanie zalecenia

Strona zalecenia to, gdzie można przeglądać najlepsze rekomendacje na ich potencjalny wpływ na podstawie w celu zwiększenia wydajności. Możesz również wyświetlić stan operacji historycznych. Zaznacz zalecenia lub stanu, aby wyświetlić więcej szczegółów.

Aby wyświetlić i stosowanie zalecenia, potrzebne są uprawnienia poprawne [Kontrola dostępu oparta na rolach](../active-directory/role-based-access-control-configure.md) w Azure. **Czytnik**uprawnienia **Współautora bazy danych SQL** są wymagane do widoku zalecenia i **właściciela**, wymagane są uprawnienia **Współautora bazy danych SQL** do wykonania akcji; Tworzenie lub usuwanie indeksy i anulowanie Tworzenie indeksu.

1. Zaloguj się do [portalu Azure](https://portal.azure.com/).
2. Kliknij pozycję **więcej usług** > **bazy danych programu SQL**i wybierz bazę danych.
5. Kliknij pozycję **rekomendacji wydajności** , aby wyświetlić dostępne zalecenia dotyczące wybranej bazy danych.

> [AZURE.NOTE] Aby uzyskać zalecenia dotyczące bazy danych musi mieć o dnia zastosowania, a musi istnieć kilka działań. Należy również niektóre spójne aktywności. Advisor bazy danych SQL łatwiej może optymalizować wzorców spójne kwerendy, nie może dla losowe seria spotty działalności. Jeśli zalecenia nie są dostępne, na stronie **rekomendacji wydajności** powinien zawierać komunikat wyjaśniający, dlaczego.

![Zalecenia](./media/sql-database-advisor-portal/recommendations.png)

Oto przykład "Rozwiązać problem schematu" zalecenia w portalu Azure.

![Rozwiązywanie problemu schematu](./media/sql-database-advisor-portal/sql-database-advisor-schema-issue.png)

Zalecenia są sortowane według ich potencjalny wpływ na wydajność na następujące cztery kategorie:

| Wpływ | Opis |
| :--- | :--- |
| Duży | Zalecenia duży wpływ powinien zawierać największy wpływ na wydajność. |
| Średnia | Średnie wpływ zalecenia należy poprawić wydajność, ale nie znacznie. |
| Niska | Zalecenia wpływ niskiej powinien zawierać lepszą wydajność niż bez, ale ulepszenia może nie być istotne. 


### <a name="removing-recommendations-from-the-list"></a>Usunięcie zalecenia z listy

Jeśli lista zaleceń zawiera elementy, które chcesz usunąć z listy, można odrzucić zalecenie:

1. Wybierz z listy **zaleceń**zalecenia.
2. Kliknij przycisk **Odrzuć** na karta **Szczegóły** .


W razie potrzeby można dodać elementy usuwane powrót do listy **zalecenia** :

1. Karta **zalecenia** polecenie **odrzucane widoku**.
1. Zaznacz element usuwane z listy, aby wyświetlić jego szczegóły.
1. Opcjonalnie kliknij pozycję **Cofnij Odrzuć** dodać indeks do głównej listy **zalecenia**.



## <a name="applying-recommendations"></a>Stosowanie zalecenia

Advisor bazy danych SQL zapewnia pełną kontrolę nad jak zalecenia są włączone za pomocą dowolnej z trzech poniższych opcji: 

- Stosowanie poszczególnych zalecenia jedną naraz.
- Włączanie advisor automatycznie zastosowane zalecenia (obecnie dotyczy tylko zalecenia indeks).
- Aby ręcznie zaimplementować zalecenie, uruchom zalecane skrypt T-SQL bazy danych.

Wybierz pozycję rekomendacji, aby wyświetlić jego szczegóły, a następnie kliknij przycisk **Przeglądaj skrypt** umożliwiający Przejrzyj szczegóły dokładnie tworzenia zalecenie.

Baza danych pozostaje w trybie online podczas advisor dotyczy rekomendacji — przy użyciu Advisor bazy danych SQL nigdy nie ma bazy danych w trybie offline.

### <a name="apply-an-individual-recommendation"></a>Stosowanie poszczególnych rekomendacji

Można przejrzeć i zaakceptować zalecenia jednym naraz.

1. Wybierz polecenie karta **zalecenia** zalecenia.
2. Na karta **Szczegóły** kliknij przycisk **Zastosuj**.

    ![Stosowanie rekomendacji](./media/sql-database-advisor-portal/apply.png)

### <a name="enable-automatic-index-management"></a>Włącz automatyczne indeks Zarządzanie

Można ustawić Advisor bazy danych SQL automatycznie wdrożyć zalecenia. Zgodnie z zaleceniami staną się dostępne automatycznie zostaną one zastosowane. Co z wszystkie operacje indeksu zarządzane przez usługę, jeśli wpływ na wydajność ma wartość ujemną zalecenie będzie można przywrócić.

1. Wybierz polecenie karta **zalecenia** **Automatyzacja**:

    ![Ustawienia Advisor](./media/sql-database-advisor-portal/settings.png)

2. Ustawianie advisor automatyczne **Tworzenie** lub **Usuwanie** indeksów:

    ![Zalecane indeksów](./media/sql-database-advisor-portal/automation.png)


### <a name="manually-run-the-recommended-t-sql-script"></a>Ręczne uruchamianie zalecane skrypt T-SQL

Wybierz pozycję rekomendacji, a następnie kliknij przycisk **Przeglądaj skrypt**. Uruchom ten skrypt przed bazę danych, aby ręcznie Zastosuj zalecenia.

*Indeksy, które są wykonywane ręcznie nie są monitorowane i zatwierdzony do wpływ na wydajność przez usługę* , zaleca się monitorowanie tych wskaźników po utworzeniu, aby sprawdzić ich zapewniają zwiększenie wydajności i dostosować lub je usunąć, jeśli to konieczne. Aby uzyskać szczegółowe informacje o tworzeniu indeksów zobacz [Tworzenie INDEKSU (Transact-SQL)](https://msdn.microsoft.com/library/ms188783.aspx).


### <a name="canceling-recommendations"></a>Anulowanie zalecenia

Zalecenia dotyczące, które są w stanie **Oczekujące**, **Sprawdzanie**lub **sukcesu** można je anulować. Nie można anulować zalecenia ze stanem **Wykonywanie** .

1. Wybierz pozycję zalecenia w obszarze **Historia dostosowywania** , aby otworzyć karta **Szczegóły zalecenia** .
2. Kliknij przycisk **Anuluj** , aby przerwać proces stosowania zalecenia.



## <a name="monitoring-operations"></a>Monitorowanie operacji

Stosowanie zalecenie może nie wystąpić natychmiast. Portalu zawiera szczegółowe informacje dotyczące stanu operacji rekomendacji. Poniżej przedstawiono możliwe stany, które można indeksu w:

| Stan | Opis |
| :--- | :--- |
| Oczekiwanie na | Stosowanie rekomendacji polecenie została otrzymana i jest zaplanowane do wykonania. |
| Wykonywanie | Zalecenie jest stosowana. |
| Sukcesu | Zalecenie zostały pomyślnie zastosowane. |
| Błąd | Wystąpił błąd podczas stosowania zalecenia. Może to być problem przejściowych lub schemat Zmienianie do tabeli i skrypt nie jest już prawidłowe. |
| Przywracanie | Zalecenie została zastosowana, ale został uznany za nie performant i zostanie automatycznie zamieniona. |
| Wycofane | Zalecenie zostało anulowane. |

Kliknij pozycję rekomendacji w trakcie z listy, aby wyświetlić więcej szczegółów:

![Zalecane indeksów](./media/sql-database-advisor-portal/operations.png)


### <a name="reverting-a-recommendation"></a>Przywracanie zalecenia

Jeśli użyto advisor stosowania zalecenie (co oznacza, że ręczne nie uruchamianie skryptu T-SQL) automatycznie zostanie przywrócona go jeśli znajdzie się ujemny wpływ na wydajność. Dowolnego powodu po prostu chcesz przywrócić zalecenia można wykonaj następujące czynności:


1. Wybierz pozycję rekomendacji pomyślnie zastosowanego w obszarze **Historia dostrajania** .
2. Kliknij przycisk **Przywróć** na karta **Szczegóły rekomendacji** .

![Zalecane indeksów](./media/sql-database-advisor-portal/details.png)


## <a name="monitoring-performance-impact-of-index-recommendations"></a>Monitorowanie wpływ na wydajność zaleceń indeksu

Po pomyślnym zaimplementowanych zalecenia (obecnie operacji indeksowania i definiowanie parametrów kwerend zalecenia tylko) można kliknąć **Wniosków kwerendy** Karta Szczegóły rekomendacji, aby otworzyć [Wniosków wydajności kwerend](sql-database-query-performance.md) i zobaczyć wpływ na wydajność górny kwerend.

![Wpływ na wydajność monitora](./media/sql-database-advisor-portal/query-insights.png)



## <a name="summary"></a>Podsumowanie

Advisor bazy danych SQL zawiera zalecenia dotyczące zwiększania wydajności bazy danych SQL. Udostępniając skryptów T-SQL, oraz osoby i pełni automatyczne (obecnie tylko indeks), advisor zawiera przydatne pomocy w optymalizacji bazy danych i ostatecznie zwiększania wydajności kwerendy.



## <a name="next-steps"></a>Następne kroki

Monitorowanie Twoje rekomendacje i przejdź do ich zastosowania w celu zaktualizowania wydajności. Obciążenia bazy danych są dynamiczne i zmień przez cały czas. Baza danych SQL advisor będzie nadal monitorować i zalecenia, które potencjalnie może zwiększyć wydajność bazy danych. 

 - Aby uzyskać omówienie Advisor bazy danych SQL, zobacz [Advisor bazy danych SQL](sql-database-advisor.md) .
 - Zobacz [Wniosków wydajności kwerendy](sql-database-query-performance.md) do informacje na temat wyświetlania wpływ na wydajność górny kwerend.

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Magazyn kwerendy](https://msdn.microsoft.com/library/dn817826.aspx)
- [TWORZENIE INDEKSU](https://msdn.microsoft.com/library/ms188783.aspx)
- [Kontrola dostępu oparta na rolach](../active-directory/role-based-access-control-configure.md)






