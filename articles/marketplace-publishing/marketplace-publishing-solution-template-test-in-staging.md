<properties
   pageTitle="Testowanie Twoją ofertę szablonu rozwiązanie dla Marketplace | Microsoft Azure"
   description="Opis sposobu testowania Twoją ofertę szablonu rozwiązanie dla usługi Azure Marketplace."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/04/2015"
   ms.author="hascipio; v-divte" />

# <a name="test-your-solution-template-offer-in-staging"></a>Przetestuj swoją ofertę szablonu rozwiązanie podczas przenoszenia
Organizowanie oznacza, że wdrażanie Twoją ofertę w prywatną "piaskownicy" miejsce, w którym można przetestować i sprawdź jego funkcje przed naciśnięcie go do produkcji. Oferta zostanie wyświetlony w tymczasowej tak samo, jak ją do klienta, który został wdrożony go. Musisz certyfikat Twoją ofertę zostać przeniesiony do tymczasowego.

Po oferty są umieszczane, można wyświetlać i przetestować oferty w [Azure Portal](https://portal.azure.com/).

Wykonaj poniższe czynności, aby push Twoją ofertę do przemieszczania i sprawdź go w [Azure Portal](https://portal.azure.com/):

1.  Przejdź do pozycji [Portal publikowania](https://publish.windowsazure.com) > karta**Szablony rozwiązanie** > Twoją ofertę > **Publikuj** > **przekazać do tymczasowego**.
2.  Podaj na liście Azure subskrypcji, które będą używane do przetestowania Twoją ofertę.
3.  Zaloguj się do portalu Azure podglądu przy użyciu Identyfikatora subskrypcji, którego użyto w poprzednim kroku.
4.  Wykonaj co najmniej jedną round badań w portal Azure preview w punktach wymienionych poniżej:
  - Upewnij się, że treści marketingowych jest wyświetlany poprawnie na Azure Marketplace.
  - Wdrażanie zakończenia do końca topologii.
  - Wykonywanie testów wydajności oraz testów skrajnych warunków.
  - Upewnij się, że topologii odpowiada najlepsze rozwiązania.

## <a name="next-steps"></a>Następne kroki
Jeśli jest zadowalający, a następnie można przystąpić do fazy publikowania oferty końcowy **Krok 4**: [Wdrażanie Twoją ofertę do witryny Marketplace programu](marketplace-publishing-push-to-production.md). W przeciwnym razie zmiany w Twoją ofertę i poproś certyfikacji ponownie.

> [AZURE.NOTE] Marketingowych zmian zawartości certyfikacji nie jest wymagane.

Zobacz [Wprowadzenie: jak publikować ofertę Azure Marketplace](marketplace-publishing-getting-started.md) przewodnika do wszystkich zadań w programie publisher.
