<properties
 pageTitle="Harmonogram dostępności i niezawodności"
 description="Harmonogram dostępności i niezawodności"
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
 ms.topic="article"
 ms.date="08/16/2016"
 ms.author="deli"/>


# <a name="scheduler-high-availability-and-reliability"></a>Harmonogram dostępności i niezawodności

## <a name="azure-scheduler-high-availability"></a>Harmonogram Azure dostępności

Jak podstawowego platformy Azure usługi Azure harmonogram wysokiej dostępności i funkcji w zarówno wdrażanie usługi zbędne geo i replikacji regionalne geo zadania.

### <a name="geo-redundant-service-deployment"></a>Wdrażanie usługi zbędne Geo

Harmonogram Azure jest dostępna za pośrednictwem interfejsu użytkownika w regionie prawie wszystkie geo, który znajduje się w Azure dzisiaj. Lista regionów, które harmonogram Azure jest dostępna w znajduje się [na liście](https://azure.microsoft.com/regions/#services). Jeśli centrum danych w regionie hostowanej staje się niedostępna, możliwości przeniesienia awaryjnego harmonogramu Azure są takie, że usługa jest dostępna z innego centrum danych.

### <a name="geo-regional-job-replication"></a>Zadanie regionalne Geo replikacji

Nie tylko jest zewnętrzną, dostępne dla żądania zarządzania, ale własne zadania jest również replikowane geo harmonogram Azure. Jeśli istnieje awarii w jednym regionie, harmonogram Azure kończy się niepowodzeniem, na i zapewnia uruchamiania zadania z innym centrum danych w parach regionu geograficznego.

Na przykład po utworzeniu zadanie w południe centralnej nam, harmonogram Azure automatycznie replikuje to zadanie w Północnej centralnej US. Przypadku awarii w południe centralnej nam, harmonogram Azure zapewnia uruchamiania zadania z Północna centralnej US. 

![][1]

W wyniku harmonogram Azure gwarantuje, że w tym samym pełniejsza regionu geograficznego w przypadku błędu Azure granicach danych. W wyniku nie potrzebujesz zduplikowane zadania tak, aby dodać wysokiej dostępności — harmonogram Azure automatycznie zapewnia funkcje wysokiej dostępności dla zadań.

## <a name="azure-scheduler-reliability"></a>Harmonogram Azure niezawodność

Harmonogram Azure gwarantuje własnej wysokiej dostępności i mają inne podejście do zadania utworzone przez użytkownika. Na przykład zadanie może wywołać punktu końcowego HTTP, która jest niedostępna. Harmonogram Azure próbuje jednak pomyślnie, wykonywanie pracy przez alternatywny opcjami na zajęcie się błąd. Harmonogram Azure powinien się tym zająć na dwa sposoby:

### <a name="configurable-retry-policy-via-retrypolicy"></a>Można konfigurować zasady ponów próbę za pośrednictwem "retryPolicy"

Harmonogram Azure umożliwia konfigurowanie zasad ponów próbę. Domyślnie jeśli zadanie nie powiedzie się, harmonogram spróbuje zadanie ponownie cztery razy, w odstępach 30-sekundowe. Ponownie skonfigurowaniu tych zasad ponów próbę być skuteczniejsze (na przykład 10 razy w odstępach 30-sekundowe) lub im (na przykład dwa razy codziennie.)

Na przykład gdy może to ułatwić możesz utworzyć zadanie, które jest uruchamiany raz w tygodniu, a następnie wywołuje punktu końcowego HTTP. Jeśli punkt końcowy HTTP działa przez kilka godzin, gdy jest uruchomiony zadania, nie można oczekiwać więcej tydzień dla zadania ponownie uruchomić, ponieważ nie powiedzie się nawet domyślnych zasad ponów próbę. W takich przypadkach może być skonfigurowanie zasady standardowe ponów próbę próbować co trzy godziny (na przykład) zamiast co 30 sekund.

Aby dowiedzieć się, jak skonfigurować zasady ponów próbę, skorzystaj z [retryPolicy](scheduler-concepts-terms.md#retrypolicy).

### <a name="alternate-endpoint-configurability-via-erroraction"></a>Alternatywny dużych możliwości konfigurowania punktu końcowego za pośrednictwem "errorAction"

Jeśli punkt końcowy docelowej dla zadania harmonogramu Azure pozostaje niedostępne, harmonogram Azure przechodzi do naprzemiennych końcowy obsługi błędów po wykonaniu polityki ponów próbę. Jeśli skonfigurowano alternatywny końcowy obsługi błędów, harmonogram Azure wywołuje go. Z punktem końcowym alternatywny własne zadania są bardzo dostępne obliczu błąd.

Na przykład na poniższej ilustracji harmonogram Azure wykonuje polityki ponów próbę trafienie Nowy Jork Usługa sieci web. Po ponowne próby kończą się niepowodzeniem, sprawdza, czy istnieje alternatywny. Następnie przechodzi do przodu i uruchamia udostępnianie żądania alternatywnymi przy użyciu tych samych zasad ponów próbę.

![][2]

Zauważ, że te same zasady ponów próbę dotyczy cofniętą akcję i akcja alternatywny błędu. Użytkownik może również być akcji błędu alternatywny typ akcji różnić się od typ akcji głównym akcji. Na przykład podczas Akcja głównym może wywoływanie punktu końcowego HTTP, błąd czynności zamiast tego może być kolejki miejsca do magazynowania, kolejka bus serwisowych lub usługa bus tematu akcję, która ma rejestrowanie błędów.

Aby dowiedzieć się, jak skonfigurować punkt końcowy alternatywny, zapoznaj się z [errorAction](scheduler-concepts-terms.md#action-and-erroraction).

## <a name="see-also"></a>Zobacz też

 [Co to jest harmonogram?](scheduler-intro.md)

 [Azure pojęcia harmonogram, terminologia i hierarchia jednostki](scheduler-concepts-terms.md)

 [Wprowadzenie do korzystania z harmonogramu w portalu Azure](scheduler-get-started-portal.md)

 [Plany i rozliczenia w harmonogramie Azure](scheduler-plans-billing.md)

 [Sposoby tworzenia złożonych harmonogramów i zaawansowane cyklu z harmonogramem Azure](scheduler-advanced-complexity.md)

 [Odwołanie interfejsu API usługi REST harmonogram Azure](https://msdn.microsoft.com/library/mt629143)

 [Dokumentacja dotycząca Azure poleceń cmdlet środowiska PowerShell harmonogramu](scheduler-powershell-reference.md)

 [Azure limity harmonogram, wartości domyślne i kody błędów](scheduler-limits-defaults-errors.md)

 [Azure uwierzytelniania ruchu wychodzącego harmonogramu](scheduler-outbound-authentication.md)


[1]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image1.png

[2]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image2.png
