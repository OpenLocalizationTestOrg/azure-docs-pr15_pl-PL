# <a name="compute"></a>Obliczenia

Azure umożliwia wdrażanie i monitorowanie kodzie aplikacji uruchomiony z centrum danych firmy Microsoft. Podczas tworzenia aplikacji i uruchomić go Azure, kod i konfiguracji razem nosi nazwę Azure hostowana usługa. Obsługiwane usługi są łatwe w zarządzaniu, skalowanie w górę lub w dół, skonfigurowanie i aktualizowanie z nowej wersji aplikacji kodu. W tym artykule omówiono Azure hostowanej model aplikacji usługi.<a id="compare" name="compare"></a>

## Spis treści<a id="_GoBack" name="_GoBack"></a>

-   [Zalety modelu Azure aplikacji][]
-   [Hostowana usługa podstawowych koncepcji][]
-   [Zagadnienia projektowe hostowana usługa][]
-   [Projektowanie aplikacji Skala][]
-   [Definicja usług hostingowych i konfiguracji][]
-   [Plik definicji usługi][]
-   [Plik konfiguracyjny usługi][]
-   [Tworzenie i wdrażanie usług hostingowych][]
-   [Odwołania][]

## <a id="benefits"> </a>Zalet modelu azure aplikacji

Podczas wdrażania aplikacji jako hostowana usługa Azure tworzy jeden lub więcej wirtualnych maszyn zawierających kod, aplikacji i uruchamiania maszyny wirtualne na komputerach fizycznie znajdujących się w jednym z centrami danych Azure. Żądania klienta do aplikacji hostowanej wprowadzania centrum danych, równoważenia obciążenia rozdziela te żądania do maszyny wirtualne. Gdy aplikacja znajduje się w Azure, otrzymuje trzy główne zalety:

-   **Wysokiej dostępności.** Wysoce oznacza, że Azure gwarantuje, że aplikacja działa w jak największym stopniu, a może reagować na żądania klientów. Jeśli aplikacji kończy (z powodu nieobsługiwanego wyjątku, na przykład), a następnie Azure wykryje to i będzie automatycznie ponowne uruchomienie aplikacji. Jeśli komputer aplikacja działa na środowiska awaria sprzętu, następnie Azure będzie również wykrywania automatyczne tworzenie nowych maszyn wirtualnych na innym komputerze fizycznie pracy i uruchomić kod w. Uwaga: Aby aplikacja uzyskanie umowy o poziomie usług firmy Microsoft % 99,95 dostępne, musi mieć co najmniej dwa maszyny wirtualne uruchomiony kodzie aplikacji. Dzięki temu jeden maszyn wirtualnych przetwarzania żądania klienta, gdy Azure przenosi kodu z uszkodzonego maszyn wirtualnych do nowego, warto maszyn wirtualnych.

-   **Skalowalność.** Azure pozwala na łatwe i dynamicznie zmienić liczbę maszyny wirtualne uruchomiony kodzie aplikacji do obsługi rzeczywiste obciążenie zostanie umieszczona w swojej aplikacji. Pozwala na dostosowanie aplikacji obciążenie pracą, który klientów umieszcza się nad nim podczas płacisz tylko za pośrednictwem SMS, potrzebujesz, gdy ich potrzebujesz. Jeśli chcesz zmienić liczbę maszyny wirtualne Azure odpowiada ciągu kilku minut, co umożliwia dynamicznie zmienić liczbę maszyny wirtualne działającego jako liczbę razy.

-   **Zarządzanie.** Ponieważ Azure jest platformą jako usługę (PaaS), zarządza infrastruktury (samego sprzętu, energii elektrycznej i sieci) wymagane do zachowania tych komputerów z systemem. Azure zarządza także platformę, zapewnić aktualność systemu operacyjnego z wszystkich poprawek poprawne i aktualizacje zabezpieczeń, a także aktualizacje składników, takich jak .NET Framework i Internet Information Server. Ponieważ maszyny wirtualne działają w systemie Windows Server 2008, Azure udostępnia dodatkowe funkcje, takie jak diagnostyczne monitorowania, obsługa pulpitu zdalnego, zapory i konfiguracja magazyn certyfikatów. Te funkcje są dostępne bez dodatkowych kosztów. W rzeczywistości po uruchomieniu aplikacji platformy Azure licencji systemu operacyjnego (OS) system Windows Server 2008 jest dołączany. Ponieważ wszystkie maszyny wirtualne działa system Windows Server 2008, cały kod, który jest uruchamiany w systemie Windows Server 2008 działa świetnie, gdy działa Azure.

## <a id="concepts"> </a>Hostowana usługa podstawowych koncepcji

Po wdrożeniu aplikacji jako hostowanej usługi Azure uruchamia jednego lub kilku *role.* *Rola* odnosi się do plików aplikacji i konfiguracji. Można zdefiniować jedną lub więcej ról dla aplikacji, każda z własny zestaw plików aplikacji i konfiguracji. Dla poszczególnych ról w aplikacji można określić liczbę maszyny wirtualne lub *wystąpienia roli*, aby uruchomić. Na poniższym rysunku pokazuj dwa przykłady prostych aplikacji zaprojektowany jako hostowanej usługi przy użyciu ról i wystąpienia roli.

##### <a name="figure-1-a-single-role-with-three-instances-vms-running-in-an-azure-data-center"></a>Rysunek 1: Roli jednej z trzech wystąpień (pośrednictwem SMS) w centrum danych Azure

![Obraz][0]

##### <a name="figure-2-two-roles-each-with-two-instances-vms-running-in-an-azure-data-center"></a>Rysunek 2: Dwie role, każda z dwóch wystąpień pośrednictwem (SMS), uruchamianie w centrum danych Azure

![Obraz][1]

Rola wystąpienia zwykle przetwarzać żądania klientów Internet wprowadzanie centrum danych, przez co nosi nazwę punktu *końcowego wprowadzania danych*. Pojedynczy rola może mieć 0 lub więcej wprowadzania punktów końcowych. Każdy punkt końcowy wskazuje protokół (HTTP, HTTPS lub TCP) i port. Często jest konfigurowania roli ma dwa wprowadzania punkty końcowe: HTTP nasłuchują na porty 80 i HTTPS nasłuchują na porcie 443. Na poniższym rysunku przedstawiono przykład dwa różne role użytkownika z różnych wprowadzania punkty końcowe kierujące żądania klienta do nich.

![Obraz][2]

Po utworzeniu usług hostingowych w Azure przypisano publicznie adresach adres IP, który klienci mogą używać do nich dostęp. Podczas tworzenia usług hostingowych możesz wybrać prefiks adresu URL, który jest zamapowany na ten adres IP. Ten prefiks muszą być unikatowe, jak są zasadniczo rezerwowania *Prefiks*. cloudapp.net adres URL, tak że nikt pogodzić. Klienci komunikować się z usługi wystąpienia roli za pomocą adresu URL. Będzie zwykle nie rozpowszechnianie lub publikowanie Azure *Prefiks*. cloudapp.net adresu URL. Zamiast tego będzie Kup nazwę DNS u rejestratora DNS wybór i skonfiguruj swoją nazwę DNS, aby przekierować żądania klienta do adresu URL Azure. Aby uzyskać więcej informacji zobacz [Konfigurowanie nazwy domeny niestandardowej w Azure][].

## <a id="considerations"> </a>Hostowana usługa zagadnienia projektowe

Podczas projektowania aplikacji do uruchamiania w środowisku chmury, istnieje kilka zagadnień ułatwiających wziąć pod uwagę, takich jak czas oczekiwania, wysokiej dostępności i skalowalność.

Decydowanie o lokalizacji kodzie aplikacji jest ważne, gdy działa hostowana usługa Azure. Są często wdrażania aplikacji do centrów dane, które znajdują się najbliżej klientów, aby zmniejszyć opóźnienie i uzyskać najlepszą wydajność możliwe.
Jednak możesz wybrać centrum danych bliżej Twojej firmy lub bliżej danych, jeśli masz kilka jurysdykcyjnych lub prawne wątpliwości danych i miejsce, w którym się ona znajduje. Istnieje sześć centrach danych na całym świecie możliwością obsługi kodzie aplikacji. W poniższej tabeli przedstawiono dostępne lokalizacje:

<table border="2" cellspacing="0" cellpadding="5" style="border: 2px solid #000000;">
<tbody>
<tr>
<td style="width: 100px;">
**Kraj/Region**

</td>
<td style="width: 200px;">
**Regionu**

</td>
</tr>
<tr>
<td>
Stany Zjednoczone

</td>
<td>
Południowej centralnego i Ameryka Północna, środkowa

</td>
</tr>
<tr>
<td>
Europa

</td>
<td>
Ameryka Północna i zachód

</td>
</tr>
<tr>
<td>
Kraje Azji

</td>
<td>
Kopiec i wschód

</td>
</tr>
</tbody>
</table>
Podczas tworzenia usług hostingowych, zaznacz obszar podrzędny wskazujące lokalizację, w której chcesz kodu do wykonania.

Aby osiągnąć wysoką dostępność i skalowalność, jest bardzo ważne, przechowywanie danych aplikacji w centralnym repozytorium dostępne dla wielu wystąpień roli. Aby pomóc z tym, Azure oferuje kilka opcji miejsca do magazynowania przykład obiektów blob, tabele i baza danych SQL. Zobacz artykuł [Ofertą miejsca do magazynowania danych platformy Azure][] , aby uzyskać więcej informacji na temat tych technologii miejsca do magazynowania. Na poniższym rysunku przedstawiono, jak równoważenia obciążenia w centrum danych Azure rozdzielanie żądań klientów wystąpieniach inną rolę, które mają dostęp do tego samego magazynu danych.

![Obraz][3]

Zazwyczaj chcesz zlokalizować kodzie aplikacji i danych w tym samym centrum danych, jak dzięki temu krótki czas oczekiwania (lepszą wydajność) po kodzie aplikacji uzyskuje dostęp do danych. Ponadto nie zajmujesz przepustowości podczas przenoszenia danych w tym samym centrum danych.

## <a id="scale"> </a>Projektowania aplikacji Skala

Czasami warto wykonać jedną aplikację (na przykład prostej witryny sieci web) i jest obsługiwany w Azure. Ale często, aplikacja może się składać z kilku ról że współpracują ze sobą. Na przykład na poniższej ilustracji istnieją dwa wystąpienia roli witryny sieci Web, trzy wystąpienia roli kolejności przetwarzania i jedno wystąpienie roli Generator raportów. Role te wszystkie wspólnie, a kod każdy z nich mogą być spakowanych razem, a wdrożony jako całość maksymalnie Azure.

![Obraz][4]

Głównym powodem dzielenie aplikacji do różne role, każdy pracuje na własny zestaw wystąpień roli (czyli maszyny wirtualne) jest niezależne skalowanie role. Na przykład podczas świąt, wielu klientów może być zakupu produktów z Twojej firmy, można zwiększyć liczbę wystąpień roli roli użytkownika witryny sieci Web, a także liczbę wystąpień roli działa rola kolejności przetwarzania. Po świąt może zostać wyświetlony wiele produktów zwróconych, więc nadal należy wiele wystąpienia witryny sieci Web, ale mniejsza liczba wystąpień kolejności przetwarzania. W dalszej części roku konieczne może być kilka witrynę sieci Web i kolejności przetwarzania. W całości tego może być konieczne tylko jedno wystąpienie Generator raportów. Elastyczność oparta na rolach wdrażania Azure umożliwia łatwe dostosowania aplikacji do potrzeb firmy.

Są często ma rolę, którą wystąpienia w ramach usługi hostowanej komunikowania się między sobą. Na przykład roli witryny sieci Web akceptuje zamówienia danego klienta, ale następnie je offloads przetwarzania wystąpieniach roli kolejności przetwarzania zamówień. Najlepszym sposobem przekazać jeden zestaw wystąpień roli na inny zestaw wystąpień jest przy użyciu technologii Kolejkowanie dostarczony przez Azure, formularz pracy usługi kolejki albo kolejek Bus usług. Stosowanie kolejki jest kluczową sekcji poniżej. Kolejka umożliwia usług hostingowych przeskalować jego role umożliwiające niezależne równoważenia obciążenia przed koszt. Jeśli liczba wiadomości w kolejce zwiększa w czasie, można skalować liczby kolejności przetwarzania wystąpienia roli. Jeśli w czasie zmniejszy się liczba wiadomości w kolejce, można skalować liczbę wystąpień kolejności przetwarzania ról w dół. Dzięki temu tylko realizujesz wystąpień wymagany do obsługi obciążenie pracą rzeczywistą.

Kolejka zawiera również niezawodności. Podczas skalowania liczbę wystąpień kolejności przetwarzania roli, Azure zdecyduje, które wystąpienia, aby zakończyć. Może podjąć decyzję zakończyć wystąpienia, które znajduje się w środku przetwarzania wiadomości kolejki. Jednak ponieważ przetwarzania wiadomości nie zostanie ukończona pomyślnie, wiadomość jest widoczna ponownie w innej kolejności przetwarzania roli wystąpienia, które go przejmuje i przetwarza je. Ze względu na widoczność wiadomości kolejki wiadomości zapewniające ostatecznie przetwarzanie. Kolejka pełnić rolę usługi równoważenia obciążenia skuteczne przekazując swoje wiadomości do roli wszystkich wystąpień, które poproś wiadomości.

Dla wystąpienia roli witryny sieci Web, można monitorować ruch wkrótce do nich i zdecydować przeskalować liczbę w górę lub w dół, a także. Kolejka umożliwia przeskalować liczbę wystąpień roli witrynę sieci Web niezależnie od wystąpienia roli kolejności przetwarzania. To jest bardzo zaawansowanym i zapewnia dużą elastyczność. Oczywiście aplikacja składa się z dodatkowych ról, można dodać dodatkowe kolejki jako kanał do komunikowania się między nimi, aby można było korzystać z tym samym skalowania i kosztów korzyści.

## <a id="defandcfg"> </a>Hostowanej definicji usługi i konfiguracji

Wdrażanie hostowana usługa Azure wymaga również plik definicji usługi i plik konfiguracyjny usługi. Obydwa typy plików są plikami XML i umożliwiają deklaratywnie określić opcje wdrażania usługi hostowanej. Plik definicji usługi w tym artykule opisano wszystkie role, które składają się z usług hostingowych i sposób komunikacji. Plik konfiguracyjny usługi opisuje liczbę wystąpień dla poszczególnych ról i ustawień umożliwia konfigurowanie każde wystąpienie roli.

## <a id="def"> </a>Pliku definicji usługi

Jak mogę wspomniano wcześniej, definicji usługi (CSDEF) plik znajduje się plik XML w tym artykule opisano różne role, które składają się na pełną aplikację. Ukończono schematu pliku XML można znaleźć tutaj: [[http://msdn.microsoft.com/library/windowsazure/ee758711.aspx]].
Plik CSDEF zawiera element WebRole lub WorkerRole dla poszczególnych ról w aplikacji. Wdrażanie roli rolę sieci web (za pomocą elementu WebRole) oznacza, że kod będzie działać wystąpienia roli zawierające Internet Information Server (IIS) i systemu Windows Server 2008.
Wdrażanie roli jako roli pracownika (za pomocą elementu WorkerRole) oznacza, że wystąpienie roli będzie miał systemu Windows Server 2008 na nim (usług IIS nie zostanie zainstalowana).

Na pewno można tworzyć i wdrażać rolę pracownika, która korzysta z niektórych innego mechanizmu Aby odsłuchać dla przychodzących żądań sieci web (na przykład kod może tworzenie i używanie .NET HttpListener). Ponieważ wystąpienia roli są systemem Windows Server 2008, kodzie mogą wykonywać żadnych operacji, które są dostępne dla aplikacji uruchomionej w systemie Windows Server
2008.

W przypadku każdej roli oznacza odpowiedniej rozmiar pamięci Wirtualnej, który ma być używany wystąpienia tej roli. W poniższej tabeli przedstawiono różnych rozmiarach maszyn wirtualnych dostępne już dziś i atrybuty każdego:

<table border="2" cellspacing="0" cellpadding="5" style="border: 2px solid #000000;">
<tbody>
<tr align="left" valign="top">
<td>
**Rozmiar pamięci Wirtualnej**

</td>
<td>
**PROCESOR**

</td>
<td>
**PAMIĘCI RAM**

</td>
<td>
**Dysku**

</td>
<td>
**Alokacja maksymalna sieciowe We/Wy**

</td>
</tr>
<tr align="left" valign="top">
<td>
**Bardzo mały**

</td>
<td>
1,0 1 GHz

</td>
<td>
768 MB

</td>
<td>
20GB

</td>
<td>
\~5 MB/s

</td>
</tr>
<tr align="left" valign="top">
<td>
**Małe**

</td>
<td>
1 procesory 1,6 GHz

</td>
<td>
1,75 GB

</td>
<td>
225GB

</td>
<td>
\~100 MB/s

</td>
</tr>
<tr align="left" valign="top">
<td>
**Średnia**

</td>
<td>
2 procesory 1,6 GHz

</td>
<td>
3,5 GB

</td>
<td>
490GB

</td>
<td>
\~200 MB/s

</td>
</tr>
<tr align="left" valign="top">
<td>
**Duży**

</td>
<td>
4 procesory 1,6 GHz

</td>
<td>
7 GB

</td>
<td>
1TB

</td>
<td>
\~400 MB/s

</td>
</tr>
<tr align="left" valign="top">
<td>
**Bardzo duży**

</td>
<td>
8 procesory 1,6 GHz

</td>
<td>
14 GB

</td>
<td>
2TB

</td>
<td>
\~800 MB/s

</td>
</tr>
</tbody>
</table>
Zajmujesz co godzinę dla każdego maszyn wirtualnych użyć jako wystąpienie roli i są również opłatę za wszelkie dane czy wystąpienia do roli wysyłać poza centrum danych. Możesz nie są naliczane danych wprowadzanie centrum danych. Aby uzyskać więcej informacji zobacz [Azure ceny] []. Ogólnie zaleca się używanie wielu wystąpień małych roli zamiast kilka wystąpień dużych tak, aby aplikacja jest bardziej elastyczne awarii. Po zakończeniu masz mniej wystąpienia roli, więcej katastrofalny a błąd w jednym z nich jest ogólny aplikacji. Ponadto wymienione przed należy wdrożyć co najmniej dwa wystąpienia dla każdej roli aby można było uzyskiwać 99,95% Umowa dotycząca poziomu usług, które firma Microsoft świadczy.

Plik definicji (CSDEF) usługi jest również miejsce, w którym chcesz określić wiele atrybutów każdej roli w aplikacji. Poniżej przedstawiono niektóre elementy przydatności dla Ciebie dostępne:

-   **Certyfikaty**. Program certyfikaty szyfrowania danych lub jeśli usługa sieci web obsługuje protokół SSL. Wszystkie certyfikaty należy przekazać do Azure. Aby uzyskać więcej informacji zobacz [Zarządzanie certyfikatów w Azure][]. To ustawienie XML instaluje certyfikaty przekazane wcześniej w magazynie certyfikatów wystąpienie roli, tak aby były użyteczne w kodzie aplikacji.

-   **Nazwy ustawienia konfiguracji**. Wartości, które chcesz z aplikacji do czytania podczas uruchamiania wystąpienia roli. Rzeczywista wartość ustawienia konfiguracji znajduje się w pliku konfiguracji (CSCFG) usług, które można aktualizować w dowolnym momencie bez konieczności ponownego wdrażania kodu. Może w rzeczywistości kod aplikacji w taki sposób, wykrywanie wartości konfiguracji zmienione bez ponoszenia dowolnego przestoje.

-   **Wprowadzanie punktów końcowych**. Tutaj określić dowolną HTTP, HTTPS lub TCP punkty końcowe (z portów), które chcesz udostępnić ze światem za pośrednictwem usługi *Prefiks*. cloadapp.net adresu URL. Po Azure wdrożono roli użytkownika, jego skonfiguruje zapory na wystąpienie roli automatycznie.

-   **Wewnętrzna punktów końcowych**. Tutaj możesz określić dowolną HTTP lub port TCP punkty końcowe odpowiednie na inne wystąpienia roli, wdrożonych jako część aplikacji. Wewnętrzna punkty końcowe Zezwalaj na wszystkie wystąpienia ról w aplikacji aby porozmawiać ze sobą, ale nie są dostępne dla wystąpienia roli znajdujących się poza aplikacji.

-   **Moduły importu**. Zainstaluj te opcjonalnie przydatne składniki na wystąpienia do roli. Składniki istnieje diagnostyczne monitorowania, pulpitu zdalnego i Azure nawiązywanie połączenia (co pozwala wystąpienia roli dostępu do lokalnych zasobów za pośrednictwem bezpiecznego kanału).

-   **Lokalne przechowywanie**. To przydziela podkatalogów na wystąpienie roli dla aplikacji używać. Opisano bardziej szczegółowo w artykule [Ofertą miejsca do magazynowania danych platformy Azure][] .

-   **Uruchamianie zadania**. Uruchamianie zadania umożliwiają umożliwia instalowanie wstępne składników na wystąpienia roli, jak uruchomi się w górę. Zadania można uruchomić z pełnymi uprawnieniami jako administrator w razie potrzeby.

## <a id="cfg"> </a>Plik konfiguracyjny usługi

Plik konfiguracyjny (CSCFG) usługi jest plikiem XML, który opisano ustawienia, które mogą być zmieniane bez ponownego wdrożenia aplikacji. Ukończono schematu pliku XML można znaleźć tutaj: [http://msdn.microsoft.com/library/windowsazure/ee758710.aspx][].
Plik CSCFG zawiera element ról dla każdej roli w aplikacji. Oto niektóre elementy, które można określić w pliku CSCFG:

-   **Wersja systemu operacyjnego**. Ten atrybut pozwala wybrać wersję systemu operacyjnego (OS), odpowiednie używane dla wszystkich wystąpień roli działa kodzie aplikacji. Ten system operacyjny jest znany jako *Gość systemu operacyjnego*, a każda nowa wersja obejmuje najnowsze poprawki zabezpieczeń i aktualizacje dostępne w chwili wydania gościa systemu operacyjnego. Jeśli ustawisz wartość atrybutu element osVersion "\*", a następnie Azure automatycznie aktualizuje gościa OS na wszystkich wystąpień swojej roli jako nowe wersje systemu operacyjnego gościa są dostępne. Jednak można zaprzestać korzystania z aktualizacji automatycznych, wybierając Gość określonej wersji systemu operacyjnego. Na przykład ustawienie atrybutu element osVersion wartość "OS 2,8, WA GOŚCIA w-\_201109-01" powoduje, że wszystkie wystąpienia programu roli uzyskanie opisano w tej witrynie: [http://msdn.microsoft.com/library/hh560567.aspx][]. Aby uzyskać więcej informacji o wersjach systemu operacyjnego gościa zobacz [Zarządzanie uaktualnienia do systemu operacyjnego gości Azure].

-   **Wystąpienia**. Wartość tego elementu wskazuje liczbę wystąpień roli, które ma być ustanawianie wykonywanie kodu dla konkretnej roli. Ponieważ możesz przekazać nowy plik CSCFG do Azure (bez ponownego wdrożenia aplikacji), jest proste trivially Zmień wartość dla tego elementu, a następnie Przekaż nowy plik CSCFG aby dynamicznie zwiększyć lub zmniejszyć liczbę wystąpień roli działa kodzie aplikacji. Pozwala na łatwe skalowanie aplikacji w górę lub w dół do wymaganego spełniają obciążenia rzeczywiste podczas również sterowanie, ile naliczanego za usługę uruchomione wystąpienia roli.

-   **Wartość ustawienia konfiguracji**. Ten element wskazuje wartości ustawień (zdefiniowana w pliku CSDEF). Twoja rola można przeczytać te wartości, gdy jest uruchomiony. Te wartości ustawienia konfiguracji są zwykle używane do parametry połączenia bazy danych SQL lub magazyn Azure, ale może służyć do celów oczekiwane.

## <a id="hostedservices"> </a>Tworzenie i wdrażanie usług hostingowych

Tworzenie usług hostingowych wymaga najpierw przejdź do [Portalu zarządzania Azure] i świadczenie usług hostingowych, określając prefiks DNS i centrum danych ostatecznie chcesz w kodzie. Następnie w środowisku rozwoju możesz utworzyć plik definicji (CSDEF) usługi, tworzenie kod aplikacji, a wszystkie te pliki pakietu (zip) do pliku pakietu (CSPKG) usługi. Należy również przygotować usługi pliku konfiguracji (CSCFG). Aby wdrożyć roli użytkownika, możesz przekazać pliki CSPKG i CSCFG z interfejsem API zarządzania usługi Azure. Po wdrożeniu Azure, będzie obsługa administracyjna wystąpienia rolę w centrum danych (na podstawie danych konfiguracji), Wyodrębnij kodzie aplikacji z pakietu, skopiować go do roli wystąpienia i uruchamiania wystąpień. Teraz kodu jest i rozpocząć pracę.

Na poniższym rysunku przedstawiono pliki CSPKG i CSCFG, utworzonego na komputerze dewelopera. Plik CSPKG zawiera plik CSDEF oraz kod dwie role. Po przekazaniu pliki CSPKG i CSCFG z interfejsem API zarządzania usługi Azure, Azure tworzy wystąpienia rolę w centrum danych. W tym przykładzie plik CSCFG wskazuje, że Azure należy utworzyć trzy wystąpienia roli \#1 i dwóch wystąpień roli \#2.

![Obraz][5]

Aby uzyskać więcej informacji na temat wdrażania uaktualnianie i ponowne konfigurowanie poszczególnych ról, zobacz artykuł [Wdrażanie i aktualizowanie aplikacji Azure][] .<a id="Ref" name="Ref"></a>

## <a id="references"> </a>Odwołania

-   [Tworzenie hostowanej usługi dla Azure][]

-   [Zarządzanie usługami obsługiwane platformy Azure][]

-   [Migrowanie aplikacjom Azure][]

-   [Konfigurowanie aplikacji Azure][]

<div style="width: 700px; border-top: solid; margin-top: 5px; padding-top: 5px; border-top-width: 1px;">

<p>Napisane przez i Richter (Wintellect)</p>

</div>

  [Zalety modelu Azure aplikacji]: #benefits
  [Hostowana usługa podstawowych koncepcji]: #concepts
  [Zagadnienia projektowe hostowana usługa]: #considerations
  [Projektowanie aplikacji Skala]: #scale
  [Definicja usług hostingowych i konfiguracji]: #defandcfg
  [Plik definicji usługi]: #def
  [Plik konfiguracyjny usługi]: #cfg
  [Tworzenie i wdrażanie usług hostingowych]: #hostedservices
  [Odwołania]: #references
  [0]: ./media/application-model/application-model-3.jpg
  [1]: ./media/application-model/application-model-4.jpg
  [2]: ./media/application-model/application-model-5.jpg
  [Konfigurowanie niestandardowej nazwy domeny w Azure]: http://www.windowsazure.com/develop/net/common-tasks/custom-dns/
  [Oferty miejsca do magazynowania danych platformy Azure]: http://www.windowsazure.com/develop/net/fundamentals/cloud-storage/
  [3]: ./media/application-model/application-model-6.jpg
  [4]: ./media/application-model/application-model-7.jpg
  
  [Azure Pricing]: http://www.windowsazure.com/pricing/calculator/
  [Zarządzanie certyfikaty platformy Azure]: http://msdn.microsoft.com/library/windowsazure/gg981929.aspx
  [http://msdn.microsoft.com/library/windowsazure/ee758710.aspx]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
  [http://msdn.microsoft.com/library/hh560567.aspx]: http://msdn.microsoft.com/library/hh560567.aspx
  [Zarządzanie uaktualnienia dla gości Azure systemu operacyjnego]: http://msdn.microsoft.com/library/ee924680.aspx
  [Portal Azure zarządzania]: http://manage.windowsazure.com/
  [5]: ./media/application-model/application-model-8.jpg
  [Wdrażanie i aktualizowanie aplikacji Azure]: http://www.windowsazure.com/develop/net/fundamentals/deploying-applications/
  [Tworzenie hostowanej usługi dla Azure]: http://msdn.microsoft.com/library/gg432967.aspx
  [Zarządzanie usługami obsługiwane platformy Azure]: http://msdn.microsoft.com/library/gg433038.aspx
  [Migrowanie aplikacjom Azure]: http://msdn.microsoft.com/library/gg186051.aspx
  [Konfigurowanie aplikacji Azure]: http://msdn.microsoft.com/library/windowsazure/ee405486.aspx
