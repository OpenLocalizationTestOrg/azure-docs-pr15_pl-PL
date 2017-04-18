<properties 
    pageTitle="Izolacji aplikacji usługi Bus przed dostawie i awarii | Microsoft Azure"
    description="W tym artykule opisano technik, których możesz chronić aplikacje przed potencjalnych awarii usługi Bus."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="tysonn" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/02/2016"
    ms.author="sethm" />

# <a name="best-practices-for-insulating-applications-against-service-bus-outages-and-disasters"></a>Najważniejsze wskazówki dotyczące izolacji aplikacji przed dostawie Bus usługi i awarii

Kluczowych aplikacji musi działać w sposób ciągły, nawet w obecności niezaplanowane dostawie lub awarii. W tym temacie opisano technik, który służy do ochrony aplikacji usługi Bus przed potencjalnymi awarii usługi lub awarii.

Awaria jest definiowana jako tymczasowej niedostępności Bus usługi Azure. Awaria może wpłynąć na niektóre składniki Bus usług, takich jak magazyn wiadomości lub nawet całego centrum danych. Gdy problem został rozwiązany, Bus usługi staje się dostępna ponownie. Zazwyczaj awarii nie powoduje utratę wiadomości lub innych danych. Przykład awarii składnika jest niedostępności określonego magazynu wiadomości. Przykład awaria centrum danych na poziomie jest awarii zasilania centrum danych lub przełącznik sieci uszkodzony centrum danych. Awaria może trwać od kilku minut do kilku dni.

Po awarii jest definiowana jako nieodwracalnej utracie jednostki skali Bus usługi lub centrum danych. Centrum danych może lub nie może stać się niedostępne ponownie. Zwykle danych powoduje utratę niektórych lub wszystkich wiadomości lub innych danych. Przykłady awarii fire, zalewu lub trzęsienie ziemi.

## <a name="current-architecture"></a>Architektura bieżącego

Usługa Bus używa wielu magazynów wiadomości do przechowywania wiadomości, które są wysyłane do kolejki lub tematów. Partycją kolejki lub temat zostanie przypisany do jednego magazynu wiadomości. Jeśli ten magazyn wiadomości jest niedostępna, wszystkie operacje w tym kolejki lub tematu nie powiedzie się.

Wszystkie obiekty wiadomości usługi Bus (kolejek, tematy, przekaźniki) znajdują się w nazw usługa, która jest powiązana z centrum danych. Bus usługa nie obsługuje automatycznego replikacji geo danych i nie zezwala nazw obejmować wielu centrach danych.

## <a name="protecting-against-acs-outages"></a>Ochrona przed dostawie ACS

Jeśli korzystasz z poświadczeniami ACS i ACS staje się niedostępna, klienci mogą uzyskiwać już tokeny. Klienci, którzy mają tokenu w czasie awarii ACS nadal korzystać Bus usługi aż tokeny wygaśnie. Okres ważności tokenu domyślne to 3 godziny.

Aby chronić się przed dostawie ACS, za pomocą tokeny udostępniane podpis programu Access (SA). W tym przypadku klient uwierzytelnia bezpośrednio z usługi Bus logując samodzielnie minted token przy użyciu klucza tajnego. Połączenia do ACS już nie są wymagane. Aby uzyskać więcej informacji na temat tokeny skojarzeń zabezpieczeń zobacz [Uwierzytelnianie Bus usługi][].

## <a name="protecting-queues-and-topics-against-messaging-store-failures"></a>Ochrona kolejek i tematy przed wiadomości błędy sklepu

Partycją kolejki lub temat zostanie przypisany do jednego magazynu wiadomości. Jeśli ten magazyn wiadomości jest niedostępna, wszystkie operacje w tym kolejki lub tematu nie powiedzie się. Podzielony na partycje kolejki, z drugiej strony, składa się z wielu fragmentów. Każdy fragment jest przechowywany w innym magazynie wiadomości. Gdy wiadomość jest wysyłana do kolejki podzielone na partycje lub tematu, usługa Bus przypisuje wiadomość do jednego fragmentów. Jeśli odpowiedniego magazynu wiadomości jest niedostępna, Bus usługi zapisuje wiadomości do innego fragmentu, jeśli to możliwe. Aby uzyskać więcej informacji na temat jednostki podzielone na partycje Zobacz [partycją podmioty wiadomości][].

## <a name="protecting-against-datacenter-outages-or-disasters"></a>Ochrona przed dostawie centrum danych lub awarii

Aby zezwolić na trybie awaryjnym między dwoma centrach danych, możesz utworzyć nazw usługi Bus usługi w każdym centrum danych. Na przykład nazw usługi Bus usługi **contosoPrimary.servicebus.windows.net** mogą znajdować się w regionie Stanów Zjednoczonych Północna-centralnego i **contosoSecondary.servicebus.windows.net** mogą znajdować się w regionie nam południowo-centralnego. Jeśli Bus usługi wiadomości jednostki będą dostępne w obecności awaria centrum danych, możesz utworzyć tego obiektu w obu obszarach nazw.

Aby uzyskać więcej informacji zobacz sekcję "Błąd Bus usługi w Azure centrum danych" w [asynchroniczne wzorców wiadomości i wysokiej dostępności][].

## <a name="protecting-relay-endpoints-against-datacenter-outages-or-disasters"></a>Ochrona punkty końcowe przekazywania przed dostawie centrum danych lub awarii

Replikacja Geo punkty końcowe przekazywania umożliwia usługa, która udostępnia punkt końcowy przekazywania być dostępne w obecności dostawie Bus usługi. Aby osiągnąć geo replikacji, usługa musisz utworzyć dwa przekazywania punkty końcowe w różnych obszarach nazw. Obszary nazw muszą znajdować się w różnych centrach danych i dwa punkty końcowe muszą mieć różne nazwy. Na przykład podstawowego punktu końcowego można się z Tobą w obszarze **contosoPrimary.servicebus.windows.net/myPrimaryService**podczas jego odpowiednikiem pomocniczą można się z Tobą w obszarze **contosoSecondary.servicebus.windows.net/mySecondaryService**.

Usługa następnie wykrywa na oba punkty końcowe, a klient może wywoływanie usługi za pośrednictwem końców. Aplikacją kliencką losowo wybiera jedną przekaźniki jako punkt końcowy podstawowego i wysyła jego żądanie do aktywnego punktu końcowego. Jeśli kończy się niepowodzeniem z kodem błędu, tego błędu wskazuje, że punkt końcowy przekazywania nie jest dostępna. Aplikacja otwiera kanał kopii zapasowej punktu końcowego i wysyła zaproszenie. W tym momencie aktywności i kopii zapasowej punkty końcowe przełączanie ról: z aplikacją kliencką są uwzględnione stare aktywnego punktu końcowego jako nowy punkt końcowy kopii zapasowej i stare końcowy kopii zapasowej jako nowy punkt końcowy aktywne. Jeśli oba wysłać niepowodzenie operacji, role dwie jednostki pozostaną niezmienione i, jest zwracany błąd.

Przykładowe [Geo replikacji z wiadomościami przekazywanie Bus usługi][] przedstawiono sposób odtworzyć przekaźniki.

## <a name="protecting-queues-and-topics-against-datacenter-outages-or-disasters"></a>Ochrona kolejek i tematy przed dostawie centrum danych lub awarii

Aby osiągnąć odporność przed dostawie centrum danych za pomocą brokered wiadomości, usługa Bus obsługuje dwie metody: *aktywna* i *pasywne* replikacji. Dla każdego rozwiązaniem Jeśli danej kolejki lub temat będą dostępne w obecności awaria centrum danych, możesz utworzyć je w obu obszarach nazw. Obie te jednostki mogą mieć tej samej nazwy. Na przykład podstawowego kolejki można się z Tobą w obszarze **contosoPrimary.servicebus.windows.net/myQueue**podczas jego odpowiednikiem pomocniczą można się z Tobą w obszarze **contosoSecondary.servicebus.windows.net/myQueue**.

Jeśli aplikacji nie wymaga stałej łączności nadawcy do odbiorcy, aplikacja zaimplementować trwałe kolejki po stronie klienta, aby zapobiec utracie wiadomości i aby nadawcy z przejściowych błędy Bus usługi.

## <a name="active-replication"></a>Replikacja usługi Active

Replikacja Active używa jednostki w obu obszarach nazw dla każdej operacji. Klient wysyła wiadomość wysyła dwie kopie tej samej wiadomości. Pierwsza kopia jest wysyłana z obiektem podstawowym (na przykład **contosoPrimary.servicebus.windows.net/sales**), a druga kopia wiadomości są wysyłane do jednostki pomocnicze (na przykład **contosoSecondary.servicebus.windows.net/sales**).

Klient odbiera wiadomości z obu kolejek. Odbiorca przetwarza pierwszą kopię wiadomości, a druga kopia są pomijane. Aby wyłączyć zduplikowanych wiadomości, nadawca, musisz oznaczyć każdej wiadomości z unikatowym identyfikatorem. Obie kopie wiadomości muszą być oznakowanie o tym samym identyfikatorze. Za pomocą właściwości [BrokeredMessage.MessageId][] lub [BrokeredMessage.Label][] lub właściwość niestandardowa składnika Aby oznakować wiadomość. Odbiorca musi obsługiwać listę wiadomości, które zawiera już odebrane.

W przykładzie [Geo replikacji z wiadomościami Brokered Bus usługi][] pokazano aktywnej replikacji wiadomości podmiotów.

> [AZURE.NOTE] Metody aktywnej replikacji zwiększa dwukrotnie liczbę operacji, dlatego ta metoda może prowadzić do wyższy koszt.

## <a name="passive-replication"></a>Replikacja w stronie biernej

W przypadku usterek pasywne replikacji jest używany tylko jeden z dwóch obiektów wiadomości. Klient wysyła wiadomość do aktywnego obiektu. W przypadku niepowodzenia operacji dla aktywnego jednostki z kodem błędu, która wskazuje, że centrum danych, obsługującym aktywnej jednostki mogą być niedostępne, klient wysyła kopię wiadomości z jednostką kopii zapasowej. W tym momencie aktywności i kopii zapasowej podmioty przełączanie ról: wysyłanie klienta są uwzględnione stare active obiekt, który ma być nowy obiekt kopii zapasowej, a stare jednostki kopii zapasowej jest nowy obiekt aktywne. Jeśli oba wysłać niepowodzenie operacji, role dwie jednostki pozostaną niezmienione i, jest zwracany błąd.

Klient odbiera wiadomości z obu kolejek. Ponieważ istnieje prawdopodobieństwo, że odbiorca otrzyma dwie kopie tej samej wiadomości, odbiorca musi wyświetlanie zduplikowanych wiadomości. Można pominąć duplikaty w taki sam sposób, zgodnie z opisem aktywnej replikacji.

Na ogół pasywne replikacji jest bardziej ekonomicznych niż replikacja usługi active, ponieważ w większości przypadków przeprowadzana jest operacja tylko jeden. Opóźnienie i przepustowość i koszcie są identyczne scenariusz — replikowane.

Używając replikacji w stronie biernej, w następujących scenariuszach wiadomości zostanie utracone lub otrzymano dwa razy:

-   **Opóźnienie wiadomości lub utraty**: przyjęto założenie, że nadawcy pomyślnie wysłane m1 wiadomości do kolejki podstawowy, a następnie kolejki jest niedostępny, zanim odbiorca otrzyma m1. Nadawca wysyła m2 wyświetlony komunikat do kolejki pomocniczą. Jeśli podstawowy kolejka jest tymczasowo niedostępny, odbiorca otrzyma m1 po kolejka staje się dostępna ponownie. W przypadku awarii odbiorca nigdy nie może zostać wyświetlony m1.

-   **Duplikowanie odbioru**: przyjęto założenie, że nadawca wysyła wiadomość m do podstawowego kolejki. Usługa Bus pomyślnie przetwarza m, ale nie można wysyłać odpowiedzi. Po tej operacji wysyłania przekroczenie limitu czasu, nadawca wysyła identyczne kopię m do pomocniczej kolejki. Jeśli odbiorca jest możliwość uzyskania pierwszą kopię m przed podstawowego kolejki staje się niedostępna, odbiorca otrzyma kopię m w tym samym czasie. Jeśli odbiorca nie będzie mógł otrzymywać pierwszą kopię m przed podstawowego kolejki staje się niedostępna, odbiorca początkowo otrzymuje drugą kopię m, ale otrzyma drugą kopię m podstawowego kolejki stał się dostępny.

W przykładzie [Geo replikacji z wiadomościami Brokered Bus usługi][] pokazano pasywne replikacji wiadomości podmiotów.

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat awarii, zobacz następujące artykuły:

- [Zapewnianie ciągłości bazy danych Azure SQL][]
- [Wytyczne techniczne elastyczność Azure][]

  [Usługa Bus uwierzytelniania]: service-bus-authentication-and-authorization.md
  [Podzielony na partycje podmioty wiadomości]: service-bus-partitioning.md
  [Asynchroniczne wzorców wiadomości i wysokiej dostępności]: service-bus-async-messaging.md#failure-of-service-bus-within-an-azure-datacenter
  [Replikacja Geo z Bus usługi przekazywanie wiadomości]: http://code.msdn.microsoft.com/Geo-replication-with-16dbfecd
  [BrokeredMessage.MessageId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
  [BrokeredMessage.Label]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx
  [Replikacja Geo z Bus usługi Brokered wiadomości]: http://code.msdn.microsoft.com/Geo-replication-with-f5688664
  [Zapewnianie ciągłości bazy danych Azure SQL]: ../sql-database/sql-database-business-continuity.md
  [Wytyczne techniczne elastyczność Azure]: ../resiliency/resiliency-technical-guidance.md
