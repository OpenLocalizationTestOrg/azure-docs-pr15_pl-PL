<properties
   pageTitle="Transfer praw własności subskrypcji usługi Azure | Microsoft Azure"
   description="Jak przełączyć Azure subskrypcji do innego użytkownika, a niektóre często zadawane pytania (FAQ) dotyczące procesu"
   services=""
   documentationCenter=""
   authors="genlin"
   manager="stevenpo"
   editor=""
   tags="billing,top-support-issue"/>

<tags
   ms.service="billing"
   ms.workload="na"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="10/10/2016"
   ms.author="genli"/>

# <a name="transferring-ownership-of-an-azure-subscription"></a>Transfer praw własności subskrypcji usługi Azure

Jeśli nie ma:

- Musisz przekazać rozliczenia własności subskrypcję Azure do innej osoby?
- Chcesz zmienić konto używane do konta w usłudze Azure? Być może użyto Account Microsoft, ale przeznaczona do pracy za pomocą konta służbowego zamiast tego?
- Chcesz przenieść swoją subskrypcję Azure z jednego katalogu do innego?
- Azure i usługi Office 365 w różnych dzierżaw i chcesz skonsolidować?

Możesz teraz to zrobić łatwo w programie Microsoft Azure konta Centrum — repartycyjny, MSDN, Pack akcji lub BizSpark subskrypcji.  Dodaliśmy możliwość przekazywania subskrypcji do innego użytkownika. Innymi słowy możesz teraz zmienić administratorów konta w dowolnej repartycyjny, MSDN, pakiet akcji lub BizSpark subskrypcję, do której jesteś właścicielem, niezależnie od tego, które kraju działać w. Teraz obsługujemy transferu Azure Marketplace zakupów dla tych typów subskrypcji.

> [AZURE.NOTE] Aby zmienić subskrypcję na różnych oferty, zobacz [Przełączanie subskrypcji Azure na inną ofertę](billing-how-to-switch-azure-offer.md) , aby uzyskać więcej informacji. Jeśli potrzebujesz dodatkowej pomocy w dowolnym momencie, w tym artykule, szybko [kontakt z pomocą techniczną](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , aby uzyskać problemu rozwiązać, sprawdź.

## <a name="how-to-transfer-ownership-of-an-azure-subscription"></a>Jak przełączyć własności subskrypcji usługi Azure

> [AZURE.VIDEO transfer-an-azure-subscription]

1.  Zaloguj się w witrynie <https://account.windowsazure.com/Subscriptions>. Musisz być administratorem konta, aby przeprowadzić przeniesienia własności. Aby uzyskać więcej informacji na temat dowiedzieć się, kto jest administratorem konta subskrypcji zobacz [często zadawane pytania](#faq).

2.  Wybierz subskrypcję, aby przenieść.

3.  Kliknij opcję **Przełącz subskrypcji** .

    ![Karta subskrypcje konto Azure](./media/billing-subscription-transfer/image1.png)

4.  Postępuj zgodnie z instrukcjami, aby określić adresata.

    ![Przełączanie subskrypcji, okno dialogowe](./media/billing-subscription-transfer/image2.PNG)

5.  Adresat automatycznie otrzymają wiadomość e-mail z łączem do zatwierdzenia.

    ![Subskrypcja e-mail przełączania do adresata](./media/billing-subscription-transfer/image3.png)

6.  Odbiorca kliknie łącze i obserwuje instrukcji, łącznie z wprowadzania ich informacje o płatności.

    ![Pierwsza strona sieci web transfer subskrypcji](./media/billing-subscription-transfer/image4.png)

    ![Druga strona sieci web transfer subskrypcji](./media/billing-subscription-transfer/image5.png)

7. Powodzenia! Teraz jest przenoszona subskrypcji.

<a id="faq"></a>
## <a name="frequently-asked-questions-faq"></a>Często zadawane pytania

-   **Jak ustalić, kto jest administratorem konta subskrypcji?**

    Można sprawdzić, kto jest administratorem konta subskrypcji w następujący sposób:

    1. Zaloguj się do [portalu Azure](https://portal.azure.com).
    2. W menu Centrum wybierz **subskrypcję**.
    3. Wybierz subskrypcję, którą chcesz sprawdzić, a następnie wybierz pozycję **Ustawienia**.
    4. Wybierz pozycję **Właściwości**. Administrator konta subskrypcji będzie wyświetlany w polu **Konto administratora** .  

-   **Transfer subskrypcji powoduje w dowolnym przestojów?**

    Nie ma żadnego wpływu na usługę. To skutecznie anulowanie subskrypcji w obszarze bieżące konto administratora i tworzy nową koncie adresata, ale przypisuje źródłowych usług Azure nowej subskrypcji. Identyfikator subskrypcji nie zmienia się.

-   **Jak używać tego mechanizmu zmiany katalogu subskrypcji?**-   
    Azure subskrypcji zostanie utworzona w katalogu, należący do konta administratora. Tak aby zmienić katalog, po prostu Przełącz subskrypcji do konta użytkownika w katalogu docelowym. Po zakończeniu czynności, aby zaakceptować transfer tego użytkownika subskrypcji automatycznie przejdzie do katalogu docelowego.

-   **Jeśli przejąć własności rozliczeń za subskrypcję z innej organizacji są nadal będą mieć dostęp do moich zasobów?**

    Jeśli subskrypcja został przeniesiony do innej dzierżawy, użytkowników skojarzonych z poprzedniego dzierżawy utraci dostęp do subskrypcji. Nawet jeśli użytkownik nie jest administratorem usługi lub administrator Co już, może być nadal masz dostęp do subskrypcji przez inne mechanizmy zabezpieczeń. W tym:
    - Zarządzanie certyfikatów, które Udziel użytkownikowi praw administratora do subskrypcji zasobów. Aby uzyskać więcej informacji zobacz [Tworzenie i przekazywanie certyfikatu zarządzania Azure](https://msdn.microsoft.com/library/azure/gg551722.aspx)
    -   Klawisze dostępu dla usług, takich jak miejsca do magazynowania. Aby uzyskać więcej informacji zobacz [Wyświetlanie, kopiowanie i klawisze dostępu wyniku miejsca do magazynowania](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)
    -   Zdalny poświadczenia dostępu do usług, takich jak maszyn wirtualnych Azure

    Nie jest pełną listę. Adresat należy rozważyć aktualizowanie wszelkie hasła skojarzonego z usługą, jeśli są potrzebne ograniczyć dostęp do swoich zasobów. Większość zasobów można aktualizować w następujący sposób:

    1.   Przejdź do portalu Azure: [ *https://portal.azure.com*](https://portal.azure.com)

    2.    Kliknij pozycję Przeglądaj wszystkie -&gt; wszystkie zasoby

    3.    Wybierz zasób. Spowoduje to otwarcie karta zasobu.

    4.    W karta zasobu kliknij przycisk **Ustawienia**. W tym miejscu można wyświetlać i aktualizować istniejące hasła.


-   **Jeśli można przesłać subskrypcji w środku cyklu rozliczeniowego, czy cyklu adresatów płatności dla całego rozliczeń?**

    Nadawca jest odpowiedzialny za płatności dowolnego użycia zgłoszono do momentu zakończenie transferu. Adresat jest odpowiedzialny za zastosowania zgłoszone od momentu przeniesienia lub nowszym. Może istnieć kilka zastosowania nastąpiło przed przeniesieniem, które zgłoszono późniejsza. To zostaną uwzględnione w rachunku adresata.

-   **Czy odbiorca ma dostęp do użycia i historii rozliczeń?**

    W tym momencie tylko informacje czytelne dla adresata jest kwotą ostatniej faktury (lub bieżące saldo, jeśli subskrypcja została przeniesiona przed pierwszym rachunku został wygenerowany). Pozostałe zastosowania i historii rozliczeń nie są przekazywane z subskrypcją.

-   **Oferty mogą być zmieniane podczas przenoszenia?**

    Oferty pozostają takie same. Aby zmienić Twoją ofertę, musisz najpierw [kontakt z pomocą techniczną](http://go.microsoft.com/fwlink/?LinkID=619338).

-   **Czy można przesłać subskrypcji do konta użytkownika w innym kraju?**

    Nie, w tej chwili to nie jest obsługiwane. Konto użytkownika adresata musi być w tym samym kraju.

-   **Adresat służy mechanizm różnych płatności?**

    Wartość Tak. Istnieją pewne ograniczenia tutaj: teraz subskrypcję historię rachunków jest podzielić na dwa konta. Ale zaletą jest to, że możesz to zrobić, bez konieczności [kontakt z pomocą techniczną](http://go.microsoft.com/fwlink/?LinkID=619338).

-   **Metody płatności dotyczy problem po przeniesieniu Azure subskrypcję?**

    Aby zaakceptować termicznego subskrypcji, należy podać karty kredytowej lub podobnej metody płatności płatności za subskrypcję. Na przykład Robert przekaże Joanna subskrypcji, Anna akceptuje transferu Joanna musi także udostępnić metodę płatności, który osoba będzie używany do zapłacić za subskrypcję. Po zakończeniu transferu Robert już nie jest rozliczana według przetransferować on Joanna subskrypcji.

## <a name="next-steps-after-accepting-ownership-of-a-subscription"></a>Następne kroki po zaakceptowaniu własności subskrypcji

1. Teraz jesteś administratorem konta. Przejrzyj i zaktualizuj Administrator usługi i administratorów współpracujących. Zarządzanie administratorów w [portalu klasyczny Azure](https://manage.windowsazure.com) , przechodząc do pozycji ustawienia. Aby [uzyskać więcej informacji](http://go.microsoft.com/fwlink/?LinkID=533293).
2. Za pomocą kontrola dostępu oparta na rolach (RBAC) dla swojej subskrypcji i usług. Odwiedź [Azure portal](https://portal.azure.com) [Dowiedz się więcej o RBAC](http://go.microsoft.com/fwlink/?LinkID=544802)
3. Aktualizowanie poświadczeń skojarzonych z usługami tej subskrypcji. W tym:
    - Zarządzanie certyfikatów, które Udziel użytkownikowi praw administratora do zasobów subskrypcji. Aby uzyskać więcej informacji zobacz [Tworzenie i przekazywanie zarządzania certyfikat do Azure](https://msdn.microsoft.com/library/azure/gg551722.aspx)
    -   Klawisze dostępu dla usług, takich jak miejsca do magazynowania. Aby uzyskać więcej informacji zobacz [Wyświetlanie, kopiowanie i klawisze dostępu wyniku miejsca do magazynowania](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)
    -   Zdalny poświadczenia dostępu do usług, takich jak maszyn wirtualnych Azure
4. Aktualizowanie rozliczeń alertów dla tej subskrypcji, w [Centrum konto Azure](https://account.windowsazure.com/Subscriptions),[Aby uzyskać więcej informacji](http://go.microsoft.com/fwlink/?LinkID=533292)  
5.  Jeśli pracujesz z partnerem, warto rozważyć aktualizowanie identyfikator partnera w tej subskrypcji. Można to zrobić w [Centrum konto Azure](https://account.windowsazure.com/Subscriptions).

> [AZURE.NOTE] Jeśli nadal masz dalsze pytania, szybko [kontakt z pomocą techniczną](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , aby uzyskać problemu rozwiązać, sprawdź.
