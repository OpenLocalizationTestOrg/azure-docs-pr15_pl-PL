<properties
   pageTitle="Azure wykazu danych obsługiwanych źródeł danych | Microsoft Azure"
   description="Specyfikacja obecnie obsługiwanych źródeł danych."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="jstrauss"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/15/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-supported-data-sources"></a>Azure wykazu danych obsługiwanych źródeł danych

Umożliwia użytkownikom wykazu danych Azure Publikowanie metadanych za pomocą publicznej API, kliknij pozycję-raz rejestracja narzędzie lub ręcznie wprowadzając informacje bezpośrednio do wykazu danych sieci web portalu. Poniższa siatka zawiera podsumowanie wszystkich źródeł obsługiwane przez wykaz dzisiaj, a funkcje publikowania dla każdej z nich.  Na liście również są narzędzia danych zewnętrznych, których poszczególnych źródeł można uruchamiać z naszych obsługi portalu "Otwórz w". Druga siatki w artykule ma bardziej techniczny Specyfikacja każdej właściwości połączenia źródła danych.


## <a name="list-of-supported-data-sources"></a>Lista obsługiwanych źródeł danych

<table>

    <tr>
       <td><b>Obiekt źródła danych</b></td>
       <td><b>INTERFEJS API</b></td>
       <td><b>Ręczne wprowadzenie</b></td>
       <td><b>Narzędzie do rejestracji</b></td>
       <td><b>Otwórz na karcie Narzędzia</b></td>
       <td><b>Notatki</b></td>
    </tr>

    <tr>
      <td>Katalog magazynu Lake danych Azure</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Plik magazynu Lake danych Azure</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Azure magazyn obiektów Blob</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Katalog Azure</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Tabela Azure miejsca do magazynowania</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>
        <font size="2"></font>
      </td>
      <td>
        <font size="2"></font>
      </td>
    </tr>

    <tr>
      <td>Katalog HDFS</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Plik HDFS</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Tabelę programu hive</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Program Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Widok gałęzi</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Program Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Tabela MySQL</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Widok MySQL</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Tabela bazy danych Oracle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Widok bazy danych Oracle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Inne (Ogólne zasobów)</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Tabela magazynu danych SQL</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Widok magazynu danych SQL</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis usług wymiarów</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services kluczowego wskaźnika wydajności</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services pomiarowe</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Tabela programu SQL Server Analysis Services</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Raport usług Reporting Services programu SQL Server</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Przeglądarka</font></td>
      <td><font size=2>Tylko serwery w trybie macierzystym. Tryb programu SharePoint nie jest obsługiwane.</font></td>
    </tr>

    <tr>
      <td>Tabela programu SQL Server</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Widok programu SQL Server</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Tabela programu Teradata</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Program Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Widok programu Teradata</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Program Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Widok Hana SAP</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>PowerBI</font></td>
      <td><font size=2>Widoki obliczeń i widoków analitycznych. Atrybut widoki nie są obsługiwane.</font></td>
    </tr>

    <tr>
      <td>Tabela Db2</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Widok Db2</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Pliku</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Katalog FTP</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>FTP, plik</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Raport http</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Punkt końcowy http</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Plik protokołu HTTP</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Zestaw jednostek OData</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Funkcja OData</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Tabela Postgresql</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Widok Postgresql</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Widok Hana SAP</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td> Obiekt usług SalesForce</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Listy programu SharePoint </td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

</table>

Jeśli potrzebujesz pomocy dotyczącej dodatkowe źródła, Prześlij żądanie funkcja, za pomocą [wykazu danych Azure forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).


<br>
<br>
## <a name="data-source-reference-specification"></a>Specyfikacja odwołania źródła danych
> [AZURE.NOTE] Kolumna "Struktura DSL" w poniższej tabeli tylko list właściwości połączenia zbiór właściwości "adres" używane przez wykaz danych Azure (to znaczy "adres" grupa właściwości może zawierać inne właściwości połączenia źródła danych, który wykazu danych Azure będzie nadal występował, ale nie korzysta z).
<table>
    <tr>
       <td><b>Typ źródła</b></td>
       <td><b>Typ zawartości</b></td>
       <td><b>Typy obiektów</b></td>
       <td><b>Struktura DSL<b></td>
    </tr>
    <tr>
      <td>Magazyn Lake danych Azure</td>
      <td>Kontener</td>
      <td>Lake danych</td>
      <td>
        <font size=2>Protocol (protokół): webhdfs <br>uwierzytelnianie: {podstawowe, oauth} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL</font>
      </td>
    </tr>
    <tr>
      <td>Magazyn Lake danych Azure</td>
      <td>Tabela</td>
      <td>Katalog, plik</td>
      <td>
        <font size=2>Protocol (protokół): webhdfs <br>uwierzytelnianie: {podstawowe, oauth} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL</font>
      </td>
    </tr>
    <tr>
      <td>Azure miejsca do magazynowania</td>
      <td>Kontener</td>
      <td>Kontener</td>
      <td>
        <font size=2>Protocol (protokół): obiektów blob platformy azure <br>uwierzytelnianie: {azure klawisza dostępu} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domeny <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;konta <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kontener</font>
      </td>
    </tr>
    <tr>
      <td>Azure miejsca do magazynowania</td>
      <td>Tabela</td>
      <td>BLOB i katalogu</td>
      <td>
        <font size=2>Protocol (protokół): obiektów blob platformy azure <br>uwierzytelnianie: {azure klawisza dostępu} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domeny <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;konta <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kontener <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Nazwa</font>
      </td>
    </tr>
    <tr>
      <td>Azure miejsca do magazynowania</td>
      <td>Kontener</td>
      <td>Kontener</td>
      <td>
        <font size=2>Protocol (protokół): tabel platformy azure <br>uwierzytelnianie: {azure klawisza dostępu} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domeny <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;konta</font>
      </td>
    </tr>
    <tr>
      <td>Azure miejsca do magazynowania</td>
      <td>Tabela</td>
      <td>Tabela</td>
      <td>
        <font size=2>Protocol (protokół): tabel platformy azure <br>uwierzytelnianie: {azure klawisza dostępu} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domeny <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;konta <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Nazwa</font>
      </td>
    </tr>
    <tr>
      <td>Cosmos</td>
      <td>Kontener</td>
      <td>Klaster wirtualny</td>
      <td>
        <font size=2>Protocol (protokół): cosmos <br>uwierzytelnianie: {podstawowe, windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL</font>
      </td>
    </tr>
    <tr>
      <td>Cosmos</td>
      <td>Tabela</td>
      <td>Strumień, ustaw strumienia widok</td>
      <td>
        <font size=2>Protocol (protokół): cosmos <br>uwierzytelnianie: {podstawowe, windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL</font>
      </td>
    </tr>
    <tr>
      <td>DataZen</td>
      <td>Kontener</td>
      <td>Witryny</td>
      <td>
        <font size=2>Protocol (protokół): http <br>uwierzytelnianie: {Brak, basic, windows, oauth} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL</font>
      </td>
    </tr>
    <tr>
      <td>DataZen</td>
      <td>Raport</td>
      <td>Raport pulpitu nawigacyjnego</td>
      <td>
        <font size=2>Protocol (protokół): http <br>uwierzytelnianie: {Brak, basic, windows, oauth} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL</font>
      </td>
    </tr>
    <tr>
      <td>Db2</td>
      <td>Kontener</td>
      <td>Bazy danych</td>
      <td>
        <font size=2>Protocol (protokół): db2 <br>uwierzytelnianie: {podstawowe, windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych</font>
      </td>
    </tr>
    <tr>
      <td>Db2</td>
      <td>Tabela</td>
      <td>Widoku tabeli</td>
      <td>
        <font size=2>Protocol (protokół): db2 <br>uwierzytelnianie: {podstawowe, windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schematu</font>
      </td>
    </tr>
    <tr>
      <td>System plików</td>
      <td>Tabela</td>
      <td>Plik</td>
      <td>
        <font size=2>Protocol (protokół): plik <br>uwierzytelnianie: {Brak, podstawowe, windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Ścieżka</font>
      </td>
    </tr>
    <tr>
      <td>FTP</td>
      <td>Tabela</td>
      <td>Katalog, plik</td>
      <td>
        <font size=2>Protocol (protokół): ftp <br>uwierzytelnianie: {Brak, podstawowe, windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL</font>
      </td>
    </tr>
    <tr>
      <td>Hadoop Distributed File System</td>
      <td>Kontener</td>
      <td>Klaster</td>
      <td>
        <font size=2>Protocol (protokół): webhdfs <br>uwierzytelnianie: {podstawowe, oauth} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL</font>
      </td>
    </tr>
    <tr>
      <td>Hadoop Distributed File System</td>
      <td>Tabela</td>
      <td>Katalog, plik</td>
      <td>
        <font size=2>Protocol (protokół): webhdfs <br>uwierzytelnianie: {podstawowe, oauth} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL</font>
      </td>
    </tr>
    <tr>
      <td>Gałąź</td>
      <td>Kontener</td>
      <td>Bazy danych</td>
      <td>
        <font size=2>Protocol (protokół): gałęzi <br>uwierzytelnianie: {hdinsight, podstawowe, nazwa użytkownika, Brak} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>connectionProperties: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serverProtocol: {hive2}</font>
      </td>
    </tr>
    <tr>
      <td>Gałąź</td>
      <td>Tabela</td>
      <td>Widoku tabeli</td>
      <td>
        <font size=2>Protocol (protokół): gałęzi <br>uwierzytelnianie: {hdinsight, podstawowe, nazwa użytkownika, Brak} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt <br>connectionProperties: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serverProtocol: {hive2}</font>
      </td>
    </tr>
    <tr>
      <td>Http</td>
      <td>Kontener</td>
      <td>Witryny</td>
      <td>
        <font size=2>Protocol (protokół): http <br>uwierzytelnianie: {Brak, basic, windows, oauth} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL</font>
      </td>
    </tr>
    <tr>
      <td>Http</td>
      <td>Raport</td>
      <td>Raport pulpitu nawigacyjnego</td>
      <td>
        <font size=2>Protocol (protokół): http <br>uwierzytelnianie: {Brak, basic, windows, oauth} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL</font>
      </td>
    </tr>
    <tr>
      <td>Http</td>
      <td>Tabela</td>
      <td>Punkt końcowy, plik</td>
      <td>
        <font size=2>Protocol (protokół): http <br>uwierzytelnianie: {Brak, basic, windows, oauth} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL</font>
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>Kontener</td>
      <td>Bazy danych</td>
      <td>
        <font size=2>Protocol (protokół): mysql <br>uwierzytelnianie: {Protocol (protokół), windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych</font>
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>Tabela</td>
      <td>Widoku tabeli</td>
      <td>
        <font size=2>Protocol (protokół): mysql <br>uwierzytelnianie: {Protocol (protokół), windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt</font>
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Kontener</td>
      <td>Kontener jednostki</td>
      <td>
        <font size=2>Protokół: odata <br>uwierzytelnianie: {Brak, podstawowe, windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL</font>
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Tabela</td>
      <td>Zestaw jednostek, funkcja</td>
      <td>
        <font size=2>Protokół: odata <br>uwierzytelnianie: {Brak, podstawowe, windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;zasób</font>
      </td>
    </tr>
    <tr>
      <td>Baza danych Oracle</td>
      <td>Kontener</td>
      <td>Bazy danych</td>
      <td>
        <font size=2>Protocol (protokół): oracle <br>uwierzytelnianie: {Protocol (protokół), windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych</font>
      </td>
    </tr>
    <tr>
      <td>Baza danych Oracle</td>
      <td>Tabela</td>
      <td>Widoku tabeli</td>
      <td>
        <font size=2>Protocol (protokół): oracle <br>uwierzytelnianie: {Protocol (protokół), windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schematu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt</font>
      </td>
    </tr>
    <tr>
      <td>Postgresql</td>
      <td>Kontener</td>
      <td>Bazy danych</td>
      <td>
        <font size=2>Protocol (protokół): postgresql <br>uwierzytelnianie: {podstawowe, windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych</font>
      </td>
    </tr>
    <tr>
      <td>Postgresql</td>
      <td>Tabela</td>
      <td>Widoku tabeli</td>
      <td>
        <font size=2>Protocol (protokół): postgresql <br>uwierzytelnianie: {podstawowe, windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schematu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt</font>
      </td>
    </tr>
    <tr>
      <td>Power BI</td>
      <td>Kontener</td>
      <td>Witryny</td>
      <td>
        <font size=2>Protocol (protokół): http <br>uwierzytelnianie: {Brak, basic, windows, oauth} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL</font>
      </td>
    </tr>
    <tr>
      <td>Power BI</td>
      <td>Raport</td>
      <td>Raport pulpitu nawigacyjnego</td>
      <td>
        <font size=2>Protocol (protokół): http <br>uwierzytelnianie: {Brak, basic, windows, oauth} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL</font>
      </td>
    </tr>
    <tr>
      <td>Dodatek Power Query</td>
      <td>Tabela</td>
      <td>Rozwiązania połączonego danych</td>
      <td>
        <font size=2>Protocol (protokół): dodatek power query <br>uwierzytelnianie: {oauth} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL</font>
      </td>
    </tr>
    <tr>
      <td>Usługi SalesForce</td>
      <td>Tabela</td>
      <td>Obiekt</td>
      <td>
        <font size=2>Protocol (protokół): com usług salesforce <br>uwierzytelnianie: {podstawowe, windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;loginServer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;klasy <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Nazwa elementu</font>
      </td>
    </tr>
    <tr>
      <td>SAP Hana</td>
      <td>Kontener</td>
      <td>Serwer</td>
      <td>
        <font size=2>Protocol (protokół): sap hana sql <br>uwierzytelnianie: {Protocol (protokół), windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer</font>
      </td>
    </tr>
    <tr>
      <td>SAP Hana</td>
      <td>Tabela</td>
      <td>Widok</td>
      <td>
        <font size=2>Protocol (protokół): sap hana sql <br>uwierzytelnianie: {Protocol (protokół), windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schematu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt</font>
      </td>
    </tr>
    <tr>
      <td>Programu SharePoint</td>
      <td>Tabela</td>
      <td>Lista</td>
      <td>
        <font size=2>Protocol (protokół): listy programu sharepoint <br>uwierzytelnianie: {podstawowe, windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL</font>
      </td>
    </tr>
    <tr>
      <td>Magazyn danych SQL</td>
      <td>Polecenie</td>
      <td>Procedura składowana</td>
      <td>
        <font size=2>Protocol (protokół): tds <br>uwierzytelnianie: {Protocol (protokół), windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schematu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt</font>
      </td>
    </tr>
    <tr>
      <td>Magazyn danych SQL</td>
      <td>TableValuedFunction</td>
      <td>Funkcji zwracających tabelę</td>
      <td>
        <font size=2>Protocol (protokół): tds <br>uwierzytelnianie: {Protocol (protokół), windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schematu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt</font>
      </td>
    </tr>
    <tr>
      <td>Magazyn danych SQL</td>
      <td>Kontener</td>
      <td>Bazy danych</td>
      <td>
        <font size=2>Protocol (protokół): tds <br>uwierzytelnianie: {Protocol (protokół), windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych</font>
      </td>
    </tr>
    <tr>
      <td>Magazyn danych SQL</td>
      <td>Tabela</td>
      <td>Widoku tabeli</td>
      <td>
        <font size=2>Protocol (protokół): tds <br>uwierzytelnianie: {Protocol (protokół), windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schematu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt</font>
      </td>
    </tr>
    <tr>
      <td>Program SQL Server</td>
      <td>Polecenie</td>
      <td>Procedura składowana</td>
      <td>
        <font size=2>Protocol (protokół): tds <br>uwierzytelnianie: {Protocol (protokół), windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schematu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt</font>
      </td>
    </tr>
    <tr>
      <td>Program SQL Server</td>
      <td>TableValuedFunction</td>
      <td>Funkcji zwracających tabelę</td>
      <td>
        <font size=2>Protocol (protokół): tds <br>uwierzytelnianie: {Protocol (protokół), windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schematu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt</font>
      </td>
    </tr>
    <tr>
      <td>Program SQL Server</td>
      <td>Kontener</td>
      <td>Bazy danych</td>
      <td>
        <font size=2>Protocol (protokół): tds <br>uwierzytelnianie: {Protocol (protokół), windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych</font>
      </td>
    </tr>
    <tr>
      <td>Program SQL Server</td>
      <td>Tabela</td>
      <td>Widoku tabeli</td>
      <td>
        <font size=2>Protocol (protokół): tds <br>uwierzytelnianie: {Protocol (protokół), windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schematu <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services wielowymiarowych</td>
      <td>Kontener</td>
      <td>Model</td>
      <td>
        <font size=2>Protocol (protokół): usług analysis services <br>uwierzytelnianie: {windows, podstawowe i anonimowe, Brak} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Model</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services wielowymiarowych</td>
      <td>KLUCZOWY WSKAŹNIK WYDAJNOŚCI</td>
      <td>KLUCZOWY WSKAŹNIK WYDAJNOŚCI</td>
      <td>
        <font size=2>Protocol (protokół): usług analysis services <br>uwierzytelnianie: {windows, podstawowe i anonimowe, Brak} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Typ obiektu: {kluczowego wskaźnika wydajności}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services wielowymiarowych</td>
      <td>Miary</td>
      <td>Miary</td>
      <td>
        <font size=2>Protocol (protokół): usług analysis services <br>uwierzytelnianie: {windows, podstawowe i anonimowe, Brak} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Typ obiektu: {miary}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services wielowymiarowych</td>
      <td>Tabela</td>
      <td>Wymiarów</td>
      <td>
        <font size=2>Protocol (protokół): usług analysis services <br>uwierzytelnianie: {windows, podstawowe i anonimowe, Brak} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Typ obiektu: {wymiaru}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tabelaryczny</td>
      <td>Kontener</td>
      <td>Model</td>
      <td>
        <font size=2>Protocol (protokół): usług analysis services <br>uwierzytelnianie: {windows, podstawowe i anonimowe, Brak} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Model</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tabelaryczny</td>
      <td>KLUCZOWY WSKAŹNIK WYDAJNOŚCI</td>
      <td>KLUCZOWY WSKAŹNIK WYDAJNOŚCI</td>
      <td>
        <font size=2>Protocol (protokół): usług analysis services <br>uwierzytelnianie: {windows, podstawowe i anonimowe, Brak} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Typ obiektu: {kluczowego wskaźnika wydajności}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tabelarycznym</td>
      <td>Miary</td>
      <td>Miary</td>
      <td>
        <font size=2>Protocol (protokół): usług analysis services <br>uwierzytelnianie: {windows, podstawowe i anonimowe, Brak} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Typ obiektu: {miary}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tabelarycznym</td>
      <td>Tabela</td>
      <td>Tabela</td>
      <td>
        <font size=2>Protocol (protokół): usług analysis services <br>uwierzytelnianie: {windows, podstawowe i anonimowe, Brak} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Typ obiektu: {tabeli}</font>
      </td>
    </tr>
    <tr>
      <td>Usługi SQL Server Reporting Services</td>
      <td>Kontener</td>
      <td>Serwer</td>
      <td>
        <font size=2>Protocol (protokół): usług reporting services <br>uwierzytelnianie: {windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Wersja: {ReportingService2010}</font>
      </td>
    </tr>
    <tr>
      <td>Usługi SQL Server Reporting Services</td>
      <td>Raport</td>
      <td>Raport</td>
      <td>
        <font size=2>Protocol (protokół): usług reporting services <br>uwierzytelnianie: {windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Ścieżka <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Wersja: {ReportingService2010}</font>
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Kontener</td>
      <td>Bazy danych</td>
      <td>
        <font size=2>Protocol (protokół): teradata <br>uwierzytelnianie: {Protocol (protokół), windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych</font>
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Tabela</td>
      <td>Widoku tabeli</td>
      <td>
        <font size=2>Protocol (protokół): teradata <br>uwierzytelnianie: {Protocol (protokół), windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Serwer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bazy danych <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;obiekt</font>
      </td>
    </tr>
    <tr>
      <td>Usługi danych SQL Server wzorca</td>
      <td>Kontener</td>
      <td>Model</td>
      <td>
        <font size="2">Protocol (protokół): mssql mds <br>uwierzytelnianie: {windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Wersja</font>
      </td>
    </tr>
    <tr>
      <td>Usługi danych SQL Server wzorca</td>
      <td>Tabela</td>
      <td>Jednostki</td>
      <td>
        <font size="2">Protocol (protokół): mssql mds <br>uwierzytelnianie: {windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;adres URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Wersja <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;jednostki</font>
      </td>
    </tr>
    <tr>
      <td>Inne (nie w jednym z powyższych)</td>
      <td>\*</td>
      <td>\*</td>
      <td>
        <font size=2>Protocol (protokół): rodzajowy zawartości <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IDzasobu</font>
      </td>
    </tr>
</table>
