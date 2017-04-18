<properties
    pageTitle="Opis usługi Azure zewnętrznych opłat za usługi | Microsoft Azure"
    description="Informacje na temat rozliczeń usług zewnętrznych, wcześniej znana pod nazwą witryny Marketplace, opłaty platformy Azure."
    services=""
    documentationCenter=""
    authors="adpick"
    manager="felixwu"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="adpick"/>

# <a name="understand-your-azure-external-service-charges"></a>Opis usługi Azure kosztów usług zewnętrznych

W tym artykule wyjaśniono, rozliczenie usług zewnętrznych platformy Azure. Usług zewnętrznych umożliwia się o nazwie zamówienia Marketplace. Usług zewnętrznych są dostarczane przez dostawców niezależnych usługi, ale są całkowicie zintegrowane w ekosystemie Azure. Dowiedz się, jak:

- Identyfikowanie usług zewnętrznych
- Opis sposobu rozliczenie różni się od innych Azure zasobów
- Wyświetlanie i śledzić koszty, które możesz Naliczanie z korzystania z usług zewnętrznych
- Zarządzanie zleceniami zewnętrznych i jak zapłacić za

## <a name="what-are-azure-external-services"></a>Co to są Azure usług zewnętrznych?

Usług zewnętrznych umożliwia wywołać Azure Marketplace. Ogólnie rzecz biorąc są one usług opublikowany przez dostępne trzecich Azure. Na przykład ClearDB i SendGrid są usług zewnętrznych, które mogą zakupić Azure, ale nie są publikowane przez firmę Microsoft.

### <a name="identify-external-services"></a>Identyfikowanie usług zewnętrznych

Po przepisu nowych usług zewnętrznych lub zasobów jest pokazywana Ostrzeżenie:

![Ostrzeżenie dotyczące zakupu Marketplace](./media/billing-understand-your-azure-marketplace-charges/marketplace-warning.PNG)

>[AZURE.NOTE] Usługi zewnętrzne są publikowane przez firmy, które nie są firmy Microsoft, ale czasami produktów firmy Microsoft, są również określane jako usług zewnętrznych.

### <a name="external-services-are-billed-separately"></a>Dotyczy oddzielnie rozliczenie usług zewnętrznych

Usług zewnętrznych są traktowane jako poszczególnych zamówień w ramach subskrypcji usługi Azure. Okres rozliczeniowy dla każdej usługi jest ustawiona, gdy Kup usługę. Nie należy mylić z okresu rozliczeniowego subskrypcji, w którym został zakupiony. Otrzymasz osobnych rozliczenia i karty kredytowej jest naliczany oddzielnie.

### <a name="each-external-service-has-a-different-billing-model"></a>Każda zewnętrznych usługa ma inny model rozliczeń

Niektóre usługi są wystawiona w sposób płatne, podczas gdy inne osoby za pomocą modelu podstawie płatności miesięcznych. Potrzebujesz karty kredytowej dla Azure usług zewnętrznych, nie można kupić usług zewnętrznych z płacą faktury.

### <a name="you-cant-use-monthly-free-credits-for-external-services"></a>Nie można użyć miesięczny środków bezpłatne dla usług zewnętrznych

Jeśli korzystasz z subskrypcji usługi Azure, zawierającego [bezpłatne środków](https://azure.microsoft.com/pricing/spending-limits/), nie można zastosować do zewnętrznej usługi rozliczenia. Zakup usług zewnętrznych za pomocą karty kredytowej.

## <a name="view-external-service-spending-and-history"></a>Widok zewnętrznej usługi wydatków i Historia

Można wyświetlić listę usług zewnętrznych, znajdujących się w każdej subskrypcji w [portalu Azure](https://portal.azure.com/): 

1. Zaloguj się do [portalu Azure](https://portal.azure.com/) i [Przejdź do strony karta **rozliczenia** ](https://portal.azure.com/?flight=1#blade/Microsoft_Azure_Billing/BillingBlade).

    ![Wybierz pozycję rozliczenia, w menu Centrum](./media/billing-understand-your-azure-marketplace-charges/billing-button.png) 
  
2. W sekcji **koszty subskrypcja** Wybierz subskrypcję, do której chcesz wyświetlić. 
   
    ![Wybierz subskrypcję w karta rozliczeń](./media/billing-understand-your-azure-marketplace-charges/select-sub.png)

3. Kliknij pozycję **usług zewnętrznych**.

    ![Kliknij pozycję usług zewnętrznych w karta subskrypcji](./media/billing-understand-your-azure-marketplace-charges/external-service-blade.png)

4. Powinien zostać wyświetlony poszczególnych zamówień zewnętrznej usługi, nazwę wydawcy, warstwa usług, których używasz, nazwa nadana zasobu oraz bieżący stan zamówienia. Wybierz pozycję zewnętrznej usługi, aby wyświetlić ostatnie rozliczenia.

    ![Wybierz pozycję zewnętrznej usługi](./media/billing-understand-your-azure-marketplace-charges/external-service-blade2.png)

5. W tym miejscu można wyświetlić wcześniejsze kwoty rachunku tym podziału podatku.

    ![Wyświetlanie usług zewnętrznych historię rachunków](./media/billing-understand-your-azure-marketplace-charges/billing-overview-blade.png)

## <a name="manage-payment-methods-for-external-service-orders"></a>Zarządzanie formy płatności dla zleceń serwisowych zewnętrznych

Aktualizowanie metodami płatności dla zleceń serwisowych zewnętrznych z poziomu [Centrum konta](https://account.windowsazure.com/).

> [AZURE.NOTE] Jeśli zakupiono subskrypcję za pomocą konta służbowe należy [kontakt z pomocą techniczną](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , aby zmienić metodę płatności.

1. Zaloguj się do [Centrum konta](https://account.windowsazure.com/) , a następnie [Przejdź do karty **marketplace** ](https://account.windowsazure.com/Store)

    ![Wybierz pozycję marketplace w Centrum konta](./media/billing-understand-your-azure-marketplace-charges/select-marketplace.png)

2. Wybierz usługę zewnętrznych, którą chcesz zarządzać

    ![Wybierz usługę zewnętrznych, którą chcesz zarządzać](./media/billing-understand-your-azure-marketplace-charges/select-ext-service.png)

3. Kliknij pozycję **Zmień metodę płatności** w prawej części strony. To łącze łączy do różnych portalu Zarządzanie metodę płatności.
    
    ![Podsumowanie zamówienia](./media/billing-understand-your-azure-marketplace-charges/change-payment.PNG)

4. Kliknij przycisk **Edytuj informacje** , a następnie postępuj zgodnie z instrukcjami, aby zaktualizować informacje o płatności.

    ![Wybierz pozycję Edytuj informacje](./media/billing-understand-your-azure-marketplace-charges/edit-info.png)
    
## <a name="cancel-an-external-service-order"></a>Anulowanie zamówienia zewnętrznej usługi

Jeśli chcesz anulować zamówienie zewnętrznej usługi, musisz usunąć zasobu w [Azure portal](https://portal.azure.com).

![Usuwanie zasobu](./media/billing-understand-your-azure-marketplace-charges/deleteMarketplaceOrder.PNG)

## <a name="need-help-contact-support"></a>Potrzebujesz pomocy? Kontakt z pomocą techniczną.

Jeśli nadal masz dalsze pytania, szybko [kontakt z pomocą techniczną](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , aby uzyskać problemu rozwiązać, sprawdź.
