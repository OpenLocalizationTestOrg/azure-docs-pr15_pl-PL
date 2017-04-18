
<properties
    pageTitle="Migrowanie do usług Azure poznawcze zalecenia interfejsu API z zaleceniami DataMarket interfejsu API | Microsoft Azure"
    description="Azure maszynowego uczenia zalecenia — migracji do usługi poznawcze zalecenia"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="luisca"/>


# <a name="migrate-to-azure-cognitive-services-recommendations-api-from-the-datamarket-recommendations-api"></a>Migrowanie do usług Azure poznawcze zalecenia interfejsu API z zaleceniami DataMarket interfejsu API
W tym artykule pokazano, jak przeprowadzić migrację z [Interfejsu API programu Microsoft DataMarket zalecenia](https://datamarket.azure.com/dataset/amla/recommendations) [Microsoft Azure poznawcze zalecenia interfejs API usług](https://www.microsoft.com/cognitive-services/en-us/recommendations-api).

Interfejs API zalecenia DataMarket przestanie akceptować nowych klientów 31 grudnia 2016 i zostanie zastąpiona 28 lutego 2017.

## <a name="how-do-i-start-using-the-azure-cognitive-services-recommendations-api"></a>Jak rozpocząć korzystanie z platformy Azure poznawcze usług zalecenia interfejsu API?

Aby przeprowadzić migrację poznawcze API zalecenia dotyczące usług, wykonaj następujące czynności:

1.  Jeśli nie masz jeszcze Azure subskrypcji, [Utwórz konto](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1) jednego. 

1.  Uzyskaj instrukcje krok po kroku z [Przewodnik Szybki Start](cognitive-services-recommendations-quick-start.md) do konta w usłudze poznawcze interfejs API zalecenia dotyczące usług i używać go programistycznie. 

1.  Przejdź do pozycji [Strona początkowa poznawcze interfejs API zalecenia dotyczące usług](https://www.microsoft.com/cognitive-services/en-us/recommendations-api) Dowiedz się więcej o usłudze i znajdowanie dokumentacji.

## <a name="i-used-the-recommendations-ui-to-build-my-models-is-there-a-similar-tool-for-the-cognitive-services-recommendations-api"></a>Zalecenia dotyczące interfejsu użytkownika używane do tworzenia Moje modeli. W przypadku poznawcze usług zalecenia interfejsu API jest podobne narzędzie?

Naprawdę! Adres URL nowego [Interfejsu użytkownika zalecenia](http://recommendations-portal.azurewebsites.net/) jest http://recommendations-portal.azurewebsites.net. 

>[AZURE.NOTE] Poświadczenia usługi DataMarket nie działa w tym miejscu. Rejestrowanie subskrypcji w portalu Azure i uzyskać klucz konta potrzebny do korzystania z nowego [Interfejsu użytkownika zalecenia](http://recommendations-portal.azurewebsites.net/).
Jeśli nie masz klucz konta, zobacz zadania 1 [Przewodnik Szybki Start](cognitive-services-recommendations-quick-start.md).

##<a name="is-the-new-api-format-the-same-as-the-datamarket-recommendations-api"></a>Na nowy format interfejsu API jest taki sam, jak interfejs API zalecenia DataMarket?

Interfejs API obsługuje samej scenariusze i proces przepływów jako te scenariusze obsługiwane w wersji DataMarket, ale rzeczywista interfejs API został zaktualizowany, aby pasowały do [Wskazówki dotyczące interfejsu API pozostałych Microsoft](https://github.com/Microsoft/api-guidelines/blob/master/Guidelines.md). Za pośrednictwem interfejsów API są bardziej spójny i pracy lepiej przy użyciu narzędzia obsługujące Swagger.

Aby zrozumieć poszczególnych interfejsów API, zapoznaj się z [interfejsu API Eksploratora](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3db).
Użyj, *Spróbuj* go przycisk, aby przetestować wywołanie API. Format plików zużywanej przez interfejs API zalecenia (pliki wykazu i zastosowania) nie została zmieniona, możesz korzystać same pliki i/lub infrastruktury skonstruowaniu generowanie tych plików.

##<a name="what-are-some-new-features-in-the-cognitive-services-recommendations-api"></a>Jakie są niektóre nowe funkcje w poznawcze API zalecenia dotyczące usług?

W ciągu ostatnich dwóch miesięcy masz opublikowano nowe funkcje dla poznawcze interfejsu API zalecenia dotyczące usług:
-   Zalecenia dotyczące interfejsu użytkownika dla szkoleń i testowania modeli bez konieczności pisania jednego wiersza kodu
-   Partia wyników umożliwia tysiące zalecenia jednocześnie
-   Tworzenie metryki pomocy technicznej do kwerendy jakości modeli zalecenia
-   Obsługa reguł biznesowych
-   Możliwość wyliczanie i pobieranie plików użycia i wykazu
-   Klasyfikowania możliwość tworzenie kwerendy jakości funkcje elementu w modelu zalecenia
-   Dodano możliwość wyszukiwania dla produktu w katalogu

## <a name="when-does-microsoft-stop-supporting-the-datamarket-recommendations-api"></a>Gdy Microsoft zatrzymać pomocnicze API zalecenia DataMarket?

[Interfejs API zalecenia na DataMarket](https://datamarket.azure.com/dataset/amla/recommendations) przestaje przyjmować nowych klientów po 31 grudnia 2016 i będzie można całkowicie przestarzałe przez 28 lutego 2017. 

## <a name="what-if-i-dont-have-the-data-that-i-need-to-recreate-my-models-in-the-cognitive-services-recommendations-api"></a>Co zrobić, jeśli nie mam dane, które trzeba ponownie utworzyć Moje modeli w poznawcze interfejs API zalecenia dotyczące usług?

Potrzebne zmiany ustawień, wystarczy dla Ciebie. Firma Microsoft może ułatwić przenoszenie modelach stare z konta usługi DataMarket do nowej subskrypcji interfejs API Azure poznawcze usług zalecenia. Kontakt z nami u [mlapi@microsoft.com](mailto://mlapi@microsoft.com). Zostanie wyświetlony monit o podanie usługi DataMarket identyfikator subskrypcji i identyfikator subskrypcji poznawcze usług, przed modeli powiązane z nowego konta.
