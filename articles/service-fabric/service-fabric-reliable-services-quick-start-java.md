<properties
   pageTitle="Rozpoczynanie pracy z usługami zaufanego | Microsoft Azure"
   description="Wprowadzenie do tworzenia aplikacji Microsoft Azure usługi tkaninie z usługami bezstanowe i stanowe."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/26/2016"
   ms.author="vturecek"/>

# <a name="get-started-with-reliable-services"></a>Rozpoczynanie pracy z usługami zaufanego

> [AZURE.SELECTOR]
- [C# w systemie Windows](service-fabric-reliable-services-quick-start.md)
- [Java w systemie Linux](service-fabric-reliable-services-quick-start-java.md)

W tym artykule opisano podstawy usługi zaufanego tkaninie usługi Azure i przeprowadzi Cię przez tworzenie i wdrażanie prostych aplikacji usługi zaufanego napisany w języku Java.

## <a name="installation-and-setup"></a>Instalacja i Konfiguracja
Zanim zaczniesz, upewnij się, że masz środowisko projektowania tkaninie usługi konfigurowanie na tym komputerze.
Jeśli chcesz skonfigurować ją, przejdź do [wprowadzenie na komputerze Mac](service-fabric-get-started-mac.md) lub [wprowadzenie w systemie Linux](service-fabric-get-started-linux.md).

## <a name="basic-concepts"></a>Podstawowe pojęcia
Aby rozpocząć pracę z usługami zaufanego, wystarczy opis kilku podstawowych pojęć:

 - **Typ usługi**: jest to implementacją usługi. Jest definiowana klasa pisania `StatelessService` i dowolne inne kod lub zależności używany, oraz nazwę i numer wersji.

 - **Nazwane wystąpienie usługi**: uruchamiania usługi, tworzenia nazwane wystąpienia tego typu usługi, podobnie jak tworzenie obiektów typu klasy. Wystąpienia usługi są w rzeczywistości wystąpieniami obiektu klasie usługi, którą piszesz. 

 - **Host usługi**: wystąpień nazwanych usługi, możesz utworzyć potrzebne do uruchamiania wewnątrz hosta. Hosta usługi jest po prostu procesem miejsce, w którym można uruchamiać wystąpienia tej usługi.

 - **Rejestracja usług**: rejestracji łączy wszystko. Typ usługi musi być zarejestrowana w runtime tkaninie usługi w hosta usługi, aby umożliwić tkaninie usługi utworzyć wystąpień do uruchomienia.  

## <a name="create-a-stateless-service"></a>Tworzenie stateless usługi

Zacznij od utworzenia nowej aplikacji usługi tkaninie. Usługa SDK tkaninie Linux zawiera Yeoman generator zapewnienie rusztowania dla aplikacji usługi tkaninie usłudze bezstanowym. Uruchom, uruchamiając Yeoman następujące polecenie:

```bash
$ yo azuresfjava
```

Postępuj zgodnie z instrukcjami, aby utworzyć **Zaufanego Stateless usługi**. Ten samouczek nadać nazwę aplikacji "HelloWorldApplication" i "HelloWorld". Wynik będzie zawierać katalogów `HelloWorldApplication` i `HelloWorld`.

```bash
HelloWorldApplication/
├── build.gradle
├── HelloWorld
│   ├── build.gradle
│   └── src
│       └── statelessservice
│           ├── HelloWorldServiceHost.java
│           └── HelloWorldService.java
├── HelloWorldApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   └── _readme.txt
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="implement-the-service"></a>Implementowanie usługi

Otwórz **HelloWorldApplication/HelloWorld/src/statelessservice/HelloWorldService.java**. Ta klasa określa typ usługi i może zostać uruchomiony dowolny kod. Interfejs API usługi zawiera dwóch punktów wejścia kodu:

 - Metody punktu wejścia otwarty, o nazwie `runAsync()`, w którym można rozpocząć wykonywanie dowolnej obciążeń pracą, łącznie z obciążeń pracą długim obliczeń.

```java
@Override
protected CompletableFuture<?> runAsync() {
    ...
}
```

 - Punkt wejścia komunikacji, gdzie możesz podłączyć stosu komunikacji wybór. Jest to miejsce, w którym możesz rozpocząć odbierania żądań od użytkowników i innych usług.

```java
@Override
protected List<ServiceInstanceListener> createServiceInstanceListeners() {
    ...
}
```

W tym samouczku firma Microsoft poświęcona `runAsync()` metody punktu wejścia. Jest to miejsce, w którym natychmiast można rozpocząć generowanie kodu.

### <a name="runasync"></a>RunAsync

Platformy połączeń tej metody, w przypadku wystąpienia usługi umieścić i gotowe do wykonania. Cykl otwierania/zamykania wystąpienia usługi mogą występować wielokrotnie w okresie istnienia usługi jako całość. Może się to zdarzyć z różnych powodów, w tym:

- System powoduje przejście do wystąpień usługi równoważenia zasobu.
- Występują błędy w kodzie.
- Aplikacja lub system jest uaktualniany.
- Używanego sprzętu ulegnie awarii.

Odbywa się to aranżacji przez tkaninie usługi, aby zachować usługi wysokiej dostępności i poprawnie dopasowane.

#### <a name="cancellation"></a>Anulowanie

Ważne jest, który kodu w `runAsync()` można zatrzymać wykonywanie, gdy powiadomienia tkaninie usługi. `CompletableFuture` Zwrócone przez `runAsync()` została anulowana, gdy usługa tkaninie wymaga usługi, aby zatrzymać wykonywanie. Poniższy przykład przedstawia sposób obsługi zdarzeń anulowania: 

```java
    @Override
    protected CompletableFuture<?> runAsync() {

        CompletableFuture<?> completableFuture = new CompletableFuture<>();
        ExecutorService service = Executors.newFixedThreadPool(1);
        
        Future<?> userTask = service.submit(() -> {
            while (!Thread.currentThread().isInterrupted()) {
                try
                {
                   logger.log(Level.INFO, this.context().serviceName().toString());
                   Thread.sleep(1000);
                }
                catch (InterruptedException ex)
                {
                    logger.log(Level.INFO, this.context().serviceName().toString() + " interrupted. Exiting");
                    return;
                }
            }
         });
 
        completableFuture.handle((r, ex) -> {
            if (ex instanceof CancellationException) {
                userTask.cancel(true);
                service.shutdown();
            }
            return null;
        });
 
        return completableFuture;
   }
``` 

### <a name="service-registration"></a>Rejestracja usług

Typy usług musi być zarejestrowana w środowisko uruchomieniowe usługi tkaninie. Typ usługi jest zdefiniowana w `ServiceManifest.xml` i klasie usługi, który wykonuje `StatelessService`. Rejestracja usług odbywa się w proces głównego punktu wejścia. W tym przykładzie jest proces głównego punktu wejścia `HelloWorldServiceHost.java`:

```java
public static void main(String[] args) throws Exception {
    try {
        ServiceRuntime.registerStatelessServiceAsync("HelloWorldType", (context) -> new HelloWorldService(), Duration.ofSeconds(10));
        logger.log(Level.INFO, "Registered stateless service type HelloWorldType.");
        Thread.sleep(Long.MAX_VALUE);
    } 
    catch (Exception ex) {
        logger.log(Level.SEVERE, "Exception in registration: {0}", ex.toString());
        throw ex;
    }
}
```

## <a name="run-the-application"></a>Uruchamianie aplikacji

Yeoman rusztowania zawiera skrypt gradle do tworzenia aplikacji i urodzinową skrypty do wdrożenia i wyczyść-wdrożyć aplikację. Aby uruchomić aplikację, należy najpierw utworzyć aplikację z gradle:

```bash
$ gradle
```

Spowoduje to pakiet aplikacji tkaninie usług, które mogą być wdrażane za pomocą usługi tkaninie Azure interfejsu wiersza polecenia. Skrypt install.sh zawiera niezbędne polecenia polecenie Azure, aby wdrożyć pakiet aplikacji. Po prostu uruchom skrypt install.sh do wdrażania:

```bask
$ ./install.sh
```
