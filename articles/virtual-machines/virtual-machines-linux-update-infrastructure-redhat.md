<properties
   pageTitle="Infrastruktura aktualizacji funkcję czerwony (RHUI) | Microsoft Azure"
   description="Dowiedz się więcej o czerwony funkcję aktualizacji infrastruktury (RHUI) wystąpień Red funkcję Enterprise Linux na żądanie w Microsoft Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="BorisB2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="borisb"/>

# <a name="red-hat-update-infrastructure-rhui-for-on-demand-red-hat-enterprise-linux-vms-in-azure"></a>Infrastruktura aktualizacji funkcję czerwony (RHUI) dla maszyny wirtualne Linux Enterprise na żądanie czerwony funkcję platformy Azure

Aby uzyskać dostęp do czerwony funkcję aktualizacji infrastruktury (RHUI) wdrożony w Azure są rejestrowane maszyn wirtualnych utworzone na podstawie obrazów czerwony funkcję Enterprise Linux (RHEL) na żądanie dostępnych w Azure Marketplace.  Wystąpienia RHEL na żądanie mają dostęp do repozytorium regionalne yum i może odbierać aktualizacje przyrostowe.

Na liście repozytorium yum, który jest zarządzane przez RHUI, jest skonfigurowany w programie RHEL podczas inicjowania obsługi administracyjnej. Nie musisz wykonywać wszelkie dodatkowe Konfiguracja — Uruchom `yum update` po wystąpienia RHEL jest gotowy, aby uzyskać najnowsze aktualizacje.

> [AZURE.NOTE] Azure infrastruktury RHUI ostatnio zaktualizowano (wrzesień 2016) i wymaga zmiany w konfiguracji istniejących wystąpień RHEL nieprzerwanie dostępu do Azure RHUI. Zapoznaj się z sekcją aktualizacji infrastruktury Azure RHUI, aby uzyskać szczegółowe informacje.


## <a name="rhui-azure-infrastructure-update"></a>Aktualizacja Azure infrastruktury RHUI
Od 2016 wrzesień Azure ma nowy zestaw serwerów czerwony funkcję aktualizacji infrastruktury (RHUI). Tych serwerach są wdrażane za [Pomocą Menedżera ruch Azure]( https://azure.microsoft.com/services/traffic-manager/) tak, aby jeden punkt końcowy (rhui 1.micrsoft.com) mogą być używane przez dowolnego maszyn wirtualnych bez względu na region. Mogą również używać certyfikatu SSL, który jest powiązane do dobrze znanych urząd certyfikacji (Baltimore główny). Tworzenie tej aktualizacji automatycznych będzie niebezpieczne dla niektórych klientów, którzy mają list ACL lub niestandardowe tabele routingu dla serwerów aktualizacji RHUI, zatem aktualizacja jest "Wyraź zgodę na." Ręczne dla ułatwiającej rozpoczęcie korzystania z tymi serwerami nowego są dostępne na tej stronie i kompletny skrypt dla ułatwiającej rozpoczęcie korzystania w zautomatyzowany sposób (na Weryfikacja poszczególne czynności). Nowe obrazy między RHEL Azure Marketplace (wersje z września 2016 lub nowsza) zostanie automatycznie wskaż nowych serwerów Azure RHUI i nie wymagają żadnych dodatkowych działań.

### <a name="the-new-azure-rhui-infrastructure-onboarding-timeline"></a>Nowe osi czasu ułatwiającej rozpoczęcie korzystania infrastruktury Azure RHUI

| Data | Uwaga |
| --- | --- |
|22 września 2016 | Serwery RHUI i instalacji wskazówek dotyczących dojazdu dostępne do użycia. Maszyny wirtualne wdrożyć za pomocą nowego (z 2016 wrzesień) między RHEL marketplace obrazy automatycznie użyje nowych serwerów RHUI, ale istniejące maszyny wirtualne są "Wyraź zgodę na"
|1 listopada 2016 | Starsze obrazów maszyn wirtualnych między RHEL, (korzystających ze starej serwery Azure RHUI) zostanie usunięty z galerii Azure Marketplace
|16 stycznia 2017 | Będzie można zlikwidować stare serwery Azure RHUI. Aktualizowanie wszystkich usługi dotyczy maszyny wirtualne RHEL między przy tym razem w celu uzyskania dostępu do Azure RHUI

### <a name="the-ips-for-the-new-rhui-content-delivery-servers-are"></a>Adresy IP dla nowych serwerów dostarczania zawartości RHUI są

```
# Azure Global
13.91.47.76
40.85.190.91
52.187.75.218
52.174.163.213

# Azure US Government
13.72.186.193
```

### <a name="manual-update-procedure-to-use-the-new-azure-rhui-servers"></a>Ręczna aktualizacja procedurę, aby korzystać z nowych serwerów Azure RHUI

Pobieranie (przez zwinięcie) publicznej podpisu klucza

```
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 
```

Sprawdź klucz pobrany

```
gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release
```

Zaznacz dane wyjściowe, sprawdź `keyid` i `user ID packet`:

```
Version: GnuPG v1.4.7 (GNU/Linux)
:public key packet:
        version 4, algo 1, created 1446074508, expires 0
        pkey[0]: [2048 bits]
        pkey[1]: [17 bits]
        keyid: EB3E94ADBE1229CF
:user ID packet: "Microsoft (Release signing) <gpgsecurity@microsoft.com>"
:signature packet: algo 1, keyid EB3E94ADBE1229CF
        version 4, created 1446074508, md5len 0, sigclass 0x13
        digest algo 2, begin of digest 1a 9b
        hashed subpkt 2 len 4 (sig created 2015-10-28)
        hashed subpkt 27 len 1 (key flags: 03)
        hashed subpkt 11 len 5 (pref-sym-algos: 9 8 7 3 2)
        hashed subpkt 21 len 3 (pref-hash-algos: 2 8 3)
        hashed subpkt 22 len 2 (pref-zip-algos: 2 1)
        hashed subpkt 30 len 1 (features: 01)
        hashed subpkt 23 len 1 (key server preferences: 80)
        subpkt 16 len 8 (issuer key ID EB3E94ADBE1229CF)
        data: [2047 bits]
```

Instalowanie klucz publiczny

```
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release
```

Pobieranie, weryfikowanie i instalowanie klienta prędkości.

Plik do pobrania: Dla RHEL 6

```
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel6/rhui-azure-rhel6-2.0-2.noarch.rpm 
```

Aby uzyskać RHEL 7

```
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel7/rhui-azure-rhel7-2.0-2.noarch.rpm  
```

Sprawdź:

```
rpm -Kv azureclient.rpm
```

Sprawdzanie danych wyjściowych tego podpisu pakietu jest OK

```
azureclient.rpm:
    Header V3 RSA/SHA256 Signature, key ID be1229cf: OK
    Header SHA1 digest: OK (927a3b548146c95a3f6c1a5d5ae52258a8859ab3)
    V3 RSA/SHA256 Signature, key ID be1229cf: OK
    MD5 digest: OK (c04ff605f82f4be8c96020bf5c23b86c)
```

Instalowanie prędkości.

```
sudo rpm -U azureclient.rpm
```

Po zakończeniu zweryfikować, że można uzyskać dostęp do formularza Azure RHUI maszyn wirtualnych

### <a name="all-in-one-script-for-automating-the-above-task"></a>Automatyzowanie zadań powyżej skrypt w jednym
Skorzystaj z tego skryptu odpowiednio do automatyzowania zadań aktualizacji dotyczy maszyny wirtualne na nowe serwery Azure RHUI.

```
# Download key
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 

# Validate key
if ! gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release | grep -q "keyid: EB3E94ADBE1229CF"; then
    echo "Keyfile azure.asc NOT valid. Exiting."
    exit 1
fi

# Install Key
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release

# Download RPM package
if grep -q "release 7" /etc/redhat-release; then
    ver=7
elif  grep -q "release 6" /etc/redhat-release; then
    ver=6
else
    echo "Version not supported, exiting"
    exit 1
fi

url=https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel$ver/rhui-azure-rhel$ver-2.0-2.noarch.rpm
curl -o azureclient.rpm "$url"

# Verify package
if ! rpm -Kv azureclient.rpm | grep -q "key ID be1229cf: OK"; then
    echo "RPM failed validation ($url)"
    exit 1
fi

# Install package
sudo rpm -U azureclient.rpm
```

## <a name="rhui-overview"></a>Omówienie RHUI
[Infrastruktura aktualizacji funkcję czerwony](https://access.redhat.com/products/red-hat-update-infrastructure) oferuje wysoce skalowalna rozwiązanie zarządzanie zawartością repozytorium yum dla wystąpień chmury Red funkcję Enterprise Linux, które są obsługiwane przez dostawców certyfikowane przez funkcję czerwony chmury. W zależności od nadrzędnego projektu masy, RHUI umożliwia dostawców chmury lokalnie lustrzane hostowaną funkcję czerwony repozytorium zawartości, tworzyć niestandardowe repozytoria własną zawartością i udostępnić te repozytoria dużej grupy użytkowników końcowych za pośrednictwem równoważenia obciążenia systemu dostarczania zawartości.

## <a name="regions-where-rhui-is-available"></a>Regiony, w której znajduje się RHUI
RHUI jest dostępna we wszystkich regionach, w których RHEL obrazy na żądanie są dostępne. Obecnie zawiera wszystkie publicznej regionów wymienionych na stronie [stanu Azure pulpitu nawigacyjnego](https://azure.microsoft.com/status/) i regiony Azure nam dla instytucji rządowych. RHUI dostępu dla maszyny wirtualne z obrazami na żądanie RHEL znajduje się w ich ceny. Dostępność dodatkowe opcje regionalne i krajowe chmury zostanie zaktualizowana zgodnie z rozszerzymy RHEL na żądanie dostępność w przyszłości.

> [AZURE.NOTE] Dostęp do hostowanej Azure RHUI jest ograniczone do maszyny wirtualne w [zakresów adresów IP centrum danych Azure firmy Microsoft](https://www.microsoft.com/download/details.aspx?id=41653).

## <a name="get-updates-from-another-update-repository"></a>Pobieranie aktualizacji z innego repozytorium aktualizacji

Jeśli chcesz pobrać aktualizacje z inną aktualizację repozytorium (zamiast hostowanej Azure RHUI) będzie konieczne wyrejestrowywanie wystąpień usługi z RHUI i ponownie zarejestrować infrastruktury odpowiedniej aktualizacji (na przykład czerwony funkcję satelitarny lub czerwony funkcję klienta portalu CDN). Konieczne będzie odpowiedni subskrypcje funkcję czerwony tych usług i rejestracji [Czerwony funkcję dostęp](https://access.redhat.com/ecosystem/partners/ccsp/microsoft-azure)w chmurze platformy Azure.

Aby unregister RHUI i zarejestrować do swojego Obserwuj infrastruktury aktualizacji kroki.

1.  Edytowanie /etc/yum.repos.d/rh-cloud.repo i zmienić wszystkie `enabled=1` do `enabled=0`. Na przykład:

        # sed -i 's/enabled=1/enabled=0/g' /etc/yum.repos.d/rh-cloud.repo

2.  Edytowanie /etc/yum/pluginconf.d/rhnplugin.conf i zmienianie `enabled=0` do `enabled=1`.
3.  Następnie rejestrować odpowiedniej infrastruktury, takich jak czerwony funkcję klienta portalu. Prowadnicy funkcję czerwony rozwiązanie dotyczące [rejestrowania i zasubskrybuj systemu do portalu czerwony funkcję klienta](https://access.redhat.com/solutions/253273).

> [AZURE.NOTE] Dostęp do RHUI hostowanej Azure jest uwzględniony w cenie obraz RHEL repartycyjny (między). Wyrejestrowywanie maszyny RHEL między z RHUI hostowanej Azure nie konwertować maszyny wirtualnej Przesuń i-właścicielem-licencji (BYOL) typu maszyn wirtualnych i w związku z tym możesz może być naliczania podwójne opłat po zarejestrowaniu samej maszyn wirtualnych z innego źródła aktualizacji. 
> 
> Jeśli musisz spójne infrastruktura aktualizacji za pomocą innych niż hostowanej Azure RHUI warto rozważyć tworzenie i wdrażanie własnych obrazów (typu BYOL), zgodnie z opisem w artykule [Tworzenie i oparte na przekazywanie funkcję czerwony maszyn wirtualnych dla Azure](virtual-machines-linux-redhat-create-upload-vhd.md) .

## <a name="next-steps"></a>Następne kroki
Aby utworzyć maszyny Linux Enterprise funkcję czerwony z obrazem Azure Marketplace repartycyjny i dźwigni hostowanej Azure RHUI przejdź do [Usługi Azure Marketplace](https://azure.microsoft.com/marketplace/partners/redhat/). Można użyć `yum update` wystąpienia RHEL bez wszelkie dodatkowe ustawienia.