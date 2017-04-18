<properties 
   pageTitle="Wyświetlanie i Zarządzanie alertami tablicy Virtual StorSimple | Microsoft Azure"
   description="W tym artykule opisano tablicy Virtual StorSimple warunków alertów i ważności i jak używać usługi Menedżera StorSimple do zarządzania alertami."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/07/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-view-and-manage-alerts-for-the-storsimple-virtual-array"></a>Wyświetlanie i Zarządzanie alertami dla macierzy Virtual StorSimple za pomocą usługi Menedżera StorSimple

## <a name="overview"></a>Omówienie

Kartę **alerty** w usłudze Menedżer StorSimple umożliwia przeglądanie i wyczyść związane z tablicy Virtual StorSimple alertów na podstawie w czasie rzeczywistym. Z poziomu tej karty można monitorować centralnie kondycją StorSimple macierzy wirtualnych (nazywane także StorSimple lokalnego urządzenia wirtualne) i ogólne rozwiązanie Microsoft Azure StorSimple.

Ten samouczek w tym artykule opisano sposób konfigurowania alertów, typowych warunków alertów, poziomy ważności alertu i jak przeglądać i śledzenie alertów. Ponadto zawiera alert podręczny wykaz tabel, które pozwalają szybko zlokalizować określonego alertu i odpowiednio odpowiadać.

![Strony alerty](./media/storsimple-ova-manage-alerts/alerts1.png)

## <a name="configure-alert-settings"></a>Konfigurowanie ustawień alertów

Możesz wybrać, czy chcesz otrzymywać powiadomienia pocztą e-mail alertu warunków dla każdego urządzenia wirtualnego StorSimple. Ponadto Adresaci alertów można określić, wprowadzając ich adresy e-mail w polu **innych adresatów wiadomości e-mail** , rozdzielając je średnikami.

>[AZURE.NOTE] Można wprowadzić maksymalnie 20 adresów e-mail na urządzenie wirtualne.

Po włączeniu powiadomienia e-mail dla urządzenia wirtualnego członków listy powiadomień otrzymają wiadomość e-mail z każdym alert krytyczny wystąpieniu. Wysyłane z *storsimple-alerts-noreply@mail.windowsazure.com* i opisując alert warunku. Adresaci mogą kliknij pozycję **Anuluj subskrypcję** , aby usunąć się z listy powiadomień e-mail.

#### <a name="to-enable-email-notification-of-alerts-for-a-virtual-device"></a>Aby włączyć powiadomienia e-mail alertów dla urządzenia wirtualnego

1. Przejdź do pozycji **urządzenia** > **Konfiguracja** urządzenia wirtualnego. Przejdź do sekcji **ustawień alertów** .

    ![Ustawienia alertów](./media/storsimple-ova-manage-alerts/alerts2.png)

2. W obszarze **Ustawienia alertów**ustaw następujące opcje:

    1. W polu **Wyślij powiadomienie e-mail** wybierz pozycję **Tak**.

    2. W polu **Administratorzy usługi poczty E-mail** wybierz pozycję **Tak** , jeśli chcesz mieć administrator usługi i wszystkich administratorów współpracujących odbierać powiadomienia alertów.

    3. W polu **innych adresatów wiadomości e-mail** wpisz adresy e-mail wszystkich adresatów, którzy mają otrzymywać powiadomienia alertów. Wprowadź nazwy w formacie *someone@somewhere.com*. Oddzielanie adresy e-mail za pomocą średnikami. Można skonfigurować maksymalnie 20 adresów e-mail na urządzenie wirtualne. 

        ![Konfiguracja powiadomień alertów](./media/storsimple-ova-manage-alerts/alerts3.png)

3. U dołu strony kliknij przycisk **Zapisz** , aby zapisać konfigurację.

4. Aby wysłać powiadomienie e-mail test, kliknij ikonę strzałki obok przycisku **Wyślij wiadomość e-mail test**. Usługa Menedżera StorSimple będą wyświetlane komunikaty o stanie przekazuje powiadomienia test. 

5. Gdy zostanie wyświetlony następujący komunikat, kliknij **przycisk OK**. 

    ![Alerty testowanie wysłana wiadomość e-mail z powiadomieniem](./media/storsimple-ova-manage-alerts/alerts4.png)

    >[AZURE.NOTE] Jeśli nie można wysłać wiadomości z powiadomieniem o test, usługa Menedżera StorSimple wyświetli odpowiedni komunikat. Kliknij **przycisk OK**, poczekaj kilka minut i spróbuj ponownie wysłać wiadomość powiadomienie test. 

    Wiadomość z powiadomieniem test będzie podobny do następującego.

    ![Alerty przetestować przykład wiadomości e-mail](./media/storsimple-ova-manage-alerts/alerts5.png)

## <a name="common-alert-conditions"></a>Typowe warunki alertów

Wirtualna macierzy StorSimple generuje alerty w odpowiedzi na szereg warunków. Poniżej przedstawiono najczęstsze typy alertów warunków:

- **Problemy z łącznością** — te alerty występują w przypadku trudności z przesyłania danych. Problemów z komunikacją mogą występować podczas przekazywania danych do i z konta Azure miejsca do magazynowania lub z powodu braku łączności między urządzeń wirtualnych i usługa Menedżer StorSimple. Problemy dotyczące komunikacji przedstawiono kilka najtrudniejsze rozwiązać problem, ponieważ istnieje wiele punktów awarii. Zawsze najpierw należy sprawdzić połączenie sieciowe i dostęp do Internetu są dostępne przed kontynuowaniem było bardziej zaawansowane rozwiązywanie problemów. Uzyskać informacji na temat portów zapory i ustawień przejdź do [tablicy Virtual StorSimple wymagania systemowe](storsimple-ova-system-requirements.md). Aby uzyskać pomoc w rozwiązywaniu problemów przejdź do [Rozwiązywanie problemów z polecenia cmdlet Testuj połączenie](storsimple-troubleshoot-deployment.md).

- **Problemy z wydajnością** — te alerty są spowodowane przeprowadzając systemu nie jest optymalnie, takich jak kiedy jest przy dużym obciążeniu.

Ponadto może zostać wyświetlony alertów dotyczących zabezpieczeń, aktualizacji lub błędy zadań.

## <a name="alert-severity-levels"></a>Poziom ważności alertu

Alerty mają różne poziomy ważności, w zależności od wpływu, jaki ma sytuację alertów i potrzebę odpowiedź na wpis. Poziomy ważności są następujące:

- **Krytyczne** — ten alert znajduje się w odpowiedzi na warunku, który ma wpływ na pomyślne wydajność systemu. Aby zapewnić, że StorSimple ciągłość usługi są wymagane działania.

- **Ostrzeżenie dotyczące** — ten warunek może stać się krytyczne, gdy nie można rozpoznać. Należy Zbadaj sytuację i podejmować żadnych działań wymagane wyczyszczenie problem.

- **Informacje o** — ten alert zawiera informacje, które mogą być przydatne w śledzenia i zarządzania systemem.

## <a name="view-and-track-alerts"></a>Wyświetlanie i śledzenie alertów

Pulpit nawigacyjny usługi Menedżera StorSimple pozwala szybko podglądać liczbę alertów na urządzeniach wirtualnych rozmieszczone według poziom ważności.

![Pulpit nawigacyjny alertów](./media/storsimple-ova-manage-alerts/alerts6.png)

Kliknięcie przycisku poziom ważności otwarcia kartę **alerty** . Wyniki obejmują tylko alerty, które są zgodne z tym samym poziomie ważności.

![Raport alerty występujące w typ alertu](./media/storsimple-ova-manage-alerts/alerts7.png)

Kliknięcie alertu na liście oferuje dodatkowe szczegóły alertu, łącznie z ostatniego Zgłoszono alert, liczba wystąpień alertu na urządzenie i zalecane działanie, aby rozwiązać alert.

Jeśli chcesz wysłać informacje do firmy Microsoft Support, możesz skopiować szczegóły alertu do pliku tekstowego. Po obserwowane zalecenia i rozpoznawania alert warunek lokalnego, należy wyczyścić alertu z zaznaczając alert na karcie **alertów** i klikając przycisk **Wyczyść**. Aby wyczyścić wiele alertów, zaznacz każdy alert, kliknij wszystkich kolumn z wyjątkiem kolumny **Alert** , a po wybraniu można wyczyścić wszystkie alerty kliknij przycisk **Wyczyść** . Należy zauważyć, że niektóre alerty są automatycznie wyczyszczone, gdy problem zostanie rozwiązany lub gdy aktualizowana przez system alertu o nowe informacje.

Po kliknięciu przycisku **Wyczyść**będą mieć możliwość komentarz alertu i działania, które miały Aby rozwiązać ten problem. 

![alert komentarzy](./media/storsimple-ova-manage-alerts/clear-alert.png)

Kliknij ikonę wyboru ![Ikona wyboru](./media/storsimple-ova-manage-alerts/check-icon.png) Aby zapisać swoje komentarze.

Niektóre zdarzenia zostanie usunięty przez system, jeśli inne zdarzenie zostanie wywołana przy użyciu nowych informacji. W takim przypadku zostanie wyświetlony poniższy komunikat.

![Komunikat alertu wyczyść pole wyboru](./media/storsimple-ova-manage-alerts/alerts8.png)

## <a name="sort-and-review-alerts"></a>Sortowanie i recenzowanie alertów

Karta **alertów** można wyświetlić alerty do 250. Po przekroczeniu tej liczby alertów, nie wszystkie alerty będą wyświetlane w widoku domyślnym. Można połączyć Dostosowywanie alerty są wyświetlane następujące pola:

- **Stan** — można wyświetlać alerty **aktywnej** lub **wyczyszczone** . Aktywne nadal jest alertów w systemie, gdy wyczyszczone alerty zostały albo ręcznie wyczyszczone przez administratora lub programowy wyczyszczone, ponieważ system zaktualizowane alert warunek przy użyciu nowych informacji.

- **Ważności** — można wyświetlać alerty wszystkie poziomy ważności (krytyczne, ostrzeżenie, informacje o) lub tylko niektórych ważności, takie jak tylko krytyczne alerty.

- **Źródła** — wyświetlaj alertów ze wszystkich źródeł lub ograniczyć alerty do tych, które pochodzą z jednego lub wszystkich urządzeń wirtualnych lub usługi.

- **Zakres czasu** —, określając **od** i **do** daty i sygnatury czasowe, możesz obejrzeć alerty w tym okresie, który Cię interesuje.

## <a name="alerts-quick-reference"></a>Podręczna karta informacyjna alertów

W poniższej tabeli wymieniono niektóre alerty Microsoft Azure StorSimple, które można napotkać, a także dodatkowe informacje i zalecenia w przypadku gdy dostępne. Alerty urządzenia wirtualnego StorSimple należą do jednego z następujących kategorii:

- [Alerty łączności chmury](#cloud-connectivity-alerts)

- [Konfiguracja alertów](#configuration-alerts)

- [Alerty dotyczące błędów zadań](#job-failure-alerts)

- [Alerty wydajności](#performance-alerts)

- [Alerty zabezpieczeń](#security-alerts)

- [Informacje o aktualizacjach](#update-alerts)

### <a name="cloud-connectivity-alerts"></a>Alerty łączności chmury

|Tekst alertu|Zdarzenia|Więcej informacji / polecane akcje|
|:---|:---|:---|
|Urządzenie *<device name>* nie jest podłączony do chmury.|Nazwanego urządzenia nie można połączyć w chmurze. |Nie można nawiązać połączenia z chmury. Może to być spowodowane jedną z następujących czynności:<ul><li>Czasami może występować problem z ustawieniami sieci na urządzeniu.</li><li>Czasami może występować problem z magazynu poświadczeń konta.</li></ul>Aby uzyskać więcej informacji na temat rozwiązywania problemów z łącznością przejdź do [lokalnych web interfejsu użytkownika](storsimple-ova-web-ui-admin.md) urządzenia.|


### <a name="configuration-alerts"></a>Konfiguracja alertów

|Tekst alertu|Zdarzenia|Więcej informacji / zalecane akcje|
|:---|:---|:---|
|Konfiguracja urządzenia wirtualnego lokalnego nieobsługiwane.|Spadek wydajności.|Bieżąca konfiguracja może spowodować spadek wydajności. Upewnij się, że serwer spełnia wymagania minimalne konfiguracji. Aby uzyskać więcej informacji przejdź do [Wirtualnego wymagania tablicy StorSimple](storsimple-ova-system-requirements.md). 
|Korzystasz z miejsca na dysku ustanawianie <*Nazwa urządzenia*>.|Ostrzeżenie dotyczące miejsca na dysku.|Możesz zaczyna brakować miejsca na dysku ustanawianie. Aby zwolnić miejsce, rozważ przeniesienie obciążenia do innego woluminu lub udostępnianie lub usuwanie danych.

### <a name="job-failure-alerts"></a>Alerty dotyczące błędów zadań

|Tekst alertu|Zdarzenia|Więcej informacji / polecane akcje|
|:---|:---|:---|
|Nie można ukończyć wykonywanie kopii zapasowych <*Nazwa urządzenia*>.|Błąd zadania wykonywania kopii zapasowej.|Nie można utworzyć kopię zapasową. Należy rozważyć jedną z następujących czynności:<ul><li>Problemy z łącznością może uniemożliwiać wykonywanie kopii zapasowej na ukończenie pomyślnie. Upewnij się, czy nie ma problemów z łącznością. Aby uzyskać więcej informacji na temat rozwiązywania problemów z łącznością przejdź do [lokalnych web interfejsu użytkownika](storsimple-ova-web-ui-admin.md) dla swojego urządzenia wirtualnego.</li><li>Osiągnięto limit magazynowania danych. Aby zwolnić miejsce, należy rozważyć usunięcie żadnej kopii zapasowej, które nie są już potrzebne.</li></ul> Rozwiązywanie problemów, wyczyść alertu i spróbuj jeszcze raz.|
|Nie można ukończyć przywracania <*Nazwa urządzenia*>.|Przywracanie błąd zadania.|Nie można przywrócić z kopii zapasowej. Należy rozważyć jedną z następujących czynności:<ul><li>Na liście kopii zapasowej mogą być nieprawidłowe. Odśwież listę, aby sprawdzić, czy jest nadal ważna.</li><li>Problemy z łącznością może uniemożliwiać pomyślne wykonanie operacji przywracania. Upewnij się, czy nie ma problemów z łącznością.</li><li>Osiągnięto limit magazynowania danych. Aby zwolnić miejsce, należy rozważyć usunięcie żadnej kopii zapasowej, które nie są już potrzebne.</li></ul>Rozwiązywanie problemów, wyczyść alertu i ponowić próbę przywracania.|
|Nie można ukończyć klonowanie <*Nazwa urządzenia*>.|Klonowanie błąd zadania.|Nie można utworzyć klonowanie. Należy rozważyć jedną z następujących czynności:<ul><li>Na liście kopii zapasowej mogą być nieprawidłowe. Odśwież listę, aby sprawdzić, czy jest nadal ważna.</li><li>Problemy z łącznością może uniemożliwiać pomyślne ukończenie operacji klonowanie. Upewnij się, czy nie ma problemów z łącznością.</li><li>Osiągnięto limit magazynowania danych. Aby zwolnić miejsce, należy rozważyć usunięcie żadnej kopii zapasowej, które nie są już potrzebne.</li></ul>Rozwiązywanie problemów, wyczyść alertu i spróbuj jeszcze raz.|

### <a name="performance-alerts"></a>Alerty wydajności

|Tekst alertu|Zdarzenia|Więcej informacji / zalecane akcje|
|:---|:---|:---|
|Występują nieoczekiwane opóźnienia transferu danych.|Transfer danych działa wolno.|Ograniczanie błędów po przekroczeniu celów skalowalność Usługa magazynu. Usługa magazynu oznacza to, aby upewnić się, że nie pojedynczego klienta lub dzierżawy za pomocą usługi koszt innych osób. Uzyskać więcej informacji dotyczących rozwiązywania problemów z Twojego konta Azure miejsca do magazynowania przejdź do [monitora, diagnozowanie i rozwiązywanie problemów z magazynem tabel platformy Microsoft Azure](storage-monitoring-diagnosing-troubleshooting.md).
|Możesz mało miejsca na dysku lokalnym zastrzeżenie <*Nazwa urządzenia*>.|Długi czas odpowiedzi.|10% całkowity rozmiar ustanawianie <*Nazwa urządzenia*> zarezerwowana na urządzeniem lokalnym i możesz teraz zaczyna brakować miejsca zastrzeżone. Obciążenie pracą na <*Nazwa urządzenia*> generuje wyższym pochodząca lub w przypadku może ostatnio migracji dużych ilości danych. To może spowodować ograniczenie wydajności. Należy rozważyć jedną z następujących czynności, aby rozwiązać ten problem:<ul><li>Zwiększanie przepustowości chmury do tego urządzenia.</li><li>Zmniejszanie lub Przenieś obciążenia do innego woluminu lub udostępnianie.</li></ul>

### <a name="security-alerts"></a>Alerty zabezpieczeń

|Tekst alertu|Zdarzenia|Więcej informacji / polecane akcje|
|:---|:---|:---|
|Hasło dla <*Nazwa urządzenia*> wygaśnie w <*numer*> dni.|Ostrzeżenie o hasło.| Twoje hasło wygaśnie w < numer < dni. Należy rozważyć zmianę hasła. Aby uzyskać więcej informacji przejdź do sekcji [Zmienianie hasło administratora urządzenia StorSimple wirtualnych tablicy](storsimple-ova-change-device-admin-password.md).

### <a name="update-alerts"></a>Informacje o aktualizacjach

|Tekst alertu|Zdarzenia|Więcej informacji / zalecane akcje|
|:---|:---|:---|
|Nowe aktualizacje są dostępne dla danego urządzenia.|Aktualizacje do tablicy, Virtual StorSimple są dostępne.|Nowe aktualizacje można zainstalować ze strony **konserwacji** .|
|Nie można Skanuj w poszukiwaniu nowych aktualizacji <*Nazwa urządzenia*>.|Aktualizowanie błąd. |Wystąpił błąd podczas instalowania nowych aktualizacji. Można ręcznie zainstaluj aktualizacje. Jeśli problem będzie nadal występował, skontaktuj się z [Pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md).|


## <a name="next-steps"></a>Następne kroki

- [Więcej informacji na temat tablicy Virtual StorSimple](storsimple-ova-overview.md).


