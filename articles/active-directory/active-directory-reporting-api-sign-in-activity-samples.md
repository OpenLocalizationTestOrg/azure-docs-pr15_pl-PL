<properties
    pageTitle="Azure Active Directory logowania aktywności raportu interfejsu API próbek | Microsoft Azure"
    description="Jak rozpocząć pracę przy użyciu Azure Active Directory raportowania interfejsu API"
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/25/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="azure-active-directory-sign-in-activity-report-api-samples"></a>Azure próbki raportu interfejsu API logowania działania usługi Active Directory

W tym temacie jest częścią zbiór tematów dotyczących usługi Azure Active Directory raportowania interfejsu API.  
Raportowanie Azure AD oferuje interfejs API, który umożliwia dostęp do danych aktywności logowania przy użyciu kodu lub pokrewnych narzędzi.  
Zakres tego tematu jest dostarczanie przykładowy kod **logowania działania interfejsu API**.

Zobacz:

- [Dzienniki inspekcji](active-directory-reporting-azure-portal.md#audit-logs) , aby uzyskać więcej informacji koncepcyjnych
- Aby uzyskać więcej informacji na temat raportowania interfejsu API, [wprowadzenie Azure Active Directory raportowania API](active-directory-reporting-api-getting-started.md) .

Pytania, problemów lub opinii skontaktuj się z [AAD raportowania pomocy](mailto:aadreportinghelp@microsoft.com).


## <a name="prerequisites"></a>Wymagania wstępne
Zanim będzie można używać przykłady w tym temacie, należy wykonać [wymagania wstępne dotyczące dostępu Azure AD raportowania interfejsu API](active-directory-reporting-api-prerequisites.md).  


##<a name="powershell-script"></a>Skrypt programu PowerShell

    # This script will require the Web Application and permissions setup in Azure Active Directory
    $ClientID       = "<clientId>"             # Should be a ~35 character string insert your info here
    $ClientSecret   = "<clientSecret>"         # Should be a ~44 character string insert your info here
    $loginURL       = "https://login.windows.net/"
    $tenantdomain   = "<tenantDomain>"
    $ daterange            # For example, contoso.onmicrosoft.com

    $7daysago = "{0:s}" -f (get-date).AddDays(-7) + "Z"
    # or, AddMinutes(-5)

    Write-Output $7daysago

    # Get an Oauth 2 access token based on client id, secret and tenant domain
    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}

    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    if ($oauth.access_token -ne $null) {
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

    $url = "https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&`$filter=signinDateTime ge $7daysago"
    
    $i=0
    
    Do{
        Write-Output "Fetching data using Uri: $url"
        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        Write-Output "Save the output to a file SigninActivities$i.json"
        Write-Output "---------------------------------------------"
        $myReport.Content | Out-File -FilePath SigninActivities$i.json -Force
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
        $i = $i+1
    } while($url -ne $null)

    } else {
    
        Write-Host "ERROR: No Access Token"
    }




## <a name="executing-the-script"></a>Wykonywanie skryptu
Po Zakończ edycję skrypt, uruchom go i sprawdź, czy oczekiwane dane z inspekcji dzienniki raportu, jest zwracany.

Skrypt zwraca wynik z raportu logowania w formacie JSON. Tworzy również `SigninActivities.json` plik z taki sam wynik. Możesz wypróbować zmieniając skrypt zwrócenie danych z innych raportów i komentarz się formatów wyjściowych, których nie potrzebujesz.



## <a name="next-steps"></a>Następne kroki

- Czy chcesz dostosować próbki w tym temacie? Zapoznaj się z [usługi Azure Active Directory logowania działania interfejsu API odwołania](active-directory-reporting-api-sign-in-activity-reference.md). 

- Jeśli chcesz, zobacz Omówienie wykonane za pomocą usługi Azure Active Directory raportowania interfejsu API, zobacz [Wprowadzenie do usługi Azure Active Directory raportowania interfejsu API](active-directory-reporting-api-getting-started.md).

- Jeśli chcesz dowiedzieć się więcej o raportowaniu usługi Azure Active Directory, zobacz [Azure Active Directory raportowania przewodnika](active-directory-reporting-guide.md).  