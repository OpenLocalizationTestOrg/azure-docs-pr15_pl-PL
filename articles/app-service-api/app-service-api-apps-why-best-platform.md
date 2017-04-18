<properties 
    pageTitle="Wprowadzenie do aplikacji interfejsu API | Microsoft Azure" 
    description="Dowiedz się, jak usługa Azure aplikacji pomaga opracowywać hosta i używanie interfejsów API RESTful." 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/23/2016" 
    ms.author="rachelap"/>

# <a name="api-apps-overview"></a>Omówienie aplikacji interfejsu API

Interfejs API aplikacji w usłudze Azure aplikacji zapewniają funkcje, które ułatwiają opracowanie hosta i używanie interfejsów API w chmurze i lokalnego. W aplikacji interfejsu API otrzymasz zabezpieczeń klasy przedsiębiorstwa, kontrola dostępu prosty łączności hybrydowe, automatyczne generowanie SDK i integrację z [Aplikacjami logiczny](../app-service-logic/app-service-logic-what-are-logic-apps.md).

[Usługa Azure aplikacji](../app-service/app-service-value-prop-what-is.md) jest w pełni zarządzanych platformy dla sieci web urządzeń przenośnych i scenariusze integracji. Interfejs API aplikacji jest jednym z cztery typy aplikacji oferowanych przez [Usługę aplikacji Azure](../app-service/app-service-value-prop-what-is.md).

![Typy aplikacji w usłudze Azure aplikacji](./media/app-service-api-apps-why-best-platform/appservicesuite.png)

## <a name="why-use-api-apps"></a>Dlaczego warto używać interfejsu API aplikacje?

Poniżej przedstawiono niektóre kluczowe funkcje interfejsu API aplikacji:

- **Przesuń do istniejącego interfejsu API jako-jest** — nie trzeba zmienić dowolne z kodu do istniejącego API, aby korzystać z interfejsu API aplikacji — po prostu wdrażanie aplikacji interfejsu API kodu. Z interfejsu API można użyć dowolnego języka lub framework obsługiwane przez usługi aplikacji, w tym programu ASP.NET i C#, Java, PHP, Node.js i Python.

- **Łatwe zużycie** - zintegrowaną obsługę [metadanych Swagger interfejsu API](http://swagger.io/) sprawia, że do interfejsów API jest łatwo eksploatacyjnych przez różnych klientów.  Pole wyboru automatycznie Generuj kod klienta dla interfejsów API, w różnych językach, takich jak C#, Java i Javascript. Prosty sposób konfigurować [CORS](app-service-api-cors-consume-javascript.md) bez zmieniania kodu. Aby uzyskać więcej informacji zobacz [metadanych interfejsu API usługi aplikacji do generowania odnajdowanie i kod interfejsu API](app-service-api-metadata.md) i [Consume aplikacji interfejsu API z kodu JavaScript za pomocą CORS](app-service-api-cors-consume-javascript.md). 

- **Kontrola dostępu proste** — ochrona aplikacji interfejsu API z nieuwierzytelnionych programu access bez żadnych zmian w kodzie. Wbudowany uwierzytelnianie usługi bezpiecznego interfejsy API dostępu przez innych usług lub klientów reprezentujących użytkowników. Dostawcy tożsamości obsługiwane obejmują usługi Azure Active Directory, Facebook, Twitter, Google i Account Microsoft. Klienci mogą używać Active Directory uwierzytelniania biblioteki (ADAL) lub SDK aplikacje Mobile. Aby uzyskać więcej informacji zobacz [Uwierzytelnianie i autoryzacja interfejsu API aplikacji w usłudze Azure aplikacji](app-service-api-authentication.md).

- **Integracja z programem visual Studio** - dedykowane narzędzia w programie Visual Studio usprawnić pracę tworzenia, wdrażania przez inne, debugowania i zarządzanie aplikacjami interfejsu API. Aby uzyskać więcej informacji zobacz [informowanie o SDK Azure 2.8.1 dla środowiska .NET](/blog/announcing-azure-sdk-2-8-1-for-net/).

- **Integracja z logiki aplikacje** — aplikacji interfejsu API, które możesz utworzyć może być używana przez [Logiczny usługi aplikacji](../app-service-logic/app-service-logic-what-are-logic-apps.md).  Aby uzyskać więcej informacji zobacz [Korzystanie z niestandardowej interfejsu API hostowana w aplikacji usługi z aplikacjami logika](../app-service-logic/app-service-logic-custom-hosted-api.md) i [Nowy schemat wersji 2015-08-01-Podgląd](../app-service-logic/app-service-logic-schema-2015-08-01.md).

Aplikację interfejsu API można także korzystać z funkcji oferowanych przez [Aplikacje sieci Web](../app-service-web/app-service-web-overview.md) i [Aplikacji Mobile](../app-service-mobile/app-service-mobile-value-prop.md). Odwrotna obowiązuje również: Jeśli udostępnić interfejs API za pomocą aplikacji sieci web lub aplikacji dla urządzeń przenośnych go można korzystać z funkcji interfejsu API aplikacji, takich jak Swagger metadanych dla klienta generowanie kodu i CORS dostępu przeglądarki innej domeny. Jedyną różnicą między typami trzy aplikacji (API, web urządzeń przenośnych) jest nazwa i ikona dla nich stosowane w portalu Azure.

## <a name="whats-the-difference-between-api-apps-and-azure-api-management"></a>Jaka jest różnica między aplikacjami interfejsu API i zarządzania interfejsu API Azure?

Interfejs API aplikacji i [Zarządzania interfejsu API Azure](../api-management/api-management-key-concepts.md) są dopełniającej usług:

* Zarządzanie interfejsu API jest o zarządzaniu interfejsów API. Umieszczenie fronton interfejsu API zarządzania na interfejs API do zastosowania monitor i ograniczenia, manipulować dane wejściowe i wyjściowe, konsolidowanie wielu interfejsów API w jeden z punktów końcowych i tak dalej. Za pośrednictwem interfejsów API zarządzany można obsługiwać dowolnego miejsca.
* Interfejs API aplikacji jest temat hostingu interfejsów API. Usługa zawiera funkcje, które ułatwiają opracowywania i dużo interfejsów API, ale nie robi rodzajów monitorowania, ograniczania, manipulowanie lub konsolidowania zarządzania interfejsu API nie. Jeśli nie potrzebujesz funkcje zarządzania interfejsu API, może obsługiwać interfejsy API w aplikacjach interfejsu API bez użycia interfejsu API zarządzania.

Poniżej przedstawiono diagram, który przedstawia zarządzanie interfejsu API interfejsy API obsługiwane w aplikacjach interfejsu API i w innych miejscach.

![Zarządzanie Azure interfejsu API i aplikacje interfejsu API](./media/app-service-api-apps-why-best-platform/apia-apim.png)

Niektóre funkcje interfejsu API zarządzania i interfejs API aplikacje mają podobne funkcje.  Na przykład obie zautomatyzować CORS pomocy technicznej. Jeśli korzystasz ze sobą dwie usługi, należy użyć interfejsu API zarządzania dla CORS od jako fronton do aplikacji interfejsu API. 

## <a name="getting-started"></a>Wprowadzenie

Aby rozpocząć pracę z aplikacjami interfejsu API wdrażając przykładowy kod do jednego, zobacz samouczek niezależnie od Framework wolisz:

* [PROGRAMU ASP.NET](app-service-api-dotnet-get-started.md) 
* [Node.js](app-service-api-nodejs-api-app.md) 
* [Java](app-service-api-java-api-app.md) 

Zadawać pytania dotyczące aplikacji interfejsu API, należy uruchomić wątku [forum aplikacji interfejsu API](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps). 
