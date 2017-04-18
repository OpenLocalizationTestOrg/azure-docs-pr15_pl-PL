<properties
   pageTitle="Niezawodne notatki uczestników Aktor wpisz szeregowania | Microsoft Azure"
   description="W tym artykule omówiono podstawowe wymagania dotyczące Definiowanie można klas, które mogą być używane do określania Państwa aktorów zaufanego tkaninie usługi i interfejsów"
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
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="notes-on-service-fabric-reliable-actors-type-serialization"></a>Wpisz notatki uczestników zaufanego tkaninie usługi szeregowania


Argumenty wszystkie metody, typy wyników zadania zwracane przez każdą z tych metod w interfejsie aktor i obiektów przechowywanych w Menedżerze stan Aktor musi być [Umowy dotyczącej danych można](https://msdn.microsoft.com/library/ms731923.aspx)... Dotyczy to również argumenty metody zdefiniowane w [interfejsów zdarzeń aktora](service-fabric-reliable-actors-events.md#actor-events). (Metod interfejsu zdarzenia Aktor zawsze zwrotów.)

## <a name="custom-data-types"></a>Niestandardowe typy danych

W tym przykładzie interfejsu Aktor definiuje metodę, która zwraca niestandardowego typu danych o nazwie `VoicemailBox`.

```csharp
public interface IVoiceMailBoxActor : IActor
{
    Task<VoicemailBox> GetMailBoxAsync();
}
```

Interfejs jest impelemented przez aktora, Menedżer stanu jest używana do przechowywania `VoicemailBox` obiektu:

```csharp
[StatePersistence(StatePersistence.Persisted)]
public class VoiceMailBoxActor : Actor, IVoicemailBoxActor
{
    public VoiceMailBoxActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<VoicemailBox> GetMailboxAsync()
    {
        return this.StateManager.GetStateAsync<VoicemailBox>("Mailbox");
    }
}

```

W tym przykładzie `VoicemailBox` obiekt jest seryjny podczas:
 - Obiekt jest przesyłany między wystąpienie aktor i rozmówcy.
 - Obiekt jest zapisywany w miejsce, w którym są zachowywane na dysku i replikować do innych węzłów w Menedżerze stan.
 
Struktura zaufanego aktora używa szeregowania DataContract. W związku z tym obiekty niestandardowych danych i ich członkowie musi być odnoszący przy użyciu atrybutów **DataContract** i **DataMember** odpowiednio

```csharp
[DataContract]
public class Voicemail
{
    [DataMember]
    public Guid Id { get; set; }

    [DataMember]
    public string Message { get; set; }

    [DataMember]
    public DateTime ReceivedAt { get; set; }
}
```

```csharp
[DataContract]
public class VoicemailBox
{
    public VoicemailBox()
    {
        this.MessageList = new List<Voicemail>();
    }

    [DataMember]
    public List<Voicemail> MessageList { get; set; }

    [DataMember]
    public string Greeting { get; set; }
}
```

## <a name="next-steps"></a>Następne kroki
 - [Cykl życia i śmieci zbioru Aktor](service-fabric-reliable-actors-lifecycle.md)
 - [Jaka aktor i przypomnienia](service-fabric-reliable-actors-timers-reminders.md)
 - [Aktor zdarzenia](service-fabric-reliable-actors-events.md)
 - [Aktor ponownego rozpoczęcia](service-fabric-reliable-actors-reentrancy.md)
 - [Polimorfizm aktor i desenie obiektowych projektu](service-fabric-reliable-actors-polymorphism.md)
 - [Narzędzia diagnostyczne aktor i monitorowanie wydajności](service-fabric-reliable-actors-diagnostics.md)
