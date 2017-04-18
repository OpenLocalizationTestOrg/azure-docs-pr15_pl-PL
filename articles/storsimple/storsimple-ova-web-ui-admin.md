<properties 
   pageTitle="Tablica Virtual StorSimple sieci web Administracja interfejsu użytkownika | Microsoft Azure"
   description="W tym artykule opisano sposób wykonywania zadań administracyjnych podstawowe urządzenia za pośrednictwem sieci web tablicy Virtual StorSimple interfejsu użytkownika."
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
   ms.workload="TBD"
   ms.date="04/07/2016"
   ms.author="alkohli" />

# <a name="use-the-web-ui-to-administer-your-storsimple-virtual-array"></a>Administrowanie macierzy Virtual StorSimple za pomocą Interfejsu sieci Web

![przebieg procesu konfiguracji](./media/storsimple-ova-web-ui-admin/manage4.png)

## <a name="overview"></a>Omówienie

Samouczki w tym artykule dotyczą usługi Microsoft Azure StorSimple wirtualnych tablicy (nazywane także StorSimple lokalnej wirtualną urządzenia) z marca 2016 wersji ogólnodostępną (GA). W tym artykule opisano niektóre z złożonych przepływów pracy i zadania związane z zarządzaniem, które mogą być wykonywane na tablicy Virtual StorSimple. Możesz zarządzać tablicy Virtual StorSimple za pomocą Menedżera StorSimple usługi interfejsu użytkownika (nazywane portalu interfejsu użytkownika) i lokalne sieci web interfejsu użytkownika dla danego urządzenia. W tym artykule omówiono zadania, że można wykonywać przy użyciu interfejsu użytkownika w sieci web.

Ten artykuł zawiera następujące samouczki:

- Uzyskiwanie klucza szyfrowania danych usługi
- Rozwiązywanie problemów z błędami konfiguracji interfejsu użytkownika sieci web
- Generowanie pakiet dziennika
- Zamknij i uruchom ponownie urządzenie

## <a name="get-the-service-data-encryption-key"></a>Uzyskiwanie klucza szyfrowania danych usługi

Po zarejestrowaniu urządzenia pierwszego usłudze Menedżer StorSimple, jest generowany klucza szyfrowania danych usługi. Ten klawisz jest wymagane przy użyciu klucza rejestru usługi do zarejestrowania dodatkowe urządzenia z usługą Menedżera StorSimple.

Jeśli masz do klucza szyfrowania danych usługi i trzeba go pobrać, wykonaj poniższe czynności w lokalnej sieci web interfejs urządzenie zarejestrowane z usługą.

#### <a name="to-get-the-service-data-encryption-key"></a>Aby uzyskać klucza szyfrowania danych usługi

1. Nawiązywanie połączenia do lokalnego interfejsu użytkownika w sieci web. Przejdź do pozycji **konfiguracji** > **Ustawienia chmury**.
  

2. U dołu strony kliknij przycisk **Pobierz klucza szyfrowania danych usługi**. Klucz zostanie wyświetlona. Skopiuj i Zapisz ten klucz.
    
    ![Uzyskiwanie klucza szyfrowania danych usługi 1](./media/storsimple-ova-web-ui-admin/image27.png)
   


## <a name="troubleshoot-web-ui-setup-errors"></a>Rozwiązywanie problemów z błędami konfiguracji interfejsu użytkownika sieci web

W niektórych przypadkach po skonfigurowaniu urządzenia za pośrednictwem lokalnych sieci web interfejsu użytkownika, możesz napotkać błędy. Diagnozowanie i rozwiązywanie problemów z błędami takie, można uruchamiać testów diagnostycznych.

#### <a name="to-run-the-diagnostic-tests"></a>Aby uruchomić testy diagnostyczne

1. Lokalne witryny interfejsu użytkownika, przejdź do pozycji **Rozwiązywanie problemów** > **diagnostycznych**.

    ![Uruchamianie diagnostyki 1](./media/storsimple-ova-web-ui-admin/image29.png)

2. U dołu strony kliknij przycisk **Uruchom testy diagnostyczne**. Rozpocznie to sprawdza do diagnozowania wszelkie problemy z Twojej sieci, urządzenia, serwer proxy sieci web, godziny lub ustawienia chmury. Użytkownik będzie powiadamiany, że urządzenie działa testów.

3. Po testów zostały ukończone, będą wyświetlane wyniki. W poniższym przykładzie pokazano wyniki testów diagnostycznych. Należy zauważyć, że ustawienia serwera proxy w sieci web nie zostały skonfigurowane na tym urządzeniu, a więc badanie serwera proxy sieci web nie zostało uruchomione. Wszystkie testy ustawień sieci, serwer DNS i ustawienia czasu zostały pomyślnie.

    ![Uruchamianie diagnostyki 2](./media/storsimple-ova-web-ui-admin/image30.png)

## <a name="generate-a-log-package"></a>Generowanie pakiet dziennika

Pakiet dziennika składa się z wszystkich odpowiednich dzienników, które może pomóc Microsoft Support Rozwiązywanie problemów z dowolnego urządzenia. W tej wersji pakietu dziennika może być generowane za pośrednictwem lokalnych sieci web interfejsu użytkownika.

#### <a name="to-generate-the-log-package"></a>Aby wygenerować pakietu dziennika

1. Lokalne witryny interfejsu użytkownika, przejdź do pozycji **Rozwiązywanie problemów** > **Dzienniki systemu**.

    ![Generowanie pakietu dziennika 1](./media/storsimple-ova-web-ui-admin/image31.png)

2. U dołu strony kliknij pozycję **Utwórz pakiet dziennika**. Opakowanie dzienniki systemu zostanie utworzona. To może potrwać kilka minut.

    ![Generowanie pakietu dziennika 2](./media/storsimple-ova-web-ui-admin/image32.png)

    Po pomyślnym utworzeniu pakietu, a strony zostaną zaktualizowane, aby wskazać, godziny i daty utworzenia pakietu, otrzymasz powiadomienie.

    ![Generowanie dziennika pakiet 3](./media/storsimple-ova-web-ui-admin/image33.png)

3. Kliknij przycisk **Pobierz dziennika**. Pakiet zip będą pobierane na komputerze.

    ![Generowanie dziennika pakiet 4](./media/storsimple-ova-web-ui-admin/image34.png)

4. Możesz Rozpakuj plik dziennika pobrany pakiet i przeglądać pliki dziennika systemu.

## <a name="shut-down-and-restart-your-device"></a>Zamknij i uruchom ponownie urządzenie

Możesz zamknąć lub uruchom ponownie urządzenie wirtualnych korzystanie z lokalnego interfejsu użytkownika w sieci web. Firma Microsoft zalecamy, aby przed ponownym wykonaj wielkości lub akcji w trybie offline na hoście, a następnie urządzenie. Ten sposób można zminimalizować możliwości uszkodzenie danych. 

#### <a name="to-shut-down-your-virtual-device"></a>Aby zamknąć wirtualnego urządzenia

1. Lokalne witryny interfejsu użytkownika, przejdź do **utrzymania** > **Power ustawienia**.

2. U dołu strony kliknij przycisk **Zamknij**.

    ![urządzenie zamknięcia 1](./media/storsimple-ova-web-ui-admin/image36.png)

3. Informacją, że wyłączenie urządzenia zakłóci Jo dowolnego uzyskując przestoje zostały w toku, zostanie wyświetlone ostrzeżenie. Kliknij ikonę wyboru ![Zaznacz ikony](./media/storsimple-ova-web-ui-admin/image3.png).

    ![Ostrzeżenie o zamknięcie urządzenia](./media/storsimple-ova-web-ui-admin/image37.png)

    Otrzymasz powiadomienie zainicjowano zamknięcie.

    ![zamknięcie urządzenia pracę](./media/storsimple-ova-web-ui-admin/image38.png)

    Teraz wyłączy urządzenia. Jeśli chcesz zacząć urządzenia, musisz to zrobić za pomocą Menedżera funkcji Hyper-V.

#### <a name="to-restart-your-virtual-device"></a>Aby ponownie uruchomić wirtualnego urządzenia

1. Lokalne witryny interfejsu użytkownika, przejdź do **utrzymania** > **Power ustawienia**.

2. U dołu strony kliknij przycisk **Uruchom**.

    ![Uruchom ponownie urządzenie](./media/storsimple-ova-web-ui-admin/image36.png)

3. Zostanie wyświetlone ostrzeżenie informacją, że ponownym uruchomieniu urządzenia zakłóci dowolnego IOs będące w toku, uzyskując przestoje. Kliknij ikonę wyboru ![Zaznacz ikony](./media/storsimple-ova-web-ui-admin/image3.png).

    ![Ostrzeżenie o ponownym uruchomieniu](./media/storsimple-ova-web-ui-admin/image37.png)

    Otrzymasz powiadomienie zainicjowano po ponownym uruchomieniu komputera.

    ![Uruchom ponownie zainicjowana](./media/storsimple-ova-web-ui-admin/image39.png)

    Po ponownym uruchomieniu komputera w trakcie, aby interfejsu użytkownika zostaną utracone połączenie. Po ponownym uruchomieniu komputera można monitorować okresowo odświeżając interfejsu użytkownika. Ponadto można monitorować stan ponownym uruchomieniu urządzenia za pomocą Menedżera funkcji Hyper-V.

## <a name="next-steps"></a>Następne kroki

Dowiedz się, jak [za pomocą Menedżera StorSimple usługi Zarządzanie Twoim urządzeniem](storsimple-manager-service-administration.md).
