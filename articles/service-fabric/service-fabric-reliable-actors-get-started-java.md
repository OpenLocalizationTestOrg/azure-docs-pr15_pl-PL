<properties
   pageTitle="Wprowadzenie do usługi tkaninie zaufanego aktorów | Microsoft Azure"
   description="Ten samouczek przeprowadzi Cię przez kroki tworzenia, debugowania i wdrażanie proste usługi opartej na Aktor przy użyciu usługi tkaninie zaufanego aktorów."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/25/2016"
   ms.author="vturecek"/>

# <a name="getting-started-with-reliable-actors"></a>Wprowadzenie do zaufanego uczestników

> [AZURE.SELECTOR]
- [C# w systemie Windows](service-fabric-reliable-actors-get-started.md)
- [Java w systemie Linux](service-fabric-reliable-actors-get-started-java.md)

W tym artykule opisano podstawy aktorów zaufanego tkaninie usługi Azure i przeprowadzi Cię przez tworzenie i wdrażanie prostą aplikację zaufanego aktora w Java.

## <a name="installation-and-setup"></a>Instalacja i Konfiguracja
Zanim zaczniesz, upewnij się, że masz środowisko projektowania tkaninie usługi konfigurowanie na tym komputerze.
Jeśli chcesz skonfigurować ją, przejdź do [wprowadzenie na komputerze Mac](service-fabric-get-started-mac.md) lub [wprowadzenie w systemie Linux](service-fabric-get-started-linux.md).

## <a name="basic-concepts"></a>Podstawowe pojęcia
Aby rozpocząć pracę z zaufanego uczestników, wystarczy poznać kilka podstawowych pojęć związanych z:

 * **Usługa aktora**. Niezawodne uczestników są dostarczane w usługach zaufanego, które mogą być wdrażane w infrastrukturze tkaninie usługi. Wystąpienia Aktor są aktywowane w przypadku nazwaną usługę.
 
 * **Rejestracja aktora**. Jako z usługami zaufanego Aktor zaufanego usługa musi być zarejestrowany na środowisko uruchomieniowe usługi tkaninie. Ponadto typ aktora musi być zarejestrowana w czasie wykonywania aktora.
 
 * **Interfejs aktora**. Interfejs aktor jest używany do definiowania jednoznacznie określonym interfejsu publicznego aktora. W terminologii modelu zaufanego aktora interfejs Aktor definiuje typy wiadomości, które mogą zrozumieć aktor i proces. Interfejs aktor jest używane przez innych uczestników i aplikacje klienckie do "Wyślij" (asynchroniczne) wiadomości aktora. Niezawodne aktorów mogą zawierać wiele interfejsy.
 
 * **Klasy ActorProxy**. Klasy ActorProxy jest używane w aplikacjach klienckich do wywołania metod udostępnianych za pośrednictwem interfejsu aktora. Klasa ActorProxy udostępnia dwie ważne funkcje:
    * Rozpoznawanie nazw: jest w stanie zlokalizować aktora w klastrze (Znajdź węzeł klaster, w której jest ona hostowana).
    * Błąd obsługi: go ponowić próbę wywołania metod i ponownie rozpoznać lokalizacji Aktor po, na przykład błąd, który wymaga Aktor do przeniesiona do innego węzła w klastrze.

Następujące reguły, które dotyczą interfejsów Aktor są warte tworzenie wzmianki o:

- Nie można obciążać metod interfejsu aktora.
- Interfejs aktora, który się nie może mieć metody, ref lub parametry opcjonalne.
- Rodzajowy interfejsów nie są obsługiwane.

## <a name="create-an-actor-service"></a>Tworzenie usługi Aktor
Zacznij od utworzenia nowej aplikacji usługi tkaninie. Usługa SDK tkaninie Linux zawiera Yeoman generator zapewnienie rusztowania dla aplikacji usługi tkaninie usłudze bezstanowym. Uruchom, uruchamiając Yeoman następujące polecenie:

```bash
$ yo azuresfjava
```

Postępuj zgodnie z instrukcjami, aby utworzyć **Zaufanego usługi aktora**. Ten samouczek nadać nazwę aplikacji "HelloWorldActorApplication" i aktor "HelloWorldActor." Zostanie utworzona rusztowania następujące czynności:

```bash
HelloWorldActorApplication/
├── build.gradle
├── HelloWorldActor
│   ├── build.gradle
│   ├── settings.gradle
│   └── src
│       └── reliableactor
│           ├── HelloWorldActorHost.java
│           └── HelloWorldActorImpl.java
├── HelloWorldActorApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldActorPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   ├── _readme.txt
│       │   └── Settings.xml
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── HelloWorldActorInterface
│   ├── build.gradle
│   └── src
│       └── reliableactor
│           └── HelloWorldActor.java
├── HelloWorldActorTestClient
│   ├── build.gradle
│   ├── settings.gradle
│   ├── src
│   │   └── reliableactor
│   │       └── test
│   │           └── HelloWorldActorTestClient.java
│   └── testclient.sh
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="reliable-actors-basic-building-blocks"></a>Niezawodne aktorów podstawowe bloki konstrukcyjne

Podstawowe pojęcia opisanych wcześniej przekształcić podstawowe elementy składowe usługi zaufanego aktora.

### <a name="actor-interface"></a>Interfejs Aktor

Ta strona zawiera definicji interfejsu aktora. Ten interfejs definiuje Umowa aktora, która mają być udostępniane przez implementacji aktor i klientów, dzwoniąc Aktor, zwykle warto zdefiniować go w miejsce, w którym różni się od wykonania aktor i może być współużytkowany przez wielu innych usług lub aplikacji klienckich.

`HelloWorldActorInterface/src/reliableactor/HelloWorldActor.java`:

```java
public interface HelloWorldActor extends Actor {
    @Readonly   
    CompletableFuture<Integer> getCountAsync();

    CompletableFuture<?> setCountAsync(int count);
}
```

### <a name="actor-service"></a>Usługa Aktor 
Ta strona zawiera do wykonania aktor i kodu rejestracji aktora. Klasa aktora wykonuje interfejs aktora. Jest to miejsce, w którym usługi Aktor działa jej.

`HelloWorldActor/src/reliableactor/HelloWorldActorImpl`:

```java
@ActorServiceAttribute(name = "HelloWorldActor.HelloWorldActorService")
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
public class HelloWorldActorImpl extends ReliableActor implements HelloWorldActor {
    Logger logger = Logger.getLogger(this.getClass().getName());

    protected CompletableFuture<?> onActivateAsync() {
        logger.log(Level.INFO, "onActivateAsync");

        return this.stateManager().tryAddStateAsync("count", 0);
    }

    @Override
    public CompletableFuture<Integer> getCountAsync() {
        logger.log(Level.INFO, "Getting current count value");
        return this.stateManager().getStateAsync("count");
    }

    @Override
    public CompletableFuture<?> setCountAsync(int count) {
        logger.log(Level.INFO, "Setting current count value {0}", count);
        return this.stateManager().addOrUpdateStateAsync("count", count, (key, value) -> count > value ? count : value);
    }
}
```

### <a name="actor-registration"></a>Rejestracja Aktor

Usługa Aktor musi być zarejestrowana w typ usługi w czasie wykonywania tkaninie usługi. Aby usługę Aktor do uruchamiania wystąpień usługi aktora typu Aktor musi być zarejestrowany z usługą aktora. `ActorRuntime` Metoda rejestracji wykona tę czynność dla uczestników.

`HelloWorldActor/src/reliableactor/HelloWorldActorHost`:

```java
public class HelloWorldActorHost {
    
    public static void main(String[] args) throws Exception {
        
        try {
            ActorRuntime.registerActorAsync(HelloWorldActorImpl.class, (context, actorType) -> new ActorServiceImpl(context, actorType, ()-> new HelloWorldActorImpl()), Duration.ofSeconds(10));

            Thread.sleep(Long.MAX_VALUE);
            
        } catch (Exception e) {
            e.printStackTrace();
            throw e;
        }
    }
}
```

### <a name="test-client"></a>Klient testowy

To jest aplikacją kliencką prosty test może zostać uruchomiony oddzielnie z poziomu aplikacji tkaninie usługi, aby przetestować usługi aktora. Przykład: miejsce, w którym ActorProxy można aktywować i komunikować się z wystąpienia aktora. Uzyskiwanie nie wdrożone z tej usługi.

### <a name="the-application"></a>Aplikacja 

Na koniec aplikacji pakiety usługa aktor i inne usługi, które można dodać w przyszłości razem do wdrożenia. Zawiera on posiadacze *ApplicationManifest.xml* i umieszczanie pakietu z aktora.

## <a name="run-the-application"></a>Uruchamianie aplikacji

Yeoman rusztowania zawiera skrypt gradle do tworzenia aplikacji i urodzinową skrypty do wdrożenia i wyczyść-wdrożyć aplikację. Aby uruchomić aplikację, należy najpierw utworzyć aplikację z gradle:

```bash
$ gradle
```

Spowoduje to pakiet aplikacji tkaninie usług, które mogą być wdrażane za pomocą usługi tkaninie Azure interfejsu wiersza polecenia. Skrypt install.sh zawiera niezbędne polecenia polecenie Azure, aby wdrożyć pakiet aplikacji. Po prostu uruchom skrypt install.sh do wdrażania:

```bask
$ ./install.sh
```
