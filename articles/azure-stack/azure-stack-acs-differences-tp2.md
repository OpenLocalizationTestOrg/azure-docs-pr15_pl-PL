
<properties
    pageTitle="Spójne Azure miejsca do magazynowania: różnice i zagadnienia | Microsoft Azure"
    description="Opis różnic między Azure i innych spójnych Azure zagadnienia dotyczące rozmieszczania miejsca do magazynowania."
    services="azure-stack"
    documentationCenter=""
    authors="MChadalapaka"
    manager="siroy"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="mchad"/>

# <a name="azure-consistent-storage-differences-and-considerations"></a>Spójne Azure miejsca do magazynowania: różnice i zagadnienia

Spójne Azure magazynowania to zbiór usług w chmurze miejsca do magazynowania w programie Microsoft Azure stosie. Magazyn spójne Azure oferuje obiektów blob, tabeli kolejki i funkcje zarządzania kontami z spójne Azure znaczeń właściwych. W tym artykule podsumowano różnice miejsca do magazynowania spójne Azure znane z nośnikami Azure. Zawiera podsumowanie także inne zagadnienia należy pamiętać podczas wdrażania obecnie publicznie wersja zapoznawcza Microsoft Azure stosu.

<span id="Concepts" class="anchor"><span id="_Toc386544169" class="anchor"><span id="_Toc389466742" class="anchor"><span id="_Ref428966996" class="anchor"><span id="_Toc433223853" class="anchor"></span></span></span></span></span>
## <a name="known-differences"></a>Znane różnice

Tej wersji Technical Preview spójne Azure przestrzeni dyskowej nie ma 100% funkcji zgodności z Azure miejsca do magazynowania dla wersji interfejsu API, które są obsługiwane. Znane różnic między funkcjami są następujące:

-   Niektóre typy kont miejsca do magazynowania nie są jeszcze dostępne, na przykład standardowe\_RAGRS i standardowy\_GRS.

-   Premium\_LRS magazyn kont można obsługi administracyjnej. Istnieją jednak obecnie nie wartości granicznych wydajności lub gwarancji.

-   Azure funkcja plików nie jest jeszcze dostępna.

-   Pobieranie strony zakresy interfejs API nie obsługuje pobierania stron, które różnią się migawek blob strony.

-   Pobieranie strony zakresy interfejs API zwraca strony, które mają 4 KB szczegółowości.

-   Kluczy partycją i wierszy w spójnego Azure wykonania miejsca do magazynowania w tabeli są ograniczone do 400 znaków (bajtów 800) w polu rozmiar każdej.

-   Nazwy obiektów blob w celu wykonania spójne Azure magazyn obiektów Blob usługi są ograniczone do 880 znaków (bajtów 1760) w polu rozmiar.

-   Azure spójnego wykonania miejsca do magazynowania danych dotyczących użycia miejsca do magazynowania dzierżawy raportowania udostępnia metr użycie miejsca do magazynowania, które są identyczne z tymi na Azure, z tym samym znaczeń właściwych i jednostki. Obecnie jednak licznik transakcje miejsca do magazynowania w przypadku zastosowania nie zawiera transakcje IaaS i miernik zastosowania przesyłania danych nie różnicowanie wykorzystania przepustowości przez ruch sieciowy wewnętrznych i zewnętrznych do regionu stosem Azure.

-   W zakresie funkcji zarządzania miejsca do magazynowania istnieją pewne różnice. Nie można na przykład zmienić typ konta lub mieć domen niestandardowych. Ponadto tylko spójność na poziomie interfejs API umożliwiający premii\_LRS miejsca do magazynowania w obszarze Typ konta.

## <a name="deployment-considerations"></a>Zagadnienia dotyczące rozmieszczania

-   **Tylko do testów.** Nie należy wdrażać spójne Azure miejsca do magazynowania w środowisku produkcyjnym, korzystające z bieżącej wersji programu Microsoft Azure stos Technical Preview. Ta wersja jest przeznaczona tylko do celów oceny w testowych.

-   **Wydajność**. Microsoft Azure stos Technical Preview wersja przestrzeni dyskowej spójne Azure jest zoptymalizowany wydajności.

-   **Skalowalność.** Wersji programu Microsoft Azure stos Technical Preview spójne Azure magazynu nie jest zoptymalizowana pod kątem skalowalność.

## <a name="version-support-considerations"></a>Zagadnienia dotyczące obsługi wersji

Następujące wersje są obsługiwane w tej wersji preview spójne Azure przestrzeni dyskowej:

> Biblioteka klienta Azure miejsca do magazynowania: [Zestawu SDK usługi Microsoft Azure miejsca do magazynowania 6.x .NET](http://www.nuget.org/packages/WindowsAzure.Storage/6.2.0)
>
> Azure usług danych miejsca do magazynowania: [2015-04-05 wersji interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/mt705637.aspx)
>
> Azure usługi zarządzania miejsca do magazynowania: [2015-05-01-Podgląd](https://msdn.microsoft.com/library/azure/mt163683.aspx)
> [2015-06-15](https://msdn.microsoft.com/library/azure/mt163683.aspx)
## <a name="next-steps"></a>Następne kroki

-   [Wprowadzenie do magazynu spójne Azure](azure-stack-storage-overview.md)
