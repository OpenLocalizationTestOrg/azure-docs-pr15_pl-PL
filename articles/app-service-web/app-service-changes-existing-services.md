<properties
    pageTitle="Usługa Azure aplikacjami i ich wpływu na istniejące usługi Azure"
    description="W tym artykule wyjaśniono, jak nowej usługi Azure w aplikacji i jego funkcji wpływ na istniejące usługi platformy Azure."
    services="app-service"
    documentationCenter=""
    authors="yochay"
    manager="nirma"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/12/2016"
    ms.author="yochayk"/>


# <a name="azure-app-service-and-existing-azure-services"></a>Azure aplikacji usługi i istniejące usługi Azure

W tym artykule omówiono zmiany istniejących usług Azure jako część zmian ze sobą kilka usług Azure do [Azure aplikacji usługi](https://azure.microsoft.com/services/app-service/), to nowa oferta zintegrowane.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="overview"></a>Omówienie

[Usługa Azure aplikacji](https://azure.microsoft.com/services/app-service/) jest usługi w chmurze nowych i unikatowe, które umożliwia tworzenie witryny sieci web i aplikacji dla urządzeń przenośnych dla każdej platformie i dowolnego urządzenia. Usługa aplikacji jest zintegrowane przeznaczone do usprawnianie powtarzających się funkcje kodowania, integracja z systemami władz akredytacji bezpieczeństwa i przedsiębiorstwa i automatyzowanie procesów biznesowych podczas spotkania wymaganiom dotyczącym zabezpieczeń, niezawodności oraz skalowalność.

Aplikacji usługi zgromadzono następujące istniejące Azure usługi — [Telefon komórkowy](https://azure.microsoft.com/services/mobile-services/), [witryn sieci Web](https://azure.microsoft.com/services/websites/)i [Usług Biztalk](https://azure.microsoft.com/services/biztalk-services/) do pojedynczej usługi połączone, dodając nowe możliwości.  Usługa aplikacji umożliwia obsługiwać następujących typów aplikacji:

-   Aplikacje sieci Web
-   Aplikacje dla urządzeń przenośnych
-   Interfejs API aplikacji
-   Aplikacje warunków logicznych

W poniższej tabeli opisano, jak istniejącej mapy usług Azure do aplikacji usługi i typy aplikacji dostępne w nim.

<table>
<thead>
<tr class="header">
<th align="left", style="width:10%">Istniejąca usługa Azure</th>
<th align="left", style="width:10%">Azure aplikacji usługi</th>
<th align="left", style="width:80%">Co zmieniło się</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Azure witryn sieci Web</td>
<td align="left">Aplikacje sieci Web</td>
<td align="left"><li>Azure witryn sieci Web usługa aplikacji jest ograniczone do zmiana nazwy witryny sieci Web do aplikacji sieci Web.
<p><li>Wszystkie wystąpienia istniejącej witryny sieci Web są teraz aplikacji sieci Web w aplikacji usługi.</p>
<p><li>Można korzystać z istniejącej witryny sieci Web przez <a href="http://go.microsoft.com/fwlink/?LinkId=529715">Azure Portal</a>, gdzie można znaleźć wszystkie istniejące witryny w obszarze <em>Aplikacje sieci Web</em>.</p>
<p><li><em>Planowanie hostingu w sieci Web</em> jest teraz <em>Plan usługi aplikacji</em>. <em>Planowanie usługi aplikacji</em> może obsługiwać dowolnego typu aplikacji usługi aplikacji, takiej jak aplikacje sieci Web, telefon komórkowy, logiczny lub interfejsu API.</p>
<p><li>Azure usługi aplikacji sieci Web jest na ogół dostępności.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/web/">Dowiedz się więcej o aplikacjach sieci Web</a>.</p></td>
</tr>
<tr class="even">
<td align="left">Azure usług mobilnych</td>
<td align="left">Aplikacje dla urządzeń przenośnych</td>
<td align="left"><p><li>Telefon komórkowy usługi nadal są dostępne jako samodzielnej usługi i pozostawanie w pełni obsługiwane.</p>
<p><li>Aplikacje Mobile jest typem aplikacji w aplikacji usługi, której integruje wszystkie funkcje usług Mobile i nie tylko.</p>
<p><li>Jest proste, aby <a href="http://go.microsoft.com/fwlink/?LinkID=724279&clcid=0x409">przeprowadzić migrację z usług Mobile aplikacji Mobile</a>.</p>
<p><li>Częścią aplikacji usługi aplikacji Mobile Uzyskaj nowe funkcje poza Mobile usług, takich jak integracja z lokalnego i systemy władz akredytacji bezpieczeństwa tymczasowej gniazda, WebJobs, opcje lepiej skalowania i innych elementów.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/mobile/">Dowiedz się więcej o aplikacji Mobile</a>.</p>
</tr>
<tr class="odd">
<td align="left"></td>
<td align="left">Interfejs API aplikacji</td>
<td align="left">
<p><li>Interfejs API aplikacji jest nowy typ aplikacji w aplikacji usługi, który pozwala na łatwe tworzenie i używanie interfejsów API w chmurze.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/api/">Dowiedz się więcej o aplikacji interfejsu API</a>.</p></td>
</tr>
<tr class="even">
<td align="left"></td>
<td align="left">Aplikacje warunków logicznych</td>
<td align="left">
<p><li>Logika aplikacji jest nowy typ aplikacji w aplikacji usługi, która pozwala na łatwe zautomatyzowanie procesów biznesowych.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/logic/">Dowiedz się więcej o aplikacjach logiki</a>.</p></td>
</tr>
<tr class="odd">
<td align="left">Usługi Azure BizTalk</td>
<td align="left">Aplikacje BizTalk interfejsu API</td>
<td align="left">
<li><p>Usługi BizTalk nadal są dostępne jako samodzielnej usługi i pozostawanie w pełni obsługiwane.</p>
<li><p>Wszystkie funkcje usług BizTalk zostały zintegrowane w aplikacji usługi jako aplikacje interfejsu API umożliwiając użytkownikom wykonywanie integracji aplikacji dla przedsiębiorstw i B2B scenariuszy integracji z dowolnego typu aplikacji w aplikacji usługi</p>
<li><p>Z aplikacjami logiczny można teraz automatyzowanie procesów biznesowych za pomocą środowisko projektowania wizualnego tworzenie przepływów pracy</p></td>
</tr>
</tbody>
</table>

Aby uzyskać więcej informacji, odwiedź stronę [dokumentacji usługi aplikacji](https://azure.microsoft.com/documentation/services/app-service/).
