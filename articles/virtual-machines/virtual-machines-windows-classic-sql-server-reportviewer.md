<properties 
    pageTitle="W witrynie sieci Web za pomocą ReportViewer | Microsoft Azure"
    description="W tym temacie opisano, jak tworzyć witryny sieci Web usługi Microsoft Azure z formant Visual Studio ReportViewer do wyświetlania raportu zapisanego na komputerze wirtualnej Microsoft Azure."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="guyinacube"
    manager="erikre"
    editor="monicar" 
    tags="azure-service-management" />
<tags 
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/04/2016"
    ms.author="asaxton" />

# <a name="use-reportviewer-in-a-web-site-hosted-in-azure"></a>Za pomocą ReportViewer w witrynie sieci Web hostowanej platformy Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Można tworzyć witryny sieci Web usługi Microsoft Azure z formant Visual Studio ReportViewer do wyświetlania raportu zapisanego na komputerze wirtualnej Microsoft Azure. Formant ReportViewer znajduje się w aplikacji sieci Web, tworzenie przy użyciu szablonu aplikacji sieci Web programu ASP.NET.

>[AZURE.IMPORTANT] Szablony aplikacji sieci Web programu ASP.NET MVC nie obsługują formant ReportViewer.

Aby dołączyć ReportViewer do witryny sieci Web usługi Microsoft Azure, należy wykonać następujące zadania.

- **Dodawanie** Zestawy pakietu wdrażania

- **Konfigurowanie** Uwierzytelnianie i autoryzacja

- **Publikowanie** aplikacji sieci Web programu ASP.NET z platformy Azure

## <a name="prerequisites"></a>Wymagania wstępne

Zapoznaj się z sekcją "ogólne zalecenia i najważniejsze wskazówki" w [Analizy biznesowej programu SQL Server w maszyn wirtualnych Azure](virtual-machines-windows-classic-ps-sql-bi.md).

>[AZURE.NOTE] Formanty ReportViewer są dostarczane z programem Visual Studio Standard Edition lub powyżej. Jeśli korzystasz z sieci Web Developer Express Edition, należy zainstalować [RUNTIME 2012 PODGLĄDU raportu firmy MICROSOFT](https://www.microsoft.com/download/details.aspx?id=35747) do korzystania z funkcji Środowisko uruchomieniowe ReportViewer.
>
>ReportViewer skonfigurowane w trybie przetwarzania lokalne nie jest obsługiwane w programie Microsoft Azure.

Przejrzyj dokument [formancie podglądu raportów usług Reporting Services i maszyn wirtualnych platformy Microsoft Azure oparta na serwerach raportowania](http://download.microsoft.com/download/2/2/0/220DE2F1-8AB3-474D-8F8B-C998F7C56B5D/Reporting%20Services%20report%20viewer%20control%20and%20Azure%20VM%20based%20report%20servers.docx).

## <a name="adding-assemblies-to-the-deployment-package"></a>Dodawanie zestawów do pakietu wdrażania

Jeśli programu ASP.NET aplikacji w wersji lokalnej, zestawy ReportViewer bezpośrednio w globalnej pamięci podręcznej zestawów (GAC) serwera IIS zazwyczaj są instalowane podczas instalacji programu Visual Studio, gdzie są dostępne bezpośrednio przez aplikację. Jednak jeśli aplikacja ASP.NET w chmurze, Microsoft Azure nie jest możliwe nic musi być zainstalowany do globalnej pamięci podręcznej zestawu, więc należy się upewnić, że zestawy ReportViewer są dostępne lokalnie dla aplikacji. Można to zrobić, dodając odwołania do nich w projekcie i skonfigurować je do skopiowania lokalnie.

W trybie Przetwarzanie zdalne formant ReportViewer używa następujących zestawów:

- **Microsoft.ReportViewer.WebForms.dll**: zawiera kod ReportViewer, którego należy użyć ReportViewer na stronie. Odwołanie dla tego zestawu jest dodawany do projektu po upuszczeniu formant ReportViewer na stronę programu ASP.NET w projekcie.

- **Microsoft.ReportViewer.Common.dll**: zawiera klasy używane przez formant ReportViewer w czasie wykonywania. Go nie jest automatycznie dodawany do projektu.

### <a name="to-add-a-reference-to-microsoftreportviewercommon"></a>Aby dodać odwołanie do Microsoft.ReportViewer.Common

- Kliknij prawym przyciskiem myszy węzeł **odwołania** projektu i wybierz pozycję **Dodaj odwołanie**, wybierz zestaw na karcie .NET i kliknij **przycisk OK**.

### <a name="to-make-the-assemblies-locally-accessible-by-your-aspnet-application"></a>Aby udostępnić zestawów lokalnie w aplikacji programu ASP.NET

1. W folderze **odwołania** kliknij zestaw Microsoft.ReportViewer.Common tak, aby jego właściwości są wyświetlane w okienku właściwości.

1. W okienku właściwości ustaw **Kopii lokalnej** wartość PRAWDA.

1. Powtórz kroki 1 i 2 dla Microsoft.ReportViewer.WebForms.

### <a name="to-get-reportviewer-language-pack"></a>Aby uzyskać pakiet językowy ReportViewer

1. Zainstaluj odpowiedni pakiet redystrybucyjny Microsoft raport podglądu 2012 Runtime z [Centrum pobierania Microsoft](http://go.microsoft.com/fwlink/?LinkId=317386).

1. Wybierz język z listy rozwijanej i strony przekierowaniem do odpowiedniej strony Centrum pobierania.

1. Kliknij przycisk **Pobierz** , aby rozpocząć pobieranie ReportViewerLP.exe.

1. Po pobraniu ReportViewerLP.exe, kliknij przycisk **Uruchom** , aby zainstalować natychmiast, lub kliknij przycisk **Zapisz** , aby zapisać go na komputerze. Kliknięcie przycisku **Zapisz**, należy pamiętać, nazwę folderu, w którym możesz zapisać plik.

1. Zlokalizuj folder, w której zapisano plik. Kliknij prawym przyciskiem myszy ReportViewerLP.exe, kliknij polecenie **Uruchom jako administrator**, a następnie kliknij przycisk **Tak**.

1. Po uruchomieniu ReportViewerLP.exe, zostanie wyświetlona c:\windows\assembly występują zasobu pliki **Microsoft.ReportViewer.Webforms.Resources** i **Microsoft.ReportViewer.Common.Resources**.

### <a name="to-configure-for-localized-reportviewer-control"></a>Aby skonfigurować dla zlokalizowane Kontrolka ReportViewer

1. Pobierz i zainstaluj pakiet redystrybucyjny Microsoft raport podglądu 2012 Runtime zgodnie z instrukcjami określonych powyżej.

1. Tworzenie <language> pliki zestawu zasobów dla folderu w programie project i kopiowanie. Pliki zestawu zasobów do skopiowania są: **Microsoft.ReportViewer.Webforms.Resources.dll** i **Microsoft.ReportViewer.Common.Resources.dll**. Wybierz pliki zestawu zasobów, a w okienku właściwości ustaw **Kopiuj do katalogu wyjściowego** **zawsze skopiować"**.

1. Ustaw kultury i UICulture dla projektu sieci web. Aby uzyskać więcej informacji o konfigurowaniu kultury i kultura interfejsu użytkownika dla strony sieci Web programu ASP.NET, zobacz [jak: Ustawianie kultury i kultura interfejsu użytkownika dla globalizacji strony sieci Web programu ASP.NET](http://go.microsoft.com/fwlink/?LinkId=237461).

## <a name="configuring-authentication-and-authorization"></a>Konfigurowanie uwierzytelniania i autoryzacji

ReportViewer należy używać odpowiednich poświadczeń do uwierzytelniania na serwerze raportów, a poświadczenia muszą być autoryzowane przez serwer raportów, aby uzyskać dostęp do odpowiednich raportów. Aby uzyskać informacje na temat uwierzytelniania zobacz oficjalny dokument [formancie podglądu raportów usług Reporting Services i maszyn wirtualnych platformy Microsoft Azure oparta na serwerach raportowania](https://msdn.microsoft.com/library/azure/dn753698.aspx).

## <a name="publish-the-aspnet-web-application-to-azure"></a>Publikowanie aplikacji sieci Web programu ASP.NET Azure

Aby uzyskać instrukcje dotyczące publikowania aplikacji sieci Web programu ASP.NET Azure, zobacz [jak: Migrowanie i publikowanie aplikacji sieci Web Azure z programu Visual Studio](../vs-azure-tools-migrate-publish-web-app-to-cloud-service.md) i [Wprowadzenie do aplikacji sieci Web i programu ASP.NET](../app-service-web/web-sites-dotnet-get-started.md).

>[AZURE.IMPORTANT] Polecenie Dodaj projekt wdrożenia Azure lub Dodaj projekt usługi Cloud Azure nie jest wyświetlana w menu skrótów w Eksploratorze rozwiązań, może być konieczne zmienianie struktury docelowej projektu .NET Framework 4.
>
>Dwa polecenia podaj zasadniczo te same funkcje. Jeden lub innych polecenie pojawi się w menu skrótów w zależności od wersji systemu Microsoft Azure SDK zainstalowano.

## <a name="resources"></a>Zasoby

[Raporty programu Microsoft](http://go.microsoft.com/fwlink/?LinkId=205399)

[Analizy biznesowej programu SQL Server w środowisku maszyn wirtualnych Azure](virtual-machines-windows-classic-ps-sql-bi.md)

[Tworzenie Azure maszyn wirtualnych z trybu macierzystego serwer raportów przy użyciu programu PowerShell](virtual-machines-windows-classic-ps-sql-report.md)

[Raportowanie w formancie podglądu raportów usług i Microsoft Azure maszyn wirtualnych podstawie serwerów raportów](http://download.microsoft.com/download/2/2/0/220DE2F1-8AB3-474D-8F8B-C998F7C56B5D/Reporting%20Services%20report%20viewer%20control%20and%20Azure%20VM%20based%20report%20servers.docx)
