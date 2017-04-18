<properties
   pageTitle="Zarządzanie obrazu maszyn wirtualnych na Azure Marketplace | Microsoft Azure"
   description="Szczegółowy przewodnik dotyczące zarządzania obrazu maszyn wirtualnych na Azure Marketplace po opublikowaniu początkowej."
   services="Azure Marketplace"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="Azure"
   ms.workload="na"
   ms.date="08/03/2016"
   ms.author="hascipio;"/>

# <a name="post-production-guide-for-virtual-machine-offers-in-the-azure-marketplace"></a>Przewodnik po produkcji dla oferty maszyn wirtualnych usługi Azure Marketplace

W tym artykule wyjaśniono, aktualizowania live oferty maszyn wirtualnych w Azure Marketplace. Również pozwala na dodawanie jednego lub więcej nowej wersji produktu do istniejącej oferty i usuwanie live oferty maszyn wirtualnych lub SKU z usługi Azure Marketplace.

Po oferty i SKU są umieszczane w [Azure Portal](http://portal.azure.com), nie można zmienić pola podanych poniżej:

- **Oferuje identyfikator:** [Portal publikowania -> maszyn wirtualnych -> Wybierz Twoją ofertę -> karta obrazów maszyn wirtualnych -> oferuje identyfikator]
- **Identyfikator SKU:** [Portal publikowania -> maszyn wirtualnych -> Wybierz -> Twoją ofertę wersji produktu -> karta Dodawanie SKU]
- **Namespace programu publisher:** [Portal publikowania -> maszyn wirtualnych -> Instruktaż Powiedz nam informacje o firmie (znaleziony w obszarze "Krok 2 zarejestrować Twoja firma") -> -> karta Namespace programu Publisher -> Namespace]

Po oferty i SKU jest wyświetlana w [Azure Marketplace](http://azure.microsoft.com/marketplace), nie można zmienić pola podanych poniżej:

- **Oferuje identyfikator:** [Portal publikowania -> maszyn wirtualnych -> Wybierz Twoją ofertę -> karta obrazów maszyn wirtualnych -> oferuje identyfikator]
- **Identyfikator SKU:** [Portal publikowania -> maszyn wirtualnych -> Wybierz -> Twoją ofertę wersji produktu -> karta Dodawanie SKU]
- **Namespace programu publisher:** [Portal publikowania -> maszyn wirtualnych -> Instruktaż -> karta Namespace programu Publisher Powiedz nam o Twoja firma (znaleziony w kroku 2 zarejestrować) -> Namespace]
- **Porty** [Portal publikowania -> maszyn wirtualnych -> Wybierz Twoją ofertę -> karta obrazów maszyn wirtualnych -> Otwórz porty]
- **Cennik Zmień wymienionych SKU(s)**
- **Zmiana modelu SKU(s) wymienionych rozliczenia**
- **Usunięcie rozliczenia regionów wymienionych SKU(s)**
- **Zmienianie dysku danych Liczba wymienionych SKU(s)**



## <a name="1-how-to-update-the-technical-details-of-a-sku"></a>1. jak zaktualizować szczegóły techniczne SKU

Można dodać nową wersję do wymienionych SKU, a następnie ponownie opublikować Twoją ofertę, wykonując kroki podane poniżej:

1. Zaloguj się do [portalu publikowania](https://publish.windowsazure.com).
2. Przejdź do karty **maszyn wirtualnych** i wybierz pozycję Twoją ofertę.
3. Z menu po lewej stronie kliknij kartę **Obrazów maszyn wirtualnych** .
4. W sekcji **wersji produktu** na karcie **Obrazów maszyn wirtualnych** Znajdź używaną WERSJĘ, którą chcesz zaktualizować.
5. Po wykonaniu tej Dodaj nowy numer wersji używaną WERSJĘ i kliknij przycisk **"+"** . Nowa wersja powinny być formatu X.Y.Z, gdzie X, Y Z są liczbami całkowitymi. Należy tylko przyrostowe zmiany wersji.
6. W polu **Adres URL wirtualnego dysku twardego OS** Dodawanie podpisu do udostępnienia, identyfikator URI utworzony przez system operacyjny wirtualnego dysku twardego i zapisać zmiany.

    >[AZURE.IMPORTANT] Możesz nie przyrost Zmniejsz liczbę dysku danych wymienionych SKU. Należy utworzyć w tym przypadku z nowej wersji produktu. Można znaleźć w sekcji [3. Dodawanie nowej wersji w obszarze wymienionych oferty](#3-how-to-add-a-new-sku-under-a-live-offer) szczegółowe wskazówki.

7. Po wprowadzeniu zmian, przejdź do karty **Publikowanie** i kliknij przycisk **PUSH do tymczasowego**. Szczegółowe wytyczne dotyczące testowanie Twoją ofertę w środowisku tymczasowy można znaleźć w tym [łącza](marketplace-publishing-vm-image-test-in-staging.md)
8. Po przetestowaniu Twoją ofertę podczas przenoszenia, przejdź do karty **PUBLIKUJ** w publikacji portal i kliknij przycisk **ŻĄDAJ zatwierdzenia do PUSH do produkcji** ponownie opublikować swoją ofertę w Azure Marketplace.

    ![Rysunek](media/marketplace-publishing-vm-image-post-publishing/img01_07.png)

## <a name="2-how-to-update-the-non-technical-details-of-an-offer-or-a-sku"></a>2. jak zaktualizować zarówno szczegółów oferty lub SKU

Możesz zaktualizować zarówno (działań marketingowych, prawne, pomocy technicznej, kategorie) szczegóły oferty live lub SKU w Azure Marketplace.

### <a name="21-update-the-offer-description-and-logos"></a>2.1 aktualizowanie ofertę opis i logo

Można zaktualizować szczegóły oferty i ponowne publikowanie Twoją ofertę, wykonując poniższe czynności:

1. Zaloguj się do [portalu publikowania](https://publish.windowsazure.com).
2. Przejdź do karty **maszyn wirtualnych** i wybierz pozycję Twoją ofertę.
3. Z menu po lewej stronie kliknij kartę **MARKETINGOWYCH** .
4. Kliknij przycisk **Języka ANGIELSKIEGO (Stany Zjednoczone)** .
5. W menu po lewej stronie kliknij kartę **Szczegóły** . Na karcie **Szczegóły** w sekcji *Opis* można zaktualizować tytuł oferty, oferują podsumowanie, oferują długie Podsumowanie i zapisać zmiany.

    >[AZURE.NOTE] Należy zwrócić uwagę z następujących podczas aktualizowania informacji SKU.
    **Nie wprowadzaj zduplikowany tekst w obszarze Opis oferty i opis SKU. Nie wprowadzaj zduplikowany tekst w obszarze tytuł SKU i ofertę długo podsumowania. Nie wprowadzaj zduplikowany tekst w obszarze tytuł SKU i Podsumowanie oferty.**

    ![Rysunek](media/marketplace-publishing-vm-image-post-publishing/img02.1_05.png)

6. W sekcji *logo* na kartę **Szczegóły** możesz zaktualizować logo. Jednak upewnij się, że logo postępuj zgodnie z [zaleceniami dotyczącymi usługi Azure Marketplace](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content) (zajrzyj do sekcji Krok 1: Podaj Marketplace marketingowych zawartości -> Szczegóły -> Azure Marketplace Logo wskazówki).

    >[AZURE.NOTE] Ikona główny jest opcjonalna. Użytkownik może nie ikona główny przekazywania. Po przesłaniu ikona główny następnie istnieje jednak zapewnienie go usunąć z publikowania portalu. W takim przypadku należy wykonać [wskazówki ikona główny](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content) (zajrzyj do sekcji Krok 1: Podaj Marketplace marketingowych zawartości -> Szczegóły -> dodatkowe wskazówki dotyczące transparent logo główny).

7. Po wprowadzeniu zmian, przejdź do karty **Publikowanie** i kliknij przycisk **PUSH do tymczasowego**. Szczegółowe wskazówki dotyczące testowanie Twoją ofertę w środowisku tymczasowy zajrzyj do tego [łącza](marketplace-publishing-vm-image-test-in-staging.md).
8. Po przetestowaniu Twoją ofertę podczas przenoszenia, przejdź do karty **PUBLIKUJ** w publikacji portal i kliknij przycisk **ŻĄDAJ zatwierdzenia do PUSH do produkcji** ponownie opublikować swoją ofertę w Azure Marketplace.

    ![Rysunek](media/marketplace-publishing-vm-image-post-publishing/img02.1_08.png)

### <a name="22-update-the-sku-description"></a>2.2. Zaktualizuj opis SKU

Można zaktualizować szczegóły SKU i ponownie opublikować Twoją ofertę, wykonując poniższe czynności:

1. Zaloguj się do [portalu publikowania](https://publish.windowsazure.com)
2. Przejdź do karty **maszyn wirtualnych** i wybierz pozycję Twoją ofertę.
3. Z menu po lewej stronie kliknij kartę **MARKETINGOWYCH** .
4. Kliknij przycisk **Języka ANGIELSKIEGO (Stany Zjednoczone)** .
5. Z menu po lewej stronie kliknij kartę **planów** . Na karcie **Plany** w sekcji *wersji produktu* można zaktualizować tytuł SKU, SKU Podsumowanie i szczegóły opis SKU i zapisać zmiany.

    >[AZURE.NOTE] Należy zwrócić uwagę z następujących podczas aktualizowania informacji SKU. **Nie wprowadzaj zduplikowany tekst w obszarze Opis oferty i opis SKU. Nie wprowadzaj zduplikowany tekst w obszarze tytuł SKU i długi Podsumowanie oferty. Nie wprowadzaj zduplikowany tekst w obszarze tytuł SKU i Podsumowanie oferty.**

6. Po wprowadzeniu zmian, przejdź do karty **Publikowanie** i kliknij przycisk **PUSH do tymczasowego**. Szczegółowe wskazówki dotyczące testowanie Twoją ofertę w środowisku tymczasowy można znaleźć do tego łącza
7. Po przetestowaniu Twoją ofertę podczas przenoszenia, przejdź do karty **PUBLIKUJ** w publikacji portal i kliknij przycisk **ŻĄDAJ zatwierdzenia do PUSH do produkcji** ponownie opublikować swoją ofertę w Azure Marketplace.

    ![Rysunek](media/marketplace-publishing-vm-image-post-publishing/img02.2_07.png)

### <a name="23-change-the-existing-links-or-add-new-links"></a>2.3 zmienić istniejące łącza lub dodać nowe łącza

Możesz zmienić istniejące łącza lub dodać nowe łącza, a następnie ponownie opublikować Twoją ofertę, wykonując poniższe czynności:

1. Zaloguj się do [portalu publikowania](https://publish.windowsazure.com)
2. Przejdź do karty **maszyn wirtualnych** i wybierz pozycję Twoją ofertę.
3. Z menu po lewej stronie kliknij kartę **MARKETINGOWYCH** .
4. Kliknij przycisk **Języka ANGIELSKIEGO (Stany Zjednoczone)** .
5. Z menu po lewej stronie kliknij kartę **łącza** .
6. Jeśli chcesz dodać nowe łącze, następnie w obszarze *łącza* sekcji kliknij przycisk **Dodaj** . Zostanie otwarte okno dialogowe *"Dodaj łącze"* . W tym oknie dialogowym możesz dodać łącze tytuł i adres URL pola i zapisać zmiany. Możesz wprowadzić dowolne łącze, zawierający informacje, które mogą pomóc klientów.
7. Jeśli chcesz zaktualizować lub usunąć istniejące łącze, a następnie wybierz odpowiednie łącze i kliknij przycisk Edytuj lub usuń odpowiednio.

    >[AZURE.NOTE] Upewnij się, że łącza, które zostały wprowadzone w tej sekcji działają poprawnie, te łącza uzyskiwanie zatwierdzonych podczas procesu produkcji wezwanie.

8. Po wprowadzeniu zmian, przejdź do karty **Publikowanie** i kliknij przycisk **PUSH do tymczasowego**. Szczegółowe wskazówki dotyczące testowanie Twoją ofertę w środowisku tymczasowy zajrzyj do tego [łącza](marketplace-publishing-vm-image-test-in-staging.md).
9. Po przetestowaniu Twoją ofertę podczas przenoszenia, przejdź do karty **PUBLIKUJ** w publikacji portal i kliknij przycisk **ŻĄDAJ zatwierdzenia do PUSH do produkcji** ponownie opublikować swoją ofertę w Azure Marketplace.

    ![Rysunek](media/marketplace-publishing-vm-image-post-publishing/img02.3_09-01.png)

    ![Rysunek](media/marketplace-publishing-vm-image-post-publishing/img02.3-2.png)

### <a name="24-change-an-existing-sample-image-or-add-a-new-sample-image"></a>2,4 zmienić istniejący obraz próbki lub dodać nowe przykładowy obraz

Możesz zmienić istniejące przykładowe obrazy lub dodać nowe przykładowe obrazy, a następnie ponownie opublikować Twoją ofertę, wykonując poniższe czynności:

>[AZURE.NOTE] Przykładowy tylko jeden obraz jest wyświetlany w [https://portal.azure.com](https://portal.azure.com).

1. Zaloguj się do [portalu publikowania](https://publish.windowsazure.com)
2. Przejdź do karty **maszyn wirtualnych** i wybierz pozycję Twoją ofertę.
3. Z menu po lewej stronie kliknij kartę **MARKETINGOWYCH** .
4. Kliknij przycisk **Języka ANGIELSKIEGO (Stany Zjednoczone)** .
5. Z menu po lewej stronie kliknij kartę **Przykładowe obrazy** .
6. Jeśli chcesz dodać nowy obraz próbki, następnie w sekcji *Przykładowe obrazy* kliknij przycisk **PRZEKAŻ nowy obraz** , a następnie zapisz zmiany.

    >[AZURE.NOTE] Przykładowy obraz w tym jest opcjonalny.

7. Jeśli chcesz zaktualizować lub usunąć istniejącego obrazu próbki, zlokalizuj przykładowy odpowiedni obraz i kliknij przycisk **ZAMIEŃ obraz** lub usuń odpowiednio.

8. Po wprowadzeniu zmian, przejdź do karty **Publikowanie** i kliknij przycisk **PUSH do tymczasowego**. Szczegółowe wskazówki dotyczące testowanie Twoją ofertę w środowisku tymczasowy zajrzyj do tego [łącza](marketplace-publishing-vm-image-test-in-staging.md).
9. Po przetestowaniu Twoją ofertę podczas przenoszenia, przejdź do karty **PUBLIKUJ** w publikacji portal i kliknij przycisk **ŻĄDAJ zatwierdzenia do PUSH do produkcji** ponownie opublikować swoją ofertę w Azure Marketplace.

    ![Rysunek](media/marketplace-publishing-vm-image-post-publishing/img02.4_09.png)

### <a name="25-update-the-legal-content"></a>2,5 aktualizacji zawartości prawnej

Można zaktualizować zawartości prawnej i ponowne publikowanie Twoją ofertę, wykonując poniższe czynności:

1. Zaloguj się do [portalu publikowania](https://publish.windowsazure.com)
2. Przejdź do karty **maszyn wirtualnych** i wybierz pozycję Twoją ofertę.
3. Z menu po lewej stronie kliknij kartę **MARKETINGOWYCH** .
4. Kliknij przycisk **Języka ANGIELSKIEGO (Stany Zjednoczone)** .
5. Z menu po lewej stronie kliknij kartę **prawne** . W sekcji *prawne* możesz zaktualizować usługi zasady i warunki użytkowania. Wprowadź lub Wklej zasad i warunków w polu tekstowym *Warunki użytkowania* i zapisać zmiany.
6. Limit liczby znaków dla prawne warunki użytkowania to 1 000 000 znaków.
7. Po wprowadzeniu zmian, przejdź do karty **Publikowanie** i kliknij przycisk **PUSH do tymczasowego**. Szczegółowe wytyczne dotyczące testowanie Twoją ofertę w środowisku tymczasowy można znaleźć w tym [łącza](marketplace-publishing-vm-image-test-in-staging.md)
8. Po przetestowaniu Twoją ofertę podczas przenoszenia, przejdź do karty **PUBLIKUJ** w publikacji portal i kliknij przycisk **ŻĄDAJ zatwierdzenia do PUSH do produkcji** ponownie opublikować swoją ofertę w Azure Marketplace.

    ![Rysunek](media/marketplace-publishing-vm-image-post-publishing/img02.5_08.png)

### <a name="26-update-the-support-information"></a>2.6 aktualizowanie informacji o pomocy technicznej

Można zaktualizować informacje pomocy technicznej i ponowne publikowanie Twoją ofertę, wykonując poniższe czynności:

1. Zaloguj się do [portalu publikowania](https://publish.windowsazure.com)
2. Przejdź do karty **maszyn wirtualnych** i wybierz pozycję Twoją ofertę.
3. Z menu po lewej stronie kliknij kartę **Obsługa** .
4. W sekcji *Kontakt konstrukcyjną* kartę **Obsługa** możesz zaktualizować dane kontaktowe. Informacje te są używane tylko do komunikacji wewnętrznej między firmy Microsoft i partnera.
5. Na karcie **pomocy technicznej** , w sekcji *Obsługi klienta* można aktualizować dane kontaktowe pomocy technicznej, takich jak **Nazwa, adres E-mail, telefonu** i **Adres URL pomocy technicznej**. Informacje te są używane tylko do komunikacji wewnętrznej między firmy Microsoft i partnera.

    >[AZURE.NOTE] Jeśli chcesz podać tylko pomoc techniczna e-mail Podaj numer telefonu fikcyjna w sekcji **Pomocy technicznej** . W tym przypadku dostarczonych poczty e-mail zostanie użyty.

6. Po wprowadzeniu zmian, przejdź do karty **Publikowanie** i kliknij przycisk **PUSH do tymczasowego**. Szczegółowe wytyczne dotyczące testowanie Twoją ofertę w środowisku tymczasowy można znaleźć w tym [łącza](marketplace-publishing-vm-image-test-in-staging.md)
7. Po przetestowaniu Twoją ofertę podczas przenoszenia, przejdź do karty **PUBLIKUJ** w publikacji portal i kliknij przycisk **ŻĄDAJ zatwierdzenia do PUSH do produkcji** ponownie opublikować swoją ofertę w Azure Marketplace.

    ![Rysunek](media/marketplace-publishing-vm-image-post-publishing/img02.6_07.png)

### <a name="27-update-the-categories"></a>2.7 aktualizowanie kategorii

Można zaktualizować sekcji kategorie dla Twoją ofertę i ponownie opublikować Twoją ofertę, wykonując poniższe czynności:

1. Zaloguj się do [portalu publikowania](https://publish.windowsazure.com)
2. Przejdź do karty **maszyn wirtualnych** i wybierz pozycję Twoją ofertę.
3. Z menu po lewej stronie kliknij kartę **Kategorie** .
4. W sekcji *Kategorie* można zaktualizować kategorie dla Twoją ofertę i zapisać zmiany. Możesz wybrać maksymalnie pięciu kategorii dla galerii Azure Marketplace.
5. Po wprowadzeniu zmian, przejdź do karty **Publikowanie** i kliknij przycisk **PUSH do tymczasowego**. Szczegółowe wytyczne dotyczące testowanie Twoją ofertę w środowisku tymczasowy można znaleźć w tym [łącza](marketplace-publishing-vm-image-test-in-staging.md)
6. Po przetestowaniu Twoją ofertę podczas przenoszenia, przejdź do karty **PUBLIKUJ** w publikacji portal i kliknij przycisk **ŻĄDAJ zatwierdzenia do PUSH do produkcji** ponownie opublikować swoją ofertę w Azure Marketplace.

    ![Rysunek](media/marketplace-publishing-vm-image-post-publishing/img02.7_06.png)

## <a name="3-how-to-add-a-new-sku-under-a-listed-offer"></a>3. jak dodawać nowe SKU w obszarze wymienionych oferty

Możesz dodać nowej wersji w obszarze Twoją ofertę live, wykonując kroki podane poniżej:

1. Zaloguj się do [portalu publikowania](https://publish.windowsazure.com).
2. Przejdź do karty **maszyn wirtualnych** i wybierz pozycję Twoją ofertę.
3. Z menu po lewej stronie kliknij kartę **wersji produktu** . Po tym kliknij przycisk **Dodaj A SKU**.  Zostanie otwarte nowe okno dialogowe. Wprowadź identyfikator SKU rozpoczynających się małą literą. Zaznacz pole wyboru wyświetlić swój własne model(BYOL) rozliczeń, jeśli chcesz opublikować nowy SKU z modelem rozliczeń BYOL. W przeciwnym razie usuń zaznaczenie pola wyboru dla BYOL. Po kliknie znaczników w oknie dialogowym Tworzenie nowej wersji produktu. Jeśli nie wybierzesz modelu rozliczeń BYOL dla nowej wersji produktu, następnie modelu rozliczeń będzie automatycznie wynosić co godzina dla nowej wersji produktu. Jeśli chcesz włączyć bezpłatną wersję próbną 30days godzinowe modelu rozliczeń, następnie kliknij opcję "Miesiąc" "bezpłatną wersję próbną dostępna jest?". W przeciwnym razie wybierz pozycję "Brak wersję PRÓBNĄ". [Uwaga: opcja "jest bezpłatna wersja próbna dostępne?" jest wyświetlana tylko jeśli nie wybrano BYOL w oknie dialogowym podczas tworzenia nowego SKU.]

    >[AZURE.IMPORTANT] Wybrana opcja "Ukryj ten SKU z witryny Marketplace, ponieważ zawsze powinna zostać zakupione za pomocą szablonu rozwiązanie" powinny być oznaczone jako "Tak" tylko wtedy, gdy są zatwierdzone do publikowania oferty szablonu rozwiązanie w Azure Marketplace. W przeciwnym razie ta opcja zawsze ma zostać oznaczona jako "Nie".

4. Teraz z menu po lewej stronie, kliknij kartę **Obrazów maszyn wirtualnych** i Dowiedz się, nowej wersji, które zostały utworzone.
5. Aby skonfigurować nowy SKU, zapoznaj się z KROKU 5 tego [łącza](marketplace-publishing-vm-image-creation.md#5-obtain-certification-for-your-vm-image) orientacji.
6. Aby dodać materiałów marketingowych dla nowej wersji produktu, zajrzyj do sekcji Krok 1: Podaj Marketplace marketingowych zawartości -> Szczegóły -> liczby 2 do 5 tego [łącza](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).
7. Aby dodać cennik informacje dla nowej wersji produktu, zapoznaj się z sekcją 2.1. Ustawianie cenach maszyn wirtualnych tego [łącza](marketplace-publishing-push-to-staging.md#step-2-set-your-prices)
8. Po wprowadzeniu zmian, przejdź do karty **Publikowanie** i kliknij przycisk **PUSH do tymczasowego**. Szczegółowe wytyczne dotyczące testowanie Twoją ofertę w środowisku tymczasowy można znaleźć w tym [łącza](marketplace-publishing-vm-image-test-in-staging.md)
9. Po przetestowaniu Twoją ofertę podczas przenoszenia, przejdź do karty **PUBLIKUJ** w publikacji portal i kliknij przycisk **ŻĄDAJ zatwierdzenia do PUSH do produkcji** ponownie opublikować swoją ofertę w Azure Marketplace.

    ![Rysunek](media/marketplace-publishing-vm-image-post-publishing/img03_09-01.png)

    ![Rysunek](media/marketplace-publishing-vm-image-post-publishing/img03_09-02.png)

## <a name="4-how-to-change-the-data-disk-count-for-a-listed-sku"></a>4. sposobu zmieniania danych Liczba dysków wymienionych SKU

Możesz nie przyrost Zmniejsz liczbę dysku danych wymienionych SKU. W tym przypadku należy utworzyć nowej wersji. Można znaleźć w sekcji [3. Dodawanie nowej wersji w obszarze ofertę live](#3-how-to-add-a-new-sku-under-a-live-offer) szczegółowe wskazówki.

## <a name="5---how-to-delete-a-listed-offer-from-the-azure-marketplace"></a>5. jak można usunąć ofertę wymienionych z usługi Azure Marketplace

Istnieją różne aspekty, które należy można wybrać ofertę w przypadku żądanie, aby usunąć live oferty. Wykonaj poniższe czynności, aby uzyskać wskazówki z zespołem pomocy technicznej, aby usunąć wymienionych oferty z usługi Azure Marketplace:

1.  Podnoszenie bilet pomocy technicznej, przy użyciu tego [łącza](https://support.microsoft.com/en-us/getsupport?wf=0&tenant=ClassicCommercial&oaspworkflow=start_1.0.0.0&locale=en-us&supportregion=en-us&pesid=15635&ccsid=635993707583706681)
2.  Wybierz typ problemu jako **"Zarządzanie ofert"** i wybierz kategorię jako **"Modyfikowanie oferty i/lub SKU już w produkcji"**
3.  Przesyłanie żądania

Zespół pomocy technicznej przeprowadzi Cię przez proces usuwania oferty i SKU.

>[AZURE.NOTE] Oferty można usunąć zawsze, gdy jest on w stanie Wersja robocza (to znaczy nie przemieszczania i produkcji), klikając przycisk **Odrzuć wersję ROBOCZĄ** na karcie **Historia** .

## <a name="6-how-to-delete-a-listed-sku-from-the-azure-marketplace"></a>6. sposobu usuwania wymienionych SKU z Azure Marketplace

Możesz usunąć wymienionych SKU z usługi Azure Marketplace, wykonując kroki podane poniżej:

1. Zaloguj się do [portalu publikowania](https://publish.windowsazure.com).
2. Przejdź do karty **maszyn wirtualnych** i wybierz pozycję Twoją ofertę.
3. W okienku po lewej stronie kliknij kartę **wersji produktu** .
4. Wybierz używaną WERSJĘ, którą chcesz usunąć, a następnie kliknij przycisk Usuń przed tej wersji produktu.
5. Po zakończeniu przejdź na kartę Publikowanie publikowania portal i kliknij przycisk **ŻĄDAJ zatwierdzenia do PUSH do produkcji** ponowne publikowanie oferty w Azure Marketplace.
6. Gdy oferty otrzymuje ponownie opublikowane w Azure Marketplace, używaną WERSJĘ zostaną usunięte z usługi Azure Marketplace i Azure Portal.

## <a name="7-how-to-delete-the-current-version-of-a-listed-sku-from-the-azure-marketplace"></a>7. jak można usunąć bieżącej wersji SKU wymienionych z usługi Azure Marketplace

Możesz usunąć bieżącej wersji SKU wymienionych z Azure Marketplace, wykonując kroki opisane poniżej. Po ukończeniu procesu używaną WERSJĘ zostanie wycofana poprzedniej wersji.

1. Zaloguj się do [portalu publikowania](https://publish.windowsazure.com).
2.  Przejdź do karty **maszyn wirtualnych** i wybierz pozycję Twoją ofertę.
3.  W okienku po lewej stronie kliknij kartę **Obrazów maszyn wirtualnych** .
4.  Wybierz pozycję SKU którego bieżącą wersję chcesz usunąć, a następnie kliknij przycisk Usuń przed tej wersji.
5.  Po zakończeniu przejdź na kartę **Publikowanie** publikowania portal i kliknij przycisk **ŻĄDAJ zatwierdzenia do PUSH do produkcji** ponowne publikowanie oferty w Azure Marketplace.
6.  Po ofertę otrzymuje ponownie opublikowane w Azure Marketplace, bieżącej wersji SKU wymienionych zostaną usunięte z usługi Azure Marketplace i Azure Portal. Jednostka SKU zostaną wycofane poprzedniej wersji.

## <a name="8-how-to-revert-listing-price-to-production-values"></a>8. jak cofnąć cena listę wartości produkcji
Zostały zmienione ceny wymienionych SKU (lub usunięte rozliczeń regionów wymienionych wersji). Ponieważ nie jest obsługiwane w Azure Marketplace, chcę cofnąć wprowadzone zmiany wartości produkcji. Jak uzyskać, który?

Wykonaj kroki podane poniżej:

1. Zaloguj się do [portalu publikowania](https://publish.windowsazure.com).
2. Przejdź do karty **maszyn wirtualnych** i wybierz pozycję Twoją ofertę.
3. Z menu po lewej stronie kliknij kartę **ceny** .
4. Na karcie ceny zaznacz obszar, którego cennik chcesz zresetować.

    ![Rysunek](media/marketplace-publishing-vm-image-post-publishing/img08-04.png)

5. W przypadku wersji produktu z godzinowe modelu rozliczeń Zresetuj ceny dla wszystkich rdzeni są produkcji dla wybranego regionu. Dla wersji produktu z modelem rozliczeń BYOL, udostępnić używaną WERSJĘ w regionie, zaznaczając pole wyboru przed SKU w sekcji dostępność SKU EXTERNALLY-LICENSED (BYOL) (zobacz zrzut ekranu poniżej).

    ![Rysunek](media/marketplace-publishing-vm-image-post-publishing/img08-05.png)

6. Kliknij przycisk **AUTOPRICE innych rynkach oparte na ceny w Zjednoczone Stany**.

    >[AZURE.NOTE] Etykieta przycisku mogą się różnić w zależności od regionu, która jest zaznaczona. Ponieważ Wybraliśmy Stanów Zjednoczonych podczas tworzenia tego dokumentu, więc przycisk jest oznaczona jako "Automatyczne cena rynkach na podstawie cen w Stanach Zjednoczonych" poniżej ekranu.

    ![Rysunek](media/marketplace-publishing-vm-image-post-publishing/img08-06.png)

7. Zostanie otwarty Kreator cena automatyczne. Pierwsza strona wyświetla zaznaczenia dla rynku podstawowej. Tworzenie sekcji i przejdź do następnej strony, klikając przycisk **"->"** .

    ![Rysunek](media/marketplace-publishing-vm-image-post-publishing/img08-07.png)

8. Opcja do wyboru rdzenie i plany będą wyświetlane na stronie 2. Wybierz odpowiednie plany i rdzenie, a następnie kliknij pozycję "->" przycisk.

    ![Rysunek](media/marketplace-publishing-vm-image-post-publishing/img08-08.png)

9. Strona 3 wyświetla rynkach i regionów. Kliknij przycisk Przełącz wszystko, aby zaznaczyć wszystkie regiony lub ręcznie zaznacz pola obok obszaru. Kliknij przycisk "->", aby przejść do następnej strony.

    ![Rysunek](media/marketplace-publishing-vm-image-post-publishing/img08-09.png)

10. Strona 4 Wyświetla kursach. Kliknij przycisk Zakończ, aby wykonać kroki. Kreator zostaną zresetowane ceny według zaznaczenia.

11. Teraz przejdź do karty cennik i kliknij przycisk "WIDOKU podsumowania i zmian".
Wybierz pozycję "Wersja robocza" w sekcji "Widok wersji" i "Produkcji" w sekcji "Porównaj z" (zobacz zrzut ekranu poniżej). Jeśli widzisz żadnej różnicy cennik, oznacza to, że ceny został zmieniony z powrotem do wartości produkcji pomyślnie.

    ![Rysunek](media/marketplace-publishing-vm-image-post-publishing/img08-11.png)

12. Po wprowadzeniu zmian, przejdź do karty Publikowanie i kliknij przycisk **PUSH do tymczasowego**. Szczegółowe wytyczne dotyczące testowanie Twoją ofertę w środowisku tymczasowy można znaleźć w tym [łącza](marketplace-publishing-vm-image-test-in-staging.md)
13. Po przetestowaniu Twoją ofertę podczas przenoszenia, przejdź do karty PUBLIKUJ w publikacji portal i kliknij przycisk **ŻĄDAJ zatwierdzenia do PUSH do produkcji** ponownie opublikować swoją ofertę w Azure Marketplace.

## <a name="9-how-to-revert-billing-model-to-production-values"></a>9. jak cofnąć rozliczeń modelu wartości produkcji
Zostały zmienione modelu rozliczeń wymienionych SKU Ponieważ nie jest obsługiwane w Azure Marketplace, chcę cofnąć wprowadzone zmiany wartości produkcji. Jak uzyskać, który?

Wykonaj poniższe czynności:

1. Zaloguj się do [portalu publikowania](https://publish.windowsazure.com).
2. Przejdź do karty **maszyn wirtualnych** i wybierz pozycję Twoją ofertę.
3. Z menu po lewej stronie kliknij kartę **wersji produktu** .
4. Kliknij przycisk Edytuj, aby cofnąć rozliczeń modelu. Zostanie otwarte okno. Zaznacz lub wyczyść pole wyboru pole wyboru **"rozliczenia i licencjonowanie zakończeniu zewnętrznie z platformy Azure (czyli Przesuń swój własny licencji)"** .

    ![Rysunek](media/marketplace-publishing-vm-image-post-publishing/img09-04.png)

5. Raz można znaleźć odpowiedzi na pytanie 8 ten dokument, aby powrócić ceny.
6. Po której przejdź do karty **PUBLIKUJ** w publikacji portal i wypychanych ofertę do przemieszczania należy je przetestować. Po zakończeniu badania oferty, następnie kliknij przycisk **Żądanie zatwierdzenia do PUSH do produkcji** ponownie opublikować swoją ofertę w Azure Marketplace.

## <a name="10-how-to-revert-visibility-setting-of-a-listed-sku-to-the-production-value"></a>10. jak przywrócić ustawienia widoczności dla podanych SKU wartość produkcji

Wykonaj poniższe czynności:

1. Zaloguj się do [portalu publikowania](https://publish.windowsazure.com).
2. Przejdź do karty **maszyn wirtualnych** i wybierz pozycję Twoją ofertę.
3. Z menu po lewej stronie kliknij kartę **wersji produktu** .
4. Wybierz swojego SKU i przywrócić ustawienia widoczności dla do wartości produkcji wersji.

    ![Rysunek](media/marketplace-publishing-vm-image-post-publishing/img10-04.png)

5. Po zakończeniu zmiany, a następnie na przycisku **Żądanie zatwierdzenia do PUSH do produkcji** ponownie opublikować swoją ofertę w Azure Marketplace.

## <a name="see-also"></a>Zobacz też
- [Wprowadzenie: Jak publikowanie ofertę Azure Marketplace](marketplace-publishing-getting-started.md)
- [Opis wniosków ze sprzedawcą raportowania](marketplace-publishing-report-seller-insights.md)
- [Opis wypłata raportowania](marketplace-publishing-report-payout.md)
- [Jak zmienić chęć odsprzedawcy swojego dostawcy rozwiązań chmury](marketplace-publishing-csp-incentive.md)
- [Rozwiązywanie typowych problemów publikowania na rynku](marketplace-publishing-support-common-issues.md)
- [Uzyskiwanie pomocy technicznej jako wydawca](marketplace-publishing-get-publisher-support.md)
- [Tworzenie obrazu maszyn wirtualnych lokalnego](marketplace-publishing-vm-image-creation-on-premise.md)
- [Tworzenie wirtualnych komputera z systemem Windows w portal Azure preview](../virtual-machines/virtual-machines-windows-hero-tutorial.md)
