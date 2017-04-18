<properties
    pageTitle="Azure Active Directory B2C: Token, sesji i konfiguracji rejestracji jednokrotnej | Microsoft Azure"
    description="Token, sesji i pojedynczego logowania jednokrotnego konfiguracji Azure Active Directory B2C"
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
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-token-session-and-single-sign-on-configuration"></a>Azure Active Directory B2C: Token, sesji i konfiguracji rejestracji jednokrotnej

Ta funkcja zapewnia precyzyjne sterowanie, [Podstawa na zasady](active-directory-b2c-reference-policies.md)programu:
 
1. Istnienia tokenów zabezpieczających dostarczanych przez B2C usługi Azure Active Directory (Azure AD).
2. Istnienia zarządzane przez Azure AD B2C sesji aplikacji sieci web.
3. Logowania jednokrotnego (SSO) zachowanie w wielu aplikacji i zasad w dzierżawie B2C.

Tej funkcji można użyć w dzierżawie B2C następujący:

1. Wykonaj poniższe czynności Przejdź [do karta funkcje B2C](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) portalu Azure.
2. Kliknij pozycję **zasady logowania**. *Uwaga: można użyć tej funkcji na dowolnego typu zasad, nie tylko w* *Zasady logowania***.
3. Otwórz zasady, klikając go. Na przykład polecenie **B2C_1_SiIn**.
4. Kliknij przycisk **Edytuj** w górnej części karta.
5. Kliknij przycisk **Token, sesji i konfiguracji rejestracji jednokrotnej**.
6. Wprowadź odpowiednie zmiany. Informacje na temat dostępne właściwości w kolejnych sekcjach.
7. Kliknij **przycisk OK**.
8. Kliknij pozycję **Zapisz** w górnej części karta.

![Zrzut ekranu: token, sesji i konfiguracji rejestracji jednokrotnej](./media/active-directory-b2c-token-session-sso/token-session-sso.png)

## <a name="token-lifetimes-configuration"></a>Konfiguracja istnienia tokenu

Azure AD B2C obsługuje [Protokół OAuth 2.0](active-directory-b2c-reference-protocols.md) umożliwiających bezpiecznego dostępu do chronionych zasobów. Aby zastosować tę obsługę, Azure AD B2C emituje różnych [tokenów zabezpieczających](active-directory-b2c-reference-tokens.md). Poniżej przedstawiono właściwości, które umożliwiają zarządzanie istnienia tokenów zabezpieczających dostarczanych przez Azure AD B2C:

- **Dostęp do i identyfikator istnienia tokenu (w minutach)**: istnienia token okaziciela OAuth 2.0 umożliwia uzyskanie dostępu do chronionych zasobów. Azure AD B2C problemów tylko identyfikator tokeny w tej chwili. Ta wartość będzie dotyczy również tokeny dostępu, gdy Dodajemy obsługę je.
   - Domyślne = 60 minut.
   - Minimalna (włącznie) = 5 minut.
   - Maksymalna (włącznie) = 1440 minut.
- **Okres ważności tokenu odświeżanie (dni)**: maksymalny okresu, przed którym tokenu odświeżanie umożliwia uzyskiwanie nowych access lub token Identyfikatora (i opcjonalnie nowego odświeżanie tokenu, jeśli udzieliła aplikacji `offline_access` zakresu).
   - Domyślne = 14 dni.
   - Minimalna (włącznie) = 1 dzień.
   - Maksimum (włącznie) = 90 dni.
- **Token odświeżania przesuwanie okres istnienia okna (dni)**: po upływie tego okresu użytkownik jest wymuszone uwierzytelnienie, niezależnie od okres ważności najnowsze Odśwież token nabyte przez aplikację. Może być udostępniana, tylko jeśli przełącznik jest ustawiona na **Bounded**. Wymagane jest większe niż lub równe **okres ważności tokenu odświeżania (dni)** wartość. Jeśli przełącznik jest ustawiona na **Unbounded**, nie zawiera określonej wartości.
   - Domyślne = 90 dni.
   - Minimalna (włącznie) = 1 dzień.
   - Maksymalna (włącznie) = 365 dni.

Oto kilka przypadków użycia, które można włączyć przy użyciu tych właściwości:

- Umożliwia użytkownikowi być zarejestrowany w aplikacji mobilnej, dopóki dana osoba jest ciągłe aktywne na pasku aplikacji. Można to zrobić przez ustawienie **odświeżania przesuwania okna okres ważności tokenu (dni)** przełączyć się **Unbounded** w zasad logowania.
- Spełnia zabezpieczeń usługi branżowe i zgodności z przepisami, ustawiając istnienia token dostępu.

## <a name="session-configuration"></a>Konfigurowanie sesji

Azure AD B2C obsługuje [Łączenie OpenID protokół uwierzytelniania](active-directory-b2c-reference-oidc.md) umożliwiających bezpiecznego logowania do aplikacji sieci web. Poniżej przedstawiono właściwości, które umożliwiają zarządzanie sesji aplikacji sieci web:

- **W przeglądarce czas trwania sesji (w minutach)**: istnienia cookie sesji Azure AD B2C przechowywany w przeglądarce użytkownika po pomyślnym uwierzytelnieniu.
   - Domyślne = 1440 minut.
   - Minimalna (włącznie) = 15 minut.
   - Maksimum (włącznie) = 1440 minut.
- **Limit czasu sesji aplikacji sieci Web**: Jeśli ta opcja jest ustawiona na **bezwzględne**, użytkownik wymuszone uwierzytelnienie po terminie określonym przez upłynie **aplikacji Web app czas trwania sesji (w minutach)** . Jeśli ta opcja jest ustawiona na **stopniowe** (ustawienie domyślne), użytkownik pozostaje zalogowania, jak użytkownik jest wciąż aktywna w aplikacji sieci web.

Oto kilka przypadków użycia, które można włączyć przy użyciu tych właściwości:

- Wymagań branży, na zabezpieczenia i zgodność przez ustawienie sesji aplikacji sieci web odpowiedni istnienia.
- Wymusić ponowne uwierzytelnianie po pewnym okresie Ustawianie czasu podczas interakcji użytkownika z wysokim poziomie zabezpieczeń część aplikacji sieci web. 

## <a name="single-sign-on-sso-configuration"></a>Konfiguracja logowania jednokrotnego (SSO)

Jeśli masz wiele aplikacji i zasad w dzierżawie B2C, możesz zarządzać interakcji między użytkownikami a wokół nich przy użyciu właściwości **konfiguracji rejestracji jednokrotnej** . Można ustawić właściwość do jednego z następujących ustawień:

- **Dzierżawy**: jest to ustawienie domyślne. Za pomocą tej opcji umożliwia wielu aplikacji i zasad w dzierżawie B2C udostępnianie tej samej sesji użytkownika. Na przykład gdy użytkownik zaloguje się do aplikacji, zakupów firmy Contoso dana osoba może również bezproblemowo Zaloguj się do innego jednej, farmaceutycznego firmy Contoso, po dostępu do niego.
- **Aplikacja**: pozwala na Obsługa sesji użytkownika wyłącznie dla aplikacji, niezależnie od innych aplikacji. Na przykład jeśli chcesz użytkownika do zalogowania się do farmaceutycznego Contoso (przy użyciu tych samych poświadczeń), nawet jeśli dana osoba jest użytkownikiem zalogowanym do zakupów firmy Contoso, innej aplikacji na tym samym B2C dzierżawy. 
- **Zasady**: pozwala na Obsługa sesji użytkownika wyłącznie do zasad, niezależnie od aplikacji korzystania z niego. Na przykład jeśli użytkownik został już zalogowany i wykonane krokiem wielokrotne czynniki uwierzytelniania (MFA), dana osoba może mieć dostęp do części wysokim poziomie zabezpieczeń wielu aplikacji dopóki nie wygaśnie sesji powiązane zasady.
- **Wyłączone**: wymusza użytkownikom na uruchamianie przez wszystkich użytkowników podróży przy każdym wykonaniu zasady. Jeśli na przykład pozwoli to wielu użytkownikom na utworzenie konta w aplikacji (w scenariuszu udostępnionego pulpitu), nawet podczas jednego użytkownika pozostaje zalogowania przez cały czas.
