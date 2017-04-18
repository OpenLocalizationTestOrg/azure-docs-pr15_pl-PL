<properties
   pageTitle="Rozpoczynanie pracy z szablonami prywatne | Microsoft Azure"
   description="Dodawanie, zarządzanie i udostępniać prywatne szablonów przy użyciu Azure portal, polecenie Azure lub programu PowerShell."
   services="marketplace-customer"
   documentationCenter=""
   authors="VybavaRamadoss"
   manager="asimm"
   editor=""
   tags="marketplace, azure-resource-manager"
   keywords=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/18/2016"
   ms.author="vybavar"/>

# <a name="get-started-with-private-templates-on-the-azure-portal"></a>Rozpoczynanie pracy z szablonami prywatne na Azure Portal

Szablon [Azure Menedżera zasobów](../resource-group-authoring-templates.md) jest deklaracyjnych szablonu można definiować wdrożenia. Można zdefiniować zasoby, aby rozmieścić rozwiązanie i określić parametrów i zmiennych, które umożliwiają wartości wejściowe dla różnych środowiskach. Szablon zawiera JSON i wyrażeń, których można użyć do utworzenia wartości dla wdrożenia.

Umożliwia nowa funkcja **Szablony** w [Azure Portal](https://portal.azure.com) wraz z dostawcą zasobów **Microsoft.Gallery** jako rozszerzenie [Azure Marketplace](https://azure.microsoft.com/marketplace/) umożliwiają tworzenie i wdrażanie szablonów prywatne z biblioteki osobistej i zarządzanie nimi.

Ten dokument przeprowadzi Cię przez dodawanie, zarządzania i udostępniania prywatnych **szablonu** za pomocą Azure Portal.

## <a name="guidance"></a>Wskazówki

Następujące sugestie mogą pomóc w pełni wykorzystać **Szablony** podczas pracy z rozwiązanie:

- **Szablon** jest włókienka zasób, który zawiera szablon Menedżera zasobów i dodatkowe metadane. Działa bardzo podobnie do elementu na rynku. Różnica klucza jest elementu prywatnego zamiast publicznej elementów Marketplace.
- Biblioteka **szablonów** sprawdza się w przypadku użytkowników, którzy potrzebują dostosowywanie ich wdrożenia.
- **Szablony** sprawdza się w przypadku użytkowników, którzy potrzebują prostej repozytorium w Azure.
- Uruchom z istniejącego szablonu Menedżera zasobów. Znajdowanie szablonów w [github](https://github.com/Azure/azure-quickstart-templates) lub [Eksportuj szablon](../resource-manager-export-template.md) z istniejącej grupy zasobów.
- **Szablony** są powiązane z użytkownika, który je publikuje. Nazwa wydawcy jest widoczny dla każdego, kto ma dostęp do odczytu do niego.
- **Szablony** są zasobami Menedżera zasobów i nie można zmienić nazwy po opublikowaniu.

## <a name="add-a-template-resource"></a>Dodawanie zasobu szablonu

Istnieją dwa sposoby tworzenia zasobu **szablonu** w portalu Azure.

### <a name="method-1--create-a-new-template-resource-from-a-running-resource-group"></a>Metoda 1: Tworzenie nowego zasobu szablonu na podstawie uruchomionego grupa zasobów

1. Przejdź do istniejącej grupy zasobów na Azure Portal. W obszarze **Ustawienia**zaznacz opcję **Eksportuj szablon** .
2. Po wyeksportowaniu szablonu Menedżera zasobów, użyj przycisku **Zapisz szablon** zapisanie go w repozytorium **szablonów** . Znajdowanie pełne szczegóły szablonu eksportu [poniżej](../resource-manager-export-template.md).
<br /><br />
![Eksportowanie grupa zasobów](media/rg-export-portal1.PNG)  <br />

3. Wybierz przycisk polecenia **Zapisz w szablonie** .
<br /><br />

4. Wprowadź następujące informacje:

    - Nazwa — nazwy obiektu szablonu (Uwaga: jest to nazwa Menedżera zasobów Azure podstawie. Wszystkie nazewnictwa ograniczeniom i nie można zmienić, raz utworzone).
    - Opis — krótkie podsumowanie o szablonie.

    ![Zapisywanie szablonu](media/save-template-portal1.PNG)  <br />

5. Kliknij przycisk **Zapisz**.

    > [AZURE.NOTE] Karta szablonu eksportu Wyświetla powiadomienia, gdy wyeksportowanych szablonów Menedżera zasobów występują błędy, ale nadal będzie można zapisać ten szablon Menedżera zasobów do szablonów. Upewnij się, sprawdza i rozwiązywanie problemów szablonu którykolwiek z menedżerów zasobów przed ponowne rozmieszczanie wyeksportowanych szablonów Menedżera zasobów.

### <a name="b-method-2--add-a-new-template-resource-from-browse"></a>B. Metoda 2: Dodawanie nowego zasobu szablonu z Przeglądaj

Możesz również dodać nowy **szablon** możliwość korzystania z wymazywanie + Dodaj przycisk polecenia w **Przejdź > Szablony**. Należy podać nazwę, opis i szablon Menedżera zasobów JSON.

![Dodawanie szablonu](media/add-template-portal1.PNG)  <br />

> [AZURE.NOTE] Microsoft.Gallery jest podstawą dzierżawy dostawcy Azure zasobów. Zasób szablonu jest związany użytkownika, który go utworzył. Nie jest związana z dowolnego określonego subskrypcji. Subskrypcji należy należy wybrać tylko wtedy, gdy wdrażanie szablonu.

## <a name="view-template-resources"></a>Przeglądanie zasobów szablonu

Wszystkie **Szablony** dostępne są widoczne u **Przejdź > Szablony**. W tym **Szablony** zostały utworzone i obramowania, które zostały udostępnione Tobie różne poziomy uprawnień. Więcej informacji w sekcji [Kontrola dostępu](#access-control-for-a-tenant-resource-provider) poniżej.

![Wyświetl szablon](media/view-template-portal1.PNG)  <br />

Aby wyświetlić szczegóły **szablonu** , kliknij przycisk do elementu na liście.

![Wyświetl szablon](media/view-template-portal2c.png)  <br />

## <a name="edit-a-template-resource"></a>Edytowanie szablonu zasobu

Można zainicjować przepływu Edytuj **szablon** , klikając prawym przyciskiem myszy element na liście Przeglądaj lub przez wybranie przycisku polecenia Edytuj.

![Edytowanie szablonu](media/edit-template-portal1a.PNG)  <br />

Możesz edytować opis lub tekst szablonu Menedżera zasobów. Nie można edytować nazwę, ponieważ jest to nazwa zasobu Menedżera zasobów. Po możesz edytować szablon Menedżera zasobów JSON, firma Microsoft będzie sprawdzać poprawność, aby upewnić się, które jest prawidłowe JSON. Wybierz **przycisk OK** , a następnie **Zapisz** zapisać zaktualizowane szablonu.

![Edytowanie szablonu](media/edit-template-portal2a.PNG)  <br />

Po zapisaniu **szablon** zostanie wyświetlony potwierdzenia.

![Edytowanie szablonu](media/edit-template-portal3b.png)  <br />

## <a name="deploy-a-template-resource"></a>Wdrażanie zasobu szablonu

Można wdrażać dowolny **szablon** , którego masz uprawnienia do **odczytu** na. Przepływ wdrożenia otwiera standardowy karta wdrożenia Azure szablonu. Wypełnij wartości parametrów szablonu Menedżera zasobów na kontynuowanie wdrożenia.

![Wdrażanie szablonu](media/deploy-template-portal1b.png)  <br />

## <a name="share-a-template-resource"></a>Udostępnianie zasobu szablonu

Zasób **szablonu** będą udostępniane współpracownicy. Udostępnianie działa podobnie jak [przypisania roli dla każdego zasobu Azure](../active-directory/role-based-access-control-configure.md). Właściciel **szablonu** zawiera uprawnienia do innych użytkowników, którzy mogą korzystać z zasobem szablonu. Osoby lub grupy osób, którym udostępniasz **szablonu** z będzie możliwe zobaczenie szablonu Menedżera zasobów i jego właściwości galerii.

### <a name="access-control-for-the-microsoftgallery-resources"></a>Kontrola dostępu do zasobów Microsoft.Gallery

Rola | Uprawnienia
---|----
Właściciel | Umożliwia pełną kontrolę na zasobu szablonu, w tym udostępnianie
Czytnik | Umożliwia odczyt i Execute(Deploy) zasobu szablonu
Trybu współautora | Umożliwia edytowanie i usuwanie uprawnień do zasobu szablonu. Użytkownik nie można udostępnić szablonu z innymi osobami

Wybierz pozycję **Udostępnij** na elemencie Przeglądaj, klikając prawym przyciskiem myszy lub karta wyświetlanie określonego elementu. Spowoduje to otwarcie środowisko Udostępnij.

![Udostępnianie szablonu](media/share-template-portal1a.png)  <br />

 Teraz możesz roli i użytkownika lub grupy, aby umożliwić dostęp do określonego **szablonu**. Dostępne role są właściciela, czytnika i współautorów. Więcej informacji w poprzedniej sekcji [Kontrola dostępu](#access-control-for-a-tenant-resource-provider) .

![Udostępnianie szablonu](media/share-template-portal2b.png)  <br />

![Udostępnianie szablonu](media/share-template-portal3b.png)  <br />

Kliknij przycisk **Ok**i **Zaznacz** . Teraz widać, użytkowników lub grup, do których dodano do zasobu.

![Udostępnianie szablonu](media/share-template-portal4b.png)  <br />

> [AZURE.NOTE] Szablon tylko będą udostępniane użytkowników i grup w tej samej dzierżawy usługi Azure Active Directory. Jeśli udostępniasz szablonu adres e-mail, który nie znajduje się w Twojej dzierżawy, zaproszenie zostanie wysłane prośbą na dołączanie do dzierżawy jako Gość.

## <a name="next-steps"></a>Następne kroki

- Aby uzyskać informacje dotyczące tworzenia szablonów Menedżera zasobów, zobacz [Narzędzia do tworzenia szablonów](../resource-group-authoring-templates.md)
- Aby poznać funkcje, których można użyć w szablonie Menedżera zasobów, zobacz [funkcje szablonu](../resource-group-template-functions.md)
- Aby uzyskać wskazówki dotyczące projektowania szablonów zobacz [Najważniejsze wskazówki dotyczące projektowania szablonów Azure Menedżera zasobów](../best-practices-resource-manager-design-templates.md)
