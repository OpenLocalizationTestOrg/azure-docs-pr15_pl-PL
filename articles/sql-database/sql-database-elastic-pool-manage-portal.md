<properties
    pageTitle="Monitorowanie i zarządzanie nimi puli elastyczne bazy danych za pomocą portalu Azure | Microsoft Azure"
    description="Dowiedz się, jak za pomocą Azure portal i analizy wbudowanych SQL Database zarządzanie, monitorowanie i prawo rozmiar puli skalowalna elastyczne bazy danych optymalizacji wydajności baz danych i zarządzać nimi, koszt."
    keywords=""
    services="sql-database"
    documentationCenter=""
    authors="ninarn"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/22/2016"
    ms.author="ninarn"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="monitor-and-manage-an-elastic-database-pool-with-the-azure-portal"></a>Monitorowanie i zarządzanie nimi puli elastyczne bazy danych za pomocą portalu Azure

> [AZURE.SELECTOR]
- [Azure portal](sql-database-elastic-pool-manage-portal.md)
- [Programu PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)


Azure portal służy do monitorowania i zarządzania puli elastyczne bazy danych i baz danych w puli. W portalu można monitorować wykorzystanie puli elastycznych i baz danych w tej puli. Można także wprowadzanie zmian w puli użytkownika elastycznych i przesłać wszystkie zmiany w tym samym czasie. Zmiany te obejmują dodawanie lub usuwanie baz danych, zmiana ustawień puli elastycznych i zmienianie ustawień bazy danych.

Poniższy rysunek przedstawia puli elastyczne przykład. Widok zawiera:

*  Wykresy do monitorowania użycia zasobów puli elastycznych i zawartych w puli baz danych.
*  Przycisk **Konfiguruj** puli, aby wprowadzić zmiany w puli elastyczne.
*  Przycisk **Utwórz bazę danych** , która umożliwia utworzenie nowej bazy danych i dodaje ją do bieżącej puli elastyczne.
*  Elastyczne zadania, które ułatwiają zarządzanie dużą liczbą baz danych przez uruchamianie skryptów Transact SQL względem wszystkich baz danych na liście.

![Widok puli][2]

Aby pracować kroków w tym artykule, musisz program SQL server w Azure z co najmniej jedną bazę danych i elastycznym puli. Jeśli nie masz elastyczne puli, zobacz [Tworzenie puli](sql-database-elastic-pool-create-portal.md); Jeśli nie masz bazy danych, zobacz [Samouczek z bazą danych SQL](sql-database-get-started.md).

## <a name="elastic-pool-monitoring"></a>Monitorowanie elastyczne puli

Możesz przejść do określonego puli, aby wyświetlić jej wykorzystanie zasobów. Domyślnie puli jest skonfigurowany do pokazywać użycie miejsca do magazynowania i eDTU przez godzinę ostatniej. Wykres można skonfigurować pokazanie różnych metryki na różnych okien czasu.

1. Zaznacz pulę do pracy z.
2. W obszarze **Elastyczne monitorowania puli** wykresu jest oznaczony **wykorzystania zasobów**. Kliknij wykres.

    ![Monitorowanie elastyczne puli][3]

    Karta **metryki** zostanie otwarty, przedstawiający szczegółowe informacje dotyczące określonej metryki nad określonym przedziałem czasu.   

    ![Karta metryczne][9]

### <a name="to-customize-the-chart-display"></a>Aby dostosować wykres

Wykres i metryczne karta, aby wyświetlić inne wskaźniki, takie jak wartość procentową, procent Jo danych i procent Jo Dziennik używany Procesora można edytować.

2. Karta metryczne wybierz polecenie **Edytuj**.

    ![Kliknij pozycję Edytuj][6]

- W karta **Edytowanie wykresu** zaznacz nowy zakres czasu (poza godzinę, obecnie lub ostatni tydzień), lub kliknij pozycję **niestandardowe** zaznacz dowolny zakres dat w ciągu ostatnich dwóch tygodni. Wybierz typ wykresu (pasek lub linia), a następnie wybierz pozycję zasoby, aby monitorować.

> Uwaga: Tylko metryki z tym samym jednostki miary mogą być wyświetlane na wykresie w tym samym czasie. Na przykład w przypadku wybrania "eDTU procent" następnie tylko będzie można wybrać inne wskaźniki z wartościami procentowymi jako jednostki miary.

    ![Click edit](./media/sql-database-elastic-pool-manage-portal/edit-chart.png)

- Kliknij przycisk **OK**.


## <a name="elastic-database-monitoring"></a>Monitorowanie elastyczne bazy danych

Pojedyncze bazy danych można monitorować w taki sposób, aby potencjalne problemy.

1. W obszarze **Elastyczne monitorowania bazy danych**istnieje wyświetlającego metryki pięć bazach danych dla wykresu. Domyślnie wykres był wyświetlany górny 5 baz danych w puli użyciem średnia eDTU w ostatniej godziny. Kliknij wykres.

    ![Monitorowanie elastyczne puli][4]

2. Zostanie wyświetlona karta **Użycie zasobu bazy danych** . To zawiera szczegółowe informacje dotyczące użycia bazy danych w puli. Korzystanie z siatki w dolnej części karta, można wybrać dowolny baz danych w puli, aby wyświetlić jego użyciem na wykresie (maksymalnie 5 bazy danych). Można także dostosować okna metryki i godziny wyświetlane na wykresie, klikając pozycję **Edytuj wykres**.

    ![Karta wykorzystania zasobów bazy danych][8]

### <a name="to-customize-the-view"></a>Aby dostosować widok

1. W karta **Użycie zasobu bazy danych** kliknij przycisk **Edytuj wykresu**.

    ![Kliknij pozycję Edytuj wykres](./media/sql-database-elastic-pool-manage-portal/db-utilization-blade.png)

2. W karta wykresu **Edytowanie** zaznacz nowy zakres czasu (poza godzinę lub ostatnie 24 godziny) lub kliknij pozycję **niestandardowe** aby wybrać inny dzień w ciągu ostatnich 2 tygodnie do wyświetlenia.

    ![Kliknij pozycję niestandardowe](./media/sql-database-elastic-pool-manage-portal/editchart-date-time.png)

3. Kliknij listę rozwijaną **Porównywanie baz danych** , aby wybrać inny metryki podczas porównywania baz danych.

    ![Edytowanie wykresu](./media/sql-database-elastic-pool-manage-portal/edit-comparison-metric.png)

### <a name="to-select-databases-to-monitor"></a>Aby zaznaczyć baz danych do monitorowania

Na liście bazy danych w karta **Użycie zasobu bazy danych** możesz znaleźć określonego baz danych, wyświetlając kolejnych stron na liście lub przez wpisanie nazwy bazy danych. Wybierz bazę danych za pomocą pola wyboru.

![Wyszukiwanie baz danych do monitorowania][7]


## <a name="add-an-alert-to-a-pool-resource"></a>Dodawanie alertu do puli zasobów

Reguły można dodać do zasobów, które powodują wysyłanie wiadomości e-mail do osoby lub alert ciągów do punktów końcowych adresu URL podczas zasobu trafienia progu wykorzystania skonfigurowanego.

> [AZURE.IMPORTANT]Monitorowanie puli elastyczne wykorzystanie zasobów ma zwłoki co najmniej 20 minut. Ustawianie alertów mniej niż 30 minut elastyczne puli nie jest obecnie obsługiwane. Wszystkie alerty, ustawianie elastyczne puli się kropką (parametr o nazwie "-Rozmiar_okna" interfejsu API programu PowerShell) mniej niż 30 minut może nie zostać wyzwolone. Upewnij się, że wszystkie alerty, zdefiniowanych elastyczne puli używać kropki (Rozmiar_okna) 30 minut lub więcej.

**Aby dodać alert do dowolnego zasobu:**

1. Kliknij wykres **wykorzystania zasobów** , aby otworzyć karta **metryczne** , kliknij pozycję **Dodaj alert**, a następnie wypełnij informacje w karta **dodać regułę alertu** , (**zasobów** jest automatycznie ustawić puli, w którym obecnie pracujesz z).
2. Wpisz **nazwę** i **Opis** , który identyfikuje w alercie i adresaci.
3. Wybierz pozycję **metryczne** odpowiedni alert z listy.

    Wykres pokazuje dynamicznie wykorzystania zasobów dla tej metryki ułatwiające wybór progu.

4. Wybierz **warunek** (większe, mniejsze niż, itp.) i **progu**.
5. Kliknij **przycisk OK**.



## <a name="move-a-database-into-an-elastic-pool"></a>Przenoszenie bazy danych do puli elastyczne

Można dodać lub usunąć baz danych z istniejącej puli. Bazy danych mogą mieć inne pule. Jednak można dodawać tylko baz danych znajdujących się na tym samym serwerze logiczne.

1. W karta puli w obszarze **elastyczne baz danych** kliknij **puli Konfiguruj**.

    ![Kliknij pozycję Konfiguruj puli][1]

2. W karta **Konfigurowanie puli** kliknij przycisk **Dodaj do puli**.

    ![Kliknij pozycję Dodaj do puli](./media/sql-database-elastic-pool-manage-portal/add-to-pool.png)


3. W karta **Dodaj baz danych** wybierz bazę danych lub baz danych, aby dodać do puli. Następnie kliknij przycisk **Wybierz**.

    ![Zaznacz bazy danych, aby dodać](./media/sql-database-elastic-pool-manage-portal/add-databases-pool.png)

    Karta **puli Konfiguruj** teraz lista wybranej do dodania z jego stan **Oczekiwanie**bazy danych.

    ![Oczekiwanie na puli dodatki](./media/sql-database-elastic-pool-manage-portal/pending-additions.png)

3. W **konfiguracji karta puli**kliknij przycisk **Zapisz**.

    ![Kliknij przycisk Zapisz](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="move-a-database-out-of-an-elastic-pool"></a>Przenoszenie bazy danych z elastycznych puli

1. W karta **Konfigurowanie puli** wybierz bazę danych lub baz danych, aby usunąć.

    ![Wyświetlanie listy baz danych](./media/sql-database-elastic-pool-manage-portal/select-pools-removal.png)

2. Kliknij przycisk **Usuń z puli**.

    ![Wyświetlanie listy baz danych](./media/sql-database-elastic-pool-manage-portal/click-remove.png)

    Karta **puli Konfiguruj** teraz lista wybranej do usunięcia z jego stan **Oczekiwanie**bazy danych.

    ![Dodawanie bazy danych Podgląd i usuwania](./media/sql-database-elastic-pool-manage-portal/pending-removal.png)

3. W **konfiguracji karta puli**kliknij przycisk **Zapisz**.

    ![Kliknij przycisk Zapisz](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="change-performance-settings-of-a-pool"></a>Zmienianie ustawień wydajności pula

Jak można monitorować wykorzystanie zasobów do puli, może się okazać, że niektóre dostosowania są potrzebne. Być może puli musi zmiany w granicach wydajności lub miejsce do magazynowania. Być może chcesz zmienić ustawienia bazy danych w puli. Ustawienia puli można zmienić w dowolnym momencie, aby uzyskać najlepszy stosunek wydajności i kosztów. Zobacz [po puli elastyczne bazy danych należy?](sql-database-elastic-pool-guidance.md) uzyskać więcej informacji.

**Aby zmienić limity eDTUs lub miejsce do magazynowania na pulę i eDTUs na bazę danych:**

1. Otwórz karta **puli Konfiguruj** .

    W obszarze **Ustawienia puli elastyczne bazy danych**za pomocą suwaka albo zmienić ustawienia puli.

    ![Wykorzystanie elastyczne puli zasobów](./media/sql-database-elastic-pool-manage-portal/resize-pool.png)

2. Po zmianie ustawienia pokazuje szacowany koszt miesięczny zmiany.

    ![Aktualizowanie puli i nowy koszt miesięczny](./media/sql-database-elastic-pool-manage-portal/pool-change-edtu.png)


## <a name="create-and-manage-elastic-jobs"></a>Tworzenie i zarządzanie zadaniami elastyczne

Elastyczne zadań pozwalają na uruchamianie skryptów w języku Transact-SQL przed dowolną liczbę baz danych w puli. Tworzenie nowych zadań lub zarządzanie istniejących zadań za pomocą portalu.

![Tworzenie i zarządzanie zadaniami elastyczne][5]


Przed skorzystaniem z zadaniami, zainstaluj składniki elastyczne zadań i podaj poświadczenia. Aby uzyskać więcej informacji zobacz [Omówienie zadań elastyczne bazy danych](sql-database-elastic-jobs-overview.md).

Zobacz [Skalowanie się z bazą danych SQL Azure](sql-database-elastic-scale-introduction.md): za pomocą narzędzia bazy danych elastyczne skala w nowym oknie, przenoszenie danych, kwerendy lub utworzyć transakcji.



## <a name="additional-resources"></a>Dodatkowe zasoby

- [Elastyczne puli bazy danych SQL](sql-database-elastic-pool.md)
- [Tworzenie puli elastyczne bazy danych za pomocą portalu](sql-database-elastic-pool-create-csharp.md)
- [Tworzenie puli elastyczne bazy danych przy użyciu programu PowerShell](sql-database-elastic-pool-create-powershell.md)
- [Tworzenie puli elastyczne bazy danych przy użyciu języka C#](sql-database-elastic-pool-create-csharp.md)
- [Ceny i wydajności zagadnienia związane z pul elastyczne bazy danych](sql-database-elastic-pool-guidance.md)


<!--Image references-->
[1]: ./media/sql-database-elastic-pool-manage-portal/configure-pool.png
[2]: ./media/sql-database-elastic-pool-manage-portal/basic.png
[3]: ./media/sql-database-elastic-pool-manage-portal/basic-2.png
[4]: ./media/sql-database-elastic-pool-manage-portal/basic-3.png
[5]: ./media/sql-database-elastic-pool-manage-portal/elastic-jobs.png
[6]: ./media/sql-database-elastic-pool-manage-portal/edit-metric.png
[7]: ./media/sql-database-elastic-pool-manage-portal/select-dbs.png
[8]: ./media/sql-database-elastic-pool-manage-portal/db-utilization.png
[9]: ./media/sql-database-elastic-pool-manage-portal/metric.png
