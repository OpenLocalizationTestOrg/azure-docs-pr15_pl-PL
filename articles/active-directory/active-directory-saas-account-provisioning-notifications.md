<properties
    pageTitle="Konto inicjowania obsługi administracyjnej powiadomienia | Microsoft Azure"
    description="Dowiedz się, jak zapewnić, że użytkownik jest powiadamiany o problemy związane z przypisywanie użytkowników, wymagające uwagi, umożliwiając obsługi administracyjnej powiadomienia konta."
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
    ms.date="10/10/2016"
    ms.author="markusvi"/>


# <a name="account-provisioning-notifications"></a>Konto inicjowania obsługi administracyjnej powiadomienia

Z przypisywanie użytkowników, można zautomatyzować proces Zarządzanie użytkownikami w aplikacji władz akredytacji bezpieczeństwa innych firm. <br>
W trakcie procesu automatycznego od czasu do czasu wymagane jest kontaktów z tego procesu. <br>
Jest to, na przykład tak, gdy hasło konta, które zostały skonfigurowane do wymiany danych z innej strony władz akredytacji bezpieczeństwa aplikacji wygasła. 

Po włączeniu obsługi administracyjnej powiadomienia konta, możesz zdecydować, użytkownik jest powiadamiany kwestii związanych z przypisywanie użytkowników, wymagające uwagi.

Aktywowanie lub dezaktywowanie konta inicjowania obsługi administracyjnej powiadomienia jako część użytkownika inicjowania obsługi administracyjnej konfiguracji dla władz akredytacji bezpieczeństwa aplikacji innej firmy.

![Przypisywanie użytkowników][1] 



Aby aktywować konto inicjowania obsługi administracyjnej powiadomienia, zaznacz pole wyboru pokrewne na stronie okno dialogowe **potwierdzenia** , a następnie wpisz alias e-mail adresata.

![Konto inicjowania obsługi administracyjnej powiadomienia][2]
 


Na liście dystrybucyjnej można wprowadzić jako adresata; jednak należy pamiętać, że wiadomość e-mail z powiadomieniem zawiera łącza do raportów dostępnych tylko przez administratorów Azure AD.

Jeśli masz konto inicjowania obsługi administracyjnej powiadomienia włączone, będziesz otrzymywać wiadomości e-mail dotyczące krytycznych problemów, które dotyczą przypisywanie użytkowników. Jednak aby uniknąć zabezpieczenie poczty e-mail, otrzymasz tylko jedną wiadomość e-mail z powiadomieniem dziennie dla każdej aplikacji władz akredytacji bezpieczeństwa została włączona funkcja wiadomość e-mail z powiadomieniem.


##<a name="related-articles"></a>Artykuły pokrewne

- [Artykuł indeks do zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
- [Automatyzowanie użytkownika inicjowania obsługi administracyjnej i cofanie ubezpieczeń do aplikacji władz akredytacji bezpieczeństwa](active-directory-saas-app-provisioning.md)
- [Dostosowywanie mapowań atrybutów dla użytkownika inicjowania obsługi administracyjnej.](active-directory-saas-customizing-attribute-mappings.md)
- [Wyrażeń do mapowania atrybutów](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Określanie zakresu filtry dla użytkownika inicjowania obsługi administracyjnej.](active-directory-saas-scoping-filters.md)
- [Aby włączyć automatyczne inicjowania obsługi administracyjnej użytkowników i grup z usługi Azure Active Directory do aplikacji przy użyciu SCIM](active-directory-scim-provisioning.md)
- [Lista samouczki dotyczące integracja aplikacji władz akredytacji bezpieczeństwa](active-directory-saas-tutorial-list.md)



<!--Image references-->
[1]: ./media/active-directory-saas-account-provisioning-notifications/ic766307.png
[2]: ./media/active-directory-saas-account-provisioning-notifications/ic766308.png