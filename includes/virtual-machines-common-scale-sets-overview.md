

W przypadku aplikacji, które powinny przeskalować zasobów obliczeń, a w skali, które operacje niejawnie są zbilansowane domen usterek i aktualizacji. Wprowadzenie do skali maszyn wirtualnych zestawy odwołują się do ostatnio używane [Zawiadomienie o Azure blogu](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/).

Zapoznaj się te klipy wideo, aby uzyskać więcej informacji na temat zestawów skali maszyn wirtualnych:

 - [Oznaczanie rozmów Russinovichem zestawy skali Azure](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  

 - [Skala maszyn wirtualnych zestawy z Bowerman specjalisty z zakresu](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## <a name="creating-and-managing-vm-scale-sets"></a>Tworzenie i zarządzanie nimi skali maszyn wirtualnych zestawów

Skala maszyn wirtualnych można zdefiniować i wdrożyć za pomocą szablonów JSON i [Interfejsów API pozostałych](https://msdn.microsoft.com/library/mt589023.aspx) tylko zestawy, takich jak poszczególne maszyny wirtualne Menedżera zasobów Azure. W związku z tym można używać dowolnego standardowego metody wdrażania Azure Menedżera zasobów. Aby uzyskać więcej informacji na temat szablonów zobacz [Szablony do tworzenia Azure Menedżera zasobów](../articles/resource-group-authoring-templates.md).

Zestaw szablonów przykład dla zestawów skali maszyn wirtualnych można znaleźć w repozytorium GitHub szablony Szybki Start Azure poniżej:

[https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates) - wyglądu dla szablonów z _vmss_ w tytule.

Dla tych szablonów na stronach Szczegóły pojawi się przycisk, który odwołuje się do portalu wdrażania funkcji. Aby wdrożyć zestaw skali maszyn wirtualnych, kliknij przycisk, a następnie wypełnij parametry, które są wymagane w portalu. Jeśli nie masz pewności, czy zasób obsługuje wielkie lub małe litery jest bezpieczniejsze zawsze używaj wartości parametrów małe litery. Istnieje także przydatnego sekcji wideo maszyn wirtualnych Skala Ustaw szablonu poniżej:

[Ustawianie skali maszyn wirtualnych sekcji szablonu](https://channel9.msdn.com/Blogs/Windows-Azure/VM-Scale-Set-Template-Dissection/player)

## <a name="scaling-a-vm-scale-set-out-and-in"></a>Skalowanie skalę maszyn wirtualnych Ustawianie i

Aby zwiększyć lub zmniejszyć liczbę maszyn wirtualnych w zestawie skali maszyn wirtualnych, po prostu zmienić właściwość _pojemność_ i ponownie wdróż szablonu. Ten uproszczenia ułatwia pisanie własnych niestandardowych skalowania warstwy, jeśli chcesz zdefiniować Skala niestandardowa zdarzenia, które nie są obsługiwane przez Azure autoscale.

Jeśli jest ponowne rozmieszczanie szablonu, aby zmienić pojemność, można zdefiniować wiele mniejszych szablon, który obejmuje tylko używaną WERSJĘ i możliwości zaktualizowane. Poniżej przedstawiono przykład: [https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-scale-in-or-out/azuredeploy.json](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-linux-nat/azuredeploy.json).

Aby mógłby wykonać kroki tworzonych przez zestaw skali, który jest automatycznie skalowany, zobacz [Automatycznie skali maszyny w zestawie skali maszyn wirtualnych](../articles/virtual-machines/virtual-machines-windows-vmss-powershell-creating.md)

## <a name="monitoring-your-vm-scale-set"></a>Ustawianie monitorowania na skalę maszyn wirtualnych

Obecnie zalecane jest wyświetlanie zestawów skali maszyn wirtualnych za pomocą [Eksploratora zasobów Azure](https://resources.azure.com) . Zestawy skali maszyn wirtualnych są zasobu w obszarze Microsoft.Compute, więc z tej witryny były widoczne po rozwinięciu z poniższych łączy:

    subscriptions -> your subscription -> resourceGroups -> providers -> Microsoft.Compute -> virtualMachineScaleSets -> your VM scale set -> etc.

## <a name="vm-scale-set-scenarios"></a>Ustawianie skali maszyn wirtualnych scenariuszy

W tej sekcji przedstawiono kilka typowych scenariuszy zestawu skali maszyn wirtualnych. Niektóre wyższy poziom usługi Azure (na przykład partii, tkaninie usługi, usługa kontenera Azure) będzie używał tych scenariuszy.

 - **RDP / SSH skali maszyn wirtualnych set wystąpienia** — Ustawianie skali maszyn wirtualnych jest tworzony wewnątrz VNET i poszczególnych maszyny wirtualne w nie są przydzielane publicznych adresów IP. Jest to użyteczna, ponieważ nie ma zazwyczaj ogólnych wydatków i zarządzanie podziału osobne adresy IP do wszystkich stateless zasobów w siatce obliczeń i można łatwo nawiązać te maszyny wirtualne od innych zasobów w Twojej VNET, w tym te, które mają publiczne adresy IP, takich jak urządzenia do równoważenia obciążenia lub maszyn wirtualnych autonomicznego.

 - **Nawiązywanie połączenia przy użyciu reguł translatora adresów Sieciowych maszyny wirtualne** — tworzenie publiczny adres IP, przypisz go do równoważenia obciążenia i Definiowanie reguły przychodzące translatora adresów Sieciowych mapujące portu na adres IP do portu maszyn wirtualnych w zestawie skali maszyn wirtualnych. Np.

    Publiczny adres IP -> portu 50000 -> vmss\_0 -> Port 22 publicznej IP - > Port 50001 -> vmss\_1 -> Port 22 publicznej IP - > Port 50002 -> vmss\_2 -> Port 22

    Oto przykład tworzenia zestaw skali maszyn wirtualnych, który używa reguły translatora adresów Sieciowych, aby umożliwić połączenie SSH każdej maszyn wirtualnych w skali zestaw przy użyciu jednego publiczny adres IP: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat)

    Oto przykład wykonujące te same z RDP i systemu Windows: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat)

 - **Nawiązywanie połączenia przy użyciu "jumpbox" maszyny wirtualne** — po utworzeniu skali maszyn wirtualnych ustaw i autonomicznego maszyn wirtualnych w tym samym VNET, autonomicznego maszyn wirtualnych i odpowiedni zestaw skali maszyn wirtualnych maszyny wirtualne można nawiązać za pomocą wewnętrznych IP adresy zdefiniowane przez VNET-podsieci. Jeśli tworzysz publiczny adres IP i przypisać go do autonomicznej maszyn wirtualnych do autonomicznej maszyn wirtualnych możesz RDP lub SSH, a następnie nawiązać połączenie z tego komputera z wystąpienia zestawu skali maszyn wirtualnych. W tym momencie może być Zauważ, że zestaw prosty skali maszyn wirtualnych jest założenia bezpieczniejsze niż prosty autonomicznego maszyn wirtualnych przy użyciu publicznego adresu IP w konfiguracji domyślnej.

    Na przykład tej metody ten szablon umożliwia utworzenie prostej klaster Mesos składający się z w wersji autonomicznej maszyn wirtualnych wzorca, która zarządza klastrze maszyn wirtualnych Skala Ustaw podstawie maszyny wirtualne: [https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json)

 - **Okrężnego równoważenia obciążenia skali maszyn wirtualnych Ustawianie wystąpienia** — Jeśli chcesz przedstawić pracy do klastrów obliczeniowych: maszyny wirtualne za pomocą sposób "okrężny", można skonfigurować równoważenia obciążenia Azure równoważenia obciążenia odpowiednio reguły. Można także zdefiniować sondy w celu zweryfikowania, aplikacja jest uruchomiona przez polecenie ping porty z określonej ścieżce protokół, interwału i wezwanie.

    Oto przykładowy, który tworzy zestaw skali maszyn wirtualnych maszyny wirtualne uruchomiony serwer sieci web usług IIS i korzysta z usługi równoważenia obciążenia do równoważenia obciążenia otrzymuje poszczególnych maszyn wirtualnych. Także używa protokołu HTTP do określonego adresu URL na poszczególnych maszyn wirtualnych ping: [https://github.com/gbowerman/azure-myriad/blob/master/vmss-win-iis-vnet-storage-lb.json](https://github.com/gbowerman/azure-myriad/blob/master/vmss-win-iis-vnet-storage-lb.json) - przyjrzeć się typ zasobu Microsoft.Network/loadBalancers i networkProfile i extensionProfile w virtualMachineScaleSet.

 - **Wdrażanie skalę maszyn wirtualnych Ustaw jako klastrów obliczeniowych w Menedżerze klaster PaaS** - Skala maszyn wirtualnych zestawy są nazywane także roli Pracownik generacji. Jest prawidłowy opis ale także ryzyka skomplikowana skali zestawu funkcji z funkcjami roli Pracownik PaaS w wersji 1. W ujęciu skali maszyn wirtualnych zestawy Podaj wartość PRAWDA "roli Pracownik" lub zasób pracownika, w tym zapewniają zasób uogólniony obliczeń, który jest platformy-środowisko uruchomieniowe niezależne, można dostosować i integruje Azure IaaS Menedżera zasobów.

    Roli PaaS pracownika w wersji 1, podczas gdy ograniczone pod względem obsługi platformy/runtime (tylko obrazów platformy systemu Windows) zawiera również usług, takich jak wymiany VIP, można konfigurować ustawienia uaktualnienia, środowisko uruchomieniowe i wdrażania określonych ustawień aplikacji, które nie są _jeszcze_ dostępne w maszyn wirtualnych przeskalować zestawy lub większością innych wyższy poziom usługi PaaS, takie jak usługa tkaninie. Dzięki temu w pamiętać, że mogą przeglądać zestawy skali maszyn wirtualnych jako infrastrukturę, który obsługuje PaaS. To znaczy Rozwiązania PaaS, takich jak usługa tkaninie lub menedżerowie klaster, takich jak Mesos można tworzyć na bieżąco maszyn wirtualnych skalowanie zestawy jako warstwę skalowalna obliczeń.

    Oto przykład klaster Mesos, który wdraża skali maszyn wirtualnych w tym samym VNET w wersji autonomicznej maszyn wirtualnych Ustaw. Autonomicznego maszyn wirtualnych jest wzorzec Mesos i Ustawianie skali maszyn wirtualnych reprezentuje zestaw węzły podrzędne: [https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json). Przyszłych wersjach [Usługa Azure kontenera](https://azure.microsoft.com/blog/azure-container-service-now-and-the-future/) będą wdrażać więcej zespolonej kula wersji tego scenariusza oparta na zestawach skali maszyn wirtualnych.

## <a name="vm-scale-set-performance-and-scale-guidance"></a>Skala maszyn wirtualnych Ustaw wydajność i skalowanie wskazówki

- Podczas publicznej podglądu nie należy tworzyć więcej niż 500 maszyny wirtualne w wiele zestawów skali maszyn wirtualnych jednocześnie.
- Plan nie więcej niż 40 maszyny wirtualne dla każdego konta miejsca do magazynowania.
- Rozłożone pierwszych liter nazwiska możliwie nazwy kont miejsca do magazynowania.  Przykładowe szablony VMSS w [szablonach Szybki Start Azure](https://github.com/Azure/azure-quickstart-templates/) zawierają przykłady wykonywania tego zadania.
- Jeśli używasz niestandardowych maszyny wirtualne, planowanie nie więcej niż 40 maszyny wirtualne na zestaw skali maszyn wirtualnych, z jednego miejsca do magazynowania konta.  Konieczne będzie obraz wstępnie skopiowany pod uwagę miejsca do magazynowania, przed rozpoczęciem wdrażania zestawu skali maszyn wirtualnych. Zobacz często zadawane pytania, aby uzyskać więcej informacji.
- Plan nie więcej niż 2048 maszyny wirtualne na VNET.  Ten limit zostanie zwiększony w przyszłości.
- Liczba maszyny wirtualne można utworzyć jest ograniczona przez przydziału podstawowych obliczeń w dowolnym regionie. Konieczne może się z pomocą techniczną, aby zwiększyć limit przydziału obliczeń zwiększone, nawet jeśli masz już dziś wysoki limit rdzeni do użytku z usługami w chmurze lub IaaS w wersji 1. Do kwerendy przydziału można uruchom następujące polecenie polecenie Azure: _Użycie listy azure maszyn wirtualnych_i następującego polecenia programu PowerShell: _Get-AzureRmVMUsage_ (jeśli jest używana wersja programu PowerShell poniżej 1.0 użyj _Get-AzureVMUsage_).

## <a name="vm-scale-set-frequently-asked-questions"></a>Skala maszyn wirtualnych ustawianie często zadawane pytania

**PYTANIA.** Ile maszyny wirtualne może mieć w zestawie skali maszyn wirtualnych?

**ODPOWIEDZI.** 100, jeśli korzystasz z platformy obrazów, które mogą być rozpowszechniane w ramach wielu kont miejsca do magazynowania. Jeśli używasz niestandardowych obrazów do 40, ponieważ niestandardowe obrazy są ograniczone do konta jednego miejsca do magazynowania w wersji preview.

**Pytania** dla zestawów skali maszyn wirtualnych istnieją inne limity zasobów?

**ODPOWIEDZI.** Możesz są ograniczone do tworzenia nie więcej niż 500 maszyny wirtualne w wiele zestawów skali w rozbiciu na regiony w okresie Podgląd. Istniejące [limity Azure subskrypcji usługi-](../articles/azure-subscription-service-limits.md) stosowanie.

**PYTANIA.** Są obsługiwane dyski danych w zestawy skali maszyn wirtualnych?

**ODPOWIEDZI.** Nie w wersji początkowej. Dostępne są następujące opcje przechowywania danych:

- Azure plików (SMB udostępnione dyski)

- Dysk systemu operacyjnego

- Dysk TEMP (lokalny, nie kopii przez Magazyn Azure)

- Usługi danych zewnętrznych (zdalnej bazy danych, Azure tabel, obiektów blob platformy Azure)

**PYTANIA.** Regiony Azure obsługę zestawów skali maszyn wirtualnych?

**ODPOWIEDZI.** Dowolny region, który obsługuje Menedżera zasobów Azure obsługuje zestawy skali maszyn wirtualnych.

**PYTANIA.** Jak utworzyć skalę maszyn wirtualnych zestawy przy użyciu niestandardowego obrazu?

**ODPOWIEDZI.** Puste właściwość vhdContainers, na przykład:

    "storageProfile": {
        "osDisk": {
            "name": "vmssosdisk",
            "caching": "ReadOnly",
            "createOption": "FromImage",
            "image": {
                "uri": [https://mycustomimage.blob.core.windows.net/system/Microsoft.Compute/Images/mytemplates/template-osDisk.vhd](https://mycustomimage.blob.core.windows.net/system/Microsoft.Compute/Images/mytemplates/template-osDisk.vhd)
            },
            "osType": "Windows"
        }
    },


**PYTANIA.** Jeśli zmniejszyć Moje skali maszyn wirtualnych Ustaw pojemności 20 15, które zostaną usunięte pośrednictwem SMS?

**ODPOWIEDZI.** Maszyn wirtualnych zostaną usunięte z Skala Ustaw równomiernie wielu błędów a domenami uaktualnienia maksymalne zwiększenie dostępności.

**PYTANIA.** Jak o niego, o ile następnie zwiększyć wydajność od 15-18?

**ODPOWIEDZI.** Jeśli zwiększysz do 18, maszyny wirtualne z indeksem 15, 16, 17 zostanie utworzona. W obu przypadkach maszyny wirtualne są zbilansowane różnych FDs i UDs.

**PYTANIA.** Gdy używasz wielu rozszerzenia w zestawie skali maszyn wirtualnych, może wymusić kolejność wykonywania?

**ODPOWIEDZI.** Nie bezpośrednio, ale z rozszerzeniem customScript skrypt można zaczekać, aż inne rozszerzenie do wykonania (na przykład przez monitorowanie rozszerzenie logowania — zobacz [https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install\_lap.sh](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install_lap.sh)).

**PYTANIA.** Zestawy skali maszyn wirtualnych działają z zestawami dostępność Azure?

**ODPOWIEDZI.** Wartość Tak. Zestaw skali maszyn wirtualnych jest niejawne dostępność zestaw z 3 FDs i 5 UDs. Nie trzeba konfigurować w obszarze virtualMachineProfile. W przyszłych wersjach, zestawy skali maszyn wirtualnych mogą obejmować kilka dzierżaw, ale teraz ustawić skalę to dostępność pojedynczy zestaw.
