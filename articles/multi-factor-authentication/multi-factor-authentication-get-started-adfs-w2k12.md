<properties
    pageTitle="MFA Server z systemem Windows Server 2012 R2 AD FS | Microsoft Azure"
    description="W tym artykule opisano, jak rozpocząć pracę z uwierzytelnianie wieloskładnikowe Azure i usług AD FS w systemie Windows Server 2012 R2."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>


# <a name="secure-your-cloud-and-on-premises-resources-using-azure-multi-factor-authentication-server-with-ad-fs-in-windows-server-2012-r2"></a>Zabezpieczanie chmury i lokalnych zasobów serwera uwierzytelniania wieloskładnikowego Azure za pomocą usług AD FS w systemie Windows Server 2012 R2

Jeśli za pomocą usługi Active Directory Federation Services (AD FS) i chcesz bezpiecznego chmury lub zasobów lokalnych, można skonfigurować serwer uwierzytelniania wieloskładnikowego Azure do pracy z usług AD FS. Ta konfiguracja uaktywnia Weryfikacja dwuetapowa dla punktów końcowych wysokiej wartości.

W tym artykule omówimy serwer uwierzytelniania wieloskładnikowego Azure za pomocą usług AD FS w systemie Windows Server 2012 R2. Aby uzyskać więcej informacji przeczytaj temat [bezpiecznego chmury i lokalnych zasobów przy użyciu serwera uwierzytelnianie wieloskładnikowe Azure z usług AD FS 2.0](multi-factor-authentication-get-started-adfs-adfs2.md).

## <a name="secure-windows-server-2012-r2-ad-fs-with-azure-multi-factor-authentication-server"></a>Zabezpieczanie systemu Windows Server 2012 R2 AD FS z serwerem Azure uwierzytelnianie wieloskładnikowe

Po zainstalowaniu serwera uwierzytelniania wieloskładnikowego Azure, masz następujące możliwości:

- Instalowanie serwera uwierzytelniania wieloskładnikowego Azure lokalnie na tym samym serwerze usług AD FS
- Zainstaluj kartę uwierzytelnianie wieloskładnikowe Azure lokalnie na serwerze usług AD FS, a następnie zainstaluj serwer uwierzytelniania wieloskładnikowego na innym komputerze

Przed rozpoczęciem należy pamiętać o następujące informacje:

- Nie są wymagane do zainstalowania serwera uwierzytelniania wieloskładnikowego Azure na serwerze usług AD FS. Jednak zainstalowanie karty uwierzytelnianie wieloskładnikowe usług AD FS w systemie Windows Server 2012 R2, uruchomionym usług AD FS. Serwer można zainstalować na innym komputerze, jeśli jest obsługiwanej wersji i oddzielnie zainstalować kartę usług AD FS na serwerze Federacja usług AD FS. Zobacz poniższych procedur, aby dowiedzieć się, jak zainstalować karty oddzielnie.

- Gdy utworzonym karty MFA serwera AD FS, przewiduje została usług AD FS może przekazać nazwę strony ufającej do karty. Następnie uzależnionej nazwa firmy może służyć jako nazwa aplikacji. Jednak włączona nie należy wielkość liter. Jeśli organizacja korzysta z wiadomości tekstowej lub metody weryfikacji aplikacji dla urządzeń przenośnych, ciągi zdefiniowany w ustawieniach firmy zawierać symbol zastępczy, <$ $*nazwa_aplikacji*>. Ten symbol zastępczy nie jest automatycznie zastępowana, gdy karta usług AD FS. Firma Microsoft zaleca usuwanie symbolu zastępczego z ciągi właściwe, gdy bezpiecznego usług AD FS.

- Konto, którego używasz, aby zalogować się musi mieć uprawnienia do tworzenia grupy zabezpieczeń w usłudze Active Directory.

- Kreator instalacji karty uwierzytelnianie wieloskładnikowe AD FS tworzy grupę zabezpieczeń o nazwie Administratorzy PhoneFactor wystąpienia usługi Active Directory. Następnie dodaje konto usługi usług AD FS usługi federacyjne do tej grupy. Zaleca się sprawdzenie kontrolera domeny grupy administratorów PhoneFactor faktycznie zostanie utworzona, a konto usługi usług AD FS jest członkiem tej grupy. Jeśli to konieczne, ręcznie dodać konto usługi usług AD FS do grupy Administratorzy PhoneFactor kontrolera domeny.

- Aby uzyskać informacje o instalowaniu SDK usługi sieci Web za pomocą portalu użytkownika, przeczytaj o [Rozmieszczanie portalu użytkownika dla serwera uwierzytelniania wieloskładnikowego Azure.](multi-factor-authentication-get-started-portal.md)


### <a name="install-azure-multi-factor-authentication-server-locally-on-the-ad-fs-server"></a>Zainstaluj serwer uwierzytelniania wieloskładnikowego Azure lokalnie na serwerze usług AD FS

1. Pobierz i zainstaluj serwer uwierzytelniania wieloskładnikowego Azure na serwerze usług AD FS. Aby uzyskać informacje na temat instalacji przeczytaj o [Wprowadzenie do serwera uwierzytelniania wieloskładnikowego Azure](multi-factor-authentication-get-started-server.md).
2. W konsoli zarządzania serwera uwierzytelniania wieloskładnikowego Azure kliknij ikonę **usług AD FS** , a następnie wybierz odpowiednie opcje **rejestrowania użytkownika Zezwalaj** i **Zezwalaj użytkownikom na wybieranie metody**.
3. Wybierz inne potrzebne opcje, które chcesz określić Twojej organizacji.
4. Kliknij przycisk **Zainstaluj AD FS karty**.
<center>![Chmury](./media/multi-factor-authentication-get-started-adfs-w2k12/server.png)</center>
5. Jeśli zostanie wyświetlone okno usługi Active Directory, oznacza to dwie czynności. Komputer jest dołączony do domeny i konfiguracji usługi Active Directory do zabezpieczania komunikacji między kartą usług AD FS i Usługa uwierzytelniania wieloskładnikowego jest niepełna. Kliknij przycisk **Następny** automatyczne wykonywanie tej konfiguracji, lub zaznacz **pominąć automatycznej konfiguracji usługi Active Directory i konfigurowanie ustawień ręcznie** zaznacz pole wyboru, a następnie kliknij przycisk **Dalej**.
6. Jeśli grupa lokalna systemu windows jest wyświetlany, oznacza to dwie czynności. Komputer nie jest dołączony do domeny i konfiguracji lokalnej grupy do zabezpieczania komunikacji między kartą usług AD FS i Usługa uwierzytelniania wieloskładnikowego jest niepełna. Kliknij przycisk **Następny** automatyczne wykonywanie tej konfiguracji, lub zaznacz **pominąć automatycznej konfiguracji grupy lokalnej i konfigurowanie ustawień ręcznie** zaznacz pole wyboru, a następnie kliknij przycisk **Dalej**.
7. W Kreatorze instalacji kliknij przycisk **Dalej**. Serwer uwierzytelniania wieloskładnikowego Azure tworzy grupy administratorów PhoneFactor i dodaje konto usługi usług AD FS do grupy Administratorzy PhoneFactor.
<center>![Chmury](./media/multi-factor-authentication-get-started-adfs-w2k12/adapter.png)</center>
8. Na stronie **Uruchom Instalatora** kliknij przycisk **Dalej**.
9. W uwierzytelnianie wieloskładnikowe AD FS Instalator karty kliknij przycisk **Dalej**.
10. Po zakończeniu instalacji kliknij przycisk **Zamknij** .
11. Po zainstalowaniu karta musi być zarejestrowany z usług AD FS. Otwieranie programu Windows PowerShell i wpisz następujące polecenie:<br>
    `C:\Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1`
   <center>![Chmury](./media/multi-factor-authentication-get-started-adfs-w2k12/pshell.png)</center>
12. Użyć karty nowo zarejestrowanych, Edytuj zasady globalnej uwierzytelniania w programie AD FS. W konsoli zarządzania usług AD FS przejdź do węzła **Zasady uwierzytelniania** . W sekcji **Uwierzytelnianie wieloskładnikowe** kliknij łącze **Edytuj** obok sekcji **Ustawienia globalne** . W oknie **Edytowanie globalnej zasad uwierzytelniania** wybierz **Uwierzytelnianie wieloskładnikowe** jako metoda uwierzytelniania dodatkowe, a następnie kliknij **przycisk OK**. Karta jest zarejestrowany jako WindowsAzureMultiFactorAuthentication. Uruchom usługę usług AD FS rejestracji została uwzględniona.

<center>![Chmury](./media/multi-factor-authentication-get-started-adfs-w2k12/global.png)</center>

W tym momencie serwer uwierzytelniania wieloskładnikowego jest skonfigurowane do się z dostawcą dodatkowego uwierzytelniania za pomocą usług AD FS.

## <a name="install-a-standalone-instance-of-the-ad-fs-adapter-by-using-the-web-service-sdk"></a>Zainstaluj wystąpienie autonomicznej karty usług AD FS przy użyciu zestawu SDK usługi sieci Web
1. Zainstaluj zestaw SDK usługi sieci Web na serwerze, na którym jest uruchomiony serwer uwierzytelniania wieloskładnikowego.
2. Skopiuj następujące pliki z \Program Files\Multi-katalogu czynniki uwierzytelniania serwera na serwerze, na którym zamierzasz zainstalować kartę usług AD FS:
  - MultiFactorAuthenticationAdfsAdapterSetup64.msi
  - Rejestr MultiFactorAuthenticationAdfsAdapter.ps1
  - Unregister MultiFactorAuthenticationAdfsAdapter.ps1
  - MultiFactorAuthenticationAdfsAdapter.config
3. Uruchamianie pliku instalacyjnego MultiFactorAuthenticationAdfsAdapterSetup64.msi.
4. W usług AD FS uwierzytelnianie wieloskładnikowe Instalator karty kliknij przycisk **Dalej** , aby rozpocząć instalację.
5. Po zakończeniu instalacji kliknij przycisk **Zamknij** .

## <a name="edit-the-multifactorauthenticationadfsadapterconfig-file"></a>Edytowanie pliku MultiFactorAuthenticationAdfsAdapter.config

Wykonaj poniższe czynności, aby edytować plik MultiFactorAuthenticationAdfsAdapter.config:

1. Węzeł **UseWebServiceSdk** ustaw **wartość true**.  
2. Ustaw wartość dla **WebServiceSdkUrl** do adresu URL SDK usługi sieci Web uwierzytelnianie wieloskładnikowe. Na przykład: *https://contoso.com/&lt;certificatename&gt;/MultiFactorAuthWebServicesSdk/PfWsSdk.asmx*, gdzie certificatename jest nazwę certyfikatu.  
3. Edytowanie skryptu MultiFactorAuthenticationAdfsAdapter.ps1 rejestru, dodając *- ConfigurationFilePath &lt;ścieżki&gt; * do końca `Register-AdfsAuthenticationProvider` polecenie, miejsce, w którym * &lt;ścieżki&gt; * jest pełną ścieżką do pliku MultiFactorAuthenticationAdfsAdapter.config.

### <a name="configure-the-web-service-sdk-with-a-username-and-password"></a>Konfigurowanie SDK usługi sieci Web przy użyciu nazwy użytkownika i hasła

Dostępne są dwie opcje dotyczące konfigurowania SDK usługi sieci Web. Pierwszym jest za pomocą nazwy użytkownika i hasła, drugi z certyfikatem klienta. Wykonaj te kroki dla opcji pierwszy, lub od razu przejść do drugiego.  

1. Ustaw wartość dla **WebServiceSdkUsername** do konta, które jest członkiem grupy zabezpieczeń Administratorzy PhoneFactor. Używanie &lt;domeny&gt;& #92; &lt;nazwa użytkownika&gt; format.  
2. Ustaw wartość dla **WebServiceSdkPassword** hasło odpowiednie konto.

### <a name="configure-the-web-service-sdk-with-a-client-certificate"></a>Konfigurowanie SDK usługi sieci Web z certyfikatem klienta

Jeśli nie chcesz, aby za pomocą nazwy użytkownika i hasła, wykonaj poniższe czynności, aby skonfigurować SDK usługi sieci Web z certyfikatem klienta.

1. Certyfikat klienta można uzyskać od urzędu certyfikacji serwera, który jest uruchomiony SDK usługi sieci Web. Dowiedz się, jak [uzyskać certyfikat klienta](https://technet.microsoft.com/library/cc770328.aspx).  
2. Importowanie certyfikatu klienta w magazynie certyfikatów osobistych komputera lokalnego na serwerze, który jest uruchomiony SDK usługi sieci Web. Upewnij się, czy certyfikat publiczny urząd certyfikacji znajduje się w magazynie certyfikatów zaufanych certyfikatów głównych.  
3. Eksportowanie kluczy publicznych i prywatnych certyfikatu klienta do pliku pfx.  
4. Klucz publiczny w formacie Base64 eksportowanie do pliku cer.  
5. W Menedżerze serwera sprawdź, czy jest zainstalowany, funkcja Uwierzytelnianie mapowań certyfikatów klientów \Web Server\Security\IIS serwer sieci Web (IIS). Jeśli nie jest zainstalowany, wybierz pozycję **Dodaj role i funkcje** dodać tę funkcję.  
6. W Menedżerze IIS kliknij dwukrotnie **Edytora konfiguracji** w witrynie sieci Web zawierającą katalog wirtualny SDK usługi sieci Web. Należy to zrobić na poziomie witryny sieci Web, a nie na poziomie katalogu wirtualnego.  
7. Przejdź do sekcji **system.webServer/security/authentication/iisClientCertificateMappingAuthentication** .  
8. Włącz **wartość PRAWDA**.  
9. Ustawianie oneToOneCertificateMappingsEnabled na **wartość true**.  
10. Kliknij przycisk **...** oneToOneMappings, a następnie kliknij przycisk **Dodaj** .  
11. Otwórz plik cer Base64 wyeksportowanego wcześniej. Usuwanie *---rozpocząć certyfikat---*, *---zakończyć certyfikat---*i wszelkie podziały wierszy. Kopiowanie wynikowy ciąg znaków.  
12. Konfigurowanie certyfikatu do ciągu skopiowanego w poprzednim kroku.  
13. Włącz **wartość PRAWDA**.  
14. Ustawianie nazwy użytkownika do konta, które jest członkiem grupy zabezpieczeń Administratorzy PhoneFactor. Używanie &lt;domeny&gt;& #92; &lt;nazwa użytkownika&gt; format.  
15. Ustawianie hasła do hasła odpowiednie konto, a następnie zamknij Edytor konfiguracji.  
16. Kliknij łącze **Zastosuj** .  
17. W katalogu wirtualnego SDK usługi sieci Web kliknij dwukrotnie **uwierzytelniania**.  
18. Upewnij się, że ASP.NET personifikacja i uwierzytelnianie podstawowe są ustawione na **włączone**i że wszystkie pozostałe elementy są ustawione na **wyłączony**.  
19. W katalogu wirtualnego SDK usługi sieci Web kliknij dwukrotnie **Ustawienia protokołu SSL**.  
20. Ustaw **Akceptuj**certyfikaty klienta, a następnie kliknij przycisk **Zastosuj**.  
21. Skopiuj plik PFX wyeksportowanego wcześniej do serwera, na którym jest uruchomiony karty usług AD FS.  
22. Zaimportuj plik pfx w magazynie certyfikatów osobistych komputera lokalnego.  
23. Kliknij prawym przyciskiem myszy i wybierz pozycję **Zarządzaj kluczy prywatnych**, a następnie udzielić dostępu do odczytu do konta, którego użyto do zalogowania się do usług AD FS.  
24. Otwórz certyfikat klienta i skopiuj odcisku palca na karcie **Szczegóły** .  
25. W pliku MultiFactorAuthenticationAdfsAdapter.config ustawiona **WebServiceSdkCertificateThumbprint** ciąg skopiowany w poprzednim kroku.  


Na koniec, aby zarejestrować karty, uruchom \Program Files\Multi-uwierzytelniania czynniki Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1 skrypt programu PowerShell. Karta jest zarejestrowany jako WindowsAzureMultiFactorAuthentication. Uruchom usługę usług AD FS rejestracji została uwzględniona.

## <a name="related-topics"></a>Tematy pokrewne

Pomocy dotyczącej rozwiązywania problemów, zobacz [Często zadawane pytania uwierzytelnianie wieloskładnikowe na Azure](multi-factor-authentication-faq.md)
