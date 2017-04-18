
<properties 
    pageTitle="Jak funkcja RemoteApp platformy Azure zapisać danych i ustawień użytkownika? | Microsoft Azure"
    description="Dowiedz się, jak funkcja RemoteApp platformy Azure umożliwia zapisanie danych użytkownika przy użyciu dysku profilu użytkownika."
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

# <a name="how-does-azure-remoteapp-save-user-data-and-settings"></a>Jak funkcja RemoteApp platformy Azure zapisać danych i ustawień użytkownika?

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Funkcja RemoteApp Azure zapisuje tożsamość użytkownika i dostosowania różnych urządzeń i sesji. Te dane użytkownika są przechowywane w dysku na zbioru użytkownika znana pod nazwą dysku profilu użytkownika (UDP). Dysk podąża za użytkownikiem i gwarantuje, że użytkownik ma spójny wygląd, niezależnie od tego, gdzie one się zalogować. Umożliwia zapisanie 

Dyski profilu użytkownika są zupełnie przezroczysty dla użytkownika — użytkownicy zapisywać dokumenty do folderu Dokumenty (na wyglądające na dysku lokalnym) i zmienianie ustawień aplikacji w zwykły sposób. W tym samym czasie wszystkie ustawienia osobiste zachowywane podczas łączenia się funkcja RemoteApp platformy Azure na dowolnym urządzeniu. Zostanie wyświetlonych jest ich danych, w tym samym miejscu.

Każdy UDP ma 50GB miejsca do magazynowania trwałych i zawiera zarówno danych i stosowanie ustawień użytkownika. 

Poniżej szczegóły danych profilu użytkownika.

>[AZURE.NOTE] Potrzebujesz wyłączyć UDP? Jak tego — Wyewidencjonuj i Pavithra wpis w blogu: [Wyłącz użytkownika profilu dysków (UPDs) w Azure RemoteApp](https://blogs.technet.microsoft.com/enterprisemobility/2015/11/11/disable-user-profile-disks-upds-in-azure-remoteapp/), aby uzyskać szczegółowe informacje.


## <a name="how-can-an-admin-get-to-the-data"></a>Jak administrator może uzyskać dostęp do danych?

Jeśli chcesz uzyskać dostęp do danych na potrzeby jednego z użytkowników (w przypadku awarii lub jeśli użytkownik opuści firmy), skontaktuj się z obsługą Azure i podaj informacje o subskrypcji zbierania i tożsamość użytkownika. Zespół Azure RemoteApp Podaj adres URL do wirtualnego dysku twardego. Pobierz ten wirtualnego dysku twardego i pobrać wszelkie dokumenty lub pliki, które są potrzebne. Należy zauważyć, że wirtualny dysk twardy jest 50GB zajmie bit, aby ją pobrać.


## <a name="is-the-data-backed-up"></a>Dane kopii zapasowej

Tak, Zapisz kopię zapasową danych użytkownika dla lokalizacji geograficznej. Dane są tylko do odczytu i są dostępne w taki sam sposób, jak zwykłe danych będzie (kontaktu Azure RemoteApp go), jeśli Centrum danych podstawowych jest wyłączony. Dane są kopiowane w czasie rzeczywistym do tej lokalizacji i firma Microsoft nie przechowuj kopii różnych wersji. Tak na uszkodzenia danych, firma Microsoft nie będzie mógł go przywrócić do poprzednio znaną dobrą wersję, ale jeśli Centrum danych podstawowy nie działa, można pobrać dane użytkownika z innej lokalizacji.

## <a name="how-do-users-see-the-upd-on-the-server-side"></a>Jak użytkownicy wyświetlić UDP po stronie serwera?

Każdy użytkownik będzie miał własnego katalogu na serwerze mapy do ich UDP: c:\Users\username.

## <a name="whats-the-best-way-to-use-outlook-and-upd"></a>Co to jest najlepszym sposobem używania programu Outlook i UDP?

Funkcja RemoteApp Azure zapisuje stanu programu Outlook (skrzynek pocztowych, plików pst) między sesjami. Aby włączyć tę opcję, potrzebujemy PST mają być przechowywane w danych profilu użytkownika (c:\users\<nazwa_użytkownika >). To jest domyślna lokalizacja dla danych, dlatego dopóki nie zostanie zmieniony tej lokalizacji, dane pozostają między sesjami.

Zalecamy również tryb "pamięci podręcznej" w programie Outlook, a następnie "server/online" w trybie wyszukiwania.

Zapoznaj się z [tego artykułu](remoteapp-outlook.md) , aby uzyskać więcej informacji na temat korzystania z programu Outlook i Azure RemoteApp.

## <a name="what-about-redirection"></a>Co z otwieraniem przekierowania?
Możesz skonfigurować RemoteApp Azure, aby umożliwić użytkownikom dostęp do urządzenia lokalne konfigurując [przekierowywania](remoteapp-redirection.md). Lokalne urządzenia będzie mógł uzyskać dostęp do danych na UDP.

## <a name="can-i-use-my-upd-as-a-network-share"></a>Czy mogę używać Moje UDP jako udziału sieciowego?
Wartość nie. UPDs nie można użyć jako udziału sieciowego. UDP jest dostępne dla użytkownika tylko w przypadku, gdy użytkownik aktywnie jest połączony ze Azure RemoteApp.

## <a name="if-i-delete-a-user-from-a-collection-is-their-upd-deleted"></a>Jeśli można usunąć użytkownika z kolekcji, zostanie usunięty ich UDP?

Nie, po usunięciu użytkownika firma Microsoft nie należy automatycznie usuwać UDP — zamiast tego przechowywane dane do momentu usunąć zbiór. 90 dni po usunięciu zbioru, możemy Usuń wszystkie UPDs. 

Jeśli musisz usunąć UDP z kolekcji, skontaktuj się z Azure RemoteApp - możemy usunąć UDP z naszych strony.

## <a name="can-i-access-my-users-upds-either-current-or-deleted-users"></a>Można uzyskać dostęp do moich użytkowników UPDs (użytkownicy bieżącego lub usunięte)?

Tak, jeśli kontakt [Azure RemoteApp](mailto:remoteappforum@microsoft.com), możemy skonfigurować możesz przy użyciu adresu URL, aby uzyskać dostęp do danych. Aby pobrać wszystkie dane lub pliki z UDP przed wygaśnięciem dostępu będą dostępne około 10 godzin.

## <a name="are-upds-available-offline"></a>Czy UPDs jest dostępna w trybie offline?

Obecnie nie udzielamy dostęp w trybie offline do UPDs, oprócz okna programu access 10 godzin opisany powyżej. Oznacza to, że firma Microsoft nie jest w sposób zapewniają dostęp długo za mało do wykonania bardziej skomplikowane zadania, takie jak oprogramowanie antywirusowe na UPDs lub uzyskiwania dostępu do danych w celu inspekcji.

## <a name="do-registry-key-settings-persist"></a>Ustawienia kluczy rejestru są zachowywane?
Tak, niczego zapisywać HKEY_Current_User jest częścią UDP.

## <a name="can-i-disable-upds-for-a-collection"></a>Czy mogę wyłączyć UPDs dla zbioru?

Tak, możesz zadać RemoteApp Azure, aby wyłączyć UPDs dla subskrypcji, ale nie można tego samodzielnie. Oznacza to, że UPDs zostanie wyłączona dla wszystkich zbiorów w subskrypcji.

Warto wyłączyć UPDs w następujących sytuacjach: 

- Potrzebna jest pełna dostępu i kontroli nad dane użytkowników (dla inspekcji i przeglądanie celów, takich jak instytucje finansowe).
- Masz 3rd strony profilu zarządzania rozwiązania lokalnego i chcesz, aby nadal z nich korzystać z nazwami Azure RemoteApp domeny. Wymagany agent profilu do załadowania do złoty obrazu. 
- Nie ma potrzeby wszelkie lokalne przechowywanie danych i wyświetlić wszystkie dane w chmurze lub w udziale plików, a potem sterowania zapisywania danych lokalnie za pomocą Azure RemoteApp.

Uzyskać więcej informacji, zobacz [Wyłączanie użytkownika profilu dysków (UPDs) w Azure RemoteApp](https://blogs.technet.microsoft.com/enterprisemobility/2015/11/11/disable-user-profile-disks-upds-in-azure-remoteapp/) .

## <a name="can-i-restrict-users-from-saving-data-to-the-system-drive"></a>Czy mogę ograniczyć użytkowników z zapisywania danych na dysk systemowy

Tak, ale musisz skonfigurować który w obraz szablonu przed utworzeniem kolekcji. Aby zablokować dostęp do dysku systemowym, wykonaj następujące czynności:

1. Uruchom **gpedit.msc** na obraz szablonu.
2. Przejdź do **Konfiguracja użytkownika > Szablony administracyjne > składniki Windows > Eksploratora**.
3. Zaznacz następujące opcje:
    - **Ukryj te określone dyski w oknie Mój komputer**
    - **Zapobiegaj dostępowi do dysków z mojego komputera**

## <a name="can-i-seed-upds-i-want-to-put-some-data-in-the-upd-thats-available-the-first-time-the-user-signs-in"></a>Czy może zapełnić UPDs? Chcę niektóre dane są rozmieszczone w UDP, który jest dostępny po raz pierwszy użytkownik znaki w.

Tak, gdy tworzysz obraz szablonu, możesz dodać informacje do domyślnego profilu. Te informacje są następnie jest dodawana do UDP.

## <a name="can-i-change-the-size-of-the-upd-depending-on-how-much-data-i-want-to-store"></a>Czy można zmienić rozmiar UDP w zależności od ilości przetwarzanych danych, które mają być przechowywane?

Nie, wszystkie UPDs mają 50 GB miejsca do magazynowania. Jeśli chcesz przechowywać różne ilości danych, spróbuj wykonać następujące czynności:

1. Wyłącz UPDs kolekcji.
2. Konfigurowanie udziału plików dostępu dla użytkowników.
3. Załaduj udziale plików za pomocą skryptu uruchamiania. Zobacz poniższą sekcję, aby uzyskać szczegółowe informacje na temat uruchamiania skryptów w Azure RemoteApp.
4. Udostępniając użytkownikom zapisywanie wszystkich danych w udziale plików.


## <a name="how-do-i-run-a-startup-script-in-azure-remoteapp"></a>Jak uruchomić skryptu uruchamiania w Azure RemoteApp?

Jeśli chcesz uruchomić skryptu uruchamiania, uruchomić, tworząc zaplanowane zadanie w obraz szablonu, którego chcesz użyć dla zbioru. (Zająć się tym *przed* uruchomieniem narzędzia sysprep.) 

![Tworzenie zadania systemowe](./media/remoteapp-upd/upd1.png)

![Tworzenie zadania systemu uruchamianego podczas logowania użytkownika](./media/remoteapp-upd/upd2.png)

Na karcie **Ogólne** upewnij się zmienić **Konto użytkownika** w obszarze zabezpieczenia, aby "BUILTIN\Użytkownicy."

![Zmienianie konta użytkownika do grupy](./media/remoteapp-upd/upd4.png)

Zadanie zaplanowane uruchomi skrypt uruchamiania przy użyciu poświadczeń użytkownika. Planowanie zadania, aby uruchomić co godzinę użytkownika logowania.

![Ustawianie wyzwalacz dla zadania jako "podczas logowania"](./media/remoteapp-upd/upd3.png)

Można również używać [skryptów uruchamiania zasad grupy](https://technet.microsoft.com/library/cc779329%28v=ws.10%29.aspx). 

## <a name="what-about-placing-a-startup-script-in-the-start-menu-would-that-work"></a>Jak umieszczanie skryptu uruchamiania, w Start menu? Czy to problemu?

Innymi słowy można utworzyć plik bat, który jest uruchamiany skrypt okno konfiguracji i zapisać go w folderze Start\Programy\Autostart c:\ProgramData\Microsoft\Windows\Start i wybrać tego skryptu uruchamianie przy każdym uruchomieniu sesji RemoteApp?

Nie, który nie jest obsługiwany z RemoteApp Azure, który używa RDSH, która nie obsługuje uruchamiania skryptów w Start menu.

## <a name="can-i-use-mstscexe-the-remote-desktop-program-to-configure-logon-scripts"></a>Aby skonfigurować skrypty logowania można używać mstsc.exe (program pulpitu zdalnego)?

Nie, dziękuję nie obsługuje Azure RemoteApp.

## <a name="can-i-store-data-on-the-vm-locally"></a>Dane można zapisać na maszyn wirtualnych lokalnie?

Nie, zostaną utracone dane przechowywane w dowolne miejsce na maszyn wirtualnych innych niż w UDP. Istnieje dużym prawdopodobieństwem użytkownika pojawi się tym samym maszyn wirtualnych przy następnym uruchomieniu Zaloguj się do Azure RemoteApp. Firma Microsoft nie obsługują utrzymywanie maszyn wirtualnych użytkownika, więc użytkownik nie będzie zalogować się do samej maszyn wirtualnych, a dane zostaną utracone. Ponadto podczas aktualizowania kolekcji, istniejące maszyny wirtualne są zastępowane nowego zestawu maszyny wirtualne -, który oznacza, że wszelkie dane przechowywane w maszyn wirtualnych się zostaną utracone. Zaleca się przechowywanie danych w UDP, udostępnionej przestrzeni dyskowej, takie jak pliki Azure, na serwerze plików wewnątrz VNET lub w chmurze przy użyciu systemu magazynowania chmury, takie jak skrzynki.

## <a name="how-do-i-mount-an-azure-file-share-on-a-vm-using-powershell"></a>Jak zainstalować udziale plików Azure na Głosowa, przy użyciu programu PowerShell?

Za pomocą polecenia cmdlet PSDrive netto ma zostać zainstalowany na dysku, w następujący sposób:

    New-PSDrive -Name <drive-name> -PSProvider FileSystem -Root \\<storage-account-name>.file.core.windows.net\<share-name> -Credential :<storage-account-name>


Możesz również zapisać poświadczenia, uruchamiając następujące czynności:

    cmdkey /add:<storage-account-name>.file.core.windows.net /user:<storage-account-name> /pass:<storage-account-key>


Które pozwala pominąć parametr Credential w polecenia cmdlet New-PSDrive.
