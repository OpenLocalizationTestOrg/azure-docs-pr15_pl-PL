<properties
    pageTitle="Nawiązywanie połączenia z bazą danych SQL przy użyciu Python | Microsoft Azure"
    description="Przedstawia Python przykładowy kod, który umożliwia nawiązywanie połączenia z bazą danych SQL Azure."
    services="sql-database"
    documentationCenter=""
    authors="meet-bhagdev"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="meetb"/>


# <a name="connect-to-sql-database-by-using-python"></a>Nawiązywanie połączenia z bazą danych SQL przy użyciu Python


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 


W tym temacie przedstawiono sposób łączenia i kwerendy bazy danych SQL Azure za pomocą Python. W tym przykładzie można uruchamiać z platformy systemu Windows, Ubuntu Linux lub Mac.


## <a name="step-1-create-a-sql-database"></a>Krok 1: Tworzenie bazy danych SQL

Zobacz [Wprowadzenie do strony](sql-database-get-started.md) informacje o sposobie tworzenia przykładowej bazy danych.  Należy pamiętać, że tworzenie **szablonu bazy danych AdventureWorks**prowadnicy. Przykłady pokazano poniżej tylko Praca ze **schematem AdventureWorks**. Po utworzeniu wprowadzonych zmianach bazy danych czy włączeniu dostępu do swojego adresu IP, włączając reguły zapory, zgodnie z opisem w [Strona wprowadzenie](sql-database-get-started.md)

## <a name="step-2-configure-development-environment"></a>Krok 2: Skonfiguruj środowisko projektowania

### <a name="mac-os"></a>**Mac OS**   
### <a name="install-the-required-modules"></a>Instalowanie wymaganych modułów
Otwieranie usługi terminalowe i instalowanie

    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    brew install FreeTDS
    sudo -H pip install pymssql=2.1.1

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**

Otwórz usługi terminalowe i przejdź do katalogu, gdzie planujesz tworzenie usługi python skrypt. Wpisz następujące polecenia, aby zainstalować **FreeTDS** i **pymssql**. pymssql używa FreeTDS, aby nawiązać połączenie bazy danych programu SQL.

    sudo apt-get --assume-yes update
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    sudo apt-get --assume-yes install python-dev python-pip
    sudo pip install pymssql=2.1.1
    
### <a name="windows"></a>**Systemu Windows**

Zainstaluj pymssql [**tutaj**](http://www.lfd.uci.edu/~gohlke/pythonlibs/#pymssql). 

Upewnij się, że została wybrana poprawne whl pliku. Na przykład: Jeśli korzystasz z Python 2.7 na komputerze z 64-bitowej wersji wybierz: pymssql‑2.1.1‑cp27‑none‑win_amd64.whl. Po pobraniu pliku .whl umieszczanie go w folderze C:/Python27.

Zainstaluj teraz sterownika pymssql przy użyciu pip z wiersza polecenia. dysk CD w C:/Python27 i uruchom poniższe czynności
    
    pip install pymssql‑2.1.1‑cp27‑none‑win_amd64.whl

Instrukcje umożliwiające pip Użyj można znaleźć [w tym miejscu](http://stackoverflow.com/questions/4750806/how-to-install-pip-on-windows)

## <a name="step-3-run-sample-code"></a>Krok 3: Uruchamiać kod przykładowy

Tworzenie pliku o nazwie **sql_sample.py** i wklej następujący kod w nim. Używanie wiersza polecenia można uruchomić to:
    
    python sql_sample.py

### <a name="connect-to-your-sql-database"></a>Nawiązywanie połączenia z bazą danych SQL

Funkcja [pymssql.connect](http://pymssql.org/en/latest/ref/pymssql.html) umożliwia nawiązywanie połączenia z bazą danych SQL.

    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')


### <a name="execute-an-sql-select-statement"></a>Wykonywanie instrukcji SELECT

Funkcja [cursor.execute](http://pymssql.org/en/latest/ref/pymssql.html#pymssql.Cursor.execute) może służyć do pobierania zestawu wyników, w kwerendzie w bazie danych SQL. Ta funkcja zasadniczo akceptuje jakiejkolwiek kwerendy i zwraca zestaw wyników, które można powtórzyć nad przy użyciu [cursor.fetchone()](http://pymssql.org/en/latest/ref/pymssql.html#pymssql.Cursor.fetchone).


    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute('SELECT c.CustomerID, c.CompanyName,COUNT(soh.SalesOrderID) AS OrderCount FROM SalesLT.Customer AS c LEFT OUTER JOIN SalesLT.SalesOrderHeader AS soh ON c.CustomerID = soh.CustomerID GROUP BY c.CustomerID, c.CompanyName ORDER BY OrderCount DESC;')
    row = cursor.fetchone()
    while row:
        print str(row[0]) + " " + str(row[1]) + " " + str(row[2])   
        row = cursor.fetchone()


### <a name="insert-a-row-pass-parameters-and-retrieve-the-generated-primary-key"></a>Wstawienie wiersza, przekazać parametry i pobrać wygenerowane klucza podstawowego

W bazie danych SQL właściwości [tożsamości](https://msdn.microsoft.com/library/ms186775.aspx) i obiekt [SEKWENCJI](https://msdn.microsoft.com/library/ff878058.aspx) umożliwia automatyczne generowanie wartości [klucza podstawowego](https://msdn.microsoft.com/library/ms179610.aspx) . 


    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('SQL Server Express', 'SQLEXPRESS', 0, 0, CURRENT_TIMESTAMP)")
    row = cursor.fetchone()
    while row:
        print "Inserted Product ID : " +str(row[0])
        row = cursor.fetchone()


### <a name="transactions"></a>Transakcje


W tym przykładzie kodu zaprezentowano stosowania transakcje, w którym możesz:

* Rozpoczynanie transakcji
* Wstawienie wiersza danych
* Wycofywanie transakcji cofnąć Wstaw 

Wklej następujący kod wewnątrz sql_sample.py.
    
    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute("BEGIN TRANSACTION")
    cursor.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('SQL Server Express New', 'SQLEXPRESS New', 0, 0, CURRENT_TIMESTAMP)")
    cnxn.rollback()

## <a name="next-steps"></a>Następne kroki

* Przejrzyj [Omówienie tworzenia bazy danych SQL](sql-database-develop-overview.md)
* Więcej informacji na temat [Sterownik Python firmy Microsoft dla programu SQL Server](https://msdn.microsoft.com/library/mt652092.aspx)
* Odwiedź witrynę [Centrum deweloperów Python](/develop/python/).

## <a name="additional-resources"></a>Dodatkowe zasoby 

* [Desenie projektu dla wielu dzierżawy władz akredytacji bezpieczeństwa aplikacji z bazy danych Azure SQL](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Eksplorowanie wszystkie [Funkcje bazy danych SQL](https://azure.microsoft.com/services/sql-database/)
