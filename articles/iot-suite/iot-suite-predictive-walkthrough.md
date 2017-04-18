<properties
 pageTitle="Instruktaż przewidywanych konserwacji | Microsoft Azure"
 description="Instrukcje obsługi przewidywanych Azure IoT wstępnie rozwiązanie."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="aguilaaj"
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

# <a name="predictive-maintenance-preconfigured-solution-walkthrough"></a>Przewodnik po rozwiązanie konserwacji przewidywanych wstępnie

## <a name="introduction"></a>Wprowadzenie

Rozwiązanie konserwacji przewidywanych wstępnie IoT pakietu jest kompleksowe — rozwiązania dla scenariusza biznesowego, który przewiduje punktu po mogą wystąpić błąd. Można to wstępnie rozwiązanie w wyprzedzeniem działań, takich jak optymalizowania konserwacji. Rozwiązanie łączy klucza usługi Azure IoT pakietu, w tym [Azure maszynowego uczenia] [ lnk_machine_learning] obszaru roboczego. Ten obszar roboczy zawiera doświadczeń, na podstawie zestawu danych przykładowych publicznej, przewidywanie pozostała przydatne życia (RUL) z aparatu samolotu. Rozwiązanie w pełni wykonuje scenariusza biznesowego IoT jako punktu początkowego planowania i implementowania rozwiązanie, które spełniają potrzeb firmy.

## <a name="logical-architecture"></a>Architektura logicznych.

Poniższy diagram przedstawia logiczne składniki wstępnie rozwiązania:

![][img-architecture]

Niebieski elementy są usług Azure, z których zainicjowano obsługę administracyjną w lokalizacji wybranej podczas Zapewnij wstępnie rozwiązanie. Umożliwia obsługę wstępnie rozwiązania w programie regionu wschodniego US, północnej Europie lub Azji Wschodniej.

Niektóre zasoby nie są dostępne w regionach, w którym obsługi administracyjnej wstępnie rozwiązanie. Pomarańczowy elementy na diagramie reprezentują usług Azure obsługi administracyjnej w najbliższym dostępne regionie (południe centralnej nam, zachód Europy lub Azji Południowo-) podane wybranego regionu.

Zielony elementu to urządzenie symulowany reprezentujący aparat samolotu. Więcej informacji o tych urządzeniach symulowany w następnej sekcji.

Szary elementy reprezentują składniki, których wdrożenia funkcji *Administracja urządzenia* . Bieżącej wersji rozwiązanie konserwacji przewidywanych wstępnie nie obsługi administracyjnej te zasoby. Aby dowiedzieć się więcej o administrowaniu urządzenia, zapoznaj się z [zdalne monitorowanie wstępnie skonfigurowane rozwiązanie][lnk-remote-monitoring].

## <a name="simulated-devices"></a>Symulowany urządzeń

W rozwiązaniu wstępnie symulowany urządzenie reprezentuje aparat samolotu. Rozwiązanie jest obsługi administracyjnej dwa aparaty, które mapowanie jednego samolotu. Każdy aparat emituje cztery typy telemetrycznego: 9 czujnika, 11 czujnika, czujnik 14 i 15 czujnik zawiera dane niezbędne do modelu nauki maszynowego do obliczania pozostała przydatne życia (RUL) dla aparatu. Każde urządzenie symulowany wysyła następujące komunikaty telemetrycznego do koncentratora IoT:

*Liczba cykli*. Cykl reprezentuje złożonym lotów o zmiennej długości między godziną 2-10, w których dane telemetryczne jest rejestrowany co pół godziny podczas lotu.

*Telemetrycznego*. Istnieją cztery czujniki, reprezentujących atrybuty aparat. Czujników ogólnie są oznaczone czujnik 9, 11 czujnika, czujnik 14 i 15 czujnika. Czujniki te 4 przedstawiają telemetrycznego wystarczające, aby uzyskać przydatne wyniki z modelu komputera szkoleniowe dla RUL. Ten model jest tworzony z publicznej zestaw danych, który zawiera dane czujnika rzeczywistą aparat. Aby uzyskać więcej informacji dotyczących tworzenia modelu z oryginalnego zbioru danych, zobacz temat [Cortana galerii przewidywanych konserwacji szablon analizy][lnk-cortana-analytics].

Symulowany urządzeń mogą obsługiwać następujących poleceń wysłanych z koncentratora IoT:

| Polecenie | Opis |
|---------|-------------|
| StartTelemetry | Określa stan symulacji.<br/>Pozwala uruchomić urządzenie wysyłające telemetrycznego     |
| StopTelemetry  | Określa stan symulacji.<br/>Urządzenie wysyłające telemetrycznego tabulatorów |

Centrum IoT zawiera potwierdzenie polecenia urządzenia.

## <a name="azure-stream-analytics-job"></a>Azure analizy strumieniu zadania

**Zadania: telemetrycznego** działa na przychodzące strumienia telemetrycznego urządzenia za pomocą dwie instrukcje. Pierwszy wybiera wszystkie telemetrycznego z urządzenia i wysyła te dane do blob przestrzeni dyskowej, z którym jest zwizualizować w aplikacji sieci web. Druga instrukcja oblicza wartości średniej czujnik przez dwie minuty okno przesuwania i wysyła te dane za pośrednictwem Centrum zdarzeń **Procesor zdarzeń**.

## <a name="event-processor"></a>Przetwarzanie zadań

**Procesor zdarzeń** pobiera wartości średniej czujnik złożonym cyklu. Jej szkolenia te wartości do interfejs API, który udostępnia nauki maszynowego upływem tego modelu do obliczania RUL aparatu.

## <a name="azure-machine-learning"></a>Nauka Azure komputera

Aby uzyskać więcej informacji dotyczących tworzenia modelu z oryginalnego zbioru danych, zobacz temat [Cortana galerii przewidywanych konserwacji szablon analizy][lnk-cortana-analytics].

## <a name="lets-start-walking"></a>Zacznijmy analizowanie

W tej sekcji przeprowadzi Cię przez składniki rozwiązania, w tym artykule opisano litery przeznaczone oraz przykłady.

### <a name="predictive-maintenance-dashboard"></a>Pulpit nawigacyjny przewidywanych konserwacji

Kontrolki języka PowerBI JavaScript używa tej strony w aplikacji sieci web (zobacz [repozytorium efekty wizualne PowerBI][lnk-powerbi]) do wizualizacji:

- Dane wyjściowe z zadań analizy strumieniu w magazynie obiektów blob.
- Liczba RUL i cykl na aparat samolotu.

### <a name="observing-the-behavior-of-the-cloud-solution"></a>Obserwowanie zachowanie rozwiązanie chmury

W portalu Azure przejdź do grupy zasobów z nazwą rozwiązanie, wybrane do wyświetlania zasobów ustanawianie.

![][img-resource-group]

Po postanowień wstępnie rozwiązanie, otrzymasz wiadomość e-mail z łączem do obszaru roboczego nauki komputera. Możesz również przejść do obszaru roboczego nauki komputera z [azureiotsuite.com] [ lnk-azureiotsuite] strony rozwiązanie ustanawianie, gdy jest **gotowa** do drukowania.

![][img-machine-learning]

W portalu rozwiązanie widać, że próbka jest obsługi administracyjnej z czterech urządzeń symulowany reprezentować dwa samolotu dwa aparaty na samolotu, każda z czterech czujników. Po pierwszym przejdź do portalu rozwiązanie, symulacji jest zablokowany.

![][img-simulation-stopped]

Kliknij przycisk **Start symulacji** zacząć symulacji, w której można wyświetlić historię czujnika, RUL, cykli, i historii RUL wypełnić pulpitu nawigacyjnego.

![][img-simulation-running]

Gdy RUL jest mniejsza niż 160 (dowolnego próg wybrany na potrzeby pokaz), portalu rozwiązanie zostanie wyświetlony symbol ostrzeżenia obok wyświetlania RUL i wyróżnienie aparat samolotu w żółty. Zwróć uwagę, jak wartości RUL mają ogólnej ogólnego tendencji w dół, ale zwykle pojawia podskokiem w górę lub w dół. To zachowanie wyniki z różnych długości cyklu i dokładności modelu.

![][img-simulation-warning]

Pełny symulacji trwa około 35 minut, aby ukończyć 148 cykli. Próg RUL 160 zostały spełnione, po raz pierwszy około 5 minut i obu aparatów naciśnij progu na około 8 minut.

Symulacja działa za pośrednictwem pełny zestaw danych dla cykli 148 i rozlicza na wartości końcowe RUL i cykl.

Możesz wyłączyć w dowolnym momencie symulacji, ale kliknięcie przycisku **Uruchom symulacji** odtwarzaniem symulacji od początku zestawu danych.

## <a name="next-steps"></a>Następne kroki

Po uruchomieniu rozwiązanie konserwacji przewidywanych wstępnie, warto ją zmodyfikować, zobacz [wskazówki dotyczące dostosowywania rozwiązań wstępnie][lnk-customize].

Wpis w blogu [IoT Suite — w obszarze Ustawienia zaawansowane — konserwacja przewidywanych](http://social.technet.microsoft.com/wiki/contents/articles/33527.iot-suite-under-the-hood-predictive-maintenance.aspx) TechNet zawiera dodatkowe szczegóły dotyczące rozwiązanie konserwacji przewidywanych wstępnie.

Można również zapoznać się niektóre z innych funkcji i możliwości rozwiązań pakietu IoT wstępnie:

- [Często zadawane pytania dotyczące pakietu IoT][lnk-faq]
- [Zabezpieczenia IoT od podstaw][lnk-security-groundup]


[img-architecture]: media/iot-suite-predictive-walkthrough/architecture.png
[img-resource-group]: media/iot-suite-predictive-walkthrough/resource-group.png
[img-machine-learning]: media/iot-suite-predictive-walkthrough/machine-learning.png
[img-simulation-stopped]: media/iot-suite-predictive-walkthrough/simulation-stopped.png
[img-simulation-running]: media/iot-suite-predictive-walkthrough/simulation-running.png
[img-simulation-warning]: media/iot-suite-predictive-walkthrough/simulation-warning.png

[lnk-powerbi]: https://www.github.com/Microsoft/PowerBI-visuals
[lnk_machine_learning]: https://azure.microsoft.com/services/machine-learning/
[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-cortana-analytics]: http://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
