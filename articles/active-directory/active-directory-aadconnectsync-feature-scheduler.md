<properties
   pageTitle="Azure AD Connect synchronizacją: harmonogram | Microsoft Azure"
   description="W tym temacie opisano funkcji wbudowanych harmonogram synchronizacji Azure AD Connect."
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
   ms.date="08/04/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-scheduler"></a>Azure AD Connect synchronizacją: harmonogram
W tym temacie opisano wbudowanych harmonogram synchronizacji Azure AD Connect (alias Aparat synchronizacji).

Ta funkcja została wprowadzona z Konstruuj 1.1.105.0 (wydane lutego 2016).

## <a name="overview"></a>Omówienie
Azure AD Connect synchronizacją zsynchronizuje zmiany dzieje w katalogu lokalnego przy użyciu harmonogramu. Istnieją dwa procesy harmonogram, jedną dla synchronizacji haseł i drugi dla atrybutu obiektu synchronizacji i konserwacyjnych. W tym temacie obejmuje te ostatnie.

W starszych wersjach harmonogram obiekty i atrybuty było zewnętrznych do aparatu synchronizacji i harmonogramu zadań systemu Windows lub osobna usługa Windows został użyty do synchronizacji. Harmonogram jest wbudowane wersji 1.1 do synchronizacji aparat i zezwolić na niektóre dostosowania. Nowe domyślna częstotliwość synchronizacji to 30 minut.

Harmonogram jest odpowiedzialny za dwa zadania:

- **Przechodzenie do synchronizacji**. Proces importowania, synchronizowanie i eksportowania zmiany.
- **Zadania konserwacyjne**. Odnowić klucze i certyfikaty służące do resetowania hasła i urządzeń usługi rejestracji (DRS). Czyszczenie starych wpisów w dzienniku operacji.

Harmonogram, się zawsze jest uruchomiony, ale można skonfigurować tylko uruchamianie jedną lub nie zawierające żadnych z tych zadań. Na przykład jeśli musisz mieć własny proces cyklu synchronizacji, można wyłączyć to zadanie w harmonogramie, ale nadal uruchamiać zadań związanych z obsługą.

## <a name="scheduler-configuration"></a>Konfiguracja harmonogramu
Aby wyświetlić bieżące ustawienia konfiguracji, przejdź do programu PowerShell i uruchom `Get-ADSyncScheduler`. Zostanie wyświetlona podobną do następującej:

![GetSyncScheduler](./media/active-directory-aadconnectsync-feature-scheduler/getsynccyclesettings.png)

Jeśli widzisz **polecenia Synchronizuj lub polecenia cmdlet jest niedostępne** po uruchomieniu tego polecenia cmdlet, modułu programu PowerShell nie jest ładowana. Może się to zdarzyć, jeśli uruchomione narzędzie Azure AD Connect na kontrolerze domeny lub na serwerze przy użyciu programu PowerShell poziomu ograniczeń niż domyślne ustawienia. Jeśli zostanie wyświetlony ten błąd, uruchom `Import-Module ADSync` udostępnienie polecenia cmdlet.

- **AllowedSyncCycleInterval**. Najczęściej Azure AD, aby umożliwić synchronizacji ma być wykonywana. Nie można synchronizować częściej niż to i nadal jest obsługiwana.
- **CurrentlyEffectiveSyncCycleInterval**. Harmonogram aktualnie obowiązujących. Będą mieć tę samą wartość co CustomizedSyncInterval (jeśli Ustawianie) Jeśli nie jest częściej niż AllowedSyncInterval. Jeśli zmienisz CustomizedSyncCycleInterval, to zostanie zastosowane po następnym cyklem synchronizacji.
- **CustomizedSyncCycleInterval**. Jeśli chcesz, aby uruchomić częstotliwością wszelkich innych niż domyślny 30 minut harmonogram, skonfiguruj to ustawienie. Aby uruchomić co godzinę zamiast tego został ustawiony na obrazie powyżej harmonogram. Jeśli ustawisz to wartość niższej niż AllowedSyncInterval, te ostatnie będą używane.
- **NextSyncCyclePolicyType**. Zmiana lub wstępne. Określa, czy następnym uruchomieniu należy tylko proces różnicy zmiany, lub jeśli następnym uruchomieniu, należy wykonać pełny importowanie i synchronizowanie, w której będzie również ponownie przetworzyć reguł nowych lub zmienionych.
- **NextSyncCycleStartTimeInUTC**. Następnym razem harmonogram zostanie uruchomiony następnym cyklem synchronizacji.
- **PurgeRunHistoryInterval**. Czas dzienniki operacji powinny być zachowywane. Można to sprawdzić w Menedżerze usługi synchronizacji. Wartość domyślna to aby zachować te przez 7 dni.
- **SyncCycleEnabled**. Wskazuje, czy harmonogram jest uruchomiony importu, synchronizowanie i procesy eksportowania w ramach jego operacji.
- **MaintenanceEnabled**. Wskazuje, czy jest włączony procesu konserwacji. Jego aktualizowane certyfikaty i klucze i czyścić dziennik działań.
- **IsStagingModeEnabled**. Pokazuje, czy jest włączony [Tryb tymczasowej](active-directory-aadconnectsync-operations.md#staging-mode) .

Można zmienić niektóre z tych ustawień z `Set-ADSyncScheduler`. Można modyfikować następujące parametry:

- CustomizedSyncCycleInterval
- NextSyncCyclePolicyType
- PurgeRunHistoryInterval
- SyncCycleEnabled
- MaintenanceEnabled

Konfiguracja harmonogramu są przechowywane w Azure AD. Jeśli masz serwerze przemieszczenia zmiany na serwerze podstawowym również wpływa na serwerze (z wyjątkiem IsStagingModeEnabled).

### <a name="customizedsynccycleinterval"></a>CustomizedSyncCycleInterval
Składnia:`Set-ADSyncScheduler -CustomizedSyncCycleInterval d.HH:mm:ss`  
d - dni, HH - godziny, mm - minuty, ss - sekundy

Przykład:`Set-ADSyncScheduler -CustomizedSyncCycleInterval 03:00:00`  
Zmieni harmonogram co trzy godziny.

Przykład:`Set-ADSyncScheduler -CustomizedSyncCycleInterval 1.0:0:0`  
Zmieni harmonogram są uruchamiane raz dziennie.

## <a name="start-the-scheduler"></a>Uruchomić harmonogram
Domyślnie harmonogram wykona co 30 minut. W niektórych przypadkach może być uruchamianie cyklu synchronizacji między cykli według harmonogramu lub jest potrzebne do uruchamiania innego typu.

**Cykl synchronizacji delta**  
Cykl synchronizacji różnicy obejmuje następujące kroki:

- Importowanie delta na wszystkich łączników
- Synchronizacja delta na wszystkich łączników
- Eksportowanie na wszystkich łączników

Możliwe, że jest pilna, zmienianie, które musi zostać zsynchronizowany natychmiast dlatego należy ręcznie uruchomić cyklu. Aby ręcznie uruchomić cyklu, następnie z programu PowerShell, uruchom `Start-ADSyncSyncCycle -PolicyType Delta`.

**Cykl pełnej synchronizacji**  
Jeśli wprowadzono jedną z następujących zmian w konfiguracji, należy uruchomić cykl pełnej synchronizacji (alias Początkowy):

- Dodać więcej obiektów lub atrybuty, które mają zostać zaimportowane z katalogu źródłowego
- Wprowadzone zmiany reguły synchronizacji
- Zmiany [filtrowania](active-directory-aadconnectsync-configure-filtering.md) , dlatego powinny podlegać inną liczbę obiektów

Jeśli zostały wprowadzone jedną z tych zmian, następnie należy uruchomić cykl pełnej synchronizacji, aby aparat synchronizacji ma możliwość ponowna spacji łącznika. Cykl pełnej synchronizacji obejmuje następujące kroki:

- Pełne importowanie na wszystkich łączników
- Pełnej synchronizacji na wszystkich łączników
- Eksportowanie na wszystkich łączników

Aby zainicjować cykl pełnej synchronizacji, uruchom `Start-ADSyncSyncCycle -PolicyType Initial` w wierszu polecenia programu PowerShell. Spowoduje to uruchomienie cykl pełnej synchronizacji.

## <a name="stop-the-scheduler"></a>Zatrzymać harmonogram
Jeśli harmonogram jest uruchomiony cyklu synchronizacji może być konieczne, aby je zatrzymać. Na przykład, aby uruchomić Kreatora instalacji i zostanie wyświetlony następujący błąd:

![SyncCycleRunningError](./media/active-directory-aadconnectsync-feature-scheduler/synccyclerunningerror.png)

Po uruchomieniu cykl synchronizacji nie można wprowadzać zmian w konfiguracji. Użytkownik może poczekaj, aż harmonogram został zakończony proces, ale możesz też wyłączyć go, możesz od razu wprowadź zmiany. Zatrzymywanie bieżącym cyklu nie jest szkodliwy i wszelkie zmiany nie przetwarzane nadal będą przetwarzane przy następnym uruchomieniu.

1. Zacznij od informacją harmonogram, aby zatrzymać bieżącą cyklu przy użyciu polecenia cmdlet programu PowerShell `Stop-ADSyncSyncCycle`.
2. Zatrzymywanie harmonogramu nie zatrzyma bieżącego łącznik z jego bieżącego zadania. Aby wymusić łącznika w celu zatrzymania, należy wykonać następujące czynności: ![StopAConnector](./media/active-directory-aadconnectsync-feature-scheduler/stopaconnector.png)
    - Uruchom **Usługę Sychronization** z start menu. Przejdź do **łączników**, zaznacz łącznik ze stanem **uruchomiony** i wybierz pozycję **Zatrzymaj** z akcji.

Harmonogram jest nadal aktywna i rozpocznie się ponownie na następny szansy sprzedaży.

## <a name="custom-scheduler"></a>Niestandardowy harmonogram
Polecenia cmdlet, opisane w tej sekcji są tylko dostępne w kompilacji [1.1.130.0](active-directory-aadconnect-version-history.md#111300) lub nowszy.

Wbudowane narzędzie scheduler nie spełnia wymagań, można zaplanować łączników przy użyciu programu PowerShell.

### <a name="invoke-adsyncrunprofile"></a>Wywołania ADSyncRunProfile
Możesz rozpocząć profilu dla łącznika w ten sposób:

```
Invoke-ADSyncRunProfile -ConnectorName "name of connector" -RunProfileName "name of profile"
```

Nazwy, które mają być używane dla [łącznika nazw](active-directory-aadconnectsync-service-manager-ui-connectors.md) i [uruchomić profilu](active-directory-aadconnectsync-service-manager-ui-connectors.md#configure-run-profiles) można znaleźć w [Menedżerze usługi synchronizacji interfejsu użytkownika](active-directory-aadconnectsync-service-manager-ui.md).

![Wywołaj uruchamianie profilu](./media/active-directory-aadconnectsync-feature-scheduler/invokerunprofile.png)  

`Invoke-ADSyncRunProfile` Polecenia cmdlet jest synchroniczne, to znaczy nie zwraca sterowania do momentu łącznik zakończeniu operacji, pomyślnie lub z powodu błędu.

Podczas planowania łączniki zaleca się planowanie je w następującej kolejności:

1. (Pełne/Delta) Importowanie z katalogów lokalnych, takich jak usługi Active Directory
2. (Pełne/Delta) Importowanie z usługi Azure Active Directory
3. (Pełne/Delta) Synchronizacja z katalogów lokalnych, takich jak usługi Active Directory
4. (Pełne/Delta) Synchronizacja z usługi Azure Active Directory
5. Eksportowanie do Azure AD
6. Eksportowanie do katalogów lokalnych, takich jak usługi Active Directory

Jeśli możesz przeglądać harmonogram wbudowanych jest kolejność uruchomienia łączników.

### <a name="get-adsyncconnectorrunstatus"></a>Get-ADSyncConnectorRunStatus
Można również monitorować aparatu synchronizacji, aby sprawdzić, czy jest zajęty lub bezczynne. To polecenie cmdlet zwróci wyniku, jeśli aparat synchronizacji jest bezczynny i nie działa łącznik. Jeśli łącznik jest uruchomiony, zwróci nazwę łącznika.

```
Get-ADSyncConnectorRunStatus
```

![Stan uruchomienia łącznika](./media/active-directory-aadconnectsync-feature-scheduler/getconnectorrunstatus.png)  
Na powyższym obrazie pierwszy wiersz jest w Państwie miejsce, w którym jest bezczynny aparat synchronizacji. Drugi wiersz podczas uruchamiania Azure AD łącznik.

## <a name="scheduler-and-installation-wizard"></a>Kreator harmonogram i instalacji
Po uruchomieniu Kreatora instalacji następnie harmonogram zostaną tymczasowo zawieszone. Jest to spowodowane przyjmuje wprowadzisz zmiany w konfiguracji i tych nie można stosować, jeśli aktywnie działa aparat synchronizacji. Z tego powodu nie opuścić kreatora instalacji Otwórz ponieważ zatrzyma aparat synchronizacji wykonywać żadnych akcji synchronizacji.

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej na temat konfiguracji [synchronizacji Azure AD Connect](active-directory-aadconnectsync-whatis.md) .

Dowiedz się więcej o [integrowaniu lokalnego tożsamości z usługą Azure Active Directory](active-directory-aadconnect.md).
