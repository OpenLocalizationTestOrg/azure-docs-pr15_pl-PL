<properties 
    pageTitle="Porty inne niż 1433 bazy danych SQL | Microsoft Azure"
    description="Czasami połączeń klientów z ADO.NET wersji 12 bazy danych SQL Azure pominąć serwer proxy i pracować bezpośrednio na bazę danych. Porty inne niż 1433 stają się ważne."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor="" />


<tags 
    ms.service="sql-database" 
    ms.workload="drivers"
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/17/2016"
    ms.author="annemill"/>


# <a name="ports-beyond-1433-for-adonet-45-and-sql-database-v12"></a>Porty inne niż 1433 ADO.NET 4,5 i wersji 12 bazy danych SQL


W tym temacie opisano zmian, które powoduje wyświetlenie wersji 12 bazy danych SQL Azure zachowanie połączenia klientów, którzy używają 4,5 ADO.NET lub nowszym.


## <a name="v11-of-sql-database-port-1433"></a>V11 bazy danych SQL: portu 1433


Gdy program Klient ADO.NET 4,5 jest używany do łączenia i kwerendy z V11 bazy danych SQL, wewnętrznych sekwencji jest następująca:


1. Pozwala podjąć próbę nawiązywanie połączenia z bazą danych SQL ADO.NET.

2. ADO.NET korzysta z portu 1433 połączenie modułu pośredniczącym i pośredniczącym nawiązuje połączenie z bazą danych SQL.

3. Baza danych SQL wysyła swoją odpowiedź pośredniczącym, który przekazuje odpowiedź ADO.NET do portu 1433.


**Terminologia:** Mówiąc, że ADO.NET interakcji z bazą danych SQL przy użyciu *serwera proxy rozsyłanie*opisano poprzedniej sekwencji. Jeśli nie pośredniczącym dotyczyło, możemy mówią że użyto *bezpośrednią drogą* .


## <a name="v12-of-sql-database-outside-vs-inside"></a>Bazy danych SQL w wersji 12: poza w porównaniu z wewnątrz


Połączeń w wersji 12 firma Microsoft pytanie, czy program klient uruchamia *poza* lub *wewnątrz* krawędzi chmura Azure. Podsekcje opisano dwa typowe scenariusze.


#### <a name="outside-client-runs-on-your-desktop-computer"></a>*Zewnętrzne:* Klient jest uruchomiony na komputerze stacjonarnym


Port 1433 jest tylko portu, który musi być otwarty na komputerze stacjonarnym, obsługującym aplikację klienta bazy danych SQL.


#### <a name="inside-client-runs-on-azure"></a>*Wewnątrz:* Klient uruchamia Azure


Po uruchomieniu klienta wewnątrz krawędzi chmura Azure używa to, co może nazywamy *bezpośrednią drogą* współdziałanie z serwerem bazy danych SQL. Po nawiązaniu połączenia dalszych interakcji między klientem i bazą danych obejmują bez pośredniczącym serwera proxy.


Sekwencja jest w następujący sposób:


1. ADO.NET 4,5 (lub nowszy) inicjuje krótki interakcji z chmurą Azure i otrzymuje numer portu dynamicznie zidentyfikowanych.
 - Numer portu dynamicznie zidentyfikowanych znajduje się w zakresie 11000 11999 lub 14000 14999.

2. ADO.NET połączy na serwerze bazy danych SQL bezpośrednio z nie pośredniczącym między.

3. Kwerendy są wysyłane bezpośrednio do bazy danych, a wyniki są zwracane bezpośrednio do klienta.


Upewnij się, że portu, który 11000 11999 i 14000-14999 na komputerze klienckim Azure pozostają dostępne dla 4,5 ADO.NET klienta interakcji z wersji 12 bazy danych SQL.

- W szczególności porty w zakresie muszą być wolne od innych blokują ruchu wychodzącego.

- Na swojej Głosowa Azure, **Zapora systemu Windows z zabezpieczeniami zaawansowanymi** określa ustawienia portu.
 - [Interfejs użytkownika zapory](http://msdn.microsoft.com/library/cc646023.aspx) umożliwia dodawanie regułę, dla której określone protokołu **TCP** wraz z zakresu portów składnię **11000 11999**.


## <a name="version-clarifications"></a>Wyjaśnienia wersji


W tej sekcji opisano zagadnienia ujęte monikerów, które odwołują się do wersji produktu. Wyświetla listę również niektóre par wersji między produktami.


#### <a name="adonet"></a>ADO.NET


- ADO.NET 4.0 obsługuje protokół TDS 7.3, ale nie 7.4.
- ADO.NET 4,5 lub nowszy obsługuje protokół TDS 7.4.


#### <a name="sql-database-v11-and-v12"></a>V11 bazy danych SQL i w wersji 12


Połączenie klienta różnic V11 bazy danych SQL i w wersji 12 są wyróżnione w tym temacie.


*Uwaga:* Instrukcji Transact-SQL `SELECT @@version;` zwraca wartości, które zaczynają się od numeru, takie jak "11". lub "12" i te odpowiadają naszych nazwy wersji V11 i w wersji 12 bazy danych SQL.


## <a name="related-links"></a>Łącza pokrewne


- ADO.NET 4.6 został wydany 20 lipca 2015 r. Zawiadomienie o blogu od zespołu .NET jest dostępna [w tym miejscu](http://blogs.msdn.com/b/dotnet/archive/2015/07/20/announcing-net-framework-4-6.aspx).


- ADO.NET 4,5 został wydany 15 sierpnia 2012. Zawiadomienie o blogu od zespołu .NET jest dostępna [w tym miejscu](http://blogs.msdn.com/b/dotnet/archive/2012/08/15/announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code.aspx).
 - Wpis w blogu o ADO.NET 4.5.1 jest dostępna [w tym miejscu](http://blogs.msdn.com/b/dotnet/archive/2013/06/26/announcing-the-net-framework-4-5-1-preview.aspx).


- [Lista wersji TDS Protocol (protokół)](http://www.freetds.org/userguide/tdshistory.htm)


- [Omówienie tworzenia bazy danych SQL](sql-database-develop-overview.md)


- [Zapora bazy danych SQL Azure](sql-database-firewall-configure.md)


- [Jak: Konfigurowanie ustawień zapory na bazy danych SQL](sql-database-configure-firewall-settings.md)

