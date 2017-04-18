<properties 
    pageTitle="Jak utworzyć ASE ILB Azure szablonom Menedżera zasobów | Microsoft Azure" 
    description="Dowiedz się, jak utworzyć ASE równoważenia obciążenia wewnętrznych korzystania z szablonów Azure Menedżera zasobów." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="nirma" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="stefsch"/>   

# <a name="how-to-create-an-ilb-ase-using-azure-resource-manager-templates"></a>Jak utworzyć ASE ILB Azure szablonom Menedżera zasobów

## <a name="overview"></a>Omówienie ##
Środowiska usługi aplikacji mogą być tworzone z adresem wewnętrznych wirtualną sieć zamiast VIP publicznej.  Ten adres wewnętrzny jest udostępniany przez składnik Azure o nazwie równoważenia obciążenia wewnętrzny (ILB).  ASE ILB można tworzyć za pomocą portalu Azure.  Można także tworzyć, przy użyciu automatyzacji przez Menedżera zasobów Azure szablonów.  W tym artykule opisano czynności i składni potrzebne do utworzenia ASE ILB z szablonami Azure Menedżera zasobów.

Istnieją trzy kroki zajmujących się Automatyzowanie tworzenia ASE ILB:
1. Najpierw ASE podstawowy zostanie utworzona w wirtualnej sieci przy użyciu adresu równoważenia obciążenia wewnętrznych zamiast VIP publicznej.  W ramach tego kroku nazwa domeny głównej przydzielono ILB ASE.
2. Po utworzeniu ILB ASE przekazaniu certyfikat SSL.  
3. Przekazane certyfikat SSL przydzielono jawnie ILB ASE jako jego certyfikat SSL "wartość domyślna".  Ten certyfikat SSL będzie używana dla ruchu protokołu SSL do aplikacji na ILB ASE, jeśli aplikacje zostały rozwiązane, przy użyciu typowych domeny głównej przypisane do ASE (np. https://someapp.mycustomrootcomain.com)

## <a name="creating-the-base-ilb-ase"></a>Tworzenie podstawy ILB ASE ##
Przykład Menedżera zasobów Azure szablonu i jego pliku z nimi parametrów są dostępne w GitHub [poniżej][quickstartilbasecreate].

Większość parametrów w pliku *azuredeploy.parameters.json* są wspólne dla tworzenia zarówno ILB ASEs, jak i ASEs powiązane z publicznej VIP.  Wymienione poniżej połączenia się z parametrów specjalna uwaga lub które są unikatowe, podczas tworzenia ASE ILB:


- *interalLoadBalancingMode*: W większości przypadków będą powiązane Ustaw tę opcję, aby 3, co oznacza zarówno ruch protokołu HTTP/HTTPS porty 80 i 443, a dane i kontroli porty kanału słuchania przez usługę FTP na ASE, ILB przydzielonego adresu wewnętrznego wirtualnej sieci.  Jeśli ta właściwość zamiast tego jest ustawiona na 2, usługę FTP związane porty (kanały zarówno kontrolka, jak i danych) będzie można powiązać z adresu ILB podczas ruchu protokołu HTTP/HTTPS pozostanie na publicznej VIP.
-  *dnsSuffix*: ten parametr określa domeny głównej domyślny przypisany do ASE.  W publicznej zmiany Azure aplikacji usługi domeny głównej domyślne dla wszystkich aplikacji sieci web jest *azurewebsites.net*.  Jednak ponieważ ASE ILB jest wewnętrzne wirtualnej sieci klienta, nie zrozumiałe używać domeny głównej domyślnych usług publicznych.  Zamiast tego ASE ILB powinien mieć domyślnej domeny głównej, dostosowaną do użytku w obrębie wewnętrznych wirtualną sieć firmy.  Na przykład teoretyczna firmy Contoso użyć domeny głównej domyślne *wewnętrznych* contoso.com dla aplikacji, które mają być tylko rozpoznawalne i dostępne w wirtualnej sieci firmy Contoso. 
-  *ipSslAddressCount*: ten parametr jest automatycznie ustawiana domyślnie wartość 0 w polu plik *azuredeploy.json* ponieważ ILB ASEs mieć tylko jeden adres ILB.  Nie ma żadnych jawnych adresów IP SSL dla ASE ILB i w związku z tym puli adresów IP SSL dla ASE ILB należy ustawić na zero, w przeciwnym razie wystąpi błąd obsługi administracyjnej. 

Gdy plik *azuredeploy.parameters.json* został wprowadzony dla ASE ILB, ILB ASE następnie można tworzyć przy użyciu następujących wstawkę kodu programu Powershell.  Zmienianie plików ścieżek zgodnie z lokalizacji plików szablonów Azure Menedżera zasobów na tym komputerze.  Pamiętaj również o podanie własne wartości dla nazwy wdrożenia Menedżera zasobów Azure i nazwa grupy zasobów.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"
    
    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Po Menedżera zasobów Azure szablon został przesłany, że zajmie kilka godzin, ASE ILB ma zostać utworzony.  Po zakończeniu tworzenia ILB ASE pojawi się w portalu interfejsu użytkownika na liście aplikacji środowiska usługi subskrypcji, którego dotyczy wdrożenia.

## <a name="uploading-and-configuring-the-default-ssl-certificate"></a>Przekazywanie i konfigurowanie certyfikatu SSL "Wartość domyślna" ##

Po utworzeniu ILB ASE certyfikat SSL powinny być skojarzone z ASE, jak używa "domyślne" certyfikatu SSL do nawiązywania połączeń SSL do aplikacji.  Kontynuowanie teoretyczna przykład Contoso Corporation, jeśli ASE domyślne DNS jednostka_urojona jest *wewnętrzny contoso.com*, a następnie połączenie *https://some-random-app.internal-contoso.com* wymaga certyfikatu SSL właściwy dla **.internal contoso.com*. 

Istnieje szereg sposobów uzyskanie ważnego certyfikatu SSL tym wewnętrznych urzędów certyfikacji, zakup od zewnętrznego wystawcy certyfikatu, a za pomocą certyfikatu z podpisem własnym.  Bez względu na to źródło certyfikatu SSL następujące atrybuty certyfikatu należy poprawnie skonfigurować:

- *Temat*: Ustaw ten atrybut **.your główny domeny here.com*
- *Alternatywna nazwa podmiotu*: ten atrybut może zawierać zarówno * *.your główny domeny here.com*i * *.scm.your-główny-domeny-here.com*.  Powód drugiej pozycji to, że zostaną wprowadzone połączenia SSL do witryny Menedżer sterowania usługami i Kudu skojarzone z każdą z nich przy użyciu adresu formularza *your-app-name.scm.your-root-domain-here.com*.

Nieprawidłowy certyfikat SSL w kształcie dłoni potrzebne są dwie dodatkowe czynności przygotowawczych.  Certyfikat SSL musi zostać przekonwertowany i zapisany jako plik pfx.  Należy pamiętać, musi zawierać wszystkie pośrednie i główne certyfikaty plik PFX, a także musi być zabezpieczone hasłem.

Następnie plik PFX wynikowy musi można przekonwertować na ciąg base64, ponieważ certyfikat SSL zostaną przekazane przy użyciu szablonu Azure Menedżera zasobów.  Ponieważ Menedżer zasobów Azure szablony są to pliki tekstowe, plik PFX potrzebuje do przekonwertowania na ciąg base64, może zostać dołączony jako parametr szablonu.

Wstawkę kodu programu Powershell poniżej przedstawiono przykład generowania certyfikat z podpisem własnym, eksportowanie certyfikatu jako plik PFX ciąg zakodowany w formacie base64 konwertowania plik PFX, a następnie zapisanie base64 zakodowany ciąg w osobnym pliku.  Kod programu Powershell kodowanie base64 pochodzi z [Blogu skryptów programu Powershell][examplebase64encoding].

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

    $certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

    $fileName = "exportedcert.pfx"
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password     
    
    $fileContentBytes = get-content -encoding byte $fileName
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
    $fileContentEncoded | set-content ($fileName + ".b64")
    
Gdy certyfikat SSL po pomyślnym wygenerowane i konwertowane na base64 zakodowany ciąg znaków, przykład Menedżera zasobów Azure szablon na GitHub [Konfigurowanie domyślny certyfikat SSL] [ configuringDefaultSSLCertificate] mogą być używane.

Poniżej wymieniono parametry w pliku *azuredeploy.parameters.json* :

- *appServiceEnvironmentName*: Nazwa ASE ILB konfigurowany.
- *existingAseLocation*: ciąg tekstowy zawierający Azure regionu, gdzie został wdrożony ILB ASE.  Na przykład: "Południe centralnej z NAMI".
- *pfxBlobString*: based64 zakodowany ciąg znaków reprezentujący plik pfx.  Używanie wstawkę kodu przedstawionym wcześniej, czy skopiuj parametry zawarte w "exportedcert.pfx.b64" i wklej go jako wartość atrybutu *pfxBlobString* .
- *hasło*: hasło użyte do zabezpieczenia plik pfx.
- *certificateThumbprint*: odcisku palca certyfikatu.  Po pobraniu tej wartości z programu Powershell (np. *$certificate. Odcisku palca* z wcześniejszych wstawkę kodu), można użyć wartości jako-jest.  Jednak jeśli skopiujesz wartości w oknie dialogowym certyfikat systemu Windows, należy pamiętać o odłączenia usunięciu spacji.  *CertificateThumbprint* powinien wyglądać na przykład: AF3143EB61D43F6727842115BB7F17BBCECAECAE
- *certificateName*: identyfikator ciągu przyjazny wybranej przez użytkownika używane do identyfikowania certyfikatu.  Nazwa jest używany jako część Unikatowy identyfikator menedżera zasobów Azure obiektu *Microsoft.Web/certificates* reprezentującą certyfikat SSL.  Nazwa **musi** kończy się sufiksem następujące: \_yourASENameHere_InternalLoadBalancingASE.  Ten sufiks jest używane przez portal jako wskazówkę, że certyfikat jest używany do zabezpieczania z włączoną obsługą ILB ASE.


Poniżej przedstawiono przykład skróconą *azuredeploy.parameters.json* :


    {
         "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json",
         "contentVersion": "1.0.0.0",
         "parameters": {
              "appServiceEnvironmentName": {
                   "value": "yourASENameHere"
              },
              "existingAseLocation": {
                   "value": "East US 2"
              },
              "pfxBlobString": {
                   "value": "MIIKcAIBAz...snip...snip...pkCAgfQ"
              },
              "password": {
                   "value": "PASSWORDGOESHERE"
              },
              "certificateThumbprint": {
                   "value": "AF3143EB61D43F6727842115BB7F17BBCECAECAE"
              },
              "certificateName": {
                   "value": "DefaultCertificateFor_yourASENameHere_InternalLoadBalancingASE"
              }
         }
    }

Po wypełnieniu plik *azuredeploy.parameters.json* domyślny certyfikat SSL można skonfigurować przy użyciu następujących wstawkę kodu programu Powershell.  Zmienianie plików ścieżek zgodnie z lokalizacji plików szablonów Azure Menedżera zasobów na tym komputerze.  Pamiętaj również o podanie własne wartości dla nazwy wdrożenia Menedżera zasobów Azure i nazwa grupy zasobów.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"
    
    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Po przesłaniu szablon Azure Menedżera zasobów, że trwa około minuty 40 minut na zewnętrzną ASE zastosować zmiany.  Na przykład z ASE domyślny rozmiar, przy użyciu obu stronach wierzch, szablon potrwa około jedną godzinę i 20 minut do wykonania.  Szablon jest uruchomiona ASE nie będą mogli skalowany.  

Po wykonaniu tego szablonu aplikacji na ILB ASE można uzyskać dostęp za pomocą protokołu HTTPS i połączenia będą chronione za pomocą domyślny certyfikat SSL.  Domyślny certyfikat SSL będzie używany podczas aplikacji na ILB ASE dotyczą przy użyciu kombinacji nazwy aplikacji oraz domyślna nazwa hosta.  Na przykład *https://mycustomapp.internal-contoso.com* użyć domyślny certyfikat SSL dla **.internal contoso.com*.

Jednak podobnie jak aplikacje uruchomionych usługi publicznych wielu dzierżawy, deweloperzy można również skonfigurować nazwy hostów niestandardowych dla poszczególnych aplikacji, a następnie skonfiguruj unikatowe powiązania certyfikatu SNI SSL dla poszczególnych aplikacji.  


## <a name="getting-started"></a>Wprowadzenie

Aby rozpocząć pracę ze środowiskami usługi aplikacji, zobacz [Wprowadzenie do środowiska usługi aplikacji](app-service-app-service-environment-intro.md)

Wszystkie artykuły i w jaki sposób — aby rekordami dla środowiska usługi aplikacji są dostępne w [pliku README dla środowiska usługi aplikacji](../app-service/app-service-app-service-environments-readme.md).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[quickstartilbasecreate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-create/
[examplebase64encoding]: http://powershellscripts.blogspot.com/2007/02/base64-encode-file.html 
[configuringDefaultSSLCertificate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl/ 
 
