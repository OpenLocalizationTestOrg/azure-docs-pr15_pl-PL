<properties
pageTitle="Włączanie Podłączanie pulpitu zdalnego dla roli w usług w chmurze Azure przy użyciu programu PowerShell"
description="Jak skonfigurować aplikację usługi azure chmurze przy użyciu programu PowerShell, aby umożliwić połączenia pulpitu zdalnego"
services="cloud-services"
documentationCenter=""
authors="thraka"
manager="timlt"
editor=""/>
<tags
ms.service="cloud-services"
ms.workload="tbd"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="08/05/2016"
ms.author="adegeo"/>

# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services-using-powershell"></a>Włączanie Podłączanie pulpitu zdalnego dla roli w usług w chmurze Azure przy użyciu programu PowerShell

>[AZURE.SELECTOR]
- [Portal Azure klasyczny](cloud-services-role-enable-remote-desktop.md)
- [Programu PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
- [Programu Visual Studio](../vs-azure-tools-remote-desktop-roles.md)


Pulpit zdalny pozwala uzyskać dostęp do pulpitu roli działa Azure. Podłączanie pulpitu zdalnego umożliwia rozwiązywanie problemów i diagnozowanie problemów z aplikacją, gdy jest uruchomiony.

W tym artykule opisano włączanie pulpitu zdalnego w poszczególnych ról usługi Cloud przy użyciu programu PowerShell. Zobacz, [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) dla wstępnych potrzebne w tym artykule. PowerShell korzysta z rozszerzeniem pulpitu zdalnego, możesz włączyć pulpitu zdalnego po wdrożeniu aplikacji.


## <a name="configure-remote-desktop-from-powershell"></a>Konfigurowanie pulpitu zdalnego z programu PowerShell

Polecenie cmdlet [Set-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495117.aspx) umożliwia włączenie pulpitu zdalnego w określonych ról lub wszystkich ról rozmieszczenia usługi w chmurze. Polecenie cmdlet umożliwia określenie nazwy użytkownika i hasła użytkownika pulpitu zdalnego przez parametr *Credential* , która akceptuje obiekt parametr PSCredential.

Jeśli korzystasz z programu PowerShell interakcyjne, łatwo można ustawić obiekt parametr PSCredential, dzwoniąc do polecenia cmdlet [Get-poświadczeń](https://technet.microsoft.com/library/hh849815.aspx) .

```
$remoteusercredentials = Get-Credential
```

To polecenie wyświetla okno dialogowe umożliwiające wprowadź nazwę użytkownika i hasło użytkownika zdalnego w bezpieczny sposób.

Ponieważ programu PowerShell pomaga w scenariuszach automatyzacji, można również skonfigurować obiekt **parametr PSCredential** w taki sposób, aby nie wymaga interakcji użytkownika. Najpierw musisz skonfigurować bezpieczne hasło. Zaczynać się określanie hasła w formacie zwykłego tekstu przekształceniu w ciągu bezpiecznego przy użyciu [ConvertTo SecureString](https://technet.microsoft.com/library/hh849818.aspx). Następnie należy konwertować ten ciąg bezpiecznego zaszyfrowany ciąg standardowego za pomocą [ConvertFrom SecureString](https://technet.microsoft.com/library/hh849814.aspx). Teraz możesz zapisać ten zaszyfrowany ciąg standardowy do pliku przy użyciu [Zestawu zawartości](https://technet.microsoft.com/library/ee176959.aspx).

Można także tworzyć pliku bezpieczne hasło, dzięki czemu nie trzeba wpisać w polu hasło zawsze. Ponadto plik bezpieczne hasło jest większa niż plik tekstowy. Tworzenie pliku bezpieczne hasło za pomocą następujących programu PowerShell:

```
ConvertTo-SecureString -String "Password123" -AsPlainText -Force | ConvertFrom-SecureString | Set-Content "password.txt"
```

>[AZURE.IMPORTANT] Podczas ustawiania hasła, upewnij się, że spełniają [wymagania dotyczące złożoności](https://technet.microsoft.com/library/cc786468.aspx).

Aby utworzyć obiekt poświadczenia z pliku bezpieczne hasło, możesz przeczytać zawartość pliku i przekonwertować je z powrotem na ciągu bezpiecznego korzystania [ConvertTo SecureString](https://technet.microsoft.com/library/hh849818.aspx).

Polecenie cmdlet [Set-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495117.aspx) akceptuje także parametr *wygaśnięcia* , który określa **daty i godziny** , co konto użytkownika wygaśnie. Na przykład można ustawić konto wygasa kilka dni od bieżącej daty i godziny.

W tym przykładzie programu PowerShell pokazano, jak skonfigurować rozszerzenia pulpitu zdalnego na usługi w chmurze:

```
$servicename = "cloudservice"
$username = "RemoteDesktopUser"
$securepassword = Get-Content -Path "password.txt" | ConvertTo-SecureString
$expiry = $(Get-Date).AddDays(1)
$credential = New-Object System.Management.Automation.PSCredential $username,$securepassword
Set-AzureServiceRemoteDesktopExtension -ServiceName $servicename -Credential $credential -Expiration $expiry
```
Można też dodatkowo określić przedział wdrożenia i role, które chcesz włączyć pulpitu zdalnego. Jeśli te parametry nie zostaną określone, polecenie cmdlet umożliwia pulpitu zdalnego na wszystkich ról w przedział wdrożenia **produkcji** .

Rozszerzenie pulpitu zdalnego jest skojarzone z wdrożenia. Jeśli tworzysz nowy wdrożenia usługi, musisz włączyć pulpitu zdalnego w tym wdrożenia. Jeśli chcesz mieć włączoną usługą Pulpit zdalny, następnie należy rozważyć integrowanie skryptów programu PowerShell wdrożenie przepływu pracy.


## <a name="remote-desktop-into-a-role-instance"></a>Pulpit zdalny do wystąpienia roli
Polecenia cmdlet [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) jest używana do pulpitu zdalnego do konkretnej roli wystąpienia usługi w chmurze. Za pomocą parametru *LocalPath* o pobranie pliku RDP lokalnie. Lub można użyć parametru *Uruchamianie* do bezpośrednio Uruchom okno dialogowe Podłączanie pulpitu zdalnego, aby uzyskać dostęp do wystąpienia roli usługi cloud.

```
Get-AzureRemoteDesktopFile -ServiceName $servicename -Name "WorkerRole1_IN_0" -Launch
```


## <a name="check-if-remote-desktop-extension-is-enabled-on-a-service"></a>Sprawdź, czy rozszerzenia pulpitu zdalnego jest włączona w usłudze
Polecenia cmdlet [Get-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495261.aspx) Wyświetla, którego Pulpit zdalny jest włączony lub wyłączony wdrożenia usługi. Polecenie cmdlet zwraca nazwę użytkownika dla użytkownika pulpitu zdalnego i ról, które włączono rozszerzenia pulpitu zdalnego. Domyślnie w takim przypadku na przedział wdrożenia i możesz użyć tymczasowy przedział.

```
Get-AzureServiceRemoteDesktopExtension -ServiceName $servicename
```

## <a name="remove-remote-desktop-extension-from-a-service"></a>Usuwanie rozszerzenia pulpitu zdalnego z usługą
Jeśli włączono już rozszerzenia pulpitu zdalnego wdrożenia i aby zaktualizować ustawienia pulpitu zdalnego, należy najpierw usunąć rozszerzenia. I włącz go ponownie z nowymi ustawieniami. Jeśli na przykład chcesz ustawić nowe hasło dla konta użytkownika zdalnego ani nie wygasły konta. Spowoduje to jest wymagane na istniejące wdrożeniach, które mają rozszerzenie pulpitu zdalnego włączone. W przypadku nowych wdrożeń możesz po prostu zastosować rozszerzenie bezpośrednio.

Aby usunąć z rozszerzeniem pulpitu zdalnego z wdrożenia, możesz użyć polecenia cmdlet [AzureServiceRemoteDesktopExtension Usuń](https://msdn.microsoft.com/library/azure/dn495280.aspx) . Możesz też dodatkowo określić przedział wdrożenia i roli, z którego chcesz usunąć z rozszerzeniem pulpitu zdalnego.

```
Remove-AzureServiceRemoteDesktopExtension -ServiceName $servicename -UninstallConfiguration
```

>[AZURE.NOTE] Aby całkowicie usunąć konfigurację rozszerzenia, można zadzwonić polecenia cmdlet *usunąć* z parametrem **UninstallConfiguration** .
>
>Parametr **UninstallConfiguration** odinstalowywania żadna konfiguracja rozszerzenia, która jest stosowana do usługi. Co Konfiguracja rozszerzenie jest skojarzony z konfiguracją usługi. Dlatego wywołanie polecenia cmdlet *usunąć* bez **UninstallConfiguration** powoduje usunięcie <mark>wdrożenia</mark> z konfiguracji rozszerzenia, skuteczne usuwanie rozszerzenia. Jednak konfiguracja rozszerzenia pozostaje skojarzone z usługą.



## <a name="additional-resources"></a>Dodatkowe zasoby

[Jak skonfigurować usług w chmurze](cloud-services-how-to-configure.md)
