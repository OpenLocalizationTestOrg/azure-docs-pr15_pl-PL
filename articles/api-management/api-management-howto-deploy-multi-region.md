<properties
    pageTitle="Jak wdrożyć wystąpienie usługi zarządzania interfejsu API Azure do wielu regionów Azure"
    description="Dowiedz się, jak wdrożyć wystąpienie usługi zarządzania interfejsu API Azure do wielu regionów Azure." 
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

# <a name="how-to-deploy-an-azure-api-management-service-instance-to-multiple-azure-regions"></a>Jak wdrożyć wystąpienie usługi zarządzania interfejsu API Azure do wielu regionów Azure

Zarządzanie interfejsu API obsługuje wdrażania wielu region, umożliwiający wydawcy interfejsu API do rozpowszechniania pojedynczego interfejsu API usługi zarządzania przez dowolną liczbę odpowiedniej regionów Azure. W ten sposób zmniejszyć żądanie opóźnienie traktowany przez geograficznie distributed interfejsu API konsumentów, a także zwiększa dostępność usługi, jeśli jeden region przechodzi do trybu offline. 

Po początkowym utworzeniu usługi zarządzania interfejsu API zawiera tylko jedną [jednostkę][] i znajduje się w pojedynczej regionu Azure, który jest oznaczony jako podstawowego Region. Dodatkowe regionów można łatwo dodawać portalu klasycznym Azure. Serwer bramy zarządzania interfejs API zostanie wdrożony każdego regionu i ruchu połączeń będą kierowane do najbliższego bramy. Jeśli obszar przechodzi do trybu offline, ruch jest automatycznie skierowana do kolejnej najbliższej bramy. 

> [AZURE.IMPORTANT] W przypadku wdrożenia jest dostępna tylko w warstwie **[Premium][]** .

## <a name="add-region"> </a>Wdrażanie wystąpienie usługi zarządzania interfejsu API do nowego regionu

Aby rozpocząć pracę, kliknij przycisk **Zarządzaj** w portalu klasyczny Azure dotyczące usługi zarządzania interfejsu API. Powoduje przejście do portalu zarządzania interfejsu API programu publisher.

![Portal programu Publisher][api-management-management-console]

>Jeśli wystąpienie usługi zarządzania interfejsu API nie został jeszcze utworzony, zobacz [Tworzenie wystąpienie usługi zarządzania interfejsu API][] samouczka [Wprowadzenie do zarządzania interfejsu API Azure][] .

Przejdź do karty **Skala** w klasycznym Portal Azure wystąpienia usługi zarządzania interfejsu API. 

![Karta Skala][api-management-scale-service]

Aby wdrożyć nowy region, kliknij na liście rozwijanej poniżej obszaru podstawowego i wybierz region z listy.

![Dodawanie regionu][api-management-add-region]

Po wybraniu obszaru Wybierz liczba jednostek nowy region z listy rozwijanej jednostek.

![Jednostki][api-management-select-units]

Po odpowiednim regionów i jednostki są skonfigurowane, kliknij przycisk **Zapisz**.

## <a name="remove-region"> </a>Usunąć wystąpienie usługi zarządzania interfejsu API z regionu

Aby usunąć wystąpienie usługi zarządzania interfejsu API z regionu, przejdź do karty **Skala** w klasycznym Portal Azure wystąpienia usługi zarządzania interfejsu API. 

![Karta Skala][api-management-scale-service]

Kliknij przycisk **X** po prawej stronie obszaru odpowiedniej do usunięcia.  

![Usuwanie regionu][api-management-remove-region]

Po odpowiednim regionów zostaną usunięte, kliknij przycisk **Zapisz**.


[api-management-management-console]: ./media/api-management-howto-deploy-multi-region/api-management-management-console.png

[api-management-scale-service]: ./media/api-management-howto-deploy-multi-region/api-management-scale-service.png
[api-management-add-region]: ./media/api-management-howto-deploy-multi-region/api-management-add-region.png
[api-management-select-units]: ./media/api-management-howto-deploy-multi-region/api-management-select-units.png
[api-management-remove-region]: ./media/api-management-howto-deploy-multi-region/api-management-remove-region.png

[Tworzenie wystąpienia usługi zarządzania interfejsu API]: api-management-get-started.md#create-service-instance
[Wprowadzenie do zarządzania interfejsu API platformy Azure]: api-management-get-started.md

[Deploy an API Management service instance to a new region]: #add-region
[Delete an API Management service instance from a region]: #remove-region

[jednostki]: http://azure.microsoft.com/pricing/details/api-management/
[Premium]: http://azure.microsoft.com/pricing/details/api-management/

