<properties
   pageTitle="Techniczne wymagania wstępne dotyczące tworzenia usługi danych dla Marketplace | Microsoft Azure"
   description="Opis wymagań dotyczących tworzenia usługi danych do wdrożenia i sprzedaży w Azure Marketplace"
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/26/2016"
   ms.author="hascipio; avikova" />

# <a name="technical-pre-requisites-for-creating-a-data-service-offer-for-the-azure-marketplace"></a>Oferty techniczne wymagania wstępne dotyczące tworzenia usługi danych dla usługi Azure Marketplace

>[AZURE.IMPORTANT] **W tej chwili firma Microsoft nie są już ułatwiającej rozpoczęcie korzystania wszelkie nowe wydawcy usługi danych. Nowy dataservices nie będzie uzyskiwanie zatwierdzany dla listy.** Jeśli chcesz opublikować na AppSource aplikacji biznesowych władz akredytacji bezpieczeństwa można znaleźć więcej informacji [w tym miejscu](https://appsource.microsoft.com/partners). Jeśli masz aplikacje IaaS lub chcesz opublikować na Azure Marketplace usługi Deweloper można znaleźć więcej informacji [w tym miejscu](https://azure.microsoft.com/marketplace/programs/certified/).

Przeczytaj proces dokładnie przed rozpoczęciem i opis, gdzie i dlaczego jest wykonywane każdego kroku. Ile to możliwe, możesz należy przygotowywanie informacji o firmie i innych danych, Pobierz niezbędne narzędzia i/lub tworzenia składników techniczne przed rozpoczęciem procesu tworzenia oferty.

Należy dysponować następującymi elementami gotowe przed rozpoczęciem procesu:

## <a name="make-a-decision-on-what-technology-will-be-used-to-publish-your-data-service-offer"></a>Podejmowania decyzji, w jakiej technologii są używane do publikowania tej oferty usługi danych

W programie Publisher można zdecydować, między wielu technologii podczas publikowania usługi danych w Azure Marketplace. Główny technologii, które są obsługiwane opisane poniżej. Niezależnie od technologii, która jest używana do publikowania usługi danych, użytkownik końcowy korzysta z danych przy użyciu **źródła strumieniowego OData** uwidocznionego przez usługę Azure Marketplace. Pełne informacje na temat usługi OData, które można znaleźć w [http://www.odata.org/](http://www.odata.org/)

## <a name="sql-azure-database"></a>SQL Azure bazy danych

Gotowe platformy SQL Azure zestawu danych jest zobowiązań programu Publisher. Musisz subskrybować Azure, obsługi administracyjnej odpowiedni rozmiar bazy danych i przekazywanie danych do bazy danych SQL Azure. Program Publisher odpowiada również zawsze aktualne jego danych. Więcej informacji na temat subskrybowania usługi Azure, które można znaleźć w [https://azure.microsoft.com/services/sql-database/](https://azure.microsoft.com/services/sql-database/)


Podczas przenoszenia danych do platformy SQL Azure, Azure Marketplace mogą ujawnić, tabele i widoki. Wydawcy można określić, które tabele i widoki i kolumny są dostępne do użytkownika końcowego. Dalsze dostawca zawartości można również określić kolumny, które mogą być wyszukiwane przez użytkownika końcowego i które z nich są tylko zwracane w ładunku. Dzięki temu wysokiego poziomu elastyczność o tym, które należy dostępne w bazie danych. Kolumny, które można wyszukiwać wymaga kopii przez jeden lub więcej wskaźników bazy danych.

## <a name="rest-based-web-service"></a>Usługa sieci web REST zależności

Obsługiwane Protocol (protokół): **tylko HTTPS**

Istniejące usługi REST podstawie można są dostępne za pośrednictwem usługi Azure Marketplace. Ponieważ zestawu danych jest zawsze dostępna użytkowników końcowych jako źródła strumieniowego OData, potrzeb usługi Azure Marketplace w celu można zamapować usługę do OData podstawie usługi. W tym celu pozostałych podstawie punkty końcowe muszą udostępnić wszystkie parametry jako parametry HTTP.

Ładunek musi się znajdować w formularzu, który można mapować do odpowiedzi ATOM. W związku z tym odpowiedzi od usług musi znajdować się w formacie XML i tylko może zawierać jeden element powtarzających się, zawierającą wartości ładunku (na przykład zestawu rekordów). Usługi Azure Marketplace będzie mapować węzeł powtarzających się do węzła wpis w ATOM i wartości ładunku na węzły właściwości w węźle wpisu.

Informacje (na przykład interfejsu API klucz, uwierzytelniania token, itp.) musi być pod warunkiem, że jako parametr HTTP lub w nagłówku HTTP (para wartości klucza) — uwierzytelnianie podstawowe jest również obsługiwane. Prawidłowego klucza musi być udostępniana i wszystkie żądania za pośrednictwem usługi Azure Marketplace są określane jako za pośrednictwem tego klawisza. Użytkownik monitorowania i rozliczenia się dzieje w warstwie Azure Marketplace.

Błędy zwrócone przez usługę muszą zostać zamapowane na kody stanu HTTP. W przypadku, gdy usługa zwraca kod XML, który zawiera błąd te mają zostać zamapowane przez usługę Azure Marketplace, aby kody stanu HTTP.

## <a name="soap-based-web-services"></a>Usługi sieci web SOAP zależności

Protokół: **Tylko HTTPS**

Wymagania są takie same jak w pozostałych podstawą sekcji usługi. Jedyna różnica polega na tym, że parametry może być udostępniana w treści XML, które są ogłaszane usłudze wydawcy z każdego żądania za pomocą usługi Azure Marketplace. Oznacza to, że parametry HTTP, które użytkownik udostępnia na zewnętrzną jest tłumaczona do elementów XML dokumentu XML, które są ogłaszane z wezwaniem do usługi sieci web dostawcy zawartości.

## <a name="odata-based-web-services"></a>OData podstawie usług sieci web

Protokół: **Tylko HTTPS**

Dane mogą być udostępniane usług OData do Azure Marketplace. System ma przekazać usługi za pomocą i zastępuje katalog główny usługi katalogu usługi Azure Marketplace — aby upewnić się, że wszystkie kolejne połączenia obsłużone Azure Marketplace.

Usługi OData nie wystarczy przejść w bazie danych w wewnętrznej bazy danych. OData obsługuje dowolnego rodzaju miejsca do magazynowania lub logikę biznesową do kierowania usługę.


## <a name="next-steps"></a>Następne kroki
Teraz, gdy przejrzeć wymagania wstępne i niezbędne zadania wykonane jako ukończone, można przechodzić do przodu z tej oferty usługi danych jako szczegółowe [Dane usługi publikacji Przewodnik](marketplace-publishing-data-service-creation.md)tworzenia.

Lub, jeśli chcesz przejrzeć całego procesu i artykuły odpowiednich dla każdej fazy publikowania, odwiedź stronę tego artykułu [Wprowadzenie: jak publikować ofertę Azure Marketplace](marketplace-publishing-getting-started.md).

[link-acct]:marketplace-publishing-accounts-creation-registration.md
