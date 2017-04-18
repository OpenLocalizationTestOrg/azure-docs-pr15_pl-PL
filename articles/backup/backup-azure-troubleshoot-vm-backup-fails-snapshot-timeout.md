<properties
   pageTitle="Azure maszyn wirtualnych kopii zapasowej kończy się niepowodzeniem: nie można nawiązać komunikacji z agentem maszyn wirtualnych migawkę stanu — Przekroczono limit czasu zadania podrzędnego migawki maszyn wirtualnych | Microsoft Azure"
   description="Symptomy przyczyny i rozwiązania dla maszyn wirtualnych Azure błędy kopii zapasowej związane z nie można komunikować się z agentem maszyn wirtualnych migawkę stanu. Zadania podrzędnego maszyn wirtualnych migawkę przekroczony błędu"
   services="backup"
   documentationCenter=""
   authors="genlin"
   manager="cfreeman"
   editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jimpark; markgal;genli"/>

# <a name="azure-vm-backup-fails-could-not-communicate-with-the-vm-agent-for-snapshot-status---snapshot-vm-sub-task-timed-out"></a>Azure maszyn wirtualnych kopii zapasowej kończy się niepowodzeniem: nie można nawiązać komunikacji z agentem maszyn wirtualnych migawkę stanu — Przekroczono limit czasu zadania podrzędnego migawki maszyn wirtualnych

## <a name="summary"></a>Podsumowanie

Po rejestrowanie i Planowanie kopii zapasowej Azure maszyn wirtualnych (maszyny), usługa Azure kopii zapasowej inicjuje zadania wykonywania kopii zapasowej w zaplanowanym terminie przez komunikowania się z kopii zapasowej rozszerzenie w maszyn wirtualnych wykonywania migawki w chwili. Istnieją warunki, które mogą uniemożliwić migawki wywołany, który prowadzi do awarii kopii zapasowej. Ten artykuł zawiera procedurę rozwiązywania problemów dla problemów związanych z kopii zapasowej błędy maszyn wirtualnych Azure związane z migawki błąd przekroczenia limitu czasu.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="symptom"></a>Symptom

Kopia zapasowa Azure Microsoft infrastruktury jako usługa (IaaS) maszyn wirtualnych kończy się niepowodzeniem i zwraca szczegóły błędu zadania w [Azure portal](https://portal.azure.com/)następujący komunikat o błędzie:

**Nie można komunikować się z agentem maszyn wirtualnych migawkę stanu — Przekroczono limit czasu zadania podrzędnego migawki maszyn wirtualnych.**

## <a name="cause"></a>Przyczyna
Istnieją cztery typowe przyczyny tego błędu:

- Maszyn wirtualnych nie ma dostęp do Internetu.
- Agent maszyn wirtualnych programu Microsoft Azure zainstalowanych w maszyn wirtualnych jest nieaktualny (w przypadku maszyny wirtualne Linux).
- Rozszerzenie kopii zapasowej kończy się niepowodzeniem, aktualizować i ładowanie.
- Nie można pobrać stanu migawki lub nie mogą być wykonywane migawki.

## <a name="cause-1-the-vm-does-not-have-internet-access"></a>Przyczyna 1: Maszyn wirtualnych nie ma dostęp do Internetu
Na wymagania wdrażania maszyn wirtualnych nie dostępem do Internetu lub występują ograniczenia w miejscu, które uniemożliwiają dostęp do infrastruktury Azure.

Rozszerzenie kopii zapasowej wymagają łączności z Azure publicznych adresów IP do prawidłowego działania. Jest to spowodowane wysyła polecenia do punktu końcowego magazyn Azure (adres URL HTTP) do zarządzania migawki maszyn wirtualnych. Jeśli rozszerzenie nie ma dostępu do publicznego Internetu, kopia zapasowa ostatecznie nie działa.

### <a name="solution"></a>Rozwiązanie
W tym scenariuszu użyj jednej z następujących metod Aby rozwiązać ten problem:

- Zakresy adresów IP listy sprawdzonej Azure centrum danych
- Utwórz ścieżkę na ruch HTTP

### <a name="to-whitelist-the-azure-datacenter-ip-ranges"></a>Do listy sprawdzonej Azure zakresów adresów IP centrum danych

1. Uzyskanie [listy Azure centrum danych adresy IP](https://www.microsoft.com/download/details.aspx?id=41653) jako whitelisted.
2. Wyłączanie blokowania adresy IP przy użyciu polecenia cmdlet New-NetRoute. Uruchom to polecenie cmdlet w maszyn wirtualnych Azure w oknie programu PowerShell podwyższonym poziomem uprawnień (polecenie Uruchom jako Administrator).
3. Dodawanie reguły do grupy zabezpieczeń sieci (NSG), jeśli masz jeden-do-Zezwalaj na dostęp do adresy IP.

### <a name="to-create-a-path-for-http-traffic-to-flow"></a>Aby utworzyć ścieżkę na ruch HTTP

1. Jeśli masz ograniczeń sieci w miejscu (na przykład NSG), należy wdrożyć serwer proxy HTTP, aby skierować ruch.
2. Jeśli masz grupy zabezpieczeń sieci (NSG), Dodaj reguły, aby zezwolić na dostęp do Internetu z serwera proxy HTTP.

Dowiedz się, jak [skonfigurować serwer proxy HTTP kopii zapasowych maszyn wirtualnych](backup-azure-vms-prepare.md#using-an-http-proxy-for-vm-backups).

## <a name="cause-2-the-microsoft-azure-vm-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>Przyczyna 2: Agent maszyn wirtualnych programu Microsoft Azure zainstalowanych w maszyn wirtualnych jest nieaktualny (w przypadku maszyny wirtualne Linux)

### <a name="solution"></a>Rozwiązanie
Problemy, które wpływają na starych agenta maszyn wirtualnych powodem jest najbardziej związane z agenta lub rozszerzenia błędy maszyny wirtualne Linux. Ogólne najlepiej pierwsze kroki, aby rozwiązać ten problem, są następujące:

1. [Zainstaluj najnowszą wersję agent maszyn wirtualnych Azure](https://github.com/Azure/WALinuxAgent).
2. Upewnij się, że Azure agent nie działa na maszyn wirtualnych. Aby to zrobić, uruchom następujące polecenie:```ps -e```

    Jeśli ten proces nie jest uruchomiony, należy użyć następujących poleceń, aby uruchomić go ponownie.

    Dla Ubuntu:```service walinuxagent start```

    Dla innych form podziału: "" usługi waagent start
```

3. [Configure the auto restart agent](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).

4. Run a new test backup. If the failures persist, please collect logs from the following folders for further debugging.

    We require the following logs from the customer’s VM:

    - /var/lib/waagent/*.xml
    - /var/log/waagent.log
    - /var/log/azure/*

If we require verbose logging for waagent, follow these steps to enable this:

1. In the /etc/waagent.conf file, locate the following line:

    Enable verbose logging (y|n)

2. Change the **Logs.Verbose** value from n to y.
3. Save the change, and then restart waagent by following the previous steps in this section.

## Cause 3: The backup extension fails to update or load
If extensions cannot be loaded, then Backup fails because a snapshot cannot be taken.

### Solution
For Windows guests:

1. Verify that the iaasvmprovider service is enabled and has a startup type of automatic.
2. If this is not the configuration, enable the service to determine whether the next backup succeeds.

For Linux guests:

The latest version of VMSnapshot Linux (extension used by backup) is 1.0.91.0

If the backup extension still fails to update or load, you can force the VMSnapshot extension to be reloaded by uninstalling the extension. The next backup attempt will reload the extension.

### To uninstall the extension

1. Go to the [Azure portal](https://portal.azure.com/).
2. Locate the particular VM that has backup problems.
3. Click **Settings**.
4. Click **Extensions**.
5. Click **Vmsnapshot Extension**.
6. Click **uninstall**.

This will cause the extension to be reinstalled during the next backup.

## Cause 4: The snapshots status cannot be retrieved or the snapshots cannot be taken
VM backup relies on issuing snapshot command to underlying storage. The backup can fail because it has no access to storage or because of a delay in snapshot task execution.

### Solution
The following conditions can cause snapshot task failure:

| Cause | Solution |
| ----- | ----- |
| VMs that have Microsoft SQL Server Backup configured. By default, VM Backup runs a VSS Full backup on Windows VMs. On VMs that are running SQL Server-based servers and on which SQL Server Backup is configured, snapshot execution delays may occur. | Set following registry key if you are experiencing backup failures because of snapshot issues.<br><br>[HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT] "USEVSSCOPYBACKUP"="TRUE" |
| VM status is reported incorrectly because the VM is shut down in RDP. If you shut down the virtual machine in RDP, check the portal to determine whether that VM status is reflected correctly. | If it’s not, shut down the VM in the portal by using the ”Shutdown” option in the VM dashboard. |
| Many VMs from the same cloud service are configured to back up at the same time. | It’s a best practice to spread out the VMs from the same cloud service to have different backup schedules. |
| The VM is running at high CPU or memory usage. | If the VM is running at high CPU usage (more than 90 percent) or high memory usage, the snapshot task is queued and delayed and eventually times out. In this situation, try on-demand backup. |
|The VM cannot get host/fabric address from DHCP.|DHCP must be enabled inside the guest for IaaS VM Backup to work.  If the VM cannot get host/fabric address from DHCP response 245, then it cannot download ir run any extensions. If you need a static private IP, you should configure it through the platform. The DHCP option inside the VM should be left enabled. View more information about [Setting a Static Internal Private IP](../virtual-network/virtual-networks-reserved-private-ip.md).|
