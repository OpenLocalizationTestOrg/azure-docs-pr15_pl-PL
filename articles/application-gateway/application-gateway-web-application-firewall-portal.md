<properties
   pageTitle="Tworzenie bramy aplikacji z zapory aplikacji sieci web za pomocą portalu | Microsoft Azure"
   description="Dowiedz się, jak utworzyć bramy aplikacji z zapory aplikacji sieci web za pomocą portalu"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-an-application-gateway-with-web-application-firewall-by-using-the-portal"></a>Tworzenie bramy aplikacji z zapory aplikacji sieci web za pomocą portalu

> [AZURE.SELECTOR]
- [Azure portal](application-gateway-web-application-firewall-portal.md)
- [Azure PowerShell Menedżera zasobów](application-gateway-web-application-firewall-powershell.md)

Zapora aplikacji sieci web (WAF) w polu Brama aplikacji Azure chroni aplikacji sieci web z typowych atakami oparte na sieci web, takich jak uruchomienie SQL, atakami skryptów między witrynami i hijacks sesji. Aplikacja sieci Web zabezpiecza przed wiele OWASP górny 10 typowych web luk.

Azure brama aplikacji jest równoważenia obciążenia warstwy-7. Zapewnia on awaryjnym przeniesieniu routingu wydajności żądania HTTP między różnych serwerów, czy są one na chmurze lub lokalnego.
Aplikacji oferuje wiele funkcji aplikacji dostarczenia kontroler (ADC), w tym równoważenia obciążenia HTTP koligacji sesji plików cookie, Secure Sockets Layer (SSL) offload sondy kondycji niestandardowe, obsługę wielu witryn i wiele innych.
Aby uzyskać pełną listę obsługiwanych funkcji, odwiedź stronę [Omówienie bramy aplikacji](application-gateway-introduction.md)

## <a name="scenarios"></a>Scenariusze

W tym artykule istnieją dwa scenariusze:

W pierwszym scenariuszu możesz Dowiedz się, jak [dodać zapory aplikacji sieci web do istniejącej bramy aplikacji](#add-web-application-firewall-to-an-existing-application-gateway).

W drugim scenariuszu możesz Dowiedz się, jak [utworzyć bramy aplikacji z zapory aplikacji sieci web](#create-an-application-gateway-with-web-application-firewall)

![Przykład scenariusza][scenario]

>[AZURE.NOTE] Sondy dodatkowej konfiguracji bramy aplikacji, w tym niestandardowe zdrowia, adresów puli wewnętrznej bazy danych i dodatkowe reguły są skonfigurowane po skonfigurowaniu bramy aplikacji, a nie podczas początkowego wdrażania.

## <a name="before-you-begin"></a>Przed rozpoczęciem

Azure bramy aplikacji wymaga osobnej podsieci. Podczas tworzenia wirtualnej sieci, upewnij się, pozostawienie miejsca adres do dostępnych jest kilka podsieci. Po rozmieszczeniu bramy aplikacji do podsieci bram tylko dodatkowe aplikacji są w stanie mają zostać dodane do danej podsieci.

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Dodawanie zapory aplikacji sieci web do istniejącej bramy aplikacji

W tym scenariuszu aktualizacji bramy istniejącej aplikacji do obsługi zapory aplikacji sieci web w tryb zapobiegania.

### <a name="step-1"></a>Krok 1

Przejdź do portalu Azure, zaznacz istniejący bramy aplikacji.

![Tworzenie bramy aplikacji][1]

### <a name="step-2"></a>Krok 2

Kliknij pozycję **Konfiguracja** i aktualizowanie ustawień bramy aplikacji. Po zakończeniu kliknij przycisk **Zapisz**

Dostępne są następujące ustawienia, aby zaktualizować istniejące bramy aplikacji do obsługi zapory aplikacji sieci web:

- **Poziom** — warstwie zaznaczone musi być **WAF** do obsługi zapory aplikacji sieci web
- **Rozmiar SKU** — to ustawienie jest rozmiarem bramy aplikacji z zapory aplikacji sieci web, dostępne są następujące opcje (**Średni** i **duży**).
- **Stan zapory** — to ustawienie wyłącza lub Włącza zaporę aplikacji sieci web.
- **Tryb zapory** — to ustawienie jest jak dotyczy szkodliwy ruch zapory aplikacji sieci web. Tryb **wykrywania** tylko dzienniki zdarzeń, gdzie tryb **zapobiegania** dzienniki zdarzeń i przestaje szkodliwy ruch.

![Karta pokazywanie podstawowych ustawień][2]

>[AZURE.NOTE] Aby wyświetlić dzienniki zapory aplikacji sieci web, musi być włączony narzędzia diagnostyczne i ApplicationGatewayFirewallLog zaznaczone. Wystąpienia liczba 1 jest dostępny do celów testowych. Należy wiedzieć, że wszelkie licznik wystąpień w dwóch przypadkach nie są objęte umowa dotycząca poziomu usług i dlatego nie zaleca się. Małe bram nie są dostępne podczas korzystania z zapory aplikacji sieci web.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Tworzenie bramy aplikacji z zapory aplikacji sieci web

W tym scenariuszu będzie:

- Tworzenie bramy aplikacji zapory aplikacji web średniej z dwóch wystąpień.
- Tworzenie wirtualnych sieci o nazwie AdatumAppGatewayVNET z zastrzeżone blok CIDR 10.0.0.0/16.
- Tworzenie podsieci o nazwie Appgatewaysubnet, która używa 10.0.0.0/28 jako jego blok CIDR.
- Konfigurowanie certyfikatu SSL odciążania.

### <a name="step-1"></a>Krok 1

Przejdź do portalu Azure kliknij pozycję **Nowy** > **sieci** > **Brama aplikacji**

![Tworzenie bramy aplikacji][1-1]

### <a name="step-2"></a>Krok 2

Następnie wypełnij podstawowe informacje o aplikacji bramy. Pamiętaj wybrać **WAF** jako warstwy. Po zakończeniu kliknij **przycisk OK**

Informacje potrzebne do podstawowych ustawień jest:

- **Nazwa** - nazwa bramy aplikacji.
- **Warstwy** - warstwie bramy aplikacji, dostępne są następujące opcje (**Standardowy** i **WAF**). Zapora aplikacji sieci Web jest dostępna tylko w warstwie WAF.
- **Rozmiar SKU** — to ustawienie jest rozmiarem bramy aplikacji, dostępne są następujące opcje (**Średni** i **duży**).
- **Zliczanie wystąpień** — liczba wystąpień, ta wartość musi być liczbą od **2** do **10**.
- **Grupa zasobów** — grupa zasobów, aby wstrzymać bramy aplikacji, może to być nowej lub istniejącej grupy zasobów.
- **Lokalizacja** — region bramy aplikacji jest tej samej lokalizacji w grupie zasobów. *Lokalizacja jest ważne jako wirtualnej sieci i publiczny adres IP musi znajdować się w tej samej lokalizacji co bramy*.

![Karta pokazywanie podstawowych ustawień][2-2]

>[AZURE.NOTE] Wystąpienia liczba 1 jest dostępny do celów testowych. Należy wiedzieć, że wszelkie licznik wystąpień w dwóch przypadkach nie są objęte umowa dotycząca poziomu usług i dlatego nie zaleca się. Małe bram nie są obsługiwane dla scenariuszy zapory aplikacji sieci web.

### <a name="step-3"></a>Krok 3

Po zdefiniowaniu podstawowych ustawień następnym krokiem jest określenie wirtualnej sieci może być używany. Wirtualna sieć zawiera bramy aplikacji równoważenia obciążenia dla aplikacji.

Kliknij przycisk **Wybierz wirtualna sieć** skonfigurowanie wirtualnej sieci.

![Karta z ustawieniami aplikacji bramy][3]

### <a name="step-4"></a>Krok 4

W karta **Wybierz wirtualnej sieci** kliknij przycisk **Utwórz nowy**

Gdy nie zostało wyjaśnione w tym scenariuszu, zaznaczenie było na tym etapie istniejącą sieć wirtualną.  Użycie istniejącej sieci wirtualnej należy wiedzieć, że wirtualną sieć wymaga puste podsieci lub podsieci tylko zasoby bramy aplikacji może być używany.

![Wybierz pozycję Karta wirtualnej sieci][4]

### <a name="step-5"></a>Krok 5

Wypełnij informacje o sieci w karta **Tworzenie wirtualnych sieci** zgodnie z opisem w poprzedniej opis [scenariusza](#scenario) .

![Tworzenie wirtualnych sieci karta z wprowadzonymi informacjami][5]

### <a name="step-6"></a>Krok 6

Po utworzeniu wirtualną sieć następnym krokiem jest określenie zewnętrzną IP bramy aplikacji. W tym momencie wybór jest zawarte między publicznego lub prywatny adres IP zewnętrzną. Wybór zależy od tego, czy aplikacja jest internet przeciwległych lub wewnętrznego tylko. W tym scenariuszu założono, przy użyciu publicznego adresu IP. Aby wybrać prywatny adres IP, można kliknąć przycisk **prywatne** . Wybrano opcję automatycznie przypisany adres IP lub klikając pole wyboru **Wybierz określony adres IP prywatny** , aby wprowadzić ręcznie.

Kliknij przycisk **Wybierz publiczny adres IP**. Jeśli istniejący publiczny adres IP jest dostępny może zostać wybrany w tym miejscu, w tym scenariuszu można tworzyć nowe publiczny adres IP. Kliknij przycisk **Utwórz nowy**

![Wybierz pozycję publiczna karta adres IP][6]

### <a name="step-7"></a>Krok 7

Następnie przyjazną nazwę publiczny adres IP i kliknij **przycisk OK**

![Tworzenie publicznej karta adres IP][7]

### <a name="step-8"></a>Krok 8

Następnie możesz skonfigurować Konfiguracja odbiornika.  Użycie **protokołu http** nic się nie zostało skonfigurowanie i można kliknąć **przycisk OK** . Użycie **protokołu https** konfiguracji są wymagane dodatkowe.

Aby użyć **https**, wymagany jest certyfikat. Klucz prywatny certyfikatu jest wymagana, aby wyeksportować PFX potrzeb certyfikat dostarczanych i hasło do pliku.

Kliknij **HTTPS**, kliknij ikonę **folderu** obok pola tekstowego **PFX przekazywanie certyfikatu** .
Przejdź do certyfikatu pfx w systemie plików. Gdy jest zaznaczona, przyjazną nazwę certyfikatu i wpisz hasło do pliku pfx.

Po zakończeniu kliknij **przycisk OK** , aby przejrzeć ustawienia bramy aplikacji.

![Sekcja konfiguracji odbiornika na karta Ustawienia][8]

### <a name="step-9"></a>Krok 9

Konfigurowanie ustawień określonych **WAF** .

- **Stan zapory** — to ustawienie włącza WAF lub wyłączone.
- **Tryb zapory** - to ustawienie określa, że WAF akcje przejmuje szkodliwy ruch. Jeśli wybrano **wykrywania** , rejestrowane jest tylko ruch.  Jeśli wybrano **zapobiegania** , ruch jest zalogowany i zatrzymane z 403 Unauthorized.

![ustawienia zapory aplikacji sieci Web][9]

### <a name="step-10"></a>Krok 10

Przejrzyj stronę podsumowania, a następnie kliknij **przycisk OK**.  Teraz bramy aplikacji jest umieszczone w kolejce i utworzone.

### <a name="step-12"></a>Krok 12

Po utworzeniu bramy aplikacji łączyć się z nim w portalu, aby kontynuować konfigurowanie bramy aplikacji.

![Widok zasobów bramy aplikacji][10]

Te kroki tworzenia bramy podstawowe aplikacji z domyślnych ustawień odbiornika, puli wewnętrznej bazy danych, ustawienia http wewnętrznej bazy danych i reguły. Można modyfikować poniższe ustawienia do potrzeb wdrożenia po pomyślnym inicjowania obsługi administracyjnej

## <a name="next-steps"></a>Następne kroki

Dowiedz się, jak skonfigurować rejestrowanie diagnostyczne logowania zdarzenia, które wykryty lub braku możliwości zapory aplikacji sieci Web, odwiedź witrynę [Aplikacji bramy diagnostyki](application-gateway-diagnostics.md)

Dowiedz się, jak tworzenie niestandardowych kondycji sondy, odwiedź witrynę [Tworzenie sondy kondycji niestandardowe](application-gateway-create-probe-portal.md)

Dowiedz się, jak skonfigurować odciążanie protokołu SSL i wykonać kosztów odszyfrowywania SSL wyłączanie serwerów sieci web, odwiedź witrynę [Skonfigurować odciążanie protokołu SSL](application-gateway-ssl-portal.md)

<!--Image references-->
[1]: ./media/application-gateway-web-application-firewall-portal/figure1.png
[2]: ./media/application-gateway-web-application-firewall-portal/figure2.png
[1-1]: ./media/application-gateway-web-application-firewall-portal/figure1-1.png
[2-2]: ./media/application-gateway-web-application-firewall-portal/figure2-2.png
[3]: ./media/application-gateway-web-application-firewall-portal/figure3.png
[4]: ./media/application-gateway-web-application-firewall-portal/figure4.png
[5]: ./media/application-gateway-web-application-firewall-portal/figure5.png
[6]: ./media/application-gateway-web-application-firewall-portal/figure6.png
[7]: ./media/application-gateway-web-application-firewall-portal/figure7.png
[8]: ./media/application-gateway-web-application-firewall-portal/figure8.png
[9]: ./media/application-gateway-web-application-firewall-portal/figure9.png
[10]: ./media/application-gateway-web-application-firewall-portal/figure10.png
[scenario]: ./media/application-gateway-web-application-firewall-portal/scenario.png