<properties
    pageTitle="Często zadawane pytania dotyczące usługi cloud | Microsoft Azure"
    description="Często zadawane pytania dotyczące usług w chmurze."
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="adegeo"/>

# <a name="cloud-services-faq"></a>Często zadawane pytania dotyczące usług w chmurze
Ten artykuł zawiera odpowiedzi na niektóre często zadawane pytania dotyczące usługi Microsoft Azure Cloud Services. Możesz również odwiedzić [Często zadawane pytania dotyczące obsługi Azure](http://go.microsoft.com/fwlink/?LinkID=185083) ogólnych informacji Azure cen i obsługa techniczna. [Rozmiar pamięci Wirtualnej usług w chmurze strony](cloud-services-sizes-specs.md) można również uzyskać informacje o rozmiarze.

## <a name="certificates"></a>Certyfikaty

### <a name="where-should-i-install-my-certificate"></a>Gdzie zainstalować certyfikatu?

- **Moje**  
Aplikacja certyfikat z kluczem prywatnym (\*PFX, \*.p12).

- **URZĄD CERTYFIKACJI**  
Wszystkie pośrednie certyfikaty Przejdź w tym magazynie (zasad i urzędów certyfikacji Sub).

- **GŁÓWNY**  
Główny urząd certyfikacji zapisywane, dzięki czemu certyfikat urzędu certyfikacji do głównego należy przejdź tutaj.

### <a name="i-cant-remove-expired-certificate"></a>Nie można usunąć wygasły certyfikat

Azure zapobiega usuwaniu certyfikatu, gdy jest on używany. Musisz usunąć wdrożenia, który będzie używał certyfikatu lub aktualizowanie wdrażania za pomocą certyfikatu innego lub ponownego.

### <a name="delete-an-expired-certificate"></a>Usuwanie wygasł

Certyfikat nie jest używany, używasz polecenia cmdlet programu PowerShell [AzureCertificate Usuń](https://msdn.microsoft.com/library/azure/mt589145.aspx) do Usuwanie certyfikatu.

### <a name="i-have-expired-certificates-named-windows-azure-service-management-for-extensions"></a>Wygasła certyfikatów dla rozszerzeń o nazwie Zarządzanie usługą Windows Azure

Te certyfikaty są tworzone przy każdym rozszerzenie zostanie dodane do usługi w chmurze takich jak rozszerzenia pulpitu zdalnego. Te certyfikaty są używane tylko do szyfrowania i odszyfrowywania prywatne konfiguracji rozszerzenia. Nie ma znaczenia, jeśli te certyfikaty wygaśnie. Data wygaśnięcia nie jest zaznaczone.

### <a name="certificates-i-have-deleted-keep-reappearing"></a>Certyfikaty, które zostały usunięte pojawia się ponownie.

Pojawia się one najprawdopodobniej ponownie ze względu na narzędzie, którego używasz, takie jak Visual Studio. Po ponownym połączeniu z narzędziem używa certyfikatu, ponownie zostanie przekazany Azure.

### <a name="my-certificates-keep-disappearing"></a>Zachowaj znika certyfikatów

Gdy odtwarza wystąpienie maszyn wirtualnych, wszystkie lokalne zmiany zostaną utracone. [Uruchamianie zadania](cloud-services-startup-tasks.md) należy zainstalować certyfikaty maszyn wirtualnych każdym uruchomieniu roli.

### <a name="i-cannot-find-my-management-certificates-in-the-portal"></a>Nie mogę znaleźć moje certyfikaty zarządzania w portalu

[Certyfikaty zarządzania](..\azure-api-management-certs.md) są dostępne tylko w portalu klasyczny Azure. Bieżący portal Azure nie korzysta z certyfikatów zarządzania. 

### <a name="how-can-i-disable-a-management-certificate"></a>Jak wyłączyć certyfikatu zarządzania?

Nie można wyłączyć [certyfikaty zarządzania](..\azure-api-management-certs.md) . Możesz usunąć je za pośrednictwem portalu klasyczny Azure po nie mają być już używane.

### <a name="how-do-i-create-an-ssl-certificate-for-a-specific-ip-address"></a>Jak utworzyć certyfikat SSL dla określonego adresu IP?

Postępuj zgodnie z instrukcjami [Tworzenie samouczek certyfikatu](cloud-services-certs-create.md). Użyj adresu IP jako nazwy DNS.

## <a name="security"></a>Zabezpieczenia

### <a name="disable-ssl-30"></a>Wyłączanie SSL 3.0

Aby wyłączyć SSL 3.0 i używanie zabezpieczeń TLS, utworzyć zadania uruchamiania, które podano na ten wpis w blogu: https://azure.microsoft.com/en-us/blog/how-to-disable-ssl-3-0-in-azure-websites-roles-and-virtual-machines/

## <a name="scale-a-cloud-service"></a>Skala usługi w chmurze

### <a name="i-cannot-scale-beyond-x-instances"></a>Nie mogę przeskalować poza X wystąpień

Twoja subskrypcja Azure ma limit liczby rdzeni, których można używać. Skalowanie nie będzie działać, jeśli użyto rdzenie dostępne. Na przykład jeśli masz limit 100 rdzeni, oznacza to, może mieć wystąpienia maszyn wirtualnych A1 o rozmiarze 100 dla usługi w chmurze lub wystąpienia maszyn wirtualnych o rozmiarze 50 A2.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

### <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>Nie można zarezerwować ilości w usłudze w chmurze VIP wielu adresów IP

Najpierw upewnij się, że wystąpienie maszyn wirtualnych, którego próbujesz rezerwowanie dla adresu IP jest włączony. Drugie upewnij się, że używasz zastrzeżone adresy IP dla odblokowane wdrożeń roboczych i produkcyjnych. **Nie** należy zmienić ustawienia, podczas uaktualniania jest wdrożenie.

