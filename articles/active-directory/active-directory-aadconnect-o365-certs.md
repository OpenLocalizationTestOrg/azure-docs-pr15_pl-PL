<properties
    pageTitle="Certyfikat odnawiania wskazówki dla użytkowników usługi Office 365 i usługi Azure Active Directory | Microsoft Azure"
    description="W tym artykule wyjaśniono użytkowników usługi Office 365, jak rozwiązywać problemy z wiadomości e-mail, które je powiadomić o odnawianiu certyfikatu."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>


# <a name="renew-federation-certificates-for-office-365-and-azure-active-directory"></a>Odnawianie certyfikatów Federacji dla usługi Office 365 i usługi Azure Active Directory

##<a name="overview"></a>Omówienie

Do pomyślnego federacji między usługą Azure Active Directory (Azure AD) i Active Directory Federation Services (AD FS) certyfikaty używany przez usług AD FS do podpisywania tokenów zabezpieczających do Azure AD powinny być takie same, co jest skonfigurowane w Azure AD. Wszelkie niezgodności może prowadzić do niepoprawnych zaufania. Azure AD gwarantuje, że informacje te są przechowywane w synchronizacji podczas wdrażania usług AD FS i serwer Proxy aplikacji sieci Web (Aby uzyskać dostęp do ekstranetu).

Ten artykuł zawiera dodatkowe informacje w celu zarządzania token certyfikaty podpisywania i synchronizowanie ich z Azure AD w następujących przypadkach:

* Nie są wdrażanie serwera Proxy aplikacji sieci Web i dlatego nie jest dostępna w ekstranetu metadanych federacji.
* Certyfikaty podpisywania tokenu nie używają domyślnej konfiguracji usług AD FS.
* Używasz dostawcy tożsamości innych firm.

## <a name="default-configuration-of-ad-fs-for-token-signing-certificates"></a>Domyślna konfiguracja usług AD FS certyfikaty podpisywania tokenu

Podpisywanie tokenów i token odszyfrowywanie certyfikaty są zwykle podpisem certyfikaty i dobry jeden rok. Domyślnie usług AD FS zawiera proces automatycznego odnawiania o nazwie **AutoCertificateRollover**. Jeśli korzystasz z usług AD FS 2.0 lub nowszy, usługi Office 365 i Azure AD automatycznie aktualizowane certyfikatu przed jej wygaśnięciem.

### <a name="renewal-notification-from-the-office-365-portal-or-an-email"></a>Powiadomienie o odnawianiu z portalu usługi Office 365 lub wiadomości e-mail

>[AZURE.NOTE] Jeśli otrzymasz wiadomość e-mail lub portalu powiadomienie prośbą o odnowieniu certyfikatu dla pakietu Office, zobacz [Zarządzanie zmieni się na certyfikaty podpisywania tokenu](#managecerts) , aby sprawdzić, czy jest potrzebny podejmować żadnych działań. Firma Microsoft jest znane wystąpić problem, który może prowadzić do powiadomienia o odnowienie certyfikatu przesyłany, nawet wtedy, gdy jest wymagane żadne działanie.

Azure AD próbuje monitorować metadanych Federacji i aktualizować token certyfikaty podpisywania wskazywane przez metadanych. 30 dni przed wygaśnięciem certyfikaty, podpisywania tokenu Azure AD sprawdza, czy nowe certyfikaty są dostępne, sondowanie metadanych federacji.

* Jeśli go pomyślnie ankieta metadanych Federacji i pobierać nowe certyfikaty, bez powiadomienia pocztą e-mail i ostrzeżenia w portalu usługi Office 365 jest wydawany dla użytkownika.
* Jeśli nie można go pobrać nowy token certyfikaty podpisywania, albo ponieważ metadanych federacyjnych nie jest dostępny lub przerzucania certyfikat automatyczne nie jest włączona, Azure AD kwestie wiadomości e-mail z powiadomieniem i ostrzeżenie w portalu usługi Office 365.

![Powiadomienie o portalu usługi Office 365](./media/active-directory-aadconnect-o365-certs/notification.png)

>[AZURE.IMPORTANT] Jeśli korzystasz z usług AD FS zapewnienie ciągłości, sprawdź, czy że serwery mają następujące aktualizacje, aby nie występują błędy uwierzytelniania dla znanych problemów. Zmniejsza to zagrożenie znane problemy serwera proxy usług AD FS dla tego odnawiania i okresy przyszłych odnawiania:
>
>Server 2012 R2 — [Windows Server zestawienie maja 2014](http://support.microsoft.com/kb/2955164)
>
>Server 2008 R2 i 2012 - [Uwierzytelnianie za pośrednictwem serwera proxy kończy się niepowodzeniem w systemie Windows 2008 R2 z dodatkiem SP1 lub Windows Server 2012](http://support.microsoft.com/kb/3094446)

## Sprawdzanie, jeśli certyfikaty muszą zostać zaktualizowane<a name="managecerts"></a>

### <a name="step-1-check-the-autocertificaterollover-state"></a>Krok 1: Sprawdź stan AutoCertificateRollover

Na serwerze usług AD FS Otwórz programu PowerShell. Sprawdź, czy wartość AutoCertificateRollover jest ustawiona na PRAWDA.

    Get-Adfsproperties

![AutoCertificateRollover](./media/active-directory-aadconnect-o365-certs/autocertrollover.png)

[AZURE.NOTE] Jeśli korzystasz z usług AD FS 2.0, uruchom Microsoft.Adfs.Powershell Pssnapin Dodaj.

### <a name="step-2-confirm-that-ad-fs-and-azure-ad-are-in-sync"></a>Krok 2: Upewnij się, że usług AD FS i Azure AD są zsynchronizowane

Na serwerze usług AD FS Otwórz wiersz Azure AD programu PowerShell i nawiązać Azure AD.

>[AZURE.NOTE] Możesz pobrać Azure AD programu PowerShell [tutaj](https://technet.microsoft.com/library/jj151815.aspx).

    Connect-MsolService

Sprawdź certyfikaty skonfigurowane w AD FS i Azure AD zaufania właściwości dla określonej domeny.

    Get-MsolFederationProperty -DomainName <domain.name> | FL Source, TokenSigningCertificate

![Get-MsolFederationProperty](./media/active-directory-aadconnect-o365-certs/certsync.png)

Zgodne z odciski palców w obu wyjść, certyfikaty użytkownika nie są synchronizowane z usługą Azure Active Directory.

### <a name="step-3-check-if-your-certificate-is-about-to-expire"></a>Krok 3: Sprawdź, czy certyfikat jest zbliża się termin wygaśnięcia

W wyniku kwerendy Get-MsolFederationProperty lub Get-AdfsCertificate Sprawdź datę w obszarze "Nie po." Jeśli data jest mniejsza niż 30 dni z dala od komputera, należy podjąć kroki.

| AutoCertificateRollover | Certyfikaty synchronizacji z usługą Azure Active Directory | Federacja metadanych są publicznie dostępne | Ważność | Akcja |
|:-----------------------:|:-----------------------:|:-----------------------:|:-----------------------:|:-----------------------:|
| Tak | Tak | Tak | - | Nie trzeba wykonywać żadnych czynności. Zobacz [token odnawiania certyfikatu podpisywania automatycznie](#autorenew). |
| Tak | Brak  | - | Mniej niż 15 dni | Odnowić natychmiast. Zobacz [token odnawiania certyfikatu podpisywania ręcznie](#manualrenew). |
| Brak | - | - | Mniej niż 30 dni | Odnowić natychmiast. Zobacz [token odnawiania certyfikatu podpisywania ręcznie](#manualrenew). |

\[Nie ma znaczenia -]

## Odnów token certyfikatu podpisywania automatycznie (zalecane)<a name="autorenew"></a>

Nie musisz wykonywać jakichkolwiek czynności ręcznie, jeśli są spełnione oba z następujących czynności:
- Możesz wdrożyć serwer Proxy aplikacji sieci Web, które umożliwiają dostęp do metadanych federacji z ekstranetu.
- W przypadku korzystania z usług AD FS konfiguracji domyślnej (AutoCertificateRollover jest włączona).

Sprawdź poniższe czynności, aby potwierdzić, że certyfikat może być aktualizowane automatycznie.

**1 właściwości usług AD FS AutoCertificateRollover musi być ustawiona na PRAWDA.** Oznacza to, że usług AD FS automatycznie generuje token odszyfrowywanie certyfikaty i nowe podpisywania tokenu, przed starą etapach wygaśnie.

**2. metadane Federacja usług AD FS są publicznie dostępne.** Sprawdź, czy metadane federacji jest dostępny publicznie, przechodząc do następujący adres URL z komputera publicznego Internetu (poza sieci firmowej):


/federationmetadata/2007-06/federationmetadata.xml https:// (your_FS_name)

miejsce, w którym `(your_FS_name) `jest zastępowany nazwa hosta usługi federacyjne organizacji jest używany, takich jak fs.contoso.com.  Jeśli możesz sprawdzić oba te ustawienia pomyślnie, nie musisz nic robić.  

Przykład: https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml

## Odnów token certyfikatu podpisywania ręcznie<a name="manualrenew"></a>

Można odnowić token certyfikaty podpisywania ręcznie. Na przykład następujące scenariusze może działać lepiej ręcznego odnawiania:
* Certyfikaty podpisywania tokenu to certyfikaty nie podpisem. Najczęstsze przyczyny to to, że Twoja organizacja zarządza certyfikatów usług AD FS zarejestrowanych od urzędu certyfikacji organizacji.
* Zabezpieczenia sieci nie zezwala na metadanych Federacji mają być publicznie dostępne.

W poniższych scenariuszach każdorazowo po zaktualizowaniu certyfikaty, podpisywania tokenu należy również zaktualizować domeny usługi Office 365 za pomocą polecenia programu PowerShell MsolFederatedDomain aktualizacji.

### <a name="step-1-ensure-that-ad-fs-has-new-token-signing-certificates"></a>Krok 1: Upewnij się, że usług AD FS ma nowy token certyfikaty podpisywania

**Konfiguracja inne niż domyślne**

Jeśli korzystasz z innej niż domyślna konfiguracja usług AD FS (gdzie **AutoCertificateRollover** jest ustawiona na **False**), prawdopodobnie używasz niestandardowych certyfikaty (nie podpisany). Aby uzyskać więcej informacji na temat odnawiania certyfikaty podpisywania tokenu usług AD FS zobacz [wskazówki dotyczące klientów nie korzystasz z usług AD FS podpisany certyfikaty](https://msdn.microsoft.com/library/azure/JJ933264.aspx#BKMK_NotADFSCert).

**Federacja metadanych nie jest publicznie dostępna**

Z drugiej strony Jeśli **AutoCertificateRollover** jest ustawiona na **wartość PRAWDA**, ale metadanych federacji nie są publicznie dostępne, najpierw upewnij się, że nowe certyfikaty podpisywania tokenu wygenerowane przez usług AD FS. Upewnij się, że masz nowy token certyfikaty podpisywania, wykonując następujące czynności:

1. Upewnij się, że użytkownik jest zalogowany na serwerze usług AD FS podstawowego.
2. Zaznacz bieżącą certyfikaty podpisywania usług AD FS, otwierając oknie polecenia programu PowerShell i uruchamiając następujące polecenie:

    PS C:\>podpisywania tokenu Get-ADFSCertificate — CertificateType

    >[AZURE.NOTE] Jeśli korzystasz z usług AD FS 2.0, należy najpierw uruchomić Microsoft.Adfs.Powershell Pssnapin Dodaj.

3. Przyjrzyj się wynik polecenia wszystkie certyfikaty na liście. Jeśli usług AD FS wygenerował nowego certyfikatu, zobacz dwa certyfikaty w wyniku kwerendy: jedną dla którego wartość **IsPrimary** jest **PRAWDA** i **nie później niż** Data jest w ciągu 5 dni, a drugi dla których **IsPrimary** ma **wartość FAŁSZ** , a **nie później niż** o roku w przyszłości.

4. Jeśli zostanie wyświetlony tylko jeden certyfikat, a **nie później niż** Data jest w ciągu 5 dni, musisz Generowanie nowego certyfikatu.

5. Aby wygenerować nowego certyfikatu, wykonaj następujące polecenie w wierszu polecenia programu PowerShell: `PS C:\>Update-ADFSCertificate –CertificateType token-signing`.

6. Sprawdź aktualizację, uruchamiając następujące polecenie: PS C:\>podpisywania tokenu Get-ADFSCertificate — CertificateType

Dwa certyfikaty powinna teraz wyświetlane, z których jedna zawiera datę **nie później niż** w przybliżeniu jeden rok i którego wartość **IsPrimary** jest **fałszywe**.

### <a name="step-2-update-the-new-token-signing-certificates-for-the-office-365-trust"></a>Krok 2: Aktualizowanie nowy token podpisywania certyfikatów zaufania usługi Office 365

Aktualizowanie usługi Office 365 przy użyciu nowego tokenu podpisywania certyfikatów może być używany dla zaufania w następujący sposób.

1.  Otwórz moduł usługi Microsoft Azure Active Directory dla programu Windows PowerShell.
2.  Uruchamianie $cred = Get-poświadczeń. Gdy to polecenie cmdlet monituje o podanie poświadczeń, wpisz swoje poświadczenia konta administratora usługi cloud.
3.  Uruchamianie MsolService łączenie — poświadczeń $cred. To polecenie cmdlet łączy do usługi w chmurze. Tworzenie kontekstu, który łączy do usługi w chmurze jest wymagane przed uruchomieniem tych dodatkowych poleceń cmdlet zainstalowany za pomocą narzędzia.
4.  Jeśli te polecenia są uruchomione na komputerze, na którym jest serwer federacyjny podstawowego usług AD FS, uruchom zestaw MSOLAdfscontext — komputer <AD FS primary server>, gdzie <AD FS primary server> jest nazwą FQDN wewnętrznych podstawowy serwer usług AD FS. To polecenie cmdlet tworzy kontekście łączącym się usług AD FS.
5.  Uruchamianie MSOLFederatedDomain aktualizacji — nazwa_domeny <domain>. To polecenie cmdlet aktualizuje ustawienia z usług AD FS do usługi w chmurze i konfiguruje relacja zaufania między nimi.


>[AZURE.NOTE] Jeśli potrzebujesz pomocy technicznej wiele domen najwyższego poziomu, na przykład contoso.com i fabrikam.com, należy użyć przełącznika **SupportMultipleDomain** z dowolnego polecenia cmdlet. Aby uzyskać więcej informacji zobacz [Obsługa wiele domen poziom góry](active-directory-aadconnect-multiple-domains.md).

## Naprawianie zaufania Azure AD przy użyciu Azure AD Connect<a name="connectrenew"></a>

Jeśli skonfigurowano do farmy programu AD FS i zaufania Azure AD przy użyciu Azure AD Connect służy narzędzie Azure AD Connect wykrywanie, jeśli musisz podejmować żadnych działań dla token certyfikaty podpisywania. Jeśli chcesz odnowić certyfikaty służy narzędzie Azure AD Connect to zrobić.

Aby uzyskać więcej informacji zobacz [Naprawianie zaufania](./active-directory-aadconnect-federation-management.md#repairing-the-trust).
