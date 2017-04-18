<properties 
    pageTitle="Wprowadzenie do narzędzia elastyczne bazy danych" 
    description="Podstawowe informacje dotyczące funkcji narzędzia elastyczne bazy danych bazy danych SQL Azure, w tym łatwe do uruchamiania aplikacji przykładowych." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove" 
    editor="CarlRabeler"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove"/>

# <a name="get-started-with-elastic-database-tools"></a>Wprowadzenie do narzędzia elastyczne bazy danych

Ten dokument, poznasz obsługi Deweloper, uruchamiając aplikację próbki. Próbka tworzy prosty aplikację sharded i opisuje kluczowe funkcje narzędzia elastyczne bazy danych. W przykładzie pokazano funkcje [Biblioteka klienta elastyczne bazy danych](sql-database-elastic-database-client-library.md)

Aby zainstalować bibliotekę, przejdź do [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Uwaga zainstalowania biblioteki za pomocą aplikacji przykładowych opisane poniżej.

## <a name="prerequisites"></a>Wymagania wstępne

1. Wymagany jest program Visual Studio 2012 lub nowszy z C#. Pobierz bezpłatną wersję w [Visual Studio — pliki do pobrania](http://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
2. Nuget 2.7 lub nowszym. Aby uzyskać najnowszą wersję, zobacz [Instalowanie NuGet](http://docs.nuget.org/docs/start-here/installing-nuget)

## <a name="download-and-run-the-sample-app"></a>Pobierz i uruchom aplikację próbki

**Elastyczne bazy danych za pomocą Azure SQL — wprowadzenie** Przykładowa aplikacja ilustruje najważniejszych aspektów proces tworzenia dla sharded aplikacji za pomocą narzędzia elastyczne bazy danych Azure SQL. Poświęcony przypadków użycia klucza [shard mapowanie zarządzania](sql-database-elastic-scale-shard-map-management.md), [dane zależne routingu](sql-database-elastic-scale-data-dependent-routing.md) i [shard wielu kwerend](sql-database-elastic-scale-multishard-querying.md). Aby pobrać i uruchomić przykład, wykonaj następujące czynności: 

1. Otwórz program Visual Studio i wybierz pozycję **Plik -> Nowy -> projektu**.
2. W oknie dialogowym kliknij pozycję **Online**.

    ![Nowy Projekt > Online][2]
3. Następnie kliknij przycisk **Visual C#** w obszarze **próbki**.

    ![Kliknij pozycję podpowiedzi wizualne#][3]
4. W polu wyszukiwania wpisz **elastyczne db** szukać próbki. Pojawi się nazwa **Elastyczne narzędzia bazy danych SQL Azure — wprowadzenie** .

    ![Pole wyszukiwania][1]
 
5. Zaznacz próbki, wybierz nazwę i lokalizację dla nowego projektu i naciśnij **przycisk OK** , aby utworzyć projekt.
6. Otwieranie pliku **app.config** w rozwiązanie projektu próbki i postępuj zgodnie z instrukcjami w pliku, można dodać nazwę serwera bazy danych Azure SQL i informacje logowania (nazwa użytkownika i hasło).
7. Tworzenie i uruchamianie aplikacji. Gdy zostanie wyświetlony monit, daj Visual Studio przywrócić pakietów NuGet rozwiązania. Pobierze najnowszą wersję pakietu Biblioteka klienta elastyczne bazy danych z NuGet.
8. Odtwarzanie z różnych opcji, aby dowiedzieć się więcej na temat możliwości biblioteki klienta. Uwaga te czynności, które aplikacja ma w konsoli dane wyjściowe i zachęcamy do Eksplorowanie kod w tle.

    ![informacje o postępie][4]

Gratulacje — pomyślnie wbudowane i uruchom pierwszą aplikację sharded za pomocą narzędzia elastyczne bazy danych w bazie danych SQL Azure. Zapoznaj się szybkie odłamki, utworzone za pomocą połączenia z programu Visual Studio lub SQL Server Management Studio z serwerem bazy danych Azure próbki. Można zauważyć shard przykładowe nowej bazy danych i shard mapy Menedżera bazy danych utworzonej próbki.

> [AZURE.IMPORTANT] Zalecane jest zawsze używać najnowszej wersji programu Management Studio do pozostawać aktualizacje Microsoft Azure i baza danych SQL. [Aktualizowanie programu SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


### <a name="key-pieces-of-the-code-sample"></a>Ważne przykładowy kod

1. **Zarządzanie odłamki i Shard mapy**: kod pokazano, jak pracować z odłamki, zakresów, i mapowania w pliku **ShardMapManagerSample.cs**. Można znaleźć więcej informacji na ten temat tutaj: [Zarządzanie mapy Shard](http://go.microsoft.com/?linkid=9862595).  
2. **Dane zależne routingu**: Routing transakcji do prawej shard są wyświetlane w **DataDependentRoutingSample.cs**. Aby uzyskać więcej informacji zobacz [Kierowanie zależne danych](http://go.microsoft.com/?linkid=9862596). 
3. **Kwerendy na wielu odłamki**: kwerendy w odłamki przedstawiono w pliku **MultiShardQuerySample.cs**. Aby uzyskać więcej informacji zobacz [Shard wielu kwerend](http://go.microsoft.com/?linkid=9862597).
4. **Dodawanie pustych odłamki**: iteracyjne dodawania nowego pustego odłamki jest wykonywane przez kod w pliku **AddNewShardsSample.cs**. Szczegóły w tym temacie omówiono tutaj: [Zarządzanie mapy Shard](http://go.microsoft.com/?linkid=9862595).

### <a name="other-elastic-scale-operations"></a>Inne operacje elastyczne skali

1. **Dzielenie istniejących shard**: możliwość podziału odłamki znajduje się za pomocą **narzędzi korespondencji seryjnej podziału**. Można znaleźć więcej informacji na temat tego narzędzia tutaj: [Omówienie narzędzia korespondencji seryjnej podziału](sql-database-elastic-scale-overview-split-and-merge.md).
2. **Wstawianie istniejącego odłamki**: scala Shard również odbywa się za pomocą **narzędzi korespondencji seryjnej podziału**. Aby uzyskać więcej informacji, odwołują się do: [Omówienie narzędzia korespondencji seryjnej podziału](sql-database-elastic-scale-overview-split-and-merge.md).   


## <a name="cost"></a>Koszt

Narzędzia elastyczne bazy danych są dostępne bezpłatnie. Narzędzia elastyczne bazy danych nie nakłada dodatkowych opłat na bieżąco koszty użycia Azure. 

Na przykład aplikacja przykładowe tworzy nowych baz danych. Koszt zależy od wersji bazy danych bazy danych SQL Azure, którą możesz wybrać i Azure zastosowania aplikacji.

Aby uzyskać informacje o cenach zobacz [Szczegóły ceny bazy danych SQL](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej informacji na temat narzędzia elastyczne bazy danych zobacz:

* [Mapa dokumentacji narzędzia elastyczne bazy danych](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/) 
-    Przykłady kodu: 
    -    [Elastyczne DB z SQL Azure — wprowadzenie](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-a80d8dc6?SRC=VSIDE)
    -    [Elastyczne DB z SQL Azure — Integracja z Framework jednostki](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-bae904ba?SRC=VSIDE)
    -    [Elastyczność shard w Centrum skryptów](https://gallery.technet.microsoft.com/scriptcenter/Elastic-Scale-Shard-c9530cbe)
-    Blog: [Anons elastyczne skali](https://azure.microsoft.com/blog/2014/10/02/introducing-elastic-scale-preview-for-azure-sql-database/)
-    Kanał 9: [Elastyczne skali omówienie wideo](http://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Database-Elastic-Scale)
-    Forum dyskusyjne: [Forum bazy danych SQL Azure](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted)
-    Do pomiaru wydajności: [liczniki wydajności Menedżera map shard](sql-database-elastic-database-client-library.md)


<!--Anchors-->
[The Elastic Scale Sample Application]: #The-Elastic-Scale-Sample-Application
[Download and Run the Sample App]: #Download-and-Run-the-Sample-App
[Cost]: #Cost
[Next steps]: #next-steps

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-get-started/newProject.png
[2]: ./media/sql-database-elastic-scale-get-started/click-online.png
[3]: ./media/sql-database-elastic-scale-get-started/click-CSharp.png
[4]: ./media/sql-database-elastic-scale-get-started/output2.png
 
