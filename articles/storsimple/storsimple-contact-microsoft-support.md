<properties 
   pageTitle="Skontaktuj się z pomocą techniczną firmy Microsoft | Microsoft Azure"
   description="Dowiedz się, jak utworzyć żądanie pomocy technicznej i rozpocząć sesję pomocy technicznej na urządzeniu StorSimple."
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
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="contact-microsoft-support"></a>Kontakt z pomocą techniczną firmy Microsoft

Jeśli napotkasz jakiekolwiek problemy z rozwiązania Microsoft Azure StorSimple, możesz utworzyć żądanie obsługi w celu uzyskania pomocy technicznej. W ramach sesji online z pracownikiem usługi pomocy technicznej również konieczne może być rozpocząć sesję pomocy technicznej na urządzeniu StorSimple. W tym artykule przeprowadzi Cię przez:

- Jak utworzyć żądanie pomocy technicznej.
- Jak rozpocząć sesję pomocy technicznej w interfejsie programu Windows PowerShell urządzenia StorSimple.

Przejrzyj [systemy obsługi serii 8000 StorSimple i informacje](https://msdn.microsoft.com/library/mt433077.aspx) przed utworzeniem prośbę o pomoc techniczną.

## <a name="create-a-support-request"></a>Utwórz żądanie obsługi

Wykonaj poniższe czynności, aby utworzyć żądanie pomocy technicznej:

#### <a name="to-create-a-support-request"></a>Aby utworzyć żądanie pomocy technicznej

1. W [portalu klasyczny Azure](https://manage.windowsazure.com/), w prawym górnym rogu kliknij nazwę swojego konta, a następnie kliknij **Kontakt z pomocą techniczną firmy Microsoft**.

    ![Obsługa MS kontakt za pośrednictwem ManagementPortal](./media/storsimple-contact-microsoft-support/Ibiza1.png)

2. Nastąpi przekierowanie do nowego portal Azure (portal.azure.com). Kliknij **Nowy obsługuje żądania** .

    ![Obsługa MS kontaktu przy użyciu nowego portalu](./media/storsimple-contact-microsoft-support/Ibiza2.png)

    Po prawej stronie ekranu zostanie wyświetlone okienko **Nowy obsługuje żądania** . 

    ![Nowe okienko żądanie pomocy technicznej](./media/storsimple-contact-microsoft-support/Ibiza3a.png)

3. W oknie dialogowym **— informacje podstawowe** wykonaj następujące czynności:                                
    1. Z listy rozwijanej **Typ problemu** wybierz **technicznych**.
    2. Z listy rozwijanej wybierz **subskrypcję** .
    3. Z listy rozwijanej **usługi** wybierz **StorSimple**. 
    4. Z listy rozwijanej wybierz **plan pomocy technicznej** . Potrzebujesz planu płatnej pomocy technicznej umożliwiające pomocy technicznej.

4. Kliknij przycisk **Dalej**. Zostanie wyświetlone okno dialogowe **Problem** .

    ![Nowe okienko żądanie pomocy technicznej](./media/storsimple-contact-microsoft-support/Ibiza5a.png) 

5. W oknie dialogowym **Problem** wykonaj następujące czynności:

    1.  Wybierz poziom **ważności** z listy rozwijanej.
    2.  Wybierz **Typ problemu** z listy rozwijanej.
    3.  Wybierz **kategorię** z listy rozwijanej. 
    4.  W oknie dialogowym **Szczegóły** krótko opisz problem.
    5.  W oknie dialogowym **przedziału czasu** wskazują, daty i godziny oraz strefę czasową odpowiada ostatnio wystąpienie problemu.
    6.  W obszarze **przekazywanie plików**kliknij ikonę folderu, aby przejść do pakietu pomocy technicznej.
    7.  Zaznacz pole wyboru **Udostępnij informacje diagnostyczne** .

6. Kliknij przycisk **Dalej**. Zostanie wyświetlone okno dialogowe **informacje kontaktowe** .

    ![Nowe okienko żądanie pomocy technicznej](./media/storsimple-contact-microsoft-support/Ibiza6a.png) 

7. Podaj swoje informacje kontaktowe i wybierz metodę kontaktu (telefonu lub poczty e-mail). 

8. Zaznacz pole wyboru **Zapisz zmiany w kontakcie wprowadzone do obsługi przyszłych żądań** .

9. Kliknij przycisk **Utwórz**.

Po żądanie zostało przesłane, z pracownikiem pomocy technicznej skontaktują się z Tobą tak szybko, jak to możliwe, aby kontynuować żądania.

## <a name="start-a-support-session-in-windows-powershell-for-storsimple"></a>Rozpoczynanie sesji pomocy technicznej w programie Windows PowerShell dla StorSimple

Aby rozwiązać wszelkie problemy, które mogą wystąpić z urządzeniem StorSimple, będzie konieczne współpracować z zespołu Support firmy Microsoft. Support firmy Microsoft, może być konieczne skorzystanie z sesji pomocy technicznej do logowania się do urządzenia. 

Wykonaj poniższe czynności, aby rozpocząć sesję pomocy technicznej:

#### <a name="to-start-a-support-session"></a>Aby rozpocząć sesję pomocy technicznej

1. Dostęp do urządzenia, bezpośrednio za pomocą konsoli szeregowego lub sesji telnet na komputerze zdalnym. Aby to zrobić, wykonaj kroki w [Kit Użyj nawiązywania połączenia z konsoli szeregowego urządzenia](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).

2. W danej sesji, który zostanie otwarty naciśnij klawisz **Enter** , aby uzyskać wiersz polecenia.

3. W menu Konsola szeregowa wybierz opcję 1, **Zaloguj się przy użyciu pełnego dostępu**.

4. W wierszu polecenia wpisz następujące hasło: 

    `Password1`

5. W wierszu polecenia wpisz następujące polecenie:

    `Enable-HcsSupportAccess`

6. Zaszyfrowany ciąg zostanie wyświetlona dla Ciebie. Skopiuj ten ciąg w edytorze tekstów, takim jak Notatnik.

7. Zapisz ten ciąg i wyślij go w wiadomości e-mail do Support firmy Microsoft. 

> [AZURE.IMPORTANT] Możesz wyłączyć dostęp pomocy technicznej, uruchamiając `Disable-HcsSupportAccess`. Urządzenie StorSimple spróbuje też wyłączyć dostęp pomocy technicznej 8 godzin, gdy sesja została zainicjowana. Jest najlepszym rozwiązaniem zmienianie poświadczeń urządzenia StorSimple po zainicjowaniu sesji pomocy technicznej.
