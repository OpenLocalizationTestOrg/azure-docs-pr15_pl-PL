<properties
   pageTitle="Integracja Azure dziennika — często zadawane pytania | Microsoft Azure"
   description="Niniejszy artykuł odpowiedzi na pytania dotyczące integracji Azure dziennika."
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
   ms.date="08/23/2016"
   ms.author="TomSh"/>

# <a name="azure-log-integration-frequently-asked-questions-faq"></a>Integracja Azure dziennika — często zadawane pytania

Niniejszy artykuł odpowiedzi na pytania dotyczące integracji Azure dziennika, usługa, która umożliwia integrowanie nowych dzienników od Azure zasobów lokalnych systemów zabezpieczeń informacji i zdarzeń zarządzania (SIEM). Integracja jest ujednolicony pulpitu nawigacyjnego wszystkich trwałych, lokalnego lub w chmurze, dzięki czemu można agregowanie, przeniesionym, analizowanie i powiadomienia dla zdarzenia zabezpieczeń skojarzone z aplikacjami.

## <a name="how-can-i-see-the-storage-accounts-from-which-azure-log-integration-is-pulling-azure-vm-logs-from"></a>Jak wyświetlić konta miejsca do magazynowania, z których integracji Azure dziennika jest pociągnięcie dzienniki Azure maszyn wirtualnych z?

Uruchom polecenie **azlog listy źródłowej**.

## <a name="how-can-i-update-the-proxy-configuration"></a>Jak można zaktualizować konfiguracji serwera proxy?

Jeśli ustawienia serwera proxy nie zezwala na dostęp do magazynu Azure bezpośrednio, otwórz **AZLOG. EXE. CONFIG** pliku w **c:\Program Files\Microsoft Azure dziennika integracji**. Zaktualizuj plik do dołączenia sekcji **defaultProxy** z adresem e-mail proxy w Twojej organizacji. Po zakończeniu aktualizacji zatrzymać i uruchomić usługę za pomocą polecenia **azlog netto tabulatora** i **azlog netto start**.

    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.net>
        <connectionManagement>
          <add address="*" maxconnection="400" />
        </connectionManagement>
        <defaultProxy>
          <proxy usesystemdefault="true"
          proxyaddress=http://127.0.0.1:8888
          bypassonlocal="true" />
        </defaultProxy>
      </system.net>
      <system.diagnostics>
        <performanceCounters filemappingsize="20971520" />
      </system.diagnostics>   

## <a name="how-can-i-see-the-subscription-information-in-windows-events"></a>Jak można wyświetlić informacje o subskrypcji w zdarzeń systemu Windows?

Dołączanie **subscriptionid** przyjazną nazwę podczas dodawania źródła.

    Azlog source add <sourcefriendlyname>.<subscription id> <StorageName> <StorageKey>  

Zdarzenia XML zawiera metadane, tak jak pokazano poniżej, łącznie z identyfikatorem subskrypcji.

![Zdarzenia XML][1]

## <a name="error-messages"></a>Komunikaty o błędach

### <a name="when-running-command-azlog-createazureid-why-do-i-get-the-following-error"></a>Podczas uruchamiania polecenia **azlog createazureid**, Dlaczego otrzymuję następujący komunikat o błędzie?

Błąd:

  *Nie można utworzyć aplikacji AAD - dzierżawa 72f988bf-86f1-41af-91ab-2d7cd011db37-powodu = "Zabronione" - komunikat = "Niewystarczające uprawnienia do ukończenia tej operacji."*

Pozwala podjąć próbę tworzenie wystawcy usługi w wszystkich dzierżaw Azure AD dla subskrypcji, w których Azure logowania ma dostęp do **Azlog createazureid** . Jeśli logowanie Azure jest tylko użytkownika Gość w tej dzierżawy Azure AD, następnie polecenia nie powiodło się "Wystarczających uprawnień do ukończenia tej operacji." Żądanie Administrator dzierżawy, aby dodać konto użytkownika w dzierżawie.

### <a name="when-running-command-azlog-authorize-why-do-i-get-the-following-error"></a>Podczas uruchamiania, polecenie **Autoryzuj azlog**, Dlaczego otrzymuję następujący komunikat o błędzie?

Błąd:

  *Ostrzeżenie dotyczące tworzenia przypisania roli - AuthorizationFailed: klient janedo@microsoft.com' z obiektem identyfikator "fe9e03e4-4dad-4328-910f-fd24a9660bd2" nie ma autoryzacji do wykonania akcji "Microsoft.Authorization/roleAssignments/write" w zakresie "/ subskrypcje/70 d 95299-d689-4c 97-b971-0d8ff0000000".*

**Autoryzuj Azlog** polecenie przypisuje subskrypcji wymienionych roli czytelnika do głównej usługi Azure AD (utworzonego za pomocą **Azlog createazureid**). Jeśli Azure logowania nie jest Współtworzenie administratora lub właściciela subskrypcji, pojawia się błąd "Nie można autoryzacji" komunikat o błędzie. Kontrola dostępu oparta na rolach Azure (RBAC) współpracujących administratora lub właściciela jest wymagane do ukończenia tej akcji.

## <a name="where-can-i-find-the-definition-of-the-properties-in-audit-log"></a>Gdzie znaleźć definicji właściwości dziennika inspekcji

Zobacz:

- [Inspekcja operacji przy użyciu Menedżera zasobów](../resource-group-audit.md)
- [Lista zdarzeń zarządzania subskrypcji w monitorze Azure interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/dn931934.aspx)

## <a name="where-can-i-find-details-on-azure-security-center-alerts"></a>Gdzie znaleźć szczegółowe informacje na alerty Centrum zabezpieczeń Azure?

Zobacz [Zarządzanie i odpowiadanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure](../security-center/security-center-managing-and-responding-alerts.md).

## <a name="how-can-i-modify-what-is-collected-with-vm-diagnostics"></a>Jak zmodyfikować, co to są zbierane Diagnostyka maszyn wirtualnych?

Zobacz za [pomocą umożliwiające diagnostyki Azure w maszyny wirtualnej z systemem Windows](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md) Aby uzyskać szczegółowe informacje na temat Get, Modyfikuj i ustawianie diagnostyki Azure w systemie Windows *(WAD)* konfiguracji. Oto przykład:

### <a name="get-the-wad-config"></a>Uzyskiwanie konfiguracji WAD

    -AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient
    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

    $xmlconfig | Out-File -Encoding utf8 -FilePath "d:\WADConfig.xml"

### <a name="modify-the-wad-config"></a>Modyfikowanie konfiguracji WAD

Poniższy przykład to konfiguracji, w której tylko 4624 identyfikator zdarzenia i 4625 identyfikator zdarzenia są pobierane z dziennika zdarzeń zabezpieczeń. Microsoft Antimalware wydarzenia są pobierane z dziennika zdarzeń systemu. Zobacz [przez inne zdarzenia] (https://msdn.microsoft.com/library/windows/desktop/dd996910 (v=vs.85) szczegółowe informacje na temat stosowania wyrażenia XPath.

    <WindowsEventLog scheduledTransferPeriod="PT1M">
        <DataSource name="Security!*[System[(EventID=4624 or EventID=4625)]]" />
        <DataSource name="System!*[System[Provider[@Name='Microsoft Antimalware']]]"/>
    </WindowsEventLog>

### <a name="set-the-wad-configuration"></a>Ustawienia konfiguracji WAD

    $diagnosticsconfig_path = "d:\WADConfig.xml"
    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName log3121 -StorageAccountKey <storage key>

Po wprowadzeniu zmian, sprawdź koncie przestrzeni dyskowej, aby zapewnić, pobierane poprawne zdarzeń.

Jeśli masz pytania dotyczące integracji dziennika Azure, Wyślij wiadomość e-mail do [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

<!--Image references-->
[1]: ./media/security-azure-log-integration-faq/event-xml.png
