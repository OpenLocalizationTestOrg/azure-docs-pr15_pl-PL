<properties
    pageTitle="Włączanie rozciąganie bazy danych w bazie danych | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować bazę danych dla rozciąganie bazy danych."
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
    ms.topic="article"
    ms.date="08/05/2016"
    ms.author="douglasl"/>

# <a name="enable-stretch-database-for-a-database"></a>Włączanie rozciąganie bazy danych w bazie danych

Aby skonfigurować istniejącej bazy danych do bazy danych rozciąganie, wybierz pozycję zadania **| Rozciąganie | Włączanie** dla z bazą danych programu SQL Server Management Studio, aby otworzyć kreatora **Rozciąganie Włączanie bazy danych** . Można również użyć Transact\-kod SQL, aby włączyć rozciąganie bazę danych w bazie danych.

W przypadku wybrania zadania **| Rozciąganie | Włączanie** dla pojedynczej tabeli, a nie została jeszcze włączona bazy danych dla bazy danych rozciąganie, Kreator konfiguruje bazę danych dla rozciąganie bazy danych i umożliwia wybranie tabel jako część procesu. Wykonaj kroki opisane w tym temacie zamiast kroki [Włącz rozciąganie bazy danych dla tabeli](sql-server-stretch-database-enable-database.md).

Włączenie rozciąganie bazy danych w bazie danych lub tabeli wymaga db\_uprawnienia właściciela. Włączenie rozciąganie bazy danych w bazie danych wymaga uprawnień do sterowania bazy danych.

 >   [AZURE.NOTE] Później Jeśli wyłączysz rozciąganie bazy danych, należy pamiętać, że wyłączenie rozciąganie bazy danych do tabeli lub bazy danych nie powoduje usunięcia obiektu zdalnego. Aby usunąć tabeli zdalnej lub zdalna baza danych, musisz upuść ją za pomocą portalu zarządzania Azure. Zdalny obiektów w dalszym ciągu ponoszenia kosztów Azure, dopóki nie zostaną usunięte je ręcznie.

## <a name="before-you-get-started"></a>Przed rozpoczęciem

-   Przed skonfigurowaniem bazy danych dla rozciąganie zalecamy uruchamiania rozciąganie Advisor bazy danych do identyfikowania bazy danych i tabele, które są uprawnione do rozciąganie. Rozciąganie Advisor bazy danych identyfikuje również problemów z blokowaniem. Aby uzyskać więcej informacji zobacz [Identyfikowanie baz danych i tabel dla bazy danych rozciąganie](sql-server-stretch-database-identify-databases.md).

-   Przejrzyj [ograniczenia dotyczące rozciąganie bazy danych](sql-server-stretch-database-limitations.md).

-   Rozciąganie bazy danych powoduje przeniesienie danych Azure. Dlatego musisz mieć konto Azure i subskrypcji rozliczenia. Aby uzyskać konto Azure, [kliknij tutaj](http://azure.microsoft.com/pricing/free-trial/).

-   Przygotować informacje logowania i połączenia, że potrzebujesz, aby utworzyć nowy serwer Azure lub zaznacz istniejący serwer Azure.

## <a name="EnableTSQLServer"></a>Wymagania wstępne: Włączanie rozciąganie bazy danych na serwerze
Aby było możliwe włączenie rozciąganie bazy danych w bazie danych lub tabeli, musisz włączyć lokalnego serwera. Ta operacja wymaga uprawnień administratora systemu lub serveradmin.

-   Jeśli masz wymagane uprawnienia administracyjne, Kreator **Włączanie bazy danych dla rozciąganie** konfiguruje serwer dla rozciąganie.

-   Jeśli nie masz wymagane uprawnienia, administrator musi włączyć opcję ręcznie, uruchamiając **sp\_Konfigurowanie** przed uruchomieniem kreatora lub administrator musi uruchomić kreatora.

Aby ręcznie włączyć rozciąganie bazy danych na serwerze, uruchom **sp\_Konfigurowanie** i włącz opcję **Dane zdalne archiwum** . Poniższy przykład włącza opcję **Dane zdalne archiwum** , ustawiając wartość 1.

```
EXEC sp_configure 'remote data archive' , '1';
GO

RECONFIGURE;
GO
```
Aby uzyskać więcej informacji zobacz [Konfigurowanie dane zdalne archiwizowanie opcja konfiguracji serwera](https://msdn.microsoft.com/library/mt143175.aspx) i [sp_configure (Transact-SQL)](https://msdn.microsoft.com/library/ms188787.aspx).

## <a name="Wizard"></a>Aby włączyć rozciąganie bazy danych w bazie danych za pomocą Kreatora
Aby uzyskać informacje o bazie danych Włączanie Kreatora rozciąganie w tym informacje, które należy wprowadzić i opcji, które należy wprowadzić, zobacz [Wprowadzenie, uruchamiając Włączanie bazy danych, Kreator rozciąganie](sql-server-stretch-database-wizard.md).

## <a name="EnableTSQLDatabase"></a>Używanie Transact\-kod SQL, aby włączyć rozciąganie bazy danych w bazie danych
Przed włączeniem rozciąganie bazy danych na poszczególnych tabel, musisz włączyć ją w bazie danych.

Włączenie rozciąganie bazy danych w bazie danych lub tabeli wymaga db\_uprawnienia właściciela. Włączenie rozciąganie bazy danych w bazie danych wymaga uprawnień do sterowania bazy danych.

1.  Przed rozpoczęciem należy wybrać serwer Azure istniejących danych migruje rozciąganie bazy danych lub utworzyć nowy serwer Azure.

2.  Na serwerze Azure należy utworzyć regułę zapory z zakresu adresów IP programu SQL Server, który pozwala programu SQL Server komunikowanie się z serwerem zdalnym.

    Można łatwo znaleźć wartości i tworzenie reguły zapory przez próby nawiązania połączenia z serwerem Azure w Eksploratorze obiektu w SQL Server Management Studio (SSMS). SSMS ułatwia tworzenie reguły przez otwarcie okna dialogowego następujące, która już zawiera wymagane wartości adresów IP.

    ![Tworzenie reguły zapory w SSMS][FirewallRule]

3.  Aby skonfigurować bazy danych programu SQL Server dla bazy danych rozciąganie, bazy danych musi mieć klucz główny bazy danych. Klucz główny bazy danych zabezpiecza poświadczeń, które rozciąganie bazy danych używa do łączenia się ze zdalną bazą danych. Oto przykład, w którym jest tworzony nowy klucz główny bazy danych.

    ```tsql
    USE <database>;
    GO

    CREATE MASTER KEY ENCRYPTION BY PASSWORD ='<password>';
    GO
    ```

    Aby uzyskać więcej informacji na temat klucza wzorzec bazy danych zobacz [Tworzenie klucza głównego (Transact-SQL)](https://msdn.microsoft.com/library/ms174382.aspx) i [Utwórz klucz główny bazy danych](https://msdn.microsoft.com/library/aa337551.aspx).

4.  Po skonfigurowaniu bazy danych dla rozciąganie bazy danych, musisz podać poświadczenia dla rozciąganie bazę danych dla komunikacji między w środowisku lokalnym programu SQL Server, a serwerem zdalnym Azure. Istnieją dwie opcje.

    -   Można udostępnić poświadczeń administratora.

        -   Po włączeniu rozciąganie bazy danych za pomocą kreatora można utworzyć poświadczeń, w tym czasie.

        -   Jeśli planujesz włączyć rozciąganie bazy danych, uruchamiając **ALTER DATABASE**, musisz utworzyć poświadczeń ręcznie przed uruchomieniem **ALTER DATABASE** rozciąganie baza danych.

        Oto przykład tworzenia nowych poświadczeń.

        ```tsql
        CREATE DATABASE SCOPED CREDENTIAL <db_scoped_credential_name>
            WITH IDENTITY = '<identity>' , SECRET = '<secret>';
        GO
        ```

        Aby uzyskać więcej informacji o poświadczenia zobacz [Tworzenie bazy danych występujące POŚWIADCZEŃ (Transact-SQL)](https://msdn.microsoft.com/library/mt270260.aspx). Tworzenie poświadczenia wymaga uprawnienia ALTER dowolny POŚWIADCZEŃ.

    -   Konto usługi federacyjne dla programu SQL Server umożliwia komunikowanie się z serwerem zdalnym Azure, gdy są spełnione wszystkie następujące warunki.

        -   Konto usługi, na którym działa wystąpienie programu SQL Server jest konta domeny.

        -   Konta domeny należy do domeny, na których usługi Active Directory federacji z usługą Azure Active Directory.

        -   Serwer Azure zdalny jest skonfigurowany do obsługi uwierzytelniania usługi Azure Active Directory.

        -   Konto usługi, na którym działa wystąpienie programu SQL Server musi być skonfigurowany jako konto na serwerze zdalnym Azure dbmanager lub administratora systemu.

5.  Aby skonfigurować bazy danych do bazy danych rozciąganie, uruchom polecenie ZMIEŃ bazę danych.

    1.  Dla argumentu serwera wprowadź nazwę istniejącego serwera Azure, łącznie z `.database.windows.net` część nazwy \- na przykład `MyStretchDatabaseServer.database.windows.net`.

    2.  Podaj istniejące poświadczenia administratora argument POŚWIADCZENIA lub określ SFEDEROWANE\_usługi\_konta = wł. W poniższym przykładzie zestawiono istniejących poświadczeń.

    ```tsql
    ALTER DATABASE <database name>
        SET REMOTE_DATA_ARCHIVE = ON
            (
                SERVER = '<server_name>',
                CREDENTIAL = <db_scoped_credential_name>
            ) ;
    GO
    ```

## <a name="next-steps"></a>Następne kroki
-   [Włączanie rozciąganie bazy danych dla tabeli](sql-server-stretch-database-enable-table.md) umożliwiający dodatkowe tabele.

-   [Monitorowanie rozciąganie w bazie danych](sql-server-stretch-database-monitor.md) , aby wyświetlić stan migracji danych.

-   [Wstrzymywanie i wznawianie rozciąganie bazy danych](sql-server-stretch-database-pause.md)

-   [Zarządzanie i rozwiązywanie problemów z rozciąganie bazy danych](sql-server-stretch-database-manage.md)

-   [Kopii zapasowej bazy danych, które są włączone rozciąganie](sql-server-stretch-database-backup.md)

## <a name="see-also"></a>Zobacz też

[Identyfikowanie bazy danych i tabele rozciąganie bazy danych](sql-server-stretch-database-identify-databases.md)

[Instrukcja ALTER DATABASE — opcje SET (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx)

[FirewallRule]: ./media/sql-server-stretch-database-enable-database/firewall.png
