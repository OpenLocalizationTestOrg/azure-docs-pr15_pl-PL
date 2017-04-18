<properties
   pageTitle="Instalowanie aktualizacji 2 na urządzeniu StorSimple | Microsoft Azure"
   description="Wyjaśniono, jak zainstalować StorSimple 8000 serii aktualizacji 2 na urządzeniu serii StorSimple 8000."
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
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="install-update-2-on-your-storsimple-device"></a>Instalowanie aktualizacji 2 na urządzeniu StorSimple

## <a name="overview"></a>Omówienie

Ten samouczek wyjaśniono, jak zainstalować aktualizacji 2 na urządzeniu StorSimple uruchomiona starsza wersja oprogramowania przez portal Azure klasyczny. Samouczek obejmuje także kroki wymagane do aktualizacji, gdy bramy jest skonfigurowany w interfejsie sieciowym inną niż 0 danych urządzenia StorSimple i chcesz zaktualizować 1 wersji sprzed aktualizacji oprogramowania.

Aktualizacja 2 zawiera aktualizacje oprogramowania urządzenia, aktualizacje sterowników LSI i aktualizacji oprogramowania układowego dysku. LSI aktualizacji oprogramowania urządzenia i Brak aktualizacji i mogą być stosowane przez portal Azure klasyczny. Aktualizacje oprogramowania układowego dysku kłopotliwe środki aktualizacje i mogą być stosowane tylko za pośrednictwem interfejsu programu Windows PowerShell urządzenia.

> [AZURE.IMPORTANT]

> -  Aktualizacji 2 mogą nie być widoczne natychmiast, ponieważ czynności fazowy wdrożenia aktualizacji. Skanowanie w poszukiwaniu aktualizacji w ciągu kilku dni ponownie jako aktualizację staną się dostępne wkrótce.
> - Zestaw ręczne i automatyczne sprawdzanie wstępna jest wykonywane przed Zainstaluj, aby określić kondycji urządzenia pod względem sprzętu stan oraz połączeń sieciowych. Te wstępne są sprawdzane tylko wtedy, gdy zastosowanie aktualizacji z portalu klasyczny Azure.
> - Zaleca się zainstalowanie aktualizacji oprogramowania i sterowników za pośrednictwem portalu klasyczny Azure. Należy tylko Przejdź interfejsu programu Windows PowerShell urządzenia (instalowanie aktualizacji), jeśli wyboru sprzed aktualizacji bramy nie powiedzie się w portalu. Aktualizacje może potrwać 4-7 godzin, aby zainstalować (w tym aktualizacji systemu Windows). Aktualizacje tryb konserwacji musi być zainstalowany za pomocą interfejsu programu Windows PowerShell urządzenia. Podczas aktualizacji tryb konserwacji są kłopotliwe środki aktualizacje, te spowoduje dół czasu dla swojego urządzenia.
> - Działania opcjonalne Menedżera migawkę StorSimple, upewnij się, że Twoja wersja Menedżera migawki została uaktualniona do aktualizacji 2 przed zaktualizowaniem urządzenie.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-2-via-the-azure-classic-portal"></a>Instalowanie aktualizacji 2 za pośrednictwem portalu klasyczny Azure

Wykonaj poniższe czynności, aby zaktualizować urządzenia do [aktualizacji 2](storsimple-update2-release-notes.md).


> [AZURE.NOTE]
Aktualizacja 2 umożliwia firmie Microsoft uwzględniał dodatkowych informacji diagnostycznych z urządzenia. W wyniku gdy nasz zespół operacje identyfikuje urządzenia, które występują problemy, możemy mają większe możliwości zbieranie informacji z urządzenia i sprawdzanie pod kątem błędów. Akceptowanie aktualizacji nr 2, umożliwia nam do obsługi aktywne.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Upewnij się, że urządzenie działa **StorSimple 8000 serii aktualizacji 2 (6.3.9600.17673)**. **Ostatnia aktualizacja daty** należy również zmienić. Zobaczysz także, że są dostępne aktualizacje tryb konserwacji (ten komunikat może być nadal wyświetlane przez 24 godziny, po zainstalowaniu aktualizacji).

    Konserwacja tryb aktualizacje są kłopotliwe środki aktualizacje, które powodują przestoje urządzenia i mogą być stosowane tylko za pośrednictwem interfejsu programu Windows PowerShell urządzenia. W niektórych przypadkach po uruchomieniu 1.2 aktualizacji oprogramowania sprzętowego dysku może już być aktualne, w takim przypadku nie musisz zainstalować wszystkie aktualizacje tryb konserwacji.

13. Pobierz aktualizacje tryb konserwacji za pomocą kroki wymienione w [celu pobrania poprawki](#to-download-hotfixes) wyszukiwanie i pobieranie KB3121899, które instalacje aktualizacji oprogramowania układowego dysku (pozostałe aktualizacje powinny być już zainstalowana przez teraz).

13. Wykonaj kroki wymienione w [instalacji i sprawdź, czy poprawki konserwacji](#to-install-and-verify-maintenance-mode-hotfixes) zainstalowanie aktualizacji tryb konserwacji.


## <a name="install-update-2-as-a-hotfix"></a>Instalowanie aktualizacji 2 jako poprawka

Jeśli podczas próby zainstalowania aktualizacji za pośrednictwem portalu klasyczny Azure nie wyboru bramy, wykonaj poniższą procedurę. Sprawdź zakończy się niepowodzeniem, masz bramy przypisane do karty sieciowej 0-danych i urządzenie działa wersja oprogramowania przed aktualizacji 1.

Wersji oprogramowania, które mogą być uaktualnione przy użyciu metody poprawki są aktualizacji 0,1, aktualizacji 0,2 i aktualizacji 0,3, aktualizacji 1, aktualizacja 1.1 i 1.2 aktualizacji. Metoda poprawki obejmuje następujące trzy kroki:

- Pobierz poprawki z wykazu usługi Microsoft Update.
- Zainstaluj i sprawdź poprawki zwykła tryb.
- Zainstaluj i sprawdź poprawki tryb konserwacji.

Aby zainstalować aktualizacji 2 jako poprawki, możesz pobrać i zainstalować następujące poprawki:

| Kolejność  | KB        | Opis                    | Typ aktualizacji  |
|--------|-----------|-------------------------|------------- |
| 1      | KB3121901 | Aktualizacji oprogramowania         |  Zwykłe     |
| 2      | KB3121900 | Sterownik LSI              |  Zwykłe     |
| 3      | KB3080728 | Poprawka Storport </br> Windows Server 2012 R2 |  Zwykłe     |
| 4      | KB3090322 | Poprawka spaceport </br> Windows Server 2012 R2 |  Zwykłe     |
| 5      | KB3121899 | Sprzętowego dysku           | Konserwacja  |


> [AZURE.IMPORTANT]
>
> - Jeśli urządzenie jest wersja Release (GA), skontaktuj się z [Pomocy technicznej firmy Microsoft](storsimple-contact-microsoft-support.md) , aby pomóc w aktualizacji.
> - Tę procedurę należy należy wykonać tylko raz, aby zastosować aktualizacji 2. Klasyczny portalu Azure umożliwia stosowanie kolejnych aktualizacji.
> - Instalacja każdej poprawki może potrwać około 20 minut do wykonania. Zainstaluj całkowity czas jest zbliżony 2 godziny.
> - Przed wykonaniem tej procedury, aby zastosować aktualizację, upewnij się, że oba kontrolery urządzeń online i wszystkie składniki sprzętowe jest prawidłowy.

Wykonaj poniższe czynności, aby zastosować tę aktualizację jako poprawki.

[AZURE.INCLUDE [storsimple-install-update2-hotfix](../../includes/storsimple-install-update2-hotfix.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]



## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o [wersji aktualizacji 2](storsimple-update2-release-notes.md).
