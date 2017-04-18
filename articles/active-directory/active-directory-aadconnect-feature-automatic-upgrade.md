<properties
   pageTitle="Narzędzie Azure AD Connect: Uaktualnianie | Microsoft Azure"
   description="W tym temacie opisano wbudowanych funkcji automatycznego uaktualnienia narzędzie Azure AD Connect synchronizacji."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/24/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-automatic-upgrade"></a>Narzędzie Azure AD Connect: Uaktualnianie
Ta funkcja została wprowadzona z kompilacji 1.1.105.0 (wydane 2016 luty).

## <a name="overview"></a>Omówienie
Zagwarantować, że instalacji usługi Azure AD Connect są zawsze aktualne nigdy nie było łatwiejsze dzięki funkcji **automatycznego uaktualnienia** . Ta funkcja jest domyślnie instalacjach express oraz DirSync uaktualnień. Po wydaniu nowej wersji instalacji jest automatycznie aktualizowane.

Uaktualnianie jest domyślnie włączone dla następujących czynności:

- Można wyrazić ustawienia instalacji i uaktualnień DirSync.
- Przy użyciu SQL Express LocalDB, która jest co ustawienia Express zawsze używaj. DirSync z programu SQL Express również użyć LocalDB.
- Konto AD jest domyślne konto MSOL_ utworzone przez ustawienia Express i DirSync.
- Masz mniej niż 100 000 obiektów w metaverse.

Bieżący stan automatycznym uaktualnieniu mogą być wyświetlane przy użyciu polecenia cmdlet programu PowerShell `Get-ADSyncAutoUpgrade`. Ma następujące stany:

Województwo | Komentarz
---- | ----
Włączone | Uaktualnianie wersji jest włączona.
Zawieszone | Ustawianie przez system tylko. System nie jest już jest uprawniony do otrzymywania automatycznych uaktualnień.
Wyłączone | Uaktualnianie jest wyłączona.

Można przełączać między **włączone** i **wyłączone** z `Set-ADSyncAutoUpgrade`. Tylko system należy ustawić stan **zawieszone**.

Uaktualnianie używa Azure AD łączenie kondycji uaktualnienia infrastruktury. Automatyczne uaktualnienia do pracy upewnij się, że zostały otwarte adresy URL w serwera proxy **Azure AD łączenie** kondycji opisane w [adresy URL usługi Office 365 i zakresy adresów IP](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

Jeśli **Menedżer usługi synchronizacji** interfejsu użytkownika jest uruchomiony na serwerze, uaktualnienie zostało zawieszone, dopóki nie zostanie zamknięty interfejsu użytkownika.

## <a name="troubleshooting"></a>Rozwiązywanie problemów
Łączenie instalacji uaktualnienia się się zgodnie z oczekiwaniami, następnie wykonaj poniższe czynności, aby dowiedzieć się, jakie mogą być nieprawidłowe.

Najpierw należy nie oczekujesz automatyczne uaktualnienie do podjąć próbę pierwszego dnia wydania nowej wersji. Przed uaktualnieniem jest tak nie alarmed instalacji nie jest od razu uaktualnienia jest zamierzone losowości.

Jeśli nie odpowiada coś, następnie pierwszego uruchomienia `Get-ADSyncAutoUpgrade` zapewnienie uaktualnianie wersji jest włączona.

Następnie upewnij się, że został otwarty odpowiednie adresy URL serwera proxy lub zapory. Automatyczna aktualizacja używa Azure AD łączenie kondycji zgodnie z opisem w [Przegląd](#overview). Jeśli korzystasz z serwera proxy, upewnij się, że kondycja została skonfigurowana do korzystania z [serwera proxy](active-directory-aadconnect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy). Również testowanie [łączności kondycji](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) do Azure AD.

Z łącznością z Azure AD zweryfikowane nadszedł czas na kwestii elementami EventLog. Uruchom Podgląd zdarzeń, a w dzienniku zdarzeń **aplikacji** . Dodać filtr zdarzeń dla źródła **Azure AD łączenie uaktualnienie** i zakresu identyfikator zdarzenia **300 399**.  
![Filtr zdarzeń dla automatycznym uaktualnieniu](./media/active-directory-aadconnect-feature-automatic-upgrade/eventlogfilter.png)  

Można teraz zobaczyć elementami Eventlog, skojarzone z stan automatycznym uaktualnieniu.  
![Filtr zdarzeń dla automatycznym uaktualnieniu](./media/active-directory-aadconnect-feature-automatic-upgrade/eventlogresult.png)  

Kod wyniku ma prefiks omówienie stanu.

Prefiks kodu wynik | Opis
--- | ---
Sukcesu | Instalacja została pomyślnie uaktualniony.
UpgradeAborted | Stan tymczasowy zatrzymane uaktualnienia. Jego zostanie wykonana ponownie ponownie i oczekuje się później powodzeniem.
UpgradeNotSupported | System ma konfiguracji, która jest spowodowana przez system z aktualizowany automatycznie. Jego zostanie wykonana ponownie wyświetlić stan się zmienia, ale oczekuje się, że system muszą zostać uaktualnione ręcznie.

Oto lista najbardziej typowych wiadomości, które znaleźć. Nie listę wszystkich, ale wiadomości wynik należy, wyczyść pole wyboru z jaki problem jest.

Wynik wiadomości | Opis
--- | ---
**UpgradeAborted** |
UpgradeAbortedCouldNotSetUpgradeMarker | Nie można zapisać w rejestrze.
UpgradeAbortedInsufficientDatabasePermissions | Wbudowanej grupy Administratorzy nie ma uprawnień do bazy danych. Ręczne uaktualnienie do najnowszej wersji Azure AD Connect, aby rozwiązać ten problem.
UpgradeAbortedInsufficientDiskSpace | Jest za mało miejsca na dysku, Obsługa uaktualniania.
UpgradeAbortedSecurityGroupsNotPresent | Nie można znaleźć i rozwiązać wszystkich grup zabezpieczeń używane przez aparat synchronizacji.
UpgradeAbortedServiceCanNotBeStarted | Nie można uruchomić usługi NT **Microsoft Azure AD Sync** .
UpgradeAbortedServiceCanNotBeStopped | Nie można zatrzymać NT usługi **Microsoft Azure AD Sync** .
UpgradeAbortedServiceIsNotRunning | NT usługi **Microsoft Azure AD Sync** nie jest uruchomiony.
UpgradeAbortedSyncCycleDisabled | Opcja SyncCycle w [Harmonogram](active-directory-aadconnectsync-feature-scheduler.md) został wyłączony.
UpgradeAbortedSyncExeInUse | [Menedżer usługi synchronizacji interfejsu użytkownika](active-directory-aadconnectsync-service-manager-ui.md) jest otwarty na serwerze.
UpgradeAbortedSyncOrConfigurationInProgress | Kreator instalacji jest uruchomiony lub synchronizacji zostało zaplanowane poza harmonogram.
**UpgradeNotSupported** |
UpgradeNotSupportedCustomizedSyncRules | Niestandardowe reguł dodano do konfiguracji.
UpgradeNotSupportedDeviceWritebackEnabled | Włączono funkcję [zapisu urządzenia](active-directory-aadconnect-feature-device-writeback.md) .
UpgradeNotSupportedGroupWritebackEnabled | Włączono funkcję [zapisu grupy](active-directory-aadconnect-feature-preview.md#group-writeback) .
UpgradeNotSupportedInvalidPersistedState | Instalacja nie jest ustawienia Express lub uaktualnianie DirSync.
UpgradeNotSupportedMetaverseSizeExceeeded | Masz więcej niż 100 000 obiektów w metaverse.
UpgradeNotSupportedMultiForestSetup | Łączysz się więcej niż jeden las. Instalacja ekspresowa łączy tylko jeden las.
UpgradeNotSupportedNonLocalDbInstall | Nie używasz bazy danych programu SQL Server Express LocalDB.
UpgradeNotSupportedNonMsolAccount | [Łącznik AD konto](active-directory-aadconnect-accounts-permissions.md#active-directory-account) nie jest już domyślne konto MSOL_.
UpgradeNotSupportedStagingModeEnabled | Serwer jest równa się w [trybie](active-directory-aadconnectsync-operations.md#staging-mode).
UpgradeNotSupportedUserWritebackEnabled | Włączono funkcję [zapisu użytkownika](active-directory-aadconnect-feature-preview.md#user-writeback) .

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej o [integrowaniu lokalnego tożsamości z usługą Azure Active Directory](active-directory-aadconnect.md).
