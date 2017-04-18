<properties
    pageTitle="Przełączanie na inną ofertę subskrypcji Azure | Microsoft Azure"
    description="Dowiedz się, jak zmienić subskrypcję Azure i przełącz się do różnych oferty, za pomocą portalu zarządzania subskrypcji"
    services=""
    documentationCenter=""
    authors="genlin"
    manager="mbaldwin"
    editor=""
    tags="billing,top-support-issue"/>

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="genli"/>

# <a name="switch-your-azure-subscription-to-another-offer"></a>Przełączanie subskrypcji Azure na inną ofertę

Odbiorcy [repartycyjny](https://azure.microsoft.com/offers/ms-azr-0003p/) może być możliwe do przełączenia subskrypcji Azure na inną ofertę w [Centrum konta](https://account.windowsazure.com/Subscriptions). Za pomocą tej funkcji można na przykład skorzystać [Miesięczny środków dla subskrybentów programu Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Jeśli masz [Bezpłatną wersję próbną](https://azure.microsoft.com/free/), Dowiedz się, jak przeprowadzić [uaktualnienie do repartycyjny](billing-upgrade-azure-subscription.md).

#### <a name="whats-supported"></a>Jakie funkcje są obsługiwane:

| Z                                                              | Aby                                                                                      |
|-------------------------------------------------------------------|-----------------------------------------------------------------------------------------|
| [Repartycyjny](https://azure.microsoft.com/offers/ms-azr-0003p/) | [Repartycyjny deweloperów Test](https://azure.microsoft.com/offers/ms-azr-0023p/)              |
| [Repartycyjny](https://azure.microsoft.com/offers/ms-azr-0003p/) | [Program Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/)          |
| [Repartycyjny](https://azure.microsoft.com/offers/ms-azr-0003p/) | [Professional Test programu Visual Studio](https://azure.microsoft.com/offers/ms-azr-0060p/)     |
| [Repartycyjny](https://azure.microsoft.com/offers/ms-azr-0003p/) | [Platformy w witrynie MSDN](https://azure.microsoft.com/offers/ms-azr-0062p/)                      |
| [Repartycyjny](https://azure.microsoft.com/offers/ms-azr-0003p/) | [Enterprise programu Visual Studio](https://azure.microsoft.com/offers/ms-azr-0063p/)            |
| [Repartycyjny](https://azure.microsoft.com/offers/ms-azr-0003p/) | [Enterprise programu Visual Studio (Bizspark)](https://azure.microsoft.com/offers/ms-azr-0064p/) |

> [AZURE.NOTE] Inne oferują zmiany, [kontakt z pomocą techniczną](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
    
## <a name="switch-subscription-offer"></a>Przełączanie subskrypcji oferty

> [AZURE.VIDEO switch-to-a-different-azure-offer]

1.  Zaloguj się w witrynie [Centrum konto Azure](https://account.windowsazure.com/Subscriptions).

2.  Wybierz subskrypcję płatne.

3.  Kliknij pozycję **Przełącz do innej oferty**. Ten przycisk jest dostępny, jeśli znajdujesz się na repartycyjny i pracy z Twojej pierwszy okres rozliczeniowy.

    ![Zwróć uwagę, przycisk Przełącz oferty w prawej części strony](./media/billing-how-to-switch-azure-offer/switchbutton.png)
    
4.  **Zaznacz oferty, które mają** na liście ofert subskrypcji możesz się przełączyć. Ta lista zależy od członkostwa, które jest skojarzony z kontem. Jeśli nic się nie jest dostępny, sprawdź [listę dostępnych ofert, których można przełączyć się](#whats-supported) i upewnij się, że masz prawo członkostwa. 

    ![Zaznacz ofertę, którą chcesz przełączyć się do](./media/billing-how-to-switch-azure-offer/selectoffer.png)

5.  W zależności od oferty, który się przełączasz może zostać wyświetlony Uwaga dotycząca wpływu przełączania. Przejdź do tej listy dokładnie i postępuj zgodnie z instrukcjami, przed kontynuowaniem.

    ![Przeglądanie notatek](./media/billing-how-to-switch-azure-offer/thingstonote.png)

6.  Możesz zmienić nazwę subskrypcji. Domyślnie możemy go ustawić zgodnie z nową nazwą oferty. Kliknij pozycję **Oferują przełącznika** , aby ukończyć proces.

    ![Kliknij zielony przycisk](./media/billing-how-to-switch-azure-offer/confirmpage.png)

7.  Powodzenia! Twoja subskrypcja jest teraz włączane do nowej oferty.

## <a name="why-cant-i-switch-offers"></a>Dlaczego nie mogę przełączyć oferty?

**Przełącz do innej oferty** mogą być niewidoczne, jeśli:

- Nie jesteś na [repartycyjny](https://azure.microsoft.com/offers/ms-azr-0003p/). Obecnie tylko repartycyjny subskrypcje mogą być przełączane do innej oferty.

    - Jeśli masz [Bezpłatną wersję próbną](https://azure.microsoft.com/free/), Dowiedz się, jak przeprowadzić [uaktualnienie do repartycyjny](billing-upgrade-azure-subscription.md).

    - Aby przełączyć się oferty z innej subskrypcji, [kontakt z pomocą techniczną](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

- Jesteś nadal na pierwszy okres rozliczeniowy; trzeba poczekać usługi pierwszy okres rozliczeniowy zakończyć przed przełączeniem ofert.

**Dostępne są dostępne w regionie lub kraju w tej chwili nie ma żadnych ofert** może zostać wyświetlony, jeśli:

- Nie masz prawo do przełączników oferty. Sprawdź [listę dostępnych ofert, których można przełączyć się](#whats-supported).

## <a name="what-does-switching-azure-offers-do-to-my-service-and-billing"></a>Co oznacza przechodzenia Azure ofert do mojej usługi i rozliczeń?

Poniżej opisano, co się dzieje po przełączeniu Azure plany w Centrum konta.

### <a name="access-to-services"></a>Dostęp do usług

Nie ma żadnych przestojów dla wszystkich użytkowników skojarzone z subskrypcją. Oferty, których można przełączyć się może mieć jednak ograniczeń. Na przykład niektóre ofert zabrania użycia w produkcji, więc trzeba przenieść zasobów produkcyjnych do innej subskrypcji.

### <a name="billing"></a>Rozliczenia

W dniu, którymi można przełączać faktury jest generowany dla wszystkich pozostałych opłaty. Następnie subskrypcji jest wystawiona na warunkach cennik nowej oferty. Data zmiany ofert zmienia rocznicy rozliczeń Twojej subskrypcji. Użycie i rozliczenia danych przed Zmień oferta nie jest zachowywana, dlatego jest zalecane, Pobierz kopię przed przełączeniem.

> [AZURE.NOTE] Ze względu na ograniczenia dotyczące rozliczeń przełączniki oferty nie są dostępne w pierwszym cyklu rozliczeniowego po utworzeniu subskrypcji.

## <a name="can-i-migrate-from-pay-as-you-go-to-cloud-solution-providerhttpspartnermicrosoftcomsolutionscloud-reseller-overview-csp-or-enterprise-agreementhttpsazuremicrosoftcompricingenterprise-agreement-ea"></a>Można przeprowadzić migrację z repartycyjny [Dostawcy rozwiązań w chmurze](https://partner.microsoft.com/Solutions/cloud-reseller-overview) lub [Enterprise Agreement](https://azure.microsoft.com/pricing/enterprise-agreement/) (EA)?

Firma Microsoft obecnie nie obsługuje oferty Przełącz na Dostawcę lub EA w Centrum konta. Aby przenieść z istniejącej subskrypcji do EA, poproś administratora o rejestracji Dodaj swoje konto do EA. Następnie otrzymasz wiadomość e-mail z zaproszeniem. Po wykonaniu z instrukcjami, aby zaakceptować zaproszenie, subskrypcji są automatycznie przenoszone w obszarze Enterprise Agreement. Aby przeprowadzić migrację do dostawcy, zobacz [Azure subskrypcji migracji do dostawcy](https://blogs.technet.microsoft.com/hybridcloudbp/2016/08/26/azure-subscription-migration-to-csp/).

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak [zarządzać rolami administratora](billing-add-change-azure-subscription-administrator.md) dla subskrypcji

- Śledzenie do zastosowania, [pobierając danych dotyczących użycia i faktura](billing-download-azure-invoice-daily-usage-date.md)

## <a name="need-help-contact-support"></a>Potrzebujesz pomocy? Kontakt z pomocą techniczną.

Jeśli nadal masz dodatkowo pytania, szybko [kontakt z pomocą techniczną](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , aby uzyskać problemu rozwiązać, sprawdź.
