<properties
   pageTitle="Porównanie funkcji zarządzania tożsamościami zewnętrznych za pomocą usługi Azure Active Directory | Microsoft Azure"
   description="Porównanie współpracy Azure Active Directory B2B, B2C i dzierżawy wielu aplikacji do obsługi uwierzytelniania i autoryzacji dla tożsamości zewnętrznych"
   services="active-directory"
   documentationCenter="" 
   authors="arvindsuthar"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="02/24/2016"
   ms.author="asuthar"/>

# <a name="comparing-capabilities-for-managing-external-identities-using-azure-active-directory"></a>Porównanie funkcji zarządzania tożsamościami zewnętrznych za pomocą usługi Azure Active Directory

Oprócz zarządzania pracowników mobilnych dostęp do aplikacji władz akredytacji bezpieczeństwa, usługi Azure Active Directory (Azure AD) mogą pomóc Twojej organizacji współużytkowanie zasobów z partnerami biznesowymi i aplikacje dla firm i konsumentów.

## <a name="developing-applications-for-businesses"></a>Tworzenie aplikacji dla przedsiębiorstw

Podane usługi lub aplikacji, takich jak usługi płac dla firm? Azure AD zapewnia platformę tożsamości, która umożliwia tworzenie aplikacji, które Bezproblemowa integracja z miliony organizacji, które już skonfigurowane Azure AD jako część pakietu wdrażania usługi Office 365 lub inne usługi enterprise.

**Przykład:** Dystrybutor farmaceutycznego udostępnia medyczne i systemów informatycznych ochrony zdrowia branży. One potrzebne pola aplikacji analizy jako medycznych rozwiązaniami i odpowiedniej klientów, aby Zarządzanie tożsamościami własne. Przedsiębiorstwo to wybrać Azure AD jako platformy tożsamości w swojej aplikacji Zarządzanie ćwiczeń, dostarczając tożsamości Azure AD klientom znak w górę, w razie potrzeby. Aby uzyskać więcej informacji zobacz [Przewodnik programisty usługi Azure Active Directory](active-directory-developers-guide.md).

## <a name="enabling-business-partner-access-to-your-corporate-resources"></a>Włączanie dostępu partnerów biznesowych do zasobów firmy

Czy masz partnerów biznesowych lub innych osób spoza firmy, które mają dostęp do zasobów przedsiębiorstwa, takich jak witryny programu SharePoint lub systemie ERP? Azure AD umożliwia udzielanie użytkowników zewnętrznych (może być lub nie istnieje w Azure AD) pojedynczy znak na dostęp do aplikacji firmowych administratorów. Zwiększenie bezpieczeństwa, jak użytkownicy utracić dostęp, gdy opuszczają organizacji partnera, podczas gdy Ty sterujesz zasady dostępu w organizacji. Jako nie trzeba zarządzać partnerów zewnętrznych katalogu lub na relacje Federacji partnera również ułatwia administrowanie.

**Przykład:** Obrazowania firmy zawiera sprzedawców z fotografią do obrazowania usług i działa największej sieci detalicznej kioski drukowania. Wybrać ich Azure AD umożliwiające tysiące użytkowników w ich detalicznej partnerów biznesowych za pomocą własnymi poświadczeniami Pobierz najnowszą partnera materiałów marketingowych i zmiana kolejności fotografii przetwarzania dostawy od dostawcy firmy ekstranetu. Aby uzyskać więcej informacji zobacz [Współpraca Azure AD B2B](active-directory-b2b-what-is-azure-ad-b2b.md).

## <a name="developing-applications-for-consumers"></a>Tworzenie aplikacji dla klientów indywidualnych

Potrzebujesz bezpiecznego i tanie publikowanie online aplikacjami, takimi jak z przodu sklepu detalicznej miliony konsumentów? Azure AD udostępnia platformę Włączanie społecznościowych oraz logowania nazwy użytkownika i hasła, oznakowanych samodzielnego konta i samodzielne Resetowanie hasła dla klientów indywidualnych, aplikacji. Zwiększa wygody dla osób korzystających ze redukując obciążenie deweloperów.

**Przykład:** \#1 sportowa franchisingowe na świecie, aby współpracować bezpośrednio z jego wentylatory 450 milionów. Aby to zrobić, że zbudowane aplikacji dla urządzeń przenośnych za pomocą Azure AD dla magazynu uwierzytelniania i profilu użytkownika. Wentylatory uzyskiwanie uproszczonej rejestracji i logowania przy użyciu konta społecznościowych, takich jak Facebook lub mogą używać tradycyjnych nazwy użytkownika i hasła płynną obsługę wielu iOS, Android i Windows telefony. Opierając się na wskazanych platformy Azure AD mocno ograniczona kodu niestandardowego podczas prowadzenia franchising dostosowania znakowania i złagodzenia zabezpieczeń, naruszenia danych oraz skalowalność. Aby uzyskać więcej informacji zobacz [Azure AD B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/).

## <a name="comparison-of-azure-ad-capabilities"></a>Porównanie funkcji Azure AD

Każdy scenariuszy już omawiane w tym artykule dotyczy możliwości w Azure AD. W tej tabeli powinny być pomocne wyjaśnienie, które funkcje są dla Ciebie najistotniejsze:

| **Należy rozważyć ten produkt...**       | [Azure AD wielu dzierżawy władz akredytacji bezpieczeństwa aplikacji](active-directory-developers-guide.md)    | [Azure AD B2B współpracy](active-directory-b2b-what-is-azure-ad-b2b.md)        | [Azure AD B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/)                |
|-----------------------|-------------------------|----------------------------|------------------------|
| **Jeśli musisz podać...** | usługi dla firm | Partner dostęp do mojej aplikacji  | usługi, aby odbiorcy |
| **I jestem podobne do...**  | Dystrybutor Pharma      | Do obrazowania firmy            | Franchisingowe sportowych       |
| **Wdrażanie aplikacji dla...**  | Zarządzanie ćwiczeń     | Dostawca ekstranetu          | Wentylatory piłka nożna            |
| **Kierowanie...**        | Oddziałów lekarza        | Partnerzy zatwierdzonych | Każda osoba z pocztą e-mail      |
| **Kiedy dostępne...**      | Zgadza się administrator klienta | Moje zaproszenia administratora           | Klient rejestruje się      |
