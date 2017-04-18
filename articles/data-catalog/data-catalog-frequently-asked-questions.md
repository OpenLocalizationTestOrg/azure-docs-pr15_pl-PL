<properties
   pageTitle="Azure wykazu danych — często zadawane pytania | Microsoft Azure"
   description="Często zadawane pytania dotyczące wykazu danych Azure, włącznie z funkcjami do odnajdowania źródła danych, adnotacji i zarządzania."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/04/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-frequently-asked-questions"></a>Azure wykazu danych — często zadawane pytania

Ten artykuł zawiera odpowiedzi na często zadawane pytania dotyczące usługi Microsoft **Azure wykazu danych** .

## <a name="q-what-is-azure-data-catalog"></a>P: co to jest Azure wykaz danych?

O: wykaz danych Microsoft Azure to usługa pełni zarządzany przechowywane w chmurze Microsoft Azure, która pełni funkcję systemu rejestracji i system odnajdowania dla źródeł danych w przedsiębiorstwie. Azure wykazu danych zawiera funkcje, które umożliwiają dowolnego użytkownika — z analityków ekspertów naukowych danych, aby deweloperów — Aby zarejestrować, odnajdowanie, opis i używanie źródeł danych.

## <a name="q-what-customer-challenges-does-azure-data-catalog-solve"></a>P: jakie klienta wzywa oznacza wykaz danych Azure rozwiązać?

Azure wykaz danych adresy największym wyzwaniem wykrywanie źródeł danych i "ciemny danych", umożliwiając użytkownikom odnajdowanie i opis źródła danych przedsiębiorstwa.

## <a name="q-who-are-the-target-audiences-for-azure-data-catalog"></a>P: kto są Docelowi odbiorcy dla wykazu danych Azure?

Wykaz danych Azure zapewnia możliwości technicznych i zarówno użytkowników, w tym:

- Deweloperów danych, BI i specjalistów analizy: osób, które są odpowiedzialne za tworzenie danych i analizy zawartości przez inne osoby do korzystania z
-   Stewards danych: Osób, które mają wiedzą o danych, co oznacza i jak mają być używane i do których celów
- Konsumentów danych: Tych, które muszą mieć możliwość łatwe odnajdowanie, opis i łączenie z danymi potrzebne do wykonania zadania przy użyciu narzędzia do siebie
- Centralny IT: tych osób, które należy setki źródeł danych odnajdowania dla użytkowników biznesowych i osób, które muszą korzystać z nadzorem nad sposób użycia danych i przez kogo

## <a name="q-what-is-the-azure-data-catalog-region-availability"></a>P: co to jest dostępność region wykaz danych Azure?

Usług Azure wykazu danych są obecnie dostępne w następujących centrach danych:

- Zachód Stany Zjednoczone
- Wschodniej Stany Zjednoczone
- Europa Zachodnia
- Europa Północna
- Wschód Australia
- Kraje Azji Południowo

## <a name="q-what-are-the-limits-on-the-number-of-data-assets-in-azure-data-catalog"></a>P: jakie są ograniczenia liczby danych składników majątku w wykazie danych Azure?

Bezpłatne Edition z Azure wykazu danych jest ograniczone do 5000 zarejestrowane dane zasoby.

Standard Edition z Azure wykazu danych obsługuje maksymalnie 100 000 elementów zarejestrowane dane.

## <a name="q-what-are-the-supported-data-source-and-asset-types"></a>P: co to są obsługiwane typy danych programu źródła i środków trwałych?

Zajrzyj do [DSR wykazu danych](data-catalog-dsr.md) na liście obecnie obsługiwanych źródeł danych.

## <a name="q-how-do-i-request-support-for-another-data-source"></a>P: jak żądanie pomocy technicznej dla innego źródła danych?

Funkcja zaproszenia i inne uwagi można wysłać na [forum Azure wykazu danych](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="q-how-do-i-get-started-with-azure-data-catalog"></a>P: jak rozpocząć pracę z wykazu danych Azure?

Jest najlepszym miejscem, aby rozpocząć pracę, wykonując instrukcje [rozpoczynania](data-catalog-get-started.md)pracy z wykazem danych. Ten artykuł dotyczy kompleksowe — omówienie funkcji w usłudze.

## <a name="q-how-do-i-register-my-data"></a>P: jak zarejestrować Moje dane?

Aby zarejestrować dane w wykazie danych Azure, uruchom narzędzie do wykazu danych Azure rejestracji z obszaru "Opublikuj" portal Azure wykazu danych. W aplikacji publikowania wykazu danych Azure Zaloguj się przy użyciu tych samych poświadczeń, którego używasz, aby uzyskać dostęp do portalu wykazu danych usługi Azure, a następnie wybierz źródło danych i określone elementy zawartości, które chcesz zarejestrować.

## <a name="q-what-properties-are-extracted-for-data-assets-that-are-registered"></a>P: jakie właściwości są wyodrębniane trwałych danych, które są zarejestrowane?

Określone właściwości będą różnić się od źródła danych w źródle danych, ale ogólnie usłudze publikowania wykazu danych Azure będzie wyodrębnić następujące informacje:

- Nazwy zasobów
- Typ zawartości
- Opis
- Nazwy atrybutów i kolumn
- Typy atrybut/kolumnę danych
- Opis atrybutu/kolumny

> [AZURE.IMPORTANT] Rejestrowanie zasobów danych dzięki wykazowi danych Azure nie przenoszenie lub kopiowanie danych do chmury. Rejestrowanie zasobów ze źródła danych zostaną skopiowane metadanych elementów członkowskich Azure, ale dane pozostaną w istniejącej lokalizacji źródła danych. Jedynym wyjątkiem od tej reguły jest, jeśli użytkownik wybierze przekazywanie rekordy podglądu lub profilu danych, gdy rejestrowanie zasobów. Po tym Podgląd, maksymalnie 20 rekordów zostaną skopiowane z poszczególnych elementów zawartości, a są przechowywane jako migawkę w wykazie danych Azure. Gdy w tym profilu danych, agregującą informacje (na przykład rozmiar tabel, procent wartości null w kolumnie i minimalną, maksymalną i średniej wartości dla kolumn) zostaną obliczone i zawarte w metadanych przechowywanych w katalogu.

<br/>

> [AZURE.NOTE] W przypadku źródeł danych, takich jak SQL Server Analysis Services, który ma najlepszych właściwości **Opis** wykazu danych Azure publikowanie aplikacji będzie wyodrębnić wartości tej właściwości. Dla programu SQL Server relacyjnych baz danych, która najlepszych właściwość **Opis** , aplikacji publikowania wykaz danych Azure będzie wyodrębnić wartości z ms_description rozszerzonego właściwości dla obiektów i kolumn. Aby uzyskać więcej informacji zobacz TechNet [Przy użyciu właściwości rozszerzone w obiektów bazy danych](https://technet.microsoft.com/library/ms190243%28v=sql.105%29.aspx).

## <a name="q-how-long-should-it-take-for-newly-registered-assets-to-appear-in-azure-data-catalog"></a>P: jak długo należy trwa nowo zarejestrowanych aktywów są wyświetlane w wykazie danych Azure?

Po zarejestrowaniu składników majątku z wykazem danych Azure może być okres 5-10 sekund przed wyświetleniem portal Azure wykazu danych.

## <a name="q-how-do-i-annotate-and-enrich-the-metadata-for-my-registered-data-assets"></a>P: jak dodawanie adnotacji i wzbogacanie metadane dla mojej trwałych zarejestrowane dane?

Najłatwiejszym sposobem udostępnienia metadanych dla środków zarejestrowanych jest wybierz trwały w portal Azure wykazu danych, a następnie wprowadź wartości metadanych w okienku właściwości lub w okienku schematu dla zaznaczonego obiektu.

Możesz także podać niektóre metadane, takie jak ekspertów i znaczniki, podczas procesu rejestracji. Wartości podane w usłudze publikowania wykazu danych Azure zostaną zastosowane do wszystkich trwałych rejestrowany w tym czasie. Aby wyświetlić ostatnio zarejestrowanych obiektów w portalu dodatkowe adnotacji, kliknij przycisk **Widok portalu** na ostatnim ekranie aplikacji do wykazu danych usługi Azure publikowania.

## <a name="q-how-do-i-delete-my-registered-data-objects"></a>P: jak usunąć obiekty zarejestrowane dane?

Obiekt z wykazu danych Azure można usunąć przez zaznaczenie obiektu w portalu, a następnie klikając przycisk **Usuń** . To spowoduje usunięcie metadanych obiektu z wykazu danych Azure, ale nie wpływa na rzeczywiste źródła danych.

## <a name="q-what-is-an-expert"></a>P: co to jest ekspertem?

Ekspertem to osoba, która ma perspektywy informacje o obiekcie danych. Obiekt może mieć wiele ekspertów. Ekspertem musi być "właściciel" obiektu; ekspertem po prostu to osoba, która wie, jak można dane i powinien być używany.

## <a name="q-how-do-i-share-information-with-the-azure-data-catalog-team-if-i-encounter-problems"></a>P: jak udostępnić informacje z zespołem wykaz danych Azure I wystąpienia problemów?

Za pomocą wykazu danych Azure forum zgłaszania problemów, udostępnianie informacji i Zadaj pytania. Forum można znaleźć w http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409

##<a name="q-does-azure-data-catalog-work-with-this-other-data-source-im-interested-in"></a>P: wykazu danych usługi Azure działa z tego innego źródła danych, które interesuje mnie?
Aktywnie pracujemy nad dodawania większej liczby źródeł danych do wykazu danych Azure. W przypadku źródła danych, które chcesz Zobacz obsługiwane, należy go proponowanie (lub głosowej pomocy technicznej w swoim, jeśli został już sugerowane) na [forum Azure wykazu danych](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="q-how-is-azure-data-catalog-related-to-the-data-catalog-in-power-bi-for-office-365"></a>P: jak wykazu danych Azure dotyczy w wykazie danych w usłudze Power BI dla Office 365?

Wykaz danych Azure można traktować jako zmiany wykazu danych. Azure wykazu danych zapewnia podobne funkcje publikowania źródła danych i odnajdowanie, ale jest mającego fokus na scenariuszach szerszym i niezależne w usłudze Office 365. Wkrótce po wykazu danych Azure staje się ogólnodostępne dwa katalogi będą scalić jednej usługi.

## <a name="q-what-permissions-does-a-user-need-to-register-assets-with-azure-data-catalog"></a>P: jakie uprawnienia czy użytkownika należy zarejestrować składniki majątku z wykazem danych Azure?

Uprawnienia do źródła danych, umożliwiająca go do odczytu metadanych źródłowej musi uruchomione narzędzie do wykazu danych Azure rejestracji przez użytkownika. Jeśli użytkownik wybierze również aby dołączyć podgląd, w użytkownik musi także mieć uprawnienia umożliwiające im więcej danych z obiektów rejestrowana.

## <a name="q-will-azure-data-catalog-be-made-available-for-on-premises-deployment-as-well"></a>P: Azure wykazu danych będą dostępne dla lokalnego wdrożenia?

Azure wykazu danych jest usługa w chmurze, który można pracować z zarówno w chmurze, jak i w lokalnych źródeł danych, dostarczania hybrydowych rozwiązanie odnajdowania źródła danych. Są obecnie nie ma planów dla wersji usługi Azure wykaz danych, która będzie działać w lokalnej.

##<a name="q-can-we-extract-more--richer-metadata-from-the-data-sources-we-register"></a>P: czy firma Microsoft wyodrębnianie metadanych więcej / bogatsze ze źródła danych, które było zarejestrować

Pracujemy aktywnie rozszerza możliwości wykazu danych Azure. W przypadku dodatkowe metadane, które chcesz wyświetlić wyodrębnione ze źródła danych podczas rejestrowania należy Zaproponuj go (lub w głosowaniu dla niego, o ile ma już sugerowane) na [forum Azure wykazu danych](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). W przyszłości będziemy umożliwi stronom trzecim Dodawanie nowych typów źródeł danych za pośrednictwem interfejs API rozszerzania.

## <a name="q-how-do-i-restrict-the-visibility-of-registered-data-assets-so-that-only-certain-people-can-discover-them"></a>P: jak ograniczyć widoczność aktywów zarejestrowane dane, tak, aby tylko niektóre osoby mogą one je znaleźć?

O: wybrać trwałe danych w wykazie danych Azure i kliknij przycisk "Własność". Właściciele środków trwałych danych w wykazie danych Azure mogą zmienić ustawienia widoczności, albo Pozwól wszystkim użytkownikom wykaz, aby odkryć własnością składniki majątku lub ograniczyć widoczność do określonych użytkowników.

## <a name="q-how-do-i-update-the-registration-for-a-data-asset-to-that-changes-in-the-data-source-are-reflected-in-the-catalog"></a>P: jak zaktualizować rejestracji trwałego danych, który zmienia się w źródle danych, zostaną one odzwierciedlone w wykazie?

O: Aby zaktualizować metadane dla elementów danych, które zostały już zarejestrowane w wykazie, wystarczy ponownie zarejestrować źródła danych, która zawiera składniki majątku. Wszelkie zmiany w źródle danych, takie jak kolumny zostanie dodane lub usunięte z tabel lub widoków, zostaną zaktualizowane w katalogu, ale adnotacji dostarczony przez użytkowników zostaną zachowane.

## <a name="q-my-question-isnt-answered-here--what-should-i-do"></a>P: mojego pytania nie ma tutaj odpowiedzi — co mogę zrobić?

Głowy na na [forum Azure wykazu danych](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). Pytania zadawane spowoduje znalezienie jak poniżej.
