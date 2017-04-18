<properties
    pageTitle="Azure AD łączenie Agent kondycji instalacji | Microsoft Azure"
    description="To jest strona Azure AD łączenie kondycji opisujący instalacji agenta dla usług AD FS i synchronizacji."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>


# <a name="azure-ad-connect-health-agent-installation"></a>Azure AD Connect instalacji agenta kondycji

Ten dokument przeprowadzi Cię przez instalowania i konfigurowania Azure AD łączenie agentów kondycji. [Tutaj](active-directory-aadconnect-health.md#download-and-install-azure-ad-connect-health-agent)możesz pobrać agentów.

##  <a name="requirements"></a>Wymagania dotyczące
Poniższa tabela jest lista wymagań dotyczących przy użyciu Azure AD łączenie kondycji.

| Wymaganie | Opis|
| ----------- | ---------- |
|Azure AD Premium| Azure AD łączenie kondycji to funkcja Azure AD Premium i wymaga Azure AD Premium. </br></br>Aby uzyskać więcej informacji zobacz [Wprowadzenie do Azure AD Premium](active-directory-get-started-premium.md) </br>Aby uruchomić bezpłatną 30-dniową wersję próbną, zobacz [uruchomić wersji próbnej.](https://azure.microsoft.com/trial/get-started-active-directory/)|
|Musisz być administratorem globalnym usługi Azure AD rozpocząć pracę z Azure AD zdrowia nawiązywanie połączenia|Domyślnie tylko administrator globalny można zainstalować i skonfigurować agentów kondycji rozpocząć pracę, uzyskać dostęp do portalu i wykonywać żadnych operacji w ramach Azure AD łączenie kondycji. Aby uzyskać więcej informacji zobacz [Administrowanie katalogu Azure AD](active-directory-administer.md). <br><br> Za pomocą kontrola dostępu oparta roli można zezwolić dostęp do Azure AD łączenie kondycji innym użytkownikom w organizacji. Aby uzyskać więcej informacji, zobacz [roli podstawie kontrola dostępu Azure AD łączenie kondycji.](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control) </br></br>**Ważne:** Konto używane podczas instalowania agentów musi być konto służbowe. Nie może być konta Microsoft. Aby uzyskać więcej informacji zobacz [konta w usłudze Azure jako organizacji](sign-up-organization.md)
|Agent kondycji Azure łączenie AD jest zainstalowany na wszystkich serwerach docelowych| Azure AD łączenie zdrowia wymaga instalacji agenta na serwerach docelowych dostarczać dane, która jest wyświetlana w portalu. </br></br>Na przykład aby pobrać dane z infrastruktury lokalnego usług AD FS, agent musi być zainstalowany na serwerach usług AD FS, serwer Proxy usług AD FS i serwer Proxy aplikacji sieci Web. Podobnie, aby pobrać dane w swojej lokalnej Infrastruktura usług AD DS, agent musi być zainstalowana na kontrolerach domeny. </br></br>**Ważne:** Konto używane podczas instalowania agentów musi być konto służbowe. Nie może być konta Microsoft. Aby uzyskać więcej informacji zobacz [konta w usłudze Azure jako organizacji](sign-up-organization.md)|
|Nawiązywanie połączenia wychodzące punkty końcowe usługi Azure|Podczas instalacji i środowisko uruchomieniowe agent wymagają łączności z punktów końcowych usługi Azure AD łączenie kondycji. Zablokowanie ruchu wychodzącego łączności upewnij się, że następujące punkty końcowe są dodawane do listy dozwolonych: </br></br><li>& #42;. blob.Core.Windows.NET </li><li>& #42;. Queue.Core.Windows.NET</li><li>adhsprodwus.servicebus.Windows.NET - portu: 5671 </li><li>https://Management.Azure.com </li><li>https://S1.adhybridhealth.Azure.com/</li><li>https://policykeyservice.dc.AD.MSFT.NET/</li><li>https://login.Windows.NET</li><li>https://login.microsoftonline.com</li><li>https://Secure.aadcdn.microsoftonline-p.com</li> |
|Portów zapory na serwerze z agentem.| Agent wymaga następujące porty zapory, aby otwarte agenta komunikowanie się z punktów końcowych usługi Azure AD kondycji.</br></br><li>Port TCP/UDP na porcie 443</li><li>Port TCP/UDP 5671</li>
|Zezwalanie następujących witryn sieci Web, jeśli została włączona funkcja rozszerzonych zabezpieczeń programu Internet Explorer| Po włączeniu rozszerzonych zabezpieczeń programu Internet Explorer następujących witryn sieci Web musi dozwolone na serwerze, który ma agent jest zainstalowany.</br></br><li>https://login.microsoftonline.com</li><li>https://Secure.aadcdn.microsoftonline-p.com</li><li>https://login.Windows.NET</li><li>Serwer federacyjny dla organizacji zaufany przez usługi Azure Active Directory. Na przykład: https://sts.contoso.com</li>




## <a name="installing-the-azure-ad-connect-health-agent-for-ad-fs"></a>Instalowanie Azure AD łączenie Agent kondycji usług AD FS
Aby rozpocząć instalację agenta, kliknij dwukrotnie pobranego pliku .exe. Na pierwszym ekranie kliknij pozycję Zainstaluj.

![Sprawdź Azure AD Connect kondycji](./media/active-directory-aadconnect-health-requirements/install1.png)

Po zakończeniu instalacji kliknij pozycję Skonfiguruj teraz.

![Sprawdź Azure AD Connect kondycji](./media/active-directory-aadconnect-health-requirements/install2.png)

Wiersz polecenia jest uruchamiany, następuje niektórych programu PowerShell, który wykonuje AzureADConnectHealthADFSAgent rejestru. Po wyświetleniu monitu zaloguj się do Azure wtyczce i zaloguj się.

![Sprawdź Azure AD Connect kondycji](./media/active-directory-aadconnect-health-requirements/install3.png)


Po zalogowaniu się, nadal będzie programu PowerShell. Po zakończeniu instalacji możesz zamknąć programu PowerShell i zakończeniu konfiguracji.

W tym momencie usługi, należy uruchomić automatycznie oferujący agenta monitorowania i zbieranie danych. Jeśli nie zostały spełnione wymagania wstępne opisane w poprzedniej sekcji, ostrzeżenia są wyświetlane w oknie programu PowerShell. Należy wykonać przed rozpoczęciem instalacji agenta [wymagania](active-directory-aadconnect-health-agent-install.md#requirements) . Następujące zrzut ekranu przedstawia przykład tych błędów.

![Sprawdź Azure AD Connect kondycji](./media/active-directory-aadconnect-health-requirements/install4.png)

Aby sprawdzić, czy został zainstalowany agent, wyszukaj następujące usługi na serwerze. Jeśli ukończone konfiguracji one już powinna być uruchomiona. W przeciwnym razie są one zatrzymane aż do zakończenia konfiguracji.

- Azure AD Connect kondycji AD FS diagnostyki usługi
- Azure AD Connect usługi wniosków kondycja usług AD FS
- Azure AD Connect kondycji AD FS monitorowanie usługi

![Sprawdź Azure AD Connect kondycji](./media/active-directory-aadconnect-health-requirements/install5.png)


### <a name="agent-installation-on-windows-server-2008-r2-servers"></a>Agent instalacji na systemie Windows Server 2008 R2 serwerów

Instrukcje dotyczące serwerów Windows Server 2008 R2:

1. Upewnij się, że serwer działa z dodatkiem Service Pack 1 lub nowszym.
1. Wyłączanie IE ESC dla instalacji agenta:
1. Instalowanie programu Windows PowerShell w wersji 4.0 na wszystkich serwerach przed instalacją agent kondycji AD. Aby zainstalować program Windows PowerShell 4.0:
 - Zainstaluj [program Microsoft .NET Framework 4,5](https://www.microsoft.com/download/details.aspx?id=40779) przy użyciu poniższego łącza do pobrania Instalatora pakietu w trybie offline.
 - Instalowanie programu PowerShell ISE (z funkcjami systemu Windows)
 - Instalowanie [Windows Management Framework 4.0.](https://www.microsoft.com/download/details.aspx?id=40855)
 - Zainstaluj program Internet Explorer w wersji 10 lub nowszy na serwerze. (Wymagane przez usługę kondycji do uwierzytelnienia przy użyciu poświadczeń administratora Azure.)
1. Aby uzyskać więcej informacji na temat instalowania programu Windows PowerShell w wersji 4.0 w systemie Windows Server 2008 R2, zobacz artykuł wiki [tutaj](http://social.technet.microsoft.com/wiki/contents/articles/20623.step-by-step-upgrading-the-powershell-version-4-on-2008-r2.aspx).

### <a name="enable-auditing-for-ad-fs"></a>Włączanie inspekcji dotyczące usług AD FS

> [AZURE.NOTE] W tej sekcji dotyczą tylko serwerów federacyjnych usług AD FS.

Dla funkcji analizy użycia do gromadzenia i analizowania danych, agent Azure AD łączenie kondycji potrzebuje informacje znajdujące się w dzienników inspekcji AD FS. Dzienniki te nie są domyślnie włączone. Skorzystaj z poniższych instrukcji, aby włączyć inspekcję usług AD FS i Znajdź dzienników inspekcji AD FS na serwerach usług AD FS.

#### <a name="to-enable-auditing-for-ad-fs-20"></a>Aby włączyć inspekcję usług AD FS 2.0

1. Kliknij przycisk **Start**, wskaż polecenie **Programy**, wskaż polecenie **Narzędzia administracyjne**, a następnie kliknij **Zasady zabezpieczeń lokalnych**.
2. Przejdź do folderu **Usługi Zarządzanie prawami do zabezpieczeń zabezpieczeń\Zasady lokalne\Przypisywanie** , a następnie kliknij dwukrotnie pozycję Generuj inspekcje zabezpieczeń.
3. Na karcie **Ustawienia zabezpieczeń lokalnych** Sprawdź wymieniony konta usług AD FS 2.0 usługi. Jeśli nie jest dostępna, kliknij pozycję **Dodaj użytkownika lub grupę** i dodać go do listy, a następnie kliknij **przycisk OK**.
4. Aby włączyć inspekcję, otwórz wiersz polecenia z podwyższonym poziomem uprawnień, a następnie uruchom następujące polecenie:<code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable</code>
5. Zamknij zasady zabezpieczeń lokalnych, a następnie otwórz przystawki Zarządzanie. Aby otworzyć przystawki Zarządzanie, kliknij przycisk **Start**, wskaż polecenie **Programy**, wskaż polecenie **Narzędzia administracyjne**, a następnie kliknij AD FS 2.0 zarządzania.
6. W okienku Akcje kliknij polecenie Edytuj właściwości usługi federacyjne.
7. W oknie dialogowym **Właściwości usługi federacyjne** kliknij kartę **zdarzenia** .
8. Zaznacz pola wyboru **inspekcje sukcesów** i **inspekcji błąd** .
9. Kliknij **przycisk OK**.

#### <a name="to-enable-auditing-for-ad-fs-on-windows-server-2012-r2"></a>Aby włączyć inspekcję usług AD FS w systemie Windows Server 2012 R2

1. Otworzyć **Zasady zabezpieczeń lokalnych** , otwierając **Menedżera serwera** na ekranie startowym lub Server Manager na pasku zadań na pulpicie, a następnie kliknij polecenie **Narzędzia/lokalne zasady zabezpieczeń**.
2. Przejdź do folderu **Zabezpieczeń zabezpieczeń\Zasady Lokalne\przypisywanie praw** , a następnie kliknij dwukrotnie **Generowanie inspekcji zabezpieczeń**.
3. Na karcie **Ustawienia zabezpieczeń lokalnych** Sprawdź wymieniony konta usługi usług AD FS. Jeśli nie jest dostępna, kliknij pozycję **Dodaj użytkownika lub grupę** i dodać go do listy, a następnie kliknij **przycisk OK**.
4. Aby włączyć inspekcję, otwórz wiersz polecenia z podwyższonym poziomem uprawnień, a następnie uruchom następujące polecenie:<code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable.</code>
5. Zamknij **Lokalnych zasad zabezpieczeń**, a następnie otwórz przystawki **AD FS Zarządzanie** (w Menedżerze serwera kliknij pozycję Narzędzia, a następnie wybierz AD FS zarządzania).
6. W okienku Akcje kliknij polecenie **Edytuj właściwości usługi federacyjne**.
7. W oknie dialogowym właściwości usługi federacyjne kliknij kartę **zdarzenia** .
8. Zaznacz pola wyboru **inspekcje sukcesów i inspekcji błąd** , a następnie kliknij **przycisk OK**.






#### <a name="to-locate-the-ad-fs-audit-logs"></a>Aby zlokalizować inspekcji AD FS logowania


1. Otwórz program **Podgląd zdarzeń**.
2. Przejdź do pozycji Dzienniki systemu Windows i wybierz pozycję **Zabezpieczenia**.
3. Po prawej stronie kliknij pozycję **Dzienniki bieżącego filtru**.
4. W obszarze źródło zdarzenia wybierz **AD FS inspekcja**.

![Dzienniki inspekcji AD FS](./media/active-directory-aadconnect-health-requirements/adfsaudit.png)

> [AZURE.WARNING] Jeśli istnieje zasady grupy, który jest wyłączenie usług AD FS inspekcji, Azure AD łączenie Agent kondycji nie może zbieranie informacji. Upewnij się, że nie ma zasad grupy, który może wyłączyć inspekcję.

[//]: # (Start of Agent Proxy Configuration Section)

## <a name="installing-the-azure-ad-connect-health-agent-for-sync"></a>Instalowanie agenta Azure AD łączenie kondycji do synchronizacji
Agent Azure AD łączenie kondycji dla synchronizacji zostanie zainstalowana automatycznie w najnowszej kompilacji Azure AD Connect. Aby użyć Azure AD Connect synchronizacji, należy pobrać najnowszą wersję Azure AD Connect i zainstaluj go. Możesz pobrać najnowszą wersję [tutaj](http://www.microsoft.com/download/details.aspx?id=47594).

Aby sprawdzić, czy został zainstalowany agent, wyszukaj następujące usługi na serwerze. Jeśli ukończone konfiguracji one już powinna być uruchomiona. W przeciwnym razie są one zatrzymane aż do zakończenia konfiguracji.

- Azure AD Connect kondycji synchronizacji wniosków usługi
- Azure AD Connect synchronizacją zdrowia monitorowanie usługi

![Sprawdź Azure AD Connect kondycji do synchronizacji](./media/active-directory-aadconnect-health-sync/services.png)

> [AZURE.NOTE] Należy pamiętać, że przy użyciu Azure AD łączenie zdrowia wymaga Azure AD Premium. Jeśli nie masz Azure AD Premium, to nie można ukończyć konfigurację w portalu Azure. Aby uzyskać więcej informacji zobacz [wymagania dotyczące strony](active-directory-aadconnect-health-agent-install.md#requirements).


## <a name="manual-azure-ad-connect-health-for-sync-registration"></a>Ręczne Azure AD łączenie kondycji rejestracji synchronizacji
Jeśli Azure kondycji łączenie AD do synchronizacji agenta rejestracji kończy się niepowodzeniem po pomyślnym zainstalowaniu Azure AD Connect, możesz ręcznie zarejestrować agenta za pomocą następującego polecenia programu PowerShell.

>[AZURE.IMPORTANT] Przy użyciu tego polecenia programu PowerShell tylko jest wymagane, jeśli rejestracja agenta zakończy się niepowodzeniem po zainstalowaniu Azure AD Connect.

Następujące programu PowerShell, które polecenie jest wymagane tylko po rejestracji agenta kondycji kończy się niepowodzeniem, nawet po pomyślnym instalowania i konfigurowania Azure AD Connect. Usługi Azure AD łączenie kondycji rozpocznie się po agent został zarejestrowany.

Można ręcznie zarejestrować agenta Azure AD łączenie kondycji dla synchronizacja przy użyciu następującego polecenia programu PowerShell:

`Register-AzureADConnectHealthSyncAgent -AttributeFiltering $false -StagingMode $false`

Polecenie ma po parametrów:

- AttributeFiltering: $true (ustawienie domyślne) — Jeśli Azure AD Connect nie można zsynchronizować atrybutu domyślne ustawienie i został dostosowany się korzystać z zestawu atrybut filtrowania. $false w inny sposób.
- StagingMode: $false (ustawienie domyślne) — Jeśli serwer Azure AD Connect nie jest w trybie $true tymczasowej, jeśli serwer jest skonfigurowany w trybie tymczasowej.

Po wyświetleniu monitu uwierzytelniania należy używać tego samego konta administratora globalnego (takie jak admin@domain.onmicrosoft.com) użytego do konfigurowania Azure AD Connect.

## <a name="installing-the-azure-ad-connect-health-agent-for-ad-ds"></a>Instalowanie Azure AD łączenie Agent kondycji w usługach AD DS
Aby rozpocząć instalację agenta, kliknij dwukrotnie pobranego pliku .exe. Na pierwszym ekranie kliknij pozycję Zainstaluj.

![Sprawdź Azure AD Connect kondycji](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install1.png)

Po zakończeniu instalacji kliknij pozycję Skonfiguruj teraz.

![Sprawdź Azure AD Connect kondycji](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install2.png)

Wiersz polecenia jest uruchamiany, następuje niektórych programu PowerShell, który wykonuje AzureADConnectHealthADDSAgent rejestru. Po wyświetleniu monitu zaloguj się do Azure wtyczce i zaloguj się.

![Sprawdź Azure AD Connect kondycji](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install3.png)

Po zalogowaniu się, nadal będzie programu PowerShell. Po zakończeniu instalacji możesz zamknąć programu PowerShell i zakończeniu konfiguracji.

W tym momencie usługi, należy uruchomić automatycznie zezwalania agenta monitorowania i zbieranie danych. Jeśli nie zostały spełnione wymagania wstępne opisane w poprzedniej sekcji, ostrzeżenia są wyświetlane w oknie programu PowerShell. Należy wykonać przed rozpoczęciem instalacji agenta [wymagania](active-directory-aadconnect-health-agent-install.md#requirements) . Następujące zrzut ekranu przedstawia przykład tych błędów.

![Sprawdź Azure AD Connect kondycji w usługach AD DS](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install4.png)

Aby sprawdzić, czy został zainstalowany agent, wyszukaj następujące usługi na kontrolerze domeny.

- Azure AD Connect usługi wniosków kondycja usług AD DS
- Azure AD Connect kondycji AD DS monitorowanie usługi

Jeśli ukończone konfiguracji, należy już uruchomiony tych usług. W przeciwnym razie są one zatrzymana przed zakończeniem konfiguracji.

![Sprawdź Azure AD Connect kondycji](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install5.png)

## <a name="installing-the-azure-ad-connect-health-agent-for-ad-ds-on-server-core"></a>Instalowanie Azure AD łączenie Agent kondycji dla usług AD DS Server Core.
Po zainstalowaniu pliku .exe można ukończyć proces rejestracji przy użyciu następującego polecenia programu PowerShell:

`Register-AzureADConnectHealthADDSAgent -Credential $cred`

## <a name="configure-azure-ad-connect-health-agents-to-use-http-proxy"></a>Konfigurowanie Azure AD łączenie kondycji agentów do użytku serwer HTTP Proxy
Możesz skonfigurować Azure AD łączenie agentów kondycji do pracy z serwer HTTP Proxy.

>[AZURE.NOTE]
- Używanie "Netsh WinHttp Ustaw ProxyServerAddress" nie jest obsługiwane, jak agent używa System.Net w celu utworzenia żądania sieci web zamiast programu Microsoft Windows HTTP Services.
- Skonfigurowany adres serwera Http Proxy służy do przekazująca zaszyfrowanych wiadomości Https.
- Proxy uwierzytelniony (za pomocą HTTPBasic) nie są obsługiwane.

### <a name="change-health-agent-proxy-configuration"></a>Konfiguracja serwera Proxy agenta kondycji zmiany
Masz następujących opcji, aby skonfigurować Azure AD łączenie Agent kondycji używać serwer HTTP Proxy.

>[AZURE.NOTE]Wszystkie usługi Azure AD łączenie Agent kondycji należy ponownie, aby ustawienia serwera proxy, które mają być aktualizowane. Uruchom następujące polecenie:<br>
Restart-Service AdHealth *

#### <a name="import-existing-proxy-settings"></a>Importowanie istniejącego serwera proxy ustawienia

##### <a name="import-from-internet-explorer"></a>Importowanie z programu Internet Explorer
Ustawienia serwera proxy Internet Explorer HTTP mogą być importowane, może być używany we Azure AD łączenie agentów kondycji. Na wszystkich serwerach agent kondycji wykonaj następujące polecenia programu PowerShell:

    Set-AzureAdConnectHealthProxySettings -ImportFromInternetSettings

##### <a name="import-from-winhttp"></a>Importowanie z WinHTTP
Ustawienia serwera proxy WinHTTP mogą być importowane, może być używany we Azure AD łączenie agentów kondycji. Na wszystkich serwerach agent kondycji wykonaj następujące polecenia programu PowerShell:

    Set-AzureAdConnectHealthProxySettings -ImportFromWinHttp

#### <a name="specify-proxy-addresses-manually"></a>Ręczne określanie adresy serwerów Proxy
Można ręcznie określić serwer proxy na wszystkich serwerach Agent kondycji, wykonując następujące polecenia programu PowerShell:

    Set-AzureAdConnectHealthProxySettings -HttpsProxyAddress address:port

Przykład: *myproxyserver Set AzureAdConnectHealthProxySettings - HttpsProxyAddress: 443*

- "adres" mogą być rozpoznawana serwer DNS lub adres IP protokołu IPv4
- "port" mogą zostać pominięte. Jeśli pominięty, a następnie 443 został wybrany jako domyślny port.

#### <a name="clear-existing-proxy-configuration"></a>Wyczyść istniejącej konfiguracji serwera proxy
Możesz wyczyścić istniejącej konfiguracji serwera proxy, uruchamiając następujące polecenie:

    Set-AzureAdConnectHealthProxySettings -NoProxy


### <a name="read-current-proxy-settings"></a>Przeczytaj bieżące ustawienia serwera proxy
Ustawienia serwera proxy skonfigurowanego można przeczytać, uruchamiając następujące polecenie:

    Get-AzureAdConnectHealthProxySettings


## <a name="test-connectivity-to-azure-ad-connect-health-service"></a>Test łączności z programem Azure AD łączenie kondycji usługi
Istnieje możliwość problemy mogą wystąpić powodujące agenta Azure AD łączenie kondycji utracisz łączność z usługą Azure AD łączenie kondycji. Należą do problemów z siecią, problemów związanych z uprawnieniami lub różnych innych powodów.

Jeśli agent nie może wysłać dane do usługi Azure AD kondycji nawiązywanie połączenia przez dłużej niż dwie godziny, wyświetlany jest z następującego alertu w portalu: "kondycja usługi dane są aktualne." Można sprawdzić, czy dotyczy Azure AD Connect agent kondycji jest można przekazywać dane z usługą Azure AD kondycji nawiązywanie połączenia przy użyciu następującego polecenia programu PowerShell:

    Test-AzureADConnectHealthConnectivity -Role ADFS

Parametr rola ma obecnie następujące wartości:

- ADFS
- Synchronizowanie
- DODAJE

Flaga — ShowResults polecenia służy do wyświetlania szczegółowych dzienników. Użyć następującego przykładu:

    Test-AzureADConnectHealthConnectivity -Role Sync -ShowResult

>[AZURE.NOTE]Aby użyć narzędzia łączności, należy wykonać rejestracji agenta. Jeśli nie jesteś dokończyć rejestrację agenta, upewnij się, że spełnione wszystkie [wymagania](active-directory-aadconnect-health-agent-install.md#requirements) , Azure AD łączenie kondycji. Domyślnie podczas rejestrowania agenta przeprowadzany jest test łączności.



## <a name="related-links"></a>Łącza pokrewne

* [Azure AD Connect kondycji](active-directory-aadconnect-health.md)
* [Azure AD Connect operacje kondycji](active-directory-aadconnect-health-operations.md)
* [Podłącz za pomocą Azure AD kondycji z usług AD FS](active-directory-aadconnect-health-adfs.md)
* [Przy użyciu Azure AD łączenie kondycji dla synchronizacji](active-directory-aadconnect-health-sync.md)
* [Za pomocą Azure AD nawiązywanie połączenia z usługami AD DS kondycji](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect kondycji — często zadawane pytania](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect kondycji Historia wersji](active-directory-aadconnect-health-version-history.md)
