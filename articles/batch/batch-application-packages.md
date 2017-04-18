<properties
    pageTitle="Instalowanie aplikacji łatwe i zarządzanie w partii Azure | Microsoft Azure"
    description="Użyj funkcji pakietów aplikacji partii Azure w prosty sposób zarządzać wielu aplikacji i wersji do instalacji na partię obliczyć węzłów."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="10/21/2016"
    ms.author="marsma" />

# <a name="application-deployment-with-azure-batch-application-packages"></a>Wdrażanie aplikacji z pakietów aplikacji partii Azure

Funkcja pakietów aplikacji partii Azure zawiera łatwe zarządzanie aplikacjami zadań i ich wdrażania węzły obliczeń w puli użytkownika. Pakiety aplikacji możesz przekazać i zarządzanie wieloma wersjami aplikacje, których uruchomić zadań, łącznie z jego pliki pomocnicze. Można następnie automatycznie wdrażać co najmniej jedną z nich do węzłów obliczeń w puli użytkownika.

W tym artykule dowiesz się, jak przekazać i zarządzanie pakietów aplikacji w portalu Azure. Możesz następnie pokażemy, jak zainstalować je w węzłach obliczeń puli program [.NET partii] [ api_net] biblioteki.

> [AZURE.NOTE] Funkcja pakietów aplikacji opisanych tutaj zastępuje funkcję "Partii aplikacje" dostępne we wcześniejszych wersjach usługi.

## <a name="application-package-requirements"></a>Wymagania dotyczące pakietu aplikacji

Należy [łącze konta magazynu platformy Azure](#link-a-storage-account) do swojego konta partii, aby skorzystać z pakietów aplikacji.

Funkcja pakietów aplikacji omawiane w tym artykule jest zgodna *tylko* z pul partii, które zostały utworzone po 10 marca 2016. Pakiety aplikacji nie zostanie wdrożony do obliczenia węzłów na pul utworzone przed tą datą.

Ta funkcja została wprowadzona w [Partii interfejsu API usługi REST] [ api_rest] wersji 2015-12-01.2.2 i odpowiadające im [.NET partii] [ api_net] wersji biblioteki 3.1.0. Zalecamy zawsze używać najnowszej wersji interfejsu API podczas pracy z partii.

> [AZURE.IMPORTANT] Tylko *CloudServiceConfiguration* pul obsługuje obecnie pakietów aplikacji. Nie można użyć pakietów aplikacji pul utworzone przy użyciu VirtualMachineConfiguration obrazy. Zobacz [konfigurację maszyny wirtualnej](batch-linux-nodes.md#virtual-machine-configuration) sekcji [Linux należy obliczyć węzłów na pul partii Azure](batch-linux-nodes.md) uzyskać więcej informacji o dwóch różnych konfiguracji.

## <a name="about-applications-and-application-packages"></a>Informacje o aplikacji i pakietów aplikacji

W partii Azure *aplikacji* odwołuje się do zestawu wersji plików binarnych, które mogą być automatycznie pobrane węzły obliczeń w puli użytkownika. *Pakiet aplikacji* odwołuje się do *określonego zestawu* wartości binarne i reprezentuje danej *wersji* aplikacji.

![Diagram wysokiego poziomu aplikacji i pakietów aplikacji][1]

### <a name="applications"></a>Aplikacje

Aplikacja w partii zawiera jedną lub więcej aplikacji pakietów i określić opcje konfiguracji aplikacji. Na przykład aplikacji można określić wersja pakietu aplikacji domyślnej zainstalować na węzłów obliczeń i czy jego opakowania może być aktualizowane lub usunięte.

### <a name="application-packages"></a>Pakiety aplikacji

Pakiet aplikacji jest plik zip zawierający plików binarnych aplikacji i pliki pomocnicze, które są wymagane do wykonania przez zadań. Każdy pakiet aplikacji reprezentuje określonej wersji aplikacji.

Możesz określić pakietów aplikacji na poziomie puli i zadania. Po utworzeniu puli lub zadania można określić jedną lub więcej z tych pakietów oraz (opcjonalnie) wersję.

* **Pakiety puli aplikacji** są rozmieszczane *Każdy* węzeł w puli. Aplikacje są rozmieszczone, gdy węzeł dołącza puli, a gdy jest uruchomiony ponownie lub reimaged.

    Pakiety puli aplikacji są odpowiednie, wszystkie węzły w puli wykonywania zadania. Po utworzeniu puli, co możesz dodać lub zaktualizować istniejącej puli pakietów można określić jeden lub więcej pakietów aplikacji. Możesz zaktualizować istniejącej puli aplikacji pakietów, należy ponownie uruchomić jego węzłów, aby zainstalować nowy pakiet.

* **Pakiety aplikacji zadania** są wdrażane tylko do węzła obliczeń zaplanowane zadania, po prostu przed uruchomieniem wiersza polecenia zadania. Jeśli pakiet określonej aplikacji i wersja jest już w węźle, go jest nie ponownego rozmieszczania serwera i istniejący pakiet są one używane.

    Pakiety aplikacji zadania są przydatne w środowiskach udostępnione puli, gdzie różne zadania są uruchamiane na jednej puli, a puli nie są usuwane po zakończeniu zadania. Jeśli zadanie ma mniej zadań niż węzłów w puli, pakietów aplikacji zadania można zminimalizować transfer danych, ponieważ aplikacja jest używany tylko węzłów, które wykonywania zadań.

    Inne scenariusze, które mogą korzystać z pakietów aplikacji zadania są zadania, które korzystają szczególności dużej aplikacji, ale małą liczbę zadań. Na przykład przetwarzania wstępnego etap lub zadania korespondencji seryjnej, gdzie aplikację wstępne przetwarzanie lub korespondencji seryjnej jest dużą.

> [AZURE.IMPORTANT] Istnieją ograniczenia liczby aplikacji i pakietów aplikacji w ramach konta partię, a także aplikacji maksymalny rozmiar pakietu. Zobacz [przydziałów i limity dotyczące usługi Azure partii](batch-quota-limit.md) szczegółowe informacje na temat tych ograniczeń.

### <a name="benefits-of-application-packages"></a>Zalety pakietów aplikacji

Pakiety aplikacji można uprościć kod w rozwiązaniu partii i zmniejszyć nakład pracy potrzebny do zarządzania aplikacje, których zadań.

Zadanie początkowe puli użytkownika nie ma Określ o dużej liczbie pliki poszczególnych zasobów, aby zainstalować w węzłach. Nie trzeba ręcznie zarządzać wiele wersji plików aplikacji w magazynie Azure lub węzły. I nie musisz martwić generowania [Skojarzeń zabezpieczeń adresów URL](../storage/storage-dotnet-shared-access-signature-part-1.md) umożliwia dostęp do plików na koncie miejsca do magazynowania. Partia działa w tle z magazynu Azure do przechowywania pakietów aplikacji i wdrażanie ich do obliczenia węzłów.

## <a name="upload-and-manage-applications"></a>Przekazywanie i zarządzanie aplikacjami

Można korzystać z [Azure portal] [ portal] lub w bibliotece [.NET zarządzania partii](batch-management-dotnet.md) do zarządzania pakietów aplikacji na swoim koncie partię. W następnej sekcji kilka możemy najpierw łączenie klienta miejsca do magazynowania, a następnie dodawanie aplikacji i pakietów i zarządzania nimi za pomocą portalu.

### <a name="link-a-storage-account"></a>Łączenie klienta miejsca do magazynowania

Aby użyć pakietów aplikacji, musisz najpierw połączyć konta magazynu platformy Azure do konta w partii. Jeśli nie zostały jeszcze skonfigurowane konto miejsca do magazynowania dla Twojego konta partii, Azure portal wyświetli komunikat ostrzegawczy, przy pierwszym kliknięciu kafelka **aplikacji** w karta **partii konta** .

> [AZURE.IMPORTANT] Partii obecnie obsługuje *tylko* typ konta magazynu **uniwersalny** zgodnie z opisem w kroku 5, [Utwórz konto miejsca do magazynowania](../storage/storage-create-storage-account.md#create-a-storage-account)w [o Azure miejsca do magazynowania konta](../storage/storage-create-storage-account.md). W przypadku łączenia konto Azure miejsca do magazynowania z Twoim kontem partię, łącze *tylko* konto **uniwersalny** miejsca do magazynowania.

![Nie miejsca do magazynowania ostrzeżenie skonfigurowano kont w Azure portal][9]

Usługa partii używa skojarzonego konta miejsca do magazynowania do przechowywania i udostępniania pakietów aplikacji. Po połączeniu dwóch kont, partię automatycznie wdrożyć pakietów przechowywanych w połączony klient miejsca do magazynowania w celu węzły obliczeń. Kliknij pozycję **Ustawienia kont miejsca do magazynowania** na karta **Ostrzeżenie** , a następnie kliknij **Konto miejsca do magazynowania** na karta **Konta miejsca do magazynowania** , aby połączyć konto miejsca do magazynowania konta partii.

![Wybieranie miejsca do magazynowania karta konta w Azure portal][10]

Zalecamy Tworzenie miejsca do magazynowania konta *specjalnie* do użytku z kontem usługi partii i zaznacz ją. Aby uzyskać szczegółowe informacje o sposobie tworzenia konta miejsca do magazynowania zobacz "Tworzenie konta miejsca do magazynowania" w [o Azure miejsca do magazynowania konta](../storage/storage-create-storage-account.md). Po utworzeniu konta miejsca do magazynowania, można je następnie połączyć z kontem partii przy użyciu karta **Konta miejsca do magazynowania** .

> [AZURE.WARNING] Ponieważ partii używa magazynu Azure do przechowywania pakiety aplikacji, jest [naliczany w zwykły sposób] [ storage_pricing] dla danych obiektów blob blok. Pamiętaj uwzględnić rozmiar i liczbę pakiety aplikacji, a okresowo usuwanie przestarzałych pakietów, aby zminimalizować koszt.

### <a name="view-current-applications"></a>Aplikacje bieżącego widoku

Aby wyświetlić listę aplikacji na swoim koncie partię, kliknij element menu **aplikacji** w lewym menu przeglądając karta **partii konta** .

![Kafelek aplikacji][2]

Spowoduje to otwarcie karta **aplikacji** :

![Wyświetlić listę aplikacji][3]

Karta **aplikacji** zostanie wyświetlony identyfikator każdej aplikacji na Twoje konto i następujące właściwości:

* **Pakiety**— liczba wersji skojarzone z tej aplikacji.
* **Domyślna wersja**— wersja zostanie zainstalowana, jeśli nie określisz wersję po ustawieniu puli aplikacji. To ustawienie jest opcjonalne.
* **Zezwalaj na aktualizacje**— wartość, która określa, czy pakiet aktualizacji, usuwania i dodatki są dozwolone. Jeśli to jest ustawiona na **nie**, pakiet aktualizacji i usuwania są wyłączone dla aplikacji. Można dodawać tylko nowej wersji pakietu aplikacji. Wartość domyślna to **Tak**.

### <a name="view-application-details"></a>Wyświetlanie szczegółów zgłoszenia

Kliknij odpowiednią aplikację karta **aplikacje** , aby otworzyć kartę, która zawiera szczegóły dotyczące tej aplikacji.

![Szczegóły aplikacji][4]

W aplikacji karta Szczegóły można skonfigurować następujące ustawienia aplikacji.

* **Zezwalaj na aktualizacje**— Określ, czy jej pakietów aplikacji może być aktualizowane lub usunięte. Zobacz "Aktualizowanie lub usuwanie pakietu aplikacji" w dalszej części tego artykułu.
* **Wersja domyślna**— Określ pakiet aplikacji domyślnej wdrożenia do obliczenia węzły.
* **Nazwa wyświetlana**— określ "przyjazną" Nazwa Twojej partii rozwiązanie można używać podczas wyświetlania informacji na temat aplikacji, takich jak w Interfejsie użytkownika usługi udostępniające klientów za pośrednictwem partii.

### <a name="add-a-new-application"></a>Dodawanie nowej aplikacji

Tworzenie nowej aplikacji, Dodawanie pakietu aplikacji i określ aplikacji nowy, unikatowy identyfikator. Pierwszy pakiet aplikacji z Identyfikatorem nowej aplikacji po dodaniu także spowoduje utworzenie nowej aplikacji.

Kliknij przycisk **Dodaj** na karta **aplikacje** , aby otworzyć karta **nowej aplikacji** .

![Nowe karta aplikacji w Azure portal][5]

Karta **nowej aplikacji** znajdują się następujące pola, aby określić ustawienia nowej aplikacji i pakiet aplikacji.

**Identyfikator aplikacji**

W tym polu określa identyfikator nowej aplikacji, którym podlega standardowy reguł sprawdzania poprawności Identyfikatora partii Azure:

* Mogą zawierać dowolną kombinację znaków alfanumerycznych, w tym łączników i podkreślenia.
* Nie może zawierać więcej niż 64 znaków.
* Musi być unikatowa w partii konta.
* Jest uwzględniana zachowywanie wielkości liter i wielkość liter.

**Wersja**

Określa wersję pakietu aplikacji, które wysyłasz. Ciągi wersji są objęte następujących reguł sprawdzania poprawności:

* Mogą zawierać dowolną kombinację znaków alfanumerycznych, w tym okresy łączniki i podkreślenia.
* Nie może zawierać więcej niż 64 znaków.
* Musi być unikatowa w tej aplikacji.
* Litery zachowania i uwzględniana wielkość liter.

**Pakiet aplikacji**

To pole umożliwia określenie pliku zip, który zawiera plików binarnych aplikacji i plików pomocniczych, które są wymagane do wykonywania aplikacji. Kliknij pole **Wybierz plik** lub ikonę folderu, a następnie odszukaj i wybierz plik zip zawierający pliki aplikacji.

Po wybraniu pliku kliknij **przycisk OK** , aby rozpocząć przekazywania magazyn Azure. Po zakończeniu operacji wysyłania, otrzymasz powiadomienie i karta zostanie zamknięty. W zależności od rozmiaru pliku, którego wysyłasz i szybkość połączenia sieciowego tej operacji może trochę potrwać.

> [AZURE.WARNING] Nie zamykaj karta **nowej aplikacji** , przed zakończeniem operacji wysyłania. To zatrzyma procesu.

### <a name="add-a-new-application-package"></a>Dodawanie nowego pakietu aplikacji

Aby dodać nową wersję pakietu aplikacji dla istniejącej aplikacji, wybierz aplikację w karta **aplikacji** , kliknij pozycję **pakiety**, a następnie kliknij przycisk **Dodaj** , aby otworzyć karta **dodaje pakiet** .

![Dodawanie aplikacji pakietu karta w Azure portal][8]

Jak widać, pola odpowiadają układowi i rozmiarowi karta **nowej aplikacji** , ale pole **Identyfikator aplikacji** jest wyłączone. Tak samo jak nowej aplikacji, określ **wersję** chcesz umieścić nowy pakiet przejdź do pliku zip **pakiet aplikacji** , a następnie kliknij **przycisk OK** , aby przekazać pakiet.

### <a name="update-or-delete-an-application-package"></a>Aktualizowanie lub usuwanie pakietu aplikacji

Aby zaktualizować lub usunąć istniejący pakiet aplikacji, otwórz Karta Szczegóły aplikacji, kliknij pozycję **pakietów** otwarcie karta **pakietów** , kliknij przycisk **wielokropka** w wierszu pakietu aplikacji, którą chcesz zmodyfikować, a następnie wybierz akcję, która ma zostać wykonana.

![Aktualizowanie lub usuwanie pakietu w Azure portal][7]

**Aktualizacja**

Po kliknięciu przycisku **Aktualizacja**zostanie wyświetlona karta *pakiet aktualizacji* . Ta karta jest podobne do karta *pakiet nowej aplikacji* , jednak tylko pola wyboru pakiet jest włączone, co umożliwia określenie nowego pliku ZIP, aby przekazać.

![Karta pakiet aktualizacji w Azure portal][11]

**Usuwanie**

Po kliknięciu przycisku **Usuń**, zostanie wyświetlony monit o potwierdzenie usunięcia wersja pakietu, a partii usuwa pakiet z magazynu Azure. Po usunięciu wersji domyślnej aplikacji ustawienie **domyślnej wersji** zostanie usunięte z aplikacji.

![Usuwanie aplikacji][12]

## <a name="install-applications-on-compute-nodes"></a>Instalowanie aplikacji na węzły obliczeń

Teraz, gdy użytkownik zobaczył, jak zarządzać pakietów aplikacji Portal Azure, można omówimy, jak je obliczyć węzłów i uruchomić je z zadaniami partii wdrożenia.

### <a name="install-pool-application-packages"></a>Zainstalować pakiety puli aplikacji

Aby zainstalować pakiet aplikacji we wszystkich węzłach obliczeń w puli, określ jeden lub więcej aplikacji pakietu *odwołania do* puli. Pakiety aplikacji, określające puli są instalowane w każdym węźle obliczeń węzeł łączy puli, a węzeł jest uruchomiony ponownie lub reimaged.

W partii .NET, określ, co najmniej jeden [CloudPool][net_cloudpool]. [ApplicationPackageReferences] [net_cloudpool_pkgref] po utworzeniu puli nowej lub istniejącej puli. [ApplicationPackageReference] [ net_pkgref] klasa określa identyfikator aplikacji i wersji, aby zainstalować w puli obliczyć węzłów.

```csharp
// Create the unbound CloudPool
CloudPool myCloudPool =
    batchClient.PoolOperations.CreatePool(
        poolId: "myPool",
        targetDedicated: "1",
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Specify the application and version to install on the compute nodes
myCloudPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "litware",
        Version = "1.1001.2b" }
};

// Commit the pool so that it's created in the Batch service. As the nodes join
// the pool, the specified application package will be installed on each.
await myCloudPool.CommitAsync();
```

>[AZURE.IMPORTANT] Jeśli wdrażania pakietu aplikacji z dowolnego powodu nie powiedzie się, usługa partii oznacza węzeł [nie będzie można używać][net_nodestate], a nie zadania są planowane wykonywanie w tym węźle. W tym przypadku należy **ponownie uruchomić** węzeł, aby kliknąć wdrażania pakietu. Ponowne uruchamianie węzeł będzie również włączyć na węźle ponownie harmonogramy zadań.

### <a name="install-task-application-packages"></a>Instalowanie pakietu aplikacji zadania

Podobnie jak w puli, można określić pakiet aplikacji *odwołania do* zadania. Podczas planowania zadania do uruchomienia na węźle pakietu pobraniu i wyodrębnionych tuż przed wykonaniem wiersza polecenia zadania. Jeśli określony pakiet i wersja jest już zainstalowany na węźle, pakietu nie są pobierane i istniejący pakiet jest używana.

Aby zainstalować pakiet aplikacji zadania, należy skonfigurować zadania [CloudTask][net_cloudtask]. [ApplicationPackageReferences] [net_cloudtask_pkgref] właściwości:

```csharp
CloudTask task =
    new CloudTask(
        "litwaretask001",
        "cmd /c %AZ_BATCH_APP_PACKAGE_LITWARE%\\litware.exe -args -here");

task.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference
    {
        ApplicationId = "litware",
        Version = "1.1001.2b"
    }
};
```

## <a name="execute-the-installed-applications"></a>Wykonywanie zainstalowane aplikacje

Pakiety określone dla puli lub zadania są pobierane i wyodrębnione do katalog o nazwie w `AZ_BATCH_ROOT_DIR` węzła. Partia tworzy również zmienna środowisko, która zawiera ścieżkę do katalogu nazwanych. Przy odwoływaniu się do aplikacji w węźle usługi wiersze polecenia zadań za pomocą tej zmiennej środowiska. Zmienna jest w następującym formacie:

`AZ_BATCH_APP_PACKAGE_APPLICATIONID#version`

`APPLICATIONID`i `version` są wartościami, które odpowiadają wersję aplikacji oraz pakiet określone do wdrożenia. Na przykład, jeśli zostanie określona ta wersja 2.7 aplikacji *mieszarce* powinny być zainstalowane, z wierszy polecenia zadania użyć tej zmiennej środowiska dostęp do swoich plików:

`AZ_BATCH_APP_PACKAGE_BLENDER#2.7`

Jeśli użytkownik określi domyślna wersja dla aplikacji, możesz pominąć sufiks wersji. Na przykład jeśli ustawisz "2.7" jako domyślnej wersji dla aplikacji *mieszarce*, zadań można odwoływać się do następujących zmiennej środowiska i będą one wykonywanie wersji 2.7:

`AZ_BATCH_APP_PACKAGE_BLENDER`

Poniższy fragment kodu przedstawia przykład wiersza polecenia zadania uruchamiająca wersji domyślnej aplikacji *mieszarce* :

```csharp
string taskId = "blendertask01";
string commandLine =
    @"cmd /c %AZ_BATCH_APP_PACKAGE_BLENDER%\blender.exe -args -here";
CloudTask blenderTask = new CloudTask(taskId, commandLine);
```

> [AZURE.TIP] Zobacz [ustawień środowiska dla zadań](batch-api-basics.md#environment-settings-for-tasks) w [partii omówienie funkcji](batch-api-basics.md) więcej informacji na temat ustawień środowiska węzeł obliczeń.

## <a name="update-a-pools-application-packages"></a>Aktualizowanie puli pakietów aplikacji

Jeśli skonfigurowano już istniejącej puli z pakietu aplikacji, możesz określić nowego pakietu puli. Jeśli użytkownik określi nowe odwołanie pakiet puli, mają zastosowanie następujące wymagania:

* Wszystkie nowe węzły, które dołączanie puli i istniejący węzeł, który jest uruchomiony ponownie lub reimaged zainstaluje nowo określony pakiet.
* Wyliczenia węzły znajdujących się już w puli podczas aktualizowania odwołań pakiet instaluje automatycznie nowy pakiet aplikacji. Te obliczyć węzły należy ponownie uruchomić lub reimaged do odbierania nowy pakiet.
* Po wdrożeniu nowego pakietu zmienne środowiska utworzone odzwierciedlają nowych odwołań pakiet aplikacji.

W tym przykładzie istniejącej puli jest wersja 2.7 stosowania *mieszarce* skonfigurowanego jako jeden z jej [CloudPool][net_cloudpool]. [ApplicationPackageReferences] [net_cloudpool_pkgref]. Aby zaktualizować węzły w puli wersją 2.76b, określ nowy [ApplicationPackageReference] [ net_pkgref] z nowej wersji i zatwierdź zmiany.

```csharp
string newVersion = "2.76b";
CloudPool boundPool = await batchClient.PoolOperations.GetPoolAsync("myPool");
boundPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "blender",
        Version = newVersion }
};
await boundPool.CommitAsync();
```

Teraz, gdy nowa wersja została skonfigurowana, *Nowy* węzeł, który łączy puli zostanie zainstalowana wersja 2.76b znajdujący się na. Aby zainstalować 2.76b na węzłów, które znajdują się *już* w puli, uruchom ponownie lub ich reimage. Należy zauważyć, że po ponownym rozruchu węzły zachowa pliki z poprzedniego wdrożenia pakietu.

## <a name="list-the-applications-in-a-batch-account"></a>Listy aplikacji na koncie partii

Można wyświetlić listę aplikacji i ich pakietów na koncie partii, przy użyciu [ApplicationOperations][net_appops]. [ListApplicationSummaries] [net_appops_listappsummaries] metody.

```csharp
// List the applications and their application packages in the Batch account.
List<ApplicationSummary> applications = await batchClient.ApplicationOperations.ListApplicationSummaries().ToListAsync();
foreach (ApplicationSummary app in applications)
{
    Console.WriteLine("ID: {0} | Display Name: {1}", app.Id, app.DisplayName);

    foreach (string version in app.Versions)
    {
        Console.WriteLine("  {0}", version);
    }
}
```

## <a name="wrap-up"></a>Zawijanie w górę

Z pakietów aplikacji może pomóc klientom Wybierz aplikacje na zadań i określ dokładnie wersji do użycia podczas przetwarzania zadania z obsługą partii usługą. Może być zapewniają również możliwość klientom przekazywanie i śledzenie własne aplikacje w usłudze.

## <a name="next-steps"></a>Następne kroki

* [Interfejsu API usługi REST partii] [ api_rest] obsługuje również do pracy z pakietów aplikacji. Na przykład, zobacz [applicationPackageReferences] [ rest_add_pool_with_packages] element [Dodaj puli z klientem] [ rest_add_pool] uzyskać informacji na temat określania pakiety do zainstalowania za pomocą interfejsu API usługi REST. Zobacz [aplikacji] [ rest_applications] szczegółowe informacje na temat sposobu uzyskiwania informacji o aplikacji przy użyciu interfejsu API usługi REST partię.

* Dowiedz się, jak programowo [zarządzać kontami partii Azure i przydziałów z partii zarządzania .NET](batch-management-dotnet.md). [.NET zarządzania partii] [ api_net_mgmt] biblioteki można włączyć funkcje tworzenia i usuwania konta dla partii aplikacji lub usługi.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[github_samples]: https://github.com/Azure/azure-batch-samples
[storage_pricing]: https://azure.microsoft.com/pricing/details/storage/
[net_appops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.aspx
[net_appops_listappsummaries]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.listapplicationsummaries.aspx
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_cloudpool_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.applicationpackagereferences.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.aspx
[net_cloudtask_pkgref]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.applicationpackagereferences.aspx
[net_nodestate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.state.aspx
[net_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationpackagereference.aspx
[portal]: https://portal.azure.com
[rest_applications]: https://msdn.microsoft.com/library/azure/mt643945.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_pool_with_packages]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_apkgreference

[1]: ./media/batch-application-packages/app_pkg_01.png "Diagram wysokiego poziomu pakietów aplikacji"
[2]: ./media/batch-application-packages/app_pkg_02.png "Kafelek aplikacji w Azure portal"
[3]: ./media/batch-application-packages/app_pkg_03.png "Karta aplikacji w Azure portal"
[4]: ./media/batch-application-packages/app_pkg_04.png "Karta Szczegóły aplikacji w Azure portal"
[5]: ./media/batch-application-packages/app_pkg_05.png "Nowe karta aplikacji w Azure portal"
[7]: ./media/batch-application-packages/app_pkg_07.png "Aktualizowanie lub usuwanie pakietów listę rozwijaną w Azure portal"
[8]: ./media/batch-application-packages/app_pkg_08.png "Nowe karta pakiet aplikacji w Azure portal"
[9]: ./media/batch-application-packages/app_pkg_09.png "Alert nie połączone konto miejsca do magazynowania"
[10]: ./media/batch-application-packages/app_pkg_10.png "Wybieranie miejsca do magazynowania karta konta w Azure portal"
[11]: ./media/batch-application-packages/app_pkg_11.png "Karta pakiet aktualizacji w Azure portal"
[12]: ./media/batch-application-packages/app_pkg_12.png "Usuwanie pakietu okno dialogowe potwierdzenia w Azure portal"
