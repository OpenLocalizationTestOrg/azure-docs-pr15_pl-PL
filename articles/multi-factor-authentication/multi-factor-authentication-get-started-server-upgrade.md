<properties 
    pageTitle="Uaktualnianie agenta PhoneFactor do Server Azure uwierzytelnianie wieloskładnikowe"
    description="W tym dokumencie opisano, jak rozpocząć pracę z serwerem MFA Azure i jak uaktualnienie starszych agenta phonefactor."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="upgrading-the-phonefactor-agent-to-azure-multi-factor-authentication-server"></a>Uaktualnianie agenta PhoneFactor do Server Azure uwierzytelnianie wieloskładnikowe

Uaktualnianie z v5.x agenta PhoneFactor lub starsza wersja na serwerze uwierzytelnianie wieloskładnikowe Azure wymaga agenta PhoneFactor i powiązanych składników odinstalowanie przed serwerem uwierzytelnianie wieloskładnikowe i ich powiązanych elementów, można zainstalować.

## <a name="to-upgrade-the-phonefactor-agent-to-azure-multi-factor-authentication-server"></a>Aby uaktualnić agenta PhoneFactor do serwera uwierzytelniania wieloskładnikowego Azure
<ol>
<li>Najpierw utwórz kopię zapasową pliku danych PhoneFactor. Domyślna lokalizacja instalacji jest C:\Program Files\PhoneFactor\Data\Phonefactor.pfdata.


<li>Jeśli zainstalowano portalu użytkownika:</li>
<ol>
<li>Przejdź do folderu instalacyjnego i Utwórz kopię zapasową pliku web.config. Domyślna lokalizacja instalacji jest C:\inetpub\wwwroot\PhoneFactor.</li>


<li>Motywy niestandardowe dodano do portalu, Utwórz kopię zapasową niestandardowego folderu poniżej katalogu C:\inetpub\wwwroot\PhoneFactor\App_Themes.</li>


<li>Odinstaluj portalu użytkownika za pośrednictwem agenta PhoneFactor (dostępne tylko jeśli zainstalowana na tym samym serwerze agenta PhoneFactor) lub Windows programy i funkcje.</li></ol>




<li>Jeśli zainstalowano usługę Mobile aplikacji sieci Web:
<ol>
<li>Przejdź do folderu instalacyjnego i Utwórz kopię zapasową pliku web.config. Domyślna lokalizacja instalacji jest C:\inetpub\wwwroot\PhoneFactorPhoneAppWebService.</li>
<li>Odinstalować usługę urządzeń przenośnych aplikacji sieci Web przez okna programy i funkcje.</li></ol>

<li>Jeśli zainstalowano SDK usługi sieci Web, należy go odinstalować za pośrednictwem agenta PhoneFactor lub Windows programy i funkcje.

<li>Odinstalowywanie agenta PhoneFactor przez okna programy i funkcje.

<li>Zainstaluj serwer uwierzytelnianie wieloskładnikowe. Należy zauważyć, że ścieżka instalacji zostanie pobrana z rejestru z poprzedniej instalacji agenta PhoneFactor więc powinni zainstalować w tym samym miejscu (przykład C:\Program Files\PhoneFactor). Nowe instalacje ma innego domyślnego zainstalować path (np. C:\Program Files\Multi czynniki uwierzytelniania serwera). Plik danych po poprzedniej agenta PhoneFactor powinny być uaktualnione podczas instalacji, więc użytkowników i ustawienia nadal powinny być dostępne po zainstalowaniu nowego serwera uwierzytelnianie wieloskładnikowe.

<li>Jeśli zostanie wyświetlony monit, aktywacji serwera uwierzytelnianie wieloskładnikowe i upewnij się, że jest przypisany do grupy replikacji poprawne.

<li>Jeśli wcześniej została zainstalowana SDK usługi sieci Web, zainstaluj nowe SDK usługi sieci Web za pomocą interfejsu użytkownika serwera uwierzytelnianie wieloskładnikowe. Należy zauważyć, że domyślna nazwa katalogu wirtualnego jest teraz "MultiFactorAuthWebServiceSdk" zamiast "PhoneFactorWebServiceSdk". Jeśli chcesz użyć poprzedniej nazwy, należy zmienić nazwę katalogu wirtualnego podczas instalacji. W przeciwnym razie jeśli zezwolisz na Zainstaluj, aby używać nowej domyślnej nazwy, musisz zmienić adres URL w wszystkie aplikacje, które odwołują się do SDK usługi sieci Web, takich jak portalu użytkownika i usługi sieci Web aplikacji mobilnej, aby wskazywały w poprawnej lokalizacji.

<li>Jeśli portalu użytkownika wcześniej została zainstalowana na serwerze agenta PhoneFactor, zainstaluj nowego użytkownika uwierzytelnianie wieloskładnikowe portalu za pośrednictwem interfejsu użytkownika serwera uwierzytelnianie wieloskładnikowe. Należy zauważyć, że domyślna nazwa katalogu wirtualnego jest teraz "MultiFactorAuth" zamiast "PhoneFactor". Jeśli chcesz użyć poprzedniej nazwy, należy zmienić nazwę katalogu wirtualnego podczas instalacji. W przeciwnym razie jeśli zezwolisz na Zainstaluj, aby używać nowej domyślnej nazwy, należy kliknij ikonę portalu użytkownika na serwerze uwierzytelnianie wieloskładnikowe i zaktualizować adres URL portalu użytkownika na karcie Ustawienia.

<li>Jeśli portalu użytkownika i/lub usługi sieci Web aplikacji mobilnej został wcześniej zainstalowany na innym serwerze agenta PhoneFactor:
<ol>
<li>Przejdź do lokalizacji instalacji (np. C:\Program Files\PhoneFactor) i skopiuj odpowiedni installer(s) do innego serwera. Istnieje 32-bitowej i 64-bitowych programów instalacyjnych portalu użytkownika i usługi sieci Web aplikacji mobilnej. Odpowiednio są nazywane MultiFactorAuthenticationUserPortalSetupXX.msi i MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi.</li>
<li>Aby zainstalować portalu użytkownika na serwerze sieci web, Otwórz okno wiersza polecenia jako administrator i uruchomić MultiFactorAuthenticationUserPortalSetupXX.msi. Należy zauważyć, że domyślna nazwa katalogu wirtualnego jest teraz "MultiFactorAuth" zamiast "PhoneFactor". Jeśli chcesz użyć poprzedniej nazwy, należy zmienić nazwę katalogu wirtualnego podczas instalacji. W przeciwnym razie jeśli zezwolisz na Zainstaluj, aby używać nowej domyślnej nazwy, należy kliknij ikonę portalu użytkownika na serwerze uwierzytelnianie wieloskładnikowe i zaktualizować adres URL portalu użytkownika na karcie Ustawienia. Istniejący użytkownicy będą musieli informowany o nowy adres URL.</li>
<li>Przejdź do portalu użytkownika lokalizacji (na przykład C:\inetpub\wwwroot\MultiFactorAuth) instalacji i edytowania pliku web.config. Skopiuj wartości w sekcji appSettings i applicationSettings z pliku web.config oryginalnego wykonano kopii zapasowej przed uaktualnieniem do nowego pliku web.config. Jeśli nowy domyślna nazwa katalogu wirtualnego znajdowały się podczas instalowania SDK usługi sieci Web, należy zmienić adres URL w sekcji applicationSettings, aby wskazywały do odpowiedniej lokalizacji. Jeśli zmieniono innych ustawień domyślnych w poprzednim pliku web.config, stosowanie tych zmian do nowego pliku web.config.</li>
<li>Aby zainstalować usługę Mobile aplikacji sieci Web na serwerze sieci web, Otwórz okno wiersza polecenia jako administrator, a następnie uruchom MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi. Należy zauważyć, że domyślna nazwa katalogu wirtualnego jest teraz "MultiFactorAuthMobileAppWebService" zamiast "PhoneFactorPhoneAppWebService". Jeśli chcesz użyć poprzedniej nazwy, należy zmienić nazwę katalogu wirtualnego podczas instalacji. Można wybrać krótszej nazwy, aby ułatwić użytkownikom końcowym w wpisz na swoich urządzeniach przenośnych. W przeciwnym razie jeśli są dozwolone Zainstaluj, aby używać nowej domyślnej nazwy, należy kliknij ikonę aplikacji Mobile na serwerze uwierzytelnianie wieloskładnikowe i aktualizować URL usługi sieci Web w aplikacji Mobile.</li>
<li>Przejdź do usługi sieci Web w aplikacji Mobile instalowanie lokalizacji (na przykład C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService) i edytowania pliku web.config. Skopiuj wartości w sekcji appSettings i applicationSettings z pliku web.config oryginalnego wykonano kopii zapasowej przed uaktualnieniem do nowego pliku web.config. Jeśli nowy domyślna nazwa katalogu wirtualnego znajdowały się podczas instalowania SDK usługi sieci Web, należy zmienić adres URL w sekcji applicationSettings, aby wskazywały do odpowiedniej lokalizacji. Jeśli zmieniono innych ustawień domyślnych w poprzednim pliku web.config, stosowanie tych zmian do nowego pliku web.config.</li></ol>
