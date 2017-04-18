<properties 
    pageTitle="Interfejs API zarządzania podstawowych pojęć" 
    description="Informacje na temat interfejsów API, produktów, ról, grupy i inne podstawowe pojęcia interfejsu API zarządzania." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-import-the-definition-of-an-api-with-operations-in-azure-api-management"></a>Importowanie definicji interfejsu API z operacjami wykonywanymi w zarządzaniu interfejsu API platformy Azure

W zarządzaniu interfejsu API można tworzyć nowe składniki interfejsu API i działania dodane ręcznie lub API można zaimportować wraz z operacji w jednym kroku.

Interfejsy API i ich działania mogą być importowane przy użyciu następujących formatów.

-   WADL
-   Swagger

Ten przewodnik pokazuje jak utworzyć nowy interfejs API i zaimportować operacjach w jednym kroku. Informacji na temat ręcznego tworzenia interfejsu API i dodawanie operacji zobacz [jak utworzyć interfejsy API][] i [jak dodać operacje, aby interfejs API][].

## <a name="import-api"> </a>Importowanie interfejs API

Interfejsy API są utworzona i skonfigurowana w portalu programu publisher. Aby uzyskać dostęp do portalu programu publisher, kliknij przycisk **Zarządzaj** w portalu klasycznym Azure dotyczące usługi zarządzania interfejsu API. Jeśli wystąpienie usługi zarządzania interfejsu API nie został jeszcze utworzony, zobacz [Tworzenie wystąpienie usługi zarządzania interfejsu API][] samouczka [Wprowadzenie do zarządzania interfejsu API Azure][] .

![Portal programu Publisher][api-management-management-console]

Kliknij pozycję **interfejsy API** z **Interfejsu API zarządzania** menu po lewej stronie, a następnie kliknij **Importowanie interfejsu API**.

![Interfejs API importu][api-management-import-apis]

Okno **Importowanie interfejsu API** zawiera trzy karty, które odpowiadają trzy sposoby zapewnienia specyfikacji interfejsu API.

-   **Ze Schowka** można wkleić pole tekstowe wyznaczonych specyfikacji interfejsu API.
-   **Z pliku** umożliwia Wyszukaj i wybierz plik, który zawiera specyfikacji interfejsu API.
-   **Z adresu URL** umożliwia podanie adresu URL do specyfikacji dla interfejsu API.

![Format importu interfejsu API][api-management-import-api-clipboard]

Po zapewnieniu specyfikacji interfejsu API, wskaż format Specyfikacja za pomocą przycisków opcji po prawej stronie. Obsługiwane są następujące formaty.

-   WADL
-   Swagger

Następnie wprowadź **sufiks adresu URL interfejsu API sieci Web**. To jest dołączany do podstawowy adres URL usługi zarządzania interfejsu API. Podstawy adres URL jest wspólne dla wszystkich interfejsów API hostowana w każdym wystąpieniu usługi zarządzania interfejsu API. Zarządzanie interfejsu API odróżnia interfejsy API przez ich sufiks i w związku z tym sufiks musi być unikatowa dla każdego interfejsu API w określonej kolejności usługi zarządzania interfejsu API.

Po wprowadzeniu wszystkich wartości kliknij przycisk **Zapisz** , aby utworzyć API i skojarzone operacji. 

>[AZURE.NOTE] Samouczek importowania prostego kalkulatora interfejsu API w formacie Swagger zobacz [Zarządzanie do pierwszego interfejsu API w zarządzaniu interfejsu API Azure](api-management-get-started.md).

## <a name="export-api"></a> Interfejs API eksportu

Oprócz zaimportować nowe składniki interfejsu API, możesz wyeksportować definicje API usługi z portalu programu publisher. W tym celu kliknij **Interfejs API eksportu** z **karta Podsumowanie** z **interfejsu API**.

![Interfejs API eksportu][api-management-export-api]

Interfejsy API można eksportować przy użyciu WADL lub Swagger. Wybierz odpowiedni format, kliknij przycisk **Zapisz**i wybierz lokalizację, w której chcesz zapisać plik.

![Format interfejs API eksportu][api-management-export-api-format]

## <a name="next-steps"> </a>Następne kroki

Po utworzeniu interfejs API i operacje zaimportowane, możesz przejrzeć i skonfigurować dodatkowe ustawienia, Dodaj API produktu, a następnie opublikować go, tak aby była dostępna dla deweloperów. Aby uzyskać więcej informacji zobacz następujące przewodniki.

-   [Jak skonfigurować ustawienia interfejsu API][]
-   [Jak tworzyć i publikować produktu][]




[api-management-management-console]: ./media/api-management-howto-import-api/api-management-management-console.png
[api-management-import-apis]: ./media/api-management-howto-import-api/api-management-api-import-apis.png
[api-management-import-api-clipboard]: ./media/api-management-howto-import-api/api-management-import-api-wizard.png
[api-management-export-api]: ./media/api-management-howto-import-api/api-management-export-api.png
[api-management-export-api-format]: ./media/api-management-howto-import-api/api-management-export-api-format.png

[Import an API]: #import-api
[Export an API]: #export-api
[Configure API settings]: #configure-api-settings
[Next steps]: #next-steps

[Wprowadzenie do zarządzania interfejsu API platformy Azure]: api-management-get-started.md
[Tworzenie wystąpienia usługi zarządzania interfejsu API]: api-management-get-started.md#create-service-instance

[Jak dodać operacje do interfejsu API]: api-management-howto-add-operations.md
[Jak tworzyć i publikować produktu]: api-management-howto-add-products.md
[Jak utworzyć interfejsy API]: api-management-howto-create-apis.md
[Jak skonfigurować ustawienia interfejsu API]: api-management-howto-create-apis.md#configure-api-settings
