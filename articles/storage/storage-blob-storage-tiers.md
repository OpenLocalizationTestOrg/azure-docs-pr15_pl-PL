<properties
    pageTitle="Azure atrakcyjne magazyn obiektów blob | Microsoft Azure"
    description="Poziomy miejsca do magazynowania dla magazyn obiektów Blob platformy Azure oferuje wydajne miejsca do magazynowania dla danych obiektu na podstawie wzorców programu access. Warstwa atrakcyjne miejsca do magazynowania jest zoptymalizowana pod kątem danych, który jest dostępny rzadziej."
    services="storage"
    documentationCenter=""
    authors="michaelhauss"
    manager="vamshik"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="mihauss"/>


# <a name="azure-blob-storage-hot-and-cool-storage-tiers"></a>Magazyn obiektów Blob platformy Azure: Hot i schłodzić poziomów miejsca do magazynowania

## <a name="overview"></a>Omówienie

Miejsca do magazynowania Azure oferuje dwie warstwy miejsca do magazynowania dla magazyn obiektów Blob (obiekt miejscem do magazynowania), dzięki czemu można przechowywać danych najbardziej ekonomiczne zależności od tego, jak z niej korzystać. Azure **ciepłej magazynowania warstwa** jest zoptymalizowana pod kątem przechowywania danych, który jest dostępny często. Azure **atrakcyjne miejsca do magazynowania warstwa** jest zoptymalizowana pod kątem przechowywania danych, czyli rzadko używanych i długotrwałe. Dane w warstwie atrakcyjne miejsca do magazynowania przeszkadzają nieco niższy dostępność, ale nadal wymaga wysokiej wytrzymałości i podobne czasu, aby uzyskać dostęp do i przepustowość właściwości ciepłej danych. Atrakcyjne danych nieco niższy dostępność Umowa dotycząca poziomu usług i wyższych kosztów programu access są dopuszczalne korzystnych rozwiązań dla dużo niższe koszty miejsca do magazynowania.

Obecnie dane przechowywane w chmurze rośnie w tempie wykładniczy. Aby zarządzać kosztów dla potrzeb rozwijanym miejsca do magazynowania, dobrze jest zorganizować dane według atrybutów, takich jak częstotliwość dostępu i okres przechowywania planowanych. Dane przechowywane w chmurze mogą być bardzo różne jak go jest generowany, przetwarzania i dostępna za pośrednictwem jego użytkowania. Niektóre dane aktywnie jest dostępna po kliknięciu pozycji i modyfikować w jej okresie istnienia. Niektóre dane jest dostępny bardzo często wczesnym etapie jego istnienia, z upuszczanie znaczna jako wieku danych programu access. Niektóre dane pozostaną bezczynności w chmurze i są dostępne, jeśli kiedykolwiek, dostępne po przechowywane.

Każdy z tych scenariuszy dostępu do danych opisanych powyżej korzyści wynikających z zróżnicowane warstwy miejsca przeznaczonego do deseniu dostępu. Wraz z wprowadzeniem poziomów ciepłej i atrakcyjne magazynu obiektów Blob platformy Azure miejsca do magazynowania adresy teraz trzeba poziomów zróżnicowane miejsca do magazynowania z oddzielne ceny modeli.

## <a name="blob-storage-accounts"></a>Konta magazyn obiektów blob

**Konta magazyn obiektów blob** są specjalistyczne magazynowania konta do przechowywania danych niestrukturalne jako obiektów blob (obiekty) w magazynie Azure. Z kontami magazyn obiektów Blob można teraz wybrać poziomów ciepłej i atrakcyjne miejsca do magazynowania do przechowywania danych atrakcyjne rzadziej używanych w dolnym przestrzeni dyskowej kosztów i przechowywania często używanych danych ciepłej w dolnym dostępu, koszt. Konta magazyn obiektów blob są podobne do istniejącego konta ogólnego przeznaczenia miejsca do magazynowania i udostępnianie wszystkich doskonałe wytrzymałości, dostępność, skalowalność i wydajność funkcji używanych obecnie łącznie 100% spójności interfejsu API dla obiektów blob blok i dołączanie obiektów blob.

> [AZURE.NOTE] Konta magazyn obiektów blob obsługuje tylko blok i dołączanie obiektów blob, a nie obiektów blob strony.

Konta magazyn obiektów blob uwidaczniają atrybutu **Poziomu dostępu** umożliwiają określenie poziomu miejsca do magazynowania jako **aktywny** lub **atrakcyjne** w zależności od danych przechowywanych w oknie konta. W przypadku zmiany w strukturze zastosowania danych, możesz także przełączać się między tych poziomów miejsca do magazynowania w dowolnym momencie.

> [AZURE.NOTE] Zmiana poziomu magazynowania może spowodować naliczenie dodatkowych opłat. Zobacz sekcję [ceny i rozliczenia](storage-blob-storage-tiers.md#pricing-and-billing) , aby uzyskać więcej informacji.

Przykładowe scenariusze zastosowania dla poziomu dostępu miejsca do magazynowania obejmują:

- Dane, które jest aktywnie używane lub oczekiwać, że są dostępne (odczytu z i zapisane) często.
- Dane, które są umieszczane przetwarzania i ewentualnego migracji w warstwie atrakcyjne miejsca do magazynowania.

Przykładowe scenariusze zastosowania dla poziomu atrakcyjne miejsca do magazynowania obejmują:

- Wykonywanie kopii zapasowych, archiwizacji i zestawy danych odzyskiwania po awarii.
- Starsze zawartości multimedialnej nie często już wyświetlane, ale ma być dostępny od razu, podczas uzyskiwania dostępu do.
- Dużych zestawów danych, które mają być przechowywane koszt skuteczne podczas przetwarzania przyszłych zbiera się większej ilości danych. (*przykład*długotrwałego przechowywania danych naukowych telemetrycznego nieprzetworzonych danych z pomieszczenia produkcji)
- Oryginalne dane (nieprzetworzonych), które mają zostać zachowane, nawet po przetworzeniu w ostatecznej formie użyteczne. (*np.*pliki multimedialne nieprzetworzonych po przekodowanie na inne formaty)
- Zgodność i archiwizacja danych, który musi być przechowywany przez dłuższy czas i bardzo rzadko jest dostępna. *(, materiału kamery zabezpieczeń, stare X-Rays-MRIs dla organizacji ochrony zdrowia, nagrania audio i transkrypcje połączeń klienta usług finansowych)*

Zobacz [temat Azure miejsca do magazynowania konta](storage-create-storage-account.md) uzyskać więcej informacji dotyczących konta miejsca do magazynowania.

Aplikacje wymagające tylko zablokować lub dołączanie magazyn obiektów blob, zaleca się przy użyciu kont magazyn obiektów Blob, aby można było korzystać zróżnicowane modelu cennik warstwowych przestrzeni dyskowej. Jednak firma Microsoft wie, że to może nie być możliwe w niektórych okolicznościach gdy za pomocą kont ogólnego przeznaczenia magazynowania byłyby tak, aby przejść, takich jak:

- Należy używać tabel, kolejek lub plików i chcesz z obiektami blob przechowywanych w tym samym kontem miejsca do magazynowania. Należy zauważyć, że jest nie techniczne zaletą przechowywania tych z tego samego konta innego niż o tej samej udostępniane klawiszy.
- Nadal potrzebujesz za pomocą modelu wdrożenia klasyczny. Konta magazyn obiektów blob są dostępne tylko za pomocą Menedżera zasobów Azure model wdrożenia.
- Należy używać obiektów blob strony. Konta magazyn obiektów blob nie obsługują blob strony. Zazwyczaj zaleca się przy użyciu obiektów blob blok, chyba że określone muszą dla obiektów blob strony.
- W wersji starszej niż 4.x przy użyciu wersji [Interfejsu API usługi REST usług miejsca do magazynowania](https://msdn.microsoft.com/library/azure/dd894041.aspx) starszej niż 2014-02-14 lub biblioteki klienta i nie można uaktualnić aplikacji.

> [AZURE.NOTE] Konta magazyn obiektów blob obecnie są obsługiwane w większości Azure regiony ze więcej do obserwowania. Można znaleźć zaktualizowaną listą regionów dostępne na stronie [Usługi Azure według regionów](https://azure.microsoft.com/regions/#services) .

## <a name="comparison-between-the-storage-tiers"></a>Porównanie poziomów miejsca do magazynowania

W poniższej tabeli wymieniono porównanie między dwoma poziomami miejsca do magazynowania:

<table border="1" cellspacing="0" cellpadding="0" style="border: 1px solid #000000;">
<col width="250">
<col width="250">
<col width="250">
<tbody>
<tr>
    <td><strong><center></center></strong></td>
    <td><strong><center>Poziom dostępu miejsca do magazynowania</center></strong></td>
    <td><strong><center>Warstwa atrakcyjne miejsca do magazynowania</center></strong></td
</tr>
<tr>
    <td><strong><center>Dostępność</center></strong></td>
    <td><center>99,9%</center></td>
    <td><center>99%</center></td>
</tr>
<tr>
    <td><strong><center>Dostępność<br>(Pomoc Zdalna GRS odczytuje)</center></strong></td>
    <td><center>99,99%</center></td>
    <td><center>99,9%</center></td>
</tr>
<tr>
    <td><strong><center>Opłaty za zużycie</center></strong></td>
    <td><center>Wyższe koszty miejsca do magazynowania<br>Niższe koszty dostępu i transakcji</center></td>
    <td><center>Niższe koszty magazynowania<br>Wyższe koszty dostępu i transakcji</center></td>
</tr>
<tr>
    <td><strong><center>Minimalny rozmiar obiektów<center></strong></td>
    <td colspan="2"><center>N/D!</center></td>
</tr>
<tr>
    <td><strong><center>Czas trwania minimalne miejsca do magazynowania<center></strong></td>
    <td colspan="2"><center>N/D!</center></td>
</tr>
<tr>
    <td><strong><center>Czas oczekiwania<br>(Pierwszy bajt czasu)<center></strong></td>
    <td colspan="2"><center>milisekundy</center></td>
</tr>
<tr>
    <td><strong><center>Wydajność i skalowalność elementów docelowych<center></strong></td>
    <td colspan="2"><center>Identyczne z kontami ogólnego przeznaczenia miejsca do magazynowania</center></td>
</tr>
</tbody>
</table>

> [AZURE.NOTE] Konta magazyn obiektów blob obsługuje samej wydajność i skalowalność elementów docelowych jako konta ogólnego przeznaczenia miejsca do magazynowania. Aby uzyskać więcej informacji, zobacz [skalowalność miejsca do magazynowania Azure i cele](storage-scalability-targets.md) .

## <a name="pricing-and-billing"></a>Ceny i rozliczenia

Konta magazyn obiektów blob Użyj nowego modelu cennik dla magazyn obiektów blob oparte na warstwie miejsca do magazynowania. Przy użyciu konta magazyn obiektów Blob, mają zastosowanie następujące kwestie rozliczeń:

- **Koszty magazynowania**: Oprócz ilości danych przechowywanych koszt przechowywania danych zależnie od poziomu miejsca do magazynowania. Koszt na gigabajt jest niższa dla poziomu atrakcyjne miejsca do magazynowania niż dla poziomu dostępu miejsca do magazynowania.
- **Koszty dostępu do danych**: dla danych w warstwie atrakcyjne miejsca do magazynowania, zostanie naliczona opłata na gigabajt danych programu access do odczytu i zapisu.
- **Koszty transakcji**: jest opłatę każdej transakcji dla obu poziomów. Jednak koszt każdej transakcji dla poziomu atrakcyjne miejsca do magazynowania jest wyższy niż w przypadku poziomu dostępu miejsca do magazynowania.
- **Koszty przeniesienia danych replikacji Geo**: to dotyczy tylko kont z geo replikacji skonfigurowane, takich jak GRS i GRS pomoc Zdalna. Transfer danych replikacji Geo wiąże się opłatę za GB.
- **Transferu koszty danych ruchu wychodzącego**: przesyłanie danych ruchu wychodzącego (dane są przenoszone poza Azure regionu) ponoszenia rozliczeń użycia przepustowości na podstawie na gigabajt, zgodnie z kontami ogólnego przeznaczenia miejsca do magazynowania.
- **Zmienianie poziomu miejsca do magazynowania**: zmiana poziomu miejsca do magazynowania z ochłodzić do hot będzie powodowało opłatę równą wszystkich danych znajdujących się na koncie miejsca do magazynowania każdej przejścia do czytania. Z drugiej strony zmieniając warstwie miejsca do magazynowania z ciepłej ostygnąć będzie bezpłatnie.

> [AZURE.NOTE] W celu umożliwienia użytkownikom wypróbować nowe poziomy magazynowania i sprawdź poprawność funkcji po wprowadzeniu opłaty za zmiany miejsca przechowywania warstwa z ochłodzić do hot będzie można zrezygnować wyłączone do momentu 2016 30 czerwca. Uruchamianie 2016 1 lipca, opłaty zostanie zastosowany do wszystkich przejścia z ochłodzić do hot. Więcej informacji na temat cennik wzór magazyn obiektów Blob kont Zobacz [Ceny miejsca do magazynowania Azure](https://azure.microsoft.com/pricing/details/storage/) strony. Więcej informacji na temat ruchu wychodzącego danych opłat przekazania zobacz strony [Szczegóły ceny przeniesienia danych](https://azure.microsoft.com/pricing/details/data-transfers/) .

## <a name="quick-start"></a>Szybki Start

W tej sekcji możemy przedstawi następujących scenariuszy przy użyciu portalu Azure:

- Jak utworzyć konto magazyn obiektów Blob.
- Jak zarządzać kontem magazyn obiektów Blob.

### <a name="using-the-azure-portal"></a>Za pomocą portalu Azure

#### <a name="create-a-blob-storage-account-using-the-azure-portal"></a>Tworzenie konta magazyn obiektów Blob za pomocą portalu Azure

1. Zaloguj się do [portalu Azure](https://portal.azure.com).

2. W menu Centrum wybierz **Nowy** > **danych + miejsca do magazynowania** > **konta miejsca do magazynowania**.

3. Wprowadź nazwę konta magazynu.

    Ta nazwa musi być globalnie unikatowe; jest używany jako część adresu URL, umożliwiające dostęp do obiektów w oknie konta miejsca do magazynowania.  

4. Wybierz pozycję **Menedżer zasobów** jako model wdrożenia.

    Magazyn warstwowych można używać tylko z kontami miejsca do magazynowania Menedżera zasobów; to jest model wdrożenia zalecane dla nowych zasobów. Aby uzyskać więcej informacji zapoznaj się z [Omówienie Menedżera zasobów Azure](../azure-resource-manager/resource-group-overview.md).  

5. Na liście rozwijanej Typ konta wybierz **Magazyn obiektów Blob**.

    Jest to, gdzie wybierz typ konta miejsca do magazynowania. Magazyn warstwowych nie jest dostępna w magazynie ogólnego przeznaczenia; jest on dostępny tylko w oknie konta typu magazyn obiektów Blob.    

    Należy zauważyć, że po wybraniu tej warstwie wydajności jest na standardowy. Magazyn warstwowych nie jest dostępna z poziomu wydajności Premium.

6. Wybierz opcję replikacji dla konta miejsca do magazynowania: **LRS**, **GRS**lub **GRS pomoc Zdalna**. Wartość domyślna to **GRS pomoc Zdalna**.

    LRS = lokalnie zbędne przechowywania; GRS = zbędne geo miejsca do magazynowania (2 regiony); Pomoc Zdalna GRS jest dostęp do odczytu zbędne geo miejsca do magazynowania (2 regiony z dostępem do odczytu do drugiego).

    Aby uzyskać więcej informacji na temat opcji replikacji magazyn Azure zapoznaj się z [replikacji magazyn Azure](storage-redundancy.md).

7. Wybierz poziom przechowywania dla potrzeb: Ustawianie **poziomu dostępu** do **atrakcyjne** lub **aktywny**. Wartość domyślna to **aktywny**.

8. Wybierz subskrypcję, w którym chcesz utworzyć nowe konto miejsca do magazynowania.

9. Podaj nową grupę zasobów lub zaznacz istniejącej grupy zasobów. Aby uzyskać więcej informacji dotyczących grup zasobów zobacz [Omówienie Menedżera zasobów Azure](../azure-resource-manager/resource-group-overview.md).

10. Wybierz region dla Twojego konta miejsca do magazynowania.

11. Kliknij przycisk **Utwórz** , aby utworzyć konto miejsca do magazynowania.

#### <a name="change-the-storage-tier-of-a-blob-storage-account-using-the-azure-portal"></a>Zmienianie warstwie przestrzeni dyskowej konto magazyn obiektów Blob przy użyciu Azure portal

1. Zaloguj się do [portalu Azure](https://portal.azure.com).

2. Aby przejść do swojego konta miejsca do magazynowania, zaznacz wszystkie zasoby, a następnie wybierz konto miejsca do magazynowania.

3. W karta Ustawienia kliknij **konfiguracji** , aby wyświetlić lub zmienić konfigurację konta.

4. Wybierz poziom przechowywania dla potrzeb: Ustawianie **poziomu dostępu** do **atrakcyjne** lub **aktywny**.

5. Kliknij przycisk Zapisz w górnej części karta.

> [AZURE.NOTE] Zmiana poziomu magazynowania może spowodować naliczenie dodatkowych opłat. Zobacz sekcję [ceny i rozliczenia](storage-blob-storage-tiers.md#pricing-and-billing) , aby uzyskać więcej informacji.

## <a name="evaluating-and-migrating-to-blob-storage-accounts"></a>Oceny i przeprowadzania migracji z kontami magazyn obiektów Blob

Celem w tej sekcji jest pomoc użytkownikom na wprowadzanie płynnego przejścia do za pomocą kont magazyn obiektów Blob. Istnieją dwa scenariusze użytkownika:

- Masz istniejącego konta ogólnego przeznaczenia miejsca do magazynowania i chce ocenić zmiany na koncie magazyn obiektów Blob z poziomu przechowywania.
- POSTANOWIŁY Użyj konta magazyn obiektów Blob lub już istnieje i chcesz, aby sprawdzić, czy należy używać warstwie ciepłej lub atrakcyjne miejsca do magazynowania.

W obu przypadkach firmy z pierwszej kolejności jest szacowanie kosztów przechowywania i uzyskiwanie dostępu do danych przechowywanych w konto magazyn obiektów Blob i porównać który bieżącego.

### <a name="evaluating-blob-storage-account-tiers"></a>Sprawdzanie poziomów konta magazyn obiektów Blob

Aby oszacować koszty przechowywania i dostęp do danych przechowywanych na koncie magazyn obiektów Blob, będzie konieczne oceny istniejących deseniu zastosowania lub przybliżenia deseniu oczekiwane wykorzystanie. Na ogół chcesz wiedzieć:

- Usługi zużycie magazynowania — jak dużo danych są przechowywane i jak czy to zmieni miesięcznych?
- Deseń dostępu do miejsca do magazynowania - ilości danych jest odczytywania i zapisywania na koncie (w tym nowe dane)? Ile transakcje są używane, aby uzyskać dostęp do danych i typach transakcji są one?

#### <a name="monitoring-existing-storage-accounts"></a>Monitorowanie istniejących kont miejsca do magazynowania

Monitoruj swoje istniejącego konta miejsca do magazynowania i zbieranie danych, mogą być wykorzystanie Azure analizy miejsca do magazynowania, który wykonuje rejestrowanie, a także danych metryki konta miejsca do magazynowania.
Miejsca do magazynowania analizy mogą zawierać metryki zawierające zagregowane transakcji statystyki i pojemnością dane dotyczące żądań z usługą Magazyn obiektów Blob zarówno ogólnego przeznaczenia magazynu kont, a także konta magazyn obiektów Blob.
Te dane są przechowywane w tabelach znanych z tego samego konta miejsca do magazynowania.

Aby uzyskać więcej informacji można znaleźć w [O metryki analizy magazynowania](https://msdn.microsoft.com/library/azure/hh343258.aspx) i [Schematu tabeli metryk analizy miejsca do magazynowania](https://msdn.microsoft.com/library/azure/hh343264.aspx)

> [AZURE.NOTE] Konta magazyn obiektów blob uwidaczniają punktu końcowego usługi tabeli tylko do przechowywania danych metryki dla tego konta.

Aby monitorować zużycie miejsca do magazynowania dla usługi Magazyn obiektów Blob, będzie konieczne włączenie metryki wydajność.
Dzięki temu włączone zdolności dane są zapisywane dzienny konto magazyn obiektów Blob usługi i zapisuje jako wpis tabeli, który jest zapisywany w tabeli *$MetricsCapacityBlob* w ramach tego samego konta miejsca do magazynowania.

Aby monitorować wzorzec dostępu do danych usługi Magazyn obiektów Blob, będzie konieczne włączenie godzinowe metryki transakcji na poziomie interfejsu API.
Dzięki temu włączone na interfejsu API transakcje są agregacją co godzinę, a został zapisany jako wpis tabeli, który jest zapisywany w tabeli *$MetricsHourPrimaryTransactionsBlob* w ramach tego samego konta miejsca do magazynowania. W tabeli *$MetricsHourSecondaryTransactionsBlob* rejestruje transakcje pomocniczej punktu końcowego w przypadku kont miejsca do magazynowania GRS pomoc Zdalna.

> [AZURE.NOTE] W przypadku, gdy masz konto ogólnego przeznaczenia miejsca do magazynowania, w którym są przechowywane blob strony i maszyn wirtualnych dysków razem z pakietem blok i dołączanie danych obiektów blob, ten proces szacowania nie jest stosowana. Jest tak, ponieważ będzie miała możliwości pojemności wyróżniający i metryk transakcji według typu blob dla tylko zablokować, a następnie dołączyć obiektów blob, które możesz przeprowadzić migrację do konta magazyn obiektów Blob.

Aby uzyskać dobre przybliżenie zużycie danych i wzorzec dostępu, zalecamy wybierz okres przechowywania dla miar, które jest przedstawiciela do normalnego użytkowania, a ekstrapolacja.
Jedną z opcji jest zachowanie danych metryki przez 7 dni i zbierania danych co tydzień, analizy na koniec miesiąca.
Innym rozwiązaniem jest zachowanie danych metryki dla ostatnich 30 dni i zbieranie i analizowanie danych na końcu okresu 30 dni.

Aby uzyskać szczegółowe informacje o włączaniu zbierania i wyświetlania danych metryki można znaleźć, [metryki magazynowania Włączanie Azure i przeglądania danych metryki](storage-enable-and-view-metrics.md).

> [AZURE.NOTE] Przechowywania i pobierania analizy danych i uzyskiwanie dostępu do również jest naliczany tak jak zwykły użytkownik danych.

#### <a name="utilizing-usage-metrics-to-estimate-costs"></a>Korzystanie z metryki zastosowania oszacowanie kosztów

##### <a name="storage-costs"></a>Koszty miejsca do magazynowania

Najnowsze wpisy w zdolności metryki tabeli *$MetricsCapacityBlob* przy użyciu klucza w wierszu *"dane"* Pokazuje pojemność zużywanej przez dane użytkownika.
Najnowsze wpisy w zdolności metryki tabeli *$MetricsCapacityBlob* przy użyciu klucza w wierszu *"analizy"* Pokazuje pojemność zużywanej przez dzienniki analizy.

Ten pojemność zużywanej przez zarówno dane użytkownika, a następnie można dzienników analizy (Jeśli włączono tę funkcję) do oszacowania kosztu przechowywania danych w oknie konta miejsca do magazynowania.
Ten sam sposób można również uzyskać Szacowanie kosztów miejsca do magazynowania dla bloku i dołączanie obiektów blob w konta ogólnego przeznaczenia miejsca do magazynowania.

##### <a name="transaction-costs"></a>Koszty transakcji

Suma *"TotalBillableRequests"*dla wszystkich pozycji dla interfejsu API w tabeli metryki wskazuje całkowita liczba transakcji dla tego określonego interfejsu API. *przykład*całkowita liczba transakcji *"GetBlob"* w danym okresie mogą być obliczane przez Suma całkowita liczba żądań fakturowanego dla wszystkich pozycji kluczem wiersza *"użytkownika; GetBlob "*.

Aby można było Szacowanie kosztów transakcja dla kont magazyn obiektów Blob, będzie konieczne podziału transakcji do trzech grup, ponieważ są one kosztują inaczej.

- Napisz transakcje, takie jak *"PutBlob"*, *"PutBlock"*, *"PutBlockList"*, *"AppendBlock"*, *"ListBlobs"*, *"ListContainers"*, *"Tworzony kontener"*, *"SnapshotBlob"*i *"CopyBlob"*.
- Usuń transakcje, takie jak *"DeleteBlob"* i *"DeleteContainer"*.
- Inne transakcje.

Aby Szacowanie kosztów transakcja dla kont ogólnego przeznaczenia przestrzeni dyskowej, należy agregowanie wszystkich transakcji niezależnie od operacji i interfejsu API.

##### <a name="data-access-and-geo-replication-data-transfer-costs"></a>Koszty przeniesienia danych programu access i geo replikacji

Podczas analizy miejsca do magazynowania nie zawiera zakres danych odczytywania i zapisywania na koncie miejsca do magazynowania, może być około oszacowana przeglądając tabeli metryki.
Suma *"TotalIngress"* dla wszystkich pozycji dla interfejsu API w tabeli metryki wskazuje łączną liczbę danych wchodzącego w bajtach dla tego określonego interfejsu API.
Podobnie sumę *"TotalEgress"* oznacza łączną liczbę dane wyjściowe, w bajtach.

Aby można było Szacowanie kosztów dostępu do danych w przypadku kont magazyn obiektów Blob, będzie konieczne podziału transakcji w dwóch grupach.

- Ilość danych pobieranych z konta magazynu można oszacować sprawdzając sumę *"TotalEgress"* przede wszystkim operacji *"GetBlob"* i *"CopyBlob"* .
- Ilość zapisanych na koncie miejsca do magazynowania danych można oszacować sprawdzając sumę *"TotalIngress"* dla przede wszystkim *"PutBlob"*, *"PutBlock"*, *"CopyBlob"* i *"AppendBlock"* operacji.

Koszt replikacji geo przesyłania danych w przypadku kont magazyn obiektów Blob oblicza się przy użyciu oszacowanie przez dane zapisane w przypadku konta miejsca do magazynowania GRS lub GRS pomoc Zdalna.

> [AZURE.NOTE] Na przykład bardziej szczegółowych informacji o obliczania kosztów dotyczące korzystania z poziomu dostępu lub atrakcyjne miejsca do magazynowania Zobacz zapoznaj się często Zadawanych pytań *"co to jest aktywny i ochłodzić poziomów dostępu i jak należy ustalić, który ma być use?"* w [Azure magazynowania ceny strony](https://azure.microsoft.com/pricing/details/storage/).

### <a name="migrating-existing-data"></a>Migrowanie istniejących danych

Konto magazyn obiektów Blob jest przeznaczone do przechowywania tylko blok i dołączanie obiektów blob. Istniejące konta ogólnego przeznaczenia miejsca do magazynowania, które umożliwiają przechowywanie tabel, kolejki, pliki i dyski, a także obiektów blob, nie można przekonwertować na kontach magazyn obiektów Blob. Aby użyć poziomów przestrzeni dyskowej, należy utworzyć nowe konta magazyn obiektów Blob i Migrowanie istniejących danych do nowo utworzonych kont.
Migrowanie istniejących danych do konta magazyn obiektów Blob z magazynu lokalnego urządzenia, dostawców magazynu w chmurze innych firm lub istniejącego konta ogólnego przeznaczenia magazynu platformy Azure umożliwia następujących metod:

#### <a name="azcopy"></a>AzCopy

AzCopy jest przeznaczona do wysokiej wydajności kopiowania danych z magazynu Azure narzędzie wiersza polecenia systemu Windows. Za pomocą AzCopy, aby skopiować dane do konta usługi Magazyn obiektów Blob z istniejącego konta ogólnego przeznaczenia miejsca do magazynowania lub przekazywać dane z urządzenia magazynu lokalnego do swojego konta magazyn obiektów Blob.

Aby uzyskać więcej informacji zobacz [Przenoszenie danych za pomocą narzędzia wiersza polecenia AzCopy](storage-use-azcopy.md).

#### <a name="data-movement-library"></a>Biblioteka przepływu danych

Azure Biblioteka przepływu danych miejsca do magazynowania dla środowiska .NET jest oparty na framework przepływu danych podstawowych uprawnień AzCopy. Biblioteka jest przeznaczona dla operacji przekazywania danych wysokiej wydajności, wiarygodnych i łatwo jest podobne do AzCopy. Dzięki temu będzie można wykonać pełne korzyści funkcje oferowane przez AzCopy w aplikacji oryginalnie bez konieczności zajmowania uruchomiony i monitorowanie zewnętrznych wystąpienia AzCopy.

Aby uzyskać więcej informacji zobacz [Biblioteki przepływu danych Azure miejsca do magazynowania dla środowiska .net](https://github.com/Azure/azure-storage-net-data-movement)

#### <a name="rest-api-or-client-library"></a>Interfejsu API usługi REST lub w bibliotece klienta

Możesz utworzyć niestandardową aplikację do migrowania danych do konta magazyn obiektów Blob przy użyciu jednej z usług Azure magazynu interfejsu API usługi REST lub bibliotek Azure klienta. Magazyn Azure oferuje bibliotek klienta wzbogaconego dla wielu języków i platform, takich jak .NET, języka Java, C++, Node.JS, PHP, dopiskiem i Python. Bibliotek klienta oferują zaawansowane funkcje, takie jak ponów próbę logicznych, rejestrowanie i równoległych przekazywania. Można także tworzyć bezpośrednio z interfejsu API usługi REST, który może być wywoływana przez dowolny język, który sprawia, że żądania HTTP/HTTPS.

Aby uzyskać więcej informacji zobacz [Rozpoczynanie pracy z magazynem obiektów Blob platformy Azure](storage-dotnet-how-to-use-blobs.md).

> [AZURE.NOTE] Blob zaszyfrowany przy użyciu szyfrowania po stronie klienta zawierają związane z szyfrowania metadanych z obiektem blob. Jest znaczenie krytyczne jakiegokolwiek mechanizmu kopii powinny zapewnić, metadane obiektów blob, a zwłaszcza związane z szyfrowania metadanych są zachowywane. Jeśli skopiujesz blob bez metadanych zawartość obiektów blob nie być ponownie uwzględnianej podczas pobierania wyników. Aby uzyskać więcej informacji dotyczących metadane związane z szyfrowania zobacz [Magazyn Azure szyfrowania po stronie klienta](storage-client-side-encryption.md).

## <a name="faqs"></a>Często zadawane pytania

1. **Istnieją nadal dostępne konta miejsca do magazynowania?**

    Tak, istniejących kont przestrzeni dyskowej są nadal dostępne i pozostają bez zmian w ceny lub funkcji.  Ta osoba nie ma możliwość wybrania w warstwie miejsca do magazynowania i nie będą mieć możliwości obsługi w przyszłości.

2. **Dlaczego i kiedy należy rozpocząć korzystanie z kont magazyn obiektów Blob?**

    Konta magazyn obiektów blob są przeznaczone do przechowywania obiektów blob i zezwalają na wprowadzenie nowych funkcji zorientowane obiektów blob. Wystawienia faktury konta magazyn obiektów Blob są zalecany sposób przechowywania obiektów blob, jako przyszłych funkcje, takie jak hierarchiczne miejsca do magazynowania i obsługi zostaną wprowadzone na podstawie tego typu konta. Jest jednak nawet gdy chcesz przeprowadzić migrację w zależności od potrzeb biznesowych.

3. **Na koncie magazyn obiektów Blob można konwertować istniejące konto miejsca do magazynowania?**

    Wartość nie. Konto magazyn obiektów blob jest innego rodzaju konta miejsca do magazynowania i trzeba będzie utworzyć nowy i migrację danych zgodnie z opisem powyżej.

4. **W obu poziomów miejsca do magazynowania z tego samego konta można przechowywać obiekty?**

    Atrybut *"poziom dostępu"* , który wskazuje poziom miejsca do magazynowania jest ustawiona na poziomie konta i dotyczy wszystkich obiektów tego konta. Nie można ustawić atrybutu poziomu dostępu na poziomie obiektu.

5. **Czy mogę zmienić warstwie przestrzeni dyskowej Moje konto magazyn obiektów Blob?**

    Wartość Tak. Można zmienić warstwie miejsca do magazynowania, ustawiając atrybut *"poziom dostępu"* na rachunku miejsca do magazynowania. Zmienianie poziomu miejsca do magazynowania zostaną zastosowane do wszystkich obiektów przechowywanych w oknie konta. Zmienianie poziomu miejsca do magazynowania z ciepłej ostygnąć nie będzie powodowało wszelkich opłat podczas zmieniania z ochłodzić do hot będzie powodowało koszt GB dla wszystkich danych w oknie konta do czytania.

6. **Jak często można zmienić warstwie przestrzeni dyskowej Moje konto magazyn obiektów Blob**

    Gdy nie można Wymuszaj ograniczenia częstotliwości mogą być zmieniane warstwie przestrzeni dyskowej, należy pamiętać, że zmiana poziomu miejsca do magazynowania z ochłodzić do hot będzie powodowało znaczną opłaty. Zaleca się często się zmieniają warstwie miejsca do magazynowania.

7. **Będą BLOB w warstwie atrakcyjne miejsca do magazynowania zachowywać się inaczej niż w warstwie ciepłej miejsca do magazynowania?**

    BLOB w warstwie ciepłej miejsca do magazynowania mają samej opóźnienie jako obiektów blob w konta ogólnego przeznaczenia miejsca do magazynowania. BLOB w warstwie atrakcyjne przestrzeni dyskowej mają podobne opóźnienie (w milisekundach) jako obiektów blob w konta ogólnego przeznaczenia przestrzeni dyskowej.

    BLOB w warstwie atrakcyjne miejsca do magazynowania będą mieć nieco niższy dostępność poziom usług (SLA) niż blob przechowywanych w warstwie ciepłej miejsca do magazynowania. Aby uzyskać więcej informacji zobacz [Umowa dotycząca poziomu usług dla miejsca do magazynowania](https://azure.microsoft.com/support/legal/sla/storage).

8. **W konta magazyn obiektów Blob mogą zawierać blob strony i maszyn wirtualnych dysków?**

    Konta magazyn obiektów blob obsługuje tylko blok i dołączanie obiektów blob, a nie obiektów blob strony. Azure maszyn wirtualnych dysków kopii przez blob strony, a w wyniku kont magazyn obiektów Blob nie może służyć do przechowywania maszyn wirtualnych dysków. Jednak jest możliwe do przechowywania kopii zapasowych dysków maszyn wirtualnych jako blob bloku na koncie magazyn obiektów Blob.

9. **Muszę zmienić istniejące aplikacje używać kont magazyn obiektów Blob?**

    Konta magazyn obiektów blob są zgodne z kontami ogólnego przeznaczenia miejsca do magazynowania dla bloku interfejsu API 100% i dołączanie obiektów blob. Jak aplikacja jest przy użyciu zablokować obiektów blob lub dołączanie obiektów blob i jest używana wersja 2014-02-14 [Interfejsu API usługi REST usług miejsca do magazynowania](https://msdn.microsoft.com/library/azure/dd894041.aspx) lub większy, następnie aplikacja powinna działać od razu. Jeśli używasz starszej wersji protokołu, będzie konieczne zaktualizować aplikację, aby korzystać z nowej wersji tak, aby współpracuje z obu typów kont miejsca do magazynowania. Ogólnie zawsze zalecane korzystać z najnowszych wersji, niezależnie od tego, którego konta magazynu typu używamy.

10. **Czy będzie zmiany w środowisku użytkownika?**

    Konta magazyn obiektów blob są bardzo podobne do kont ogólnego przeznaczenia magazynowania do przechowywania blok i dołączanie obiektów blob i obsługa techniczna najważniejsze cechy Azure miejscem do magazynowania, łącznie z wysokiej wytrzymałości i dostępność, skalowalność, wydajność i bezpieczeństwo. Oprócz funkcji oraz ograniczenia specyficzne dla kont magazyn obiektów Blob i jego warstw miejsca do magazynowania, które zostały wymienionym powyżej, o innych możliwościach zmienia się.

## <a name="next-steps"></a>Następne kroki

### <a name="evaluate-blob-storage-accounts"></a>Szacowanie kont magazyn obiektów Blob

[Sprawdzanie dostępności rachunków magazyn obiektów Blob według regionów](https://azure.microsoft.com/regions/#services)

[Szacowanie zastosowania konta bieżącego miejsca do magazynowania, umożliwiając metryki magazynowania Azure](storage-enable-and-view-metrics.md)

[Sprawdzanie ceny magazyn obiektów Blob według regionów](https://azure.microsoft.com/pricing/details/storage/)

[Przesyłanie danych wyboru ceny](https://azure.microsoft.com/pricing/details/data-transfers/)

### <a name="start-using-blob-storage-accounts"></a>Rozpoczynanie korzystania z konta magazyn obiektów Blob

[Rozpoczynanie pracy z magazynem obiektów Blob platformy Azure](storage-dotnet-how-to-use-blobs.md)

[Przenoszenie danych do i z magazynu platformy Azure](storage-moving-data.md)

[Przesyłanie danych za pomocą narzędzia wiersza polecenia AzCopy](storage-use-azcopy.md)

[Przeglądanie i eksplorowanie konta miejsca do magazynowania](http://storageexplorer.com/)
