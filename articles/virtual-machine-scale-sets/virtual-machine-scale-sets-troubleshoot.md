<properties
    pageTitle="Rozwiązywanie problemów z autoscale z zestawami skali maszyn wirtualnych | Microsoft Azure"
    description="Rozwiązywanie problemów z autoscale z zestawami skali maszyn wirtualnych. Opis typowych problemów oraz sposób ich rozwiązania."
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="guybo"/>

# <a name="troubleshooting-autoscale-with-virtual-machine-scale-sets"></a>Rozwiązywanie problemów z autoscale z zestawami skali maszyn wirtualnych

**Problem** — infrastrukturę autoscaling został utworzony w Azure Menedżera zasobów przy użyciu zestawów skali maszyn wirtualnych — na przykład wdrażając szablonu w następujący sposób: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale — masz reguł skali zdefiniowane i jego rozwiązanie idealne, z wyjątkiem, że niezależnie od tego, ile obciążenia umieszczenie na maszyny wirtualne nie będą autoscale.

## <a name="troubleshooting-steps"></a>Procedura rozwiązywania problemów

Kwestie do rozważenia obejmują:

- Ile rdzenie poszczególnych maszyn wirtualnych ma i są ładowane każdego core?
 Przykład szablon Azure Szybki Start powyżej zawiera skrypt do_work.php ładowania jednego podstawowego. Jeśli korzystasz z maszyny większy niż pojedynczy core rozmiar pamięci Wirtualnej, takie jak Standard_A1 lub D1 będą potrzebne do uruchamiania obciążenie wiele razy. Sprawdzanie, ile cores pośrednictwem usługi SMS, przeglądając [maszyn wirtualnych rozmiarów dla systemu Windows platformy Azure](../virtual-machines/virtual-machines-windows-sizes.md)

- Ile maszyny wirtualne maszyn wirtualnych zestaw skali się macie pracy nad każdą z nich?

    Skala się zdarzenia tylko ma być wykonywana, gdy średnia Procesora przez **Wszystkie** maszyny wirtualne w zestawie skali przekracza wartość progowa w czasie wewnętrznych definiowane w regułach autoscale.

- Czy nieodebrania wszelkich zdarzeń skali?

    Przejrzyj dzienniki inspekcji w portalu Azure wydarzeniami Skala. Być może wystąpił skalę w górę i skalę w dół którego została pominięta. Można filtrować według "Skali".

    ![Dzienniki inspekcji][audit]

- Różnią się wartości progowe w skali, a następnie w nowym oknie Skala wystarczająco?

    Załóżmy, że możesz ustawić reguły w celu skalowania, gdy średnia Procesora jest większe niż 50% ponad 5 minut i skali w kiedy średnia Procesora jest mniejsza niż 50%. Spowodowałby "flapping" problem podczas użycie Procesora jest zbliżony tego progu, przy użyciu skali akcje stale zwiększania i zmniejszania rozmiaru zestawu. Z tego powodu usługę autoscale próbuje zapobiec "skrzydłami", której można pojawiają się jako skalowania nie. Dlatego upewnij się, że wartości progowe poza skalowanie i skala w wystarczająco różnią się umożliwia niektórych spację pomiędzy skalowania.

- Czy zapisać szablon JSON?

    Jest proste wprowadzić błędów, dlatego Rozpoczynanie pracy od szablonu, takich jak tej, nad którym jest sprawdzone pracy i wprowadzić zmiany przyrostowe small. 

- Czy można ręcznie skalować lub pomniejszyć?

    Spróbuj ponownie wdrożyć maszyn wirtualnych Skala Ustaw zasobów z ustawieniem inną "pojemność", aby zmienić liczbę maszyny wirtualne ręcznie. Szablon przykład, w tym celu jest tutaj: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing — może być konieczne edytowanie szablonu, aby się upewnić, że ten sam rozmiar maszynowego Ci się korzysta z zestawu Skala. Jeśli liczba maszyny wirtualne można zmienić pomyślnie ręcznie, następnie wiadomo, że problem dotyczy tylko autoscale.

- Sprawdzanie swojej Microsoft.Compute/virtualMachineScaleSet i zasoby Microsoft.Insights w [Eksploratorze zasobów Azure](https://resources.azure.com/)

    Jest to niezbędne narzędzie do rozwiązywania problemów przedstawia stan zasobów Azure Menedżera zasobów. Kliknij swoją subskrypcję i przeglądać grupa zasobów, rozwiązywania problemów. W obszarze obliczeń dostawcy zasobów przeglądać maszyn wirtualnych skali zestawu została utworzona i sprawdź widok wystąpienia, w którym przedstawiono stan wdrożeniu. Również sprawdzić widok wystąpienia maszyny wirtualne w zestawie maszyn wirtualnych Skala. Następnie przejdź do dostawcy zasobów Microsoft.Insights i Sprawdź wygląd reguły autoscale.

- Jest diagnostyczne rozszerzenia pracy i wysyłających dane dotyczące wydajności?

    __Aktualizacji:__ Azure autoscale zostało rozszerzone, aby za pomocą hosta podstawie planowana metryki, której już nie wymaga rozszerzenia diagnostyki musi być zainstalowany. Oznacza to, następnym kilka akapitów nie mają zastosowania po utworzeniu aplikacji autoscaling przy użyciu nowego procesu. Przykłady szablonów Azure, które zostały przekonwertowane na używanie proces host: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale, https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-lapstack-autoscale. 

    Za pomocą hosta podstawie metryki dla autoscale jest lepsza z następujących powodów:

    - Mniej ruchomych elementów jako nie rozszerzenia diagnostyki musi być zainstalowany.
    - Szablony prostsze. Po prostu dodaj reguły autoscale wniosków do istniejącego szablonu zestawu Skala.
    - Raportowanie niezawodne i szybsze uruchamianie nowego maszyny wirtualne.

    Tylko z następujących powodów warto korzystać z rozszerzeniem diagnostyczne będzie, czy jest potrzebny pamięci diagnostyki raportowania i skalowanie. Metryki hosta podstawie nie zgłaszać pamięci.

    Tego pamiętać tylko wykonaj dalszej części tego artykułu, jeśli nadal używasz rozszerzenia diagnostycznych dla swojego autoscaling.

    Autoscale w Menedżerze zasobów Azure pracować (ale nie ma) za pomocą maszyny rozszerzenia diagnostyki wywołane rozszerzenie. Dane dotyczące wydajności na koncie miejsca do magazynowania, zdefiniowanych w szablonie go emituje. Te dane następnie jest agregowane przez usługę Azure Monitor.

    Jeśli usługa wniosków odczytanie danych z pośrednictwem SMS, ma wysyłać wiadomości e-mail — na przykład gdyby maszyny wirtualne w dół, więc warto sprawdzać poczty e-mail (język, który wybrano podczas tworzenia konta Azure).

    Możesz również przejść i sprawdź dane. Przyjrzyj się konta magazynu platformy Azure za pomocą Eksploratora chmury. Na przykład przy użyciu [Programu Visual Studio chmury Eksploratora](https://visualstudiogallery.msdn.microsoft.com/aaef6e67-4d99-40bc-aacf-662237db85a2), zaloguj się i wybierz Azure subskrypcji korzystasz, i nazwę konta magazynu diagnostyki, do których odwołują się definicja rozszerzenia diagnostyki w szablonie wdrożenia.

    ![Eksplorator chmury][explorer]

    W tym polu zostanie wyświetlona wiele tabel miejsce, w którym są przechowywane dane z każdego maszyn wirtualnych. Sporządzanie Linux i metrykę Procesora, na przykład Znajdź najbardziej aktualnych wierszy. Eksplorator chmury Visual Studio obsługuje języka kwerend, więc można uruchomić kwerendę, takie jak "sygnatura czasowa BT datetime'2016-02-02T21:20:00Z" "Aby upewnić się, zostanie wyświetlony ostatnich zdarzeń (zakładając, czas jest w czasie UTC). Czy dane, które widać w odpowiadają reguł skali Skonfiguruj? W poniższym przykładzie Procesora dla komputera 20 pracę, zwiększyć na 100% ostatniego 5 minut.

    ![Magazyn tabel][tables]

    Jeśli dane nie są dostępne, następnie zakłada, że problem jest rozszerzeniem diagnostyczne uruchomione w maszyny wirtualne. Jeśli dane są dostępne, oznacza to, że występuje problem z reguł skali lub z usługą wnioski. Sprawdź [Stan Azure](https://azure.microsoft.com/status/).

    Po pozostajesz te kroki, jeśli nadal występują problemy autoscale może spróbuj na forach w [witrynie MSDN](https://social.msdn.microsoft.com/forums/azure/home?category=windowsazureplatform%2Cazuremarketplace%2Cwindowsazureplatformctp)lub [stosowo przepełnienia](http://stackoverflow.com/questions/tagged/azure)lub logowania pomocy technicznej. Przygotuj się na udostępnianie szablonu i widoku dane dotyczące wydajności.

[audit]: ./media/virtual-machine-scale-sets-troubleshoot/image3.png
[explorer]: ./media/virtual-machine-scale-sets-troubleshoot/image1.png
[tables]: ./media/virtual-machine-scale-sets-troubleshoot/image4.png
