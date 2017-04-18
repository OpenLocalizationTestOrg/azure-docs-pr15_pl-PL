<properties
   pageTitle="Interfejs API wersja wyszukiwania Azure | Microsoft Azure | Interfejs API wyszukiwania"
   description="Wersja zasad dla interfejsy API pozostałych wyszukiwania Azure i Biblioteka klienta w .NET SDK."
   services="search"
   documentationCenter=""
   authors="brjohnstmsft"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="dotnet"
   ms.workload="search"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.date="08/16/2016"
   ms.author="brjohnst"/>

# <a name="api-versions-in-azure-search"></a>Interfejs API wersji w wyszukiwaniu Azure

Azure wyszukiwania z rozwiniętymi informacjami aktualizacji funkcja regularne. Czasami, ale nie zawsze aktualizacje te wymagają nam Publikowanie nowej wersji naszych interfejsu API w celu zachowania zgodności z poprzednimi wersjami. Publikowania nowej wersji pozwala kontrolować, kiedy i jak zintegrować aktualizacje usługi wyszukiwania w kodzie.

Jako zasadę firma Microsoft, spróbuj opublikować nowsze wersje tylko wtedy, gdy jest to konieczne, ponieważ może dotyczyć niektórych starań, aby uaktualnić swój kod, aby używać nowej wersji interfejsu API. Nowa wersja firma Microsoft opublikuje tylko, jeśli trzeba zmienić niektóre aspekty interfejsu API w taki sposób, aby podziały zgodność z poprzednimi wersjami. Dzieje się tak ze względu na rozwiązania z istniejących funkcji lub ze względu na nowe funkcje, które zmieniają istniejących powierzchni interfejsu API.

Firma Microsoft wykonaj tej samej reguły SDK aktualizacje. SDK wyszukiwania Azure regułom [semantyczny przechowywania wersji](http://semver.org/) , co oznacza, że wersja zawiera trzy elementy: główne, pomocnicza i zbudowania numer (na przykład 1.1.0). Firma Microsoft zostanie udostępniony w nowej wersji głównej SDK tylko w przypadku zmiany, które podziału zgodność z poprzednimi wersjami. Aktualizacji funkcji nierozdzielające firma Microsoft będzie zwiększać wersja pomocnicza, a dla poprawki możemy tylko zwiększa wersji kompilacji.

##<a name="snapshot-of-current-versions"></a>Migawka bieżącej wersji 

Poniżej jest migawki bieżących wersji wszystkie interfejsy programowania do wyszukiwania Azure. Planów i inne szczegóły można znaleźć w kolejnych sekcjach tego dokumentu.

Interfejsy|Najnowsza wersja główna|Stan
----------|-------------------------|------
[ZESTAW SDK PROGRAMU .NET](https://msdn.microsoft.com/library/azure/dn951165.aspx)|1.1|Zazwyczaj dostępny, wydane lutego 2016
[Podgląd SDK .NET](https://msdn.microsoft.com/library/mt761536%28v=azure.103%29.aspx)|Podgląd 2.0|Podgląd wydane sierpnia 2016
[Interfejs API usługi REST](https://msdn.microsoft.com/library/azure/dn798935.aspx)|2015-02-28|Ogólnodostępne
[Usługi REST interfejsu API Preview](search-api-2015-02-28-preview.md)|2015-02-28-Podgląd|Podgląd
[Zarządzanie interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/dn832684.aspx)|2015-08-19|Ogólnodostępne

Dla pozostałych interfejsów API, łącznie z `api-version` w każdej konwersacji jest wymagane. Ułatwia kierowania określonej wersji, na przykład podgląd interfejsu API. Poniższy przykład przedstawia sposób, w jaki `api-version` parametr:

    GET https://adventure-works.search.windows.net/indexes/bikes?api-version=2015-02-28

> [AZURE.NOTE] Mimo że ma każdego żądania `api-version`, zaleca się używanie tej samej wersji dla wszystkich żądań interfejsu API. Jest to zwłaszcza wtedy, gdy nowe wersje interfejsu API wprowadzenie atrybutów lub operacji, które nie są rozpoznawane przez poprzednie wersje. Mieszanie wersji interfejsu API może mieć niezamierzone konsekwencje i należy unikać.
> 
Interfejs API usługi REST i interfejsu API usługi REST zarządzania są wersji niezależnie od siebie. Wszelkie podobieństwo numery wersji jest przypadkowe Współtworzenie.

Ogólnodostępne (lub GA) interfejsów API może być używana w produkcji i będą podlegać umów dotyczących poziomu usług Azure. Podgląd wersje mają badawczych funkcje, które zawsze nie są migrowane do wersji GA. **Zdecydowanie zalecamy, aby przed przy użyciu podglądu interfejsy API w aplikacjach produkcji.**

##<a name="sdk-version-roadmap"></a>Przewodnik po materiałach SDK wersji

W każdej wersji .NET SDK jest przeznaczony dla określonej wersji interfejsu API usługi REST. Funkcje są wdrażania w interfejsie API usługi REST najpierw, a następnie zaimplementowana w zestawie SDK.

.NET SDK jest zazwyczaj dostępny i pracy jest już trwającej na następną wersję. Poniższa tabela wygląda wcześniejsze przyszłych wersjach zestawu SDK, dzięki czemu będziesz mieć uzyskać ogólny obraz tego nadchodzących wydarzeniach.

Wersja .NET SDK|Wersja interfejsu API usługi REST|Funkcje|ETA
----------------|----------------|--------|---
1.1|2015-02-28|Składnia kwerendy Lucene|Luty 2016
Podgląd 2.0|2015-02-28-Podgląd|Niestandardowe programy do analizowania, obiektów Blob platformy Azure i tabeli indeksatory mapowania pól ETags|Sierpnia 2016
2.x|Nowa wersja interfejsu API GA|Identyczne Podgląd 2.0|Wcześniejsze Q4 2016

##<a name="about-preview-and-generally-available-versions"></a>Informacje o wersji Preview i ogólnodostępne

Azure wyszukiwania zawsze wstępnie udostępnia funkcje badawczych za pośrednictwem interfejsu API usługi REST najpierw następnie za pośrednictwem wersje wstępne SDK .NET.

Funkcje Preview nie gwarantuje są przenoszone do wersji GA. Funkcje w wersji GA są uznane za stabilny i prawdopodobne, aby zmienić z wyjątkiem małych poprawki zgodny z poprzednimi wersjami i rozszerzenia, Podgląd funkcje są dostępne dla badań i doświadczeń, w celu zbieranie opinii na funkcję projektowania i wdrażania. 

Jednak funkcji podglądu jest mogą ulec zmianie, dlatego zalecamy przed pisania kodu produkcji, która ma zależność od wersji preview. Jeśli używasz starszej wersji preview, zaleca się migrowanie ogólnodostępne wersji (GA). 

Dla zestawu SDK .NET: wskazówki dotyczące migracji kodu można znaleźć na [uaktualnienie .NET SDK](search-dotnet-sdk-migration.md).

Ogólnodostępną oznacza, że Azure wyszukiwania jest teraz w obszarze Umowa dotycząca poziomu usług (SLA). Umowa dotycząca poziomu usług można znaleźć w [Azure umów dotyczących poziomu usług wyszukiwania](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

