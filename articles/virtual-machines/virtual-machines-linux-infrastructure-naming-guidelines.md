<properties
    pageTitle="Infrastruktura nazewnictwa wskazówki | Microsoft Azure"
    description="Informacje na temat ważnych wskazówek projektowanie i wdrażanie nazw w usługach Azure infrastruktury."
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="infrastructure-naming-guidelines"></a>Wskazówki dotyczące nazewnictwa infrastruktury

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

W tym artykule omówiono wiedzieć, jak rozwiązać konwencje nazewnictwa dla wszystkich zasobów Azure różnych Tworzenie zestawu logicznej i identyfikację zasobów w środowisku usługi.

## <a name="implementation-guidelines-for-naming-conventions"></a>Implementacja wskazówki dotyczące konwencje nazewnictwa

Decyzje:

- Co to są usługi konwencje nazewnictwa dla zasobów Azure?

Zadania:

- Definiowanie afiksów, aby zachować spójność za pomocą wielu zasobów.
- Definiowanie podane wymagania być globalnie unikatowe nazwy kont miejsca do magazynowania.
- Dokument konwencji nazewnictwa i rozpowszechnianie do wszystkich stron biorących udział w celu zapewnienia spójności przez wdrożenia.

## <a name="naming-conventions"></a>Konwencje nazewnictwa

Przed utworzeniem niczego w Azure, należy użyć dobre konwencji nazewnictwa w miejscu. Konwencje nazewnictwa gwarantuje, że wszystkie zasoby przewidywalne nazwy, co pomaga zmniejszyć obciążenie administracyjne związane z zarządzaniem tych zasobów.

Warto obserwować określonego zestawu konwencje nazewnictwa zdefiniowane dla całej organizacji lub określoną subskrypcję Azure lub konta. Chociaż jest łatwe dla osób w ramach organizacji ustalenie niejawne reguł podczas pracy z zasobami Azure, należy mieć możliwość skali członkom zespołu współpracujących Azure.

Uzgodnić zestawu konwencje nazewnictwa na początku. Istnieje kilka zagadnień dotyczących konwencji nazewnictwa Wycinanie przez ustawionych reguł.

## <a name="affixes"></a>Umieszcza

Jak ustalić do definiowania konwencji nazewnictwa, jednej decyzji jest czy Afiks znajduje się na:

- Na początku nazwy (prefiks)
- Na końcu nazwy (sufiks)

Na przykład w tym miejscu są dwie możliwe nazwy dla grupy zasobów za pomocą `rg` umieścić:

- Aplikacja RG sieci Web (prefiks)
- Aplikacja sieci Web — Rg (sufiks)

Afiksów może odnosić się do różnych aspektów opisujące określonego zasobów. W poniższej tabeli przedstawiono kilka przykładów zazwyczaj używany.

| Proporcji                               | Przykłady                                                               | Notatki                                                                                                      |
|:-------------------------------------|:-----------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------|
| Środowisko                          | deweloperów, stg, produktu                                                         | W zależności od przeznaczenia i nazwa każdego środowiska.                                                     |
| Lokalizacja                             | usw (Zachodnia Stany Zjednoczone), użyj (USA wschodniego 2)                                         | W zależności od obszaru centrum danych lub regionu, w organizacji.                               |
| Składnik Azure, usługi lub produktu | RG grupy zasobów, VNet dla wirtualnej sieci                        | W zależności od produktu, dla którego zasobu udostępnia obsługę.                                          |
| Rola                                 | DB, aplikacji sieci web                                                           | W zależności od roli maszyny wirtualnej.                                                              |
| Wystąpienia                             | 01, 02, 03, itp.                                                       | Dla zasobów, które mają więcej niż jedno wystąpienie. Na przykład Załaduj strategie serwerów sieci web w usłudze w chmurze. |


Tworząc konwencji nazewnictwa, upewnij się, że Województwo wyraźnie które afiksów dla każdego typu zasobu, w których położenie (sufiks w porównaniu z prefiksem).

## <a name="dates"></a>Daty

Często jest ważne jest określenie daty utworzenia od nazwy zasobu. Zalecamy RRRRMMDD format daty. Ten format gwarantuje, nie tylko jest pełny daty został zapisany, ale również tego dwa zasoby których nazwy różnią się tylko na podstawie daty są sortowane alfabetycznie i chronologicznie.

## <a name="naming-resources"></a>Nadawanie nazw zasobów

Definiowanie każdego typu zasobu w konwencji nazewnictwa, które mają reguł, które określają, jak przypisać nazwy do każdego zasobu, która zostanie utworzona. Te reguły ma być stosowane do wszystkich typów zasobów, na przykład:

- Subskrypcje
- Konta
- Konta miejsca do magazynowania
- Wirtualnych sieci
- Podsieci
- Zestawy dostępności
- Grupy zasobów
- Maszyn wirtualnych
- Punkty końcowe
- Grupy zabezpieczeń sieci
- Role

Aby upewnić się, że nazwa zawiera wystarczających informacji, aby określić, do którego odnosi się zasobu należy używać nazwy opisowe.

## <a name="computer-names"></a>Nazwy komputera

Po utworzeniu maszyny wirtualnej (maszyn wirtualnych) Azure wymaga nazwy maszyn wirtualnych maksymalnie 64 znaki używaną w polu Nazwa zasobu. Azure używa tej samej nazwy dla system operacyjny zainstalowany w maszyn wirtualnych. Jednak tych nazw mogą nie być taka sama.

Jeśli maszyny jest utworzony na podstawie pliku obrazu vhd, która już zawiera system operacyjny, nazwę maszyn wirtualnych w Azure mogą różnić się od nazwy komputera maszyn wirtualnych systemu operacyjnego. Do zarządzania maszyn wirtualnych, dlatego nie zaleca się takiej sytuacji można dodać stopień trudności. Przypisywanie zasobów maszyn wirtualnych Azure taką samą nazwę jak nazwa komputera, która przypisana do tego maszyn wirtualnych systemu operacyjnego.

Zaleca się, że nazwa maszyn wirtualnych Azure jest taka sama, jak źródłowej nazwa komputera systemu operacyjnego.

## <a name="storage-account-names"></a>Nazwy kont miejsca do magazynowania

W przypadku kont przestrzeni dyskowej są specjalne zasady ich nazwy. Można używać tylko małe litery i cyfry. Aby uzyskać więcej informacji, zobacz [Tworzenie konta miejsca do magazynowania](../storage/storage-create-storage-account.md#create-a-storage-account) . Ponadto nazwę konta magazynu z core.windows.net, należy globalnie prawidłową, unikatową nazwę DNS. Na przykład jeśli konto miejsca do magazynowania jest nazywane mystorageaccount, następujących wyniku nazw DNS powinny być unikatowe:

- mystorageaccount.blob.Core.Windows.NET
- mystorageaccount.TABLE.Core.Windows.NET
- mystorageaccount.Queue.Core.Windows.NET


## <a name="next-steps"></a>Następne kroki
[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 