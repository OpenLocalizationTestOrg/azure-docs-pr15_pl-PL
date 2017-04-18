<properties
   pageTitle="Omówienie zabezpieczeń internetowych rzeczy | Microsoft Azure"
   description=" Azure internet usług czynności (IoT) oferuje szeroką gamę możliwości. Ten artykuł ułatwia zrozumienie sposobu bezpiecznego rozwiązanie IoT platformy Azure. "
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/09/2016"
   ms.author="terrylan"/>

# <a name="internet-of-things-security-overview"></a>Omówienie zabezpieczeń internetowych elementów

Azure internet usług czynności (IoT) oferuje szeroką gamę możliwości. Te usługi kategorii enterprise umożliwiają:

- Zbierz dane od urządzenia
- Analizowanie danych strumieni w ruchu
- Przechowywanie i kwerendy dużych zestawów danych
- Wizualizowanie danych zarówno w czasie rzeczywistym i historyczne
- Integracja z systemami office Wstecz

Do przeprowadzania te funkcje, opakowania pakietu IoT Azure razem wielu usług Azure za pomocą rozszerzenia niestandardowe jako wstępnie rozwiązań. Te rozwiązania wstępnie są podstawowej implementacji typowe wzorce rozwiązanie IoT, które pomagają Aby skrócić czas, które można wykonać, aby usługi rozwiązań IoT. Przy użyciu IoT software development kit, można dostosować i rozszerzania tych rozwiązań do własnych wymagań. Umożliwia także te rozwiązania jako przykłady lub szablonów podczas tworzenia nowych rozwiązań IoT.

Pakiet Azure IoT to zaawansowane rozwiązanie dla potrzeb IoT. Jest jednak upmost znaczenie że rozwiązanie IoT zaprojektowano z zabezpieczeniami pamiętać od początku. Ze względu na znanej ogólnej liczby IoT urządzeń wszelkie zdarzeniem bezpieczeństwa szybko może stać się szerokie zdarzenie z znacząco wpłynąć.

Aby ułatwić zrozumienie sposobu zabezpieczenia rozwiązanie IoT, zawiera następujące informacje.

## <a name="security-architecture"></a>Architektura zabezpieczeń

Podczas projektowania systemu, należy zrozumieć potencjalnych zagrożeń do tego systemu i dodaj odpowiednie zabezpieczenia w związku z tym, jak system jest przeznaczona i zaprojektowane pod. Należy szczególnie do projektowania produktu od początku z myślą o bezpieczeństwie, ponieważ opis jak atakującej może być możliwe złamanie ułatwia, upewnij się, że odpowiednie czynniki znajdują się w miejscu od początku.

Architektura zabezpieczeń IoT można zapoznać, czytając [Internet rzeczy Architektura zabezpieczeń](../iot-suite/iot-security-architecture.md).

W tym artykule omówiono następujące tematy:

- [Rozpoczyna się Model zagrożenie bezpieczeństwa](../iot-suite/iot-security-architecture.md#security-starts-with-a-threat-model)
- [Zabezpieczenia programu IoT](../iot-suite/iot-security-architecture.md#security-in-iot)
- [Zagrożenia modelowania architektura Azure IoT odwołania](../iot-suite/iot-security-architecture.md#threat-modeling-the-azure-iot-reference-architecture)

## <a name="security-from-the-ground-up"></a>Zabezpieczenia od podstaw

IoT stanowi unikatowe wyzwania zabezpieczenia, prywatność i zgodność dla firm na całym świecie. W odróżnieniu od tradycyjnych cyber technologii miejsce, w którym te problemy obracać się wokół oprogramowania i jak jest stosowana IoT dotyczy, co się dzieje po cyber i fizycznie światem zbieżne. Ochrona IoT rozwiązań wymaga, zapewnianie bezpiecznego inicjowania obsługi urządzeń, bezpieczna łączność między tych urządzeń i chmury i ochronę danych w chmurze podczas przetwarzania i przechowywania. Jednak pracy przed takie funkcje, jest ograniczona zasobów urządzeń, rozmieszczenie geograficzne wdrożeń i dużej liczby urządzeń w obrębie rozwiązania.

Możesz dowiedzieć się, jak obsługiwać zabezpieczeń w następujących obszarach, czytając [zabezpieczeń Internet czynności od podstaw](../iot-suite/securing-iot-ground-up.md).

W tym artykule omówiono następujące tematy:

- [Bezpieczny infrastruktury od podstaw](../iot-suite/securing-iot-ground-up.md#secure-infrastructure-from-the-ground-up)
- [Microsoft Azure — bezpieczną infrastrukturę IoT dla swojej firmy](../iot-suite/securing-iot-ground-up.md#microsoft-azure---secure-iot-infrastructure-for-your-business)

## <a name="best-practices"></a>Najważniejsze wskazówki

Zabezpieczanie infrastruktury IoT wymaga rygorystyczne strategii zabezpieczeń w głębokości. Każdej warstwy, rozpoczynając od zabezpieczania danych w chmurze, ochrona integralności danych znajduje się w drodze publicznie w Internecie i daje możliwość bezpiecznego Inicjowanie obsługi urządzeń, tworzy gwarancję zabezpieczeń w całej infrastruktury.

Można znaleźć informacje o zabezpieczeniach Internet rzeczy najważniejsze wskazówki, czytając [Najważniejsze wskazówki dotyczące zabezpieczeń Internet rzeczy](../iot-suite/iot-security-best-practices.md).

W tym artykule omówiono następujące tematy:

- [Producent sprzętu IoT/integrator](../iot-suite/iot-security-best-practices.md#iot-hardware-manufacturerintegrator)
- [Deweloper rozwiązanie IoT](../iot-suite/iot-security-best-practices.md#iot-solution-developer)
- [Narzędzie wdrażania rozwiązania IoT](../iot-suite/iot-security-best-practices.md#iot-solution-deployer)
- [Operator rozwiązanie IoT](../iot-suite/iot-security-best-practices.md#iot-solution-operator)
