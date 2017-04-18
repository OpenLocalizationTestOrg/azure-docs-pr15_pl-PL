<properties 
    pageTitle="Konfigurowanie rejestracji jednokrotnej dla aplikacji, które nie znajdują się w galerii aplikacji usługi Azure Active Directory | Microsoft Azure" 
    description="Dowiedz się, jak do samodzielnego łączyć aplikacje usługi Azure Active Directory przy użyciu SAML i oparte na hasło logowania jednokrotnego" 
    services="active-directory" 
    authors="asmalser-msft"  
    documentationCenter="na" manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="02/09/2016" 
    ms.author="asmalser" />

#<a name="configuring-single-sign-on-to-applications-that-are-not-in-the-azure-active-directory-application-gallery"></a>Konfigurowanie rejestracji jednokrotnej dla aplikacji, które nie znajdują się w galerii aplikacji usługi Azure Active Directory

Ten artykuł dotyczy funkcji, która umożliwia administratorom konfigurowanie rejestracji jednokrotnej dla aplikacji nie ma w usługi Azure Active Directory aplikacji gallery *bez pisania kodu*. Ta funkcja została wydana z technical preview 18 listopada 2015 i znajduje się w [Usłudze Azure Active Directory Premium](active-directory-editions.md). Jeśli zamiast tego szukasz Deweloper wytyczne dotyczące sposobu integracja niestandardowe aplikacje z Azure AD przy użyciu kodu, zobacz [Scenariusze uwierzytelniania Azure AD](active-directory-authentication-scenarios.md).

Galeria aplikacji usługi Azure Active Directory zawiera listę aplikacji, które są znane do obsługi formie rejestracji jednokrotnej z usługi Azure Active Directory, zgodnie z opisem w [tym artykule](active-directory-appssoaccess-whatis.md). Po (jako IT specialist lub system integratora w Twojej organizacji) znalezieniu aplikacji chcesz nawiązać połączenie, można rozpocząć, postępuj zgodnie z instrukcjami krok po kroku przedstawione w portalu zarządzania Azure umożliwiające rejestracji jednokrotnej.

Te dodatkowe funkcje być także przeznaczony dla klientów z licencjami [Azure Active Directory Premium](active-directory-editions.md) :

* Sklep internetowy integracji dowolnej aplikacji obsługującej dostawcy tożsamości SAML 2.0 (zainicjowane SP lub zainicjowane protokołu IdP)
* Sklep internetowy integracji dowolnej aplikacji sieci web, zawierającej opartych na języku HTML strona logowania przy użyciu [oparte na hasło logowania jednokrotnego](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)
* Połączenie samodzielnego aplikacji, które korzystają z protokołu SCIM dla użytkownika obsługi administracyjnej ([opisane poniżej](active-directory-scim-provisioning.md))
* Możliwość dodawania łączy do dowolnej aplikacji w ikonie [Uruchamianie aplikacji usługi Office 365](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) lub [panel dostępu Azure AD](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users)

Może to być nie tylko aplikacje władz akredytacji bezpieczeństwa, za pomocą przez użytkownika, które mają nie jeszcze zostały na następować do galerii aplikacji Azure AD, ale aplikacji sieci web innej firmy, które w organizacji wdrożono do serwerów, które można kontrolować, w chmurze lub lokalnego.

Te funkcje, nazywane także *Szablony integracji aplikacji*, podaj punktów połączeń zgodne ze standardami dla aplikacji, które obsługują SAML, SCIM lub uwierzytelniania opartego na formularzach i obejmują elastyczne opcje i ustawienia pod kątem zgodności z numerem ogólne aplikacji. 

##<a name="adding-an-unlisted-application"></a>Dodawanie aplikacji spoza listy

Aby połączyć aplikacji przy użyciu szablonu integracji aplikacji, zaloguj się do portalu zarządzania Azure przy użyciu konta administratora usługi Azure Active Directory, a następnie przejdź do **usługi Active Directory > [katalog] > aplikacje** sekcji, wybierz pozycję **Dodaj**, a następnie **Dodaj aplikację z galerii**. 

![][1]

W galerii aplikacji możesz dodać aplikację spoza listy przy użyciu Kategoria **Niestandardowa** , po lewej stronie lub klikając łącze **Dodawanie aplikacji spoza listy** , który jest wyświetlany w wynikach wyszukiwania, jeśli nie odnaleziono odpowiedniej aplikacji. Po wprowadzeniu nazwy aplikacji, można skonfigurować opcje rejestracji jednokrotnej i zachowaniem. 

**Szybka wskazówka**: zgodnie z zaleceniami dotyczącymi użyć funkcji Szukaj, aby sprawdzić, czy aplikacja już istnieje w galerii aplikacji. Jeśli aplikacja znajduje się i jej opis wzmianki "logowaniu jednokrotnym", a następnie aplikacja jest już obsługiwane dla federacyjnych rejestracji jednokrotnej. 

![][2]

Dodawanie aplikacji w ten sposób zapewnia bardzo podobne do dostępne dla wstępnie zintegrowane aplikacje. Aby rozpocząć, wybierz pozycję **Konfigurowanie logowania jednokrotnego**. Następnym ekranie przedstawiono następujące trzy opcje konfigurowania rejestracji jednokrotnej, które zostały opisane w poniższych sekcjach.

![][3]


##<a name="azure-ad-single-sign-on"></a>Azure AD rejestracji jednokrotnej

Wybierz tę opcję, aby skonfigurować opartego na protokole SAML uwierzytelniania dla aplikacji. Wymaga to, że katalog application support SAML 2.0, dzięki czemu powinny zbierać informacje na temat korzystania z możliwości SAML aplikacji przed kontynuowaniem. Po wybraniu **następnego**, wyświetli monit o wprowadź trzy różne adresy URL odpowiadającego punkty końcowe SAML dla aplikacji. 

![][4]
 
Są to:

* **Logowania na adres URL (zainicjowane SP tylko)** — w przypadku, gdy użytkownik przechodzi do logowania się do tej aplikacji. Jeśli aplikacja jest skonfigurowana do wykonywania usługi inicjowanych przez dostawcę rejestracji jednokrotnej na, a następnie, gdy użytkownik przechodzi do ten adres URL, usługodawca wykona niezbędne przekierowywania Azure AD do uwierzytelniania i logowania użytkownika w. Jeśli to pole jest wypełniane, następnie Azure AD przy użyciu tego adresu URL uruchamianie aplikacji usługi Office 365 i Panel Azure AD dostępu. Jeśli to pole jest ommited, a następnie Azure AD wykona zamiast tego dostawcy tożsamości — inicjowane znak na gdy aplikacja zostanie uruchomione z usługi Office 365, panelu Azure AD dostępu lub Azure AD pojedynczy znak na adres URL (copiable na karcie pulpitu nawigacyjnego).

* **Adres URL wystawcy** - wystawcy, który adres URL należy jednoznacznie identyfikować aplikacji, dla których rejestracji jednokrotnej na jest konfigurowany. Jest to wartość Azure AD wysyła do stosowania SAML token jako parametr **odbiorców** , a aplikacja ma poprawność. Ta wartość pojawia się też jako **Identyfikator jednostki** w dowolnym metadanych SAML określonej przez aplikację. W dokumentacji aplikacji SAML szczegółowych informacji na temat co to jest identyfikator jednostki lub jest wartość odbiorców. Poniżej przedstawiono przykładowy wygląd adres URL odbiorców w token SAML zwracane do aplikacji:

```
    <Subject>
    <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:unspecificed">chad.smith@example.com</NameID>
        <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
      </Subject>
      <Conditions NotBefore="2014-12-19T01:03:14.278Z" NotOnOrAfter="2014-12-19T02:03:14.278Z">
        <AudienceRestriction>
          <Audience>https://tenant.example.com</Audience>
        </AudienceRestriction>
      </Conditions>
```

* **Adres URL odpowiedzi** — odpowiedź, która jest używany adres URL miejsce, w którym zamierza odbieranie SAML token aplikacji. Jest to także nazywane adres **URL usługi dla klientów indywidualnych potwierdzenia (ACS)**. Sprawdź dokumentacji SAML aplikacji, aby uzyskać szczegółowe informacje o możliwościach odpowiedzi tokenu SAML adres URL lub adres URL ACS.
 Po tych zostały wprowadzone, kliknij przycisk **Dalej** , aby przejść do następnego ekranu. Na tym ekranie zawiera informacje o co musi być skonfigurowane na stronie aplikacji, aby mógł zaakceptować token SAML z usługi Azure Active Directory. 

![][5]

Wartości, które są wymagane będzie się zmieniać w zależności od aplikacji, więc zaglądaj dokumentacji SAML aplikacji, aby uzyskać szczegółowe informacje. **Logowania jednokrotnego** i **wyrejestrowywania** usługi zarówno rozpoznawać samego punktu końcowego, który jest punkt końcowy żądania obsługi SAML dla wystąpienia programu Azure AD adres URL. Adres URL wystawcy jest wartość, która jest wyświetlana jako "Wystawcy" wewnątrz SAML token wydany do aplikacji. 

Po skonfigurowaniu aplikacji kliknij przycisk **Dalej** , a następnie **Zakończ** , aby zamknąć okno dialogowe. 

##<a name="assigning-users-and-groups-to-your-saml-application"></a>Przypisywanie użytkowników i grup w aplikacji SAML 

Po skonfigurowaniu aplikacji umożliwia Azure AD jako dostawcy tożsamości opartego na protokole SAML jest niemal przystąpić do testowania. Jako formant zabezpieczeń Azure AD nie wydaje tokenu, co pozwala im zalogować się do aplikacji, chyba że były udzielono dostępu za pomocą Azure AD. Użytkownicy mogą mieć udzielony dostęp bezpośrednio lub za pośrednictwem grupy, do której są one członkiem. 

Aby przypisać użytkownikowi lub grupie do aplikacji, kliknij przycisk **Przypisz użytkowników** . Wybierz użytkownika lub grupy, którą chcesz przypisać, a następnie kliknij przycisk **Przypisz** . 

![][6]

Przypisywanie użytkownika, aby umożliwić Azure AD ogłaszania tokenu dla użytkownika, a także powoduje kafelka aplikacji są wyświetlane w panelu dostępu użytkownika. Kafelka aplikacji będzie także wyświetlane w obszarze Uruchamianie aplikacji usługi Office 365, jeśli użytkownik korzysta z usługi Office 365. 

Możesz przekazać logo kafelka aplikacji przy użyciu przycisku **Przekaż Logo** na karcie **Konfigurowanie** aplikacji. 

###<a name="customizing-the-claims-issued-in-the-saml-token"></a>Dostosowywanie oświadczeniach wydawane w SAML token 

Jeśli użytkownik uwierzytelnia się do aplikacji, Azure AD problemów SAML token do aplikacji, zawierający informacje (lub oświadczeniach) o użytkowniku, który jednoznacznie identyfikuje je. Domyślnie zawiera nazwę użytkownika, adres e-mail, imię i nazwisko użytkownika. 

Można wyświetlić lub edytować oświadczeniach wysłane w token SAML do aplikacji na karcie **atrybuty** . 

![][7]

Istnieją dwie możliwe przyczyny, dlaczego trzeba edytowanie oświadczeniach wydawane w SAML token: •maksymalny aplikacji zostały zapisane w wymagają innego zestawu roszczeń identyfikatory URI lub wartości oświadczeń wdrożeniu aplikacji •Twój w sposób, który wymaga NameIdentifier rościć sobie na inną niż nazwa użytkownika (AKA głównej nazwy użytkownika) przechowywanych w usłudze Azure Active Directory. 

Aby uzyskać informacje na temat dodawania i edytowania roszczeń dotyczących tych scenariuszy zapoznaj się z tego [artykułu na oświadczeniach dostosowania](active-directory-saml-claims-customization.md). 

###<a name="testing-the-saml-application"></a>Testowanie aplikacji SAML 

Po SAML adresy URL i certyfikat zostały skonfigurowane w Azure AD i w aplikacji, użytkowników lub grup zostały przypisane do aplikacji w Azure, roszczenia zostały przejrzeć i edytować w razie potrzeby, a następnie użytkownik jest gotowa do zalogowania się do aplikacji. 

Aby przetestować, po prostu logowania w Azure AD dostęp do panelu u https://myapps.microsoft.com przy użyciu konta użytkownika, które zostały przydzielone do aplikacji, a następnie kliknij na kafelku dla aplikacji rozpocząć proces logowania pojedynczy. Możesz też przejść bezpośrednio do logowania jednokrotnego adres URL aplikacji i logowanie stamtąd. 

Debugowanie porady, zobacz ten [artykuł na temat debugowania opartego na protokole SAML rejestracji jednokrotnej dla aplikacji](active-directory-saml-debugging.md) 

##<a name="password-single-sign-on"></a>Hasło logowania jednokrotnego

Wybierz tę opcję, aby skonfigurować [oparte na hasłach rejestracji jednokrotnej](active-directory-appssoaccess-whatis.md) dla aplikacji sieci web zawierającą stronę logowania HTML. Oparte na hasło logowania jednokrotnego również określone hasło vaulting, umożliwia zarządzanie dostępem użytkowników i hasła w aplikacjach sieci web, które nie obsługują federacji tożsamości. Jest również przydatne w sytuacjach, gdzie kilku użytkowników potrzeba udostępnienia dla jednego konta, takich jak do kont aplikacji społecznościowych w Twojej organizacji. 

Po wybraniu **następnego**, wyświetli monit o wprowadź adres URL aplikacji sieci web strona logowania. Należy zauważyć, że należy strony, która zawiera pola wprowadzania nazwy użytkownika i hasła. Po wpisaniu, Azure AD uruchamia proces przeanalizować strony logowania do wprowadzania nazwy użytkownika i hasła wprowadzania. Jeśli nie powiedzie się proces, następnie prowadzi użytkownika przez alternatywny proces instalowania rozszerzenia przeglądarki (wymaga programu Internet Explorer, Chrome i Firefox), które umożliwia przechwytywanie ręcznie pola.

Gdy strona logowania jest rejestrowany, można przypisać użytkownikom i grupom i zasady poświadczeń można ustawić tak jak zwykły [hasła logowania jednokrotnego aplikacje](active-directory-appssoaccess-whatis.md).

Uwaga: Możesz przekazać logo kafelka aplikacji przy użyciu przycisku **Przekaż Logo** na karcie **Konfigurowanie** aplikacji. 

##<a name="existing-single-sign-on"></a>Istniejące logowania jednokrotnego

Wybierz tę opcję, aby dodać łącze do aplikacji do portalu Panel Azure AD dostępu lub usługi Office 365 Twojej organizacji. Umożliwia to dodać łącza do aplikacji web niestandardowe, które obecnie używają Azure Active Directory Federation Services (lub innej usługi federacyjne) zamiast Azure AD dla uwierzytelniania. Lub można dodawać głębokości łącza do określonych stron programu SharePoint lub innych stron sieci web, które chcesz po prostu wyświetlane na panele dostępu użytkownika. 

Po wybraniu **następnego**, wyświetli monit o wprowadź adres URL aplikacji, aby utworzyć łącze do. Po zakończeniu, użytkownicy i grupy mogą przypisany do aplikacji, która powoduje, że aplikacja się w ikonę [Uruchamianie aplikacji usługi Office 365](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) lub [panel dostępu Azure AD](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users) dla tych użytkowników.

Uwaga: Możesz przekazać logo kafelka aplikacji przy użyciu przycisku **Przekaż Logo** na karcie **Konfigurowanie** aplikacji.

## <a name="related-articles"></a>Artykuły pokrewne

- [Artykuł indeks do zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
- [Jak dostosować oświadczeniach wydawane w Token SAML dla wstępnie zintegrowane aplikacje](active-directory-saml-claims-customization.md)
- [Rozwiązywanie problemów z opartego na protokole SAML logowania jednokrotnego](active-directory-saml-debugging.md)

<!--Image references-->
[1]: ./media/active-directory-saas-custom-apps/customapp1.png
[2]: ./media/active-directory-saas-custom-apps/customapp2.png
[3]: ./media/active-directory-saas-custom-apps/customapp3.png
[4]: ./media/active-directory-saas-custom-apps/customapp4.png
[5]: ./media/active-directory-saas-custom-apps/customapp5.png
[6]: ./media/active-directory-saas-custom-apps/customapp6.png
[7]: ./media/active-directory-saas-custom-apps/customapp7.png
