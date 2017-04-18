<properties
   pageTitle="Pracownik działań aranżacji hybrydowych: Zadanie działań aranżacji kończy się ze stanem zawieszone | Microsoft Azure"
   description="Symptomy przyczyny i rozwiązania dla pracownika działań aranżacji hybrydowych błędu zakończenia zadania."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/17/2016"
   ms.author="magoedte" />

# <a name="hybrid-runbook-worker-a-runbook-job-terminates-with-a-status-of-suspended"></a>Pracownik działań aranżacji hybrydowych: Zadanie działań aranżacji kończy się ze stanem zawieszone

## <a name="summary"></a>Podsumowanie

Do działań aranżacji zostało zawieszone wkrótce po próby wykonania go trzy razy. Istnieją warunki, które mogą przerwać działań aranżacji z pomyślnym zakończeniu i komunikat o błędzie pokrewne nie zawiera dodatkowe informacje wskazujące dlaczego. Ten artykuł zawiera procedurę rozwiązywania problemów dla problemów związanych z błędy wykonania działań aranżacji hybrydowych działań aranżacji pracownika.

Jeśli Azure problemu nie jest opisane w tym artykule, odwiedź fora Azure [MSDN i przepełnienie stosu](https://azure.microsoft.com/support/forums/). Możesz opublikować problem na forach te lub do [ @AzureSupport serwisie Twitter](https://twitter.com/AzureSupport). Ponadto można pliku żądanie obsługi Azure, wybierając pozycję **Uzyskaj pomoc** w witrynie [pomocy technicznej Azure](https://azure.microsoft.com/support/options/) .

## <a name="symptom"></a>Symptom

Wykonywanie działań aranżacji kończy się niepowodzeniem i zostanie zwrócony błąd "Akcja zadania, których nie można uruchomić"Aktywuj", ponieważ nieoczekiwane zatrzymanie procesu. Akcja zadania próbowano 3 razy".


## <a name="cause"></a>Przyczyna

Istnieje kilka możliwych przyczyn komunikat o błędzie: 

  1. Pracownik hybrydowego jest za serwera proxy lub zapory
  2. Na komputerze, którego pracownik hybrydowego jest uruchomione jest mniej niż sprzętowe minimalne [wymagania](automation-hybrid-runbook-worker.md#hybrid-runbook-worker-requirements) 
  3. Runbooks nie można uwierzytelnić z zasobów lokalnych


## <a name="cause-1-hybrid-runbook-worker-is-behind-proxy-or-firewall"></a>Przyczyna 1: Pracownik działań aranżacji hybrydowego jest za serwerem proxy lub zapory

Na komputerze, którego pracownik działań aranżacji hybrydowego jest uruchomiony na znajduje się za zapory lub serwera proxy i dostęp do sieci ruchu wychodzącego nie może być dozwolone lub skonfigurowane poprawnie.

### <a name="solution"></a>Rozwiązanie

Sprawdź, na komputerze nie ma dostępu dla połączeń wychodzących *. cloudapp.net na porty 443, 9354 i 30000 30199. 

## <a name="cause-2-computer-has-less-than-minimum-hardware-requirements"></a>Przyczyna 2: Komputer ma mniej niż minimalne wymagania sprzętowe

Komputerach pracowników działań aranżacji hybrydowych powinny spełniać minimalne wymagania sprzętowe przed wyznaczające go, aby udostępnić tę funkcję. W przeciwnym razie w zależności od wykorzystania zasobów innych procesy w tle i konfliktu spowodowany runbooks podczas wykonywania, komputer będzie stają się powyżej wykorzystane i powodować opóźnienia zadania działań aranżacji lub przekroczenia limitu czasu. 

### <a name="solution"></a>Rozwiązanie 

Najpierw upewnij się, że komputer określone używać funkcji hybrydowych działań aranżacji pracownik spełnia minimalne wymagania sprzętowe.  Jeśli tak, można monitorować wykorzystanie Procesora i pamięci, aby określić dowolny korelacji między wydajność hybrydowych działań aranżacji procesy i systemu Windows.  Jeśli istnieje pamięci lub Procesora ciśnienia, może to oznaczać konieczności uaktualnienia lub dodanie nowych procesorów lub zwiększanie pamięci, aby rozwiązać problem gardło zasobów i naprawianie błędu. Opcjonalnie zaznacz zasób różnych obliczeń, które mogą obsługiwać minimalne wymagania i skali, gdy wymaganego obciążenia wskazuje, że konieczne jest zwiększenie.         

## <a name="cause-3-runbooks-cannot-authenticate-with-local-resources"></a>Przyczyna 3: Runbooks nie można uwierzytelnić z zasobów lokalnych

### <a name="solution"></a>Rozwiązanie

Przejrzyj dziennik zdarzeń **SMA firmy Microsoft** dla odpowiednich zdarzenia z opisem *Win32 proces zakończony z kodem [4294967295]*.  Przyczyną tego błędu jest jeszcze nie skonfigurowano uwierzytelniania w swojej runbooks lub określony poświadczenia Uruchom jako dla grupy roboczych hybrydowych.  Sprawdź [uprawnienia działań aranżacji](automation-hybrid-runbook-worker.md#runbook-permissions) upewnij się, że jest poprawnie skonfigurowany uwierzytelniania dla Twojej runbooks.  


 

