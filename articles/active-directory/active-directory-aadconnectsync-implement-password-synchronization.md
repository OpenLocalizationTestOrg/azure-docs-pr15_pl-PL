<properties
    pageTitle="Implementowania synchronizacji haseł z synchronizacją Azure AD Connect | Microsoft Azure"
    description="Informacje o sposobie działania synchronizacji haseł i jak je włączyć."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>
<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="markusvi;andkjell"/>


# <a name="implementing-password-synchronization-with-azure-ad-connect-sync"></a>Implementowania synchronizacji haseł z synchronizacją Azure AD Connect
W tym temacie przedstawiono informacje potrzebne do synchronizacji haseł użytkowników z lokalnej usługi Active Directory (AD) oparte na chmurze usługi Azure Active Directory (Azure AD).

## <a name="what-is-password-synchronization"></a>Co to jest synchronizacji haseł
Prawdopodobieństwo blokowane wykonywaną pracę z powodu zapomniane hasło jest powiązana z liczby różnych haseł, o których należy pamiętać. Więcej haseł, należy należy pamiętać, że wyższa prawdopodobieństwo zapomnieć jedną. Pytania i połączeń o resetowaniu haseł i innych problemów związanych z hasła zażądać większość zasobów działu pomocy technicznej.

Synchronizacja haseł jest funkcją do synchronizacji haseł użytkowników z usługi Active Directory w lokalnej opartej na chmurze usługi Azure Active Directory (Azure AD).
Ta funkcja umożliwia logowanie się do usługi Azure Active Directory (na przykład usługi Office 365, Microsoft Intune CRM Online i usługach domenowych AD Azure) przy użyciu tego samego hasła, aby zalogować się do usługi Active Directory w lokalnej.

![Co to jest Azure AD Connect](./media/active-directory-aadconnectsync-implement-password-synchronization/arch1.png)

Ograniczając liczbę haseł, które użytkownicy muszą korzystać z tylko z jednym, synchronizacji haseł ułatwia:

- Zwiększenie produktywności użytkowników
- Zmniejszanie kosztów działu pomocy technicznej  

Ponadto zaznaczenie za pomocą [**federacji z usług AD FS**](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect)opcjonalnie umożliwia synchronizacji haseł jako kopii zapasowej na wypadek, gdyby infrastruktury programu AD FS nie powiedzie się.

Synchronizacja haseł jest rozszerzenie funkcji synchronizacji katalogów wykonywane przez narzędzie Azure AD Connect synchronizacji. Aby użyć synchronizacji haseł w środowisku usługi, należy:

- Łączenie instalacji Azure AD  
- Konfigurowanie synchronizacji katalogu między swoje lokalnie AD i usługi Azure Active Directory
- Włączanie synchronizacji haseł

Aby uzyskać więcej szczegółowych informacji zobacz [Integrowanie tożsamości lokalnych z usługą Azure Active Directory](active-directory-aadconnect.md)

> [AZURE.NOTE] Aby uzyskać więcej informacji o usługach domenowych Active Directory skonfigurowane do synchronizacji z FIPS i hasło zobacz [synchronizacji haseł i FIPS](#password-synchronization-and-fips).

## <a name="how-password-synchronization-works"></a>Jak działa synchronizacja haseł
Domeny usługi Active Directory przechowuje hasła w postaci mieszania reprezentacja wartości rzeczywiste hasła. Wartość mieszania jest wynikiem jednokierunkowej funkcji matematycznej ("*algorytmu mieszania*"). Istnieje metoda aby cofnąć wynikiem funkcji jednokierunkowe do wersji jako zwykły tekst hasła. Nie można używać skrótu hasła, aby zalogować się do sieci lokalnej.

Aby zsynchronizować swoje hasło, narzędzie Azure AD Connect synchronizacją wyodrębnia z skrótu hasła w usłudze Active Directory w lokalnej. Przetwarzanie dodatkowych zabezpieczeń jest stosowana do skrótu hasła, zanim są synchronizowane z usługą uwierzytelniania usługi Azure Active Directory. Hasła są synchronizowane na poszczególnych użytkowników, a w kolejności chronologicznej.

Przepływ rzeczywistych danych procesu synchronizacji hasła jest podobna do synchronizacji danych użytkownika, takie jak DisplayName lub adresy E-mail. Jednak hasła są synchronizowane częściej niż inne atrybuty w oknie synchronizacji katalogu standard. Proces synchronizacji haseł uruchamia co 2 minuty. Nie można zmodyfikować częstotliwość ten proces. Po zsynchronizowaniu hasła zastępuje dotychczasowe hasło chmury.

Po raz pierwszy, Włącz funkcję Synchronizacja hasła, wykonuje początkowej synchronizacji hasła wszystkich użytkowników w zakresie. Nie można jawnie zdefiniować podzbiór hasła użytkowników, które chcesz zsynchronizować.

Po zmianie hasła lokalnego zaktualizowane hasło jest synchronizowane, najczęściej używane w ciągu kilku minut.
Funkcja synchronizacji hasła automatycznie prób prób synchronizacji nie powiodło się. Jeśli wystąpi błąd podczas próby synchronizować hasła, komunikat o błędzie jest zalogowany podglądu zdarzeń.

Synchronizacja hasła nie ma wpływu na obecnie zalogowany użytkownik.
Zmiany synchronizowane hasła, która pojawia się podczas po zalogowaniu się do usługi w chmurze nie występuje bezpośrednio w bieżącej sesji usługi cloud. Jednak podczas usług w chmurze wymaga uwierzytelniania ponownie, musisz podać nowe hasło.

> [AZURE.NOTE] Synchronizacji haseł jest obsługiwana tylko dla użytkownika typ obiektu w usłudze Active Directory. Nie jest obsługiwane dla typu obiektu iNetOrgPerson.

### <a name="how-password-synchronization-works-with-azure-ad-domain-services"></a>Jak działa synchronizacja haseł z usług domenowych AD Azure
Za pomocą funkcji synchronizacji hasło do synchronizacji hasła lokalnego w [usługach domenowych AD Azure](../active-directory-domain-services/active-directory-ds-overview.md). W tym scenariuszu umożliwia Azure usługach domenowych AD do uwierzytelniania użytkowników w chmurze przy użyciu metody dostępne w swojej lokalnej AD. Doświadczenia w tym scenariuszu jest podobne do korzystania z narzędzia migracji usługi Active Directory (ADMT) w środowisku lokalnym.

### <a name="security-considerations"></a>Zagadnienia dotyczące zabezpieczeń
Podczas synchronizacji haseł wersji zwykłego tekstu hasła jest udostępniana funkcję Synchronizacja hasła w celu Azure AD lub na dowolnej stronie skojarzonych usług.

Ponadto jest wymagany w usłudze Active Directory w lokalnej Aby zapisać hasło w formacie szyfrowanych. Skrót skrótu hasła usługi Active Directory jest używany do przekazywania między w lokalnym AD i usługi Azure Active Directory. Skrót skrótu hasła nie można uzyskać dostęp do zasobów w środowisku lokalnym.

### <a name="password-policy-considerations"></a>Uwagi dotyczące zasady haseł
Istnieją dwa rodzaje zasady haseł, które mogą mieć wpływ Włączanie synchronizacji haseł:

1. Zasady złożoności hasła
2. Zasad wygasania hasła

**Zasady złożoności hasła**  
Po włączeniu synchronizacji haseł zasady złożoność haseł w usłudze Active Directory w lokalnej zastępują zasady złożoność w chmurze dla synchronizowani użytkownicy. Wszystkie prawidłowe hasła usługi Active Directory w lokalnej umożliwia dostęp do usługi Azure AD.

> [AZURE.NOTE] Hasła dla użytkowników, które są tworzone bezpośrednio w chmurze są nadal objęte zasady haseł zdefiniowane w chmurze.

**Zasad wygasania hasła**  
Jeśli użytkownik znajduje się w zakresie synchronizacji haseł, hasło konta w chmurze jest równa "*Nigdy nie wygasa*".
Nadal można się zalogować do usługi cloud przy użyciu zsynchronizowane hasło, które już wygasła w środowisku lokalnym. Hasło chmurze jest aktualizowana przy następnym Zmienianie hasła w środowisku lokalnym.

### <a name="overwriting-synchronized-passwords"></a>Zastępowanie synchronizacji haseł
Administrator może ręcznie zresetować hasło, za pomocą programu Windows PowerShell.

W tym przypadku nowe hasło zastępuje synchronizowane hasła i wszystkie zasady haseł zdefiniowane w chmurze są stosowane do nowego hasła.

Jeśli zmienisz hasło lokalnego ponownie nowe hasło jest synchronizowane w chmurze, a zastępuje ręcznie zaktualizowane hasło.

## <a name="enabling-password-synchronization"></a>Włączanie synchronizacji haseł
Synchronizacja haseł jest włączany automatycznie, podczas instalowania narzędzie Azure AD Connect przy użyciu **Ustawienia ekspresowe**. Aby uzyskać więcej informacji zobacz [Wprowadzenie do narzędzie Azure AD Connect przy użyciu ustawień express](./connect/active-directory-aadconnect-get-started-express.md).

Jeśli używasz ustawień niestandardowych po zainstalowaniu Azure AD Connect, należy włączyć synchronizacji haseł na stronie logowania użytkownika. Aby uzyskać więcej informacji zobacz [Instalacja niestandardowa: narzędzie Azure AD Connect](./connect/active-directory-aadconnect-get-started-custom.md).

![Włączanie synchronizacji haseł](./media/active-directory-aadconnectsync-implement-password-synchronization/usersignin.png)

### <a name="password-synchronization-and-fips"></a>Synchronizacja haseł i FIPS
Serwer zostało zablokowane w dół według informacji przetwarzania FIPS (Federal Standard), następnie MD5 została wyłączona.

**Aby włączyć MD5 synchronizacji haseł, wykonaj następujące czynności:**

1. Przejdź do **%programfiles%\Azure AD Sync\Bin**.
2. Otwórz **miiserver.exe.config**.
3. Przejdź do węzeł **Konfiguracja/runtime** (na końcu pliku).
4. Dodaj węzeł następujące czynności:`<enforceFIPSPolicy enabled="false"/>`
5. Zapisz wprowadzone zmiany.

W przypadku odwołania ten wycinek jest, jak powinna wyglądać podobnie do:

```
    <configuration>
        <runtime>
            <enforceFIPSPolicy enabled="false"/>
        </runtime>
    </configuration>
```

Dla informacji dotyczących zabezpieczeń i FIPS [AAD synchronizacji haseł, szyfrowania i FIPS zgodności](https://blogs.technet.microsoft.com/enterprisemobility/2014/06/28/aad-password-sync-encryption-and-fips-compliance/)

## <a name="troubleshooting-password-synchronization"></a>Rozwiązywanie problemów z synchronizacją haseł
Jeśli nie można zsynchronizować hasła, zgodnie z oczekiwaniami, albo może być dla grupy użytkowników lub dla wszystkich użytkowników.

- Jeśli masz problem z poszczególnych obiektów, zobacz sekcję [Rozwiązywanie problemów z jednego obiektu, który nie jest synchronizowane hasła](#troubleshoot-one-object-that-is-not-synchronizing-passwords).
- Jeśli masz problem, gdzie hasła nie są synchronizowane, zobacz [Rozwiązywanie problemów, gdzie hasła nie są synchronizowane](#troubleshoot-issues-where-no-passwords-are-synchronized).

### <a name="troubleshoot-one-object-that-is-not-synchronizing-passwords"></a>Rozwiązywanie problemów z jednego obiektu, który nie jest synchronizowane hasła
Możesz łatwo rozwiązać problemy z synchronizacją haseł, przeglądając stan obiektu.

Uruchom w **użytkowników usługi Active Directory i komputerów**. Znajdź użytkownika i sprawdź, czy **użytkownik musi zmienić hasło przy następnym logowaniu** jest zaznaczona.
![Active Directory haseł wydajność](./media/active-directory-aadconnectsync-implement-password-synchronization/adprodpassword.png)  
Jeśli jest zaznaczona, następnie poproś użytkownika, aby zalogować się i zmienić hasło. Hasła tymczasowe nie są synchronizowane z Azure AD.

Jeśli wygląda poprawnie w usłudze Active Directory, następnym krokiem jest obserwowanie użytkownika aparatu synchronizacji. Wykonując użytkownika z usługi Active Directory w lokalnej do Azure AD, można sprawdzić, czy obiekt jest opisem błędu.

1. Uruchom **[Menedżera usługi synchronizacji](active-directory-aadconnectsync-service-manager-ui.md)**.
2. Kliknij pozycję **łączników**.
3. Zaznacz **Łącznik usługi Active Directory** użytkownika znajduje się w.
4. Wybierz **Obszar łączników wyszukiwania**.
5. Znajdź użytkownika, którego szukasz.
6. Wybierz kartę **dotyczące elementów nadrzędnych** i upewnij się, że co najmniej jedna reguła synchronizacji przedstawia **Synchronizacji haseł** **true**. W konfiguracji domyślnej nazwy reguły synchronizacji jest **w z usługi Active Directory - AccountEnabled użytkownika**.  
    ![Informacje dotyczące elementów nadrzędnych o użytkowniku](./media/active-directory-aadconnectsync-implement-password-synchronization/cspasswordsync.png)  
7. Następnie [Wykonaj użytkownika](active-directory-aadconnectsync-service-manager-ui-connectors.md#follow-an-object-and-its-data-through-the-system) za pośrednictwem metaverse do obszaru Azure AD łącznik. Obiekt miejsca łącznika powinien mieć reguły wychodzącej z **Synchronizacji haseł** , ustaw **wartość True**. W konfiguracji domyślnej nazwy reguły synchronizacji jest **zewnątrz do AAD — dołączanie do użytkownika**.  
    ![Łącznik odstęp właściwości użytkownika](./media/active-directory-aadconnectsync-implement-password-synchronization/cspasswordsync2.png)  
8. Aby wyświetlić szczegóły synchronizacji hasła obiektu dla ostatnim tygodniu, kliknij pozycję **dzienniku...**.  
    ![Szczegóły dziennika obiektu](./media/active-directory-aadconnectsync-implement-password-synchronization/csobjectlog.png)  

W kolumnie Stan może mieć następujące wartości:

Stan | Opis
---- | -----
Sukcesu | Hasło zostały pomyślnie zsynchronizowane.
FilteredByTarget | Hasło jest skonfigurowane do **użytkownik musi zmienić hasło przy następnym logowaniu**. Hasło nie zostały zsynchronizowane.
NoTargetConnection | Nie ma obiektu metaverse lub obszar łączników Azure AD.
SourceConnectorNotPresent | Nie można odnaleźć w obszarze łącznik usługi Active Directory w lokalnej obiektu.
TargetNotExportedToDirectory | Obiekt w obszarze łącznik Azure AD nie został wyeksportowany.
MigratedCheckDetailsForMoreInfo | Wpis dziennika został utworzony przed kompilacji 1.0.9125.0 i są wyświetlane w stanie starsze.

### <a name="troubleshoot-issues-where-no-passwords-are-synchronized"></a>Rozwiązywanie problemów, gdzie hasła nie są synchronizowane
Rozpocznij, uruchamiając skrypt w sekcji [sprawdzić stan ustawienia synchronizacji haseł](#get-the-status-of-password-sync-settings). Daje omówienie konfiguracji synchronizacji haseł.  
![Dane wyjściowe skrypt programu PowerShell ustawienia synchronizacji haseł](./media/active-directory-aadconnectsync-implement-password-synchronization/psverifyconfig.png)  
Jeśli ta funkcja nie jest włączona w Azure AD lub stan synchronizacji kanału nie jest włączona, następnie uruchom Kreatora instalacji Połącz. Wybierz **Opcje synchronizacji Dostosuj** , a następnie usuń zaznaczenie synchronizacji haseł. Ta zmiana tymczasowo wyłącza funkcję. Następnie ponownie uruchom kreatora i ponowne włączanie synchronizacji haseł. Uruchom skrypt ponownie, aby sprawdzić, czy konfiguracja jest poprawny.

Jeśli skrypt wskazuje, że istnieje nie weryfikowania, następnie uruchom skrypt w [wyzwalacza pełnej synchronizacji wszystkich haseł](#trigger-a-full-sync-of-all-passwords). Skrypt można również dla innych sytuacjach, gdy konfiguracja jest poprawna, ale hasła nie są synchronizowane.

#### <a name="get-the-status-of-password-sync-settings"></a>Pobierz stan ustawienia synchronizacji haseł

```
Import-Module ADSync
$connectors = Get-ADSyncConnector
$aadConnectors = $connectors | Where-Object {$_.SubType -eq "Windows Azure Active Directory (Microsoft)"}
$adConnectors = $connectors | Where-Object {$_.ConnectorTypeName -eq "AD"}
if ($aadConnectors -ne $null -and $adConnectors -ne $null)
{
    if ($aadConnectors.Count -eq 1)
    {
        $features = Get-ADSyncAADCompanyFeature -ConnectorName $aadConnectors[0].Name
        Write-Host
        Write-Host "Password sync feature enabled in your Azure AD directory: "  $features.PasswordHashSync
        foreach ($adConnector in $adConnectors)
        {
            Write-Host
            Write-Host "Password sync channel status BEGIN ------------------------------------------------------- "
            Write-Host
            Get-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector.Name
            Write-Host
            $pingEvents =
                Get-EventLog -LogName "Application" -Source "Directory Synchronization" -InstanceId 654  -After (Get-Date).AddHours(-3) |
                    Where-Object { $_.Message.ToUpperInvariant().Contains($adConnector.Identifier.ToString("D").ToUpperInvariant()) } |
                    Sort-Object { $_.Time } -Descending
            if ($pingEvents -ne $null)
            {
                Write-Host "Latest heart beat event (within last 3 hours). Time " $pingEvents[0].TimeWritten
            }
            else
            {
                Write-Warning "No ping event found within last 3 hours."
            }
            Write-Host
            Write-Host "Password sync channel status END ------------------------------------------------------- "
            Write-Host
        }
    }
    else
    {
        Write-Warning "More than one Azure AD Connectors found. Please update the script to use the appropriate Connector."
    }
}
Write-Host
if ($aadConnectors -eq $null)
{
    Write-Warning "No Azure AD Connector was found."
}
if ($adConnectors -eq $null)
{
    Write-Warning "No AD DS Connector was found."
}
Write-Host
```

#### <a name="trigger-a-full-sync-of-all-passwords"></a>Wyzwalanie pełnej synchronizacji wszystkich haseł
Mogą wyzwolić pełnej synchronizacji wszystkich haseł przy użyciu następującego skryptu:

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"
$aadConnector = "<CASE SENSITIVE AAD CONNECTOR NAME>"
Import-Module adsync
$c = Get-ADSyncConnector -Name $adConnector
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1
$c.GlobalParameters.Remove($p.Name)
$c.GlobalParameters.Add($p)
$c = Add-ADSyncConnector -Connector $c
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $false
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $true
```

## <a name="next-steps"></a>Następne kroki

* [Azure AD Sync łączenie: Dostosowywanie opcji synchronizacji](active-directory-aadconnectsync-whatis.md)
* [Integrowanie tożsamości lokalnych z usługi Azure Active Directory](active-directory-aadconnect.md)
