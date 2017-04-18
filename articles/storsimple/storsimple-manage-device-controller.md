<properties
   pageTitle="Zarządzanie kontrolery urządzeń StorSimple | Microsoft Azure"
   description="Dowiedz się, jak zatrzymać, uruchom ponownie, zamknij lub resetowanie kontrolerach urządzenia StorSimple."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/11/2016"
   ms.author="alkohli" />

# <a name="manage-your-storsimple-device-controllers"></a>Zarządzanie kontrolerach urządzenia StorSimple

## <a name="overview"></a>Omówienie

Ten samouczek zawiera opis różnych operacji, które mogą być wykonywane na kontrolerach urządzenia StorSimple. Kontrolery w urządzeniu StorSimple są zbędne (funkcja) kontrolery w konfiguracji aktywny pasywne. W danej chwili tylko jeden kontroler jest aktywna i przetwarza wszystkie operacje dysku i sieci. Drugi kontroler znajduje się w stronie biernej tryb. Jeśli active kontrolera kończy się niepowodzeniem, pasywne kontroler automatycznie staje się aktywne.

Ten samouczek zawiera instrukcje krok po kroku, aby zarządzać kontrolerów urządzeń za pomocą:

- Sekcja **kontrolery** strony **konserwacji** w usłudze Menedżer StorSimple
- Windows PowerShell dla StorSimple.

Firma Microsoft zaleca zarządzania kontrolery urządzeń za pośrednictwem usługi Menedżera StorSimple. Jeśli akcji można wykonać tylko przy użyciu programu Windows PowerShell dla StorSimple, samouczka sprawia, że je zapisać.

Po przeczytaniu tego samouczka, będą mogli:

- Ponownie uruchomić lub zamknąć kontrolera urządzenia StorSimple
- Zamknięcia urządzenia StorSimple
- Przywrócić domyślne ustawienia fabryczne urządzenia StorSimple


## <a name="restart-or-shut-down-a-single-controller"></a>Ponownie uruchomić lub zamknąć pojedynczy kontroler

Zamknięcie lub ponowne uruchomienie kontrolera nie jest wymagane jako część normalnego działania. Operacje zamknięcia na kontrolerze jedno urządzenie występują tylko w przypadkach, w których wymagana jest wymiana składnik sprzętu urządzenia nie powiodło się. Ponowne uruchomienie kontrolera może również być wymagane w sytuacji, w której wydajności dotyczy nadmiarowe pamięć lub kontroler nieprawidłowo. Również konieczne może być ponowne uruchomienie kontrolera po zastąpienie kontrolera pomyślnie, jeśli chcesz włączyć i przetestować zamienionego kontrolera.

Ponowne uruchamianie urządzenia nie jest przerw w pracy inicjator połączenia, przy założeniu, że w stronie biernej kontroler jest dostępny. Jeśli w stronie biernej kontroler nie jest dostępna lub wyłączony wyłączone, a następnie ponownego uruchomienia active kontrolera może spowodować przerwanie usługi i przestoje.

> [AZURE.IMPORTANT]

> - **Kontroler uruchomionego powinien nigdy nie zostać usunięty jako to może spowodować utratę nadmiarowości i zwiększone ryzyko przestoje.**

> - Poniższa procedura dotyczy tylko urządzenia fizycznego StorSimple. Aby dowiedzieć się, jak uruchomić, zatrzymać i uruchom ponownie urządzenie wirtualne Zobacz [Praca z urządzenia wirtualnego](storsimple-virtual-device-u2.md#work-with-the-storsimple-virtual-device).

Możesz ponownie uruchomić lub zamknąć kontrolerze jednego urządzenia za pomocą portalu klasyczny Azure usługa Menedżera StorSimple lub środowiska Windows PowerShell dla StorSimple

Aby zarządzać kontrolerach urządzenia z portalem klasyczny Azure, wykonaj następujące czynności.

#### <a name="to-restart-or-shut-down-a-controller-in-classic-portal"></a>Aby ponownie uruchomić lub zamknąć kontrolera w klasycznym portalu

1. Przejdź do **urządzeń > Konserwacja**.

1. Przejdź do **Stanu sprzętu** i sprawdź, czy stan kontroler na urządzeniu jest **dobrej kondycji**.

    ![Sprawdź, czy kontrolery urządzeń StorSimple są prawidłowy](./media/storsimple-manage-device-controller/IC766017.png)

1. U dołu strony **konserwacji** kliknij pozycję **Zarządzanie kontrolerów**.

    ![Zarządzanie kontrolerami urządzenia StorSimple](./media/storsimple-manage-device-controller/IC766018.png)</br>

    >[AZURE.NOTE] Jeśli nie widzisz **Zarządzanie kontrolerów**, należy zainstalować aktualizacje. Aby uzyskać więcej informacji zobacz [Aktualizowanie urządzenia StorSimple](storsimple-update-device.md).

1. W oknie dialogowym **Zmienianie ustawień kontrolerze** wykonaj następujące czynności:
    1. Z listy rozwijanej **Wybierz kontrolera** Wybierz kontroler, który chcesz zarządzać. Opcje są kontrolera 0 i 1 kontrolera. Tych kontrolerów są również identyfikowane jako aktywne lub pasywne.

        >[AZURE.NOTE] Kontroler nie mogą być zarządzane, jeśli jest niedostępny lub wyłączony wyłączone i nie będzie wyświetlany na liście rozwijanej.

    2. Z listy rozwijanej **Wybierz pozycję Akcja** wybierz pozycję **Uruchom ponownie kontrolera** lub **Zamknij kontrolera**.

        ![Uruchom ponownie kontrolera pasywne urządzenia StorSimple](./media/storsimple-manage-device-controller/IC766020.png)
    3. Kliknij ikonę wyboru ![Ikona modułu sprawdzania](./media/storsimple-manage-device-controller/IC740895.png).

To zostanie ponownie uruchomić lub zamknąć administratora. W poniższej tabeli przedstawiono szczegółowe informacje, co się dzieje w zależności od wybrane w oknie dialogowym **Ustawienia kontrolera Zmień** opcje.  


|Zaznaczenie #|Jeśli wybierzesz...|Dzieje się tak.|
|---|---|---|
|1.|Uruchom ponownie kontroler pasywne.|Zadanie zostanie utworzony go ponownie na kontrolerze, a otrzymasz powiadomienie po pomyślnym utworzeniu zadania. Inicjowanie konwersacji po ponownym uruchomieniu komputera kontroler. Można monitorować procesu ponownego po zalogowaniu się do **usługi > pulpit nawigacyjny > Wyświetlanie dzienników operacji** , a następnie filtrowania według parametrów specyficzne dla tej usługi.|
|2.|Uruchom ponownie kontrolera aktywna.|Pojawi się następujące ostrzeżenie: "po ponownym uruchomieniu kontroler aktywne urządzenie zakończy się niepowodzeniem na administratorowi pasywne. Czy chcesz kontynuować?" </br>Jeśli chcesz kontynuować operację, następne kroki będą identycznych do tych używanych go ponownie na stronie biernej kontrolerze (zobacz zaznaczenia 1).|
|3.|Zamknij kontrolera pasywne.|Zostanie wyświetlony następujący komunikat: "po zakończeniu zamykania należy naciśnij przycisk power na kontrolerze, aby ją włączyć. Czy na pewno chcesz zamknąć ten kontroler?" </br>Jeśli chcesz kontynuować operację, następne kroki będą identycznych do tych używanych go ponownie na stronie biernej kontrolerze (zobacz zaznaczenia 1).|
|4.|Zamknięcie aktywnego kontrolera.|Zostanie wyświetlony następujący komunikat: "po zakończeniu zamykania należy naciśnij przycisk power na kontrolerze, aby ją włączyć. Czy na pewno chcesz zamknąć ten kontroler?" </br>Jeśli chcesz kontynuować operację, następne kroki będą identycznych do tych używanych go ponownie na stronie biernej kontrolerze (zobacz zaznaczenia 1).|


#### <a name="to-restart-or-shut-down-a-controller-in-windows-powershell-for-storsimple"></a>Aby ponownie uruchomić lub zamknąć kontrolera w programie Windows PowerShell dla StorSimple
Wykonaj poniższe czynności, aby zamknąć lub ponownie uruchomić pojedynczy kontroler na urządzeniu StorSimple z portalu klasyczny Azure.


1. Dostęp do urządzenia przy użyciu konsoli szeregowego lub sesji telnet na komputerze zdalnym. Połącz kontrolerze 0 lub 1 kontrolerze, wykonując czynności opisane w [Kit Użyj nawiązywania połączenia z konsoli szeregowego urządzenia](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).

1. W menu Konsola szeregowa wybierz opcję 1, **Zaloguj się przy użyciu pełnego dostępu**.

1. W komunikacie transparent Zanotuj kontroler nawiązano (kontroler 0 lub 1 kontroler) i czy jest aktywny pasywne kontroler (wstrzymania).
    - Zamykanie pojedynczy kontroler, w wierszu polecenia wpisz:

        `Stop-HcsController`

        Kontroler, które są podłączone do zostanie zamknięty. Jeśli zatrzymasz active kontrolera następnie go zakończy się niepowodzeniem na na stronie biernej kontroler przed zamknięciem go.
    - Aby ponownie uruchomić kontrolera w wierszu polecenia wpisz:

        `Restart-HcsController`

        Uruchom ponownie kontroler, które są podłączone do. Po ponownym uruchomieniu active kontrolera go zakończy się niepowodzeniem na na stronie biernej kontroler przed po ponownym uruchomieniu komputera.


## <a name="shut-down-a-storsimple-device"></a>Zamknięcia urządzenia StorSimple

W tej sekcji wyjaśniono, jak zamknąć bieżącą lub urządzenie StorSimple nie powiodło się na komputerze zdalnym. Urządzenie jest wyłączone po zamykanie kontrolerów urządzeń. Zamknięcie urządzenie jest wykonane, gdy urządzenie jest przenoszony fizycznie lub pochodzi z usługi.

> [AZURE.IMPORTANT] Przed zamknięciem urządzenie sprawdzić kondycję składniki urządzenia. Przejdź do **urządzeń > konserwacja > Stan sprzętu** i sprawdź, czy stan LED wszystkie składniki jest zielony. Prawidłowy urządzenia będą mieć zielone stanu. Jeśli urządzenie jest zamknięty w celu Zamień składnik nieprawidłowo, pojawi się nie powiodło się (czerwony) lub ograniczone stan (żółte) dla odpowiednich składników.

#### <a name="to-shut-down-a-storsimple-device"></a>Aby zamknąć urządzenia StorSimple

1. Procedura [ponownego uruchomienia lub zamknięcia kontrolera](#restart-or-shut-down-a-single-controller) do identyfikowania i Zamknij w stronie biernej kontroler na urządzeniu. Operacja można wykonywać w portalu klasyczny Azure lub w programie Windows PowerShell dla StorSimple.
2. Powtórz krok powyżej Zamykanie aktywnej kontrolerze.
3. Konieczne będzie teraz przeglądać wstecz linii urządzenia. Po dwóch kontrolerów są całkowicie zamknięty, stan LED na obu kontrolerach należy miganie czerwony. Jeśli chcesz całkowicie wyłączyć urządzenia w tej chwili Przerzucanie przełączniki power zarówno na Power i chłodzenia modułów (PCMs) do pozycji wyłączone. Wyłączyć to urządzenie.


<!--#### To shut down a StorSimple device in Windows PowerShell for StorSimple

1. Connect to the serial console of the StorSimple device by following the steps in [Use PuTTY to connect to the device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-serial-console).

1. In the serial console menu, verify from the banner message that the controller you are connected to is the passive controller. If you are connected to the active controller, disconnect from this controller and connect to the other controller.

1. In the serial console menu, choose option 1, **log in with full access**.

1. At the prompt, type:

    `Stop-HCSController`

    This should shut down the current controller. To verify whether the shutdown has finished, check the back of the device. The controller status LED should be solid red.

1. Repeat steps 1 through 4 to connect to the active controller and then shut it down.

1. After both the controllers are completely shut down, the status LEDs on both should be blinking red. If you need to turn off the device completely at this time, flip the power switches on both Power and Cooling Modules (PCMs) to the OFF position.-->

## <a name="reset-the-device-to-factory-default-settings"></a>Przywrócić domyślne ustawienia fabryczne urządzenia

> [AZURE.IMPORTANT] Jeśli chcesz przywrócić domyślne ustawienia fabryczne urządzenia, skontaktuj się Support firmy Microsoft. Procedury opisane poniżej stosuje się tylko w połączeniu z Support firmy Microsoft.

Tej procedurze opisano sposób przywrócić domyślne ustawienia fabryczne przy użyciu programu Windows PowerShell dla StorSimple urządzenia Microsoft Azure StorSimple.
Resetowanie urządzenia spowoduje usunięcie wszystkich danych i ustawień z całego klastrze domyślne.

Wykonaj poniższe czynności, aby przywrócić domyślne ustawienia fabryczne urządzenia Microsoft Azure StorSimple:

### <a name="to-reset-the-device-to-default-settings-in-windows-powershell-for-storsimple"></a>Aby przywrócić ustawienia domyślne w programie Windows PowerShell dla StorSimple na urządzeniu

1. Dostęp do urządzenia w swojej szeregowego konsoli. Zaznacz wiadomość transparentu, aby upewnić się, że nawiązano do aktywnego kontrolera.

1. W menu Konsola szeregowa wybierz opcję 1, **Zaloguj się przy użyciu pełnego dostępu**.

1. W wierszu polecenia wpisz następujące polecenie, aby zresetować cały klaster, usuwając wszystkie ustawienia danych, metadanych i kontrolera:

    `Reset-HcsFactoryDefault`

    Aby zamiast tego zresetować pojedynczy kontroler, należy użyć polecenia cmdlet [Resetuj HcsFactoryDefault](http://technet.microsoft.com/library/dn688132.aspx) z `-scope` parametru.)

    Komputer uruchomi się ponownie kilka razy. Gdy pomyślnie Zresetuj, otrzymasz powiadomienie. W zależności od modelu system może potrwać 45-60 minut na urządzeniu z systemem 8100 i 60 90 minut na 8600 zakończyć ten proces.

    > [AZURE.TIP]

    > - Jeśli korzystasz z 1.2 aktualizacji lub starszym za pomocą `–SkipFirmwareVersionCheck` parametr, aby pominąć sprawdź wersję oprogramowania układowego (w przeciwnym razie wyświetlony zostanie błąd niezgodności sprzętowego: fabryczne nie może kontynuować z powodu niezgodności w wersjach oprogramowania układowego. ).

    > - Procedury Resetowanie factory może zakończyć się niepowodzeniem w przypadku urządzeń StorSimple, które działają aktualizacji 1 lub 1.1 w portalu dla instytucji rządowych i zlecić zastępuje pomyślnego kontroler pojedyncze lub podwójne (z kontrolerów wymiana, które zostały wysłane z oprogramowaniem sprzed aktualizacji 1). Dzieje, gdy resetowanie fabryki czy obraz jest sprawdzana obecność pliku SHA1 na kontrolerze, który nie istnieje dla oprogramowania 1 sprzed aktualizacji. Jeśli zostanie wyświetlony resetowania factory ten błąd, skontaktuj się Support firmy Microsoft, aby pomóc w następnych kroków. Ten problem nie jest widoczny z kontrolerów zamienny, które zostały wysłane z fabryki za pomocą aktualizacji 1 lub nowszy oprogramowania.


## <a name="questions-and-answers-about-managing-device-controllers"></a>Pytania i odpowiedzi dotyczące zarządzania kontrolery urządzeń

W tej sekcji, możemy podsumowane niektóre z często zadawanych pytań dotyczących zarządzania kontrolery urządzeń StorSimple.

**PYTANIA.** Co się stanie, jeśli kontrolery na moim urządzeniu jest prawidłowy i wyłączone na i I ponownie uruchomić lub zamknąć Aktywne kontroler?

**ODPOWIEDZI.** Jeśli kontrolery na urządzeniu jest prawidłowy i wyłączone, zostanie wyświetlony monit o potwierdzenie. Można wybrać opcję:

- **Uruchom ponownie kontrolera aktywne** — otrzymasz powiadomienie, że ponowne uruchomienie usługi active kontrolera spowoduje, że urządzenie nie w stronie biernej kontrolera. Kontroler uruchomi się ponownie.

- **Zamknięcie aktywnego kontroler** — otrzymasz powiadomienie, że Zamykanie aktywnej kontroler spowoduje przestoje. Konieczne będzie również naciśnij przycisk power na tym urządzeniu, aby włączyć funkcję administratora.

**PYTANIA.** Co się stanie, jeśli pasywne kontroler na Moje urządzenie jest niedostępne lub wyłączony wyłączone i I ponownie uruchomić lub zamknąć Aktywne kontroler?

**ODPOWIEDZI.** Jeśli pasywne kontroler na urządzeniu jest niedostępny lub wyłączony wyłączone, a użytkownik chce:

- **Uruchom ponownie kontrolera aktywne** — powiadomienie, że kontynuowanie tej operacji spowoduje tymczasowe przerwy w świadczeniu usługi i zostanie wyświetlony monit o potwierdzenie.

- **Zamknięcie aktywnego kontroler** — powiadomienie, że kontynuowanie tej operacji spowoduje przestoje i czy będzie konieczne naciśnięcie przycisku power na co najmniej obu kontrolerze, aby włączyć urządzenie. Pojawi się monit o potwierdzenie.

**PYTANIA.** Gdy powoduje zamknięcie lub ponowne uruchomienie kontrolera nie postępu?

**ODPOWIEDZI.** Ponowne uruchamianie i zamykanie kontrolera może się nie powieść, jeśli:

- Aktualizacja urządzenie jest w toku.

- Ponowne uruchomienie kontrolera jest już w toku.

- Zamknięcie kontroler jest już w toku.

**PYTANIA.** Jak można poznać Jeśli kontrolera został ponownie uruchomiony lub zamknięty?

**ODPOWIEDZI.** Możesz sprawdzić stan kontrolera na stronie konserwacja. Kontroler pokaże czy kontroler został ponownie uruchomiony lub zamknięty. Ponadto na stronie Alerty będzie zawierać alertu informacyjne, jeśli kontroler został ponownie uruchomiony lub zamknięty. Operacje na kontrolerze zamknięcia i ponownego uruchomienia również są rejestrowane w dziennikach operacji. Aby uzyskać więcej informacji o dziennikach operacji przejdź do [wyświetlenia dzienników operacji](storsimple-service-dashboard.md#view-the-operations-logs).

**PYTANIA.** Czy istnieje wpływu do operacji dotyczących wyniku pracy awaryjnej kontroler?

**ODPOWIEDZI.** Połączeń między Inicjator i active kontrolera zostaną zresetowane wyniku pracy awaryjnej kontrolerze, ale zostanie ponownie nawiązane dopiero po stronie biernej kontroler założono operacji. W trakcie tej operacji może być wstrzymanie tymczasowe (mniej niż 30 sekund) aktywności We/Wy między Inicjator i urządzenia.

**PYTANIA.** Jak zwrócić Moje kontroler do obsługi po go zamknąć i usunięte?

**ODPOWIEDZI.** Aby przywrócić kontrolera usługi, musisz wstawić go do obudowy zgodnie z opisem w polu [Zamień moduł kontrolera na urządzeniu StorSimple](storsimple-controller-replacement.md).

## <a name="next-steps"></a>Następne kroki

- Jeśli napotkasz jakiekolwiek problemy z kontrolerach urządzenia StorSimple, których nie można rozwiązać za pomocą procedury wymienione w tym samouczku, [skontaktuj się z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md).

- Aby dowiedzieć się więcej o korzystaniu z usługi Menedżera StorSimple, przejdź do tematu [Używanie usługę Menedżer StorSimple administrowania urządzenia StorSimple](storsimple-manager-service-administration.md).
