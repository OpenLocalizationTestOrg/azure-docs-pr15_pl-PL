<properties
   pageTitle="Integrowanie alertów Centrum zabezpieczeń Azure z dziennika Azure integracji (wersja Preview) | Microsoft Azure"
   description="Ten artykuł ułatwia wprowadzenie do integrowania alertów Centrum zabezpieczeń z integracji Azure dziennika."
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
   ms.date="08/08/2016"
   ms.author="terrylan"/>

# <a name="integrating-security-center-alerts-with-azure-log-integration-preview"></a>Integrowanie alertów Centrum zabezpieczeń z dziennika Azure integracji (wersja Preview)

Wiele operacji zabezpieczeń i zespołów zdarzenia odpowiedzi są oparte na rozwiązanie informacje dotyczące zabezpieczeń i zdarzeń zarządzania (SIEM) jako punkt początkowy dla triaging i badania alertów zabezpieczeń. Dzięki usłudze Azure dziennika integracji klientów synchronizować alerty Centrum zabezpieczeń Azure wraz z zdarzeń zabezpieczeń maszyn wirtualnych zebranych przez Azure narzędzia diagnostyczne i dzienników inspekcji Azure, z ich analizy dziennika lub rozwiązanie SIEM w pobliżu w czasie rzeczywistym.

Integracja dziennika Azure działa z HP ArcSight, Splunk IBM QRadar i innych osób.

## <a name="what-logs-can-i-integrate"></a>Jakie dzienniki można zintegrować?

Azure tworzy szczegółowe rejestrowanie dla każdej usługi. Dzienniki te są określane jako:

- **Dzienniki kontroli i zarządzania nimi** , który zapewnić wgląd operacje Azure Menedżer zasobów utworzyć, AKTUALIZOWANIE i usuwanie.
- **Dzienniki płaszczyzny danych** , która zapewnia widoczność do zdarzenia wywoływane, gdy przy użyciu Azure zasobu. Przykład jest dziennik zdarzeń systemu Windows — bezpieczeństwo i dzienniki aplikacji w maszyny wirtualnej.

Integracja Azure dziennika obsługuje obecnie integracji:

- Azure dzienniki maszyn wirtualnych
- Dzienniki inspekcji Azure
- Alerty Centrum zabezpieczeń Azure

## <a name="install-azure-log-integration"></a>Instalowanie integracji Azure dziennika

Pobierz [Azure logowania integracji](https://www.microsoft.com/download/details.aspx?id=53324).

Usługi integracji Azure rejestrowania zbiera dane telemetryczne z komputera, na którym jest zainstalowana.  Telemetrycznego danych zebranych jest:

- Integracja z programem dziennika wyjątków, występujące podczas wykonywania Azure
- Metryki o liczbie kwerend i zdarzeń przetwarzane
- Statystyki, o które Azlog.exe są używane opcje wiersza polecenia

> [AZURE.NOTE] Zbieranie danych telemetrycznych można wyłączyć przez usunięcie zaznaczenia tej opcji.

## <a name="integrate-azure-audit-logs-and-security-center-alerts"></a>Integracja dzienników inspekcji Azure i Centrum zabezpieczeń alerty

1. Otwórz wiersz polecenia i **cd** do **c:\Program Files\Microsoft Azure dziennika integracji**.

2. Uruchom polecenie **azlog createazureid** , aby utworzyć [Wystawcy usługi Azure Active Directory](../active-directory/active-directory-application-objects.md) w dzierżaw Azure Active Directory (AD), które obsługują Azure subskrypcji.

    Zostanie wyświetlony monit dla usługi Azure logowania.

    > [AZURE.NOTE] Musi być subskrypcji właściciel lub Administrator Współtworzenie subskrypcji.

    Uwierzytelnianie Azure jest wykonywane za pośrednictwem Azure AD.  Tworzenie wystawcy usługi integracji Azure dziennika utworzy tożsamości Azure AD, które będzie mieć dostęp do odczytu z Azure subskrypcji.

3. Uruchamianie **Autoryzuj azlog <SubscriptionID> ** polecenie, aby przypisać dostęp czytelnika dla tej subskrypcji kapitału usługi utworzony w kroku 2. Jeśli nie określisz **SubscriptionID**, następnie wystawcy usługi są przypisywane roli czytnik do wszystkich subskrypcji, do których masz dostęp.

    > [AZURE.NOTE] Ostrzeżenia mogą być wyświetlane, jeśli Uruchom polecenie **Autoryzuj** natychmiast po poleceniu **createazureid** , ponieważ istnieje kilka opóźnienie między po utworzeniu konta Azure AD i gdy konto jest dostępny do użytku. Jeśli poczekać około 10 sekund po uruchomieniu polecenia **createazureid** w celu uruchomienia polecenia **Autoryzuj** należy nie widzisz tych ostrzeżeń.

4. Sprawdź poniższych folderów, aby potwierdzić, że JSON pliki dziennika inspekcji są dostępne:

  - **c:\Users\azlog\AzureResourceManagerJson**
  - **c:\Users\azlog\AzureResourceManagerJsonLD**

5. Sprawdź, aby potwierdzić, że alerty Centrum zabezpieczeń istnieje w nich następujące foldery:

  - **c:\Users\azlog\ AzureSecurityCenterJson**
  - **c:\Users\azlog\AzureSecurityCenterJsonLD**

6. Wskaż standardowe złącze przesyłania pliku SIEM do odpowiedniego folderu w potoku danych do wystąpienia SIEM. Zajrzyj do [konfiguracji SIEM](https://azsiempublicdrops.blob.core.windows.net/drops/ALL.htm) na konfiguracji SIEM.

Jeśli masz pytania dotyczące integracji dziennika Azure, Wyślij wiadomość e-mail do [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat dzienników inspekcji Azure i definicje właściwości, zobacz:

- [Inspekcja operacji przy użyciu Menedżera zasobów](../resource-group-audit.md)
- [Lista zdarzeń zarządzania subskrypcją](https://msdn.microsoft.com/library/azure/dn931934.aspx) — pobrać zdarzeń dziennika inspekcji.

Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

- [Zarządzanie notatkami i odpowiadanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md) — Dowiedz się, jak zarządzać i odpowiadanie na alerty zabezpieczeń.
- [Często zadawane pytania dotyczące Azure zabezpieczeń Centrum](security-center-faq.md) — znajdowanie często zadawane pytania dotyczące korzystania z usługi.
- [Blog dotyczący zabezpieczeń Azure](http://blogs.msdn.com/b/azuresecurity/) — uzyskiwanie najnowsze Azure zabezpieczeń i informacji.
