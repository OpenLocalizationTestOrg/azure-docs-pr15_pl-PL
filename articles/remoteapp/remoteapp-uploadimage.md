
<properties
    pageTitle="Przekaż obraz niestandardowy dla Azure RemoteApp | Microsoft Azure"
    description="Dowiedz się, jak przekazać obraz niestandardowy do Azure RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="ericorman"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="ericor" />



# <a name="upload-a-custom-image-for-azure-remoteapp"></a>Przekaż obraz niestandardowy dla Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Teraz, gdy został utworzony szablon niestandardowy obraz lub być aktualizowane na podstawie zmian, musisz przekazać ten obraz do biblioteki obrazów Azure RemoteApp. Wykonaj poniższe kroki.


## <a name="before-you-start"></a>Przed rozpoczęciem

1.      Sprawdź, czy obraz niestandardowy spełnia [wymagania obrazu](remoteapp-imagereqs.md) i [wymagania dotyczące aplikacji](remoteapp-appreqs.md).
2.      Zainstaluj [moduł Azure programu PowerShell](../powershell-install-configure.md).

## <a name="step-by-step-on-how-to-upload-custom-image"></a>Krok po kroku dotyczące sposobu Przekaż obraz niestandardowy

1.      Otwórz Portal Azure zarządzania i przejdź do strony RemoteApp.
2.      Na karcie **obrazów szablonów** kliknij pozycję **Przekaż** u dołu strony.
4.      Wprowadź przyjazną nazwę obrazu i określ lokalizację konta miejsca do magazynowania. Upewnij się, że znajduje się w tej samej lokalizacji co kolekcji RemoteApp lub lokalizację, w której chcesz utworzyć.
5.      Gdy zostanie wyświetlony monit, Pobierz skrypt na komputer lokalny.
6.      Skopiuj parametry polecenia w polu tekst do Schowka.
7.      Otwórz okno programu Windows PowerShell podwyższonym poziomem uprawnień.
8.      W oknie programu Windows PowerShell podwyższonym poziomem uprawnień przejdź do katalogu, w której został pobrany skrypt.
9.      Wklej skopiowany polecenie i naciśnij klawisz **Enter**.

    Rozpocznie proces przekazywania i czas trwania może zależeć od wielu czynników, łącznie z szybkość sieci i rozmiar obrazu

11.    Jeśli przekazania kończy się niepowodzeniem z powodu przerwy sieci lub elementy tak jak, możesz zawsze wznowić rozpoczęciu procesu przekazywania. Aby wznowić przekazywanie, uruchom skrypt ponownie, używając tym samym wierszu polecenia.

> [AZURE.WARNING] Nigdy nie zmodyfikować skrypt przekazywania. Przeprowadzanie testów pod kątem zastosowano aby upewnić się, że obraz spełnia wymagania obrazu i wymagania dotyczące aplikacji.

## <a name="common-problems"></a>Typowe problemy

- Upewnij się, że korzystasz z programu Windows PowerShell, nie Azure programu PowerShell. Należy zainstalować moduł programu PowerShell Azure, ponieważ niektóre moduły są potrzebne podczas przekazywania.
- Reguły sprawdzania poprawności są dostępne dla Twojej wygody, nigdy nie zmienia skrypt.
- Jeśli plik wirtualnego dysku twardego zostaje zablokowany podczas przekazywania, skopiuj plik lub przenieś ją do nowego przekazywania lokalizację i próba ponownie. Mogą występować pewne proces systemu Windows, który uniemożliwia przekazywania.  
