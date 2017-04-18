<properties
   pageTitle="Przetestuj swoją ofertę maszyn wirtualnych dla Marketplace | Microsoft Azure"
   description="Opis sposobu testowanie obrazu maszyn wirtualnych dla usługi Azure Marketplace."
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
   ms.date="08/01/2016"
   ms.author="hascipio" />

# <a name="test-your-vm-offer-for-the-azure-marketplace-in-staging"></a>Przetestuj swoją ofertę maszyn wirtualnych dla usługi Azure Marketplace podczas przenoszenia

Organizowanie oznacza, że wdrażanie usługi SKU w prywatną "piaskownicy", gdzie możesz przetestować i sprawdź poprawność działanie przed wdrożeniem jej do witryny Marketplace programu. Jednostka SKU pojawia się w tymczasowej tak samo, jak ją do klienta, który został wdrożony go. Obraz maszyn wirtualnych musi certyfikat zostać przeniesiony do tymczasowego.

## <a name="step-1-push-your-offer-to-staging"></a>Krok 1: Push Twoją ofertę do przemieszczania

1. Na karcie **Publikowanie** kliknij pozycję **przekazać do tymczasowego**.

    ![Rysunek](media/marketplace-publishing-vm-image-test-in-staging/vm-image-push-to-staging.png)

2. Jeśli Portal publikowania powiadomienia o jakiekolwiek błędy, popraw je.
3.  W **dostępem Twoją ofertę etapowej?** okno dialogowe Wprowadź na liście Azure subskrypcji, których będą korzystać, aby wyświetlić podgląd Twoją ofertę w [portal Azure preview](https://portal.azure.com).

    >[AZURE.NOTE] W przypadku maszyn wirtualnych i rozwiązanie szablony, zapoznaj się z **nie** subskrypcje listy sprawdzonej typu dostawcy, DreamSpark lub Azure w Otwieranie.


    > W przypadku maszyn wirtualnych po kliknięciu przycisku **PUSH do przemieszczania**następujące czynności są wykonywane za sceny. Można wyświetlić postęp każdego kroku na karcie PUBLIKUJ w publikacji portalu. Należy sprawdzić tę stronę w regularnych interwałach (do momentu stan jest wyświetlany ETAPOWA) o podanie informacji błąd, które wymagają korekty do zakończenia.

    > - Na początku tymczasowy żądania prowadzi do zespołu certyfikacji, do którego sprawdzanie poprawności wirtualnego dysku twardego. Jednak jeśli żądania otrzymano tylko marketingowe, zmienianie, następnie kroku certyfikacji jest pomijane.
    > - Po zakończeniu certyfikacji replikacja oferty rozpocząć przez wszystkich Azure centrach danych. Zwykle trwa 24-48hours na ukończenie replikacji ale może potrwać do tygodnia w zależności od rozmiaru wirtualnego dysku twardego. Jeśli jednak żądania otrzymano tylko marketingowe, zmienianie, następnie replikacji jest szybsze.
    > - Po zakończeniu replikacji następnie oferty będą dostępne w [Azure portal](http:/portal.azure.com). W tym czasie stan stają się umieszczane w publikacji portalu. Etapowej oferta jest widoczny w [Azure portal](http:/portal.azure.com) tylko przy użyciu identyfikatorów e-mail skojarzonego z subskrypcją, z którym jest umieszczane oferty.

4. Zaloguj się do [portalu Podgląd Azure](https://portal.azure.com) przy użyciu jednego z subskrypcji Azure wymienionych w poprzednim kroku.
5. Znajdź swoją ofertę i sprawdź poprawność punkty maszyn wirtualnych obrazu:
  - Upewnij się, że treści marketingowych jest wyświetlany poprawnie na rynku.
  - Kompleksowe — wdrożenie obrazu maszyn wirtualnych.

      ![img mapy portalu](media/marketplace-publishing-push-to-staging/pubportal-mapping-azure-portal.jpg)

> [AZURE.IMPORTANT] Twoją ofertę pozostanie w tymczasowej do momentu powiadomienia firmy Microsoft przy użyciu Portal publikowania [kartę**Publikowanie** > kliknij przycisk **"żądanie zatwierdzenia do wypychanych do produkcji"**] można przystąpić do produkcji. Jest to idealny czas nie wszystko podczas przygotowywania Twoją ofertę będzie widoczna dla wszystkich członków zespołu czeku.

> Tymczasowy platformy jest przeznaczony dla testów oferty w trybie Podgląd przez wydawcę. Firma Microsoft zdecydowanie zapobieżenia go za pomocą tego platofrm na potrzeby dostępnych w handlu.

## <a name="next-steps"></a>Następne kroki
Teraz, gdy Twoja oferta jest "umieszczane" i przetestowaniu działanie i treści marketingowych, można przejść do ostatniej fazy publikowania **Krok 4**: [Wdrażanie Twoją ofertę do witryny Marketplace programu](marketplace-publishing-push-to-production.md).

## <a name="see-also"></a>Zobacz też
- [Wprowadzenie: jak publikować ofertę Azure Marketplace](marketplace-publishing-getting-started.md)
