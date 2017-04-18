<properties
 pageTitle="Limity harmonogram i ustawieniami domyślnymi"
 description="Limity harmonogram i ustawieniami domyślnymi"
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
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="scheduler-limits-and-defaults"></a>Limity harmonogram i ustawieniami domyślnymi

## <a name="scheduler-quotas-limits-defaults-and-throttles"></a>Harmonogram przydziałów, limity, wartości domyślne i ograniczenia

[AZURE.INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## <a name="the-x-ms-request-id-header"></a>Nagłówek x-ms żądanie id

Każde żądanie na usługę Harmonogram zwraca nagłówka odpowiedzi o nazwie**x-ms żądanie id**. Nagłówek ten zawiera wartość nieprzezroczystości, która jednoznacznie identyfikuje żądanie.

Jeśli żądanie spójne nie działa prawidłowo i upewnieniu się, że wniosek jest prawidłowo opracowany, może być raportowanie błędów firmie Microsoft za pomocą tej wartości. W raporcie to wartość argumentu x-ms żądanie id, przybliżonego czasu, który wysłano żądanie, identyfikator subskrypcji, zbioru zadań, i/lub zadanie i typ operację, której próba wezwanie.

## <a name="see-also"></a>Zobacz też


 [Co to jest harmonogram?](scheduler-intro.md)

 [Azure pojęcia harmonogram, terminologia i hierarchia jednostki](scheduler-concepts-terms.md)

 [Wprowadzenie do korzystania z harmonogramu w portalu Azure](scheduler-get-started-portal.md)

 [Plany i rozliczenia w harmonogramie Azure](scheduler-plans-billing.md)

 [Odwołanie interfejsu API usługi REST harmonogram Azure](https://msdn.microsoft.com/library/mt629143)

 [Dokumentacja dotycząca Azure poleceń cmdlet środowiska PowerShell harmonogramu](scheduler-powershell-reference.md)

 [Azure Harmonogram dostępności i niezawodności](scheduler-high-availability-reliability.md)

 [Azure uwierzytelniania ruchu wychodzącego harmonogramu](scheduler-outbound-authentication.md)
