<properties
   pageTitle="Uzyskiwanie samego środowiska usługi Office 365 na dowolnym urządzeniu z Azure RemoteApp | Microsoft Azure"
   description="Dowiedz się, jak udostępniać dowolnej aplikacji usługi Office 365 użytkowników za pomocą Azure RemoteApp."
   services="remoteapp"
   documentationCenter=""
   authors="guscatalano"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="guscatal;elizapo"/>


# <a name="get-the-same-office-365-experience-on-any-device-with-azure-remoteapp"></a>Uzyskiwanie samego środowiska usługi Office 365 na dowolnym urządzeniu z Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

W tym artykule opisano sposób wdrażania usługi Office 365 na dowolnym urządzeniu w firmie. Użytkownicy mogli takie same możliwości i doświadczeń interfejsu użytkownika z systemem Android, Apple i systemu Windows.

Firma Microsoft będzie to zrobić przy użyciu Azure RemoteApp, udostępniając usługi Office 365 w przypadku możliwe skali maszyn wirtualnych w Azure, które użytkownicy mogą połączyć się. Ten zestaw maszyn wirtualnych nazywamy zbioru"chmury".

## <a name="create-a-cloud-collection"></a>Tworzenie zbioru chmury

Najpierw po utworzeniu konta usługi Azure, przejdź do **trybu RemoteApp** klikając łącze po lewej stronie.
![Wyświetlanie Azure RemoteApp na Azure Portal](./media/remoteapp-tutorial-o365anywhere/1-menu.png)

Następnie kontynuować, klikając pozycję **Nowy** na dole i "szybkie tworzenie" zbioru. Podaj nazwę, regionu, subskrypcji, plan i obraz "Proffesional pakietu Office 2013" Firma Microsoft udostępnia.
![Okno dialogowe Tworzenie](./media/remoteapp-tutorial-o365anywhere/2-quickcreate.png)

Po zakończeniu formularz, który należy uruchomić proces tworzenia zbioru. To może potrwać do godziny lub tak.

![Oczekiwanie](./media/remoteapp-tutorial-o365anywhere/3-waiting.png)

Po zakończeniu procesu wygląda mniej więcej tak. Jeśli firma Microsoft kliknij polecenie **publikowania** widać, że większość aplikacji pakietu Office zostały opublikowane dla nas już.
![Kolekcja utworzona](./media/remoteapp-tutorial-o365anywhere/4-done.png)

![Opublikowane aplikacje](./media/remoteapp-tutorial-o365anywhere/5-publish.png)

Na tym etapie można również dodać więcej użytkowników, które mają dostęp do tej kolekcji, klikając pozycję **Dostępu użytkowników**.
![Konfigurowanie dostępu użytkowników](./media/remoteapp-tutorial-o365anywhere/6-user.png)

Teraz spróbuj się nawiązywanie połączenia z usługą Office 365!

## <a name="connect-to-office-365"></a>Nawiązywanie połączenia z usługi Office 365

Firma Microsoft będzie głowy w celu [https://www.remoteapp.windowsazure.com/](https://www.remoteapp.windowsazure.com/), przewiń w dół i kliknij przycisk **Pobierz klientów** zainstalować klienta Azure RemoteApp na urządzenie, którego jesteś na. Zrzuty ekranu poniżej są dla systemu Windows.

Po uruchomieniu aplikacji zostanie wyświetlony monit o zalogowanie się przy użyciu konta Microsoft (dawniej nazywanych "Live ID"), należy użyć tego samego jako konto Azure teraz. Po zalogowaniu w należy wyświetlić powiadomienie o nowej zaproszenia, kliknij przycisk i powinna być widoczna lista podobny do poniżej. Zaakceptowanie zaproszenia, która jest zgodna z kontem e-mail właściciela konto Azure.

![Nowe zaproszenie](./media/remoteapp-tutorial-o365anywhere/7-araclient.png)

Jak wygląda w przypadku nowego zaproszenia.

![Zaakceptuj aplikacji](./media/remoteapp-tutorial-o365anywhere/8-invitation.png)

Po zaakceptowaniu zaproszenia powinny być widoczne wszystkie aplikacje pakietu Office na komputerze klienckim Azure RemoteApp.

![Wykaz aplikacji](./media/remoteapp-tutorial-o365anywhere/9-work.png)

Po kliknięciu dowolnego z tych, które aplikacja powinna rozpoczynać się na Azure maszyny wirtualnej i powinny być ustawione! Ciesz się!

![Uruchamianie](./media/remoteapp-tutorial-o365anywhere/10-arastart.png)

![Program PowerPoint](./media/remoteapp-tutorial-o365anywhere/11-pp.png)
