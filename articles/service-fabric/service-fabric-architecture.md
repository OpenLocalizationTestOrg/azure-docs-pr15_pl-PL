<properties
   pageTitle="Usługa architektury | Microsoft Azure"
   description="Usługa jest używany do tworzenia aplikacji skalowalna, wiarygodnych i łatwo zarządzane dla chmury platformy systemów dystrybucji. W tym artykule przedstawiono architektura tkaninie usługi."
   services="service-fabric"
   documentationCenter=".net"
   authors="rishirsinha"
   manager="timlt"
   editor="rishirsinha"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/09/2016"
   ms.author="rsinha"/>

# <a name="service-fabric-architecture"></a>Usługa architektury

Usługa tkaninie jest wbudowany z podsystemów ułożone warstwowo. Tych podsystemów umożliwiają pisać aplikacje, które są:

* Wysokiej dostępności
* Skalowalna
* Łatwe w zarządzaniu
* Testable

Na poniższym diagramie przedstawiono głównych podsystemów materiału usługi.

![Diagram architektury usługi](media/service-fabric-architecture/service-fabric-architecture.png)

W układzie rozłożone ważnych jest możliwość bezpiecznego komunikowania się między węzłów w klastrze. Na podstawie stosu jest podsystemu transportu, która udostępnia komunikacyjne węzłów. Powyżej transportu podsystemu leży podsystemu Federacji klastrów różne węzły do całość (o nazwie klastrów) tak, aby tkaninie usługi można wykrywać błędy, prowadzenie wybory znak wiodący i zapewniają spójne routingu. Podsystemu niezawodności w warstwie powyżej podsystemu federacji jest odpowiedzialny za niezawodności usług tkaninie usługi przez mechanizmy, takich jak replikacji, zarządzania zasobami i pracy awaryjnej. Federacja podsystemu źródłową również podsystemu hosting i aktywacji, który służy do zarządzania cyklem aplikacji w jednym węźle. Podsystemu zarządzania służy do zarządzania cyklem aplikacji i usług. Podsystemu pola ułatwia testowanie swoich usług za pomocą symulowany błędy przed i po podczas wdrażania aplikacji i usług w środowisku produkcyjnym deweloperów aplikacji. Usługa tkaninie umożliwia rozwiązania lokalizacji usługi za pośrednictwem jego podsystemu komunikacji. Modele programowania aplikacji na deweloperzy są w warstwie powyżej tych podsystemów wraz z modelu aplikacji, aby włączyć narzędzia.

## <a name="transport-subsystem"></a>Podsystemu transportu
Podsystemu transportu wykonuje kanału komunikacji datagramów typu punkt-punkt. Ten kanał jest używana do komunikacji w ramach usługi tkaninie klastrów i komunikacji między klaster tkaninie usług i klientów. Obsługa jednokierunkowe i wzorce komunikacji żądania odpowiedzi, które stanowi podstawę dla implementacji emisja i multiemisja w warstwie federacji. Podsystemu transportu zabezpiecza komunikacji za pomocą X509 certyfikaty lub zabezpieczeń systemu Windows. Tego podsystemu jest używane wewnętrznie tkaninie usługi i nie jest dostępna bezpośrednio dla deweloperów programowania aplikacji.

## <a name="federation-subsystem"></a>Federacja podsystemu
Aby przyczyny o zestawu węzłów w układzie rozłożone, można muszą być spójne widok systemu. Federacja podsystemu używa pierwotnych komunikacji dostarczony przez podsystemu transportu i odwzorowywały w jeden klaster ujednolicony, które mogą go przyczyny o różnych typach węzłów. Udostępnia pierwotnych systemów dystrybucji wymagane przez innych podsystemów - wykrywanie awarii, wybór znak wiodący i spójne routing. U góry tabel mieszania rozłożone spacją 128-bitowego token jest tworzona podsystemu federacji. Podsystemu tworzy topologii pierścienia w węzłach z każdego węzła Dzwoń przydzieloną podzbiór token miejsca dla własności. Dla wykrywania awarii warstwie używa mechanizmu leasingu na podstawie serce kruszenie i postępowania arbitrażowego. Podsystemu Federacji również gwarantuje za pośrednictwem sprzężenia skomplikowanych i protokołów wylotów jednego właściciela tokenu istnieje w dowolnym momencie. Podaj to wyboru znak wiodący i spójne gwarancje routingu.

## <a name="reliability-subsystem"></a>Podsystemu niezawodność
Podsystemu niezawodności udostępnia mechanizm wysokiej dostępności stanu usługi tkaninie usługi przy użyciu _Replikator_, _Menedżer pracy awaryjnej_i _Równoważenia zasobów_.

* Replikator gwarantuje, że zmian stanu w replice podstawowej usługi będą automatycznie replikowane do pomocniczej replik zachowaniu spójności między replikami głównego i pomocniczego w zestawie replice usługi. Replikator jest odpowiedzialne za zarządzanie kworum między replikami w zestawie. Interakcji z jednostką pracy awaryjnej, aby wyświetlić listę operacje, aby odtworzyć, a agent ponowna konfiguracja zapewnia konfiguracji zestawie. Tej konfiguracji wskazuje dana operacje mają być replikowane. Tkaninie Usługa udostępnia Replikator domyślny o nazwie replikatora tkaninie, które mogą być używane przez interfejs API modelu aby wysoce dostępne i zaufanego stanu usługi.
* Menedżer pracy awaryjnej gwarantuje, że po węzły zostaną dodane lub usunięte z klastrem, załaduj zostanie automatycznie ponownie rozdzielona między w węzłach dostępne. Jeśli węzeł w klastrze nie powiedzie się, klaster będzie automatycznie skonfigurować repliki usługi, aby utrzymać dostępność.
* Menedżer zasobów umieszcza repliki usługi przez domenę błąd w klastrze i gwarantuje, że wszystkich jednostek pracy awaryjnej działają. Menedżer zasobów również sald zasobów usługi przez źródłowych udostępnionej puli węzłach uzyskanie optymalnego obciążenia jednolitego rozkładu.

## <a name="management-subsystem"></a>Zarządzanie podsystemu
Podsystemu zarządzania udostępnia usługi zakończenia do końca i zarządzanie aplikacjami cyklu życia. Polecenia cmdlet programu PowerShell oraz interfejsy API administracyjne umożliwiają zapewnianie, wdrażanie, poprawki, uaktualnianie i usuwanie aplikacji bez utraty dostępności. Podsystemu zarządzania wykonuje to za pomocą następujących usług.

* **Menedżer klastrów**: jest to podstawowej usługi interakcji z Menedżer pracy awaryjnej z niezawodności, aby umieścić aplikacje w węzłach na podstawie ograniczeń położenie usługi. Menedżer zasobów pracy awaryjnej podsystemu zapewnia, że ograniczeń nigdy nie są przerwane. Menedżer klaster zarządza do zarządzania cyklem życia aplikacji z zapewnienie usuwanie. Jest zintegrowany z Menedżera kondycji, aby upewnić się, że dostępność aplikacji nie są tracone z perspektywy zdrowia semantyczny podczas uaktualniania.
* **Menedżer kondycji**: ta usługa umożliwia monitorowanie kondycji aplikacji, usług i klaster podmiotów. Klaster obiektów (takich jak węzły partycje usługi i repliki) zgłosić informacji o kondycji, która następnie jest agregowane w magazynie scentralizowane kondycji. Te informacje dotyczące kondycji zawiera migawkę kondycji ogólnej-chwili usług i węzły rozkładany wiele węzłów w klastrze, co pozwala podjąć niezbędne działania naprawcze. Kwerenda kondycji interfejsów API umożliwiają kwerendy zdarzenia kondycji zgłoszone podsystemu kondycji. Kwerenda kondycji, którą interfejsy API zwracana kondycji nieprzetworzonych danych przechowywanych w magazynie kondycji lub zagregowane, interpretowany kondycji danych dla jednostki określonej klaster.
* **Obraz ze sklepu**: ta usługa zawiera przechowywania i dystrybucji plików binarnych aplikacji. Ta usługa zawiera w sklepie plik protokołu simple rozłożone miejsce, w którym przekazane do i pobrane z aplikacji.


## <a name="hosting-subsystem"></a>Hostingu podsystemu
Menedżer klaster informuje hostingu podsystemu (działającego w każdym węźle) usług, które należy zarządzanie dla określonego węzła. Następnie hostingu podsystemu zarządza do zarządzania cyklem życia aplikacji w tym węźle. Interakcji ze składnikami niezawodności i kondycji, aby zapewnić, że repliki są właściwie rozmieszczone i jest prawidłowy.

## <a name="communication-subsystem"></a>Podsystemu komunikacji
Tego podsystemu znajdują się wiarygodnych wiadomości w ramach odnajdowania klaster i usług za pośrednictwem usługi nazw. Usługa nazw jest rozpoznawana jako nazwy usług do lokalizacji w klastrze i umożliwia użytkownikom zarządzanie nazwy usług i właściwości. Przy użyciu usługi nazw, klienci mogą bezpiecznie komunikować się z dowolnym węźle w klastrze rozpoznawania nazw usługi i pobierania metadanych usługi. Użytkowników z usługi przy użyciu prostej interfejs API klienta nazw, można tworzyć usług i klientów ułatwiających rozwiązanie bieżącą lokalizację sieciową pomimo dynamiki węzeł lub ponowne rozmiaru klaster.

## <a name="testability-subsystem"></a>Pola podsystemu
Pola jest to zestaw narzędzi, które są zaprojektowane specjalnie dla usługi oparty na tkaninie usługa testowania. Narzędzia umożliwiają Deweloper łatwo powodować błędy zrozumiałej i uruchamianie scenariuszy testowania do wykonywania i wiele państw i przejścia, które usługi będą wrażenia całej jego istnienia, wszystkich kontroli i bezpieczny sposób. Pola udostępnia mechanizm ma być uruchamiana bez utraty dostępności różnych możliwych błędów dłużej testów, które można przejść. Zawiera środowisku test w produkcji.
