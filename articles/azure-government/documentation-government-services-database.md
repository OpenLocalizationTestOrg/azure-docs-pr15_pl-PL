<properties
    pageTitle="Azure dokumentacji dla instytucji rządowych | Microsoft Azure"
    description="Umożliwia porównanie funkcji i wskazówki dotyczące tworzenia aplikacji dla instytucji rządowych Azure"
    services="Azure-Government"
    cloud="gov"
    documentationCenter=""
    authors="ryansoc"
    manager="zakramer"
    editor=""/>

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/18/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-databases"></a>Azure dla instytucji rządowych baz danych

##  <a name="sql-database"></a>Baza danych SQL

Zapoznaj się z<a href="https://msdn.microsoft.com/en-us/library/bb510589.aspx"> Centrum zabezpieczeń firmy Microsoft dla aparatu bazy danych SQL</a> i [Dokumentacji publicznej bazy danych programu SQL Azure](https://azure.microsoft.com/documentation/services/sql-database/) dodatkowe wskazówki dotyczące konfiguracji widoczność metadanych oraz najlepsze rozwiązania ochrony.

### <a name="variations"></a>Odmiany

Baza danych SQL w wersji 12 jest zazwyczaj dostępny w Azure dla instytucji rządowych.

Różni się adres serwerów Azure SQL Azure dla instytucji rządowych:

Typ usługi|Azure publicznej|Azure dla instytucji rządowych
---|---|---
Baza danych SQL|*. database.windows.net|*. database.usgovcloudapi.net

### <a name="considerations"></a>Zagadnienia dotyczące

Poniższe informacje określa granicę dla instytucji rządowych Azure SQL Azure:

| Dane regulacji kontrolowane dozwolone | Dane regulacji kontrolowane niedozwolone |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Wszystkie dane przechowywane i przetwarzane w programie Microsoft Azure SQL może zawierać dane regulacji Azure dla instytucji rządowych. Przesyłania danych regulacji Azure dla instytucji rządowych danych, należy użyć narzędzia bazy danych. | Metadane SQL Azure nie jest dozwolona zawierać kontrolowane Eksportuj dane. Metadane to między innymi wszystkie dane konfiguracji wprowadzane podczas tworzenia i przechowywania produktu miejsca do magazynowania.  Nie wprowadzaj dane regulacji kontrolowane w następujących polach: bazy danych, nazwę, nazwę subskrypcji, grup zasobów, nazwa serwera, logowania administrator serwera, wdrażania nazwy i zasobów, znaczniki zasobów

## <a name="azure-redis-cache"></a>Azure Redis pamięci podręcznej

Aby uzyskać szczegółowe informacje dotyczące tej usługi i jak z niego korzystać dokumentacji [Azure Redis w pamięci podręcznej publicznej](https://azure.microsoft.com/documentation/services/redis-cache/).

### <a name="variations"></a>Odmiany

Adresy URL do uzyskiwania dostępu i zarządzanie pamięci podręcznej Redis Azure w Azure dla instytucji rządowych różnią się:

Typ usługi|Azure publicznej|Azure dla instytucji rządowych
---|---|---
Punkt końcowy pamięci podręcznej|*. redis.cache.windows.net|*. redis.cache.usgovcloudapi.net
Azure portal|https://Portal.Azure.com|https://Portal.Azure.us

>[AZURE.NOTE] Wszystkie skrypty i kod należy uwzględnić odpowiednie punkty końcowe i w środowisku. Aby uzyskać więcej informacji zobacz [jak połączyć w chmurze dla instytucji rządowych Azure](../redis-cache/cache-howto-manage-redis-cache-powershell.md#how-to-connect-to-azure-government-cloud-or-azure-china-cloud).


### <a name="considerations"></a>Zagadnienia dotyczące

Poniższe informacje identyfikuje granicę dla instytucji rządowych Azure Azure Redis w pamięci podręcznej:

| Dane regulacji kontrolowane dozwolone | Dane regulacji kontrolowane niedozwolone |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Wszystkie dane przechowywane i przetwarzane w pamięci podręcznej Azure Redis mogą zawierać dane regulacji Azure dla instytucji rządowych. | Azure metadanych Redis pamięci podręcznej nie jest dozwolona zawierać kontrolowane Eksportuj dane. Nie wprowadzaj dane regulacji kontrolowane w następujących polach: pamięci podręcznej nazwę, nazwę VNET, nazwę subskrypcji, grup zasobów, znaczniki zasobów, Redis właściwości.  

##  <a name="next-steps"></a>Następne kroki

Aby uzyskać dodatkowe informacje i aktualizacje subskrybowanie <a href="https://blogs.msdn.microsoft.com/azuregov/">Blog dotyczący programu Microsoft Azure dla instytucji rządowych.</a>
