<properties
    pageTitle="Rozwiązywanie problemów z diagnostyki Azure"
    description="Rozwiązywanie problemów z podczas przy użyciu diagnostyki Azure usług w chmurze Azure, maszyn wirtualnych i "
    services="multiple"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="robb"/>


# <a name="azure-diagnostics-troubleshooting"></a>Narzędzia diagnostyczne Azure, rozwiązywanie problemów
Informacje dotyczące używania narzędzia diagnostyczne Azure do rozwiązywania problemów. Aby uzyskać więcej informacji na diagnostyki Azure zobacz [Omówienie diagnostyki Azure](azure-diagnostics.md#cloud-services).

## <a name="azure-diagnostics-is-not-starting"></a>Diagnostyka Azure nie jest uruchamiana.

Narzędzia diagnostyczne składa się z dwóch części: wtyczki agenta gościa i monitorowania agenta. Możesz sprawdzić pliki dziennika **DiagnosticsPluginLauncher.log** i **DiagnosticsPlugin.log** informacji na temat Dlaczego diagnostyki nie można uruchomić.  
  
W roli usługi w chmurze pliki dziennika dla wtyczki agenta gości znajdują się w: 

``` 
C:\Logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\1.6.3.0\ 
``` 

W Azure maszyny wirtualnej, pliki dziennika dla wtyczki agenta gości znajdują się w: 

``` 
C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\1.6.3.0\Logs\ 
``` 
 
Ostatni wiersz pliki dziennika będzie zawierać kod zakończenia.  

``` 
DiagnosticsPluginLauncher.exe Information: 0 : [4/16/2016 6:24:15 AM] DiagnosticPlugin exited with code 0 
``` 

Dodatek zwraca poniższe kody zakończenia:

Kod wyjścia|Opis
---|---
0|Zakończyło się pomyślnie.
-1|Błąd ogólny.
-2|Nie można załadować pliku rcf.<p>Jest to błąd wewnętrzny, który powinien wystąpić tylko, jeśli niepoprawnie, uruchom dodatek agenta gościa ręcznie jest wywoływana na maszyn wirtualnych.
-3|Nie można załadować pliku konfiguracji diagnostyki.<p><p>Rozwiązanie: Jest to wynikiem pliku konfiguracji nie przechodzące sprawdzania poprawności schematu. Rozwiązanie jest zapewnienie pliku konfiguracji, który jest zgodny ze schematem.
-4|Inne wystąpienie kontrolna diagnostyki już korzysta z katalogu zasobów lokalnych.<p><p>Rozwiązanie: Określ inną wartość dla **LocalResourceDirectory**.
-6|Uruchom dodatek agenta gościa próbował Uruchamianie diagnostyki z nieprawidłowego wiersza polecenia.<p><p>Jest to błąd wewnętrzny, który powinien wystąpić tylko, jeśli niepoprawnie, uruchom dodatek agenta gościa ręcznie jest wywoływana na maszyn wirtualnych.
-10|Dodatek Narzędzia diagnostyczne zostało zakończone z powodu nieobsługiwanego wyjątku.
-11|Agent gościa nie może utworzyć odpowiedzialnego za uruchamianie i monitorowanie agenta monitorowania procesu.<p><p>Rozwiązanie: Sprawdź, czy wystarczające zasoby systemowe są dostępne dla uruchamianie nowych procesów.<p>
-101|Nieprawidłowe argumenty podczas połączenia dodatek Narzędzia diagnostyczne.<p><p>Jest to błąd wewnętrzny, który powinien wystąpić tylko, jeśli niepoprawnie, uruchom dodatek agenta gościa ręcznie jest wywoływana na maszyn wirtualnych.
-102|Proces wtyczka nie może się zainicjować.<p><p>Rozwiązanie: Sprawdź, czy wystarczające zasoby systemowe są dostępne dla uruchamianie nowych procesów.
-103|Proces wtyczka nie może się zainicjować. W szczególności jest nie można utworzyć obiektu rejestratora.<p><p>Rozwiązanie: Sprawdź, czy wystarczające zasoby systemowe są dostępne dla uruchamianie nowych procesów.
-104|Nie można załadować pliku rcf dostarczonego przez agenta gościa.<p><p>Jest to błąd wewnętrzny, który powinien wystąpić tylko, jeśli niepoprawnie, uruchom dodatek agenta gościa ręcznie jest wywoływana na maszyn wirtualnych.
-105|Dodatek diagnostyki nie można otworzyć pliku konfiguracji diagnostyki.<p><p>Jest to błąd wewnętrzny, który należy tylko wystąpić, jeśli niepoprawnie, dodatek diagnostyki ręcznie jest wywoływana na maszyn wirtualnych.
-106|Nie można odczytać pliku konfiguracji diagnostyki.<p><p>Rozwiązanie: Jest to wynikiem pliku konfiguracji nie przechodzące sprawdzania poprawności schematu. Dlatego rozwiązanie jest zapewnienie pliku konfiguracji, który jest zgodny ze schematem. Można znaleźć plik XML, który został dostarczony do rozszerzenia diagnostyki w folderze *%SystemDrive%\WindowsAzure\Config* na maszyn wirtualnych. Otwórz odpowiedni plik XML oraz wyszukiwania **Microsoft.Azure.Diagnostics**, a następnie w polu **xmlCfg** . Dane są kodowane, więc musisz [dekodowanie go](http://www.bing.com/search?q=base64+decoder) wyświetlić plik XML, który został załadowany przez diagnostyki base64.<p>
-107|Przejście katalogu zasobów do monitorowania agenta jest nieprawidłowy.<p><p>Jest to błąd wewnętrzny, który należy tylko wystąpić, jeśli niepoprawnie, jednostka kontrolna ręcznie jest wywoływana na maszyn wirtualnych.</p>
-108    |Nie można przekonwertować pliku konfiguracji diagnostyki do monitorowania pliku konfiguracji agenta.<p><p>Jest to błąd wewnętrzny, który należy tylko wystąpić, jeśli dodatek diagnostyki ręcznie wywoływana jest nieprawidłowy plik konfiguracji.
-110|Błąd konfiguracji diagnostyki ogólne.<p><p>Jest to błąd wewnętrzny, który należy tylko wystąpić, jeśli dodatek diagnostyki ręcznie wywoływana jest nieprawidłowy plik konfiguracji.
-111|Nie można uruchomić agenta monitorowania.<p><p>Rozwiązanie: Sprawdź, czy zasoby systemowe wystarczające są dostępne.
-112|Błąd ogólny


## <a name="diagnostics-data-is-not-logged-to-azure-storage"></a>Narzędzia diagnostyczne dane znajdują się nie jest zalogowany do magazynu platformy Azure
Diagnostyka Azure są przechowywane wszystkie dane w magazynie Azure.

Najczęstsze przyczyny brakujące dane zdarzenie jest nieprawidłowe zdefiniowanej przechowywania informacji o koncie.

Rozwiązanie: Popraw pliku konfiguracji narzędzia diagnostyczne i ponownie zainstalować narzędzia diagnostyczne.
Jeśli problem będzie nadal występował, po ponownym zainstalowaniu rozszerzenia diagnostyki, a następnie może być konieczne debugowanie dalszych przeglądając monitorowania jakiekolwiek błędy agenta. Zanim dane zdarzenie zostało przesłane do swojego konta miejsca do magazynowania znajduje się w LocalResourceDirectory.

Dla roli usługi w chmurze LocalResourceDirectory jest:

    C:\Resources\Directory\<CloudServiceDeploymentID>.<RoleName>.DiagnosticStore\WAD<DiagnosticsMajorandMinorVersion>\Tables

W przypadku maszyn wirtualnych LocalResourceDirectory jest:

    C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\WAD<DiagnosticsMajorandMinorVersion>\Tables

Jeśli nie ma żadnych plików w folderze LocalResourceDirectory, nie można uruchomić jest jednostka kontrolna. Jest to zazwyczaj spowodowane przez plik nieprawidłowa konfiguracja zdarzenie, które powinny być zgłaszane w CommandExecution.log.

Agent monitorowania pomyślnie zbiera dane zdarzenie zobaczysz pliki .tsf dla każdego zdarzenia zdefiniowane w pliku konfiguracji. Agent monitorowania dzienniki jej błędów w pliku MaEventTable.tsf. Za pomocą aplikacji tabel2csv Aby przekonwertować plik .tsf pliku values(.csv) oddzielone przecinkami, aby sprawdzić zawartość tego pliku:

Z uprawnieniami usługi w chmurze:

    %SystemDrive%\Packages\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<DiagnosticsVersion>\Monitor\x64\table2csv maeventtable.tsf

*% Dysk_systemowy %* z uprawnieniami usługi w chmurze jest zazwyczaj D:

Na komputerze wirtualnej:

    C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\Monitor\x64\table2csv maeventtable.tsf

Powyższe polecenia generuje dziennika plik *maeventtable.csv*, który można otworzyć, a inspekcja komunikaty o błędach.    


## <a name="diagnostics-data-tables-not-found"></a>Narzędzia diagnostyczne danych nie można odnaleźć tabel
Tabele w magazynie Azure przechowywania danych diagnostyki Azure są nazywane za pomocą kodu poniżej:

        if (String.IsNullOrEmpty(eventDestination)) {
            if (e == "DefaultEvents")
                tableName = "WADDefault" + MD5(provider);
            else
                tableName = "WADEvent" + MD5(provider) + eventId;
        }
        else
            tableName = "WAD" + eventDestination;

Oto przykład:

        <EtwEventSourceProviderConfiguration provider=”prov1”>
          <Event id=”1” />
          <Event id=”2” eventDestination=”dest1” />
          <DefaultEvents />
        </EtwEventSourceProviderConfiguration>
        <EtwEventSourceProviderConfiguration provider=”prov2”>
          <DefaultEvents eventDestination=”dest2” />
        </EtwEventSourceProviderConfiguration>

Który wygeneruje 4 tabel:

Zdarzenia|Nazwa tabeli
---|---
Dostawca = "prov1" &lt;identyfikator zdarzenia = "1"-&gt;|WADEvent + MD5("prov1") + "1"
Dostawca = "prov1" &lt;identyfikator zdarzenia = "2" eventDestination = "dest1"-&gt;|WADdest1
Dostawca = "prov1" &lt;DefaultEvents-&gt;|WADDefault+MD5("prov1")
Dostawca = "prov2" &lt;DefaultEvents eventDestination = "dest2"-&gt;|WADdest2
