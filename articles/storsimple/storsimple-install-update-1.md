<properties
   pageTitle="Instalowanie aktualizacji 1.2 na urządzeniu StorSimple | Microsoft Azure"
   description="Wyjaśniono, jak zainstalować 1.2 aktualizacji serii 8000 StorSimple na urządzeniu serii StorSimple 8000."
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
   ms.date="08/22/2016"
   ms.author="alkohli" />

# <a name="install-update-12-on-your-storsimple-device"></a>Instalowanie aktualizacji 1.2 na urządzeniu StorSimple

## <a name="overview"></a>Omówienie

Ten samouczek wyjaśniono, jak zainstalować aktualizację 1.2 na urządzeniu StorSimple jest uruchomiona wersja oprogramowania przed aktualizacji 1. Samouczek obejmuje także dodatkowe kroki wymagane do aktualizacji, gdy brama jest skonfigurowana w interfejsie sieciowym inną niż 0 danych urządzenia StorSimple.

Aktualizacja 1.2 zawiera aktualizacje oprogramowania urządzenia, aktualizacje sterowników LSI i aktualizacji oprogramowania układowego dysku. LSI sterownik aktualizacji oprogramowania i Brak aktualizacji i mogą być stosowane przez portal Azure klasyczny. Aktualizacje oprogramowania układowego dysku kłopotliwe środki aktualizacje i mogą być stosowane tylko za pośrednictwem interfejsu programu Windows PowerShell urządzenia.

Zależności od tego, jaka wersja urządzenie działa, można określić, jeśli zostanie zastosowany 1.2 aktualizacji. Możesz sprawdzić wersję oprogramowania urządzenia, przechodząc do sekcji **Szybkie rzut** urządzenia **pulpitu nawigacyjnego**.

</br>

| Jeśli zainstalowana wersja oprogramowania...   | Co się dzieje w portalu?                              |
|---------------------------------|--------------------------------------------------------------|
| Wersja Release - GA                    | Jeśli korzystasz z wersji (GA), nie mają zastosowania tej aktualizacji. Zapoznaj [się z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md) , aby zaktualizować urządzenie.|
| Aktualizowanie 0,1                      | Portal dotyczy 1.2 aktualizacji.                                |
| Aktualizowanie 0,2                      | Portal dotyczy 1.2 aktualizacji.                                |
| Aktualizowanie 0,3                      | Portal dotyczy 1.2 aktualizacji.                                |
| Aktualizacja 1                        | Tej aktualizacji nie będzie dostępna.                           |
| Aktualizowanie 1.1                      | Tej aktualizacji nie będzie dostępna.                           |

</br>

> [AZURE.IMPORTANT]

> -  1.2 aktualizacji mogą nie być widoczne natychmiast, ponieważ czynności fazowy wdrożenia aktualizacji. Skanowanie w poszukiwaniu aktualizacji w ciągu kilku dni ponownie jako aktualizację staną się dostępne wkrótce.
> - Aktualizacja zestawu ręczne i automatyczne testów przed w celu ustalenia stanu urządzenia pod względem sprzętu stan oraz połączeń sieciowych. Te wstępne są sprawdzane tylko wtedy, gdy zastosowanie aktualizacji z portalu klasyczny Azure.
> - Zaleca się zainstalowanie aktualizacji oprogramowania i sterowników za pośrednictwem portalu klasyczny Azure. Należy tylko Przejdź interfejsu programu Windows PowerShell urządzenia (instalowanie aktualizacji), jeśli wyboru sprzed aktualizacji bramy nie powiedzie się w portalu. Aktualizacje może potrwać 5-10 godzin, aby zainstalować (w tym aktualizacji systemu Windows). Aktualizacje tryb konserwacji musi być zainstalowany za pomocą interfejsu programu Windows PowerShell urządzenia. Podczas aktualizacji tryb konserwacji są kłopotliwe środki aktualizacje, te spowoduje dół czasu dla swojego urządzenia.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-12-via-the-azure-classic-portal"></a>Instalowanie aktualizacji 1.2 za pośrednictwem portalu klasyczny Azure

Wykonaj poniższe czynności, aby zaktualizować urządzenie do [aktualizacji 1.2](storsimple-update1-release-notes.md). Tylko wtedy, gdy bramy skonfigurowana w interfejsie 0 sieci danych na urządzeniu, wykonaj poniższą procedurę.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Upewnij się, że urządzenie działa **1.2 aktualizacji serii 8000 StorSimple (6.3.9600.17584)**. **Ostatnia aktualizacja daty** należy również zmienić. Zobaczysz także, że są dostępne aktualizacje tryb konserwacji (ten komunikat może być nadal wyświetlane przez 24 godziny, po zainstalowaniu aktualizacji).

    Konserwacja tryb aktualizacje są kłopotliwe środki aktualizacje, które powodują przestoje urządzenia i mogą być stosowane tylko za pośrednictwem interfejsu programu Windows PowerShell urządzenia.

    ![Strony konserwacji składników] (./media/storsimple-install-update-1/InstallUpdate12_10M.png "Strony konserwacji składników")

13. Pobierz aktualizacje tryb konserwacji za pomocą kroki wymienione w [celu pobrania poprawki]( #to-download-hotfixes) wyszukiwanie i pobieranie KB3063416, które aktualizacje oprogramowania układowego dysku instalacji (pozostałe aktualizacje powinny być już zainstalowana przez teraz).

13. Wykonaj kroki wymienione w [instalacji i sprawdź, czy poprawki konserwacji](#to-install-and-verify-maintenance-mode-hotfixes) zainstalowanie aktualizacji tryb konserwacji.

14. W portalu klasyczny Azure przejdź na stronę **konserwacji** i u dołu strony kliknij pozycję **Skanowanie aktualizacji** Sprawdź aktualizacje systemu Windows, a następnie kliknij pozycję **Zainstaluj aktualizacje**. Zakończeniu po pomyślnym są zainstalowane wszystkie aktualizacje.



## <a name="install-update-12-on-a-device-that-has-a-gateway-configured-for-a-non-data-0-network-interface"></a>Instalowanie aktualizacji 1.2 na urządzeniu, zawierającą bramę skonfigurowane dla karty sieciowej 0-danych

Tę procedurę należy używać tylko wtedy, gdy nie zostanie wyboru bramy, podczas próby zainstalowania aktualizacji za pośrednictwem portalu klasyczny Azure. Sprawdź zakończy się niepowodzeniem, masz bramy przypisane do karty sieciowej 0-danych i urządzenie działa wersja oprogramowania przed aktualizacji 1. Jeśli urządzenia nie ma bramy w interfejsie sieciowym 0-danych, możesz zaktualizować urządzenie bezpośrednio z poziomu portalu klasyczny Azure. Zobacz [Instalowanie aktualizacji 1.2 za pośrednictwem portalu klasyczny Azure](#install-update-1.2-via-the-azure-classic-portal).

Wersje oprogramowania, które mogą być uaktualnione przy użyciu tej metody są Update 0,1, aktualizacja 0,2 i Update 0,3.


> [AZURE.IMPORTANT]
>
> - Jeśli urządzenie jest wersja Release (GA), skontaktuj się z [Pomocy technicznej firmy Microsoft](storsimple-contact-microsoft-support.md) , aby pomóc w aktualizacji.
> - Tę procedurę należy należy wykonać tylko raz, aby zastosować 1.2 aktualizacji. Klasyczny portalu Azure umożliwia stosowanie kolejnych aktualizacji.

Jeśli Twoje urządzenie działa sprzed aktualizacji oprogramowania 1 i ma bramy ustawić dla karty sieciowej inną niż 0 danych, możesz zastosować 1.2 aktualizacji w następujących sposobów:

- **Opcja 1**: pobieranie aktualizacji i zastosować go za pomocą `Start-HcsHotfix` polecenia cmdlet z interfejsu programu Windows PowerShell urządzenia. Jest to metoda zalecana. **Nie należy używać tej metody można użyć do zastosowania 1.2 aktualizacji, jeśli Twoje urządzenie działa 1.0 aktualizacji lub Aktualizuj 1.1.**

- **Opcja 2**: usuwanie konfiguracji bramy i zainstaluj aktualizację bezpośrednio z poziomu portalu klasyczny Azure.


W poniższych sekcjach znajdują się szczegółowe instrukcje dla każdego z nich.

## <a name="option-1-use-windows-powershell-for-storsimple-to-apply-update-12-as-a-hotfix"></a>Opcja 1: Użyj Windows PowerShell dla StorSimple zastosować 1.2 aktualizacji jako poprawka

Tę procedurę należy używać tylko wtedy, gdy używasz aktualizacji 0,1, 0,2, 0,3 oraz jeśli czeku bramy nie powiodło się podczas próby zainstalowania aktualizacji z portalu klasyczny Azure. Jeśli korzystasz z wersji produktu oprogramowania, zapoznaj się z [Pomocy technicznej firmy Microsoft](storsimple-contact-microsoft-support.md) , aby zaktualizować urządzenie.

Aby zainstalować aktualizację 1.2 jako poprawki, musisz pobrać i zainstalować następujące poprawki:

| Kolejność  | KB        | Opis             | Typ aktualizacji  |
|--------|-----------|-------------------------|------------- |
| 1      | KB3063418 | Aktualizacji oprogramowania         |  Zwykłe     |
| 2      | KB3043005 | Aktualizacja Kontroler LSI skojarzenia zabezpieczeń |  Zwykłe     |
| 3      | KB3063416 | Sprzętowego dysku           | Konserwacja  |

Przed wykonaniem tej procedury, aby zastosować aktualizację, upewnij się, że:

- Oba kontrolery urządzeń są w trybie online.

Wykonaj poniższe czynności, aby zastosować 1.2 aktualizacji. **Aktualizacje może zająć około 2 godzin, aby ukończyć (około 30 minut w przypadku oprogramowania 30 minut w przypadku sterownika 45 minut w przypadku sprzętowego dysku).**

[AZURE.INCLUDE [storsimple-install-update-option1](../../includes/storsimple-install-update-option1.md)]


## <a name="option-2-use-the-azure-classic-portal-to-apply-update-12-after-removing-the-gateway-configuration"></a>Opcja 2: Stosowanie 1.2 aktualizacji po usunięciu konfiguracji bramy za pomocą portalu klasyczny Azure

Ta procedura dotyczy tylko urządzeń StorSimple, które jest używana wersja oprogramowania przed aktualizacji 1 i mieć bramy ustawiać karty sieciowej niż danych 0. Należy wyczyścić bramy przed zastosowaniem aktualizacji.

Aktualizacja może potrwać kilka godzin. Jeśli hosty w różnych podsieci, usuwając konfiguracji bramy na interfejsach iSCSI może spowodować przestoje. Zalecamy, aby skonfigurować danych 0 dla ruchu iSCSI, aby zmniejszyć przestoje.

Wykonaj poniższe czynności, aby wyłączyć sieciowej z bramą, a następnie zastosować aktualizację.

[AZURE.INCLUDE [storsimple-install-update-option2](../../includes/storsimple-install-update-option2.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]


## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o [wersji 1.2 aktualizacji](storsimple-update1-release-notes.md).
