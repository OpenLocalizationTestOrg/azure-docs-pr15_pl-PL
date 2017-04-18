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
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/25/2016"
   ms.author="vturecek"/>

# <a name="getting-started-with-reliable-actors"></a>Wprowadzenie do zaufanego uczestników

> [AZURE.SELECTOR]
- [C# w systemie Windows](service-fabric-reliable-actors-get-started.md)
- [Java w systemie Linux](service-fabric-reliable-actors-get-started-java.md)

W tym artykule opisano podstawy aktorów zaufanego tkaninie usługi Azure i przeprowadzi Cię przez tworzenie, debugowanie i wdrażanie prostej aplikacji zaufanego aktora w programie Visual Studio.

## <a name="installation-and-setup"></a>Instalacja i Konfiguracja
Zanim zaczniesz, upewnij się, że masz środowisko projektowania tkaninie usługi konfigurowanie na tym komputerze.
Jeśli chcesz skonfigurować ją, zobacz szczegółowe informacje o tym, [jak skonfigurować środowisko projektowania](service-fabric-get-started.md).

## <a name="basic-concepts"></a>Podstawowe pojęcia
Aby rozpocząć pracę z zaufanego uczestników, wystarczy poznać kilka podstawowych pojęć związanych z:

 * **Usługa aktora**. Niezawodne uczestników są dostarczane w usługach zaufanego, które mogą być wdrażane w infrastrukturze tkaninie usługi. Wystąpienia Aktor są aktywowane w przypadku nazwaną usługę.
 
 * **Rejestracja aktora**. Jako z usługami zaufanego zaufanego aktora usługa musi być zarejestrowany na środowisko uruchomieniowe usługi tkaninie. Ponadto typ aktora musi być zarejestrowany na środowisko uruchomieniowe aktora.
 
 * **Interfejs aktora**. Interfejs aktor jest używany do definiowania jednoznacznie określonym interfejsu publicznego aktora. W terminologii modelu zaufanego aktora interfejs Aktor definiuje typy wiadomości, które mogą zrozumieć aktor i proces. Interfejs aktor jest używane przez innych uczestników i aplikacje klienckie do "Wyślij" (asynchroniczne) wiadomości aktora. Niezawodne aktorów mogą zawierać wiele interfejsy.
 
 * **Klasy ActorProxy**. Klasy ActorProxy jest używane w aplikacjach klienckich do wywołania metod udostępnianych za pośrednictwem interfejsu aktora. Klasa ActorProxy udostępnia dwie ważne funkcje:
    * Rozpoznawanie nazw: jest w stanie zlokalizować aktora w klastrze (Znajdź węzeł klaster, w której jest ona hostowana).
    * Błąd obsługi: go ponowić próbę wywołania metod i ponownie rozpoznać lokalizacji Aktor po, na przykład błąd, który wymaga Aktor do przeniesiona do innego węzła w klastrze.

Następujące reguły, które dotyczą interfejsów Aktor są warte tworzenie wzmianki o:

- Nie można obciążać metod interfejsu aktora.
- Interfejs aktora, który się nie może mieć metody, ref lub parametry opcjonalne.
- Rodzajowy interfejsów nie są obsługiwane.

## <a name="create-a-new-project-in-visual-studio"></a>Tworzenie nowego projektu w programie Visual Studio
Po zainstalowaniu narzędzia tkaninie usługi programu Visual Studio, można tworzyć nowe typy projektu. Nowe typy projektu są w **chmurze** kategorii okno dialogowe **Nowy projekt** .


![Narzędzia tkaninie usługi programu Visual Studio — nowego projektu][1]

W oknie dialogowym dalej możesz wybrać typ projektu, który chcesz utworzyć.

![Szablony projektów tkaninie usługi][5]

Dla projektu HelloWorld można użyć usługę aktorów zaufanego tkaninie usługi.

Po utworzeniu rozwiązanie powinna być widoczna następującą strukturę:

![Usługa tkaninie struktury projektu][2]

## <a name="reliable-actors-basic-building-blocks"></a>Niezawodne aktorów podstawowe bloków konstrukcyjnych

Typowe rozwiązania zaufanego aktorów składa się z trzech projektów:

* **Projekt aplikacji (MyActorApplication)**. To jest projekt, który pakiety wszystkich usług razem do wdrożenia. Zawiera skrypty *ApplicationManifest.xml* i programu PowerShell do zarządzania aplikacji.

* **Projekt interfejsu (MyActor.Interfaces)**. To jest projekt, który zawiera definicji interfejsu aktora. W programie project MyActor.Interfaces można zdefiniować interfejsów, które będą używane przez uczestników w rozwiązaniu. Interfejsów Aktor można zdefiniować w dowolnym projektu z dowolną nazwę jednak interfejs definiuje umowy Aktor udostępnianą wprowadzenia w życie aktor i klientów wywołujących Aktor, więc zwykle warto zdefiniować go w zestawie różni się od wykonania aktora, który może być współużytkowany przez wielu innych projektów.

```csharp
public interface IMyActor : IActor
{
    Task<string> HelloWorld();
}
```

* **Projekt usługi Aktor (MyActor)**. Jest to projektu można definiować usługa tkaninie usługa, która ma obsługiwać aktora. Zawiera on stosowania aktora. Implementacja aktor jest klasa pochodzi od typu podstawowego `Actor` i wykonuje interfejsy, które są zdefiniowane w programie project MyActor.Interfaces. Klasę aktora także zaimplementować konstruktora akceptującym `ActorService` wystąpienie i `ActorId` i przekazuje je do podstawy `Actor` zajęć. Umożliwia uruchomienie zależności konstruktora zależności platformy.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<string> HelloWorld()
    {
        return Task.FromResult("Hello world!");
    }
}
```

Usługa Aktor musi być zarejestrowana w typ usługi w czasie wykonywania tkaninie usługi. Aby usługę Aktor do uruchamiania wystąpień usługi aktora typu Aktor musi być zarejestrowany z usługą aktora. `ActorRuntime` Metoda rejestracji wykona tę czynność dla uczestników.

```csharp
internal static class Program
{
    private static void Main()
    {
        try
        {
            ActorRuntime.RegisterActorAsync<MyActor>(
                (context, actorType) => new ActorService(context, actorType, () => new MyActor())).GetAwaiter().GetResult();

            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ActorEventSource.Current.ActorHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

Jeśli masz tylko jedną definicję Aktor rozpocząć od nowego projektu w programie Visual Studio, rejestracji jest domyślnie dostępne w kodzie generowany przez program Visual Studio. Jeśli zdefiniujesz innych uczestników w usłudze, musisz dodać rejestracji Aktor przy użyciu:

```csharp
 ActorRuntime.RegisterActorAsync<MyOtherActor>();

```

> [AZURE.TIP] Środowisko uruchomieniowe usługi tkaninie aktorów emituje niektóre [liczniki wydajności i zdarzenia związane z metod aktora](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters). Są one przydatne narzędzia diagnostyczne i monitorowanie wydajności.


## <a name="debugging"></a>Debugowanie

Narzędzia tkaninie usługi programu Visual Studio obsługuje debugowania na komputerze lokalnym. Możesz rozpocząć sesji debugowania przez naciśnięcie klawisza F5. Tworzy programu Visual Studio (Jeśli to konieczne) pakietów. Również wdrożenie aplikacji na lokalnym klaster tkaninie usługi i dołącza debugowania.

Podczas procesu wdrażania można wyświetlić postęp w oknie **dane wyjściowe** .

![Okno dane wyjściowe debugowania tkaninie usługi][3]


## <a name="next-steps"></a>Następne kroki
 - [Jak korzystać z zaufanego aktorów platformy tkaninie usługi](service-fabric-reliable-actors-platform.md)
 - [Zarządzanie stanem Aktor](service-fabric-reliable-actors-state-management.md)
 - [Cykl życia i śmieci zbioru Aktor](service-fabric-reliable-actors-lifecycle.md)
 - [Aktor interfejsu API dokumentacji](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Przykładowy kod](https://github.com/Azure/servicefabric-samples)


<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject.PNG
[2]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-projectstructure.PNG
[3]: ./media/service-fabric-reliable-actors-get-started/debugging-output.PNG
[4]: ./media/service-fabric-reliable-actors-get-started/vs-context-menu.png
[5]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject1.PNG
