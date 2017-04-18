<properties
    pageTitle="Zatwierdzone podziału Linux | Microsoft Azure"
    description="Informacje na temat Linux na zatwierdzone Azure dystrybucji, między innymi wskazówki dla Ubuntu, OpenLogic, Oracle i SUSE."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management,azure-resource-manager"
    />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="szark"/>



#<a name="linux-on-azure-endorsed-distributions"></a>Linux na zatwierdzone Azure dystrybucji

> [AZURE.NOTE] Jeśli masz kilka chwil Pomóż nam ulepszać dokumentacji maszyn wirtualnych Linux Azure, nie podejmując tę [ankietę szybkie](https://aka.ms/linuxdocsurvey) środowiska usługi. Co odpowiedzi pomagają nam pomocy, których wykonanie pracy.

Obrazy Linux w galerii Azure lub Marketplace są udostępniane przez liczbę partnerów i pracujemy obecnie z różnych społeczności Linux, aby dodać jeszcze więcej podtypy do listy dystrybucyjnej zatwierdzone. W międzyczasie dla przydziałów, które nie są dostępne z galerii można zawsze Przesuń i-właścicielem-Linux zgodnie z wytycznymi zawartymi na [tej stronie](virtual-machines-linux-classic-create-upload-vhd.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="supported-distributions--versions"></a>Obsługiwane dystrybucji i wersji ##

W poniższej tabeli wymieniono dystrybucji Linux i wersje, które są obsługiwane w Azure. Również zajrzyj do [pomocy technicznej dotyczącej Linux obrazów w programie Microsoft Azure](https://support.microsoft.com/en-us/kb/2941892) uzyskać bardziej szczegółowe informacje.

Sterowniki Linux Integration Services (LIS) dla funkcji Hyper-V i Azure to moduły jądra, które Microsoft składa się bezpośrednio do nadrzędnego jądra Linux.  Sterowniki LIS albo wbudowanych w jądra rozkład domyślnie lub starszy RHEL-CentOS-based dystrybucji dostępnych do pobrania osobno [poniżej](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409).  Można znaleźć [w tym artykule](virtual-machines-linux-create-upload-generic.md#linux-kernel-requirements) , aby uzyskać więcej informacji na temat sterowników LIS.

Azure Linux Agent jest już wstępnie zainstalowany na zdjęcia z galerii Azure i jest zazwyczaj dostępny z repozytorium pakietu dystrybucji.  Kod źródłowy można znaleźć w [GitHub](https://github.com/azure/walinuxagent).

ROZKŁAD|Wersja|Sterowniki|Agent
---|---|---|---
CentOS przez OpenLogic | CentOS 6.3 + 7.0 + | CentOS 6.3: [Pobieranie LIS](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409)<p>CentOS 6.4 +: W jądra | Pakiet: W [OpenLogic repo](http://olcentgbl.trafficmanager.net/openlogic/6/openlogic/x86_64/RPMS/) w obszarze "WALinuxAgent" <br/>Kod źródłowy: [GitHub](https://github.com/Azure/WALinuxAgent)
[CoreOS](https://coreos.com/docs/running-coreos/cloud-providers/azure/) | 494.4.0+ | Jądra | Kod źródłowy: [GitHub](https://github.com/coreos/coreos-overlay/tree/master/app-emulation/wa-linux-agent)
Debian | Debian 7,9 +, 8.2 + | Jądra | Pakiet: W repo w obszarze "waagent" <br/>Kod źródłowy: [GitHub](https://github.com/Azure/WALinuxAgent)
Oracle Linux | 6.4 +, 7.0 + | Jądra | Pakiet: W repo w obszarze "WALinuxAgent" <br/>Kod źródłowy: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998)
Czerwone funkcję Enterprise Linux | RHEL 6,7 + 7.1 + | Jądra|Pakiet: W repo w obszarze "WALinuxAgent" <br/>Kod źródłowy: [GitHub](https://github.com/Azure/WALinuxAgent)
SUSE Linux Enterprise | SLES 11 SP4, SLES 12 z dodatkiem SP1 + i <p> SLES SAP 11 z dodatkiem SP3 + systemu | Jądra | Pakiet: W repo [Chmury: narzędzia](https://build.opensuse.org/project/show/Cloud:Tools) w obszarze "python azure agenta" <br/>Kod źródłowy: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998)
openSUSE | openSUSE 13.2 + | Jądra | Pakiet: W repo [Chmury: narzędzia](https://build.opensuse.org/project/show/Cloud:Tools) w obszarze "python azure agenta" <br/>Kod źródłowy: [GitHub](https://github.com/Azure/WALinuxAgent)
Ubuntu|Ubuntu 12.04, 14.04, 16.04, 16.10 | Jądra | Pakiet: W repo w obszarze "walinuxagent" <br/>Kod źródłowy: [GitHub](https://github.com/Azure/WALinuxAgent)


## <a name="partners"></a>Partnerzy

### <a name="openlogic"></a>OpenLogic
[http://www.openlogic.com/Azure](http://www.openlogic.com/azure)

OpenLogic jest wiodącym dostawcą rozwiązań Otwórz źródło enterprise chmury i centrum danych. OpenLogic pomaga setki prowadzące przedsiębiorstwa w szerokim zakresie działalności do bezpieczne uzyskiwanie pomocy technicznej i kontroli oprogramowania Otwórz źródło. OpenLogic oferuje pomoc techniczna ocen komercyjnych i odszkodowań 600 pakietów Otwórz źródło przez społeczność eksperta OpenLogic, łącznie z poziomu obsługę CentOS w przedsiębiorstwie, a także jako partnerzy uruchamianie umożliwiające obrazów opartych na CentOS Azure.

### <a name="coreos"></a>CoreOS
[https://CoreOS.com/docs/running-CoreOS/cloud-Providers/Azure/](https://coreos.com/docs/running-coreos/cloud-providers/azure/)

W witrynie sieci Web CoreOS:

*CoreOS jest przeznaczony dla zabezpieczeń, zgodności i niezawodności. Zamiast instalowanie pakietów przy użyciu funkcji yum lub stosowne, CoreOS używa kontenerów Linux oraz do zarządzania usługami na wyższy poziom abstrakcji. Kod jednego usługi i wszystkie zależności są zawarte w kontenerze można uruchamiać na jednym lub kilku komputerach CoreOS.*


### <a name="credativ"></a>Credativ
[http://www.credativ.co.uk/credativ-blog/debian-images-Microsoft-Azure](http://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure)

Credativ jest niezależnych doradcze i usługi firmy, zajmujących się rozwój i wdrażanie rozwiązań profesjonalny przy użyciu bezpłatnego oprogramowania. Jako zera wiodące specjalistów Otwórz źródło Credative ma międzynarodowego uznania z wielu działów informatycznych przy użyciu pomocy technicznej. W połączeniu z firmą Microsoft Credativ jest obecnie przygotowywanie odpowiednie obrazy Debian Debian 8 (Joasia) i Debian przed 7 (Wheezy), która zaprojektowane specjalnie dla uruchamianie Azure i można łatwo zarządzać za pomocą platformy. Credativ będzie obsługują także konserwacji długoterminową i aktualizowanie obrazów Debian dla Azure za pośrednictwem jego centra Otwórz źródło pomocy technicznej.

### <a name="oracle"></a>Oracle
[http://www.Oracle.com/technetwork/topics/cloud/FAQ-1963009.HTML](http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html)

Strategia programu Oracle polega na oferuje szeroką gamę rozwiązań dla chmur publicznych i prywatnych podczas określeniu klientów wybór i elastyczność w sposób ich rozmieszczanie oprogramowania Oracle w chmury Oracle, a także inne chmury.  Programu Oracle powiązania z firmą Microsoft umożliwia klientom wdrożenia oprogramowania Oracle w publicznych i prywatnych chmury firmy Microsoft z pewnością certyfikacji i pomocy technicznej z programu Oracle.  Zobowiązania i inwestycji w rozwiązania cloud publicznych i prywatnych Oracle programu Oracle pozostaje bez zmian.

### <a name="red-hat"></a>Funkcję czerwony
[http://www.RedHat.com/en/partners/Strategic-Alliance/Microsoft](http://www.redhat.com/en/partners/strategic-alliance/microsoft)

Na świecie wiodącym dostawcą rozwiązań Otwórz źródło, czerwony funkcję ułatwia więcej niż 90% firm magazyn Fortune 500 rozwiązać problemy biznesowe, Wyrównaj do ich IT i strategie biznesowe i przygotować w przyszłości technologii. Funkcję czerwony powinien się tym zająć, dostarczając bezpiecznego rozwiązania za pośrednictwem modelu biznesowego otwarte i przystępnej, przewidywalne subskrypcji.

### <a name="suse"></a>SUSE
[http://www.SUSE.com/SUSE-Linux-Enterprise-Server-on-Azure](http://www.suse.com/suse-linux-enterprise-server-on-azure)

SUSE Linux Enterprise Server Azure jest sprawdzone platformy, która zapewnia najwyższej niezawodności i zabezpieczenia usługi w chmurze obliczeniowych. Uniwersalny platformy Linux i SUSE doskonale zintegrowany z usługami w chmurze Azure do przeprowadzania środowiskiem chmury łatwe w zarządzaniu. I więcej niż 9,200 aplikacje autoryzowane ponad 1,800 niezależnych dostawców oprogramowania dla SUSE Linux Enterprise Server SUSE zapewnia obciążenia uruchomiony obsługiwane w centrum danych mogą być bezproblemowe rozmieszczone na Azure.

### <a name="canonical"></a>Kanoniczny
[http://www.ubuntu.com/cloud/Azure](http://www.ubuntu.com/cloud/azure)

Inżynieria kanonicznych i sukcesu Ubuntu społeczność otwarta zarządzania dysk w chmurze obliczeniowych, łącznie z usługami w chmurze osobistych dla klientów indywidualnych, serwerów i klientów. Kanoniczny firmy wzroku ujednolicony bezpłatnej platformy w Ubuntu, z telefonu do chmury rodzinie spójne interfejsów dla telefonu, tabletu, telewizji i pulpit, sprawia, że Ubuntu pierwszej opcji dla różnych instytucji od dostawców publicznych chmury wskazówek elektronicznych i Ulubione między poszczególne technologists.

Z deweloperów i konstrukcyjną centra na świecie, Canonical jednoznacznie znajduje się pod kątem współpracy z producenci sprzętu, dostawców i deweloperów do rozwiązania Ubuntu rynku, na komputerach PC do serwerów i urządzeń przenośnych.

