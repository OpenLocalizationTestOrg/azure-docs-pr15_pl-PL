<properties
   pageTitle="Eksplorator Azure zasobów | Microsoft Azure"
   description="W tym artykule opisano Explorer zasobów Azure i jak go można przeglądać i aktualizować wdrożeń za pomocą Menedżera zasobów Azure"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="stuartleeks"
   manager="ankodu"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/01/2016"
   ms.author="stuartle;tomfitz"/>

# <a name="use-azure-resource-explorer-to-view-and-modify-resources"></a>Wyświetlanie i modyfikowanie zasobów za pomocą Eksploratora zasobów Azure
[Eksplorator zasobów Azure](https://resources.azure.com) jest doskonałym narzędziem do przeglądania zasoby, które zostały już utworzone w ramach subskrypcji. Za pomocą tego narzędzia, można dowiedzieć się, jak zasoby mają strukturę i zobaczyć właściwości przypisane do każdego zasobu. Informacje o działaniach interfejsu API usługi REST i poleceń cmdlet programu PowerShell, które są dostępne dla typu zasobu można, a można wysyłać polecenia za pośrednictwem interfejsu. Eksplorator zasobów może być szczególnie pomocne, gdy tworzysz szablony Menedżera zasobów, ponieważ umożliwia wyświetlanie właściwości istniejących zasobów.

Źródło narzędzie Explorer zasobów jest dostępna na [github](https://github.com/projectkudu/ARMExplorer), które znajdują się informacje przydatne, jeśli chcesz wdrożyć podobne zachowanie w własnych aplikacji.

## <a name="view-resources"></a>Widok zasobów
Przejdź do [https://resources.azure.com](https://resources.azure.com) i zaloguj się przy użyciu tych samych poświadczeń, której używasz [Azure Portal](https://portal.azure.com).

Po załadowaniu widoku drzewa po lewej stronie umożliwia przechodzenie do szczegółów w Twojej subskrypcji i grup zasobów:

![widoku drzewa](./media/resource-manager-resource-explorer/are-01-treeview.png)

Jak przechodzić do grupy zasobów zostanie wyświetlony dostawców, dla których istnieją zasoby z tej grupy:

![dostawców](./media/resource-manager-resource-explorer/are-02-treeview-providers.png)

Stamtąd możesz rozpocząć przechodzenia do do wystąpienia zasobu. Zrzut ekranu poniżej widać `sltest` wystąpienie programu SQL Server w widoku drzewa. Na po prawej stronie znajdują się informacje dotyczące żądań interfejsu API usługi REST, których można używać z tego zasobu. Przechodząc do węzła dla zasobu Eksploratora zasobów automatycznie podejmie żądania GET pobieranie informacji o zasobie. W obszarze duży blok tekstu pod adres URL zostanie wyświetlona odpowiedź z interfejsu API. 

Jak zapoznanie się ze szablony Menedżera zasobów główną zawartość uruchamia szukać znanych! Sekcja **Właściwości** odpowiedzi zastępuje wartości, które umożliwiają w sekcji **Właściwości** szablonu.

![Program SQL server](./media/resource-manager-resource-explorer/are-03-sqlserver-with-response.png)

Eksplorator zasobów umożliwia zachowywanie przechodzenia do szczegółów, aby zasoby podrzędne, w przypadku serwera bazy danych SQL, istnieją zasoby podrzędne elementów takich jak baz danych i reguły zapory.

Poznawanie bazy danych są wyświetlane us właściwości dla tej bazy danych. Poniżej ekranu widać, że baza danych `edition` jest `Standard` oraz `serviceLevelObjective` (lub Warstwa bazy danych) jest `S1`.

![Baza danych SQL](./media/resource-manager-resource-explorer/are-04-database-get.png)

## <a name="change-resources"></a>Zmiana zasobów

Po przejściu do zasobu, można wybrać przycisk Edytuj, aby umieścić zawartość JSON można edytować. Następnie za pomocą Eksploratora zasobów do edytowania JSON i wysłać żądanie położenie, aby zmienić tego zasobu. Na przykład na poniższej ilustracji przedstawiono warstwy bazy danych, zmieniono na `S0`:

![Baza danych — Wyślij żądanie](./media/resource-manager-resource-explorer/are-05-database-put.png)

Wybierając **umieszczenie** przesłaniu żądania. 

Po przesłaniu żądania Eksploratora zasobów ponownie problemy żądania GET Odśwież stan. W tym przypadku widać, że `requestedServiceObjectiveId` została zaktualizowana i różni się od `currentServiceObjectiveId` wskazująca, że skalowania operacja jest w toku. Przycisk Pobierz ręcznie odświeżyć stanu.

![Baza danych — Pobierz request2](./media/resource-manager-resource-explorer/are-06-database-get2.png)

## <a name="performing-actions-on-resources"></a>Wykonywanie akcji dla zasobów

Na karcie **Akcje** umożliwia Zobacz i wykonywać operacje dodatkowe pozostałych. Na przykład po wybraniu zasobu witryny sieci web na kartę Akcje przedstawia o dużej liczbie operacji, niektóre z nich są wyświetlane poniżej.

![sieć Web - żądania POST](./media/resource-manager-resource-explorer/are-web-post.png)

## <a name="invoking-the-api-via-powershell"></a>Wywoływanie interfejsu API przy użyciu programu PowerShell
Na karcie programu PowerShell w Eksploratorze zasobów zawiera polecenia cmdlet, aby korzystać z zasobów, które są obecnie poznawanie za pomocą. W zależności od typu zasobu jest zaznaczona, wyświetlana skrypt programu PowerShell może przyjmować wartości od prostej polecenia cmdlet (takie jak `Get-AzureRmResource` i `Set-AzureRmResource`) do bardziej skomplikowana polecenia cmdlet (na przykład zamiana gniazda w witrynie sieci web). 

![Programu PowerShell](./media/resource-manager-resource-explorer/are-07-powershell.png)

Aby uzyskać więcej informacji na Azure poleceń cmdlet zobacz [Za pomocą programu Azure przy użyciu Menedżera zasobów Azure](powershell-azure-resource-manager.md)

## <a name="summary"></a>Podsumowanie
Podczas pracy z Menedżerem zasobów, Eksploratora zasobów może być bardzo przydatne narzędzie. Jest to doskonały sposób na znaleźć sposoby za pomocą programu PowerShell kwerendy, a następnie wprowadź zmiany. Jeśli pracujesz z interfejsu API usługi REST to doskonały sposób na rozpoczęcie pracy i szybko przetestować interfejsu API przed rozpoczęciem pisania kodu. Oraz jeśli piszesz szablony, że może to być doskonały sposób na zrozumienie hierarchii zasobów i Znajdź lokalizację umieszczenia Konfiguracja — można zmiany w portalu, a następnie odszukaj odpowiednie wpisy w Eksploratorze zasobów!

Aby uzyskać więcej informacji Obejrzyj [wideo z Scotta Hanselman i David Ebbo kanału 9](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Resource-Manager-Explorer-with-David-Ebbo)


