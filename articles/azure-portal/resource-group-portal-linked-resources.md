<properties 
    pageTitle="Zasoby pokrewne i połączone w galerii kafelków" 
    description="Informacje na temat powiązanych i połączonych zasobów, które są wyświetlane w galerii kafelków portal Azure preview." 
    services="azure-portal" 
    documentationCenter="" 
    authors="adamabdelhamed" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="azure-portal" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/16/2015" 
    ms.author="adamab"/>

# <a name="related-and-linked-resources-in-the-tile-gallery"></a>Zasoby pokrewne i połączone w galerii kafelków

Galeria kafelków umożliwia znajdowanie Kafelki dla określonego zasobu i przeciągnij je do bieżącego karta. Przy użyciu galerii kafelków, można utworzyć widoki zarządzania, obejmujące zasoby. Dla każdego zasobu określonego zasoby pokrewne obejmują wszystkie zasoby, które udostępnianie tej samej grupy zasobów jako zasobu i wszystkie zasoby, które łącze do lub z tego zasobu.

## <a name="linked-resources-in-azure-resource-manager"></a>Połączone zasobów w Menedżerze zasobów Azure

Łączenie to funkcja z Menedżera zasobów Azure.  Umożliwia deklarować relacje między zasoby, nawet jeśli nie znajdują się w tej samej grupy zasobów. Łączenie nie ma wpływu na środowisko uruchomieniowe zasobów, nie wpływu na rozliczenia i nie wpływu na dostępu oparta na rolach.  Jest po prostu mechanizmu, których możesz wskazać relacje tak, aby narzędzia, takie jak Galeria kafelków umożliwiają zarządzanie sformatowanego środowiska.  Sprawdzania łącza, za pomocą łączy interfejsu API i podaj zarządzanie relacjami niestandardowe napotkania także służy narzędziami. 

## <a name="how-do-i-link-my-resources"></a>Jak łącze zasoby?

Po utworzeniu zasobów za pośrednictwem portalu lub wdrażając szablonu za pomocą programu PowerShell Azure lub polecenie Azure łącza są tworzone automatycznie dla niektórych zależne zasobów. Zasoby można łączyć również programowo przy użyciu [Interfejsu API usługi REST połączonych zasobów](https://msdn.microsoft.com/library/azure/mt238499.aspx) lub deklarowanie relacje w szablonie. Aby uzyskać szczegółowe omówienie pracy z zasobami połączone zobacz [Linking zasobów w Menedżerze zasobów Azure](../resource-group-link-resources.md).

## <a name="next-steps"></a>Następne kroki

- Jeśli potrzebujesz wprowadzenie do pisania szablony Menedżera zasobów Azure, zobacz [Narzędzia do tworzenia szablonów](../resource-group-authoring-templates.md).
- Aby zagłębić się w bardziej szczegółowe informacje o tworzeniu połączeń między zasoby, zobacz [Łączenie zasobów w Menedżerze zasobów Azure](../resource-group-link-resources.md).
- Aby uzyskać więcej informacji na temat pracy z grupami zasobów za pośrednictwem portalu podglądu, zobacz [Korzystanie z Portal Azure podglądu do zarządzania zasobami Azure](resource-group-portal.md).
