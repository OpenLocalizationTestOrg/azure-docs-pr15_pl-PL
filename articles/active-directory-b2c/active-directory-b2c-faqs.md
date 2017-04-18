<properties
    pageTitle="B2C Azure Active Directory: Często zadawane pytania | Microsoft Azure"
    description="Często zadawane pytania dotyczące Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/09/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-faqs"></a>Azure Active Directory B2C: często zadawane pytania

Ta strona zawiera odpowiedzi na często zadawane pytania dotyczące B2C usługi Azure Active Directory (Azure AD). Sprawdzać dostępność aktualizacji.

### <a name="can-i-use-azure-ad-b2c-features-in-my-existing-employee-based-azure-ad-tenant"></a>Azure AD B2C funkcji można korzystać w mojej dzierżawie Azure AD istniejących, oparte na pracowników?

Obecnie funkcje Azure AD B2C nie można włączyć w istniejącej dzierżawy usługi Azure AD. Zaleca się utworzenie oddzielnych dzierżawy za pomocą funkcji Azure AD B2C Zarządzanie osób korzystających ze.

### <a name="can-i-use-azure-ad-b2c-to-provide-social-login-facebook-and-google-into-office-365"></a>Czy można używać Azure AD B2C zapewnienie społecznościowych logowania (Facebook i Google +) do usługi Office 365?

Nie można używać Azure AD B2C przy użyciu usługi Microsoft Office 365. Na ogół nie można użyć aby zapewnić uwierzytelnienie żadnych aplikacji władz akredytacji bezpieczeństwa (Office 365, usługi Salesforce, Workday itp.). Umożliwia zarządzanie tożsamości i dostęp tylko dla sieci web dla klientów indywidualnych skierowaną i aplikacji dla urządzeń przenośnych, a nie ma zastosowania do pracowników lub u partnera scenariuszy.

### <a name="what-are-local-accounts-in-azure-ad-b2c-how-are-they-different-from-work-or-school-accounts-in-azure-ad"></a>Co to są konta lokalne w Azure AD B2C? Czym różnią się one z kont służbowych lub szkolnych w Azure AD?

W dzierżawie usługi Azure AD wszystkich użytkowników w dzierżawie (z wyjątkiem użytkowników z istniejącego konta serwera Microsoft) zaloguje się przy użyciu adresu e-mail w postaci `<xyz>@<tenant domain>`, gdzie `<tenant domain>` jest jednym z zweryfikowanych domen w dzierżawie lub początkowe `<...>.onmicrosoft.com` domeny. Ten typ konta jest konto służbowe.

Większość aplikacji ma zostać użytkownika, zaloguj się przy użyciu dowolnego adresu e-mail dowolnego dzierżawę Azure AD B2C (na przykład joe@comcast.net, bob@gmail.com, sarah@contoso.com, lub jim@live.com). Ten typ konta jest kontem lokalnym. Dzisiaj również obsługujemy nazwy dowolnego użytkownika (tylko zwykły ciągi) jako konta lokalne (na przykład joe, Robert, Aneta lub jim). Możesz wybrać jedną z tych dwóch typów lokalnego konta w usłudze Azure AD B2C.

### <a name="which-social-identity-providers-do-you-support-now-which-ones-do-you-plan-to-support-in-the-future"></a>Które dostawcy tożsamości społecznościowych czy obsługiwany jest teraz? Które z nich należy zaplanować obsługiwać w przyszłości?

Firma Microsoft obsługuje obecnie Facebook, Google +, LinkedIn i Amazon. Zostanie dodana obsługa innych dostawców popularne tożsamości społecznościowe oparte na żądanie klienta.

### <a name="can-i-configure-scopes-to-gather-more-information-about-consumers-from-various-social-identity-providers"></a>Czy można skonfigurować zakresy zebrać więcej informacji na temat odbiorcy z różnych dostawców tożsamości społecznościowe?

Nie, ale ta funkcja jest w naszym przewodnik. Zakresy domyślne używane dla naszych obsługiwane zestawu dostawcy tożsamości społecznościowych są:

- Facebook: wiadomości e-mail
- Google +: wiadomości e-mail
- Konto Microsoft: openid profilu e-mail
- Amazon: profilu
- LinkedIn: r_emailaddress, r_basicprofile

### <a name="does-my-application-have-to-be-run-on-azure-for-it-work-with-azure-ad-b2c"></a>Czy Moja aplikacja muszą być uruchamiane Azure działa on z Azure AD B2C?

Nie, może obsługiwać aplikacja w dowolnym miejscu (w chmurze lub lokalnego). Wszystkie potrzebne współdziałanie z Azure AD B2C jest możliwość wysyłania i odbierania żądania HTTP na publicznie dostępne punkty końcowe.

### <a name="i-have-multiple-azure-ad-b2c-tenants-how-can-i-manage-them-on-the-azure-portal"></a>Masz kilka dzierżaw Azure AD B2C. Jak zarządzać nimi w portalu Azure?

Każdy dzierżawy Azure AD B2C ma własny karta funkcje B2C portalu Azure. Zobacz [Azure AD B2C: zarejestrować aplikację](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) Aby dowiedzieć się, jak możesz przejść do określonego dzierżawy B2C funkcje karta portalu Azure. Przełączanie między katalogów Azure AD B2C na Azure portal nie zostaną zachowane funkcje B2C karta otworzyć w większości przeglądarek.

### <a name="how-do-i-customize-verification-emails-the-content-and-the-from-field-sent-by-azure-ad-b2c"></a>Jak dostosować weryfikacji wiadomości e-mail (zawartość i "od:" pole) wysyłane przez Azure AD B2C?

[Funkcja znakowania firmy](../active-directory/active-directory-add-company-branding.md) umożliwia dostosowywanie zawartości wiadomości e-mail weryfikacji. W szczególności można dostosować te dwa elementy poczty e-mail:

- **Transparent Logo**: znajdujący się w prawym dolnym rogu.
- **Kolor tła**: wyświetlany u góry.

    ![Zrzut ekranu przedstawiający niestandardowe wiadomość e-mail](./media/active-directory-b2c-faqs/company-branded-verification-email.png)

Podpis e-mail zawiera nazwę dzierżawy B2C, podana podczas tworzenia dzierżawy B2C. Możesz zmienić nazwę, używając następujące instrukcje:

- Zaloguj się do [portalu klasyczny Azure](https://manage.windowsazure.com/) jako Administrator subskrypcji.
- Przejdź do swojej dzierżawy B2C.
- Kliknij kartę **Konfiguruj** .
- Zmienianie **nazwy** pola w sekcji **Właściwości katalogu** .
- Kliknij pozycję **Zapisz** u dołu strony.

Obecnie jest sposobem zmiany "od:" w wiadomości e-mail. Jeśli interesuje Cię w tej funkcji i w pełni dostosowywania treści tę wiadomość e-mail, Zagłosuj na daną funkcję [możliwości](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/15334335-fully-customizable-verification-emails).

### <a name="how-can-i-migrate-my-existing-user-names-passwords-and-profiles-from-my-database-to-azure-ad-b2c"></a>Jak można przeprowadzić migrację mojej istniejącej nazwy użytkowników, hasła i profile z bazy danych Azure AD B2C?

Za pomocą interfejsu API Azure AD wykresu napisać narzędzia do migracji. Zobacz [przykładowy interfejsu API wykresu](active-directory-b2c-devquickstarts-graph-dotnet.md) Aby uzyskać szczegółowe informacje. Firma Microsoft przekazuje różne opcje migracji i narzędzia z gotowych do w przyszłości.

### <a name="what-password-policy-is-used-for-local-accounts-in-azure-ad-b2c"></a>Jakie zasady haseł jest używany dla kont lokalnych w Azure AD B2C?

Zasady haseł Azure AD B2C dla kont lokalnych jest oparty na zasady dla Azure AD. Azure AD B2C jest zapisów, zapisywania lub logowania i hasła Resetowanie zastosowania zasady siłę "silne" hasło, a nie wygasania hasła. Przeczytaj [zasady dotyczące haseł Azure AD](https://msdn.microsoft.com/library/azure/jj943764.aspx) uzyskać więcej szczegółowych informacji.

### <a name="can-i-use-azure-ad-connect-to-migrate-consumer-identities-that-are-stored-on-my-on-premises-active-directory-to-azure-ad-b2c"></a>Aby przeprowadzić migrację tożsamości dla klientów indywidualnych, które są przechowywane w mojej lokalnej usługi Active Directory Azure AD B2C można używać Azure AD Connect?

Nie, Azure AD Connect nie jest przeznaczona do pracy z Azure AD B2C. Firma Microsoft przekazuje różne opcje migracji i narzędzia z gotowych do w przyszłości.

### <a name="does-azure-ad-b2c-work-with-crm-systems-such-as-microsoft-dynamics"></a>Azure AD B2C działa systemie takich jak program Microsoft Dynamics CRM?

Obecnie nie. Integracji tych systemów jest w naszym przewodnik.

### <a name="does-azure-ad-b2c-work-with-sharepoint-on-premises-2016-or-earlier"></a>Usługa Azure AD B2C pracować z lokalnego programu SharePoint 2016 lub starszym?

Obecnie nie. Azure AD B2C nie ma obsługę tokeny SAML 1.1 portali i aplikacjach elektronicznego oparty na potrzeby lokalnego programu SharePoint. Należy zauważyć, że Azure AD B2C nie jest przeznaczony dla programu SharePoint zewnętrznego udostępniania partnera scenariusza; Zamiast tego zobacz [Azure AD B2B](http://blogs.technet.com/b/ad/archive/2015/09/15/learn-all-about-the-azure-ad-b2b-collaboration-preview.aspx) .

### <a name="should-i-use-azure-ad-b2c-or-b2b-to-manage-external-identities"></a>Czy należy używać Azure AD B2C lub B2B Zarządzanie tożsamościami zewnętrznych?

W tym artykule o [tożsamości zewnętrznych](../active-directory/active-directory-b2b-compare-external-identities.md) , aby dowiedzieć się więcej na temat stosowania odpowiednie właściwości w swoim scenariuszu zewnętrznych tożsamości.

### <a name="what-reporting-and-auditing-features-does-azure-ad-b2c-provide-are-they-the-same-as-in-azure-ad-premium"></a>Jakie raportowania i inspekcja funkcje są dostępne Azure AD B2C? Są one takie same jak Azure AD Premium?

Nie, Azure AD B2C nie obsługuje ten sam zestaw raporty jako Azure AD Premium. Azure AD B2C będzie udostępnia, podstawowe raportowania i szybko inspekcja interfejsów API.

### <a name="can-i-localize-the-ui-of-pages-served-by-azure-ad-b2c-what-languages-are-supported"></a>Czy można zlokalizowania interfejsu użytkownika obsługiwanych przez Azure AD B2C stron? Jakie języki są obsługiwane?

Obecnie Azure AD B2C jest zoptymalizowane dla języka angielskiego tylko. Planujemy wdrożeniem funkcji lokalizacji tak szybko, jak to możliwe.

### <a name="can-i-use-my-own-urls-on-my-sign-up-and-sign-in-pages-that-are-served-by-azure-ad-b2c-for-instance-can-i-change-the-url-from-loginmicrosoftonlinecom-to-logincontosocom"></a>Czy mogę używać własnej adresy URL na stronach zapisów i logowania, które są obsługiwane przez Azure AD B2C? Na przykład można zmienić adres URL z login.microsoftonline.com na login.contoso.com?

Obecnie nie. Ta funkcja jest w naszym przewodnik. Należy również zauważyć, że weryfikacją domeny na karcie **Domains** dzierżawy usługi w portalu klasyczny Azure nie wykonasz to.

### <a name="how-do-i-delete-my-azure-ad-b2c-tenant"></a>Jak usunąć mojej dzierżawy Azure AD B2C?

Wykonaj poniższe czynności, aby usunąć dzierżawy usługi Azure AD B2C:

- Wykonaj poniższe czynności Przejdź [do karta funkcje B2C](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) portalu Azure.
- Przejdź do karty **aplikacji**, **dostawcy tożsamości** i **wszystkich zasad** i Usuń wszystkie wpisy w każdej z nich.
- Teraz zalogować się do [portalu klasyczny Azure](https://manage.windowsazure.com/) jako Administrator subskrypcji. (Jest to taką samą pracę lub konta służbowego lub tego samego konta Microsoft, którego użyto do konta w usłudze Azure.)
- Przejdź do rozszerzenie usługi Active Directory po lewej stronie, a następnie kliknij pozycję dzierżawcy usługi B2C.
- Kliknij kartę **Użytkownicy** .
- Zaznacz każdy użytkownik z kolei (Wyklucz użytkownika, którego użytkownik jest obecnie zalogowany jako, to znaczy subskrypcji Administrator). Kliknij przycisk **Usuń** w dolnej części strony, a następnie kliknij przycisk **Tak,** po wyświetleniu monitu.
- Kliknij kartę **aplikacje** .
- W polu listy rozwijanej **Pokaż** wybierz pozycję **aplikacje właścicielem mojej firmy** i kliknij znacznik wyboru.
- Zobaczysz aplikacji o nazwie **b2c rozszerzenia aplikacji** wymienione poniżej. Kliknij przycisk **Usuń** w dolnej części strony, a następnie kliknij przycisk **Tak,** po wyświetleniu monitu.
- Przejdź do rozszerzenie usługi Active Directory ponownie i wybierz pozycję dzierżawcy usługi B2C.
- Kliknij przycisk **Usuń** w dolnej części strony. Postępuj zgodnie z instrukcjami na ekranie, aby ukończyć proces.

### <a name="can-i-get-azure-ad-b2c-as-part-of-enterprise-mobility-suite"></a>Gdzie mogę uzyskać Azure AD B2C jako część pakietu mobilności Enterprise?

Nie, Azure AD B2C jest repartycyjny usługi Azure i nie jest częścią pakietu mobilności przedsiębiorstwa.

### <a name="how-do-i-report-issues-with-azure-ad-b2c"></a>Jak zgłaszać problemy związane ze Azure AD B2C?

Zobacz [plik pomocy technicznej żądań Azure Active Directory B2C](active-directory-b2c-support.md).

## <a name="more-information"></a>Więcej informacji

Można także przeglądać bieżące [ograniczenia dotyczące usług, ograniczeń oraz ograniczenia](active-directory-b2c-limitations.md).
