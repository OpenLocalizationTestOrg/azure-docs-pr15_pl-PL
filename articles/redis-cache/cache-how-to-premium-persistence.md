<properties 
    pageTitle="Jak skonfigurować utrzymywanie danych na Premium Azure Redis potrzeby pamięci podręcznej" 
    description="Dowiedz się, jak skonfigurować i zarządzać nimi utrzymywanie danych usługi Azure Redis w pamięci podręcznej wystąpień warstwa Premium" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/30/2016" 
    ms.author="sdanie"/>

# <a name="how-to-configure-data-persistence-for-a-premium-azure-redis-cache"></a>Jak skonfigurować utrzymywanie danych na Premium Azure Redis potrzeby pamięci podręcznej

Azure Redis pamięci podręcznej ma ofertą różnych pamięci podręcznej, które zapewniają elastyczność w wyborze rozmiar pamięci podręcznej i funkcji, takich jak nowy poziom Premium.

Poziom premium Azure Redis w pamięci podręcznej zawiera funkcje, takie jak klaster, utrzymywanie i obsługę wirtualnej sieci. Ten artykuł zawiera opis sposobu konfigurowania pozostawanie w przypadku Azure Redis w pamięci podręcznej premium.

Informacji o innych funkcjach pamięci podręcznej premium zobacz [Wprowadzenie do poziomu Azure Redis pamięci podręcznej Premium](cache-premium-tier-intro.md).

## <a name="what-is-data-persistence"></a>Co to jest utrzymywanie danych?
Utrzymywanie redis umożliwia utrzymują dane przechowywane w Redis. Możesz również migawek i tworzenia kopii zapasowych danych, które można załadować w przypadku awarii sprzętu. Jest to dużą zaletą na warstwie Basic lub standardowe miejsce, w którym wszystkie dane są przechowywane w pamięci i może być potencjalną utratą danych w przypadku awarii umiejscowienia węzły pamięci podręcznej w dół. 

Azure Redis pamięci podręcznej oferuje utrzymywanie Redis przy użyciu [modelu RDB](http://redis.io/topics/persistence), gdzie są przechowywane dane konto Azure miejsca do magazynowania. Po skonfigurowaniu trwałe pamięci podręcznej Azure Redis będzie nadal występował migawkę Redis pamięci podręcznej w formacie binarnym Redis na dysku na podstawie na można konfigurować częstotliwości kopii zapasowej. Jeśli wystąpi losowych zdarzenie, które wyłączają podstawowy i replice pamięć podręczną, pamięci podręcznej zrekonstruowaniu przy użyciu najnowszych migawkę.

Utrzymywanie można skonfigurować z karta **Nową pamięć podręczną Redis** podczas tworzenia pamięci podręcznej i karta **Ustawienia** dla istniejącego premium pamięci podręcznej.

## <a name="create-a-premium-cache"></a>Tworzenie pamięci podręcznej premium

Aby utworzyć pamięć podręczną i skonfigurować utrzymywanie, zaloguj się do [portalu Azure](https://portal.azure.com) i kliknij pozycję **Nowy**->**danych + miejsce**>**Redis pamięci podręcznej**.

![Tworzenie Redis pamięci podręcznej][redis-cache-new-cache-menu]

Aby skonfigurować utrzymywanie, najpierw zaznacz jedną z pamięci podręcznej **Premium** w karta **Wybierz z poziomu cennik** .

![Wybierz pozycję z poziomu cen][redis-cache-premium-pricing-tier]

Po premium ceny warstwa jest zaznaczony, kliknij przycisk **Redis trwałe**.

![Utrzymywanie redis][redis-cache-persistence]

W poniższej sekcji opisano sposób skonfigurowania utrzymywanie Redis w pamięci podręcznej premium nowy. Po skonfigurowaniu utrzymywanie Redis kliknij przycisk **Utwórz** , aby utworzyć z nową pamięć podręczną premium z utrzymywanie Redis.

## <a name="configure-redis-persistence"></a>Konfigurowanie utrzymywanie Redis

Redis utrzymywanie jest skonfigurowany na karta **Redis utrzymywanie danych** . Dla nowej pamięci podręcznej ta karta jest dostępna w trakcie procesu tworzenia pamięci podręcznej, zgodnie z opisem w poprzedniej sekcji. Karta **Redis utrzymywanie danych** jest dostępny dla istniejącej pamięci podręcznej, karta **Ustawienia** dla pamięci podręcznej.

![Redis ustawienia][redis-cache-settings]

Aby włączyć utrzymywanie Redis, kliknij **włączone** wykonywanie kopii zapasowych RDB (Redis bazy danych). Aby wyłączyć Redis pozostawanie w pamięci podręcznej premium wcześniej włączona, kliknij opcję **wyłączone**.

Aby skonfigurować interwał wykonywania kopii zapasowej, wybierz **Częstotliwość kopii zapasowej** z listy rozwijanej. Można wybierać **15 minut**, **30 minut**, **60 minut**, **6 godzin**, **12 godzin**i **24 godziny**. Ten interwał uruchomiony, licząc w dół po poprzedniej wykonywanie kopii zapasowej powiedzie i po jego upływie została zainicjowana nową kopię zapasową.

Kliknij **Konto miejsca do magazynowania** do przechowywania umożliwia wybór konta, a określanie **klucza podstawowego** lub **klucza pomocniczego** do używania z listy rozwijanej **Klucz magazynowania** . Należy wybrać konto miejsca do magazynowania w tym samym regionie jako pamięci podręcznej, a konto **Magazynowania Premium** jest zalecane, ponieważ magazynowania premium ma wyższej przepustowości. 

>[AZURE.IMPORTANT] Jeśli klucz miejsca do magazynowania dla Twojego konta utrzymywanie jest generowane, należy rechoose inny klawisz z listy rozwijanej **Klucz magazynowania** .

![Utrzymywanie redis][redis-cache-persistence-selected]

Kliknij **przycisk OK** , aby zapisać konfigurację trwałe.

Następna kopia zapasowa (lub pierwszej kopii zapasowej dla nowej pamięci podręcznej) jest inicjowane po czasu częstotliwość kopii zapasowej.



## <a name="persistence-faq"></a>Utrzymywanie — często zadawane pytania

Poniższa lista zawiera odpowiedzi na często zadawane pytania dotyczące utrzymywanie Azure Redis w pamięci podręcznej.

-   [Czy można włączyć pozostawanie w poprzednio utworzone pamięci podręcznej?](#can-i-enable-persistence-on-a-previously-created-cache)
-   [Częstotliwość kopii zapasowej można zmienić po utworzeniu pamięci podręcznej?](#can-i-change-the-backup-frequency-after-i-create-the-cache)
-   [Dlaczego mając kopii zapasowej częstotliwości 60 minut jest większa niż 60 minut między kopii zapasowych?](#why-if-i-have-a-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups)
-   [Co się dzieje starych kopii zapasowych po nawiązaniu nową kopię zapasową?](#what-happens-to-the-old-backups-when-a-new-backup-is-made)
-   [Co się stanie, jeśli masz skalowanie inny rozmiar i kopii zapasowej zostanie przywrócony wprowadzonej przed działaniem skalowania?](#what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation)

### <a name="can-i-enable-persistence-on-a-previously-created-cache"></a>Czy można włączyć pozostawanie w poprzednio utworzone pamięci podręcznej?

Tak, utrzymywanie Redis można skonfigurować zarówno na tworzenie pamięci podręcznej, jak i na istniejące premium pamięci podręcznej.

### <a name="can-i-change-the-backup-frequency-after-i-create-the-cache"></a>Częstotliwość kopii zapasowej można zmienić po utworzeniu pamięci podręcznej?

Tak, można zmienić częstotliwość kopii zapasowej na karta **Redis utrzymywanie danych** . Aby uzyskać instrukcje zobacz [Konfigurowanie Redis trwałe](#configure-redis-persistence).

### <a name="why-if-i-have-a-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups"></a>Dlaczego mając kopii zapasowej częstotliwości 60 minut jest większa niż 60 minut między kopii zapasowych?

Nie można uruchomić przedziału częstotliwości kopii zapasowej, aż poprzedniego wykonywania kopii zapasowej zakończyło się pomyślnie. Jeśli częstotliwość kopii zapasowej jest 60 minut, trwa proces tworzenia kopii zapasowej 15 minut do pomyślnego ukończenia następnej kopii zapasowej nie można uruchomić do 75 minut po uruchomieniu poprzedniej kopii zapasowej.

### <a name="what-happens-to-the-old-backups-when-a-new-backup-is-made"></a>Co się dzieje starych kopii zapasowych po nawiązaniu nową kopię zapasową?

Wszystkie kopie zapasowe z wyjątkiem najnowszego są automatycznie usuwane. To usunięcie nie może wystąpić natychmiast, ale starsze kopie zapasowe nie są zachowywane czas nieokreślony.

### <a name="what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation"></a>Co się stanie, jeśli masz skalowanie inny rozmiar i kopii zapasowej zostanie przywrócony wprowadzonej przed działaniem skalowania?

-   Jeśli masz skalowanie rozmiar nie ma żadnego wpływu.
-   Jeśli masz skalowany mniejszy rozmiar, a niestandardowych [baz danych](cache-configure.md#databases) ustawienie jest większa niż [ograniczyć baz danych](cache-configure.md#databases) dla rozmiar nowych danych w bazach danych dla tych nie można przywrócić. Aby uzyskać więcej informacji, zobacz [jest Moje niestandardowe ustawienie dotyczy podczas skalowania bazy danych?](cache-how-to-scale.md#is-my-custom-databases-setting-affected-during-scaling)
-   Jeśli masz skalowany mniejszy rozmiar, a nie jest za mało miejsca w mniejszym rozmiarze do przechowywania wszystkich danych z ostatniej kopii zapasowej, klawiszy zostanie usunięty w trakcie procesu przywracania zazwyczaj przy użyciu zasad eksmisji zwiększany [allkeys lru](http://redis.io/topics/lru-cache) .

## <a name="next-steps"></a>Następne kroki
Dowiedz się, jak korzystać z większej liczby funkcji pamięci podręcznej premium.

-   [Wprowadzenie do poziomu Azure Redis pamięci podręcznej Premium](cache-premium-tier-intro.md)
  
<!-- IMAGES -->

[redis-cache-new-cache-menu]: ./media/cache-how-to-premium-persistence/redis-cache-new-cache-menu.png

[redis-cache-premium-pricing-tier]: ./media/cache-how-to-premium-persistence/redis-cache-premium-pricing-tier.png

[redis-cache-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-persistence.png

[redis-cache-persistence-selected]: ./media/cache-how-to-premium-persistence/redis-cache-persistence-selected.png

[redis-cache-settings]: ./media/cache-how-to-premium-persistence/redis-cache-settings.png
