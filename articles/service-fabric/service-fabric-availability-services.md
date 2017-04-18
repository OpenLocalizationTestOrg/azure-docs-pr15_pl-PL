<properties
   pageTitle="Dostępność usług tkaninie usługi | Microsoft Azure"
   description="W tym artykule opisano wykrywanie błędów, pracy awaryjnej i odzyskiwanie usługi"
   services="service-fabric"
   documentationCenter=".net"
   authors="appi101"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"
   ms.author="aprameyr"/>

# <a name="availability-of-service-fabric-services"></a>Dostępność usług tkaninie usługi
Azure usługi tkaninie usługi mogą być stanowe lub bezpaństwowca. Ten artykuł zawiera omówienie jak usługa tkaninie przechowuje dostępność usługi wypadku awarii.

## <a name="availability-of-service-fabric-stateless-services"></a>Dostępność usług stateless tkaninie usługi
Bezstanowa jest usługą aplikacji, która nie ma każde [lokalne trwały stan](service-fabric-concepts-state.md).

Tworzenie stateless usługi wymaga definiowania licznik wystąpień, czyli liczbę wystąpień stateless usługa, która powinna być uruchomiona w klastrze. To jest liczba kopii logiki aplikacji, która będzie występować w klastrze. Zwiększenie liczby wystąpień jest zalecany sposób skalowania stateless usługi.

Gdy błąd zostanie wykryty na dowolne wystąpienie stateless usługi, nowego wystąpienia jest tworzony na inne uprawniony węzła w klastrze.

## <a name="availability-of-service-fabric-stateful-services"></a>Dostępność usług stanowe tkaninie usługi
Usługa stanowe ma niektórych stan skojarzone z nim. Na tkaninie usługi stanowe usługi modelu jako zestaw replik. Każdej replice jest wystąpieniem kodu usługa, która ma kopię stanu. Operacje odczytu i zapisu są wykonywane w jednej replice (nazywanych podstawowy). Zmiany stanu z zapisu operacje będą *replikować* do wielu innych replik (zwanych aktywne pomocnicze). Kombinacja repliki pomocniczej podstawowego i aktywne jest zestaw replice usługi.

Może istnieć tylko jeden podstawowy replice obsługi żądania odczytu i zapisu, ale może być wiele aktywnych replik pomocniczą. Liczba aktywnych replik pomocniczej jest konfigurowany i większej liczby replik przeszkadzają większą liczbę równoczesne oprogramowania i awarii sprzętu.

W przypadku błędów (w przypadku awarii podstawowego replice) tkaninie usługi sprawia, że jeden z aktywnych replik pomocniczej nowe replice podstawowego. Tej replice pomocniczą active ma już zaktualizowanej wersji stan (przy użyciu *replikacji*) i można kontynuować przetwarzanie dalszych odczytu i operacji zapisu.

Ten pojęcia — replice podstawowego lub aktywne pomocniczego — nosi nazwę roli replice.

### <a name="replica-roles"></a>Role replice
Rola replice jest używana do zarządzania cyklu życia stan zarządzany, że w replice. W tym artykule replice, który ma rolę podstawowe usługi żądania. Aktualizowanie stanu i replikacji zmian do aktywnego pomocnicze w zestawie jego replice są też usługi żądań zapisu. Rola aktywne pomocnicze jest odbieranie zmian stanu, które ma replikować podstawowego replice i zaktualizować swoją opinię stanu.

>[AZURE.NOTE] Wyższego poziomu modelach programowania, takich jak [framework zaufanego aktorów](service-fabric-reliable-actors-introduction.md) z dala od komputera abstrakcyjne pojęcia roli replice przez dewelopera.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na tkaninie usługi pojęcia zobacz:

- [Skalowalność usług tkaninie usługi](service-fabric-concepts-scalability.md)

- [Podziału usług tkaninie usługi](service-fabric-concepts-partitioning.md)

- [Definiowanie i stan zarządzania](service-fabric-concepts-state.md)
