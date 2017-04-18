<properties
   pageTitle="Należy podać dane kontaktowe zabezpieczeń w Centrum zabezpieczeń Azure | Microsoft Azure"
   description="Ten dokument pokazano, jak o podanie informacji o kontakcie zabezpieczeń w Centrum zabezpieczeń Azure."
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
   ms.date="10/17/2016"
   ms.author="terrylan"/>

# <a name="provide-security-contact-details-in-azure-security-center"></a>Należy podać dane kontaktowe zabezpieczeń w Centrum zabezpieczeń Azure

Centrum zabezpieczeń Azure zaleca, podać dane kontaktowe zabezpieczeń dla subskrypcji Azure, jeśli jeszcze tego nie zrobiono. Te informacje będą używane przez firmę Microsoft się z Tobą skontaktować, jeśli Centrum odpowiedzi zabezpieczeń firmy Microsoft (gdy) wykryje, że dane o klientach uzyskano dostęp przez stronę prawem lub nieautoryzowane. MSRC wykonuje monitorowanie zabezpieczeń wybierz Azure sieci i infrastruktury i odbiera zagrożenie analizy i abuse skargi od osób trzecich.

Powiadomienie e-mail jest wysyłana na pierwsze wystąpienie dzienny alertu i tylko dla alertów o wysokiej ważności. Preferencje poczty e-mail można skonfigurować tylko dla zasad subskrypcji. Grupy zasobów w ramach subskrypcji będzie dziedziczyć te ustawienia.

> [AZURE.NOTE] Ten dokument wprowadza usługę przy użyciu Przykładowe wdrożenie.  Nie jest to przewodnik krok po kroku.

## <a name="implement-the-recommendation"></a>Wykonania zalecenia

1. W karta **zalecenia** wybierz **zabezpieczeń zawiera szczegóły kontaktu**.
![Podaj zabezpieczeń kontaktu][1]

2. Spowoduje to otwarcie karta **zabezpieczeń zawiera szczegóły kontaktu**. Wybierz subskrypcję Azure o podanie informacji kontaktowych na.
![Podaj informacje kontaktowe zabezpieczeń][2]

3. Zostanie otwarta druga karta **należy podać dane kontaktowe zabezpieczeń** .

  - Wprowadź adres e-mail kontaktu zabezpieczeń lub adresy, rozdzielając je przecinkami. Nie istnieje limit liczby adresów e-mail, które można wprowadzić.
  - Wprowadź jeden zabezpieczeń kontaktów międzynarodowy numer telefonu.
  - Aby otrzymywać wiadomości e-mail o wysokiej ważności alertów, włącz opcję **Wyślij do mnie wiadomości dotyczące alertów**.
  - W przyszłości będą mieć możliwość wysyłania powiadomień e-mail właścicielom subskrypcji. Ta opcja jest obecnie wyszarzona.
  - Kliknij przycisk **OK** dotyczyć informacje kontaktowe zabezpieczeń subskrypcji.

## <a name="see-also"></a>Zobacz też

Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

- [Ustawianie zasad zabezpieczeń w Centrum zabezpieczeń Azure](security-center-policies.md) — Dowiedz się, jak skonfigurować zasady zabezpieczeń dla usługi Azure subskrypcje i grup zasobów.
- [Zarządzanie zalecenia dotyczące zabezpieczeń w Centrum zabezpieczeń Azure](security-center-recommendations.md) — Dowiedz się, jak zalecenia ułatwiają ochrony Azure zasobów.
- [Monitorowanie kondycji zabezpieczeń w Centrum zabezpieczeń Azure](security-center-monitoring.md) — Dowiedz się, jak monitorowanie kondycji Azure zasobów.
- [Zarządzanie notatkami i odpowiadanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md) — Dowiedz się, jak zarządzać i odpowiadanie na alerty zabezpieczeń.
- [Monitorowanie rozwiązań partnerów z Centrum zabezpieczeń Azure](security-center-partner-solutions.md) — Dowiedz się, jak można monitorować stan kondycji rozwiązań partnerów.
- [Azure zabezpieczeń Centrum — często zadawane pytania](security-center-faq.md) — znajdowanie często zadawane pytania dotyczące korzystania z usługi.
- [Blog dotyczący zabezpieczeń azure](http://blogs.msdn.com/b/azuresecurity/) — uzyskiwanie najnowsze Azure zabezpieczeń i informacji.

<!--Image references-->
[1]: ./media/security-center-provide-security-contacts/provide-contacts.png
[2]:./media/security-center-provide-security-contacts/provide-contact-details.png
