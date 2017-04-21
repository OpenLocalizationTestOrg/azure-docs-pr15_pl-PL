
<!--
includes/sql-database-include-connection-string-40-config.md

Latest Freshness check:  2015-09-04 , GeneMi.

## Connection string
-->


### <a name="example-config-file-for-connection-string-security"></a>Przykładowy plik konfiguracji zabezpieczeń parametry połączenia


Należy wspierać niesolidne umieszczenie parametry połączenia jako literałów w kodzie C#. Warto umieścić parametry połączenia w pliku konfiguracji. Można edytować ciągu bez konieczności ponownego kompilowania.

Załóżmy skompilowany program C# nosi nazwę **ConsoleApplication1.exe**i że ten .exe znajduje się w **bin\debug\* * katalogu.

W tym przykładzie większości części ciąg połączenia są przechowywane w pliku o nazwie dokładnie **ConsoleApplication1.exe.config**. Również muszą znajdować się w tym pliku konfiguracji **bin\debug\**.

W pliku XML następującego pliku konfiguracji zobaczysz o nazwie **ConnectionString4NoUserIDNoPassword**parametry połączenia. Kod C# wyszukuje tego ciągu.

Należy edytować rzeczywistą nazwy w symbolach zastępczych:

- {your_serverName_here}
- {your_databaseName_here}



        <?xml version="1.0" encoding="utf-8" ?>
        <configuration>
            <startup> 
                <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
            </startup>
        
            <connectionStrings>
                <clear />
                <add name="ConnectionString4NoUserIDNoPassword"
                providerName="System.Data.ProviderName"
        
                connectionString=
                "Server=tcp:{your_serverName_here}.database.windows.net,1433;
                Database={your_databaseName_here};
                Connection Timeout=30;
                Encrypt=True;
                TrustServerCertificate=False;" />
            </connectionStrings>
        </configuration>



Na tej ilustracji Wybraliśmy pominąć dwoma parametrami:

- Identyfikator użytkownika = {your_userName_here};
- Hasło = {your_password_here};


Możesz je uwzględnić, ale czasami warto mieć programu uzyskać te wartości z klawiatury przez użytkownika. To zależy.



<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
