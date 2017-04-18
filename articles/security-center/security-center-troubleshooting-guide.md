<properties
   pageTitle="Rozwiązywanie problemów z przewodnika Centrum zabezpieczeń Azure | Microsoft Azure"
   description="Ten dokument pomaga w celu rozwiązywania problemów w Centrum zabezpieczeń Azure."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-troubleshooting-guide"></a>Przewodnik dotyczący rozwiązywania problemów w Centrum zabezpieczeń Azure
Ten przewodnik jest dla informatyków dla informatyków, informacje o zabezpieczeniach analityków i administratorów chmury organizacji, w których jest używany Centrum zabezpieczeń Azure i rozwiązywanie z czy problemy związane z Centrum zabezpieczeń.

## <a name="troubleshooting-guide"></a>Rozwiązywanie problemów z przewodnika
Ten przewodnik wyjaśniono, jak rozwiązywać problemy związane z Centrum zabezpieczeń. Większość rozwiązywania problemów w Centrum zabezpieczeń ma być wykonywana, sprawdzając rekordów [Dziennik inspekcji](https://azure.microsoft.com/updates/audit-logs-in-azure-preview-portal/) dla składnika nie powiodło się. Za pomocą dzienników inspekcji można określić:

- Operacje zostały wykonane
- Kto rozpoczął operację
- Wystąpienia tej operacji
- Stan operacji
- Wartości innych właściwości, które mogą być pomocne poszukiwanie operacji

Dziennik inspekcji zawiera wszystkie operacji zapisu (położenie, WPIS, Usuń) na zasoby, ale nie zawiera operacji odczytu (Pobierz).

## <a name="troubleshooting-monitoring-agent-installation-in-windows"></a>Rozwiązywanie problemów z monitorowania instalacji agenta w systemie Windows

Centrum zabezpieczeń monitorowanie agenta jest używana do wykonywania zbioru danych. Po włączeniu zbierania danych i agent jest poprawnie zainstalowany na komputerze docelowym, te procesy powinien być na wykonanie:

- ASMAgentLauncher.exe - Azure monitorowania agenta 
- ASMMonitoringAgent.exe — wewnętrzny Azure monitorowanie zabezpieczeń
- ASMSoftwareScanner.exe — Menedżer skanowania Azure

Monitorowanie zabezpieczeń Azure rozszerzenia skanuje dla różnych konfiguracji odpowiednie zabezpieczeń i zbiera dzienniki zabezpieczeń z maszyny wirtualnej. Menedżer skanowania zostanie użyty jako skanera poprawki.

Jeśli instalacja odbywa się pomyślnie powinien zostać wyświetlony wpis podobny do tego poniżej w dzienników inspekcji dla elementu docelowego maszyn wirtualnych:

![Dzienniki inspekcji](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig1.png)

Można również uzyskać więcej informacji na temat instalowania, czytając dzienniki agentów znajdujący się w *%systemdrive%\windowsazure\logs* (przykład: C:\WindowsAzure\Logs).

> [AZURE.NOTE] Agent Centrum zabezpieczeń Azure jest źle działający, będzie konieczne ponowne uruchomienie docelowej maszyn wirtualnych, ponieważ ma polecenia, aby zatrzymać i uruchomić agenta.

## <a name="troubleshooting-monitoring-agent-installation-in-linux"></a>Rozwiązywanie problemów z monitorowania instalacji agenta w Linux
Podczas rozwiązywania problemów z instalacją agenta maszyn wirtualnych w systemie Linux należy się upewnić, że rozszerzenie został pobrany do/var/biblioteki/waagent /. Można uruchomić poniższe polecenie, aby sprawdzić, czy został zainstalowany:

`cat /var/log/waagent.log` 

Inne pliki dziennika można przeglądać dotyczących rozwiązywania problemów przeznaczenie są: 

- /var/log/mdsd.err
- / var/dziennika-azure /

W systemie pracy połączenia do procesu mdsd powinny być widoczne na TCP 29130. Jest to syslog komunikowania się z procesem mdsd. Ten problem można sprawdzić, uruchamiając poniższe polecenie:

`netstat -plantu | grep 29130`

## <a name="contacting-microsoft-support"></a>Skontaktowanie się z pomocą techniczną firmy Microsoft

Niektóre problemy dotyczące można określić przy użyciu wytyczne, pod warunkiem, w tym artykule, inne osoby, które znajdują się też opisane w publicznym Centrum zabezpieczeń [Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSecurityCenter). Jednak jeśli potrzebujesz dalszej rozwiązywania problemów, możesz otworzyć nowe żądanie pomocy technicznej przy użyciu Azure Portal, tak jak pokazano poniżej: 

![Pomoc techniczna firmy Microsoft](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig2.png)


## <a name="see-also"></a>Zobacz też

W tym dokumencie wiesz, jak skonfigurować zasady zabezpieczeń w Centrum zabezpieczeń Azure. Aby dowiedzieć się więcej na temat Centrum zabezpieczeń Azure, zobacz następujące artykuły:

- [Planowanie Centrum zabezpieczeń Azure i przewodnik](security-center-planning-and-operations-guide.md) — Dowiedz się, jak planowanie i opis zagadnienia projektowe przyjęcie Centrum zabezpieczeń Azure.
- [Prawidłowość zabezpieczeń monitorowania w Centrum zabezpieczeń Azure](security-center-monitoring.md) — Dowiedz się, jak monitorowanie kondycji Azure zasobów
- [Zarządzanie notatkami i odpowiadanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md) — Dowiedz się, jak zarządzać i odpowiadanie na alerty zabezpieczeń
- [Monitorowanie rozwiązań partnerów z Centrum zabezpieczeń Azure](security-center-partner-solutions.md) — Dowiedz się, jak można monitorować stan kondycji rozwiązań partnerów.
- [Często zadawane pytania dotyczące Azure zabezpieczeń Centrum](security-center-faq.md) — znajdowanie często zadawane pytania dotyczące korzystania z usługi
- [Blog dotyczący zabezpieczeń Azure](http://blogs.msdn.com/b/azuresecurity/) — znajdowanie blogu wpisy o Azure zabezpieczenia i zgodność
