<properties
   pageTitle="Omówienie zabezpieczeń Azure maszyn wirtualnych | Microsoft Azure"
   description=" Azure maszyn wirtualnych umożliwiają elastyczność wirtualizacji bez konieczności zakupu i zachować sprzętem fizycznym, który jest uruchamiany maszyny wirtualnej.  W tym artykule omówiono podstawowe funkcje zabezpieczeń Azure, które mogą być używane z maszyn wirtualnych Azure. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="terrylan"/>

# <a name="azure-virtual-machines-security-overview"></a>Omówienie zabezpieczeń maszyn wirtualnych Azure

Azure maszyn wirtualnych umożliwia wdrażanie szeroką gamę obliczeniowych rozwiązań w sposób adaptacyjne. Dzięki obsłudze Microsoft Windows, Linux, program Microsoft SQL Server, Oracle, IBM, SAP i usługi Azure BizTalk należy wdrożyć wszelkie obciążenie pracą i dowolny język na niemal dowolnym system operacyjny.

Azure maszyn wirtualnych umożliwia elastyczne wirtualizacji bez konieczności zakupu i obsługa sprzętu, który jest uruchamiany maszyny wirtualnej.  Możesz tworzyć i wdrażanie aplikacji przy użyciu zapewnienie, że dane są chronione i bezpiecznych w naszym wysokim poziomie zabezpieczeń centrach danych.

Z Azure można tworzyć ulepszonych zabezpieczeń, zgodności rozwiązań który:

- Ochrona przed wirusami i złośliwym oprogramowaniem maszyn wirtualnych
- Szyfrowanie danych poufnych
- Zabezpieczanie ruchu w sieci
- Identyfikowanie i wykrywanie zagrożeń
- Spełnia wymagania dotyczące zgodności

Celem tego artykułu jest zawierają omówienie podstawowych funkcji Azure zabezpieczeń, które mogą być używane z maszyn wirtualnych. Firma Microsoft udostępnia również łącza do artykułów zawierających szczegółowe informacje o każdej funkcji, aby dowiedzieć się więcej.  

Podstawowe możliwości zabezpieczeń maszyn wirtualnych Azure powinny być zawarte w tym artykule:

- Ochrony przed złośliwym oprogramowaniem
- Sprzętowego modułu zabezpieczeń
- Szyfrowanie dysków maszyn wirtualnych
- Wykonywanie kopii zapasowych maszyn wirtualnych
- Odzyskiwanie witryny Azure
- Wirtualna sieć
- Zarządzanie zasadami zabezpieczeń i raportowania
- Zgodność

## <a name="antimalware"></a>Ochrony przed złośliwym oprogramowaniem

Z Azure oprogramowanie ochrony przed złośliwym oprogramowaniem dostawców zabezpieczeń, takich jak Microsoft, Symantec Trend Micro, McAfee i Kaspersky służy do ochrony maszyn wirtualnych z programami do wyświetlania reklam, złośliwy plików i innymi zagrożeniami. W sekcji Dowiedz się więcej, poniżej, aby znaleźć artykuły u partnera rozwiązań.

Microsoft Antimalware dla usług w chmurze Azure i maszyn wirtualnych jest funkcja ochrony w czasie rzeczywistym, który ułatwia identyfikowanie i usuwanie wirusów, oprogramowania szpiegującego i innego złośliwego oprogramowania.  Microsoft Antimalware zawiera można skonfigurować alerty, gdy wiadomo, że złośliwy lub niepożądane oprogramowanie pozwala podjąć próbę instalacji lub uruchomienia systemach Azure.

Microsoft Antimalware jest rozwiązaniem agenta pojedynczej aplikacji i środowisk dzierżawy, przeznaczone do uruchamiania w tle bez udziału rozmówcy. Można wdrażać ochrony oparte na potrzeby obciążeń pracą usługi aplikacji, z obu podstawowe bezpiecznego-— domyślnie lub Zaawansowane niestandardowej konfiguracji, w tym monitorowania ochrony przed złośliwym oprogramowaniem.

Gdy wdrażanie i Włącz Antimalware firmy Microsoft, są dostępne następujące funkcje podstawowe:

- Ochrona w czasie rzeczywistym - aktywności monitorów w usług w chmurze, a w przypadku maszyn wirtualnych do wykrywania i blokowania wykonanie złośliwego oprogramowania.
- Okresowo zaplanowane skanowanie - wykonuje docelowej skanowanie wykrywanie przed złośliwym oprogramowaniem, łącznie z aktywnie uruchomione programy.
- Rozwiązywanie problemu przed złośliwym oprogramowaniem - automatycznie pobiera akcji wykryto złośliwe oprogramowanie, takie jak usuwanie lub quarantining złośliwych plików i czyszczenie wpisy rejestru złośliwy.
- Aktualizacje podpisu — automatycznie instaluje najnowszych sygnatur ochrony (definicje wirusów) w celu zapewnienia ochrony jest aktualne na ustalonej częstotliwości.
- Aparat ochrony przed złośliwym oprogramowaniem aktualizacji automatycznie aktualizuje aparat Microsoft Antimalware.
- Aktualizacje platformy ochrony przed złośliwym oprogramowaniem — automatycznie aktualizuje platformy Microsoft Antimalware.
- Aktywnej ochrony - Zwierzchnik metadanych Azure telemetrycznego o wykrytych zagrożeń i podejrzanych zasobów w celu zapewnienia szybkiego odpowiedzi, umożliwiając w czasie rzeczywistym podpisu synchroniczne dostarczania za pośrednictwem aktywnego systemu ochrony (ZAMAPUJE) firmy Microsoft.
- Przykłady raportów — dostępnych i raportów próbek z usługą Microsoft Antimalware ułatwiające Uściślanie usługę i włączyć rozwiązywanie problemów.
- Wyłączenia — umożliwia aplikacji i administratorów usługi skonfigurować niektóre pliki, procesy oraz dysków w celu wyłączenia ich z ochrony i skanowanie w poszukiwaniu wydajności i innych powodów.
- Zbieranie zdarzeń ochrony przed złośliwym oprogramowaniem - zapisuje kondycja usługi ochrony przed złośliwym oprogramowaniem, podejrzanych działań i rozwiązywanie problemu działań podjętych w dzienniku zdarzeń systemu operacyjnego i zbiera je do magazynowania Azure klienta.

Dowiedz się więcej: Aby uzyskać więcej informacji na temat ochrony przed złośliwym oprogramowaniem oprogramowanie w celu ochrony maszyn wirtualnych, zobacz:

- [Microsoft ochrony przed złośliwym oprogramowaniem, usługami w chmurze Azure i maszyn wirtualnych](../security/azure-security-antimalware.md)
- [Wdrażanie rozwiązań ochrony przed złośliwym oprogramowaniem Azure maszyn wirtualnych](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
- [Jak zainstalować i skonfigurować Trend Micro głębokości Security jako usługa na maszyn wirtualnych systemu Windows](../virtual-machines/virtual-machines-windows-classic-install-trend.md)
- [Jak zainstalować i skonfigurować ochrona punktu końcowego Symantec na maszyn wirtualnych systemu Windows](../virtual-machines/virtual-machines-windows-classic-install-symantec.md)
- [Nowe opcje ochrony przed złośliwym oprogramowaniem do ochrony Azure maszyn wirtualnych — ochrona punktu końcowego McAfee](https://azure.microsoft.com/blog/new-antimalware-options-for-protecting-azure-virtual-machines/)
- [Rozwiązania zabezpieczeń w Azure Marketplace](https://azure.microsoft.com/marketplace/?term=security)

## <a name="hardware-security-module"></a>Zabezpieczenia sprzętowe modułu

Szyfrowanie i uwierzytelnianie nie poprawia zabezpieczeń, chyba że samych kluczy jest chroniony. Przechowując je w Azure klucza magazynu może uprościć zarządzania i zabezpieczanie klawiszy i krytyczne hasła. Klucz magazynu umożliwia przechowywanie kluczy w modułach wartościowego sprzętu (HSM) certyfikat FIPS 140-2 standardy poziomu 2. Kluczy szyfrowania programu SQL Server dla kopii zapasowej lub [szyfrowania danych przezroczysty](https://msdn.microsoft.com/library/bb934049.aspx) wszystkie można przechowywać w klucza magazynu z klawiszy i hasła z aplikacji. Uprawnienia i dostęp do tych elementów chronione są zarządzane za pośrednictwem [Usługi Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).

Dowiedz się więcej:

- [Co to jest Azure klucza magazynu?](../key-vault/key-vault-whatis.md)
- [Rozpoczynanie pracy z magazynu klucza Azure](../key-vault/key-vault-get-started.md)
- [Azure blogu klucza magazynu](https://blogs.technet.microsoft.com/kv/)

## <a name="virtual-machine-disk-encryption"></a>Szyfrowanie dysków maszyn wirtualnych

Azure szyfrowanie dysku jest nowe możliwości, które umożliwia szyfrowanie dysków systemu Windows i Linux oraz maszyn wirtualnych Azure. Azure szyfrowanie dysków używa branżowe standardowa funkcja [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) systemu Windows i funkcji [kryptograficznego dm](https://en.wikipedia.org/wiki/Dm-crypt) Linux oraz w celu zapewnienia szyfrowania woluminu systemu operacyjnego i dyski danych.

Rozwiązanie jest zintegrowany z magazynu klucza Azure ułatwiające kontrolowanie i zarządzanie nimi kluczy szyfrowania dysku i hasła w ramach subskrypcji klucza magazynu, zapewniając, że wszystkie dane w dyski maszyn wirtualnych są szyfrowane na pozostałych w magazynie Azure.

Dowiedz się więcej:

- [Szyfrowanie dysków Azure dla systemu Windows i Linux oraz IaaS maszyny wirtualne](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)
- [Szyfrowanie dysków Azure Linux i maszyn wirtualnych systemu Windows](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/azure-disk-encryption-for-linux-and-windows-virtual-machines-public-preview-now-available/)
- [Szyfrowanie maszyny wirtualnej](../security-center/security-center-disk-encryption.md)

## <a name="virtual-machine-backup"></a>Wykonywanie kopii zapasowych maszyn wirtualnych

Kopia zapasowa Azure jest skalowalna rozwiązanie, które chroni dane aplikacji z zerowych inwestycji kapitałowych i minimalne koszty operacyjne. Błędy aplikacji może spowodować uszkodzenie danych, a błędy ludzi można wprowadzać usterek do aplikacji. Kopia zapasowa Azure są chronione do maszyn wirtualnych z systemem Windows i Linux.

Dowiedz się więcej:

- [Co to jest kopia zapasowa Azure?](../backup/backup-introduction-to-azure-backup.md)
- [Ścieżka nauki kopii zapasowej Azure](https://azure.microsoft.com/documentation/learning-paths/backup/)
- [Usługa Azure kopia zapasowa — często zadawane pytania](../backup/backup-azure-backup-faq.md)

## <a name="azure-site-recovery"></a>Odzyskiwanie witryny Azure

Ważne część strategii BCDR Twojej organizacji jest objaśnienie się, jak przechowywać firmową obciążenia i aplikacji i uruchamianie wystąpieniu dostawie zaplanowane i niezaplanowane. Odzyskiwanie witryny Azure pomaga dodać akompaniament replikacji, pracy awaryjnej i odzyskiwanie obciążenia i aplikacje, aby były dostępne z lokalizacji pomocniczej Jeśli Twojej lokalizacji podstawowej awarii.

Odzyskiwanie witryny:

- **Simplifies strategii BCDR** — Odzyskiwanie witryny ułatwia obsługę replikacji, pracy awaryjnej i odzyskiwanie wielu biznesowych i aplikacje z jednego miejsca. Odzyskiwanie witryny orchestrates replikacji i pracy awaryjnej, ale nie ODCIĘTA dane aplikacji lub mieć dowolne informacje.
- **Elastyczne replikacji zawiera** — odzyskiwanie przy użyciu witryny można odtworzyć obciążenia działający w środowisku maszyn wirtualnych systemu funkcji Hyper-V, maszyn wirtualnych VMware i serwerów fizycznych systemu Windows i Linux.
- **Obsługa awaryjnego i odzyskiwania** — Odzyskiwanie witryny zawiera praca awaryjna test do obsługi ćwiczenia odzyskiwania danych bez wpływu na środowisku produkcyjnym. Można również uruchomić planowana praca awaryjna ze stratą zero danych dla oczekiwanych dostawie lub niezaplanowane praca awaryjna ze stratą minimalnego danych (w zależności od częstotliwości replikacji) dla nieoczekiwane awarii. Po przełączeniu możesz powrotu do witryn podstawowego. Odzyskiwanie witryny zawiera plany odzyskiwania, które mogą zawierać skrypty i skoroszyty Azure automatyzacji tak, aby można dostosować awaryjnego i odzyskiwania wielu aplikacji.
- **Pozwala wyeliminować pomocniczej centrum danych** — można replikować do witryny pomocniczej lokalnego lub Azure. Używanie Azure jako miejsca docelowego dla odzyskiwanie eliminuje kosztów i złożoności utrzymania pomocniczej witryny. Replikowane dane są przechowywane w magazynie Azure.
- **Integrates z istniejących technologii BCDR** — Odzyskiwanie witryny współpracuje z innymi funkcjami BCDR aplikacji. Na przykład umożliwia odzyskiwanie witryny ochrony typu programu SQL Server obciążenia firmy. Dotyczy to również natywnych obsługi programu SQL Server (AlwaysOn) do zarządzania awaryjnego przeniesienia grupy dostępności.

Dowiedz się więcej:

- [Co to jest Azure Odzyskiwanie witryny?](../site-recovery/site-recovery-overview.md)
- [Jak działa odzyskiwanie Azure witryny?](../site-recovery/site-recovery-components.md)
- [Co obciążenia są chronione za pomocą Odzyskiwanie witryny Azure?](../site-recovery/site-recovery-workload.md)

## <a name="virtual-networking"></a>Wirtualna sieć

Maszyn wirtualnych potrzebujesz połączenia sieciowego. Aby obsługiwać ten wymóg, Azure wymaga maszyn wirtualnych jest połączenie z siecią wirtualną Azure. Wirtualna sieć Azure jest logiczne konstrukcji wbudowane na wierzchu tkaninie sieci fizycznej Azure. Każdy logiczne Azure wirtualnej sieci jest ze wszystkich innych Azure wirtualnej sieci. Ten izolacji pomaga upewnić się, że ruch sieciowy wdrożeń nie jest dostępny dla innych klientów Microsoft Azure.

Dowiedz się więcej:

- [Omówienie zabezpieczeń sieci Azure](security-network-overview.md)
- [Omówienie wirtualnych sieci](../virtual-network/virtual-networks-overview.md)
- [Funkcje sieci i partnerstwa dla przedsiębiorstw](https://azure.microsoft.com/blog/networking-enterprise/)

## <a name="security-policy-management-and-reporting"></a>Zarządzanie zasadami zabezpieczeń i raportowania

Centrum zabezpieczeń Azure ułatwia zapobieganie, wykrywanie i odpowiadanie na zagrożenia i zapewnia zwiększone wgląd i kontrolować, zabezpieczenia Azure zasobów. Zapewnia zabezpieczenia zintegrowane monitorowania i zasad zarządzania w subskrypcjach usługi Azure, pomaga wykrywać zagrożenia, które w przeciwnym razie przejdź niezauważalnie i współpraca z Szeroki ekosystem rozwiązania zabezpieczeń.

Centrum zabezpieczeń Azure pomaga zoptymalizować i monitorowanie zabezpieczeń maszyn wirtualnych:

- Dostarczając maszyn wirtualnych [zalecenia dotyczące zabezpieczeń](../security-center/security-center-recommendations.md) , takich jak stosowanie aktualizacji systemu, konfigurowanie punktów końcowych list ACL, włączanie ochrony przed złośliwym oprogramowaniem, włączanie grup zabezpieczeń sieci i stosowanie szyfrowania dysku.
- Monitorowanie stanu maszyn wirtualnych

Dowiedz się więcej:

- [Wprowadzenie do Centrum zabezpieczeń Azure](../security-center/security-center-intro.md)
- [Centrum zabezpieczeń Azure — często zadawane pytania](../security-center/security-center-faq.md)
- [Planowanie Centrum zabezpieczeń Azure i operacji](../security-center/security-center-planning-and-operations-guide.md)

## <a name="compliance"></a>Zgodność

Azure maszyn wirtualnych ma certyfikat dla dotyczących FISMA, FedRAMP HIPAA, PCI DSS poziomu 1 i innych programów zgodności klucza. Certyfikat ułatwia Azure aplikacji spełnić wymagania dotyczące zgodności, a dla swojej firmy, adresem szeroką gamę połączenia krajowe i międzynarodowe regulacji.

Dowiedz się więcej:

- [Centrum zaufania programu Microsoft: zgodność](https://www.microsoft.com/TrustCenter/Compliance/default.aspx)
- [Zaufanych chmurze: Microsoft Azure zabezpieczenia, prywatność i zgodność](http://download.microsoft.com/download/1/6/0/160216AA-8445-480B-B60F-5C8EC8067FCA/WindowsAzure-SecurityPrivacyCompliance.pdf)
