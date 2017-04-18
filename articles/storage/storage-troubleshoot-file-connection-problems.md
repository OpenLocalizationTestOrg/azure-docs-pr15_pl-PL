<properties
    pageTitle="Rozwiązywanie problemów z konfiguracją Azure plik miejsca do magazynowania | Microsoft Azure"
    description="Rozwiązywanie problemów z konfiguracją Azure plik miejsca do magazynowania"
    services="storage"
    documentationCenter=""
    authors="genlin"
    manager="felixwu"
    editor="na"
    tags="storage"/>

<tags
    ms.service="storage"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="genli"/>

# <a name="troubleshooting-azure-file-storage-problems"></a>Rozwiązywanie problemów z miejsca do magazynowania plików Azure

Ten artykuł zawiera listę problemów, które dotyczą magazyn plików w programie Microsoft Azure, gdy łączysz się z klientami Windows i Linux. Umożliwia także możliwe przyczyny i rozwiązania tych problemów.

**Ogólne problemy (występuje w systemach Windows i Linux klientów)**

- [Przydział komunikat o błędzie podczas próby otwarcia pliku](#quotaerror)

- [Spadek wydajności podczas uzyskiwania dostępu do przechowywania plików Azure z systemem Windows lub Linux](#slowboth)

**Problemy z klientami systemu Windows**

- [Spadek wydajności podczas uzyskiwania dostępu do przechowywania plików Azure z systemem Windows 8.1 lub Windows Server 2012 R2](#windowsslow)

- [Błąd 53 próba zainstalowania udziale plików Azure](#error53)

- [Użyj netto zakończyło się pomyślnie, ale nie ma udziału zainstalowanych w Eksploratorze Windows Azure pliku](#netuse)

- [Moje konto miejsca do magazynowania zawiera "/" i sieci za pomocą polecenia kończy się niepowodzeniem](#slashfails)

- [Moje aplikacji i usług nie ma dostępu zainstalowanego dysku Azure plików.](#accessfiledrive)

- [Dodatkowe zalecenia w celu zoptymalizowania wydajności](#additional)

**Problemy z klientami Linux**

- ["Kopiujesz plik do miejsca docelowego, która nie obsługuje szyfrowania" błąd podczas przekazywania i kopiowania plików do plików Azure](#encryption)

- [Udostępnia "Host nie działa" Błąd w systemie istniejący plik lub powłoki zawiesza się podczas listę poleceń w punkcie instalacji](#errorhold)

- [Błąd instalacji 115 podczas próby Azure instalacji plików na maszyn wirtualnych Linux](#error15)

- [Linux maszyn wirtualnych występują opóźnienia przekształcania przypadkowych pomysłów w poleceń, takich jak "ls"](#delayproblem)

## <a name="general-problems"></a>Ogólne problemy
<a id="quotaerror"></a>
### <a name="quota-error-when-trying-to-open-a-file"></a>Przydział komunikat o błędzie podczas próby otwarcia pliku

W systemie Windows komunikaty o błędach podobne do następujących:

**1816 ERROR_NOT_ENOUGH_QUOTA <> — 0xc0000044**

**STATUS_QUOTA_EXCEEDED**

**Za mało zasobów jest dostępna do przetworzenia tego polecenia**

**Uchwyt nieprawidłowe wartości GetLastError: 53**

W systemie Linux komunikaty o błędach podobne do następujących:

**<filename>[odmowa uprawnień]**

**Przekroczono przydział dysku**

#### <a name="cause"></a>Przyczyna

Problem występuje, ponieważ osiągnięto górną granicę równoczesne otwieranie uchwyty, przeznaczonych dla pliku.

#### <a name="solution"></a>Rozwiązanie

Zmniejsz liczbę równoczesne otwieranie uchwyty zamykając niektóre uchwytów, a następnie ponów próbę. Aby uzyskać więcej informacji zobacz [Microsoft Azure miejsca do magazynowania wydajność i skalowalność — Lista kontrolna](storage-performance-checklist.md).

<a id="slowboth"></a>
### <a name="slow-performance-when-accessing-file-storage-from-windows-or-linux"></a>Spadek wydajności podczas uzyskiwania dostępu do przechowywania plików z systemu Windows i Linux oraz

- Jeśli nie masz szczegółowe minimalne wymagania rozmiar we/wy, zaleca się użycie 1 MB jako rozmiar we/wy umożliwiające uzyskanie optymalnej wydajności.

- Jeśli znasz końcowy rozmiar pliku powiększa się zapisy i oprogramowanie nie ma problemy ze zgodnością podczas zakończenie nie zostały jeszcze zapisane w pliku zawierającego zer, ustaw rozmiar pliku z wyprzedzeniem zamiast każdego zapisu jest rozszerzanie zapisu.

## <a name="windows-client-problems"></a>Problemy z klientami systemu Windows
<a id="windowsslow"></a>
### <a name="slow-performance-when-accessing-the-file-storage-from-windows-81-or-windows-server-2012-r2"></a>Spadek wydajności podczas uzyskiwania dostępu do przechowywania plików w systemie Windows 8.1 lub Windows Server 2012 R2

Dla klientów, którzy korzystają z systemu Windows 8.1 lub Windows Server 2012 R2 upewnij się, czy jest zainstalowany, poprawka [KB3114025](https://support.microsoft.com/kb/3114025) . Ta poprawka zwiększa Utwórz i zamknij uchwyt wydajności.

Można uruchomić następujący skrypt, aby sprawdzić, czy poprawka został zainstalowany na:

`reg query HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies`

Jeśli jest zainstalowana poprawka, jest wyświetlany następujący komunikat:

**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies**

**{96c345ef-3cac-477b-8fcd-bea1a564241c} REG_DWORD 0x1**

> [AZURE.NOTE]  Obrazy systemu Windows Server 2012 R2 w Azure Marketplace mają poprawki KB3114025 instalowane domyślnie począwszy od grudnia 2015.

<a id="additional"></a>
### <a name="additional-recommendations-to-optimize-performance"></a>Dodatkowe zalecenia w celu zoptymalizowania wydajności

Nigdy nie Tworzenie lub otwieranie pliku dla pamięci podręcznej we/wy żąda zapisu, ale nie dostęp do odczytu. Oznacza to, że po możesz zadzwonić **CreateFile()**, nigdy nie określ **GENERIC_WRITE**, ale zawsze określić **GENERIC_READ | GENERIC_WRITE**. Uchwyt tylko do zapisu nie buforowania lokalnie, małych zapisu, nawet wtedy, gdy jest uchwytu tylko otwieranie pliku. To obniża trudnych wydajność dla małych zapisów. Należy zauważyć, że "" trybu CRT **fopen()** otwiera uchwyt tylko do zapisu.

<a id="error53"></a>
### <a name="error-53-when-you-try-to-mount-or-unmount-an-azure-file-share"></a>"Błąd 53" podczas próby zainstalować lub odinstalować udziale plików Azure

Ten problem może być spowodowany następujące warunki:

#### <a name="cause-1"></a>Przyczyny 1

"Wystąpił błąd systemu 53. Odmowa dostępu." Ze względów bezpieczeństwa połączenia do udziałów plików Azure są zablokowane, jeśli nie jest zaszyfrowany kanał komunikacji i nie następuje próba nawiązania połączenia z tym samym centrum danych, w którym znajdują się udziały plików Azure. Szyfrowanie kanału komunikacji jest niedostępna w przypadku użytkownika klienta system operacyjny nie obsługuje szyfrowania SMB. To jest wskazywana przez "Wystąpił błąd systemu 53. Odmowa dostępu"komunikat o błędzie podczas próby zainstalowania udziału plików z lokalnego lub centrum danych. Windows 8, Windows Server 2012 i nowsze wersje każdego żądania negotiate obejmującą 3.0 SMB obsługuje szyfrowanie.

#### <a name="solution-for-cause-1"></a>Rozwiązanie dla przyczyny 1

Nawiązywanie połączenia z klientem, która spełnia wymagania systemu Windows 8, Windows Server 2012 lub nowszy lub że nawiązywanie połączenia z maszyny wirtualnej, która znajduje się w tym samym centrum danych jako konto Azure miejsca do magazynowania, które jest używane do udziału pliku Azure.

#### <a name="cause-2"></a>Przyczyny 2

"Błąd systemu 53" podczas instalowania udziale plików Azure może wystąpić, jeśli Port 445 komunikacji wychodzącej do centrum danych plików Azure jest zablokowany. Kliknij [tutaj](http://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx) Aby wyświetlić podsumowanie usługodawców internetowych, którzy zezwalanie lub niezezwalanie na dostęp z portu 445.

Zablokowanie tego portu, Comcast i niektórych organizacjach IT. Aby dowiedzieć się, czy przyczyny za komunikat "Błąd systemu 53", umożliwia Portqry kwerendy punkt końcowy TCP:445. Jeśli punkt końcowy TCP:445 jest wyświetlane jako filtrowane, TCP port jest zablokowany. Oto przykładowe zapytania:

`g:\DataDump\Tools\Portqry>PortQry.exe -n [storage account name].file.core.windows.net -p TCP -e 445`

Jeśli TCP 445 jest zablokowany przez regułę na ścieżce sieci, zostanie wyświetlony następujący komunikat:

**Port TCP 445 (usługa microsoft ds): FILTROWANE**

Aby uzyskać więcej informacji na temat korzystania z narzędzia Portqry zobacz [Opis wiersza polecenia narzędzia Portqry.exe](https://support.microsoft.com/kb/310099).

#### <a name="solution-for-cause-2"></a>Rozwiązanie dla przyczyny 2

Praca z Twojej organizacji IT, aby otworzyć Port 445 wychodzące do [zakresów adresów IP Azure](https://www.microsoft.com/download/details.aspx?id=41653).

#### <a name="cause-3"></a>Przyczyny 3

"Błąd systemu 53" można także otrzymać włączenie komunikacji NTLMv1 na komputerze klienckim. Masz włączone NTLMv1 tworzy bezpiecznego mniej klienta. W związku z tym komunikacji zostaną zablokowane pliki Azure. Aby sprawdzić, czy jest to przyczyną błędu, upewnij się, że następujący podklucz rejestru jest ustawiona na wartość 3:

Kluczu HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel.

Aby uzyskać więcej informacji zobacz temat [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) w witrynie TechNet.

#### <a name="solution-for-cause-3"></a>Rozwiązanie dla przyczyny 3

Aby rozwiązać ten problem, zostaną przywrócone wartość LmCompatibilityLevel w kluczu HKLM\SYSTEM\CurrentControlSet\Control\Lsa klucza rejestru na wartość domyślną 3.

Pliki Azure obsługuje tylko uwierzytelnianie NTLMv2. Upewnij się, że zasady grupy są stosowane do klientów. Spowoduje to zapobiec występowaniu tego błędu. To jest również uważany za ze względów bezpieczeństwa. Aby uzyskać więcej informacji, zobacz [sposobu konfigurowania używania NTLMv2 przez klientów za pomocą zasad grupy](https://technet.microsoft.com/library/jj852207(v=ws.11).aspx)

Ustawienie zasad zalecane jest **tylko NTLMv2 wysłać odpowiedź**. Odpowiada to wartość rejestru 3. Klienci używać tylko uwierzytelnianie NTLMv2, a następnie użyj zabezpieczeń sesji NTLMv2, jeśli serwer ją obsługuje. Kontrolery domen akceptują uwierzytelnianie LM, NTLM i NTLMv2.

<a id="netuse"></a>
### <a name="net-use-was-successful-but-dont-see-the-azure-file-share-mounted-in-windows-explorer"></a>Użyj netto zakończyło się pomyślnie, ale nie widać udziału zainstalowanych w Eksploratorze Windows Azure pliku

#### <a name="cause"></a>Przyczyna

Domyślnie Eksploratora Windows nie polecenie Uruchom jako Administrator. Po uruchomieniu **netto korzystanie** z wiersza polecenia administratora, mapowanie dysku sieciowym "jako"Administrator. Ponieważ zamapowane dyski są oparte na użytkownika, konto użytkownika, który jest zalogowany nie są wyświetlane dyski, jeśli są one zainstalowanego w ramach innego konta użytkownika.

#### <a name="solution"></a>Rozwiązanie

Zainstaluj udział w wierszu polecenia niebędący administratorami. Można też kliknąć można wykonać [w tym temacie TechNet](https://technet.microsoft.com/library/ee844140.aspx) , aby skonfigurować wartość rejestru **EnableLinkedConnections** .

<a id="slashfails"></a>
### <a name="my-storage-account-contains--and-the-net-use-command-fails"></a>Moje konto miejsca do magazynowania zawiera "/" i sieci za pomocą polecenia kończy się niepowodzeniem

#### <a name="cause"></a>Przyczyna

Po uruchomieniu polecenia **Użyj netto** w obszarze wiersz polecenia (cmd.exe) jest analizowany, dodając "/" jako opcja wiersza polecenia. Ta opcja powoduje mapowanie dysków, aby zakończyć się niepowodzeniem.

#### <a name="solution"></a>Rozwiązanie

Jedną z poniższych kroków umożliwia obejść ten problem:

• Użyj następującego polecenia programu PowerShell:

`New-SmbMapping -LocalPath y: -RemotePath \\server\share  -UserName acountName -Password "password can contain / and \ etc"`

Z plik wsadowy można to zrobić jako

`Echo new-smbMapping ... | powershell -command –`

• Ująć podwójny cudzysłów klawisz, aby obejść ten problem, chyba że jest to pierwszy znak "/". Jeśli jest to, użyj tryb interakcyjny i wprowadź hasło oddzielnie albo wygenerować kluczy, aby uzyskać klucz, który nie rozpoczyna się znakiem ukośnika (/).

<a id="accessfiledrive"></a>
### <a name="my-applicationservice-cannot-access-mounted-azure-files-drive"></a>Moje aplikacji usługa nie ma dostępu zainstalowanego dysku pliki Azure

#### <a name="cause"></a>Przyczyna

Dyski są instalowane dla poszczególnych użytkowników. Aplikacji lub usługi jest uruchomiona w ramach innego konta użytkownika, użytkownicy nie będą widzieli stacji dysków.

#### <a name="solution"></a>Rozwiązanie

Dysk można zainstalować z tego samego konta użytkownika, w którym aplikacja jest. Można to zrobić za pomocą narzędzi, takich jak psexec.

Można także utworzyć nowego użytkownika, który ma takie same uprawnienia jak konto usługi lub systemu sieci, a następnie uruchom **cmdkey** i **Użyj netto** na tym koncie. Nazwa użytkownika należy nazwę konta magazynu, i hasło należy klucz konta miejsca do magazynowania. Innym rozwiązaniem do **użytku netto** jest przenieść nazwę konta magazynu i klucza w parametrach nazwę i hasło użytkownika polecenie **Użyj netto** .

Po wykonaniu tych instrukcji, może zostać wyświetlony następujący komunikat o błędzie: "Wystąpił błąd systemu 1312. Określona sesja logowania nie istnieje. Go może została już zakończona"po uruchomieniu **Użyj netto** dla konta usługi systemu i sieci. W takim przypadku upewnij się, że nazwa użytkownika, co wartość przekazywana do **użycia netto** zawiera informacje o domenie (na przykład: "[nazwę konta magazynu]. file.core.windows .net").

## <a name="linux-client-problems"></a>Problemy z klientami Linux

<a id="encryption"></a>
### <a name="error-you-are-copying-a-file-to-a-destination-that-does-not-support-encryption"></a>Błąd "Kopiujesz plik do miejsca docelowego, która nie obsługuje szyfrowania"

#### <a name="cause"></a>Przyczyna

Pliki zaszyfrowane funkcji BitLocker można skopiować do plików Azure. Magazyn plików nie obsługuje jednak NTFS systemu szyfrowania plików. W związku z tym prawdopodobnie używasz systemu szyfrowania plików w tym przypadku. Jeśli masz pliki zaszyfrowane system szyfrowania operacji kopiowania do przechowywania plików mogą nie być chyba że polecenia Kopiuj odszyfrowuje kopiowanego pliku.

#### <a name="workaround"></a>Obejście

Aby skopiować pliku do przechowywania plików, należy go najpierw odszyfrować. Możesz to zrobić przy użyciu jednej z następujących metod:

• Użyj **opcji/d. Kopiuj**.

• Ustawić następujący klucz rejestru:

- Ścieżka = HKLM\Software\Policies\Microsoft\Windows\System
- Typ wartości = DWORD
- Nazwa = CopyFileAllowDecryptedRemoteDestination
- Wartość = 1

Jednak pamiętać, że ustawienie klucza rejestru ma wpływ na wszystkie operacje kopii na udziały sieciowe.

<a id="errorhold"></a>
### <a name="host-is-down-error-on-existing-file-shares-or-the-shell-hangs-when-you-run-list-commands-on-the-mount-point"></a>Udostępnia "Host nie działa" Błąd w systemie istniejący plik lub powłoki zawiesza się po uruchomieniu listę poleceń w punkcie instalacji

#### <a name="cause"></a>Przyczyna

Ten błąd występuje na komputerze klienckim Linux, gdy przez dłuższy czas klienta. Jeśli wystąpi ten błąd, odłącza klienta i limit czasu połączenia klienta.

#### <a name="solution"></a>Rozwiązanie

Ten problem teraz zostanie ustawiony w jądrze Linux jako część [Zmienianie zestawu](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/fs/cifs?id=4fcd1813e6404dd4420c7d12fb483f9320f0bf93), oczekiwanie Poprawka usterki systemu do dystrybucji Linux.

Aby działa rozwiązać ten problem wyważonego połączenia i zapobiec wyświetlaniu w stan bezczynności, plik jest przechowywany w udziale plików Azure, którym okresowo pisać. To musi być operacji zapisu, na przykład poprawiania daty utworzone i modyfikacji na plik. W przeciwnym razie może zostać wyświetlony pamięci podręcznej wyniki, a operacja nie może być wyzwolenia połączenie.

<a id="error15"></a>
### <a name="mount-error-115-when-you-try-to-mount-azure-files-on-the-linux-vm"></a>"Błąd 115 instalacji" podczas próby zainstalowania plików Azure na maszyn wirtualnych Linux

#### <a name="cause"></a>Przyczyna

Rozkład Linux obsługuje jeszcze funkcji szyfrowania w SMB 3.0. W niektórych dystrybucji użytkownika może zostać wyświetlony komunikat o błędzie "115" Jeśli próby Azure instalacji plików przy użyciu SMB 3.0 ze względu na Brak funkcji.

#### <a name="solution"></a>Rozwiązanie

Jeśli używany klient Linux SMB nie obsługuje szyfrowania, Azure instalacji plików przy użyciu 2.1 SMB z maszyny Linux na tym samym centrum danych za pomocą konta miejsca do magazynowania plików.

<a id="delayproblem"></a>
### <a name="linux-vm-experiencing-random-delays-in-commands-like-ls"></a>Linux maszyn wirtualnych występują opóźnienia przekształcania przypadkowych pomysłów w poleceń, takich jak "ls"

#### <a name="cause"></a>Przyczyna

Dzieje się tak, gdy polecenie instalacji nie zawiera opcji **serverino** . Bez **serverino**polecenia ls działa **Stan** na każdy plik.

#### <a name="solution"></a>Rozwiązanie

Sprawdź **serverino** we wpisie "/ itp/fstab":

azureuser.File.Core.Windows.NET/WMS/Comer na/główne/sampledir typu cifs (rw, nodev, relatime, ver = 2.1, s = ntlmssp, pamięci podręcznej = ściśle, nazwa użytkownika = xxx domeny = X, file_mode = 0755, dir_mode = 0755, serverino, rsize = 65536, wsize = 65536, actimeo = 1)

Jeśli nie ma opcji **serverino** , odinstaluj i zainstaluj ponownie pliki Azure przez zaznaczoną opcją **serverino** .

## <a name="learn-more"></a>Dowiedz się więcej

- [Wprowadzenie do przechowywania plików Azure w systemie Windows](storage-dotnet-how-to-use-files.md)

- [Wprowadzenie do przechowywania plików Azure w systemie Linux](storage-how-to-use-files-linux.md)
