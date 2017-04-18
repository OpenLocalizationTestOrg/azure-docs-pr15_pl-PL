<properties
    pageTitle="Często zadawane pytania dotyczące bazy danych ClearDB MySql za pomocą usługi aplikacji Azure | Microsoft Azure"
    description="Odpowiedzi na często zadawane pytania dotyczące używania bazy danych ClearDB MySQL z Azure aplikacji usługi."
    documentationCenter="php"
    services=""
    authors="sunbuild"
    manager="yochayk"
    editor=""
    tags="mysql"/>

<tags
    ms.service="multiple"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="sumuth"/>

# <a name="faq-for-cleardb-mysql-databases-with-azure-app-service"></a>Często zadawane pytania dotyczące bazy danych ClearDB MySql z Azure aplikacji usługi

Niniejszy artykuł zawiera odpowiedzi na często zadawane pytania dotyczące przy użyciu i zakupów ClearDB MySQL baz danych dla aplikacji sieci Web Azure.

## <a name="what-options-do-i-have-for-mysql-on-azure"></a>Jakie opcje są dostępne dla MySQL Azure?

Dostępnych jest kilka możliwości:

* [Bazy danych MySQL udostępnione ClearDB](/marketplace/partners/cleardb/databases/)

* [Klastrów ClearDB MySQL Premium](/marketplace/partners/cleardb-clusters/cluster/)

* [Klaster programu MySQL uruchomionych maszyn wirtualnych Azure](https://github.com/azure/azure-quickstart-templates/tree/master/mysql-replication)

* [Jedno wystąpienie MySQL uruchomionych maszyn wirtualnych Azure](virtual-machines/virtual-machines-windows-classic-mysql-2008r2.md)

ClearDB jest MySQL usłudze hostingu i zarządza infrastruktury MySQL. Po uruchomieniu własny klaster programu MySQL lub bazy danych na maszyn wirtualnych Azure, musisz skonfigurować serwer MySQL i aktualizacji poprawki.

## <a name="do-i-need-a-credit-card-for-the-web-app--mysql-template-in-the-azure-marketplace"></a>Czy potrzebuję karty kredytowej dla aplikacji sieci Web + szablonu MySQL w Azure Marketplace?

To zależy od typu subskrypcji, którego używasz. Oto niektóre typy często używane subskrypcji:

* [Płatność przy zakupie](/offers/ms-azr-0003p/): wymaga z karty kredytowej, a przy zakupie płatnej bazy danych MySQL jest naliczany karty kredytowej.

* [Bezpłatna wersja próbna](https://azure.microsoft.com/pricing/free-trial/): zawiera środków do użytku z Microsoft Azure usług, ale nie zezwala na zakup zasobów innych firm. Aby kupić usług innych firm lub płatnej bazy danych MySQL, musisz użyć karty kredytowej włączone subskrypcji. W przypadku aplikacji sieci Web możesz utworzyć bazą danych ClearDB MySQL bezpłatne.

* [Subskrypcji MSDN](/pricing/member-offers/msdn-benefits/) i **MSDN deweloperów Test zapłacić jako możesz przejść**: podobne do bezpłatnej wersji próbnej, subskrypcji MSDN wymaga karty kredytowej w celu zakupu płatnej rozwiązanie MySQL ClearDB.

* [Umowa Enterprise (EA)](/pricing/enterprise-agreement/): EA klienci są wystawiona przed ich EA każdego kwartału dla wszystkich swoich zakupów (innych firm) Azure Marketplace na fakturze oddzielnych, skonsolidowane. Konta poza zobowiązanie pieniężne na żadnych zakupów marketplace. Należy zauważyć, że w tej chwili sklepu Azure nie jest dostępna dla klientów w Azerbejdżan, Chorwacja, Norwegia i Portoryko. 

*  [DreamSpark](https://www.dreamspark.com/Product/Product.aspx?productid=99): możesz utworzyć tylko bezpłatne ClearDB baz danych dla aplikacji sieci Web. Nie istnieje limit liczby bezpłatne ClearDB MySQL baz danych, które można utworzyć. Zauważ, że są bezpłatne bazy danych nie ma być używana w przypadku aplikacji sieci web produkcji, zgodnie z tej usługi jest przeznaczona tylko dla wersji próbnej.

## <a name="why-was-i-charged-350-for-a-web-app--mysql-from-the-azure-marketplace"></a>Dlaczego został poniesiony 3.50 $ dla aplikacji sieci Web + MySQL z Azure Marketplace

Titan, czyli $3.50 jest domyślna opcja bazy danych. Nie możemy wyświetlić koszt podczas tworzenia bazy danych, a przez pomyłkę zakupiono bazy danych, które nie mają być. Firma Microsoft próby znalezienia sposób w celu zwiększenia możliwości, ale do tego czasu możesz sprawdzić wszystkie do wybranego cennik poziomów dla aplikacji sieci web i bazy danych przed klikając pozycję **Tworzenie** i uruchamianie wdrożenia zasobów.

## <a name="i-am-running-mysql-on-my-own-azure-virtual-machine-can-i-connect-my-azure-web-app-to-my-database"></a>Używam MySQL w moim własnym Azure maszyn wirtualnych. Czy mogę nawiązać Moja aplikacja sieci web Azure bazy danych?

Wartość Tak. Jak długo maszyn wirtualnych usługi Azure udzieliła dostępu zdalnego do aplikacji sieci web można połączyć aplikacji sieci web do bazy danych. Aby uzyskać więcej informacji zobacz [Instalowanie MySQL na komputerze wirtualnych](virtual-machines/virtual-machines-windows-classic-mysql-2008r2.md).

## <a name="in-which-countries-are-cleardb-premium-mysql-clusters-supported"></a>Kraje znajdują się klastrów ClearDB Premium MySQL obsługiwane?

[ClearDB Premium MySQL klastrów](/marketplace/partners/cleardb-clusters/cluster/) są dostępne we wszystkich regionach Azure na całym świecie, z wyjątkiem Indie, Australii, Brazylii na południu i Chiny.

## <a name="can-i-create-a-new-cluster-prior-to-creating-a-database-with-cleardb-premium-cluster-solution"></a>Czy można utworzyć nowy klaster przed utworzeniem bazy danych przy użyciu ClearDB premium klaster rozwiązanie?

Nie, tworzenie pustego klastrów ClearDB nie jest obsługiwane. Azure portal umożliwia tworzenie bazy danych w klastrze, może utworzyć nowy klaster w tym samym czasie.

## <a name="will-i-be-warned-if-i-try-to-delete-a-cleardb-mysql-database-that-is-in-use-by-one-of-my-applications"></a>Będzie można wyświetlane ostrzeżenie Jeśli próby usunięcia bazą danych ClearDB MySQL, który jest używany przez jeden z moich aplikacji?

Nie, Azure nie ostrzega należy usunąć zakupu marketplace, która zależy od aplikacji.

## <a name="which-regions-can-i-create-cleardb-databases-in"></a>Regiony można utworzyć ClearDB baz danych w?

Azure Marketplace nie jest dostępna dla klientów w Azerbejdżan, Chorwacja, Norwegia lub Portoryko. ClearDB nie jest dostępna w następujących regionach.

## <a name="what-pricing-tier-should-i-choose-for-a-production-web-app-and-database"></a>Jakie cennik Warstwa wybierz dla aplikacji sieci web produkcji i bazy danych

W przypadku aplikacji sieci Web za pomocą Basic lub wyższego poziomu cennik. ClearDB zalecamy Saturn albo Jowisz planu. Przejrzyj funkcje i ograniczenia każdej warstwy cennik zarówno [Aplikacji sieci Web](/pricing/details/app-service/) i [baz danych ClearDB MySQL](/marketplace/partners/cleardb/databases/) , wybierz ten, który odpowiada Twoim potrzebom.

## <a name="how-do-i-upgrade-my-cleardb-database-from-one-plan-to-another"></a>Jak uaktualnić ClearDB bazy danych z jednego planu do innego?

[Azure portal](https://portal.azure.com)można rozbudowy ClearDB udostępnionej hostingu bazy danych. Przeczytaj ten [artykuł](https://blogs.msdn.microsoft.com/appserviceteam/2016/10/06/upgrade-your-cleardb-mysql-database-in-azure-portal/) , aby dowiedzieć się więcej. Firma Microsoft obecnie nie obsługuje uaktualnienia dla klastrów ClearDB Premium w portalu Azure.

## <a name="i-cant-see-my-cleardb-database-in-azure-portal"></a>Nie widzę bazy danych dodatku ClearDB Azure portal?

Jeśli tworzymy ClearDB bazy danych za pomocą Menedżera zasobów Azure lub [Nowy Portal Azure](https://portal.azure.com)nie będą widoczne w [Starej Portal Azure](https://manage.windowsazure.com). Obejście to jest aby ręcznie połączyć bazy danych aplikacji sieci web. Podobnie jeśli utworzyć bazę danych ClearDB w [portalu stare](https://manage.windowsazure.com) nie będzie możliwe zobaczenie bazy danych w [Nowych Portal Azure](https://portal.azure.com). Istnieje nie obejście ostatnie scenariusza.

## <a name="who-do-i-contact-for-support-when-my-database-is-down"></a>Kto skontaktować obsługi po bazy danych nie działa?

Problemy związane z kontaktów [ClearDB pomocy technicznej](https://www.cleardb.com/developers/help/support) dla każdej bazy danych. Przygotuj się na udostępnienie ich z informacjami o Azure subskrypcji.

## <a name="can-i-create-additional-users-for-my-cleardb-mysql-database-cluster-solution"></a>Czy można utworzyć dodatkowych użytkowników do mojej bazy danych ClearDB MySQL klastrze? 

Wartość nie. Nie można tworzyć dodatkowych użytkowników, ale można tworzyć dodatkowych baz danych na ClearDB klaster bazy danych.  

## <a name="can-basicpro-series-databases-be-upgraded-in-place-similar-to-planetary-plans-today-on-cleardb-portal"></a>Można serii Basic/Pro baz danych można uaktualnienie w miejscu podobne do dzisiaj granicznej planów portalu ClearDB?

Tak, podstawowe serię, którą baz danych może być uaktualnione w miejscu (podstawowe 60 za pośrednictwem podstawowe 500). Seria Pro może być uaktualnienie w miejscu (Pro 125 za pośrednictwem Pro 1000) z wyjątkiem Pro 60. Uaktualnianie bazy danych Pro 60 obecnie nie jest obsługiwana. 

## <a name="when-i-migrate-my-resources-from-one-subscription-to-another-does-my-cleardb-mysql-database-get-migrated-as-well"></a>Jeśli zasoby przeprowadzić migrację z jedną subskrypcję do innego, czy bazy danych ClearDB MySQL uzyskać migracji także? 

Po wykonaniu migracji zasobów w subskrypcjach zastosować pewne [ograniczenia](./app-service-web/app-service-move-resources.md) . Bazy danych ClearDB MySQL jest usługi innych firm i w związku z tym nie uzyskać migracji podczas migracji Azure subskrypcji. Jeśli nie zarządzać migracji bazy danych MySQL przed migracji zasobów Azure, bazy danych ClearDB MySQL można wyłączyć. Ręczne Migrowanie najpierw baz danych, a następnie wykonaj migracji Azure subskrypcji dla aplikacji sieci web. 

## <a name="can-i-transfer-a-cleardb-database-from-a-credit-card-subscription-to-an-ea-subscription"></a>Czy można przesłać ClearDB bazy danych z subskrypcji karty kredytowej do subskrypcji EA?

Istniejące ClearDB bazy danych za pomocą karty kredytowej skojarzone z istniejącej subskrypcji. Aby użyć subskrypcji EA musisz przeprowadzić migrację danych do nowej bazy danych:

* Kup nową bazę danych ClearDB z subskrypcją EA.
* Migrowanie danych do nowej bazy danych.
* Aktualizowanie aplikacji do nowej bazy danych.
* Usuń stary ClearDB bazy danych.

Podczas tworzenia nowej aplikacji sieci web z MySQL (ClearDB) lub tworzenie bazy danych MySQL (ClearDB), subskrypcję, którą możesz wybrać Określa, jak chcesz zapłacić za usługę. Z subskrypcją usługi EA firma Microsoft nie blokuje zamówień usług innych firm, takich jak ClearDB w portalu Azure. Subskrypcje EA są wystawiona poza zobowiązania pieniężne i są zafakturowane kwartalny i zaległe. Klienta EA musi skonfigurować metodę płatności, takich jak karty kredytowej płatności dla wszystkich usług innych firm marketplace.

## <a name="where-can-i-see-the-charges-for-cleardb-resources-in-an-ea-subscription"></a>Gdzie zobaczyć opłaty za ClearDB zasobów w postaci subskrypcji EA?

Dla klientów bezpośrednich EA opłaty Azure Marketplace są widoczne w module Enterprise Portal. Zauważ, że wszystkie zakupy marketplace i zużycie są wystawiona poza zobowiązania pieniężne i są wystawiona kwartalne i zaległe. EA klienci muszą płacić bezpośrednio do dostawców usług innych firm i to zrobić, włączając metodę płatności, takiej jak karta kredytowa przy użyciu swojego konta EA.

Pośrednie EA klienci można znaleźć subskrypcji usługi Azure Marketplace na stronie **Zarządzaj subskrypcjami** Enterprise Portal, ale ceny jest ukryty. Klienci powinni kontaktować się ich LSP uzyskać informacji na temat opłaty marketplace.

Dostęp do usługi Azure Marketplace usługi innych firm można zarządzać administratorów rejestrowania EA Azure. Ich wyłączyć lub ponownie włączyć dostęp zakupów strona 3 z magazynu w subskrypcji i Zarządzaj kontami w sekcji kont w module Enterprise Portal.

## <a name="who-do-i-contact-for-questions-about-my-bill-for-cleardb-services-in-my-ea-subscription"></a>Kto skontaktować się na pytania dotyczące rachunku dla usług ClearDB w subskrypcji EA?

W odniesieniu do rozliczenia w obszarze ich rejestrowania EA, skontaktuj się z [Obsługą klienta przedsiębiorstwa](http://aka.ms/AzureEntSupport) . Zespół pomocy technicznej portalu EA będzie odpowiedzi na Twoje pytanie lub ułatwić rozwiązanie problemu.

 



## <a name="more-information"></a>Więcej informacji

[Często zadawane pytania dotyczące usługi Azure Marketplace](/marketplace/faq/)
