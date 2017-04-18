<properties
   pageTitle="Przewodnik Szybki start Centrum zabezpieczeń Azure | Microsoft Azure"
   description="Ten artykuł pozwoli Ci szybko Rozpocznij pracę z Centrum zabezpieczeń Azure przeprowadzi Cię składniki zarządzania monitorowania i zasad zabezpieczeń i łącząc możesz do następnych kroków."
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
   ms.date="10/28/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-quick-start-guide"></a>Przewodnik Szybki start Centrum zabezpieczeń Azure

Ten artykuł ułatwia szybkie rozpoczęcie pracy z Centrum zabezpieczeń Azure monitorowania i zasad zarządzania składniki zabezpieczeń Centrum zabezpieczeń przeprowadzi Cię przez.

> [AZURE.NOTE] W tym artykule przedstawiono usługi przy użyciu Przykładowe wdrożenie. Ten artykuł nie dotyczy przewodnik krok po kroku.

## <a name="prerequisites"></a>Wymagania wstępne

Aby rozpocząć pracę z Centrum zabezpieczeń, musi mieć subskrypcję usługi Microsoft Azure. Jeśli nie masz subskrypcji, możesz zalogować [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial/).

Bezpłatne warstwa Centrum zabezpieczeń jest włączany automatycznie w ramach Twojej subskrypcji, a także wgląd stanu zabezpieczeń Azure zasobów. Umożliwia zarządzanie zasadami zabezpieczeń podstawowe, zalecenia dotyczące zabezpieczeń i integracja z zabezpieczeń produktów i usług od partnerów Azure.

Masz dostęp do Centrum zabezpieczeń z [Azure portal](https://azure.microsoft.com/features/azure-portal/). Aby dowiedzieć się więcej na temat Azure portal, zapoznaj się z [dokumentacją portalu](https://azure.microsoft.com/documentation/services/azure-portal/).

## <a name="data-collection"></a>Zbieranie danych

Centrum zabezpieczeń zbiera dane z usługi wirtualnych maszyn oceny stanu zabezpieczeń, podaj zalecenia dotyczące zabezpieczeń i powiadomienia na zagrożenia. Po raz pierwszy uzyskujesz dostęp do Centrum zabezpieczeń, zbierania danych jest włączone wszystkie maszyny wirtualne w ramach subskrypcji. Zbieranie danych jest zalecane, ale można zrezygnować przez wyłączenie zbieranie danych w Centrum zabezpieczeń zasad.

Poniżej opisano sposób uzyskiwania dostępu i korzystania z składników Centrum zabezpieczeń. W tych krokach pokażemy, jak wyłączyć zbierania danych, jeśli chcesz zrezygnować z aplikacji.

## <a name="access-security-center"></a>Centrum zabezpieczeń programu Access

W portalu wykonaj poniższe czynności, aby uzyskać dostęp do Centrum zabezpieczeń:

1. W menu **Microsoft Azure** wybierz **Centrum zabezpieczeń**.
![Azure menu][1]

2. Jeśli uzyskujesz dostęp do Centrum zabezpieczeń po raz pierwszy, zostanie wyświetlona karta **Zapraszamy** . Wybierz pozycję **Tak! Chcę, aby Centrum zabezpieczeń Azure uruchamianie** aby otworzyć karta **Centrum zabezpieczeń** i umożliwiający zbieranie danych.
![Ekran powitalny][10]

3. Po uruchomić Centrum zabezpieczeń z karta Zapraszamy lub wybierz z menu Microsoft Azure Centrum zabezpieczeń, zostanie wyświetlona karta **Centrum zabezpieczeń** . W celu ułatwienia dostępu do karta **Centrum zabezpieczeń** w przyszłości, wybierz opcję **Karta numeru Pin do pulpitu nawigacyjnego** (w prawym górnym rogu).
![Karta numeru PIN do opcji pulpitu nawigacyjnego][2]

## <a name="use-security-center"></a>Korzystanie z Centrum zabezpieczeń

Można skonfigurować zasady zabezpieczeń dla Twojej subskrypcji Azure i grup zasobów. Załóżmy Konfigurowanie zasad zabezpieczeń dla subskrypcji:

1. Na karta **Centrum zabezpieczeń** wybierz Kafelek **zasad** .
![Zasady zabezpieczeń][3]

2. Na karta **Zasady zabezpieczeń — opracowanie zasad dla każdej grupy subskrypcji lub zasób** Wybierz subskrypcję.
3. Karta **Zasady zabezpieczeń** **zbierania danych** jest włączone automatyczne zbieranie dzienników. Rozszerzenie monitorowania jest włączona na wszystkie bieżące oraz nowe maszyny wirtualne w subskrypcji. (Można zaprzestać korzystania z zbierania danych przez ustawienie **zbierania danych** , aby **wyłączyć**, ale zapobiega to Centrum zabezpieczeń umożliwiając alertów zabezpieczeń i zalecenia).
4. Na karta **zasad zabezpieczeń** zaznacz opcję **Wybierz konto miejsca do magazynowania w rozbiciu na regiony**. Dla każdego regionu, w którym masz maszyny wirtualne uruchomiony możesz wybrać konta miejsca do magazynowania, w której są przechowywane dane zebrane z tych maszyny wirtualne. Jeśli nie zostanie wybrana konto miejsca do magazynowania dla każdego regionu, zostanie utworzony dla Ciebie. Dane, które są zbierane jest logicznie odizolowane od innych klientów danych ze względów bezpieczeństwa.

     > [AZURE.NOTE] Zaleca się włączenie zbierania danych, a następnie wybierz konto miejsca do magazynowania na poziomie subskrypcji najpierw. Można ustawić zasady zabezpieczeń na poziomie grupy zasobów i poziom subskrypcji Azure, ale konfiguracja zbierania danych i konto przestrzeni dyskowej następuje tylko na poziomie subskrypcji.

5. Na karta **Zasady zabezpieczeń** wybierz **zasad zapobiegania**. Spowoduje to otwarcie karta **zasad zapobiegania** .
![Zasad zapobiegania][4]

6. Na karta **zasad zapobiegania** włączyć zalecenia, które mają być wyświetlane jako część zasady dotyczące zabezpieczeń. Przykłady:

 - Konfigurowanie **aktualizacji systemu** **w** skanuje wszystkie obsługiwane maszyn wirtualnych brakujących aktualizacji systemu operacyjnego.
 - Ustawianie **luk OS** **na** skanuje wszystkie obsługiwane maszyn wirtualnych do identyfikowania dowolnej konfiguracji systemu operacyjnego, które może być bardziej podatne na ataki maszyny wirtualnej.

### <a name="view-recommendations"></a>Zalecenia dotyczące widoku

1. Wróć do karta **Centrum zabezpieczeń** i zaznacz opcję Podziel **zalecenia** . Centrum zabezpieczeń okresowo analizuje stan zabezpieczeń Azure zasobów. Gdy Centrum zabezpieczeń identyfikuje słabych zabezpieczeń, zawiera zalecenia w karta **zalecenia** .
![Zalecenia w Centrum zabezpieczeń Azure][5]

2.  Wybierz pozycję zalecenie dotyczące karta **zalecenia** , aby wyświetlić więcej informacji i/lub wykonywanie akcji, aby rozwiązać ten problem.

### <a name="view-the-health-and-security-state-of-your-resources"></a>Sprawdzanie stanu kondycji i zabezpieczenia zasobów

1.  Powróć do **Centrum zabezpieczeń** karta. Fragment **prawidłowość zabezpieczeń zasobów** zawiera wskaźniki stanu zabezpieczeń dla maszyn wirtualnych, sieci, danych i aplikacji.
2.  Wybierz pozycję **maszyn wirtualnych** , aby wyświetlić więcej informacji. Karta **maszyn wirtualnych** otwiera i wyświetla podsumowanie stanu programów ochrony przed złośliwym oprogramowaniem, aktualizacje systemu, ponowne uruchamianie i luk OS z pośrednictwem usługi SMS.
![Zasoby kafelka kondycji w Centrum zabezpieczeń Azure][6]

3.  Wybierz pozycję rekomendacji w obszarze **Zalecenia maszyn wirtualnych** , aby wyświetlić więcej informacji i/lub wykonywanie akcji skonfigurowanie niezbędnych kontroli.
4.  Wybierz pozycję maszyn wirtualnych w **środowisku maszyn wirtualnych systemu** , aby wyświetlić dodatkowe informacje.

### <a name="view-security-alerts"></a>Wyświetl alerty zabezpieczeń

1.  Wróć do **Centrum zabezpieczeń** karta i zaznacz opcję Podziel **alertów zabezpieczeń** . Karta **alertów zabezpieczeń** zostanie otwarty i zostanie wyświetlona lista alertów. Analiza Centrum zabezpieczeń dzienniki zabezpieczeń i aktywności sieci generuje tych alertów. Alerty z rozwiązań partnerów zintegrowane są uwzględniane.
![Alerty zabezpieczeń w Centrum zabezpieczeń Azure][7]

    > [AZURE.NOTE] Alerty zabezpieczeń są dostępne tylko po włączeniu warstwie standardowy Centrum zabezpieczeń. 90 dni bezpłatną wersję próbną standardowej warstwy jest dostępna. Aby uzyskać informacje dotyczące uzyskiwania standardowa warstwa, zobacz [Następne kroki](#next-steps) .

2.  Wybierz pozycję alert, aby wyświetlić dodatkowe informacje. W tym przykładzie po zaznaczeniu **wykryte zmodyfikowano system binarny**. Spowoduje to otwarcie karty, które zapewniają dodatkowe informacje dotyczące alertu.
![Szczegóły alertu zabezpieczeń w Centrum zabezpieczeń Azure][8]

### <a name="view-the-health-of-your-partner-solutions"></a>Przeglądanie informacji o kondycji rozwiązań partnerów

1. Powróć do **Centrum zabezpieczeń** karta. Fragment **rozwiązań partnerów** umożliwia monitorowanie w skrócie, stan kondycji rozwiązań partnerów zintegrowany z subskrypcji usługi Azure.
2. Wybierz Kafelek **rozwiązań partnerów** . Karta zostanie otwarty i zostanie wyświetlona lista z rozwiązań partnerów połączony z Centrum zabezpieczeń.
![Rozwiązań partnerów][9]

3. Wybierz rozwiązanie partnera. W tym przykładzie po zaznaczeniu rozwiązanie **F5 WAF** .  Karta zostanie otwarty i przedstawiono stan rozwiązania partnerów i skojarzonych z nimi zasobów tego rozwiązania. Wybierz pozycję **konsoli rozwiązanie** , aby otworzyć środowisko zarządzania partnerów dla tego rozwiązania.

## <a name="next-steps"></a>Następne kroki
W tym artykule wprowadzone do monitorowania i zasad zarządzania składniki zabezpieczeń Centrum zabezpieczeń. Teraz, gdy użytkownicy zaznajomieni z Centrum zabezpieczeń, spróbuj wykonać następujące czynności:

- Konfigurowanie zasad zabezpieczeń dla subskrypcji Azure. Aby uzyskać więcej informacji, zobacz [Ustawienia zasad zabezpieczeń w Centrum zabezpieczeń Azure](security-center-policies.md).
- Użyj zalecenia w Centrum zabezpieczeń ułatwiających ochronę Azure zasobów. Aby uzyskać więcej informacji, zobacz [Zarządzanie zalecenia dotyczące zabezpieczeń w Centrum zabezpieczeń Azure](security-center-recommendations.md).
- Przeglądanie i zarządzanie bieżącego alerty zabezpieczeń. Aby uzyskać więcej informacji, zobacz [Zarządzanie i odpowiadanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md).
- Dowiedz się więcej o [Zaawansowane funkcje wykrywania zagrożenie](security-center-detection-capabilities.md) które są dostarczane z [poziomu standardowej](security-center-pricing.md) Centrum zabezpieczeń. 90 dni bezpłatną wersję próbną standardowej warstwy jest dostępna.
- Jeśli masz pytania dotyczące korzystania z Centrum zabezpieczeń, zobacz [Często zadawane pytania dotyczące Azure zabezpieczeń Centrum](security-center-faq.md).

<!--Image references-->
[1]: ./media/security-center-get-started/azure-menu.png
[2]: ./media/security-center-get-started/security-center-pin.png
[3]: ./media/security-center-get-started/security-policy.png
[4]: ./media/security-center-get-started/prevention-policy.png
[5]: ./media/security-center-get-started/recommendations.png
[6]: ./media/security-center-get-started/resources-health.png
[7]: ./media/security-center-get-started/security-alert.png
[8]: ./media/security-center-get-started/security-alert-detail.png
[9]: ./media/security-center-get-started/partner-solutions.png
[10]: ./media/security-center-get-started/welcome.png
