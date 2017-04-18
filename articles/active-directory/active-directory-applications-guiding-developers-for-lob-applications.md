<properties
    pageTitle="Azure AD i aplikacjach: przeprowadzi deweloperów | Microsoft Azure"
    description="Przeznaczony dla specjalistów IT, w tym artykule przedstawiono wskazówki dotyczące integracji Azure aplikacji z usługą Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="kgremban"/>

# <a name="azure-ad-and-applications-develop-line-of-business-apps"></a>Azure AD i aplikacjach: można opracowywać aplikacje LOB:

Ten przewodnik zawiera omówienie tworzenia aplikacji (LoB) z LOB dla Azure Active Directory (AD). Określonej grupy odbiorców jest administratorów globalnych usługi Active Directory i Office 365.

## <a name="overview"></a>Omówienie

Tworzenie aplikacji zintegrowany z usługą Azure Active Directory umożliwia użytkownikom w Twojej organizacji logowania jednokrotnego w usłudze Office 365. Wniosek o w Azure AD zapewnia kontrolę nad zasady uwierzytelniania dla aplikacji. Aby dowiedzieć się więcej na temat warunkowe dostęp i jak chronić aplikacje z uwierzytelnianie wieloskładnikowe (MFA), zobacz [Konfigurowanie programu access reguły](active-directory-conditional-access-azuread-connected-apps.md).

Zarejestruj się w aplikacji za pomocą usługi Azure Active Directory. Rejestrowanie aplikacji oznacza, że deweloperów można używać Azure AD do uwierzytelniania użytkowników i żądanie dostępu do zasobów użytkownika, takich jak wiadomości e-mail, kalendarza i dokumenty.

Każdy członek katalogu (nie gości) można zarejestrować aplikacji, nazywanego *tworzenia obiektu aplikacji*.

Rejestrowanie aplikacji umożliwia użytkownikowi wykonaj następujące czynności:

- Pobieranie tożsamości w swojej aplikacji, który rozpoznaje Azure AD
- Uzyskiwanie jeden lub więcej haseł i kluczy, które aplikacji można używać w celu uwierzytelnienia bramy do AD
- Oznaczanie marką aplikacji w portalu Azure przy użyciu niestandardowej nazwy, logo itp.
- Stosowanie funkcji autoryzacji Azure AD do ich aplikacji, w tym:
  - Kontrola dostępu oparta na rolach (RBAC)
  - Azure Active Directory oAuth autoryzacji serwera (bezpiecznego interfejs API ujawnionego przez aplikację)

- Zadeklaruj wymagane uprawnienia niezbędne w celu zastosowania funkcji zgodnie z oczekiwaniami, w tym:-uprawnień aplikacji (tylko administratorzy globalny). Na przykład: członkostwo ról w innej Azure AD aplikacji lub roli członkostwa względem Azure zasób, grupa zasobów, lub innej subskrypcji - delegowane uprawnienia (każdego użytkownika). Na przykład: Azure AD logowania, a następnie przeczytaj profilu


> [AZURE.NOTE]Domyślnie każdy uczestnik można zarejestrować aplikacji. Aby dowiedzieć się, jak ograniczyć uprawnienia do rejestrowania aplikacji do określonych członków, zobacz [jak aplikacje są dodawane do Azure AD](active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).

Oto, co, administrator globalny, należy wykonać w celu rozwiązania przygotowania ich stosowania do produkcji:

- Konfigurowanie reguły dostępu (zasady dostępu i MFA)
- Konfigurowanie aplikacji wymagają przypisania użytkowników i przypisywanie użytkowników
- Pomiń domyślne środowisko zgody użytkownika

## <a name="configure-access-rules"></a>Konfigurowanie reguł programu access

Konfigurowanie reguły każdej aplikacji dostępu do swoich aplikacji władz akredytacji bezpieczeństwa. Można na przykład wymagają MFA lub tylko w zaufanych sieciach Zezwalaj na dostęp do użytkowników. Szczegóły dotyczące tego są dostępne w dokumencie [Konfigurowanie reguły dostępu](active-directory-conditional-access-azuread-connected-apps.md).

## <a name="configure-the-app-to-require-user-assignment-and-assign-users"></a>Konfigurowanie aplikacji wymagają przypisania użytkowników i przypisywanie użytkowników

Domyślnie użytkownicy mają dostęp do aplikacji bez przyznania. Jednak w aplikacji udostępnia role lub ma być wyświetlane na panel dostępu użytkownika aplikacji, należy wymagać Przypisywanie użytkownika.

[Wymaganie Przypisywanie użytkownika](active-directory-applications-guiding-developers-requiring-user-assignment.md)

Jeśli jest to Twoja subskrypcja Azure AD Premium lub pakietu mobilności przedsiębiorstwa (EMS), firma Microsoft zaleca używanie grup. Przypisywanie grup aplikacji umożliwia delegowanie zarządzania zapewnienia stałego dostępu do właściciela grupy. Możesz utworzyć grupę lub poproś strony odpowiedzialnej w Twojej organizacji, aby utworzyć grupę za pomocą funkcji zarządzania z grupy.

[Przypisywanie użytkowników do aplikacji](active-directory-applications-guiding-developers-assigning-users.md)  
[Przypisywanie grup aplikacji](active-directory-applications-guiding-developers-assigning-groups.md)

## <a name="suppress-user-consent"></a>Pomiń zgody użytkownika

Domyślnie każdy użytkownik przez moduł obsługi zgody, aby się zalogować. Środowisko zgody, pytaniem użytkownikom uprawnienia do aplikacji, może być niejasna dla użytkowników, którzy nie zna podejmowania takich decyzji.

W przypadku aplikacji, którym ufasz może uprościć środowiska użytkownika, wyrażając do aplikacji w imieniu Twojej organizacji.

Aby uzyskać więcej informacji na temat zgody użytkownika i środowiska zgody w Azure zobacz [Integracji aplikacji z usługą Azure Active Directory](active-directory-integrating-applications.md).

##<a name="related-articles"></a>Artykuły pokrewne

- [Włączanie bezpieczny dostęp zdalny do aplikacji lokalnego serwera Proxy aplikacji Azure AD](active-directory-application-proxy-get-started.md)
- [Podgląd Azure dostępu warunkowego dla aplikacji władz akredytacji bezpieczeństwa](active-directory-conditional-access-azuread-connected-apps.md)
- [Zarządzanie dostępem do aplikacji z usługą Azure Active Directory](active-directory-managing-access-to-apps.md)
- [Artykuł indeks do zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
