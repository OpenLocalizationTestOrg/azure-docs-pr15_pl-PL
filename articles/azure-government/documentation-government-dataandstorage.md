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
    ms.date="09/30/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-data-and-storage"></a>Dane dla instytucji rządowych Azure i miejsca do magazynowania

##  <a name="azure-storage"></a>Azure miejsca do magazynowania

Aby uzyskać szczegółowe informacje dotyczące tej usługi i jak z niego korzystać dokumentacji [Magazyn Azure publicznej](https://azure.microsoft.com/documentation/services/storage/).

### <a name="variations"></a>Odmiany

Adresy URL dla kont miejsca do magazynowania w Azure dla instytucji rządowych różnią się:

Typ usługi|Azure publicznej|Azure dla instytucji rządowych
---|---|---
Magazyn obiektów blob|*. blob.core.windows.net|*. blob.core.usgovcloudapi.net
Magazyn kolejek|*. queue.core.windows.net|*. queue.core.usgovcloudapi.net
Magazyn tabel|*. table.core.windows.net| *. table.core.usgovcloudapi.net

>[AZURE.NOTE] Wszystkie skrypty i kod należy uwzględnić odpowiednich punktów końcowych.  Zobacz [Konfigurowanie parametry połączenia Azure miejsca do magazynowania](../storage-configure-connection-string.md#creating-a-connection-string-to-the-explicit-storage-endpoint). 

Aby uzyskać więcej informacji na temat interfejsów API zobacz <a href="https://msdn.microsoft.com/en-us/library/azure/mt616540.aspx">Konstruktora konta miejsca do magazynowania w chmurze</a>.

Sufiks punkt końcowy do użycia w tych overloads jest core.usgovcloudapi.net 

### <a name="considerations"></a>Zagadnienia dotyczące

Poniższe informacje określa granicę dla instytucji rządowych Azure do przechowywania Azure:

| Dane regulacji kontrolowane dozwolone | Dane regulacji kontrolowane niedozwolone |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Dane wprowadzone, przechowywane i przetwarzane w magazyn Azure produktu może zawierać kontrolowane Eksportuj dane. Statyczne uwierzytelnienia, takich jak haseł i PIN karty inteligentnej, aby uzyskać dostęp do składniki platformy Azure. Kluczy prywatnych certyfikatów służące do zarządzania składniki platformy Azure. Inne zabezpieczeń informacji i hasła, takich jak certyfikaty, klucze szyfrowania kluczy głównych i kluczy miejsca do magazynowania przechowywanych w usługach Azure. | Azure magazynu metadanych nie jest dozwolona zawierać kontrolowane Eksportuj dane. Metadane to między innymi wszystkie dane konfiguracji wprowadzone podczas tworzenia i przechowywania produktu miejsca do magazynowania.  Nie wprowadzaj dane Regulated kontrolowane w następujących polach: grup zasobów, nazwy wdrożenia i zasobów, znaczniki zasobów  

##  <a name="premium-storage"></a>Magazyn Premium

Szczegółowe informacje na temat tej usługi i jak z niego korzystać, [miejsca do magazynowania Premium: wysokiej wydajności miejsca do magazynowania dla Azure maszyn wirtualnych obciążenia](../storage/storage-premium-storage.md).

###  <a name="variations"></a>Odmiany

Miejsce do magazynowania Premium jest powszechnie dostępny w USGov Virginia. Ta opcja uwzględnia maszyn wirtualnych Zasadami serii. 

### <a name="considerations"></a>Zagadnienia dotyczące

Z tym samym miejsca do magazynowania danych zagadnieniami dotyczącymi wymienionych powyżej dotyczą kont miejsca do magazynowania premium. 

##  <a name="sql-database"></a>Baza danych SQL

Zapoznaj się z<a href="https://msdn.microsoft.com/en-us/library/bb510589.aspx"> Centrum zabezpieczeń firmy Microsoft dla aparatu bazy danych SQL</a> i [Dokumentacji publicznej bazy danych programu SQL Azure](https://azure.microsoft.com/documentation/services/sql-database/) dodatkowe wskazówki dotyczące konfiguracji widoczność metadanych oraz najlepsze rozwiązania ochrony.

### <a name="variations"></a>Odmiany

Baza danych SQL w wersji 12 jest zazwyczaj dostępny w Azure dla instytucji rządowych.

Różni się adres serwerów Azure SQL Azure dla instytucji rządowych:

Typ usługi|Azure publicznej|Azure dla instytucji rządowych
---|---|---
Baza danych SQL|*. database.windows.net|*. database.usgovcloudapi.net

### <a name="considerations"></a>Zagadnienia dotyczące

Poniższe informacje określa granicę dla instytucji rządowych Azure do przechowywania Azure:

| Dane regulacji kontrolowane dozwolone | Dane regulacji kontrolowane niedozwolone |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Wszystkie dane przechowywane i przetwarzane w programie Microsoft Azure SQL może zawierać dane regulacji Azure dla instytucji rządowych. Przesyłania danych regulacji Azure dla instytucji rządowych danych, należy użyć narzędzia bazy danych. | Metadane SQL Azure nie jest dozwolona zawierać kontrolowane Eksportuj dane. Metadane to między innymi wszystkie dane konfiguracji wprowadzone podczas tworzenia i przechowywania produktu miejsca do magazynowania.  Nie wprowadzaj dane regulacji kontrolowane w następujących polach: bazy danych, nazwę, nazwę subskrypcji, grup zasobów, nazwa serwera, logowania administrator serwera, wdrażania nazwy i zasobów, znaczniki zasobów

##  <a name="next-steps"></a>Następne kroki

Aby uzyskać dodatkowe informacje i aktualizacje subskrybowanie <a href="https://blogs.msdn.microsoft.com/azuregov/">Blog dotyczący programu Microsoft Azure dla instytucji rządowych.</a>
