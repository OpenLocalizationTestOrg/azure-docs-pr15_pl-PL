<properties
   pageTitle="Polimorfizm w ramach zaufanego aktorów | Microsoft Azure"
   description="Tworzenie hierarchii interfejsów .NET i typów w ramach zaufanego uczestników do ponownego użycia funkcji i definicje interfejsu API."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/07/2016"
   ms.author="seanmck"/>

# <a name="polymorphism-in-the-reliable-actors-framework"></a>Polimorfizm w ramach zaufanego uczestników

Struktura zaufanego uczestników umożliwia tworzenie uczestników przy użyciu wielu tych samych metod, których można użyć w projekcie obiektowych. Jedną z tych metod jest polimorfizm, dzięki któremu typów, a interfejsy odziedziczone więcej ogólnych elementów nadrzędnych. Dziedziczenie w ramach zaufanego aktorów zwykle wykonuje modelu .NET przy użyciu kilku dodatkowe ograniczenia.

## <a name="interfaces"></a>Interfejsy

Framework zaufanego aktorów wymaga zdefiniowania co najmniej jeden interfejs do wykonania przez użytkownika typ aktora. Ten interfejs jest używany do generowania klasy proxy, które mogą być używane przez klientów na komunikowanie się z uczestnikami. Interfejsy może dziedziczyć inne interfejsy, jak każdy interfejs, który jest wykonywane przez typ aktora i wszystkich nadrzędnych ostatecznie pochodzić od IActor. IActor jest zdefiniowane platformy interfejs podstawowy dla uczestników. W związku z tym przykładzie klasyczny polimorfizm za pomocą kształtów może wyglądać następująco:

![Hierarchia interfejsu dla uczestników kształtu][shapes-interface-hierarchy]


## <a name="types"></a>Typy

Można także tworzyć hierarchii typów aktora, które pochodzą od klasy Aktor podstawowej dostarczonego przez platformę. W przypadku kształtów, może być przy podstawie `Shape` typu:

```csharp
public abstract class Shape : Actor, IShape
{
    public abstract Task<int> GetVerticeCount();

    public abstract Task<double> GetAreaAsync();
}
```

Subtypes z `Shape` można zastąpić metody od podstawy.

```csharp
[ActorService(Name = "Circle")]
[StatePersistence(StatePersistence.Persisted)]
public class Circle : Shape, ICircle
{
    public override Task<int> GetVerticeCount()
    {
        return Task.FromResult(0);
    }

    public override async Task<double> GetAreaAsync()
    {
        CircleState state = await this.StateManager.GetStateAsync<CircleState>("circle");

        return Math.PI *
            state.Radius *
            state.Radius;
    }
}
```

Uwaga `ActorService` atrybut typ aktora. Ten atrybut instruuje framework zaufanego aktora, że automatycznie należy utworzyć usługę hostingu uczestników tego typu. W niektórych przypadkach może być wymagane utworzenie typu podstawowego są wyłącznie przeznaczone do udostępniania funkcji podtypy, która nigdy nie są używane do wystąpienia konkretnych uczestników. W tych przypadkach należy używać `abstract` słowo kluczowe, aby wskazać, nigdy nie utworzysz aktor na podstawie tego typu.


## <a name="next-steps"></a>Następne kroki

- Zobacz, [jak framework zaufanego aktorów wykorzystuje platformy tkaninie usługi](service-fabric-reliable-actors-platform.md) , aby zapewnić niezawodność, skalowalność i spójna.
- Więcej informacji na temat [cyklu życia aktora](service-fabric-reliable-actors-lifecycle.md).

<!-- Image references -->

[shapes-interface-hierarchy]: ./media/service-fabric-reliable-actors-polymorphism/Shapes-Interface-Hierarchy.png
