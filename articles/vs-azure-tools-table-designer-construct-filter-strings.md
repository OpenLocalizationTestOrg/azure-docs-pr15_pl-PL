<properties
   pageTitle="Budowa ciągów filtr dla projektanta tabel | Microsoft Azure"
   description="Budowa ciągów filtr dla projektanta tabel"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="storage"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="constructing-filter-strings-for-the-table-designer"></a>Budowa ciągów filtr dla projektanta tabel

## <a name="overview"></a>Omówienie

Aby filtrować dane w tabeli Azure wyświetlanych w programie Visual Studio **Projektanta tabel**, skonstruować ciąg filtru i wprowadzić w polu Filtr. Składnia ciągu filtr jest określona przez usługi danych WCF i jest podobna do klauzuli WHERE języka SQL, ale są wysyłane do usługi tabeli przy użyciu żądania HTTP. **Projektanta tabel** obsługuje, poprawne kodowanie dla Ciebie tak, aby filtrować według wartości odpowiednie właściwości, tylko potrzebne wprowadź nazwę właściwości, operator porównania, wartość kryterium i opcjonalnie wartość logiczna operator w polu Filtr. Nie zawiera opcji kwerendy $filter, jak w przypadku, jeśli zostały konstruowania kwerend w tabeli przy użyciu [Magazynu usługi REST API Reference](http://go.microsoft.com/fwlink/p/?LinkId=400447)adresu URL.

Usługi danych WCF są oparte na [Protokołu Open Data](http://go.microsoft.com/fwlink/p/?LinkId=214805) (OData). Aby uzyskać szczegółowych informacji na temat opcji kwerendy systemu filtr (**$filter**) zobacz [Specyfikacja konwencje URI OData](http://go.microsoft.com/fwlink/p/?LinkId=214806).

## <a name="comparison-operators"></a>Operatory porównania

Dla wszystkich typów właściwości są obsługiwane następujące operatory logiczne:

|Operator logiczny|Opis|Przykład ciąg filtru|
|---|---|---|
|EQ|Równa się|Miasto eq "Redmond"|
|BT|Większe niż|Cena BT 20|
|stronę|Większe niż lub równe|Cena stronę 10|
|lt|Mniejsze niż|Cena lt 20|
|le|Mniejsze lub równe|Cena le 100|
|nowe|Równa się|Miasto No "Londyn"|
|oraz|Oraz|Cena le 200 i cena BT 3.5|
|lub|Lub|Cena le 3.5 lub cena BT 200|
|nie|Nie|nie isAvailable|

Podczas tworzenia ciąg filtru, ważne są następujące reguły:

- Używanie operatorów logicznych do porównywania właściwości na wartość. Należy zauważyć, że nie jest możliwe do porównywania właściwości na wartość dynamiczne; po jednej stronie wyrażenia musi być stałą.

- Wszystkie części ciąg filtru jest uwzględniana wielkość liter.

- Stała wartość musi być tego samego typu danych jako właściwości w kolejności filtru zwraca prawidłowe wyniki. Aby uzyskać więcej informacji o typach obsługiwanych właściwości zobacz [Opis modelu danych z tabeli](http://go.microsoft.com/fwlink/p/?LinkId=400448).

## <a name="filtering-on-string-properties"></a>Filtrowanie właściwości ciągów

Po zakończeniu filtrowania na właściwości ciągów ujmij stałej w postaci ciągu w pojedynczy cudzysłów.

Następujące filtry przykład właściwości **PartitionKey** i **RowKey** ; dodatkowe właściwości wartością klucza może również dodany do ciąg filtru:

    PartitionKey eq 'Partition1' and RowKey eq '00001'

Mimo że nie jest wymagane może być częścią każdego wyrażenia filtru w nawiasach:

    (PartitionKey eq 'Partition1') and (RowKey eq '00001')

Należy zauważyć, że usługa tabeli nie obsługuje symbol wieloznaczny kwerendy, a nie są obsługiwane w Projektancie tabel albo. Można jednak wykonać prefiks pasujących przy użyciu operatorów porównania na odpowiednie prefiks. W poniższym przykładzie zwracany podmioty nazwisko właściwość rozpoczynający się od litery "A":

    LastName ge 'A' and LastName lt 'B'

## <a name="filtering-on-numeric-properties"></a>Filtrowanie właściwości liczbowych

Aby filtrować według liczby całkowitej lub liczba zmiennoprzecinkowa, określić liczbę bez cudzysłowów.

W tym przykładzie zwraca wszystkich jednostek właściwość wieku, której wartość jest większa niż 30:

    Age gt 30

W tym przykładzie zwraca wszystkich obiektów z właściwością AmountDue, których wartość jest mniejsza niż lub równa 100.25:

    AmountDue le 100.25

## <a name="filtering-on-boolean-properties"></a>Filtrowanie operatory logiczne

Aby filtrować wartość logiczną, określ **wartość PRAWDA** lub **FAŁSZ,** bez cudzysłowów.

W poniższym przykładzie zwracany wszystkie obiekty, których właściwość IsActive jest ustawiona na **PRAWDA**:

    IsActive eq true

Można także napisać wyrażenie filtru bez operatorów logicznych. W poniższym przykładzie usłudze tabeli również zwróci wszystkie obiekty których IsActive jest **prawdziwa**:

    IsActive

Aby zwrócić wszystkich obiektów, gdzie IsActive jest FAŁSZ, możesz użyć nie operatora:

    not IsActive

## <a name="filtering-on-datetime-properties"></a>Filtrowanie właściwości daty i godziny

Aby filtrować według wartości daty/godziny, określ słowo kluczowe **daty i godziny** , a po nim Stała data/godzina w pojedynczy cudzysłów. Stała data/godzina musi być w formacie UTC Scalonej, zgodnie z opisem w [Formatowania wartości daty/godziny](http://go.microsoft.com/fwlink/p/?LinkId=400449).

W poniższym przykładzie zwracany jednostki, gdy właściwość CustomerSince jest równe 10 lipca 2008:

    CustomerSince eq datetime'2008-07-10T00:00:00Z'
