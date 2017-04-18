<properties
    pageTitle="Tworzenie nazw Bus usługi, za pomocą portalu Azure | Microsoft Azure"
    description="Aby rozpocząć pracę z Bus usługi, musisz przestrzeń nazw. Oto jak je utworzyć za pomocą portalu Azure."
    services="service-bus"
    documentationCenter=".net"
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="jotaub"/>

# <a name="create-a-service-bus-namespace-using-the-azure-portal"></a>Tworzenie nazw Bus usługi, za pomocą portalu Azure

Przestrzeń nazw jest kontenerem wspólne dla wszystkich wiadomości składników. Wielu kolejek i tematy mogą znajdować się w jednym nazw i nazw często służyć jako kontenerów aplikacji. Obecnie są dwa różne sposoby tworzenia nazw Bus usługi.

1.  Portal Azure (ten artykuł)

2.  [Szablony Menedżera zasobów][create-namespace-using-arm]

## <a name="create-a-namespace-in-the-azure-portal"></a>Tworzenie obszaru nazw w portalu Azure

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Gratulacje! Teraz utworzono przestrzeń nazw usługi Bus wiadomości.

## <a name="next-steps"></a>Następne kroki

Zapoznaj się z naszą [próbki GitHub] [ github-samples] której pokazano niektóre bardziej zaawansowanych funkcji Azure usługi Bus wiadomości.

[create-namespace-using-arm]: service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples