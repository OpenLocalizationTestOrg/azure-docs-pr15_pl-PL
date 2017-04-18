<properties
    pageTitle="Monitorowanie powierzchniowy koncentratory z dziennika analizy | Microsoft Azure"
    description="Rozwiązanie Centrum powierzchni umożliwia śledzenie kondycji usługi koncentratory powierzchni i zrozumieć, jak są one używane."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="monitor-surface-hubs-with-log-analytics"></a>Monitorowanie powierzchniowy koncentratory z analizy dziennika

W tym artykule opisano, jak można rozwiązanie Centrum powierzchni w analizy dziennika monitorowanie urządzeń Centrum Microsoft Surface z Microsoft operacje zarządzania pakietu (usługi OMS). Analizy dziennika ułatwia śledzenie kondycji usługi koncentratory powierzchni a także zrozumieć, jak są one używane.

Każdy powierzchniowy jest zainstalowany monitorowania agenta firmy Microsoft. Jego za pośrednictwem agenta, że dane można wysyłać z Twoim Centrum powierzchni do usługi OMS. Pliki dziennika są odczytywane z powierzchni koncentratory i są, a następnie są wysyłane do usługę. Zagadnienia, takie jak serwery jest w trybie offline nie można zsynchronizować, kalendarz lub w przypadku konta urządzenia nie można zalogować się do programu Skype są wyświetlane w usługi OMS na pulpicie nawigacyjnym Centrum powierzchni. Przy użyciu danych na pulpicie nawigacyjnym, można określić urządzeń, których nie ma uruchomionego lub są występują inne problemy, a potencjalnie zastosować rozwiązania problemów wykryto.


## <a name="installing-and-configuring-the-solution"></a>Instalowanie i konfigurowanie rozwiązanie

Aby zainstalować i skonfigurować rozwiązanie, korzystając z następujących informacji. Aby można było zarządzać koncentratorów z powierzchni z Microsoft operacje zarządzania pakietu (usługi OMS), musisz następujące czynności:

- Ważna Subskrypcja do [usługi OMS](http://www.microsoft.com/oms).
- Poziom [subskrypcji usługi OMS](https://azure.microsoft.com/pricing/details/log-analytics/) obsługujące liczbę urządzeń, które chcesz monitorować. Cennik usługi OMS zależnie od zarejestrowanych ile urządzeń i ilości danych go procesów. Należy to uwzględnić podczas planowania wdrożenia Centrum powierzchni.

Następnie spowoduje dodanie subskrypcji usługi OMS do istniejącej subskrypcji usługi Microsoft Azure lub tworzenie nowego obszaru roboczego bezpośrednio w portalu usługi OMS. Szczegółowe instrukcje dotyczące korzystania z jednej z metod znajduje się na [Wprowadzenie do analizy dziennika](log-analytics-get-started.md). Po skonfigurowaniu subskrypcji usługi OMS istnieją dwa sposoby rejestracji urządzenia Surface Centrum:

- Automatycznie za pośrednictwem usługi
- Samodzielnie przy użyciu **ustawień** na urządzeniu Centrum powierzchni.

## <a name="set-up-monitoring"></a>Konfigurowanie monitorowania

Można monitorować kondycję i działania usługi Centrum powierzchni przy użyciu analizy dziennika w usługi OMS. Centrum powierzchni w usługi OMS mogą rejestrować za pomocą InTune lub lokalnie za pomocą **ustawień** w Centrum powierzchni.

## <a name="connect-surface-hubs-to-oms-through-intune"></a>Łączenie koncentratorów Surface z usługi OMS za pośrednictwem usługi

W obszarze roboczym usługi OMS, która będzie zarządzać koncentratorów z powierzchni musisz identyfikator obszaru roboczego i klucz obszaru roboczego. Możesz przejść z portalem usługi OMS.

InTune jest produktu firmy Microsoft, który umożliwia centralne zarządzanie ustawienia konfiguracji usługi OMS, które są stosowane do jednej lub kilku urządzeniach. Wykonaj poniższe czynności, aby skonfigurować urządzenia za pośrednictwem usługi:

1. Zaloguj się do usługi.
2. Przejdź do **pozycji ustawienia** > **połączonych źródeł**.
3. Tworzenie lub edytowanie zasady na podstawie szablonu Centrum powierzchni.
4. Przejdź do sekcji usługi OMS (wniosków operacyjne Azure) zasady, a następnie dodaj *Identyfikator obszaru roboczego* i *Klucz obszaru roboczego* do zasad.
5. Zapisywanie zasady.
6. Kojarzenie zasady z odpowiedniej grupy urządzeń.

  ![Zasady InTune](./media/log-analytics-surface-hubs/intune.png)

Następnie InTune synchronizuje ustawienia usługi OMS z urządzeniami w grupie docelowej rejestrowania ich w obszarze roboczym usługi OMS.

## <a name="connect-surface-hubs-to-oms-using-the-settings-app"></a>Koncentratory powierzchni nawiązać połączenie przy użyciu aplikacji ustawień usługi OMS

W obszarze roboczym usługi OMS, która będzie zarządzać koncentratorów z powierzchni musisz identyfikator obszaru roboczego i klucz obszaru roboczego. Możesz przejść z portalem usługi OMS.

Jeśli nie korzystasz z usługi do środowiska zarządzania mogą rejestrować urządzeń ręcznie za pomocą **ustawień** w Centrum każdej powierzchni:

1. Korzystając z Twoim Centrum powierzchni Otwórz **Ustawienia**.
2. Wprowadź poświadczenia administratora urządzenia po wyświetleniu monitu.
3. Kliknij **urządzenie**, a w obszarze **monitorowania**, kliknij pozycję **Konfigurowanie ustawień usługi OMS**.
4. Zaznacz opcję **Włącz monitorowania**.
6. W oknie dialogowym Ustawienia usługi OMS wpisz **Identyfikator obszaru roboczego** i wpisz **Klucz obszaru roboczego**.  
  ![Ustawienia](./media/log-analytics-surface-hubs/settings.png)
7. Kliknij przycisk **OK** , aby zakończyć konfigurację.

Pojawia się potwierdzenie informujący o tym, czy konfiguracja usługi OMS pomyślnie została zastosowana do urządzenia. Jeśli został on pojawi się komunikat z informacją, że agent pomyślnie połączony usługę. Następnie urządzenie zacznie wysyłanie danych do usługi OMS miejsce, w którym można przeglądać i pracy nad nim.

## <a name="monitor-surface-hubs"></a>Monitorowanie koncentratory powierzchniowy

Monitorowanie usługi koncentratory powierzchni przy użyciu usługi OMS jest monitorowanie innych urządzeń zarejestrowany.

1. Zaloguj się do portalu usługi OMS.
2. Przejdź do Centrum powierzchni pack rozwiązanie z pulpitu nawigacyjnego.
3. Kondycja na urządzeniu jest wyświetlany.

  ![Powierzchniowy Centrum pulpitu nawigacyjnego](./media/log-analytics-surface-hubs/surface-hub-dashboard.png)

Można tworzyć [alerty](log-analytics-alerts.md) według istniejącego lub niestandardowego wyszukiwania dziennika. Korzystając z danymi, które usługi OMS zbiera z Twojej koncentratory powierzchni, można wyszukiwać problemy i alert na warunkach zdefiniowanych dla urządzeń.


## <a name="next-steps"></a>Następne kroki

- Umożliwia wyświetlanie szczegółowych danych powierzchni centrum [wyszukiwania dziennika analizy dziennika](log-analytics-log-searches.md) .
- Tworzenie [alertów](log-analytics-alerts.md) o osiągnięciu występują problemy z Twojej koncentratory powierzchni.
