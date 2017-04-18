<properties
    pageTitle="Rejestrować analizy danych | Microsoft Azure"
    description="Informacje na temat sposobu analizy dziennika ochrony prywatności i chroni dane."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/23/2016"
    ms.author="banders"/>

# <a name="log-analytics-data-security"></a>Rejestrować analizy danych

Firma Microsoft poświęca ochrony prywatności i zabezpieczanie danych, dostępne podczas przedstawiania oprogramowania i usług, które ułatwiają zarządzanie infrastrukturą Twojej organizacji. Firma Microsoft rozpoznawać, że po powierzyć danych innym osobom, danej relacji zaufania wymaga rygorystyczne zabezpieczeń. Microsoft jest zgodny ściśle zabezpieczenia i zgodność z wytycznymi — z kodowania do działania usługi.

Zabezpieczanie i ochrona danych jest priorytetem dla firmy Microsoft. Skontaktuj się z nami za pomocą dowolnego pytania, sugestie lub problemów o następujące informacje, w tym nasze zasady zabezpieczeń na [Azure opcje pomocy technicznej](http://azure.microsoft.com/support/options/).

W tym artykule wyjaśniono, jak dane są zbierane, przetwarzane i zabezpieczone dziennika analizy w pakiecie zarządzania operacje usługi (OMS). Czynniki umożliwia nawiązywanie połączenia z usługą sieci web, System Center Operations Manager umożliwia zbieranie danych operacyjnych lub pobierania danych z Azure informacje diagnostyczne dla używany przez analizy dziennika. Zebrane dane są wysyłane przez Internet za pomocą uwierzytelniania opartego na certyfikat SSL 3 z usługą analizy dziennika, który jest obsługiwany w programie Microsoft Azure. Dane są kompresowane przez agenta przed wysłaniem.

Usługa dziennika Analytics zarządza danych oparte na chmurze bezpieczne przy użyciu następujących metod:

- podziału danych
- przechowywanie danych
- fizyczny zabezpieczeń
- Zarządzanie zdarzeniami
- zgodność
- Certyfikaty standardy zabezpieczeń


## <a name="data-segregation"></a>Podziału danych

Dane klientów są przechowywane w logiczny sposób oddzielnie dla każdego składnika w całym usługę. Wszystkie dane są oznakowane dla organizacji. Znakowanie ten będzie nadal występował w całym cyklu życia danych, a została wprowadzona w każdej warstwie usługi. Każdy klient ma dedykowane obiektów blob platformy Azure zawierającego długoterminowe danych

## <a name="data-retention"></a>Przechowywanie danych

Indeksowane dziennika wyszukiwanie danych są przechowywane i zachowywane według planu cennik. Aby uzyskać więcej informacji zobacz [Ceny analizy dziennika](https://azure.microsoft.com/pricing/details/log-analytics/).

Microsoft powoduje usunięcie danych klienta 30 dni po zamknięciu obszaru roboczego usługi OMS. Microsoft powoduje także usunięcie konta magazynu platformy Azure miejsce, w którym znajdują się dane. Po usunięciu danych klientów nie dysków fizycznych są niszczone.

W poniższej tabeli wymieniono niektóre dostępnych rozwiązań w usługi OMS i przykłady typów danych zbieranych.

| **Rozwiązanie** | **Typy danych** |
| --- | --- |
| Ocena konfiguracji | Dane konfiguracji, metadane i dane o stanie |
| Planowanie pojemności | Dane dotyczące wydajności i metadanych |
| Ochrony przed złośliwym oprogramowaniem | Konfiguracja dane i metadane |
| Ocena aktualizacji systemu | Metadane i stan danych |
| Zarządzanie dziennikiem | Zdefiniowane przez użytkownika dzienniki zdarzeń, dzienniki zdarzeń systemu Windows i/lub dzienniki programu IIS |
| Śledzenie zmian | Spisu oprogramowania i metadanych usługi systemu Windows |
| SQL i ocena usługi Active Directory | Dane, dane rejestru, dane dotyczące wydajności i SQL Server dynamiczne zarządzanie wyświetlanie wyników |

W poniższej tabeli pokazano przykłady typów danych:

| **Typ danych** | **Pola** |
| --- | --- |
| Alert | Alert nazwa, opis alertu, BaseManagedEntityId, identyfikator problemu, IsMonitorAlert, RuleId, ResolutionState, priorytet, ważności, kategoria, właściciela, ResolvedBy, TimeRaised, TimeAdded, Ostatnia modyfikacja, ostatnio zmodyfikowane przez, LastModifiedExceptRepeatCount, TimeResolved, TimeResolutionStateLastModified, TimeResolutionStateLastModifiedInDB, RepeatCount |
| Konfiguracja | Id_klienta, AgentID, identyfikator jednostki, ManagedTypeID, ManagedTypePropertyID, Bieżąca_wartość, ChangeDate |
| Zdarzenia | Identyfikator zdarzenia, EventOriginalID, BaseManagedEntityInternalId, RuleId, PublisherId, PublisherName, FullNumber, liczba, kategoria, ChannelLevel, LoggingComputer, funkcji EventData, EventParameters, TimeGenerated, TimeAdded <br>**Uwaga:** Podczas rejestrowania zdarzeń z polami niestandardowymi w dzienniku zdarzeń systemu Windows, usługi OMS zbiera je. |
| Metadane | BaseManagedEntityId, ObjectStatus, jednostka organizacyjna, ActiveDirectoryObjectSid, PhysicalProcessors, NazwaSieci, adres IP, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, adres IP, NetbiosDomainName, LogicalProcessors, Nazwa_dns, DisplayName, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime |
| Wydajność | Nazwa obiektu, CounterName, PerfmonInstanceName, PerformanceDataId, PerformanceSourceInternalID, SampleValue, TimeSampled, TimeAdded |
| Województwo | StateChangeEventId, StateId, NewHealthState, OldHealthState, kontekst, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, Ostatnia modyfikacja, LastGreenAlertGenerated, DatabaseTimeModified |

## <a name="physical-security"></a>Fizyczny zabezpieczeń

Analizy dziennika w usługę jest obsługiwanego przez pracowników firmy Microsoft, a wszystkie działania są rejestrowane i inspekcji. Usługa działa całkowicie w Azure i spełnia kryteria Azure konstrukcyjną typowych. Na stronie 18 [Omówienie zabezpieczeń firmy Microsoft Azure](http://download.microsoft.com/download/6/0/2/6028B1AE-4AEE-46CE-9187-641DA97FC1EE/Windows%20Azure%20Security%20Overview%20v1.01.pdf), możesz wyświetlić szczegółowe informacje dotyczące zabezpieczeń fizycznych środków trwałych Azure. Fizycznie uprawnienia do bezpiecznego obszarów, są zmieniane w ciągu dnia roboczego jednej dla każdego, kto ma już odpowiedzialność za usługę, w tym transfer i zakończenie. Więcej o globalnej fizycznej infrastruktury używanych w [Centrach danych firmy Microsoft](https://www.microsoft.com/en-us/server-cloud/cloud-os/global-datacenters.aspx).

## <a name="incident-management"></a>Zarządzanie zdarzeniami

Usługi OMS ma to proces Zarządzanie zdarzeniami, który zgodny wszystkich usług firmy Microsoft. Podsumowując, firma Microsoft:

- Za pomocą modelu wspólnej odpowiedzialności miejsce, w którym część zobowiązań zabezpieczeń należy Microsoft podczas części należy do klienta
- Zarządzanie bezpieczeństwem Azure
  - Wykrywanie zdarzenia w pierwsze wskazanie, aby rozpocząć dochodzeniem
  - Oceń wpływ i ważności zdarzenia przez członka zespołu zdarzenia odpowiedzi na połączenia. Na podstawie dowodów, oceny może lub nie może powodować dalszych eskalacji do zespołu odpowiedź zabezpieczeń.
  - Diagnozowanie zdarzenia ekspertów odpowiedź zabezpieczeń przeprowadzanie dochodzenia technicznych lub sądowej, określenie strategii zamknięcia, łagodzenia i rozwiązania. Jeśli zespół zabezpieczeń uważa, że dane klientów może stać się podsumowującymi osobie prawem lub nieautoryzowanych, równolegle rozpoczyna się równoległego procesu powiadomienie o zdarzeniu klienta.  
  - Ustabilizować i odzyskiwanie z zdarzenia. Zespół zdarzenia odpowiedzi utworzy plan odzyskiwania zmniejszeniu problem. Czynności zamknięcia kryzysu quarantining systemów ryzyko może wystąpić natychmiast i równolegle z diagnostyki. Dłużej czynniki terminów mogą być planowane występujące po upływie bezpośrednie ryzyko.  
  - Zamknij zdarzenia i przeprowadzanie podsumowaniem końcowym. Zespół zdarzenia odpowiedzi utworzy podsumowaniem końcowym wokół szczegóły zdarzenia z zamiarem zmiany zasad, procedur i procesów, aby zapobiec ponownemu wystąpieniu zdarzenia.
- Powiadamiać klientów zdarzeń
  - Określanie zakresu ryzyko klientów oraz zapewnienie każda osoba dotyczy to tak szczegółowe powiadomienie o możliwie
  - Tworzenie powiadomienie o zapewnienie użytkownikom szczegółowe wystarczających informacji, aby mogą wykonywać dochodzeniem na ich zakończenia i spełniają wszelkie zobowiązania wprowadzone do użytkowników końcowych podczas nie powodując opóźnienia procesu powiadomienia.
  - Upewnij się, a zadeklarować zdarzenia, stosownie do potrzeb.
  - Powiadom klientom powiadomienia o wystąpieniu zdarzenia niezwłocznie nadmierne i zgodnie z jakiegokolwiek zobowiązania prawa lub umowy. Powiadomienie o bezpieczeństwem będą dostarczane do jednej lub większej liczby Administratorzy klienta w jakikolwiek sposób, który zaznacza firmy Microsoft, łącznie z pocztą e-mail.
- Przeprowadzanie gotowości zespołu i materiałów szkoleniowych
  - Personelowi firmy Microsoft są wymagane do ukończenia zabezpieczeń i szkolenia Sygnalizacja dostępności, co ułatwia ich do identyfikowania i zgłaszać zabezpieczeń potencjalne problemy.  
  - Operatory pracy w usłudze Microsoft Azure mają zobowiązań szkolenie dodanie otaczający im dostępu do poufnych systemy obsługi danych klientów.
  - Specjalistyczne szkolenie dla ich ról odbieranie personelowi odpowiedź zabezpieczeń firmy Microsoft


W przypadku utraty danych dowolnego klienta możemy powiadomić każdego z klientów w jeden dzień. Jednak utratą danych klienta nigdy nie wystąpił z usługi OMS. Ponadto możemy zachowywania kopii danych, który został utworzony i jest rozdzielana geograficznie.

Aby uzyskać więcej informacji na temat jak Microsoft odpowiada zdarzeń zobacz [Microsoft Azure zabezpieczeń odpowiedź w chmurze](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678/file/150826/1/Microsoft Azure Security Response in the cloud.pdf).

## <a name="compliance"></a>Zgodność

Usługi OMS software development i usługa zespołu informacji zabezpieczeń i zarządzania program obsługuje wymagań i działa zgodnie z prawem i przepisami, zgodnie z opisem w [Centrum zaufania programu Microsoft Azure](https://azure.microsoft.com/support/trust-center/) i [Zgodność Centrum zaufania programu Microsoft](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx). Jak usługi OMS określa wymagania dotyczące zabezpieczeń, identyfikuje kontroli bezpieczeństwa, zarządza i monitoruje ryzyka także opisano dostępne. Raz w roku przeprowadza przegląd zasady standardy, procedur i wytycznych.

Każdy członek zespołu opracowywania usługi OMS otrzymuje szkolenie dotyczące zabezpieczeń formalne aplikacji. Wewnętrznie firma Microsoft korzysta z systemem tworzenia oprogramowania. Każdy projekt oprogramowania jest chroniony przez system kontroli wersji.

Firma Microsoft udostępnia zabezpieczenia i zgodność zespołu nadzoruje i ocenia wszystkich usług w programie Microsoft. Zabezpieczenia informatyków uzupełnić zespołu i nie są one skojarzone z konstrukcyjną działy, które można opracowywać usługi OMS. Biuro zabezpieczeń ma własne łańcuchu zarządzania i przeprowadzanie niezależnych ocen produktów i usług, aby upewnić się, zabezpieczenia i zgodność.

Rada nadzorcza firmy Microsoft jest powiadamiany przez i sprawozdanie o wszystkich bezpieczeństwa informacji programów firmy Microsoft.

Zespół rozwoju i usługa oprogramowania usługi OMS aktywnie współpracuje z zespołów Microsoft Legal i zgodność i innych partnerów branżowe uzyskiwania różnych certyfikaty.

## <a name="security-standards-certifications"></a>Certyfikaty standardy zabezpieczeń

Zaloguj się analizy usługi OMS obecnie spełniają następujące normy zabezpieczeń:

- [ISO/IEC 27001](http://www.iso.org/iso/home/standards/management-standards/iso27001.htm) i [ISO/IEC 27018:2014](http://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=61498) zgodności
- Płatności karty (PCI zgodne) danych zabezpieczeń standardowego (PCI DSS) Rada standardy zabezpieczeń PCI.
- [Typ usługi organizacji kontrolek (SOC) 1 1 i SOC 2 Type 1](https://www.microsoft.com/en-us/TrustCenter/Compliance/SOC1-and-2) zgodności
- Wspólne kryteria techniczny systemu Windows
- Certyfikacji obliczeniowych w wiarygodny firmy Microsoft
- Jako usługa Azure składników używanych przez usługi OMS spełniać wymagania dotyczące zgodności Azure. Więcej w [Zgodności Centrum zaufania firmy Microsoft](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx).


## <a name="cloud-computing-security-data-flow"></a>Obliczanie przepływu danych zabezpieczeń w chmurze
Na poniższym diagramie przedstawiono Architektura zabezpieczeń chmury jako przepływu informacji z Twojej firmy i jak są zabezpieczone, tak jak jest przenoszony do usługi analizy dziennika ostatecznie widoczne przez Ciebie w portalu usługi OMS. Więcej informacji na temat każdego kroku wykonuje diagramu.

![Obraz usługi OMS zbierania danych i zabezpieczeń](./media/log-analytics-security/log-analytics-security-diagram.png)

## <a name="1-sign-up-for-log-analytics-and-collect-data"></a>1. konta na potrzeby analizy dziennika i zbierania danych

Dla Twojej organizacji na wysyłanie danych do analizy dziennika można skonfigurować Windows agentami, czynników uruchomionych Azure maszyn wirtualnych lub działanie agentów usługi OMS Linux. Jeśli używasz programu Operations Manager czynników, następnie użyjesz kreatora konfiguracji w konsoli operacje ich konfigurowania. Użytkownicy, (które mogą być, innym użytkownikom lub grupy osób) Utwórz jedno lub więcej kont usługi OMS (obszarów roboczych usługi OMS) i zarejestrować czynników, stosując jedną z następujących kont:


- [Identyfikator organizacji](../active-directory/sign-up-organization.md)

- [Konto Microsoft — program Outlook, Office Live i MSN](http://www.microsoft.com/account/default.aspx)

Obszar roboczy usługi OMS jest miejsce, w którym danych jest zebrane, agregacją, analizowane i prezentowane. Używane głównie w sposób partition danych obszaru roboczego, a każdym obszarze roboczym jest unikatowy. Na przykład można mieć danych produkcji zarządzane z jednego obszaru roboczego usługi OMS i danych test zarządzane przy użyciu innego obszaru roboczego. Obszary robocze również pomóc administrator kontrola dostępu użytkowników do danych. Każdego obszaru roboczego może mieć wiele kont użytkowników skojarzone z nim, a wszystkie konta użytkowników można uzyskać dostęp do wielu usługi OMS obszaru roboczego. Możesz tworzyć obszary robocze na podstawie regionu centrum danych. Każdy obszar roboczy jest replikować do innych centrach danych w regionie przede wszystkim dla usługi OMS dostępność usługi.

Po zakończeniu działania Kreatora konfiguracji każdej grupy zarządzania programu Operations Manager dla programu Operations Manager nawiąże połączenie z usługą analizy dziennika. Następnie użyj Kreatora dodawania komputerów do wyboru, które komputery w grupie Zarządzanie mogą wysyłać dane do usługi. W przypadku innych typów agenta poszczególnych łączy bezpieczne usługę.

Wszystkie komunikacji między połączonych systemów i usług analiz dziennika jest zaszyfrowany.  Protokół TLS (HTTPS) jest używany do szyfrowania.  Proces Microsoft SDL występuje mieć pewność, że analizy dziennika są aktualne najnowsze osiągnięcia protokołami cryptographic.

Każdego typu agenta zbiera dane dotyczące analizy dziennika. Typ danych, które są zbierane zależy od rodzaju rozwiązań używane. Można wyświetlić podsumowanie zbierania danych rozwiązania [Dodawanie analizy dziennika z galerii rozwiązań](log-analytics-add-solutions.md). Ponadto bardziej szczegółowe informacje o zbioru jest dostępna dla większości rozwiązań. Rozwiązanie jest pakiet wstępnie zdefiniowanych widoków, kwerend wyszukiwania dziennika reguły zbioru danych i logiki przetwarzania. Tylko administratorzy mogą używać analizy dziennika, aby zaimportować rozwiązanie. Po zaimportowaniu rozwiązania jest przenoszony do serwerów zarządzania programu Operations Manager (jeśli jest używany), a następnie na wszelkie czynniki, które wybrano. Później czynników zbieranie danych.

## <a name="2-send-data-from-agents"></a>2. Wyślij dane czynnikami

Zarejestruj wszystkie typy agenta przy użyciu klucza rejestrowania i bezpieczne połączenie jest nawiązywane między agentem a usługą analizy dziennika na podstawie certyfikatu uwierzytelniania i SSL za pomocą na porcie 443. Usługi OMS używa magazynu tajny do generowania i Obsługa kluczy. Klucze prywatne są obrócone co 90 dni i są przechowywane w Azure i zarządza operacje Azure, którzy wskazówkami ściśle przepisami i zgodność.

Z programu Operations Manager zarejestrować obszaru roboczego z usługą analizy dziennika i nawiązaniu bezpiecznego połączenia HTTPS między serwer zarządzania programu Operations Manager.

Dla systemu Windows agentami uruchomionych Azure maszyn wirtualnych klucz magazynowania tylko do odczytu służy do odczytu zdarzenia diagnostyczne w tabelach Azure.

W przypadku nie może nawiązać połączenia z usługą dowolnego powodu każdy agent zebranych danych jest przechowywany lokalnie w pamięci podręcznej tymczasowych i serwer zarządzania próby ponownego wysłania danych, co 8 minut przez dwie godziny. Buforowane agenta jest chroniony magazynu poświadczeń system operacyjny. Jeśli usługa nie może przetworzyć danych po 2 godzinach, agentów zostanie kolejka danych. Jeśli zapełni kolejki usługi OMS uruchamia upuszczanie typów danych, zaczynając od dane dotyczące wydajności. Limit kolejki agent jest klucza rejestru w celu zmodyfikowania, w razie potrzeby. Zebrane dane są kompresowane i wysyłane do usługi, pominięcia lokalnej bazy danych, aby nie dodawać wszelkie ładowanie do nich. Po wysłaniu zebranych danych zostanie usunięty z pamięci podręcznej.

Zgodnie z powyższym opisem danych z czynnikami są wysyłane przez SSL z centrami danych Microsoft Azure. Opcjonalnie można ExpressRoute zabezpieczenia dodatkowych danych. ExpressRoute to sposób bezpośrednio z Azure z istniejącej sieci WAN, takie jak wiele protokołów etykieta przełączania VPN (MPLS), dostarczony przez usługodawcę sieci. Aby uzyskać więcej informacji zobacz [ExpressRoute](https://azure.microsoft.com/services/expressroute/).


## <a name="3-the-log-analytics-service-receives-and-processes-data"></a>3 Usługa dziennika analizy odbiera i przetwarza dane

Usługi analizy dziennika sprawia, że dane przychodzące jest z zaufanego źródła, modyfikując certyfikaty i integralności danych z uwierzytelnianiem Azure. Nieprzetworzonych danych nieprzetworzonych następnie jest przechowywana jako obiektów blob w [Magazynie Microsoft Azure](../storage/storage-introduction.md) i nie są szyfrowane. Jednak każdy Azure magazyn obiektów blob ma zestaw unikatowego zestawu klawiszy, która jest dostępna tylko dla tego użytkownika. Typ danych, który jest przechowywany zależy od rodzaju rozwiązania, które zostały zaimportowane i używana do zbierania danych. Następnie usługa Analytics dziennika przetwarza dane dla blogu Azure miejsca do magazynowania.

## <a name="4-use-log-analytics-to-access-the-data"></a>4. za pomocą analizy dziennika uzyskiwać dostęp do danych

Możesz zalogować się do analizy dziennika w portalu usługi OMS za pomocą konta organizacji lub konta Microsoft, które skonfigurowano wcześniej. Cały ruch między portal usługi OMS i analizy dziennika w usługi OMS są wysyłane przez bezpiecznego kanału HTTPS. W przypadku korzystania z portalu usługi OMS, identyfikator sesji jest generowany na komputerze klienckim użytkownika (w przeglądarce sieci web), a dane są przechowywane w lokalnej pamięci podręcznej, aż do zakończenia sesji. Zakończone, pamięci podręcznej są usuwane. Pliki cookie po stronie klienta, które nie zawierają dane osobowe, nie są automatycznie usuwane. Pliki cookie sesji są oznaczane HTTPOnly i są zabezpieczone. Po ustalonej okres bezczynności zakończenia sesji portalu usługi OMS.

Za pomocą portalu usługi OMS, można eksportować dane do pliku CSV i masz dostęp do danych przy użyciu interfejsów API wyszukiwania. Eksport CSV jest ograniczone do 50 000 wierszy na eksportu i danych interfejsu API jest ograniczony do 5000 wierszy na wyszukiwania.

## <a name="next-steps"></a>Następne kroki

- [Wprowadzenie do analizy dziennika](log-analytics-get-started.md) , aby dowiedzieć się więcej o analizy dziennika i uzyskiwanie pracę w minutach.
