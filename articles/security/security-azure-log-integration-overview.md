<properties
   pageTitle="Wprowadzenie do integracji dziennika Microsoft Azure | Microsoft Azure"
   description="Informacje na temat integracji Azure dziennika, klucza możliwości i sposób działania."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TerryLanfear"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/24/2016"
   ms.author="TomSh"/>

# <a name="introduction-to-microsoft-azure-log-integration-preview"></a>Wprowadzenie do integracji dziennika Microsoft Azure (wersja Preview)

Informacje na temat integracji Azure dziennika, klucza możliwości i sposób działania.

## <a name="overview"></a>Omówienie

Platformy jako usługa (PaaS) i infrastruktury jako usługa (IaaS) obsługiwany w Azure generować dużych ilości danych z dzienników zabezpieczeń. Te dzienniki zawierają ważnych informacji, który może dostarczać analizy i zaawansowanych analiz do naruszenia zasad, zagrożeń wewnętrznych i zewnętrznych, problemy zgodności z przepisami i różnic w odniesieniu w sieci, hosta i aktywności użytkownika.

Integracja dziennika Azure umożliwia integrowanie nowych dzienników od Azure zasobów lokalnych systemów zabezpieczeń informacji i zdarzeń zarządzania (SIEM). Integracja Azure dziennika zbiera diagnostyki Azure z systemu Windows *(WAD)* maszyn wirtualnych, a także diagnostyki z rozwiązań partnerów takich jak zapory aplikacji sieci Web (WAF). Integracja jest ujednolicony pulpitu nawigacyjnego wszystkich trwałych, lokalnego lub w chmurze, dzięki czemu można agregacji, przeniesionym, analizowanie i powiadomienia dla zdarzenia zabezpieczeń.

![Integracja Azure dziennika][1]

## <a name="what-logs-can-i-integrate"></a>Jakie dzienniki można zintegrować?

Azure tworzy szczegółowe rejestrowanie dla każdej usługi Azure. Dzienniki te są posortowane według dwa główne typy:

- **Dzienniki kontroli i zarządzania nimi**, która zapewnia widoczność na operacje Azure Menedżer zasobów utworzyć, AKTUALIZOWANIE i usuwanie. Przykładem tego typu dziennika jest Azure dzienników inspekcji.
- **Dzienniki płaszczyzny danych**, która zapewnia widoczność do zdarzenia wywoływane w ramach zastosowania Azure zasobów. Przykłady tego typu dziennika są zdarzeń systemu Windows System zabezpieczeń, a dzienniki aplikacji w maszyny wirtualnej.

Integracja Azure dziennika obsługuje obecnie integracji dzienników inspekcji Azure, maszyn wirtualnych dzienników i alertów Centrum zabezpieczeń Azure.

Jeśli masz pytania dotyczące integracji dziennika Azure, Wyślij wiadomość e-mail do [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## <a name="next-steps"></a>Następne kroki

W tym dokumencie zostały wprowadzone do integracji Azure dziennika. Aby dowiedzieć się więcej na temat integracji dziennika Azure i typów dzienników obsługiwane, zobacz następujące artykuły:

- [Integracja dziennika Azure Microsoft Azure dzienników (wersja Preview)](https://www.microsoft.com/download/details.aspx?id=53324) — Centrum pobierania szczegóły, wymagania systemowe i instrukcji instalacji na integracji Azure dziennika.
- [Wprowadzenie do integracji Azure dziennika](security-azure-log-integration-get-started.md) — ten samouczek przeprowadzi Cię przez proces instalacji integracji dziennika Azure i integracji dzienników z Azure miejsca do magazynowania, dzienników inspekcji Azure i alertów Centrum zabezpieczeń.
- [Kroki konfiguracji partnera](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) — ten wpis w blogu pokazano, jak skonfigurować integrację Azure dziennika do pracy z rozwiązań partnerów Splunk, HP ArcSight i IBM QRadar.
- [Azure dziennika integracji często zadawane pytania](security-azure-log-integration-faq.md) — ten często zadawane pytania dotyczące odpowiedzi na pytania dotyczące integracji Azure dziennika.
- [Alerty Centrum zabezpieczeń Integracja z Azure logowania integracji](../security-center/security-center-integrating-alerts-with-log-integration.md) — ten dokument pokazano, jak zsynchronizować alertów Centrum zabezpieczeń oraz zdarzeń zabezpieczeń maszyn wirtualnych zebranych przez Azure narzędzia diagnostyczne i dzienników inspekcji Azure, z dziennika analizy lub rozwiązanie SIEM.
- [Nowe funkcje dla Azure narzędzia diagnostyczne i dzienników inspekcji Azure](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) — ten wpis w blogu poznasz dzienników inspekcji Azure i inne funkcje, które ułatwiają uzyskanie wniosków na operacje Azure zasobów.

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png
