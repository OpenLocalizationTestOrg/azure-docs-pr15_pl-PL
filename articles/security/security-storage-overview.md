<properties
   pageTitle="Omówienie zabezpieczeń Azure magazynowania | Microsoft Azure"
   description=" Azure magazyn jest masowej chmury zastosowaniach nowoczesny, zależne od wytrzymałości, dostępność i skalowalność na potrzeby swoich klientów. Ten artykuł zawiera omówienie podstawową funkcji Azure zabezpieczeń, które mogą być używane z magazyn Azure. "
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
   ms.date="09/16/2016"
   ms.author="terrylan"/>

# <a name="azure-storage-security-overview"></a>Omówienie zabezpieczeń Azure miejsca do magazynowania

Azure magazyn jest masowej chmury nowoczesne aplikacje, które korzystają z wytrzymałości, dostępność i skalowalność na potrzeby swoich klientów. Magazyn Azure oferuje pełnego zestawu funkcji zabezpieczeń:

- Konta magazynu mogą być chronione za pomocą kontrola dostępu oparta na rolach i usługi Azure Active Directory.
- Dane mogą być chronione podczas przesyłania między aplikacją i Azure za pomocą szyfrowania po stronie klienta, HTTPS lub SMB 3.0.
- Dane można ustawić do automatycznie szyfrowane zapisywane magazyn Azure używa szyfrowania usługi miejsca do magazynowania.
- Można ustawić dysków systemu operacyjnego i dane używane przez maszyn wirtualnych zaszyfrowana przy użyciu szyfrowania dysku Azure.
- Delegowanej dostęp do obiektów danych w magazynie Azure można udzielić używania podpisów dostępu udostępnione.
- Metoda uwierzytelniania używana przez osobę, gdy będą uzyskiwać dostęp do miejsca do magazynowania mogą być śledzone przy użyciu analizy miejsca do magazynowania.

Bardziej szczegółowe przyjrzeć zabezpieczeń w magazynie Azure zobacz [Przewodnik po zabezpieczeniach magazyn Azure](../storage/storage-security-guide.md). Ten przewodnik zawiera dive głębokości do funkcji zabezpieczeń magazyn Azure takich jak klawiszy konta miejsca do magazynowania, szyfrowania danych podczas przesyłania i odpoczynku i analizy miejsca do magazynowania.

Ten artykuł zawiera omówienie funkcji zabezpieczeń Azure, których można używać z nośnikami Azure. Łącza są dostarczane do artykułów zawierających szczegółowy opis poszczególnych funkcji, możesz dowiedzieć się więcej.

Poniżej przedstawiono podstawowe funkcje powinny być zawarte w tym artykule:

- Kontrola dostępu oparta na rolach
- Delegowanej dostęp do przechowywania obiektów
- Szyfrowanie w drodze
- Szyfrowanie w pozostałych/miejsca do magazynowania szyfrowanie usługi
- Szyfrowanie dysków Azure
- Azure klucza magazynu

## <a name="role-based-access-control-rbac"></a>Kontrola dostępu oparta na rolach (RBAC)

Można zabezpieczyć swoje konto miejsca do magazynowania w kontrola dostępu oparta na rolach (RBAC). Ograniczanie dostępu w oparciu o zasady zabezpieczeń [wiedzieć](https://en.wikipedia.org/wiki/Need_to_know) i [jak najmniejszych uprawnień](https://en.wikipedia.org/wiki/Principle_of_least_privilege) jest konieczne dla organizacji, które mają być wymuszania zasad zabezpieczeń, aby uzyskać dostęp do danych. Te uprawnienia są udzielane przez przypisywanie odpowiednią rolę RBAC do grup i wniosków określonego zakresu. Za pomocą [wbudowanych role RBAC](../active-directory/role-based-access-built-in-roles.md), takich jak współpracownik konta przestrzeni dyskowej, aby przypisać uprawnienia do użytkowników.

Dowiedz się więcej:

- [Kontrola dostępu oparta na rolach Azure Active Directory](../active-directory/role-based-access-control-configure.md)

## <a name="delegated-access-to-storage-objects"></a>Delegowanej dostęp do przechowywania obiektów

Podpis udostępniania (SA) udostępnia delegowanej zasobów na koncie miejsca do magazynowania. Skojarzenia zabezpieczeń oznacza, że można udzielać że klient ograniczone uprawnienia do obiektów na koncie miejsca do magazynowania przez określony czas i określony zestaw uprawnień. Tych ograniczone uprawnień można udzielać bez konieczności udostępniania kluczy dostępu do konta. Jest identyfikatora URI, który obejmuje w jego parametry kwerendy wszystkie informacje niezbędne dla uwierzytelniony dostęp do zasobu miejsca do magazynowania. Dostęp do miejsca do magazynowania zasobów z skojarzeń zabezpieczeń, klienta musi tylko w celu przekazania w skojarzeń zabezpieczeń do odpowiedniego konstruktora lub metody.

Dowiedz się więcej:

- [Opis modelu skojarzenia zabezpieczeń](../storage/storage-dotnet-shared-access-signature-part-1.md)
- [Tworzenie i używanie skojarzeń zabezpieczeń z magazynem obiektów Blob](../storage/storage-dotnet-shared-access-signature-part-2.md)

## <a name="encryption-in-transit"></a>Szyfrowanie w drodze
Szyfrowanie w drodze jest mechanizmu ochrony danych podczas ich przesyłania w sieciach. Magazyn Azure można zabezpieczyć przy użyciu danych:

- [Poziomu transportu szyfrowania](../storage/storage-security-guide.md#encryption-in-transit), takich jak HTTPS podczas przesyłania danych do lub poza magazyn Azure.
- [Szyfrowanie drutu](../storage/storage-security-guide.md#using-encryption-during-transit-with-azure-file-shares), na przykład szyfrowanie SMB 3.0 Azure udziałach plików.
- [Szyfrowania po stronie klienta](../storage/storage-security-guide.md#using-client-side-encryption-to-secure-data-that-you-send-to-storage), do szyfrowania danych przed jego przeniesieniem do magazynu i odszyfrowywanie danych po przeniesieniu Brak miejsca.

Dowiedz się więcej na temat szyfrowania po stronie klienta:

- [Szyfrowanie po stronie klienta dla magazynu platformy Microsoft Azure](https://blogs.msdn.microsoft.com/windowsazurestorage/2015/04/28/client-side-encryption-for-microsoft-azure-storage-preview/)
- [Zabezpieczenia chmury sterują serii: szyfrowania danych podczas przesyłania](http://blogs.microsoft.com/cybertrust/2015/08/10/cloud-security-controls-series-encrypting-data-in-transit/)

## <a name="encryption-at-rest"></a>Szyfrowanie w pozostałych

W przypadku wielu organizacji [szyfrowanie danych spoczynku](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) jest obowiązkowe krok w kierunku prywatność danych, zgodności i suwerenności danych. Istnieją trzy Azure funkcje, które zapewniają szyfrowania danych, który jest "w stanie spoczynku":

- [Szyfrowanie usługi przechowywania](../storage/storage-security-guide.md#encryption-at-rest) pozwala na żądanie, że Usługa magazynu automatycznie szyfrować danych podczas zapisywania go do magazynu Azure.
- [Szyfrowania po stronie klienta](../storage/storage-security-guide.md#client-side-encryption) są także funkcję szyfrowania spoczynku.
- [Szyfrowanie dysków Azure](../storage/storage-security-guide.md#using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines) umożliwia szyfrowanie dyski systemu operacyjnego i dane używane przez maszyn wirtualnych IaaS.

Dowiedz się więcej na temat szyfrowanie usługi miejsca do magazynowania:

- [Szyfrowanie usługi Azure miejsca do magazynowania](https://azure.microsoft.com/services/storage/) jest dostępne dla [Magazyn obiektów Blob platformy Azure](https://azure.microsoft.com/services/storage/blobs/). Aby uzyskać szczegółowe informacje na innych typów Azure miejsca do magazynowania zobacz [plik](https://azure.microsoft.com/services/storage/files/), [dysku (magazynowanie Premium)](https://azure.microsoft.com/services/storage/premium-storage/), [tabeli](https://azure.microsoft.com/services/storage/tables/)i [kolejki](https://azure.microsoft.com/services/storage/queues/).
- [Szyfrowanie usługi Azure miejsca do magazynowania danych w pozostałych](../storage/storage-service-encryption.md)

## <a name="azure-disk-encryption"></a>Szyfrowanie dysków Azure

Azure szyfrowanie dysków wirtualnych maszyn ułatwia adres organizacji bezpieczeństwa i zgodności z przepisami, ponieważ do szyfrowania dysków maszyn wirtualnych (w tym dysków uruchamiania i danych) przy użyciu klawiszy i zasady, które kontrolę w [Azure klucza magazynu](https://azure.microsoft.com/services/key-vault/).

Szyfrowanie dysków maszyny wirtualne działa w systemach operacyjnych Windows i Linux. Używa też klucza magazynu ułatwiające zabezpieczenia, zarządzanie i inspekcja użycia kluczy szyfrowania dysku. Wszystkie dane w dysków maszyn wirtualnych jest szyfrowane na pozostałych przy użyciu technologii szyfrowania przemysłowym kont magazyn Azure. Rozwiązanie szyfrowania dysku dla systemu Windows jest oparty na [Microsoft dysków funkcją BitLocker](https://technet.microsoft.com/library/cc732774.aspx)i rozwiązanie Linux jest oparty na [kryptograficznego dm](https://en.wikipedia.org/wiki/Dm-crypt).

Dowiedz się więcej:

- [Szyfrowanie dysków Azure dla systemu Windows i Linux oraz IaaS maszyn wirtualnych](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)

## <a name="azure-key-vault"></a>Azure klucza magazynu

Azure szyfrowanie dysków używa [Magazynu klucza Azure](https://azure.microsoft.com/services/key-vault/) ułatwiające kontrolowanie i zarządzanie nimi kluczy szyfrowania dysku i hasła w ramach subskrypcji klucza magazynu, zapewniając, że wszystkie dane w dyski maszyn wirtualnych są szyfrowane na pozostałych w magazynie Azure. Za pomocą klucza magazynu należy inspekcji kluczy i użycia zasad.

Dowiedz się więcej:

- [Co to jest Azure klucza magazynu?](../key-vault/key-vault-whatis.md)
- [Rozpoczynanie pracy z magazynu klucza Azure](../key-vault/key-vault-get-started.md)
