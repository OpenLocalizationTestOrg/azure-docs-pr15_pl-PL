<properties
    pageTitle="Azure Active Directory Domain Services: Rozwiązywanie problemów z przewodnika | Microsoft Azure"
    description="Przewodnik rozwiązywania problemów dla usług domenowych AD Azure"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services---troubleshooting-guide"></a>Azure usługach domenowych AD - Podręcznik rozwiązywania problemów
Ten artykuł zawiera wskazówki dotyczące rozwiązywania problemów, które mogą wystąpić podczas konfigurowania lub administrowaniem usługami domeny Azure Active Directory (AD).


## <a name="you-cannot-enable-azure-ad-domain-services-for-your-azure-ad-directory"></a>Nie można włączyć usługi Azure AD domeny w katalogu Azure AD
W tej sekcji ułatwia rozwiązywanie problemów z błędami podczas próby włączyć usługi Azure AD domeny w katalogu i kończy się niepowodzeniem, lub otrzymuje ich "Wyłączona".

Wybierz odpowiadające czynności rozwiązywania problemów z komunikatem o błędzie napotkanych.

|**Komunikat o błędzie**|**Rozdzielczość**|
|---|:---|
|*Contoso100.com nazwa jest już używana w tej sieci. Określ nazwę, która nie jest używany.*|[Konflikt nazw domeny w wirtualnej sieci](active-directory-ds-troubleshooting.md#domain-name-conflict)
|*Nie można włączyć usług domenowych w dzierżawy Azure AD. Usługa nie ma odpowiednie uprawnienia do aplikacji o nazwie "Synchronizacji usługi Azure AD domeny". Usuwanie aplikacji o nazwie "Synchronizacji usługi Azure AD domeny", a następnie spróbuj włączyć usługi domen dla dzierżawy usługi Azure AD.*|[Usługi domenowe nie ma odpowiednie uprawnienia do aplikacji do synchronizacji usługi Azure AD domeny](active-directory-ds-troubleshooting.md#inadequate-permissions)|
|*Nie można włączyć usług domenowych w dzierżawy Azure AD. Aplikacja usług domenowych w dzierżawie usługi Azure AD nie ma wymagane uprawnienia, aby włączyć usługi domeny. Usuwanie aplikacji z d87dcbc6-a371-462e-88e3-28ad15ec4e64 identyfikator aplikacji, a następnie spróbuj włączyć usługi domen dla dzierżawy usługi Azure AD.*|[Aplikacja usług domenowych nie jest prawidłowo skonfigurowana w dzierżawie](active-directory-ds-troubleshooting.md#invalid-configuration)|
|*Nie można włączyć usług domenowych w dzierżawy Azure AD. Aplikacja Microsoft Azure AD jest wyłączona w dzierżawie usługi Azure AD. Włączanie aplikacji 00000002-0000-0000-c000-000000000000 identyfikator aplikacji, a następnie spróbuj włączyć usługi domen dla dzierżawy usługi Azure AD.*|[Aplikacja Microsoft Graph jest wyłączona w dzierżawie usługi Azure AD](active-directory-ds-troubleshooting.md#microsoft-graph-disabled)|


### <a name="domain-name-conflict"></a>Konflikt nazw domeny
**Komunikat o błędzie:**

*Contoso100.com nazwa jest już używana w tej sieci. Określ nazwę, która nie jest używany.*

**Rozwiązywanie problemu:**

Upewnij się, że nie masz istniejącej domeny z tą samą nazwą domeny, dostępne w tym wirtualnej sieci. Na przykład przyjęto założenie, że masz domenie o nazwie "contoso.com" już dostępne w zaznaczonej wirtualnej sieci. Później podczas próby Włączanie usług domenowych AD Azure zarządzanych domeny z tą samą nazwą domeny (czyli "contoso.com") w tym wirtualnej sieci. Wystąpienia awarii podczas próby włączyć usługi Azure AD domeny.

Ten błąd jest ze względu na konflikt nazw dla nazwy domeny, w tym wirtualnej sieci. W tej sytuacji należy użyć pod inną nazwą konfigurowania domeny zarządzanych usług domenowych AD Azure. Alternatywnie możesz Anuluj obsługi administracyjnej istniejącej domeny i Kontynuuj, aby włączyć usługi Azure AD domeny.


### <a name="inadequate-permissions"></a>Niewystarczające uprawnienia
**Komunikat o błędzie:**

*Nie można włączyć usług domenowych w dzierżawy Azure AD. Usługa nie ma odpowiednie uprawnienia do aplikacji o nazwie "Synchronizacji usługi Azure AD domeny". Usuwanie aplikacji o nazwie "Synchronizacji usługi Azure AD domeny", a następnie spróbuj włączyć usługi domen dla dzierżawy usługi Azure AD.*

**Rozwiązywanie problemu:**

Sprawdź, czy jest aplikacji o nazwie "Synchronizacji usługi Azure AD domeny" w katalogu Azure AD. Jeśli istnieje tej aplikacji, usuń ją, a następnie ponownie włączyć usług domenowych AD Azure.

Jeśli aplikacja istnieje, należy wykonać następujące czynności Sprawdzanie obecności aplikacji i usuwania:

  1. Przejdź do **portalu klasyczny Azure** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).
  2. Wybierz węzeł **Usługi Active Directory** w okienku po lewej stronie.
  3. Wybierz pozycję dzierżawy Azure AD (katalog), dla którego chcesz włączyć usługi Azure AD domeny.
  4. Przejdź do karty **aplikacji** .
  5. Wybierz opcję **aplikacji właścicielem mojej firmy** , na liście rozwijanej.
  6. Sprawdź, czy aplikacji o nazwie **synchronizacji usługi Azure AD domeny**. Jeśli aplikacja istnieje, przejdź do ją usunąć.
  7. Po usunięciu aplikacji, spróbuj ponownie włączyć usługi Azure AD domeny.


### <a name="invalid-configuration"></a>Nieprawidłowa konfiguracja
**Komunikat o błędzie:**

*Nie można włączyć usług domenowych w dzierżawy Azure AD. Aplikacja usług domenowych w dzierżawie usługi Azure AD nie ma wymagane uprawnienia, aby włączyć usługi domeny. Usuwanie aplikacji z d87dcbc6-a371-462e-88e3-28ad15ec4e64 identyfikator aplikacji, a następnie spróbuj włączyć usługi domen dla dzierżawy usługi Azure AD.*

**Rozwiązywanie problemu:**

Sprawdź, czy masz wniosku o nazwie "AzureActiveDirectoryDomainControllerServices" (z identyfikator aplikacji d87dcbc6-a371-462e-88e3-28ad15ec4e64) w katalogu Azure AD. Jeśli istnieje tej aplikacji, należy ją usunąć, a następnie ponownie włączyć usług domenowych AD Azure.

Poniższy skrypt programu PowerShell umożliwia znajdowanie aplikacji i usuń go.

> [AZURE.NOTE] Ten skrypt używa **Azure AD w wersji 2** poleceń cmdlet. Aby uzyskać pełną listę wszystkich dostępnych poleceń cmdlet i pobrać moduł zapoznaj się z [programu AzureAD PowerShell dokumentacji](https://msdn.microsoft.com/library/azure/mt757189.aspx).

```
$InformationPreference = "Continue"
$WarningPreference = "Continue"

$aadDsSp = Get-AzureADServicePrincipal -Filter "AppId eq 'd87dcbc6-a371-462e-88e3-28ad15ec4e64'" -ErrorAction Ignore
if ($aadDsSp -ne $null)
{
    Write-Information "Found Azure AD Domain Services application. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $aadDsSp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services application."
}

$identifierUri = "https://sync.aaddc.activedirectory.windowsazure.com"
$appFilter = "IdentifierUris eq '" + $identifierUri + "'"
$app = Get-AzureADApplication -Filter $appFilter
if ($app -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync application. Deleting it ..."
    Remove-AzureADApplication -ObjectId $app.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync application."
}

$spFilter = "ServicePrincipalNames eq '" + $identifierUri + "'"
$sp = Get-AzureADServicePrincipal -Filter $spFilter
if ($sp -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync service principal. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $sp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync service principal."
}
```
<br>

### <a name="microsoft-graph-disabled"></a>Program Microsoft Graph wyłączone
**Komunikat o błędzie:**

Nie można włączyć usług domenowych w dzierżawy Azure AD. Aplikacja Microsoft Azure AD jest wyłączona w dzierżawie usługi Azure AD. Włączanie aplikacji 00000002-0000-0000-c000-000000000000 identyfikator aplikacji, a następnie spróbuj włączyć usługi domen dla dzierżawy usługi Azure AD.

**Rozwiązywanie problemu:**

Sprawdź wyłączenie aplikacji z identyfikatorem 00000002-0000-0000-c000-000000000000. Ta aplikacja jest aplikacji Microsoft Azure AD i zapewnia interfejsu API wykresu dostęp do Twojej dzierżawy Azure AD. Azure usług domenowych AD musi tej aplikacji są włączone do synchronizowania dzierżawcy usługi Azure AD z domeną zarządzane.

Aby rozwiązać ten problem, Włącz tę aplikację, a następnie spróbuj włączyć usługi domen dla dzierżawy usługi Azure AD.


## <a name="users-are-unable-to-sign-in-to-the-azure-ad-domain-services-managed-domain"></a>Użytkownicy nie są w stanie się zalogować do Azure AD domenie usług domenowych zarządzanych
Jeśli nie można się zalogować do nowo utworzonego domeny zarządzanych jednego lub kilku użytkowników w dzierżawie usługi Azure AD, wykonaj następujące czynności rozwiązywania problemów:

- **Logowania przy użyciu formatu UPN:** Spróbuj zalogować się przy użyciu formatu UPN (na przykład 'joeuser@contoso.com') zamiast format SAMAccountName (CONTOSO\joeuser). Atrybuty SAMAccountName mogą być automatycznie generowane dla użytkowników, których Prefiks głównej nazwy użytkownika jest zbyt długa lub jest taka sama jak innego użytkownika w domenie zarządzanych. Formatu UPN jest gwarantowana być unikatowa w dzierżawie usługi Azure AD.

> [AZURE.NOTE] Zalecamy korzystanie z formatem głównej nazwy użytkownika do zalogowania się do Azure AD domenie usług domenowych zarządzanych.

- Upewnij się, że masz [włączony synchronizacji haseł](active-directory-ds-getting-started-password-sync.md) zgodnie z instrukcjami podanymi w podręczniku Wprowadzenie.

- **Kont zewnętrzne:** Upewnij się, że konto użytkownika nie jest kontem zewnętrznych w dzierżawie Azure AD. Zewnętrzne kont przykładami konta serwera Microsoft (na przykład 'joe@live.com') lub innego konta użytkownika z zewnętrznego katalogu Azure AD. Ponieważ usług domenowych AD Azure nie ma poświadczeń dla tych kont użytkowników, tych użytkowników nie możesz się zalogować do domeny zarządzanych.

- **Synchronizowane konta:** Jeśli konta użytkownika są synchronizowane z katalogu lokalnego, sprawdź następujące elementy:
    - Możesz wdrożyć lub zaktualizowane do [najnowszych zalecane wersji Azure AD Connect](active-directory-ds-getting-started-password-sync.md#install-or-update-azure-ad-connect).

    - Skonfigurowano narzędzie Azure AD Connect do [wykonania pełnej synchronizacji](active-directory-ds-getting-started-password-sync.md).

    - W zależności od rozmiaru katalogu może trochę potrwać w przypadku kont użytkowników i poświadczeń mieszania mają być dostępne w usługach domenowych AD Azure. Upewnij się, że Poczekaj wystarczająco długo przed Ponawianie uwierzytelniania (w zależności od rozmiaru katalogu — kilka godzin na dzień lub dwa dla dużych katalogów).

    - Jeśli problem będzie nadal występował po sprawdzeniu powyższych kroków, spróbuj zrestartować usługi Microsoft Azure AD Sync. Z komputera synchronizacji Uruchom wiersz polecenia i wykonać następujące polecenia:
      1. Zatrzymaj netto "Microsoft Azure AD Sync"
      2. Rozpocznij netto "Microsoft Azure AD Sync"

- **Konta tylko do chmury**: Jeśli konto użytkownika to konto użytkownika tylko do chmury, upewnij się, użytkownik zmienił swoje hasło po włączeniu usług domenowych AD Azure. Ten krok sprawia, że poświadczenia mieszania wymagane dla usług Azure AD domeny do wygenerowania.


## <a name="users-removed-from-your-azure-ad-tenant-are-not-removed-from-your-managed-domain"></a>Użytkownicy usuwane z dzierżawy usługi Azure AD nie zostaną usunięte z domeny zarządzanych
Azure AD uniemożliwia przypadkowemu usunięciu obiektów użytkowników. Po usunięciu konta użytkownika z dzierżawcy usługi Azure AD obiekt użytkownika odpowiadające im jest przenoszony do Kosza. Operacja usuwania jest synchronizowane z domeną zarządzane, powoduje odpowiednie konto użytkownika, które mają być oznaczane wyłączony. Ta funkcja ułatwia odzyskiwanie lub później cofanie usunięcia konta użytkownika.

Aby usunąć konto użytkownika w pełni z zarządzanych domeny, należy trwale usunąć użytkownika z dzierżawy usługi Azure AD. Należy użyć polecenia cmdlet programu PowerShell Usuń-MsolUser z "-RemoveFromRecycleBin" opcja, zgodnie z opisem w tym [artykule w witrynie MSDN](https://msdn.microsoft.com/library/azure/dn194132.aspx).


## <a name="contact-us"></a>Kontakt z nami
Skontaktuj się z zespołem produktu Azure Active Directory Domain Services do [wyrażanie opinii lub obsługę] (aktywne — katalogu — ds — kontakt us.md).
