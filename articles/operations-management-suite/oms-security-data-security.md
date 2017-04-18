<properties
   pageTitle="Operacje zarządzania pakietu zabezpieczeń i zabezpieczania danych rozwiązanie inspekcji | Microsoft Azure"
   description="W tym dokumencie omówiono sposobu zarządzania i w operacji zarządzania pakietu zabezpieczeń i rozwiązanie inspekcji danych."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="yurid"/>

# <a name="operations-management-suite-security-and-audit-solution-data-security"></a>Operacje zarządzania pakietu zabezpieczeń i inspekcji zabezpieczania danych rozwiązanie

Aby ułatwić klientom zapobiegania, wykrywania i odpowiadanie na zagrożenia, [Zabezpieczenia pakietu usługi (OMS) zarządzania operacje i rozwiązanie inspekcji](operations-management-suite-overview.md) gromadzi i przetwarza dane na temat zasobów, która zawiera:

- Dziennik zdarzeń zabezpieczeń
- Zdarzenie zdarzenia śledzenia systemu Windows (ETW)
- Zdarzenia inspekcji zasad ograniczeń oprogramowania
- Dziennik zapory systemu Windows
- Zaawansowane analizy zagrożenia zdarzenia
- Wyniki oceny według planu bazowego
- Wyniki oceny ochrony przed złośliwym oprogramowaniem
- Wyniki oceny aktualizacji i poprawek
- Strumienie Syslogs, które zostały jawnie włączone agenta do

Udzielamy silnych zobowiązań ochrony prywatności i zabezpieczanie danych. Microsoft jest zgodny ściśle zabezpieczenia i zgodność z wytycznymi — z kodowania do działania usługi.
W tym artykule wyjaśniono, jak danych oraz zarządzania w zabezpieczeń usługi OMS i rozwiązanie inspekcji.

## <a name="data-sources"></a>Źródła danych

Zabezpieczenia usługi OMS i rozwiązanie inspekcji analizowania danych w środowisku maszyn wirtualnych systemu i komputerów zainstalowanego agenta usługi OMS. Zabezpieczenia usługi OMS i inspekcji rozwiązanie może zbierać informacje dotyczące zdarzenia zabezpieczeń, takich jak zdarzeń systemu Windows, dzienników inspekcji, dzienniki programu IIS i wiadomości syslog konfiguracji. Przykłady takich danych: typ systemu operacyjnego i wersji systemem procesów, nazwa komputera, adresy IP, rejestrowane w użytkownika i identyfikator dzierżawy.  

## <a name="data-protection"></a>Ochrona danych

**Podziału danych**: dane są przechowywane logicznie oddzielnie dla każdego składnika w całej usłudze. Wszystkie dane są oznakowane dla organizacji. Znakowanie ten będzie nadal występował w całym cyklu życia danych, a została wprowadzona w każdej warstwie usługi. 

**Dostęp do danych**: Aby podać zalecenia dotyczące zabezpieczeń i badanie potencjalnych zagrożeniach, personelowi firmy Microsoft mogą dostęp do informacji zbieranych lub analizowane przez usługi. Firma Microsoft zgodny [Postanowienia dotyczące usług Online firmy Microsoft](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) i [Zasady zachowania poufności informacji](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), których stan firmy Microsoft będzie nie korzystanie z danych klienta lub uzyskania informacji z niego w celach komercyjnych ogłoszeń i podobne. Aby zapewnić zalecenia dotyczące zabezpieczeń i badanie potencjalnych zagrożeniach, personelowi firmy Microsoft mogą dostęp do informacji zbieranych lub analizowane przez usługi. Firma Microsoft tylko przy użyciu klienta danych stosownie do potrzeb umożliwiają usługi Azure, w tym celów zgodny z ich dostarczania tych usług. Możesz zachować wszystkie prawa do własnych danych.

**Używanie danych**: Firma Microsoft używa wzorców i analizy zagrożenia widoczne przez kilka dzierżaw rozszerza możliwości naszych zapobiegania i wykrywania; Firma Microsoft zrobić zgodnie z zobowiązań prywatności opisanych w nasze [Zasady zachowania poufności informacji](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).

> [AZURE.NOTE] Lokalizacja danych jest skonfigurowany na poziomie usługi OMS obszaru roboczego, podczas tworzenia obszaru roboczego, który jest częścią początkowej proces konfiguracji zabezpieczeń usługi OMS i inspekcji.

## <a name="see-also"></a>Zobacz też

W tym dokumencie wiesz, jak zarządzać i w usługi OMS danych. Aby dowiedzieć się więcej na temat usługi OMS zabezpieczeń i inspekcji rozwiązanie, zobacz:

- [Przegląd operacji usługi zarządzania pakietu (OMS)](operations-management-suite-overview.md)
- [Monitorowanie i reagowanie na alerty zabezpieczeń w operacji zarządzania pakietu zabezpieczeń i rozwiązanie inspekcji](oms-security-responding-alerts.md)
- [Monitorowanie zasobów operacje zarządzania pakietu zabezpieczeń i rozwiązanie inspekcji](oms-security-monitoring-resources.md)

