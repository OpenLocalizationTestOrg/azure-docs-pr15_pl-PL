<properties
   pageTitle="Planowanie dla aplikacji usługi tkaninie pojemności | Microsoft Azure"
   description="W tym artykule opisano sposób identyfikowania liczby węzłów obliczeń wymagane dla aplikacji usługi tkaninie"
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="markfuss"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>


# <a name="capacity-planning-for-service-fabric-applications"></a>Planowanie dla aplikacji usługi tkaninie pojemności


Ten dokument przedstawiono sposób oszacować ilość zasobów (procesory, RAM, miejsca) jest potrzebne do uruchamiania aplikacji tkaninie usługi Azure. Są często zapotrzebowania na zasoby zmiany w czasie. Można opracowywać/testowanie usługi, a następnie wymaga większej ilości zasobów, przejdź do produkcji i aplikacja rozwoju w popularności zwykle wymagają niewielkiej ilości zasobów. Podczas projektowania aplikacji Pomyśl za pośrednictwem długoterminowe wymagania i wybrać opcje umożliwiające usługi przeskalować do zapotrzebowania wysoka klienta.

 Po utworzeniu klastrze tkaninie usługi możesz określić, jakie rodzaje wirtualnych maszyn składające się na klaster. Każdy maszyn wirtualnych zawiera ograniczoną ilość zasobów w postaci procesorów (rdzenie i szybkości), przepustowości sieci, pamięci RAM i ilość miejsca do magazynowania. W miarę usługi w czasie, można też uaktualnić do maszyny wirtualne, które oferują większe zasoby i/lub dodać więcej maszyny wirtualne do klaster. Aby wykonać te ostatnie, możesz musi zaprojektować usługi początkowo dzięki go można korzystać z nowej maszyny wirtualne dynamicznie uzyskać dodane do klaster.

Niektóre usługi Zarządzanie nieco do żadnych dodatkowych danych o maszyny wirtualne się. W związku z tym planowanie tych usług pojemności należy skoncentrować się przede wszystkim na wydajność, co oznacza, wybierając odpowiednie procesory maszyny wirtualne (rdzenie i szybkości). Ponadto należy rozważyć przepustowości sieci, w tym, jak często występują przeniesienia sieci i ile dane są przesyłane. Jeśli usługi musi wykonać również wraz ze wzrostem zastosowania usługi, możesz dodać więcej maszyny wirtualne bilansu klaster i obciążenie sieci żądania przez maszyny wirtualne.

Dla usług, które zarządzają dużych ilości danych na maszyny wirtualne Planowanie wydajności należy skoncentrować się przede wszystkim na rozmiar. W związku z tym należy rozważyć wydajność RAM maszyn wirtualnych i ilość miejsca do magazynowania. Pamięć wirtualna systemu zarządzania w systemie Windows powoduje, że miejsca na dysku wyglądać pamięci RAM dla kod aplikacji. Ponadto runtime tkaninie usługi zawiera inteligentne stronicowania przechowywanie tylko ciepłej danych w pamięci i przenoszenie zimnej danych na dysku. Aplikacje, a więc mogą używać więcej pamięci niż jest fizycznie dostępna na maszyn wirtualnych. Po prostu o większej ilości pamięci RAM zwiększa wydajność, ponieważ maszyn wirtualnych można przechowywać więcej miejsca w pamięci RAM. Maszyn wirtualnych, które można wybrać powinien mieć dysku mała, aby przechowywać dane, które mają na maszyn wirtualnych. Podobnie maszyn wirtualnych powinny mieć za mało pamięci RAM do zapewnienia wydajności, oczekiwane. Jeśli ilości danych przez usługę w czasie, możesz dodać więcej maszyny wirtualne klaster i partycją danych przez maszyny wirtualne.

## <a name="determine-how-many-nodes-you-need"></a>Określanie liczby węzłów potrzebne

Podział usługi umożliwia skalowania danych tej usługi. Aby uzyskać więcej informacji dotyczących podziału zobacz [Podziału tkaninie usługi](service-fabric-concepts-partitioning.md). Każdy partition musi mieszczą się pojedynczego maszyn wirtualnych, ale w jednym maszyn wirtualnych można umieścić wiele partycje (mały). Tak o kolejne partycje małych zapewnia większą elastyczność niż w przypadku kilka partycje większe. Rozwiązanie jest wiele partycje zwiększa tkaninie usług ogólnych, a nie można wykonywać operacje transakcji w partycje. Istnieje także więcej potencjalnych ruch sieciowy, jeśli często kod usługi musi uzyskać dostęp do części danych, które są przechowywane w różne partycje. Podczas projektowania usługi, należy więc rozważyć następujące wad i zalet na efektywnej strategii podziału.

Załóżmy, że aplikacja ma pojedynczą usługą stanowe, która ma rozmiar magazynu, który będzie rosnąć do DB_Size GB w roku. Chcesz dodać więcej aplikacji (i partycje) jako możliwości wzrostu poza roku.  Współczynnik replikacji (Rosyjskiej), który określa liczbę repliki tej usługi wpływa na całkowity DB_Size. Suma DB_Size przez wszystkich replik jest współczynnik replikacji pomnożoną przez DB_Size.  Node_Size reprezentuje miejsca na dysku i RAM na węzeł, który ma być używany dla usługi. Dla uzyskania najlepszej wydajności należy DB_Size należy zmieściło się w pamięci w klastrze, a Node_Size, który jest wokół pamięci RAM maszyn wirtualnych powinna być wybrana. Przydzielając Node_Size, którego rozmiar przekracza pojemność pamięci RAM są Polegaj na stronicowanie dostarczony przez środowisko uruchomieniowe usługi tkaninie. W związku z tym wydajność może nie być optymalny, jeśli cały danych jest uważany ciepłej (od tego momentu danych jest stronicowanej/się). Dla wielu usług, w przypadku ciepłej tylko część danych, jest jednak redukcji kosztów.

Liczby węzłów wymagane dla maksymalnej wydajności można obliczyć w następujący sposób:

```
Number of Nodes = (DB_Size * RF)/Node_Size

```


## <a name="account-for-growth"></a>Konto w usłudze wzrostu

Być może zechcesz obliczenia liczby węzłów na DB_Size, której oczekujesz usługi w celu usprawnienia, oprócz DB_Size, rozpoczęcia z podstawie. Następnie powiększ liczby węzłów w miarę usługi, tak aby nie są zbyt inicjowanie liczby węzłów. Ale liczbę partycje powinny być oparte na liczbę węzłów, które są potrzebne w razie korzystania z tej usługi na maksymalną wzrostu.

Warto mieć kilka dodatkowych komputerach dostępne w dowolnym momencie, tak aby można obsługiwać ani tych najwyższych wartościach nieoczekiwany błąd (na przykład kilka maszyny wirtualne Przejdź w dół).  Dodatkowa pojemność należy określić przy użyciu usługi oczekiwanych tych najwyższych wartościach, punkt początkowy jest zarezerwować kilka dodatkowych maszyny wirtualne (5-10 procent dodatkowe).

Poprzedni zakłada jednej usługi stanowe. Jeśli masz więcej niż jedną usługę stanowe, należy dodać DB_Size skojarzone z innych usług do równania. Ponadto można obliczyć liczby węzłów osobno dla każdej usługi stanowe.  Twoja usługa może być replik lub partycje, które nie są strategie. Należy pamiętać, że partycje mogą mieć także większej ilości danych niż inne. Aby uzyskać więcej informacji dotyczących podziału zobacz [artykuł na temat najlepszych rozwiązań podziału](service-fabric-concepts-partitioning.md). Jednak poprzedniego Równanie jest partycją i replice o niesprecyzowanym, ponieważ usługa tkaninie sprawia, że repliki są rozmieszczone między węzły w sposób zoptymalizowany.


## <a name="use-a-spreadsheet-for-cost-calculation"></a>Za pomocą arkusza kalkulacyjnego obliczania kosztów

Teraz Przyjrzyjmy umieścić kilka liczb rzeczywistych w formule. [Arkusz kalkulacyjny przykładzie](https://servicefabricsdkstorage.blob.core.windows.net/publicrelease/SF%20VM%20Cost%20calculator-NEW.xlsx) pokazano, jak planowanie możliwości dla aplikacji, która zawiera trzy typy obiektów danych. Dla każdego obiektu możemy zbliżenie jego rozmiar i ile obiektów, że oczekujemy mają. Możemy również zaznaczyć repliki ilu chcemy każdego typu obiektu. Arkusz kalkulacyjny oblicza łączną liczbę pamięci mają być przechowywane w grupie.

Firma Microsoft wprowadź rozmiar pamięci Wirtualnej i koszt miesięczny. W zależności od rozmiar pamięci Wirtualnej, arkusz kalkulacyjny zawiera informację, minimalna liczba partycje, które należy użyć dzielenia danych w celu dopasowania fizycznie w węzłach. Może być wymagane większą liczbą partycje tak, aby zezwalały obliczeń określonej aplikacji i musi ruch sieciowy. Arkusz kalkulacyjny jest wyświetlana liczba partycje, które są zarządzanie obiektami profilu użytkownika ma zwiększona od jednego do 6.

Teraz w zależności od tych informacji, arkusz kalkulacyjny zawiera można fizycznie pobrać wszystkie dane z odpowiednim partycje i repliki w klastrze węzeł 26. Jednak klaster czy gęsto zapewniać, więc możesz kilka dodatkowych węzłów, aby zezwalały na awarii węzłów i aktualizacje. Arkusz kalkulacyjny zawiera również, że masz więcej niż 57 węzły zapewnia żadne dodatkowe wartości, ponieważ wymaga węzły puste. Ponownie można znaleźć się wyżej węzły 57 mimo to, aby zezwalały na awarii węzłów i aktualizacje. Można dostosować a arkusza kalkulacyjnego w celu odpowiada wymaganiom aplikacji.   

![Arkusz kalkulacyjny obliczania kosztów][Image1]



## <a name="next-steps"></a>Następne kroki

Zapoznaj się z [usługami podziału tkaninie usługi] [ 10] Aby dowiedzieć się więcej o partycjonowaniu tej usługi.



<!--Image references-->
[Image1]: ./media/SF-Cost.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-concepts-partitioning.md
