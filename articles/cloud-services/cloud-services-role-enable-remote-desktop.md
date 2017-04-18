<properties 
pageTitle="Włączanie Podłączanie pulpitu zdalnego dla roli w usług w chmurze Azure" 
description="Jak skonfigurować aplikację usługi azure chmurze, aby umożliwić połączenia pulpitu zdalnego" 
services="cloud-services" 
documentationCenter="" 
authors="sbtron" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="02/17/2016" 
ms.author="saurabh"/>

# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a>Włączanie Podłączanie pulpitu zdalnego dla roli w usług w chmurze Azure

>[AZURE.SELECTOR]
- [Portal Azure klasyczny](cloud-services-role-enable-remote-desktop.md)
- [Programu PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
- [Programu Visual Studio](../vs-azure-tools-remote-desktop-roles.md)


Pulpit zdalny pozwala uzyskać dostęp do pulpitu roli działa Azure. Podłączanie pulpitu zdalnego umożliwia rozwiązywanie problemów i diagnozowanie problemów z aplikacją, gdy jest uruchomiony. 

Możesz włączyć połączenia pulpitu zdalnego w swojej roli podczas opracowywania dodając moduły pulpitu zdalnego w swojej definicji usługi lub można włączyć Pulpit zdalny za pośrednictwem rozszerzenia pulpitu zdalnego. Preferowana jest użycie rozszerzenia pulpitu zdalnego, jak można włączyć pulpitu zdalnego, nawet po wdrożeniu aplikacji bez konieczności ponownego wdrażania aplikacji. 


## <a name="configure-remote-desktop-from-the-azure-classic-portal"></a>Konfigurowanie pulpitu zdalnego z portalu klasycznym Azure
Klasyczny portalu Azure wykorzystuje metodę rozszerzenia pulpitu zdalnego, więc można włączyć pulpitu zdalnego, nawet po wdrożeniu aplikacji. Na stronie **Konfigurowanie** usługi w chmurze pozwala na włączanie pulpitu zdalnego, zmień lokalnego konta administratora używane do łączenia się maszyn wirtualnych, certyfikat używany w przypadku uwierzytelniania i ustawianie daty wygaśnięcia. 


1. Kliknij **Usług w chmurze**, kliknij nazwę usługi w chmurze, a następnie kliknij przycisk **Konfiguruj**.

2. Kliknij pozycję **zdalnym**.
    
    ![Zdalny usług w chmurze](./media/cloud-services-role-enable-remote-desktop/CloudServices_Remote.png)
    
    > [AZURE.WARNING] Wszystkie wystąpienia rola zostanie uruchomiony ponownie po najpierw włączyć Pulpit zdalny i kliknij przycisk OK (znacznik wyboru). Aby zapobiec Uruchom ponownie komputer, certyfikatu służącego do szyfrowania hasła musi być zainstalowany na rolę. Aby uniemożliwić ponowne uruchomienie komputera, [przekazywanie certyfikatu usługi w chmurze](cloud-services-how-to-create-deploy/#how-to-upload-a-certificate-for-a-cloud-service) , a następnie powróć do tego okna dialogowego.
    

3. **Role**wybierz rolę, którą chcesz zaktualizować, lub zaznacz **Wszystkie** dla wszystkich ról.

4. Wprowadź jedną z następujących zmian:
    
    - Aby włączyć pulpitu zdalnego, zaznacz pole wyboru **Włącz pulpit zdalny** . Aby wyłączyć pulpitu zdalnego, wyczyść pole wyboru.
    
    - Utwórz konto, które będzie używane w połączeniach pulpitu zdalnego wystąpieniach roli.
    
    - Zaktualizuj hasło do istniejącego konta.
    
    - Wybierz certyfikat przekazane uwierzytelniania (Przekaż certyfikat przy użyciu **przekazać** na stronie **Certyfikaty** ) lub utworzeniu nowego certyfikatu. 
    
    - Zmiana daty wygaśnięcia dla konfiguracji pulpitu zdalnego.

5. Po zakończeniu konfiguracji aktualizacje, kliknij przycisk **OK** (znacznik wyboru).


## <a name="remote-into-role-instances"></a>Zdalne do roli wystąpienia
Po włączeniu pulpitu zdalnego ról możesz zdalnego do wystąpieniem roli za pomocą różnych narzędzi.

Aby połączyć się wystąpieniem roli z portalu klasyczny Azure:
    
  1.   Kliknij przycisk **wystąpień** , aby otworzyć stronę **wystąpienia** .
  2.   Wybierz wystąpienie roli zawierającej pulpitu zdalnego skonfigurowane.
  3.   Kliknij pozycję **Połącz**, a następnie postępuj zgodnie z instrukcjami, aby otworzyć pulpitu. 
  4.   Kliknij przycisk **Otwórz** , a następnie **Połącz** , aby rozpocząć połączenie pulpitu zdalnego. 


### <a name="use-visual-studio-to-remote-into-a-role-instance"></a>Za pomocą programu Visual Studio zdalnego do wystąpienia roli

W programie Visual Studio Eksploratora serwera:

1. Rozwijanie **Azure\\usług w chmurze\\[nazwa usługi cloud]** węzeł.
2. Rozwiń **tymczasowym** lub **produkcji**.
3. Rozwijanie poszczególnych ról.
4. Kliknij prawym przyciskiem myszy wystąpienia roli, kliknij pozycję **Połącz z pulpitu zdalnego...**, a następnie wprowadź nazwę użytkownika i hasło. 

![Pulpit zdalny Eksploratora serwera](./media/cloud-services-role-enable-remote-desktop/ServerExplorer_RemoteDesktop.png)


### <a name="use-powershell-to-get-the-rdp-file"></a>Pobierz plik RDP za pomocą programu PowerShell
Za pomocą polecenia cmdlet [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) do pobierania pliku RDP. Następnie umożliwia pliku RDP z Podłączanie pulpitu zdalnego dostępu do usług w chmurze.

### <a name="programmatically-download-the-rdp-file-through-the-service-management-rest-api"></a>Programowo pobrać plik RDP za pośrednictwem interfejsu API usługi REST zarządzania usługi
[Pobierz plik RDP](https://msdn.microsoft.com/library/jj157183.aspx) operacji pozostałych umożliwia pobranie pliku RDP. 



## <a name="to-configure-remote-desktop-in-the-service-definition-file"></a>Konfigurowanie pulpitu zdalnego w pliku definicji usługi

Ta metoda pozwala na włączanie pulpitu zdalnego dla aplikacji w czasie projektowania. Ta metoda wymaga szyfrowania haseł można przechowywać w konfiguracji usługi plików i wszelkie aktualizacje konfigurację pulpitu zdalnego wymaga ponownego wdrażania aplikacji. Jeśli chcesz uniknąć tych downsides należy używać metody zdalnego rozszerzenia pulpitu podstawie opisany powyżej.  

Visual Studio umożliwia [włączenie połączenia pulpitu zdalnego](../vs-azure-tools-remote-desktop-roles.md) przy użyciu metody pliku definicji usługi.  
Poniżej opisano zmiany konieczne dla plików modelu usługi, aby włączyć Pulpit zdalny. Program Visual Studio będzie automatycznie zmienia te podczas publikowania.

### <a name="set-up-the-connection-in-the-service-model"></a>Konfigurowanie połączenia w modelu usługi 
Element **Importowanie** umożliwia zaimportowanie moduł **dostęp zdalny** i moduł **RemoteForwarder** do pliku [ServiceDefinition.csdef](cloud-services-model-and-package.md#csdef) .

Plik definicji usługi powinna być podobna do następującego przykładu z `<Imports>` element dodany.

```xml
<ServiceDefinition name="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2013-03.2.0">
    <WebRole name="WebRole1" vmsize="Small">
        <Sites>
            <Site name="Web">
                <Bindings>
                    <Binding name="Endpoint1" endpointName="Endpoint1" />
                </Bindings>
            </Site>
        </Sites>
        <Endpoints>
            <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
            <Import moduleName="Diagnostics" />
            <Import moduleName="RemoteAccess" />
            <Import moduleName="RemoteForwarder" />
        </Imports>
    </WebRole>
</ServiceDefinition>
```
Plik [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#cscfg) powinien być podobny do następującego przykładu, zwróć uwagę `<ConfigurationSettings>` i `<Certificates>` elementy. Certyfikat określony należy [przekazać do usługi w chmurze](../cloud-services-how-to-create-deploy.md#how-to-upload-a-certificate-for-a-cloud-service).

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2013-03.2.0">
    <Role name="WebRole1">
        <Instances count="2" />
        <ConfigurationSettings>
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.Enabled" value="true" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountUsername" value="[name-of-user-account]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountEncryptedPassword" value="[base-64-encrypted-user-password]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountExpiration" value="[certificate-expiration]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteForwarder.Enabled" value="true" />
        </ConfigurationSettings>
        <Certificates>
            <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="[certificate-thumbprint]" thumbprintAlgorithm="sha1" />
        </Certificates>
    </Role>
</ServiceConfiguration>
```


## <a name="additional-resources"></a>Dodatkowe zasoby

[Jak skonfigurować usług w chmurze](cloud-services-how-to-configure.md)