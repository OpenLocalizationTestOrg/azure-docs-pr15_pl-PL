<properties 
   pageTitle="Azure SDK dla środowiska .NET 2.6 informacje o wersji" 
   description="Azure SDK dla środowiska .NET 2.6 informacje o wersji" 
   services="app-service/web" 
   documentationCenter=".net" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/17/2016"
   ms.author="juliako"/>

 
# <a name="azure-sdk-for-net-26-release-notes"></a>Azure SDK dla środowiska .NET 2.6 informacje o wersji

Ten dokument zawiera informacje o wersji dla zestawu SDK Azure w wersji .NET 2.6. 

Aplikacje usługi cloud (PaaS) kierowanie .NET 4.5.2 lub .NET 4.6, pod warunkiem, że ręcznie zainstalować docelowej .NET Framework dla roli usługi Cloud można tworzyć 2.6 SDK Azure. Zobacz [Instalowanie .NET z uprawnieniami usługi w chmurze](http://go.microsoft.com/fwlink/?LinkID=309796).


##<a name="service-bus-updates"></a>Aktualizacje usług Bus

- Koncentratory zdarzeń: 

    - Teraz umożliwia kontrola dostępu docelowej podczas wysyłania zdarzeń, umożliwiając dodatkowe publisher punkt końcowy dla koncentratorów zdarzenia.
    - Dodatkowe stabilność i poprawy jakości dodane do funkcji koncentratory zdarzenia.
    - Dodawanie obsługi protokołu Amqp nad WebSocket do obsługi wiadomości i koncentratory zdarzenia.

##<a name="hdinsight-tools-for-visual-studio-updates"></a>Narzędzia HDInsight aktualizacji programu Visual Studio

- **Rozszerzenie IntelliSense**: zdalnego metadanych sugestii

    Usługa HDInsight Tools for Visual Studio obsługuje teraz uzyskiwania zdalnego metadanych podczas edytowania skrypt gałęzi. Na przykład, można wpisać * *Wybierz* od** i zostaną wyświetlone wszystkie nazwy tabeli. Ponadto nazwy kolumn będą wyświetlane po określeniu tabeli.

- **Obsługa emulatora HDInsight**

    Teraz HDInsight Tools for Visual Studio obsługuje łączenia się z usługi HDInsight emulatora, więc skrypty gałęzi można opracowywać lokalnie, bez wprowadzania kosztami, wykonanie tych skryptów przed klastrów HDInsight. 

    Aby uzyskać więcej informacji zapoznaj się z [tym ręcznie](http://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409).

- **Usługa HDInsight Tools for Visual Studio obsługę ogólnego klastrów Hadoop** (Wersja preview)

    HDInsight Tools for Visual Studio obsługują ogólnego klastrów Hadoop, aby móc używać narzędzia HDInsight programu Visual Studio, wykonaj następujące czynności:

    - Połącz się z klastrem 
    - pisanie kwerenda gałęzi z rozszerzona obsługa technologii IntelliSense — autouzupełniania, 
    - Wyświetlanie wszystkich zadań w klastrze z intuicyjny interfejs użytkownika. 

    Aby uzyskać więcej informacji zapoznaj się z [tym ręcznie](http://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409).

##<a name="in-role-cache-updates"></a>Aktualizacje z pamięci podręcznej w roli

- **Pamięć podręczną w roli** została zaktualizowana do za pomocą **Microsoft Azure miejsca do magazynowania SDK** wersji 4.3. Do tej pory **Pamięci podręcznej w roli** została przy użyciu zestawu SDK usługi Azure przechowywania wersji 1.7.

    Klientów przy użyciu 2,5 SDK Azure lub poniżej należy zaktualizować Azure SDK 2.6 i Przenieś do nowej wersji zestawu SDK miejsca do magazynowania Azure. 

    W tej chwili Azure przechowywania wersji 2011-08-18 zaplanowano można usunąć 1 sierpnia 2016. Wszelkie migracji pamięci podręcznej w roli z 2,5 SDK Azure lub poniżej do 2.6 musi zostać zakończone przy tym razem. Aby uzyskać więcej informacji dotyczących emerytury Azure przechowywania wersji 2011-08-18, zobacz [Aktualizacja usuwania wersji usługi Microsoft Azure miejsca do magazynowania: rozszerzenie 2016](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx).

>[AZURE.IMPORTANT]Firma Microsoft jest informowanie o 30 listopada 2016 za usługi zarządzanych pamięci podręcznej Azure i pamięci podręcznej w roli Azure. Zaleca się migrowanie Azure Redis w pamięci podręcznej w przygotowaniu tej emerytury. Aby uzyskać więcej informacji na daty i wskazówki migracji, zobacz [oferująca której Azure pamięci podręcznej jest dla mnie odpowiednia?](../redis-cache/cache-faq.md#which-azure-cache-offering-is-right-for-me)

##<a name="azure-app-service-tools"></a>Narzędzia Azure aplikacji usługi

Poniższe elementy zostały zaktualizowane w wersji Azure SDK 2.6.

- Publikowanie Azure móc korzystać z aplikacji interfejsu API Azure jako cel wdrożenia.
- Interfejs API aplikacje inicjowania obsługi administracyjnej funkcji w celu umożliwienia użytkownikom przy użyciu funkcji tworzenia i inicjowania obsługi administracyjnej aplikacji interfejsu API.
- Eksplorator serwera zmieniony w celu dostosowania nowy węzeł aplikacji usługi z aplikacjami sieci Web, Mobile i interfejsu API pogrupowane według grup zasobów.
- Dodawanie gest Azure klienta aplikacji API dodane do większości C# projektów, które umożliwi automatyczne generowanie obsługą Swagger interfejsu API aplikacje uruchomione w subskrypcji Azure użytkownika.
- Narzędzia z interfejsu API aplikacje i węzły aplikacji usługi w Eksploratorze serwera są dostępne w Visual Studio 2013 tylko. 

##<a name="azure-resource-manager-tools-updates"></a>Azure aktualizacje narzędzia Menedżera zasobów

Narzędzia Menedżera zasobów Azure zostały zaktualizowane do uwzględnienia szablonów dla maszyn wirtualnych, sieci i miejsca do magazynowania. Aby uwzględnić nowy widok konspektu szablony i możliwość edytowania szablonów przy użyciu wstawki JSON została zaktualizowana JSON środowisko edytowania. Szablony wdrożony z programu Visual Studio za pomocą skryptu programu PowerShell dostarczonego z projektu, dlatego wszelkie zmiany wprowadzone do skryptu będą używane przez program Visual Studio.

##<a name="diagnostics-improvements-for-cloud-services"></a>Narzędzia diagnostyczne ulepszenia dotyczące usług w chmurze

Azure SDK 2.6 ponownie zapewnia obsługę do zbierania dzienników diagnostycznych w emulatorze Azure obliczeń i przenoszenia ich opracowywania miejsca do magazynowania. Wszelkie diagnostyki dzienniki (w tym aplikacji śledzenia dzienniki, śledzenie dzienniki systemu Windows (ETW), liczników wydajności, infrastruktura dzienniki zdarzeń systemu windows i dzienniki zdarzeń) wygenerowane podczas aplikacja działa w emulatorze mogą zostać przeniesione do rozwoju przestrzeni dyskowej, aby sprawdzić, czy na komputerze lokalnym działa z rejestrowanie diagnostyczne. 

Teraz można określić konta diagnostyki miejsca do magazynowania w pliku konfiguracji (.cscfg) usługi, ułatwiając używać różnych diagnostyki miejsca do magazynowania kont w różnych środowiskach. Istnieje kilka istotnych różnic między jak pracy parametry połączenia w 2,4 SDK Azure i Azure SDK 2.6. Aby uzyskać więcej informacji na temat korzystania z połączenia miejsca do magazynowania diagnostyki ciągu oraz ich wpływ projektów, zobacz [Konfigurowanie informacje diagnostyczne dla usług w chmurze Azure](http://go.microsoft.com/fwlink/?LinkID=532784).

##<a name="breaking-changes"></a>Przerywanie zmian

###<a name="azure-resource-manager-tools"></a>Azure Narzędzia Menedżera zasobów 

- Typ projektu **Projektów rozmieszczania chmury** dostępne w 2,5 SDK Azure została zmieniona na **Azure grupa zasobów**.
- Typ **Projektów rozmieszczania chmury** projektów utworzonych w 2,5 SDK Azure mogą zostać użyte w 2.6, ale wdrażanie szablonu z programu Visual Studio nie powiedzie się. Jednak wdrożenie z skrypt programu PowerShell wciąż działają tak jak poprzednio.  Aby uzyskać informacje na temat korzystania **Projektów rozmieszczania w chmurze** w 2.6 przeczytaj ten [opublikować](http://go.microsoft.com/fwlink/?LinkID=534086).
 
##<a name="known-issues"></a>Znane problemy

- Zbieranie dzienników diagnostycznych w emulatorze wymaga 64-bitowym systemie operacyjnym. Dzienniki diagnostyczne nie będą zbierane w razie korzystania z 32-bitowym systemie operacyjnym. Nie dotyczy to innych funkcji emulatora. 

- Azure 2.6 SDK wydany 2015-4-29 ma dwie kwestie: 

    - Nie można załadować uniwersalny aplikacji w Visual Studio 2015, Azure SDK 2.6 został zainstalowany na komputerze.
    - Debugowanie projektu usługi w chmurze przestaną działać w Visual Studio 2013 i Visual Studio 2015 miejsce, w którym Visual Studio przestaje odpowiadać i ulega awarii podczas wyświetlania okna dialogowego z komunikatem "Konfigurowanie informacje diagnostyczne dla emulatora".
    
    Aktualizacja dla Azure SDK 2.6 został wydany 2015-5-18. Zaktualizowana wersja jest 2.6.30508.1601; zawiera poprawki dotyczące dwóch problemów opisany powyżej. Można określić, tworzenie zestawu SDK z poziomu Panelu sterowania -> programy i funkcje -> Narzędzia pakietu Microsoft Azure dla programu Microsoft Visual Studio 2013 — v 2.6. W kolumnie wersja będzie wyświetlany numer kompilacji.

    Jeśli nadal są przeciwległych powyższe problemy, należy zainstalować najnowszą wersję zestawu SDK 2.6 Azure [w PORÓWNANIU z 2012](http://go.microsoft.com/fwlink/p/?linkid=323511&clcid=0x409), [2013 w PORÓWNANIU z](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) lub [2015 w PORÓWNANIU z](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409).
 
##<a name="see-also"></a>Zobacz też

[Pomocy technicznej i informacji dotyczących Azure SDK emerytury .NET i interfejsów API](https://msdn.microsoft.com/library/azure/dn479282.aspx/)
