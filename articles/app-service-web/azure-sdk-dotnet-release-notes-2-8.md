
<properties 
   pageTitle="Azure SDK dla .NET 2,8 informacje o wersji" 
   description="Azure SDK dla .NET 2,8 informacje o wersji" 
   services="app-service\web" 
   documentationCenter=".net" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/17/2016"
   ms.author="juliako"/>
 
# <a name="azure-sdk-for-net-28-281-and-282"></a>Azure SDK dla środowiska .NET 2,8, 2.8.1 i 2.8.2

##<a name="overview"></a>Omówienie
 
Ten artykuł zawiera informacje o wersji (które obejmuje znanych problemów i zmiany podziału) dla Azure SDK dla 2,8 .NET, 2.8.1 i 2.8.2 wersjach. 

Aby uzyskać pełną listę nowe funkcje i aktualizacje wprowadzone w tej wersji Zobacz powiadomienie [2,8 SDK Azure Visual Studio 2013 i Visual Studio 2015 r](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/) . 

##  <a name="azure-sdk-for-net-28"></a>Azure SDK dla środowiska .NET 2,8

### <a name="download-azure-sdk-for-net-28"></a>Pobierz zestaw SDK Azure dla .NET 2,8

[Azure SDK dla środowiska .NET 2,8 Visual Studio 2015 r.](http://go.microsoft.com/fwlink/?LinkId=699285) 

[Azure SDK dla środowiska .NET 2,8 Visual Studio 2013 r.](http://go.microsoft.com/fwlink/?LinkId=699287)
 
### <a name="net-452-support"></a>.NET 4.5.2 pomocy technicznej 

####<a name="known-issues"></a>Znane problemy

Azure 2,8 SDK .NET umożliwia tworzenie .NET 4.5.2 pakietów usługi w chmurze. Jednak .NET 4.5.2 framework nie zostanie zainstalowana na domyślny Zwolnij obrazów systemu operacyjnego gościa do stycznia 2016 system operacyjny gościa. Przed, 4.5.2 .NET framework będzie dostępna za pośrednictwem osobnych gościnny system operacyjny wersji — listopada 2015-02. Odwiedź stronę [wersjach systemu operacyjnego gościa Azure i SDK zgodności macierzy](../cloud-services/cloud-services-guestos-update-matrix.md) do śledzenia będą dodawane obrazu.  Po zwolnieniu obraz 2015-02 listopada można zaktualizować plik (.cscfg) pliku konfiguracji usługi w chmurze za pomocą tego obrazu. W konfiguracji usługi plik atrybut element osVersion elementu ServiceConfiguration do ciągu "WA-Gość-OS-4.26_201511-02". Jeśli chcesz dołączyć do używać ten obraz, a następnie nie będzie już uzyskać aktualizacje automatyczne, aby system operacyjny gościa. Aby otrzymywać aktualizacje automatyczne element osVersion musi mieć wartość "*" i .NET 4.5.2 tylko będzie dostępna za pośrednictwem aktualizacji automatycznych w stycznia 2016 r.

###<a name="azure-data-factory"></a>Factory Azure danych

####<a name="known-issues"></a>Znane problemy 

Podczas tworzenia projektu **Szablon Factory danych** obejmujących dane przykładowe skrypt powłoki azure power mogą nie działać w przypadku wersji powłoki azure power zainstalowany na komputerze po 0.9.8.

Aby pomyślnie utworzyć ten typ projektu, należy zainstalować [wersję powłoki azure power 0.9.8](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi).


### <a name="azure-resource-manager-tools"></a>Azure Narzędzia Menedżera zasobów 

####<a name="breaking-changes"></a>Przerywanie zmian

Skrypt programu PowerShell podanych przez projektu grupa zasobów Azure została zaktualizowana w tej wersji do pracy z nowe polecenia cmdlet programu PowerShell Azure w wersji 1.0.  Ten nowy skrypt nie będzie działać z z programem Visual Studio podczas korzystania z wersji SDK przed 2,8.  

Skryptów z projektów utworzonych we wcześniejszych wersjach pakietu nie będzie działać w programie Visual Studio podczas korzystania z 2,8 SDK.  Wszystkie skrypty będą nadal działać poza Visual Studio z odpowiednią wersję polecenia cmdlet programu PowerShell Azure.  

2,8 SDK wymaga wersji 1.0 polecenia cmdlet programu PowerShell Azure.  Wszystkie inne wersje pakietu wymagana wersja 0.9.8 poleceń cmdlet programu PowerShell Azure.  Aby uzyskać więcej informacji, zobacz [tym](http://go.microsoft.com/fwlink/?LinkID=623011) blogu.

###<a name="web-tools-extensions"></a>Rozszerzenia narzędzia sieci Web

####<a name="known-issues"></a>Znane problemy

Następujące znane problemy dotyczące zostanie rozwiązany w następujących wersji.

- Aplikacji usługi zawartości związanej z chmury i Server Explorer gest środowiskach innych niż produkcyjnych (na przykład Chiny Azure lub klientów stos Azure) nie działają. W przypadku klientów w następujących obszarach ryzyko pobieranie profilu publikowania z portalem Azure umożliwi możliwości publikowania. Przyszłej wersji będzie napraw gestów, takie jak "Dołącz debugowania" i "Wyświetl Streaming dzienniki" Chiny Azure i klientów stosu. 
- Klienci mogą pojawić się błędy podczas tworzenia podczas wystąpienia wniosków aplikacji, dla którego są wdrażania znajduje się w regionie niż wschodniego USA aplikacji usługi. W tych sytuacjach tworzenie aplikacji usługi w portalu i pobieranie profilu publikowania umożliwi scenariuszy publikowania. 

###<a name="azure-hdinsight-tools"></a>Narzędzia Usługa Azure HDInsight

####<a name="new-updates"></a>Nowe aktualizacje

- Można wykonania kwerendy gałęzi w klastrze za pośrednictwem HiveServer2 z niemal nie ogólnych, a dzienniki zadania czasie rzeczywistym.
- Przy użyciu nowych gałęzi wykonanie widoku zadań można wyświetlić w bardziej zaawansowane zadania, Znajdź więcej szczegółów i identyfikowanie potencjalnych problemów.

Aby uzyskać informacje zobacz [2,8 SDK Azure Visual Studio 2013 i Visual Studio 2015 r](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/). 

## <a name="azure-sdk-for-net-281"></a>Azure SDK dla środowiska .NET 2.8.1

### <a name="known-issues-for-visual-studio-2013-and-visual-studio-2015"></a>Znane problemy dotyczące programu Visual Studio 2013 i Visual Studio 2015 r.
 
1. WebJob wyzwalane publikuje będzie gniazda Pokaż i błędów, a nie zestaw serii rozłożonych w czasie, ale przesunie WebJob Azure. Klienci, którzy wymagają zadanie zaplanowane używać Azure Portal skonfigurować harmonogram WebJob. 
2. Python mogą wystąpić problemy z debugowania. Zespołu usługi jest wdrażania rozwiązania tego, ale jeśli występuje klientów, powiadom Microsoft wiesz na forach lub w blogu zawiadomienie o lub zwalnianie sekcji komentarze notatki. 
3. Klienci w niektórych regionach (na przykład Południowej Indie) wystąpią błędy inicjowania obsługi administracyjnej aplikacji usługi. Jest to zgodne z portalem i klienci, którzy ten problem może być żądanie dostępu do publikowania w następujących regionach geo Azure portal. Po ich żądanie dostępu do tych regionów za pomocą Azure portalu inicjowania obsługi administracyjnej powinna działać. 

##<a name="azure-sdk-for-net-282"></a>Azure SDK dla środowiska .NET 2.8.2

Po zainstalowaniu 2.8.2 narzędzia klienci mogą wystąpić następujące problem.         

- Jeśli używasz systemu Windows 10, a nie jest zainstalowany program Internet Explorer, może zostać wyświetlony komunikat o błędzie "Nie można znaleźć programu Internet Explorer".
Aby rozwiązać ten problem, należy zainstalować program Internet Explorer za pomocą okna dialogowego Dodaj lub Usuń składniki systemu Windows.

Jeśli obserwuje ten problem, należy użyć funkcji Wyślij a uśmiech do raportu.

Aby uzyskać więcej informacji zobacz [ten](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-2-for-net/) opublikować.
##<a name="other-updates"></a>Pozostałe aktualizacje

W przypadku innych aktualizacji zobacz [powiadomienie 2,8 SDK Azure opublikować](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).

##<a name="also-see"></a>Zobacz też

[Wpis zawiadomienie o 2,8 SDK Azure](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/)

[Pomocy technicznej i informacji dotyczących Azure SDK emerytury .NET i interfejsów API](https://msdn.microsoft.com/library/azure/dn479282.aspx)

