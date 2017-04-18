<properties
   pageTitle="Aplikacja sieci web skalowalna | Architektura Azure odwołanie | Microsoft Azure"
   description="Poprawa skalowalność w aplikacji sieci web w programie Microsoft Azure."
   services="app-service,app-service\web,sql-database"
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/12/2016"
   ms.author="mwasson"/>


# <a name="improving-scalability-in-a-web-application"></a>Zwiększanie skalowalność w aplikacji sieci web 

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

W tym artykule przedstawiono zalecany architektura poprawy skalowalność i wydajność w aplikacji sieci web uruchomionych Microsoft Azure. Architektura opiera się na [Architektura Azure odwołania: aplikacji sieci web podstawowe][basic-web-app]. Zalecenia i zagadnienia w tym artykule dotyczą również tej architektury.

>[AZURE.NOTE] Azure ma dwa różne wdrożenia modele: Menedżer zasobów i klasyczny. W tym artykule używa Menedżera zasobów, które firma Microsoft zaleca się w przypadku wdrożeń nowy.

## <a name="architecture-diagram"></a>Diagram architektury

![[0]][0]

Architektura zawiera następujące składniki:

- **Grupa zasobów**. [Grupa zasobów] [ resource-group] to kontener logiczny Azure zasobów. 

- ** [Aplikacji web app] [ app-service-web-app] ** i ** [aplikacji interfejsu API][app-service-api-app]**. Typowa aplikacja nowoczesnego mogą zawierać zarówno witryny sieci Web i co najmniej jeden RESTful w sieci web interfejsów API. Interfejsu API sieci web może być zużyte, przez klientów przeglądarki za pośrednictwem AJAX, przez aplikacje klientami lub aplikacji po stronie serwera. Aby uzyskać informacje w sieci web projektowania interfejsów API, zobacz [wskazówki dotyczące projektowania interfejsu API][api-guidance].    

- **WebJob**. Używanie [Azure WebJobs] [ webjobs] do wykonywania zadań długim w tle. WebJobs można uruchamiać zgodnie z harmonogramem dalej, lub w odpowiedzi na wyzwalacza, takie jak wprowadzanie wiadomości w kolejce. WebJob działa w tle w kontekście aplikacji usługi aplikacji. 

- **Kolejka**. W architekturze tu kolejek aplikacji pozycję zadania w tle przez umieszczanie wiadomości na [Magazyn kolejki Azure] [ queue-storage] kolejki. Wiadomość uaktywnia funkcję WebJob. 

    Można też kliknąć można zastosować kolejki Bus usługi. W celu porównania, zobacz [kolejki Azure i kolejki usługi Bus - porównaniu i porównanie oznaczonymi][queues-compared].

- **Pamięci podręcznej**. Częściowo statycznych dane są przechowywane w [Pamięci podręcznej Azure Redis][azure-redis].  

- **Sieci CDN**. [Sieci dostarczania zawartości Azure] za pomocą[ azure-cdn] (CDN) do publicznej zawartości pamięci podręcznej, mniejsze opóźnienia i szybsze dostarczania zawartości.

- **Magazynowanie danych.** Używanie [Bazy danych SQL Azure] [ sql-db] dla danych relacyjnych. -Relacyjnych danych, należy rozważyć, czy w sklepie NoSQL, takich jak magazyn tabel platformy Azure lub [DocumentDB][documentdb].

- **Wyszukiwanie azure**. Za pomocą funkcji [Wyszukiwania Azure] [ azure-search] dodać funkcję wyszukiwania, w tym sugestie dotyczące wyszukiwania, rozmyty wyszukiwania i wyszukiwanie specyficzne dla języka. Azure wyszukiwania jest zazwyczaj używany w połączeniu z innego magazynu danych, zwłaszcza jeśli magazynu podstawowego danych wymaga ścisłych spójności. W tej metody czy autorytatywnych danych w magazynie danych, a wprowadzone indeksu wyszukiwania do wyszukiwania Azure. Można także Azure wyszukiwania do skonsolidowania indeks wyszukiwania jednego z wielu baz danych.  

- **Wiadomości e-mail i wiadomości SMS**. Jeśli aplikacja wymaga do wysyłania wiadomości e-mail lub wiadomości SMS, za pomocą usługi innych firm, takich jak SendGrid lub Twilio, zamiast tworzenia tej funkcji bezpośrednio do aplikacji.

## <a name="recommendations"></a>Zalecenia

### <a name="app-service-apps"></a>Aplikacje aplikacji usługi 

Zaleca się tworzenie aplikacji sieci web oraz interfejsu API sieci web jako osobne aplikacje aplikacji usługi. Tego projektu pozwala uruchomić je w osobnych planów usług aplikacji, które z kolei pozwala skalować je niezależnie od siebie. Jeśli nie potrzebujesz skalowalność na tym samym poziomie najpierw możesz wdrażanie aplikacji do tego samego planu i przenieść je do oddzielnych planów później, jeśli to konieczne. (W przypadku planów Basic, Standard i Premium konta wystąpień maszyn wirtualnych w planie, nie dla aplikacji. Zobacz [ceny aplikacji usługi][app-service-pricing])

Jeśli zamierzasz użyć *Łatwe tabel* lub *Łatwe interfejsy API* funkcje Mobile usługi aplikacji, w tym celu utwórz osobne aplikacji usługi aplikacji.  Te funkcje są oparte na ramy określonej aplikacji, aby umożliwić im.

### <a name="webjobs"></a>WebJobs

Jeśli WebJob jest ilości zasobów, warto rozważyć wdrożenie go na puste aplikacji aplikacji usługi w osobne planu aplikacji usługi, aby zapewnić dedykowanego wystąpienia WebJob. Zobacz [wskazówki na temat zadań tła][webjobs-guidance].  

### <a name="cache"></a>Pamięć podręczna

Można poprawić wydajność i skalowalność przy użyciu [Pamięci podręcznej Azure Redis] [ azure-redis] buforować niektóre dane. Należy rozważyć użycie Redis pamięci podręcznej:

- Dane transakcji statycznych.

- Stan sesji.

- Dane wyjściowe HTML. Może to być przydatne w aplikacji renderowanych złożonych wyników HTML. 

Aby uzyskać bardziej szczegółowe wskazówki dotyczące projektowania strategii buforowania, zobacz [pamięci podręcznej wytycznych][caching-guidance].

### <a name="cdn"></a>SIECI CDN 

[Sieci CDN Azure] za pomocą[ azure-cdn] na zawartość statyczną pamięci podręcznej. Największą zaletą CDN jest zmniejszenie opóźnienie dla użytkowników, ponieważ zawartość jest buforowana na *serwerze granicznym* geograficznie jest zbliżony użytkownika. Sieci CDN można również zmniejszyć obciążenia na pasku aplikacji, ponieważ ruch nie jest obsługiwane przez aplikację.

- Jeśli aplikacji obejmuje przeważnie statycznych stron, rozważ użycie CDN buforować całą aplikację. Informacje na temat [użyć Azure CDN w usłudze Azure aplikacji][cdn-app-service].

- W przeciwnym razie umieszczenie zawartość statyczną, takimi jak obrazy, CSS i HTML pliki, do magazynowania Azure i użyj CDN buforowania tych plików. Zobacz [Integracja konto miejsca do magazynowania z sieci CDN][cdn-storage-account].

> [AZURE.NOTE] Azure CDN nie może służyć zawartości, która wymaga uwierzytelniania.

Aby uzyskać bardziej szczegółowe wskazówki, zobacz [wskazówki na temat sieci dostarczania zawartości (CDN)][cdn-guidance]. 

### <a name="storage"></a>Miejsca do magazynowania

Nowoczesne aplikacje często procesu dużych ilości danych. Aby skali w chmurze, należy wybrać typ przechowywania. Poniżej przedstawiono kilka zaleceń według planu bazowego.  Aby uzyskać bardziej szczegółowe wskazówki, zobacz [Oceny możliwości magazynu danych rozwiązań utrzymywanie Polyglot][polyglot-storage].

Mają być przechowywane | Przykład | Zalecane miejsca do magazynowania
--- | --- | ---
Pliki | Obrazy, dokumenty, pliki PDF | Magazyn obiektów Blob platformy Azure
Pary klucz-wartość | Wyszukiwane przy użyciu Identyfikatora użytkownika danych profilu użytkownika | Magazyn tabel platformy Azure
Krótkie wiadomości mają wyzwalanie dalszego przetwarzania | Żądania | Magazyn kolejek Azure, kolejki Bus usługi lub usługa Bus tematu
-Relacyjne dane ze schematem elastyczne, wymaganie podstawowe kwerendy | Katalog produktów | Dokument bazy danych, takich jak Azure DocumentDB, MongoDB lub Apache CouchDB
Danych relacyjnych wymagające bardziej rozbudowane obsługę kwerend, ścisłych schematu i/lub znaczący spójności | Magazyn produktów | Baza danych SQL Azure

## <a name="scalability-considerations"></a>Zagadnienia dotyczące skalowalność

### <a name="app-service-app"></a>Aplikacja aplikacji usługi

Jeśli rozwiązanie zawiera kilka aplikacje aplikacji usługi, należy rozważyć, czy wdrażanie ich oddzielanie plany aplikacji usługi. Tej metody można skalować je niezależnie od tego, ponieważ są uruchamiane w osobnych wystąpień. Aby uzyskać więcej informacji na temat Skalowanie zewnętrzne, zobacz [zagadnienia skalowalność] [ basic-web-app-scalability] sekcji [Architektura aplikacji sieci web podstawowe][basic-web-app].

Podobnie warto rozważyć wprowadzenie WebJob do własnej planu, dlatego zadania w tle nie działają w tym samym wystąpienia, które obsługują żądania HTTP.  

### <a name="sql-database"></a>Baza danych SQL

Zwiększanie skalowalność z bazą danych SQL *sharding* bazy danych &mdash; oznacza to, że poziomie podziału bazy danych. Sharding umożliwia poziomo skalowania bazy danych za pomocą [Narzędzia bazy danych elastyczną][sql-elastic]. Potencjalne zalety sharding:

- Lepsze przepustowość transakcji.

- Kwerendy działa szybciej nad podzestawu danych. 

### <a name="azure-search"></a>Azure wyszukiwania

Azure wyszukiwania usuwa ogólnych wykonywania wyszukiwania złożonych danych z magazynu danych podstawowy, a można skalować do obsługi ładowania. Zobacz [poziomy zasobów skali kwerendy i indeksowania obciążenia w wyszukiwaniu Azure][azure-search-scaling].

## <a name="security-considerations"></a>Zagadnienia dotyczące zabezpieczeń

### <a name="cross-origin-resource-sharing-cors"></a>Zasób Origin krzyżowe udostępniania (CORS)

Po utworzeniu witryny sieci Web i interfejsu API sieci web jako osobne aplikacje witryny sieci Web nie można nawiązywać połączenia AJAX po stronie klienta API, chyba że włączysz CORS. 

> [AZURE.NOTE] Poziom zabezpieczeń przeglądarki uniemożliwia wprowadzanie żądania AJAX do innej domeny przez strony sieci web. To ograniczenie nosi zasady takie samo pochodzenie i zapobiega niebezpieczną witrynę odczytu sentitive danych z innej witryny. CORS jest standard W3C, który umożliwia serwera złagodzenie zasad samego pochodzenia i umożliwić niektóre żądania origin krzyżowe podczas odrzucając innych osób.

Aplikacja usługi ma wbudowaną obsługę CORS, bez konieczności programowania aplikacji. Zobacz [Consume aplikacji interfejsu API z kodu JavaScript za pomocą CORS][cors]. Dodawanie witryny sieci Web do listy dozwolonych miejsc pochodzenia dla interfejsu API. 

### <a name="sql-database-encryption"></a>Szyfrowanie bazy danych SQL

[Przezroczyste szyfrowanie danych] za pomocą[ sql-encryption] potrzeby szyfrowania danych spoczynku w bazie danych. Ta funkcja wykonuje w czasie rzeczywistym szyfrowania i odszyfrowywania całej bazy danych (w tym pliki dziennika transakcji i kopie zapasowe) bez wprowadzania zmian w aplikacji. Szyfrowanie Dodaj niektórych opóźnienie, aby najlepiej oddzielanie dane, które musi być bezpieczna do własnej bazy danych i włączanie szyfrowania tylko dla tej bazy danych.  

## <a name="next-steps"></a>Następne kroki

- Wyższą dostępność wdrażanie aplikacji w więcej niż jeden region i za pomocą [Menedżera ruch Azure] [ tm] do przełączania awaryjnego. Aby uzyskać więcej informacji, zobacz [Architektura Azure odwołania: aplikacja sieci Web o wysokiej dostępności][web-app-multi-region].    

<!-- links -->

[api-guidance]: ../best-practices-api-design.md
[app-service-web-app]: ../app-service-web/app-service-web-overview.md
[app-service-api-app]: ../app-service-api/app-service-api-apps-why-best-platform.md
[app-service-pricing]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[azure-cdn]: https://azure.microsoft.com/en-us/services/cdn/
[azure-redis]: https://azure.microsoft.com/en-us/services/cache/
[azure-search]: https://azure.microsoft.com/en-us/documentation/services/search/
[azure-search-scaling]: ../search/search-capacity-planning.md
[background-jobs]: ../best-practices-background-jobs.md
[basic-web-app]: guidance-web-apps-basic.md
[basic-web-app-scalability]: guidance-web-apps-basic.md#scalability-considerations 
[caching-guidance]: ../best-practices-caching.md
[cdn-app-service]: ../app-service-web/cdn-websites-with-cdn.md
[cdn-storage-account]: ../cdn/cdn-create-a-storage-account-with-cdn.md
[cdn-guidance]: ../best-practices-cdn.md
[cors]: ../app-service-api/app-service-api-cors-consume-javascript.md
[documentdb]: https://azure.microsoft.com/en-us/documentation/services/documentdb/
[polyglot-storage]: https://github.com/mspnp/azure-guidance/blob/master/Polyglot-Solutions.md
[queue-storage]: ../storage/storage-dotnet-how-to-use-queues.md
[queues-compared]: ../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md
[resource-group]: ../resource-group-overview.md
[sql-db]: https://azure.microsoft.com/en-us/documentation/services/sql-database/
[sql-elastic]: ../sql-database/sql-database-elastic-scale-introduction.md
[sql-encryption]: https://msdn.microsoft.com/en-us/library/dn948096.aspx
[tm]: https://azure.microsoft.com/en-us/services/traffic-manager/
[web-app-multi-region]: ./guidance-web-apps-multi-region.md
[webjobs-guidance]: ../best-practices-background-jobs.md
[webjobs]: ../app-service/app-service-webjobs-readme.md
[0]: ./media/blueprints/paas-web-scalability.png "Aplikacji sieci Web platformy Azure o zwiększona skalowalność"