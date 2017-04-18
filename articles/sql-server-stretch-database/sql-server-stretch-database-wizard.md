<properties
    pageTitle="Wprowadzenie, uruchamiając Włączanie bazy danych dla Kreatora rozciąganie | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować bazy danych dla bazy danych rozciąganie, uruchamiając Włączanie bazy danych dla Kreatora rozciąganie."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/05/2016"
    ms.author="douglasl"/>

# <a name="get-started-by-running-the-enable-database-for-stretch-wizard"></a>Wprowadzenie, uruchamiając kreatora rozciąganie Włączanie bazy danych

Aby skonfigurować bazy danych dla rozciąganie bazy danych, uruchom Włączanie bazy danych dla Kreatora rozciąganie.  W tym temacie opisano informacje, które należy wprowadzić i opcji, które należy w kreatorze.

Aby dowiedzieć się więcej na temat rozciąganie bazy danych, zobacz [Rozciąganie bazy danych](sql-server-stretch-database-overview.md).

 >   [AZURE.NOTE] Później Jeśli wyłączysz rozciąganie bazy danych, należy pamiętać, że wyłączenie rozciąganie bazy danych do tabeli lub bazy danych nie powoduje usunięcia obiektu zdalnego. Aby usunąć tabeli zdalnej lub zdalna baza danych, musisz upuść ją za pomocą portalu zarządzania Azure. Zdalny obiektów w dalszym ciągu ponoszenia kosztów Azure, dopóki nie zostaną usunięte je ręcznie. 

## <a name="launch-the-wizard"></a>Uruchom Kreatora

1.  W programie SQL Server Management Studio w Eksploratorze obiektów wybierz bazę danych, na którym chcesz włączyć rozciąganie.

2.  Prawo\-kliknij i wybierz **zadania**, a następnie wybierz pozycję **Rozciągnij**, a następnie zaznacz pole wyboru **Włącz** , aby uruchomić kreatora.

## <a name="Intro"></a>Wprowadzenie
Przejrzyj przeznaczenie Kreator i wymagania wstępne.

Ważne wymagania wstępne są następujące:

-   Musisz być administratorem, aby zmienić ustawienia bazy danych.
-   Musisz mieć subskrypcję Microsoft Azure.
-   Program SQL Server musi mieć możliwość komunikowania się z serwerem zdalnym Azure.

![Strona wprowadzająca kreatora rozciąganie bazy danych][StretchWizardImage1]

## <a name="Tables"></a>Wybierz tabele
Wybierz tabele, które chcesz włączyć dla rozciąganie.

Tabele z dużą liczbą wierszy są wyświetlane u góry listy posortowanych. Zanim kreator wyświetli listę tabel, analizuje ich dla typów danych, które nie są obecnie obsługiwane przez rozciąganie bazy danych.

![Wybierz tabele stronę kreatora rozciąganie bazy danych][StretchWizardImage2]

|Kolumny|Opis|
|----------|---------------|
|(bez tytułu)|Zaznacz pole wyboru w tej kolumnie, aby włączyć zaznaczonej tabeli dla rozciąganie.|
|**Nazwa**|Nazwa kolumny w tabeli.|
|(bez tytułu)|Symbol w tej kolumnie mogą stanowić ostrzeżenie bez\'t uniemożliwić Włączanie rozciąganie zaznaczonej tabeli. Może również reprezentować problemu z blokowaniem, uniemożliwi włączenie zaznaczonej tabeli dla rozciąganie \- na przykład, ponieważ w tabeli są używane nieobsługiwany typ danych. Umieść wskaźnik na symbolu, aby wyświetlić więcej informacji w etykietce narzędzia. Aby uzyskać więcej informacji zobacz [ograniczenia rozciąganie bazy danych](sql-server-stretch-database-limitations.md).|
|**Rozciągnięciu**|Wskazuje, czy tabela jest już włączone dla rozciąganie.|
|**Migrowanie**|Możesz przeprowadzić migrację całej tabeli (**Całą tabelę**) lub można określić filtru na istniejącej kolumny w tabeli. Jeśli chcesz przy użyciu funkcji inny filtr zaznacz wiersze, aby przeprowadzić migrację, uruchom instrukcja ALTER TABLE, aby określić funkcja filter po zakończeniu kreatora. Aby uzyskać więcej informacji na temat funkcji filtrowania zobacz [Wybieranie wierszy, aby przeprowadzić migrację przy użyciu funkcji Filtr](sql-server-stretch-database-predicate-function.md). Aby uzyskać więcej informacji na temat stosowania funkcji zobacz [Włączanie rozciąganie bazy danych dla tabeli](sql-server-stretch-database-enable-table.md) lub [ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx).|
|**Wiersze**|Umożliwia określenie liczby wierszy w tabeli.|
|**Rozmiar (KB)**|Określa rozmiar tabeli w Kilobajtach.|

## <a name="Filter"></a>Opcjonalnie podaj filtr wierszy

Jeśli chcesz podać funkcję Filtr zaznacz wiersze, aby przeprowadzić migrację, należy wykonać następujące czynności na stronie **Wybierz tabele** .

1.  Na liście **Wybierz tabele, z którymi chcesz rozciągnąć** kliknij **Całą tabelę** w wierszu tabeli. Zostanie wyświetlone okno dialogowe **Zaznacz wiersze, aby rozciągnąć** .

    ![Definiowanie funkcji filtru][StretchWizardImage2a]

2.  W oknie dialogowym **Zaznacz wiersze, aby rozciągnąć** zaznacz **Wiersze wybierz**.

3.  W **polu Nazwa**Podaj nazwę dla funkcji filtrowania.

4.  Dla klauzuli **Where** wybierz kolumnę z tabeli, wybierz operator i podać wartość.

5. Kliknij przycisk **Sprawdź** , aby przetestować funkcję. Jeśli funkcja zwraca wyniki z tabeli — oznacza to, że jeśli są wiersze, aby przeprowadzić migrację spełniających warunek - test raportów o **powodzeniu**.

    >   [AZURE.NOTE] Pole tekstowe, który wyświetla kwerendę filtru jest tylko do odczytu. Nie można edytować kwerendy w polu tekstowym.

6.  Kliknij przycisk Gotowe, aby powrócić do strony **Wybierz pozycję tabele** .

Funkcja filter jest tworzony w programie SQL Server, tylko po zakończeniu pracy z kreatorem. Do tego czasu można zwracać na stronie **Wybierz tabele** , aby zmiana zadania lub nazwy funkcji filtrowania.

![Wybierz stronę tabele po zdefiniowaniu funkcji filtru][StretchWizardImage2b]

Jeśli chcesz użyć innego typu funkcja filter wybierz wiersze do migracji, wykonaj jedną z następujących czynności:  

-   Zamknij kreatora i uruchom instrukcja ALTER TABLE, aby włączyć rozciąganie dla tabeli i określanie funkcji filtru. Aby uzyskać więcej informacji zobacz [Włączanie rozciąganie bazy danych dla tabeli](sql-server-stretch-database-enable-table.md).  

-   Uruchom instrukcja ALTER TABLE, aby określić funkcję filtru po zakończeniu kreatora. Kroki wymagane zobacz [Dodawanie funkcji filtrowania po uruchomieniu kreatora](sql-server-stretch-database-predicate-function.md#addafterwiz).

## <a name="Configure"></a>Konfigurowanie Azure wdrażania

1.  Zaloguj się do programu Microsoft Azure za pomocą konta Microsoft.

    ![Zaloguj się do Azure - Kreator rozciąganie bazy danych][StretchWizardImage3]

2.  Wybierz pozycję istniejącej subskrypcji Azure służących do rozciąganie bazy danych.

3.  Wybierz Azure region.
    -   Jeśli tworzysz nowy serwer, serwer zostanie utworzona w tym regionie.  
    -   Jeśli masz istniejące serwery w wybranego regionu Kreator wyświetla je po wybraniu **istniejącego serwera**.

    Aby zminimalizować opóźnienie, wybierz pozycję Azure region, w którym znajduje się program SQL Server. Aby uzyskać więcej informacji na temat regionów zobacz [Obszary Azure](https://azure.microsoft.com/regions/).

4.  Określ, czy chcesz użyć istniejącego serwera lub utworzyć nowy serwer Azure.

    Jeśli usługi Active Directory na serwerze SQL federacji z usługą Azure Active Directory, można opcjonalnie Użyj konta usługi federacyjne dla programu SQL Server można komunikować się z serwerem zdalnym Azure. Aby uzyskać więcej informacji na temat wymagań dotyczących tej opcji zobacz [Zmiany bazy danych Ustawianie opcji (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx).

    -   **Tworzenie nowego serwera**

        1.  Tworzenie logowania i hasła administratora serwera.

        2.  Opcjonalnie można komunikować się z serwerem zdalnym Azure przy użyciu konta usługi federacyjne dla programu SQL Server.

        ![Utwórz nowy serwer Azure - Kreator rozciąganie bazy danych][StretchWizardImage4]

    -   **Istniejący serwer**

        1.  Zaznacz istniejący serwer Azure.

        2.  Wybierz metodę uwierzytelniania.

            -   Jeśli wybierzesz **Uwierzytelnianie programu SQL Server**, podaj identyfikator logowania administratora i hasło.

            -   Wybierz opcję **Zintegrowane uwierzytelnianie usługi Active Directory** można komunikować się z serwerem zdalnym Azure za pomocą konta usługi federacyjne dla programu SQL Server. Jeśli wybrany serwer nie jest zintegrowany z usługi Azure Active Directory, ta opcja nie jest wyświetlane.

        ![Zaznacz istniejący server Azure - Kreator rozciąganie bazy danych][StretchWizardImage5]

## <a name="Credentials"></a>Zabezpieczanie poświadczeń
Musisz mieć zainstalowany klucz główny bazy danych do zabezpieczenia poświadczeń, które rozciąganie bazy danych używa do łączenia się ze zdalną bazą danych.  

Jeśli klucz główny bazy danych już istnieje, wprowadź hasło dla niego.  

![Zabezpieczanie strony poświadczeń kreatora rozciąganie bazy danych][StretchWizardImage6b]

Jeśli baza danych nie ma istniejący klucz wzorca, wprowadź silne hasło, aby utworzyć klucz główny bazy danych.  

![Zabezpieczanie strony poświadczeń kreatora rozciąganie bazy danych][StretchWizardImage6]

Aby uzyskać więcej informacji na temat klucza wzorzec bazy danych zobacz [Tworzenie klucza głównego (Transact-SQL)](https://msdn.microsoft.com/library/ms174382.aspx) i [Utwórz klucz główny bazy danych](https://msdn.microsoft.com/library/aa337551.aspx). Aby uzyskać więcej informacji o poświadczenia, które kreator utworzy zobacz [Tworzenie bazy danych występujące POŚWIADCZEŃ (Transact-SQL)](https://msdn.microsoft.com/library/mt270260.aspx).

## <a name="Network"></a>Zaznacz adres IP
Aby utworzyć reguły zapory na Azure, umożliwiającego programu SQL Server komunikowanie się z serwerem zdalnym Azure za pomocą zakres adresów IP podsieci (zalecane) lub publiczny adres IP serwera SQL.

Adres IP lub adresy podane na tej stronie można określić serwer Azure umożliwia przychodzące dane, kwerend i operacji zarządzania zainicjowane przez program SQL Server za pośrednictwem zapory platformy Azure. Kreator nie powoduje zmiany w ustawieniach zapory w programie SQL Server.

![Wybierz stronę adres IP kreatora rozciąganie bazy danych][StretchWizardImage7]

## <a name="Summary"></a>Podsumowanie
Przejrzyj wprowadzone wartości i opcji wybranych w kreatorze i kosztami Azure. Następnie wybierz pozycję **Zakończ** , aby włączyć rozciąganie.

![Strona podsumowania kreatora rozciąganie bazy danych][StretchWizardImage8]

## <a name="Results"></a>Wyniki
Przejrzyj wyniki.

Aby monitorować stan migracji danych, zobacz [Monitor i rozwiązywanie problemów z migracji danych (baza danych rozciąganie)](sql-server-stretch-database-monitor.md).

![Strona wyników kreatora rozciąganie bazy danych][StretchWizardImage9]

## <a name="KnownIssues"></a>Rozwiązywanie problemów z Kreatora
**Kreator rozciąganie bazy danych nie powiodło się.**
Jeśli rozciąganie baza danych nie jest jeszcze włączona na poziomie serwera i uruchomić kreatora bez systemu uprawnienia administratora, aby ją włączyć, Kreator zakończy się niepowodzeniem. Poproś administratora systemu o włączenie rozciąganie bazy danych w wystąpieniu lokalnego serwera, a następnie ponownie uruchom kreatora. Aby uzyskać więcej informacji, zobacz [wstępne: uprawnień do włączenia rozciąganie bazy danych na serwerze](sql-server-stretch-database-enable-database.md#EnableTSQLServer).

## <a name="next-steps"></a>Następne kroki
Włącz dodatkowe tabele dla rozciąganie bazy danych. Monitorowanie migracji danych i zarządzanie nimi rozciąganie\-włączone bazy danych i tabele.

-   [Włączanie rozciąganie bazy danych dla tabeli](sql-server-stretch-database-enable-table.md) umożliwiający dodatkowe tabele.

-   [Monitor i rozwiązywanie problemów z migracji danych](sql-server-stretch-database-monitor.md) Aby wyświetlić stan migracji danych.

-   [Wstrzymywanie i wznawianie rozciąganie bazy danych](sql-server-stretch-database-pause.md)

-   [Zarządzanie i rozwiązywanie problemów z rozciąganie bazy danych](sql-server-stretch-database-manage.md)

-   [Kopii zapasowej bazy danych, które są włączone rozciąganie](sql-server-stretch-database-backup.md)

## <a name="see-also"></a>Zobacz też

[Włączanie rozciąganie bazy danych w bazie danych](sql-server-stretch-database-enable-database.md)

[Włączanie rozciąganie bazy danych dla tabeli](sql-server-stretch-database-enable-table.md)

[StretchWizardImage1]: ./media/sql-server-stretch-database-wizard/stretchwiz1.png
[StretchWizardImage2]: ./media/sql-server-stretch-database-wizard/stretchwiz2.png
[StretchWizardImage2a]: ./media/sql-server-stretch-database-wizard/stretchwiz2a.png
[StretchWizardImage2b]: ./media/sql-server-stretch-database-wizard/stretchwiz2b.png
[StretchWizardImage3]: ./media/sql-server-stretch-database-wizard/stretchwiz3.png
[StretchWizardImage4]: ./media/sql-server-stretch-database-wizard/stretchwiz4.png
[StretchWizardImage5]: ./media/sql-server-stretch-database-wizard/stretchwiz5.png
[StretchWizardImage6]: ./media/sql-server-stretch-database-wizard/stretchwiz6.png
[StretchWizardImage6b]: ./media/sql-server-stretch-database-wizard/stretchwiz6b.png
[StretchWizardImage7]: ./media/sql-server-stretch-database-wizard/stretchwiz7.png
[StretchWizardImage8]: ./media/sql-server-stretch-database-wizard/stretchwiz8.png
[StretchWizardImage9]: ./media/sql-server-stretch-database-wizard/stretchwiz9.png
