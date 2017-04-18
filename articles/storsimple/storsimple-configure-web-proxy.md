<properties 
   pageTitle="Konfigurowanie serwera proxy sieci web dla urządzenia StorSimple | Microsoft Azure"
   description="Dowiedz się, jak skonfigurować ustawienia serwera proxy sieci web dla swojego urządzenia StorSimple za pomocą programu Windows PowerShell dla StorSimple."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="configure-web-proxy-for-your-storsimple-device"></a>Konfigurowanie serwera proxy sieci web dla swojego urządzenia StorSimple

## <a name="overview"></a>Omówienie

Ten samouczek opisano konfigurowanie i wyświetlanie ustawień serwera proxy sieci web dla swojego urządzenia StorSimple za pomocą programu Windows PowerShell dla StorSimple. Ustawienia serwera proxy sieci web są używane przez urządzenia StorSimple komunikowania się z chmurą. Serwer proxy sieci web umożliwia dodawanie dodatkowej warstwy zabezpieczeń, filtr zawartości pamięci podręcznej w celu ułatwienia wymagania dotyczące przepustowości lub nawet pomocy dotyczącej analizy.

Serwer proxy sieci Web jest opcjonalnym dla swojego urządzenia StorSimple. Serwer proxy sieci web tylko przy użyciu programu Windows PowerShell można skonfigurować dla StorSimple. Konfiguracja jest procesem dwuetapowym w następujący sposób:

1. Skonfiguruj ustawienia serwera proxy w sieci web przy użyciu Kreatora konfiguracji lub środowiska Windows PowerShell dla StorSimple poleceń cmdlet.

2. Następnie Włącz ustawienia serwera proxy sieci web skonfigurowaną przy użyciu programu Windows PowerShell dla StorSimple poleceń cmdlet.

Po zakończeniu konfiguracji serwera proxy sieci web dla StorSimple można wyświetlać ustawienia serwera proxy sieci web skonfigurowaną w programach usługi Microsoft Azure StorSimple menedżera, jak i programu Windows PowerShell. 

Po przeczytaniu tego samouczka, będą mogli:

- Konfigurowanie serwera proxy sieci web przy użyciu Kreatora konfiguracji i poleceń cmdlet
- Włącz serwer proxy sieci web przy użyciu poleceń cmdlet
- Wyświetlanie ustawień serwera proxy sieci web w portalu klasyczny Azure
- Rozwiązywanie problemów z błędami podczas konfiguracji serwera proxy dla sieci web


## <a name="configure-web-proxy-via-windows-powershell-for-storsimple"></a>Konfigurowanie serwera proxy sieci web przy użyciu programu Windows PowerShell dla StorSimple

Użyj jednej z następujących czynności na konfigurowanie ustawień serwera proxy w sieci web:

- Kreator konfiguracji do wykonaj kroki konfiguracji.

- Polecenia cmdlet w programie Windows PowerShell dla StorSimple.

W poniższych sekcjach omówiono każdej z tych metod.

## <a name="configure-web-proxy-via-the-setup-wizard"></a>Konfigurowanie serwera proxy sieci web za pomocą Kreatora konfiguracji

Kreator konfiguracji umożliwia wykonaj kroki konfiguracji serwera proxy w sieci web. Wykonaj poniższe czynności, aby skonfigurować serwer proxy sieci web na urządzeniu.

#### <a name="to-configure-web-proxy-via-the-setup-wizard"></a>Aby skonfigurować serwer proxy sieci web za pomocą Kreatora konfiguracji

1. W menu Konsola szeregowa wybierz opcję 1, **Zaloguj się przy użyciu pełnego dostępu** i podaj **hasło administratora urządzenia**. Wpisz następujące polecenie, aby rozpocząć sesję kreatora konfiguracji:

    `Invoke-HcsSetupWizard`

2. Jeśli jest użycie Kreatora konfiguracji rejestracji urządzenia po raz pierwszy, należy skonfigurować wszystkie ustawienia sieci wymagane do momentu wyświetlenia konfiguracji serwera proxy w sieci web. Jeśli urządzenie jest już zarejestrowana, możesz zaakceptować wszystkie ustawienia sieci skonfigurowane do momentu wyświetlenia konfiguracji serwera proxy w sieci web. W Kreatorze konfiguracji gdy zostanie wyświetlony monit, aby skonfigurować ustawienia serwera proxy w sieci web, wpisz **Tak**.

3. **Adres URL serwera Proxy sieci Web**Określ adres IP lub w pełni kwalifikowaną nazwę domeny (FQDN) serwera proxy usług sieci web i numer portu TCP, który ma urządzenia używane do komunikowania się z chmurą. Użyj następującego formatu:

    `http://<IP address or FQDN of the web proxy server>:<TCP port number>`

    Domyślnie numer portu TCP 8080 zostało określone.

4. Wybierz typ uwierzytelniania **NTLM**, **podstawowe**lub **Brak**. Podstawowe jest najmniej bezpieczna uwierzytelniania dla konfiguracji serwera proxy. NT LAN Manager (NTLM) jest protokołem uwierzytelniania wysokiego poziomu zabezpieczeń i złożone, korzystającym z systemu obsługi wiadomości trzy, (czasami cztery Jeśli wymagane jest dodatkowe integralności) do uwierzytelnienia użytkownika. Uwierzytelnianie domyślne jest NTLM. Aby uzyskać więcej informacji zobacz [podstawowe](http://hc.apache.org/httpclient-3.x/authentication.html) i [NTLM](http://hc.apache.org/httpclient-3.x/authentication.html). 

    > [AZURE.IMPORTANT] **W usłudze Menedżer StorSimple wykresami monitorowania urządzenie nie działa, gdy podstawowe lub uwierzytelnianie NTLM jest włączona w konfiguracji serwera proxy dla urządzenia. W przypadku monitorowania wykresów do pracy należy upewnić się, że uwierzytelniania wybrano opcję Brak.**

5. Jeśli korzystasz z uwierzytelniania, należy podać **Nazwę użytkownika serwera Proxy w sieci Web** i **Hasła Proxy usług sieci Web**. Konieczne będzie również Potwierdź hasło.

    ![Konfigurowanie serwera Proxy sieci Web w StorSimple Device1](./media/storsimple-configure-web-proxy/IC751830.png)

Jeśli rejestrujesz się urządzenia po raz pierwszy, należy kontynuować rejestracji. Jeśli urządzenie była już zarejestrowana, Kreator zakończy działanie. Skonfigurowane ustawienia zostaną zapisane.

Serwer proxy sieci Web zostanie również jest włączone. Można pominąć krok [włączyć serwer proxy sieci web](#enable-web-proxy) i przejść bezpośrednio do [widoku ustawienia serwera proxy w sieci web w portalu klasyczny Azure](#view-web-proxy-settings-in-the-azure-classic-portal).


## <a name="configure-web-proxy-via-windows-powershell-for-storsimple-cmdlets"></a>Konfigurowanie serwera proxy sieci web przy użyciu programu Windows PowerShell dla polecenia cmdlet StorSimple

Innym sposobem Konfiguruj ustawienia serwera proxy w sieci web jest za pomocą programu Windows PowerShell dla StorSimple poleceń cmdlet. Wykonaj poniższe czynności, aby skonfigurować serwer proxy sieci web.

#### <a name="to-configure-web-proxy-via-cmdlets"></a>Aby skonfigurować serwer proxy sieci web przy użyciu polecenia cmdlet

1. W menu Konsola szeregowa wybierz opcję 1, **Zaloguj się przy użyciu pełnego dostępu**. Po wyświetleniu monitu należy podać **hasło administratora urządzenia**. Hasło domyślne to `Password1`.

2. W wierszu polecenia wpisz:

    `Set-HcsWebProxy -Authentication NTLM -ConnectionURI "<http://<IP address or FQDN of web proxy server>:<TCP port number>" -Username "<Username for web proxy server>"`

    Udostępnia i Potwierdź hasło, gdy zostanie wyświetlony monit, tak jak pokazano poniżej.

    ![Konfigurowanie serwera Proxy sieci Web w StorSimple Device3](./media/storsimple-configure-web-proxy/IC751831.png)

Serwer proxy sieci web jest skonfigurowany i musi być włączona.

## <a name="enable-web-proxy"></a>Włącz serwer proxy sieci web

Serwer proxy sieci Web jest domyślnie wyłączona. Po skonfigurowaniu ustawień serwera proxy sieci web na urządzeniu StorSimple, musisz włączyć ustawienia serwera proxy w sieci web za pomocą programu Windows PowerShell dla StorSimple.

> [AZURE.NOTE] **Ten krok nie jest wymagane, jeśli użyto Kreatora konfiguracji skonfigurowanie serwera proxy sieci web. Serwer proxy sieci Web jest automatycznie włączana domyślnie po sesji Kreatora konfiguracji.**

W programie Windows PowerShell dla StorSimple włączyć serwer proxy sieci web na urządzeniu, wykonaj następujące czynności:

#### <a name="to-enable-web-proxy"></a>Aby włączyć serwer proxy sieci web

1. W menu Konsola szeregowa wybierz opcję 1, **Zaloguj się przy użyciu pełnego dostępu**. Po wyświetleniu monitu należy podać **hasło administratora urządzenia**. Hasło domyślne to `Password1`.

2. W wierszu polecenia wpisz:

    `Enable-HcsWebProxy`

    Teraz włączono konfiguracji serwera proxy sieci web na urządzeniu StorSimple.

    ![Konfigurowanie serwera Proxy sieci Web w StorSimple Device4](./media/storsimple-configure-web-proxy/IC751832.png)

## <a name="view-web-proxy-settings-in-the-azure-classic-portal"></a>Wyświetlanie ustawień serwera proxy sieci web w portalu klasyczny Azure

Ustawienia serwera proxy sieci web są skonfigurowane za pośrednictwem interfejsu programu Windows PowerShell i nie można zmienić z poziomu portalu klasyczny. Można jednak wyświetlać te ustawienia skonfigurowane w portalu klasyczny. Wykonaj poniższe czynności, aby wyświetlić serwer proxy sieci web.

#### <a name="to-view-web-proxy-settings"></a>Aby wyświetlić ustawienia serwera proxy w sieci web
1. Przejdź do **usługi Menedżera StorSimple > urządzeń**. Zaznacz i kliknij urządzenie, a następnie przejdź do **konfiguracji**.
1. Przewiń w dół na stronie **Konfigurowanie** do sekcji **Ustawienia serwera proxy w sieci Web** . Ustawienia serwera proxy sieci web skonfigurowaną można wyświetlić na urządzeniu StorSimple, tak jak pokazano poniżej.

    ![Widok serwera Proxy sieci Web w portalu zarządzania](./media/storsimple-configure-web-proxy/ViewWebProxyPortal_M.png)
 
## <a name="errors-during-web-proxy-configuration"></a>Błędy podczas konfiguracji serwera proxy dla sieci web

Jeśli ustawienia serwera proxy w sieci web zostały skonfigurowane niepoprawnie, komunikaty o błędach będą wyświetlane użytkownikowi w programie Windows PowerShell dla StorSimple. W poniższej tabeli opisano niektóre z tymi komunikatami o błędach, prawdopodobne przyczyny i zalecane działania.

|Numer serii.|Kod błędu HRESULT|Możliwe przyczyny|Zalecane działanie|
|:---|:---|:---|:---|
|1.|0x80070001|Polecenie jest wykonywane w stronie biernej kontroler i nie będzie mógł komunikować się z active kontrolera.|Uruchom polecenie na kontrolerze aktywna. Aby uruchomić polecenia w stronie biernej kontrolerze, będzie konieczne rozwiązać łączność w stronie biernej do aktywnego kontrolera. Konieczne będzie nawiązanie Support firmy Microsoft, jeśli to połączenie zostało przerwane.|
|2.|0x800710dd - identyfikator operacji jest nieprawidłowy|Ustawienia serwera proxy nie są obsługiwane na urządzeniu virtual StorSimple.|Ustawienia serwera proxy nie są obsługiwane na urządzeniu virtual StorSimple. Te można skonfigurować tylko na urządzeniu fizycznie StorSimple.|
|3.|0x80070057 - nieprawidłowy parametr|Jeden z parametrów dla ustawienia serwera proxy jest nieprawidłowy.|Identyfikator URI nie jest dostępna w poprawnym formacie. Użyj następującego formatu:`http://<IP address or FQDN of the web proxy server>:<TCP port number>`|
|4.|0x800706BA - serwer RPC nie jest dostępna|Głównej przyczyny jest jednym z następujących czynności:</br></br>Klaster nie jest w górę.</br></br>Usługa ścieżki danych nie działa.</br></br>Polecenie jest wykonywane w stronie biernej kontroler i nie będzie mógł komunikować się z active kontrolera.|Należy prowadzić Microsoft Support aby upewnić się, że klaster działa i działa usługa ścieżki danych.</br></br>Uruchom polecenie z aktywnego kontrolera. Chcesz Uruchom polecenie w stronie biernej kontrolerze, należy upewnić się, że w stronie biernej kontroler można komunikować się z active kontrolera. Konieczne będzie nawiązanie Support firmy Microsoft, jeśli to połączenie zostało przerwane.|
|5.|0x800706be - wywołanie RPC nie powiodło się|Klaster nie działa.|Należy prowadzić Support firmy Microsoft w celu zapewnienia klaster w górę.|
|6.|0x8007138f — nie można odnaleźć zasobu Klaster|Nie można odnaleźć zasobu klaster usługi platformy. Dzieje się tak, gdy instalacja nie pisane z wielkiej litery.|Może być konieczne wykonywanie fabryki Resetowanie na urządzeniu. Może być konieczne utworzyć zasób platformy. Skontaktuj się z Support firmy Microsoft dla następnych kroków.|
|7.|0x8007138c - zasobu Klaster nie w trybie online|Zasoby klastrów platformy lub ścieżki danych nie są w trybie online.|Skontaktuj się z Support firmy Microsoft w celu zapewnienia, że ścieżki danych oraz platformę zasobów usługi online.|

> [AZURE.NOTE] 
> 
> -  Powyżej listy komunikaty o błędach nie jest pełna. 
> - Błędy związane z ustawienia serwera proxy w sieci web nie będą wyświetlane w portalu klasyczny Azure w usłudze Menedżer StorSimple. Jeśli występuje problem z serwerem proxy sieci web po zakończeniu konfiguracji, stan urządzenia zostanie zmieniona na **tryb Offline** w portalu klasyczny. |

## <a name="next-steps"></a>Następne kroki

- Występują problemy podczas wdrażania urządzenia lub skonfigurować ustawienia serwera proxy w sieci web, zapoznaj się z [Rozwiązywanie problemów z wdrożeniem urządzenia StorSimple](storsimple-troubleshoot-deployment.md).

- Aby dowiedzieć się, jak korzystać z usługi Menedżera StorSimple, przejdź do [za pomocą usługi Menedżera StorSimple administrowanie urządzenia StorSimple](storsimple-manager-service-administration.md).
