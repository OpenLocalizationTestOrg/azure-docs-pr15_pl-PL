<properties
   pageTitle="Usługa Azure tkaninie w systemie Linux | Microsoft Azure"
   description="Klastrów tkaninie usługi pomocy technicznej Linux i Java, co oznacza możesz wdrożyć i aplikacji tkaninie usługi hosta napisana Java i C# w systemie Linux."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="Java"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/26/2016"
   ms.author="SubramaR"/>

# <a name="service-fabric-on-linux"></a>Usługa tkaninie w systemie Linux

Podgląd tkaninie usługi w systemie Linux umożliwia tworzenie i wdrażanie oraz zarządzanie wysokiej dostępności, wysoce skalowalna aplikacji w systemie Linux, tak jak w systemie Windows. Ramy tkaninie usługi (zaufanego usług i zaufanego aktorów) są dostępne w Java w systemie Linux oprócz C# (.NET Core).  Można również tworzyć [gościa wykonywalny usługi](service-fabric-deploy-existing-app.md) przy użyciu języka ani framework. Ponadto Podgląd obsługuje także orchestrating kontenery Docker. Kontenery docker można uruchomić plików wykonywalnych gości lub natywnych usług tkaninie usług, które za pomocą struktury tkaninie usługi.

Usługa tkaninie w systemie Linux odpowiada koncepcji tkaninie usługi w systemie Windows (z wyjątkiem szczegółów systemu operacyjnego i obsługa języka programowania). W związku z tym większość naszą [dokumentację istniejących](http://aka.ms/servicefabricdocs) dotyczy ułatwiają zapoznanie się z technologią.

> [AZURE.VIDEO service-fabric-linux-preview]

## <a name="supported-operating-systems-and-programming-languages"></a>Obsługiwane systemy operacyjne i formularzy

Ograniczone Podgląd obsługuje tworzenie klastrów rozwoju jednego pola oprócz wielokrotnego stanowiska klastrów platformy Azure działa 16.04 serwera Ubuntu. Podgląd obsługuje zaufanego uczestnikami i struktury zaufanego Stateless usług w Java i C# oprócz plików wykonywalnych gościa i orchestrating Docker kontenerów.  

>[AZURE.NOTE] Niezawodne zbiory nie są jeszcze obsługiwane w Linux. Nie są obsługiwane tylko klastrów podstawy albo — tylko jedno pole i Azure Linux wielokrotnego stanowiska klastrów są obsługiwane w podglądzie.

## <a name="supported-tooling"></a>Obsługiwane narzędzia

Podgląd obsługuje interakcji z klastrem za pośrednictwem interfejsu wiersza polecenia Azure. Dla deweloperów języka Java Integracja z Zaćmienie i Yeoman jest dostarczany z Zaćmienie obsługiwane na Linux i OSX. Integracja OSX używa maszyny Linux w obszarze Ustawienia zaawansowane za pośrednictwem vagrant. W przypadku C# deweloperów Integracja z Yeoman znajduje się do generowania szablony aplikacji.

## <a name="next-steps"></a>Następne kroki


1. Zapoznaj się ze środowisk programowania [Zaufanego uczestników](service-fabric-reliable-actors-introduction.md) i [Niezawodne usługi](service-fabric-reliable-services-introduction.md) .

2. [Przygotowywanie środowiska programowania w systemie Linux](service-fabric-get-started-linux.md)

3. [Przygotowywanie środowiska programowania na OSX](service-fabric-get-started-mac.md)

4. [Tworzenie pierwszej aplikacji usługi tkaninie Java w systemie Linux](service-fabric-create-your-first-linux-application-with-java.md)
