<properties
   pageTitle="Wymagania wstępne dotyczące wdrażania ExpressRoute | Microsoft Azure"
   description="Ta strona zawiera listę wymagań, które powinny być spełnione, zanim zostanie zamówień obwód Azure ExpressRoute."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>


# <a name="expressroute-prerequisites--checklist"></a>Wymagania wstępne dotyczące ExpressRoute i Lista kontrolna  

Aby połączyć się z usługami w chmurze firmy Microsoft przy użyciu ExpressRoute, musisz sprawdzić, czy zostały spełnione następujące wymagania wymienione w poniższych sekcjach.

[AZURE.INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

## <a name="azure-account"></a>Konto Azure

- Konto Microsoft Azure prawidłowe i aktywne. Jest to wymagane do instalacji obwód ExpressRoute. Obwody ExpressRoute są zasoby w ramach subskrypcji Azure. Azure subskrypcji jest wymagane, nawet jeśli połączenie jest ograniczone do usługami w chmurze Azure Microsoft, takich jak usługi Office 365 i CRM w trybie online.
- Aktywne subskrypcji usługi Office 365 (Jeśli za pomocą usługi Office 365). Zobacz sekcję [wymagania określone usługi Office 365](#office-365-specific-requirements) tego artykułu, aby uzyskać więcej informacji.

## <a name="connectivity-provider"></a>Dostawca połączeń
- Możesz pracować z [partnerem łączności ExpressRoute](expressroute-locations.md#partners) , nawiązywanie połączenia z chmury firmy Microsoft. Możesz skonfigurować połączenie między sieci lokalnej i w programie Microsoft na [trzy sposoby](expressroute-introduction.md#howtoconnect). 
- Jeśli dostawca nie jest partnerem łączności ExpressRoute, możesz nadal łączyć w chmurze firmy Microsoft za pośrednictwem [dostawcy programu exchange w chmurze](expressroute-locations.md#nonpartners).

## <a name="network-requirements"></a>Wymagania sieci
- **Łączność zbędne**: nie jest wymagane nadmiarowości na fizycznie łączność między użytkownikiem a dostawcy. Microsoft wymagają zbędne sesje BGP zostać skonfigurowane między routerami firmy Microsoft i peering routery, nawet wtedy, gdy masz tylko [jedną fizycznego połączenia exchange chmury](expressroute-faqs.md#onep2plink). 
- **Routing**: w zależności od tego, jak możesz połączyć się z programu Microsoft Cloud, dostawcy potrzebować konfigurowania i zarządzać sesjami BGP dla [domen routingu](expressroute-circuit-peerings.md). Niektóre dostawcą łączności Ethernet lub dostawca programu exchange w chmurze mogą oferować zarządzania BGP jako wartość Dodaj usługę.
- **Translatora adresów Sieciowych**: Microsoft akceptuje tylko publicznych adresów IP za pośrednictwem zaglądanie firmy Microsoft. Jeśli korzystasz z prywatnych adresów IP w sieci lokalnej, możesz lub Twoim potrzebom dostawcy do tłumaczenia prywatnych adresów IP w celu publiczny adres IP adresy [przy użyciu translatora adresów Sieciowych](expressroute-nat.md).
- **QoS**: Skype dla firm obejmuje różne usługi (np. głos, wideo i tekst), które wymagają zróżnicowane traktowanie QoS. Możesz i dostawcy powinien wykonaj [wymagania QoS](expressroute-qos.md).
- **Zabezpieczenia sieci**: [Zabezpieczenia sieci](../best-practices-network-security.md) należy rozważyć podczas nawiązywania połączenia Cloud Microsoft za pośrednictwem ExpressRoute.
 
## <a name="office-365"></a>Usługi Office 365

Jeśli planujesz włączyć usługi Office 365 na ExpressRoute, sprawdź następujące dokumenty, aby uzyskać więcej informacji o wymaganiach usługi Office 365.


- [Omówienie ExpressRoute dla usługi Office 365](https://support.office.com/en-us/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd)
- [Routing z ExpressRoute dla usługi Office 365](https://support.office.com/en-us/article/Routing-with-ExpressRoute-for-Office-365-e1da26c6-2d39-4379-af6f-4da213218408)
- [Usługa Office 365 adresy URL i zakresy adresów IP](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
- [Planowanie sieci i dostosowywanie wydajności dla usługi Office 365](https://support.office.com/en-us/article/Network-planning-and-performance-tuning-for-Office-365-e5f1228c-da3c-4654-bf16-d163daee8848)
- [Narzędzia i kalkulatory przepustowości sieci](https://support.office.com/en-us/article/Network-and-migration-planning-for-Office-365-f5ee6c33-bcd7-4b0b-b0f8-dc1d9fb8d132)
- [Integracja usługi Office 365 ze środowiskami lokalnymi](https://support.office.com/en-us/article/Office-365-integration-with-on-premises-environments-263faf8d-aa21-428b-aed3-2021837a4b65)

## <a name="crm-online"></a>CRM w trybie Online 
Jeśli planujesz umożliwiające CRM Online na ExpressRoute, sprawdź następujące dokumenty, aby uzyskać więcej informacji na temat CRM Online

- [CRM Online adresy URL](https://support.microsoft.com/kb/2655102) i [zakresy adresów IP](https://support.microsoft.com/kb/2728473)

## <a name="next-steps"></a>Następne kroki

- Aby uzyskać więcej informacji o ExpressRoute zobacz [Często zadawane pytania dotyczące ExpressRoute](expressroute-faqs.md).
- Znajdowanie dostawcy usługi wiadomości łączności ExpressRoute. Zobacz [ExpressRoute partnerów i zaglądanie lokalizacji](expressroute-locations.md).
- Sprawdź wymagania dotyczące [routingu](expressroute-routing.md), [translatora adresów Sieciowych](expressroute-nat.md) i [QoS](expressroute-qos.md).
- Konfigurowanie połączenia ExpressRoute.
    - [Tworzenie obwodu ExpressRoute](expressroute-howto-circuit-classic.md)
    - [Konfigurowanie routingu](expressroute-howto-routing-classic.md)
    - [Łącze VNet obwód ExpressRoute](expressroute-howto-linkvnet-classic.md)

