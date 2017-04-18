<properties
   pageTitle="Lokalnie monitorowania i diagnozowanie usług napisane za pomocą tkaninie usługi Azure | Microsoft Azure"
   description="Dowiedz się, jak można monitorować i diagnozowanie usług napisanym w języku Microsoft Azure usługi tkaninie na komputerze lokalnym rozwoju."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/24/2016"
   ms.author="subramar"/>


# <a name="monitor-and-diagnose-services-in-a-local-machine-development-setup"></a>Monitorowanie i diagnozowanie usług w ustawieniach rozwoju komputer lokalny


> [AZURE.SELECTOR]
- [Systemu Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
- [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)

Monitorowanie, wykrywanie, diagnozowanie i rozwiązywanie problemów Zezwalaj dla usług kontynuować pracę z minimalnymi zakłócenia w pracy użytkownika. Monitorowania i diagnostyczne są krytyczne w środowisku produkcyjnym wdrożonym rzeczywiste. Przyjęcie modelu podobne podczas opracowywania usług gwarantuje, że proces diagnostyczne działa po przeniesieniu do środowisku produkcyjnym. Usługa tkaninie ułatwia dla deweloperów usługi do wykonania diagnostyki bezproblemowo pracować przez zarówno ustawienia rozwoju lokalnego pojedynczego komputera, jak i ustawień klaster produkcji rzeczywistych.


## <a name="debugging-service-fabric-java-applications"></a>Debugowanie aplikacji usługi tkaninie Java

W przypadku aplikacji Java [wielu RAM rejestrowania](http://en.wikipedia.org/wiki/Java_logging_framework) są dostępne. Ponieważ `java.util.logging` jest domyślna opcja z JRE, jest ono także używane dla [Przykłady kodu w github](http://github.com/Azure-Samples/service-fabric-java-getting-started).  Następujące dyskusji wyjaśniono, jak skonfigurować `java.util.logging` framework. 
 
Za pomocą java.util.logging można przekierowywać Dzienniki aplikacji do pamięci, strumienie wyjściowe, pliki konsoli lub sockets. Dla każdej z tych opcji istnieje już podany w ramach obsługi domyślne. Możesz utworzyć `app.properties` pliku, aby skonfigurować obsługi plików aplikacji przekierowywać wszystkich dzienników do pliku lokalnego. 

Poniższy fragment kodu zawiera przykładowej konfiguracji: 

```java 
handlers = java.util.logging.FileHandler
 
java.util.logging.FileHandler.level = ALL
java.util.logging.FileHandler.formatter = java.util.logging.SimpleFormatter
java.util.logging.FileHandler.limit = 1024000
java.util.logging.FileHandler.count = 10
java.util.logging.FileHandler.pattern = /tmp/servicefabric/logs/mysfapp%u.%g.log             
```

Folder wskazywany przez `app.properties` plik musi istnieć. Po `app.properties` jest tworzony plik, należy zmodyfikować skrypt punktu wejścia `entrypoint.sh` w `<applicationfolder>/<servicePkg>/Code/` folder, aby ustawić właściwość `java.util.logging.config.file` do `app.propertes` pliku. Wpis powinien wyglądać wstawkę kodu następujące:

```sh 
java -Djava.library.path=$LD_LIBRARY_PATH -Djava.util.logging.config.file=<path to app.properties> -jar <service name>.jar
```
 
 
Ta konfiguracja powoduje dzienników są zbierane w sposób obracania na `/tmp/servicefabric/logs/`. **Więcej** i **%g** umożliwia tworzenie więcej plików, za pomocą mysfapp0.log nazwy plików, mysfapp1.log i tak dalej. Domyślnie jeśli brak obsługi jawnie są skonfigurowane, obsługi konsoli jest zarejestrowany. Jeden można wyświetlać dzienniki na syslog w obszarze /var/log/syslog.
 
Aby uzyskać więcej informacji zobacz [Przykłady kodu w github](http://github.com/Azure-Samples/service-fabric-java-getting-started).  



## <a name="next-steps"></a>Następne kroki
Ten sam kod śledzenia dodane do aplikacji ma zastosowanie również Diagnostyka aplikacji w klastrze Azure. Zapoznaj się z następujących artykułów, które omówiono różne opcje dla narzędzi i opisano, jak je skonfigurować pocztę.
* [Jak zebrać dzienniki Diagnostyka Azure](service-fabric-diagnostics-how-to-setup-lad.md)
