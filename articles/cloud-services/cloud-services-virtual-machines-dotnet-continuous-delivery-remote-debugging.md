<properties
    pageTitle="Włączanie debugowania zdalnego z dostarczaniem ciągły | Microsoft Azure"
    description="Dowiedz się, jak włączyć debugowanie zdalnych w przypadku wdrożenia Azure za pomocą ciągły dostarczenia"
    services="cloud-services"
    documentationCenter=".net"
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="enable-remote-debugging-when-using-continuous-delivery-to-publish-to-azure"></a>Włączanie debugowania zdalnego, używając ciągły dostarczenia publikowanie Azure

Można włączyć zdalnego debugowanie w Azure, dla usług w chmurze lub maszyn wirtualnych, korzystając z [ciągłym dostarczenia](cloud-services-dotnet-continuous-delivery.md) publikowanie Azure, wykonując następujące czynności.

## <a name="enabling-remote-debugging-for-cloud-services"></a>Włączanie zdalnego debugowania dla usług w chmurze

1. Na agenta kompilacji skonfigurować środowisko pierwotne dla Azure opisane w [Tworzenie wiersza polecenia dla Azure](http://msdn.microsoft.com/library/hh535755.aspx).
2. Ponieważ środowisko uruchomieniowe zdalnego debugowania (msvsmon.exe) jest wymagana dla pakietu, zainstaluj [Zdalnego narzędzia Visual Studio 2015 r](http://www.microsoft.com/en-us/download/details.aspx?id=48155) (lub [Narzędzia zdalnej dla programu Microsoft Visual Studio 2013 aktualizacji 5](https://www.microsoft.com/en-us/download/details.aspx?id=48156) , jeśli korzystasz z programu Visual Studio 2013). Alternatywnie możesz skopiować pliki binarne debugowania zdalnego z systemem zawierającym Visual Studio zainstalowany.
3. Tworzenie certyfikatu z opisane w [Omówienie certyfikatów dla usług w chmurze Azure](cloud-services-certs-create.md). Zachowaj PFX i odcisku palca certyfikatu RDP i przekaż certyfikatu do usługi w chmurze docelowej.
4. Użyj poniższych opcji w wierszu polecenia MSBuild do tworzenia i pakowanie z zdalnego debugowania włączone. (Podstawić rzeczywiste ścieżki do plików systemowych i project dla elementów oddzielona pod kątem).

        msbuild /TARGET:PUBLISH /PROPERTY:Configuration=Debug;EnableRemoteDebugger=true;VSX64RemoteDebuggerPath="<remote tools path>";RemoteDebuggerConnectorCertificateThumbprint="<thumbprint of the certificate added to the cloud service>";RemoteDebuggerConnectorVersion="2.7" "<path to your VS solution file>"

    `VSX64RemoteDebuggerPath`jest ścieżka do folderu zawierającego msvsmon.exe na karcie Narzędzia zdalnego programu Visual Studio.
    `RemoteDebuggerConnectorVersion`jest to wersja Azure SDK w usłudze w chmurze. Również powinny być takie same zainstalowanej z programem Visual Studio wersji.

5. Publikowanie usług w chmurze docelowej przy użyciu tego pliku pakietu i .cscfg wygenerowane w poprzednim kroku.
6. Importowanie certyfikatu (pfx) do komputera, na którym przez .NET jest już zainstalowany program Visual Studio z Azure SDK. Upewnij się zaimportować do `CurrentUser\My` magazyn certyfikatów, w przeciwnym razie dołączaniu do debugowania w programie Visual Studio nie powiedzie się.

## <a name="enabling-remote-debugging-for-virtual-machines"></a>Włączanie zdalnego debugowania dla maszyn wirtualnych

1. Tworzenie Azure maszyn wirtualnych. Zobacz [Tworzenie wirtualnych komputera z systemem Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md) lub [Tworzenie i zarządzanie nimi Azure maszyn wirtualnych w programie Visual Studio](../virtual-machines/virtual-machines-windows-classic-manage-visual-studio.md).
2. [Azure klasyczny strony portalu](http://go.microsoft.com/fwlink/p/?LinkID=269851)wyświetlanie pulpitu nawigacyjnego maszyny wirtualnej, aby wyświetlić maszyny wirtualnej **Odcisku PALCA certyfikatu RDP**. Ta wartość jest używana dla `ServerThumbprint` wartości w konfiguracji rozszerzenia.
3. Tworzenie certyfikatu klienta, zgodnie z zaleceniami w [Omówienie certyfikatów dla usług w chmurze Azure](cloud-services-certs-create.md) (PFX i odcisku palca certyfikatu RDP Zachowaj).
4. Instalowanie programu Powershell Azure (wersja 0.7.4 lub nowsza) opisane w [sposób instalowania i konfigurowania środowiska PowerShell Azure](../powershell-install-configure.md).
5. Uruchom następujący skrypt, aby włączyć rozszerzenie RemoteDebug. Zamień ścieżek i danych osobistych własny, takich jak nazwy subskrypcji, nazwa usługi i odcisku palca.

    >[AZURE.NOTE] Skonfigurowano tego skryptu programu Visual Studio 2015 r. Jeśli używasz programu Visual Studio 2013 modyfikowanie `$referenceName` i `$extensionName` przydziałów poniżej, aby użyć `RemoteDebugVS2013` (zamiast `RemoteDebugVS2015`).

    <pre>
   Dodawanie AzureAccount

    Wybierz pozycję AzureSubscription "Moja subskrypcja Microsoft"

    $vm = "mytestvm1" get AzureVM - nazwausługi-nazwa "mytestvm1"

    $endpoints = @( ,@{Name="RDConnVS2013"; PublicPort = 30400; PrivatePort = 30398} ,@{Name="RDFwdrVS2013"; PublicPort = 31400; PrivatePort = 31398})  

    foreach ($endpoint w $endpoints) {AzureEndpoint Dodaj - maszyn wirtualnych $vm-nazwa $endpoint. Name - protokołu tcp - PublicPort $endpoint. PublicPort - port lokalny $endpoint. PrivatePort}

    $referenceName = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug.RemoteDebugVS2015" $publisher = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug" $extensionName = "RemoteDebugVS2015" $version = "1.*" $publicConfiguration = "<PublicConfig>PRAWDA < /Connector.Enabled > < Connector.Enabled ><ClientThumbprint>56D7D1B25B472268E332F7FC0C87286458BFB6B2</ClientThumbprint><ServerThumbprint>E7DCB00CB916C468CC3228261D6E4EE45C8ED3C6</ServerThumbprint><ConnectorPort>30398</ConnectorPort><ForwarderPort>31398</ForwarderPort></PublicConfig>"

    $vm | Zestaw AzureVMExtension `
    -ReferenceName $referenceName ` 
    -Publisher $publisher `
    -ExtensionName $extensionName ` 
    — wersja $version "- PublicConfiguration $publicConfiguration

    foreach ($extension w $vm. MASZYN WIRTUALNYCH. ResourceExtensionReferences) {Jeśli (($extension. Nazwa_odwołania - eq $referenceName) `
    -and ($extension.Publisher -eq $publisher) ` 
    - i ($extension. Nazwisko - eq $extensionName) "- i ($extension. Wersja - eq $version)) {$extension. ResourceExtensionParameterValues [0]. Klucz = podział "pliku config.txt"}}

    $vm | Aktualizacja AzureVM </pre>

6. Importowanie certyfikatu (pfx) do komputera, na którym przez .NET jest już zainstalowany program Visual Studio z Azure SDK.
