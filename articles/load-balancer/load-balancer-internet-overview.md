
<properties
   pageTitle="Internet przeciwległych Omówienie równoważenia obciążenia | Microsoft Azure "
   description="Omówienie dostępnych równoważenia obciążenia i jego funkcji przez Internet. Jak równoważenia obciążenia działa w przypadku Azure za pomocą maszyn wirtualnych i usług w chmurze."
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />


# <a name="internet-facing-load-balancer-overview"></a>Omówienie równoważenia obciążenia przeciwległych Internet

Usługi równoważenia obciążenia Azure mapy publiczny adres IP i port numer ruchu przychodzącego do prywatnych adresów IP adres i numer portu maszyny wirtualnej i odwrotnie ruchu odpowiedzi z maszyny wirtualnej. Zasady równoważenia obciążenia umożliwia rozpowszechnianie określone typy ruchu między wieloma maszyn wirtualnych lub usług. Na przykład można rozmieszczać Załaduj ruchu w sieci web żądania w wielu serwerów sieci web lub role w sieci web.

Dla usługi w chmurze, zawierającą wystąpienia role w sieci web lub role pracownika można określić punkt końcowy publicznej w pliku definicji (.csdef) usługi.

Plik _servicedefinition.csdef_ zawiera konfigurację punktu końcowego i gdy masz wielu wystąpień roli wdrożenia roli sieci web lub pracownika, równoważenia obciążenia będzie konfiguracji dla niego. Zliczanie wystąpień na plik konfiguracyjny usługi (.csfg) zmienia się sposobem dodania wystąpienia do wdrożenia chmury.

Na poniższej ilustracji pokazano równoważenia obciążenia punkt końcowy dla ruchu w sieci web zaszyfrowane współużytkowany trzy maszyn wirtualnych do publicznych i prywatnych port TCP 443. Następujące trzy maszyn wirtualnych znajdują się w zestawie równoważenia obciążenia.

![przykład równoważenia obciążenia publicznej](./media/load-balancer-internet-overview/IC727496.png))

Rysunek 1 - równoważenia obciążenia punkt końcowy dla ruchu w sieci web zaszyfrowane

Gdy klienci Internet wysyłać żądania strony sieci web do publiczny adres IP usługi w chmurze na TCP port 443, usługi równoważenia obciążenia Azure rozdziela żądania między trzema maszyn wirtualnych w zestawie równoważenia obciążenia. Aby uzyskać więcej informacji na temat algorytmów równoważenia obciążenia zobacz [Ładowanie strony Omówienie równoważenia](load-balancer-overview.md#load-balancer-features).

Domyślnie równoważenia obciążenia Azure rozdziela ruch sieciowy równomiernie wielu wystąpień maszyn wirtualnych. Można również skonfigurować koligacja sesji, aby uzyskać więcej informacji, zobacz [Tryb rozkładu równoważenia obciążenia](load-balancer-distribution-mode.md).

## <a name="next-steps"></a>Następne kroki

Informacje o [wewnętrznego Usługa równoważenia obciążenia](load-balancer-internal-overview.md) lepiej zrozumieć, które równoważenia obciążenia jest lepszym rozwiązaniem dla wdrożenia chmury.

Możesz także [rozpocząć tworzenie internetowej przeciwległych równoważenia obciążenia](load-balancer-get-started-internet-arm-ps.md) i konfigurowanie jakiego rodzaju [Tryb rozkładu](load-balancer-distribution-mode.md) zachowanie ruch sieci równoważenia obciążenia określonych.

Jeśli aplikacja wymaga do utrzymywania połączenia przy życiu dla serwerów za równoważenia obciążenia, można zrozumieć więcej o [Ustawienia limitu czasu bezczynności TCP dla usługi równoważenia obciążenia](load-balancer-tcp-idle-timeout.md). Może pomóc w Aby uzyskać informacje o zachowanie bezczynne połączenia, korzystając z usługi równoważenia obciążenia Azure.
