<properties 
   pageTitle="Debugowanie usług w chmurze Azure | Microsoft Azure"
   description="Debugowanie usług w chmurze Azure"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="debugging-cloud-services"></a>Debugowanie usług w chmurze

Za pomocą różnych metod do debugowania aplikacji Azure za pomocą narzędzi Azure dla programu Microsoft Visual Studio i Azure SDK:

- Azure aplikacji z programu Visual Studio można debugowania podczas tworzenia, tak jak w dowolnej aplikacji Visual C# lub języka Visual Basic. Aby uzyskać więcej informacji zobacz [Debugowanie usługi cloud na komputerze lokalnym](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-your-cloud-service-on-your-local-computer).

- Narzędzia diagnostyczne Azure służy do logowania szczegółowych informacji z kodu działającego w role, czy role są uruchomione w środowisku programowania lub platformy Azure. Aby uzyskać więcej informacji zobacz [Collecting rejestrowanie danych przy użyciu zestawu narzędzi Diagnostyka Azure](http://go.microsoft.com/fwlink/p/?LinkId=400450).

- Jeśli korzystasz z programu Visual Studio Enterprise napisać role przeznaczone dla użytkowników programu .NET Framework 4 i 4,5 .NET Framework, możesz włączyć IntelliTrace w czasie wdrażania usługi w chmurze Azure z programu Visual Studio. IntelliTrace zawiera dziennika, który z programem Visual Studio umożliwia debugowanie aplikacji tak, jakby były uruchomione w Azure. Aby uzyskać więcej informacji zobacz [Debugowanie usługi w chmurze opublikowanych z IntelliTrace i Visual Studio]( http://go.microsoft.com/fwlink/p/?LinkId=623016).

- Aby umożliwić zdalne debugowanie usług w chmurze w momencie, gdy wdrażanie usługi w chmurze z programu Visual Studio. Jeśli wybierzesz Włączanie debugowania zdalnego wdrożenia usługi zdalnej debugowania są zainstalowane na maszyn wirtualnych, uruchamianych każde wystąpienie roli. Te usługi, takie jak msvsmon.exe, nie wpływają na wydajność i powodują dodatkowych kosztów. Aby uzyskać więcej informacji zobacz [Debugowanie usługi w chmurze platformy Azure](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-a-cloud-service-in-azure).



