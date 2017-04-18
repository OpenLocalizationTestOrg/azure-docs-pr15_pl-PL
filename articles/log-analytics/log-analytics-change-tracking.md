<properties
    pageTitle="Rozwiązanie do analizy dziennika śledzenia zmian | Microsoft Azure"
    description="Umożliwia śledzenie zmian w konfiguracji rozwiązanie do analizy dziennika ułatwiającą identyfikowanie oprogramowania i usług Windows zmian w środowisku usługi — identyfikowanie te zmiany w konfiguracji może pomóc w określeniu operacyjne problemów."
    services="operations-management-suite"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="operations-management-suite"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="change-tracking-solution-in-log-analytics"></a>Zmienianie śledzenia rozwiązanie do analizy dziennika


Umożliwia śledzenie zmian w konfiguracji rozwiązanie w dzienniku analizy ułatwiającą identyfikowanie oprogramowania i usług Windows zmiany i Linux oraz demon występujące w środowisku — identyfikowanie te zmiany w konfiguracji może pomóc w określeniu operacyjne problemów.

Zainstaluj rozwiązanie, aby zaktualizować typ agenta, który jest zainstalowany. Zmiany zainstalowanego oprogramowania i usług systemu Windows na serwerach monitorowane są odczytywane i następnie dane są przesyłane do analizy dziennika usługi w chmurze przetwarzania. Logika jest stosowana do odbierane dane i usług w chmurze rekordów danych. W przypadku znalezienia zmiany serwery ze zmianami są wyświetlane na pulpicie nawigacyjnym śledzenia zmian. Korzystając z informacji na pulpicie nawigacyjnym śledzenia zmian, umożliwia łatwe wyświetlanie zmian wprowadzonych w infrastrukturze serwera.

## <a name="installing-and-configuring-the-solution"></a>Instalowanie i konfigurowanie rozwiązanie
Aby zainstalować i skonfigurować rozwiązanie, korzystając z następujących informacji.

- Programu Operations Manager jest wymagana rozwiązanie śledzenia zmian.
- Agent systemu Windows lub programu Operations Manager musi mieć na każdym komputerze, na którym chcesz monitorować zmiany.
- Dodaj rozwiązanie śledzenia zmian do obszaru roboczego usługi OMS przy użyciu procesu opisanego w [rozwiązań Dodawanie analizy dziennika z galerii rozwiązań](log-analytics-add-solutions.md).  Nie jest wymagana żadna konfiguracja dalsze.


## <a name="change-tracking-data-collection-details"></a>Zmienianie szczegółów zbioru danych śledzenia

Śledzenie zmian gromadzi spisu oprogramowania i metadanych usługi systemu Windows za pomocą czynników, które są włączone.

W poniższej tabeli przedstawiono metody zbioru danych i inne szczegóły dotyczące sposobu dane są zbierane dla śledzenia zmian.

| platformy | Bezpośrednie agenta | SCOM agent | Azure miejsca do magazynowania | SCOM wymagane? | Dane agenta SCOM wysyłane za pośrednictwem grupy zarządzania | częstotliwość pobierania |
|---|---|---|---|---|---|---|
|Systemu Windows|![Tak](./media/log-analytics-change-tracking/oms-bullet-green.png)|![Tak](./media/log-analytics-change-tracking/oms-bullet-green.png)|![Brak](./media/log-analytics-change-tracking/oms-bullet-red.png)|            ![Brak](./media/log-analytics-change-tracking/oms-bullet-red.png)|![Tak](./media/log-analytics-change-tracking/oms-bullet-green.png)| co godzina|

## <a name="use-change-tracking"></a>Za pomocą śledzenia zmian

Po zainstalowaniu rozwiązanie, możesz wyświetlić podsumowanie zmian monitorowane serwerów przy użyciu kafelka **Śledzenia zmian** na stronie **Omówienie** w usługi OMS.

![Obraz kafelka śledzenia zmian](./media/log-analytics-change-tracking/oms-changetracking-tile.png)

Można wyświetlić zmiany do serwisu infrastruktury, a następnie przechodzenia do szczegółów dla następujących kategorii:

- Zmiany wprowadzone przez typ konfiguracji dla oprogramowania i usług systemu Windows
- Oprogramowanie zmieni się na aplikacje i aktualizacje dla poszczególnych serwerów
- Całkowita liczba zmian w oprogramowaniu dla każdej aplikacji
- Zmiany dotyczące poszczególnych serwerach service dla systemu Windows

![Obraz pulpitu nawigacyjnego śledzenia zmian](./media/log-analytics-change-tracking/oms-changetracking01.png)

![Obraz pulpitu nawigacyjnego śledzenia zmian](./media/log-analytics-change-tracking/oms-changetracking02.png)

### <a name="to-view-changes-for-any-change-type"></a>Aby wyświetlić zmiany w przypadku każdej Zmień typ

1. Na stronie **Przegląd** kliknij **Śledzenia zmian** .
2. Na pulpicie **Zmienianie śledzenia** przejrzeć informacje podsumowujące w jednym z karty typ zmiany, a następnie kliknij jedną, aby wyświetlić szczegółowe informacje na stronie **wyszukiwania dziennika** .
3. Na dowolnej stronie wyszukiwania dziennika możesz wyświetlić wyniki przez godzinę, szczegółowe wyniki i historii wyszukiwania dziennika. Możesz również filtrować według aspekty w celu zawężenia wyników.

## <a name="next-steps"></a>Następne kroki

- Umożliwia wyświetlanie Zmień szczegółowe dane śledzenia [dziennika wyszukiwania analizy dziennika](log-analytics-log-searches.md) .
