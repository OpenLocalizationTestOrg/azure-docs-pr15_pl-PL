<properties
   pageTitle="Monitorowanie kondycji usługi tkaninie | Microsoft Azure"
   description="Wprowadzenie do monitorowania modelu, który zawiera monitorowania klaster aplikacji i usług zdrowia tkaninie usługi Azure."
   services="service-fabric"
   documentationCenter=".net"
   authors="oanapl"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/28/2016"
   ms.author="oanapl"/>

# <a name="introduction-to-service-fabric-health-monitoring"></a>Wprowadzenie do monitorowania kondycji usługi tkaninie
Azure tkaninie usługi wprowadza modelu kondycji, który zawiera oceny kondycji sformatowany, elastycznych i extensible i raportowania. Model pozwala w czasie rzeczywistym monitorowania stanu klaster i usług działających w nim. Można łatwo uzyskać informacje dotyczące kondycji i poprawić potencjalnych problemów przed kaskadowych i powodować duże dostawie. Typowe modelu usług wysyłanie raportów opartych na ich lokalnych widoków i że pole jest agregowane informacji o podanie ogólny klaster poziomie widoku.

Składniki usługi tkaninie za pomocą tego modelu kondycji sformatowanego raportowania ich bieżącym stanie. Za pomocą ten sam mechanizm zdrowia raportu z aplikacji. Jeśli należy inwestować w raportach kondycji wysokiej jakości, którym niestandardowe warunki, możesz wykrywanie i rozwiązywanie problemów dotyczących aplikacji uruchomionego znacznie łatwiejsze.

> [AZURE.NOTE] Firma Microsoft pracę podsystemu kondycji adresem potrzeba monitorowane uaktualnienia. Usługa tkaninie zawiera monitorowane uaktualnienia aplikacji i klaster, zapewniające dostępność pełny, brak przestojów i minimalna liczba do nie udziału użytkownika. Aby osiągnąć poniższe cele, uaktualnianie sprawdza opartych na skonfigurowane zasad uaktualniania i umożliwia uaktualnienia, aby kontynuować, tylko po odpowiednim progi uwzględnia kondycji. W przeciwnym razie uaktualnienie jest automatycznie wycofana lub wstrzymana, aby umożliwić administratorom możliwość rozwiązują problemy. Aby dowiedzieć się więcej na temat uaktualnienia aplikacji, zobacz [Ten artykuł](service-fabric-application-upgrade.md).

## <a name="health-store"></a>Magazyn kondycji
W sklepie kondycji informuje kondycji uzyskać informacje dotyczące obiektów w klastrze ułatwia ich odnalezienie i oceny. Jest stosowana jako tkaninie usługi zachowywane akumulujące usług zapewniające wysoką dostępność i skalowalność. W sklepie kondycji jest częścią **tkaninie:-System** aplikacji która jest dostępna, gdy klaster działa i uruchomiona.

## <a name="health-entities-and-hierarchy"></a>Jednostki zdrowia i hierarchii
Jednostki kondycji są zorganizowane w logicznej hierarchii, która rejestruje interakcji i zależności między różnymi jednostkami. Magazynie kondycji na podstawie raportów otrzymane od składników usługi tkaninie automatycznie utworzono obiektów i hierarchii.

Jednostki kondycji lustrzane podmioty tkaninie usługi. (Na przykład **Jednostka aplikacji kondycji** zgodna wystąpienie aplikacji wdrożony w klastrze, gdy **kondycji węzeł podmiot** zgodne węzła tkaninie usługi). Hierarchia kondycji rejestruje interakcji obiektów systemowych, która jest podstawą oceny kondycji zaawansowane. Aby Dowiedz się więcej o podstawowe pojęcia dotyczące usługi tkaninie [Omówienie kwestii technicznych tkaninie usługi](service-fabric-technical-overview.md). Aby uzyskać więcej informacji o aplikacji zobacz [model aplikacji usługi tkaninie](service-fabric-application-model.md).

Jednostki zdrowia i hierarchii Zezwalaj klaster i aplikacje skuteczne zgłoszone, debugowania, i monitorowane. Model kondycji zapewnia dokładną, *szczegółowego* reprezentację kondycji przenoszenie części wiele w klastrze.

![Podmioty kondycji.][1]
Jednostki kondycji zorganizowane w hierarchii, na podstawie relacji nadrzędny podrzędny.

[1]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy.png

Jednostki kondycji są:

- **Klaster**. Reprezentuje kondycji klastrze tkaninie usługi. Raporty dotyczące kondycji klaster opisują warunki, które wpływają na cały klaster i nie można zawęzić na jedno lub więcej dzieci nieprawidłowe. Jako przykład można wymienić badania nad umysłem klastrze dzielenie z powodu problemów z siecią podziału lub komunikacji.

- **Węzeł**. Reprezentuje kondycji węzła tkaninie usługi. Raporty dotyczące kondycji węzeł opisują warunki, które wpływa na działanie węzeł. Zazwyczaj wpływają na wszystkich jednostek wdrożonym uruchomiony. Jako przykład można wymienić węzeł za mało miejsca na dysku (lub innej właściwości komputera, takie jak pamięci, połączenia) oraz węzeł nie działa. Jednostki węzeł jest identyfikowany przez nazwę węzła (ciąg).

- **Aplikacja**. Reprezentuje kondycji wystąpienie aplikacji uruchamiania w klastrze. Raporty dotyczące kondycji aplikacji opisują warunki, które mają wpływ na ogólną kondycji aplikacji. Ta osoba nie można zawęzić do poszczególnych elementów podrzędnych (usług lub rozmieszczonych aplikacji). Jako przykład można wymienić zakończenia do końca interakcji między usługami różnych w aplikacji. Jednostka aplikacji jest identyfikowany przez nazwę aplikacji (URI).

- **Usługa**. Reprezentuje kondycji usługi uruchamiania w klastrze. Raporty dotyczące kondycji usługi opisano warunki, które mają wpływ na ogólną kondycji usługi oraz ich nie może być zawężana ani replice. Jako przykład można wymienić Konfiguracja usługi (na przykład port lub udziale plików zewnętrznych), który powoduje problemy dotyczące wszystkie partycje. Jednostki usługi jest identyfikowany przez nazwę usługi (URI).

- **Partycją**. Reprezentuje kondycji partycją usługi. Raporty dotyczące kondycji partition opisują warunki, które mają wpływ na zestawie całego. Jako przykład można wymienić liczba replik jest mniejsza liczba docelowej a partycją jest utratę kworum. Jednostki partycją jest identyfikowany przez partycją Identyfikatora (GUID).

- **Replice**. Reprezentuje kondycji replice stanowe usługi lub wystąpienia stateless usługi. Najmniejszą jednostką, która watchdogs i składniki systemu można raporty dotyczące aplikacji. Dla usług stanowe jako przykład można wymienić replice podstawowego raportowania, gdy nie można go powielić operacje pomocnicze i kiedy replikacja nie jest postępowania na oczekiwanej tempie. Ponadto stateless wystąpienie zgłosić, gdy zaczyna brakować zasobów lub występują problemy z łącznością. Jednostki replice jest identyfikowany przez partycją Identyfikatora (GUID) i identyfikator replice lub wystąpienia (długa).

- **DeployedApplication**. Reprezentuje kondycji *Aplikacja uruchomiona w węźle*. Raporty dotyczące kondycji aplikacji opisują warunki dotyczące tej aplikacji w węźle, który nie zawężana do pakietów usług wdrożony w tym samym węźle. Jako przykład można wymienić gdy nie można pobrać pakiet aplikacji, w tym węźle i występuje problem konfigurowania aplikacji podmioty zabezpieczeń w węźle. Wdrożonej aplikacji jest identyfikowany przez nazwę aplikacji (URI) i nazwa węzła (ciąg).

- **DeployedServicePackage**. Reprezentuje kondycji pakietu usługi uruchomiona w węźle w klastrze. Opisuje warunki określone do pakietu usług, które nie ma wpływu na pozostałe pakiety usługi w tym samym węźle dla tej samej aplikacji. Przykłady kodu pakietu w pakiecie usługi, której nie można uruchomić i konfiguracji pakiet, którego nie można odczytać. Pakiet wdrożonym usługi jest identyfikowany przez nazwę aplikacji (URI), nazwa węzła (ciąg) i nazwa manifestu usługi (ciąg).

Szczegółowości modelu kondycji ułatwia wykrywanie i Korygowanie problemów. Na przykład jeśli usługa nie odpowiada, jest to możliwe, aby zgłosić, że wystąpienie aplikacji jest nieprawidłowe. Raportowanie na że poziom jest idealnym rozwiązaniem, jednak, ponieważ problem mogą nie mieć wpływ na wszystkie usługi tej aplikacji. Raport powinny być stosowane z usługą nieprawidłowe lub partycją podrzędne określonego, jeśli punktów więcej informacji do niej. Dane automatycznie pobierający za pośrednictwem hierarchii i nieprawidłowe partycją jest widoczne na poziomie usług i aplikacji. Ten agregacji ułatwia identyfikowanie i rozwiązywanie głównej przyczyny problem szybciej.

Hierarchia kondycji składa się z relacji nadrzędny podrzędny. Klaster składa się z węzłów i aplikacje. Aplikacje usług i wdrożony aplikacji. Rozmieszczonych aplikacji rozmieścić pakietów usług. Usługi mają partycje i każdego partition ma jednej lub większej liczbie replik. Istnieje specjalne relacji między węzły i wdrożonym podmiotów. Jeśli węzeł jest nieprawidłowe zgłoszone przez jego system urzędu certyfikacji (usługa Menedżer pracy awaryjnej), dotyczy rozmieszczonych aplikacji, pakietów usług i repliki wdrożone na nim.

Hierarchia kondycji reprezentuje najnowszą stan systemu według najnowsze raporty dotyczące kondycji, czyli informacji niemal w czasie rzeczywistym.
Na samej jednostki, na podstawie warunków logicznych specyficzne dla aplikacji lub niestandardowe warunków monitorowane zgłosić watchdogs wewnętrznych i zewnętrznych. Raporty użytkownika współistnienie raportów systemu.

Planowanie na zainwestowanie w jak raportować i odpowiadanie na kondycji podczas projektowania usługi w chmurze duży, aby ułatwić debugowanie, monitorowanie i działanie usługi.

## <a name="health-states"></a>Stany kondycji
Usługa tkaninie użyto trójstanowy kondycji w celu opisania, czy obiekt jest prawidłowy: OK ostrzeżenie i błąd. Każdy raport wysyłane do sklepu kondycji należy określić jeden z tych stanów. Wynik oceny kondycji to jeden z tych stanów.

Możliwe [stany kondycji](https://msdn.microsoft.com/library/azure/system.fabric.health.healthstate) to:

- **OK**. Jednostka jest prawidłowy. Nie są znane problemy zgłoszone lub jej elementów podrzędnych (w stosownych przypadkach).

- **Ostrzeżenie**. Jednostka występują niektóre problemy, ale nie jest jeszcze nieprawidłowe (na przykład występują opóźnienia, ale nie powoduje problemy funkcjonalności jeszcze). W niektórych przypadkach stan ostrzeżenia może się ustalić bez wszelkie specjalne udziału, a jest przydatne zapewnić, że wgląd co się dzieje. W innych przypadkach stan ostrzeżenia może zmniejszyć trudnych problem, bez udziału użytkownika.

- **Błąd**. Jednostka jest nieprawidłowe. Akcja należy rozwiązać stan obiektu, ponieważ nie będzie działać poprawnie.

- **Nieznany**. Jednostka nie istnieje w magazynie kondycji. Wynik można uzyskać z rozkładem kwerend, które wyniki z wielu składników scalania. Na przykład uzyskać kwerendy listy węzła osiąga **FailoverManager** i **HealthManager**, Pobierz aplikację listy kwerendy przechodzi na **ClusterManager** i **HealthManager**. Te zapytania Scal wyniki z wielu składników systemu. Jeśli innego składnika systemu zwraca jednostkę, która nie została osiągnięta lub czyszczone ze sklepu kondycji, wyniku scalenia ma nieznany stan kondycji.

## <a name="health-policies"></a>Zasady dotyczące kondycji
W sklepie kondycji stosuje zasady dotyczące kondycji do określenia, czy obiekt jest prawidłowy na podstawie jego raportów i jej elementów podrzędnych.

> [AZURE.NOTE] Zasady dotyczące kondycji można określić w manifeście klaster (dla klastrów i węzłów oceny kondycji) lub manifest aplikacji (w przypadku aplikacji oceny i wszystkich jej elementów podrzędnych). Żądań oceny kondycji można również przekazać w zasady oceny kondycji niestandardowe, które są używane tylko do tej oceny.

Domyślnie usługa tkaninie dotyczy ścisłych reguł (wszystko musi być prawidłowy) dla relacji hierarchii nadrzędny podrzędny. Jeśli przynajmniej jedno z elementów podrzędnych ma jedną zdarzenie nieprawidłowe, nadrzędny jest traktowany jako nieprawidłowe.

### <a name="cluster-health-policy"></a>Zasady dotyczące kondycji klaster
[Zasady dotyczące kondycji klaster](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.aspx) jest używana do obliczania klaster kondycję i stany kondycji węzeł. Zasady można zdefiniować w manifeście klaster. Jeśli nie jest dostępna, jest używany domyślnych zasad (zero tolerowaną błędy).
Zasady dotyczące kondycji klaster zawiera:

- [ConsiderWarningAsError](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.considerwarningaserror.aspx). Określa, czy traktowania ostrzeżenie o kondycji raporty jako błędne wersja próbna kondycji. Domyślne: FAŁSZ.

- [MaxPercentUnhealthyApplications](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.maxpercentunhealthyapplications.aspx). Określa maksymalną tolerowaną procent aplikacje, które mogą być nieprawidłowe, zanim klaster jest traktowany jako błąd.

- [MaxPercentUnhealthyNodes](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.maxpercentunhealthynodes.aspx). Określa maksymalną tolerowaną procent węzłów, które mogą być nieprawidłowe, zanim klaster jest traktowany jako błąd. W dużych klastrów niektóre węzły są zawsze w dół lub na zewnątrz do napraw, aby ta wartość procentowa należy skonfigurować tolerowania, który.

- [ApplicationTypeHealthPolicyMap](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.applicationtypehealthpolicymap.aspx). Mapowanie zasad kondycji typ aplikacji może służyć wersja próbna kondycji klaster celu opisania typów specjalne aplikacji. Domyślnie wszystkie aplikacje są wprowadzane do puli i obliczane przy użyciu MaxPercentUnhealthyApplications. Niektóre typy aplikacji powinny być traktowane inaczej, może zostać pobrany z globalnej puli. Zamiast tego są sprawdzane pod kątem zgodności procentów skojarzony z jego nazwą typu aplikacji na mapie. Na przykład w klastrze istnieją tysiące aplikacji różnych typów i kilka wystąpień aplikacji sterowania typu specjalnej aplikacji. Aplikacje sterowania nigdy nie powinny być błędu. Możesz określić globalny MaxPercentUnhealthyApplications 20% tolerowania określonych błędów, ale typ aplikacji, które "ControlApplicationType" równa 0 MaxPercentUnhealthyApplications. Dzięki temu Jeśli niektóre z wielu aplikacji są nieprawidłowe, ale poniżej globalnej nieprawidłowe wartości procentowej, klaster oceniono ostrzeżenie. Stan kondycji ostrzeżenie nie ma wpływu na uaktualnienie klaster lub innych monitorowania wyzwalane przez kondycję błędu. Ale jeszcze jeden sterowania błąd spowodowałby klaster nieprawidłowe, które wyzwalane wycofywanie lub można wstrzymać uaktualnianie klaster, w zależności od konfiguracji uaktualnienia.
Dla typów aplikacji zdefiniowanych w planie wszystkie wystąpienia aplikacji są pobierane z globalnej puli aplikacji. Ich stosowania na podstawie całkowitej liczby aplikacji typ aplikacji przy użyciu określonego MaxPercentUnhealthyApplications z mapy. Wszystkie pozostałe aplikacje znajdują się w puli globalnej i są obliczane przy użyciu MaxPercentUnhealthyApplications.

Poniższy przykład to fragment manifest klaster. Aby zdefiniować wpisy w planie typ aplikacji, przed nazwą parametru "ApplicationTypeMaxPercentUnhealthyApplications-", a następnie wpisz nazwę aplikacji.

```xml
<FabricSettings>
  <Section Name="HealthManager/ClusterHealthPolicy">
    <Parameter Name="ConsiderWarningAsError" Value="False" />
    <Parameter Name="MaxPercentUnhealthyApplications" Value="20" />
    <Parameter Name="MaxPercentUnhealthyNodes" Value="20" />
    <Parameter Name="ApplicationTypeMaxPercentUnhealthyApplications-ControlApplicationType" Value="0" />
  </Section>
</FabricSettings>
```

### <a name="application-health-policy"></a>Zasady dotyczące kondycji aplikacji
[Zasady dotyczące kondycji aplikacji](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.aspx) opisano, jak zakończeniu oceny wydarzeń i Zjednoczone podrzędny agregacji dla aplikacji i jej elementów podrzędnych. Można zdefiniować w oczywiste aplikacji, **ApplicationManifest.xml**w pakiet aplikacji. Jeśli zasady nie zostaną określone, tkaninie usługi zakłada, że podmiot jest nieprawidłowe, jeśli został raport dotyczący kondycji lub element podrzędny w stan kondycji ostrzeżenia lub błędu.
Można konfigurować zasady są:

- [ConsiderWarningAsError](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.considerwarningaserror.aspx). Określa, czy traktowania ostrzeżenie o kondycji raporty jako błędne wersja próbna kondycji. Domyślne: FAŁSZ.

- [MaxPercentUnhealthyDeployedApplications](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.maxpercentunhealthydeployedapplications.aspx). Określa maksymalną tolerowaną procent rozmieszczonych aplikacji, które mogą być nieprawidłowe, zanim aplikacji jest traktowany jako błąd. Ta wartość procentowa oblicza się dzieląc liczbę nieprawidłowe rozmieszczonych aplikacji liczby węzłów, które aplikacje są obecnie wdrożone w klastrze. Przy obliczaniu zaokrągla w górę tolerowania jeden błąd małej liczby węzłów. Domyślne wartości procentowej: zero.

- [DefaultServiceTypeHealthPolicy](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.defaultservicetypehealthpolicy.aspx). Umożliwia określenie domyślnych zasad kondycji typu usługa, która zastępuje domyślnych zasad kondycji dla wszystkich typów usług w aplikacji.

- [ServiceTypeHealthPolicyMap](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.servicetypehealthpolicymap.aspx). Zawiera mapę zasady dotyczące kondycji usługi dla typów usług. Te zasady zastępują domyślne zasady kondycji typ usługi dla każdego typu określonej usługi. Na przykład jeśli aplikacja ma typ usługi stateless bramy i typ usługi Aparat stanowe, można skonfigurować zasady dotyczące kondycji ich oceny inaczej. Po określeniu zasad dla typów usług można uzyskać większą kontrolę nad kondycji usługi.

### <a name="service-type-health-policy"></a>Zasady dotyczące kondycji typu usługi
[Zasady dotyczące kondycji usługi typu](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.aspx) określa sposób oceny i agregowanie usług i elementów podrzędnych usług. Zasady zawierają:

- [MaxPercentUnhealthyPartitionsPerService](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthypartitionsperservice.aspx). Określa maksymalną tolerowaną wartością procentową nieprawidłowe partycje przed usługi jest traktowany jako nieprawidłowe. Domyślne wartości procentowej: zero.

- [MaxPercentUnhealthyReplicasPerPartition](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyreplicasperpartition.aspx). Określa maksymalną tolerowaną procent replik nieprawidłowe, przed partycją jest traktowany jako nieprawidłowe. Domyślne wartości procentowej: zero.

- [MaxPercentUnhealthyServices](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyservices.aspx). Określa maksymalną tolerowaną procent nieprawidłowe usług, zanim aplikacji jest traktowany jako nieprawidłowe. Domyślne wartości procentowej: zero.

Poniższy przykład jest fragment manifest aplikacji:

```xml
    <Policies>
        <HealthPolicy ConsiderWarningAsError="true" MaxPercentUnhealthyDeployedApplications="20">
            <DefaultServiceTypeHealthPolicy
                   MaxPercentUnhealthyServices="0"
                   MaxPercentUnhealthyPartitionsPerService="10"
                   MaxPercentUnhealthyReplicasPerPartition="0"/>
            <ServiceTypeHealthPolicy ServiceTypeName="FrontEndServiceType"
                   MaxPercentUnhealthyServices="0"
                   MaxPercentUnhealthyPartitionsPerService="20"
                   MaxPercentUnhealthyReplicasPerPartition="0"/>
            <ServiceTypeHealthPolicy ServiceTypeName="BackEndServiceType"
                   MaxPercentUnhealthyServices="20"
                   MaxPercentUnhealthyPartitionsPerService="0"
                   MaxPercentUnhealthyReplicasPerPartition="0">
            </ServiceTypeHealthPolicy>
        </HealthPolicy>
    </Policies>
```

## <a name="health-evaluation"></a>Ocenianie kondycji
Użytkownicy i usługi automatycznego oszacować kondycji dla dowolnego obiektu w dowolnym momencie. Aby ocenić prawidłowość jednostki, agregacje magazynu kondycji wszystkich zdrowia raporty dotyczące obiektu i oblicza wszystkie jej elementów podrzędnych (w stosownych przypadkach). Algorytm agregacji kondycji używa zasady kondycji, które określają, jak ma być obliczona raporty dotyczące kondycji oraz sposób agregacji stany kondycji podrzędny (w stosownych przypadkach).

### <a name="health-report-aggregation"></a>Kondycja raportu agregacji
Jeden podmiot może mieć wiele raporty dotyczące kondycji wysyłane przez różne podmioty (składniki systemu lub watchdogs) na różne właściwości. Agregacja używa zasady skojarzone kondycji, w szczególności ConsiderWarningAsError członkiem aplikacji lub klaster zasady dotyczące kondycji. ConsiderWarningAsError Określa, jak ma być obliczona ostrzeżeń.

Stan kondycji zagregowane jest wyzwalane przez *najgorszego* raporty dotyczące kondycji w jednostce. W przypadku błędu co najmniej jeden raport dotyczący kondycji zagregowane kondycji jest komunikat o błędzie.

![Kondycja agregacji raportu z raportu o błędach.][2]

Jednostki kondycja, która ma co najmniej jeden raporty dotyczące kondycji błąd jest szacowana jako błąd. Dotyczy to także raport dotyczący kondycji wygasłe, niezależnie od stanu kondycji.

[2]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-error.png

W przypadku nie raportów o błędach i jedno lub więcej ostrzeżeń, stan kondycji zagregowane jest ostrzeżenie lub błąd, w zależności od flagi zasad ConsiderWarningAsError.

![Kondycja agregacji raportu z raportem ostrzeżenia i ConsiderWarningAsError wartość FAŁSZ.][3]

Kondycja agregacji raportu z raportem ostrzeżenia i ConsiderWarningAsError ustawiona na wartość false (ustawienie domyślne).

[3]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-warning.png

### <a name="child-health-aggregation"></a>Agregacją kondycji elementu podrzędnego
Stan kondycji zagregowane jednostka odzwierciedla stany kondycji podrzędny (w stosownych przypadkach). Algorytm dla agregacji stany kondycji podrzędny używa zasady dotyczące kondycji dotyczy według typu obiektu.

![Podrzędne jednostki kondycji agregacji.][4]

Na podstawie zasad kondycji agregacji podrzędne.

[4]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy-eval.png

Po magazynie kondycji oszacowana wszystkie elementy podrzędne, agreguje stanu kondycji na podstawie skonfigurowaną procentu maksymalna liczba elementów podrzędnych nieprawidłowe. Ta wartość procentowa jest pobierana z zasad zależy od typu obiektu i podrzędne.

- Jeśli wszystkie elementy podrzędne stany OK, stan kondycji podrzędne agregacją jest przycisk OK.

- Jeśli dzieci zawiera zarówno OK i Stany ostrzeżenie o kondycji podrzędne agregacją jest ostrzeżenie.

- W przypadku dzieci z stanów błędu, które nie przestrzega maksymalna dozwolona wartość procentową nieprawidłowe dzieci zagregowane kondycji jest komunikat o błędzie.

- Jeżeli dzieci z stany błędu przestrzegać maksymalna dozwolona wartość procentowa nieprawidłowe dzieci, stan kondycji zagregowane jest ostrzeżenie.

## <a name="health-reporting"></a>Raportowanie kondycji
Składniki systemu, System tkaninie aplikacji i watchdogs wewnętrznych i zewnętrznych mogą zgłaszać zadania jednostki tkaninie usługi. Podmioty wprowadź *lokalne* oznaczeń kondycję monitorowane wpisy, na podstawie warunków, które są one monitorowania. Nie należy przeglądać stanu globalnego ani agregowanie danych. Zachowanie ma podmioty proste, a nie złożonych organizmów, które należy przeglądać wiele rzeczy, aby ustalić, jakie informacje, aby wysłać.

Wysyłanie danych kondycji do sklepu kondycji, reporter musi identyfikowania jednostkę, którego dotyczy problem, i utworzyć raport dotyczący kondycji. Następnie można wysłać raportu, za pośrednictwem interfejsu API przy użyciu [FabricClient.HealthClient.ReportHealth](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient_members.aspx), przy użyciu programu PowerShell lub pozostałych.

### <a name="health-reports"></a>Raporty dotyczące kondycji
[Raporty dotyczące kondycji](https://msdn.microsoft.com/library/azure/system.fabric.health.healthreport.aspx) dla każdej jednostki w klastrze zawiera następujące informacje:

- **SourceId**. Ciąg, który identyfikuje reporter zdarzenia kondycji.

- **Identyfikator jednostki**. Służy do identyfikowania obiektu miejsce, w którym zastosowano raportu. Różni się pod względem [jednostki wpisz](service-fabric-health-introduction.md#health-entities-and-hierarchy):

  - Klaster. Brak.

  - Węzeł. Nazwa węzła (ciąg).

  - Aplikacja. Nazwa aplikacji (URI). Reprezentuje nazwę wystąpienia aplikacji wdrożony w klastrze.

  - Usługa. Nazwa usługi (URI). Odpowiada nazwie wystąpienia usługi wdrożony w klastrze.

  - Partycją. Identyfikator (GUID). Reprezentuje unikatowy identyfikator partycją.

  - Replice. Identyfikator replice stanowe usługi lub identyfikator wystąpienia stateless usługi (INT64).

  - DeployedApplication. Nazwa aplikacji (URI) i nazwa węzła (ciąg).

  - DeployedServicePackage. Nazwa aplikacji (URI), nazwa węzła (ciąg) i usługa pojawiają się nazwy (ciąg).

- **Właściwość**. *Ciąg* (nie stały wyliczenia) umożliwiający reporter kategoryzowania zdarzenia kondycji dla określonej właściwości obiektu. Na przykład reporter odpowiedzi można Zgłoś zdrowia właściwość Node01 "magazynowania" i reporter B zgłosić kondycji właściwość Node01 "łączności". W sklepie kondycji tych raportów są traktowane jako osobne kondycji zdarzeń dla obiektu Node01.

- **Opis**. Ciąg, który umożliwia reporter przygotować szczegółowe informacje dotyczące zdarzenia kondycji. **SourceId**, **Właściwości**i **HealthState** w pełni powinny opisać raportu. Opis dodaje zrozumiałą informacje na temat raportu. Tekst ułatwia dla administratorów i użytkowników poznać raport dotyczący kondycji.

- **HealthState**. [Wyliczenie](service-fabric-health-introduction.md#health-states) opisującą kondycję raportu. Dopuszczalne wartości są OK, ostrzeżenie i błąd.

- **Licznika TimeToLive**. Przedziału czasu, który wskazuje, jak długo raport dotyczący kondycji jest prawidłowy. Połączone z **RemoveWhenExpired**, umożliwia sklepu kondycji dowiedzieć się, jak ma być obliczona wygasłe zdarzeń. Domyślnie wartość jest nieograniczony, a raport obowiązuje zawsze.

- **RemoveWhenExpired**. Wartość logiczna. Jeśli wartość true, raport dotyczący kondycji wygasłe automatycznie zostanie usunięte z magazynu zdrowia i raport nie wpływają na ocenianie kondycji jednostki. Używane podczas raport jest prawidłowe w podanym okresie tylko raz, a reporter nie musi jawnie Wyczyść. Jest również używana do usunięcia raportów z magazynu kondycji (na przykład watchdog zostanie zmieniony i zatrzymuje wysyłanie raportów przy użyciu poprzedniej źródła i właściwości). Może wysłać raport z krótkim licznika TimeToLive wraz z RemoveWhenExpired, aby rozwiązać wszelkie poprzedniego stanu ze sklepu kondycji. Jeśli wartość jest ustawiona na wartość FAŁSZ, wygasłe raport jest traktowana jako błąd w oceny kondycji. W magazynie kondycji wartość FAŁSZ sygnalizuje, że źródła Zgłoś okresowo dla tej właściwości. W przeciwnym razie należy problem z watchdog. Kondycja watchdog są przechwytywane, ustalając zdarzenia jako błąd.

- **SequenceNumber**. Dodatnią liczbą całkowitą, powinien być stale rosnącej, przedstawia kolejność raportów. Jest używany przez sklepu kondycji wykrywanie starych raportów, które są odbierane opóźnione ze względu na opóźnienia sieci lub innych problemów. Raport zostanie odrzucony, jeśli numerem jest mniejsza niż lub równa najczęściej ostatnio zastosowany numer dla tej samej jednostki, źródła i właściwości. Jeśli nie zostanie określony, numerem jest generowany automatycznie. Należy umieścić w numerem tylko wtedy, gdy raportów o stanie przejścia. W tej sytuacji źródła należy pamiętać raportów, które wysłanie go i przechowywać informacje dotyczące odzyskiwania na pracy awaryjnej.

Cztery sztuk informacji — SourceId, identyfikator, właściwości i HealthState — są wymagane dla każdej raport dotyczący kondycji. SourceId, których nie może być ciągiem zaczynać się prefiks "**systemu.**", która jest zastrzeżona systemu raportów. Dla tego samego obiektu jest tylko jeden raport dla tego samego źródła i właściwości. Wiele raportów z tego samego źródła i właściwości zastępują siebie, po stronie klienta kondycji (jeśli jest przetwarzany wsadowo) lub dotyczące kondycji sklepu strony. Zastąpienie jest oparty na numerach; Raporty nowsze (mają wyższe numery sekwencji) Zamień starsze raportów.

### <a name="health-events"></a>Zdarzenia kondycji
Wewnętrznie w sklepie kondycji informuje [kondycji zdarzenia](https://msdn.microsoft.com/library/azure/system.fabric.health.healthevent.aspx), zawierające wszystkie informacje z raportów i dodatkowe metadane. Metadane zawiera czas, otrzymaną raport do klienta zdrowia i modyfikacji po stronie serwera. Zdarzenia kondycji są zwracane przez [kondycji kwerend](service-fabric-view-entities-aggregated-health.md#health-queries).

Dodano metadane zawierają:

- **SourceUtcTimestamp**. Czas otrzymał Raport kondycji klienta (uniwersalny czas koordynowany).

- **LastModifiedUtcTimestamp**. Czas ostatniej modyfikacji raportu po stronie serwera (uniwersalny czas koordynowany).

- **IsExpired**. Flaga wskazuje, czy raport wygasła w momencie kwerenda została wykonana w sklepie kondycji. Zdarzenia można wygasła, tylko wtedy, gdy RemoveWhenExpired ma wartość FAŁSZ. W przeciwnym razie zdarzenie nie jest zwróconych przez kwerendę, a zostanie usunięty z magazynu.

- **LastOkTransitionAt**, **LastWarningTransitionAt** **LastErrorTransitionAt**. Godzina ostatniego przejścia błąd OK-ostrzeżenie. Te pola nadać historii kondycji stan przejścia dla zdarzenia.

Pola przejścia stan mogą być używane dla sprawniejszą alertów i informacje o zdarzeniach kondycji "historycznych". Scenariusze umożliwiają takie jak:

- Alert, gdy właściwość na ostrzeżenie i błąd dla więcej niż X min. Sprawdzanie warunek w okresie pozwala uniknąć alerty na tymczasowe warunków. Na przykład można przekształcić alertu, jeśli stan kondycji ma zostały ostrzeżenia o więcej niż pięć minut (HealthState == ostrzeżenia i — LastWarningTransitionTime > 5 minut).

- Alert tylko na warunki, które zostały zmienione w ciągu ostatnich X min. Jeśli raport był już w błąd przed określonym czasie, można zignorować, ponieważ został już sygnalizowane wcześniej.

- Jeśli właściwość jest przełączanie ostrzeżenie i błąd, określić, jak długo trwa był nieprawidłowe (oznacza to, że nie OK). Na przykład można przekształcić alertu, jeśli właściwość nie został prawidłowy na więcej niż pięć minut (HealthState! = Ok i — LastOkTransitionTime > 5 minut).

## <a name="example-report-and-evaluate-application-health"></a>Przykład: Raport i oceny kondycji aplikacji
W poniższym przykładzie wysyłana raport dotyczący kondycji przy użyciu programu PowerShell na pasku aplikacji **tkaninie:-WordCount** ze źródła **MyWatchdog**. Raport dotyczący kondycji zawiera informacje o kondycji właściwość "dostępność" w stanie kondycji błędu z nieskończonej licznika TimeToLive. Następnie sprawdza kondycji aplikacji, która oblicza agregacją błędy stan kondycji i zdarzeń zgłoszoną kondycji na liście zdarzeń kondycji.

```powershell
PS C:\> Send-ServiceFabricApplicationHealthReport –ApplicationName fabric:/WordCount –SourceId "MyWatchdog" –HealthProperty "Availability" –HealthState Error

PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            :
                                  Error event: SourceId='MyWatchdog', Property='Availability'.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error

                                  ServiceName           : fabric:/WordCount/WordCountWebService
                                  AggregatedHealthState : Ok

DeployedApplicationHealthStates :
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_0
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_2
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_3
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_4
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_1
                                  AggregatedHealthState : Ok

HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 360
                                  SentAt                : 3/22/2016 7:56:53 PM
                                  ReceivedAt            : 3/22/2016 7:56:53 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 7:56:53 PM, LastWarning = 1/1/0001 12:00:00 AM

                                  SourceId              : MyWatchdog
                                  Property              : Availability
                                  HealthState           : Error
                                  SequenceNumber        : 131032204762818013
                                  SentAt                : 3/23/2016 3:27:56 PM
                                  ReceivedAt            : 3/23/2016 3:27:56 PM
                                  TTL                   : Infinite
                                  Description           :
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Ok->Error = 3/23/2016 3:27:56 PM, LastWarning = 1/1/0001 12:00:00 AM
```

## <a name="health-model-usage"></a>Sposób użycia modelu kondycji
Model kondycji umożliwia usługami w chmurze i podstawowej platformy tkaninie usługi przeskalować, ponieważ monitorowania i kondycji są rozmieszczone w różnych monitorów w klastrze.
Inne systemy usługę pojedynczy, scentralizowane na poziomie klaster analizowania wszystkie *potencjalnie* przydatne informacje wysyłane przez usługi. Tej metody przeszkadza ich skalowalność. Go też nie zezwala na potrzeby zbierania bardzo precyzyjne informacje ułatwiające identyfikowanie problemów i potencjalne problemy jako zbliżony przyczyny możliwie.

Model kondycji używany jest intensywnie monitorowania i diagnostyki, do oceniania kondycji klaster i aplikacji i monitorowane uaktualnienia. Inne usługi przy użyciu danych kondycji automatyczne naprawić, tworzenie historii kondycji klaster i problemu alerty w określonych warunkach.

## <a name="next-steps"></a>Następne kroki
[Wyświetlanie raportów dotyczących kondycji usługi tkaninie](service-fabric-view-entities-aggregated-health.md)

[Używanie raportów dotyczących kondycji systemu do rozwiązywania problemów](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Jak zgłaszać i sprawdzanie kondycji usługi](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Dodawanie niestandardowych raporty dotyczące kondycji usługi tkaninie](service-fabric-report-health.md)

[Monitorowanie i diagnozowanie usług lokalnie](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Uaktualnianie aplikacji tkaninie usługi](service-fabric-application-upgrade.md)
