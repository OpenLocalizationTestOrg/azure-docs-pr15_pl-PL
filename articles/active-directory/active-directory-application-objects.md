<properties
pageTitle="Aplikacja Azure Active Directory i obiektów wystawcy usługi | Microsoft Azure"
description="Omówienie relacji między aplikacji i usług obiektów kapitału w usługi Azure Active Directory"
documentationCenter="dev-center-name"
authors="bryanla"
manager="mbaldwin"
services="active-directory"
editor=""/>

<tags
ms.service="active-directory"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="identity"
ms.date="08/10/2016"
ms.author="bryanla;mbaldwin"/>

# <a name="application-and-service-principal-objects-in-azure-active-directory"></a>Aplikacji i usług obiektów kapitału w usługi Azure Active Directory
Po przeczytaniu o Azure Active Directory (AD) "aplikacji" nie zawsze jest wyczyść dokładnie co to jest odwołuje się do autora. Celem tego artykułu jest był wyraźny, definiując koncepcyjny i konkretnych aspektów Azure AD integracja aplikacji, z przykładem rejestrowania i zgody na [wielu dzierżawy aplikacji](active-directory-dev-glossary.md#multi-tenant-application).

## <a name="overview"></a>Omówienie
Aplikacja Azure AD jest szerszy niż tylko piece oprogramowania. Jest koncepcyjny terminów, odwołując się nie tylko do aplikacji, ale również jego rejestracji (czyli: konfiguracji tożsamości) z Azure AD, co umożliwia mu uczestniczenie w uwierzytelniania i autoryzacji "konwersacje" w czasie rzeczywistym. Zgodnie z definicją aplikacja może działać w roli [klienta](active-directory-dev-glossary.md#client-application) (używające zasobu), rola [serwera zasobów](active-directory-dev-glossary.md#resource-server) (stwarzająca API klientom) lub nawet oba. Protokół konwersacji jest określona przez [Przepływ Udziel autoryzacji 2.0 OAuth](active-directory-dev-glossary.md#authorization-grant), mając na celu zezwolenia klienta i zasobów do programu access i ochrony zasobów danych odpowiednio. Teraz Przyjrzyjmy niższego poziomu i zobacz, jak model aplikacji Azure AD reprezentuje aplikację wewnętrznie. 

## <a name="application-registration"></a>Rejestrowanie aplikacji
Podczas rejestrowania aplikacji w [portal Azure klasyczny][AZURE-Classic-Portal], dwa obiekty są tworzone w dzierżawie usługi Azure AD: obiekt aplikacji, a obiektem głównej usługi.

#### <a name="application-object"></a>Obiekt aplikacji
Aplikacja Azure AD jest *zdefiniowane* przez jego i tylko obiekt aplikacji, który znajduje się w dzierżawie Azure AD, w której aplikacja została zarejestrowana, nazywane "domowy" dzierżawy aplikacji. Aplikacja zawiera informacje dotyczące tożsamości dla aplikacji, a jest szablon, z którego odpowiednie obiekty głównej usługi są *uzyskane* do użytku w czasie wykonywania. 

Możesz myśleć aplikację jako *globalny* reprezentację aplikacji (do użycia przez wszystkich dzierżaw) i wystawcy usługi jako *lokalnej* reprezentacji (do użycia w określonych dzierżawy). Azure AD wykresu [Jednostka aplikacji] [ AAD-Graph-App-Entity] definiuje schemat obiekt aplikacji. Obiekt aplikacji w związku z tym ma relację 1:1 z aplikacji, a wartość 1:*n* relację z jego odpowiednie obiekty głównej usługi *n* .

#### <a name="service-principal-object"></a>Obiekt głównej usługi
Obiekt głównej usługi definiuje zasady i uprawnienia dla aplikacji, dostarczając podstawę podmiot zabezpieczeń reprezentować aplikację podczas uzyskiwania dostępu do zasobów w czasie wykonywania. Azure AD wykresu [jednostki ServicePrincipal] [ AAD-Graph-Sp-Entity] definiuje schemat obiektem głównej usługi. 

Obiekt głównej usługi jest wymagane w każdym dzierżawy, dla którego wystąpienie zastosowania aplikacji musi być reprezentowana Włączanie bezpiecznego dostępu do zasobów należącą do kont użytkowników z dzierżawy. Aplikację pojedyncze dzierżawy będą mieć tylko jeden wystawcy usługi (w jego głównym dzierżawy). Wiele dzierżawy [aplikacji sieci Web](active-directory-dev-glossary.md#web-client) będzie miał wystawcy usługi w każdym dzierżawy, gdzie administrator lub użytkowników z dzierżawy wyrazić zgodę, umożliwiając go, aby uzyskać dostęp do swoich zasobów. Po zgody obiekt głównej usługi będzie konsultacji z przyszłych autoryzacji żądania. 

> [AZURE.NOTE] Wszelkie zmiany wprowadzone do obiektu aplikacji, także są odzwierciedlane w głównym celem usługi w aplikacji głównym dzierżawie tylko (dzierżawy, w którym została zarejestrowana). W przypadku wielu dzierżawy aplikacji zmiany do obiektu aplikacji zostaną odzwierciedlone w obiektach głównej usługi dowolnego dzierżaw dla klientów indywidualnych, dzierżawy dla klientów indywidualnych usuwa dostęp i ponownie udziela dostępu.

## <a name="example"></a>Przykład
Poniższy diagram przedstawia relacje między aplikacją obiektu aplikacji i odpowiadający mu usługi kapitału obiektów, w kontekście aplikacji wielu dzierżawy przykładowe o nazwie **HR aplikacji**. Istnieją trzy dzierżaw Azure AD w tym scenariuszu: 

- **Adatum** - dzierżawy używane przez firmę, która opracowanych **aplikacji godz.**
- **Contoso** — dzierżawy używane przez organizację firmy Contoso, czyli dla klientów indywidualnych **HR aplikacji**
- **Fabrikam** - dzierżawy używane przez organizację Fabrikam zajmuje także **HR aplikacji**

![Powiązanie obiekt aplikacji z obiektem głównej usługi](./media/active-directory-application-objects/application-objects-relationship.png)

Na diagramie poprzedniego kroku 1 jest proces tworzenia aplikacji i obiektów głównej usługi w dzierżawie głównym aplikacji.

W kroku 2 po zgody, wykonywane przez administratorów firmy Contoso i Fabrikam obiektem głównej usługi jest utworzony w dzierżawie Azure AD firmy i przypisane uprawnienia, które administrator udzielone. Należy również zauważyć, że aplikacji HR może być skonfigurowany pozwalają zgody przez użytkowników do użytku poszczególnych.

W kroku 3 dzierżaw dla klientów indywidualnych HR aplikacji (Contoso i Fabrikam) każdego mieć własne obiektu głównej usługi. Każdy reprezentuje zgodę ich stosowania wystąpienie aplikacji w czasie wykonywania objętych uprawnienia przez administratora odpowiednich.

## <a name="next-steps"></a>Następne kroki
Obiekt aplikacji aplikacji są dostępne za pośrednictwem interfejsu API Azure AD wykresu reprezentowaną przez jego OData [Jednostka aplikacji][AAD-Graph-App-Entity]

Obiekt głównej aplikacji usługi są dostępne za pośrednictwem interfejsu API Azure AD wykresu, reprezentowaną przez jego OData [ServicePrincipal jednostki][AAD-Graph-Sp-Entity]



<!--Image references-->

<!--Reference style links -->
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AZURE-Classic-Portal]: https://manage.windowsazure.com