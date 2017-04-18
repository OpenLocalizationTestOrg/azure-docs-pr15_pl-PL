<properties
   pageTitle="Instalowanie aktualizacji 2.2 na urządzeniu StorSimple | Microsoft Azure"
   description="Wyjaśniono, jak zainstalować StorSimple 8000 serii aktualizacji 2.2 na urządzeniu serii StorSimple 8000."
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
   ms.date="08/02/2016"
   ms.author="alkohli" />

# <a name="install-update-22-on-your-storsimple-device"></a>Instalowanie aktualizacji 2.2 na urządzeniu StorSimple

## <a name="overview"></a>Omówienie

Ten samouczek wyjaśniono, jak zainstalować aktualizację 2.2 na urządzeniu StorSimple uruchomiona starsza wersja oprogramowania przez portal Azure klasyczny i przy użyciu metody poprawki. Metoda poprawki jest używana podczas brama jest skonfigurowana w interfejsie sieciowym inną niż 0 danych urządzenia StorSimple i chcesz zaktualizować 1 wersji sprzed aktualizacji oprogramowania.

2.2 aktualizacja oprogramowania urządzenia, WMI i aktualizacje iSCSI. Aktualizowania z wersji 2.1, aktualizacji oprogramowania urządzenia muszą być stosowane. Jeśli aktualizowane z wersji 2 sprzed aktualizacji, również trzeba będzie stosowanie LSI sterownik, Spaceport Storport i aktualizacji oprogramowania układowego dysku. Oprogramowanie urządzenia, WMI iSCSI, LSI sterownik, Spaceport i Storport poprawki Brak aktualizacji i mogą być stosowane przez portal Azure klasyczny. Aktualizacje oprogramowania układowego dysku kłopotliwe środki aktualizacje i mogą być stosowane tylko za pośrednictwem interfejsu programu Windows PowerShell urządzenia. 

> [AZURE.IMPORTANT]

> - Zestaw ręczne i automatyczne sprawdzanie wstępna jest wykonywane przed Zainstaluj, aby określić kondycji urządzenia pod względem sprzętu stan oraz połączeń sieciowych. Te wstępne są sprawdzane tylko wtedy, gdy zastosowanie aktualizacji z portalu klasyczny Azure.
> - Zaleca się zainstalowanie aktualizacji oprogramowania i sterowników za pośrednictwem portalu klasyczny Azure. Należy tylko Przejdź interfejsu programu Windows PowerShell urządzenia (instalowanie aktualizacji), jeśli wyboru sprzed aktualizacji bramy nie powiedzie się w portalu. W zależności od wersji, którą chcesz zaktualizować z aktualizacji może potrwać godzin 2,5 1,5 do zainstalowania. Aktualizacje tryb konserwacji musi być zainstalowany za pomocą interfejsu programu Windows PowerShell urządzenia. Podczas aktualizacji tryb konserwacji są kłopotliwe środki aktualizacje, te spowoduje dół czasu dla swojego urządzenia.
> - Działania opcjonalne Menedżera migawkę StorSimple, upewnij się, że przed zaktualizowaniem urządzenie została uaktualniona wersji Menedżera migawkę 2.2 aktualizacji.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-22-via-the-azure-classic-portal"></a>Instalowanie aktualizacji 2.2 za pośrednictwem portalu klasyczny Azure

Wykonaj poniższe czynności, aby zaktualizować urządzenia do [aktualizacji 2.2](storsimple-update21-release-notes.md).


> [AZURE.NOTE]
Jeśli chcesz zastosować aktualizacji 2 lub później (w tym Update 2.1), Microsoft będą mogli pobierać dodatkowe informacje diagnostyczne z urządzenia. W wyniku gdy nasz zespół operacje identyfikuje urządzenia, które występują problemy, możemy mają większe możliwości zbieranie informacji z urządzenia i sprawdzanie pod kątem błędów. Akceptowanie aktualizacji 2 lub nowszy, umożliwia nam do obsługi aktywne.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Upewnij się, że urządzenie działa **StorSimple 8000 serii aktualizacji 2.2 (6.3.9600.17708)**. **Ostatnia aktualizacja daty** należy również zmienić. 

    Jeśli aktualizujesz z wersji przed aktualizacji nr 2, pojawi się także o dostępnych aktualizacjach tryb konserwacji (ten komunikat może być nadal wyświetlane przez 24 godziny, po zainstalowaniu aktualizacji).

    Konserwacja tryb aktualizacje są kłopotliwe środki aktualizacje, które powodują przestoje urządzenia i mogą być stosowane tylko za pośrednictwem interfejsu programu Windows PowerShell urządzenia. W niektórych przypadkach po uruchomieniu 1.2 aktualizacji oprogramowania sprzętowego dysku może już być aktualne, w takim przypadku nie musisz zainstalować wszystkie aktualizacje tryb konserwacji.

    Jeśli aktualizujesz z aktualizacji nr 2, urządzenie powinno być teraz aktualne. Możesz pominąć pozostałe kroki.

13. Pobierz aktualizacje tryb konserwacji za pomocą kroki wymienione w [celu pobrania poprawki](#to-download-hotfixes) wyszukiwanie i pobieranie KB3121899, które instalacje aktualizacji oprogramowania układowego dysku (pozostałe aktualizacje powinny być już zainstalowana przez teraz).

13. Wykonaj kroki wymienione w [instalacji i sprawdź, czy poprawki konserwacji](#to-install-and-verify-maintenance-mode-hotfixes) zainstalowanie aktualizacji tryb konserwacji. 

  

## <a name="install-update-22-as-a-hotfix"></a>Instalowanie aktualizacji 2.2 jako poprawka

Jeśli podczas próby zainstalowania aktualizacji za pośrednictwem portalu klasyczny Azure nie wyboru bramy, wykonaj poniższą procedurę. Sprawdź zakończy się niepowodzeniem, masz bramy przypisane do karty sieciowej 0-danych i urządzenie działa wersja oprogramowania przed aktualizacji 1.

Wersje oprogramowania, które mogą być uaktualnione przy użyciu metody poprawki są:

- Aktualizowanie 0,1, 0,2, 0,3
- Aktualizowanie 1, 1.1, 1.2
- Aktualizacja 2, 2.1 

> [AZURE.IMPORTANT]
>
> - Jeśli urządzenie jest wersja Release (GA), skontaktuj się z [Pomocy technicznej firmy Microsoft](storsimple-contact-microsoft-support.md) , aby pomóc w aktualizacji.

Metoda poprawki obejmuje następujące trzy kroki:

- Pobierz poprawki z wykazu usługi Microsoft Update.
- Zainstaluj i sprawdź poprawki zwykła tryb.
- Zainstaluj i sprawdź poprawki tryb konserwacji (tylko wtedy, gdy aktualizacje sprzed aktualizacji oprogramowania 2).

#### <a name="download-updates-for-a-device-running-update-21-software"></a>Pobieranie aktualizacji dla urządzenia z oprogramowaniem 2.1 aktualizacji

**Jeśli Twoje urządzenie działa aktualizowanie 2.1**, musisz pobrać tylko urządzenia aktualizacji oprogramowania KB3179904. Zainstaluj tylko plik binarny nazwą "wszystkie hcsmdssoftwareudpate". Nie można zainstalować Cis i nazwą MDS agent update `all-cismdsagentupdatebundle`. Komunikat o błędzie spowoduje aktualizujesz. Jest to Brak aktualizacja, Jo nie zostanie zerwane i urządzenie nie będą mieć dowolną przestoje.


#### <a name="download-updates-for-a-device-running-update-2-software"></a>Pobieranie aktualizacji dla urządzenia z oprogramowaniem aktualizacji 2

**Jeśli Twoje urządzenie działa aktualizacji nr 2**, musisz pobrać i zainstalować następujące poprawki w określonej kolejności:

| Kolejność  | KB        | Opis                    | Typ aktualizacji  | Instalowanie czasu |
|--------|-----------|-------------------------|------------- |-------------|
| 1.      | KB3179904 | Aktualizacji oprogramowania & #42;  |  Zwykłe <br></br>Bezproblemowa     | ~ 45 minut |
| 2.      | KB3146621 | Pakiet iSCSI | Zwykłe <br></br>Bezproblemowa  | ~ 20 minutach |
| 3.      | KB3103616 | Pakiet WMI |  Zwykłe <br></br>Bezproblemowa      | ~ min 12 |


 & #42;  Aktualizacja *notatkę, oprogramowania składa się z dwóch plików binarnych: nazwą aktualizacji oprogramowania urządzenia `all-hcsmdssoftwareupdate` i agenta Cis i Mds nazwą `all-cismdsagentupdatebundle`. Aktualizacji oprogramowania urządzenie musi być zainstalowany przed agenta Cis i Mds. Ponadto należy ponownie uruchomić kontroler aktywnej za pośrednictwem `Restart-HcsController` aktualizowanie polecenia cmdlet po zastosowaniu agenta Cis i MDS (i przed zastosowanie pozostałe aktualizacje).* 

#### <a name="download-updates-for-a-device-running-pre-update-2-software"></a>Pobieranie aktualizacji dla urządzenia z systemem sprzed aktualizacji oprogramowania 2

**Jeśli Twoje urządzenie działa w wersjach 0,2, 0,3, 1.0 i 1.1**, musisz pobrać i zainstaluj LSI sterownik i sprzętowego zaktualizuj oprócz oprogramowania, iSCSI i WMI aktualizacji. Ta aktualizacja jest już zainstalowana, jeśli są uruchomione aktualizowanie 1.2 lub 2. 
 
| Kolejność  | KB        | Opis                    | Typ aktualizacji  | Instalowanie czasu |
|--------|-----------|-------------------------|------------- |-------------|
| 4.      | KB3121900 | Sterownik LSI i sprzętowego             |  Zwykłe <br></br>Bezproblemowa      | ~ 20 minutach |


<br></br>
**Jeśli Twoje urządzenie działa w wersjach 0,2, 0,3, 1.0, 1.1 i 1.2**, należy pobrać i zainstalować Spaceport i napraw Storport. Te są już zainstalowane, jeśli są uruchomione aktualizacji 2.

| Kolejność  | KB        | Opis                    | Typ aktualizacji  | Instalowanie czasu |
|--------|-----------|-------------------------|------------- |-------------|
| 5.      | KB3090322 | Poprawka spaceport </br> Windows Server 2012 R2 |  Zwykłe <br></br>Bezproblemowa      | ~ 20 minutach |
| 6.      | KB3080728 | Poprawka Storport </br> Windows Server 2012 R2 |  Zwykłe <br></br>Bezproblemowa      | ~ 20 minutach |



<br></br>
Konieczne może również zainstalować aktualizacji oprogramowania układowego dysku. Można sprawdzić, czy potrzebujesz aktualizacje oprogramowania układowego dysku, uruchamiając `Get-HcsFirmwareVersion` polecenia cmdlet. Jeśli korzystasz z tych wersji sprzętowego: `XMGG`, `XGEG`, `KZ50`, `F6C2`, `VR08`, a następnie nie trzeba zainstalować następujące aktualizacje.


| Kolejność  | KB        | Opis                    | Typ aktualizacji  | Instalowanie czasu |
|--------|-----------|-------------------------|------------- |-------------|
| 7.      | KB3121899 | Sprzętowego dysku              |  Konserwacja <br></br>Kłopotliwe środki      | ~ 30 minut |
 
<br></br>

> [AZURE.IMPORTANT]
>
> - Tę procedurę należy należy wykonać tylko raz, aby zastosować 2.2 aktualizacji. Klasyczny portalu Azure umożliwia stosowanie kolejnych aktualizacji.
> - Jeśli aktualizacje aktualizacji nr 2, podczas instalacji sumy jest zbliżony 1,5 godziny.
> - Przed wykonaniem tej procedury, aby zastosować aktualizację, upewnij się, że kontrolery urządzeń są w trybie online i wszystkie składniki sprzętowe jest prawidłowy.

Wykonaj poniższe czynności, aby pobrać i zainstalować poprawki.

[AZURE.INCLUDE [storsimple-install-update21-hotfix](../../includes/storsimple-install-update21-hotfix.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o [wersji 2.1 aktualizacji](storsimple-update21-release-notes.md).
