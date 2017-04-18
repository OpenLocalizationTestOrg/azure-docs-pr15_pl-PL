
<properties
    pageTitle="Migracji danych użytkownika z Azure RemoteApp | Microsoft Azure"
    description="Dowiedz się, jak przeprowadzić migrację danych użytkowników i Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="how-to-migrate-data-into-and-out-of-azure-remoteapp"></a>Jak przeprowadzić migrację danych do i wylogowywanie się z Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Za pomocą wielu różnych narzędzi i metod do przesyłania [danych użytkownika](remoteapp-upd.md) do i wylogowywanie się z Azure RemoteApp. Poniżej przedstawiono kilka metod:

- Kopiowanie i wklejanie przy użyciu Schowka udostępniania
- Kopiowanie plików i danych na serwerze plików
- Kopiowanie plików do usługi OneDrive dla firm za pomocą przeglądarki
- Kopiowanie plików za pomocą przekierowania

>[AZURE.NOTE] 
> Nie można włączyć usługi OneDrive dla firmy lub dla klientów indywidualnych agentów synchronizacji — ta osoba [nie są obsługiwane](remoteapp-onedrive.md) w Azure RemoteApp.

## <a name="use-copy-and-paste-in-file-explorer"></a>Za pomocą kopiowania i wklejania w Eksploratorze plików

Kopiowanie i wklejanie przy użyciu Schowka jest włączona funkcja RemoteApp wdrożeń [Domyślnie](remoteapp-redirection.md). Dzięki temu użytkownicy kopiowania plików między ich lokalnym komputera i RemoteApp aplikacji. Często w czasie normalnego korzystanie z aplikacji w RemoteApp, użytkownicy zapisali pliki do ich UPDs - przenoszenia, że dane z RemoteApp jest łatwe:

1. [Publikowanie Eksploratora plików, jako aplikację](remoteapp-publish.md) w zbiorze RemoteApp. (Należy zauważyć, że to zadanie administracyjne).
2. Bezpośrednie użytkownikom na uruchamianie aplikacji Eksploratora plików, która została opublikowana za pomocą którego skopiowania i wklejenia plików, zarówno w ich UDP, jak i wylogowywanie się z jej.

## <a name="upload-files-and-data-to-a-file-server-by-using-standard-network-file-copy"></a>Przekazywanie plików i danych na serwerze plików przy użyciu sieci standardowy plik kopii

Organizacje często używać serwerów plików do przechowywania danych ogólne. Jeśli znasz nazwę serwera lub lokalizację, użytkownicy mogą przeglądanie sieci lokalnej na serwerze, a następnie skopiuj swoje pliki, podobnie jak powyżej. Ponownie warto publikowanie RemoteApp Eksploratora plików, a następnie udostępnij go innym użytkownikom.

>[AZURE.NOTE] 
> Serwer plik musi być sieci routingu RemoteApp został wdrożony w.

## <a name="copy-files-to-onedrive-for-business"></a>Kopiowanie plików do usługi OneDrive dla firm
Chociaż nie można włączyć usługi OneDrive dla firm synchronizacji agenta w RemoteApp, można nadal kopiować pliki z usługi UDP do usługi OneDrive dla firm za pośrednictwem przeglądarki sieci. 

1. Publikowanie RemoteApp Eksploratora plików, a następnie poinformuj użytkowników, aby uzyskać dostęp do plików za pomocą tej aplikacji. 
2. Najłatwiej jest je przesyłać pliki, jeśli muszą być skompresowane, aby użytkownicy należy utworzyć plik zip zawierający wszystkie pliki, aby przejść do usługi OneDrive dla firm.
3. Poproś użytkowników o przejdź do portalu usługi Office 365, a następnie przejdź do usługi OneDrive i przekaż plik zip.

## <a name="copy-files-by-using-drive-redirection"></a>Kopiowanie plików przy użyciu przekierowywanie dysków

Po włączeniu [przekierowywanie dysków](remoteapp-redirection.md)dysku mapowanego zostały już utworzone dla użytkowników. W tym przypadku można ich plików na dysk zip i zapisywanie ich do komputera lokalnego.