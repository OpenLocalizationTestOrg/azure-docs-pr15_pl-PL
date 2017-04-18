<properties
    pageTitle="Azure replikacji miejsca do magazynowania | Microsoft Azure"
    description="Dane na koncie Microsoft Azure przestrzeni dyskowej są replikowane wytrzymałości i wysokiej dostępności. Opcje replikacji to lokalnie zbędne przestrzeni dyskowej (LRS), zbędne strefy przestrzeni dyskowej (ZRS), zbędne geo przestrzeni dyskowej (GRS) i dostęp do odczytu zbędne geo miejsca do magazynowania (Pomoc Zdalna GRS)."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="tamram"/>

# <a name="azure-storage-replication"></a>Azure replikacji miejsca do magazynowania

Aby zapewnić wytrzymałości i wysoka dostępność zawsze replikacji danych na koncie Microsoft Azure miejsca do magazynowania. Replikacja kopiuje danych, w tym samym centrum danych lub do drugiego centrum danych, w zależności od wybranej opcji replikacji możesz wybrać. Replikacja chroni dane i zachowuje Twojej aplikacji wydłużyć czas wypadku awarii przejściowych sprzętu. Jeśli dane są replikowane do drugiego centrum danych, która również chroni dane przed losowych awariami w lokalizacji podstawowej.

Replikacja zapewnia, że Twoje konto miejsca do magazynowania spełnia [Umowa dotycząca poziomu usług (SLA) do przechowywania](https://azure.microsoft.com/support/legal/sla/storage/) nawet obliczu błędy. Zobacz gwarancje Umowa dotycząca poziomu usług, aby uzyskać informacje dotyczące magazynu Azure wytrzymałości i dostępności. 

Podczas tworzenia konta miejsca do magazynowania, można wybrać jedną z następujących opcji replikacji:  

- [Lokalnie zbędne przestrzeni dyskowej (LRS)](#locally-redundant-storage)
- [Strefa zbędne przestrzeni dyskowej (ZRS)](#zone-redundant-storage)
- [Zbędne Geo przestrzeni dyskowej (GRS)](#geo-redundant-storage)
- [Dostęp do odczytu zbędne geo miejsca do magazynowania (Pomoc Zdalna GRS)](#read-access-geo-redundant-storage)

Dostęp do odczytu geo zbędne miejsce do magazynowania (Pomoc Zdalna GRS) jest domyślna opcja podczas tworzenia nowego konta miejsca do magazynowania.

Poniższa tabela zawiera krótkie omówienie różnic między LRS, ZRS, GRS i GRS pomoc Zdalna, kolejne sekcje dotyczy każdego typu replikacji bardziej szczegółowo.

| Strategia replikacji                                                               | LRS | ZRS | GRS | POMOC ZDALNA GRS |
|:----------------------------------------------------------------------------------|:---|:---|:---|:------|
| Dane są replikowane przez wielu centrach danych.                                     | Brak  | Tak | Tak | Tak    |
| Dane można odczytywać z lokalizacji pomocniczej, a także z lokalizacji podstawowej. | Brak  | Brak  | Brak  | Tak    |
| Liczba kopii danych obsługiwanych na osobne węzły.                             | 3   | 3   | 6   | 6      |

Zobacz [Ceny miejsca do magazynowania Azure](https://azure.microsoft.com/pricing/details/storage/) cennik dla opcji różnych nadmiarowości.

>[AZURE.NOTE] Magazyn Premium obsługuje tylko lokalnie zbędne przestrzeni dyskowej (LRS). Aby uzyskać informacje dotyczące magazynu Premium, zobacz [miejsca do magazynowania Premium: wysokiej wydajności miejsca do magazynowania dla Azure maszyn wirtualnych obciążenia](storage-premium-storage.md).

## <a name="locally-redundant-storage"></a>Lokalnie zbędne miejsca do magazynowania

Lokalnie zbędne przestrzeni dyskowej (LRS) replikuje dane trzy razy w skali szafka, który znajduje się w centrum danych w regionie, w którym zostały utworzone konta miejsca do magazynowania. Żądanie zapisu pomyślnie zwraca tylko wtedy, gdy została zapisana do wszystkich replik trzy. Te trzy repliki każdego znajdują się w oddzielnych błędów a domenami uaktualnienia w jednym szafka Skala. 

Jednostka skali miejsca do magazynowania jest zbiorem stojaków węzłów magazynu. Domeny błędów (FD) to grupa węzłów reprezentują fizycznie jednostka błąd, które mogą zostać uznane za węzły należących do tej samej szafy fizycznym. Uaktualnianie domeny (UD) to grupa węzłów, które są uaktualniane razem podczas procesu uaktualniania usługi (rozmieszczenia). Trzy repliki są rozmieszczony UDs i FDs w jedną jednostkę skali przestrzeni dyskowej, aby upewnić się, że dane są dostępne, nawet w przypadku awarii sprzętu wpływa na stojaków pojedynczej lub po uaktualnieniu węzłów podczas fazy. 

LRS jest najniższe opcję Koszt i zawiera co najmniej wytrzymałości w porównaniu z innych opcji. W przypadku awarii poziomu Centrum danych (fire zalewu itp.) wszystkie trzy repliki może być utraconych lub odzyskać. Aby zmniejszyć ryzyko Geo zbędne przestrzeni dyskowej (GRS) jest zalecane dla większości aplikacji.

Magazyn lokalnie zbędne nadal może być pożądane w niektórych scenariuszach: 

- Udostępnia najwyższych maksymalną przepustowość opcje replikacji magazyn Azure.

- Jeśli aplikacja są przechowywane dane, które można łatwo odtworzony, mogą wybrać dla LRS.

- Niektóre aplikacje są ograniczone do replikacji danych tylko w kraju ze względu na wymagania dotyczące zarządzania danymi. Obszar iloczynów mogą znajdować się w innym kraju; zobacz [Azure regionów](https://azure.microsoft.com/regions/) uzyskać informacji na temat pary region.

## <a name="zone-redundant-storage"></a>Strefa zbędne miejsca do magazynowania

Strefa zbędne przestrzeni dyskowej (ZRS) asynchroniczne replikuje dane w centrach danych w ramach jednej lub dwóch regionów oprócz przechowywania trzy repliki podobne do LRS, zapewniając wytrzymałości wyższymi niż LRS. Dane przechowywane w ZRS jest trwały, nawet jeśli podstawowe centrum danych jest niedostępne lub odzyskać.
Klienci, którzy planujesz używać ZRS należy pamiętać, że: 

- ZRS jest dostępna tylko dla obiektów blob bloku w celu ogólne miejsca do magazynowania konta i jest obsługiwana tylko w wersjach usługi przechowywania 2014-02-14 lub nowszej. 

- Ponieważ asynchroniczne replikacji wiąże się opóźnienie, wypadku awarii lokalne istnieje możliwość zmiany, które nie zostały jeszcze replikowane pomocniczej zostaną utracone jeśli dane nie można odzyskać z serwera podstawowego.

- Do momentu Microsoft inicjuje przejęcie awaryjne do pomocniczej replice może być niedostępne.

- ZRS konta nie można przekonwertować później LRS lub GRS. Podobnie istniejącego konta LRS lub GRS nie można przekonwertować na konto ZRS.

- Metryki lub funkcja rejestrowania nie jest ZRS konta. 

## <a name="geo-redundant-storage"></a>Zbędne Geo miejsca do magazynowania

Zbędne Geo przestrzeni dyskowej (GRS) replikuje danych na pomocniczej regionu, który jest setki mila poza podstawowym region. Jeśli Twoje konto miejsca do magazynowania ma GRS włączona, dane są trwałe nawet w przypadku awarii regionalne wykonane lub danych, w którym podstawowy region nie jest odzyskania.

Konta miejsca do magazynowania z GRS włączone aktualizacja najpierw poświęca podstawowego regionu, w których są replikowane trzy razy. Następnie aktualizacja jest replikowane asynchroniczne pomocniczej regionu, w których również są replikowane trzy razy. 

GRS głównego i pomocniczego regionów Zarządzanie repliki domen oddzielnych błędów i uaktualnianie domen w skali szafka zgodnie z opisem z LRS.

Zagadnienia:

- Ponieważ asynchroniczne replikacji wiąże się opóźnienie, w przypadku awarii regionalne istnieje możliwość zmiany, które nie zostały replikowane do obszaru pomocniczej zostaną utracone jeśli dane nie można odzyskać z podstawowego regionu.

- Replice nie jest dostępna, chyba że Microsoft inicjuje przejęcie awaryjne do obszaru pomocniczą.

- Jeśli aplikacja chce odczyt pomocniczej region użytkownika należy włączyć GRS pomoc Zdalna. 

Podczas tworzenia konta miejsca do magazynowania, możesz wybrać region podstawowy dla konta. Pomocnicza region jest określany na podstawie regionu podstawowy, a nie można zmienić. W poniższej tabeli przedstawiono par głównego i pomocniczego region.

| Podstawowy             | Pomocnicze           |
|---------------------|---------------------|
| Ameryka Północna centralnej w Stanach Zjednoczonych    | Południowej centralnej Stany Zjednoczone    |
| Południowej centralnej Stany Zjednoczone    | Ameryka Północna centralnej w Stanach Zjednoczonych    |
| Wschodniej Stany Zjednoczone             | Zachód Stany Zjednoczone             |
| Zachód Stany Zjednoczone             | Wschodniej Stany Zjednoczone             |
| Stany Zjednoczone wschód 2           | Centralny Stany Zjednoczone          |
| Centralny Stany Zjednoczone          | Stany Zjednoczone wschód 2           |
| Europa Północna        | Europa Zachodnia         |
| Europa Zachodnia         | Europa Północna        |
| Południowej wschodnioazjatyckie     | Wschodnioazjatyckie           |
| Wschodnioazjatyckie           | Południowej wschodnioazjatyckie     |
| Chiny wschodniego          | Chiny północnej         |
| Chiny północnej         | Chiny wschodniego          |
| Japonia wschód          | Japonia zachód          |
| Japonia zachód          | Japonia wschód          |
| Brazylia Południowej        | Południowej centralnej Stany Zjednoczone    |
| Wschód Australia      | Australia kopiec |
| Australia kopiec | Wschód Australia      |
| Południe Indie         | Centralny Indie       |
| Centralny Indie       | Południe Indie         |
| Stany Zjednoczone Gov Iowa         | Stany Zjednoczone Gov Virginia     |
| Stany Zjednoczone Gov Virginia     | Stany Zjednoczone Gov Iowa         |
| Kanada Środkowa      | Wschód Kanada          |
| Wschód Kanada         | Kanada Środkowa      |
| Zachód UK             | Południe USA            |
| Południe USA            | Zachód UK             |
| Centralny Niemcy     | Niemcy Koc   |
| Niemcy Koc   | Centralny Niemcy     |
| Stany Zjednoczone zachód 2           | Centralny zachód Stany Zjednoczone     |
| Centralny zachód Stany Zjednoczone     | Stany Zjednoczone zachód 2           |

Aby uzyskać aktualne informacje na temat obsługiwane przez Azure zobacz [Regionów Azure](https://azure.microsoft.com/regions/).
 
## <a name="read-access-geo-redundant-storage"></a>Dostęp do odczytu zbędne geo przestrzeni dyskowej

Dostęp do odczytu zbędne geo miejsca do magazynowania (Pomoc Zdalna GRS) maksymalizuje dostępność dla Twojego konta miejsca do magazynowania, dostarczając tylko do odczytu dostęp do danych w lokalizacji pomocniczej, oprócz replikacja za pośrednictwem dwóch regionów dostarczony przez GRS. 

Po włączeniu dostępu tylko do odczytu do danych w regionie pomocniczej danych jest dostępna na pomocniczym punkt końcowy oprócz podstawowego punkt końcowy dla Twojego konta miejsca do magazynowania. Punkt końcowy pomocniczej jest podobne do punktu końcowego podstawowego, ale dołącza sufiks `–secondary` do nazwy konta. Na przykład w przypadku usługi podstawowego punktu końcowego usługi obiektów Blob `myaccount.blob.core.windows.net`, to jest punkt końcowy pomocniczej `myaccount-secondary.blob.core.windows.net`. Klawisze dostępu do konta miejsca do magazynowania jest taka sama dla głównego i pomocniczego końcowych.

Zagadnienia:

- Aplikacja ma zarządzać który punkt końcowy jest interakcja z podczas korzystania z GRS pomoc Zdalna. 

- Pomoc Zdalna GRS jest przeznaczony do celów wysokiej dostępności. Orientacji skalowalność Przejrzyj [listy kontrolnej wydajności](storage-performance-checklist.md).

## <a name="next-steps"></a>Następne kroki

- [Cennik Azure miejsca do magazynowania](https://azure.microsoft.com/pricing/details/storage/)
- [Informacje o kontach Azure miejsca do magazynowania](storage-create-storage-account.md)
- [Skalowalność Azure miejsca do magazynowania i cele](storage-scalability-targets.md)
- [Opcje nadmiarowości magazynu platformy Microsoft Azure i dostęp do odczytu Geo zbędne miejsca do magazynowania](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)  
- [Papier SOSP - Azure miejsca do magazynowania: Wysokiej dostępności przestrzeni dyskowej usługi w chmurze z silnych spójności](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)  
