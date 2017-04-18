<properties
 pageTitle="Co to jest Azure harmonogram? | Microsoft Azure"
 description="Harmonogram Azure umożliwia deklaratywnie opisać działania, aby uruchomić w chmurze. Go, a następnie planuje i te akcje są uruchamiane automatycznie."
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="hero-article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="what-is-azure-scheduler"></a>Co to jest Azure harmonogram?

Harmonogram Azure umożliwia deklaratywnie opisać działania, aby uruchomić w chmurze. Go, a następnie planuje i te akcje są uruchamiane automatycznie.  Harmonogram powinien się tym zająć przy użyciu [Azure portal](scheduler-get-started-portal.md), kod, [Interfejsu API usługi REST](https://msdn.microsoft.com/library/mt629143.aspx)lub Azure programu PowerShell.

Harmonogram tworzy, przechowuje i wywołuje ilość pracy według harmonogramu.  Harmonogram nie obsługi dowolnego zadań lub uruchomić kod. Tylko kod _wywołuje_ hostowaną gdzie indziej IT — w Azure, lokalnego, lub u innego dostawcy. Wywołuje go za pośrednictwem protokołu HTTP, HTTPS, kolejki miejsca do magazynowania, kolejki bus usługi lub tematu bus usługi.

Harmonogram harmonogramy [zadań](scheduler-concepts-terms.md), powoduje Historia wyniki wykonania zadania, że jeden można przeglądać i deterministically i niezawodne planuje obciążenia mają być wykonywane. Azure WebJobs (część funkcji aplikacji sieci Web w usłudze Azure aplikacji) i inne funkcje planowania Azure za pomocą harmonogramu w tle. [Harmonogram interfejsu API usługi REST](https://msdn.microsoft.com/library/mt629143.aspx) ułatwia zarządzanie komunikacji dla tych działań. Jako takie harmonogram obsługuje [złożone harmonogramów i zaawansowane cyklu](scheduler-advanced-complexity.md) łatwe.

Istnieje kilka scenariuszy, które nadają się do zastosowania harmonogramu. Na przykład:

+ _Akcji aplikacji cykliczne:_ Okresowo zbieranie danych od Twitter do źródła danych.
+ _Dzienny Konserwacja:_ Dzienny oczyszczania dzienniki, wykonywanie kopii zapasowych i innych czynności. Na przykład administrator może wybrać do tworzenia kopii zapasowej bazy danych 1 o godzinie: 00 każdy dzień następnego 9 miesięcy.

Harmonogram umożliwia tworzenie, aktualizowanie, usuwanie, wyświetlanie i zarządzania zadaniami i [zbiory zadania](scheduler-concepts-terms.md) programistycznie, za pomocą skryptów i w portalu.

## <a name="see-also"></a>Zobacz też

 [Azure pojęcia harmonogram, terminologia i hierarchia jednostki](scheduler-concepts-terms.md)

 [Wprowadzenie do korzystania z harmonogramu w portalu Azure](scheduler-get-started-portal.md)

 [Plany i rozliczenia w harmonogramie Azure](scheduler-plans-billing.md)

 [Sposoby tworzenia złożonych harmonogramów i zaawansowane cyklu z harmonogramem Azure](scheduler-advanced-complexity.md)

 [Odwołanie interfejsu API usługi REST harmonogram Azure](https://msdn.microsoft.com/library/mt629143)

 [Dokumentacja dotycząca Azure poleceń cmdlet środowiska PowerShell harmonogramu](scheduler-powershell-reference.md)

 [Azure Harmonogram dostępności i niezawodności](scheduler-high-availability-reliability.md)

 [Azure limity harmonogram, wartości domyślne i kody błędów](scheduler-limits-defaults-errors.md)

 [Azure uwierzytelniania ruchu wychodzącego harmonogramu](scheduler-outbound-authentication.md)
