<properties
    pageTitle="Azure AD Connect wiele domen"
    description="W tym dokumencie opisano instalowaniu i konfigurowaniu usługi Office 365 i Azure AD wiele domen najwyższego poziomu."
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

# <a name="multiple-domain-support-for-federating-with-azure-ad"></a>Obsługa wielu domeny dla federacyjnego z usługą Azure Active Directory
Poniższą dokumentację zawiera wskazówki dotyczące sposobu używania wielu domen najwyższego poziomu i poddomenach po federacyjnego z usługi Office 365 lub Azure AD domen.

## <a name="multiple-top-level-domain-support"></a>Obsługa wielu domen najwyższego poziomu
Używanie federacyjnych wielu, domen najwyższego poziomu z usługą Azure Active Directory wymaga kilka dodatkowych konfiguracji, która nie jest wymagane w przypadku federacyjnego z domenami najwyższego poziomu.

Jeśli Federacji domeny z usługą Azure Active Directory, niektóre właściwości są ustawione w domenie platformy Azure.  Jedną ważne jest IssuerUri.  To jest identyfikator URI, który jest używany przez Azure AD do identyfikowania domeny, którą jest skojarzony token.  Identyfikator URI nie trzeba rozpoznawać cokolwiek, ale musi być prawidłowy identyfikator URI.  Domyślnie Azure AD ustawia to wartość identyfikatora usługi federacyjne w sieci lokalnej usług AD FS konfiguracji.

>[AZURE.NOTE]Identyfikator usług federacyjnych jest identyfikatora URI, który identyfikuje usługi federacyjnej.  Usługi federacyjne to wystąpienie usług AD FS stosowanego jako usługa tokenu zabezpieczającego. 

Można wyświetlić IssuerUri za pomocą polecenia programu PowerShell `Get-MsolDomainFederationSettings - DomainName <your domain>`.

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

Problem pojawia się, gdy chcemy dodać więcej niż jedna domena najwyższego poziomu.  Na przykład załóżmy, że masz Federacji konfiguracji między Azure AD i środowiska lokalnego.  Dla tego dokumentu korzystam z bmcontoso.com.  Po dodaniu domeny najwyższego poziomu, druga, bmfabrikam.com.

![Domeny](./media/active-directory-multiple-domains/domains.png)

Gdy firma Microsoft próbują konwertowanie naszej domenie bmfabrikam.com Federacja jest, możemy komunikat o błędzie.  Przyczyną jest Azure AD ma ograniczenie, które nie zezwala na właściwość IssuerUri mają taką samą wartość więcej niż jedną domenę.  
  

![Federacja błędu](./media/active-directory-multiple-domains/error.png)

### <a name="supportmultipledomain-parameter"></a>Parametr SupportMultipleDomain

W celu obejścia tego trzeba dodać różnych IssuerUri, które można wykonać przy użyciu `-SupportMultipleDomain` parametru.  Ten parametr jest używany z następujące polecenia cmdlet:
    
- `New-MsolFederatedDomain`
- `Convert-MsolDomaintoFederated`
- `Update-MsolFederatedDomain`

Ten parametr powoduje, że Azure AD IssuerUri tak skonfigurować program jest oparty na nazwę domeny.  Są to unikatowe w katalogach w Azure AD.  Przy użyciu parametru umożliwia polecenia programu PowerShell pomyślnie.

![Federacja błędu](./media/active-directory-multiple-domains/convert.png)

Przeglądająca ustawień naszych nową domenę bmfabrikam.com zobacz następujące artykuły:

![Federacja błędu](./media/active-directory-multiple-domains/settings.png)

Należy zauważyć, że `-SupportMultipleDomain` nie zmienia się końcowych, które nadal są skonfigurowane do wskaż naszej usługi federacyjne na adfs.bmcontoso.com.

Niekiedy który `-SupportMultipleDomain` czy jest on gwarantuje, że system usług AD FS obejmuje poprawną wartość wystawcy w tokeny wydawane dla Azure AD. Jest to możliwe dzięki sporządzanie fragment domeny użytkowników głównej nazwy użytkownika i konfigurowania tej domeny w IssuerUri, to znaczy https://{upn sufiks} / adfs i usług i zaufania. 

Dlatego podczas uwierzytelniania Azure AD lub usługi Office 365, element IssuerUri tokenu użytkownika jest używane do znajdowania domeny w Azure AD.  Jeśli nie można znaleźć dopasowanie, że uwierzytelnianie zakończy się niepowodzeniem. 

Na przykład jeśli UPN użytkownika jest bsimon@bmcontoso.com, element IssuerUri problemów token AD FS jest równa http://bmcontoso.com/adfs/services/trust. To będzie pasować do konfiguracji Azure AD i powiedzie uwierzytelniania.

Reguła dostosowane roszczeń wykonuje tę logikę jest następująca:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type =   "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));


>[AZURE.IMPORTANT]Aby podczas próby Dodaj nowe lub konwertowanie już dodanych domen, należy użyć przełącznika - SupportMultipleDomain, musisz skonfigurowaniu usługi federacyjne zaufania do ich pierwotnie obsługi.  


## <a name="how-to-update-the-trust-between-ad-fs-and-azure-ad"></a>Jak zaktualizować zaufanie między usług AD FS i Azure AD
Jeśli nie konfiguracji federacyjnego zaufanie między usług AD FS i wystąpienia programu Azure AD, może być konieczne ponownie utworzyć ten zaufania.  To jest, ponieważ, gdy jest pierwotnie instalację bez `-SupportMultipleDomain` , IssuerUri jest ustawiona z wartością domyślną.  Ekranu przedstawiono poniżej widać, że IssuerUri jest ustawiona na https://adfs.bmcontoso.com/adfs/services/trust.

Teraz tak, jeśli firma Microsoft w portal Azure AD pomyślnie dodano nową domenę, a następnie spróbuj przekonwertować ją przy użyciu `Convert-MsolDomaintoFederated -DomainName <your domain>`, uzyskując następujący komunikat o błędzie.

![Federacja błędu](./media/active-directory-multiple-domains/trust1.png)

Jeśli próbujesz dodać `-SupportMultipleDomain` Przełącz możemy zostanie komunikat o błędzie:

![Federacja błędu](./media/active-directory-multiple-domains/trust2.png)

Po prostu próby uruchomienia `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` w oryginalnej domenie będą również powodują błąd.

![Federacja błędu](./media/active-directory-multiple-domains/trust3.png)

Wykonaj poniższe czynności, aby dodać dodatkowe domen najwyższego poziomu.  Jeśli dodano już domenę, a nie użyto `-SupportMultipleDomain` parametru zaczyna się od instrukcje dotyczące usuwania i aktualizowanie oryginalnej domeny.  Jeśli nie dodano domeny najwyższego poziomu jeszcze możesz rozpocząć z instrukcje dotyczące dodawania domeny za pomocą programu PowerShell Azure AD Connect.

Usuwanie zaufania Microsoft Online i zaktualizować oryginalny domeny, wykonaj następujące czynności.

2.  Na serwerze Federacja usług AD FS Otwórz **AD FS zarządzania.** 
2.  Po lewej stronie rozwiń węzeł **Relacje zaufania** i **Polegaj strona zaufanie**
3.  Po prawej stronie Usuń tę pozycję **Microsoft Office 365 tożsamości platformy** .
![Usuwanie Online firmy Microsoft](./media/active-directory-multiple-domains/trust4.png)
1.  Uruchom następujące polecenie na komputerze z zainstalowanym [Azure Active Directory modułu dla programu Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) : `$cred=Get-Credential`.  
2.  Wprowadź nazwę użytkownika i hasło administratora globalnego dla domeny Azure AD, które są federacyjnego z
2.  W programie PowerShell wprowadź`Connect-MsolService -Credential $cred`
4.  W programie PowerShell wprowadź `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`.  Jest to pierwotnej domeny.  Za pomocą powyższych domen, który będzie:`Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`


Wykonaj następujące czynności, aby dodać nową domenę najwyższego poziomu przy użyciu programu PowerShell

1.  Uruchom następujące polecenie na komputerze z zainstalowanym [Azure Active Directory modułu dla programu Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) : `$cred=Get-Credential`.  
2.  Wprowadź nazwę użytkownika i hasło administratora globalnego dla domeny Azure AD, które są federacyjnego z
2.  W programie PowerShell wprowadź`Connect-MsolService -Credential $cred`
3.  W programie PowerShell wprowadź`New-MsolFederatedDomain –SupportMultipleDomain –DomainName`

Wykonaj następujące czynności, aby dodać nową domenę najwyższego poziomu za pomocą narzędzie Azure AD Connect.

1.  Uruchom narzędzie Azure AD Connect z pulpitu lub menu start
2.  Wybierz pozycję "Dodaj dodatkową domenę Azure AD" ![dodać dodatkowe domeny Azure AD](./media/active-directory-multiple-domains/add1.png)
3.  Wprowadź usługi Azure AD i poświadczenia usługi Active Directory
4.  Zaznacz drugą domenę, którą chcesz skonfigurować do Federacji.
![Dodawanie dodatkowego domeny Azure AD](./media/active-directory-multiple-domains/add2.png)
5.  Kliknij pozycję Zainstaluj


### <a name="verify-the-new-top-level-domain"></a>Sprawdź nowej domeny najwyższego poziomu
Za pomocą polecenia programu PowerShell `Get-MsolDomainFederationSettings - DomainName <your domain>`możesz wyświetlić zaktualizowana IssuerUri.  Zrzut ekranu poniżej przedstawiono Federacji ustawienia zostały zaktualizowane w naszym oryginalny http://bmcontoso.com/adfs/services/trust domeny

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

I IssuerUri w naszym nowa domena została ustawiona jako https://bmfabrikam.com/adfs/services/trust

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/settings2.png)


##<a name="support-for-sub-domains"></a>Obsługa poddomenach
Po dodaniu domeny podrzędne, ze względu na sposób Azure AD obsługiwane domeny będzie ją dziedziczą ustawienia nadrzędnej.  Oznacza to, że IssuerUri musi odpowiadać elementów nadrzędnych.

Dlatego umożliwia Powiedz, na przykład, że można mieć bmcontoso.com, a następnie dodaj corp.bmcontoso.com.  Oznacza to, że IssuerUri dla użytkownika z corp.bmcontoso.com muszą być **http://bmcontoso.com/adfs/services/trust.**  Jednak standardowy regułę dla Azure AD zrealizowane powyżej wygeneruje token z wystawcy jako **http://corp.bmcontoso.com/adfs/services/trust.** nie będą zgodne domeny wymagane wartości i uwierzytelniania zakończy się niepowodzeniem.

### <a name="how-to-enable-support-for-sub-domains"></a>Jak włączyć obsługę poddomenach
W celu obejścia tego problemu usług AD FS uzależnionej zaufania firmy Microsoft Online musi zostać zaktualizowane.  Aby to zrobić, musisz skonfigurować reguły niestandardowej roszczeń tak, aby podczas tworzenia niestandardowych wartość wystawcy paski wyłączanie domen podrzędnych z użytkownika sufiksu głównej nazwy użytkownika. 

Następujące roszczeń będzie wykonaj następujące czynności:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^((.*)([.|@]))?(?<domain>[^.]*[.].*)$", "http://${domain}/adfs/services/trust/"));

Wykonaj następujące czynności, aby dodać niestandardowe roszczeń do obsługi poddomenach.

1.  Otwieranie AD FS zarządzania
2.  Kliknij prawym przyciskiem myszy zaufania RP Online firmy Microsoft i wybierz polecenie Edytuj roszczeń reguł
3.  Zaznacz regułę, trzecia roszczeń i zamienianie ![roszczeń edycji](./media/active-directory-multiple-domains/sub1.png)
4.  Zamień bieżący roszczeń:
    
        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)","http://${domain}/adfs/services/trust/"));
        
    z
    
        `c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^((.*)([.|@]))?(?<domain>[^.]*[.].*)$", "http://${domain}/adfs/services/trust/"));`
    
![Zamienianie roszczeń](./media/active-directory-multiple-domains/sub2.png)
5.  Kliknij przycisk Ok.  Kliknij przycisk Zastosuj.  Kliknij przycisk Ok.  Zamknij AD FS zarządzania.

