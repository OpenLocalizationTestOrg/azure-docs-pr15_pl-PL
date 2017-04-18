<properties
    pageTitle="Co to jest obrazów szablonu Azure RemoteApp? | Microsoft Azure"
    description="Informacje na temat obrazów szablonów dostępnych w programie Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="what-is-in-the-azure-remoteapp-template-images"></a>Co to jest obrazów szablonu Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Twoja subskrypcja Azure RemoteApp obejmuje trzy obrazy szablonu:


- System Windows Server 2012
- Microsoft Office 365 ProPlus (wymagany subskrypcji usługi Office 365)
- Microsoft Office 2013 Professional Plus (tylko w wersji próbnej)

> [AZURE.IMPORTANT]Twoja subskrypcja Azure RemoteApp udziela dostępu do programu obrazów, z wyjątkiem usługi Office 365 ProPlus, której wymaga oddzielnych subskrypcji i Office 2013, który nie może być używana w produkcji. Oznacza to, że można udostępniać programów lub aplikacji na obrazów szablonów użytkowników. Na przykład jeśli utworzysz zbioru, który używa obrazu Windows Server 2012 R2, możesz opublikować ochrona punktu końcowego Centrum systemu dostępu przy użyciu funkcji RemoteApp dla użytkowników.
>
> Zapoznaj się z [Szczegóły licencjonowania RemoteApp](remoteapp-licensing.md) uzyskać więcej informacji. I informacje o licencji [Przy użyciu pakietu Office przy użyciu Azure RemoteApp](remoteapp-o365.md) dla pakietu Office.

Poniżej szczegółowych informacji na temat poszczególnych obraz zawiera.

## <a name="windows-server-2012-r2--the-vanilla-image"></a>Windows Server 2012 R2 ("obraz wanilii")
Ten obraz jest oparty na systemie operacyjnym centrum danych Microsoft Windows Server 2012 R2 i zawiera następujące role i funkcje instalowane do wymagań dla obrazów szablonów Azure RemoteApp:


- .NET framework 4,5, 3.5.1, 3.5
- Środowisko pulpitu
- Usługi dotyczące pisma ręcznego i pisanie odręczne
- Media Foundation
- Hosta sesji pulpitu zdalnego
- Środowiska Windows PowerShell 4.0
- Windows PowerShell ISE
- Obsługa WoW64

Ten obraz zawiera również następujących aplikacji:

- Programu Adobe Flash Player
- Program Microsoft Silverlight
- Ochrona punktu końcowego 2012 Centrum systemu Microsoft
- Microsoft Windows Media Player


## <a name="microsoft-office-365-proplus-subscription-required"></a>Microsoft Office 365 ProPlus (subskrypcji wymagane)
Usługa Office 365 jest najbardziej żądanej aplikacji, więc możemy utworzyć obraz "niestandardowy" pracę z.

Ten obraz jest rozszerzeniem wanilii obrazu i zawiera następujące składniki pakietu Microsoft Office 365 ProPlus zainstalowany oprócz składników opisanych w obrazie systemu Windows Server 2012 R2:


- Programu Access
- Program Excel
- Programu Lync
- Program OneNote
- OneDrive dla firm (Uwaga agenta synchronizacji nie jest obsługiwana przez Azure RemoteApp)
- Program Outlook
- Program PowerPoint
- Program Word
- Narzędzia sprawdzające pakietu Microsoft Office

Obraz zawiera również program Visio Pro i programu Project Pro.

I następujące aplikacje, a także:

- SQL Native client
- Sterownik ODBC
- Klient wyszukiwania danych programu SQL Server
- MasterDataServices klienta
- Program Microsoft Publisher
- PowerQuery
- Dodatku Power map


Funkcje aplikacji usługi Office 365 ProPlus jest dostępna tylko dla użytkowników, którzy mają plan usługi Office 365 ProPlus. Aby uzyskać więcej informacji na temat usługi Office 365 planów subskrypcji, zobacz [Plany usługi Office 365](http://technet.microsoft.com/library/office-365-plan-options.aspx). Nadal masz pytania? Zapoznaj się z informacjami [usługi Office 365 + RemoteApp](remoteapp-o365.md) . Także zajrzeć do nowego artykułu, [jak korzystać z subskrypcji usługi Office 365 z Azure RemoteApp](remoteapp-officesubscription.md).

Należy zauważyć, że trzeba oddzielnie licencji usługi Office 365 ProPlus, program Visio Pro i program Project Pro - mają własne licencji.

## <a name="microsoft-office-2013-professional-plus-trial-only"></a>Microsoft Office 2013 Professional Plus (tylko w wersji próbnej)
W okresie bezpłatnej wersji próbnej możesz przetestować usługę z obrazem pakietu Office 2013.

Ten obraz jest rozszerzeniem wanilii obrazu i zawiera następujące składniki pakietu Microsoft Office 2013 Professional Plus zainstalowany oprócz składników opisanych w obrazie systemu Windows Server 2012 R2:


- Programu Access
- Program Excel
- Programu Lync
- Program OneNote
- OneDrive dla firm (Uwaga agenta synchronizacji nie jest obsługiwana przez Azure RemoteApp)
- Program Outlook
- Program PowerPoint
- Projektu
- Program Visio
- Program Word
- Narzędzia sprawdzające pakietu Microsoft Office

> [AZURE.IMPORTANT]**Informacje prawne:** Ten obraz nie ma licencji Microsoft Office i *nie można używać do produkcji*. Office 2013 Professional Plus obraz jest przeznaczony tylko dla wersji próbnej. Jeśli chcesz korzystać z aplikacji pakietu Office w Azure RemoteApp produkcji, musisz korzystać z usługi Office 365 ProPlus obrazu. Aby uzyskać więcej szczegółów dotyczących licencjonowania pakietu Office zobacz [przy użyciu usługi Office 365 z Azure RemoteApp](remoteapp-o365.md)
