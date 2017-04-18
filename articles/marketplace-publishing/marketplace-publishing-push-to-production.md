<properties
   pageTitle="Wdrażanie Twoją ofertę Azure Marketplace | Microsoft Azure"
   description="Więcej informacji na temat i szczegółową z instrukcjami, aby wdrożyć Twoją ofertę — obraz maszyn wirtualnych, usługa Deweloper, Usługa danych itp. — usługą Azure Marketplace."
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
   ms.date="08/02/2016"
   ms.author="hascipio" />

# <a name="deploy-your-offer-to-the-azure-marketplace"></a>Wdrażanie Twoją ofertę Azure Marketplace
Po zakończeniu Twoją ofertę (czyli przetestowaniu scenariuszy, działań marketingowych zawartości, itp.) i chcesz uruchomić, poproś **Push produkcji** na karcie **Publikowanie** .  

1. Cztery kroki opisane w temacie INSTRUKTAŻ strony publikowania portalu powinny być ukończone i zielony. Dla maszyn wirtualnych ofert upewnij się, przestrzeganie tych wskazówek.

    ![Rysunek][img-pubportal-walkthru-checked]

2. Wybierz kartę **Publikowanie** z listy po lewej stronie.

    ![Rysunek][img-pubportal-menu-publish]

3. Kliknij przycisk **Żądaj zatwierdzenia do produkcji**. Po wniosku, zespołu zatwierdzenia wykonuje końcowego przeglądu, a następnie Twoją ofertę będą dostępne w Azure Marketplace.

    ![Rysunek][img-pubportal-publish-pushproduction]

>[AZURE.IMPORTANT] W przypadku maszyn wirtualnych po kliknięciu przycisku Zatwierdzanie żądania do produkcji, następujące czynności są wykonywane za sceny. Można wyświetlić postęp każdego kroku na karcie PUBLIKUJ w publikacji portalu. Należy sprawdzić tę stronę w regularnych interwałach (do momentu stan jest wyświetlany stan "Wymienione") o podanie informacji błąd, które wymagają korekty do zakończenia.

> - Na początku żądania produkcji przechodzi do zespołu certyfikacji, do którego sprawdzanie poprawności wirtualnego dysku twardego. Jednak Jeśli aktualizujesz Twoją ofertę już wymienionych i żądania otrzymano tylko marketingowe, zmienianie, następnie certyfikacji krok jest pomijany.
> - W następnym kroku żądania wejścia do zespołu poprawności zawartości, do którego weryfikację treści marketingowych oferty.
> - W przypadku pomyślnego powyższe czynności oferta jest zatwierdzony w produkcji. W tej chwili stan stają się "liście" portal publikowania. Jednak ten status "Wymienione" nie oznacza, że proces zostanie zakończony. Poniższe czynności muszą być ukończone, zanim oferta jest dostępna w Azure Marketplace.
> - Po zaakceptowaniu oferty produkcji w poprzednim kroku, replikacja oferty rozpocząć przez wszystkich Azure centrach danych. Zwykle trwa 24-48hours na ukończenie replikacji ale może potrwać do tygodnia w zależności od rozmiaru wirtualnego dysku twardego. Jeśli chcesz zaktualizować Twoją ofertę już wymienionych i otrzymano tylko marketingowe, zmienianie, następnie replikacji jest jednak szybsze.
> - Po zakończeniu replikacji następnie oferty będą dostępne w Azure Marketplace.

> Możesz usunąć oferty zawsze, gdy jest on w stanie **Wersja robocza** , (to znaczy nigdy nie **przekazać do przemieszczania** lub **Naciśnij, aby produkcji**). Na karcie **historii** kliknij przycisk **Odrzuć wersję roboczą** w dolnej części strony, aby usunąć wersję roboczą.


## <a name="production-checklist-for-all-virtual-machine-offers"></a>Lista kontrolna produkcji wszystkie oferty maszyn wirtualnych

- Upewnij się, że są partnera firmy Microsoft Azure certyfikowane
- Na karcie SKU opcję "Ukryj ten SKU z witryny Marketplace ponieważ zawsze powinna zostać zakupione za pomocą szablonu rozwiązanie" powinny być oznaczane jako tak tylko wtedy, gdy używaną WERSJĘ części szablonu rozwiązanie. We wszystkich innych przypadkach ta opcja zawsze ma zostać oznaczona jako wartość nie.
- Należy pamiętać: Nie należy zmieniać SKU widoczność ustawienie, gdy jest wyświetlany używaną WERSJĘ. Ta funkcja nie jest obsługiwana.
- Upewnij się, że logo zgodny z wytycznymi logo Azure Marketplace podanych poniżej.
- Opis oferty i SKU nie powinny być takie same.
- W wersji tytuł i oferują długie podsumowanie nie powinny być takie same.
- Tytuł SKU i oferują podsumowanie nie powinny być takie same.
- Jednostka SKU tytuły nie powinny być identyczne konta w ofercie z wielu wersji produktu.

**Wytyczne logo usługi Azure Marketplace**

- Azure projekt ma paletę kolorów prosty. Pozostaw liczbę głównego i pomocniczego kolorów na logo niski.
- Kolory motywu Azure portal są białe i czarne. W związku z tym należy unikać te kolory jako kolor tła swoje logo. Za pomocą niektórych kolor, który spowodowałby swoje logo widocznych w portalu Azure. Zaleca się proste kolory podstawowe. Jeśli korzystasz z przezroczystym tłem, następnie upewnij się, że logo i tekst nie jest biała lub czarna.
- Nie należy używać tła z gradientem na logo.
- Unikaj umieszczania tekstu, nawet firmie lub marką, na logo.
- Wygląd i działanie logo powinno być "prostym" i należy unikać gradienty.
- Logo nie powinny być rozciągnięciu.

**Dodatkowe wskazówki dotyczące logo główny:**

- Logo główny jest opcjonalna. Nie przekazać logo główny można wybrać wydawcy. **Jednak raz przekazane z publikowania nie można usunąć ikonę główny portalu. W tym czasie partnera należy wykonać Azure Marketplace wytyczne dotyczące ikony główny jeszcze oferta nie zostanie zatwierdzony, aby produkcji.**
- Nazwa wyświetlana w programie Publisher, tytuł SKU i długi Podsumowanie oferty są wyświetlane w kolor biały czcionki. W związku z tym należy unikać, pamiętając dowolny uproszczonej kolor tła ikonę główny. Czarny, biały i przezroczystości tła nie jest dozwolona główny ikon.
- Wydawcy wyświetlane nazwy, SKU tytuł, czas Podsumowanie oferty i przycisk Utwórz są osadzone programowy wewnątrz logo główny po oferty przechodzi wymienionych. Nie należy więc wprowadzić dowolny tekst, podczas projektowania logo główny. Po prostu pozostaw puste miejsce po prawej stronie, ponieważ tekst (to znaczy wyświetlania programu Publisher nazwisko, tytuł SKU długo Podsumowanie oferty) objęte programowy nam przez niego. Puste miejsce w tekście powinny być 415 x 100 po prawej stronie (i przesunięcia przez 370px z lewej strony).


## <a name="additional-production-checklist-for-already-listed-virtual-machine-offers"></a>Oferuje dodatkowe produkcji listy kontrolnej dotyczącej już wymienionych maszyn wirtualnych

- Sprawdź, czy jest już ofertę o takiej samej nazwie oferty z Twojej firmy. Jeśli tak, należy dodać nową wersję używaną WERSJĘ w ofercie istniejących zamiast tworzenia nowej oferty zduplikowane.
- Dysku danych nie należy zmieniać między dwie wersje tego samego SKU.
- Azure Marketplace nie obsługuje cennik Zmień wymienionych wersji produktu, zgodnie z ich wpływ rozliczenie obecnych klientów. Upewnij się, że nie zmienisz ceny wymienionych wersji produktów w regionach, w której znajduje się wersję produktu. Można jednak dodawanie nowej wersji produktu lub dodać nowe obszary do istniejącego SKU.


## <a name="next-steps"></a>Następne kroki
Gdy oferty przechodzi live, przetestuj scenariuszy, aby sprawdzić, czy wszystkich umów i funkcji działał poprawnie, w środowisku produkcyjnym jako przetestowane i sprawdzone w środowisku tymczasowy.

## <a name="see-also"></a>Zobacz też
- [Wprowadzenie: jak publikować ofertę Azure Marketplace](marketplace-publishing-getting-started.md)

[img-pubportal-walkthru-checked]:media/marketplace-publishing-push-to-production/pubportal-walkthru-checked.png
[img-pubportal-menu-publish]:media/marketplace-publishing-push-to-production/pubportal-menu-publish.png
[img-pubportal-publish-pushproduction]:media/marketplace-publishing-push-to-production/pubportal-publish-pushproduction.png
