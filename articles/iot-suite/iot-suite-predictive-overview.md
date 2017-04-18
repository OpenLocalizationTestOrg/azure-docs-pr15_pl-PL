<properties
 pageTitle="Konserwacja przewidywanych wstępnie rozwiązanie | Microsoft Azure"
 description="Opis konserwacji przewidywanych Azure IoT wstępnie rozwiązanie."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="stevehob"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/17/2016"
 ms.author="araguila"/>

# <a name="predictive-maintenance-preconfigured-solution-overview"></a>Omówienie rozwiązania przewidywanych konserwacji wstępnie

Rozwiązanie *konserwacji przewidywanych* wstępnie jest jednym z [wstępnie rozwiązań] [ lnk_preconfigured_solutions] wydany jako część pakietu [Microsoft Azure IoT][lnk_iot_suite]. To rozwiązanie można zintegrować zbioru telemetrycznego urządzenia w czasie rzeczywistym z model przewidywanych utworzony przy użyciu [Azure maszynowego uczenia][lnk_machine_learning].


Z pakietem IoT Azure przedsiębiorstw można szybko i łatwo połączyć się i monitorowanie składniki majątku i analizowania danych w czasie rzeczywistym. Rozwiązanie konserwacji przewidywanych wstępnie trwa danych i używa sformatowanego pulpitów nawigacyjnych i wizualizacji w celu zapewnienia firmy przy użyciu nowego analizy, można sterują wydajności i ulepszanie strumienie zysk.

## <a name="the-scenario"></a>Scenariusz

Fabrikam to regionalne linii lotniczych omawiający doskonałe wrażenia cenach konkurencyjności. Jedną z przyczyn opóźnienia lotów jest konserwacji problemów i konserwacji aparat samolotu jest szczególnie trudne. Aparat błąd podczas lotu należy unikać za wszelką cenę, Fabrikam regularnie sprawdza jego aparaty i jest zgodny z programem zaplanowanych. Jednak aparatów samolotu nie zawsze należy nosić takie same. Niektóre niepotrzebne konserwacji jest wykonywane na aparatów. Co ważne pojawiają się problemy z którego ziemi samolotu, aż odbywa się konserwacji. Ta opcja powoduje opóźnienia kosztów, zwłaszcza jeśli samolot jest w miejscu, w którym prawo techników lub części nie są dostępne.

Aparaty samolotu firmy działają z czujników, które można monitorować warunki aparat podczas lotu. Za pomocą pakietu IoT Azure Fabrikam zbieranie danych czujnik zebranych podczas lotu. Po kumulowanych lat aparat operacyjne i dane awarii naukowców danych firmy mają modelowania umożliwia przewidywanie pozostała przydatne życia (RUL) z aparatu samolotu. Co to jest zidentyfikował jest korelacji między danymi z czterech czujników aparat z zużycia aparat, który prowadzi do ewentualnych błędach. Podczas Fabrikam do przeprowadzania regularnych kontroli w celu zapewnienia bezpieczeństwa będzie nadal występował, może teraz używać modeli do obliczenia RUL dla każdego aparatu po każdej lotu przy użyciu telemetrycznego zebrane z aparatów podczas lotu. Fabrikam można teraz przewidywanie przyszłych punkty błąd i Planowanie obsługi i napraw z wyprzedzeniem.

> [AZURE.NOTE] Model rozwiązanie używa danych zużycie rzeczywiste aparat.

Przez przewidywania punktu, gdy konserwacji jest wymagany, Fabrikam można zoptymalizować jej operacje, aby zmniejszyć koszty. Konserwacja koordynatorów pracować nad pracownikom Planowanie obsługi co zbiegło się ze samolotu zatrzymywanie w określonym miejscu i zapewnienie wystarczającą ilość czasu na samolot nieobecność usług, nie powodując zakłócenia harmonogramu. Fabrikam mogą planować techników zapewnienie samolotu są obsługiwane na wydajność bez czas oczekiwania. Menedżerowie sterowania zapasami otrzymają plany konserwacji, aby zoptymalizować procesu sortowania i zapasowe spisu części. Wszystko to umożliwia Fabrikam, aby zminimalizować samolotu podłożem czas i zmniejszyć koszty operacyjne zapewniając bezpieczeństwa osób i personelu.

Zapoznać się z jak [Azure IoT pakietu] [ lnk_iot_suite] zawiera funkcje, których klienci muszą wykorzystania możliwości przewidywanych konserwacji, przejrzyj ten [infographic][lnk_infographic].

## <a name="how-the-predictive-maintenance-solution-is-built"></a>Jak wbudowane rozwiązanie przewidywanych konserwacji

Rozwiązanie wykorzystuje istniejącego modelu Azure maszynowego uczenia dostępny jako szablon, aby wyświetlić te możliwości pracy z zebranych za pośrednictwem usługi pakietu IoT telemetrycznego urządzenia. Microsoft wbudowanej [modelu regresji] [ lnk_regression_model] z aparatu samolotu i opublikowane szablonie wykonania danych<sup>\[1\]</sup>i instrukcje krok po kroku dotyczące sposobu używania modelu.

Rozwiązanie konserwacji przewidywanych wstępnie Azure IoT używa modelu regresji utworzone na podstawie tego szablonu. jest używany do subskrypcji usługi Azure i dostępne za pośrednictwem interfejsu API automatycznie wygenerowanego. Rozwiązanie zawiera podzestawu danych testowania, reprezentującą 4 (całkowity 100) aparaty i 4 (21 całkowity) strumienie danych czujnika, zapewniające dokładne wyniku przeszkolony modelu.

*\[1\] A. Saxena i Goebel K. (2008). "Turbowentylatorowe aparat obniżenie wydajności symulacji zestawu danych", NASA repozytorium danych Prognostics Ames (http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/), Centrum badań Ames NASA, Moffett pola, CA*

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat sposobu Azure IoT umożliwia scenariuszy przewidywanych konserwacji, przeczytaj [Przechwytywanie wartość z Internetem co][lnk_capture_value].

Wykonaj [instrukcje] [ lnk-predictive-walkthrough] przewidywanych konserwacji wstępnie rozwiązanie.

[lnk-predictive-walkthrough]: iot-suite-predictive-walkthrough.md
[lnk_preconfigured_solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk_iot_suite]: iot-suite-overview.md
[lnk_machine_learning]: https://azure.microsoft.com/services/machine-learning/
[lnk_infographic]: https://www.microsoft.com/server-cloud/predictivemaintenance/Index.html
[lnk_regression_model]: http://gallery.cortanaanalytics.com/Collection/Predictive-Maintenance-Template-3
[lnk_capture_value]: http://download.microsoft.com/download/0/7/D/07D394CE-185D-4B96-AC3C-9B61179F7080/Capture_value_from_the_Internet%20of%20Things_with_Predictive_Maintenance.PDF

Można również zapoznać się niektóre z innych funkcji i możliwości rozwiązań pakietu IoT wstępnie:

- [Często zadawane pytania dotyczące pakietu IoT][lnk-faq]
- [Zabezpieczenia IoT od podstaw][lnk-security-groundup]

[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
