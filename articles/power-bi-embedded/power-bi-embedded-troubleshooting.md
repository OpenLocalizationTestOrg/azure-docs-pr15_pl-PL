<properties
   pageTitle="Rozwiązywanie problemów z Microsoft Power BI Preview osadzony"
   description="Rozwiązywanie problemów z Microsoft Power BI Preview osadzony"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="microsoft-power-bi-embedded-preview-troubleshooting"></a>Rozwiązywanie problemów z Microsoft Power BI Preview osadzony
Ten artykuł zawiera odpowiedzi jak rozwiązywać problemy z **Power BI osadzony**.

<a name="connection-string"/>
## <a name="setting-sql-server-connection-strings"></a>Ustawianie parametry połączenia programu SQL Server
Aby ustawić programu SQL Server, połączenie ciągu, należy wykonywać określonego formatu. Poniżej znajduje przykład parametry połączenia dla programu SQL Server.

```
"Persist Security Info=False;Integrated Security=true;Initial Catalog=Northwind;server=(local)"
```

Aby dowiedzieć się więcej na temat ciągów połączeń programu SQL Server, zobacz następujące artykuły:

-   [Parametry połączenia programu SQL Server](https://msdn.microsoft.com/library/jj653752.aspx)
-   [SqlConnection.ConnectionString](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.connectionstring.aspx)

<a name="credentials"/>
## <a name="setting-credentials"></a>Ustawianie poświadczeń
W przypadku, gdy masz poświadczenia dla rozwoju lub tymczasowy środowiska, takich jak nazwa użytkownika i hasło może być konieczne zaktualizowanie poświadczeń, które są zgodne z rozwiązanie produkcji.

## <a name="see-also"></a>Zobacz też
- [Rozpoczynanie pracy z próbki](power-bi-embedded-get-started-sample.md)
- [Co to jest Power BI osadzonego](power-bi-embedded-what-is-power-bi-embedded.md)
