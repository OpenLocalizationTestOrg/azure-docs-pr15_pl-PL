<properties
   pageTitle="Obsługa zdarzenia zabezpieczeń w Centrum zabezpieczeń Azure | Microsoft Azure"
   description="Ten dokument pomaga za pomocą Centrum zabezpieczeń Azure możliwości obsługi zdarzeń."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="handling-security-incident-in-azure-security-center"></a>Obsługa zdarzenia zabezpieczeń w Centrum zabezpieczeń Azure 
Triaging i badania alertów zabezpieczeń może zająć dużo czasu dla nawet najbardziej specjalistycznej analityków zabezpieczeń, a dla wielu trudno nawet wiedzieć, gdzie zacząć. Za pomocą [analizy](security-center-detection-capabilities.md) do łączenie informacji między odrębnych [alertów zabezpieczeń](security-center-managing-and-responding-alerts.md), Centrum zabezpieczeń może udostępnić pojedynczego widoku kampanii atakiem i wszystkie powiązane alerty — można szybko zrozumieć działania tej zajęła i zasobów zostały wpływ.

Jak używać możliwości alertów zabezpieczeń w Centrum zabezpieczeń pomaga obsługi zdarzeń omówione w tym dokumencie.


## <a name="what-is-a-security-incident"></a>Co to jest zdarzeniem bezpieczeństwa?

W Centrum zabezpieczeń zdarzeniem bezpieczeństwa jest agregacją wszystkie alerty dla zasobu, które są dostosowane deseniami [skasować łańcucha](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/) . Zdarzenia są wyświetlane w kafelku [Alertów zabezpieczeń](security-center-managing-and-responding-alerts.md) i karta. Zdarzenia spowoduje wyświetlenie listy powiązanych alertów, dzięki czemu można uzyskać więcej informacji na temat każdego wystąpienia.

## <a name="managing-security-incidents"></a>Zarządzanie bezpieczeństwem

Zapoznanie się z bieżącym zdarzeń sprawdzając kafelków alerty zabezpieczeń. Uzyskiwanie dostępu do portalu Azure i postępuj zgodnie z instrukcjami poniżej, aby wyświetlić więcej szczegółów dotyczących poszczególnych zdarzeniem bezpieczeństwa:

1. Na pulpicie nawigacyjnym Centrum zabezpieczeń zostanie wyświetlona kafelków **alertów zabezpieczeń** .

    ![Alerty zabezpieczeń kafelka w Centrum zabezpieczeń](./media/security-center-incident/security-center-incident-fig1.png)

2.  Kliknij na tym kafelku, aby rozwinąć ją, a jeśli zostanie wykryta zdarzeniem bezpieczeństwa, pojawi się w obszarze wykresu alerty zabezpieczeń, tak jak pokazano poniżej:

    ![Zdarzeniem bezpieczeństwa](./media/security-center-incident/security-center-incident-fig2.png)

3.  Zwróć uwagę, że opis zdarzenia zabezpieczeń ma inną ikonę w porównaniu z innych alertów. Kliknij go, aby wyświetlić więcej szczegółów dotyczących tego zdarzenia.

    ![Zdarzeniem bezpieczeństwa](./media/security-center-incident/security-center-incident-fig3.png)

4.  Na **wypadek** karta pojawi się bardziej szczegóły dotyczące tego zdarzenia zabezpieczeń zawiera pełny opis, jego ważności (czyli w tym przypadku wysoki), jego bieżącym stanie (w tym przypadku nadal jest *aktywne*, co oznacza użytkownika nie zrecenzowania aby *odrzucić* go — można to zrobić po kliknięciu prawym przyciskiem myszy zdarzenie w karta **alertów zabezpieczeń** ) , zaatakowanych zasobu (w tym przypadku *VM1*), działań naprawczych kroki dla zdarzenia, a w dolnym okienku masz alertów, które zostały uwzględnione w tym zdarzeniem. Jeśli chcesz uzyskać więcej informacji na temat poszczególnych alertów, wystarczy kliknąć go i karta innego zostanie otwarte, tak jak pokazano poniżej:

    ![Zdarzeniem bezpieczeństwa](./media/security-center-incident/security-center-incident-fig4.png)

Informacje na ten karta zależy od alert. Aby uzyskać więcej informacji o zarządzaniu tych alertów, przeczytaj [Zarządzanie i odpowiadanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md) . Niektóre ważne zagadnienia dotyczące tej funkcji:

- Nowy filtr umożliwia dostosowywanie widoku tylko zdarzenia i/lub tylko alerty. 
- Tego samego ostrzeżenia może istnieć w ramach zdarzenia (jeśli dotyczy), a także mają być wyświetlane jako alert autonomicznego. 
- Odrzucanie zdarzenia nie będzie Odrzuć powiązanych alertów.

## <a name="see-also"></a>Zobacz też

W tym dokumencie wiesz, jak używać funkcji zdarzenia zabezpieczeń w Centrum zabezpieczeń. Aby dowiedzieć się więcej na temat Centrum zabezpieczeń, zobacz następujące artykuły:

- [Zarządzanie i odpowiadanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md)
- [Możliwości wykrywania Centrum zabezpieczeń Azure](security-center-detection-capabilities.md)
- [Planowanie Centrum zabezpieczeń Azure i przewodnik](security-center-planning-and-operations-guide.md)
- [Zarządzanie i odpowiadanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md)
- [Azure zabezpieczeń Centrum — często zadawane pytania](security-center-faq.md)— znajdowanie często zadawane pytania dotyczące korzystania z usługi.
- [Blog dotyczący zabezpieczeń azure](http://blogs.msdn.com/b/azuresecurity/)— blog Znajdź wpisy o Azure zabezpieczenia i zgodność.
