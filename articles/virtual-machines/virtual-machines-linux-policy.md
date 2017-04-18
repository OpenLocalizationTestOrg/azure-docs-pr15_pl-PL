<properties
    pageTitle="Stosowanie zasad do maszyn wirtualnych Menedżera zasobów Azure | Microsoft Azure"
    description="Jak stosowanie zasad do Azure Menedżera zasobów Linux maszyn wirtualnych"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/13/2016"
    ms.author="singhkay"/>

# <a name="apply-policies-to-azure-resource-manager-virtual-machines"></a>Stosowanie zasad do maszyn wirtualnych Menedżera zasobów Azure

Korzystając z zasad organizacji można wymusić różne konwencje i reguł w przedsiębiorstwie. Stosowanie zachowanie może pomóc w ograniczanie ryzyka podczas uczestniczy sukcesu organizacji. W tym artykule firma Microsoft będzie opisano, jak zasady Menedżera zasobów Azure umożliwia definiowanie zachowania odpowiedniej dla Twojej organizacji maszyn wirtualnych.

Konspekt z instrukcjami, aby osiągnąć ten cel jest jako poniżej

1. Zasada Menedżera zasobów Azure 101
2. Opracowanie zasad komputera wirtualnych
3. Tworzenie zasad
4. Stosowanie zasad

## <a name="azure-resource-manager-policy-101"></a>Zasada Menedżera zasobów Azure 101

Wprowadzenie do zasad Menedżera zasobów Azure, zaleca w artykule poniżej, a następnie kontynuowaniem opisanych w tym artykule. Poniżej opisano podstawowe definicji i struktury zasady, jak zasady uzyskiwanie obliczane i zapewnia różne przykłady definicje zasad.

* [Zarządzanie zasobami i kontrolowania dostępu za pomocą zasad](../resource-manager-policy.md)

## <a name="define-a-policy-for-your-virtual-machine"></a>Opracowanie zasad komputera wirtualnych

Jeden z typowych scenariuszy w przedsiębiorstwie może być tylko umożliwić użytkownikom tworzenie maszyn wirtualnych z określonych systemów operacyjnych, które zostały przetestowane w celu zachowania zgodności z aplikacją LOB. Za pomocą zasad Menedżera zasobów Azure tego zadania można wykonywać w kilku kroków. W tym przykładzie zasad chwilę umożliwia tylko Ubuntu 14.04.2-LTS maszyn wirtualnych do utworzenia. Definicja zasad wygląda poniżej

```
"if": {
  "allOf": [
    {
      "field": "type",
      "equals": "Microsoft.Compute/virtualMachines"
    },
    {
      "not": {
        "allOf": [
          {
            "field": "Microsoft.Compute/virtualMachines/imagePublisher",
            "equals": "Canonical"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/imageOffer",
            "equals": "UbuntuServer"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/imageSku",
            "equals": "14.04.2-LTS"
          }
        ]
      }
    }
  ]
},
"then": {
  "effect": "deny"
}
```

Powyższe zasady łatwo można zmienić scenariuszy, w których może zajść umożliwia dowolny obraz KÓW Ubuntu może być używany do wdrażania maszyn wirtualnych z poniżej zmiany

```
{
  "field": "Microsoft.Compute/virtualMachines/imageSku",
  "like": "*LTS"
}
```

#### <a name="virtual-machine-property-fields"></a>Pola właściwości maszyn wirtualnych

W poniższej tabeli opisano właściwości maszyn wirtualnych, które mogą być używane jako pola w swojej definicji zasad. Aby uzyskać więcej informacji na temat zasad pola zobacz artykuł poniżej:

* [Pola i źródła](../resource-manager-policy.md#fields-and-sources)


| Nazwa pola     | Opis                                        |
|----------------|----------------------------------------------------|
| imagePublisher | Określa wydawcy obrazu               |
| imageOffer     | Określa oferty w programie publisher wybranego obrazu |
| imageSku       | Określa SKU dla wybranej oferty             |
| imageVersion   | Określa wersję obrazu dla wybranego SKU     |

## <a name="create-the-policy"></a>Tworzenie zasad

Zasady można łatwo tworzyć przy użyciu poleceń cmdlet środowiska PowerShell lub interfejsu API usługi REST bezpośrednio. Do tworzenia zasad, zobacz artykuł z wymienionych poniżej:

* [Tworzenie zasad](../resource-manager-policy.md#creating-a-policy)


## <a name="apply-the-policy"></a>Stosowanie zasad

Po utworzeniu zasady musisz go zastosować na określonego zakresu. Zakres może być subskrypcji, grupa zasobów lub nawet zasobów. Stosowania zasad, zobacz artykuł z wymienionych poniżej:

* [Tworzenie zasad](../resource-manager-policy.md#applying-a-policy)
