<properties 
    pageTitle="Jak skonfigurować usługi w chmurze (portal) | Microsoft Azure" 
    description="Dowiedz się, jak skonfigurować usług w chmurze w Azure. Dowiedz się, jak zaktualizować konfigurację usługi w chmurze i konfigurowanie dostępu zdalnego do roli wystąpień. W tych przykładach użyto Azure portal." 
    services="cloud-services" 
    documentationCenter="" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016"
    ms.author="adegeo"/>

# <a name="how-to-configure-cloud-services"></a>Jak skonfigurować usług w chmurze

> [AZURE.SELECTOR]
- [Azure portal](cloud-services-how-to-configure-portal.md)
- [Portal Azure klasyczny](cloud-services-how-to-configure.md)

W portalu Azure można skonfigurować najczęściej używane ustawienia usługi w chmurze. Lub, jeśli chcesz zaktualizować pliki konfiguracji bezpośrednio, Pobierz plik konfiguracji usługi, aby zaktualizować, a następnie przekaż zaktualizowanego pliku i zaktualizować usługi w chmurze za pomocą zmiany konfiguracji. W obu przypadkach aktualizacji konfiguracji są przenoszone do wszystkich wystąpień roli.

Można również zarządzać wystąpienia role usługi w chmurze lub pulpitu zdalnego do nich.

Jeśli masz co najmniej dwa wystąpienia ról dla każdej roli Azure tylko zapewnia dostępność usługi procent 99,95 podczas aktualizowania konfiguracji. Umożliwiające maszyn wirtualnych przetwarzania żądania klienta drugi się podczas aktualizacji. Aby uzyskać więcej informacji zobacz [Umowy](https://azure.microsoft.com/support/legal/sla/).

## <a name="change-a-cloud-service"></a>Zmienianie usługi w chmurze

Po otwarciu [Azure portal](https://portal.azure.com/), przejdź do usługi w chmurze. W tym miejscu możesz zarządzać wielu aspektów. 

![Ustawienia strony](./media/cloud-services-how-to-configure-portal/cloud-service.png)

**Ustawienia** lub **wszystkie ustawienia** łącza otworzy karta **Ustawienia** miejsce, w którym można zmienić **Właściwości**, zmień **konfigurację**, zarządzanie **Certyfikaty**, ustawienia **reguł alertów**i zarządzanie **użytkowników** , którzy mają dostęp do usługi w chmurze.

![Karta Ustawienia usługi Azure chmury](./media/cloud-services-how-to-configure-portal/cs-settings-blade.png)

>[AZURE.NOTE]
>Nie można zmienić system operacyjny używany dla usług w chmurze za pomocą **Azure portal**, mogą tylko zmienić to ustawienie, za pomocą [portal Azure klasyczny](http://manage.windowsazure.com/). Jest to szczegółowe [tutaj](cloud-services-how-to-configure.md#update-a-cloud-service-configuration-file).

## <a name="monitoring"></a>Monitorowanie

Alerty można dodać do usługi w chmurze. Kliknij pozycję **Ustawienia** > **Reguły alertu** > **Dodaj alert**. 

![](./media/cloud-services-how-to-configure-portal/cs-alerts.png)

W tym miejscu możesz skonfigurować alert. Z pola listy rozwijanej **Mertic** możesz skonfigurować alert dla następujących typów danych.

- Odczytu dysku
- Zapisu na dysku
- Sieci w
- Sieci się
- Procesor procent 

![](./media/cloud-services-how-to-configure-portal/cs-alert-item.png)

### <a name="configure-monitoring-from-a-metric-tile"></a>Konfigurowanie monitorowania metryczne kafelków

Zamiast używać **Ustawienia** > **Reguł alertów**, można kliknąć jeden z kafelków metryki w sekcji **Monitorowanie** karta **usługi w chmurze** .

![Usługa w chmurze monitorowania](./media/cloud-services-how-to-configure-portal/cs-monitoring.png)

W tym miejscu możesz dostosować wykres z fragmentu lub dodać regułę alertu.


## <a name="reboot-reimage-or-remote-desktop"></a>Uruchom ponownie komputer, reimage lub pulpitu zdalnego

W tej chwili nie można skonfigurować za pomocą **Azure portal**pulpitu zdalnego. Jednak możesz skonfigurować ją przez [portal Azure klasycznego](cloud-services-role-enable-remote-desktop.md) [programu PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)lub w ramach [Programu Visual Studio](../vs-azure-tools-remote-desktop-roles.md). 

Najpierw kliknij wystąpienie usługi w chmurze.

![Wystąpienie usługi w chmurze](./media/cloud-services-how-to-configure-portal/cs-instance.png)

Z karta że otwiera uou można zainicjować Podłączanie pulpitu zdalnego, zdalnie uruchom ponownie wystąpienie lub zdalnie reimage (Uruchom z obrazem świeży) wystąpienie.

![Przyciski wystąpienie usługi chmury](./media/cloud-services-how-to-configure-portal/cs-instance-buttons.png)



## <a name="reconfigure-your-cscfg"></a>Skonfigurowanie usługi .cscfg

Konieczne może być skonfigurowanie możesz usługa w chmurze za pomocą pliku [konfiguracji usługi (cscfg)](cloud-services-model-and-package.md#cscfg) . Musisz najpierw pobrać plik .cscfg, zmodyfikuj ją, a następnie przekaż go.

1. Kliknij ikonę **Ustawienia** lub łącze **wszystkie ustawienia** , aby otworzyć konto karta **Ustawienia** .

    ![Ustawienia strony](./media/cloud-services-how-to-configure-portal/cloud-service.png)

2. Kliknij pozycję **konfiguracji** .

    ![Karta konfiguracji](./media/cloud-services-how-to-configure-portal/cs-settings-config.png)

3. Kliknij przycisk **Pobierz** .

    ![Plik do pobrania](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-download.png)

4. Po zaktualizowaniu plik konfiguracyjny usługi przekazywanie i zastosować aktualizacje konfiguracji:

    ![Przekazywanie](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-upload.png) 
    
5. Zaznacz plik .cscfg i kliknij **przycisk OK**.

            
## <a name="next-steps"></a>Następne kroki

* Dowiedz się, jak [wdrożyć usługi w chmurze](cloud-services-how-to-create-deploy-portal.md).
* Skonfiguruj [niestandardową nazwę domeny](cloud-services-custom-domain-name-portal.md).
* [Zarządzanie usługi w chmurze](cloud-services-how-to-manage-portal.md).
* Konfigurowanie [certyfikatów ssl](cloud-services-configure-ssl-certificate-portal.md).