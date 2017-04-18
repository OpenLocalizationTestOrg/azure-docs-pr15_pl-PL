<properties
   pageTitle="Zastosowania platformy Microsoft Azure i Włącz interfejsy API RateCard Cloudyn w celu zapewnienia ITFM dla klientów | Microsoft Azure"
   description="Udostępnia unikatową perspektywę od firmy Microsoft Azure rozliczenia partnera Cloudyn, na swoimi doświadczeniami integrowanie API rozliczenia Azure produktu.  To jest szczególnie przydatne dla Azure i Cloudyn klientów, którzy chcą przy użyciu wypróbowaniu Cloudyn usługi Azure."
   services=""
   documentationCenter=""
   authors="BryanLa"
   manager="mbaldwin"
   editor=""
   tags="billing"/>

<tags
   ms.service="billing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="billing"
   ms.date="08/16/2016"
   ms.author="mobandyo;bryanla"/>

# <a name="microsoft-azure-usage-and-ratecard-apis-enable-cloudyn-to-provide-itfm-for-customers"></a>Zastosowania platformy Microsoft Azure i Włącz interfejsy API RateCard Cloudyn w celu zapewnienia ITFM dla klientów

Cloudyn, partnera rozwoju firmy Microsoft i wiodącym dostawcą funkcji zarządzania w chmurze, została wybrana prywatne Podgląd nowego użycie zasobu usługi Microsoft Azure oraz interfejsy API RateCard.  Użycie interfejsu API zapewnia dostęp do danych szacowane zużycie Azure dla subskrypcji. Interfejs API RateCard informacje pełną cennik wszystkich usług Azure, w przypadku klientów — z przedsiębiorstwa umowy EA. Zintegrowany ze sobą, te interfejsy API stanowić podstawę pełne informacje o wprowadzenie informacji do narzędzi IT finansowy zarządzania (ITFM), takich jak oferowane przez Cloudyn.

## <a name="introduction"></a>Wprowadzenie

Tak zwany "iloczyn" danych z interfejsu API zastosowania za pomocą danych z interfejsu API RateCard (cena zastosowania [jednostki] [$unit] = szczegółowy opis sposobu użycia i koszt) tworzy najbardziej szczegółowego, dokładne i niezawodne informacje rozliczeniowe dostępne dla Azure dzisiaj.

![Omówienie ITFM][1]

Używające te interfejsy API zawiera kluczowe informacje dotyczące użycia i koszty zezwalania Cloudyn analizowanie klientach w sposób prosty i programowych i wykonywać różne zadania ITFM klientom klientów.

## <a name="integrating-cloudyn-with-the-ratecard-and-usage-apis"></a>Integrowanie Cloudyn z RateCard oraz interfejsy API zastosowania
Interfejs API RateCard wymaga kilku parametrów wejściowych — takie jak informacje o regionu, waluty i ustawienia regionalne —, ale jest jednym z najważniejszych OfferDurableID, która określa, czy typ Azure oferuje klienta korzysta (repartycyjny, plany starsze zobowiązania 6 i 12-miesięczny, MSDN ofert, MPN ofert, promocyjnych ofert i inne). OfferDurableID można znaleźć w [zastosowania Azure i rozliczeń portalu](https://account.windowsazure.com/Subscriptions), w obszarze "Oferuje identyfikator" dla danej subskrypcji.

Po rejestracji usługi [Cloudyn dla Azure](https://www.cloudyn.com/microsoft-azure/) klientów można dodać ich OfferDurableID kod, który umożliwia Cloudyn uwzględniał ich informacje cennik za pośrednictwem interfejsu API RateCard.  Informacje na temat różnych typów oferty można znaleźć jedną stronę [Szczegółów oferty Azure firmy Microsoft](https://azure.microsoft.com/support/legal/offer-details/) .

![Przegląd aparatu Cloudyn ITFM][2]

Zastosowanie Cloudyn zastosowania i interfejsów API RateCard, oprócz Azure API wydajności, aby utworzyć kolejne warstwy wizualizacji, analizy, alertów, raportowania, koszt zarządzania i praktyczne zalecenia udostępnieniem Azure klientów narzędzia ITFM chmury zaufanego przedsiębiorstwa.

## <a name="cloudyn-itfm-use-cases-enabled-by-usage-and-ratecard-api-integration"></a>Korzystając z integracji użycia i interfejsu API RateCard przypadków użycia Cloudyn ITFM
Typowe ITFM Cloudyn przypadkach włączane przez użycie i interfejsów API RateCard zawiera:

+ Umożliwia **Analizę kosztów** — w chmurze koszty podzielone dowolnych macierzystych wymiarów identyfikacyjnym (dostawcy usługi, konto, region itp.). Zastosowania Azure oraz interfejsy API RateCard ułatwić to proste zadania, zapewniając najbardziej szczegółowego podziału pracy oraz dane na konto, które następnie zgrupowane i przefiltrowane według Cloudyn i prezentowane użytkownikowi w postaci grafiki forma tabelaryczna kosztów.

![Wykres kołowy analizy kosztów][3]

+ **Koszt alokacji 360** - umożliwia Finanse i administratorom odkrywanie podziału koszt rzeczywisty, sterowniki i trendów ich wdrożenia chmury. Umożliwia dalsze menedżerów łatwo skojarzyć koszty wdrażania jednostek biznesowych, działy regionów i uzyskać więcej informacji, dostarczając bezprecedensowej wniosków w chmurze kosztów i ułatwienia obciążeniach zwrotnych przedsiębiorstwa i showbacks. Użycie Azure oraz interfejsy API RateCard służyć jako danych wejściowych dla aparatu alokacji koszt Cloudyn osoby, które uzupełnia za pośrednictwem interfejsów API przez definiowanie metod i reguł biznesowych do alokacji zasobów bez znaczników lub untaggable.

![Alokacja kosztu 360 wykresu][4]

+ **Zmiany rozmiaru cost-Effective** - zawiera zmiany rozmiaru na prawej zalecenia dotyczące dzieląc maszyn wirtualnych, co powoduje zmniejszenie wydatków klienta na komputerach zbyt duży lub nadmiernie ustanawianie. Robi to sprawdzając maszyn wirtualnych Procesora i pamięci RAM metryki (za pośrednictwem interfejsu API wydajności), godzinach wykonywania (za pośrednictwem interfejsu API zastosowania) i koszt (za pośrednictwem interfejsu API RateCard). Cloudyn zawiera zalecenia dotyczące zmiany rozmiaru na prawej wyczerpaniu zasobów Procesora i pamięci RAM (wydajność) i oblicza szacowaną oszczędności iloczyn różnica cen (RateCard) między maszyny wirtualne rzeczywisty czas — wykorzystania (użycie) dzieląc komputera.

![Koszt skutecznych zmiany rozmiaru][5]

+ **Zalecenia dotyczące przenoszenia w chmurze** — udostępnia finansowe porady dotyczące chmury przenoszenia. Sprawdza, czy koszty bieżącego użytkownika chmurze zasobów, które są wdrożone na chmurze głównych dostawców i jej porównanie kosztów równoważne wdrażania Azure. Następnie znajdują się w nim będącego na zasób, finansowy oparte na przenoszenia zalecenia Azure. Po przeprowadzeniu oceny równoważne wdrożenie jest wymagane Azure (w oparciu o wydajności preferencje metryki i użytkownika), Cloudyn używa interfejsu API RateCard w celu Szacowanie kosztów wdrażania równoważne Azure.

+ **Raportów dotyczących wydajności** — włączone, wydajności i Azure API, tych raportów zawierają tablicy funkcji z wykorzystania Procesora i pamięci RAM zalecenia optymalizacji. Oto przykład raportu wykorzystania wystąpienie, prezentowanie podziału wystąpienia przez użycie Procesora średnia.

![Raportów dotyczących wydajności][6]

+ **Menedżer kategorii** — zaawansowana funkcja Cloudyn, aby przesunąć kolejności unorganized chmurze zasobów. Umożliwia użytkownikom swobody utworzyć własne unikatowe kategorie (znaczniki) skutecznych pomiaru i raportowania, która jest zgodna z firmy. Ponadto użytkownicy mogą łatwo regulowania i kategoryzowanie niespójne znakowania (to znaczy literówek i innych niezgodności) i automatycznie Wykryj znaczników zasoby dla przyznawania dokładne koszt.

![Menedżer kategorii][7]

## <a name="video"></a>Klip wideo

Oto krótki klip wideo, który pokazuje, jak klientem Azure mogą używać Cloudyn Azure i API rozliczenia Azure uzyskanie wniosków z dane o jego zużycie Azure.

> [AZURE.VIDEO cloudyn-provides-cloud-itfm-tools-via-microsoft-azure-apis]


## <a name="next-steps"></a>Następne kroki

+ Uruchom w bezpłatnej wersji próbnej [Cloudyn dla Azure](https://www.cloudyn.com/microsoft-azure/) , aby zobaczyć, jak można uzyskać kosztu przezroczystości przy użyciu raportów niestandardowych i analizy dla usługi Microsoft Azure w chmurze wdrożenia.
+ Zawiera omówienie dotyczące użycia zasobów Azure oraz interfejsy API RateCard, zobacz [Uzyskiwanie wniosków do usługi Microsoft Azure zużycie zasobów](billing-usage-rate-card-overview.md) .
+ Zapoznaj się z [Azure rozliczenia pozostałych API Reference](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) więcej informacji na temat obu interfejsów API, które są częścią zestawu interfejsów API udostępnianych przez Menedżera zasobów Azure.
+ Jeśli chcesz zagłębić się w prawo w kodzie przykładowych, zapoznaj się z naszą Microsoft Azure rozliczenia interfejsu API przykłady kodu na [Przykłady kodu Azure](https://azure.microsoft.com/documentation/samples/?term=billing).

## <a name="learn-more"></a>Dowiedz się więcej
+ Aby dowiedzieć się więcej na temat usługi Microsoft Azure Enterprise umowy (EA) ofert, odwiedź stronę [Licencjonowanie Azure w przedsiębiorstwie] (https://azure.microsoft.com/pricing/enterprise-agreement/)
+ Zobacz artykuł [Omówienie Menedżera zasobów Azure](azure-resource-manager/resource-group-overview.md) , aby dowiedzieć się więcej o Azure Menedżera zasobów.
+ Dodatkowe informacje na temat pakietu narzędzia niezbędne do pomoc w uzyskaniu zrozumienia chmury wydatków, można znaleźć w artykule Gartner [Rynku — przewodnik dotyczący narzędzia IT finansowy zarządzania (ITFM)](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb).

<!--Image references-->
[1]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-ITFM-Overview.png
[2]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-ITFM-Engine-Overview.png
[3]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Analysis-Pie-Chart.png
[4]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Allocation-360-Chart.png
[5]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Effective-Sizing.png
[6]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Performance-Reports.png
[7]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Category-Manager.png
