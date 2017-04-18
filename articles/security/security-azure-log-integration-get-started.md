<properties
   pageTitle="Wprowadzenie do integracji Azure dziennika | Microsoft Azure"
   description="Dowiedz się, jak zainstalować usługę Azure Dziennik integracji i integracja dzienników z Azure miejsca do magazynowania, dzienników inspekcji Azure i alertów Centrum zabezpieczeń Azure."
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

# <a name="get-started-with-azure-log-integration-preview"></a>Wprowadzenie do integracji Azure dziennika (wersja Preview)

Integracja dziennika Azure umożliwia integrowanie nowych dzienników od Azure zasobów lokalnych systemów zabezpieczeń informacji i zdarzeń zarządzania (SIEM). Integracja jest ujednolicony pulpitu nawigacyjnego wszystkich trwałych, lokalnego lub w chmurze, dzięki czemu można agregowanie, przeniesionym, analizowanie i powiadomienia dla zdarzenia zabezpieczeń skojarzone z aplikacjami.

Ten samouczek przeprowadzi Cię przez sposobu instalowania integracji dziennika Azure i integracja dzienników z Azure miejsca do magazynowania, dzienników inspekcji Azure i alertów Centrum zabezpieczeń Azure. Szacowany czas użycia tego samouczka jest godzinę.

## <a name="prerequisites"></a>Wymagania wstępne

Aby użyć tego samouczka, musisz mieć następujące czynności:

- Komputerze (lokalnego lub w chmurze) do zainstalowania usługi integracji Azure dziennika. Ten komputer musi być zainstalowany program .net 4.5.1 zainstalowany 64-bitowej systemu operacyjnego Windows. Ten komputer jest określana mianem **Azlog Integrator**.
- Azure subskrypcji. Jeśli nie masz, możesz zalogować [bezpłatne konto](https://azure.microsoft.com/free/).
- Diagnostyka Azure włączone dla usługi Azure wirtualnych maszyn. Aby włączyć diagnostyki dla usług w chmurze, zobacz [Włączanie diagnostyki Azure w Azure usług w chmurze](../cloud-services/cloud-services-dotnet-diagnostics.md). Aby włączyć diagnostyki dla maszyn wirtualnych Azure z systemem Windows, zobacz za [pomocą umożliwiające diagnostyki Azure w systemie Windows maszyn wirtualnych](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md).
- Nawiązywanie połączenia z Azlog Integrator do magazynu Azure i uwierzytelniania i autoryzacji do subskrypcji usługi Azure.
- W przypadku maszyn wirtualnych Azure dzienników agenta SIEM (na przykład Splunk uniwersalny usługi przesyłania dalej, agent zbierających zdarzeń systemu Windows ArcSight HP lub IBM QRadar WinCollect) musi być zainstalowana na Azlog Integrator.

## <a name="deployment-considerations"></a>Zagadnienia dotyczące rozmieszczania

Można uruchamiać wielu wystąpień Azlog Integrator, jeśli wydarzenie głośności jest wysoki. Diagnostyka Azure kont miejsca do magazynowania dla systemu Windows *(WAD)* i liczbę subskrypcji, aby zapewnić dla wystąpienia równoważenia obciążenia powinny być oparte na wydajność.

Na komputerze 8-procesora (core) jedno wystąpienie Azlog Integrator może przetwarzać zdarzenia około 24 miliony dziennie (~1M/hour).

Na komputerze procesor 4 (podstawowych) jedno wystąpienie Azlog Integrator może przetwarzać zdarzenia około 1,5 miliona dziennie (~62.5K/hour).

## <a name="install-azure-log-integration"></a>Instalowanie integracji Azure dziennika

Pobierz [Azure logowania integracji](https://www.microsoft.com/download/details.aspx?id=53324).

Usługi integracji Azure rejestrowania zbiera dane telemetryczne z komputera, na którym jest zainstalowana.  Telemetrycznego danych zebranych jest:

- Integracja z programem dziennika wyjątków, występujące podczas wykonywania Azure
- Metryki o liczbie kwerend i zdarzeń przetwarzane
- Statystyki, o które Azlog.exe są używane opcje wiersza polecenia

> [AZURE.NOTE] Zbieranie danych telemetrycznych można wyłączyć przez usunięcie zaznaczenia tej opcji.

## <a name="integrate-azure-vm-logs-from-your-azure-diagnostics-storage-accounts"></a>Integracja dzienniki Azure maszyn wirtualnych z kont miejsca do magazynowania diagnostyki Azure

1. Sprawdź wymagania wstępne dotyczące wymienionych powyżej, aby upewnić się, że konta miejsca do magazynowania WAD zbierania dzienników, przed kontynuowaniem integracji usługi Azure dziennika. Nie należy wykonywać następujące czynności, jeśli konta miejsca do magazynowania WAD nie jest zbieranie dzienników.

2. Otwórz wiersz polecenia i **cd** do **c:\Program Files\Microsoft Azure dziennika integracji**.

3. Uruchamianie polecenia

        azlog source add <FriendlyNameForTheSource> WAD <StorageAccountName> <StorageKey>

      Zamień StorageAccountName nazwę konta magazynu platformy Azure skonfigurowane do odbierania zdarzeń diagnostyki z Twojej maszyn wirtualnych.

        azlog source add azlogtest WAD azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==

      Jeśli chcesz identyfikator subskrypcji się pojawić w przypadku XML, Dołącz identyfikator subskrypcji przyjazną nazwę:

        azlog source add <FriendlyNameForTheSource>.<SubscriptionID> WAD <StorageAccountName> <StorageKey>

4. Poczekaj 30 – 60 minut (go może potrwać godziny), a następnie wybierz zdarzenia, które są pobierane z konta miejsca do magazynowania. Aby wyświetlić, otwórz **Podgląd zdarzeń > Dzienniki systemu Windows > zdarzenia przekazane** na Azlog Integrator.

5. Upewnij się, że wybrany standardowy łącznik SIEM zainstalowany na komputerze jest skonfigurowany do wybierz zdarzenia z folderu **Zdarzenia przekazane** i potoku je do wystąpienia SIEM. Przejrzyj SIEM określonej konfiguracji do konfigurowania i zobacz dzienniki integracji.

## <a name="what-if-data-is-not-showing-up-in-the-forwarded-events-folder"></a>Co zrobić, jeśli dane nie jest widoczne w folderze zdarzenia przekazane?

Jeśli po upływie godziny danych nie jest widoczne w folderze **Zdarzenia przekazane** następnie:

1. Uruchom na tym komputerze i upewnij się, że można uzyskać dostęp do Azure. Aby przetestować połączenia, spróbuj otworzyć [Azure portal](http://portal.azure.com) z poziomu przeglądarki.

2. Upewnij się, że konto użytkownika **azlog** ma uprawnienia do zapisu w folderze **users\azlog**.

3. Upewnij się, konto miejsca do magazynowania, dodane w polecenie **Dodaj źródło azlog** znajduje się po uruchomieniu polecenia **azlog listy źródłowej**.
4. Przejdź do pozycji **Podgląd zdarzeń > Dzienniki systemu Windows > aplikacji** Aby sprawdzić, czy są błędy zgłoszone z integracji Azure dziennika.

Jeśli nadal nie widać zdarzenia, następnie:

1. Pobierz [Eksplorator magazynu platformy Microsoft Azure](http://storageexplorer.com/).

2. Łączenie z kontem miejsca do magazynowania, dodane w polecenie **Dodaj źródło azlog**.

3. W Eksploratorze Azure miejsca do magazynowania przejdź do tabeli **WADWindowsEventLogsTable** , aby sprawdzić, czy wszystkie dane. Jeśli nie, następnie diagnostyki w maszyn wirtualnych nie jest poprawnie skonfigurowany.

## <a name="integrate-azure-audit-logs-and-security-center-alerts"></a>Integracja dzienników inspekcji Azure i alerty Centrum zabezpieczeń

1. Otwórz wiersz polecenia i **cd** do **c:\Program Files\Microsoft Azure dziennika integracji**.

2. Uruchamianie polecenia

        azlog createazureid

      To polecenie jest wyświetlany monit dla usługi Azure logowania. Polecenie tworzy następnie [Wystawcy usługi Azure Active Directory](../active-directory/active-directory-application-objects.md) w Azure AD dzierżaw, które obsługują Azure subskrypcji, w których zalogowany użytkownik jest administratorem, Współtworzenie administratora lub właściciela. Polecenie zakończy się niepowodzeniem, jeśli tylko Gość w dzierżawie Azure AD jest zalogowany użytkownik. Uwierzytelnianie Azure jest wykonywane za pośrednictwem Azure Active Directory (AD).  Tworzenie wystawcy usługi integracji Azlog tworzy tożsamości Azure AD, które będzie mieć dostęp do odczytu z Azure subskrypcji.

3. Uruchamianie polecenia

        azlog authorize <SubscriptionID>

      W ten sposób dostęp czytelnika na subskrypcję głównej usługi utworzony w kroku 2. Jeśli nie określisz SubscriptionID, następnie próbuje przypisanie roli kapitału czytnika usługi, aby wszystkie subskrypcje, do którego masz dostęp do wszystkich.

        azlog authorize 0ee9d577-9bc4-4a32-a4e8-c29981025328

      > [AZURE.NOTE] Ostrzeżenia mogą być wyświetlane po uruchomieniu polecenia **Autoryzuj** zaraz po polecenie **createazureid** . Istnieje kilka opóźnienie między po utworzeniu konta Azure AD i gdy konto jest dostępny do użytku. Jeśli poczekać około 10 sekund po uruchomieniu polecenia **createazureid** w celu uruchomienia polecenia **Autoryzuj** należy nie widzisz tych ostrzeżeń.

4. Sprawdź poniższych folderów, aby potwierdzić, że JSON pliki dziennika inspekcji są dostępne:

  - **c:\Users\azlog\AzureResourceManagerJson**
  - **c:\Users\azlog\AzureResourceManagerJsonLD**

5. Sprawdź, aby potwierdzić, że alerty Centrum zabezpieczeń istnieje w nich następujące foldery:

  - **c:\Users\azlog\ AzureSecurityCenterJson**
  - **c:\Users\azlog\AzureSecurityCenterJsonLD**

6. Wskaż standardowe złącze przesyłania pliku SIEM do odpowiedniego folderu w potoku danych do wystąpienia SIEM. Może być konieczne niektóre mapowania pól według produktu SIEM, którego używasz.

Jeśli masz pytania dotyczące integracji dziennika Azure, Wyślij wiadomość e-mail do [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## <a name="next-steps"></a>Następne kroki

W tym samouczku wiesz, jak zainstalować integracji dziennika Azure i integracja dzienników z magazynu Azure. Aby uzyskać więcej informacji, zobacz następujące artykuły:

- [Integracja dziennika Azure Microsoft Azure dzienników (wersja Preview)](https://www.microsoft.com/download/details.aspx?id=53324) — Centrum pobierania szczegóły, wymagania systemowe i instrukcji instalacji na integracji Azure dziennika.
- [Wprowadzenie do integracji Azure dziennika](security-azure-log-integration-overview.md) — ten dokument wprowadzenie do integracji Azure dziennika, klucza możliwości i jak to działa.
- [Kroki konfiguracji partnera](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) — ten wpis w blogu pokazano, jak skonfigurować integrację Azure dziennika do pracy z rozwiązań partnerów Splunk, HP ArcSight i IBM QRadar.
- [Azure dziennika integracji często zadawane pytania](security-azure-log-integration-faq.md) — ten często zadawane pytania dotyczące odpowiedzi na pytania dotyczące integracji Azure dziennika.
- [Alerty Centrum zabezpieczeń Integracja z Azure logowania integracji](../security-center/security-center-integrating-alerts-with-log-integration.md) — ten dokument pokazano, jak zsynchronizować alertów Centrum zabezpieczeń oraz zdarzeń zabezpieczeń maszyn wirtualnych zebranych przez Azure narzędzia diagnostyczne i dzienników inspekcji Azure, z dziennika analizy lub rozwiązanie SIEM.
- [Nowe funkcje dla Azure narzędzia diagnostyczne i dzienników inspekcji Azure](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) — ten wpis w blogu poznasz dzienników inspekcji Azure i inne funkcje, które ułatwiają uzyskanie wniosków na operacje Azure zasobów.
