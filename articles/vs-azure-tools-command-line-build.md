<properties
   pageTitle="Tworzenie wiersza polecenia dla Azure | Microsoft Azure"
   description="Tworzenie wiersza polecenia dla Azure"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="command-line-build-for-azure"></a>Tworzenie wiersza polecenia dla Azure

## <a name="overview"></a>Omówienie

Możesz utworzyć pakiet wdrożenia Azure, uruchamiając MSBuild w wierszu polecenia. Można skonfigurować i definiowanie kompilacjach debugowania, organizowanie i produkcji, oprócz Automatyzowanie część procesu tworzenia.


## <a name="microsoft-build-engine-msbuild"></a>Aparat kompilacji firmy Microsoft (MSBuild)

Za pomocą aparatu tworzenie firmy Microsoft (MSBuild), można tworzyć produktów w kompilacji celach szkoleniowych, w którym nie jest zainstalowany program Visual Studio. MSBuild używa XML format dla plików projektów, które extensible i w pełni obsługiwane przez firmę Microsoft. W tym formacie można opisać co elementy muszą być przeznaczony dla platformy i konfiguracji.

Możesz również uruchomić MSBuild w wierszu polecenia, a w tym temacie opisano podejście. Ustawiając właściwości w wierszu polecenia, można tworzyć Konfiguracja projektu. Podobnie można także zdefiniować celów, które będą tworzyć polecenie MSBuild. Aby uzyskać więcej informacji na temat parametrów wiersza polecenia i MSBuild zobacz [Informacje dotyczące wiersza polecenia MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).

## <a name="installation"></a>Instalacji

Jak w poniższej procedurze opisano, należy zainstalować oprogramowanie i narzędzia na serwerze kompilacji przed utworzeniem pakietu Azure za pomocą MSBuild:

1. Instalowanie programu .NET Framework 4 lub później, które zawierają MSBuild.

1. Instalowanie [Narzędzia do tworzenia Azure](http://go.microsoft.com/fwlink/?LinkId=394615) (poszukaj MicrosoftAzureAuthoringTools x64.msi lub MicrosoftAzureAuthoringTools x86.msi.

1. Instalowanie [Bibliotek Azure dla środowiska .NET](http://go.microsoft.com/fwlink/?LinkId=394616) (poszukaj MicrosoftAzureLibsForNet x64.msi lub MicrosoftAzureLibs x86.msi.

1. Skopiuj plik Microsoft.WebApplication.targets z instalacją programu Visual Studio na innym komputerze.

    Plik znajduje się w katalogu \MSBuild\Microsoft\Visual Studio\v12.0\WebApplications C:\Program Files (x86) (v11.0 for Visual Studio 2012), a następnie należy skopiować go do tego samego katalogu na serwerze kompilacji.

1. Instalowanie [Narzędzia Azure programu Visual Studio](http://go.microsoft.com/fwlink/?LinkId=394616).

    Poszukaj WindowsAzureTools.vs120.exe tworzyć projekty Visual Studio 2013.

## <a name="msbuild-parameters"></a>Parametry MSBuild

Najprostszym sposobem utworzenia pakietu jest uruchomienie MSBuild z `/t:Publish` opcji. Domyślnie to polecenie tworzy katalog względem folderu głównego dla projektu, na przykład ProjectDir\bin\Configuration\app.publish\. podczas tworzenia projektu Azure wygenerować dwa pliki, w samym pliku pakietu i tym pliku konfiguracji:

- Project.cspkg

- ServiceConfiguration.TargetProfile.cscfg

Domyślnie każdy Azure projekt zawiera język, który tworzy plik konfiguracji usługi dla lokalnego (debugowanie) i innego kompilacjach chmury (tymczasowym lub produkcji), ale można dodać lub usunąć pliki Konfiguracja usługi, stosownie do potrzeb. Jeśli konstruujesz pakietu w programie Visual Studio, zostanie wyświetlony monit Konfiguracja usługi plików do uwzględnienia razem z pakietem pakietu. Po utworzeniu pakietu przy użyciu MSBuild domyślnie znajduje się plik lokalny Konfiguracja usługi. Aby dołączyć inny plik konfiguracji usługi, należy ustawić `TargetProfile` właściwość polecenie MSBuild (`MSBuild /t:Publish /p:TargetProfile=ProfileName`).

Jeśli chcesz użyć innego katalogu przechowywane pakietu i plików konfiguracji, Ustaw ścieżkę przy użyciu `/p:PublishDir=Directory\` opcji, w tym końcowe separator ukośnik odwrotny.

## <a name="deployment"></a>Wdrożenie

Po utworzeniu pakietu należy wdrożyć go Azure. Samouczek demonstrujący ten proces Zobacz Azure witryny sieci Web. Aby uzyskać informacje o zautomatyzować ten proces, zobacz [Ciągły dostawy dla usług w chmurze platformy Azure](./cloud-services/cloud-services-dotnet-continuous-delivery.md).
