<properties
   pageTitle="Niezawodne zdarzeń aktorów | Microsoft Azure"
   description="Wprowadzenie do zdarzeń dla usługi tkaninie zaufanego aktorów."
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
   ms.date="08/30/2016"
   ms.author="amanbha"/>


# <a name="actor-events"></a>Aktor zdarzenia
Zdarzenia Aktor umożliwiają wysyłania powiadomień optymalny z Aktor do klientów. Zdarzenia Aktor są przeznaczone dla komunikacji Aktor do klienta i nie może być używana dla komunikacji Aktor do aktora.

Następujące fragmenty kodu przedstawiono sposoby używania Aktor zdarzenia w aplikacji.

Definiowanie interfejs, który opisuje zdarzenia opublikowane przez aktora. Ten interfejs musi pochodzić z `IActorEvents` interfejsu. Argumenty metod muszą być [można kontraktu danych](service-fabric-reliable-actors-notes-on-actor-type-serialization.md). Metody musi zwrotów, jako zdarzenie powiadomienia są jednym ze sposobów i starań.

```csharp
public interface IGameEvents : IActorEvents
{
    void GameScoreUpdated(Guid gameId, string currentScore);
}
```

Zadeklaruj zdarzeń publikowane przez aktora w interfejsie aktora.

```csharp
public interface IGameActor : IActor, IActorEventPublisher<IGameEvents>
{
    Task UpdateGameStatus(GameStatus status);

    Task<string> GetGameScore();
}
```

Po stronie klienta wdrożenie obsługi zdarzeń.

```csharp
class GameEventsHandler : IGameEvents
{
    public void GameScoreUpdated(Guid gameId, string currentScore)
    {
        Console.WriteLine(@"Updates: Game: {0}, Score: {1}", gameId, currentScore);
    }
}
```

Na komputerze klienckim Utwórz serwer proxy aktora publikującej wydarzenie i subskrybowanie zdarzeń jej.

```csharp
var proxy = ActorProxy.Create<IGameActor>(
                    new ActorId(Guid.Parse(arg)), ApplicationName);

await proxy.SubscribeAsync<IGameEvents>(new GameEventsHandler());
```

W przypadku pracy awaryjnej Aktor może się nie powieść przez inny proces lub węzeł. Serwer proxy Aktor zarządza aktywne subskrypcje i automatycznie ponownie subskrybowane przez ich. Możesz sterować interwał ponownego subskrypcji za pośrednictwem `ActorProxyEventExtensions.SubscribeAsync<TEvent>` interfejsu API. Aby anulować subskrypcję, za pomocą `ActorProxyEventExtensions.UnsubscribeAsync<TEvent>` interfejsu API.

Na Aktor po prostu Publikuj zdarzenia po ich wprowadzeniu. W przypadku subskrybentów zdarzenie środowisko uruchomieniowe aktorów będzie wysyłać je powiadomienie.

```csharp
var ev = GetEvent<IGameEvents>();
ev.GameScoreUpdated(Id.GetGuidId(), score);
```

## <a name="next-steps"></a>Następne kroki
 - [Aktor ponownego rozpoczęcia](service-fabric-reliable-actors-reentrancy.md)
 - [Narzędzia diagnostyczne aktor i monitorowanie wydajności](service-fabric-reliable-actors-diagnostics.md)
 - [Aktor interfejsu API dokumentacji](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Przykładowy kod](https://github.com/Azure/servicefabric-samples)
