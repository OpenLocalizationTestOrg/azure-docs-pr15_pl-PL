<properties 
   pageTitle="Rozwiązywanie problemów z wdrożonym urządzeniem StorSimple | Microsoft Azure"
   description="W tym artykule opisano, jak diagnozowanie i rozwiązywanie błędów występujących na urządzeniu StorSimple, która jest obecnie wdrożonej i operacyjne."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/16/2016"
   ms.author="v-sharos" />

# <a name="troubleshoot-an-operational-storsimple-device"></a>Rozwiązywanie problemów z działającego urządzenia StorSimple

## <a name="overview"></a>Omówienie

Ten artykuł zawiera przydatne rozwiązywania problemów wskazówki dotyczące rozwiązywania problemów z konfiguracją że mogą wystąpić po wdrożeniu i operacyjne urządzenia StorSimple. Go w tym artykule opisano typowe problemy, możliwe przyczyny i zalecanych kroków pomocne podczas rozwiązywania problemów, które mogą wystąpić podczas korzystania z programu Microsoft Azure StorSimple. Te informacje dotyczą zarówno urządzenia fizycznego lokalnego StorSimple i urządzenia wirtualnego StorSimple.

Na końcu tego artykułu, które można znaleźć listę kodów błędów, które mogą wystąpić podczas operacji Microsoft Azure StorSimple a także czynności można wykonać, aby naprawić błędy. 

## <a name="setup-wizard-process-for-operational-devices"></a>Proces Kreatora konfiguracji urządzeń operacyjne

Korzystanie z Kreatora konfiguracji ([Wywołaj HcsSetupWizard][1]) do Sprawdź konfigurację urządzenia, a następnie wykonaj czynności naprawcze, w razie potrzeby.

Po uruchomieniu Kreatora konfiguracji na urządzeniu wcześniej skonfigurowane i operacyjne różni się przebieg procesu. Można zmienić tylko następujące pozycje:

- Adres IP, maska podsieci i bramy
- Podstawowy serwer DNS
- Podstawowy serwer NTP
- Opcjonalnie web konfiguracji serwera proxy

Kreator konfiguracji wykonuje operacje związane z rejestracji zbieranie i urządzenia hasła.

## <a name="errors-that-occur-during-subsequent-runs-of-the-setup-wizard"></a>Błędy występujące podczas kolejnych zostanie uruchomiony Kreator konfiguracji

W poniższej tabeli opisano błędy mogą wystąpić podczas korzystania z Kreatora konfiguracji na urządzeniu z systemem operacyjnym, możliwe przyczyny błędów i zalecane akcje w celu ich rozwiązania. 

| Wartość nie. | Komunikat o błędzie lub warunku | Możliwe przyczyny | Zalecane działanie |
|:--- |:-------------------------- |:--------------- |:------------------ |
|  1  | Błąd 350032: To urządzenie już został dezaktywowany. | Po uruchomieniu Kreatora konfiguracji na urządzeniu, który zostanie dezaktywowana, zostanie wyświetlony ten błąd. | [Kontakt z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md) dla następnych kroków. Wyłączone urządzenia nie można umieścić w usłudze. Resetowanie factory może wymagać aktywowania urządzenie ponownie. |
|  2  | Wywołania HcsSetupWizard: ERROR_INVALID_FUNCTION (wyjątek od HRESULT: 0x80070001) | Aktualizacja serwera DNS kończy się niepowodzeniem. Ustawienia DNS są ustawień globalnych i zostaną zastosowane przez wszystkie interfejsy z obsługą sieci. | Włącz interfejs i ponownie zastosować ustawienia DNS. Może zakłócać sieć dla innych interfejsów włączone, ponieważ te ustawienia są globalnej. |
|  3  | Urządzenie wydaje się być w trybie online w portalu usługi Menedżera StorSimple, ale podczas próby wykonania minimalnej konfiguracji i zapisać konfigurację kończy się niepowodzeniem. | Podczas początkowej konfiguracji serwera proxy sieci web nie został skonfigurowany, nawet jeśli było serwer proxy rzeczywiste w miejscu. | Użyj polecenia [cmdlet Test HcsmConnection] [ 2] zlokalizować komunikat o błędzie. [Kontakt z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md) , jeśli nie możesz rozwiązać ten problem. |
|  4  | Wywołania HcsSetupWizard: Wartość nie mieści się w zakresie oczekiwanych. | Niepoprawna maska podsieci daje w wyniku tego błędu. Możliwe przyczyny są następujące: <ul><li> Maska podsieci jest lub jest pusty.</li><li>Format prefiks Ipv6 jest nieprawidłowy.</li><li>Interfejs jest włączona w chmurze, ale bramy jest brakujące lub niepoprawne.</li></ul>Zauważ, że dane 0 jest włączany automatycznie, chmury — Jeśli skonfigurować za pomocą Kreatora konfiguracji. | Aby określić ten problem, za pomocą podsieci 0.0.0.0 lub 256.256.256.256, a następnie spójrz na dane wyjściowe. Wprowadź prawidłowe wartości maski podsieci, bramy i prefiks Ipv6, stosownie do potrzeb. |
 
## <a name="error-codes"></a>Kody błędów

Błędy znajdują się w porządku numerycznym.

|Numer błędu|Tekst błędu lub opisu|Akcja polecane użytkownika|
|:---|:---|:---|
|10502|Wystąpił błąd podczas uzyskiwania dostępu do konta miejsca do magazynowania.|Poczekaj kilka minut i spróbuj ponownie. Jeśli ten błąd będzie nadal występował, skontaktuj się z programu Microsoft obsługą, dla następnych kroków.|
|40017|Wykonywanie kopii zapasowej nie powiodło się, jak nie można odnaleźć woluminu określonego w kopii zapasowej zasady na tym urządzeniu.|Spróbuj ponownie kopii zapasowej operacji, jeśli ten błąd będzie nadal występował, skontaktuj się z Support firmy Microsoft. dla następnych kroków.|
|40018|Wykonywanie kopii zapasowej nie powiodło się, jak znaleziono żadnych wielkości określone w zasadzie kopii zapasowej na urządzeniu. |Spróbuj ponownie kopii zapasowej operacji, jeśli ten błąd będzie nadal występował, skontaktuj się z Support firmy Microsoft. dla następnych kroków.|
|390061|System jest zajęty lub niedostępny.|Poczekaj kilka minut i spróbuj ponownie. Jeśli ten błąd będzie nadal występował, skontaktuj się z programu Microsoft obsługą, dla następnych kroków.|
|390143|Wystąpił błąd z kodem błędu 390143. (Nieznany błąd).|Jeśli ten błąd będzie nadal występował, skontaktuj się Support firmy Microsoft dla następnych kroków.|

## <a name="next-steps"></a>Następne kroki

Jeśli nie możesz rozwiązać ten problem, [skontaktuj się z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md) w celu uzyskania pomocy. 


[1]: https://technet.microsoft.com/en-us/%5Clibrary/Dn688135(v=WPS.630).aspx
[2]: https://technet.microsoft.com/en-us/%5Clibrary/Dn715782(v=WPS.630).aspx
