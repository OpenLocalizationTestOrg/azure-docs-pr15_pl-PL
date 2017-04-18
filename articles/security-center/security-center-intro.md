<properties
   pageTitle="Wprowadzenie do Centrum zabezpieczeń Azure | Microsoft Azure"
   description="Informacje na temat Centrum zabezpieczeń Azure, klucza możliwości i sposobu jej działania."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/21/2016"
   ms.author="terrylan"/>

# <a name="introduction-to-azure-security-center"></a>Wprowadzenie do Centrum zabezpieczeń Azure

Informacje na temat Centrum zabezpieczeń Azure, klucza możliwości i sposobu jej działania.

> [AZURE.NOTE] Ten dokument wprowadza usługę przy użyciu Przykładowe wdrożenie.

## <a name="what-is-azure-security-center"></a>Co to jest Centrum zabezpieczeń Azure?
 Centrum zabezpieczeń ułatwia zapobieganie, wykrywanie i odpowiadanie na zagrożenia za pomocą lepszą wgląd i kontroli nad zabezpieczeniami Azure zasobów. Zapewnia zabezpieczenia zintegrowane monitorowania i zasad zarządzania w subskrypcjach usługi Azure, pomaga wykrywać zagrożenia, które w przeciwnym razie przejdź niezauważalnie i współpraca z Szeroki ekosystem rozwiązania zabezpieczeń.

##  <a name="key-capabilities"></a>Kluczowe funkcje
 Centrum zabezpieczeń udostępnia zagrożenie łatwe w obsłudze i skuteczne zapobieganie, wykrywanie i odpowiedź możliwości, jakie są wbudowane Azure. Kluczowe funkcje są:

| | |
|----- |-----|
| Zapobieganie | Monitoruje stan zabezpieczeń Azure zasobów |
| | Określa zasady Azure subskrypcje i grup zasobów na podstawie wymagań dotyczących zabezpieczeń firmy, typy aplikacje, których używasz i czułości danych |
| | Zalecenia dotyczące zabezpieczeń przy użyciu sterowanych zasad ułatwi właściciele usługi przez proces implementacji potrzebne kontrolki |
| | Szybkie wdrożenie usługi zabezpieczeń i wyposażenia firmy Microsoft i partnerów |
| Wykrywanie |Automatycznie zbiera i analizuje zabezpieczeń danych z usługi Azure zasobów, sieci i rozwiązań partnerów, takich jak programy ochrony przed złośliwym oprogramowaniem i zapory |
| | Globalne wykorzystuje schematy zagrożenie analizy produktów firmy Microsoft i usług, jednostkę zbrodni cyfrowego (DCU) firmy Microsoft, zabezpieczeń odpowiedzi centrum MSRC (Microsoft) i zewnętrznych źródeł danych |
| | Stosuje zaawansowanej analizy, w tym maszynowego uczenia się i analizy funkcjonalne |
| Odpowiadanie | Udostępnia zdarzeń i alerty zabezpieczeń priorytetów |
| | Udostępnia spostrzeżeń źródła atakiem i ryzyko zasobów |
| | Podano sposoby zatrzymać bieżącą atakiem i zabezpieczyć się przed atakami przyszłych |

## <a name="introductory-walkthrough"></a>Przewodnik wprowadzający
 Masz dostęp do Centrum zabezpieczeń z [Azure portal](https://azure.microsoft.com/features/azure-portal/). , [Zaloguj się do portalu](https://portal.azure.com), wybierz pozycję **Przeglądaj**, a następnie przewiń do opcji **Centrum zabezpieczeń** lub wybór **Centrum zabezpieczeń** przypięte wcześniej do portalu pulpitu nawigacyjnego.

![Kafelek zabezpieczeń w Azure portal][1]

Centrum zabezpieczeń można ustawić zasady zabezpieczeń, monitorowanie konfiguracji zabezpieczeń i wyświetlanie alertów zabezpieczeń.

### <a name="security-policies"></a>Zasady zabezpieczeń

Można zdefiniować zasady Azure subskrypcje i grup zasobów według wymagania dotyczące zabezpieczeń firmy. Można także dostosować ich typów aplikacji, którego używasz lub poufność danych w każdej subskrypcji. Na przykład zasobów używanych do rozwoju lub testowania może być wymagania dotyczące zabezpieczeń innym niż używane do produkcji aplikacji. Ponadto aplikacje z regulowanym danych, takich jak osoby mogą wymagać wyższy poziom zabezpieczeń.

> [AZURE.NOTE] Aby zmodyfikować zasady zabezpieczeń na poziomie subskrypcji lub grupa zasobów, musi być właścicielem subskrypcji lub współautora do niego.

Na karta **Centrum zabezpieczeń** wybierz kafelku **zasad** lista subskrypcje i grup zasobów.   

![Karta Centrum zabezpieczeń][2]

Na karta **Zasady zabezpieczeń** Wybierz subskrypcję, aby wyświetlić szczegóły zasady.

![Subskrypcja karta Zasady zabezpieczeń][3]

**Zbieranie danych** (patrz powyżej) umożliwia zbieranie danych zasady zabezpieczeń. Włączenie umożliwia:

- Dzienny skanowanie wszystkie obsługiwane maszyn wirtualnych monitorowanie zabezpieczeń i zalecenia.
- Kolekcja zdarzeń zabezpieczeń dla wykrywania analizy i zagrożenia.

**Wybierz konto miejsca do magazynowania w rozbiciu na regiony** (patrz powyżej) pozwala wybrać, dla każdego regionu, w którym masz maszyn wirtualnych uruchomiony, konto miejsca do magazynowania, w której są przechowywane dane zebrane z tych maszyn wirtualnych. Jeśli nie zostanie wybrana konto miejsca do magazynowania dla każdego regionu, zostanie on utworzony dla Ciebie. Dane, które są zbierane jest logicznie odizolowane od innych klientów danych ze względów bezpieczeństwa.

> [AZURE.NOTE] Zbieranie danych i wybierając pozycję konto miejsca do magazynowania w rozbiciu na regiony jest skonfigurowane na poziomie subskrypcji.

Wybierz **zasad zapobiegania** (patrz powyżej), aby otworzyć karta **zasad zapobiegania** . **Pokazywanie zalecenia dotyczące** pozwala wybrać kontroli zabezpieczeń, które chcesz monitorować i zaleca się w zależności od potrzeb zabezpieczeń Zasoby w ramach subskrypcji.

Następnie wybierz grupę zasobów, aby wyświetlić szczegóły zasady.

![Grupa zasobów karta Zasady zabezpieczeń][4]

**Dziedziczenie** (patrz powyżej) pozwala na zdefiniowanie grupa zasobów jako:

- Dziedziczona i (ustawienie domyślne) dla danej grupy zasobów co oznacza wszystkie zasady zabezpieczeń są dziedziczone z poziomu subskrypcji.
- Unikatowe co oznacza, grupy zasobów uzyskuje zasady zabezpieczeń niestandardowe. Należy wprowadzić zmiany w obszarze **Pokaż zalecenia dotyczące**.

> [AZURE.NOTE] W przypadku konfliktu między zasadami na poziomie subskrypcji i poziomu zasad grupy zasobów, zasady poziomu grupy zasobów ma pierwszeństwo.

### <a name="security-recommendations"></a>Zalecenia dotyczące zabezpieczeń

 Centrum zabezpieczeń analizuje stan zabezpieczeń Azure zasobów do identyfikowania słabych zabezpieczeń. Lista zalecenia przeprowadzi Cię przez proces konfigurowania potrzebnych formantów. Przykłady:

- Obsługa administracyjna ochrony przed złośliwym oprogramowaniem do identyfikowania i usuwania złośliwego oprogramowania
- Konfigurowanie reguł kontrola ruchu do maszyn wirtualnych oraz grup zabezpieczeń sieci
- Inicjowania obsługi administracyjnej zapory aplikacji sieci web, aby pomóc Ci chronić przed atakami tego docelowej aplikacji sieci web
- Wdrażanie brakujących aktualizacji systemu
- Adresowanie konfiguracji systemu operacyjnego, które nie są zgodne zalecane plany bazowe

Kliknij Kafelek **zalecenia** dla listy zaleceń. Kliknij każdy rekomendacji, aby wyświetlić dodatkowe informacje lub wykonać czynność, aby rozwiązać ten problem.

![Zalecenia dotyczące zabezpieczeń w Centrum zabezpieczeń Azure][5]

### <a name="resource-health"></a>Kondycja zasobów

Kafelków **prawidłowość zabezpieczeń zasobów** zawiera ogólny postawie zabezpieczeń środowiska według typów zasobów, w tym maszyn wirtualnych, aplikacji sieci web i inne zasoby.   

Wybierz typ zasobu na kafelku **prawidłowość zabezpieczeń zasobów** , aby wyświetlić więcej informacji, w tym listę dowolnego słabych zabezpieczeń, które zostały określone. (**Maszyn wirtualnych** jest zaznaczona w poniższym przykładzie).

![Zasoby dotyczące kondycji kafelków][6]

### <a name="security-alerts"></a>Alerty zabezpieczeń

 Centrum zabezpieczeń automatycznie gromadzi, analizuje i integruje dziennika danych z usługi Azure zasobów, sieci i rozwiązań partnerów, takich jak programy ochrony przed złośliwym oprogramowaniem i zapory. Gdy zostaną wykryte zagrożenia, zostanie utworzona alert zabezpieczeń. Przykłady wykrywania:

- Złamany maszyn wirtualnych komunikowania się z znanego złośliwego adresów IP
- Zaawansowane złośliwe oprogramowanie wykryte przy użyciu raportowania błędów systemu Windows
- Atakami przed maszyn wirtualnych
- Alerty zabezpieczeń z programów zintegrowane ochrony przed złośliwym oprogramowaniem i zapory

Kliknięcie kafelka **alertów zabezpieczeń** Wyświetla listę priorytetów alertów.

![Alerty zabezpieczeń][7]

Wybieranie alertu zawiera więcej informacji na temat atakiem i sugestie dotyczące korygowania go.

![Szczegóły alertu zabezpieczeń][8]

### <a name="partner-solutions"></a>Rozwiązań partnerów

Fragment **rozwiązań partnerów** umożliwia monitor przejrzenie kondycja usługi rozwiązań partnerów zintegrowany z subskrypcji usługi Azure. Centrum zabezpieczeń są wyświetlane alerty pochodzące z rozwiązania.

Wybierz Kafelek **rozwiązań partnerów** . Zostanie wyświetlona karta, zawierające listę wszystkich rozwiązań partnerów połączonego.

![Rozwiązań partnerów][9]

## <a name="get-started"></a>Rozpoczynanie pracy
Aby rozpocząć pracę z Centrum zabezpieczeń, należy subskrypcję usługi Microsoft Azure. Centrum zabezpieczeń jest włączone z subskrypcją usługi Azure. Jeśli nie masz subskrypcji, można Załóż [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).

 Masz dostęp do Centrum zabezpieczeń z [Azure portal](https://azure.microsoft.com/features/azure-portal/). Zapoznaj się z [dokumentacją portalu](https://azure.microsoft.com/documentation/services/azure-portal/) Aby dowiedzieć się więcej.

[Wprowadzenie do Centrum zabezpieczeń Azure](security-center-get-started.md) szybko prowadzi przez monitorowanie zabezpieczeń i zarządzania zasadami składników Centrum zabezpieczeń.

## <a name="see-also"></a>Zobacz też
W tym dokumencie zostały wprowadzone do Centrum zabezpieczeń, klucza możliwości i jak rozpocząć pracę. Aby uzyskać więcej informacji, zobacz następujące artykuły:

- [Ustawianie zasad zabezpieczeń w Centrum zabezpieczeń Azure](security-center-policies.md) — Dowiedz się, jak skonfigurować zasady zabezpieczeń dla usługi Azure subskrypcje i grup zasobów.
- [Zarządzanie zalecenia dotyczące zabezpieczeń w Centrum zabezpieczeń Azure](security-center-recommendations.md) — Dowiedz się, jak zalecenia ułatwiają ochrony Azure zasobów.
- [Prawidłowość zabezpieczeń monitorowania w Centrum zabezpieczeń Azure](security-center-monitoring.md) — Dowiedz się, jak monitorowanie kondycji Azure zasobów.
- [Zarządzanie notatkami i odpowiadanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md) — Dowiedz się, jak zarządzać i odpowiadanie na alerty zabezpieczeń.
- [Monitorowanie rozwiązań partnerów z Centrum zabezpieczeń Azure](security-center-partner-solutions.md) — Dowiedz się, jak można monitorować stan kondycji rozwiązań partnerów.
- [Często zadawane pytania dotyczące Azure zabezpieczeń Centrum](security-center-faq.md) — znajdowanie często zadawane pytania dotyczące korzystania z usługi.
- [Blog dotyczący zabezpieczeń Azure](http://blogs.msdn.com/b/azuresecurity/) — uzyskiwanie najnowsze Azure zabezpieczeń i informacji.

<!--Image references-->
[1]: ./media/security-center-intro/security-tile.png
[2]: ./media/security-center-intro/security-center.png
[3]: ./media/security-center-intro/security-policy.png
[4]: ./media/security-center-intro/security-policy-blade.png
[5]: ./media/security-center-intro/recommendations.png
[6]: ./media/security-center-intro/resources-health.png
[7]: ./media/security-center-intro/security-alert.png
[8]: ./media/security-center-intro/security-alert-detail.png
[9]: ./media/security-center-intro/partner-solutions.png
