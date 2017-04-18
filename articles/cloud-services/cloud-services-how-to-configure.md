<properties 
    pageTitle="Jak skonfigurować usługi w chmurze (klasyczny portal) | Microsoft Azure" 
    description="Dowiedz się, jak skonfigurować usług w chmurze w Azure. Dowiedz się, jak zaktualizować konfigurację usługi w chmurze i konfigurowanie dostępu zdalnego do roli wystąpień." 
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

W portalu klasyczny Azure można skonfigurować najczęściej używane ustawienia usługi w chmurze. Lub, jeśli chcesz zaktualizować pliki konfiguracji bezpośrednio, Pobierz plik konfiguracji usługi, aby zaktualizować, a następnie przekaż zaktualizowanego pliku i zaktualizować usługi w chmurze za pomocą zmiany konfiguracji. W obu przypadkach aktualizacji konfiguracji są przenoszone do wszystkich wystąpień roli.

Azure portalu Klasyczny umożliwia [Włączenie Podłączanie pulpitu zdalnego dla roli w usług w chmurze Azure](cloud-services-role-enable-remote-desktop.md)

Jeśli masz co najmniej dwa wystąpienia ról dla każdej roli Azure tylko zapewnia dostępność usługi procent 99,95 podczas aktualizowania konfiguracji. Umożliwiająca maszyn wirtualnych przetwarzania żądania klienta drugi się podczas aktualizacji. Aby uzyskać więcej informacji zobacz [Świadczeniu](https://azure.microsoft.com/support/legal/sla/).

## <a name="change-a-cloud-service"></a>Zmienianie usługi w chmurze

1. W [portal Azure klasyczny](http://manage.windowsazure.com/)kliknij **Usług w chmurze**, kliknij nazwę usługi w chmurze i kliknij przycisk **Konfiguruj**.

    ![Strona konfiguracji](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage1.png)
    
    Na stronie **Konfigurowanie** można skonfigurować monitorowania, zaktualizuj ustawienia roli i wybierz system operacyjny gościa i rodziny wystąpienia roli. 

2. **Monitorowanie**Ustaw poziom monitorowania pełne lub minimalnego i skonfigurować diagnostyki ciągów połączeń, które są wymagane do pełnego monitorowania.

3. W przypadku usługi ról (pogrupowane według roli) można aktualizować następujące ustawienia:
    
    >**Ustawienia**  
    >Zmodyfikuj wartości różne ustawienia, które uwzględniono w elementach *appSettings* plik konfiguracyjny (.cscfg) usługi.
    >
    >**Certyfikaty**  
    >Zmienianie odcisku palca certyfikat, który jest używany w szyfrowania SSL dla roli. Aby zmienić certyfikatu, możesz przekazać nowego certyfikatu (na stronie **Certyfikaty** ). Następnie zaktualizuj odcisku palca w ciągu certyfikat wyświetlane w obszarze Ustawienia roli.

4. W **systemie operacyjnym**Zmienianie rodziny systemu operacyjnego lub wersji dla roli wystąpienia lub wybrać opcję **automatycznego** Aby włączyć automatyczne aktualizowanie wersję bieżącego systemu operacyjnego. Ustawienia systemu operacyjnego dotyczą ról w sieci web i pracownika, ale nie ma wpływu na maszyn wirtualnych.

    Podczas rozmieszczania najnowsza wersja systemu operacyjnego jest zainstalowana na wszystkich wystąpień roli i systemy operacyjne są aktualizowane automatycznie domyślnie. 
    
    Jeśli potrzebujesz tej usługi cloud uruchomienie wymagania dotyczące zgodności w kodzie w wersji systemów operacyjnych, możesz wybrać rodzinie systemu operacyjnego i wersji. Po wybraniu wersji określonego systemu operacyjnego, są zawieszone aktualizacje automatyczne system operacyjny dla usług w chmurze. Należy upewnić się, że systemy operacyjne otrzymywać aktualizacje.
    
    Jeśli rozwiązać wszystkie problemy ze zgodnością, których Twoje aplikacje z najbardziej aktualną wersję systemu operacyjnego, możesz włączyć Aktualizacje automatyczne systemu operacyjnego, ustawiając wersję systemu operacyjnego na **Automatyczne**. 
    
    ![Ustawienia systemu operacyjnego](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage_OSSettings.png)

5. Aby zapisać ustawienia konfiguracji i przekazać je do wystąpienia roli, kliknij przycisk **Zapisz**. (Kliknij **pozycję Odrzuć,** Aby anulować zmiany). **Zapisywanie** i **Odrzucanie** zostaną dodane do paska poleceń po zmianie tego ustawienia.

## <a name="update-a-cloud-service-configuration-file"></a>Aktualizowanie pliku konfiguracji usługi chmury

1. Pobierz chmury usługi pliku konfiguracji (.cscfg) z bieżącej konfiguracji. Na stronie **Konfigurowanie** usług w chmurze kliknij przycisk **Pobierz**. Następnie kliknij przycisk **Zapisz**lub kliknij pozycję **Zapisz jako** do zapisania pliku.

2. Po zaktualizowaniu plik konfiguracyjny usługi przekazywanie i zastosować aktualizacje konfiguracji:

    1. Na stronie **Konfigurowanie** kliknij przycisk **Przekaż**.
    
        ![Przekazywanie konfiguracji](./media/cloud-services-how-to-configure/CloudServices_UploadConfigFile.png)
    
    2. W **pliku konfiguracji**użyj **przycisku Przeglądaj** , zaznacz plik .cscfg zaktualizowane.
    
    3. Jeśli usługa w chmurze zawiera wszystkie role, mające tylko jedno wystąpienie, zaznacz pole wyboru **Zastosuj konfigurację, nawet jeśli jedną lub więcej ról zawierają jedno wystąpienie** , aby włączyć aktualizacje konfiguracji dla ról kontynuować.
    
        Chyba że zdefiniujesz co najmniej dwa wystąpienia każdej roli Azure nie daje gwarancji co najmniej 99,95% dostępności usługi w chmurze podczas aktualizowania konfiguracji usługi. Aby uzyskać więcej informacji zobacz [Świadczeniu](https://azure.microsoft.com/support/legal/sla/).
    
    4. Kliknij przycisk **OK** (znacznik wyboru). 


## <a name="next-steps"></a>Następne kroki

* Dowiedz się, jak [wdrożyć usługi w chmurze](cloud-services-how-to-create-deploy.md).
* Skonfiguruj [niestandardową nazwę domeny](cloud-services-custom-domain-name.md).
* [Zarządzanie usługi w chmurze](cloud-services-how-to-manage.md).
* [Włączanie Podłączanie pulpitu zdalnego dla roli w usług w chmurze Azure](cloud-services-role-enable-remote-desktop.md)
* Konfigurowanie [certyfikatów ssl](cloud-services-configure-ssl-certificate.md).
