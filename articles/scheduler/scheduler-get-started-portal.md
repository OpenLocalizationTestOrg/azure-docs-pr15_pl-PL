<properties
 pageTitle="Rozpoczynanie pracy z harmonogramem Azure w Azure portal | Microsoft Azure"
 description="Rozpoczynanie pracy z harmonogramem Azure w Azure portal"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="hero-article"
 ms.date="08/10/2016"
 ms.author="deli"/>

# <a name="get-started-with-azure-scheduler-in-azure-portal"></a>Rozpoczynanie pracy z harmonogramem Azure w Azure portal

Jest łatwe tworzenie zadań zaplanowanych w harmonogramie Azure. W tym samouczku dowiesz się, jak utworzyć zadanie. Dowiesz się możliwości monitorowania i zarządzania harmonogramu.

## <a name="create-a-job"></a>Tworzenie zadania

1.  Zaloguj się do [portalu Azure](https://portal.azure.com/).  

2.  Kliknij pozycję **+ Nowe** > w polu wyszukiwania wpisz ciąg _Harmonogram_ > Wybierz pozycję **Harmonogram** w wynikach > kliknij przycisk **Utwórz**.

     ![][marketplace-create]

3.  Tworzenie zadania, które po prostu trafienia http://www.microsoft.com/ z żądania GET. Na ekranie **Zadanie harmonogramu** wprowadź następujące informacje:

    1.  **Nazwa:**`getmicrosoft`  

    2.  **Subskrypcji:** Twoja subskrypcja Azure   

    3.  **Zadania zbioru:** Wybierz istniejącą kolekcję zadania lub kliknij pozycję **Utwórz nowy** > Wprowadź nazwę.

4.  Następnie w obszarze **Ustawienia akcji**, określ następujące wartości:

    1.  **Typ akcji:**` HTTP`  

    2.  **Metoda:**`GET`  

    3.  **Adres URL:**` http://www.microsoft.com`  

      ![][action-settings]

5.  Na koniec załóżmy definiowanie serii rozłożonych w czasie. Zadania można zdefiniować jako jednorazowego zadania, ale Przejdźmy wybierz harmonogram cyklu:

    1. **Cykl**:`Recurring`

    2. **Rozpoczynanie**: dzisiejsza data

    3. **Cykliczności co**:`12 Hours`

    4. **Zakończenie do**: dwa dni od dnia bieżącego użytkownika daty  

      ![][recurrence-schedule]

6.  Kliknij przycisk **Utwórz**

## <a name="manage-and-monitor-jobs"></a>Monitorowanie zadań i zarządzanie nimi

Po utworzeniu zadania jest wyświetlany na głównym Azure pulpitu nawigacyjnego. Kliknij zadanie i zostanie otwarte nowe okno z następujących kartach:

1.  Właściwości  

2.  Ustawienia akcji  

3.  Harmonogram  

4.  Historia

5.  Użytkownicy

    ![][job-overview]

### <a name="properties"></a>Właściwości

Te właściwości tylko do odczytu opisują metadanych zarządzania dla zadania harmonogramu.

   ![][job-properties]


### <a name="action-settings"></a>Ustawienia akcji

Kliknięcie przycisku zadanie na ekranie **zadań** umożliwia konfigurowanie tego zadania. W ten sposób można skonfigurować ustawienia zaawansowane, jeśli nie skonfigurowano je w szybkie tworzenie kreatora.

Dla wszystkich typów akcji możesz zmienić zasady ponów próbę i akcja błędu.

Typu HTTP i HTTPS Akcja zadania możesz zmienić metodę dowolnego dozwolonych zlecenia HTTP. Możesz również dodać, usunąć lub zmienić nagłówki i uwierzytelnianie podstawowe informacje.

Dla typów akcji kolejki miejsca do magazynowania możesz zmienić konto miejsca do magazynowania, nazwa kolejki, token skojarzeń zabezpieczeń i treści.

Dla typów akcji bus usług możesz zmienić przestrzeń nazw ścieżki tematu i kolejki, ustawienia uwierzytelniania, typ transportu, właściwości wiadomości i treść wiadomości.

   ![][job-action-settings]

### <a name="schedule"></a>Harmonogram

W ten sposób można skonfigurować harmonogram, jeśli chcesz zmienić harmonogram został utworzony w szybkie tworzenie kreatora.

To jest możliwość tworzenia [złożonych harmonogramów i zaawansowane cyklu w swojej pracy](scheduler-advanced-complexity.md)

Możesz zmienić datę rozpoczęcia i czas, cykl harmonogram i końcu Data i godzina (Jeśli to zadanie jest cykliczne.)

   ![][job-schedule]


### <a name="history"></a>Historia

Karta **Historia** wyświetla zaznaczonego metryki dla każdej wykonywania zadań w systemie dla wybranego zadania. Te metryki wprowadź wartości w czasie rzeczywistym dotyczące kondycji usługi harmonogramu:

1.  Stan  

2.  Szczegóły  

3.  Ponowne próby

4.  Wystąpienie: 1st, 2nd i 3rd, itp.

5.  Czas rozpoczęcia wykonania  

6.  Czas zakończenia działania

   ![][job-history]

Możesz kliknąć Uruchom, aby wyświetlić jego **Szczegóły historii**, łącznie z całego odpowiedzi na każdym wykonaniu. To okno dialogowe umożliwia także skopiować dane do Schowka.

   ![][job-history-details]

### <a name="users"></a>Użytkownicy

Azure oparta na rolach programu Access Control (RBAC) umożliwia zarządzanie dostępem szerokiego Azure harmonogramu. Aby dowiedzieć się, jak użyć karty użytkowników, należy zapoznać się z [Azure Role-Based kontroli dostępu](../active-directory/role-based-access-control-configure.md)


## <a name="see-also"></a>Zobacz też

 [Co to jest harmonogram?](scheduler-intro.md)

 [Harmonogram pojęcia, terminologia i hierarchia jednostki](scheduler-concepts-terms.md)

 [Plany i rozliczenia w harmonogramie Azure](scheduler-plans-billing.md)

 [Sposoby tworzenia złożonych harmonogramów i zaawansowane cyklu z harmonogramem Azure](scheduler-advanced-complexity.md)

 [Harmonogram odwołanie interfejsu API usługi REST](https://msdn.microsoft.com/library/mt629143)

 [Dokumentacja dotycząca poleceń cmdlet środowiska PowerShell harmonogramu](scheduler-powershell-reference.md)

 [Harmonogram dostępności i niezawodności](scheduler-high-availability-reliability.md)

 [Limity harmonogram, wartości domyślne i kody błędów](scheduler-limits-defaults-errors.md)

 [Harmonogram uwierzytelniania ruchu wychodzącego](scheduler-outbound-authentication.md)


[marketplace-create]: ./media/scheduler-get-started-portal/scheduler-v2-portal-marketplace-create.png
[action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-action-settings.png
[recurrence-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-recurrence-schedule.png
[job-properties]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-properties.png
[job-overview]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-overview-1.png
[job-action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-action-settings.png
[job-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-schedule.png
[job-history]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history.png
[job-history-details]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history-details.png


[1]: ./media/scheduler-get-started-portal/scheduler-get-started-portal001.png
[2]: ./media/scheduler-get-started-portal/scheduler-get-started-portal002.png
[3]: ./media/scheduler-get-started-portal/scheduler-get-started-portal003.png
[4]: ./media/scheduler-get-started-portal/scheduler-get-started-portal004.png
[5]: ./media/scheduler-get-started-portal/scheduler-get-started-portal005.png
[6]: ./media/scheduler-get-started-portal/scheduler-get-started-portal006.png
[7]: ./media/scheduler-get-started-portal/scheduler-get-started-portal007.png
[8]: ./media/scheduler-get-started-portal/scheduler-get-started-portal008.png
[9]: ./media/scheduler-get-started-portal/scheduler-get-started-portal009.png
[10]: ./media/scheduler-get-started-portal/scheduler-get-started-portal010.png
[11]: ./media/scheduler-get-started-portal/scheduler-get-started-portal011.png
[12]: ./media/scheduler-get-started-portal/scheduler-get-started-portal012.png
[13]: ./media/scheduler-get-started-portal/scheduler-get-started-portal013.png
[14]: ./media/scheduler-get-started-portal/scheduler-get-started-portal014.png
[15]: ./media/scheduler-get-started-portal/scheduler-get-started-portal015.png
