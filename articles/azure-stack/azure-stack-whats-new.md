<properties
    pageTitle="Co nowego w stos Azure | Microsoft Azure"
    description="Co nowego w stos Azure"
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="helaw"/>

# <a name="whats-new-in-azure-stack-technical-preview-2"></a>Co nowego w Azure stos Technical Preview 2
Ta wersja oferuje nowe funkcje dla dzierżaw i administratorów.

## <a name="network"></a>Sieci   
   - [IDN](azure-stack-understanding-dns-in-tp2.md) zawiera rejestracji nazwę sieci wewnętrznej i rozpoznawania systemu DNS (Domain Name) bez dodatkowych infrastruktury DNS.
   - [Wirtualna sieć bram](azure-stack-create-vpn-connection-one-node-tp2.md) umożliwiają VPN łączności Azure lub lokalnych zasobów.
   - Trasy zdefiniowane przez użytkownika mogą przesyłać ruch sieciowy za pośrednictwem zapory, zabezpieczenia innych urządzeń i innych usług.
   - Możesz utworzyć zasobów sieciowych z witryny Marketplace.   

## <a name="storage"></a>Miejsca do magazynowania
 - [Kolejki Azure](https://msdn.microsoft.com/library/dd179353.aspx) Włącz zaufanego i trwałych wiadomości.
 - Dane dotyczące wydajności miejsca do magazynowania przechwytywania [analizy miejsca do magazynowania](https://msdn.microsoft.com/library/azure/hh343270.aspx) . Za pomocą tych danych do śledzenia żądania, analizowania trendów zastosowania i diagnozowanie problemów za pomocą konta miejsca do magazynowania.
 - Magazyn obiektów blob obsługuje [Blok operację dołączania](https://msdn.microsoft.com/library/azure/mt427365.aspx).
 - Pomoc dotycząca konta interfejsu API magazynu Premium.
 - Dzierżawa Obsługa usługi miejsca do magazynowania dla typowych narzędzi i SDK, takich jak Azure interfejsu wiersza polecenia programu PowerShell, .NET, Python i Java SDK. 
 - [Podpis dostępu do udostępnionego konta miejsca do magazynowania](https://msdn.microsoft.com/library/azure/mt584140.aspx) włączyć szczegółowego Delegowanie dostępu do usług miejsca do magazynowania bez konieczności udostępnić swój klucz konta pełny.  
 - Usługi Magazyn teraz korzystać [Grupy zarządzanych kont usług](https://technet.microsoft.com/library/hh831477.aspx) dla silne zabezpieczenia z Zarządzanie niski.
 - Można odzyskać dzierżawy nieużywane zasoby na żądanie.
 - Długość przechowywania konta usunięte miejsca do magazynowania jest konfigurowany.
 - Można odzyskać usunięte dzierżawy miejsca do magazynowania konta.

## <a name="compute"></a>Obliczenia
- Można cofnąć maszyn wirtualnych.
- Można ponownie wdróż rozszerzenia maszyn wirtualnych na potrzeby zarządzania Rozwiązywanie problemów i konfiguracji.
- Możesz zmienić rozmiar maszyn wirtualnych dysków.
- Maszyn wirtualnych może zawierać wiele interfejsów sieciowych.

### <a name="portal-experience"></a>Środowisko portalu
 - Azure regiony stos są jednostka logiczna skali i zarządzanie w stos Azure. W tym podglądzie umożliwia wyświetlanie informacji na usługi, takie jak obliczeń, sieci i przestrzeni dyskowej według regionów.
 - Teraz można wyświetlić podgląd interfejsu [azure stos updates.md] (aktualizacje).

## <a name="key-vault"></a>Kluczowe magazynu
- [Klucz magazynu w stos Azure](azure-stack-kv-intro.md) umożliwia zarządzanie bezpiecznego klawiszy i haseł aplikacje w chmurze.
- Możesz inspekcji i monitorowanie użycia klucza za pośrednictwem SMS i aplikacji.

## <a name="billing-and-usage"></a>Rozliczenia i zastosowania
 - [Rozliczenia i zużycie interfejsy API](azure-stack-billing-and-chargeback.md) uwidaczniają danych, w jaki sposób są używane usługi.  
 - Delegowanej dostawców włączyć odsprzedawców klientom usługi Azure stos ich.

## <a name="monitoring-and-health"></a>Monitorowanie i kondycji
 - Nowe funkcje monitorowania dostępne w portalu oraz interfejsy API umożliwiają wyprzedzeniem wyświetlanie alertów i zarządzanie nimi w środowisku usługi.  

## <a name="next-steps"></a>Następne kroki
- [Opis architektury Zapewnić stos Azure](azure-stack-architecture.md)      
- [Opis warunki wstępne wdrażania](azure-stack-deploy.md)
- [Wdrażanie stos Azure](azure-stack-run-powershell-script.md)

  
