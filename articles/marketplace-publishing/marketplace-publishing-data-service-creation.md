<properties
   pageTitle="Przewodnik tworzenia usługi danych dla Marketplace | Microsoft Azure"
   description="Szczegółowe informacje, jak tworzyć, certyfikowanie i wdrażanie usługi danych dla zakupu na Azure Marketplace."
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
      ms.date="08/26/2016"
      ms.author="hascipio; avikova" />

# <a name="data-service-publishing-guide-for-the-azure-marketplace"></a>Usługi danych publikowania — przewodnik dotyczący Azure Marketplace

>[AZURE.IMPORTANT] **W tej chwili firma Microsoft nie są już ułatwiającej rozpoczęcie korzystania wszelkie nowe wydawcy usługi danych. Nowy dataservices nie będzie uzyskiwanie zatwierdzany dla listy.** Jeśli chcesz opublikować na AppSource aplikacji biznesowych władz akredytacji bezpieczeństwa można znaleźć więcej informacji [w tym miejscu](https://appsource.microsoft.com/partners). Jeśli masz aplikacje IaaS lub chcesz opublikować na Azure Marketplace usługi Deweloper można znaleźć więcej informacji [w tym miejscu](https://azure.microsoft.com/marketplace/programs/certified/).

Po wykonaniu kroku 1, [Tworzenie konta i rejestracji](marketplace-publishing-accounts-creation-registration.md), możemy wskazówki, możesz za pomocą [zarówno ogólne](marketplace-publishing-pre-requisites.md) i [Wymagania techniczne](marketplace-publishing-data-service-creation-prerequisites.md) oferty usługi danych na Azure Marketplace. Teraz możemy przeprowadzi Cię przez etapy tworzenia oferty usługi danych w [Portal publikowania] [ link-pubportal] dla usługi Azure Marketplace.

## <a name="1---login-to-the-publishing-portal"></a>1. Zaloguj się do publikowania portalu.

Przejdź do [https://publish.windowsazure.com](https://publish.windowsazure.com. )

**Pierwszy czasu logowania do Portal publikowania za pomocą tego samego konta, z którym firmy sprzedawcy profilu została zarejestrowana w Centrum deweloperów.**  (Później możesz dodać dowolnego pracownika firmy jako administratorów współpracujących w portalu publikowania).

Kliknij w polu **Publikuj usług danych** w przypadku pierwszego logowania się do portalu publikowania.

## <a name="2---choose-data-services-in-the-navigation-menu-on-the-left-side"></a>2. w menu nawigacji po lewej stronie wybierz pozycję **Usług danych** .

  ![Rysunek](media/marketplace-publishing-data-service-creation/pubportal-main-nav.png)

## <a name="3---create-a-new-data-service"></a>3. Tworzenie nowej usługi danych

Wprowadź tytuł dla Twojej nowe dane usługi oferty, a następnie kliknij polecenie na "+" po prawej stronie.

  ![Rysunek](media/marketplace-publishing-data-service-creation/step-3.png)

## <a name="4---review-the-sub-menu-under-the-newly-created-data-service-in-the-navigation-menu"></a>4. Przejrzyj podmenu w obszarze nowo utworzonego usługi danych w menu nawigacji.

Kliknij kartę **Instruktaż** , a następnie przejrzeć wszystkie czynności niezbędnych do opublikowania prawidłowo usługi danych na Azure Marketplace.

> [AZURE.TIP] Możesz zawsze kliknij łącza na stronie "Instruktaż" lub użyć karty w podmenu oferty usługi danych po lewej stronie.

## <a name="5---create-a-new-plan"></a>5. Tworzenie nowego planu.

### <a name="offers-plans-transactions"></a>Transakcje ofert, planów.

Każdej oferty mogą mieć wiele planów, ale musi mieć co najmniej jeden (1) planu. Użytkownicy końcowi subskrybowanie Twoją ofertę ich Subskrybuj jednego planu oferty. Każdy plan określa, jak użytkownicy końcowi będą mogli używać tej usługi.

Obecnie Azure Marketplace obsługuje tylko subskrypcji miesięcznych transakcji podstawie modelu usług danych, to znaczy użytkownicy końcowi będą płacić opłaty miesięcznej według ceny określonego planu, który subskrybują i będzie mógł korzystać każdego miesiąca liczba transakcji zdefiniowane w planie.

Każdej transakcji, zazwyczaj definiowane jako liczby rekordów zwrócenie usługi danych na podstawie kwerendy wysyłane do usługi. Wartość domyślna to 100. Liczba transakcji zwracane dla każdej kwerendy będą liczby rekordów podzielona przez 100 i zaokrąglona w górę do najbliższej liczby całkowitej.

Odpowiada Azure Marketplace usługi warstwy monitorowanie (licznik) liczba transakcji zużywanej przez poszczególnych zapytań.

> [AZURE.IMPORTANT] Użytkownicy końcowi, które osiągnięto limit transakcji w miesiącu zostaną zablokowane z dalszego używania usługi do końca ich miesięcznego cyklu subskrypcji.

> Plan lub jednego z planów może ale nie musi zawierać dowolną liczbę transakcji.

### <a name="create-a-plan"></a>Tworzenie planu.
1. Polecenie **"+"** obok "Dodaj nowy plan".

2. Wybierz jedną z opcji: użycie **Nieograniczone** lub **ograniczone** do tego planu.  Jeśli ograniczone podać liczba transakcji plan pozwalają korzystać w ciągu miesiąca.

    ![Rysunek](media/marketplace-publishing-data-service-creation/step-5.1.png)  

    Portal publikowania również sugeruje "Identyfikator planu", który będzie używany do komunikowania się nazwa planu w interfejsie użytkownika dla użytkowników końcowych i też używane przez usługę rynku do identyfikowania planu. Jeśli chcesz, możesz zmienić "Plan identyfikator".

    > [AZURE.NOTE] "Plan identyfikator" muszą być unikatowe w zakresie każdej oferty. Jak wiele innych identyfikatory używane w planowanie Portal publikowania identyfikator zostanie zablokowana po pierwszego publikowania do produkcji, dzięki czemu nie będzie można zmienić tego identyfikatora.

3. Kliknij, aby zaakceptować wybór.

4. Następnie zostanie wyświetlony monit kilka odpowiedzi na dodatkowe pytania dotyczące nowo utworzony Plan.

    ![Rysunek](media/marketplace-publishing-data-service-creation/step-5.2.png)


|Pytanie|Znaczenie|
|----|----|
|**Ten Plan jest bezpłatna i dostępna na całym świecie?**|Możesz utworzyć plan całkowicie wolny z opłat. Jeśli jest tylko plan tej oferty — oznacza to, że publikujesz "Oferują bezpłatne" na rynku. Jeśli jest tylko dla jednej (z mało) planu, ten to zapewnia możliwość oferowania użytkowników końcowych, aby dowiedzieć się więcej na temat usługi z stosunkowo niewielką liczbą transakcji w miesiącu.  Jeśli odpowiedź brzmi "Tak", zostanie wyświetlony monit nie dalsze pytania.|

> [AZURE.NOTE] Użytkownicy końcowi zawsze można uaktualnić do płatnej planów.

|Pytanie|Znaczenie|
|----|----|
|**Czy jest dostępna bezpłatna wersja próbna?**|Można wybrać "Brak wersję próbną" wcale lub przekazać opcję, aby użyć Plan "Miesiąc". Aby użyć tej opcji zapewnienie użytkownikom końcowym możliwości opis korzyści wynikających z oferty bezpłatnie przez jeden miesiąc, takich jak wydawcy.|

> [AZURE.IMPORTANT] Użytkownicy końcowi tylko będzie można kupić w bezpłatnej wersji próbnej, jeśli nawiązał instrument płatności np karty kredytowej, licencja enterprise agreement.

> Po miesiąca bezpłatną wersję próbną usługi Azure Marketplace rozpocznie się ładowania klientów cena w dniu subskrypcji, chyba że klienta zainicjowane anulowania subskrypcji. Żadne specjalne powiadomienie będzie dostępna dla użytkowników końcowych.

|Pytanie|Znaczenie|
|----|----|
|**Ten plan wymaga kod promocyjny do zakupu?**| Wydawcy mają opcję, aby ograniczyć dostęp do ich plany usługi, dostarczając specjalne kodu o nazwie "Promocode" do określonych odbiorców. Tylko użytkownicy końcowi, które ten Promocode będą mogli subskrybować planu. Jeśli wybierzesz "Nie", następnie akceptujesz że każda osoba z regionu, gdzie oferta jest dostępna (zobacz [Przewodnik po zawartości marketingowych Marketplace](marketplace-publishing-push-to-staging.md) uzyskać więcej szczegółowych informacji) będą mogli subskrybować ten plan. Zostanie wyświetlony monit nie dalsze pytania.|
|**Także ukryć ten plan z każda osoba, która nie zawiera kod promocyjny prawidłowe?**|Jeśli odpowiedź na pytanie poprzedni "Tak" jest wydawcy ma odpowiednią opcję, aby całkowicie usunąć ten plan z wyświetlane w Interfejsie użytkownika witryny Marketplace. Oznacza to, klienci nie zostanie wyświetlony ten plan na stronie Szczegóły oferty. Użytkownicy końcowi, które otrzymają promocode do zakupu, będzie można subskrybować go przy użyciu tego promocode.|

## <a name="6---create-your-marketplace-marketing-content"></a>6. Tworzenie usługi rynku treści marketingowych
Jak podać wymagane informacje na kartach **Marketing, ceny, pomocy technicznej i kategorii** należy odwiedź [Marketplace marketingowych przewodnik zawartości](marketplace-publishing-push-to-staging.md) , który jest wspólny dla wszystkich artefaktów opublikowanych w galerii Azure Marketplace.  

## <a name="7---connect-your-offer-to-your-service-sql-azure-based-or-web-service-based"></a>7. nawiązać Twoją ofertę usługi (podstawie platformy SQL Azure lub usługi sieci Web, na podstawie).

Kliknij polecenie w podmenu **Usług danych** .

W górnej części strony zostanie wyświetlony monit o podanie oferty **Namespace**.  

  ![Rysunek](media/marketplace-publishing-data-service-creation/step-7.png)

Poniżej pytanie określi, jak wydawcy ma Uwidacznianie nowo utworzone oferty Azure Marketplace. (Aby uzyskać więcej informacji zobacz [Podręcznik techniczny wstępne usług danych](marketplace-publishing-data-service-creation-prerequisites.md)).

  ![Rysunek](media/marketplace-publishing-data-service-creation/step-7.2.png)

**Usługa bazie publikowania**

Kliknij w **bazie danych**. Powoduje wyświetlenie następnej strony:

  ![Rysunek](media/marketplace-publishing-data-service-creation/step-7.3.png)

Aby utworzyć mapowanie CSDL dla zestawu danych oparty na bazie danych SQL Azure:

  ![Rysunek](media/marketplace-publishing-data-service-creation/step-7.4.png)

A następnie dla każdej tabeli

  ![Rysunek](media/marketplace-publishing-data-service-creation/step-7.5.png)

  ![Rysunek](media/marketplace-publishing-data-service-creation/step-7.6.png)

Jeśli usługa sieci Web

  ![Rysunek](media/marketplace-publishing-data-service-creation/step-7.7.png)

> [AZURE.IMPORTANT] Aby uzyskać szczegółowe informacje i przykłady dotyczące tworzenia usługi sieci Web CSDL, przeczytaj [Mapowanie istniejącej usługi sieci web do OData za pośrednictwem CSDL](marketplace-publishing-data-service-creation-odata-mapping.md) .

## <a name="next-steps"></a>Następne kroki
Teraz, gdy został utworzony Twoją ofertę usługi danych, upewnij się, wykonać instrukcje dotyczące [Marketingowe przewodnik zawartości witryny Marketplace](marketplace-publishing-push-to-staging.md) , zanim przejście do [testowania usługi danych podczas przenoszenia](marketplace-publishing-data-service-test-in-staging.md).

## <a name="see-also"></a>Zobacz też
- [Wprowadzenie: Jak publikowanie ofertę Azure Marketplace](marketplace-publishing-getting-started.md)
- Jeśli interesuje Cię opis ogólny proces mapowania OData i przeznaczenie, w tym artykule [Mapowania OData usługi danych](marketplace-publishing-data-service-creation-odata-mapping.md) do przejrzenia definicje, struktur i instrukcje.
- Jeśli interesuje Cię nauki i opis określonych węzłów i ich parametrów w tym artykule [Węzłów mapowania danych usługi OData](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) definicje i objaśnienia, przykłady i używać kontekstu wielkość liter.
- Jeśli jesteś zrecenzować przykładów, przeczytaj ten artykuł [Przykłady mapowania danych usługi OData](marketplace-publishing-data-service-creation-odata-mapping-examples.md) , aby wyświetlić przykładowy kod i opis Składnia kodu i kontekst.


[link-pubportal]:https://publish.windowsazure.com
