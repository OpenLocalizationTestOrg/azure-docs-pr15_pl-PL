<properties 
    pageTitle="Monitorowanie współzależności, wyjątki i czasów wykonania w aplikacjach sieci web języka Java" 
    description="Rozszerzone monitorowania Java witryny sieci Web przy użyciu aplikacji wniosków" 
    services="application-insights" 
    documentationCenter="java"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="awills"/>
 
# <a name="monitor-dependencies-exceptions-and-execution-times-in-java-web-apps"></a>Monitorowanie współzależności, wyjątki i czasów wykonania w aplikacjach sieci web języka Java

*Wnioski aplikacji jest w podglądzie.*

Jeśli masz [narzędzia Java aplikacji sieci web przy użyciu aplikacji wniosków][java], Java Agent umożliwia uzyskiwanie wniosków szczegółowego, bez żadnych zmian kodu:


* **Zależności:** Dane dotyczące połączeń, które aplikacja powoduje inne składniki, w tym:
 * **Umieść połączeń** przez HttpClient, OkHttp i RestTemplate (Wiosenna).
 * **Redis** wywołań za pomocą klienta Jedis. Jeśli połączenie trwa dłużej niż 10s, agent również pobiera argumenty połączenia.
 * **[Połączenia JDBC](http://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)** - MySQL, SQL Server, PostgreSQL, SQLite, Oracle DB lub DB DOM Apache. "executeBatch" połączenia są obsługiwane. Jeśli połączenie trwa dłużej niż 10s, agent MySQL i PostgreSQL raportów planu kwerend. 
* **Otrzymanych wyjątki:** Dane dotyczące wyjątków, które są obsługiwane przez kod.
* **Czasu wykonywania metoda:** Dane dotyczące czas potrzebny do wykonywania określonych metod.

Aby użyć agenta Java, można ją zainstalować na serwerze. Twoje aplikacje sieci web musi narzędzia przy użyciu [Aplikacji wniosków Java SDK][java].

## <a name="install-the-application-insights-agent-for-java"></a>Instalowanie agenta wniosków aplikacji dla języka Java

1. Na komputerze z serwerem Java, [Pobierz agenta](https://aka.ms/aijavasdk).
2. Edytowanie skryptu uruchamiania aplikacji serwera, a następnie dodaj następujące maszyny wirtualnej Java:

    `javaagent:`*Pełna ścieżka do pliku SŁOIK agenta*

    Na przykład w Tomcat na komputerze Linux:

    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path to agent JAR file>"`


3. Uruchom ponownie serwer aplikacji.

## <a name="configure-the-agent"></a>Konfigurowanie agenta

Tworzenie pliku o nazwie `AI-Agent.xml` i umieszczanie go w tym samym folderze co plik SŁOIK agenta.

Ustawianie zawartości pliku xml. Edytuj poniższy przykład można uwzględnić lub pominąć funkcji potrzebne. 

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsightsAgent>
      <Instrumentation>
        
        <!-- Collect remote dependency data -->
        <BuiltIn enabled="true">
           <!-- Disable Redis or alter threshold call duration above which arguments are sent.
               Defaults: enabled, 10000 ms -->
           <Jedis enabled="true" thresholdInMS="1000"/>
           
           <!-- Set SQL query duration above which query plan is reported (MySQL, PostgreSQL). Default is 10000 ms. -->
           <MaxStatementQueryLimitInMS>1000</MaxStatementQueryLimitInMS>
        </BuiltIn>

        <!-- Collect data about caught exceptions 
             and method execution times -->

        <Class name="com.myCompany.MyClass">
           <Method name="methodOne" 
               reportCaughtExceptions="true"
               reportExecutionTime="true"
               />

           <!-- Report on the particular signature
                void methodTwo(String, int) -->
           <Method name="methodTwo"
              reportExecutionTime="true"
              signature="(Ljava/lang/String;I)V" />
        </Class>
        
      </Instrumentation>
    </ApplicationInsightsAgent>

```

Musisz włączyć wyjątek raportów i chronometrażu metody dla poszczególnych metod.

Domyślnie `reportExecutionTime` jest prawdziwa i `reportCaughtExceptions` ma wartość FAŁSZ.

## <a name="view-the-data"></a>Widok danych

W zasobie wniosków aplikacji zagregowane zdalnego współzależności metody wykonanie godziny i jest wyświetlany w [obszarze kafelków wydajności][metrics]. 

Aby wyszukać poszczególne wystąpienia współzależności wyjątku i metody raportów, otwórz [wyszukiwania][diagnostic]. 

[Diagnozowanie problemów zależności — Dowiedz się więcej](app-insights-dependencies.md#diagnosis).



## <a name="questions-problems"></a>Masz pytania? Problemy?

* Brak danych? [Konfigurowanie wyjątków zapory](app-insights-ip-addresses.md)
* [Rozwiązywanie problemów z języka Java](app-insights-java-troubleshoot.md)



<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[usage]: app-insights-web-track-usage.md

 
