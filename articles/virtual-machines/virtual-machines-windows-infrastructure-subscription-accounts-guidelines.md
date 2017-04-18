<properties
    pageTitle="Subskrypcja i wskazówki dotyczące konta | Microsoft Azure"
    description="Informacje na temat ważnych wskazówek projektowania i wdrażania dla kont Azure i subskrypcji."
    documentationCenter=""
    services="virtual-machines-windows"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="subscription-and-accounts-guidelines"></a>Wskazówki dotyczące konta i subskrypcji

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

W tym artykule omówiono wiedzieć, jak zarządzanie subskrypcji i konta jako środowiska i użytkowników rozwoju.


## <a name="implementation-guidelines-for-subscriptions-and-accounts"></a>Zasady implementacji dla kont i subskrypcji

Decyzje:

- Jakie zestaw subskrypcje i konta, potrzebne do przechowywania z pracą IT lub infrastruktury?
- W jaki sposób podziału hierarchii, aby dopasować organizacji?

Zadania:

- Definiują hierarchii logiczne organizacji chcesz zarządzać nim z poziomu subskrypcji.
- Zgodnie z tym hierarchia logiczna, zdefiniuj wymagane konta i subskrypcje w przypadku każdego konta.
- Tworzenie zestawu kont przy użyciu Konwencja nazewnictwa i subskrypcji.


## <a name="subscriptions-and-accounts"></a>Konta i subskrypcji

Aby pracować z Azure, potrzebujesz jedną lub więcej subskrypcji Azure. Zasoby takie jak wirtualnych maszyn lub wirtualnych sieci znajdują się w tych subskrypcji.

- Przedsiębiorstwa ma zwykle rejestracji przedsiębiorstwa, zasobów najwyższy w hierarchii, i jest skojarzony z co najmniej jednego konta.
- Dla odbiorcy i klienci bez rejestrowania przedsiębiorstwa zasób najwyższy to konto.
- Subskrypcje są skojarzone z kontami, a może być jedną lub więcej subskrypcji dla danego konta. Rekordy Azure rozliczenia informacji na poziomie subskrypcji.

Ze względu na limit dwóch poziomów hierarchii w relacji konto-subskrypcji należy do wyrównywania Konwencja nazewnictwa kont i subskrypcje na potrzeby rozliczeń. Na przykład jeśli globalnej firma korzysta z Azure, ich może mieć jedno konto w rozbiciu na regiony i subskrypcje udało u poziom region:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub01.png)

Na przykład można użyć następującą strukturę:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub02.png)

Jeśli obszar zdecyduje się na więcej niż jedną subskrypcję skojarzone z określoną grupą, konwencji nazewnictwa należy dołączyć umożliwia kodowanie dodatkowe dane na koncie lub nazwę subskrypcji. Ta organizacja umożliwia massaging rozliczeń danych w celu wygenerowania nowych poziomów hierarchii podczas rozliczeń raporty:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub03.png)

Organizacja może wyglądać następująco:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub04.png)

Firma Microsoft udostępnia szczegółowe rozliczeń za pośrednictwem pliku do pobrania dla jednego konta lub dla wszystkich kont umowy dla przedsiębiorstw.


## <a name="next-steps"></a>Następne kroki

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 