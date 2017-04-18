<properties
   pageTitle="Stosowanie szyfrowanie dysków w Centrum zabezpieczeń Azure | Microsoft Azure"
   description="Ten dokument pokazano, jak wykonania zalecenia Centrum zabezpieczeń Azure **Zastosuj szyfrowanie dysków**."
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
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="apply-disk-encryption-in-azure-security-center"></a>Stosowanie szyfrowanie dysków w Centrum zabezpieczeń Azure

Centrum zabezpieczeń Azure zaleca, zastosowania szyfrowania dysku, jeśli masz dyski systemu Windows i Linux oraz maszyn wirtualnych, które nie są szyfrowane przy użyciu szyfrowania dysku Azure. Szyfrowanie dysków umożliwia szyfrowanie dysków systemu Windows i Linux oraz IaaS w maszyn wirtualnych.  Szyfrowanie jest zalecane dla wielkości system operacyjny i danych na swojej maszyn wirtualnych.


Szyfrowanie dysków wykorzystuje branżowe standardowa funkcja [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) systemu Windows i funkcji [Kryptograficznego DM](https://en.wikipedia.org/wiki/Dm-crypt) Linux, aby zapewnić szyfrowanie danych i systemu operacyjnego, aby chronić i ochrona danych i spełniają Twojej organizacji, zabezpieczenia i zgodność zobowiązań. Szyfrowanie dysków jest zintegrowany z [Magazynu klucza Azure](https://azure.microsoft.com/documentation/services/key-vault/) ułatwiające kontrolowanie i zarządzanie nimi kluczy szyfrowania dysku i hasła w ramach subskrypcji magazynu klucza, zapewniając, że wszystkie dane w dyski maszyn wirtualnych są szyfrowane na pozostałych w [Magazyn Azure](https://azure.microsoft.com/documentation/services/storage/).

> [AZURE.NOTE] Azure szyfrowanie dysku jest obsługiwane na następujące systemy operacyjne Windows server — Windows Server 2008 R2, Windows Server 2012 i Windows Server 2012 R2. Szyfrowanie dysków jest obsługiwana w następujących Linux serwera systemów operacyjnych - Ubuntu, CentOS, SUSE i SUSE Linux Enterprise Server (SLES).

## <a name="implement-the-recommendation"></a>Wykonania zalecenia

1. W karta **zalecenia** wybierz pozycję **Zastosuj szyfrowanie dysków**.
2. W karta **Szyfrowanie dysków Zastosuj** będą widoczne listę maszyny wirtualne, dla których zaleca się szyfrowanie dysków.
3. Postępuj zgodnie z instrukcjami, aby zastosować te maszyny wirtualne szyfrowania.

![][1]

Aby zaszyfrować maszyn wirtualnych Azure, które zostały określone przez Centrum zabezpieczeń jako wymagające szyfrowania, zaleca się następujące czynności:

- Instalowanie i konfigurowanie Azure programu PowerShell. Pozwoli do uruchomienia polecenia programu PowerShell wymagane do skonfigurowania wstępne wymaganie szyfrowanie maszyn wirtualnych Azure.
- Uzyskaj i uruchom skrypt programu PowerShell Azure dysku szyfrowania wymagania wstępne dotyczące Azure.
- Zaszyfruj maszyn wirtualnych.

[Szyfrowanie maszyn wirtualnych Azure](security-center-disk-encryption.md) przeprowadzi Cię przez następujące etapy.  W tym temacie założono, że korzystasz z systemu Windows 10 jako komputer kliencki, z której zostanie skonfigurowane szyfrowanie dysków.

Istnieje wiele metod, które mogą być używane do konfiguracji wymagania wstępne i skonfigurować szyfrowanie dla maszyn wirtualnych Azure. Jeśli masz już dobrze versed Azure programu PowerShell lub interfejsu wiersza polecenia Azure, może być woli używać alternatywnych metod. Aby uzyskać informacje o tych inne metody zobacz [Szyfrowanie Azure dysku](../security/azure-security-disk-encryption.md).



## <a name="see-also"></a>Zobacz też

Ten dokument, były wyświetlane, jak wdrażać rekomendacji Centrum zabezpieczeń "Zastosuj szyfrowanie dysków". Aby dowiedzieć się więcej na temat szyfrowania dysku, zobacz następujące artykuły:

- [Szyfrowanie i zarządzania kluczami z magazynu klucza Azure](https://azure.microsoft.com/documentation/videos/azurecon-2015-encryption-and-key-management-with-azure-key-vault/) (wideo i 36 min 39 s) — Dowiedz się, jak chronić i ochrona danych za pomocą dysku szyfrowania zarządzania IaaS maszyny wirtualne i Azure klucza magazynu.
- [Szyfrowanie Azure maszyn wirtualnych](security-center-disk-encryption.md) (dokument) — Dowiedz się, jak szyfrować maszyn wirtualnych Azure.
- [Szyfrowanie dysków Azure](../security/azure-security-disk-encryption.md) (dokument) — Dowiedz się, jak włączyć szyfrowanie dysku dla systemu Windows i Linux oraz maszyny wirtualne.

Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

- [Ustawianie zasad zabezpieczeń w Centrum zabezpieczeń Azure](security-center-policies.md) — Dowiedz się, jak skonfigurować zasady zabezpieczeń.
- [Monitorowanie kondycji zabezpieczeń w Centrum zabezpieczeń Azure](security-center-monitoring.md) — Dowiedz się, jak monitorowanie kondycji Azure zasobów.
- [Zarządzanie notatkami i odpowiadanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md) — Dowiedz się, jak zarządzać i odpowiadanie na alerty zabezpieczeń.
- [Zarządzanie zalecenia dotyczące zabezpieczeń w Centrum zabezpieczeń Azure](security-center-recommendations.md) — Dowiedz się, jak zalecenia ułatwiają ochrony Azure zasobów.
- [Azure zabezpieczeń Centrum — często zadawane pytania](security-center-faq.md) — znajdowanie często zadawane pytania dotyczące korzystania z usługi.
- [Blog dotyczący zabezpieczeń azure](http://blogs.msdn.com/b/azuresecurity/) — blog Znajdź wpisy o Azure zabezpieczenia i zgodność.



<!--Image references-->
[1]: ./media/security-center-apply-disk-encryption/apply-disk-encryption.png
