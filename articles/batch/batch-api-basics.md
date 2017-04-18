<properties
    pageTitle="Omówienie funkcji usługi Azure partii dla deweloperów | Microsoft Azure"
    description="Dowiedz się, funkcje usługi partii i jego interfejsy API z punktu widzenia rozwoju."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="09/29/2016"
    ms.author="marsma"/>

# <a name="batch-feature-overview-for-developers"></a>Omówienie funkcji partii dla deweloperów

W tym omówienie podstawowe składniki usługi Azure partii omówimy funkcji podstawowej usługi i zasoby, które partii deweloperzy mogą używać do tworzenia dużych równolegle obliczyć rozwiązań.

Czy opracowywania aplikacji rozproszonej obliczeniowych lub usługa problemy bezpośredni [Interfejsu API usługi REST] [ batch_rest_api] połączenia lub korzystasz z jednej [Partii SDK](batch-technical-overview.md#batch-development-apis), za pomocą wielu zasobów i funkcje omówione w tym artykule.

> [AZURE.TIP] Wyższego poziomu wprowadzenie do usługi partii zobacz [Podstawy partia Azure](batch-technical-overview.md).

## <a name="batch-service-workflow"></a>Przepływ pracy usługi partii

Następujące wysokiego poziomu przepływ pracy jest typowe niemal wszystkich aplikacji i usług, które korzystają z usługi partii przetwarzania równoległego obciążenia:

1. Przekazywanie **plików danych** przetwarzania [Magazyn Azure] [ azure_storage] konta. Partia zawiera wbudowaną obsługę uzyskiwanie dostępu do magazyn obiektów Blob platformy Azure i zadań można pobrać te pliki do [obliczenia węzły](#compute-node) uruchomienie zadania.

2. Przekaż **pliki aplikacji** zadań będzie działać. Te pliki mogą być plików binarnych lub skrypty i ich zależności i są wykonywane przez zadania w zadań. Zadania można pobrać te pliki z Twojego konta miejsca do magazynowania lub za pomocą funkcji [pakietów aplikacji](#application-packages) partii zarządzania aplikacjami i wdrażania.

3. Tworzenie [puli](#pool) węzłów obliczeń. Po utworzeniu puli Określ liczby węzłów obliczeń dla puli, jej rozmiar i system operacyjny. Po uruchomieniu każdego zadania w pracy przypisał do wykonania na jednym z węzłów na użytkownika.

4. Tworzenie [zadania](#job). Zadanie zarządza zbioru zadań. Kojarzenie każdego zadania do określonych puli, gdzie spowoduje uruchomienie tego zadania.

5. Dodawanie [zadań](#task) do zadania. Każde zadanie uruchamia aplikacji lub skrypt, które zostały przekazane do procesu pliki danych, które pobierania go z kontem miejsca do magazynowania. Jak ukończeniu każdego zadania go przekazać jej wyniki do magazynowania Azure.

6. Monitorowanie postępu zadania i Pobierz dane wyjściowe zadania z magazynu Azure.

W poniższych sekcjach omówiono te i inne zasoby partii, umożliwiające rozłożone rozwiązania obliczeniowych.

> [AZURE.NOTE] Wymagane jest [konto partii](batch-account-create-portal.md) do korzystania z usługi partii. Ponadto niemal wszystkich rozwiązań za pomocą [Magazyn Azure] [ azure_storage] konto do przechowywania plików i pobierania. Partii obecnie obsługuje tylko **uniwersalny** miejsca do magazynowania typ konta, zgodnie z opisem w kroku 5, [Utwórz konto miejsca do magazynowania](../storage/storage-create-storage-account.md#create-a-storage-account) w [o Azure miejsca do magazynowania konta](../storage/storage-create-storage-account.md).

## <a name="batch-service-resources"></a>Zasoby usługi partii

Niektóre z poniższych zasobów — kont, wyliczenia węzły, pul, zadań i zadań — są wymagane przez wszystkich rozwiązań, które korzystają z usługi partii. Inne, takich jak harmonogramy zadań i pakietów aplikacji są przydatne, ale opcjonalny, funkcje.

- [Konta](#account)
- [Obliczenia węzła](#compute-node)
- [Pula](#pool)
- [Zadanie](#job)

  - [Harmonogramy zadań](#scheduled-jobs)

- [Zadanie](#task)

  - [Rozpoczynanie zadania](#start-task)
  - [Menedżer zadania](#job-manager-task)
  - [Przygotowanie i wersji zadania](#job-preparation-and-release-tasks)
  - [Wielu wystąpień zadania (MPI)](#multi-instance-tasks)
  - [Zależności między zadaniami](#task-dependencies)

- [Pakiety aplikacji](#application-packages)

## <a name="account"></a>Konta

Konto partii to podmiot jednoznacznie zdefiniowane w usłudze partię. Przetwarzanie wszystkich jest skojarzone z kontem partię. Podczas wykonywania operacji z usługą partii, należy zarówno nazwę konta, a w jednym z jego kluczy konta. Możesz [utworzyć konto partii Azure za pomocą Azure portal](batch-account-create-portal.md).

## <a name="compute-node"></a>Obliczenia węzła

Węzeł obliczeń jest Azure maszyn wirtualnych (maszyn wirtualnych), służącego do przetwarzania część aplikacji pracą. Rozmiar węzła określa liczbę rdzenie Procesora, pamięci i rozmiaru system lokalny plik przydzielona do węzła. Możesz utworzyć pul węzłów systemu Windows i Linux oraz przy użyciu usług w chmurze Azure lub Marketplace maszyn wirtualnych obrazów. Zobacz następną sekcję [puli](#pool) , aby uzyskać więcej informacji na temat tych opcji.

Węzły można uruchamiać dowolny plik wykonywalny lub skrypt, który jest obsługiwany przez środowiska systemu operacyjnego węzła. Ta opcja uwzględnia \*.exe, \*cmd, \*bat i skryptów programu PowerShell dla systemu Windows — i danych binarnych, szkielet i Python skrypty Linux.

Wszystkie wyliczenia węzły w partii również zawierać:

- Standardowy [Struktura folderów](#files-and-directories) i skojarzone [Zmienne środowiska](#environment-settings-for-tasks) dostępnych dla odwołanie do zadania.
- Ustawienia **zapory** , które są skonfigurowane do kontrolowania dostępu.
- [Dostęp zdalny](#connecting-to-compute-nodes) do systemu Windows (protokół RDP (Remote Desktop)) i węzły Linux (Secure Shell (SSH)).

## <a name="pool"></a>Pula

Puli to zbiór węzłów, które aplikacja działa na. Puli można tworzyć ręcznie przez użytkownika lub automatycznie przez usługę partii po określeniu pracy. Można tworzyć i zarządzać puli, która spełnia wymagań dotyczących zasobów aplikacji. Puli można używać tylko przy użyciu konta partii, w której został utworzony. Konto partii może zawierać więcej niż jednej puli.

Azure pul partii oparte na platformie Azure obliczeń podstawowego. Zapewniają dużych alokacji, instalowania aplikacji dystrybucji danych, monitorowanie kondycji i elastycznej liczby węzłów obliczeń w puli ([Skalowanie](#scaling-compute-resources)).

Każdy dodawany do puli węzeł otrzymuje unikatową nazwę i adres IP. Po usunięciu węźle z puli wszelkie zmiany wprowadzone w systemie operacyjnym lub pliki zostaną utracone, a jego nazwę i adres IP wydawanych do użycia w przyszłości. Gdy węzeł opuści puli, ważności został przekroczony.

Po utworzeniu puli, można określić następujące atrybuty:

- Obliczenia węzła **systemu operacyjnego** i **wersji**

    Istnieją dwie opcje, po wybraniu system operacyjny dla węzłów w puli użytkownika: **Konfiguracja maszyn wirtualnych** i **Konfiguracja usług w chmurze**.

    **Konfiguracja maszyna wirtualna** zawiera obrazy zarówno systemu Windows, jak i Linux oraz dla obliczenia węzły z [Azure Marketplace maszyn wirtualnych][vm_marketplace].
    Po utworzeniu puli, która zawiera węzły konfigurację maszyny wirtualnej, musisz określić nie tylko rozmiar węzły, ale również **Odwołanie maszyn wirtualnych** i partii **agenta węzeł SKU** musi być zainstalowany na węzłach. Aby uzyskać więcej informacji na temat określania tych właściwości puli zobacz [Linux należy obliczyć węzłów na pul partii Azure](batch-linux-nodes.md).

    **Konfiguracja usługi chmury** zawiera Windows węzły *tylko*obliczenia. Dostępne systemy operacyjne puli Konfiguracja usług w chmurze są widoczne w [wersjach systemu operacyjnego gościa Azure i SDK zgodności macierzy](../cloud-services/cloud-services-guestos-update-matrix.md). Po utworzeniu puli, która zawiera węzły usług w chmurze, musisz określić rozmiar węzeł i jego *Rodzina systemu operacyjnego*. Podczas tworzenia pul systemu Windows obliczyć węzły, najczęściej korzystasz z usług w chmurze.

    - *Rodzina OS* określa także, które wersje programu .NET są zainstalowane z systemem operacyjnym.
    - Jak pracownik ról w ramach usług w chmurze, możesz określić *Wersja systemu operacyjnego* (Aby uzyskać więcej informacji dotyczących ról pracownik, zobacz [Powiedz mi o usług w chmurze](../cloud-services/cloud-services-choose-me.md#tell-me-about-cloud-services) sekcji [Omówienie usług w chmurze](../cloud-services/cloud-services-choose-me.md)).
    - Zgodnie z rolami pracownik, zalecamy, możesz określić `*` dla *Wersji systemu operacyjnego* , aby węzły są uaktualniane automatycznie, a istnieje nie pracy wymaganej do zaspokojenia do nowo wydane wersje. W przypadku użycia podstawowego Zaznaczanie określonej wersji systemu operacyjnego jest zapewnienie zgodności aplikacji, co pozwala zgodność z poprzednimi wersjami dokonywane przed zezwoleniem wersji mają być aktualizowane. Po weryfikacji, *Wersja systemu operacyjnego* puli mogą być aktualizowane i nowy obraz systemu operacyjnego może być zainstalowana — wszystkie uruchomione zadania są przerwane i niepełniącego.

- **Rozmiar węzłów**

    **Konfiguracja usług w chmurze** obliczeń węzeł rozmiary są widoczne w [rozmiarów usług w chmurze](../cloud-services/cloud-services-sizes-specs.md). Partia obsługuje wszystkie rozmiary usług w chmurze, z wyjątkiem `ExtraSmall`.

    **Konfiguracja maszyn wirtualnych** obliczyć węzeł, którego rozmiary są widoczne w [rozmiarów maszyn wirtualnych w Azure](../virtual-machines/virtual-machines-linux-sizes.md) (Linux) i [rozmiarów maszyn wirtualnych w Azure](../virtual-machines/virtual-machines-windows-sizes.md) (Windows). Partia obsługuje wszystkie rozmiary maszyn wirtualnych Azure, z wyjątkiem `STANDARD_A0` pochodzącymi premium przestrzeni dyskowej (`STANDARD_GS`, `STANDARD_DS`, i `STANDARD_DSV2` serii).

    Podczas wybierania rozmiaru węzła obliczeń, należy rozważyć, czy właściwości i wymagania dotyczące aplikacji, z którego jest uruchamiany w węzłach. Aspekty, takie jak czy aplikacja jest wielowątkowe i ilości pamięci wymaga może pomóc określić rozmiar węzeł najbardziej odpowiednich i skutecznych. Jest najczęściej wybierz rozmiar węzeł zakładając, że jedno zadanie zostanie uruchomiona w węźle naraz. Jednak użytkownik może mieć wiele zadań (i w związku z tym wielu wystąpień aplikacji) [równolegle](batch-parallel-node-tasks.md) na obliczanie węzły podczas wykonywania zadania. W tym przypadku jest wspólne dla wybierz większy rozmiar węzeł, aby zezwalały na zwiększone zapotrzebowanie wykonywanie zadań równoległych. Aby uzyskać więcej informacji, zobacz [Zasady planowania zadań](#task-scheduling-policy) .

    Wszystkie węzły w puli mają taki sam rozmiar. Jeśli zamierzasz uruchamianie aplikacji z różne wymagania systemowe i/lub ładowanie poziomów, zaleca użycie oddzielnych pul.

- **Docelowej liczby węzłów**

    To liczby węzłów obliczeń, które chcesz wdrożyć w puli. To jest określane jako *docelowej* ponieważ w niektórych sytuacjach puli użytkownika nie może być osiągnięcia odpowiedniej liczby węzłów. Puli nie może być osiągnięcia odpowiedniej liczby węzłów, jeśli osiągnie ona [core przydziału](batch-quota-limit.md#batch-account-quotas) dla Twojego konta partii — lub jeśli jest formułą automatyczne skalowanie zastosowany do puli, który ogranicza maksymalną liczbę węzłów (zobacz następną sekcję "Skalowania zasad").

- **Skalowanie zasad**

    Oprócz Określanie statyczne liczby węzłów, możesz zamiast tego pisanie i zastosować [Automatyczne skalowanie formułę](#scaling-compute-resources) do puli. Usługa partii okresowo wynikiem formuły i dopasowuje liczby węzłów w puli na podstawie różnych puli, zadania i parametry zadania, które można określić.

- **Zasady planowania zadań**

    Opcja konfiguracji [max zadania na węzeł](batch-parallel-node-tasks.md) określa maksymalną liczbę zadań, które mogą być uruchamiane równolegle w każdym węźle obliczeń w puli.

    Konfiguracji domyślnej to, że jedno zadanie naraz jest uruchamiany w węźle, ale istnieją scenariusze jest miejsce, w którym masz więcej niż jednego zadania wykonywane w węźle jednocześnie. Zobacz [przykładu](batch-parallel-node-tasks.md#example-scenario) w artykule [równoczesne węzeł zadania](batch-parallel-node-tasks.md) , aby zobaczyć, jak można korzystać z wielu zadań na węzeł.

    Można również określić *Typ wypełnienia* , która określa, czy partia strony widzące zadania równomiernie we wszystkich węzłach w puli, czy pakiety dla każdego węzła z maksymalną liczbę zadań przed przydzielania zadań do innego węzła.

- **Stan komunikacji** węzłów obliczeń

    W większości scenariuszy zadania działają niezależnie i nie ma potrzeby komunikowanie się ze sobą. Istnieją jednak niektóre aplikacje, w których przekazuje zadań, takich jak [MPI scenariuszy](batch-mpi.md).

    Możesz skonfigurować puli, aby umożliwić komunikacji między węzłów w ramach it —**komunikacji między węzłami**. Po włączeniu komunikacji między węzłami węzłach Cloud Services konfigurację pul można komunikować się ze sobą portów większa niż 1100 i pule konfiguracji komputera wirtualnych nie Ograniczaj ruch na dowolnym porcie.

    Zauważ, że włączenie komunikacji między węzłami również wpływa na położenie węzłów w ramach klastrów i może ograniczyć maksymalną liczbę węzłów w puli z powodu ograniczeń wdrożenia. Jeśli aplikacja nie wymaga komunikacji między węzły, usługę partii może przydzielić potencjalnie dużej liczby węzłów puli z wielu różnych klastrów i centrach danych umożliwiające zwiększenie power przetwarzanie równoległe.

- W przypadku węzły obliczeń **Rozpoczęcie zadania**

    Opcjonalnie *uruchomić zadania* wykonuje w każdym węźle, zgodnie z tym węźle łączy puli i w każdym węźle ponowne uruchomienie lub reimaged. Zadanie początkowe jest szczególnie przydatne do przygotowania węzły obliczeń wykonywania zadań, takich jak instalowanie aplikacje, których zadań w węzłach obliczeń.

- **Pakiety aplikacji**

    Możesz określić [pakietów aplikacji](#application-packages) do wdrożenia węzły obliczeń w puli. Pakiety aplikacji zapewniają uproszczone rozmieszczanie i przechowywanie wersji aplikacje, których zadań. Pakiety aplikacji, określające puli są zainstalowane na każdym węźle, który łączy tej puli, a co czas węzeł jest ponownie uruchomić lub reimaged. Pakiety aplikacji obecnie są obsługiwane w węzłach obliczeń Linux.

- **Konfiguracja sieci**

    Możesz określić identyfikator Azure [wirtualnej sieci (VNet)](../virtual-network/virtual-networks-overview.md) w jakiej w puli obliczyć węzły ma zostać utworzona. Wymagania dotyczące określania VNet dla użytkownika można znaleźć w [Dodaj puli z klientem] [ vnet] odwołania interfejsu API usługi REST partię.

> [AZURE.IMPORTANT] Wszystkie konta partii mają domyślny **Przydział** ogranicza liczbę **rdzenie** (i w związku z tym węzły obliczeń) na koncie partię. Domyślne przydziały i instrukcje na temat, aby [zwiększyć przydział](batch-quota-limit.md#increase-a-quota) (na przykład maksymalna liczba rdzeni na koncie partii) można znaleźć w [przydziałami i limity dotyczące usługi Azure partię](batch-quota-limit.md). Jeśli okaże się, że pytaniem "Dlaczego nie Moje puli osiągnięcia więcej niż X węzły?" Przyczyną może być przydział core.

## <a name="job"></a>Zadanie

Zadanie jest zbiorem zadań. Zarządza jak obliczenia odbywa się w swoich zadań w węzłach obliczeń w puli.

- Zadania określa **puli** , w którym ma być uruchamiane pracy. Możesz utworzyć nową pulę dla każdego zadania lub używać jednej puli dla wielu zadań. Można utworzyć pulę dla każdego zadania, z którą jest skojarzony z harmonogram zadań lub dla wszystkich zadań, które są skojarzone z planowania zadań.

- Można określić opcjonalne **Priorytet zadania**. Gdy zadanie zostaje przesłane o wyższym priorytecie niż zadania, które są obecnie w toku, zadania dla zadania wyższy priorytet są wstawiane w kolejce związaną z wcześniejszym zadań dla zadań niższym priorytecie. Zadania w dolnym priorytet zadania, które jest już zainstalowany nie są zastępowane.

- Zadanie **ograniczeń** pozwala określić limity określonych zadań:

    **Maksymalna liczba wallclock czasu**, można ustawić i jeśli zadanie trwa dłużej niż określony czas maksymalny wallclock, zadania i wszystkich swoich zadań są zamykane.

    Partii może wykryć, a następnie ponów próbę niepowodzeniem. **Maksymalna liczba prób zadania** można określić jako ograniczenie, w tym, czy zadanie jest *zawsze* lub *nigdy nie* po ponownych próbach. Ponawianie próby zadania oznacza, że zadanie jest niepełniącego ponownie uruchomić.

- Aplikacja klienta można dodawać zadania do zadania lub można określić [zadania Menedżera zadań](#job-manager-task). Menedżer zadania zawiera informacje, które są niezbędne do utworzenia wymaganych zadań dla zadania, przy użyciu Menedżera zadania uruchomione w jednym z węzłów obliczeń w puli. Menedżer zadania jest obsługiwane w szczególności przez partii — go znajduje się w kolejce zaraz po zadaniu zostanie utworzona i uruchomieniu, jeśli go nie powiedzie się. Menedżer zadania jest *wymagane* dla zadań, które są tworzone przez [Harmonogram zadań](#scheduled-jobs) , ponieważ jest jedynym sposobem definiują zadania, zanim zadanie zostanie uruchomiony.

- Domyślnie zadania pozostaną w stanie aktywnym po zakończeniu wszystkich zadań w grupie. Można zmienić to zachowanie, tak, aby zadanie kończy się automatycznie po zakończeniu wszystkich zadań w zadaniu. Należy ustawić właściwość **onAllTasksComplete** zadania ([OnAllTasksComplete] [ net_onalltaskscomplete] w partii .NET) do *terminatejob* automatycznie zakończenia zadania po wszystkich swoich zadań wykonanych stan.

    Należy zauważyć, że usługa partii są uwzględnione zadanie z *żadnych* zadań mieć dostęp do całej jej ukończonych zadań. Dlatego ta opcja jest najczęściej używane z [Menedżera zadania](#job-manager-task). Jeśli chcesz użyć zakończenia zadania wykonywania automatycznej bez Menedżera zadań, możesz należy początkowo *noaction*należy ustawić właściwość **onAllTasksComplete** nowe zadanie, a następnie ustaw *terminatejob* dopiero po zakończeniu dodawania zadania do zadania.

### <a name="job-priority"></a>Priorytet zadania

Priorytet można przypisać do zadania, które zostały utworzone w partii. Usługa partii używa wartości z pola Priorytet zadania, aby określić kolejność planowania zadań w ramach jednego konta (to nie należy mylić z [Zaplanowane zadanie](#scheduled-jobs)). Priorytet wartości z zakresu od -1000 do 1000, gdzie-1000 jest najniższe priorytet, a 1000 najwyższy. Priorytet zadania można aktualizować przy użyciu [Właściwości zadania] [ rest_update_job] operacji (RESZTA partii) lub zmieniając [CloudJob.Priority] [ net_cloudjob_priority] właściwości (partii .NET).

W ramach tego samego konta wyższy priorytet zadania zawierają, planowanie pierwszeństwa na niższym priorytecie zadania. Zadanie o wartości wyższy priorytet w jedno konto nie ma planowanie pierwszeństwo nad zadanie z wartością niższym priorytecie z innego konta.

Planowanie wielu pul jest niezależna. Między różnych zestawów nie gwarancji jest, że zadanie wyższy priorytet zostało zaplanowane, w przypadku jego skojarzony puli bezczynne węzły. W tej samej puli zadania o tym samym poziomie Priority (priorytet) zawierają planowany prawdopodobieństwo równości.

### <a name="scheduled-jobs"></a>Zaplanowane zadania

[Zadanie harmonogramów] [ rest_job_schedules] umożliwiają tworzenie zadań cyklicznych w usłudze partię. Harmonogram zadań umożliwia określenie, kiedy zadań i zawiera specyfikacje dotyczące zadania, które mają być uruchamiane. Możesz określić czas trwania harmonogramu — czasu oraz jeśli harmonogramu — i jak często podczas tego okresu, która ma zostać utworzona zadania.

## <a name="task"></a>Zadanie

Zadanie jest jednostka obliczeń, który jest skojarzony z zadaniem. Działają w węźle. Zadań przypisanych do węzła wykonywania lub znajduje się w kolejce aż wolnego przybierze kształt węzła. Po prostu umieścić, zadanie jest uruchamiane jeden lub więcej programów lub skryptów w węźle obliczeń do wykonywania pracy potrzebne.

Po utworzeniu zadania można określić:

- **Wiersz polecenia** zadania. To polecenie uruchamianej aplikacji lub skryptu w węźle obliczeń.

    Należy pamiętać, że wiersza polecenia rzeczywistości jest uruchomiony powłoki. W związku z tym, oryginalnie nie móc skorzystać z funkcji powłoki takich jak rozszerzeń [zmiennej środowiska](#environment-settings-for-tasks) (obejmuje to `PATH`). Aby móc skorzystać z tych funkcji, należy wywołać powłoki w wierszu polecenia — na przykład, uruchamiając `cmd.exe` w węzłach systemu Windows lub `/bin/sh` w systemie Linux:

    `cmd /c MyTaskApplication.exe %MY_ENV_VAR%`

    `/bin/sh -c MyTaskApplication $MY_ENV_VAR`

    Jeśli zadań potrzebne do uruchamiania aplikacji lub skrypt, który nie znajduje się w węzła `PATH` lub odwoływać się do środowiska zmiennych, wywołania powłoki w wierszu polecenia zadania.

- **Plików zasobów** , które zawierają dane, które mają być przetwarzane. Te pliki są automatycznie kopiowane do węzła z magazynem obiektów Blob na koncie magazyn Azure **uniwersalny** przed wykonaniem wiersza polecenia zadania. Aby uzyskać więcej informacji zobacz [uruchomić zadania](#start-task) i [pliki i katalogi](#files-and-directories).

- **Zmienne środowiska** , które są wymagane przez aplikację. Aby uzyskać więcej informacji zobacz sekcję [ustawień środowiska dla zadań](#environment-settings-for-tasks) .

- **Ograniczenia** , w którym mają być wykonywane zadania. Na przykład maksymalny czas, o jaki zadanie może być uruchamiane, maksymalną liczbę powtórzeń zadania nie powiodło się powinny być próby wykonania i maksymalny czas tego pliki w katalogu roboczym zadania są zachowywane.

- **Pakiety aplikacji** do wdrożenia do węzła obliczeń zaplanowano zadania do uruchomienia. [Pakiety aplikacji](#application-packages) zapewniają uproszczone rozmieszczanie i przechowywanie wersji aplikacje, których zadań. Pakiety aplikacji na poziomie zadania są przydatne w środowiskach udostępniony puli, gdzie różne zadania są uruchamiane na jednej puli, a puli nie są usuwane po zakończeniu zadania. Jeśli zadanie ma zadań mniej niż węzły w puli, pakietów aplikacji zadania można zminimalizować transfer danych, ponieważ aplikacja jest używany tylko węzłów, które wykonywania zadań.

Oprócz zadania, które można zdefiniować do wykonywania obliczeń w węźle następujące zadania specjalne również dostępne są przez usługę partii:

- [Rozpoczynanie zadania](#start-task)
- [Menedżer zadania](#job-manager-task)
- [Przygotowanie i wersji zadania](#job-preparation-and-release-tasks)
- [Wielu wystąpień zadania (MPI)](#multi-instance-tasks)
- [Zależności między zadaniami](#task-dependencies)

### <a name="start-task"></a>Rozpoczynanie zadania

Kojarząc **Rozpocznij zadanie** z puli, można przygotować środowisko operacyjne jego węzłów. Na przykład można wykonywać akcji, takich jak aplikacje, których zadań instalowania i uruchamiania procesy w tle. Zadanie rozpoczęcia będzie uruchamiane zawsze podczas uruchamiania węzeł, dla jak pozostaje w puli — w tym po pierwszym węzeł dodane do puli, a gdy jest ponowne uruchomienie lub reimaged.

Główną zaletą rozpoczęcia zadania to, że może zawierać wszystkie informacje, które są niezbędne do skonfigurowania węzła obliczeń i zainstalować aplikacje, które są wymagane do wykonania zadania. Dlatego zwiększenie liczby węzłów w puli jest tak proste jak określanie nowego licznika węzeł docelowej — partia już zawiera informacje, które są potrzebne do skonfigurowania nowe węzły i przygotowywanie ich akceptowania zadania.

Zgodnie z dowolnego zadania partii Azure, można określić listę plików **zasobów** w [Magazynie Azure][azure_storage], oprócz **wiersza polecenia** do wykonania. Partii najpierw skopiuje pliki zasobów do węzła z magazynu Azure, a następnie uruchamia wiersza polecenia. Pula rozpoczęcia zadania listę plików zazwyczaj zawiera aplikację zadania i jego zależności.

Jednak może także zawierać dane odwołanie może być używany we wszystkich zadań, które są uruchomione w węźle obliczeń. Na przykład, można wykonać wiersz polecenia zadania Rozpoczęcie `robocopy` operacji, aby skopiować pliki aplikacji, (które zostały określone jako pliki zasobów i pobrane do węzła) między zadania Rozpoczęcie [pracy katalogu](#files-and-directories) do [folderu udostępnionego](#files-and-directories), a następnie uruchom Instalatora MSI lub `setup.exe`.

> [AZURE.IMPORTANT] Partii obecnie obsługuje *tylko* typ konta magazynu **uniwersalny** , zgodnie z opisem w kroku 5, [Utwórz konto miejsca do magazynowania](../storage/storage-create-storage-account.md#create-a-storage-account) w [o Azure miejsca do magazynowania konta](../storage/storage-create-storage-account.md). Zadań partii (w tym zadań standardowych, rozpoczęcia zadania, przygotowanie zadania i zadania wersja release) podaj plików zasobów, które znajdują się *tylko* w **celu ogólne** miejsca do magazynowania konta.

Jest zazwyczaj usługi partii czekać na wykonanie zadania Rozpoczęcie przed uwzględnieniem węzeł gotowy do przydzielania zadań, ale można to skonfigurować.

Jeśli zadanie początkowe nie powiedzie się na węźle obliczeń, następnie stan węzła jest aktualizowany w celu odzwierciedlenia awarię i węzeł nie jest przypisana żadnych zadań. Zadanie początkowe może zakończyć się niepowodzeniem, jeśli występuje problem kopiowanie jego zasobów plików z magazynu lub jeśli proces wykonywane przez jego wiersza polecenia zwraca kod wyjścia niezerową.

Jeśli dodajesz lub aktualizowanie zadań Rozpocznij *istniejącej* puli, musisz ponownie uruchomić jego węzłów obliczeń rozpoczęcia zadania mają być stosowane do węzłów.

### <a name="job-manager-task"></a>Menedżer zadania

Zazwyczaj **Menedżer zadania** do sterowania i/lub monitorowanie wykonywania zadań — na przykład do tworzenia i przesyłania zadań dla zadania, należy określić dodatkowe zadania, aby uruchomić i określanie po zakończeniu pracy. Menedżer zadania jest ograniczone do tych działań. Jest w pełni użytecznym zadanie, które można wykonywać żadnych akcji, które są wymagane do zadania. Na przykład Menedżer zadania może pobrać plik, który jest określony jako parametr, analizowanie zawartość tego pliku i przesyłanie dodatkowych zadań na podstawie tych zawartości.

Menedżer zadania jest uruchomiona przed innych zadań. Dostępne są następujące funkcje:

- Który jest automatycznie przesyłany jako zadanie przez usługę partii po utworzeniu zadania.

- Jest ono planowane do wykonania przed inne zadania w ramach zadania.

- Jego skojarzony węzeł jest ostatnim ma zostać usunięta z puli, gdy jest podejmowana postaci puli.

- Jego zakończenia można powiązanych zakończenie wszystkie zadania.

- Zadania Menedżera zadań, wyznaczona najwyższy priorytet, gdy konieczne jest ponowne uruchomienie. Jeśli bezczynne węzeł nie jest dostępny, usługa partii może zakończyć jedną uruchomione zadania w puli, aby zrobić miejsce dla Menedżera zadania uruchomić.

- Zadania Menedżera zadań w jedno zadanie nie mają pierwszeństwo przed zadań inne zadania. W miejscu pracy tylko priorytety na poziomie zadania są obserwowane.

### <a name="job-preparation-and-release-tasks"></a>Przygotowanie i wersji zadania

Partia przewiduje przygotowanie zadania wykonywania zadań wstępnej konfiguracji. Wersji zadania są dla zadania po konserwacji lub czyszczenia.

- **Przygotowanie zadania**: przygotowanie obowiązków działa we wszystkich węzłach obliczeń, które według harmonogramu do wykonywania zadań, zanim dowolne inne zadania są wykonywane. Umożliwia przygotowanie zadania, aby skopiować dane, które mają być udostępniane przez wszystkich zadań, ale jest unikatowy dla zadania, na przykład.
- **Zadanie Zwolnij zadanie**: po ukończeniu zadania zadania Zwolnij zadanie jest uruchamiany w każdym węźle puli wykonać co najmniej jedno zadanie. Za pomocą zadania Zwolnij zadanie usunąć dane, które są kopiowane przez przygotowanie zadania, lub mają być kompresowane i przekazywanie danych dzienników diagnostycznych, na przykład.

Oba zadania przygotowanie i zadań wersji umożliwiają określenie wiersz polecenia, aby było uruchamiane po wywołaniu zadania. Oferują funkcje, takie jak pobieranie pliku, wykonanie podwyższonym poziomem uprawnień, zmienne środowiska niestandardowych, wykonanie maksymalny czas trwania, liczba powtórzeń i czas przechowywania pliku.

Aby uzyskać więcej informacji na przygotowanie i wersji zadania zobacz [Uruchom przygotowanie i zakończenia zadania w partii Azure obliczyć węzły](batch-job-prep-release.md).

### <a name="multi-instance-task"></a>Wielu wystąpień zadania

[Wielu wystąpień zadania](batch-mpi.md) to zadanie, które jest skonfigurowany do uruchomienia na więcej niż jeden węzeł obliczeń jednocześnie. Korzystając z wielu wystąpień zadania możesz włączyć wysokiej wydajności komputerowych scenariuszy, które wymagają grupy węzłów obliczeń, które są przydzielane razem podczas pojedynczego obciążenie pracą (takich jak wiadomości przechodzące Interface (MPI)).

Szczegółowe omówienie na wykonywania zadań MPI w partii za pomocą biblioteki .NET partię zapoznaj się z [użycia wielu wystąpień zadania do uruchamiania aplikacji wiadomości przechodzące Interface (MPI) w partii Azure](batch-mpi.md).

### <a name="task-dependencies"></a>Zależności między zadaniami

[Współzależności zadań](batch-task-dependencies.md), jak wskazuje nazwa, umożliwiają określenie, że zadanie zależy od zakończenia innych zadań przed jego wykonaniem. Ta funkcja zapewnia obsługę sytuacje, w których zadania "poniżej" pobiera dane wyjściowe zadania "powyżej" lub nadrzędny zadań wykonuje niektóre inicjowanie wymagane przez podrzędne zadania. Aby użyć tej funkcji, należy najpierw włączyć współzależności zadań na zadania partię. Następnie dla każdego zadania, która zależy od innego (lub wielu innych) można określić zadania, które zależy od tego zadania.

Współzależności zadań można skonfigurować scenariusze podobnej do następującej:

* *taskB* zależy od *taskA* (*taskB* nie rozpocznie się wykonanie zakończenia *taskA* ).
* *taskC* zależy od tego, zarówno *taskA* , jak i *taskB*.
* przed rozpoczęciem wykonywania *taskD* zależy zakres zadań, takich jak zadania od *1* do *10*.

Zapoznaj się z [współzależności zadań w partii Azure](batch-task-dependencies.md) i [TaskDependencies] [ github_sample_taskdeps] przykładowy kod w [azure partii prób] [ github_samples] GitHub repozytorium szczegółowo szczegółowe informacje na temat tej funkcji.

## <a name="environment-settings-for-tasks"></a>Ustawienia środowiska dla zadań

Każdego zadania wykonywane przez usługę partii ma dostęp do zmiennych środowiska, ustawionych w węzłach obliczeń. To między innymi zmienne środowiska zdefiniowane przez usługę partii ([Usługa zdefiniowana][msdn_env_vars]) i zmiennych środowiska niestandardowych, zdefiniowanych dla zadań. Aplikacje i skrypty, których wykonanie zadania mają dostęp do tych zmiennych środowiska podczas wykonywania.

Zmienne środowiska niestandardowej można ustawić na poziomie zadania lub zadania, wypełniać właściwości *ustawień środowiska* dla tych obiektów. Na przykład, zobacz [Dodawanie zadań do zadania] [ rest_add_task] operacji (interfejsu API usługi REST partii) lub [CloudTask.EnvironmentSettings] [ net_cloudtask_env] i [CloudJob.CommonEnvironmentSettings] [ net_job_env] właściwości w partii .NET.

Z aplikacji klienckiej lub usługi, można uzyskać zmiennych środowiska zadania, zarówno zdefiniowanych przez usługę i niestandardowych, przy użyciu [Uzyskaj informacje o zadaniu] [ rest_get_task_info] operacji (RESZTA partii) lub po zalogowaniu się do [CloudTask.EnvironmentSettings] [ net_cloudtask_env] właściwości (partii .NET). Procesów wykonywania na węźle obliczeń dostępu tych i innych czynników środowiska na węźle, na przykład przy użyciu znanych `%VARIABLE_NAME%` (Windows) lub `$VARIABLE_NAME` składni (Linux).

Pełna lista wszystkich zmiennych zdefiniowanych przez usługę środowiska można znaleźć w [obliczenia zmiennych środowiska węzeł][msdn_env_vars].

## <a name="files-and-directories"></a>Pliki i katalogi

Każde zadanie ma *katalog roboczy* w obszarze który tworzy zero lub więcej plików i katalogów. Ten katalog roboczy może służyć do przechowywania programu uruchamianego przez zadanie, dane, które przetwarza i dane wyjściowe przetwarzania wykonywana. Wszystkie pliki i katalogi zadania są własnością użytkownika zadania.

Usługa partii udostępnia część system plików w węźle *katalog główny*. Zadania można uzyskać dostęp do katalogu głównego przez odwołanie `AZ_BATCH_NODE_ROOT_DIR` zmiennej środowiska. Aby uzyskać więcej informacji o używaniu zmienne środowiska zobacz [Ustawienia środowiska dla zadań](#environment-settings-for-tasks).

Katalog główny zawiera następującą strukturę katalogów:

![Obliczenia węzła struktury katalogów][1]

- **udostępnione**: ten katalog zapewnia dostęp do *wszystkich* zadań, które są uruchamiane w węźle odczytu/zapisu. Dowolne zadanie, które jest uruchamiany w węźle można tworzenie, odczytywanie, aktualizowanie i usuwanie plików w tym katalogu. Zadania dostęp do tego katalogu przy odwoływaniu się `AZ_BATCH_NODE_SHARED_DIR` zmiennej środowiska.

- **Uruchamianie**: ten katalog jest używany przez rozpoczęcia zadania jako jej katalog roboczy. Wszystkie pliki, które są pobierane do węzła przez zadanie początkowe są przechowywane w tym miejscu. Zadania Rozpoczęcie można tworzenie, odczytywanie, aktualizowanie i usuwanie pliki w tym katalogu. Zadania dostęp do tego katalogu przy odwoływaniu się `AZ_BATCH_NODE_STARTUP_DIR` zmiennej środowiska.

- **Zadania**: katalog jest tworzony dla każdego zadania, które jest uruchamiany w węźle. Jest dostępny przez odwołanie `AZ_BATCH_TASK_DIR` zmiennej środowiska.

    W katalogu każdego zadania, usługa partii tworzy katalog roboczy (`wd`) którego unikatowe ścieżka jest określona przez `AZ_BATCH_TASK_WORKING_DIR` zmiennej środowiska. Ten katalog zapewnia dostęp do odczytu/zapisu do zadania. Zadania można tworzenie, odczytywanie, aktualizowanie i usuwanie pliki w tym katalogu. Ten katalog jest zachowywana oparte na ograniczenie *RetentionTime* określonego zadania.

    `stdout.txt`i `stderr.txt`: te pliki są zapisywane w folderze zadania podczas wykonywania zadania.

>[AZURE.IMPORTANT] Gdy węzeł zostanie usunięty z puli, zostaną usunięte *Wszystkie* pliki, które są przechowywane w węźle.

## <a name="application-packages"></a>Pakiety aplikacji

Funkcja [pakietów aplikacji](batch-application-packages.md) zapewnia łatwe zarządzanie i wdrażanie aplikacji węzły obliczeń w puli usługi. Możesz przekazać i zarządzanie wieloma wersjami aplikacje, zadań, łącznie z ich plików binarnych i plików pomocy technicznej. Następnie można automatycznie wdrożyć co najmniej jedną z nich do węzłów obliczeń w puli użytkownika.

Możesz określić pakietów aplikacji na poziomie puli i zadania. Po określeniu pakietów aplikacji puli aplikacji zostanie wdrożony każdy węzeł w puli. Po określeniu pakietów aplikacji zadania aplikacji zostanie wdrożony tylko węzłów, które zaplanowane co najmniej jeden z zlecenia zadania, tuż przed uruchomieniu wiersza polecenia zadania.

Partia obsługuje szczegółowe informacje o pracy z magazynu Azure do przechowywania pakiety aplikacji i wdrażanie ich do obliczenia węzły, więc może być uproszczone ogólnych obu kod i zarządzania.

Aby dowiedzieć się więcej na temat funkcji pakiet aplikacji, zapoznaj się z [wdrażaniem aplikacji za pomocą pakietów aplikacji partii Azure](batch-application-packages.md).

>[AZURE.NOTE] Jeśli dodasz pakiety puli aplikacji do *istniejącej* puli, musisz ponownie uruchomić jego węzłów obliczeń dla pakietów aplikacji w węzłach.

## <a name="pool-and-compute-node-lifetime"></a>Czas użytkowania węzeł puli i obliczeń

Podczas projektowania rozwiązania partii Azure należy decyzja projektu, jak i podczas pul są tworzone i jak długo trwa wyliczenia węzłów na pul są przechowywane.

Na jednym końcu można utworzyć pulę dla każdego zadania, które można przesłać i usuwanie puli zaraz po jego zadań zakończenie wykonanie. Maksymalizuje wykorzystania, ponieważ węzły są przydzielane tylko, gdy potrzebne, a następnie zamknij jak są one bezczynne. Podczas oznacza to, że zadanie trzeba poczekać węzły do przydzielenia, należy pamiętać, że zadania są planowane do wykonania po zainicjowaniu węzły dostępnych pojedynczo, przydzielone, i Rozpocznij zadanie zostało ukończone. Czy partii *nie* Zaczekaj, aż wszystkie węzły w puli dostępnych przed przydzielania zadań do węzłów. Dzięki temu maksymalne wykorzystanie wszystkich dostępnych węzłów.

Na drugim końcu podejście Jeśli natychmiast rozpocząć zadania jest najwyższy priorytet, możesz utworzyć pulę wyprzedzeniem i udostępnić jego węzłów przed przesłaniem zadania. W tym scenariuszu od razu rozpocząć zadania, ale węzły mogą znajdują bezczynne podczas oczekiwania na ich przydzielania.

Powierzchniowych zazwyczaj jest używana do obsługi obciążenia zmiennych, ale trwających. Możesz mieć puli wielu zadań są przesyłane do, ale można skalować liczby węzłów w górę lub w dół według zadania ładowanie (zobacz [Skalowanie obliczyć zasobów](#scaling-compute-resources) w sekcji poniżej). Możesz to zrobić rozbudowy, na podstawie bieżące obciążenie lub wyprzedzeniem, jeśli można przewidzieć ładowania.

## <a name="scaling-compute-resources"></a>Skalowanie zasobów obliczeń

[Automatyczne skalowanie](batch-automatic-scaling.md)można połączyć usługę partii dynamiczne dostosować liczby węzłów obliczeń w puli według bieżące użycie zasobów i pracą scenariusz obliczeń. Pozwala na obniżenie całkowitego kosztu uruchamiania aplikacji przy użyciu tylko zasoby, które są potrzebne, a zwalnianie tych, których nie potrzebujesz.

Możesz włączyć automatyczne skalowanie przez wpisywać [Automatyczne skalowanie formułę](batch-automatic-scaling.md#automatic-scaling-formulas) i kojarzenie tę formułę z puli. Usługa partii używa formuły do określenia docelowej liczby węzłów w puli do następnego przedziału skalowania (interwał, którą można skonfigurować). Możesz określić ustawienia automatycznych skalowania puli podczas jej tworzenia lub Włączanie skalowania w puli później. Możesz także zaktualizować ustawienia skalowania w puli obsługą skalowania.

Na przykład być może zadanie wymaga przesłaniu bardzo dużej liczby zadań do wykonania. Skalowanie formuły można przypisać do puli skoryguje liczby węzłów w puli na podstawie numeru bieżącej kolejce zadań i stawkę ukończenia zadania. Usługa partii okresowo oblicza formułę i zmienia rozmiar puli, na podstawie obciążenia (Dodawanie węzłów dla wielu zadań w kolejce i usuwanie węzłów żadnych zadań kolejce lub nie działa) oraz inne ustawienia formuły.

Skalowanie formuły mogą być oparte na metryk następujące czynności:

- **Metryki godziny** są oparte na statystyki zbierane co pięć minut w określonej liczby godzin.

- **Metryki zasobów** są oparte na użycie Procesora, wykorzystania przepustowości, użycie pamięci i liczby węzłów.

- **Metryki zadania** są oparte na stanu zadań, takich jak *aktywna* (kolejki), *Uruchamianie*lub *ukończone*.

Jeśli automatyczne skalowanie zmniejszenie liczby węzłów obliczeń w puli, należy rozważyć sposobu obsługi zadania, które są uruchomione w czasie operacji spadek. Aby zezwalały na to, partii udostępnia *opcję dezalokacji węzeł* , zawierające w formułach. Na przykład można określić, że uruchomione zadania są natychmiast zatrzymany, natychmiast zatrzymany, a następnie niepełniącego do wykonania w innym węźle lub może zakończyć się przed węzeł zostanie usunięty z puli.

Aby uzyskać więcej informacji na temat Automatyczne skalowanie aplikacji zobacz [automatycznie skali obliczyć węzły w puli partii Azure](batch-automatic-scaling.md).

> [AZURE.TIP] Aby zmaksymalizować wykorzystanie zasobów obliczeń, ustaw docelowej liczby węzłów zera na końcu zadania, ale zezwalaj na uruchamianie zadań, aby zakończyć.

## <a name="security-with-certificates"></a>Zabezpieczenia certyfikatów

Zwykle potrzebne do korzystania z certyfikatów podczas szyfrowania lub odszyfrowywania poufnych informacji dotyczących zadań, takich jak klucz [konta magazynu platformy Azure][azure_storage]. W tym możesz zainstalować certyfikaty w węzłach. Zaszyfrowane hasła są przekazywane do zadań przy użyciu parametrów wiersza polecenia lub osadzony w jeden z zasobów zadania i zainstalowanych certyfikatów może służyć do ich odszyfrowywanie.

Używanie [certyfikatów Dodaj] [ rest_add_cert] operacji (RESZTA partii) lub [CertificateOperations.CreateCertificate] [ net_create_cert] metody (partii .NET) Dodawanie certyfikatu na konto partię. Następnie można skojarzyć certyfikatu do nowego lub istniejącego puli. Podczas certyfikatu jest skojarzony z puli, usługę partii instaluje certyfikat na każdym węźle w puli. Usługa partii instaluje odpowiednie certyfikaty podczas uruchamiania węzeł, przed uruchomieniem jakichkolwiek zadań (w tym do rozpoczęcia zadania, a Menedżer zadania).

Jeśli dodasz certyfikaty do *istniejącej* puli, musisz ponownie uruchomić jego węzłów obliczeń dla certyfikatów, które mają być stosowane do węzłów.

## <a name="error-handling"></a>Obsługa błędów

Może być konieczne do obsługi błędów zarówno zadania, jak i aplikacji w rozwiązaniu partii.

### <a name="task-failure-handling"></a>Obsługa błąd zadania
Błędy zadania należą do tych kategorii:

- **Planowanie błędy**

    Jeśli transfer plików, które są określane dla zadania nie powiedzie się z dowolnego powodu, "planowania błąd" zostanie ustawiony dla zadania.

    Planowanie błędów może wystąpić, jeśli pliki zasobów zadania zostały przeniesione, konto miejsca do magazynowania nie jest już dostępna lub innego problemu wystąpił, który można zapisać pomyślnego kopiowanie plików do węzła.

- **Błędy aplikacji**

    Proces, którą określono za pomocą wiersza polecenia zadania również może nie działać. Ten proces jest uważany za nie powiodła się podczas kod niezerową zakończenia został zwrócony przez proces, która zostanie wykonana przez zadanie (zobacz *kody zakończenia zadania* w następnej sekcji).

    Błędy aplikacji można skonfigurować partię automatycznie próbować zadania do określoną liczbę razy.

- **Ograniczenia błędy**

    Można ustawić ograniczenie, które umożliwia określenie wykonanie maksymalny czas trwania zadania lub zadania, *maxWallClockTime*. Może to być przydatne w przypadku zakończenia zadania "zawieszone".

    Przekroczono maksymalną ilość czasu, zadanie zostanie oznaczone jako *zakończone*, ale kod wyjścia jest ustawiona na `0xC000013A` i pole *schedulingError* jest oznaczone jako `{ category:"ServerError", code="TaskEnded"}`.

### <a name="debugging-application-failures"></a>Debugowania błędów aplikacji

- `stderr`oraz`stdout`

    Podczas wykonywania aplikacja może powodować diagnostyczne dane wyjściowe, które umożliwiają rozwiązywanie problemów z. Wymienione w sekcji wcześniejszych [pliki i katalogi](#files-and-directories)usługa partii zapisuje standardowe dane wyjściowe i rozdzielczość błędu standardowego na `stdout.txt` i `stderr.txt` pliki w katalogu zadania w węźle obliczeń. Azure portal lub jeden z SDK partii umożliwia Pobierz te pliki. Na przykład, można pobrać tych i innych plików w celu rozwiązywania problemów za pomocą [ComputeNode.GetNodeFile] [ net_getfile_node] i [CloudTask.GetNodeFile] [ net_getfile_task] w bibliotece .NET partię.

- **Kody zakończenia zadania**

    Jak wspomniano wcześniej, zadanie zostało oznaczone jako zakończone niepowodzeniem przez usługę partii, jeśli proces, który jest wykonywane przez zadanie zwraca kod niezerową zakończenia. Gdy zadanie jest wykonywana procesu, partii wypełnia właściwość kod wyjścia zadania z *zwraca kod procesu*. Należy pamiętać, że kod zakończenia zadania **nie** określona przez usługę partii — jest określona przez sam proces lub system operacyjny, na którym wykonywane procesu.

### <a name="accounting-for-task-failures-or-interruptions"></a>Uwzględniania błędy zadania lub przerwań

Zadania może czasami się nie powieść lub przeszkadzano. Samej aplikacji zadania może się nie powieść, węzeł, na którym działa zadanie może zostać, należy ponownie uruchomić lub węzeł może zostać usunięta z puli podczas operacji zmiany rozmiaru Jeśli zasady dezalokacji w puli jest ustawiona na natychmiast usunąć węzły nie czekając na zakończenie zadań. We wszystkich przypadkach zadania może być automatycznie niepełniącego przez partii do wykonania w innym węźle.

Istnieje także możliwość przejściowymi problemu spowodowało rozłączyć lub trwa zbyt długo, aby wykonać zadanie. Można ustawić maksymalny czas wykonywania zadania. Jeśli przekroczeniu, partii przerwań aplikacji zadania.

### <a name="connecting-to-compute-nodes"></a>Nawiązywanie połączenia z obliczenia węzły

Można wykonać dodatkowe debugowania i rozwiązywanie problemów po zalogowaniu się do węzła obliczeń zdalnie. Azure portal umożliwia pobrać plik protokołu RDP (Remote Desktop) dla systemu Windows węzłów i uzyskaj informacje o połączeniu Secure Shell (SSH) dla węzłów Linux. Możesz to także zrobić przy użyciu partii interfejsów API — na przykład [.NET partii] [ net_rdpfile] lub [Python partię](batch-linux-nodes.md#connect-to-linux-nodes).

>[AZURE.IMPORTANT] Aby połączyć się węzeł za pośrednictwem RDP lub SSH, należy najpierw utworzyć użytkownika w węźle. Aby to zrobić, możesz użyć Azure portalu, [dodać konto użytkownika do węzła] [ rest_create_user] za pomocą interfejsu API usługi REST partię, połączenie [ComputeNode.CreateComputeNodeUser] [ net_create_user] metoda .NET partii lub połączenia [add_user] [ py_add_user] metody w module Python partię.

### <a name="troubleshooting-bad-compute-nodes"></a>Węzły obliczyć rozwiązywania problemów "bad"

W sytuacjach, gdy niektórych zadań występują z aplikacją kliencką partii lub usługi, można sprawdzić metadane niepowodzeniem do identyfikacji zachowania węzła. Każdy węzeł w puli, wyznaczona Unikatowy identyfikator, a węzeł, na którym jest uruchomiona zadania znajduje się w metadanych zadań. Po zostały określone węzeł problemu, możesz wykonać kilka dodatkowych czynności z nim:

- **Uruchom ponownie węzeł** ([REST][rest_reboot] | [.NET][net_reboot])

    Ponowne uruchamianie węzeł czasami można rozwiązać ukryty problemów, takich jak procesy zablokowane lub awaria. Należy zauważyć, że jeśli puli użytkownika używa rozpoczęcia zadania lub zadania używa przygotowanie zadania, są wykonywane po ponownym uruchomieniu węzła.

- **Reimage węzeł** ([REST][rest_reimage] | [.NET][net_reimage])

    W węźle ponownie instaluje system operacyjny. Podobnie jak w przypadku ponownego uruchomienia węzeł, uruchom zadania i przygotowanie zadania są ponownie po węzeł zostały reimaged.

- **Usuń węzeł z puli** ([REST][rest_remove] | [.NET][net_remove])

    Czasami jest to konieczne całkowicie usunąć węzeł z puli.

- **Wyłączanie planowania w węźle zadań** ([REST][rest_offline] | [.NET][net_offline])

    To skutecznie trwa węzeł "w trybie offline", aby żadne następne zadania są przypisane do niego, ale umożliwia węzeł powinien pozostawać uruchomiony i w puli. To umożliwia dalsze badania przyczyny błędów bez utraty danych nie powiodło się zadań — i bez węzeł powodujące błędy dodatkowe zadania. Na przykład można wyłączyć planowania w węźle [Zaloguj zdalnego](#connecting-to-compute-nodes) , aby sprawdzić myszy węzeł Dzienniki zdarzeń lub rozwiązywania problemów z innych zadań. Po zakończeniu do badania, możesz następnie zabrać ze sobą węzeł trybu online, umożliwiając planowania zadań ([REST][rest_online] | [.NET][net_online]), lub wykonaj jedną z innych czynności, które zostało to opisane wcześniej.

> [AZURE.IMPORTANT] Z każdej akcji, która jest opisane w tej sekcji — Uruchom ponownie, reimage, Usuń i Wyłącz planowania zadań — możesz określić, jak zadania, obecnie uruchomione w węźle są obsługiwane podczas wykonywania akcji. Na przykład po wyłączeniu harmonogramy zadań w węźle za pomocą biblioteki klienta .NET partię, możesz określić [DisableComputeNodeSchedulingOption] [ net_offline_option] wartość wyliczenia, aby określić, czy do **końca** uruchamianie zadań, **Requeue** je do planowania na innych węzłach lub zezwalanie na uruchamianie zadań do wykonania przed wykonaniem akcji (**TaskCompletion**).

## <a name="next-steps"></a>Następne kroki

- Szczegółową przykładową aplikację partii krok po kroku w [Rozpoczynanie pracy z biblioteką partii Azure dla środowiska .NET](batch-dotnet-get-started.md). Istnieje także [wersji Python](batch-python-tutorial.md) samouczka uruchamianej obciążenie pracą w węzłach obliczeń Linux.

- Pobieranie i tworzenie [Partii Eksploratora] [ github_batchexplorer] przykładowy projekt do użytku podczas opracowywania rozwiązań z partii. Korzystając z Eksploratora partię, mogą wykonywać następujące czynności i więcej:
  - Monitorowanie i manipulować pul, zadań i zadań w ramach konta partii
  - Pobierz `stdout.txt`, `stderr.txt`oraz inne pliki z węzłów
  - Tworzenie użytkowników w węzłach i pobieranie plików RDP do logowania zdalnego

- Dowiedz się, jak [tworzyć pule węzłów obliczeń Linux](batch-linux-nodes.md).

- Odwiedź [forum partii Azure] [ batch_forum] w witrynie MSDN. Forum to dobre miejsce do zadawać pytania, po prostu nauki korzystania z czy są ekspertem w przy użyciu partii.

[1]: ./media/batch-api-basics/node-folder-structure.png

[azure_storage]: https://azure.microsoft.com/services/storage/
[batch_forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurebatch
[cloud_service_sizes]: ../cloud-services/cloud-services-sizes-specs.md
[msmpi]: https://msdn.microsoft.com/library/bb524831.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_sample_taskdeps]:  https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch_net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[msdn_env_vars]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[net_cloudjob_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobmanagertask.aspx
[net_cloudjob_priority]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.priority.aspx
[net_cloudpool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_cloudtask_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.environmentsettings.aspx
[net_create_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.createcertificate.aspx
[net_create_user]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.createcomputenodeuser.aspx
[net_getfile_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getnodefile.aspx
[net_getfile_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.getnodefile.aspx
[net_job_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.commonenvironmentsettings.aspx
[net_multiinstancesettings]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_onalltaskscomplete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.onalltaskscomplete.aspx
[net_rdp]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getrdpfile.aspx
[net_reboot]: https://msdn.microsoft.com/library/azure/mt631495.aspx
[net_reimage]: https://msdn.microsoft.com/library/azure/mt631496.aspx
[net_remove]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.removefrompoolasync.aspx
[net_offline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.disableschedulingasync.aspx
[net_online]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.enableschedulingasync.aspx
[net_offline_option]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.disablecomputenodeschedulingoption.aspx
[net_rdpfile]: https://msdn.microsoft.com/library/azure/Mt272127.aspx
[vnet]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_netconf

[py_add_user]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.ComputeNodeOperations.add_user

[batch_rest_api]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[rest_add_job]: https://msdn.microsoft.com/library/azure/mt282178.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_cert]: https://msdn.microsoft.com/library/azure/dn820169.aspx
[rest_add_task]: https://msdn.microsoft.com/library/azure/dn820105.aspx
[rest_create_user]: https://msdn.microsoft.com/library/azure/dn820137.aspx
[rest_get_task_info]: https://msdn.microsoft.com/library/azure/dn820133.aspx
[rest_job_schedules]: https://msdn.microsoft.com/library/azure/mt282179.aspx
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx
[rest_multiinstancesettings]: https://msdn.microsoft.com/library/azure/dn820105.aspx#multiInstanceSettings
[rest_update_job]: https://msdn.microsoft.com/library/azure/dn820162.aspx
[rest_rdp]: https://msdn.microsoft.com/library/azure/dn820120.aspx
[rest_reboot]: https://msdn.microsoft.com/library/azure/dn820171.aspx
[rest_reimage]: https://msdn.microsoft.com/library/azure/dn820157.aspx
[rest_remove]: https://msdn.microsoft.com/library/azure/dn820194.aspx
[rest_offline]: https://msdn.microsoft.com/library/azure/mt637904.aspx
[rest_online]: https://msdn.microsoft.com/library/azure/mt637907.aspx

[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
