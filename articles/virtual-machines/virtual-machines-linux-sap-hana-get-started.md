<properties
   pageTitle="Przewodnik Szybki Start dla ręcznego instalowania SAP HANA na maszyny wirtualne Azure | Microsoft Azure"
   description="Przewodnik Szybki Start dla ręcznego instalowania SAP HANA na maszyny wirtualne Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="hermanndms"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/15/2016"
   ms.author="hermannd"/>

# <a name="quickstart-guide-for-manual-installation-of-single-instance-sap-hana-on-azure-vms"></a>Przewodnik Szybki Start dla ręcznego instalowania jednego wystąpienia HANA SAP na maszyny wirtualne Azure

## <a name="introduction"></a>Wprowadzenie

Ten przewodnik Szybki Start pomoże skonfigurować jednego wystąpienia SAP HANA prototypu pokaz na maszyny wirtualne Azure, Instalacja ręczna SAP NetWeaver 7.5 i SAP HANA SP12.
Przewodnik przyjęto założenie, że czytnik jest znajomość Azure IaaS, takimi jak wdrożyć maszyn wirtualnych lub wirtualnych sieci albo przez Azure portal lub programu Powershell i polecenie tym opcję za pomocą szablonów json. Ponadto oczekuje się zapoznać się z SAP HANA, SAP NetWeaver i jak ją zainstalować lokalnego czytelnika.

Przewiduje się, że czytnik jest świadomy ogólne dokumentacji SAP Azure wymienionych w sekcji Ogólne informacje na końcu tego artykułu.

Ze względu na to ograniczenie do systemów innych niż produkcji tego przewodnika nie będzie obejmować tematy, takie jak HA, utworzyć kopię zapasową, DR, wysoka wydajność i zagadnienia dotyczące zabezpieczeń specjalne.

Konfiguracja przykładowych została ukończona przy użyciu dwóch maszyn wirtualnych do wykonania instalacji rozproszonej SAP NetWeaver za pośrednictwem modelu Azure Menedżera zasobów, jak SAP — Linux-Azure jest obsługiwana tylko na na Menedżera zasobów Azure i nie modelu Klasyczny. Łącza do dalszych informacji dotyczących Menedżera zasobów Azure można znaleźć w sekcji Ogólne informacje na końcu tego artykułu.

Były one maszyny wirtualne dwóch test na potrzeby instalacji przykładowe:

* Hana-appsrv (typu DS3) do obsługi wystąpienie NW 7.5 ASCS + PAS
* Hana dbsrv (typu GS4) hosta HANA SP12
* oba maszyny wirtualne należały do jednej sieci wirtualnej Azure (azure-hana-test-vnet)
* System operacyjny w obu przypadkach została SLES 12 z dodatkiem SP1


Należy pamiętać, fakt, mimo że z lipca 2016 SAP HANA tylko w pełni jest obsługiwana przez OLAP (PC) systemami maszyn wirtualnych Azure typu GS5. Podczas testowania, gdzie jeden nie oczekuje oficjalne wsparcie SAP jest wyrażenie zgody na używanie mniejszą, takich jak np. GS4.
Co zawsze należy używać HANA SAP Azure jest Azure premium miejsca do magazynowania dla HANA pliki dziennika i danych — można znaleźć w sekcji "Konfigurowanie dysku" dalszych szczegółów. Dotyczące szczegółów, o których SAP produkty są obsługiwane na Azure Sprawdź sekcji Ogólne informacje na końcu tego artykułu.


Przewodnik opisano dwa różne sposoby, aby ręcznie zainstalować SAP HANA w maszyny wirtualne Azure:

* Instalowanie HANA SAP za pomocą Menedżera inicjowania obsługi administracyjnej oprogramowania SAP (SWPM) jako część instalacji rozproszonej NetWeaver w kroku "wystąpienie bazy danych"
* Instalowanie HANA SAP przy użyciu hdblcm narzędzie HANA cyklu życia menedżera, a następnie zainstaluj NetWeaver później

Użytkownik może również używać SWPM i zainstaluj wszystkie składniki (SAP HANA, serwer aplikacji SAP, ASCS wystąpienia, Graficznym SAP) w jeden pojedynczy maszyn wirtualnych. Ta opcja nie jest opisane w tym artykule, ale elementy, które mają być traktowane jako są takie same.

Przed rozpoczęciem instalacji sekcji po list kontrolnych poniżej o konfigurowaniu maszyny wirtualne Azure test należy rozumieć uniknąć kilku podstawowych błędów, występujące podczas korzystania z tylko domyślną konfigurację maszyn wirtualnych Azure.


## <a name="checklist-sap-hana-installation-via-sap-swpm"></a>Lista kontrolna instalacji SAP HANA za pośrednictwem SAP SWPM

Jest to prostej listy kontrolnej najważniejszych elementów związanych z Instalacja ręczna SAP HANA jednego wystąpienia dla pokaz lub prototypami pursposes za pośrednictwem SWPM SAP, wykonując rozłożone 7.5 NW SAP instalacji. Poszczególne elementy są wyjaśnione i wyświetlane w formularzu zrzuty ekranu bardziej szczegółowo w całym tego artykułu:

* Tworzenie Azure wirtualnej sieci, obejmujące maszyny wirtualne dwóch test 
* Wdrażanie dwóch Azure maszyny wirtualne z dodatkiem SP1 12 SLES OS za pomocą Menedżera zasobów Azure modelu 
* Dołączanie dwa dyski standardowego magazynu na serwerze aplikacji maszyn wirtualnych (na przykład 75GB i 500GB)
* Dołączanie czterech dysków na serwerze bazy danych HANA maszyn wirtualnych - 2 standardowego magazynu dysków podobnie jak na serwerze aplikacji maszyn wirtualnych + 2 premium dysków w magazynie (np. 2x512GB)
* w zależności od rozmiaru i/lub przepustowość wymagania dołączyć wiele dysków i tworzenie rozłożonych albo za pomocą lvm lub mdadm na poziomie systemu operacyjnego wewnątrz maszyn wirtualnych
* Tworzenie systemów plików XFS na dyskach załączonych / logiczne wielkości
* Instalowanie nowych systemów plik XFS na poziomie systemu operacyjnego. Zachowaj oprogramowania SAP i drugi przykład dla katalogu sapmnt i może kopie zapasowe za pomocą jednego systemu plików. Na SAP HANA DB server instalacji XFS systemy plików na dyskach miejsca do magazynowania premium jako /hana i /usr/sap, jest to wszystkie niezbędne w celu uniknięcia, że wypełnia plików główny, który nie jest zbyt duży na maszyny wirtualne Azure Linux
* Wprowadź adresy lokalny adres ip testu maszyny wirtualne w/itp/hosts
* Wprowadź parametr nofail w /etc/fstab
* Ustawianie parametrów jądra według Uwagi SAP HANA-SLES-12 (Zobacz szczegóły niżej w sekcji parametrów jądra)
* Dodawanie miejsca wymiany
* Jeśli - Zainstaluj pulpitu graficznych w teście maszyny wirtualne. W przeciwnym razie przeprowadzić instalację zdalnego sapinst
* Pobieranie oprogramowania SAP z witryny marketplace usługi SAP
* Instalowanie wystąpienia SAP ASCS na serwerze aplikacji maszyn wirtualnych
* udostępnianie katalogu sapmnt za pośrednictwem NFS między test maszyny wirtualne (serwer aplikacji maszyn wirtualnych jest serwer NFS)
* Instalowanie wystąpienie bazy danych, w tym HANA za pośrednictwem SWPM na serwerze bazy danych maszyn wirtualnych
* Instalowanie PAS na serwerze aplikacji maszyn wirtualnych
* Uruchom SAP MC i przykład połączenia za pomocą Graficznym SAP-HANA Studio 



## <a name="checklist-sap-hana-installation-via-hdblcm"></a>Lista kontrolna instalacji SAP HANA za pośrednictwem hdblcm

Jest to prostej listy kontrolnej najważniejszych elementów związanych z Instalacja ręczna SAP HANA jednego wystąpienia dla pokaz lub prototypami pursposes za pośrednictwem SWPM SAP, wykonując rozłożone 7.5 NW SAP instalacji. Poszczególne elementy są wyjaśnione i wyświetlane w formularzu zrzuty ekranu bardziej szczegółowo w całym tego artykułu:

* Tworzenie Azure wirtualnej sieci, obejmujące maszyny wirtualne dwóch test 
* Wdrażanie dwóch Azure maszyny wirtualne z dodatkiem SP1 12 SLES OS za pomocą Menedżera zasobów Azure modelu 
* Dołączanie dwa dyski standardowego magazynu na serwerze aplikacji maszyn wirtualnych (na przykład 75GB i 500GB)
* Dołączanie czterech dysków na serwerze bazy danych HANA maszyn wirtualnych - 2 standardowego magazynu podobnie jak na serwerze aplikacji maszyn wirtualnych + 2 premium dysków w magazynie (np. 2x512GB)
* w zależności od rozmiaru i/lub przepustowość wymagania dołączyć wiele dysków i tworzenie rozłożonych albo za pomocą lvm lub mdadm na poziomie systemu operacyjnego wewnątrz maszyn wirtualnych
* Tworzenie systemów plików XFS dołączonych dysków i logiczne wielkości
* Instalowanie nowych systemów plik XFS na poziomie systemu operacyjnego. Zachowaj oprogramowania SAP i drugi przykład dla katalogu sapmnt i może kopie zapasowe za pomocą jednego systemu plików. Na SAP HANA DB server instalacji XFS systemy plików na dyskach miejsca do magazynowania premium jako /hana i /usr/sap, jest to wszystkie niezbędne w celu uniknięcia, że wypełnia plików główny, który nie jest zbyt duży na maszyny wirtualne Azure Linux
* Wprowadź adresy lokalny adres ip testu maszyny wirtualne w/itp/hosts
* Wprowadź parametr nofail w /etc/fstab
* Ustawianie parametrów jądra według Uwagi SAP HANA-SLES-12 (Zobacz szczegóły niżej w sekcji parametrów jądra)
* Dodawanie miejsca wymiany
* Jeśli - Zainstaluj pulpitu graficznych w teście maszyny wirtualne. W przeciwnym razie przeprowadzić instalację zdalnego sapinst
* Pobieranie oprogramowania SAP z witryny marketplace usługi SAP
* Tworzenie grupy "sapsys" identyfikatorem grupy 1001 na maszyn wirtualnych serwera DB HANA
* Instalowanie SAP HANA na serwerze bazy danych maszyn wirtualnych przy użyciu hdblcm
* Instalowanie wystąpienia SAP ASCS na serwerze aplikacji maszyn wirtualnych
* udostępnianie katalogu sapmnt za pośrednictwem NFS między test maszyny wirtualne (serwer aplikacji maszyn wirtualnych jest serwer NFS)
* Instalowanie wystąpienie bazy danych, w tym HANA za pośrednictwem SWPM na serwerze bazy danych maszyn wirtualnych
* Instalowanie PAS na serwerze aplikacji maszyn wirtualnych
* Uruchom SAP MC i przykład połączenia za pomocą Graficznym SAP-HANA Studio 




## <a name="prepare-azure-vms-for-manual-installation-of-sap-hana"></a>Przygotowywanie maszyny wirtualne Azure ręcznej instalacji SAP HANA

Niniejszy rozdział dotyczące przygotowania maszyny wirtualne Azure do ręcznego instalowania SAP HANA składa się z pięciu sekcji, które obejmują następujące tematy:

* Konfiguracja dysku
* Parametry jądra
* FileSystems
* / itp/hosts
* / itp/fstab


### <a name="disk-setup"></a>Konfiguracja dysku

W systemie plików głównego w maszyny Linux Azure jest ograniczony rozmiar. W związku z tym jest to konieczne dołączyć dodatkowe miejsce na dysku do maszyny do uruchamiania SAP. W przypadku rozwiązania SAP server aplikacji używanych w środowisku wyłącznie prototypu pokaz maszyn wirtualnych jest poprawnie używać dysków Azure standardowego magazynu. Pliki danych i dziennika SAP HANA DB — dysków magazynu Azure Premium powinny być stosowane nawet w poziomej nie produkcji.

Zobacz niektóre informacje dotyczące dołączania dysków do maszyny Linux [tutaj](virtual-machines-linux-add-disk.md)

Jeśli chodzi o buforowania dysku Azure — jeden, należy użyć "Brak" w przypadku dysków, które będą używane w celu przechowywania dzienników transakcji HANA. HANA pliki danych, które jest gotowy do używania w artykule pamięci podręcznej. Tak jak HANA bazy danych w pamięci zależy od ogólnej deseniu zastosowania ilość odczytu pamięci podręcznej na poziomie Azure dysku zwiększa wydajność (np. uruchamianie HANA i odczytywania danych z dysku do pamięci).

Szczegółowe informacje na temat przechowywania Premium Azure [tutaj](../storage/storage-premium-storage.md)

[W tym miejscu](https://github.com/Azure/azure-quickstart-templates) możesz znaleźć przykładowe szablony json, aby utworzyć maszyny wirtualne.
"101 maszyn wirtualnych — proste — linux" pokazano, jak wygląda w tym sekcji miejsca do magazynowania, co spowoduje dodanie dyskiem danych 100GB szablonu podstawowego.

[Ten artykuł](virtual-machines-linux-sap-on-suse-quickstart.md) zawiera niektóre dowiedzieć się, jak znaleźć obraz SUSE przy użyciu programu Powershell lub interfejsu wiersza polecenia i wysoki priorytet, aby dołączyć dysku za pośrednictwem UUID.


W zależności od rozmiaru wymagania systemowe i przepustowość może być konieczne dołączyć wiele dysków zamiast jednej i nowszy na tworzenie zestawu na tych dyskach na poziomie systemu operacyjnego paskowego. Oto dwa aspekty Dlaczego jedną Tworzenie zestawu na wielu dyskach Azure paskowego:

* zwiększenie produktywności
* potrzebujesz pojedynczy plików > 1TB bieżący limit rozmiaru plików dla dysku Azure jest 1TB (stan 2016 lipiec)


Więcej informacji na temat dwa główne narzędzia do konfigurowania rozkładanie można znaleźć tutaj:

[Artykuł informacje o konfigurowaniu raid oprogramowania Linux na maszyn wirtualnych Azure za pomocą mdadm](virtual-machines-linux-configure-raid.md)

[Artykuł informacje o konfigurowaniu Menedżer głośności logicznych na maszyny Azure Linux](virtual-machines-linux-configure-lvm.md)



![](./media/virtual-machines-linux-sap-hana-get-started/image003.jpg)

W teście dwa dyski Azure standardowego magazynu środowiska zostały dołączone do serwera aplikacji SAP maszyn wirtualnych. Zostało ono użyte do przechowywania wszystkich oprogramowanie SAP instalacji (np. NetWeaver 7.5, Graficznym SAP SAP HANA... ), a drugi do za mało miejsca na inną mogą być wymagane (np. Aby utworzyć kopię zapasową, testowymi danymi) oraz katalogu sapmnt (np. profile SAP) do udostępnienia wśród wszystkich maszyny wirtualne należące do samej pozioma SAP.

![](./media/virtual-machines-linux-sap-hana-get-started/image004.jpg)

W przeciwieństwie do serwera aplikacji maszyn wirtualnych czterech dysków zostały dołączone na serwerze SAP HANA maszyn wirtualnych. Na przykład przed dwa dyski były używane do zachowania oprogramowania SAP (jeden może także udostępnić dysku oprogramowania SAP za pośrednictwem NFS) i masz za mało miejsca na przykład dla kopii zapasowej. Dodatkowe dwa dyski były dysków magazynu Azure Premium zachować SAP HANA danych i plików dziennika, a także katalogu /usr/sap.


### <a name="kernel-parameters"></a>Parametry jądra


SAP HANA wymaga określonego ustawienia jądra Linux, które nie są częścią standardowej Azure galerii obrazów i mieć można ustawić ręcznie. Ma określoną notatkę SAP, na którym opisano ustawienia. 


Uwaga systemu SAP SAP HANA DB: Zalecane ustawienia systemu operacyjnego SLES 12-SLES dla SAP aplikacji 12: [2205917 Uwaga systemu SAP](https://launchpad.support.sap.com/#/notes/2205917)

Jeden temat dodatkowe reagrding pamięci podręcznej strony związane z uruchamianiem SAP HANA na SLES można znaleźć [tutaj](https://www.suse.com/documentation/sles_for_sap/singlehtml/sles_for_sap_guide/sles_for_sap_guide.html#sec.s4s.configure.page-cache) w rozdział 6.1 jądra: Limit pamięci podręcznej stron

Istnieje także SAP notatki dotyczące limit pamięci podręcznej stron [1557506 Uwaga systemu SAP](https://service.sap.com/sap/support/notes/1557506)

SLES 12 ma nowego narzędzia, która zastępuje stary narzędzie sapconf. To "dostosowanych adm" i specjalnego profilu SAP HANA jest dostępna. Jeden można znaleźć więcej informacji na temat tego narzędzia po dwóch poniższych łączy.

SLES dokumentację dotyczącą profilu dostosowanych adm sap — hana można znaleźć [w tym miejscu](https://www.suse.com/documentation/sles-for-sap-12/book_s4s/data/sec_s4s_configure_sapconf.html)

SLES dokumentację dotyczącą dostosowanych adm profilu hana sap — rozdziału 6.2 Dostosowywanie systemów SAP obciążenie pracą z dostosowanych adm — można znaleźć [w tym miejscu](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/book_s4s/book_s4s.pdf)


![](./media/virtual-machines-linux-sap-hana-get-started/image005.jpg)

W tym miejscu jedną Zobacz, jak "dostosowanych adm" zmieniły się transparent_hugepage, a także wartości numa_balancing zgodnie z ustawieniami wymagane SAP HANA.


Aby wprowadzić ustawienia jądra SAP HANA trwały będzie miał grub2 w systemie SLES 12. Dodatkowe informacje o grub2 można znaleźć [w tym miejscu](https://www.suse.com/documentation/sled-12/book_sle_admin/data/sec_grub2_file_structure.html)


![](./media/virtual-machines-linux-sap-hana-get-started/image006.jpg)

Na tym zrzucie ekranu pokazano, jak ustawienia jądra zostały zmienione w pliku konfiguracji i następnie "skompilowany" za pośrednictwem grub2 mkconfig


![](./media/virtual-machines-linux-sap-hana-get-started/image007.jpg)

Innym rozwiązaniem jest zmiana ustawień za pośrednictwem Yast i uruchamiania moduł ładujący jądra parametru.


### <a name="filesystems"></a>FileSystems 

![](./media/virtual-machines-linux-sap-hana-get-started/image008.jpg)

W tym miejscu jedną Zobacz systemy plików, które zostały utworzone na serwerze aplikacji SAP maszyn wirtualnych na bieżąco dwa dyski Azure standardowy masowej. Obie filesystems typu XFS i zainstalować /sapdata i /sapsoftware.

Nie jest konieczne robić w ten sposób. Istnieją różne opcje jak struktury miejsca na dysku.
Najważniejszym aspektem jest unikanie, że plików głównego zostanie uruchomiona brakować miejsca. 


![](./media/virtual-machines-linux-sap-hana-get-started/image009.jpg)

Dotyczące maszyn wirtualnych DB HANA SAP ważne jest, aby pamiętać, że podczas instalacji bazy danych za pośrednictwem sapinst (swpm) i tylko za pomocą opcji instalacji prostej "typowe" zainstaluje rzeczy domyślnie w obszarze /hana i /usr/sap. Ustawieniem domyślnym dla kopii zapasowej dziennika SAP HANA np znajduje się w obszarze /usr/sap.
Jak przed klawisz, aby uniknąć plików głównego uruchamia brakować miejsca. Dlatego jedną należy upewnić się, że przed zainstalowaniem SAP HANA za pośrednictwem swpm jest za mało miejsca w obszarze /hana i /usr/sap.

[W tym artykule](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4c/24d332a37b4a3caad3e634f9900a45/frameset.htm) z SAP opisuje układ standardowy plików SAP HANA 


![](./media/virtual-machines-linux-sap-hana-get-started/image010.jpg)

Podczas instalowania SAP NetWeaver na standardowy Galeria SLES 12 Azure będzie komunikat, że jest bez spacji wymiany. Aby usunąć ten komunikat jedną może np ręcznie dodać plik wymiany zgodnie z opisem w tym dokumencie za pomocą dd, mkswap i swapon. Po prostu wyszukać "Dodawanie Zamień plik ręcznie" w [tym artykule](https://www.suse.com/documentation/sled-12/book_sle_deployment/data/sec_yast2_i_y2_part_expert.html)

Innym rozwiązaniem jest skonfigurowanie obszar wymiany za pośrednictwem agenta maszyn wirtualnych Linux. Więcej informacji można znaleźć [w tym miejscu](virtual-machines-linux-agent-user-guide.md)


### <a name="etchosts"></a>/ itp/hosts

![](./media/virtual-machines-linux-sap-hana-get-started/image011.jpg)

Innym ważnym aspektem przed rozpoczęciem instalacji SAP jest umieszczenie nazwy hostów i adresy IP maszyny wirtualne SAP w pliku Hosts. Jeden należy wdrożyć VMs SAP w jednym Azure wirtualną sieć, a następnie użyj adresów IP.

### <a name="etcfstab"></a>/ itp/fstab

![](./media/virtual-machines-linux-sap-hana-get-started/image000c.jpg)

Podczas testowania fazy go włączona być dobrym pomysłem jest dodanie parametru nofail do fstab. Jeśli wystąpią problemy z dysków maszyn wirtualnych będzie nadal pojawiły się i nie są wysuwane w procesie uruchamiania. Ale będzie miał wymagające uwagi tak jak w tym przypadku dodatkowego miejsca na dysku może nie być dostępna i procesów może zapełnić plików głównego. W przypadku /hana może brakować SAP HANA nie rozpocząć, jeśli w ogóle.


## <a name="install-graphical-gnome-desktop-on-sles-12"></a>Instalowanie graficzne pulpitu Gnome na SLES 12

Niniejszy rozdział składa się z dwóch secitions, które obejmują następujące tematy:

* Instalacja pulpitu Gnome i xrdp na SLES 12
* Uruchamianie za pomocą programu Firefox na SLES 12 MC SAP opartego na języku Java

Jeden można także stosować inne możliwości, takich jak Xterminal, VNC, ale od 2016 wrz w tym artykule opisano tylko xrdp.

### <a name="installation-of-gnome-desktop-and-xrdp-on-sles-12"></a>Instalacja pulpitu Gnome i xrdp na SLES 12

Zwłaszcza dla osób, które mają tło Microsoft Windows i chcesz umożliwia graficzne pulpitu bezpośrednio z poziomu maszyny wirtualne Linux SAP w celu uruchomienia programu Firefox, Sapinst, SAP Graficznym, SAP MC lub HANA Studio i może połączyć się z maszyn wirtualnych za pośrednictwem RDP z systemu Microsoft Windows istnieje prosty sposób, aby osiągnąć ten cel. Gdy to mogą nie być właściwe np produkcji serwera bazy danych jest prawidłowy w środowisku wyłącznie prototypu pokaz. Poniżej przedstawiono kroki, aby zainstalować pulpitu Gnome na maszyn wirtualnych Azure SLES 12:

Zainstaluj pulpit gnome przez następujące polecenie (np. w oknie Kit):

   zypper we wzorcu -t gnome basic

następnie zainstalować xrdp do przyłączenia się do maszyn wirtualnych za pośrednictwem RDP:

   zypper w xrdp

Edytuj /etc/sysconfig/windowmanager i Ustaw domyślnego menedżera systemu windows na Gnome:

   DEFAULT_WM = "gnome"

Uruchamianie chkconfig, aby upewnić się, że xrdp rozpoczyna się automatycznie po ponownym uruchomieniu: 

  chkconfig-xrdp poziomu 3 na

w przypadku, gdy powinny być problemu z połączeniem RDP spróbuj ponownie uruchomić (być może poza oknem Kit):

  Uruchom ponownie /etc/xrdp/xrdp.sh

w przypadku ponownego xrdp jako wymienionych powyżej nie działają wyboru, jeśli znajduje się plik .pid i je usunąć:

  Sprawdzanie /var/run i odszukaj xrdp.pid   
  Usuń go, a następnie spróbuj ponownie po ponownym uruchomieniu komputera



### <a name="sap-mc"></a>SAP MC


Aby rozpocząć SAP MC opartego na języku Java graficzne poza Firefox działa maszyn wirtualnych Azure SLES 12 po zainstalowaniu Gnome puplitu zgodnie z opisem w poprzedniej sekcji, jedną wystąpi błąd z powodu braku Java przeglądarki.

Adres URL, aby rozpocząć SAP MC jest <server>: 5 < instance_number > 13

Więcej informacji można znaleźć [w tym miejscu](https://help.sap.com/saphelp_nwce10/helpdata/en/48/6b7c6178dc4f93e10000000a42189d/frameset.htm)


![](./media/virtual-machines-linux-sap-hana-get-started/image013.jpg)

Na powyższym zrzut ekranu jedną można zobaczyć, jak komunikat o błędzie może wyglądać po dodatek Java przeglądarki nie jest widoczny. 

![](./media/virtual-machines-linux-sap-hana-get-started/image014.jpg)

Jedną możliwości rozwiązania tego problemu jest po prostu Zainstaluj wtyczkę Brak za pośrednictwem Yast.

![](./media/virtual-machines-linux-sap-hana-get-started/image015.jpg)

Powtarzanie adres URL SAP MC powoduje wyświetlenie nieco czasu na okno dialogowe pierwszej prośbę o aktywowanie wtyczkę.


Jeden dodatkowe problem, który może być wyświetlana jest komunikat o błędzie dotyczące brakującego pliku: javafx.properties jest najprawdopodobniej związany z instalacją Java 1.8, który jest wymagane na potrzeby 7.4 Graficznym SAP

Wersja IBM Java, widoczne za pośrednictwem Yast nie zawiera ten plik. Rozwiązanie jest pobieranie Java z programu Oracle.
Artykuł, który zawiera informacje o tym zagadnieniu można znaleźć [w tym miejscu](https://scn.sap.com/thread/3908306)



## <a name="manual-sap-hana-installation-via-swpm-as-part-of-a-netweaver-75-installation"></a>SAP HANA Instalacja ręczna za pośrednictwem SWPM w ramach instalacji NetWeaver 7.5


Poniższa lista zrzuty ekranu przedstawiono podstawowe etapy instalowania SAP NetWeaver 7.5 i SP12 HANA SAP za pośrednictwem SWPM (sapinst). W ramach NW 7.5 instalacja SWPM udostępnia funkcje również zainstalować HANA bazy danych jako jedno wystąpienie.


![](./media/virtual-machines-linux-sap-hana-get-started/image012.jpg)

W teście przykładowe środowiska tylko jeden serwer aplikacji ABAP został zainstalowany. Opcja "Rozproszony System" został użyty do zainstalowania wystąpienie ASCS oraz wystąpienie serwera podstawowego aplikacji w jednym maszyn wirtualnych Azure i SAP HANA jako systemu bazy danych w innej maszyn wirtualnych Azure.


![](./media/virtual-machines-linux-sap-hana-get-started/image016.jpg)

Po wystąpieniu ASCS jest zainstalowana na serwerze aplikacji maszyn wirtualnych i jest równa "zielony" w SAP MC katalog sapmnt, który zawiera przykład katalogu profilu SAP musi zostać udostępniona z serwerem SAP HANA DB maszyn wirtualnych.
Kroku instalacji DB musi mieć dostęp do tych informacji. Najlepszym sposobem jest użycie NFS, która może być skonfigurowana przy użyciu Yast.


![](./media/virtual-machines-linux-sap-hana-get-started/image017b.jpg)

W aplikacji można udostępnić serwera maszyn wirtualnych katalogu sapmnt za pośrednictwem NFS za pomocą opcji "rw" oraz "no_root_squash". Domyślna to "ro" i "root_squash", co może powodować problemy z podczas instalowania wystąpienie bazy danych.


![](./media/virtual-machines-linux-sap-hana-get-started/image018b.jpg)

Na serwerze bazy danych HANA SAP maszyn wirtualnych sapmnt udziału z serwera aplikacji, które maszyn wirtualnych musi być skonfigurowany za pomocą "Klient NFS" (na przykład za pomocą Yast)


![](./media/virtual-machines-linux-sap-hana-get-started/image019.jpg)

Następnie będzie miał do logowania na serwerze bazy danych HANA SAP maszyn wirtualnych i rozpocząć SWPM do wykonywania następnego kroku rozłożone instalacja NW 7.5 — "Wystąpienie bazy danych".


![](./media/virtual-machines-linux-sap-hana-get-started/image035b.jpg)

Związany z instalacją SAP HANA nie ma w rzeczywistości zbyt dużo wprowadzanie po wybraniu opcji instalacji "standardowej". Oprócz ścieżkę do multimediów installatiom jedną musi wprowadzić DB SID, nazwa hosta, numer i hasło administratora dotyczącego bazy danych.

 
![](./media/virtual-machines-linux-sap-hana-get-started/image036b.jpg)

Następnym krokiem jest wprowadzenie hasła dla schematu DBACOCKPIT.



![](./media/virtual-machines-linux-sap-hana-get-started/image037b.jpg)

Skąd pytanie o podanie hasła schematu SAPABAP1.


![](./media/virtual-machines-linux-sap-hana-get-started/image023.jpg)

Na końcu powinny być tylko zielony zaznaczeń przed każdym etapie procesu instalacji bazy danych, a następnie w oknie komunikatu mały, który pojawia się powinna być widoczna informacja "wykonanie... Wystąpienie bazy danych została ukończona".

 
![](./media/virtual-machines-linux-sap-hana-get-started/image024.jpg)

Po pomyślnym zakończeniu instalacji SAP MC należy również wyświetlić wystąpienia bazy danych jako "zielony" i Pełna lista procesów SAP HANA (hdbindexserver, hdbcompileserver)


![](./media/virtual-machines-linux-sap-hana-get-started/image025.jpg)

Na tym zrzucie ekranu przedstawiono części struktura pliku w obszarze /hana/shared, które SWPM utworzone podczas instalacji HANA. Wystąpił nie opcję, aby określić inną ścieżkę. Dlatego jest tak ważny do zainstalowania dodatkowego miejsca na dysku w obszarze /hana przed instalacją SAP HANA za pośrednictwem SWPM, aby uniknąć, że system plików głównego zostanie uruchomiona brakować miejsca.


![](./media/virtual-machines-linux-sap-hana-get-started/image026.jpg)

Jedną tutaj zobaczyć te same kroki, zgodnie z opisem przed katalogu /usr/sap.


![](./media/virtual-machines-linux-sap-hana-get-started/image027b.jpg)

Ostatnim krokiem instalacji rozproszonej ABAP jest "Podstawowy serwer aplikację"


![](./media/virtual-machines-linux-sap-hana-get-started/image028b.jpg)

Po zainstalowaniu masz PAS, a także Graficznym SAP jedną sprawdzić za pośrednictwem transakcji "dbacockpit", że wszystko wygląda bezpośrednio z instalacją SAP HANA.

 
![](./media/virtual-machines-linux-sap-hana-get-started/image038b.jpg)

Ostatnio pierwszy krok można zainstalować SAP HANA Studio na serwerze aplikacji SAP maszyn wirtualnych i połączyć się z wystąpieniem SAP HANA na serwerze bazy danych maszyn wirtualnych.




## <a name="manual-sap-hana-installation-via-hana-life-cycle-manager-tool-hdblcm"></a>Instalacja ręczna SAP HANA za pośrednictwem HANA cyklu Menedżer narzędzie hdblcm


Oprócz instalowania SAP HANA w ramach instalacji rozproszonej za pośrednictwem SWPM jest również possibe najpierw zainstalować autonomiczny HANA przy użyciu hdblcm i zainstaluj np SAP NetWeaver 7.5 później. Na liście zrzuty ekranu poniżej pokazano, jak to działa.

Oto trzy źródeł informacji na temat narzędzia hdblcm HANA:

[Wybieranie HDBLCM HANA poprawne SAP zadania](https://help.sap.com/saphelp_hanaplatform/helpdata/en/68/5cff570bb745d48c0ab6d50123ca60/content.htm)

[Narzędzia do zarządzania cyklu życia HANA SAP](http://saphanatutorial.com/sap-hana-lifecycle-management-tools/)

[Przewodnik aktualizacji i instalacji serwera HANA SAP](http://help.sap.com/hana/SAP_HANA_Server_Installation_Guide_en.pdf)



![](./media/virtual-machines-linux-sap-hana-get-started/image030.jpg)

Aby uniknąć pracy do problemów później z domyślnych ustawień identyfikator grupy dla \<HANA SID\>adm użytkownika (utworzone za pomocą narzędzia hdblcm) jedną należy zdefiniować nową grupę o nazwie "sapsys" identyfikatorem grupy 1001 przed zainstalowaniem HANA SAP przy użyciu hdblcm.




![](./media/virtual-machines-linux-sap-hana-get-started/image031.jpg)

Uruchamianie hdblcm pierwszym razem, gdy będzie menu start prosty miejsce, w którym będzie miał wybierz element, 1 "zainstalować nowy System"



![](./media/virtual-machines-linux-sap-hana-get-started/image032.jpg)

Na tym zrzucie ekranu jedną znajdują się wszystkie opcje klucza, które zostały wprowadzone przed. Ważne — katalogów nazwany HANA ilości danych i dziennika, a także ścieżka instalacji (/ hana/udostępniony w tym przykładzie), a/usr/sap nie powinny być częścią plików głównego, ale należeć do dyski Azure danych, które zostały dołączone do maszyn wirtualnych, zgodnie z opisem w sekcji Ustawienia maszyn wirtualnych Azure. To pozwoli uniknąć ryzyka, że plików katalogu głównego może brakować miejsca.
Jeden można też wyświetlić użytkownik Administrator HANA identyfikator użytkownika 1005 i stanowi część grupy sapsys (identyfikator 1001), które zostało zdefiniowane przed instalacją.



![](./media/virtual-machines-linux-sap-hana-get-started/image033.jpg)

Jeden sprawdzanie HANA \<HANA SID\>szczegóły użytkownika adm (azdadm w tym przykładzie) w/itp/haseł



![](./media/virtual-machines-linux-sap-hana-get-started/image034.jpg)

Po zainstalowaniu HANA SAP przy użyciu hdblcm widać w SAP HANA Studio. Schemat SAPABAP1, który zawiera przykład wszystkich SAP NetWeaver tabel nie jest jeszcze dostępne.



![](./media/virtual-machines-linux-sap-hana-get-started/image035b.jpg)

Po zainstalowaniu systemu SAP HANA jedną Zainstaluj SAP NetWeaver znajdujący się na nim. W tym przykładzie, które zostało wykonane za pomocą "instalacji rozproszonej" za pośrednictwem SWPM zgodnie z opisem w odpowiedniej sekcji powyżej.
Podczas instalowania wystąpienie bazy danych za pośrednictwem SWPM jedną tylko musi wprowadzić te same dane jako przed z hdblcm (np. hostname, HANA SID numer wystąpienia). SWPM zostanie następnie użyj istniejącej instalacji HANA i dodać schematów dodatkowych.



![](./media/virtual-machines-linux-sap-hana-get-started/image036b.jpg)

Jest to obraz kroku instalacji SWPM miejsce, w którym będzie miał do wprowadzania danych dotyczących schematu DBACOCKPIT.


![](./media/virtual-machines-linux-sap-hana-get-started/image037b.jpg)

Następnie SWPM oczekuje wprowadzanie danych informacje o schemacie SAPABAP1.


![](./media/virtual-machines-linux-sap-hana-get-started/image038b.jpg)

Po zakończeniu instalacji wystąpienia bazy danych SWPM jedną Zobacz Schemat SAPABAP1 w HANA Studio.



![](./media/virtual-machines-linux-sap-hana-get-started/image039b.jpg)

A na koniec po zakończeniu instalacji serwera aplikacji SAP i Graficznym SAP jedną powinno być możliwe Sprawdź wystąpienie HANA DB transakcji "dbacockpit".




## <a name="general-information-related-to-sap-azure-certifications-running-sap-hana-on-azure-and-sap-software-download"></a>Informacje ogólne dotyczące certyfikaty SAP Azure uruchomionych SAP HANA Azure i SAP w celu pobierania oprogramowania

* Ogólne dokument SAP Azure o uruchamianiu SAP Azure z systemu operacyjnego Windows w trybie klasycznym: [Za pomocą systemu SAP w przypadku maszyn wirtualnych systemu Windows platformy Azure](virtual-machines-windows-classic-sap-get-started.md)

* informacje o istniejących szablonów SAP w celu użycia przez klientów: [Azure szablony Szybki Start dla SAP](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/)

* Ogólne dokument SAP Azure o uruchamianiu SAP Azure z systemem operacyjnym Linux w modelu Menedżera zasobów Azure: [Za pomocą SAP na Linux wirtualnych maszyn](virtual-machines-linux-sap-get-started.md)

* certyfikowanego katalog sprzętu SAP HANA, który zawiera listę typów Azure maszyn wirtualnych, które są obsługiwane w przypadku produkcji: [Certyfikowane HANA SAP® sprzętu katalogu](https://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html)

* informacji na temat rozmiarów maszyn wirtualnych w szczególności dla obciążenia Linux: [rozmiarów maszyn wirtualnych platformy Azure](virtual-machines-linux-sizes.md)

* Uwaga systemu SAP, w której znajdują się wszystkie obsługiwane produkty SAP Azure i obsługiwane typy Azure maszyn wirtualnych systemu SAP: [1928533 Uwaga systemu SAP](https://launchpad.support.sap.com/#/notes/1928533/E)

* SAP — Uwaga dotycząca SAP "rozszerzony monitorowania" z maszyny wirtualne Linux Azure: [2191498 Uwaga systemu SAP](https://launchpad.support.sap.com/#/notes/2191498/E)

* HANA SAP oferuje na Azure "Dużych wystąpienia". Należy pamiętać, że nie jest to dotyczących uruchamiania SAP HANA na maszyny wirtualne Azure, ale we wdrożeniu hybrydowym środowiska miejsce, w którym serwery aplikacji SAP uruchamiane w maszyny wirtualne Azure, ale SAP HANA działa na serwerach zera: [2316233 Uwaga systemu SAP](https://launchpad.support.sap.com/#/notes/2316233/E)

* Uwaga systemu SAP informacje o SAPOSCOL w systemie Linux: [1102124 Uwaga systemu SAP](https://launchpad.support.sap.com/#/notes/1102124/E)

* Kluczowe monitorowania metryki SAP w programie Microsoft Azure: [Uwaga systemu SAP 2178632](https://launchpad.support.sap.com/#/notes/2178632/E)

* Informacje dotyczące Menedżera zasobów Azure: [Omówienie Menedżera zasobów Azure](../azure-resource-manager/resource-group-overview.md)

* Informacje dotyczące wdrażania Linux maszyny wirtualne za pomocą szablonów: [rozmieszczanie i zarządzanie nimi maszyn wirtualnych przy użyciu szablonów Menedżera zasobów Azure i polecenie Azure](virtual-machines-linux-cli-deploy-templates.md)

* Porównanie wdrożenia modeli między Menedżer zasobów Azure i klasycznym: [Menedżera zasobów Azure a klasyczny wdrażania: Opis modeli wdrażania i stan zasobów](../resource-manager-deployment-model.md)

* Pobierz NetWeaver 7.5 dla Linux-HANA z witryny Marketplace usługi SAP:![](./media/virtual-machines-linux-sap-hana-get-started/image001.jpg)

* Pobierz HANA SP12 platformy Edition z witryny Marketplace usługi SAP:![](./media/virtual-machines-linux-sap-hana-get-started/image002.jpg)

