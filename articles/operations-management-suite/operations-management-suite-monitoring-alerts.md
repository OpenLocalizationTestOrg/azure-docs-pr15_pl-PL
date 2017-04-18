<properties 
   pageTitle="Alert zarządzania w programie Microsoft monitorowania produktów | Microsoft Azure"
   description="Alert wskazuje niektórych problem, który wymaga uwagi przez administratora.  W tym artykule opisano różnice w sposób tworzenia i zarządzania nimi System Center Operations Manager (SCOM) i analizy dziennika alertów, a także najlepsze rozwiązania dotyczące używanie dwóch produktów strategii zarządzania alertami hybrydowych." 
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="bwren" />

# <a name="managing-alerts-with-microsoft-monitoring"></a>Zarządzanie alertami monitorowanie firmy Microsoft 

Alert wskazuje niektórych problem, który wymaga uwagi przez administratora.  Istnieje wyraźnych różnic między System Center Operations Manager (SCOM) i analizy dziennika usługi pakietu w operacji zarządzania (OMS) tworzenia alertów, jak są zarządzane i analizowane i sposób powiadamiania wykryto problem krytyczne.

## <a name="alerts-in-operations-manager"></a>Alerty w programie Operations Manager
Alerty w SCOM są generowane przez poszczególnych reguł lub monitorów, aby wskazać konkretnego problemu.  Monitor mogą powodować zwrócenie alertu, gdy znajdzie się ona stanie błędu podczas reguły może generować alert, aby wskazać niektórych krytycznych problem, który nie jest bezpośrednio związane z kondycji obiektu zarządzanego.  Pakiety zarządzania zawierają różne przepływów pracy, które Tworzenie alertów dla aplikacji lub usługi, które zarządzają.  Część procesu konfigurowania nowy pakiet zarządzania jest dostosowywania go, aby upewnić się, że nie otrzymasz nadmiarowe alertów dla problemów, których nie można uznać za krytyczne.

![Wyświetlanie alertu SCOM](media/operations-management-suite-monitoring-alerts/scom-alert-view.png)

SCOM zapewnia pełną zarządzania alertami alerty o statusie, które mogą być zmieniane przez administratorów w trakcie pracy, aby rozwiązać ten problem.  Jeśli problem został rozwiązany, administrator ustawia alert, aby zamknąć co będą już wyświetlane w widokach wyświetlanie aktywnej alertów.  Alerty generowane przez monitory można rozwiązać automatycznie, gdy monitor powraca do stanu prawidłowy.

![Stan alertu](media/operations-management-suite-monitoring-alerts/scom-alert-status.png)

## <a name="alerts-in-log-analytics"></a>Alerty w dzienniku analizy
Alert w analizy dziennika jest tworzona z kwerendy dziennika, która jest uruchamiana automatycznie w regularnych odstępach czasu.  Możesz utworzyć regułę alertu z dowolnej kwerendy dziennika.  Jeśli kwerenda zwraca wyniki, spełniającym kryteria polu przez użytkownika, zostanie utworzony alertu.  Może to być określone kwerendę, która powoduje utworzenie alertu, jeśli zostanie wykryta określonego zdarzenia lub można użyć bardziej ogólne kwerendę, która wyszukuje zdarzenie błędów związanych z określonej aplikacji.

Alerty analizy dziennika są zapisywane do repozytorium usługi OMS jako wydarzenie i mogą być pobierane za pomocą kwerendy dziennika.  Nie mają stan, takich jak zdarzenia SCOM tak, aby można wskazać, kiedy ten problem został rozwiązany.

![Alert usługi OMS](media/operations-management-suite-monitoring-alerts/oms-alert.png)

Gdy SCOM jest używany jako źródła danych dla analizy dziennika, alerty SCOM są zapisywane w repozytorium usługi OMS podczas tworzenia i modyfikacji.  

![SCOM alert](media/operations-management-suite-monitoring-alerts/scom-alert.png)

[Rozwiązanie do zarządzania alertami](http://technet.microsoft.com/library/mt484092.aspx) zawiera podsumowanie aktywnego alertów i kilka typowych kwerend do pobierania różnych zestawów alertów.  Oferuje bardziej skutecznych analiz alertów niż raportu w SCOM.  Możesz przechodzenia do szczegółów z podsumowań do szczegółowych danych i tworzenie zapytań ad hoc w celu pobierania różnych zestawów alertów.

![Rozwiązanie do zarządzania alertami](media/operations-management-suite-monitoring-alerts/alert-management.png)

## <a name="notifications"></a>Powiadomienia
Powiadomienia w SCOM wysłać poczty lub tekstu w odpowiedzi na alerty, które są zgodne z kryteriami.  Możesz utworzyć subskrypcje różnych powiadomienie, mające różne osoby powiadomienia w zależności od takich kryteriów, jak obiekt monitorowane, ważności alertu, rodzaj problemu, który wykryte lub godzinę.

Subskrypcje kilka umożliwia wdrożenie strategii pełną powiadomienie o dużej liczby pakietów zarządzania.

![Alerty SCOM](media/operations-management-suite-monitoring-alerts/alerts-overview-scom.png)

Dziennik analizy Cię powiadamiać pocztą czy alert został utworzony przez ustawienie akcji powiadomień e-mail dla każdej [reguły alertu](http://technet.microsoft.com/library/mt614775.aspx).  Możliwość SCOM nie ma Subskrybuj wiele alertów za pomocą jednej reguły.  Trzeba również utworzyć alert reguł, ponieważ usługi OMS nie zapewnia wstępnie.

![Alerty analizy dziennika](media/operations-management-suite-monitoring-alerts/alerts-overview-oms.png)

Nie możesz całkowicie zarządzać SCOM alerty w dzienniku analizy jednak ponieważ można modyfikować tylko je w konsoli operacji.  Analizy dziennika jest przydatna jako część zarządzania alertami jednak procesu dla dostarczając narzędzi do analizy tego samego SCOM nie ma.

## <a name="alert-remediation"></a>Alert rozwiązywanie problemu
[Rozwiązywanie problemu](http://technet.microsoft.com/library/mt614775.aspx) odwołuje się do próba, aby automatycznie rozwiązać ten problem, oznaczona alertu.
  
SCOM pozwala na uruchamianie narzędzia diagnostyczne i odzyskiwania w odpowiedzi na monitorze wprowadzanie stan nieprawidłowe.  Dzieje się jednoczesnego Monitor Tworzenie alertu.  Jako skrypt uruchamianej na agenta są zwykle stosowane diagnostyki i odzyskiwania.  Diagnostyczne próbuje zebrać więcej informacji na temat wykryto problem podczas próby odzyskiwania rozwiązać ten problem.

Analizy dziennika pozwala na rozpoczęcie [działań aranżacji automatyzacji Azure](https://azure.microsoft.com/documentation/services/automation/) lub połączenie webhook w odpowiedzi na alert analizy dziennika.  Runbooks może zawierać złożone logiczny zaimplementowana w programie PowerShell.  Skrypt działa w Azure i mają dostęp do dowolnego Azure zasobów lub zasobów zewnętrznych, które są dostępne z chmury.  Azure automatyzacji mają możliwość wykonania runbooks na serwerze w lokalnym centrum danych, ale ta funkcja nie jest obecnie dostępny podczas uruchamiania działań aranżacji w odpowiedzi na alerty analizy dziennika.

Zarówno odzyskiwania w SCOM i runbooks w usługi OMS mogą zawierać skryptów programu PowerShell, ale odzyskiwania są trudniejsze do tworzenia i zarządzania nimi, ponieważ muszą być zawarte w pakiecie zarządzania.  Runbooks są przechowywane w automatyzacji Azure, która zapewnia funkcje tworzenia, testowanie i zarządzanie runbooks.

Jeśli używasz SCOM jako źródła danych na potrzeby analizy dziennika, można utworzyć alert analizy dziennika, pobrać alerty SCOM przechowywane w repozytorium usługi OMS za pomocą kwerendy dziennika.  Pozwoli do uruchomienia działań aranżacji automatyzacji Azure w odpowiedzi na SCOM alert.  Oczywiście ponieważ działań aranżacji będzie działać w Azure, nie będzie rentowny strategii odzyskiwania problemów lokalnego.

## <a name="next-steps"></a>Następne kroki

- Poznaj szczegóły alertów [W System Centrum Operations Manager (SCOM)](https://technet.microsoft.com/library/hh212913.aspx).