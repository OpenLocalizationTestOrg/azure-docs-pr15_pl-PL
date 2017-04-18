<properties
     pageTitle="Umożliwia utworzenie Centrum IoT Azure portal | Microsoft Azure"
     description="Omówienie sposobu tworzenia i zarządzać koncentratorów Azure IoT za pośrednictwem portalu Azure"
     services="iot-hub"
     documentationCenter=""
     authors="dominicbetts"
     manager="timlt"
     editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="na"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/30/2016"
     ms.author="dobett"/>

# <a name="create-an-iot-hub-using-the-azure-portal"></a>Tworzenie Centrum IoT za pomocą portalu Azure

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]


## <a name="introduction"></a>Wprowadzenie

W tym artykule opisano, jak znaleźć Centrum IoT usługi w portalu Azure i jak tworzyć i zarządzać nimi koncentratory IoT.

## <a name="where-to-find-iot-hubs"></a>Gdzie można znaleźć koncentratory IoT

Istnieje różnych miejscach, w którym można znaleźć koncentratory IoT.

1. **+ Nowe**: **Centrum IoT Azure** to usługa IoT i można je znaleźć w kategorii **Internet czynności**w obszarze **+ Nowy**, podobnie jak inne usługi.

2. Koncentratory IoT również są dostępne za pośrednictwem witryny Marketplace programu jako usługa główny w obszarze **Internet rzeczy**.

## <a name="create-an-iot-hub"></a>Tworzenie koncentratora IoT

Można tworzyć Centrum IoT za pomocą następujących metod:

- Tworzenie Centrum IoT za pomocą opcji **+ Nowe** prowadzi do karta wyświetlane w następnej zrzut ekranu. Etapy tworzenia Centrum IoT za pośrednictwem ta metoda i rynku są identyczne.

- Tworzenie Centrum IoT na rynku: kliknięcie przycisku **Utwórz** otwarcia kartę, która jest identyczny z poprzedniego karta **+ Nowe** możliwości. Kilka etapy tworzenia Centrum IoT znajdują się w następnej sekcji.

### <a name="choose-the-name-of-the-iot-hub"></a>Wybierz nazwę Centrum IoT

Aby utworzyć koncentratora IoT, musisz nadać nazwę koncentratora. Ta nazwa musi być unikatowa we koncentratory. Nie ma duplikatów koncentratorów jest dozwolona w wewnętrznej, dlatego zalecane jest, w sposób unikatowy nazwie tego Centrum.

### <a name="choose-the-pricing-tier"></a>Wybieranie poziomu cen

Możesz wybrać z czterech poziomów: **wolny**, **Standardowy 1** i **2 standardowy**i **Standardowy S3**. Bezpłatne warstwa umożliwia tylko 500 urządzenia będą podłączone do koncentratora IoT i do maksymalnie 8000 wiadomości dziennie.

**Standardowe S1**: S1 koncentratory IoT edition jest przeznaczony dla IoT rozwiązania, które dużej liczby urządzeń generowania niewielkich ilości danych na urządzenie. Każdej jednostki S1 edition umożliwia do 400 000 wiadomości dziennie na wszystkich urządzeniach połączonego.

**Standardowy S2**: S2 Centrum IoT edition jest przeznaczony dla rozwiązań IoT, w których urządzeń generowanie dużych ilości danych. Każdej jednostki S2 edition umożliwia do 6 milionów wiadomości dziennie między wszystkich urządzeń połączonych.

**Standardowy S3**: S3 Centrum IoT edition jest przeznaczony dla rozwiązań IoT, generujących dużych ilości danych. Każdej jednostki S3 edition umożliwia maksymalnie 300 milionów wiadomości dziennie między wszystkich urządzeń połączonych.

![][4]

> [AZURE.NOTE] Centrum IoT zapewnia tylko jeden bezpłatne Centrum na subskrypcję Azure.

### <a name="iot-hub-units"></a>Jednostki koncentratora IoT

Jednostki Centrum IoT zawiera określoną liczbę wiadomości dziennie. Całkowita liczba wiadomości obsługiwane dla tego koncentratora jest liczba jednostek pomnożona przez liczbę wiadomości dziennie dla tego poziomu. Na przykład jeśli chcesz Centrum IoT do obsługi wnikaniem 700,000 wiadomości, możesz wybrać dwie S1 poziomu jednostki.

### <a name="device-to-cloud-partitions-and-resource-group"></a>Urządzenia do chmury partycje i grupa zasobów

Można zmienić liczbę partycje koncentratora IoT. Domyślne partycje są ustawione na 4; Jednak możesz wybrać inną liczbę partycje z listy rozwijanej.

Dla grup zasobów nie musisz jawnie utworzyć grupę zasobów puste. Podczas tworzenia zasobu, można utworzyć nową grupę zasobów lub użycie istniejącej grupy zasobów.

![][5]

### <a name="choose-subscriptions"></a>Wybierz pozycję subskrypcji

Azure Centrum IoT automatycznie wyświetlana na liście subskrypcji Azure, z którymi jest połączony konta użytkownika. Możesz wybrać jedną z opcji, w tym miejscu chcesz skojarzyć tę subskrypcję Azure Centrum IoT.

### <a name="choose-the-location"></a>Wybierz lokalizację

Opcja położenie zawiera listę regionów, w których jest oferowana IoT Centrum. Centrum IoT jest dostępna do wdrożenia w następujących lokalizacjach: Wschód Australia, kopiec Australia, Azji Wschodniej, kopiec Azji, Europa Północna, zachód Europe, wschód Japonia Zachodnia Japonia nam Wschodzie, nam Zachód.

### <a name="create-the-iot-hub"></a>Tworzenie Centrum IoT

Po zakończeniu wszystkie poprzednie kroki Centrum IoT jest gotowe do utworzenia. Kliknij przycisk **Utwórz** , aby rozpocząć proces wewnętrznej tworzenia tego Centrum IoT z określonymi opcjami i wdrożenie go do lokalizacji określonej.

Może potrwać kilka minut na Centrum IoT ma zostać utworzony jako czas wdrożenia wewnętrznej ma być wykonywana na serwerach odpowiedniej lokalizacji.

## <a name="change-the-settings-of-the-iot-hub"></a>Zmienianie ustawień Centrum IoT

Możesz zmienić ustawienia istniejącego Centrum IoT po jego utworzeniu z karta IoT Centrum.

![][8]

**Zasady dostępu udostępnione**: te zasady definiują uprawnienia dla urządzeń i usług do łączenia się IoT Centrum. Masz dostęp do tych zasad, klikając **Zasady dostępu udostępnione** w obszarze **Ogólne**. W tym karta można zmodyfikować istniejące zasady lub dodać nową zasadę.

### <a name="create-a-policy"></a>Tworzenie zasad

- Kliknij przycisk **Dodaj** , aby otworzyć kartę. W tym miejscu można wprowadzić nową nazwę zasady i uprawnienia, które chcesz skojarzyć z tymi zasadami, jak pokazano na poniższym rysunku.

    Istnieje kilka uprawnień, które można skojarzyć z tych zasad udostępnionego. Dwa pierwsze zasad, **rejestru czytanie** i **Pisanie rejestru**, udzielanie odczytu i zapisu uprawnienia do magazynu tożsamości urządzenia lub rejestru tożsamości. Wybieranie opcji zapisu automatycznie wybiera także przeczytaj opcji.

    **Usługa połączyć** zasad udziela uprawnień dostępu do punktów końcowych chmury stronie takie jak w przypadku usług nawiązywanie połączenia z poziomu Centrum IoT grupy dla klientów indywidualnych. **Połącz urządzenie** zasad przyznaje uprawnienia do wysyłania i odbierania wiadomości na stronie urządzenia punkty końcowe Centrum IoT.

- Kliknij przycisk **Utwórz** , aby dodać nowo utworzonej zasady do istniejącej listy.

![][10]

## <a name="messaging"></a>Wiadomości

Kliknij pozycję **wiadomości** , aby wyświetlić listę wiadomości właściwości koncentratora IoT, który jest modyfikowany. Istnieją dwa główne typy właściwości, które można modyfikować lub kopiowanie: **chmurą urządzenia** i **urządzenia w chmurze**.

- Ustawienia **chmurą urządzenia** : to ustawienie ma dwa subsettings: **Cloud urządzenia TTL** (czas wygaśnięcia) i **czas przechowywania** wiadomości. Podczas koncentratora IoT oba te ustawienia są tworzone z wartością domyślną godzinę. Aby ustawić te wartości, za pomocą suwaka lub wpisz wartości.

- Ustawienia **urządzenia w chmurze** : to ustawienie ma kilka subsettings, niektóre z nich są o nazwie i przypisane po Centrum IoT zostanie utworzona i czy można skopiować tylko do innych subsettings, które można dostosować. Te ustawienia znajdują się w następnej sekcji.

**Partycje**: Ta wartość jest ustawiona, gdy Centrum IoT zostanie utworzona i mogą być zmieniane za pomocą tych ustawień.

**Nazwa zgodnego Centrum zdarzeń i punkt końcowy**: Centrum gdy IoT jest tworzony, Centrum zdarzenie jest tworzona wewnętrznie potrzebować dostępu do w niektórych okolicznościach. Ta nazwa zgodnego Centrum zdarzeń i punkt końcowy nie można dostosowywać, ale jest dostępny do użytku za pomocą przycisku **Kopiuj** .

**Czas przechowywania**: domyślnie ustawiona na jeden dzień, ale można dostosować do innych wartości na liście rozwijanej. Ta wartość jest w dniach dla urządzenia w chmurze, a nie w godziny, tak jak podobne ustawienie chmury do urządzenia.

**Grupy dla klientów indywidualnych**: grupy dla klientów indywidualnych są podobne do innych systemów obsługi wiadomości, które mogą być używane do pobierania danych w określony sposób, aby nawiązać Centrum IoT innych aplikacji lub usług ustawienie. Co Centrum IoT zostanie utworzona i grupę domyślną dla klientów indywidualnych. Jednak można dodawać lub usuwać grupy konsumentów do swojego koncentratorów IoT przez użytkownika.

> [AZURE.NOTE] Grupy domyślnej dla klientów indywidualnych nie można edytować ani usuwać.

![][11]

## <a name="pricing-and-scale"></a>Ceny i Skala

Cennik istniejących Centrum IoT można zmienić, modyfikując ustawienia **ceny** , występują następujące wyjątki:

- W bieżącej implementacji koncentratora IoT za pomocą bezpłatnej wersji nie można zmienić warstwy do jednego z płatnej wersji produktu, lub odwrotnie.
- Może istnieć tylko jeden Centrum IoT bezpłatne warstwa Azure subskrypcji.

![][12]

Przechodzenie z wyższego poziomu (S2 lub S3) obniżyć poziom (S1 lub S2) może znajdować się tylko wtedy, gdy liczba wiadomości wysłanych do tego dnia nie jest w konflikcie. Na przykład jeśli liczba wiadomości dziennie przekracza 400 000 zł, następnie warstwie koncentratora IoT mogą być zmieniane. Jednak w przypadku zmiany w warstwie S1 koncentratora jest ograniczenie dla tego dnia.

## <a name="delete-the-iot-hub"></a>Usuwanie Centrum IoT

Można przejść do Centrum IoT, który chcesz usunąć, klikając przycisk **Przeglądaj**, a następnie wybierając odpowiednie Centrum do usunięcia. Kliknij przycisk **Usuń** pod nazwą Centrum, aby usunąć Centrum.

## <a name="next-steps"></a>Następne kroki

Skorzystaj z poniższych łączy, aby dowiedzieć się więcej o zarządzaniu Centrum IoT Azure:

- [Zbiorcze zarządzanie urządzeniami IoT][lnk-bulk]
- [Metryki zastosowania][lnk-metrics]
- [Operacje monitorowania][lnk-monitor]

Aby dodatkowo Poznawanie możliwości Centrum IoT, zobacz:

- [Przewodnik dewelopera][lnk-devguide]
- [Symulowanie urządzenia z IoT SDK bramy][lnk-gateway]
- [Zabezpieczanie rozwiązania IoT od podstaw w górę][lnk-securing]


  [4]: ./media/iot-hub-create-through-portal/create-iothub.png
  [5]: ./media/iot-hub-create-through-portal/location1.png
  [8]: ./media/iot-hub-create-through-portal/portal-settings.png
  [10]: ./media/iot-hub-create-through-portal/shared-access-policies.png
  [11]: ./media/iot-hub-create-through-portal/messaging-settings.png
  [12]: ./media/iot-hub-create-through-portal/pricing-error.png

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md