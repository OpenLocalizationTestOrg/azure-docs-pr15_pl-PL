<properties
   pageTitle="Planowanie pojemności klaster tkaninie usługa | Microsoft Azure"
   description="Zagadnienia związane z planowaniem wydajność klaster tkaninie usługi. Nodetypes, wytrzymałości i niezawodności warstwy"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="chackdan"/>


# <a name="service-fabric-cluster-capacity-planning-considerations"></a>Zagadnienia związane z planowaniem zdolności klaster tkaninie usługi

Dla każdego wdrożenia produkcji planowania pojemności jest ważny krok. Poniżej przedstawiono kilka elementów, które należy wziąć pod uwagę w ramach tego procesu.

- Liczba typy węzłów, które klaster musi zaczynać
- Właściwości każdego typu węzeł (rozmiar, podstawową, internet przeciwległych liczba maszyny wirtualne itd.)
- Cech niezawodności i wytrzymałości klaster

Pozwól nam krótko przejrzeć każdy z tych elementów.

## <a name="the-number-of-node-types-your-cluster-needs-to-start-out-with"></a>Liczba typy węzłów, które klaster musi zaczynać

Najpierw musisz obliczanie klaster tworzonego przejścia ma być używany dla i jakie typy aplikacji zamierzasz wdrożyć w tym klastrze. Jeśli nie jesteś wyczyść w celu klaster, możesz prawdopodobnie nie została jeszcze gotowe do wprowadź wydajność procesu planowania.

Ustalić liczbę typów węzeł, który klaster musi zaczynać.  Każdego typu węzła są mapowane na zestaw skali maszyn wirtualnych. Każdego typu węzła można następnie skalować w górę lub w dół niezależnie od tego, mają różne zestawy otwarte porty i nie może mieć inną pojemność metryki. Dlatego decyzja liczbę typów węzeł zasadniczo przychodzące w dół następujące kwestie:

- Aplikacja ma wiele usług, i jedną z nich, muszą być publiczna lub internetową? Typowe aplikacje zawierają odbierająca dane wejściowe w kliencie usługi frontonu bramy i jeden lub więcej wewnętrznej usług, które komunikować się z usługami zewnętrzną. Aby w tym przypadku na końcu mających co najmniej dwa typy węzłów.

- Usług (które składają się na aplikację) mają różne infrastruktury musi takich jak większa pamięci RAM lub nowszy procesor? Na przykład Powiadom nas założono, że aplikację, którą chcesz wdrożyć zawiera usługi frontonu i wewnętrznej. Usługa zewnętrzną można uruchamiać na mniejszych maszyny wirtualne (maszyn wirtualnych rozmiarów takich jak D2), zawierających porty Otwórz z Internetem.  Usługi wewnętrznej, jednak jest wymagające obliczenia i należy uruchomić na większym maszyny wirtualne (o rozmiarze maszyn wirtualnych, takich jak D15 D4, D6,), które nie są internet przeciwległych.

 W tym przykładzie mimo że możesz zdecydować umieścić wszystkich usług dla jednego węzła typu zaleca się umieszczać je w klastrze z dwoma typami węzeł.  Dzięki temu dla każdego typu węzła mają różne właściwości, takie jak łączność z Internetem lub rozmiar pamięci Wirtualnej. Liczba maszyny wirtualne można skalować niezależnie od siebie, jak również.  

- Ponieważ nie można przewidzieć przyszłości, przejdź z fakty, które znasz i określ typy węzłów, które aplikacje muszą zaczynać się od liczby. Można zawsze dodać lub usunąć typy węzłów w późniejszym czasie. Klaster tkaninie usługa musi mieć co najmniej jeden węzeł typ.

## <a name="the-properties-of-each-node-type"></a>Właściwości każdego typu węzła

**Typ węzła** są widoczne jako równoważna ról w usług w chmurze. Typy węzłów Definiowanie rozmiarów maszyn wirtualnych, liczba maszyny wirtualne i ich właściwości. Każdego typu węzeł, która jest zdefiniowana w klastrze tkaninie usługi jest skonfigurowana jako osobne zestaw maszyn wirtualnych Skala. Zestawy skali maszyn wirtualnych są Azure obliczyć zasób, który umożliwia wdrażanie i zarządzanie zbiorem maszyn wirtualnych jako zestaw. Został określony jako różne zestawy skali maszyn wirtualnych, każdego typu węzła można następnie skalować w górę lub w dół niezależnie od tego, mają różne zestawy otwarte porty i nie może mieć inną pojemność metryki.

Klaster mogą mieć więcej niż jeden typ węzeł, ale typ podstawowy węzeł (pierwszego podany w portalu) musi mieć co najmniej pięć maszyny wirtualne dla klastrów na potrzeby produkcji obciążenia (lub co najmniej trzy maszyny wirtualne dla klastrów test). Jeśli tworzysz klaster przy użyciu szablonu Menedżera zasobów spowoduje znalezienie atrybutem **jest główny** w obszarze Definicja typu węzeł. Typ główny węzeł jest typ węzła rozmieszczenie usług systemowych tkaninie usługi.  

### <a name="primary-node-type"></a>Typ węzła podstawowego
Klaster z różnymi typami węzeł będzie konieczne wybierz jedną z nich podstawowy. Poniżej przedstawiono właściwości Typ podstawowy węzła:

- Minimalny rozmiar maszyny wirtualne dla danego typu główny węzeł zależy od poziomu wytrzymałości, wybrane. Wartość domyślna dla poziomu wytrzymałości to brązowy. Przewiń w dół do szczegółowych informacji na temat co to jest warstwie wytrzymałości i wartości, które można wykonać.  

- Minimalna liczba maszyny wirtualne dla danego typu główny węzeł zależy od poziomu niezawodności, wybrane. Wartość domyślna dla poziomu niezawodności to Silver. Przewiń w dół do szczegółowych informacji na temat co to jest warstwie niezawodności i wartości, które można wykonać.

- Usługi systemowe tkaninie usługi (na przykład usługa Menedżera klaster lub Usługa magazynu obraz) są umieszczane w typ węzła podstawowy, a więc niezawodności i ważności klaster jest określona przez wartość poziomu niezawodności i wartość warstwa wytrzymałości, wybrane dla typu węzła podstawowego.

![Zrzut ekranu przedstawiający klastrze występują dwa typy węzłów ][SystemServices]


### <a name="non-primary-node-type"></a>Typ węzła nie podstawowa
Klaster z różnymi typami węzeł jest typu główny węzeł i pozostała ich jest inne niż podstawowe. Poniżej przedstawiono właściwości Typ węzła inne niż podstawowe:

- Minimalny rozmiar maszyny wirtualne dla tego typu węzeł zależy od poziomu wytrzymałości, wybrane. Wartość domyślna dla poziomu wytrzymałości to brązowy. Przewiń w dół do szczegółowych informacji na temat co to jest warstwie wytrzymałości i wartości, które można wykonać.  

- Minimalna liczba maszyny wirtualne dla tego typu węzeł może być jedną. Jednak należy wybrać ten numer na podstawie liczby replik aplikacji i usług, które chcesz uruchomić w ten typ węzła. Numer w polu Typ węzeł maszyny wirtualne można zwiększyć po wdrożeniu klaster.


## <a name="the-durability-characteristics-of-the-cluster"></a>Właściwości wytrzymałości klaster

Warstwa wytrzymałości jest używana w celu wskazania systemowi uprawnień, które mają pośrednictwem usługi SMS z podstawowej infrastruktury Azure. W polu wpisz podstawowy węzeł to uprawnienie umożliwia tkaninie usługę wstrzymać dowolnego maszyn wirtualnych poziomu infrastruktury żądania (na przykład maszyn wirtualnych Uruchom ponownie komputer, reimage maszyn wirtualnych lub migracji maszyn wirtualnych) wpływają na przepisów dla usługi systemowe i stanowe usług. W polu typy węzeł inne niż podstawowe to uprawnienie umożliwia tkaninie usługi wstrzymać dowolnego żądania poziomu infrastruktury maszyn wirtualnych maszyn wirtualnych Uruchom ponownie komputer, reimage maszyn wirtualnych, migracji maszyn wirtualnych itd., które wpływają na przepisów dla usług stanowe uruchomiony w nim.

To uprawnienie jest wyrażona w następujących wartości:

- Złoty - zadania infrastruktury może być wstrzymana czas trwania 2 godziny na UD

- Może być wstrzymana Silver - zadania infrastruktury o czasie trwania 30 minut na UD (to obecnie nie jest włączony do użycia. Raz włączone to będzie dostępny dla wszystkich pośrednictwem standardowej SMS i pojedynczy core).

- Brązowy — nie uprawnień. Jest to wartość domyślna.

## <a name="the-reliability-characteristics-of-the-cluster"></a>Właściwości niezawodności klaster

Poziom niezawodności służy do ustawiania liczba replik usług systemowych, które chcesz uruchomić w tym klastrze dla podstawowego węzła typu. Więcej liczby replik, bardziej niezawodne usługi systemowe są w klastrze.  

Poziom niezawodności można wykonać następujące wartości.

- Platinum - uruchamianie usługi systemowe z elementu docelowego replice Ustaw liczbę 9

- Złoty - uruchamianie usługi systemowe z elementu docelowego replice Ustaw liczbę z 7

- Silver - uruchamianie usługi systemowe z elementu docelowego replice Ustawianie liczby 5

- Brązowy — uruchamianie usługi systemowe z elementu docelowego replice Ustaw liczbę 3

>[AZURE.NOTE] Warstwie niezawodności wybranym określa minimalną liczbę węzły, które musi być typu główny węzeł. Poziom niezawodności ma żadnego wpływu na maksymalny rozmiar klaster. Można więc klastrze węzeł 20 uruchomionym na brązowy niezawodności.

 Możesz zaktualizować niezawodności klaster z jednej warstwy do drugiego. Spowoduje to spowoduje uaktualnienie klaster potrzeby zmienić replice usług systemu ustaw liczbę. Poczekaj, aż uaktualnienie w toku do wykonania przed wprowadzeniem jakichkolwiek innych zmian z klastrem, takie jak dodawanie węzły itp.  Można monitorować postęp uaktualnienia usługi tkaninie Eksploratora lub uruchamiając [Get-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt126012.aspx)

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Następne kroki

Po zakończeniu możliwości planowanie i konfigurowanie klastrze przeczytaj następujące czynności:
- [Usługa tkaninie klaster zabezpieczeń](service-fabric-cluster-security.md)
- [Wprowadzenie modelu kondycji usługi tkaninie](service-fabric-health-introduction.md)

<!--Image references-->
[SystemServices]: ./media/service-fabric-cluster-capacity/SystemServices.png
