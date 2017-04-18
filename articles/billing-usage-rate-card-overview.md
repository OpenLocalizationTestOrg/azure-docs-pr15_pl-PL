<properties
   pageTitle="Uzyskanie wniosków do usługi Microsoft Azure zużycie zasobów | Microsoft Azure"
   description="Zawiera omówienie zastosowania rozliczenia Azure i interfejsów API RateCard, które służą do tworzenia spostrzeżeń zużycie zasobów Azure i trendów."
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

# <a name="gain-insights-into-your-microsoft-azure-resource-consumption"></a>Uzyskanie wniosków do usługi Microsoft Azure zużycie zasobów

Klienci i partnerzy muszą dokładnie przewidywanie i zarządzać nimi kosztów Azure.  Jak przenieść ich z kapitałowych z modelem oferowane, muszą również możliwość wykonaj showback a obciążenia zwrotnego analizy, a także podać wierności tryb w oszacowanie i rozliczeń, zwłaszcza w przypadku wdrożeń dużych chmury.

Stopa karty API i użycie zasobu Azure omówione w ten adres artykuł tych wymagań, włączając nowe wnioski do swojego zużycie zasobów Azure.  

## <a name="introducing-the-azure-resource-usage-and-ratecard-apis"></a>Wprowadzenie do użycia zasobów Azure oraz interfejsy API RateCard

Użycie zasobu Azure oraz interfejsy API RateCard są wykonywane jako dostawcę zasobu w ramach rodziny interfejsów API udostępniane przez Menedżera zasobów Azure.  

### <a name="azure-resource-usage-api-preview"></a>Użycie zasobu Azure interfejsu API (wersja Preview)
Klienci i partnerzy umożliwia API użycia zasobów Azure otrzymują dane szacowane zużycie Azure. Funkcje:

- **Kontrola dostępu oparta na rolach azure** — klienci i partnerzy można skonfigurować jej zasady dostępu na [Azure portal](https://portal.azure.com) lub za pośrednictwem [polecenia cmdlet programu PowerShell Azure](powershell-install-configure.md) , aby określić, którzy użytkownicy lub aplikacji może uzyskać dostęp do danych dotyczących użycia tej subskrypcji. Osoby dzwoniące, należy użyć standardowego tokeny usługi Azure Active Directory uwierzytelniania. Wywołujący również musi zostać dodana do roli czytnika, właściciel albo trybu współautora, aby uzyskać dostęp do danych dotyczących użycia dla danej subskrypcji Azure.

- **Co godzina lub agregacji dzienny** - osób dzwoniących można określić, czy mają swoje dane Azure zastosowania w godzinowe przedziałów lub dzienny pakiety. Wartość domyślna to dzienny.

- **Metadanych wystąpienie podanych (obejmuje znaczniki zasobów)** — poziom wystąpienia szczegóły, takie jak identyfikator uri zasobu w pełni kwalifikowana (/subscriptions/ {identyfikator subskrypcji}-...), wraz z zasobu znaczników grup zasobów i informacje zostaną udostępnione w odpowiedzi. To pomóc klientom deterministically i programowy przydzielić zastosowania przez znaczniki, dla przypadków użycia, takich jak ładowania krzyżowe.

- **Zasób metadanych udostępnianych** - szczegółów zasobów, takich jak nazwa licznik, miernik kategorii, miernik podkategorii, jednostek i region również będą przekazywane w odpowiedzi, aby nadać osób dzwoniących lepiej zrozumieć, co została wykorzystana. Firma Microsoft pracują również do wyrównywania terminologia metadanych zasobów na Azure portal, Azure zastosowania CSV, EA rozliczenia CSV oraz innych środowiska publicznych w celu umożliwienia klientom przeniesionym danych przez środowiska.

- **Zastosowania dla wszystkich typów oferty** — danych dotyczących użycia będą dostępne dla wszystkich oferują typy obejmujące między innymi repartycyjny, MSDN zobowiązań pieniężnych, pieniężne kredytowej i EA.

### <a name="azure-resource-ratecard-api-preview"></a>Azure zasobów RateCard interfejsu API (wersja Preview)
Klienci i partnerzy umożliwia API RateCard zasobów Azure przejść na liście dostępne zasoby Azure wraz z szacowany cennik dla każdej z nich. Funkcje:

- **Kontrola dostępu oparta na rolach azure** — klienci i partnerzy można skonfigurować jej zasady dostępu na [Azure portal](https://portal.azure.com) lub za pośrednictwem [polecenia cmdlet programu PowerShell Azure](powershell-install-configure.md) , aby określić, którzy użytkownicy lub aplikacji może uzyskać dostęp do danych RateCard. Osoby dzwoniące, należy użyć standardowego tokeny usługi Azure Active Directory dla uwierzytelniania. Wywołujący również musi zostać dodana do roli czytnika, właściciel albo trybu współautora, aby uzyskać dostęp do danych dotyczących użycia dla danej subskrypcji Azure.

- **Obsługę repartycyjny, MSDN, zobowiązań pieniężnych i pieniężnych kredytowej oferuje (EA nieobsługiwane)** — ten interfejs API informacje Azure stawka oferty poziom, a poziom subskrypcji.  Obiekt wywołujący tej musi przekazać informacje oferty, aby uzyskać szczegółów zasobów i stawek.  Jak ofert EA dostosowanych stawki na rejestrowania, możemy podać stawki EA w tej chwili.

## <a name="scenarios"></a>Scenariusze

Poniżej przedstawiono kilka scenariuszy, które są możliwe przy użyciu kombinacji użycia oraz interfejsy API RateCard:

- **Azure poświęcają miesiącu** - klienci mogli używać użycia i RateCard interfejsy API w połączeniu, aby uzyskać lepszą spostrzeżeń ich chmury wydatków w miesiącu, analizując przedziałów godzinowe i dzienne szacowania użycia i opłaty.

- **Skonfiguruj alerty** — klienci i partnerzy można skonfigurować alerty zasobów oparte lub pieniężnych na ich zużycie chmury uzyskując szacowane zużycie i oszacowanie opłat za pomocą wykorzystania i interfejsu API RateCard.

- **Rachunek Predict** — klienci i partnerzy mogli ich szacowane zużycie i chmury poświęcają i stosowanie algorytmów nauki maszynowego przewidywanie ich rachunku będzie na końcu cyklu rozliczeniowego.

- **Koszt zużycia wstępnej analizy** — klienci mogą również korzystanie z interfejsu API RateCard do prognozowania ilość ich rachunku będzie, jakby były przez przeniesienie ich obciążenie pracą Azure, zapewniające konieczne użycie liczb. Klienci, mając istniejących obciążeń pracą w innych chmury lub chmur prywatne, można również mapować ich zastosowania z Azure poświęcają stawki uzyskanie lepiej oszacować ich szacowany Azure. Zapewnia lepszy widok co można uzyskać za pomocą [kalkulatora Azure ceny](https://azure.microsoft.com/pricing/calculator/)(na przykład) partnerami rozliczeń udostępnia możliwość przestawianie na oferty i porównaj i kontrast między typami różnych oferty poza repartycyjny, w tym zobowiązań pieniężnych i kredytowe pieniężne. Za pośrednictwem interfejsów API zapewniają również możliwość kosztów zmiany oszacowanie według regionów, włączanie typ analizy warunkowej wymagane przy podejmowaniu decyzji wdrażania, jak wdrażanie zasobów w różnych DCs na całym świecie mają bezpośredni wpływ na całkowity koszt.

- **Analiza warunkowa** -

    - Klienci i partnerzy można określić, czy chcesz być redukcji kosztów, aby uruchomić ich obciążenie pracą w innym regionie lub w innej konfiguracji Azure zasobu. Azure zasobów, które mogą się różnić koszty według Azure region, w którym jest uruchomiony, a dzięki temu klienci i partnerzy wywołać optymalizacji koszt.

    - Klienci i partnerzy można także określić, jeśli innego typu oferty Azure zapewnia lepsze stawki Azure zasobu.

## <a name="partner-solutions"></a>Rozwiązań partnerów

[Microsoft Azure użycia i interfejsów API RateCard włączyć Cloudyn, aby zapewnić ITFM dla klientów,](billing-usage-rate-card-partner-solution-cloudyn.md) w tym artykule opisano możliwości integracji oferowanych przez partnera Azure rozliczenia API [Cloudyn](https://www.cloudyn.com/microsoft-azure/).  Ten artykuł zawiera szczegółowe zakres ich zastosowań, w tym krótki klip wideo pokazano, jak klientem Azure Cloudyn i umożliwia API rozliczenia Azure wniosków zysków z dane o jego zużycie Azure.

[Chmura Cruiser i integracji rozliczenia interfejsu API programu Microsoft Azure](billing-usage-rate-card-partner-solution-cloudcruiser.md) w tym artykule opisano [Express Cruiser chmury pakietu Azure](http://www.cloudcruiser.com/partners/microsoft/) działania bezpośrednio z poziomu portalu WAP, umożliwiając klientom bezproblemowo zarządzanie operacyjne i finansowe aspekty ich Microsoft Azure chmury połączeń publicznej prywatne lub hostowanej z pojedynczy interfejs użytkownika.   

## <a name="next-steps"></a>Następne kroki
+ Zapoznaj się z [Azure rozliczenia pozostałych API Reference](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) więcej informacji na temat obu interfejsów API, które są częścią zestawu interfejsów API udostępnianych przez Menedżera zasobów Azure.
+ Jeśli chcesz zagłębić się w prawo w kodzie przykładowych, zapoznaj się z naszą Microsoft Azure rozliczenia interfejsu API przykłady kodu na [Przykłady kodu Azure](https://azure.microsoft.com/documentation/samples/?term=billing).

## <a name="learn-more"></a>Dowiedz się więcej
+ Zobacz artykuł [Omówienie Menedżera zasobów Azure](azure-resource-manager/resource-group-overview.md) , aby dowiedzieć się więcej o Azure Menedżera zasobów.
+ Dodatkowe informacje na temat pakietu narzędzia niezbędne do pomoc w uzyskaniu zrozumienia chmury wydatków, można znaleźć w artykule Gartner [Rynku — przewodnik dotyczący narzędzia IT finansowy zarządzania (ITFM)](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb).
