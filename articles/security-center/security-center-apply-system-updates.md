<properties
   pageTitle="Stosowanie aktualizacji systemu w Centrum zabezpieczeń Azure | Microsoft Azure"
   description="Ten dokument zawiera jak wdrażać zalecenia Centrum zabezpieczeń Azure **Stosowanie aktualizacji systemu** i **Uruchom ponownie po uaktualnieniu systemu**."
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
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="apply-system-updates-in-azure-security-center"></a>Stosowanie aktualizacji systemu w Centrum zabezpieczeń Azure

Centrum zabezpieczeń Azure codziennie monitoruje Windows i Linux wirtualnych maszyn brakujących aktualizacji systemu operacyjnego. Centrum zabezpieczeń pobiera listę dostępnych zabezpieczeń i aktualizacje krytyczne z witryny Windows Update lub Windows Server Update Services (WSUS), w zależności od tego, która usługa jest skonfigurowana na maszyn wirtualnych systemu Windows.  Centrum zabezpieczeń sprawdza również najnowsze aktualizacje w systemach Linux. Jeśli do maszyn wirtualnych brakuje aktualizacji systemu, Centrum zabezpieczeń Zalecamy stosowanie aktualizacji systemu

> [AZURE.NOTE] Ten dokument wprowadza usługę przy użyciu Przykładowe wdrożenie.  Nie jest to przewodnik krok po kroku.

## <a name="implement-the-recommendation"></a>Wykonania zalecenia

1. W karta **zalecenia** wybierz **aktualizacje systemu Zastosuj**.
![Stosowanie aktualizacji systemu][1]

2. Karta **aktualizacje systemu Zastosuj** zostanie otwarty wyświetlona lista maszyny wirtualne brakujących aktualizacji systemu. Wybierz pozycję maszyny.
![Wybierz pozycję maszyny][2]

3. Karta zostanie wyświetlona, zawierające listę brakujących aktualizacji dla tego maszyn wirtualnych. Wybierz pozycję aktualizacji systemu. W tym przykładzie po zaznaczeniu KB3156016.
![Brakujące aktualizacje zabezpieczeń][3]

4. Postępuj zgodnie z instrukcjami karta **Aktualizacji zabezpieczeń** w celu zastosowania brakujących aktualizacji.
![Aktualizacji zabezpieczeń][4]

## <a name="reboot-after-system-updates"></a>Uruchom ponownie po uaktualnieniu systemu

5. Wróć do karta **zalecenia** . Nowy wpis został wygenerowany po zastosowaniu aktualizacji systemu, o nazwie **Uruchom ponownie po uaktualnieniu systemu**. Ten wpis informuje o konieczności Uruchom ponownie maszyn wirtualnych, aby ukończyć proces stosowania aktualizacji systemu.
![Uruchom ponownie po uaktualnieniu systemu][5]

6. Wybierz pozycję **Uruchom ponownie po uaktualnieniu systemu**. Spowoduje to otwarcie **Oczekiwanie przeprowadzenie aktualizacji systemu jest ponowne uruchomienie** karta wyświetlanie listy maszyny wirtualne, które należy ponownie uruchomić, aby ukończyć proces aktualizacji systemu Zastosuj.
![Oczekiwanie na ponowne uruchomienie][6]

Uruchom ponownie maszyn wirtualnych z platformy Azure, aby ukończyć proces.

## <a name="see-also"></a>Zobacz też

Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

- [Ustawianie zasad zabezpieczeń w Centrum zabezpieczeń Azure](security-center-policies.md) — Dowiedz się, jak skonfigurować zasady zabezpieczeń dla usługi Azure subskrypcje i grup zasobów.
- [Zarządzanie zalecenia dotyczące zabezpieczeń w Centrum zabezpieczeń Azure](security-center-recommendations.md) — Dowiedz się, jak zalecenia ułatwiają ochrony Azure zasobów.
- [Monitorowanie kondycji zabezpieczeń w Centrum zabezpieczeń Azure](security-center-monitoring.md) — Dowiedz się, jak monitorowanie kondycji Azure zasobów.
- [Zarządzanie notatkami i odpowiadanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md) — Dowiedz się, jak zarządzać i odpowiadanie na alerty zabezpieczeń.
- [Monitorowanie rozwiązań partnerów z Centrum zabezpieczeń Azure](security-center-partner-solutions.md) — Dowiedz się, jak można monitorować stan kondycji rozwiązań partnerów.
- [Azure zabezpieczeń Centrum — często zadawane pytania](security-center-faq.md) — znajdowanie często zadawane pytania dotyczące korzystania z usługi.
- [Blog dotyczący zabezpieczeń azure](http://blogs.msdn.com/b/azuresecurity/) — blog Znajdź wpisy o Azure zabezpieczenia i zgodność.

<!--Image references-->
[1]: ./media/security-center-apply-system-updates/recommendation.png
[2]:./media/security-center-apply-system-updates/select-vm.png
[3]: ./media/security-center-apply-system-updates/missing-security-updates.png
[4]: ./media/security-center-apply-system-updates/security-update.png
[5]: ./media/security-center-apply-system-updates/reboot-after-system-updates.png
[6]: ./media/security-center-apply-system-updates/restart-pending.png
