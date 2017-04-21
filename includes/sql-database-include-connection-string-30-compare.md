
<!--
includes/sql-database-include-connection-string-30-compare.md

Latest Freshness check:  2015-09-03 , GeneMi.

## Connection string
-->


### <a name="compare-the-connection-string"></a>Porównanie parametrów połączenia


W poniższej tabeli porównano parametry połączeń, które program C# należy połączyć się z serwerem SQL w lokalnym a bazy danych SQL Azure w chmurze. Różnice są pogrubione.


| Parametry połączenia dla<br/>Baza danych SQL Azure | Parametry połączenia dla<br/>Program Microsoft SQL Server |
| :-- | :-- |
| Serwer =**tcp:**{your_serverName_here}**. database.windows.net,1433**;<br/>Identyfikator użytkownika = {your_loginName_here}**@{your_serverName_here}**;<br/>Hasło = {your_password_here};<br/>**Baza danych = {your_databaseName_here};**<br/>**Limit czasu połączenia = 30**;<br/>**Szyfrowanie = True**;<br/>**TrustServerCertificate = False**; | Serwer = {your_serverName_here};<br/>Identyfikator użytkownika = {your_loginName_here};<br/>Hasło = {your_password_here}; |


**Bazy danych =** jest opcjonalne dla programu SQL Server, ale jest wymagane dla bazy danych SQL.


[Właściwości SqlConnectionStringBuilder ADO .NET](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder_properties.aspx) — w tym artykule omówiono wszystkie parametry szczegółowo.


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
