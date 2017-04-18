W tym artykule omówiono zestaw sprawdzone wskazówki dotyczące uruchamiania maszyny wirtualnej (maszyn wirtualnych) systemu Windows Azure, zwracając uwagę na skalowalność, dostępność, zarządzanie i zabezpieczeń. 

> [AZURE.NOTE] Azure ma dwa różne wdrożenia modele: [Menedżer zasobów Azure] [ resource-manager-overview] i klasyczny. W tym artykule używa Menedżera zasobów, które firma Microsoft zaleca się w przypadku wdrożeń nowy.

Nie zalecamy przy użyciu jednego maszyn wirtualnych dla obciążeń pracą produkcji, ponieważ istnieje nie wydłużyć czas Umowa dotycząca poziomu usług (SLA) dla pojedynczej maszyny wirtualne Azure. Aby uzyskać SLA, należy wdrożyć wielu maszyny wirtualne [Ustawianie dostępności][availability-set]. Aby uzyskać więcej informacji, zobacz [Uruchamianie wielu Windows maszyn wirtualnych Azure][multi-vm]. 

## <a name="architecture-diagram"></a>Diagram architektury

Inicjowania obsługi administracyjnej maszyn wirtualnych w Azure obejmuje więcej ruchomych elementów niż tylko maszyn wirtualnych się. Istnieją elementy obliczeń, sieci i miejsca do magazynowania.

> Dokument programu Visio, który zawiera ten diagram architektury jest dostępna do pobrania w [Centrum pobierania firmy Microsoft][visio-download]. Ten diagram znajduje się na "obliczeń - pojedynczej strony maszyn wirtualnych.

![[0]][0]


- **Grupa zasobów.** [_Grupa zasobów_] [ resource-manager-overview] jest kontener zasoby pokrewne. Tworzenie grupy zasobów do przechowywania zasobów dla tego maszyn wirtualnych.

- **VM**. Umożliwia obsługę maszyny z listy opublikowanych obrazów lub pliku wirtualnego dysku twardego (VHD), które wysyłasz z magazynem obiektów blob platformy Azure.

- **Dysk systemu operacyjnego.** Dysk systemu operacyjnego jest wirtualnego dysku twardego przechowywane w [magazynie Azure][azure-storage]. Oznacza to, że istnieje, nawet jeśli na komputerze obsługującym awarii.

- **Tymczasowe dysk.** Maszyn wirtualnych jest tworzona dyskiem tymczasowe ( `D:` dysk w systemie Windows). Dysk jest przechowywany na dysku fizycznym na komputerze hoście. To _nie_ zapisane w magazynie Azure i mogą przejść z dala od komputera podczas ponownego uruchamiania i inne zdarzenia cyklu życia maszyn wirtualnych. Za pomocą tego dysku tylko w przypadku tymczasowych danych, takich jak pliki strony lub Zamień.

- **Dyski danych.** [Dysku danych] [ data-disk] jest trwała wirtualnego dysku twardego na potrzeby dane aplikacji. Dyski dane są przechowywane w magazynie Azure, takie jak dysk systemu operacyjnego.

- **Wirtualna sieć (VNet) i podsieci.** Co maszyn wirtualnych w Azure zostanie wdrożony w VNet, które podzielić na podsieci.

- **Publiczny adres IP.** Publiczny adres IP jest potrzebny do komunikowania się z maszyn wirtualnych&mdash;na przykład przez pulpitu zdalnego (RDP).

- **Sieciowej (NIC)**. Karta sieciowa umożliwia maszyn wirtualnych można komunikować się z wirtualną sieć.

- **Grupa zabezpieczeń sieci (NSG)**. [NSG] [ nsg] służy do Zezwalaj odmówić ruch sieciowy do podsieci. NSG można skojarzyć z poszczególnych NIC lub z sieci. Jeśli powiążesz z podsieci reguły NSG dotyczą wszystkich maszyny wirtualne w tej podsieci.
 
- **Narzędzia diagnostyczne.** Rejestrowanie diagnostyczne ma kluczowe znaczenie dla zarządzanie i rozwiązywanie problemów z maszyn wirtualnych.

## <a name="recommendations"></a>Zalecenia

Azure oferuje wiele różnych zasobów i typów zasobów, aby ta architektura odwołanie może być obsługi administracyjnej wiele różnych sposobów. Utworzono szablonu Menedżera zasobów Azure zainstalować architektury odwołania, która obejmuje następujące zalecenia. Jeśli chcesz utworzyć własny architektura odwołania, należy wykonać tych zaleceń, jeśli nie masz określonych wymóg, aby zalecenie nie mieści się.

### <a name="vm-recommendations"></a>Zalecenia dotyczące maszyn wirtualnych

Zalecamy serii GS i Zasadami, ponieważ te rozmiary komputer obsługuje [Magazynowania Premium][premium-storage]. Wybierz jeden z rozmiary komputera, jeśli nie masz specjalistyczne obciążenie pracą, takie jak wysokiej wydajności. Aby uzyskać szczegółowe informacje, zobacz [rozmiarów maszyn wirtualnych][virtual-machine-sizes]. Podczas przenoszenia istniejącego pracą Azure, rozpoczyna się rozmiar pamięci Wirtualnej, który najlepiej pasuje do serwerów lokalnego. Następnie miary wydajności usługi rzeczywistego obciążenia dotyczące Procesora, pamięci i dysku wyjścia operacji na sekundę (operacji i/o na SEKUNDĘ) i dopasowywanie rozmiaru, w razie potrzeby. Ponadto w razie potrzeby kart, należy pamiętać o limit NIC dla każdego rozmiaru.  

Gdy przepisu maszyn wirtualnych i inne zasoby należy określić lokalizację. Ogólnie rzecz biorąc wybierz lokalizację do użytkowników wewnętrznych lub klientów. Jednak nie wszystkie rozmiary maszyn wirtualnych mogą być dostępne we wszystkich lokalizacjach. Aby uzyskać szczegółowe informacje, zobacz [usługi według regionów][services-by-region]. Aby wyświetlić listę dostępnych rozmiarów maszyn wirtualnych w określonej lokalizacji, uruchom następujące polecenie Azure interfejs wiersza polecenia (polecenie):

```
    azure vm sizes --location <location>
```

Uzyskać informacji na temat wybierania opublikowanych obraz maszyn wirtualnych, zobacz [Nawigacja i wybierz pozycję Azure maszyn wirtualnych obrazów][select-vm-image].

### <a name="disk-and-storage-recommendations"></a>Zalecenia dotyczące dysku i miejsca do magazynowania

Celu uzyskania najlepszej wydajności dyskowej zalecamy [Magazynowania Premium][premium-storage], które są przechowywane dane w stanie stałym dyski SSD. Koszt jest oparty na rozmiar ustanawianie dysku. Operacji i/o na SEKUNDĘ i przepustowość również zależy od rozmiaru dysku, więc podczas postanowień dyskiem, należy rozważyć trzy czynniki (pojemność, operacji i/o na SEKUNDĘ i przepustowość). 

Jedno konto miejsca do magazynowania w stanie obsługiwać maszyny wirtualne 1 – 20.

Dodawanie jednego lub większej liczby dysków danych. Po utworzeniu nowego wirtualny dysk twardy jest sformatowany. Zaloguj się do maszyn wirtualnych sformatować dysk.

Jeśli masz dużą liczbą dyski danych, należy pamiętać o całkowity limity we/wy konta miejsca do magazynowania. Aby uzyskać więcej informacji, zobacz [Limity dysku maszyn wirtualnych][vm-disk-limits].

Aby uzyskać optymalną wydajność Utwórz konto przechowywania do przechowywania dzienniki diagnostyczne. Konto standardowe lokalnie zbędne przestrzeni dyskowej (LRS) jest umożliwiający dzienniki diagnostyczne.

Jeśli to możliwe, należy zainstalować aplikacje na dysku danych nie dysku systemu operacyjnego. Jednak niektórych starszych aplikacji może być konieczne zainstalowanie składników na dysku C:. W takim przypadku można [zmienić rozmiar dysku z systemem operacyjnym] [ resize-os-disk] przy użyciu programu PowerShell.

### <a name="network-recommendations"></a>Zalecenia dotyczące sieci

Publiczny adres IP może być dynamiczne lub statyczne. Wartość domyślna to dynamiczne.

- Rezerwowanie [statycznego adresu IP] [ static-ip] czy jest potrzebny stały adres IP, który nie ulegnie zmianie &mdash; na przykład, czy potrzebne do utworzenia rekordu A w systemie DNS, muszą być whitelisted adres IP.

- Można także tworzyć w pełni kwalifikowaną nazwę domeny (FQDN) z adresem IP. Następnie należy zarejestrować [rekordu CNAME] [ cname-record] w systemie DNS wskazującego na nazwy FQDN. Aby uzyskać więcej informacji, zobacz [Tworzenie w pełni kwalifikowaną nazwę domeny w portalu Azure][fqdn].

Wszystkie NSGs zawierają zestaw [domyślnych reguł][nsg-default-rules], łącznie z regułę, która blokuje cały ruch przychodzący Internet. Nie można usunąć domyślne reguły, ale innych reguł można zastąpić je. Aby włączyć ruchu internetowego, utwórz reguły zezwalające na ruch przychodzący do określonych portów &mdash; na przykład port 80 dla protokołu HTTP.  

Aby włączyć RDP, dodać regułę NSG, która zezwala na ruch przychodzący do portu TCP 3389.

## <a name="scalability-considerations"></a>Zagadnienia dotyczące skalowalność

Można skalować maszyny w górę lub w dół, [zmieniając rozmiar pamięci Wirtualnej][vm-resize]. 

Przeskalować się poziomo, umieszczać co najmniej dwa maszyny wirtualne w dostępność ustawianie za usługi równoważenia obciążenia. Aby uzyskać szczegółowe informacje, zobacz [Uruchamianie wielu Windows maszyn wirtualnych Azure][multi-vm].

## <a name="availability-considerations"></a>Zagadnienia dotyczące dostępności

Jak wspomniano powyżej, jest umowa dotycząca poziomu usług dla pojedynczego maszyn wirtualnych. Aby uzyskać Umowa dotycząca poziomu usług, należy wdrożyć wielu maszyny wirtualne w zestawie dostępności.

Usługi maszyn wirtualnych może dotyczyć [Planowana konserwacja] [ planned-maintenance] lub [konserwacji niezaplanowane][manage-vm-availability]. Możesz użyć [maszyn wirtualnych Uruchom ponownie dzienniki] [ reboot-logs] do określenia, czy ponownego uruchomienia maszyn wirtualnych była spowodowana planowanej konserwacji.

Kopii wirtualnych dysków twardych przez [Magazyn Azure][azure-storage], które są replikowane wytrzymałości i dostępność.

Aby zabezpieczyć przed utratą danych przypadkowe podczas normalnego działania (na przykład ze względu na błąd użytkownika), również wykonała kopii zapasowych w chwili, przy użyciu [obiektów blob migawek] [ blob-snapshot] lub innego narzędzia.

## <a name="manageability-considerations"></a>Zagadnienia dotyczące zarządzania

**Grupy zasobów.** Umieszczanie ściśle związane zasoby, które współużytkują tego samego cyklu życia do tej samej [grupy zasobów][resource-manager-overview]. Grupy zasobów umożliwia wdrażanie i monitorowanie zasobów jako grupy i rzutowanie rachunków kosztów według grup zasobów. Możesz również usunąć zasoby jako zestaw, który jest bardzo przydatne w przypadku wdrożeń test. Nadaj zasobów zrozumiałej nazwy. Który ułatwia Znajdź określonego zasobu i opis roli. Zobacz [zalecane konwencje nazewnictwa dla zasobów Azure][naming conventions].

**Narzędzia diagnostyczne maszyn wirtualnych.** Włącz monitorowania i diagnostyki, w tym metryki kondycji podstawowe, dzienników infrastruktury Diagnostyka i [uruchamiania narzędzia diagnostyczne][boot-diagnostics]. Diagnostyka uruchamiania może ułatwić diagnozowanie błąd uruchamiania, jeśli do maszyn wirtualnych otrzymuje stan startowy. Aby uzyskać więcej informacji, zobacz [Włączanie monitorowania i diagnostyki][enable-monitoring]. [Zbieranie dzienników Azure] za pomocą[ log-collector] rozszerzenia zbieranie dzienników platformy Azure i przekazać je do magazynu Azure.   

Następujące polecenie polecenie włącza narzędzia diagnostyczne:

```
    azure vm enable-diag <resource-group> <vm-name>
```

**Zatrzymywanie maszyny.** Azure rozróżnia "Zatrzymania" i "Deallocated". Są naliczane po stanu maszyn wirtualnych "zatrzymaniu". Możesz nie są naliczane, gdy alokację maszyn wirtualnych.

Użyj następującego polecenia polecenie, aby zwolnić maszyny:

```
    azure vm deallocate <resource-group> <vm-name>
```

Przycisk **Zatrzymaj** w portalu Azure zwalnia również maszyn wirtualnych. Jednak po zamknięciu przez system operacyjny podczas zalogowany, zatrzymaniu maszyn wirtualnych, ale _nie_ alokację, więc możesz nadal jest rozliczana według.

**Usuwanie maszyny.** Jeśli usuniesz maszyny wirtualnych dysków twardych nie zostaną usunięte. Oznacza to, że można bezpiecznie usunąć maszyn wirtualnych bez utraty danych. Jednak możesz będą nadal obciążony miejsca do magazynowania. Aby usunąć wirtualny dysk twardy, usuń go z [magazynem obiektów blob][blob-storage].

Aby zapobiec przypadkowym usunięciom, użyj [Blokada zasobu] [ resource-lock] zablokować cały zasób grupy lub Zablokuj poszczególnych zasobów, takich jak maszyn wirtualnych. 

## <a name="security-considerations"></a>Zagadnienia dotyczące zabezpieczeń

Za pomocą [Centrum zabezpieczeń Azure] [ security-center] pozwala centralnej sprawdzić stan zabezpieczeń Azure zasobów. Centrum zabezpieczeń monitoruje potencjalne problemy, takie jak aktualizacje, ochrony przed złośliwym oprogramowaniem, system, a także kompleksowy obraz kondycji zabezpieczeń wdrożenia. 

- Centrum zabezpieczeń jest skonfigurowany na subskrypcję Azure. Włącz zbieranie danych zabezpieczeń, zgodnie z opisem w [Korzystanie z Centrum zabezpieczeń].

- Po włączeniu zbierania danych Centrum zabezpieczeń automatycznie skanuje wszelkie maszyny wirtualne utworzone na podstawie tej subskrypcji.

**Zarządzanie poprawkami.** Jeśli funkcja jest włączona, Centrum zabezpieczeń sprawdza, czy zabezpieczeń i krytyczne aktualizacje nie są widoczne. [Ustawienia zasad grupy] za pomocą[ group-policy] na maszyn wirtualnych, aby włączyć Aktualizacje automatyczne systemu.

**Ochrony przed złośliwym oprogramowaniem.** Jeśli funkcja jest włączona, Centrum zabezpieczeń sprawdza, czy oprogramowanie ochrony przed złośliwym oprogramowaniem jest zainstalowany. Za pomocą Centrum zabezpieczeń instalować oprogramowania ochrony przed złośliwym oprogramowaniem z wewnątrz Azure portal.

[Kontrola dostępu oparta na rolach] za pomocą[ rbac] (RBAC) do kontrolowania dostępu do Azure zasoby, które mają być rozmieszczone. RBAC można przypisać role autoryzacji do członków zespołu DevOps. Na przykład roli czytnika można wyświetlić Azure zasoby, ale nie tworzenie, zarządzanie i usuwanie ich. Niektóre role są specyficzne dla określonego zasobu Azure typów. Na przykład Rola współautora maszyn wirtualnych można ponownie uruchomić lub cofnąć maszyny, zresetuj hasło administratora, tworzenie nowych maszyn wirtualnych itd. Inne [wbudowane role RBAC] [ rbac-roles] co może być przydatne dla tej architektury odwołanie dołączania [Użytkownika Labs DevTest] [ rbac-devtest] i [Współpracownik sieci][rbac-network]. Użytkownika można przypisać do wielu ról i można tworzyć niestandardowe role jeszcze bardziej precyzyjne uprawnień.

> [AZURE.NOTE] RBAC nie ogranicza akcje, które może wykonywać użytkownik zalogowany do maszyny. Te uprawnienia są uzależnione od typu konta na gościa systemu operacyjnego.   

Aby zresetować hasło administratora lokalnego, uruchom `vm reset-access` polecenie polecenie Azure.

```
    azure vm reset-access -u <user> -p <new-password> <resource-group> <vm-name>
```

Za pomocą [dzienników inspekcji] [ audit-logs] Aby wyświetlić inicjowania obsługi administracyjnej akcje i inne zdarzenia maszyn wirtualnych.

Należy rozważyć, czy [Szyfrowanie dysków Azure] [ disk-encryption] Jeśli musisz szyfrowanie dysków systemu operacyjnego i danych. 

## <a name="solution-deployment"></a>Wdrożenie rozwiązania

[Wdrożenie] [ github-folder] odwołania architektura demonstrujący tymi najlepszymi rozwiązaniami jest dostępny. Ta architektura odwołanie zawiera wirtualnej sieci (VNet), grupa zabezpieczeń sieci (NSG) i jednym komputerze wirtualnych (maszyn wirtualnych).

Istnieje kilka sposobów wdrażania tej architektury odwołania. Najprostszym sposobem jest wykonaj następujące czynności: 

1. Kliknij prawym przyciskiem myszy przycisk poniżej i wybierz pozycję "Otwórz łącze w nowej karcie" lub "Otwórz łącze w nowym oknie".  
[![Wdrażanie Azure](../articles/guidance/media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-single-vm%2Fazuredeploy.json)

2. Po otwarciu łącze w Azure portal, należy wprowadzić wartości dla niektórych ustawień: 
    - Nazwa **grupy zasobów** jest już zdefiniowana w pliku parametrów, więc wybierz polecenie **Utwórz nowy** i wprowadź `ra-single-vm-rg` w polu tekstowym.
    - Wybierz region w polu rozwijanym **lokalizacji** .
    - Nie należy edytować **Uri głównego szablonu** lub pól tekstowych **Uri głównego parametru** .
    - Wybierz **Typ systemu operacyjnego** z listy rozwijanej, **systemu windows**.
    - Przejrzyj warunki umowy, a następnie kliknij pole wyboru **zgodę na warunki umowy podanych powyżej** .
    - Kliknij przycisk **Kup** .

3. Poczekaj, aż wdrożenia do wykonania.

4. Pliki parametru zawierają stałe administratora nazwę użytkownika i hasło, a zdecydowanie zalecane jest, aby natychmiast zmienić oba. Polecenie maszyn wirtualnych, o nazwie `ra-single-vm0 `w Azure Portal. Następnie kliknij **Resetowanie hasła** w karta **pomocy technicznej + rozwiązywania problemów** . Zaznacz pole wyboru **Resetuj hasło** w polu listy rozwijanej **Tryb** , a następnie wybierz nową **nazwę użytkownika** i **hasło**. Kliknij przycisk **Aktualizuj** , aby nową nazwę użytkownika i hasło.

Uzyskać informacji na temat dodatkowe sposoby wdrażanie tej architektury odwołania, zobacz w pliku readme w [orientacji jedno-maszyn wirtualnych][github-folder]] Github folder. 

## <a name="customize-the-deployment"></a>Dostosowywanie rozmieszczenia

Jeśli chcesz zmienić wdrożenia zgodnie z potrzebami, postępuj zgodnie z instrukcjami [— plik readme][github-folder]. 

## <a name="next-steps"></a>Następne kroki

Aby [Umowa dotycząca poziomu usług dla maszyn wirtualnych] [ vm-sla] Aby zastosować, należy wdrożyć dwa lub więcej obiektów w zestawie dostępności. Aby uzyskać więcej informacji, zobacz [Uruchamianie wielu maszyn wirtualnych Azure][multi-vm].

<!-- links -->

[audit-logs]: https://azure.microsoft.com/en-us/blog/analyze-azure-audit-logs-in-powerbi-more/
[availability-set]: ../articles/virtual-machines/virtual-machines-windows-create-availability-set.md
[azure-cli]: ../articles/virtual-machines-command-line-tools.md
[azure-storage]: ../articles/storage/storage-introduction.md
[blob-snapshot]: ../articles/storage/storage-blob-snapshots.md
[blob-storage]: ../articles/storage/storage-introduction.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[cname-record]: https://en.wikipedia.org/wiki/CNAME_record
[data-disk]: ../articles/virtual-machines/virtual-machines-windows-about-disks-vhds.md
[disk-encryption]: ../articles/security/azure-security-disk-encryption.md
[enable-monitoring]: ../articles/monitoring-and-diagnostics/insights-how-to-use-diagnostics.md
[fqdn]: ../articles/virtual-machines/virtual-machines-windows-portal-create-fqdn.md
[github-folder]: http://github.com/mspnp/reference-architectures/tree/master/guidance-compute-single-vm
[group-policy]: https://technet.microsoft.com/en-us/library/dn595129.aspx
[log-collector]: https://azure.microsoft.com/en-us/blog/simplifying-virtual-machine-troubleshooting-using-azure-log-collector/
[manage-vm-availability]: ../articles/virtual-machines/virtual-machines-windows-manage-availability.md
[multi-vm]: ../articles/guidance/guidance-compute-multi-vm.md
[naming conventions]: ../articles/guidance/guidance-naming-conventions.md
[nsg]: ../articles/virtual-network/virtual-networks-nsg.md
[nsg-default-rules]: ../articles/virtual-network/virtual-networks-nsg.md#default-rules
[planned-maintenance]: ../articles/virtual-machines/virtual-machines-windows-planned-maintenance.md
[premium-storage]: ../articles/storage/storage-premium-storage.md
[rbac]: ../articles/active-directory/role-based-access-control-what-is.md
[rbac-roles]: ../articles/active-directory/role-based-access-built-in-roles.md
[rbac-devtest]: ../articles/active-directory/role-based-access-built-in-roles.md#devtest-lab-user
[rbac-network]: ../articles/active-directory/role-based-access-built-in-roles.md#network-contributor
[reboot-logs]: https://azure.microsoft.com/en-us/blog/viewing-vm-reboot-logs/
[resize-os-disk]: ../articles/virtual-machines/virtual-machines-windows-expand-os-disk.md
[Resize-VHD]: https://technet.microsoft.com/en-us/library/hh848535.aspx
[Resize virtual machines]: https://azure.microsoft.com/en-us/blog/resize-virtual-machines/
[resource-lock]: ../articles/resource-group-lock-resources.md
[resource-manager-overview]: ../articles/azure-resource-manager/resource-group-overview.md
[security-center]: https://azure.microsoft.com/en-us/services/security-center/
[select-vm-image]: ../articles/virtual-machines/virtual-machines-windows-cli-ps-findimage.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[static-ip]: ../articles/virtual-network/virtual-networks-reserved-public-ip.md
[storage-price]: https://azure.microsoft.com/pricing/details/storage/
[Korzystanie z Centrum zabezpieczeń]: ../articles/security-center/security-center-get-started.md#use-security-center
[virtual-machine-sizes]: ../articles/virtual-machines/virtual-machines-windows-sizes.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vm-disk-limits]: ../articles/azure-subscription-service-limits.md#virtual-machine-disk-limits
[vm-resize]: ../articles/virtual-machines/virtual-machines-linux-change-vm-size.md
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines/v1_0/
[0]: ./media/guidance-blueprints/compute-single-vm.png "Pojedynczy architektura maszyn wirtualnych systemu Windows Azure"
[readme]: https://github.com/mspnp/reference-architectures/blob/master/guidance-compute-single-vm
[blocks]: https://github.com/mspnp/template-building-blocks
