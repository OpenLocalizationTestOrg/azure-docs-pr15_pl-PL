<properties
   pageTitle="Opis rachunku | Microsoft Azure"
   description="Dowiedz się, jak do odczytania i zrozumienia użycia i rachunku dla subskrypcji Azure"
   services=""
   documentationCenter=""
   authors="genlin"
   manager="stevenpo"
   editor=""
   tags="billing"/>

<tags
   ms.service="billing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/31/2016"
   ms.author="erihur;genli"/>


# <a name="understand-your-bill-for-microsoft-azure"></a>Opis rachunku za usługę Microsoft Azure

> [AZURE.NOTE] Jeśli potrzebujesz dodatkowej pomocy w dowolnym momencie, w tym artykule, szybko [kontakt z pomocą techniczną](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , aby uzyskać problemu rozwiązać, sprawdź.

Opłaty za subskrypcje Microsoft Azure zależy od planu taryfowego. Niektóre plany stawka, takich jak subskrybenci Visual Studio przedsiębiorstwa (MPN) obejmują środków miesięcznych, których można używać na wszystkie inne usługi Azure w zależności od potrzeb.

Należy zauważyć, że w górę do 24 godzin pracy ukryty z poprzedniego okresu rozliczeniowego mogą być zgłaszane do bieżącego okresu rozliczeniowego.

Aby uzyskać więcej informacji na temat zużycie i plany taryfowe zawiera [Opcje zakupu pakietu Microsoft Azure strony](https://azure.microsoft.com/pricing/purchase-options/).

<!-- The below links cover a complete list of all Microsoft Azure services.

<!-- - [Service Details list (csv1)](https://azurepricing.blob.core.windows.net/supplemental/MOSPServices_csv1.xlsx)
<!-- - [Service Details list (csv2)](https://azurepricing.blob.core.windows.net/supplemental/MOSPServices_csv2.xlsx)

<!-- *NOTE: The **csv1** link refers to the column header names for csv version 1 and **csv2** link refers to the new column header names for csv version 2.  These files are updated monthly.*-->

### <a name="view-or-download-a-bill-for-microsoft-azure"></a>Wyświetlanie i pobieranie BOM dla Microsoft Azure:

1. Zaloguj się do [Centrum konta](https://account.windowsazure.com/subscriptions) za pomocą Account Microsoft lub identyfikatora organizacji.

2. Kliknij subskrypcję, w której chcesz wyświetlić szczegółowe informacje oraz użycia.

3. Kliknij pozycję **historię rachunków**

    ![Podsumowanie — -1 historię rachunków](./media/billing-understand-your-bill/ContentViewaBillforMA1.png)

4. W sekcji **Historii rozliczeń** listy z instrukcji dla poprzednich okresów rozliczeniowy oraz bieżącego okresu unbilled. Instrukcja dla bieżącego okresu jest szacowana opłat od czasu, wygenerowania szacowania. Te informacje tylko aktualizowanych codziennie i nie mogą zawierać wszystkie zastosowania poniesione do daty. Rachunku miesięczny mogą różnić się od tej oszacowanie.  

    ![Historii rozliczeń podsumowanie 2](./media/billing-understand-your-bill/ContentViewaBillforMA2.png)

5. Kliknij przycisk **Widok bieżącej instrukcji** Aby wyświetlić oszacowanie opłat od czasu, wygenerowania szacowaną. Te informacje tylko aktualizowanych codziennie i nie mogą zawierać wszystkie zastosowania poniesione do daty. Rachunku miesięczny mogą różnić się od tej oszacowanie.

    ![Historii rozliczeń podsumowanie 3](./media/billing-understand-your-bill/ContentViewaBillforMA3.png)

    ![Karta Podsumowanie Historia 4](./media/billing-understand-your-bill/ContentViewaBillforMA4.png)

6. Kliknij przycisk **Pobierz fakturę** wyświetlanie kopii rachunku poprzedniego.

    ![Karta Podsumowanie Historia 5](./media/billing-understand-your-bill/ContentViewaBillforMA5.png)


> [AZURE.NOTE] Opłaty wymienione na rozliczenia instrukcje dla klientów międzynarodowych służą do szacowania tylko wtedy, gdy banków mają różne koszty dla kursów wymiany.

Poniżej przedstawiono niektóre przykładowe instrukcje dla dwóch różnych ofert dostępne w programie Microsoft Azure.

 Typ oferty | Opis | Plik do pobrania |
 :--------- |:-------- | :-------|
Repartycyjny | Płatności w zaległości miesięczny | [Przykładowy plik](https://azurepricing.blob.core.windows.net/sampleinvoices/Microsoft_Azure_ccinvoice_Sample.pdf)
Oferty do zatwierdzenia | Poświęcają wydane z Twojej przedpłaconej zobowiązań | [Przykładowy plik](https://azurepricing.blob.core.windows.net/sampleinvoices/Microsoft_Azure_invoice_Sample.pdf)

## <a name="account-information"></a>Informacje o koncie

Sekcja Informacje o koncie identyfikuje stosowne informacje dotyczące użycia i w profilu.

![Nagłówek](./media/billing-understand-your-bill/Header.png)

| Terminów                | Opis                                                                                         |
|---------------------|-----------------------------------------------------------------------------------------------------|
| Nr faktury         | Identyfikator unikatowe faktury na potrzeby śledzenia                                                   |
| Cykl rozliczeniowy       | Przedział czasu, w którym zastosowania miało miejsce                                                       |
| Data faktury        | Data wygenerowania faktury                                                                 |
| Metody płatności      | Typ płatności używane na koncie (faktury lub karty kredytowej)                                   |
| Rachunek do             | Adres płatności Microsoft Azure                                                                    |
| Oferta subskrypcji  | Typ oferta subskrypcji, który został zakupiony (repartycyjny BizSpark Plus, przebieg Azure, itp.) |
| Wiadomości e-mail właściciela konta | Adres e-mail konta, konto Microsoft Azure zarejestrowany w obszarze                      |

## <a name="understand-the-invoice-summary"></a>Opis Podsumowanie faktury

W sekcji **Podsumowanie faktury** BOM zawiera podsumowanie transakcji od czasu ostatniego rachunku i bieżące opłaty za zużycie.

![Podsumowanie faktury](./media/billing-understand-your-bill/InvoiceSummary.png)

Poprzednie saldo, płatności i zaległe saldo sekcja rachunku podsumowano transakcje od czasu ostatniego rachunku.

| Terminów                                              | Opis                                                                              |
|---------------------------------------------------|------------------------------------------------------------------------------------------|
| Poprzednie saldo                                  | Całkowita kwota należności na fakturze ostatniej                                                 |
| Płatności                                          | Suma płatności zastosowane do ostatniej rachunku                                                 |
| Zaległe saldo (od poprzedniego cyklu rozliczeniowego) | Wszelkie zmiany rachunku (środków lub sald) stosowana do swojego konta od ostatniej rachunku  |

## <a name="understand-the-current-charges"></a>Opis opłat bieżących
Sekcja opłat bieżących BOM zawiera szczegółowe informacje o opłaty miesięczne. Łącza są zorganizowane na podane niżej podsekcje.

| Terminów          | Opis                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Opłaty za zużycie | Opłaty za zużycie to suma opłat miesięcznych w subskrypcji. Konta w zaległości użycia ubiegłym miesiącu.                                                                                                                                                                                                                                                                                                                                       |
| Rabatów     | Zastosowane do rachunku bieżącego rabatów usługi będą odzwierciedlane w tej pozycji.                                                                                                                                                                                                                                                                                                                                              |
| Dostosowania   | Różne korekty są różne środków lub zaległych opłat nakładanych na rachunku bieżącego. Na przykład jeśli masz Visual Studio przedsiębiorstwa z ofertą w witrynie MSDN, zobacz miesięczny zwrot w tej pozycji. Jeśli możesz anulować subskrypcję, czy Zobacz opłaty za miesięczny zastosowania powyżej miesięczny zwrot zawarte w Twoją ofertę od początku bieżącego okresu rozliczeniowego do daty rozpoczęcia anulowania subskrypcji.|

## <a name="footer-information"></a>Informacje stopki
![stopki](./media/billing-understand-your-bill/footerinformation.png)

## <a name="understand-the-additional-information"></a>Opis dodatkowych informacji
Na stronie informacje dodatkowe umożliwia odwołania do innych zasobów, aby poznać faktury i łącza do wyświetlania do użycia i innych informacji do rachunku.

![dodatkowe informacje](./media/billing-understand-your-bill/AdditionalInformation.png)

### <a name="detailed-usage"></a>Szczegółowy opis sposobu użycia
Łącza w polu w obszarze **Szczegółowy opis sposobu użycia** kieruje Centrum konta, w której można wyświetlić szczegółowe użycie dla tej subskrypcji.  Teraz są dwie wersje dostępnych do pobrania: **CSV w wersji 1** zawiera pola stare konwencji nazewnictwa i zastosowania i **CSV w wersji 2** zawiera przyjaznej nazwy klienta dla każdej kategorii oraz dodatkowe pola, które ułatwią zrozumienie, co możesz usługi jest używana w programie Microsoft Azure. Należy zauważyć, że w wersji CSV 1, że nie istnieją żadne szczegóły Azure Menedżera zasobów. Azure informacje Menedżera zasobów można znaleźć w pliku CSV w wersji 2.

### <a name="additional-information-and-useful-resources"></a>Dodatkowe informacje i przydatne zasoby
W tej sekcji znajdują się łącza do prostego pytania dotyczące rozmiarów wystąpienie obliczeń, opłaty bazy danych SQL i przydatne łącza ułatwiające dalszych odpowiedzi na pytania.

| Terminów                 | Opis                                                                                                                            |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| Sprzedaży              | To jest wstępnie z adresem profilu konta                                                                           |
| Instrukcje dotyczące płatności | Ta sekcja jest instrukcje dotyczące płatności miejsca do wysyłania testy, drutu przeniesienia lub następny dzień testy w przypadku metody płatności faktury |

## <a name="understand-detailed-usage-charges"></a>Opłaty za zużycie szczegółowy opis

W ramach naszych stale ułatwiające klientom łatwe zarządzanie ich używaniem Azure możemy zostały rozszerzonego zawierającego zastosowania pobieranie raportów użycia usług Azure i koszty.  Łącze pobierania zawiera dwie wersje pliku zastosowania:

- **W wersji 1** w istniejącym formacie

- **W wersji 2** zawiera dodatkowe informacje i aktualizacja nazw kolumn w sekcji dzienny zastosowania.  

Opłaty za zużycie to całkowity **Miesięczny** opłat subskrypcji mniej kredytowe ani rabat. Konta w zaległości użycia ubiegłym miesiącu.  W górnej części pliku wyświetla szczegółów dotyczących usług, które są konta dla cyklu rozliczeniowego w poprzednim miesiącu.  Powyższej tabeli zawiera listę nazw kolumn dla każdego z plików CSV wersji.

Wersja 1 |  W wersji 2  |  Opis|
:---------------| :---------------- | --------|
Okres rozliczenia | Okres rozliczenia | Okres rozliczeniowy, gdy została wykorzystana zasobu.
Nazwa | Miernik kategorii | Służy do identyfikowania usługi najwyższego poziomu, do której należy ten zastosowania.
Typ | Miernik podkategorii | Usługa Azure mogą być dodatkowo określone przez wpisywanie tekstu w tej kolumny, które mogą mieć wpływ na szybkość.
Zasób | Nazwa licznika | Służy do identyfikowania jednostki miary dla zasobu są używane.
Region | Miernik regionu | Określa lokalizację centrum danych dla określonych usług, które kosztują na podstawie lokalizacji centrum danych.
JEDNOSTKA SKU | JEDNOSTKA SKU | Służy do identyfikowania systemowy Unikatowy identyfikator dla każdego zasobu Azure.
Jednostki | Jednostki | Służy do identyfikowania jednostki, która usługa jest naliczany w. Jeśli na przykład GB, godziny, 10,000s.
Zużyte | Ilości zużyte | Zawiera ilość zasobu, który został zużyty w okresie rozliczeniowym.
Dostępny | Uwzględniane ilości | Zawiera ilość zasobu, do którego jest dołączany bezpłatnie w swojej bieżącego okresu rozliczeniowego.
Fakturowanego | Ilość nadmiarowych | Jeśli kwota Zużyto jest większa niż wartość dołączone, w tej kolumnie są wyświetlane różnica. Konta na tę kwotę. W przypadku oferty repartycyjny żadna kwota dołączone do oferty suma ta będzie taki sam, jak ilość Zużyto.
W ramach zatwierdzenia | W ramach zatwierdzenia | Zawiera opłaty zasobów, które są zmniejszany z kwota zobowiązania skojarzone z Twoją ofertę 6 lub 12 miesięcy. Opłaty za zasobów są zmniejszany od ilości zobowiązania w kolejności chronologicznej.
Waluta | Waluta | Służy do identyfikowania waluty odzwierciedlane w swojej bieżącego okresu rozliczeniowego.
Nadmiar | Nadmiar | Zawiera opłaty zasobów, które wykraczają poza kwota zobowiązania skojarzone z Twoją ofertę 6 lub 12 miesięcy.
Stopa zatwierdzenia | Stopa zatwierdzenia | Zawiera stawkę zobowiązania według kwota całkowita zobowiązania skojarzone z Twoją ofertę 6 lub 12 miesięcy.
Stawka | Stawka | Stopa Wyświetla stopa jest naliczany na jednostkę rozliczaniu.
Wartość | Wartość | Wyświetla wynik mnożenia rozliczaniu kolumny w kolumnie stawka. Jeśli kwota zużyto nie przekracza wielkość zawarte, nie będą bezpłatne w tej kolumnie.

## <a name="analyze-daily-usage-data"></a>Analizowanie danych dotyczących użycia dzienny
W zależności od usługi zastosowania mogą być tysiące wierszy danych dotyczących dziennych zastosowania. Jeśli chcesz przeanalizować dane, kliknij pozycję **Pobierz użycie** i wybrać wersję zmiennych plik rozdzielany przecinkami (CSV), aby wyświetlić dzienny danych użycia dla odpowiedniego okresu rozliczeniowego.  Dla odwołania możesz pobrać przykładowy plik CSV dla każdej wersji poniżej.

 Nazwa | Plik do pobrania |
 :----------:| :-------: |
  Szczegółowy opis sposobu użycia CSV w wersji 1|  [Przykładowy plik](https://azurepricing.blob.core.windows.net/supplemental/MOSPServices_csv1.xlsx)
  Szczegółowy opis sposobu użycia CSV w wersji 2 | [Przykładowy plik](https://azurepricing.blob.core.windows.net/supplemental/MOSPServices_csv2.xlsx)



![csv2screenshot](./media/billing-understand-your-bill/csv2screenshot.png)



W pliku CSV elementy dzieli się do wyświetlania listy ilość każdego zasobu została wykorzystana w bieżący okres rozliczeniowy.

![Migawka CSV](./media/billing-understand-your-bill/csvsnapshotportal.png)

Następujące kolumny zawierają szczegółowe informacje, które mają wpływ na stawki na początku okresu rozliczeniowego:

Wersja 1 |   W wersji 2   |  Opis |
:---------------| :----------------| -----|
Data zastosowania | Data zastosowania | Data, kiedy zasobu został emisję.
Nazwa | Miernik kategorii | Służy do identyfikowania usługi najwyższego poziomu, do której należy ten zastosowania.
Identyfikator GUID zasobu | Miernik identyfikator | Identyfikator rozliczonym miernik.  To jest używany jako identyfikator umożliwia ceny zastosowania rozliczeń.
Typ | Miernik podkategorii | Usługa Azure mogą być dodatkowo określone przez wpisywanie tekstu w tej kolumny, które mogą mieć wpływ na szybkość.
Zasób | Nazwa licznika | Służy do identyfikowania jednostki miary dla zasobu są używane.
Region | Miernik regionu | Określa lokalizację centrum danych dla określonych usług, które kosztują na podstawie lokalizacji centrum danych.
Jednostki | Jednostki | Służy do identyfikowania jednostki, która usługa jest naliczany w. Jeśli na przykład GB, godziny, 10,000s.
Zużyte | Ilości zużyte | Zawiera ilość zasobu, który został zużyty dla tego dnia.
Obszar podrzędny | Lokalizacja zasobu | Służy do identyfikowania centrum danych, w którym jest uruchomiony zasób.
Usługa | Zużyta usługa | W tej kolumnie jest używany do śledzenia usługa poszczególnych platformy Azure, która nie może być wyraźnie określone w kolumnie Nazwa. Dotyczy tylko tę usługę kolumna wskazuje, jakie szczególne obsługi wykorzystania.
N/D! | Grupa zasobów | _**Dodanie nowej kolumny.**_ Grupa zasobów, w którym jest uruchomiony zasób wdrożonym w. Zapoznaj się z [Omówienie Menedżera zasobów Azure](../azure-resource-manager/resource-group-overview.md)
Składnik | Identyfikator wystąpienia | Identyfikator zasobu uruchomionego. Identyfikator zawiera nazwę zadanej dla zasobu, gdy został utworzony.
N/D! | Znaczniki | _**Dodanie nowej kolumny.**_ Nowych typów zasobów w Azure umożliwiają znacznika zasobów. Odwołują się do [organizowania zasobów Azure znacznikami](http://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/)
Informacje dodatkowe | Informacje dodatkowe | Dodatkowe metadane związane z usługą.
Informacji dotyczących usługi 1 | Informacji dotyczących usługi 1 | Ta kolumna zawiera nazwę projektu, należący do usługi od Twojej subskrypcji.
Informacji dotyczących usługi 2 | Informacji dotyczących usługi 2 | Jest to starsze pole, które znajdują się opcjonalne metadanych specyficzne dla usługi.

Oprócz niektórych nowych pól i zmiany nazwy csv w wersji 2, będzie można ustandaryzowane formatowania danych w poniżej pola:

- **Identyfikator wystąpienia**: reprezentuje pole Identyfikator wystąpienia identyfikator usługi obsługi administracyjnej określony przez użytkownika. Obecnie istnieją dwa formaty, w których jest reprezentowany identyfikator wystąpienia: to nazwa zasobu, czy w pełni kwalifikowana identyfikator zasobu. Usługi Microsoft Azure jesteś w trakcie przejmowania reprezentować identyfikator wystąpienia w standardowym formacie identyfikator zasobu w pełni kwalifikowana _**(/subscriptions/<subscription id>/resourcegroups/<resourcegroupname>/providers/<providername>/<resourcename>)**_. Jak usługi przejścia na nowy format pojawi się pole danych Identyfikator wystąpienia zmienić tylko nazwę zasobu na identyfikator zasobu. Identyfikator zasobu jest format używany przez [Interfejs API Menedżera zasobów Azure](https://msdn.microsoft.com/library/azure/dn790567.aspx) do identyfikowania zasobów w subskrypcji.

![Identyfikator wystąpienia](./media/billing-understand-your-bill/instanceid.png)

- **Informacje dodatkowe**: informacje dodatkowe kolumny w pliku CSV zastosowania określa specyficzne dla usługi metadanych. Na przykład typ obrazu dla maszyn wirtualnych. Obecnie usługi emituje specyficzne dla usługi metadanych w wielu kolumnach: dodatkowe informacje, Info1 usługi i Service informacje 2. Usługi Microsoft Azure będzie normalizujących, wysyłających specyficzne dla usługi metadanych w tej kolumnie dodatkowe informacje.  Zobacz poniżej migawki standardowym formacie:

![additionalinfo_csv2](./media/billing-understand-your-bill/AdditionaInfo_csv2.png)

- **Znaczniki**: Ta kolumna zawierała użytkownika określonego znaczniki zasobów. Znaczniki można grupować rekordy rozliczeń. Na przykład znaczników umożliwia dzielenie kosztów przez dział przy użyciu usługi. Dowiedz się więcej o [korzystaniu z znaczników w celu organizowania Azure zasobów](../resource-group-using-tags.md). Usług, które obsługują emisji znaczniki są:  

    - Maszyn wirtualnych

    - Miejsca do magazynowania i

    - Usługi sieci obsługi administracyjnej za pomocą [Interfejsu API Menedżera zasobów Azure](https://msdn.microsoft.com/library/azure/dn790567.aspx)

![znaczniki](./media/billing-understand-your-bill/tags.png)


## <a name="next-steps"></a>Następne kroki

- [Konfigurowanie alertów rozliczenia](../billing-set-up-alerts.md)

- [Zarządzanie metodami płatności](../billing-how-to-change-credit-card.md)

- [Opis opłaty Azure Marketplace](../billing-understand-your-azure-marketplace-charges.md)

- [Często zadawane pytania dotyczące Azure rozliczeń i subskrypcji](../billing-subscription-faq.md)

> [AZURE.NOTE] Jeśli nadal masz dalszych pytania, szybko [kontakt z pomocą techniczną](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , aby uzyskać problemu rozwiązać, sprawdź.

<!--
OLD MSDN Articles
- [What do I do if my Azure subscription become disabled?](https://msdn.microsoft.com/library/azure/dn736049.aspx)
- [Edit payment information for an existing credit card](https://msdn.microsoft.com/library/azure/dn736053.aspx)
- [Add a new credit card to use as a payment method](https://msdn.microsoft.com/library/azure/dn736057.aspx)
- [Change the credit card on your Microsoft Azure account](https://msdn.microsoft.com/library/azure/dn736050.aspx)
- [Manage your payment method](https://msdn.microsoft.com/library/azure/dn736054.aspx)
-->



<!--Image references-->
