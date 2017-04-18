<properties
    pageTitle="Konfigurowanie programu Oracle GoldenGate za pośrednictwem SMS | Microsoft Azure"
    description="Wyświetlanie kolejnych samouczek konfigurowania i wykonywania Oracle GoldenGate na maszyny wirtualne serwera systemu Windows Azure wysoka odzyskiwania dostępności i awarii."
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


#<a name="configuring-oracle-goldengate-for-azure"></a>Konfigurowanie programu Oracle GoldenGate Azure


Ten samouczek przedstawiono sposób konfiguracji Oracle GoldenGate w środowisku maszyn wirtualnych Azure wysokiej dostępności i awarii. Samouczek omówiono [dwukierunkowego replikacji](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_about_gg.htm) dla baz danych Oracle nie RAC i obu witrynach w przypadku aktywnych.

Oracle GoldenGate obsługuje dystrybucji danych i integracji danych. Umożliwia konfigurowanie dystrybucji danych i rozwiązania synchronizacji danych przy użyciu konfiguracji replikacji programu Oracle Oracle i rozwiązaniem jest elastyczne wysokiej dostępności. Oracle GoldenGate uzupełnia zabezpieczenia danych Oracle z możliwości replikacji umożliwiające uaktualnienia informacji na poziomie przedsiębiorstwa dystrybucyjnej i przestoje zero i migracji. Aby uzyskać szczegółowe informacje zobacz [Przy użyciu programu Oracle GoldenGate z straż danych programu Oracle](http://docs.oracle.com/cd/E11882_01/server.112/e17157/unplanned.htm).

Oracle GoldenGate zawiera następujące główne składniki: wyodrębnianie danych pompa, Replicat, ścieżek i wyodrębnianie plików, punktów kontrolnych, Menedżer i nakładek. Aby dwukierunkowego replikacji między dwiema witrynami, należy utworzyć i rozpocząć wszystkie składniki w obu witrynach. Aby uzyskać szczegółowe informacje na architekturze Oracle GoldenGate zobacz [Przewodnik po GoldenGate Oracle](http://docs.oracle.com/goldengate/1212/gg-winux/index.html).

Ten samouczek założono, że już wiedzy teoretyczna i praktycznego na wysoką dostępność bazy danych programu Oracle i odzyskiwanie pojęcia, a także [Oracle GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html). Aby uzyskać więcej informacji odwiedź [witrynę sieci web programu Oracle](http://www.oracle.com/technetwork/database/features/availability/index.html).

Ponadto samouczka przyjęto założenie, masz już wdrożone następujące wymagania wstępne:

- Już przejrzeniu sekcji wysokiej dostępności i zagadnienia odzyskiwania systemu w temacie [maszyn wirtualnych Oracle obrazy — różne zagadnienia](virtual-machines-windows-classic-oracle-considerations.md) . Należy zauważyć, że Azure obsługuje autonomicznego wystąpienia bazy danych programu Oracle, ale nie Oracle rzeczywistą aplikacji klastrów (Oracle RAC) obecnie.

- Pobraniu programu Oracle GoldenGate z witryny sieci web [Programu Oracle — pliki do pobrania](http://www.oracle.com/us/downloads/index.html) . Zaznaczona pośredniczącym fuzji Oracle Pack produktu — integracji danych. Następnie wybrany Oracle GoldenGate na v11.2.1 Oracle Media Pack systemu Microsoft Windows x64 (wersja 64-bitowa) dla bazy danych Oracle 11 g. Następnie Pobierz V11.2.1.0.3 GoldenGate Oracle dla programu Oracle 11g 64-bitowej wersji systemu Windows 2008 (64-bitowa).

- Utworzono dwa wirtualnych maszyn platformy Azure za pomocą programu Oracle Enterprise Edition w systemie Windows Server. Upewnij się, że maszyn wirtualnych są w tej [samej usługi w chmurze](virtual-machines-linux-load-balance.md) i w tej samej [Wirtualnej sieci](https://azure.microsoft.com/documentation/services/virtual-network/) , aby upewnić się, że mogą uzyskać dostęp siebie na stałe prywatny adres IP.

- Nazwy maszyn wirtualnych jako "MachineGG1", a witryny i "MachineGG2" dla witryny B ustawiono w portalu klasyczny Azure.

- Został utworzony test baz danych "TestGG1" na A witryny i "TestGG2" w witrynie B.

- Możesz zalogować się do swojego serwera systemu Windows jako członek grupy Administratorzy lub członkiem grupy **ORA_DBA** .

W tym samouczku dowiesz się:

1. Konfigurowanie bazy danych w witrynie A i B witryny  

    1. Wykonywanie obciążenia dane początkowe

2. Przygotowywanie witryny A i B witryny Replikacja bazy danych

3. Tworzenie wszystkich obiektów niezbędne do obsługi replikacji DDL

4. Konfigurowanie GoldenGate menedżera witryny A i B witryny

5. Tworzenie grupy wyodrębnianie i pompować danych procesy w witrynie A i B witryny

    1. Tworzenie procesów Wyodrębnij i pompy danych w witrynie A

    2. Tworzenie tabeli punkt kontrolny GoldenGate witrynie b

    3. Dodawanie REPLICAT w witrynie B

    4. Tworzenie procesów Wyodrębnij i pompy danych witrynie b

    5. Tworzenie tabeli punkt kontrolny GoldenGate w witrynie A

    6. Dodawanie REPLICAT w witrynie A

    7. Dodawanie trandata w witrynie A i B witryny

    8. Uruchamianie procesów Wyodrębnij i pompy danych w witrynie A

    9. Uruchomić proces wyodrębniania i pompy danych w witrynie b

    10. Uruchom proces REPLICAT w witrynie A

    11. Uruchom proces REPLICAT witrynie b

6. Sprawdź procesu replikacji dwukierunkowym

>[AZURE.IMPORTANT] Ten samouczek został konfiguracji i przetestowane pod kątem zgodności następującą konfigurację oprogramowania:
>
>|                        | **Witryny bazy danych**              | **Baza danych witryn B**              |
>|------------------------|----------------------------------|----------------------------------|
>| **Oracle w wersji**     | Oracle11g w wersji 2 — (11.2.0.1) | Oracle11g w wersji 2 — (11.2.0.1) |
>| **Nazwa komputera**       | MachineGG1                       | MachineGG2                       |
>| **System operacyjny**   | Windows 2008 R2                  | Windows 2008 R2                  |
>| **Oracle SID**         | TESTGG1                          | TESTGG2                          |
>| **Schemat replikacji** | SCOTTA                            | SCOTTA                            |

Dla kolejnych wersjach bazy danych programu Oracle i Oracle GoldenGate może być niektóre dodatkowe zmiany, które są potrzebne do wykonania. Najbardziej aktualną wersję informacje szczegółowe dokumentacji [Oracle GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html) i [Baza danych programu Oracle](http://www.oracle.com/us/corporate/features/database-12c/index.html) w witrynie sieci web programu Oracle. Na przykład, w bazie danych źródłowych wersji 11.2.0.4 lub nowszym Przechwytywanie DDL odbywa się na serwerze logmining asynchroniczne i wymaga wyzwalacze nie specjalny, tabel lub innych obiektów bazy danych musi być zainstalowany. Oracle GoldenGate uaktualnienia mogą być wykonywane bez zatrzymywania aplikacji użytkownika. Gdy Wyodrębnij znajduje się w trybie zintegrowanym z bazą danych źródłowych 11g Oracle są starsze niż wersja 11.2.0.4 wymagane jest używanie wyzwalacza DDL i obiekty pomocnicze. Aby uzyskać szczegółowe instrukcje zobacz [Instalowanie i konfigurowanie GoldenGate Oracle dla bazy danych programu Oracle](http://docs.oracle.com/goldengate/1212/gg-winux/GIORA.pdf).

##<a name="1-setup-database-on-site-a-and-site-b"></a>1. konfiguracji bazy danych w witrynie A i B witryny
W tej sekcji wyjaśniono, jak przeprowadzić wymagania wstępne dotyczące bazy danych zarówno na witryny A i B. witryny Należy wykonać wszystkie kroki w tej sekcji w obu witrynach: A i B. witryny

Pierwszy, zdalny pulpit, aby witryny A i B witryn za pośrednictwem portalu klasyczny Azure. Otwarcie wiersza polecenia systemu Windows i Utwórz katalogu macierzystego dla programu Oracle GoldenGate ustawienia plików:

    mkdir C:\OracleGG

Następnie Rozpakuj plik i zainstalować oprogramowanie Oracle GoldenGate w tym folderze. Po wykonaniu tego kroku można zainicjować interpretera polecenia oprogramowania GoldenGate (GGSCI), wykonując następujące polecenie:

    C:\OracleGG\.\ggsci

Za pomocą [GGSCI](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_gettingstarted.htm) , aby uruchomić kilka poleceń, które Konfigurowanie, kontroli i monitorowanie Oracle GoldenGate.

Następnie uruchom następujące polecenie, aby utworzyć wszystkie foldery podrzędne z pakietu instalacyjnego:

    GGSCI (Hostname) 1> CREATE SUBDIRS

Uruchom następujące polecenie, aby Zamknij okno wiersza polecenia GGSCI:

    GGSCI (Hostname) 1> EXIT

Następnie należy utworzyć użytkownika bazy danych, które będą używane przez procesy Menedżera GoldenGate Oracle, wyodrębnione i replikacji. Należy zauważyć, że można utworzyć poszczególnych użytkowników dla każdego procesu lub skonfigurować tylko jeden użytkownik typowych. W tym samouczku tworzymy jeden użytkownik, który jest nazywany jako ggate. Następnie udzielamy tego użytkownika odpowiednich uprawnień. Należy zauważyć, że należy wykonać następujące operacje w witrynie A i B. witryny

Otwiera SQL\*Plus okno poleceń w witrynie A i B witryny z uprawnieniami administratora bazy danych przy użyciu **grupy SYSDBA**, takich jak:

    Enter username: / as sysdba

I uruchom:

    SQL> create tablespace ggs_data   datafile 'c:\OracleDatabase\oradata\<DBNAME>\<DBNAME>ggs_data01.dbf' size 200m;
    SQL> create user ggate identified by ggate default tablespace ggs_data  temporary tablespace temp;
          grant connect, resource to ggate;
          grant select any dictionary, select any table to ggate;
          grant create table to ggate;
          grant flashback any table to ggate;
          grant execute on dbms_flashback to ggate;
          grant execute on utl_file to ggate;
          grant create any table to ggate;
          grant insert any table to ggate;
          grant update any table to ggate;
          grant delete any table to ggate;
          grant drop any table to ggate;

Następnie znajdź inicjowania\<DatabaseSID\>. ORA plików w folderze % ORACLE\_główne %\\bazy danych w folderze w witrynie A i B witryny i dołączyć następujące parametry bazy danych do INITTEST.ora:

    UNDO\_MANAGEMENT=AUTO
    UNDO\_RETENTION=86400

Aby uzyskać pełną listę wszystkich poleceń programu Oracle GoldenGate GGSCI zobacz [informacje dotyczące programu Oracle GoldenGate dla systemu Windows](http://docs.oracle.com/goldengate/1212/gg-winux/GWURF/ggsci_commands.htm).

### <a name="perform-initial-data-load"></a>Wykonywanie obciążenia dane początkowe

Załaduj dane początkowe można wykonywać w bazie danych, wykonując kilka metod. Na przykład umożliwia [Bezpośrednie obciążenia GoldenGate Oracle](http://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_initsync.htm) lub zwykła narzędzi eksportu i importu eksportowania danych tabeli z witryny A do witryny B.

Wykazać procesu replikacji Oracle GoldenGate, ten samouczek zaprezentowano, tworząc tabelę zarówno na witryny A i B witryny za pomocą następujących poleceń.

Najpierw otworzyć SQL\*oraz polecenie Okno i uruchom następujące polecenie, aby utworzyć tabelę zapasów w bazach danych witryny A i B witryny:

    create table scott.inventory
    (prod_id number,
    prod_category varchar2(20),
    qty_in_stock number,
    last_dml timestamp default systimestamp);

Następnie dodać ograniczenia do nowo utworzonej tabeli w witrynie A i B witryny baz danych:

    alter table scott.inventory add constraint pk_inventory primary key (prod_id) ;

Następnie udziel uprawnień wszystkie w tabeli zapasy do ggate użytkownika w witrynie A i b witryny

    grant all on scott.inventory to ggate;

Następnie należy utworzyć i włączyć wyzwalacz bazy danych, INVENTORY_CDR_TRG, na nowo utworzonej tabeli, aby upewnić się, że wszystkie transakcje do nowej tabeli są rejestrowane, jeśli użytkownik nie jest ggate. Wykonywanie tej operacji w witrynie A i B. witryny

    CREATE OR REPLACE TRIGGER INVENTORY_CDR_TRG
    BEFORE UPDATE
    ON SCOTT.INVENTORY
    REFERENCING NEW AS New OLD AS Old
    FOR EACH ROW
    BEGIN
    IF SYS_CONTEXT ('USERENV', 'SESSION_USER') != 'GGATE'
    THEN
    :NEW.LAST_DML := SYSTIMESTAMP;
    END IF;
    END;
    /


##<a name="2-prepare-site-a-and-site-b-for-database-replication"></a>2. przygotowywanie witryny A i B witryny Replikacja bazy danych
W tej sekcji wyjaśniono, jak przygotować witryny A i B witryny do Replikacja bazy danych. Należy wykonać wszystkie kroki w tej sekcji w obu witrynach: A i B. witryny

Pierwszy, zdalny pulpit, aby witryny A i B witryn za pośrednictwem portalu klasyczny Azure. Przełączanie bazy danych do trybu archivelog SQL * Plus okno polecenia:

    sql>shutdown immediate
    sql>startup mount
    sql>alter database archivelog;
    sql>alter database open;


Następnie należy włączyć minimalne [uzupełniających, w tym rejestrowanie](http://docs.oracle.com/cd/E11882_01/server.112/e22490/logminer.htm) w następujący sposób:

    SQL> ALTER DATABASE ADD SUPPLEMENTAL LOG DATA (ALL) COLUMNS;

Następnie przygotowywanie bazy danych do obsługi replikacji języka DDL (data definition language):

    SQL> alter system set recyclebin=off scope=spfile;

Następnie, zamknięciu i ponownym uruchomieniu bazy danych:

    sql>shutdown immediate
    sql>startup


##<a name="3-create-all-necessary-objects-to-support-ddl-replication"></a>3. wszystkie obiekty wymagane do obsługi replikacji DDL tworzenie
W tej sekcji przedstawiono skrypty, które mają zostać użyte do utworzenia wszystkich obiektów niezbędne do obsługi replikacji DDL. Potrzebne do uruchamiania skryptów określonych w tej sekcji zarówno na witryny A i B. witryny

Otwarcie wiersza polecenia systemu Windows i przejdź do folderu Oracle GoldenGate, takie jak C:\\OracleGG. Rozpoczynanie SQL\*Plus wiersz polecenia z uprawnieniami administratora bazy danych, takich jak przy użyciu **grupy SYSDBA** w witrynie A i B. witryny

Następnie uruchom następujące skrypty:

    SQL> @marker_setup.sql  
    Enter GoldenGate schema name: ggate
    SQL> @ddl_setup.sql  
    Enter GoldenGate schema name: ggate
    SQL> @role_setup.sql
    Enter GoldenGate schema name: ggate
    SQL> grant ggs_ggsuser_role to ggate;
     Grant succeeded.
     SQL> @ddl_enable
     Trigger altered.
     SQL> @ddl_pin ggate


Narzędzie GoldenGate Oracle wymaga logowania poziomu tabeli obsługę języka DDL (data definition language). Właśnie, Włącz rejestrowanie na poziomie tabeli przy użyciu polecenia TRANDATA Dodaj dodatkowe. Otwórz okno interpretera Oracle GoldenGate polecenia, zaloguj się do bazy danych, a następnie uruchom polecenie Dodaj TRANDATA:

    GGSCI 5> DBLOGIN USERID ggate, PASSWORD ggate

    GGSCI(Hostname) 6> add trandata scott.inventory

##<a name="4-configure-goldengate-manager-on-site-a-and-site-b"></a>4. Konfigurowanie GoldenGate menedżera witryny A i B witryny
Menedżer GoldenGate Oracle wykonuje wiele funkcji, takich jak uruchamianie innych procesów GoldenGate, zarządzanie plikami dzienników dziennik i raportowania.

Musisz skonfigurować proces Menedżera GoldenGate Oracle na witryny A i B. witryny W tym celu należy wykonać następujące czynności w witrynie A i B. witryny

Otwarte okna polecenia okna i zainicjować interpretera poleceń programu Oracle GoldenGate:

    cd C:\OracleGG\
    c:\OracleGG>ggsci
    Oracle GoldenGate Command Interpreter for Oracle
    Version 11.2.1.0.3 14400833 OGGCORE_11.2.1.0.3_PLATFORMS_120823.1258
    Windows x64 (optimized), Oracle 11g on Aug 23 2012 16:50:36
    Copyright (C) 1995, 2012, Oracle and/or its affiliates. All rights reserved.


Dzienniki sesji GGSCI do bazy danych, dzięki czemu będzie wykonanie polecenia, które mają wpływ na bazę danych:

    GGSCI (HostName) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

Wyświetl stan i zwłoki (w stosownych przypadkach) dla wszystkich procesów Menedżera, wyodrębnione i Replicat na komputerze:

    GGSCI (HostName) 2> info all
    Program     Status      Group       Lag           Time Since Chkpt
    MANAGER     STOPPED

Otwieranie pliku parametrów przy użyciu polecenia Edytuj parametry, a następnie dołączyć następujące informacje:

    GGSCI (HostName) 3> edit params mgr
    PORT 7809
    USERID ggate, PASSWORD ggate
    PURGEOLDEXTRACTS  C:\OracleGG\dirdat\ex, USECHECKPOINTS

Wyświetl stan i zwłoki (w stosownych przypadkach) dla wszystkich procesów Menedżera, wyodrębnione i Replicat na komputerze:

    GGSCI (HostName) 46> info all
    Program     Status      Group       Lag           Time Since Chkpt
    MANAGER     STOPPED

Dzienniki sesji GGSCI do bazy danych, dzięki czemu będzie wykonanie polecenia, które mają wpływ na bazę danych:

    GGSCI (HostName) 47> dblogin USERID ggate, PASSWORD ggate

    Successfully logged into database.

Rozpocznij proces menedżera:

    GGSCI (HostName) 48> start manager
    Manager started.

##<a name="5-create-extract-group-and-data-pump-processes-on-site-a-and-site-b"></a>5. Tworzenie Wyodrębnij procesów grupy i pompy danych w witrynie A i B witryny

###<a name="create-extract-and-data-pump-processes-on-site-a"></a>Tworzenie procesów Wyodrębnij i pompy danych w witrynie A

Należy utworzyć procesy wyodrębniania i pompy danych na witrynę i witryny B. pulpitu zdalnego w witrynie A i B witryn za pośrednictwem portalu klasyczny Azure. Otwiera okno interpretera poleceń GGSCI. Uruchom następujące polecenia w witrynie o:

    GGSCI (MachineGG1) 14> add extract ext1 tranlog begin now
    EXTRACT added.
    GGSCI (MachineGG1) 4> add exttrail C:\OracleGG\dirdat\lt, extract ext1
    EXTTRAIL added.
    GGSCI (MachineGG1) 16> add extract dpump1 exttrailsource C:\OracleGG\dirdat\aa
    EXTRACT added.
    GGSCI (MachineGG1) 17> add rmttrail C:\OracleGG\dirdat\ab extract dpump1
    RMTTRAIL added.

Otwieranie pliku parametrów przy użyciu polecenia Edytuj parametry, a następnie dołączyć następujące informacje: GGSCI (MachineGG1) 18 > Edytuj parametry ext1 WYODRĘBNIJ ext1 identyfikator użytkownika ggate hasło ggate EXTTRAIL C:\OracleGG\dirdat\aa TRANLOGOPTIONS EXCLUDEUSER ggate tabeli scott.inventory, GETBEFORECOLS (włączone aktualizacji KEYINCLUDING (prod_category, qty_in_stock, last_dml), na usuwanie KEYINCLUDING (prod_category, qty_in_stock, last_dml));

Otwieranie pliku parametrów przy użyciu polecenia Edytuj parametry, a następnie dołączyć następujące informacje:

    GGSCI (MachineGG1) 15> edit params dpump1
    EXTRACT dpump1
     USERID ggate, PASSWORD ggate
     RMTHOST ActiveGG2orcldb, MGRPORT 7809, TCPBUFSIZE 100000
     RMTTRAIL C:\OracleGG\dirdat\ab
     PASSTHRU
     TABLE scott.inventory;

###<a name="create-a-goldengate-checkpoint-table-on-site-b"></a>Tworzenie tabeli punkt kontrolny GoldenGate witrynie b

Następnie należy dodać tabelę punktu kontrolnego na witryny B. Aby to zrobić, musisz otworzyć okno interpretera poleceń GoldenGate i uruchamianie:

    C:\OracleGG\ggsci
    GGSCI (MachineGG2) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

A następnie, dodać punkt kontrolny tabeli w bazie danych, w którym ggate jest właścicielem:

    GGSCI (MachineGG2) 2> ADD CHECKPOINTTABLE ggate.checkpointtable
    Successfully created checkpoint table ggate.checkpointtable.

Dodaj nazwę tabeli punkt wyboru do pliku GLOBALS na serwerze docelowym, czyli witryny B w tym kroku. Edytowanie pliku GLOBALS na B: witryny

    GGSCI (MachineGG2) 1> EDIT PARAMS ./GLOBALS

Następnie dołączyć parametr CHECKPOINTTABLE do istniejącego pliku GLOBALS:

    GGSCHEMA ggate
    CHECKPOINTTABLE ggate.checkpointtable

Jako ostatni krok Zapisz i zamknij plik parametru GLOBALS.


###<a name="add-replicat-on-site-b"></a>Dodawanie REPLICAT w witrynie B
W tej sekcji opisano, jak dodać proces REPLICAT "REP2" w witrynie B.

Tworzenie grupy Replicat na B: witryny przy użyciu polecenia Dodaj REPLICAT

    GGSCI (MachineGG2) 37> add replicat rep2 exttrail C:\OracleGG\dirdatab, checkpointtable ggate.checkpointtable

Otwieranie pliku parametrów przy użyciu polecenia Edytuj parametry, a następnie dołączyć następujące informacje:

    GGSCI (MachineGG2) 10> edit params rep2
    REPLICAT rep2
    ASSUMETARGETDEFS
    USERID ggate, PASSWORD ggate
    DISCARDFILE C:\OracleGG\dirdat\discard.txt, append,megabytes 10
    MAP scott.inventory, TARGET scott.inventory;

###<a name="create-extract-and-data-pump-processes-on-site-b"></a>Tworzenie procesów Wyodrębnij i pompy danych witrynie b

W tej sekcji opisano sposób tworzenia nowego procesu Wyodrębnij "EXT2" i nowy proces pompy danych "DPUMP2" w witrynie B:

    GGSCI (MachineGG2) 3> add extract ext2 tranlog begin now
     EXTRACT added.
    GGSCI (MachineGG2) 4> add exttrail C:\OracleGG\dirdat\ac extract ext2
     EXTTRAIL added.
    GGSCI (MachineGG2) 5> add extract dpump2 exttrailsource C:\OracleGG\dirdat\ac
     EXTRACT added.
    GGSCI (MachineGG2) 6> add rmttrail C:\OracleGG\dirdat\ad extract dpump2
     RMTTRAIL added.

Otwieranie pliku parametrów przy użyciu polecenia Edytuj parametry, a następnie dołączyć następujące informacje:

    GGSCI (MachineGG2) 31> edit params ext2
    EXTRACT ext2
    USERID ggate, PASSWORD ggate
    EXTTRAIL C:\OracleGG\dirdat\ac
    TRANLOGOPTIONS EXCLUDEUSER ggate
    TABLE scott.inventory,
    GETBEFORECOLS (
    ON UPDATE KEYINCLUDING (prod_category,qty_in_stock, last_dml),
    ON DELETE KEYINCLUDING (prod_category,qty_in_stock, last_dml));

Otwieranie pliku parametrów przy użyciu polecenia Edytuj parametry, a następnie dołączyć następujące informacje:

    GGSCI (MachineGG2) 32> edit params dpump2
    EXTRACT dpump2
    USERID ggate, PASSWORD ggate
    RMTHOST MachineGG1, MGRPORT 7809, TCPBUFSIZE 100000
    RMTTRAIL C:\OracleGG\dirdat\ad
    PASSTHRU
    TABLE scott.inventory;

###<a name="create-a-goldengate-checkpoint-table-on-site-a"></a>Tworzenie tabeli punkt kontrolny GoldenGate w witrynie A

Otwórz okno interpretera polecenie Oracle GoldenGate i Utwórz tabelę punkt kontrolny:

    GGSCI (MachineGG1) 1> DBLOGIN USERID ggate, PASSWORD ggate
    Successfully logged into database.

    GGSCI (MachineGG1) 2> ADD CHECKPOINTTABLE ggate.checkpointtable

    Successfully created checkpoint table ggate.checkpointtable.

Należy dodać nazwę tabeli punkt wyboru pliku GLOBALS w witrynie odpowiedzi.

Otwarcie okna interpretera poleceń programu Oracle GoldenGate i edytować plik GLOBALS w witrynie o:

    GGSCI (MachineGG1) 1> EDIT PARAMS ./GLOBALS
    Add the CHECKPOINTTABLE parameter to the existing GLOBALS file as follows:
    GGSCHEMA ggate
    CHECKPOINTTABLE ggate.checkpointtable

Zapisz i zamknij plik parametru GLOBALS.

###<a name="add-replicat-on-site-a"></a>Dodawanie REPLICAT w witrynie A

W tej sekcji opisano, jak dodać proces REPLICAT "REP1" w witrynie odpowiedzi.

Następujące polecenie tworzy rep1 grupy Replicat o nazwie dziennik i skojarzone checkpointtable.

    GGSCI (MachineGG1) 21> add replicat rep1 exttrail C:\OracleGG\dirdat\ad,checkpointtable ggate.checkpointtable
     REPLICAT added.

Otwieranie pliku parametrów przy użyciu polecenia Edytuj parametry, a następnie dołączyć następujące informacje:

    GGSCI (MachineGG1) 10> edit params rep1
    REPLICAT rep1
    ASSUMETARGETDEFS
    USERID ggate, PASSWORD ggate
    DISCARDFILE C:\OracleGG\dirdat\discard.txt, append, megabytes 10
    MAP scott.inventory, TARGET scott.inventory;

### <a name="add-trandata-on-site-a-and-site-b"></a>Dodawanie trandata w witrynie A i B witryny

Włącz rejestrowanie uzupełniających, w tym na poziomie tabeli przy użyciu polecenia Dodaj TRANDATA. Otwórz okno interpretera Oracle GoldenGate polecenia, zaloguj się do bazy danych, a następnie uruchom polecenie TRANDATA Dodaj.

Pulpit zdalny do MachineGG1, otwarcia interpretera poleceń Oracle GoldenGate i uruchom:

    GGSCI (MachineGG1) 11> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG1) 12> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    GGSCI (MachineGG1) 13> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

Pulpit zdalny do MachineGG2, otwarcia interpretera poleceń Oracle GoldenGate i uruchom:

    GGSCI (MachineGG2) 18> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG2) 14> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    Logging of supplemental redo data enabled for table SCOTT.INVENTORY.

Wyświetl informacje o stanie dodatkowe tabeli poziom rejestrowania:

    GGSCI (MachineGG2) 15> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

###<a name="add-trandata-on-site-a-and-site-b"></a>Dodawanie trandata w witrynie A i B witryny

Włącz rejestrowanie uzupełniających, w tym na poziomie tabeli przy użyciu polecenia Dodaj TRANDATA. Otwórz okno interpretera Oracle GoldenGate polecenia, zaloguj się do bazy danych, a następnie uruchom polecenie TRANDATA Dodaj.

Pulpit zdalny do MachineGG1, otwarcia interpretera poleceń Oracle GoldenGate i uruchom:

    GGSCI (MachineGG1) 11> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG1) 12> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    GGSCI (MachineGG1) 13> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

Pulpit zdalny do MachineGG2, otwarcia interpretera poleceń Oracle GoldenGate i uruchom:

    GGSCI (MachineGG2) 18> dblogin userid ggate password ggate
     Successfully logged into database.
    GGSCI (MachineGG2) 14> add trandata scott.inventory cols (prod_category,qty_in_stock, last_dml)
    Logging of supplemental redo data enabled for table SCOTT.INVENTORY.
    Display information about the state of table-level supplemental logging:
    GGSCI (MachineGG2) 15> info trandata scott.inventory
    Logging of supplemental redo log data is enabled for table SCOTT.INVENTORY.
    Columns supplementally logged for table SCOTT.INVENTORY: PROD_ID, PROD_CATEGORY, QTY_IN_STOCK, LAST_DML.

###<a name="start-extract-and-data-pump-processes-on-site-a"></a>Uruchamianie procesów Wyodrębnij i pompy danych w witrynie A

Rozpoczynanie ext1 proces wyodrębnione na o: witryny

    GGSCI (MachineGG1) 31> start extract ext1
    Sending START request to MANAGER …
    EXTRACT EXT1 starting

Rozpoczynanie dpump1 proces pompy danych na o: witryny

    GGSCI (MachineGG1) 23> start extract dpump1
    Sending START request to MANAGER …
    EXTRACT DPUMP1 starting
Wyświetlania informacji na temat ext1 grupy Wyodrębnij: GGSCI (MachineGG1) 32 > informacje wyodrębnić ext1 WYODRĘBNIĆ EXT1 ostatniego pracę 2013-11-25 08:03 stan uruchomiony punkt kontrolny zwłoki 00:00:00 (zaktualizowanych 00:00:02 temu) 08 ponowne dzienniki 2013-11-25 dziennika odczytu punkt kontrolny Oracle: 03:18 Seqno 6, RBA 3230720 SCN 0.1074371 (1074371)

Wyświetl stan i zwłoki (w stosownych przypadkach) dla wszystkich procesów Menedżera, wyodrębnione i Replicat na komputerze:

    GGSCI (MachineGG1) 16> info all
    Program     Status      Group       Lag at Chkpt  Time Since Chkpt

    MANAGER     RUNNING
    EXTRACT     RUNNING     DPUMP1      00:00:00      00:46:33
    EXTRACT     RUNNING     EXT1        00:00:00      00:00:04

###<a name="start-extract-and-data-pump-processes-on-site-b"></a>Uruchomić proces wyodrębniania i pompy danych w witrynie b

Rozpoczynanie ext2 proces wyodrębnione na B: witryny

    GGSCI (MachineGG2) 22> start extract ext2
    Sending START request to MANAGER …
    EXTRACT EXT2 starting

Rozpoczynanie dpump2 proces pompy danych na witrynę B:

    GGSCI (MachineGG2) 23> start extract dpump2
    Sending START request to MANAGER …
    EXTRACT DPUMP2 starting

Wyświetl stan i zwłoki (w stosownych przypadkach) dla wszystkich procesów Menedżera, wyodrębnione i Replicat na komputerze:

    GGSCI (ActiveGG2orcldb) 6> info all
    Program     Status      Group       Lag at Chkpt  Time Since Chkpt

    MANAGER     RUNNING
    EXTRACT     RUNNING     DPUMP2      00:00:00      136:13:33
    EXTRACT     RUNNING     EXT2        00:00:00      00:00:04

### <a name="start-replicat-process-on-site-a"></a>Uruchom proces REPLICAT w witrynie A

W tej sekcji opisano, jak rozpocząć proces REPLICAT "REP1" w witrynie odpowiedzi.

Uruchom proces Replicat w witrynie o:

    GGSCI (MachineGG1) 38> start replicat rep1
    Sending START request to MANAGER …
    REPLICAT REP1 starting

Wyświetlanie stanu grupy Replicat:

    GGSCI (MachineGG1) 39> status replicat rep1
     REPLICAT REP1: RUNNING

###<a name="start-replicat-process-on-site-b"></a>Uruchom proces REPLICAT witrynie b

W tej sekcji opisano, jak rozpocząć proces REPLICAT "REP2" w witrynie B.

Uruchom proces Replicat na B: witryny

    GGSCI (MachineGG2) 26> start replicat rep2
    Sending START request to MANAGER …
    REPLICAT REP2 starting

Wyświetlanie stanu grupy Replicat:

    GGSCI (MachineGG2) 27> status replicat rep2
    REPLICAT REP2: RUNNING

##<a name="6-verify-the-bi-directional-replication-process"></a>6. Sprawdź procesu replikacji dwukierunkowym

Aby sprawdzić konfigurację Oracle GoldenGate, należy wstawić wiersz do bazy danych w witrynie A. pulpitu zdalnego witryny A. otwiera SQL * oraz okno polecenia i uruchom: SQL > Wybierz nazwę z bazy danych$ v;

    NAME
    ———
    TESTGG

    SQL> insert into inventory values  (100,’TV’,100,sysdate);

    1 row created.

    SQL> commit;

    Commit complete.

Następnie sprawdź, jeśli ten wiersz replikowane dla witryny B. W tym celu należy pulpitu zdalnego witryny B. Otwórz okno SQL Plus w górę i uruchom:

    SQL> select name from v$database;

    NAME
    ———
    TESTGG

    SQL> select * from inventory;

    PROD_ID PROD_CATEGORY QTY_IN_STOCK LAST_DML
    ———- ——————– ———— ———
    100 TV 100 22-MAR-13

Wstawianie nowego rekordu w witrynie B:

    SQL> insert into inventory  values  (101,’DVD’,10,sysdate);
    1 row created.

    SQL> commit;

    Commit complete.

Pulpit zdalny witryna i sprawdzić, czy replikacji miało miejsce:

    SQL> select * from inventory;

    PROD_ID PROD_CATEGORY QTY_IN_STOCK LAST_DML
    ———- ——————– ———— ———
    100 TV 100 22-MAR-13
    101 DVD 10 22-MAR-13

##<a name="additional-resources"></a>Dodatkowe zasoby
[Obrazy maszyn wirtualnych Oracle dla Azure](virtual-machines-linux-classic-oracle-images.md)
