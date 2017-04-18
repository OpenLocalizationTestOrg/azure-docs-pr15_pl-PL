<properties
    pageTitle="Integracja usługi Azure Active Directory rejestracji jednokrotnej z aplikacjami władz akredytacji bezpieczeństwa |  Microsoft Azure"
    description="Włącz uwierzytelnianie rejestracji jednokrotnej i użytkownika inicjowania obsługi administracyjnej zarządzania scentralizowany dostęp aplikacji władz akredytacji bezpieczeństwa w usługi Azure Active Directory. Omówienie integracji usługi Azure Active Directory do aplikacji władz akredytacji bezpieczeństwa."
    services="active-directory"
      keywords="Integracja Azure AD przy użyciu aplikacji władz akredytacji bezpieczeństwa"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/30/2016"
    ms.author="curtand"/>

# <a name="integrate-azure-active-directory-single-sign-on-with-saas-apps"></a>Integracja usługi Azure Active Directory rejestracji jednokrotnej z aplikacjami władz akredytacji bezpieczeństwa  

> [AZURE.SELECTOR]
- [Azure portal](active-directory-enterprise-apps-manage-sso.md)
- [Portal Azure klasyczny](active-directory-sso-integrate-saas-apps.md)

[AZURE.INCLUDE [active-directory-sso-use-case-intro](../../includes/active-directory-sso-use-case-intro.md)]

Aby rozpocząć pracę, aby skonfigurować rejestracji jednokrotnej dla aplikacji w przypadku wprowadzenia w Twojej organizacji, będą korzystać z istniejącego katalogu w usłudze Azure Active Directory (Azure AD). Można uzyskać za pośrednictwem Microsoft Azure, Office 365 lub usługi Windows Intune katalogu Azure AD. Jeśli masz dwie lub więcej z następujących, zobacz [Administrowanie katalogu Azure AD](active-directory-administer.md) , aby określić, które.

## <a name="authentication"></a>Uwierzytelnianie

W przypadku aplikacji, które obsługują SAML 2.0 federacyjnych, lub łączenie OpenID protokoły certyfikaty można ustanawiać relacje zaufania podpisywania przy użyciu usługi Azure Active Directory. Aby uzyskać więcej informacji na ten temat zobacz [Zarządzanie certyfikatów dla federacyjnych rejestracji jednokrotnej](active-directory-sso-certs.md).

W przypadku aplikacji, które obsługują tylko HTML oparte na formularzach logowania, usługi Azure Active Directory używa "archiwizowanie zawartości hasło" można ustanawiać relacje zaufania. Dzięki temu użytkownikom w organizacji jest automatycznie zalogowanie się do aplikacji władz akredytacji bezpieczeństwa przez Azure AD przy użyciu informacji o koncie użytkownika z poziomu aplikacji władz akredytacji bezpieczeństwa. Azure AD gromadzi i bezpiecznie przechowuje informacje o kontach użytkownika i hasło pokrewnych. Aby uzyskać więcej informacji zobacz [oparte na hasło logowania jednokrotnego](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).

## <a name="authorization"></a>Autoryzacja

Konto ustanawianie umożliwia użytkownik ma mieć możliwość po uwierzytelnienia za pośrednictwem logowania jednokrotnego za pomocą aplikacji. Przypisywanie użytkowników można ręcznie lub w niektórych przypadkach można dodawać i usuwać informacje o użytkowniku z aplikacji władz akredytacji bezpieczeństwa na podstawie zmian wprowadzonych w usłudze Azure Active Directory. Aby uzyskać więcej informacji na temat korzystania z istniejących łączników Azure AD dla automatycznego inicjowania obsługi administracyjnej zobacz [użytkownika automatycznego inicjowania obsługi administracyjnej i Anuluj inicjowania obsługi administracyjnej aplikacji władz akredytacji bezpieczeństwa](active-directory-saas-app-provisioning.md).

W przeciwnym razie można ręcznie dodać informacje o użytkowniku do aplikacji, lub za pomocą innych rozwiązań obsługi administracyjnej, które są dostępne na rynku.

## <a name="access"></a>Programu Access

Azure AD udostępnia kilka możliwości dostosowywania sposobów wdrażania aplikacji użytkownikom końcowym w organizacji. Nie są zablokowane do dowolnego określonego wdrożenia lub rozwiązanie programu access. Możesz użyć [rozwiązanie, które najlepiej odpowiada potrzebom użytkownika](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).

## <a name="additional-considerations-for-applications-already-in-use"></a>Dodatkowe zagadnienia związane z aplikacjami już w użyciu

Konfigurowanie rejestracji jednokrotnej na dla aplikacji, która używa Twoja organizacja ma już jest inny proces z procesem tworzenia nowych kont dla nowej aplikacji. Istnieje kilka czynności wstępne, w tym: Mapowanie tożsamości użytkowników w aplikacji Azure AD tożsamości i opis, jak użytkownicy mogą mieć logowanie do aplikacji po jest zintegrowany.

> [AZURE.NOTE] Aby skonfigurować rejestracji Jednokrotnej dla istniejącej aplikacji, musisz mieć uprawnienia administratora globalnego w obu Azure AD i aplikacji władz akredytacji bezpieczeństwa.

### <a name="mapping-user-accounts"></a>Mapowanie kont użytkowników

Tożsamość użytkownika zwykle ma unikatowy identyfikator, który może być adres e-mail lub głównej nazwy użytkownika (UPN). Konieczne będzie łącze (tablica) każdego użytkownika tożsamości aplikacji do odpowiednich Azure AD tożsamości. Istnieje kilka sposobów, aby osiągnąć ten cel w zależności od tego, jak wymagania uwierzytelnienie aplikacji.

Aby uzyskać więcej informacji na temat mapowania tożsamości aplikacji z tożsamościami Azure AD zobacz [Dostosowywanie oświadczeniach wydawane w SAML token](http://social.technet.microsoft.com/wiki/contents/articles/31257.azure-active-directory-customizing-claims-issued-in-the-saml-token-for-pre-integrated-apps.aspx) i [Dostosowywanie atrybut mapowania inicjowania obsługi administracyjnej](active-directory-saas-customizing-attribute-mappings.md).

### <a name="understanding-the-users-log-in-experience"></a>Opis obsługi logowania użytkownika

Zintegrowaniem rejestracji Jednokrotnej dla aplikacji, która jest już używana jest zdać sobie sprawę, wpłynie środowiska użytkownika. Dla wszystkich aplikacji użytkownicy rozpocznie, aby zalogować się przy użyciu poświadczeń Azure AD. Możliwe także, że mogą używać różnych portalu uzyskiwać dostęp do aplikacji.

Rejestracji Jednokrotnej dla niektórych aplikacji można przeprowadzić logowania aplikacji w interfejsie, ale dla innych aplikacji, użytkownik będzie miał obsłużone centralnej portalu, na przykład ([Moje aplikacje](http://myapps.microsoft.com) lub [usługi Office 365](http://portal.office.com/myapps)), aby się zalogować. Dowiedz się więcej o różnych typach logowania jednokrotnego i ich środowiska użytkownika, w [jaki jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md).

Inny zasób przydatne jest *Zgoda Suppressing użytkownika* w artykule [Guiding programista](active-directory-applications-guiding-developers-for-lob-applications.md) .

## <a name="next-steps"></a>Następne kroki


W przypadku aplikacji władz akredytacji bezpieczeństwa, które znajdziesz w galerii aplikacji Azure AD udostępnia szereg [samouczki dotyczące integracja aplikacji władz akredytacji bezpieczeństwa](active-directory-saas-tutorial-list.md).

Jeśli aplikacji nie znajduje się w galerii aplikacji, możesz [go dodać do galerii Azure AD aplikacji jako niestandardową aplikację](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx).

Istnieje wiele więcej szczegółów we wszystkich tych problemów w bibliotece Azure.com, począwszy od [Co to jest dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory.](active-directory-appssoaccess-whatis.md).

## <a name="see-also"></a>Zobacz też

- [Artykuł indeks do zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
