<properties
   pageTitle="Defragmentacja metryki w tkaninie usługi Azure | Microsoft Azure"
   description="Omówienie przy użyciu defragmentacji lub dokumentu jako strategię metryki na tkaninie usługi"
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="defragmentation-of-metrics-and-load-in-service-fabric"></a>Defragmentacja metryki i obciążenia tkaninie usługi
Menedżer zasobów usługi tkaninie klaster głównie dotyczy równoważenia w odniesieniu do dystrybucji obciążenia — upewniając się jednakowo wszystkie węzły w klastrze wykorzystane. Jest to zazwyczaj układ najbezpieczniejsza i smartest pod względem przeżywające błędy, ponieważ daje to pewność, że niezgodności danego nie zajmuje się kilka duży procent danej obciążenie pracą. Menedżer zasobów usługi tkaninie klaster obsługuje również innych strategii, co jest defragmentacji. Defragmentacji zwykle oznacza, że zamiast próby rozpowszechnianie wykorzystania metryki w klastrze, możemy faktycznie Staraj się Konsolidacja. Jest to fortunate odwracanie naszej strategii normalnego — zamiast optymalizowania klaster oparte na minimalizowanie średnie odchylenie standardowe metryczne obciążenia dla danej metryki, możemy uruchomić Optymalizowanie pod kątem wzrost odchylenie. Ale Dlaczego możesz chcieć tej strategii?

Dobrze Jeśli możesz już równomiernie Załaduj między węzły w klastrze następnie możesz zostały spożywany w górę niektóre zasoby, które węzły mają do oferowania. Zwykle nie jest to problem, ale czasami niektóre obciążenia tworzenie usług, które są wyjątkowo duże i używanie większość węzła — powiedz 75% do 95% zasobów węzła czy trafiać dedykowane wystąpienie jednej usługi lub replice. Nie jest to problem, podczas tworzenia usługi wymagającego do dołu klaster nadając miejsca na tym duże obciążenie pracą, ustaw o co wystąpić wykryje Menedżera zasobów klaster, ale w tym samym czasie tego obciążenie pracą muszą czekać do zaplanowania w klastrze.

Zakładając, że planowania nowego obciążenia jest zazwyczaj co najmniej nieco opóźnienie liter, jeśli firma Microsoft nie robić nic inaczej możemy czasami prawo bNiski przez te zwiększany Jeśli istnieje wiele usług i stanie się przemieszczać, szczególnie jeśli są zwykle duże obciążenie pracą w klastrze (i w związku z tym trwa dłużej poruszanie się w klastrze). W rzeczywistości możemy wyrażoną godziny utworzenia w symulacji, na podstawie danych rzeczywistych klaster, możemy pokazano który gdyby wystarczającą usług i klaster dość została wykorzystana, że tworzenie tych usług dużych czy spowolnieniu w dół i że firma Microsoft to poprawić, wprowadzając zasady metryki defragmentacji.

Podobnie jak plik tworzenia lub access można uzyskać spowolnieniu w dół dysku twardym komputera został podzielony i może przyspiesza przez defragmentacji dysku, tak aby były dostępne większych blokach ciągłe, można skonfigurować metryki defragmentacji ma Menedżera zasobów klaster próby wyprzedzeniem zmniejszanie Załaduj usług na mniej węzły, dzięki czemu jest (niemal) zawsze pokoju nawet dużych usług , pozwalającego można szybko utworzyć. Większość osób będzie wymagało, ponieważ usługi zazwyczaj powinien być mały, i w związku z tym nie jest trudne znaleźć pokój dla nich, ale jeśli masz duże usług i ich tworzyć szybko potrzebuje (i bez względu na inne kompromisów, takich jak zwiększona impactfulness błędów i niektóre zasoby, które pozostają niewykorzystane przez obciążenia zaplanowany), a następnie strategii defragmentacji jest dla Ciebie.

Na poniższym diagramie daje będący wizualną reprezentacją dwóch różnych klastrów, który jest zdefragmentowany i jedną, która nie jest. W przypadku strategii warto rozważyć ruchów, które będą konieczne do miejsca, w jeden z największych obiektów usługi, gdyby nową utworzone, w porównaniu z klastrem zdefragmentowanej, gdzie go może natychmiast umieszczone w węzłach 4 i 5.

![Porównanie programów strategie i defragmentowane klastrów][Image1]

## <a name="defragmentation-pros-and-cons"></a>Defragmentacji wad i zalet
Dlatego co to są te koncepcyjny kompromisów? Firma Microsoft zaleca dokładne miary z obciążeń pracą przed włączeniem metryki defragmentacji. Oto szybki spis rzeczy, które należy wziąć pod uwagę:

| Zalety defragmentacji  | Wady defragmentacji |
|----------------------|----------------------|
|Umożliwia szybsze tworzenie dużych usług | Koncentraty załadować na mniejszej liczby węzłów zwiększenie konfliktu
|Umożliwiają obniżyć przenoszenia danych podczas tworzenia    | Błędy może mieć wpływ na więcej usług i pochodząca więcej
|Umożliwia sformatowanego opis wymagań i odzyskiwanie miejsca | Bardziej złożoną konfigurację ogólnego zarządzania zasobami

Można łączyć zdefragmentowanej i normalne metryki w tym samym klastrze i Menedżer zasobów będzie zrobić jego najważniejsze, aby mieć pewność, że układ, który powoduje konsolidowanie możliwie największą część metryki defragmentacji, jak można je podczas próby rozłożenia pozostałych. Dokładne wyniki, który zostanie wyświetlony zależy od liczby równoważenia metryki w porównaniu z numerem metryki defragmentacji i ich grubości bieżącego obciążenia, itp.

## <a name="configuring-defragmentation-metrics"></a>Konfigurowanie metryki defragmentacji
Konfigurowanie metryki defragmentacji jest globalny decyzji w klastrze, a dla defragmentacji można wybrać pojedyncze metryki:

ClusterManifest.xml:

```xml
<Section Name="DefragmentationMetrics">
    <Parameter Name="Disk" Value="true" />
    <Parameter Name="CPU" Value="false" />
</Section>
```

## <a name="next-steps"></a>Następne kroki
- Menedżer zasobów klaster zawiera wiele opcji opisu klaster. Aby dowiedzieć się więcej na temat ich zapoznaj się z niniejszego artykułu [opisem klastrze tkaninie usługi](service-fabric-cluster-resource-manager-cluster-description.md)
- Metryki przedstawiono, jak Menedżer zasobów usługi tkaninie klaster zarządza zużycie i pojemnością w klastrze. Aby dowiedzieć się więcej na temat ich i sposobie ich konfigurowania zapoznaj się z [tego artykułu](service-fabric-cluster-resource-manager-metrics.md)

[Image1]:./media/service-fabric-cluster-resource-manager-defragmentation-metrics/balancing-defrag-compared.png
