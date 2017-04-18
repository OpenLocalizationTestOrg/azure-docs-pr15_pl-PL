<properties
    pageTitle="Publikowanie aplikacji Azure RemoteApp | Microsoft Azure"
    description="Dowiedz się, jak publikować aplikacji i zasobów w Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />


# <a name="how-to-publish-an-app-in-remoteapp"></a>Jak opublikować aplikację w RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Po utworzeniu kolekcji RemoteApp, musisz opublikować aplikacji lub zasoby, które mają być dostępne dla użytkowników. Obrazów szablonu dostarczoną z subskrypcji jest tylko kilka aplikacji opublikowanych domyślnie — Udostępnianie innych aplikacji, musisz opublikować je.

> [AZURE.NOTE] Należy zaktualizować aplikację? Musisz zaktualizować [obraz](remoteapp-update.md) najpierw.

Na karcie **Publikowanie** w portalu kliknij przycisk **Publikuj**. Możesz dodać aplikację z menu **Start** obrazu szablonu lub wprowadź ścieżkę do której aplikacja jest zainstalowana na obraz szablonu. Jeśli chcesz dodać do menu **Start** , wybierz aplikację, aby opublikować z listy. Jeśli wybierzesz wprowadź ścieżkę do aplikacji, wprowadź nazwę dla tej aplikacji i ścieżkę do aplikacji. Przy użyciu zmiennych w ścieżce — na przykład "systemdrive %" zamiast "c:\".

> [AZURE.NOTE] Jeśli chcesz dodać aplikację do menu **Start** , trzeba mieć *dodać tę aplikację do * *rozpocząć* * menu Obraz szablonu.* W przeciwnym razie funkcja RemoteApp będą widzieć tylko jakie *jest* w **Start** menu i będzie niejasne. 

>Aby upewnić się, że aplikacja jest w **Start** menu, należy umieścić plik skrótu - **.lnk** — w folderze Menu\Programs %systemdrive%\ProgramData\Microsoft\Windows\Start.

> Jeśli nie pamiętasz dodać aplikację do **Start** menu po utworzeniu szablonu, należy wybrać opcję Dodaj ścieżkę do aplikacji. (Lub ponownie utworzyć obraz szablonu, ale to dość nieco więcej pracy).


 