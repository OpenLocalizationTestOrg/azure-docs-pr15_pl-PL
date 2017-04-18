<properties
  pageTitle="Azure i Linux oraz maszyn wirtualnych miejsca do magazynowania | Microsoft Azure"
  description="W tym artykule opisano Azure standardowy i miejsca do magazynowania Premium maszyn wirtualnych Linux."
  services="virtual-machines-linux"
  documentationCenter="virtual-machines-linux"
  authors="vlivech"
  manager="timlt"
  editor=""/>

<tags
  ms.service="virtual-machines-linux"
  ms.devlang="NA"
  ms.topic="article"
  ms.tgt_pltfrm="vm-linux"
  ms.workload="infrastructure"
  ms.date="10/04/2016"
  ms.author="v-livech"/>

# <a name="azure-and-linux-vm-storage"></a>Azure i Linux oraz maszyn wirtualnych

Azure magazyn jest masowej chmury nowoczesne aplikacje, które korzystają z wytrzymałości, dostępność i skalowalność na potrzeby swoich klientów.  Oprócz pozwalający programistom na tworzenie dużych aplikacji do obsługi nowych scenariuszy magazyn Azure udostępnia foundation miejsca do magazynowania dla maszyn wirtualnych Azure.

## <a name="azure-storage-standard-and-premium"></a>Azure miejsca do magazynowania: Standard i Premium

Azure maszyn wirtualnych mogą być wbudowane na standardowego magazynu dysków lub dysków miejsca do magazynowania premium.  Podczas korzystania z portalu służących do maszyn wirtualnych możesz przełączyć listy rozwijanej na ekranie — informacje podstawowe, aby wyświetlić dyski standardowych i premium.  Zrzut ekranu poniżej wyróżnienie tego menu przełączania.  Po ich SSD, tylko premium miejsca do magazynowania enabled maszyny wirtualne zostanie wyświetlona, wszystkie obsługiwane przez SSD dysków.  Po ich dysk twardy, standardowego magazynu włączone maszyny wirtualne Obracająca kopii dysków zostanie wyświetlona, wraz z miejsca do magazynowania premium maszyny wirtualne obsługiwane przez SSD.

  ![screen1](../virtual-machines/media/virtual-machines-linux-azure-vm-storage-overview/screen1.png)

Podczas tworzenia maszyn wirtualnych z `azure-cli` możesz wybrać pozycję standard i premium podczas wybierania rozmiar pamięci Wirtualnej za pośrednictwem `-z` lub `--vm-size` polecenie flagi.

### <a name="create-a-vm-with-standard-storage-vm-on-the-cli"></a>Tworzenie maszyn wirtualnych przy użyciu standardowego magazynu maszyn wirtualnych na polecenie

Polecenie Flaga `-z` wybiera Standard_A1 z A1 standardowego magazynu opartej maszyn wirtualnych Linux.

```bash
azure vm quick-create -g rbg \
exampleVMname \
-l westus \
-y Linux \
-Q Debian \
-u exampleAdminUser \
-M ~/.ssh/id_rsa.pub
-z Standard_A1
```

### <a name="create-a-vm-with-premium-storage-on-the-cli"></a>Tworzenie maszyn wirtualnych z magazynu premium na polecenie

Polecenie Flaga `-z` wybiera Standard_DS1 z DS1 magazynowania premium opartej maszyn wirtualnych Linux.

```bash
azure vm quick-create -g rbg \
exampleVMname \
-l westus \
-y Linux \
-Q Debian \
-u exampleAdminUser \
-M ~/.ssh/id_rsa.pub
-z Standard_DS1
```

## <a name="standard-storage"></a>Standardowy miejsca do magazynowania

Azure standardowego magazynu jest domyślny typ przestrzeni dyskowej.  Standardowego magazynu obowiązuje kosztu przy zachowaniu performant.  

## <a name="premium-storage"></a>Magazyn Premium

Azure magazynowania Premium udostępnia obsługę dysku wysokiej wydajności i małych opóźnień maszyn wirtualnych z systemem obciążenia I-O-intensywną. Dyski maszyn wirtualnych (maszyn wirtualnych), z magazynu Premium przechowywania danych w stanie stałym dyski SSD. Możesz przeprowadzić migrację aplikacji maszyn wirtualnych dysków do magazynu Premium Azure podjęcie zaletą szybkości i wydajności tych dysków.

Funkcje magazynowania Premium:

- Dyski miejsca do magazynowania Premium: Azure magazyn Premium obsługuje dysków maszyn wirtualnych, które można dołączać do usługi Katalogowej, DSv2 lub GS serii maszyny wirtualne Azure.

- Obiektów Blob strony Premium: Magazyn Premium obsługuje blob strony Azure, które są używane do przechowywania trwałych dysków dla Azure wirtualnych maszyn.

- Przechowywanie lokalnie zbędne Premium: Konto magazynowania Premium tylko obsługuje lokalnie zbędne przestrzeni dyskowej (LRS) jako opcja replikacji i zachowuje trzy kopie danych w jednym regionie.

- [Magazyn Premium](../storage/storage-premium-storage.md)

## <a name="premium-storage-supported-vms"></a>Magazyn Premium obsługiwane maszyny wirtualne

Magazyn Premium obsługuje serii DS, DSv2 serii serii GS i serii Fs Azure wirtualnych maszyn. Za pomocą dysków w magazynie zarówno Standard i Premium z miejsca do magazynowania Premium obsługiwane z maszyny wirtualne. Ale nie można używać dysków w magazynie Premium serii maszyn wirtualnych, które nie są zgodne magazynowania Premium.

Poniżej przedstawiono rozkładów Linux możemy sprawdzana poprawność o Premium miejsca do magazynowania.

| ROZKŁAD | Wersja                 | Obsługiwane jądra    |
|--------------|-------------------------|---------------------|
| Ubuntu       | 12.04                   | 3.2.0-75.110+       |
| Ubuntu       | 14.04                   | 3.13.0-44.73+       |
| Debian       | 7.x, 8.x                | 3.16.7-ckt4-1+      |
| SLES         | SLES 12                 | 3.12.36-38.1+       |
| SLES         | SLES 11 SP4             | 3.0.101-0.63.1+     |
| CoreOS       | 584.0.0+                | 3.18.4+             |
| Centos       | 6.5, 6.6, 6,7, 7.0, 7.1 | 3.10.0-229.1.2.el7+ |
| RHEL         | 6.8 +, 7.2 +              |                     |


## <a name="file-storage"></a>Magazyn plików

Magazyn plików Azure oferuje udziały plików w chmurze za pomocą protokołu SMB standardowy. Z plikami Azure możesz przeprowadzić migrację aplikacji przedsiębiorstwa, które zależą na serwerach plików Azure. Aplikacje platformy Azure łatwo można zainstalować udziały plików z Azure maszyn wirtualnych z systemem Linux. I z najnowszą wersją miejsca do magazynowania plików, można zainstalować udziału plików z aplikacji lokalnej, która obsługuje SMB 3.0.  Ponieważ udziałach plików są akcje SMB, możesz uzyskiwać do nich dostęp za pośrednictwem interfejsów API systemu standardowego pliku.

Magazyn plików jest oparty na tej samej technologii co magazyn obiektów Blob, tabel i kolejki, aby magazyn plików oferuje dostępność, wytrzymałości, skalowalność i nadmiarowości geo wbudowana w platformy Azure miejsca do magazynowania. Aby uzyskać szczegółowe informacje o cele miejsca do magazynowania plików i ograniczenia Zobacz skalowalność miejsca do magazynowania Azure i cele.

- [Jak korzystać z systemem Linux magazyn plików Azure](../storage/storage-how-to-use-files-linux.md)

## <a name="hot-storage"></a>Najpopularniejsze miejsca do magazynowania

Warstwa Azure ciepłej magazynowania jest zoptymalizowana pod kątem przechowywania danych, który jest dostępny często.  Najpopularniejsze miejsce do magazynowania jest domyślny typ miejsca do magazynowania dla obiektów blob sklepy.

## <a name="cool-storage"></a>Atrakcyjne miejsca do magazynowania

Warstwa Azure atrakcyjne miejsca do magazynowania jest zoptymalizowana pod kątem przechowywania danych, czyli rzadko używanych i długotrwałe. Przykład przypadków użycia atrakcyjne magazynu zawiera kopie zapasowe, zawartość multimedialną, danych naukowych, zgodności i archiwizacji danych. Ogólnie wszystkie dane, które rzadko jest dostępny jest doskonałe candidate atrakcyjne magazynu.

|                             | Poziom dostępu miejsca do magazynowania      | Warstwa atrakcyjne miejsca do magazynowania     |
|:----------------------------|:---------------------:|:---------------------:|
| Dostępność                | 99,9%                 | 99%                   |
| Dostępność (odczytuje GRS pomoc Zdalna) | 99,99%                | 99,9%                 |
| Opłaty za zużycie               | Wyższe koszty miejsca do magazynowania  | Niższe koszty magazynowania   |
|                             | Dolnym programu access          | Wyższa programu access         |
|                             | i kosztów transakcja | i kosztów transakcja |


## <a name="redundancy"></a>Nadmiarowości

Dane na koncie Microsoft Azure miejsca do magazynowania jest zawsze replikować do zapewnienia wytrzymałości i szybkiej SLA miejsca do magazynowania Azure nawet obliczu awarie sprzętu przejściowych na spotkanie.

Podczas tworzenia konta miejsca do magazynowania, należy wybrać jedną z następujących opcji replikacji:

- Lokalnie zbędne przestrzeni dyskowej (LRS)
- Strefa zbędne przestrzeni dyskowej (ZRS)
- Zbędne Geo przestrzeni dyskowej (GRS)
- Dostęp do odczytu zbędne geo miejsca do magazynowania (Pomoc Zdalna GRS)

### <a name="locally-redundant-storage"></a>Lokalnie zbędne miejsca do magazynowania

Lokalnie zbędne przestrzeni dyskowej (LRS) replikuje dane w obszarze, w którym zostały utworzone konta miejsca do magazynowania. Aby zmaksymalizować wytrzymałości, co wniosek danych na swoim koncie przestrzeni dyskowej są replikowane trzy razy. Te trzy repliki każdego znajdują się w osobnych błędów domen i uaktualniania domen.  Żądanie pomyślnie zwraca tylko wtedy, gdy została zapisana do wszystkich replik trzy.

### <a name="zone-redundant-storage"></a>Strefa zbędne miejsca do magazynowania

Strefa zbędne przestrzeni dyskowej (ZRS) replikuje danych w dwóch lub trzech urządzeń, w jednym regionie lub w dwóch regionów, dostarczając wytrzymałości wyższymi niż LRS. Jeśli Twoje konto miejsca do magazynowania ma ZRS włączona, dane są trwałe nawet w przypadku awarii w jednej z funkcji.

### <a name="geo-redundant-storage"></a>Zbędne Geo miejsca do magazynowania

Zbędne Geo przestrzeni dyskowej (GRS) replikuje danych na pomocniczej regionu, który jest setki mila poza podstawowym region. Jeśli Twoje konto miejsca do magazynowania ma GRS włączona, dane są trwałe nawet w przypadku awarii regionalne wykonane lub danych, w którym podstawowy region nie jest odzyskania.

### <a name="read-access-geo-redundant-storage"></a>Dostęp do odczytu zbędne geo przestrzeni dyskowej

Dostęp do odczytu zbędne geo miejsca do magazynowania (Pomoc Zdalna GRS) maksymalizuje dostępność dla Twojego konta miejsca do magazynowania, dostarczając tylko do odczytu dostęp do danych w lokalizacji pomocniczej, oprócz replikacja za pośrednictwem dwóch regionów dostarczony przez GRS. W przypadku, gdy dane będą niedostępne w obszarze podstawowy, aplikacja mogły odczytywać dane z obszaru pomocniczą.

Dla głębokości dive do magazynowania Azure nadmiarowości zobacz:

- [Azure replikacji miejsca do magazynowania](../storage/storage-redundancy.md)

## <a name="scalability"></a>Skalowalność

Azure miejsca do magazynowania jest znacznym stopniu skalowalna, dzięki czemu można przechowywać i proces setki terabajtów danych w celu obsługi scenariuszy duży danych wymaganych przez analizy naukowych, finansowych i aplikacji. Lub mogą zawierać niewielkich ilości danych wymaganych do witryny sieci Web małej firmy. Miejsce, w którym przypada potrzeb, możesz zapłacić tylko dane, które przechowujesz. Magazyn Azure obecnie przechowuje dziesiątki trillions obiektów unikatowe klienta i średnio obsługuje miliony żądania na sekundę.

W przypadku kont standardowego magazynu: konto standardowego magazynu ma wezwanie na maksymalna liczba całkowita liczba 20 000 operacji i/o na SEKUNDĘ. Suma operacji i/o na SEKUNDĘ we wszystkich dysków maszyn wirtualnych z konta standardowego magazynu należy nie przekracza ten limit.

W przypadku kont miejsca do magazynowania premium: konto miejsca do magazynowania premium jest maksymalna liczba całkowita przepustowość 50 GB. Ogólnej przepustowości we wszystkich dysków maszyn wirtualnych należy nie przekracza ten limit.

## <a name="availability"></a>Dostępność

Firma Microsoft gwarantuje, że co najmniej 99,99% (99,9% dla atrakcyjne poziom dostępu) czasu, firma Microsoft będzie pomyślnie przetwarzać żądania do odczytywania danych z kont Geo odczyt zbędne miejsca do magazynowania (Pomoc Zdalna GRS), pod warunkiem, że niepomyślne odczytać danych z obszaru podstawowego są po ponownych próbach na pomocniczym region.

Firma Microsoft gwarantuje, że co najmniej 99,9% (99% dla atrakcyjne poziom dostępu) czasu, firma Microsoft będzie pomyślnie przetwarzać żądania odczytywanie danych lokalnie zbędne przestrzeni dyskowej (LRS), strefy zbędne przestrzeni dyskowej (ZRS) i konta Geo zbędne przestrzeni dyskowej (GRS).

Firma Microsoft gwarantuje, że co najmniej 99,9% (99% dla atrakcyjne poziom dostępu) czasu, firma Microsoft będzie pomyślnie przetwarzać żądania zostać zapisane lokalnie zbędne przestrzeni dyskowej (LRS), strefy zbędne przestrzeni dyskowej (ZRS) i konta Geo zbędne przestrzeni dyskowej (GRS) i dostęp do odczytu Geo zbędne miejsca do magazynowania (Pomoc Zdalna GRS).

- [Azure Umowa dotycząca poziomu usług dla miejsca do magazynowania](https://azure.microsoft.com/support/legal/sla/storage/v1_1/)


## <a name="regions"></a>Obszary

Azure jest zazwyczaj dostępny w 30 regionów świata i ogłosi plany dla 4 dodatkowe regiony. Geograficzne rozszerzeń jest priorytet Azure, ponieważ umożliwia klientom uzyskać większą wydajność i obsługuje on ich wymagania i preferencji dotyczących danych lokalizacji.  Region najnowszą azures uruchomić znajduje się w Niemcy.

Niemcy Microsoft Cloud zawiera zróżnicowane opcja z usługami Microsoft Cloud już dostępne w Europie, tworzenia większe możliwości innowacje i ekonomicznych wzrostu dla wysoce regulowanym partnerów i klientów w Niemcy, Unii Europejskiej (Unii Europejskiej) i Europejska skojarzenia wolnego handlu (EFTA).

Dane klientów w tych nowych centrach danych w Magdeburgu i Frankfurt, odbywa się pod kontrolą zarządcy danych, międzynarodowe systemy T, niezależna firma niemieckim i podległej Telekom niemieckich. Usługi komercyjne chmury firmy Microsoft w tych centrach danych zgodny z danymi niemieckim obsługi przepisów i nadaj klientów dodatkowe opcje przedstawiający sposób i miejsce, w którym dane są przetwarzane.


- [Mapa regionów Azure](https://azure.microsoft.com/regions/)

## <a name="security"></a>Zabezpieczenia

Magazyn Azure oferuje pełnego zestawu funkcji zabezpieczeń, umożliwiające programistom na tworzenie bezpiecznych aplikacji. Samo konto miejsca do magazynowania mogą być chronione za pomocą kontrola dostępu oparta na rolach i usługi Azure Active Directory. Dane mogą być chronione podczas przesyłania między aplikacją i Azure za pomocą szyfrowania po stronie klienta, HTTPS lub SMB 3.0. Dane można ustawić do automatycznie szyfrowane zapisywane magazyn Azure za pomocą przestrzeni dyskowej usługi szyfrowania SSE (). Można ustawić dysków systemu operacyjnego i dane używane przez maszyn wirtualnych zaszyfrowana przy użyciu szyfrowania dysku Azure. Delegowanej dostęp do obiektów danych w magazynie Azure można udzielić używania podpisów dostępu udostępnione.

### <a name="management-plane-security"></a>Zarządzanie płaszczyzny zabezpieczeń

Płaszczyzny zarządzania składa się z zasobów służące do zarządzania kontem przestrzeni dyskowej. W tej sekcji będzie porozmawiamy o model wdrożenia Menedżera zasobów Azure i jak używać funkcji Kontrola dostępu oparta na rolach (RBAC) do kontrolowania dostępu do konta miejsca do magazynowania. Firma Microsoft będzie także porozmawiać o zarządzaniu kluczy konta miejsca do magazynowania i jak je odtworzyć.

### <a name="data-plane-security"></a>Bezpieczeństwo płaszczyzny danych

W tej sekcji opisano zezwolenia na dostęp do obiektów rzeczywistych danych na swoim koncie miejsca do magazynowania, takich jak obiektów blob, pliki kolejek i tabele, udostępniane podpisom dostępu i przechowywane zasady dostępu. Omówimy następujące zagadnienia zarówno skojarzeń zabezpieczeń poziomu usług i skojarzenia konta poziom zabezpieczeń. Widzimy będzie również sposobu ograniczyć dostęp do określonego adresu IP (lub zakres adresów IP), jak ograniczyć protokół używany do HTTPS i odwoływanie podpisu dostępu udostępnione nie czekając na wygaśnie.

## <a name="encryption-in-transit"></a>Szyfrowanie w drodze

W tej sekcji omówiono sposób zabezpieczania danych podczas przenoszenia do lub poza magazyn Azure. Będzie porozmawiamy o zalecane używanie HTTPS i szyfrowania używany przez SMB 3.0 dla Azure udziałach plików. Firma Microsoft będzie także zapoznaj się szyfrowanie po stronie klienta, co umożliwia szyfrowanie danych, zanim zostanie przekazany do miejsca do magazynowania w aplikacji klienckiej oraz odszyfrowywanie danych po przeniesieniu Brak miejsca.

## <a name="encryption-at-rest"></a>Szyfrowanie w pozostałych

Będzie porozmawiamy o przestrzeni dyskowej usługi szyfrowania SSE () oraz udostępnianie go dla konta miejsca do magazynowania, uzyskując z obiektami blob blok blob strony i dołączanie obiektów blob automatycznie są szyfrowane zapisywane magazyn Azure. Możemy również będzie wyglądać w sposób użycia szyfrowania dysku Azure i Poznaj podstawowe różnice i przypadkach szyfrowania dysku i SSE i szyfrowania po stronie klienta. Firma Microsoft krótko będzie wyglądać na FIPS zgodności dla komputerów Rządu Stanów Zjednoczonych.

- [Przewodnik po zabezpieczeniach Azure miejsca do magazynowania](../storage/storage-security-guide.md)

## <a name="cost-savings"></a>Oszczędności kosztów

- [Koszt magazynowania](https://azure.microsoft.com/pricing/details/storage/)

- [Kalkulator kosztu magazynowania](https://azure.microsoft.com/pricing/calculator/?service=storage)

## <a name="storage-limits"></a>Ograniczenia dotyczące magazynowania

- [Ograniczenia dotyczące magazynowania usługi](../azure-subscription-service-limits.md#storage-limits)
