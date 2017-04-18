<properties
   pageTitle="Nawiązywanie połączenia z magazynem danych Azure SQL | Microsoft Azure"
   description="Jak znaleźć serwera nazw i ciąg połączenia dla swojego do magazynu danych SQL Azure"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/26/2016"
   ms.author="sonyama;barbkess"/>

# <a name="connect-to-azure-sql-data-warehouse"></a>Nawiązywanie połączenia z magazynem danych Azure SQL

Ten artykuł ułatwia nawiązywaniu połączenia z usługą magazynu danych SQL po raz pierwszy.

## <a name="find-your-server-name"></a>Znajdowanie nazwy serwera

Pierwszy krok do łączenia się magazynu danych SQL jest znajomość jak znaleźć nazwy serwera.  Na przykład nazwa serwera w poniższym przykładzie jest sample.database.windows.net. Aby znaleźć nazwę serwera w pełni kwalifikowana:

1. Przejdź do [portalu Azure][].
2. Polecenie **bazy danych SQL** 
3. Kliknij bazę danych, którą chcesz się połączyć.
4. Znajdź pełną nazwę serwera.

    ![Pełną nazwę serwera][1]

## <a name="supported-drivers-and-connection-strings"></a>Obsługiwanych sterowników i ciągów połączenia

Magazyn danych SQL Azure obsługuje [ADO.NET][], [ODBC][], [PHP][]i [JDBC][]. Kliknij jeden z poprzedniego sterowników do najnowszej wersji i dokumentacji. Automatyczne generowanie parametry połączenia dla sterownika, którego używasz z portalu Azure, można kliknąć przycisk na **Pokazywanie parametry połączenia bazy danych** z poprzedniego przykładu.  Poniżej przedstawiono również kilka przykładów wygląda parametry połączenia dla każdego sterownika.

> [AZURE.NOTE] Rozważ ustawienie limitu czasu połączenia do 300 sekund do przyłączenia po krótko niedostępności.

### <a name="adonet-connection-string-example"></a>Przykład ciąg połączenia ADO.NET

```C#
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

### <a name="odbc-connection-string-example"></a>Przykład ciąg połączenia ODBC

```C#
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

### <a name="php-connection-string-example"></a>Przykład ciąg połączenia PHP

```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

### <a name="jdbc-connection-string-example"></a>Przykład ciąg połączenia JDBC

```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

## <a name="connection-settings"></a>Ustawienia połączenia

Program SQL Data Warehouse zestandaryzować niektórych ustawień podczas tworzenia obiektów i połączenia. Te ustawienia nie można zastąpić i obejmują:

| Ustawienie bazy danych       | Wartość                        |
| :--------------------- | :--------------------------- |
| [ANSI_NULLS][]         | WŁĄCZONE                           |
| [QUOTED_IDENTIFIERS][] | WŁĄCZONE                           |
| [FORMAT_DATY][]         | MDY                          |
| [DATEFIRST][]          | 7                            |

## <a name="next-steps"></a>Następne kroki

Aby połączyć i kwerendy z programu Visual Studio, zobacz [kwerendy przy użyciu programu Visual Studio][]. Aby uzyskać więcej informacji na temat opcji uwierzytelniania, zobacz [uwierzytelnianie do magazynu danych SQL Azure][].

<!--Articles-->
[Kwerenda z programu Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[Uwierzytelnianie do magazynu danych Azure SQL]: ./sql-data-warehouse-authentication.md

<!--MSDN references-->
[ADO.NET]: https://msdn.microsoft.com/library/e80y5yhx(v=vs.110).aspx
[ODBC]: https://msdn.microsoft.com/library/jj730314.aspx
[PHP]: https://msdn.microsoft.com/library/cc296172.aspx?f=255&MSPPError=-2147217396
[JDBC]: https://msdn.microsoft.com/library/mt484311(v=sql.110).aspx
[ANSI_NULLS]: https://msdn.microsoft.com/library/ms188048.aspx
[QUOTED_IDENTIFIERS]: https://msdn.microsoft.com/library/ms174393.aspx
[FORMAT_DATY]: https://msdn.microsoft.com/library/ms189491.aspx
[DATEFIRST]: https://msdn.microsoft.com/library/ms181598.aspx

<!--Other-->
[Azure portal]: https://portal.azure.com

<!--Image references-->
[1]: media/sql-data-warehouse-connect-overview/get-server-name.png


