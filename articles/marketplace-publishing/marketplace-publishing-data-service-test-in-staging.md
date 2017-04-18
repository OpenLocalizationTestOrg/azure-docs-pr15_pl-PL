<properties
   pageTitle="Testowanie Twoją ofertę usługi danych dla Marketplace | Microsoft Azure"
   description="Opis sposobu testowania Twoją ofertę usługi danych dla usługi Azure Marketplace."
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

# <a name="testing-your-data-service-offer-in-staging"></a>Testowanie Twoją ofertę usługi danych podczas przenoszenia

>[AZURE.IMPORTANT] **W tej chwili firma Microsoft nie są już ułatwiającej rozpoczęcie korzystania wszelkie nowe wydawcy usługi danych. Nowy dataservices nie będzie uzyskiwanie zatwierdzany dla listy.** Jeśli chcesz opublikować na AppSource aplikacji biznesowych władz akredytacji bezpieczeństwa można znaleźć więcej informacji [w tym miejscu](https://appsource.microsoft.com/partners). Jeśli masz aplikacje IaaS lub chcesz opublikować na Azure Marketplace usługi Deweloper można znaleźć więcej informacji [w tym miejscu](https://azure.microsoft.com/marketplace/programs/certified/).

Po zakończeniu dwa pierwsze kroki [tworzenia konta Microsoft Developer](marketplace-publishing-accounts-creation-registration.md) i [Tworzenie oferty do danych usługi w Portal publikowania](marketplace-publishing-data-service-creation.md) , możesz już przystąpić do udostępnienia Twoją ofertę w Azure Marketplace. W tym temacie przeprowadzi Cię przez przede wszystkim, jako pośrednie krok o nazwie "Tymczasowego"

Organizowanie oznacza, że wdrażanie Twoją ofertę w prywatną "piaskownicy" miejsce, w którym można przetestować i sprawdź jego funkcje przed naciśnięcie go do produkcji. Oferty będą widoczne w tymczasowej tak samo, jak ją do klienta, który został wdrożony go.

## <a name="step-1-pushing-your-offer-to-staging"></a>Krok 1. Naciśnięcie Twoją ofertę do przemieszczania
Naciśnięcie Twoją ofertę do przemieszczania umożliwia sprawdzenie oferty przed staje się dostępna dla przyszłych subskrybentów.  Widać, jak Twoją ofertę będzie wyglądać i działać odpowiadające subskrybowania danych.  

  ![Rysunek](media/marketplace-publishing-data-service-test-in-staging/step-1.1.png)

1.  Zaloguj się do [portalu publikowania](https://publish.windowsazure.com)
2.  W oknie nawigacji po lewej stronie wybierz pozycję **Usług danych**
3.  Wybierz swoją ofertę, który chcesz przekazać do tymczasowego. Zostanie wyświetlony ekran powyżej.
4.  Kliknij przycisk **Push do tymczasowego** .  
5.  W przypadku problemów z oferty, które potrzebne do wykonania przed naciśnięcie do przemieszczania zobaczysz listę wyświetlane.  Popraw te elementy, klikając każdy element na liście. Gdy wszystkich poprawek realizowana, kliknij ponownie przycisk **przekazać do tymczasowego** .

Jeśli występują problemy z Twoją ofertę pojawi się poniżej okna podręcznego.  

Jeśli możesz już nie planowania nie zatwierdzony udostępnienie Twoją ofertę w Azure Portal (obecnie ma ograniczone możliwości), po prostu zamknij okno podręczne.

Aby przetestować usługi danych w Azure Portal (oprócz DataMarket portal), konieczne będzie identyfikator subskrypcji Azure z.  Ten identyfikator subskrypcji będzie identyfikować konto, z którego będzie można przetestować Twoją ofertę.  

Wycinanie i wklejanie identyfikator subskrypcji i kliknij znacznik wyboru, aby kontynuować.

  ![Rysunek](media/marketplace-publishing-data-service-test-in-staging/step-1.2.png)

> [AZURE.NOTE] Te subskrypcje Azure identyfikatory są tylko wymagane do testowania i tymczasowej w [Portalu zarządzania Azure](https://manage.windowsazure.com). Nie są wymagane do testowania w Azure Marketplace.

Na kolejnym ekranie pojawia się pokazuje, że publikowanie odbywa się przez wyświetlanie ikony "w toku" wyróżnione żółty poniżej. Naciśnięcie do przemieszczania przejście między 10 do 15 minut.  Jeśli trwa dłużej, najpierw Odśwież przeglądarkę (w programie Internet Explorer naciśnij klawisz F5).  Czasami miejsce, w którym Twoja oferta jest nadal naciśnięcie tymczasowego po upływie godziny, kliknij pozycję Kontakt z nami łącze do trafić występuje problem.

  ![Rysunek](media/marketplace-publishing-data-service-test-in-staging/step-1.3.png)

Po zakończeniu naciśnij, aby tymczasowego ikonę "w toku" będzie przenoszenie stop i stan zostaną one zaktualizowane do "Przygotowana".  Teraz możesz przystąpić do testowania Twoją ofertę.  

## <a name="step-2-test-your-staged-offer-in-datamarket"></a>Krok 2. Przetestuj swoją ofertę etapowej w DataMarket

Kliknij łącze po tekst **"Zobacz usługi oferują przy..."** Aby wyświetlić ekran, że subskrybenta zostanie wyświetlony, gdy Twoją ofertę przechodzi do produkcji i pojawi się w DataMarket.

  ![Rysunek](media/marketplace-publishing-data-service-test-in-staging/step-2.2.png)

Testowanie lub sprawdź każdy z elementów 12 oznaczone powyżej w celu zapewnienia wszystkich logo, ceny i transakcje, tekst, obrazy, dokumentacji, a łącza są poprawne i działają poprawnie.  To jest odpowiednim czasie, aby upewnić się, że wartości test wprowadzona podczas tworzenia Twoją ofertę zostały zastąpione wartości rzeczywistych.

1. Logo oferty
2. Nazwa oferty
3. Nazwa i łącze do witryny sieci Web firmy wydawcy
4. Kategorie wyszukiwania dla Twoją ofertę
5. Łącze do pomocy technicznej Twoją ofertę ułatwiających subskrybentów
6. Opis kontekstowe Twoją ofertę
7. Plan oferty przedstawiający Szczegóły rozliczenia
8. Łącze do wykonania kodu
9. Przykładowe obrazy, ilustrujące za pomocą danych oferty
10. Metadane wejścia i wyjścia dla każdej usługi w ramach oferty
11. Oferty warunki użytkowania
12. Podgląd danych oferty


Na koniec sprawdź, że usługa działa za pośrednictwem usługi Datamarket, klikając łącze "EKSPLOROWANIE tego zestawu danych".  Zostanie otwarte nowe okno w narzędziu nazywamy "Usługi Eksploratorze", można wyświetlić podgląd wyników kwerendy przed tej usługi.  W tym oknie można wprowadź odpowiednie parametry potrzebne i wyświetlić wyniki wyświetlane z zapytanie usługi.   Ponadto wyświetlany jest adres URL dla kwerendy.  

> [AZURE.NOTE] Należy przejrzeć tekstowy opis usługi wyświetlane u góry.  A jeśli Twoją ofertę składa się z więcej niż jedną usługę połączeń, kliknij przycisk kart u dołu, aby przejść do następnego serwisu przejrzeć i przetestować.



## <a name="next-step"></a>Następny krok
Jeśli występują problemy, a potrzebna pomoc w ich rozwiązywania skontaktuj się z [Obsługą programu Publisher Azure]( http://go.microsoft.com/fwlink/?LinkId=272975).

Jeśli są spełnione i gotowa do opublikowania Twoją ofertę przeczytaj dokumentację [Żądaj zatwierdzenia Push do produkcji](marketplace-publishing-push-to-production.md) .

## <a name="see-also"></a>Zobacz też
- [Wprowadzenie: Jak publikowanie ofertę Azure Marketplace](marketplace-publishing-getting-started.md)
