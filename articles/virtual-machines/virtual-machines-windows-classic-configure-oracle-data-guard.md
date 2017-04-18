<properties
    pageTitle="Konfigurowanie straż danych Oracle za pośrednictwem SMS | Microsoft Azure"
    description="Wyświetlanie kolejnych samouczek konfigurowania i wykonywania straż danych Oracle na Azure maszyn wirtualnych do wysokiej dostępności i awarii odzyskiwania."
    services="virtual-machines-windows"
    authors="rickstercdn"
    manager="timlt"
    documentationCenter=""
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure-services"
    ms.date="09/06/2016"
    ms.author="rclaus" />

#<a name="configuring-oracle-data-guard-for-azure"></a>Konfigurowanie straż danych programu Oracle dla Azure


Ten samouczek przedstawiono sposób konfigurowania i zaimplementować zabezpieczenia danych programu Oracle w środowisku maszyn wirtualnych Azure wysokiej dostępności i awarii. Samouczek omówiono jednokierunkowe replikacji dla baz danych Oracle nie RAC.

Straż danych programu Oracle obsługuje ochrona danych i odzyskiwanie bazy danych programu Oracle. Jest proste, wysokiej wydajności, dzięki wsuwanej konstrukcji rozwiązanie awarii, ochrona danych i wysoką dostępność dla całej bazy danych programu Oracle.

Ten samouczek założono, że już teoretyczna i praktyczne wiedzę na pojęcia wysoką dostępność bazy danych programu Oracle i awarii. Aby uzyskać informacje zobacz [Oracle witryny sieci web](http://www.oracle.com/technetwork/database/features/availability/index.html) , a także [pojęcia straż danych programu Oracle i Przewodnik administrowania](https://docs.oracle.com/cd/E11882_01/server.112/e41134/toc.htm).

Ponadto samouczka przyjęto założenie, masz już wdrożone następujące wymagania wstępne:

- Już przejrzeniu sekcji wysokiej dostępności i zagadnienia odzyskiwania po awarii w temacie [maszyn wirtualnych Oracle obrazy — różne zagadnienia](virtual-machines-windows-classic-oracle-considerations.md) . Azure obsługuje obecnie autonomicznego wystąpienia bazy danych programu Oracle, ale nie Oracle rzeczywistą aplikacji klastrów (Oracle RAC).


- Utworzono dwa wirtualnych maszyn platformy Azure za pomocą tej samej platformie opisane obraz Oracle Enterprise Edition. Upewnij się, że maszyn wirtualnych są w tej [samej usługi w chmurze](virtual-machines-windows-load-balance.md) i w tej samej sieci wirtualnych, aby upewnić się, że mogą uzyskać dostęp siebie na stałe prywatny adres IP. Ponadto zalecane jest, aby umieścić maszyny wirtualne w tym samym [Ustawianie dostępności](virtual-machines-windows-manage-availability.md) umożliwia Azure, aby umieścić je w osobnych błędów domen i uaktualniania domen. Straż danych Oracle jest dostępna tylko w wersji Enterprise bazy danych programu Oracle. Każdy komputer musi mieć co najmniej 2 GB pamięci i 5 GB miejsca na dysku. Aby uzyskać najbardziej aktualne informacje na platformie opisane rozmiarów maszyn wirtualnych zobacz [Maszyn wirtualnych rozmiarów Azure](virtual-machines-windows-sizes.md). Jeśli potrzebujesz dodatkowych woluminu za pośrednictwem usługi SMS można dołączyć dodatkowe dyski. Aby uzyskać informacje zobacz [Dołączanie dysku danych maszyn wirtualnych](virtual-machines-windows-classic-attach-disk.md).



- Nazwy maszyn wirtualnych została Ustaw jako "Komputer1" podstawowego maszyn wirtualnych i "Komputer2" dla wstrzymania maszyn wirtualnych w portalu klasyczny Azure.

- Utworzono zmiennej środowiska **ORACLE_HOME** , aby wskazywały tej samej ścieżki instalacji głównego oracle w podstawowy i wstrzymania maszyn wirtualnych, takich jak `C:\OracleDatabase\product\11.2.0\dbhome_1\database`.

- Możesz zalogować się do swojego serwera systemu Windows jako członek grupy **Administratorzy** lub członkiem grupy **ORA_DBA** .

W tym samouczku dowiesz się:

Implementowanie środowiska fizycznej wstrzymania bazy danych

1. Tworzenie podstawowego bazy danych

2. Przygotowywanie podstawowej bazy danych do tworzenia wstrzymania bazy danych

    1. Włącz rejestrowanie wymuszonego

    2. Tworzenie pliku hasła

    3. Konfigurowanie dziennika wstrzymania wykonaj ponownie

    4. Włączanie archiwizacji

    5. Ustaw parametry inicjowania podstawowej bazy danych

Tworzenie fizycznie wstrzymania bazy danych

1. Przygotowanie pliku parametru inicjowania wstrzymania bazy danych

2. Konfigurowanie odbiornika i tnsnames do obsługi bazy danych na komputerach podstawowy i wstrzymania

    1. Skonfiguruj listener.ora na obu serwerach do przechowywania wpisów dla obu baz danych

    2. Aby wstrzymać pozycje podstawowych i wstrzymania baz danych, należy skonfigurować tnsnames.ora w przypadku maszyn wirtualnych podstawowy i wstrzymania. 

    3. Uruchom odbiornika i sprawdź tnsping na obu maszyn wirtualnych do obu usług.

3. Pierwsze spojrzenie na wstrzymania wystąpienie w stanie bez instalowania

4. Aby klonowanie bazy danych i tworzenie wstrzymania bazy danych za pomocą RMAN

5. Uruchamianie fizycznie wstrzymania bazy danych w trybie odzyskiwania zarządzanych

6. Sprawdź fizycznej wstrzymania bazy danych

> [AZURE.IMPORTANT] Ten samouczek został Konfigurowanie i przetestowane pod kątem zgodności następującej konfiguracji sprzętu i oprogramowania:
>
>|                      | **Podstawowy bazy danych**                      | **Wstrzymania bazy danych**                      |
>|----------------------|-------------------------------------------|-------------------------------------------|
>| **Oracle w wersji**   | Wersja Enterprise Oracle11g (11.2.0.4.0) | Wersja Enterprise Oracle11g (11.2.0.4.0) |
>| **Nazwa komputera**     | KOMPUTER1                                  | KOMPUTER2                                  |
>| **System operacyjny** | Windows 2008 R2                           | Windows 2008 R2                           |
>| **Oracle SID**       | TEST                                      | TEST\_STBY                                |
>| **Pamięci**           | Funkcje min 2 GB                                  | Funkcje min 2 GB                                  |
>| **Miejsce na dysku**       | Funkcje min 5 GB                                  | Funkcje min 5 GB                                  |

Dla kolejnych wersjach bazy danych programu Oracle i zabezpieczenia danych programu Oracle może być niektóre dodatkowe zmiany, które są potrzebne do wykonania. Aby uzyskać najbardziej aktualne informacje specyficzne dla wersji dokumentacji [Straż danych](http://www.oracle.com/technetwork/database/features/availability/data-guard-documentation-152848.html) i [Baz danych programu Oracle](http://www.oracle.com/us/corporate/features/database-12c/index.html) w witrynie sieci web programu Oracle.

##<a name="implement-the-physical-standby-database-environment"></a>Implementowanie środowiska fizycznej wstrzymania bazy danych
### <a name="1-create-a-primary-database"></a>1. Tworzenie podstawowego bazy danych

- Tworzenie podstawowego bazy danych "TEST", podstawowego maszyny wirtualnej. Aby uzyskać informacje Zobacz Tworzenie i Konfigurowanie bazy danych programu Oracle.
- Aby zobaczyć nazwę bazy danych, połączenia z bazą danych jako użytkownika dotyczącego z roli grupy SYSDBA w SQL * oraz wiersz polecenia i uruchom następującą instrukcję:

        SQL> select name from v$database;

        The result will display like the following:

        NAME
        ---------
        TEST
- Następnie kwerendy nazwy plików bazy danych w widoku systemu dba_data_files:

        SQL> select file_name from dba_data_files;
        FILE_NAME
        -------------------------------------------------------------------------------
        C:\ <YourLocalFolder>\TEST\USERS01.DBF
        C:\ <YourLocalFolder>\TEST\UNDOTBS01.DBF
        C:\ <YourLocalFolder>\TEST\SYSAUX01.DBF
        C:\<YourLocalFolder>\TEST\SYSTEM01.DBF
        C:\<YourLocalFolder>\TEST\EXAMPLE01.DBF

### <a name="2-prepare-the-primary-database-for-standby-database-creation"></a>2. przygotować podstawowej bazy danych do utworzenia wstrzymania bazy danych

Przed utworzeniem wstrzymania bazy danych, zalecane jest, że należy upewnić się, że podstawowej bazy danych jest poprawnie skonfigurowany. Oto lista czynności, które trzeba wykonać:

1. Włącz rejestrowanie wymuszonego
2. Tworzenie pliku hasła
3. Konfigurowanie dziennika wstrzymania wykonaj ponownie
4. Włączanie archiwizacji
5. Ustaw parametry inicjowania podstawowej bazy danych

#### <a name="enable-forced-logging"></a>Włącz rejestrowanie wymuszonego

Aby wdrożyć wstrzymania bazy danych, należy włączyć wymuszone rejestrowania w podstawowej bazy danych. Ta opcja zapewnia, że nawet jeśli zakończeniu operacji "nologging" Rejestrowanie życie ma pierwszeństwo i wszystkie operacje są rejestrowane w dziennikach wykonaj ponownie. W związku z tym firma Microsoft upewnij się, że wszystko w podstawowej bazy danych jest zalogowany i replikacja gotowości zawiera wszystkie operacje w podstawowej bazy danych. Aby włączyć rejestrowanie życie, uruchom instrukcja alter database:

    SQL> ALTER DATABASE FORCE LOGGING;

    Database altered.

#### <a name="create-a-password-file"></a>Tworzenie pliku hasła

Aby można było wysłać i stosowanie dzienników zarchiwizowanych z podstawowego serwera na serwerze wstrzymania, hasło dotyczącego muszą być identyczne na serwerach podstawowych i wstrzymania. Dlatego utworzyć plik hasła na głównej bazy danych i skopiuj go do serwera gotowości.

>[AZURE.IMPORTANT] Podczas korzystania z bazy danych programu Oracle 12c, istnieje nowego użytkownika, **SYSDG**, które służy do zarządzania straż danych programu Oracle. Aby uzyskać więcej informacji zobacz [zmiany w bazie danych programu Oracle 12 c wersji](http://docs.oracle.com/database/121/UNXAR/release_changes.htm#UNXAR404).

Ponadto, upewnij się, że ORACLE\_środowiska główne jest już zdefiniowana w Komputer1. Jeśli nie, należy zdefiniować go jako zmiennej środowiska przy użyciu okna dialogowego zmienne środowiska. Aby uzyskać dostęp do tego okna dialogowego, uruchom narzędzie **systemowe** , klikając dwukrotnie ikonę System w **Panelu sterowania**. następnie kliknij kartę **Zaawansowane** i wybierz **Zmienne środowiska**. Aby ustawić zmienne środowiska, kliknij przycisk **Nowy** w obszarze **Zmienne systemowe**. Po skonfigurowaniu zmienne środowiska, zamknij istniejącego wiersza polecenia systemu Windows i otworzyć nowy.

Uruchom następującą instrukcję, aby przełączyć się do programu Oracle\_katalogu Narzędzia główne, na przykład C:\\OracleDatabase\\produktu\\11.2.0\\dbhome\_1\\bazy danych.

    cd %ORACLE_HOME%\database

Następnie utwórz plik hasła przy użyciu narzędzia tworzenia plików hasła, [ORAPWD](http://docs.oracle.com/cd/B28359_01/server.111/b28310/dba007.htm). W tym samym wierszu polecenia systemu Windows w Komputer1 uruchom następujące polecenie, ustawiając wartość hasło jako hasło **dotyczącego**:

    ORAPWD FILE=PWDTEST.ora PASSWORD=password FORCE=y

To polecenie tworzy plik hasła, o nazwie jako PWDTEST.ora w ORACLE\_dla użytkowników domowych\\katalogu bazy danych. Należy skopiować ten plik na % ORACLE\_główne %\\bazy danych katalogu KOMPUTER2 ręcznie.

#### <a name="configure-a-standby-redo-log"></a>Konfigurowanie dziennika wstrzymania wykonaj ponownie

Następnie należy skonfigurować wstrzymania dziennika wykonaj ponownie, aby podstawowych poprawnie mógł otrzymywać operacji Wykonaj ponownie, gdy są one wstrzymania. Przed ich tworzeniu tutaj pozwala dzienniki Powtórz wstrzymania ma być tworzone w stanie wstrzymania. Należy skonfigurować wstrzymania ponowne dzienniki (SRL) o tym samym rozmiarze, jak online ponowne dzienników. Rozmiar bieżących plików dziennika wstrzymania Powtórz muszą dokładnie odpowiadać rozmiar bieżących plików dziennika online Powtórz podstawowej bazy danych.

Uruchom następującą instrukcję SQL\*PLUS wiersza polecenia Komputer1. Plik dziennika $ v jest widok systemu, który zawiera informacje o plikach dziennika wykonaj ponownie.

    SQL> select * from v$logfile;
    GROUP# STATUS  TYPE    MEMBER                                                       IS_
    ---------- ------- ------- ------------------------------------------------------------ ---
    3         ONLINE  C:\<YourLocalFolder>\TEST\REDO03.LOG               NO
    2         ONLINE  C:\<YourLocalFolder>\TEST\REDO02.LOG               NO
    1         ONLINE  C:\<YourLocalFolder>\TEST\REDO01.LOG               NO
    Next, query the v$log system view, displays log file information from the control file.
    SQL> select bytes from v$log;
    BYTES
    ----------
    52428800
    52428800
    52428800


Należy zauważyć, że 52428800 50 megabajtów.

Następnie w SQL\*Plus okna, uruchom następujące instrukcje dodawania nowej grupy plików dziennika Powtórz gotowości do wstrzymania bazy danych i określ numer, który identyfikuje grupy za pomocą klauzuli grupy. Za pomocą grupowania liczb można wprowadzić administrowanie grup plików dziennika wstrzymania Powtórz łatwiejsze:

    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 4 'C:\<YourLocalFolder>\TEST\REDO04.LOG' SIZE 50M;
    Database altered.
    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 5 'C:\<YourLocalFolder>\TEST\REDO05.LOG' SIZE 50M;
    Database altered.
    SQL> ALTER DATABASE ADD STANDBY LOGFILE GROUP 6 'C:\<YourLocalFolder>\TEST\REDO06.LOG' SIZE 50M;
    Database altered.

Następnie uruchom następujące widoku systemu Aby wyświetlić informacje o wykonaj ponownie pliki dziennika. Tej operacji również sprawdza, czy zostały utworzone grupy plików dziennika wstrzymania wykonaj ponownie:

    SQL> select * from v$logfile;
    GROUP# STATUS  TYPE MEMBER IS_
    ---------- ------- ------- --------------------------------------------- ---
    3         ONLINE C:\<YourLocalFolder>\TEST\REDO03.LOG NO
    2         ONLINE C:\<YourLocalFolder>\TEST\REDO02.LOG NO
    1         ONLINE C:\<YourLocalFolder>\TEST\REDO01.LOG NO
    4         STANDBY C:\<YourLocalFolder>\TEST\REDO04.LOG
    5         STANDBY C:\<YourLocalFolder>\TEST\REDO05.LOG NO
    6         STANDBY C:\<YourLocalFolder>\TEST\REDO06.LOG NO
    6 rows selected.

#### <a name="enable-archiving"></a>Włączanie archiwizacji

Następnie należy włączyć, archiwizowanie, uruchamiając następujące instrukcje jest umieszczenie podstawowej bazy danych w trybie ARCHIVELOG i włączyć automatyczne archiwizowanie. Możesz włączyć tryb dziennika archiwum instalowanie bazy danych, a następnie wykonywanie polecenia archivelog.

Najpierw zaloguj się jako grupy sysdba. W wierszu polecenia systemu Windows uruchom polecenie:

    sqlplus /nolog

    connect / as sysdba

Następnie zamknięcia z bazą danych programu SQL\*Plus wiersza polecenia:

    SQL> shutdown immediate;
    Database closed.
    Database dismounted.
    ORACLE instance shut down.

Następnie należy wykonać polecenie instalacji uruchamiania, aby zainstalować bazę danych. Dzięki temu, że Oracle przypisuje wystąpienie określonej bazie danych.

    SQL> startup mount;
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes
    Database mounted.

Następnie należy uruchomić:

    SQL> alter database archivelog;
    Database altered.

Następnie uruchom Alter ALTER database z klauzuli Otwórz, aby udostępnić bazę danych do normalnego użytkowania:

    SQL> alter database open;

    Database altered.

#### <a name="set-primary-database-initialization-parameters"></a>Ustaw parametry inicjowania podstawowej bazy danych

Aby skonfigurować zabezpieczenia danych, należy utworzyć i skonfigurować wstrzymania parametrów na zwykłą pfile (plik tekstowy parametru inicjowania) pierwszego. Gdy pfile będzie gotowa, należy przekonwertować go na plik parametru server (plik).

Możesz sterować środowiska straż danych przy użyciu parametrów podczas inicjowania. Plik ORA. Po tego samouczka, należy zaktualizować podstawowej inicjowania bazy danych. ORA, że mogą być przechowywane obu ról: podstawowa lub wstrzymania.

    SQL> create pfile from spfile;
    File created.

Następnie należy edytować pfile, aby dodać parametry wstrzymania. Aby to zrobić, otwórz INITTEST. ORA pliku w lokalizacji % ORACLE\_główne %\\bazy danych. Następnie należy dołączyć następujące instrukcje do pliku INITTEST.ora. Konwencję nazewnictwa dla swojej inicjowania. Plik ORA jest inicjowania\<YourDatabaseName\>. ORA.

    db_name='TEST'
    db_unique_name='TEST'
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(TEST,TEST_STBY)'
    LOG_ARCHIVE_DEST_1= 'LOCATION=C:\OracleDatabase\archive   VALID_FOR=(ALL_LOGFILES,ALL_ROLES) DB_UNIQUE_NAME=TEST'
    LOG_ARCHIVE_DEST_2= 'SERVICE=TEST_STBY LGWR ASYNC VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) DB_UNIQUE_NAME=TEST_STBY'
    LOG_ARCHIVE_DEST_STATE_1=ENABLE
    LOG_ARCHIVE_DEST_STATE_2=ENABLE
    REMOTE_LOGIN_PASSWORDFILE=EXCLUSIVE
    LOG_ARCHIVE_FORMAT=%t_%s_%r.arc
    LOG_ARCHIVE_MAX_PROCESSES=30
    # Standby role parameters --------------------------------------------------------------------
    fal_server=TEST_STBY
    fal_client=TEST
    standby_file_management=auto
    db_file_name_convert='TEST_STBY','TEST'
    log_file_name_convert='TEST_STBY','TEST'
    # ---------------------------------------------------------------------------------------------


Poprzedni blok instrukcji zawiera trzy elementy konfiguracji ważne:
-   **LOG_ARCHIVE_CONFIG...:** Definiowanie identyfikatorów unikatowych bazy danych za pomocą tej instrukcji.
-   **LOG_ARCHIVE_DEST_1...:** Można zdefiniować za pomocą tej instrukcji lokalizację folderu archiwum lokalnego. Zalecamy utworzenia nowego katalogu na potrzeby archiwizacji bazy danych i określ lokalizację archiwum lokalnego za pomocą tej instrukcji jawnie zamiast programu Oracle domyślnego folderu % ORACLE_HOME%\database\archive.
-   **LOG_ARCHIVE_DEST_2... ... ASYNCHRONICZNYCH LGWR:** Definiowanie procesem writer asynchronicznych (LGWR) do zbierania danych i ponowne wykonywanie czynności transakcji i przekazuje je do miejsc docelowych wstrzymania. W tym miejscu DB_UNIQUE_NAME Określa unikatową nazwę bazy danych na docelowym serwerze wstrzymania.

Gdy nowy plik parametru będzie gotowa, należy utworzyć ten plik z niego.

Przede wszystkim, zamknięcie bazy danych:

    SQL> shutdown immediate;

    Database closed.

    Database dismounted.

    ORACLE instance shut down.

Następnie uruchom polecenie bez instalowania uruchamiania w następujący sposób:

    SQL> startup nomount pfile='c:\OracleDatabase\product\11.2.0\dbhome_1\database\initTEST.ora';
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes

Teraz można utworzyć plik programu SharePoint:

    SQL>create spfile frompfile='c:\OracleDatabase\product\11.2.0\dbhome\_1\database\initTEST.ora';

    File created.

Następnie zamknięcie bazy danych:

    SQL> shutdown immediate;

    ORA-01507: database not mounted

Następnie użyj polecenia uruchamiania, aby uruchomić wystąpienie:

    SQL> startup;
    ORACLE instance started.
    Total System Global Area 1503199232 bytes
    Fixed Size                  2281416 bytes
    Variable Size             922746936 bytes
    Database Buffers          570425344 bytes
    Redo Buffers                7745536 bytes
    Database mounted.
    Database opened.

##<a name="create-a-physical-standby-database"></a>Tworzenie fizycznie wstrzymania bazy danych
W tej sekcji omówiono czynności, które należy wykonać w KOMPUTER2 do przygotowania fizycznej wstrzymania bazy danych.

Najpierw musisz pulpitu zdalnego KOMPUTER2 za pośrednictwem portalu klasyczny Azure.

Następnie na serwerze wstrzymania (KOMPUTER2) Utwórz wszystkie foldery niezbędne dla wstrzymania bazie danych, na przykład C:\\\<YourLocalFolder\>\\TEST. Podczas tego samouczka, upewnij się, że struktura folderów odpowiada struktura folderów na KOMPUTER1, aby zachować wszystkie niezbędne pliki, takie jak pliki controlfile, plików danych, redologfiles, udump, bdump i cdump. Ponadto zdefiniować ORACLE\_dla użytkowników domowych i ORACLE\_zmiennych środowiska podstawowej w KOMPUTER2. Jeśli nie, należy je zdefiniować jako zmienna środowiska przy użyciu okna dialogowego zmiennych środowiska. Aby uzyskać dostęp do tego okna dialogowego, uruchom narzędzie **systemowe** , klikając dwukrotnie ikonę System w **Panelu sterowania**. następnie kliknij kartę **Zaawansowane** i wybierz **Zmienne środowiska**. Aby ustawić zmienne środowiska, kliknij przycisk **Nowy** w obszarze **Zmienne systemowe**. Po skonfigurowaniu zmienne środowiska, musisz zamknąć istniejącego wiersza polecenia systemu Windows i otworzyć nową, aby zobaczyć zmiany.

Następnie wykonaj następujące czynności:

1. Przygotowanie pliku parametru inicjowania wstrzymania bazy danych

2. Konfigurowanie odbiornika i tnsnames do obsługi bazy danych na komputerach podstawowy i wstrzymania

    1. Skonfiguruj listener.ora na obu serwerach do przechowywania wpisów dla obu baz danych

    2. Konfigurowanie tnsnames.ora w przypadku maszyn wirtualnych podstawowy i gotowości do przechowywania wpisów podstawowych i wstrzymania baz danych

    3. Uruchom odbiornika i sprawdź tnsping na obu maszyn wirtualnych do obu usług.

3. Pierwsze spojrzenie na wstrzymania wystąpienie w stanie bez instalowania

4. Aby klonowanie bazy danych i tworzenie wstrzymania bazy danych za pomocą RMAN

5. Uruchamianie fizycznie wstrzymania bazy danych w trybie odzyskiwania zarządzanych

6. Sprawdź fizycznej wstrzymania bazy danych

### <a name="1-prepare-an-initialization-parameter-file-for-standby-database"></a>1. Przygotuj plik parametru inicjowania wstrzymania bazy danych

W tej sekcji przedstawiono sposób Przygotuj plik parametru inicjowania wstrzymania bazy danych. W tym celu należy najpierw skopiować INITTEST. ORA pliku z komputera 1 do KOMPUTER2 ręcznie. Powinny być widoczne INITTEST. ORA plików w folderze % ORACLE\_główne %\\folder bazy danych w obu komputerach. Następnie należy zmodyfikować plik INITTEST.ora KOMPUTER2 skonfigurować go do roli wstrzymania w sposób określony poniżej:

    db_name='TEST'
    db_unique_name='TEST_STBY'
    db_create_file_dest='c:\OracleDatabase\oradata\test_stby’
    db_file_name_convert=’TEST’,’TEST_STBY’
    log_file_name_convert='TEST','TEST_STBY'


    job_queue_processes=10
    LOG_ARCHIVE_CONFIG='DG_CONFIG=(TEST,TEST_STBY)'
    LOG_ARCHIVE_DEST_1='LOCATION=c:\OracleDatabase\TEST_STBY\archives VALID_FOR=(ALL_LOGFILES,ALL_ROLES) DB_UNIQUE_NAME=’TEST'
    LOG_ARCHIVE_DEST_2='SERVICE=TEST LGWR ASYNC VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE)
    LOG_ARCHIVE_DEST_STATE_1='ENABLE'
    LOG_ARCHIVE_DEST_STATE_2='ENABLE'
    LOG_ARCHIVE_FORMAT='%t_%s_%r.arc'
    LOG_ARCHIVE_MAX_PROCESSES=30


Poprzedni blok instrukcji zawiera dwa elementy konfiguracji ważne:

-   **\*. LOG_ARCHIVE_DEST_1:** należy utworzyć c:\OracleDatabase\TEST_STBY\archives folder w KOMPUTER2 ręcznie.
-   **\*. LOG_ARCHIVE_DEST_2:** jest to krok opcjonalny. Należy ustawić tak jak może być konieczne, gdy podstawowy komputer jest w stanie konserwacji i gotowości komputera staje się podstawowej bazy danych.

Następnie należy uruchomić wystąpienie w stanie wstrzymania. Na serwerze bazy danych wstrzymania wpisz następujące polecenie w wierszu polecenia systemu Windows, aby utworzyć wystąpienia programu Oracle, tworząc usługi systemu Windows:

    oradim -NEW -SID TEST\_STBY -STARTMODE MANUAL

Polecenie **Oradim** tworzy wystąpienie programu Oracle, ale nie zostanie uruchomiony. Można je znaleźć w C:\\OracleDatabase\\produktu\\11.2.0\\dbhome\_1\\katalogu KOSZA.

##<a name="configure-the-listener-and-tnsnames-to-support-the-database-on-primary-and-standby-machines"></a>Konfigurowanie odbiornika i tnsnames do obsługi bazy danych na komputerach podstawowy i wstrzymania
Przed utworzeniem wstrzymania bazy danych należy upewnić się, że podstawowy i wstrzymania baz danych w konfiguracji można porozmawiać ze sobą. Aby to zrobić, musisz skonfigurować odbiornika i TNSNames ręcznie lub przy użyciu narzędzia konfiguracji sieci NETCA. To jest obowiązkowe zadania za pomocą narzędzia Menedżer odzyskiwania (RMAN).

### <a name="configure-listenerora-on-both-servers-to-hold-entries-for-both-databases"></a>Skonfiguruj listener.ora na obu serwerach do przechowywania wpisów dla obu baz danych

Pulpit zdalny Komputer1 i Edytuj plik listener.ora jako określone poniżej. Zawsze podczas edytowania pliku listener.ora, upewnij się, że otwierający i zamykający nawias wyrównać w tej samej kolumnie. Plik listener.ora można znaleźć w następującym folderze: c:\\OracleDatabase\\produktu\\11.2.0\\dbhome\_1\\sieci\\administrator\\.

    # listener.ora Network Configuration File: C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\listener.ora

    # Generated by Oracle configuration tools.

    SID_LIST_LISTENER =
      (SID_LIST =
        (SID_DESC =
          (SID_NAME = test)
          (ORACLE_HOME = C:\OracleDatabase\product\11.2.0\dbhome_1)
          (PROGRAM = extproc)
          (ENVS = "EXTPROC_DLLS=ONLY:C:\OracleDatabase\product\11.2.0\dbhome_1\bin\oraclr11.dll")
        )
      )

    LISTENER =
      (DESCRIPTION_LIST =
        (DESCRIPTION =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
          (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
        )
      )

Następny, zdalny pulpit, aby KOMPUTER2 i Edycja listener.ora pliku w następujący sposób: # listener.ora pliku konfiguracji sieci: C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\listener.ora

    # Generated by Oracle configuration tools.

    SID_LIST_LISTENER =
      (SID_LIST =
        (SID_DESC =
          (SID_NAME = test_stby)
          (ORACLE_HOME = C:\OracleDatabase\product\11.2.0\dbhome_1)
          (PROGRAM = extproc)
          (ENVS = "EXTPROC_DLLS=ONLY:C:\OracleDatabase\product\11.2.0\dbhome_1\bin\oraclr11.dll")
        )
      )

    LISTENER =
      (DESCRIPTION_LIST =
        (DESCRIPTION =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
          (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
        )
      )


### <a name="configure-tnsnamesora-on-the-primary-and-standby-virtual-machines-to-hold-entries-for-both-primary-and-standby-databases"></a>Konfigurowanie tnsnames.ora w przypadku maszyn wirtualnych podstawowy i gotowości do przechowywania wpisów podstawowych i wstrzymania baz danych

Pulpit zdalny Komputer1 i edytowania pliku tnsnames.ora jako określone poniżej. Pliku tnsnames.ora można znaleźć w następującym folderze: c:\\OracleDatabase\\produktu\\11.2.0\\dbhome\_1\\sieci\\administrator\\.

    TEST =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test)
        )
      )

    TEST_STBY =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test_stby)
        )
      )

Pulpit zdalny KOMPUTER2 i Edytuj tnsnames.ora pliku w następujący sposób:

    TEST =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test)
        )
      )

    TEST_STBY =
      (DESCRIPTION =
        (ADDRESS_LIST =
          (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))
        )
        (CONNECT_DATA =
          (SERVICE_NAME = test_stby)
        )
      )


### <a name="start-the-listener-and-check-tnsping-on-both-virtual-machines-to-both-services"></a>Uruchom odbiornika i sprawdź tnsping na obu maszyn wirtualnych do obu usług.

Otwarcie nowego wiersza polecenia systemu Windows w środowisku maszyn wirtualnych podstawowych i wstrzymania i uruchom następujące instrukcje:

    C:\Users\DBAdmin>tnsping test

    TNS Ping Utility for 64-bit Windows: Version 11.2.0.1.0 - Production on 14-NOV-2013 06:29:08
    Copyright (c) 1997, 2010, Oracle.  All rights reserved.
    Used parameter files:
    C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\sqlnet.ora
    Used TNSNAMES adapter to resolve the alias
    Attempting to contact (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE1)(PORT = 1521))) (CONNECT_DATA = (SER
    VICE_NAME = test)))
    OK (0 msec)


    C:\Users\DBAdmin>tnsping test_stby

    TNS Ping Utility for 64-bit Windows: Version 11.2.0.1.0 - Production on 14-NOV-2013 06:29:16
    Copyright (c) 1997, 2010, Oracle.  All rights reserved.
    Used parameter files:
    C:\OracleDatabase\product\11.2.0\dbhome_1\network\admin\sqlnet.ora
    Used TNSNAMES adapter to resolve the alias
    Attempting to contact (DESCRIPTION = (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = MACHINE2)(PORT = 1521))) (CONNECT_DATA = (SER
    VICE_NAME = test_stby)))
    OK (260 msec)


##<a name="start-up-the-standby-instance-in-nomount-state"></a>Pierwsze spojrzenie na wstrzymania wystąpienie w stanie bez instalowania
Aby skonfigurować środowisko programu do obsługi wstrzymania bazy danych w stanie wstrzymania maszyny wirtualnej (KOMPUTER2).

Najpierw skopiuj hasło plik z komputera podstawowego (Komputer1) do gotowości komputera (KOMPUTER2) ręcznie. Jest to konieczne, ponieważ hasło **dotyczącego** musi być identyczne na obu komputerach.

Następnie otwórz okno wiersza polecenia systemu Windows w KOMPUTER2 i konfiguracji zmiennych środowiska, aby wskazywały wstrzymania bazy danych w następujący sposób:

    SET ORACLE_HOME=C:\OracleDatabase\product\11.2.0\dbhome_1
    SET ORACLE_SID=TEST_STBY

Następnie należy utworzyć bazę danych wstrzymania w stanie bez instalowania, a następnie Wygeneruj plik programu SharePoint.

Uruchamianie bazy danych:

    SQL>shutdown immediate;

    SQL>startup nomount
    ORACLE instance started.

    Total System Global Area  747417600 bytes
    Fixed Size                  2179496 bytes
    Variable Size             473960024 bytes
    Database Buffers          264241152 bytes
    Redo Buffers                7036928 bytes


##<a name="use-rman-to-clone-the-database-and-to-create-a-standby-database"></a>Aby klonowanie bazy danych i tworzenie wstrzymania bazy danych za pomocą RMAN
Odzyskiwanie Menedżera (RMAN) służy do dowolnego kopię zapasową podstawowego bazy danych, tworzenie fizycznie wstrzymania bazy danych.

Pulpit zdalny wstrzymania maszyn wirtualnych (KOMPUTER2) i uruchom narzędzie RMAN, określając pełne połączenie ciągu dla docelowej (podstawowy bazy danych, Komputer1) i POMOCNICZY (wstrzymania bazy danych, KOMPUTER2) wystąpienia.

>[AZURE.IMPORTANT] Nie należy używać uwierzytelniania systemu operacyjnego, ponieważ istnieje żadna baza danych na komputerze serwer w stanie wstrzymania jeszcze.

    C:\> RMAN TARGET sys/password@test AUXILIARY sys/password@test_STBY

    RMAN>DUPLICATE TARGET DATABASE
      FOR STANDBY
      FROM ACTIVE DATABASE
      DORECOVER
        NOFILENAMECHECK;

##<a name="start-the-physical-standby-database-in-managed-recovery-mode"></a>Uruchamianie fizycznie wstrzymania bazy danych w trybie odzyskiwania zarządzanych
Ten samouczek przedstawia sposób tworzenia fizycznej wstrzymania bazy danych. Aby uzyskać informacje o tworzeniu logiczne wstrzymania bazy danych zobacz dokumentację programu Oracle.

Otwiera SQL\*oraz wierszu polecenia i włączanie ochrony danych w stanie wstrzymania maszyn wirtualnych lub serwera (KOMPUTER2) w następujący sposób:

    SHUTDOWN IMMEDIATE;
    STARTUP MOUNT;
    ALTER DATABASE RECOVER MANAGED STANDBY DATABASE DISCONNECT FROM SESSION;

Po otwarciu wstrzymania bazy danych w trybie **instalacji** , Archiwizowanie dziennika wysyłki będzie nadal występował, a proces odzyskiwania zarządzanych ich dziennika obowiązującej w stanie wstrzymania bazy danych. Dzięki temu, że wstrzymania bazy danych pozostaje aktualne z podstawową bazą danych. Pamiętaj, że wstrzymania bazy danych nie mogą być dostępne w raportach w tym czasie.

Po otwarciu wstrzymania bazy danych w trybie **Odczytu tylko** archiwum logowania wysyłki będzie nadal występował. Ale przestaje procesu odzyskiwania zarządzane. Ta opcja powoduje stać się bardziej nieaktualne do momentu wznowienia procesu odzyskiwania zarządzanych wstrzymania bazy danych. W tym czasie dostępem wstrzymania bazy danych w raportach, ale dane mogą nie odzwierciedlać najnowsze zmiany.

Ogólnie zaleca się zachować wstrzymania bazy danych w trybie **instalacji** aktualne dane w bazie danych wstrzymania przypadku awarii głównej bazy danych. Jednak możesz zachować wstrzymania bazy danych w trybie **Odczytu tylko** na potrzeby raportowania w zależności od potrzeb aplikacji. Następująca procedura przedstawia sposób włączania ochrony danych w trybie tylko do odczytu przy użyciu programu SQL\*Plus:

    SHUTDOWN IMMEDIATE;
    STARTUP MOUNT;
    ALTER DATABASE OPEN READ ONLY;


##<a name="verify-the-physical-standby-database"></a>Sprawdź fizycznej wstrzymania bazy danych
W tej sekcji przedstawiono sposób Sprawdź konfigurację wysokiej dostępności jako administrator.

Otwiera SQL\*oraz okno wiersza polecenia i sprawdzanie archiwizowane ponowne dziennika na maszyny wirtualnej wstrzymania (KOMPUTER2):

    SQL> show parameters db_unique_name;

    NAME                                TYPE       VALUE
    ------------------------------------ ----------- ------------------------------
    db_unique_name                      string     TEST_STBY

    SQL> SELECT NAME FROM V$DATABASE

    SQL> SELECT SEQUENCE#, FIRST_TIME, NEXT_TIME, APPLIED FROM V$ARCHIVED_LOG ORDER BY SEQUENCE#;

    SEQUENCE# FIRST_TIM NEXT_TIM APPLIED
    ----------------  ---------------  --------------- ------------
    45                    23-FEB-14   23-FEB-14   YES
    45                    23-FEB-14   23-FEB-14   NO
    46                    23-FEB-14   23-FEB-14   NO
    46                    23-FEB-14   23-FEB-14   YES
    47                    23-FEB-14   23-FEB-14   NO
    47                    23-FEB-14   23-FEB-14   NO

Otwiera SQL * Plus logfiles okna i przełącznik wiersza polecenia na komputerze podstawowego (Komputer1):

    SQL> alter system switch logfile;
    System altered.

    SQL> archive log list
    Database log mode              Archive Mode
    Automatic archival             Enabled
    Archive destination            C:\OracleDatabase\archive
    Oldest online log sequence     69
    Next log sequence to archive   71
    Current log sequence           71

W dzienniku zarchiwizowane wykonaj ponownie na maszyny wirtualnej wstrzymania (KOMPUTER2):

    SQL> SELECT SEQUENCE#, FIRST_TIME, NEXT_TIME, APPLIED FROM V$ARCHIVED_LOG ORDER BY SEQUENCE#;

    SEQUENCE# FIRST_TIM NEXT_TIM APPLIED
    ----------------  ---------------  --------------- ------------
    45                    23-FEB-14   23-FEB-14   YES
    46                    23-FEB-14   23-FEB-14   YES
    47                    23-FEB-14   23-FEB-14   YES
    48                    23-FEB-14   23-FEB-14   YES

    49                    23-FEB-14   23-FEB-14   YES
    50                    23-FEB-14   23-FEB-14   IN-MEMORY

Sprawdź odstępu na maszyny wirtualnej wstrzymania (KOMPUTER2):

    SQL> SELECT * FROM V$ARCHIVE_GAP;
    no rows selected.

Inną metodę weryfikacji może być przełączanie awaryjne do wstrzymania bazy danych, a następnie sprawdzić, czy istnieje możliwość powrotu do podstawowej bazy danych. Aby aktywować wstrzymania bazy danych jako podstawowego bazy danych, należy zastosować następujące instrukcje:

    SQL> ALTER DATABASE RECOVER MANAGED STANDBY DATABASE FINISH;
    SQL> ALTER DATABASE ACTIVATE STANDBY DATABASE;

Jeśli nie włączono flashback na oryginalnym podstawowej bazy danych, zaleca się usunąć oryginalny podstawowy bazy danych, a następnie ponownie utworzyć jako wstrzymania bazy danych.

Firma Microsoft zaleca się włączenie flashback bazę danych, z podstawowych i wstrzymania baz danych. W trybie awaryjnym sytuacji podstawowej bazy danych można świeci powrót do czasu, zanim tym przełączeniu i szybko przekształcić wstrzymania bazą danych.

##<a name="additional-resources"></a>Dodatkowe zasoby
[Obrazy maszyn wirtualnych Oracle dla Azure](virtual-machines-windows-classic-oracle-images.md)
