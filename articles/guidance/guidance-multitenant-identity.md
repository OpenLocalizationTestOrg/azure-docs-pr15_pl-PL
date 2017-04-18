<properties
   pageTitle="Zarządzanie tożsamościami dla aplikacji Multitenant | Microsoft Azure"
   description="Najważniejsze wskazówki dotyczące zarządzania uwierzytelniania, autoryzacji i tożsamości w aplikacjach multitenant."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/02/2016"
   ms.author="mwasson"/>

# <a name="identity-management-for-multitenant-applications-in-microsoft-azure"></a>Zarządzanie tożsamościami dla multitenant aplikacji Microsoft Azure

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Tę serię opisano najważniejsze wskazówki dotyczące multitenancy, używając Azure AD dla uwierzytelniania i tożsamości zarządzanie.

Gdy tworzysz aplikację multitenant jedną z pierwszej problemach jest używana do zarządzania tożsamości użytkownika, ponieważ teraz każdy użytkownik należy do dzierżawy. Na przykład:

- Użytkownicy, zaloguj się przy użyciu poświadczeń organizacji.
- Użytkownicy powinni mieć dostęp do ich organizacji danych, ale nie danych, należącą do innych dzierżaw.
- Organizacja może konta w aplikacji, a następnie przypisać role aplikacji do jej członków.

Azure Active Directory (Azure AD) zawiera kilka doskonałych funkcji obsługujących wszystkie z tych scenariuszy.

Aby pasował serii artykułów, również utworzonych zakończeniu [wdrożenia kompleksowego] [ tailspin] multitenant aplikacji. Artykuły zastosowane, co opisano w procesie tworzenia aplikacji. Aby rozpocząć pracę z aplikacją, zobacz [Uruchamianie aplikacji ankiety](https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/docs/running-the-app.md).

Poniżej przedstawiono listę artykuły w tej serii:

- [Wprowadzenie do zarządzania tożsamością multitenant aplikacji](guidance-multitenant-identity-intro.md)
- [Informacje o aplikacji firma ankiet](guidance-multitenant-identity-tailspin.md)
- [Uwierzytelnianie przy użyciu Azure AD i łączenie OpenID](guidance-multitenant-identity-authenticate.md)
- [Praca z systemem roszczeń tożsamości](guidance-multitenant-identity-claims.md)
- [Zapisów i dzierżawa ułatwiającej rozpoczęcie korzystania](guidance-multitenant-identity-signup.md)
- [Role aplikacji](guidance-multitenant-identity-app-roles.md)
- [Oparta na rolach i opartych na zasobach autoryzacji](guidance-multitenant-identity-authorize.md)
- [Zabezpieczanie wewnętrznej bazy danych interfejsu API sieci web](guidance-multitenant-identity-web-api.md)
- [Buforowanie tokeny dostępu uwierzytelnianie OAuth2](guidance-multitenant-identity-token-cache.md)
- [Używanie federacyjnych z usług AD FS klienta multitenant aplikacji Azure](guidance-multitenant-identity-adfs.md)
- [Pobieranie tokeny dostępu z Azure AD przy użyciu klienta potwierdzenia](guidance-multitenant-identity-client-assertion.md)
- [Chronienie hasła aplikacji przy użyciu klucza magazynu](guidance-multitenant-identity-keyvault.md)

[tailspin]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
