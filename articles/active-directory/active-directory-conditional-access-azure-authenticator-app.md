
<properties
    pageTitle="Azure uwierzytelnienia dla systemu Android | Microsoft Azure"
    description="Microsoft Azure uwierzytelnienia aplikacji można używać do logowania na dostęp do zasobów pracy. Aplikacja uwierzytelnienia Azure powiadomienia o wniosku o weryfikację oczekiwanie na dwa czynniki, wyświetlając alert na urządzeniu przenośnym."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="azure-authenticator-for-android"></a>Azure uwierzytelnienia dla systemu Android

IT administrator może mieć zalecana użycie uwierzytelnienia Microsoft Azure do logowania się do uzyskać dostęp do zasobów pracy. Ta aplikacja zapewnia te dwie opcje logowania:

* Uwierzytelnianie wieloskładnikowe umożliwia zabezpieczenie konta służbowego z Weryfikacja dwuetapowa. Możesz logowania przy użyciu ustalić (na przykład hasła) i chronić konto jeszcze bardziej z innej masz (klucz zabezpieczeń z tej aplikacji). Aplikacja uwierzytelnienia Azure powiadomienia o żądania oczekujące weryfikacji dwa czynniki, wyświetlając alert na urządzeniu przenośnym. Należy po prostu widoku żądania w aplikacji i wybierz polecenie Sprawdź lub Anuluj. Można też o wprowadź kod dostępu wyświetlany w aplikacji.

* Konta służbowego umożliwia przekształcić urządzenie zaufane Android telefonu lub tabletu i podaj pojedynczej logowania jednokrotnego (SSO) do aplikacji firmy. IT administrator może wymagać można dodać konta służbowego, aby można było uzyskać dostęp do zasobów firmy. Logowania jednokrotnego pozwala zalogować się raz i automatycznie skorzystać z logowaniem we wszystkich aplikacjach, które firmy udostępniła dla Ciebie.

## <a name="installing-the-azure-authenticator-app"></a>Instalowanie aplikacji uwierzytelnienia Azure

Aplikacja uwierzytelnienia Azure można zainstalować ze sklepu Google Play.
Z instrukcjami, aby dodać konta służbowego z niż Samsung Android w porównaniu z urządzeniu z systemem Samsung Android urządzenia są nieco inne. Poniżej przedstawiono instrukcje dotyczące obu.

<a name="adding-the-work-account-from-samsung-android-device"></a>Dodawanie konta służbowego na urządzeniu z systemem Samsung Android
----------------------------------------------------------------------------------------------------------------
###<a name="adding-the-work-account-through-the-app-home-screen"></a>Dodawanie konta służbowego, za pośrednictwem ekran główny aplikacji

Poniższe instrukcje dotyczą Samsung GS3 i powyżej telefonów lub Uwaga2 oraz powyżej tabletów.

1. Na ekranie głównym aplikacji Zaakceptuj Umowę licencyjną użytkownika (oprogramowania EULA).
2. Na ekranie aktywowanie konta kliknij menu kontekstowego po prawej stronie i wybierz **konta służbowego**.
3. Na ekranie Dodaj konto wybierz** Konto pracy**.
4. Na ekranie administratora urządzenia Aktywuj kliknij pozycję **Aktywuj**.
5. Na ekranie zasady ochrony prywatności zaznacz pole wyboru, a następnie kliknij przycisk **Potwierdź**.
6. Na ekranie dołączanie do obszaru roboczego wprowadź nazwę użytkownika, udostępnionej przez organizację i kliknij przycisk **Dołącz**.
7. Aby zalogować się do aplikacji uwierzytelnienia Azure, wprowadź organizacji *** klienta i hasło i kliknij przycisk **Zaloguj**.
8. Na kolejnym ekranie są wyświetlane informacje o uwierzytelnianie wieloskładnikowe (MFA) dla zostanie dodany zabezpieczeń i jest opcjonalna. Zostanie wyświetlony ten ekran, jeśli swojej pracy lub szkole wymaga uwierzytelniania drugiego czynniki dotyczące tworzenia konta służbowego. Instrukcje celu dalszego zbadania konta.
9. Ekran dołączania pracy wyświetli komunikat "**dołączania do miejsca pracy**". Aplikacja Azure uwierzytelnienia dołącza do urządzenia do miejsca pracy.
10. Powinien zostać wyświetlony komunikat sprzężone w miejscu pracy na następnym ekranie.

>[AZURE.NOTE]
> Konto jednego pracy są dozwolone na urządzeniu.

### <a name="adding-the-work-account-from-the-settings-menu"></a>Dodawanie konta służbowego z menu Ustawienia
Po zainstalowaniu aplikacji uwierzytelnienia Azure, można również utworzyć konta służbowego za pomocą Menedżera konta systemu Android.

1. Z menu Ustawienia przejdź do **konta** , a następnie kliknij pozycję **Dodaj konto**.
2. Wykonaj kroki 3-10 procedura dodawania konta służbowego, za pośrednictwem aplikacji ekranu głównego, aby dodać konta służbowego.

<a name="adding-the-work-account-from-a-non-samsung-android-device"></a>Dodawanie konta służbowego z urządzenia Samsung Android
------------------------------------------------------------------------------------------------------------------
### <a name="adding-the-work-account-through-the-app-home-screen"></a>Dodawanie konta służbowego, za pośrednictwem ekran główny aplikacji

1. Na ekranie głównym aplikacji Zaakceptuj Umowę licencyjną użytkownika (oprogramowania EULA).
2. Na ekranie aktywowanie konta kliknij menu kontekstowego po prawej stronie i wybierz **konta służbowego**.
3. Na ekranie konta kliknij pozycję **Dodaj konto**.
4. Jeśli pojawi się ekran konta, kliknij pozycję **Dodaj konto**. Jeśli konto pracy jest już utworzony wcześniej, zostanie wyświetlony synchronizacji ekran przedstawiający istniejącą służbowym kontem. Można zachować konta służbowego, po prostu naciskając strzałkę wstecz do ekranu głównego. Alternatywnie możesz wybrać konto, aby usunąć i ponownie utworzyć nowy pracy konta w miejscu pracy: ekran dołączania, wprowadź nazwę użytkownika, udostępnionej przez organizację i kliknij pozycję Dołącz.
5. Aby zalogować się do aplikacji uwierzytelnienia Azure, wprowadź konto organizacji i hasło, a następnie kliknij przycisk **Zaloguj**.
7. Na kolejnym ekranie są wyświetlane informacje o uwierzytelnianie wieloskładnikowe (MFA) jest dla dodatkowe zabezpieczenia i jest opcjonalna. Zostanie wyświetlony ten ekran, jeśli Twojej pracy lub szkole wymaga uwierzytelniania drugiego czynniki dotyczące tworzenia konta służbowego. Instrukcje celu dalszego zbadania Twojego konta.
8. Na następnym ekranie kliknij przycisk **OK** . Nie należy zmieniać nazwę certyfikatu.
komunikat "Dołączanie miejsca pracy". Aplikacja Azure uwierzytelnienia dołącza do urządzenia do miejsca pracy.
Powinien zostać wyświetlony komunikat sprzężone w miejscu pracy na następnym ekranie.

>[AZURE.NOTE]
> Konto jednego pracy są dozwolone na urządzeniu.

Po zainstalowaniu aplikacji uwierzytelnienia Azure, można również utworzyć konta służbowego za pomocą Menedżera konta systemu Android.

1. Z menu **Ustawienia** przejdź do konta, a następnie kliknij pozycję **Dodaj konto**.
2. Wykonaj kroki od 2 do 7 w procedurze, Dodawanie konta służbowego, za pośrednictwem aplikacji głównym ekranu **, aby dodać konta służbowego.

### <a name="how-to-find-out-which-version-is-installed"></a>Jak sprawdzić, która wersja jest zainstalowana

1. Można sprawdzić, którą wersję aplikacji uwierzytelnienia Azure i wersje skojarzonej usługi są zainstalowane na urządzeniu.
2. W menu podręcznym kliknij polecenie **informacje**.
3. Na ekranie informacje są wyświetlane z usług aplikacji i wersje zainstalowanego na twoim urządzeniu.
 
### <a name="sending-log-files-to-report-issues"></a>Wysyłanie plików dziennika do problemy związane z raportem

1. Postępuj zgodnie z zawartymi w Microsoft Support raport zdarzeniem związanym z aplikacją uwierzytelnienia Azure, Uzyskaj numer zdarzenia i wysłać pliki dziennika przed przypisany numer zdarzenia w następujący sposób:
2. W menu podręcznym kliknij polecenie **rejestrowania**.
3. Jeśli masz otwarty zdarzenia z Support firmy Microsoft, zanotuj numer zdarzenia (musisz go później kroku). Jeśli nie została jeszcze utworzona pomocy technicznej zdarzenia, a potem pomocy, postępuj zgodnie z zawartymi w [Pomocy technicznej firmy Microsoft](https://support.microsoft.com/en-us/contactus) , aby otworzyć nowe zdarzenie.
4. Na ekranie logowania kliknij pozycję **Wyślij teraz**.
5. Wybierz dostawcę poczty e-mail, którego chcesz użyć.
7. Jeśli masz już otwarte zdarzenia Support firmy Microsoft, skontaktuj się z pracownikiem pomocy technicznej, aby dowiedzieć się, jak wysyłać dane dziennika i skojarzone z zdarzenie przypisany do problemu. Z pracownikiem pomocy technicznej umożliwi informacji dla wiersza adres i temat wiadomości e-mail. Jeśli nie masz już pomocy technicznej zdarzenia, postępuj zgodnie z zawartymi w Support firmy Microsoft, aby otworzyć nowe zdarzenie.
9. Edytuj **Wiersz** i **temat informacje otrzymane od firmy Microsoft Support** .
10. Aplikacja uwierzytelnienia Azure dołącza plik dziennika do wiadomości e-mail, które wysyłasz. Opisz problem, które występują, zaktualizuj listy adresatów (opcjonalnie), a następnie wyślij wiadomość e-mail.

### <a name="deleting-the-work-account-and-leaving-your-workplace"></a>Usuwanie konta służbowego i pozostawianie miejsca pracy

Możesz usunąć konta służbowego, utworzonego w dowolnym momencie:

**Aby usunąć konto pracy w menu Ustawienia**

1. Menedżer kont zaznacz **konta służbowego**.
2. Na ekranie konta służbowego w obszarze **Ustawienia ogólne**wybierz pozycję **Ustawienia kont — pozostaw sieci w miejscu pracy**.
3. Wybierz pozycję **pozostawić** na ekranie **Dołączanie do obszaru roboczego** .
4. Kliknij **przycisk OK** , gdy jest wyświetlany komunikat "Czy chcesz opuścić pracy są".
5. Dzięki temu, że usunięto konto służbowe od miejsca pracy.

>[AZURE.NOTE]
>Zaleca się, że nie używać opcji Usuń konto, aby usunąć konto pracy, jak ta opcja może nie działać w niektórych starszych wersjach systemu Android.

##<a name="uninstalling-the-app"></a>Odinstalowanie aplikacji

Na urządzeniu z systemem Samsung Android uprawnienia administratora urządzenia należy usunąć w następujący sposób przed odinstalowaniem 
1. W obszarze **Ustawienia**w obszarze **System**wybierz pozycję **Zabezpieczenia**.
2. W D**evice administracji**kliknij pozycję **Administratorzy urządzenia**. Upewnij się, że jest zaznaczone pole wyboru obok **Uwierzytelnienia Azure** .

##<a name="troubleshooting"></a>Rozwiązywanie problemów

Jeśli zostanie wyświetlony **Błąd elementu Keystore**, może to być spowodowane brakiem blokady ekranu zestawu się za pomocą numeru PIN. Aby obejść ten problem, odinstalować aplikację uwierzytelnienia Azure, konfigurowanie numeru PIN, na ekranie blokady i ponownie zainstaluj aplikację.
