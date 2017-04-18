<properties
   pageTitle="Przekazywanie poświadczeń Azure za pomocą DSC | Microsoft Azure"
   description="Omówienie bezpieczne przekazywania poświadczeń do Azure maszyn wirtualnych przy użyciu programu PowerShell potrzeby stan konfiguracji"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="passing-credentials-to-the-azure-dsc-extension-handler"></a>Przekazywania poświadczeń do obsługi rozszerzenia Azure DSC #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

W tym artykule opisano rozszerzenie potrzeby konfiguracji stanu dla Azure. Omówienie obsługi rozszerzenia DSC znajdują się na [Wprowadzenie do obsługi rozszerzenia Azure potrzeby stan konfiguracji](virtual-machines-windows-extensions-dsc-overview.md). 


## <a name="passing-in-credentials"></a>Przekazanie poświadczeń
W ramach procesu konfiguracji możesz musi skonfigurować konta użytkowników, uzyskiwać dostęp do usług lub zainstaluj program w kontekście użytkownika. Aby wykonać poniższe czynności, musisz podać poświadczenia. 

DSC umożliwia parametryczne konfiguracji, w których poświadczenia są przekazywane do konfiguracji i bezpiecznie przechowywane w plikach MOF. Obsługa rozszerzenia Azure upraszcza zarządzanie poświadczeń, dostarczając automatyczne zarządzanie certyfikaty. 

Należy rozważyć następujące skrypt konfiguracji DSC, który tworzy konto użytkownika lokalnego przy użyciu określonego hasła:

*user_configuration.ps1*

```
configuration Main
{
    param(
        [Parameter(Mandatory=$true)]
        [ValidateNotNullorEmpty()]
        [PSCredential]
        $Credential
    )    
    Node localhost {       
        User LocalUserAccount
        {
            Username = $Credential.UserName
            Password = $Credential
            Disabled = $false
            Ensure = "Present"
            FullName = "Local User Account"
            Description = "Local User Account"
            PasswordNeverExpires = $true
        } 
    }  
} 
```

Należy uwzględnić *węzeł hosta lokalnego* jako część konfiguracji. Jeśli brakuje tej instrukcji, następujące czynności nie działają jak obsługa rozszerzenia specjalnie wyszukuje instrukcji host lokalny węzeł. Należy również do uwzględnienia typecast *[parametr PsCredential]*zgodnie z określonego typu wyzwalane rozszerzenia do szyfrowania poświadczeń. 

Publikowanie tego skryptu magazyn obiektów blob:

`Publish-AzureVMDscConfiguration -ConfigurationPath .\user_configuration.ps1`

Ustawianie rozszerzenia Azure DSC i podaj poświadczenia:

```
$configurationName = "Main"
$configurationArguments = @{ Credential = Get-Credential }
$configurationArchive = "user_configuration.ps1.zip"
$vm = Get-AzureVM "example-1"
 
$vm = Set-AzureVMDSCExtension -VM $vm -ConfigurationArchive $configurationArchive 
-ConfigurationName $configurationName -ConfigurationArgument @configurationArguments
 
$vm | Update-AzureVM
```
## <a name="how-credentials-are-secured"></a>Jak są zabezpieczone poświadczeń
Uruchomienie tego kodu monituje o podanie poświadczeń. Gdy podano, jest ona przechowywana w pamięci krótko. Kiedy jest publikowana z `Set-AzureVmDscExtension` polecenia cmdlet, jego przesyłane HTTPS do maszyn wirtualnych, gdzie Azure zapisuje go szyfrowane na dysku, przy użyciu lokalnego certyfikatu maszyn wirtualnych. Następnie jest krótko odszyfrować w pamięci i jej ponowne zaszyfrowanie w celu przekazania go do DSC.

To zachowanie jest inne niż [za pomocą bezpieczne konfiguracje bez obsługi rozszerzenia](https://msdn.microsoft.com/powershell/dsc/securemof). Środowisko Azure zapewnia sposób przesyłania danych konfiguracji bezpieczne przy użyciu certyfikatów. Gdy używasz obsługi rozszerzenia DSC, istnieje potrzeba zapewniające $CertificatePath lub $CertificateID / $Thumbprint wpisowi ConfigurationData.


## <a name="next-steps"></a>Następne kroki ##

Aby uzyskać więcej informacji na obsługi rozszerzenia Azure DSC zobacz [Wprowadzenie do obsługi rozszerzenia Azure potrzeby stan konfiguracji](virtual-machines-windows-extensions-dsc-overview.md). 

Sprawdź [Menedżera zasobów Azure szablon z rozszerzeniem DSC](virtual-machines-windows-extensions-dsc-template.md).

Aby uzyskać więcej informacji na temat programu PowerShell DSC, [odwiedź witrynę Centrum dokumentacji programu PowerShell](https://msdn.microsoft.com/powershell/dsc/overview). 

Aby znaleźć dodatkowe funkcje można zarządzać przy użyciu programu PowerShell DSC, [Przejdź w galerii programu PowerShell](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) dla większej ilości zasobów DSC.
