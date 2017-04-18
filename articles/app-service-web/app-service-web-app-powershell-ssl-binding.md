<properties
    pageTitle="Powiązanie certyfikatów SSL przy użyciu programu PowerShell"
    description="Dowiedz się, jak chcesz powiązać certyfikatów SSL aplikacji sieci web przy użyciu programu PowerShell."
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/13/2016"
    ms.author="ahmedelnably"/>

# <a name="azure-app-service-ssl-certificate-binding-using-powershell"></a>Azure aplikacji usługi SSL powiązanie certyfikatu przy użyciu programu PowerShell #

W wersji programu Microsoft Azure programu PowerShell wersji 1.1.0 nowe polecenia cmdlet zostały dodane którą sposób umożliwiania użytkownika powiązać istniejącym lub nowym certyfikatów SSL z istniejącej aplikacji sieci Web.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

Aby informacje o korzystaniu z Menedżera zasobów Azure podstawą Azure programu PowerShell polecenia cmdlet, aby zarządzać swoimi aplikacjami sieci Web zaznacz pozycję [Menedżer zasobów Azure podstawie polecenia programu PowerShell dla aplikacji sieci Web Azure](app-service-web-app-azure-resource-manager-powershell.md)

## <a name="uploading-and-binding-a-new-ssl-certificate"></a>Przekazywanie i wiązanie nowego certyfikatu SSL ##

Scenariusz: Użytkownik chce powiązać certyfikatu SSL z jednej z jego aplikacji sieci web.

Informacje o nazwy grupy zasobów, która zawiera aplikacji sieci web, nazwa aplikacji sieci web, ścieżka do pliku PFX certyfikat na komputerze użytkownika, hasło do certyfikatu i niestandardowe hostname, możemy Użyj następującego polecenia programu PowerShell do utworzenia tej powiązanie SSL:

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -CertificateFilePath PathToPfxFile -CertificatePassword PlainTextPwd -Name www.contoso.com

Należy zauważyć, że przed dodaniem powiązanie SSL do aplikacji sieci web, musi mieć nazwę hosta (domeny niestandardowej) już skonfigurowane. Jeśli nie skonfigurowano nazwę hosta, a następnie zostanie wyświetlony komunikat o błędzie "hostname" nie istnieje uruchomienia AzureRmWebAppSSLBinding nowy. Nazwa hosta można dodać bezpośrednio z portalu lub przy użyciu programu PowerShell Azure. Poniższy fragment programu PowerShell można skonfigurować przed uruchomieniem AzureRmWebAppSSLBinding Nowa nazwa hosta.   
  
    $webApp = Get-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup  
    $hostNames = $webApp.HostNames  
    $HostNames.Add("www.contoso.com")  
    Set-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup -HostNames $HostNames   
  
Jest ważne dowiedzieć się, że polecenia cmdlet Set-AzureRmWebApp zastępuje nazwy hostów dla aplikacji sieci web. W związku z tym wstawkę kodu programu PowerShell podanych jest dołączanie do istniejącej listy nazw hostów dla aplikacji sieci web.  

## <a name="uploading-and-binding-an-existing-ssl-certificate"></a>Przekazywanie i wiązanie istniejący certyfikat SSL ##

Scenariusz: Użytkownik chce powiązać wcześniej przekazane certyfikatu SSL do jednej z jego aplikacji sieci web.

Będziemy mogli korzystać z listy certyfikatów już przekazane do określonej grupy zasobów przy użyciu następującego polecenia

    Get-AzureRmWebAppCertificate -ResourceGroupName myresourcegroup

Należy zauważyć, że certyfikaty są lokalne do określonej lokalizacji i grupa zasobów, użytkownik musi ponownie przekazać certyfikatu w przypadku aplikacji sieci web skonfigurowaną w innej lokalizacji zasobów grupy inne niż certyfikatu wymaganego 

Informacje o nazwy grupy zasobów, która zawiera aplikacji sieci web, nazwa aplikacji sieci web, odcisk palca certyfikatu i niestandardowe hostname, firma Microsoft Użyj następującego polecenia programu PowerShell do utworzenia tej powiązanie SSL:

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Thumbprint <certificate thumbprint> -Name www.contoso.com

## <a name="deleting-an-existing-ssl-binding"></a>Usuwanie istniejącej powiązanie SSL  ##

Scenariusz: Użytkownik chce usunąć istniejące powiązanie SSL.

Informacje o nazwy grupy zasobów, która zawiera aplikacji sieci web, nazwa aplikacji sieci web i niestandardowe hostname, możemy Użyj następującego polecenia programu PowerShell usunąć tego powiązania SSL:

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com

Należy zauważyć, że jeśli usunięto powiązanie SSL został ostatni powiązania przy użyciu tego certyfikatu w tej lokalizacji, domyślnie certyfikat zostanie usunięty, jeśli użytkownik chce zachować certyfikatu, że on zachować certyfikat za pomocą opcji DeleteCertificate

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com -DeleteCertificate $false

### <a name="references"></a>Odwołania ###
- [Menedżer zasobów Azure podstawie polecenia programu PowerShell dla aplikacji sieci Web Azure](app-service-web-app-azure-resource-manager-powershell.md)
- [Wprowadzenie do środowiska aplikacji usługi](app-service-app-service-environment-intro.md)
- [Przy użyciu programu PowerShell Azure przy użyciu Menedżera zasobów Azure](../powershell-azure-resource-manager.md)
