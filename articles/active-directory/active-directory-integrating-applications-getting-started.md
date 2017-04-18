<properties
   pageTitle="Przewodnik Wprowadzenie do integracji usługi Azure Active Directory z aplikacjami wprowadzenie |  Microsoft Azure"
   description="Ten artykuł dotyczy uzyskiwania przewodnik Wprowadzenie do integracji Azure Active Directory (AD) z aplikacjami lokalnego i aplikacje w chmurze."
   services="active-directory"
   documentationCenter=""
   authors="ihenkel"
   manager="femila"
   editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="02/09/2016"
      ms.author="inhenk"/>

# <a name="integrating-azure-active-directory-with-applications-getting-started-guide"></a>Przewodnik Wprowadzenie do integracji usługi Azure Active Directory z aplikacjami wprowadzenie
## <a name="overview"></a>Omówienie
W tym temacie ma umożliwiają przewodnik integracji aplikacji z platformy Azure Active Directory (AD). Każdy z poniższych sekcjach zawiera krótkie podsumowanie bardziej szczegółowe tematu, więc można określić, które części tego przewodnika wprowadzenie pobierania są ważne dla użytkownika.  Skorzystaj z łączy do szczegółowego dive w każdym zakresie.

## <a name="before-you-begin-take-inventory"></a>Przed rozpoczęciem należy wykonać zapasów
Przed w przejść do integrowania aplikacji z usługą Azure Active Directory, należy wiedzieć, gdzie jesteś i miejsce, w którym chcesz przejść.  Poniższe pytania mają na celu pomóc wziąć pod uwagę projektu integracji aplikacji Azure AD.

### <a name="application-inventory"></a>Inwentarz aplikacji
- Gdzie są wszystkie aplikacje? Do kogo należą ich?
- Jakiego rodzaju uwierzytelniania aplikacji wymagają?
- Kto ma dostęp do aplikacji, które?
- Czy chcesz wdrożyć nowej aplikacji?
  - Będzie tworzyć we własnym zakresie i wdrożyć go na wystąpienie obliczeń Azure?
  - Będziesz używać, jedną, które są dostępne w galerii aplikacji Azure?

### <a name="user-and-group-inventory"></a>Zapasów użytkowników i grup
- Miejsce, w którym przechowywane kont użytkowników?
 - W lokalnej usłudze Active Directory
 - Azure AD
 - W bazie danych innej aplikacji, której jesteś właścicielem
 - W aplikacjach unsanctioned
 - Wszystkie powyższe
- Jakie uprawnienia i przypisania roli poszczególni użytkownicy obecnie mają? Należy przejrzeć im dostępu lub masz pewność, że przydziały dostępu i roli użytkownika są odpowiednie teraz?
- Grupy już istnieją w usłudze Active Directory w lokalnej?
 - Jak są zorganizowane grup
 - Kim są członkowie grupy?
 - Jakie przypisania uprawnień i ról grupy obecnie mają?
- Czy będą potrzebne Oczyść bazach danych dla użytkownika lub grupy przed integracji?  (Jest to naprawdę ważne pytanie. Czyszczenie w śmieci się).

### <a name="access-management-inventory"></a>Zapasów zarządzania programu Access
- Jak można obecnie Zarządzanie dostępu użytkownika do aplikacji? , Że chcesz zmienić?  Bierzesz pod uwagę inne sposoby zarządzania programu access, takie jak z [RBAC](role-based-access-control-configure.md) na przykład?
- Kto ma do czego dostęp?

Być może użytkownik nie ma odpowiedzi na wszystkie z tych pytań na początku, ale jest to rekord.  Ten przewodnik pomoże Ci odpowiedzi na niektóre z tych pytań i niektórych świadome decyzje.

## <a name="prerequisites"></a>Wymagania wstępne
- Subskrypcję usługi Azure i Azure Active directory.  Jeśli nie masz jeszcze Azure subskrypcji, możesz wypróbować Azure bezpłatnie przez 30 dni. [Przetestuj!](https://azure.microsoft.com/trial/get-started-active-directory/)

## <a name="application-integration-with-azure-ad"></a>Integracja aplikacji z usługą Azure Active Directory
### <a name="finding-unsanctioned-cloud-applications-with-cloud-app-discovery"></a>Znajdowanie unsanctioned aplikacje w chmurze z chmury odnajdowania aplikacji
Jak wcześniej wspomniano, może to być aplikacje, które nie zostały zarządzane przez organizację do tej pory.  W ramach procesu zapasów użytkownik może znaleźć aplikacje w chmurze unsanctioned. Zobacz [Znajdowanie aplikacje w chmurze unsanctioned z chmurą odnajdowania aplikacji](active-directory-cloudappdiscovery-whatis.md).

### <a name="authentication-types"></a>Typy uwierzytelniania
Poszczególnych aplikacji mogą mieć wymagania uwierzytelniania inny. Z Azure AD certyfikaty podpisywania można używać z aplikacje, które używają SAML 2.0, federacyjnych, lub protokoły łączenie OpenID a także hasło rejestracji jednokrotnej. Aby uzyskać więcej informacji na temat aplikacji typów uwierzytelniania do użytku z usługą Azure Active Directory zobacz [Zarządzanie certyfikaty dla Federacji logowania jednokrotnego w usłudze Azure Active Directory](active-directory-sso-certs.md) i [hasła na podstawie rejestracji jednokrotnej](active-directory-appssoaccess-whatis.md).

### <a name="enabling-sso-with-azure-ad-app-proxy"></a>Włączanie usługi rejestracji Jednokrotnej z Proxy aplikacji usług Azure AD
Z Proxy aplikacji usług Microsoft Azure AD umożliwiają dostęp do aplikacji znajdujące się w sieci prywatnej bezpieczne, z dowolnego miejsca i na dowolnym urządzeniu. Po zainstalowaniu łącznik serwera proxy aplikacji w środowisku, może być łatwo skonfigurowany z Azure AD.

### <a name="integrating-applications-with-azure-ad"></a>Integracja aplikacji z usługą Azure Active Directory
W następujących artykułach omówiono różne sposoby aplikacji Integracja z usługą Azure Active Directory i podaj poradnika.

- [Określanie, które usługi Active Directory za pomocą](active-directory-administer.md)
- [W galerii Azure aplikacji przy użyciu aplikacji](active-directory-appssoaccess-whatis.md)
- [Integrowanie listy samouczki aplikacji władz akredytacji bezpieczeństwa](active-directory-saas-tutorial-list.md)

## <a name="managing-access-to-applications"></a>Zarządzanie dostępem do aplikacji
Następujące artykuły opisano sposoby możesz zarządzać dostępem do aplikacji, gdy zostały zintegrowane z Azure AD przy użyciu Azure AD łączników i Azure AD.

- [Zarządzanie dostępem do aplikacji za pomocą Azure AD](active-directory-managing-access-to-apps.md)
- [Automatyzowanie za pomocą łączników Azure AD](active-directory-saas-app-provisioning.md)
- [Przypisywanie użytkowników do aplikacji](active-directory-applications-guiding-developers-assigning-users.md)
- [Przypisywanie grup aplikacji](active-directory-applications-guiding-developers-assigning-groups.md)
- [Udostępnianie konta](active-directory-sharing-accounts.md)

## <a name="integrating-custom-applications"></a>Integrowanie aplikacje niestandardowe
Jeśli piszesz nowej aplikacji i chcesz ułatwiających deweloperów używanie power Azure AD, zobacz [deweloperów Guiding](active-directory-applications-guiding-developers-for-lob-applications.md).

Jeśli chcesz dodać niestandardowej aplikacji do galerii aplikacji Azure, zobacz ["Wprowadzić własne aplikacji" z konfiguracją Azure AD samoobsługowego SAML](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx).

## <a name="see-also"></a>Zobacz też

- [Artykuł indeks do zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
