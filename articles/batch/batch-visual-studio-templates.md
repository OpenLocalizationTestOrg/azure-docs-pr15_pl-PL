<properties
    pageTitle="Visual Studio szablony partii Azure | Microsoft Azure"
    description="Dowiedz się, jak te szablony projektów programu Visual Studio może pomóc w zaimplementować i uruchomić obciążeń pracą usługi obliczeniowych w partii Azure"
    services="batch"
    documentationCenter=".net"
    authors="fayora"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="09/07/2016"
    ms.author="marsma" />

# <a name="visual-studio-project-templates-for-azure-batch"></a>Visual Studio szablony projektów partii Azure

**Menedżer zadań** i **Szablony programu Visual Studio procesor zadania** partii Podaj kod w celu zaimplementowania i przeprowadzić obciążeń pracą usługi obliczeniowych partia z najmniejszą nakładu. Ten dokument, w tym artykule opisano tych szablonów i wskazówki dotyczące sposobu korzystania z nich.

>[AZURE.IMPORTANT] W tym artykule omówiono tylko informacje dotyczące tych dwóch szablonów i przyjęto założenie, że znasz usługa partii i podstawowych pojęć związanych z nim: pule, obliczyć węzły, zadania i zadania, Menedżer zadania, zmienne środowiska i inne informacje. Więcej informacji można znaleźć w [Podstawy partia Azure](batch-technical-overview.md), [Omówienie funkcji partii dla deweloperów](batch-api-basics.md)i [rozpocząć pracę z biblioteką partii Azure dla środowiska .NET](batch-dotnet-get-started.md).

## <a name="high-level-overview"></a>Omówienie

Szablony Menedżera zadań i procesor zadania można utworzyć dwa składniki przydatne:

* Zadanie Menedżer zadań, które wykonuje podziału zadania, które można podzielić pracę na wielu zadań, które można równolegle niezależnie od tego.

* Procesor zadania, która może służyć do wykonywania wstępne przetwarzanie i przetwarzanie końcowe wokół wiersza polecenia aplikacji.

Na przykład w scenariuszu renderowania filmu podziału zadania chcesz przekształcić w zadanie pojedynczy filmu setki lub tysiące oddzielnych zadań, które chcesz przetwarzać poszczególnych ramki oddzielnie. Odpowiednio procesor zadania może wywołać aplikacji renderowania i wszystkie procesy zależne, które są wymagane do renderowania każdej ramki, a także wykonywać żadnych dodatkowych akcji (na przykład kopiowania renderowanych ramki do lokalizacji przechowywania).

>[AZURE.NOTE] Szablony Menedżera zadań i procesor zadania są niezależnie od siebie, więc można użyć zarówno lub tylko jeden z nich, w zależności od potrzeb zadania obliczeń i preferencje.

Jak pokazano na poniższej ilustracji, zadanie obliczeniowej, która korzysta z tych szablonów zostaną przekierowane do trzech etapów:

1. Kod klienta (np. aplikacji, usługi sieci web itp.) prześle zadanie z usługą partii Azure, określając jako swojego menedżera zlecenia zadania program Menedżer zadań.

2. Menedżer zadania działa usługa partii w węźle obliczeń i podziału zadania uruchamia określoną liczbę zadań procesor zadania, na, jak wiele obliczyć węzły odpowiednio do potrzeb, na podstawie parametrów i specyfikacje w kodzie podziału zadania.

3. Zadanie zadania procesor równolegle niezależnie od tego, przetwarzanie danych wejściowych i wygenerować danych wyjściowych.

![Diagram przedstawiający interakcji kod klienta z usługą partii][diagram01]

## <a name="prerequisites"></a>Wymagania wstępne

Aby użyć szablonów partię, będą potrzebne następujące elementy:

* Komputer z programu Visual Studio 2015 r., lub nowszego, już zainstalowany na nim.

* Szablony partii, które są dostępne w [Galerii Visual Studio] [ vs_gallery] jako rozszerzenia programu Visual Studio. Istnieją dwa sposoby pobierania szablonów:

  * Instalowanie szablonów przy użyciu okna dialogowego **rozszerzenia i aktualizacji** programu Visual Studio (Aby uzyskać więcej informacji, zobacz [Znajdowanie i za pomocą programu Visual Studio rozszerzeń][vs_find_use_ext]). W oknie dialogowym **rozszerzenia i aktualizacje** wyszukiwanie i pobieranie dwa rozszerzenia:

    * Azure partii zadania w Menedżerze podziału zadania
    * Procesor zadania partii Azure

  * Pobieranie szablonów z galerii w trybie online dla programu Visual Studio: [Szablony projektów Microsoft Azure partii][vs_gallery_templates]

* Jeśli zamierzasz wdrożyć Menedżera zadań za pomocą funkcji [Pakietów aplikacji](batch-application-packages.md) i procesor zadania do partii obliczyć węzły, musisz połączyć konta miejsca do magazynowania z kontem partię.

## <a name="preparation"></a>Przygotowanie

Zaleca się utworzenie rozwiązanie, które mogą zawierać Menedżera zadań, a także używany procesor zadania, ponieważ to może ułatwić współużytkowanie kodu swojego menedżera zadań i programów procesor zadania. Aby utworzyć to rozwiązanie, wykonaj następujące kroki:

1. Otwórz program Visual Studio 2015 i wybierz **plik** > **Nowy** > **projektu**.

2. W obszarze **Szablony**rozwiń **Innych typów projektów**, kliknij pozycję **Rozwiązania programu Visual Studio**, a następnie wybierz **Pusty rozwiązanie**.

3. Wpisz nazwę opisującą aplikacji i przeznaczenia tego rozwiązania (przykład "LitwareBatchTaskPrograms").

4. Aby utworzyć nowe rozwiązanie, kliknij **przycisk OK**.

## <a name="job-manager-template"></a>Szablon Menedżera zadań

Szablon Menedżera zadań ułatwia wdrożenie Menedżera zadania można wykonywać następujące czynności:

* Podziel zadanie na wielu zadań.
* Przesyłanie tych zadań do wykonania na partię.

>[AZURE.NOTE] Aby uzyskać więcej informacji na temat Menedżera zadania zobacz [Omówienie funkcji partii dla deweloperów](batch-api-basics.md#job-manager-task).

### <a name="create-a-job-manager-using-the-template"></a>Tworzenie Menedżera zadań przy użyciu szablonu

Aby dodać rozwiązanie, który został utworzony wcześniej przez Menedżera zadań, wykonaj następujące czynności:

1. Otwieranie istniejącego rozwiązania w Visual Studio 2015 r.

2. W Eksploratorze rozwiązań, kliknij prawym przyciskiem myszy rozwiązanie, kliknij przycisk **Dodaj** > **Nowego projektu**.

3. W obszarze **Visual C#**kliknij w **chmurze**, a następnie kliknij **Azure partii zadania w Menedżerze podziału zadania**.

4. Wpisz nazwę, która zawiera opis aplikacji i identyfikuje tego projektu jako Menedżera zadań (np. "LitwareJobManager").

5. Aby utworzyć projekt, kliknij **przycisk OK**.

6. Na koniec Utwórz projekt, aby wymusić Visual Studio, aby załadować wszystkie odwołania pakiety NuGet i sprawdź, czy projekt jest prawidłowy, przed rozpoczęciem modyfikowanie.

### <a name="job-manager-template-files-and-their-purpose"></a>Pliki szablonu Menedżera zadań i ich zastosowań

Po utworzeniu projektu przy użyciu szablonu Menedżera zadań generuje trzech grup plików kodu:

* Plik programu głównym (plik Program.cs). Ta strona zawiera punkt wejścia programu i obsługi wyjątków najwyższego poziomu. Zwykle nie należy zmodyfikować to.

* Katalog Framework. Ta strona zawiera pliki odpowiedzialny za "standardowy" pracy za pomocą programu Menedżer zadań — Rozpakowywanie parametry, dodawanie zadań do zadania, itd. Zwykle nie należy zmodyfikować te pliki.

* Plik podziału zadania (JobSplitter.cs). Jest to, gdzie zostanie umieszczony logiki specyficzne dla aplikacji podziału zadania do zadania.

Oczywiście można dodać dodatkowe pliki wymagane do obsługi kodzie podziału zadania, w zależności od złożoności zadania dzielenie logicznych.

Szablon generuje również standardowe pliki projektu .NET, takie jak .csproj pliku app.config, packages.config, itp.

Pozostałą część tej sekcji zawiera opis różnych plików i ich struktury kodu i wyjaśniono, co oznacza każdej kategorii.

![Visual Studio Eksplorator rozwiązań z Menedżera zadań rozwiązanie szablonu][solution_explorer01]

**Struktura plików**

* `Configuration.cs`: Obejmuje załadowaniu danych konfiguracji zadania, takie jak partii szczegółowe informacje o koncie, poświadczenia konta połączone miejsca do magazynowania, zadania i informacje o zadaniu i parametrów zadania. Umożliwia także dostęp do zmiennych środowiska zdefiniowane partii (Zobacz ustawienia środowiska dla zadań w dokumentacji partii) za pomocą klasy Configuration.EnvironmentVariable.

* `IConfiguration.cs`: Abstracts stosowania klasa konfiguracji, tak aby można było testu jednostki do podziału zadania za pomocą obiektu fałszywe lub zasymulować konfiguracji.

* `JobManager.cs`: Orchestrates składniki programu Menedżer zadań. Odpowiada za inicjowanie podziału zadania, wywoływanie podziału zadania i wysyłki zadania zwracane przez podziału zadania dla przesyłające zadania.

* `JobManagerException.cs`: Reprezentuje błąd, który wymaga Menedżera zadań w celu zakończenia. JobManagerException jest używana do zawijanie błędy "oczekiwanych" gdzie określonych informacji diagnostycznych może być udostępniana w ramach zakończenia.

* `TaskSubmitter.cs`: Do dodawania zadań zwracane przez podziału zadania dla zadaniu odpowiada tej klasy. Agregacje klasy JobManager sekwencji zadań w partii wydajność, ale czas dodania do zadania, następnie połączeń TaskSubmitter.SubmitTasks na wątku tła dla każdej serii.

**Pasek podziału zadania**

`JobSplitter.cs`: To klasa zawiera logikę specyficzne dla aplikacji dla dzielenie zadania na zadania. Ramy wywołuje metodę JobSplitter.Split uzyskanie sekwencji zadań, które dodaje do zadania, jak metoda zwraca je. Jest to klasa, gdzie będzie wprowadzić logika zadania. Zaimplementować metodę podziału, aby zwrócić sekwencji wystąpień CloudTask przedstawiających zadania, do których chcesz podzielić zadanie.

**Standardowe pliki projektu wiersza polecenia programu .NET**

* `App.config`: Plik konfiguracyjny aplikacji standardowy .NET.

* `Packages.config`: Standardowy plik zależności pakiet NuGet.

* `Program.cs`: Zawiera punkt wejścia programu i obsługi wyjątków najwyższego poziomu.

### <a name="implementing-the-job-splitter"></a>Pasek podziału zadania do wykonania

Po otwarciu Menedżera zadań projektu szablonu projektu uzyskuje plik JobSplitter.cs domyślnie otwierane. Logika podziału zadań można zaimplementować w z pracą przy użyciu Pokaż metody Split() poniżej:

```csharp
/// <summary>
/// Gets the tasks into which to split the job. This is where you inject
/// your application-specific logic for decomposing the job into tasks.
///
/// The job manager framework invokes the Split method for you; you need
/// only to implement it, not to call it yourself. Typically, your
/// implementation should return tasks lazily, for example using a C#
/// iterator and the "yield return" statement; this allows tasks to be added
/// and to start running while splitting is still in progress.
/// </summary>
/// <returns>The tasks to be added to the job. Tasks are added automatically
/// by the job manager framework as they are returned by this method.</returns>
public IEnumerable<CloudTask> Split()
{
    // Your code for the split logic goes here.
    int startFrame = Convert.ToInt32(_parameters["StartFrame"]);
    int endFrame = Convert.ToInt32(_parameters["EndFrame"]);

    for (int i = startFrame; i <= endFrame; i++)
    {
        yield return new CloudTask("myTask" + i, "cmd /c dir");
    }
}
```

>[AZURE.NOTE] W sekcji adnotowanych `Split()` metoda jest tylko sekcji kodu szablonu Menedżera zadań, który jest przeznaczony do modyfikowania przez dodanie logiki zadań można podzielić różnych zadań. Jeśli chcesz zmodyfikować innej sekcji szablonu, upewnij się, są zapoznaniu z działania partii i wypróbować niektóre [Przykłady kodu partii][github_samples].

Implementacji Split() ma dostęp do:

* Parametry zadania za pomocą `_parameters` pola.
* Obiekt CloudJob, reprezentujący zadania, za pomocą `_job` pola.
* Obiekt CloudTask odpowiadającego zadaniu Menedżera zadań, za pomocą `_jobManagerTask` pola.

Usługi `Split()` implementacji nie trzeba dodawać zadania bezpośrednio do zadania. Zamiast tego kodu powinna zwrócić sekwencji CloudTask obiektów, a te zostaną dodane do zadania automatycznie przez klasy struktury, które wywołują podziału zadania. Często jest używanie C# i iteracyjne (`yield return`) funkcji do wykonania zadania rozdzielaczy jako dzięki temu tych zadań, aby rozpocząć działa tak szybko, jak to możliwe, a nie trwa oczekiwanie na wszystkie zadania, które ma zostać obliczona.

**Błąd podziału zadania**

Jeśli do podziału zadań wystąpi błąd, powinno albo:

* Zakończenie sekwencji przy użyciu C# `yield break` instrukcji, w której przypadku Menedżer zadań są traktowane jako pomyślnie; lub

* Zostać zgłoszony wyjątek, w której przypadku, gdy Menedżer zadań jest traktowana jako nie powiodło się i może być wykonana w zależności od sposobu klienta skonfigurował go ponownie).

W obu przypadkach żadnych zadań już zwracane przez podziału zadania i dodane do zadaniu będzie mogą być uruchamiane. Jeśli nie chcesz, aby tak się stało, następnie możesz:

* Zakończ zadanie przed powrotem z podziału zadania

* Sformułowania zbioru całe zadanie przed jej zwrócenia (oznacza to, że zwracana `ICollection<CloudTask>` lub `IList<CloudTask>` zamiast wykonania do podziału zadań przy użyciu iteracyjne C#)

* Wykonywanie wszystkich zadań są zależne od pomyślnego zakończenia Menedżera zadań za pomocą współzależności zadań

**Ponowne próby Menedżera zadań**

Jeśli Menedżer zadanie nie powiedzie się, może być wykonana ponownie przez usługę partii w zależności od ustawień ponów próbę klienta. Zazwyczaj jest to bezpieczne, ponieważ ramach dodaje zadania do zadania, ignoruje wszystkie zadania, które już istnieje. Jednak w przypadku obliczania zadania drogich, nie możesz ponoszenia kosztów ponownego obliczania zadań, które zostały już dodane do zadania; i odwrotnie jeśli ponowne uruchomienie nie jest gwarantowana do generowania tego samego zadania identyfikatorów następnie zachowanie "Ignoruj duplikaty" będzie nie grzybków. W tych przypadkach należy projektowanie do podziału zadań wykrywanie pracy, do której zostało już wykonane, a nie powtarzaj, na przykład, wykonując CloudJob.ListTasks przed rozpoczęciem rentowność zadania.

### <a name="exit-codes-and-exceptions-in-the-job-manager-template"></a>Kody zakończenia i wyjątki w szablonie Menedżera zadań

Kody zakończenia i wyjątki mechanizmu ustalenie wynik działania programu, a może pomóc identyfikowanie problemów z działania programu. Szablon Menedżera zadań zawiera kody zakończenia i wyjątki opisane w tej sekcji.

Menedżer zadania wykonywane przy szablonu Menedżera zadań można powrócić trzy kody wyjścia możliwe:

| Kod | Opis |
|------|-------------|
| 0    | Menedżer zadań, ukończona pomyślnie. Kod podziału zadania wykonane do ukończenia, a wszystkie zadania zostały dodane do zadania. |
| 1    | Menedżer zadania nie powiodło się z powodu wyjątku w "oczekiwanych" części programu. Wyjątek został przetłumaczony JobManagerException informacje diagnostyczne i, jeśli to możliwe, sugestii na temat naprawiania błędu. |
| 2    | Menedżer zadania nie powiodło się z powodu wyjątku "nieoczekiwane". Wyjątek zarejestrowania pojawi się, ale nie Menedżera zadań dodać dodatkowe informacje diagnostyczne i odnawiania. |

W przypadku awarii zadań Menedżer zadań niektóre zadania może nadal zostały dodane do usługi przed wystąpieniem błędu. Te zadania będą uruchamiane w normalny sposób. Omówienie następującą ścieżkę kodu, zobacz "Błąd podziału zadania" powyżej.

Wszystkie informacje, które są zwracane przez wyjątki są zapisywane w plikach stdout.txt i stderr.txt. Aby uzyskać więcej informacji zobacz [Obsługa błędów](batch-api-basics.md#error-handling).

### <a name="client-considerations"></a>Zagadnienia dotyczące klientów

W tej sekcji opisano niektóre wymagania dotyczące implementacji klienta podczas wywoływania Menedżera zadań na podstawie tego szablonu. Zobacz, [jak przekazać parametry i zmienne środowiska z kod klienta](#pass-environment-settings) szczegółowe informacje na temat przekazywania parametrów i ustawień środowiska.

**Wymagane poświadczenia**

Aby dodać zadania do Azure zadaniu, Menedżer zadania wymaga swój adres URL konto Azure partii i klucz. Należy przekazać w środowisku zmienne o nazwie YOUR_BATCH_URL i YOUR_BATCH_KEY. Można ustawić w Menedżerze zadań zadania ustawień środowiska. Na przykład w C# klienta:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    new EnvironmentSetting("YOUR_BATCH_URL", "https://account.region.batch.azure.com"),
    new EnvironmentSetting("YOUR_BATCH_KEY", "{your_base64_encoded_account_key}"),
};
```
**Magazyn poświadczeń**

Zazwyczaj klient nie musi o podanie poświadczeń konta magazynowania połączone z zadaniem Menedżera zadań, ponieważ () najbardziej menedżerów zadania nie są wyraźnie dostępu do konta połączone miejsca do magazynowania i (b konta połączone magazynowania często stanowi do wszystkich zadań typowe ustawienia środowiska dla zadania. Jeśli są one udostępniane konta magazynu połączone za pośrednictwem typowych ustawień środowiska i Menedżer zadań wymaga dostępu do magazynu połączone, następnie należy określić poświadczenia połączonych miejsca do magazynowania w następujący sposób:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    /* other environment settings */
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

**Ustawienia zadania Menedżera zadań**

Klient należy ustawić flagę *killJobOnCompletion* Menedżera zadań **FAŁSZ**.

Zazwyczaj jest bezpieczne dla klienta ustawić *runExclusive* na **wartość false**.

Klient powinien używać kolekcji *resourceFiles* lub *applicationPackageReferences* tego zadania Menedżer wykonywalny (i jego wymagane dll) używany do węzła obliczeń.

Domyślnie Menedżer zadań zostanie nie wykonana ponownie Jeśli go nie powiedzie się. W zależności od logiki Menedżera zadań można włączyć ponowne próby za pośrednictwem *ograniczenia*klienta/*maxTaskRetryCount*.

**Ustawienia zadania**

Jeśli pasek podziału zadania emituje zadań ze współzależnościami, klient wartość true usesTaskDependencies zadania.

W modelu podziału zadania jest nietypowe klientom chcesz dodać zadania do zadania poza tworzy podziału zadania. Klient powinien w związku z tym zwykle ustawienie zadania *onAllTasksComplete* **terminatejob**.

## <a name="task-processor-template"></a>Szablon zadania

Szablon zadania ułatwia wdrożenie procesor zadania, które można wykonać następujące czynności:

* Konfigurowanie informacji wymaganych przez każdego zadania partii do uruchomienia.
* Uruchamianie akcji wszystkich wymaganych przez każdego zadania partię.
* Zapisz wygenerowanie wyjściowe zadania.

Mimo że procesor zadania nie jest wymagane do wykonywania zadań w partii, klucza korzyścią ze stosowania procesor zadania jest opakowanie do wykonania w jednym miejscu wszystkie akcje wykonanie zadania. Na przykład, jeśli jest potrzebne do uruchamiania kilku aplikacji w kontekście każdego zadania lub jeśli chcesz skopiować dane do magazynu trwałego po zakończeniu każdego zadania.

Akcje wykonywane przez procesor zadania może być jako prosty lub zespolonych i wiele lub mało zgodnie z wymaganiami z pracą. Ponadto wdrażając wszystkie akcje zadania w jeden procesor zadania można łatwo zaktualizować lub dodać akcje na podstawie zmian w aplikacji lub wymagania obciążenie pracą. Jednak w niektórych przypadkach procesor zadań mogą nie być optymalne rozwiązanie implementacji można dodać niepotrzebne złożoności, na przykład podczas uruchamiania zadań, które można szybko uruchamiać z prostych wiersza polecenia.

### <a name="create-a-task-processor-using-the-template"></a>Tworzenie procesor zadań przy użyciu szablonu

Aby dodać procesor zadania do rozwiązania, który został utworzony wcześniej, wykonaj następujące czynności:

1. Otwieranie istniejącego rozwiązania w Visual Studio 2015 r.

2. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy rozwiązanie, kliknij przycisk **Dodaj**, a następnie kliknij **Nowy projekt**.

3. W obszarze **Visual C#**kliknij w **chmurze**, a następnie kliknij **Azure partii zadań procesor**.

4. Wpisz nazwę, która zawiera opis aplikacji i identyfikuje tego projektu jako procesor zadania (np. "LitwareTaskProcessor").

5. Aby utworzyć projekt, kliknij **przycisk OK**.

6. Na koniec Utwórz projekt, aby wymusić Visual Studio, aby załadować wszystkie odwołania pakiety NuGet i sprawdź, czy projekt jest prawidłowy, przed rozpoczęciem modyfikowanie.

### <a name="task-processor-template-files-and-their-purpose"></a>Pliki szablonu procesor zadań i cel

Po utworzeniu projektu przy użyciu szablonu procesor zadań generuje trzech grup plików kodu:

* Plik programu głównym (plik Program.cs). Ta strona zawiera punkt wejścia programu i obsługi wyjątków najwyższego poziomu. Zwykle nie należy zmodyfikować to.

* Katalog Framework. Ta strona zawiera pliki odpowiedzialny za "standardowy" pracy przez program Menedżer zadań — Rozpakowywanie parametry, dodawanie zadań do zadania, itd. Zwykle nie należy zmodyfikować te pliki.

* Plik procesor zadania (TaskProcessor.cs). Jest to, gdzie zostanie umieszczony logiki specyficzne dla aplikacji wykonywania zadania (zwykle przez nawiązywanie połączeń ze istniejący plik wykonywalny). Przed i po przetwarzania kod, na przykład pobieranie dodatkowych danych lub przekazywanie plików wynik, również tu.

Oczywiście można dodać dodatkowe pliki wymagane do obsługi kodzie procesor zadania, w zależności od złożoności zadania dzielenie logiki.

Szablon generuje również standardowe pliki projektu .NET, takie jak .csproj pliku app.config, packages.config, itp.

Pozostałą część tej sekcji zawiera opis różnych plików i ich struktury kodu i wyjaśniono, co oznacza każdej kategorii.

![Visual Studio Eksplorator rozwiązań przedstawiający rozwiązanie szablonu procesor zadania][solution_explorer02]

**Struktura plików**

* `Configuration.cs`: Obejmuje załadowaniu danych konfiguracji zadania, takie jak partii szczegółowe informacje o koncie, poświadczenia konta połączone miejsca do magazynowania, zadania i informacje o zadaniu i parametrów zadania. Umożliwia także dostęp do zmiennych środowiska zdefiniowane partii (Zobacz ustawienia środowiska dla zadań w dokumentacji partii) za pomocą klasy Configuration.EnvironmentVariable.

* `IConfiguration.cs`: Abstracts stosowania klasa konfiguracji, tak aby można było testu jednostki do podziału zadań za pomocą obiektu fałszywe lub zasymulować konfiguracji.

* `TaskProcessorException.cs`: Reprezentuje błąd, który wymaga Menedżera zadań w celu zakończenia. TaskProcessorException jest używana do zawijanie błędy "oczekiwanych" gdzie określonych informacji diagnostycznych może być udostępniana w ramach rozwiązania.

**Procesor zadania**

* `TaskProcessor.cs`: Uruchamia zadanie. Ramy wywołuje metodę TaskProcessor.Run. Jest to klasa, gdzie będzie wprowadzić logiki specyficzne dla aplikacji zadania. Implementowanie metodę Uruchom:
  * Analizowanie i sprawdzanie poprawności parametry zadania
  * Zredaguj wiersz polecenia zewnętrzny program, który chcesz wywołać
  * Zaloguj się wszelkie informacje diagnostyczne, które mogą wymagać na potrzeby debugowania
  * Rozpocznij proces przy użyciu tego wiersza polecenia
  * Poczekaj, aż proces zakończy się
  * Przechwytywanie kod zakończenia procesu, aby ustalić, czy zakończyła się pomyślnie, lub nie powiodła się
  * Zapisz wszystkie pliki wyjściowe, które mają zostać zachowane do magazynu trwałego

**Standardowe pliki projektu wiersza polecenia programu .NET**

* `App.config`: Plik konfiguracyjny aplikacji standardowy .NET.
* `Packages.config`: Standardowy plik zależności pakiet NuGet.
* `Program.cs`: Zawiera punkt wejścia programu i obsługi wyjątków najwyższego poziomu.

## <a name="implementing-the-task-processor"></a>Procesor zadań do wykonania

Po otwarciu projektu szablonu procesor zadania projektu uzyskuje plik TaskProcessor.cs domyślnie otwierane. Logika wykonywania zadań z pracą można zaimplementować przy użyciu metody Run(), jak pokazano poniżej:

```csharp
/// <summary>
/// Runs the task processing logic. This is where you inject
/// your application-specific logic for decomposing the job into tasks.
///
/// The task processor framework invokes the Run method for you; you need
/// only to implement it, not to call it yourself. Typically, your
/// implementation will execute an external program (from resource files or
/// an application package), check the exit code of that program and
/// save output files to persistent storage.
/// </summary>
public async Task<int> Run()

{
    try
    {
        //Your code for the task processor goes here.
        var command = $"compare {_parameters["Frame1"]} {_parameters["Frame2"]} compare.gif";
        using (var process = Process.Start($"cmd /c {command}"))
        {
            process.WaitForExit();
            var taskOutputStorage = new TaskOutputStorage(
            _configuration.StorageAccount,
            _configuration.JobId,
            _configuration.TaskId
            );
            await taskOutputStorage.SaveAsync(
            TaskOutputKind.TaskOutput,
            @"..\stdout.txt",
            @"stdout.txt"
            );
            return process.ExitCode;
        }
    }
    catch (Exception ex)
    {
        throw new TaskProcessorException(
        $"{ex.GetType().Name} exception in run task processor: {ex.Message}",
        ex
        );
    }
}
```
>[AZURE.NOTE] Sekcji adnotowanych metody Run() jest tylko sekcji kodu szablonu procesor zadania, która jest przeznaczona do modyfikowania przez dodanie logiki wykonywania zadań z pracą. Jeśli chcesz zmodyfikować innej sekcji szablonu, należy najpierw zapoznać się z działania partii zapoznaniu się z dokumentacją partii i testujesz kilka partii przykłady kodu.

Metoda Run() jest odpowiedzialny za uruchamianie wiersza polecenia, rozpoczynając jeden lub więcej procesów, trwa oczekiwanie na wszystkie podstawowe do wykonania, zapisywanie wyników, a na koniec zwraca kod zakończenia. Metoda Run() to miejsce, w którym zaimplementować logiki przetwarzania zadań. Struktura procesor zadania wywołuje metodę Run() dla nie trzeba zadzwonić samodzielnie.

Implementacji Run() ma dostęp do:

* Parametry zadania za pomocą `_parameters` pola.
* Identyfikatory poleceń i zadań, za pomocą `_jobId` i `_taskId` pola.
* Konfiguracja zadania za pomocą `_configuration` pola.

**Niepowodzenie zadania**

W przypadku błędu możesz wyjść metody Run(), zgłaszanie wyjątku, ale to pozostawia Obsługa wyjątków najwyższego poziomu w kontrolce kodu zakończenia zadania. Jeśli chcesz kontrolować kod zakończenia, tak, aby odróżnić różne rodzaje awarii, na przykład w celach diagnostycznych lub ponieważ niektóre Tryby awaryjne powinien zakończyć zadania i inne osoby nie powinien należy zamknąć metody Run() zwracając kod zakończenia zera. Staje się on kodu zakończenia zadania.

### <a name="exit-codes-and-exceptions-in-the-task-processor-template"></a>Kody zakończenia i wyjątki w szablonie procesor zadania

Kody zakończenia i wyjątki mechanizmu ustalenie wynik działania programu, a może pomóc identyfikowanie problemów z działania programu. Szablon procesor zadania zawiera kody zakończenia i wyjątki opisane w tej sekcji.

Zadania procesor zadania, która jest zaimplementowana za pomocą szablonu procesor zadania może zwracać trzy kody zakończenia możliwe:

| Kod | Opis |
|------|-------------|
|  [Process.ExitCode][process_exitcode] | Uruchomiono procesor zadań do wykonania. Należy zauważyć, że nie oznacza to, że program wywołany zakończyło się pomyślnie — tylko że procesor zadania wywołać ją pomyślnie i wykonać dowolną przetwarzania końcowego bez wyjątków. Znaczenie kodu wyjścia zależy od wywołany program — zwykle kod błędu 0 oznacza, że program zakończyła się pomyślnie i inny kod wyjścia oznacza, że program nie powiodło się. |
| 1    | Procesor zadania nie powiodło się z powodu wyjątku w "oczekiwanych" części programu. Wyjątek został przetłumaczony `TaskProcessorException` z informacji diagnostycznych i, jeśli to możliwe, sugestii na temat naprawiania błędu. |
| 2    | Procesor zadania nie powiodło się z powodu wyjątku "nieoczekiwane". Wyjątek zarejestrowania pojawi się, ale procesor zadania nie może dodać dodatkowe informacje diagnostyczne lub działań naprawczych. |

>[AZURE.NOTE] Jeśli program, który wywoływania używa kodów zakończenia 1 i 2, aby wskazać określonych awarii, następnie przy użyciu kodów zakończenia 1 i 2 dla zadania procesor błędów jest niejednoznacznych. Możesz zmienić kodów błędów procesor zadania do kodów zakończenia charakterystyczne, edytując przypadkach wyjątku w pliku Plik Program.cs.

Wszystkie informacje, które są zwracane przez wyjątki są zapisywane w plikach stdout.txt i stderr.txt. Aby uzyskać więcej informacji zobacz temat obsługi błędów w dokumentacji partię.

### <a name="client-considerations"></a>Zagadnienia dotyczące klientów

**Magazyn poświadczeń**

Jeśli używany procesor zadań korzysta Magazyn obiektów blob platformy Azure pozostać wyjściowe, na przykład za pomocą biblioteki Pomocnik konwencje plik, następnie go musi mieć dostęp do *albo* chmury miejsca do magazynowania klienta poświadczenia *lub* adres URL kontenera obiektów blob, który zawiera podpis udostępniania (SA). Szablon zawiera obsługę podawania poświadczeń za pośrednictwem typowych zmiennych środowiska. Klienta można przekazać poświadczenia miejsca do magazynowania w następujący sposób:

```csharp
job.CommonEnvironmentSettings = new [] {
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

Konto miejsca do magazynowania jest dostępna w klasie TaskProcessor za pośrednictwem `_configuration.StorageAccount` właściwości.

Jeśli wolisz używać adresu URL kontenera do skojarzeń zabezpieczeń, można również przekazać to za pomocą ustawienia środowiska typowe zadania, ale szablon procesor zadań nie zawiera wbudowane obsługę tej.

**Ustawienia pamięci**

Zaleca się, że zadanie manager klienta lub zadania Tworzenie kontenery, wszelkie wymagane przez zadania przed dodaniem zadania do zadania. Jest to konieczne, jeśli używasz adresu URL kontenera do skojarzeń zabezpieczeń, sposób adresu URL nie ma uprawnień do tworzenia kontenera. Zaleca się nawet w przypadku przekazywania poświadczeń konta miejsca do magazynowania, jak zapisuje każdego zadania konieczności połączenia CloudBlobContainer.CreateIfNotExistsAsync w kontenerze.

## <a name="pass-parameters-and-environment-variables"></a>Przekazywane parametry i zmienne środowiska

### <a name="pass-environment-settings"></a>Przekazywać ustawienia środowiska

Klienta można przesłać informacje do Menedżera zadania w formularzu ustawień środowiska. Te informacje mogą być używane przez Menedżera zadania następnie, podczas generowania zadania procesor zadania, które będą działać jako część obliczeń zadania. Informacje, które można przekazać jako ustawień środowiska należą:

* Klucze nazwy i konta konto miejsca do magazynowania
* Adres URL konta partii
* Klucz konta partii

Usługa partii ma prosty mechanizm przekazywać ustawienia środowiska do zadania Menedżera zadań przy użyciu `EnvironmentSettings` właściwości w [Microsoft.Azure.Batch.JobManagerTask][net_jobmanagertask].

Na przykład, aby uzyskać `BatchClient` wystąpienie konta partię, można przekazać jako zmienne środowiska w kliencie kodu adresu URL i udostępnionego klucza poświadczenia konta partię. Analogicznie Aby uzyskać dostęp do konta miejsca do magazynowania, które jest połączone z kontem partii, należy przekazać miejsca do magazynowania klucz konta i nazwę konta magazynu jako zmienne środowiska.

### <a name="pass-parameters-to-the-job-manager-template"></a>Przekazywanie parametrów do szablonu Menedżera zadań

W większości przypadków jest przydatne w celu przekazania parametrów na zadania do zadania menedżera, możesz sterować dzielenie proces zadania lub Konfigurowanie zadania dla zadania. Możesz to zrobić, przekazując JSON pliku o nazwie parameters.json jako plik zasobu do zadań Menedżer zadań. Parametry następnie może stać się dostępne w `JobSplitter._parameters` pola w szablonie Menedżera zadań.

>[AZURE.NOTE] Obsługa wbudowanych parametru obsługuje tylko ciąg znaków do ciągu słowniki. Jeśli chcesz przekazać złożone wartości JSON jako wartości parametrów, należy przekazać je jako ciągi i analizować je w podziału zadania lub zmodyfikować w ramach `Configuration.GetJobParameters` metody.

### <a name="pass-parameters-to-the-task-processor-template"></a>Przekazywanie parametrów do szablonu procesor zadania

Można również przekazywać parametry, aby poszczególne zadania wykonywane przy użyciu szablonu procesor zadania. Podobnie jak za pomocą szablonu Menedżera zadań, szablon procesor zadań wyszukuje o nazwie pliku zasobu

parameters.JSON, a gdy go znaleźć ładuje go jako słownik parametry. Istnieje kilka opcji dotyczących jak przekazać parametry do zadań procesor zadania:

* Ponowne używanie parametrów zadania JSON. To sprawdza się dobrze tylko parametry są pokazane na poziomie zadania (na przykład renderowania wysokość i szerokość). W tym celu podczas tworzenia CloudTask w podziału zadania, Dodaj odwołanie do obiektu pliku zasobu parameters.json z ResourceFiles zadania Menedżera zadań (`JobSplitter._jobManagerTask.ResourceFiles`) do kolekcji ResourceFiles CloudTask.

* Generowanie i Przekaż dokument parameters.json specyficzne dla zadania w ramach wykonywania podziału zadań i odwołanie dany obiekt blob w kolekcji plików zasobów zadania. Jest to konieczne, jeśli różne zadania mają różne parametry. Przykład może być scenariusz renderowanie 3W miejsce, w którym indeks ramki są przekazywane do zadania jako parametru.

>[AZURE.NOTE] Obsługa wbudowanych parametru obsługuje tylko ciąg znaków do ciągu słowniki. Jeśli chcesz przekazać złożone wartości JSON jako wartości parametrów, należy przekazać je jako ciągi i analizować je w procesor zadania lub zmodyfikować w ramach `Configuration.GetTaskParameters` metody.

## <a name="next-steps"></a>Następne kroki

### <a name="persist-job-and-task-output-to-azure-storage"></a>Zmiana jest zachowywana dane wyjściowe i zadań do magazynu platformy Azure

Inne przydatne narzędzie w fazie projektowania rozwiązanie partii jest [Konwencje plik wsadowy Azure][nuget_package]. Użyj tej .NET klasy biblioteki (obecnie w podglądzie) w aplikacjach .NET partii do łatwo przechowywania i pobierania wyjściowe zadania do i z miejsca do magazynowania Azure. [Utrzymują partii Azure i zadań wyjściowy](batch-task-output.md) zawiera pełną treść biblioteki i jej użycia.

### <a name="batch-forum"></a>Forum partii

[Forum partii Azure] [ forum] w witrynie MSDN to dobre miejsce do omówienia partii oraz zadawać pytania dotyczące usługi. Drugi na nad dla pomocne wpisy "lepkich" i Publikuj pytania, gdy występują one podczas tworzenia rozwiązanie partię.

[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[net_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobmanagertask.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[process_exitcode]: https://msdn.microsoft.com/library/system.diagnostics.process.exitcode.aspx
[vs_gallery]: https://visualstudiogallery.msdn.microsoft.com/
[vs_gallery_templates]: https://go.microsoft.com/fwlink/?linkid=820714
[vs_find_use_ext]: https://msdn.microsoft.com/library/dd293638.aspx

[diagram01]: ./media/batch-visual-studio-templates/diagram01.png
[solution_explorer01]: ./media/batch-visual-studio-templates/solution_explorer01.png
[solution_explorer02]: ./media/batch-visual-studio-templates/solution_explorer02.png
