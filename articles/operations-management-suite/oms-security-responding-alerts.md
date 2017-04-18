<properties
   pageTitle="Monitorowanie i reagowanie na alerty zabezpieczeń w operacji zarządzania pakietu zabezpieczeń i rozwiązanie inspekcji | Microsoft Azure"
   description="Ten dokument pomaga za pomocą opcji analizy zagrożenia dostępne w zabezpieczeń usługi OMS i inspekcji można monitorować i odpowiadanie na alerty zabezpieczeń."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.topic="article" 
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="monitoring-and-responding-to-security-alerts-in-operations-management-suite-security-and-audit-solution"></a>Monitorowanie i reagowanie na alerty zabezpieczeń w operacje zarządzania pakietu zabezpieczeń i rozwiązanie inspekcji

Ten dokument pomaga za pomocą opcji analizy zagrożenia dostępne w zabezpieczeń usługi OMS i inspekcji można monitorować i odpowiadanie na alerty zabezpieczeń.

## <a name="what-is-oms"></a>Co to są usługi OMS?

Pakiet Microsoft operacje zarządzania usługi (OMS) jest podstawą rozwiązanie do zarządzania IT, który ułatwia zarządzanie i ochrona sieci lokalnej i w chmurze infrastruktury chmury firmy Microsoft. Aby uzyskać więcej informacji na temat usługi OMS przeczytaj artykuł [Pakiet administracyjny operacji](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="threat-intelligence"></a>Zagrożenia analizy

W środowisku przedsiębiorstwa, której użytkownicy mają szeroki dostęp do sieci i łączenie z danymi firmy za pomocą różnych urządzeń konieczne jest można aktywne monitorowanie zasobów i szybko odpowiedzieć z bezpieczeństwem. Jest to szczególnie istotne z perspektywy cyklu życia zabezpieczeń, ponieważ niektóre cybersecurity, których nie może podnieść zagrożeń alerty lub podejrzane działania, które cechuje kontroli technicznych tradycyjnych zabezpieczeń. 

Za pomocą opcji **Analizy zagrożenia** dostępne w zabezpieczeń usługi OMS i inspekcji, pozwala administratorom systemów informatycznych identyfikowanie zagrożeniami środowisku, na przykład, ustalić, czy na danym komputerze znajduje się [botnet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection). Komputery może stać się węzłów na botnet ataki nielegalnie instalowania złośliwego oprogramowania, które ukryciu łączy ten komputer polecenia i kontroli. Można też wskazać potencjalne zagrożenia pochodzące z kanałów podziemnych komunikacji, takich jak [darknet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection_honeypots_darkents). 

Aby utworzyć ten analizy zagrożenia, zabezpieczenia usługi OMS i inspekcji Użyj dane pochodzące z wielu źródeł w programie Microsoft. Zabezpieczenia usługi OMS i inspekcji będzie używać tych danych do identyfikowania potencjalnych zagrożeń dla środowiska.

Okienko analizy zagrożenia składa się przez trzy główne sposoby:
- Serwery z złośliwy ruchu wychodzącego
- Typy zagrożeń wykryto
- Mapa analizy zagrożenia

> [AZURE.NOTE] Zawiera omówienie dotyczące tych opcji przeczytaj [Wprowadzenie do zabezpieczeń pakietu zarządzania operacje i rozwiązanie inspekcji](oms-security-getting-started.md).

### <a name="responding-to-security-alerts"></a>Odpowiadanie na alerty zabezpieczeń

Jeden z kroków procesu [odpowiedzi zdarzenia zabezpieczeń](https://technet.microsoft.com/library/cc512623.aspx) jest określenie ważności o kompromisów. Faza należy wykonać następujące zadania:

- Określanie rodzaju atakiem
- Określanie atakiem punktem początkowym
- Określanie zamiarem atakiem. Została atakiem specjalnie skierowany w organizacji, aby uzyskać szczegółowe informacje, czy został losową?
- Identyfikowanie systemów, które zostały złamane
- Identyfikowanie plików, które zostały dostępne i określanie czułości tych plików

Można wykorzystać informacje **Analizy zagrożenia** w zabezpieczeń usługi OMS i rozwiązanie inspekcji, aby ułatwić wykonywanie następujących zadań. Wykonaj poniższe czynności, aby uzyskać dostęp do tej opcji **Analizy zagrożenia** :

1. Na pulpicie nawigacyjnym głównym **Pakietu zarządzania operacje Microsoft** kliknij Kafelek **zabezpieczeń i inspekcji** .

    ![Zabezpieczenia i inspekcji](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)

2. Pulpit nawigacyjny **zabezpieczeń i inspekcji** pojawią się opcje **Analizy zagrożenia** po prawej stronie, tak jak pokazano poniżej:

    ![Zagrożenia firmy intel](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig2-ga.png)

Następujące trzy Kafelki nadać omówienie bieżącego zagrożeń. W **Server z złośliwy ruchu wychodzącego** , który będzie można ustalić, czy istnieje jakiś komputer, które są monitorowania (wewnątrz lub poza siecią) który wysyła szkodliwy ruch w Internecie. 

Fragment **wykryte zagrożenie typy** zawiera podsumowanie zagrożeń, które są aktualne "w znaków symboli", po kliknięciu na tym kafelku zostaną wyświetlone szczegółowe informacje o tych zagrożeń jako pokaz poniżej:

![Typy wykryto zagrożenie](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig3.png)

Więcej informacji na temat poszczególnych zagrożenie można wyodrębnić, klikając go. W poniższym przykładzie przedstawiono szczegółowe informacje o Botnet:

![więcej informacji na temat zagrożenie](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig4.png)

Opisane w tej sekcji na początku te informacje mogą być bardzo przydatne w przypadku zdarzenia odpowiedź. Można również go ważne podczas dochodzenia sądowej, gdzie musisz znalezienia źródła atakiem, jakiego systemu zostało naruszone i osi czasu. W tym raporcie można łatwo zidentyfikować niektóre kluczowe informacje o atakiem, takich jak: źródło atakiem, lokalny adres IP, który został zostało naruszone i stan bieżącej sesji połączenia. 

**Mapa analizy zagrożenia** pomoże Ci do identyfikowania bieżącej lokalizacji na całym świecie, mające szkodliwy ruch. Istnieją pomarańczowy (przychodzące) i czerwony (wychodzące) za pomocą strzałek w tym mapy określające kierunek ruchu po kliknięciu w jedną z tych strzałek, będzie widoczny typ zagrożenia i kierunkiem ruchu, tak jak pokazano poniżej:

![zagrożenia firmy intel mapy](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig5.png)

> [AZURE.NOTE] Można wyświetlić pokaz dotyczące używania tej funkcji podczas zdarzenia proces odpowiedzi, oglądając prezentacji [Mitigate centrum danych zagrożeniami z przewodnikiem badaniu przy użyciu pakietu zarządzania operacje](https://myignite.microsoft.com/videos/5000) dostarczane w Ignite firmy Microsoft.

## <a name="see-also"></a>Zobacz też

W tym dokumencie wiesz, jak korzystać z opcji **Analizy zagrożenie** bezpieczeństwa usługi OMS i rozwiązanie inspekcji reagować na alerty zabezpieczeń. Aby dowiedzieć się więcej na temat zabezpieczeń usługi OMS, zobacz następujące artykuły:

- [Przegląd operacji usługi zarządzania pakietu (OMS)](operations-management-suite-overview.md)
- [Wprowadzenie do zabezpieczeń pakietu zarządzania operacje i rozwiązanie inspekcji](oms-security-getting-started.md)
- [Monitorowanie zasobów operacje zarządzania pakietu zabezpieczeń i rozwiązanie inspekcji](oms-security-monitoring-resources.md)
