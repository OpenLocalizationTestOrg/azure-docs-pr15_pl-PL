<properties
   pageTitle="Ochrona usługa Azure SQL w Centrum zabezpieczeń Azure | Microsoft Azure"
   description="Adresy ten dokument, zalecenia w Centrum zabezpieczeń Azure, które pomagają chronić usługa Azure SQL i pozostanie zgodnie z zasadami zabezpieczeń."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/04/2016"
   ms.author="terrylan"/>

# <a name="protecting-azure-sql-service-in-azure-security-center"></a>Ochrona usługa Azure SQL w Centrum zabezpieczeń Azure

Centrum zabezpieczeń Azure analizuje stan zabezpieczeń Azure zasobów. Gdy Centrum zabezpieczeń identyfikuje słabych zabezpieczeń, tworzy zalecenia, które przeprowadzają użytkownika przez proces konfigurowania potrzebnych formantów.  Zalecenia dotyczą typów zasobów Azure: wirtualnych maszyn, sieciowych SQL i aplikacje.

Ten artykuł dotyczy zalecenia, które dotyczą usługa Azure SQL.  Centrum zalecenia dotyczące usług SQL Azure wokół włączenie inspekcji dla serwerów Azure SQL i baz danych i włączanie szyfrowanie bazy danych SQL.  Aby ułatwić zrozumienie dostępne rekomendacje usług SQL i co każdego z nich zrobić w przypadku zastosowania za pomocą poniższej tabeli jako odwołanie.

## <a name="available-sql-service-recommendations"></a>Dostępne rekomendacje usług SQL

|Zalecenia|Opis|
|-----|-----|
|[Włączanie Inspekcja programu SQL server](security-center-enable-auditing-on-sql-servers.md)|Zaleca się włączanie inspekcji dla serwerów Azure SQL (usługa Azure SQL; nie zawiera SQL uruchomionych maszyn wirtualnych).|
|[Baza danych SQL inspekcji](security-center-enable-auditing-on-sql-databases.md)|Zaleca się włączanie inspekcji dla baz danych Azure SQL (usługa Azure SQL; nie zawiera SQL uruchomionych maszyn wirtualnych).|
|[Włącz przezroczyste szyfrowanie danych w bazach danych programu SQL](security-center-enable-transparent-data-encryption.md)|Zaleca się włączenie szyfrowanie bazy danych SQL (tylko usługa Azure SQL).|

## <a name="see-also"></a>Zobacz też

Aby dowiedzieć się więcej na temat zalecenia, które dotyczą innych typów zasobów Azure, zobacz następujące artykuły:

- [Ochrona maszyn wirtualnych w Centrum zabezpieczeń Azure](security-center-virtual-machine-recommendations.md)
- [Ochrona aplikacji w Centrum zabezpieczeń Azure](security-center-application-recommendations.md)
- [Ochrona sieci w Centrum zabezpieczeń Azure](security-center-network-recommendations.md)

Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

- [Ustawianie zasad zabezpieczeń w Centrum zabezpieczeń Azure](security-center-policies.md) — Dowiedz się, jak skonfigurować zasady zabezpieczeń dla usługi Azure subskrypcje i grup zasobów.
- [Zarządzanie notatkami i odpowiadanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md) — Dowiedz się, jak zarządzać i odpowiadanie na alerty zabezpieczeń.
- [Azure zabezpieczeń Centrum — często zadawane pytania](security-center-faq.md) — znajdowanie często zadawane pytania dotyczące korzystania z usługi.
