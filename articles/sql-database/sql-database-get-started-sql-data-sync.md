<properties
    pageTitle="Wprowadzenie do synchronizacji danych bazy danych SQL"
    description="Ten samouczek ułatwia rozpoczęcie pracy z synchronizacją danych SQL Azure (wersja Preview)."
    services="sql-database"
    documentationCenter=""
    authors="jennieHubbard"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/11/2016"
    ms.author="jhubbard"/>


#<a name="getting-started-with-azure-sql-data-sync-preview"></a>Wprowadzenie do synchronizacji danych Azure SQL (wersja Preview)
W tym samouczku Dowiedz się podstawy synchronizacja danych SQL Azure za pomocą portalu klasyczny Azure.

Tego samouczka przyjęto założenie minimalnego doświadczenia z programu SQL Server i bazy danych SQL Azure. W tym samouczku możesz utworzyć grupę synchronizacji hybrydowych (program SQL Server i wystąpienia bazy danych SQL) w pełni skonfigurowane i synchronizowanie zgodnie z harmonogramem, które można ustawić.

> [AZURE.NOTE] Pełną dokumentację techniczną ustawieniem synchronizacji danych SQL Azure, wcześniej znajduje się w witrynie MSDN, jest dostępny jako plik PDF. Pobierz ją [tutaj](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf).

## <a name="step-1-connect-to-the-azure-sql-database"></a>Krok 1: Nawiązywanie połączenia z bazą danych Azure SQL

1. Zaloguj się do [portalu klasyczny](http://manage.windowsazure.com).

2. Kliknij pozycję **Bazy danych programu SQL** w okienku po lewej stronie.

3. Kliknij przycisk **SYNCHRONIZUJ** w dolnej części strony. Po kliknięciu przycisku SYNCHRONIZUJ zostanie wyświetlona lista pokazywane elementy, które można dodać - **Nowa grupa synchronizacji** i **Nowy Agent synchronizacji**.

4. Aby uruchomić Kreatora nowego agenta synchronizacji danych SQL, kliknij przycisk **Nowy Agent synchronizacji**.

5. Jeśli jeszcze nie dodano agenta wcześniej, **kliknij pozycję Pobierz go w tym miejscu**.

    ![Obraz1](./media/sql-database-get-started-sql-data-sync/SQLDatabaseScreen-Figure1.PNG)


## <a name="step-2-add-a-client-agent"></a>Krok 2: Dodanie agenta klienta
Ten krok jest wymagany tylko wtedy, gdy mają zostać uwzględnione w grupie synchronizacji bazy danych programu SQL Server lokalnego. Przejdź do kroku 4, jeśli grupy synchronizacji zawiera tylko wystąpienia bazy danych SQL.

<a id="InstallRequiredSoftware"></a>
### <a name="step-2a-install-the-required-software"></a>Krok 2a: zainstalować oprogramowanie wymagane
Upewnij się, że masz zainstalowane na komputerze, na którym jest instalowane agenta klienta następujące elementy.

- **.NET framework 4.0**

 Instalowanie programu .NET Framework 4.0 [tutaj](http://go.microsoft.com/fwlink/?linkid=205836).

- **Microsoft SQL Server 2008 R2 z dodatkiem SP1 System CLR typy (x86)**

 Instalowanie programu Microsoft SQL Server 2008 R2 z dodatkiem SP1 System CLR typów (x86) [tutaj](http://www.microsoft.com/download/en/details.aspx?id=26728)

- **Obiekty zarządzania (x86) współużytkowane programu Microsoft SQL Server 2008 R2 z dodatkiem SP1**

 Instalowanie programu Microsoft SQL Server 2008 R2 z dodatkiem SP1 udostępnione zarządzania obiektów (x86) [tutaj](http://www.microsoft.com/download/en/details.aspx?id=26728)



<a id="InstallClient"></a>
### <a name="step-2b-install-a-new-client-agent"></a>Krok 2b: Instalowanie nowego agenta klienta

Postępuj zgodnie z instrukcjami [instalacji agenta klienta (synchronizacja danych SQL)](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf) do zainstalowania agenta.



<a id="RegisterSSDb"></a>
### <a name="step-2c-finish-the-new-sql-data-sync-agent-wizard"></a>Krok 2c: zakończyć działanie Kreatora nowego agenta synchronizacji danych SQL

1.  Powróć do Kreatora nowego agenta synchronizacji danych SQL.
2.  Nadaj agent opisową nazwę.
3.  Z menu rozwijanego wybierz **REGION** (centrum danych) do obsługi tego agenta.
4.  Z menu rozwijanego wybierz **SUBSKRYPCJĘ** , do obsługi tego agenta.
5.  Kliknij strzałkę w prawo.



## <a name="step-3-register-a-sql-server-database-with-the-client-agent"></a>Krok 3: Rejestrowanie bazy danych programu SQL Server przy użyciu agenta klienta

Po zainstalowaniu agenta klienta Zarejestruj każdej bazy danych programu SQL Server lokalnego, który chcesz dołączyć do grupy synchronizacji z agentem.
Aby zarejestrować bazy danych przy użyciu agenta, postępuj zgodnie z instrukcjami w [zarejestrować bazy danych programu SQL Server, z agentem klienta](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf).



## <a name="step-4-create-a-sync-group"></a>Krok 4: Tworzenie grupy synchronizacji


<a id="StartNewSGWizard"></a>
### <a name="step-4a-start-the-new-sync-group-wizard"></a>Krok 4a: Uruchom Kreatora nowej grupy synchronizacji

1.  Wróć do [portalu klasyczny](http://manage.windowsazure.com).
2.  Kliknij pozycję **bazy danych SQL**.
3.  Kliknij pozycję **Dodaj SYNCHRONIZACJI** w dolnej części strony, a następnie wybierz pozycję Nowa grupa synchronizacji z szuflady.

    ![Obraz2](./media/sql-database-get-started-sql-data-sync/NewSyncGroup-Figure2.png)



<a id=""></a>
### <a name="step-4b-enter-the-basic-settings"></a>Krok 4b: Wprowadź ustawienia podstawowe


1.  Wprowadź opisową nazwę grupy synchronizacji.
2.  Z menu rozwijanego wybierz **REGION** (centrum danych) do obsługi tej grupy synchronizacji.
3. Kliknij strzałkę w prawo.

    ![Image3](./media/sql-database-get-started-sql-data-sync/NewSyncGroupName-Figure3.PNG)


<a id="DefineHubDB"></a>
### <a name="step-4c-define-the-sync-hub"></a>Krok 4c: Definiowanie Centrum synchronizacji

1. Z menu rozwijanego wybierz wystąpienie bazy danych SQL służyć jako centrum grupy synchronizacji.
2. Wprowadź poświadczenia dla tego wystąpienia bazy danych SQL — **Centrum nazwy użytkownika** i **HASŁA Centrum**.
3. Poczekaj, aż synchronizacja danych SQL potwierdzić nazwy użytkownika i HASŁA. Zostanie wyświetlony zielony znacznik wyboru wyglądała po prawej stronie hasło poświadczenia są potwierdzone.
4. Z menu rozwijanego wybierz zasadę **Rozwiązywanie konfliktów** .

 **Centrum Wins** - zmiany zapisane w Centrum zapisu bazy danych do bazy danych, zastępowania zmiany w tej samej odwołanie rekordu bazy danych. Funkcjonalny oznacza to, że pierwszej zmiany zapisane w Centrum propaguje do innych baz danych.


 **Klient Wins** - zmiany zapisane w Centrum są zastępowane przez zmiany w bazach danych dla odwołania. Funkcjonalną oznacza to, że ostatniej zmiany zapisane do koncentratora jest zachowywana i propagowana do innych baz danych.

5.  Kliknij strzałkę w prawo.

    ![Image4](./media/sql-database-get-started-sql-data-sync/NewSyncGroupHub-Figure4.PNG)

<a id="AddRefDB"></a>
### <a name="step-4d-add-a-reference-database"></a>Krok 4d: Dodawanie odwołania bazy danych


Powtórz ten krok dla każdej dodatkowej bazy danych, który chcesz dodać do grupy synchronizacji.

1. Z menu rozwijanego wybierz bazę danych, aby dodać.

    Baz danych na liście rozwijanej zawierać obu baz danych programu SQL Server, które zostały zarejestrowane w agenta i wystąpienia bazy danych SQL.
2.  Wprowadź poświadczenia dla tej bazy danych — **nazwy użytkownika** i **HASŁA**.
3.  Z menu rozwijanego wybierz **Kierunek SYNCHRONIZACJI** dla tej bazy danych.

    **Dwukierunkowa** - zmiany w bazie danych są zapisywane w bazie danych Centrum, a zmiany w bazie danych Centrum są zapisywane w bazie danych.

    **Synchronizowanie z poziomu Centrum** - bazy danych otrzymywania aktualizacji z poziomu Centrum. Nie wysyłaj zmiany do koncentratora.

    **Synchronizacja z poziomu Centrum** - bazy danych po zaakceptowaniu do koncentratora. Zmiany w Centrum nie są zapisane z tą bazą danych.

4.  Aby zakończyć tworzenie grupy synchronizacji, kliknij znacznik wyboru w prawym dolnym rogu kreatora. Poczekaj, aż synchronizacja danych SQL potwierdzić poświadczeń. Zielony znacznik wyboru wskazuje, że poświadczenia są potwierdzone.

5.  Kliknij pole wyboru po raz drugi. Zostanie zwraca strony **SYNCHRONIZACJI** w obszarze bazy danych SQL. Ta grupa synchronizacji znajduje się teraz z innych grup synchronizacji i agentów.

    ![Image5](./media/sql-database-get-started-sql-data-sync/NewSyncGroupReference-Figure5.PNG)


## <a name="step-5-define-the-data-to-sync"></a>Krok 5: Definiowanie danych do synchronizacji

Synchronizacja danych SQL Azure umożliwia wybranie tabel i kolumn do synchronizacji. Jeśli chcesz również filtrować kolumny, że tylko wiersze z określonymi wartościami (takich jak wiek > = 65) synchronizowanie, za pomocą portalu synchronizacja danych SQL w Azure i dokumentację w wybierz tabel, kolumn i wierszy do synchronizacji do zdefiniowania danych do synchronizacji.

1.  Wróć do [portalu klasyczny](http://manage.windowsazure.com).
2.  Kliknij pozycję **bazy danych SQL**.
3.  Kliknij kartę **SYNCHRONIZACJI** .
4.  Kliknij nazwę tej grupy synchronizacji.
5.  Kliknij kartę **Reguły SYNCHRONIZACJI** .
6.  Wybierz bazę danych chcesz podać schemat grupy synchronizacji.
7.  Kliknij strzałkę w prawo.
8.  Kliknij przycisk **ODŚWIEŻ SCHEMATU**.
9.  Dla każdej tabeli w bazie danych zaznacz kolumny, które chcesz uwzględnić w synchronizacji.
    - Nie można zaznaczyć kolumny mają nieobsługiwane typy danych.
    - Jeśli zaznaczono żadnych kolumn w tabeli, tabela jest niedostępna w grupie synchronizacji.
    - Aby usunąć zaznaczenie i wybierz pozycję wszystkie tabele, kliknij przycisk Wybierz w dolnej części ekranu.
10. Kliknij przycisk **ZAPISZ**, a następnie poczekaj, aż grupie synchronizacji, aby zakończyć inicjowania obsługi administracyjnej.
11. Aby powrócić do strona początkowa synchronizacja danych, kliknij strzałkę Wstecz w lewym górnym rogu ekranu (nad nazwą grupy synchronizacji).

    ![Image6](./media/sql-database-get-started-sql-data-sync/NewSyncGroupSyncRules-Figure6.PNG)

## <a name="step-6-configure-your-sync-group"></a>Krok 6: Konfigurowanie grupy synchronizacji

Zawsze można synchronizować grupy synchronizacji, klikając pozycję SYNCHRONIZUJ w dolnej części strona początkowa synchronizacja danych.
Aby zsynchronizować zgodnie z harmonogramem, można skonfigurować grupę synchronizacji.

1.  Wróć do [portalu klasyczny](http://manage.windowsazure.com).
2.  Kliknij pozycję **bazy danych SQL**.
3.  Kliknij kartę **SYNCHRONIZACJI** .
4.  Kliknij nazwę tej grupy synchronizacji.
5.  Kliknij kartę **Konfiguruj** .
6.  **AUTOMATYCZNE SYNCHRONIZOWANIE**
    - Aby skonfigurować grupę synchronizacji, aby synchronizować na częstotliwości zestawu, kliknij przycisk **Dalej**. Możesz nadal synchronizować na żądanie, klikając pozycję SYNCHRONIZUJ.
    - Kliknij pozycję **Wył** ., aby skonfigurować grupę synchronizacji, aby synchronizować tylko po kliknięciu SYNCHRONIZACJI.
7.  **CZĘSTOTLIWOŚĆ SYNCHRONIZACJI**
    - Jeśli automatyczne synchronizowanie, Ustaw częstotliwość synchronizacji. Częstotliwość musi być między 1 miesiąca i 5 minut.
8.  Kliknij przycisk **ZAPISZ**.

![Image7](./media/sql-database-get-started-sql-data-sync/NewSyncGroupConfigure-Figure7.PNG)

Gratulacje. Utworzono grupę synchronizacji, która zawiera zarówno w przypadku wystąpienia bazy danych SQL i bazy danych programu SQL Server.

## <a name="next-steps"></a>Następne kroki
Aby uzyskać dodatkowe informacje o bazy danych SQL i synchronizacja danych SQL, zobacz:

* [Pobierz pełny dokumentacji technicznej synchronizacja danych SQL](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf)
* [Omówienie bazy danych SQL](sql-database-technical-overview.md)
* [Zarządzanie bazą danych cyklu](https://msdn.microsoft.com/library/jj907294.aspx)
 

 
