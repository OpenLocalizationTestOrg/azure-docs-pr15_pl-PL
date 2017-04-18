<properties
   pageTitle="Tworzenie bramy aplikacji za pomocą portalu | Microsoft Azure"
   description="Dowiedz się, jak utworzyć bramy aplikacji przy użyciu portalu"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
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

# <a name="create-an-application-gateway-by-using-the-portal"></a>Tworzenie bramy aplikacji za pomocą portalu

> [AZURE.SELECTOR]
- [Azure portal](application-gateway-create-gateway-portal.md)
- [Azure PowerShell Menedżera zasobów](application-gateway-create-gateway-arm.md)
- [Azure klasycznego programu PowerShell](application-gateway-create-gateway.md)
- [Azure szablonu Menedżera zasobów](application-gateway-create-gateway-arm-template.md)
- [Polecenie Azure](application-gateway-create-gateway-cli.md)

Azure brama aplikacji jest równoważenia obciążenia warstwy-7. Zapewnia on awaryjnym przeniesieniu routingu wydajności żądania HTTP między różnych serwerów, czy są one na chmurze lub lokalnego. Brama aplikacji oferuje wiele funkcji aplikacji dostarczenia kontroler (ADC), w tym równoważenia obciążenia HTTP koligacji sesji plików cookie, Secure Sockets Layer (SSL) offload sondy niestandardowe kondycji, obsługę wielu witryn i wiele innych. Aby uzyskać pełną listę obsługiwanych funkcji, odwiedź stronę [Omówienie bramy aplikacji](application-gateway-introduction.md)

## <a name="scenario"></a>Scenariusz

W tym scenariuszu możesz dowiedzieć się, jak utworzyć bramy aplikacji za pomocą portalu Azure.

W tym scenariuszu będzie:

- Tworzenie bramy średnie aplikacji z dwoma wystąpieniami.
- Tworzenie wirtualnych sieci o nazwie AdatumAppGatewayVNET z zastrzeżone blok CIDR 10.0.0.0/16.
- Tworzenie podsieci o nazwie Appgatewaysubnet, której używa 10.0.0.0/28 jako jego blok CIDR.
- Konfigurowanie certyfikatu SSL odciążania.

![Przykład scenariusza][scenario]

>[AZURE.IMPORTANT] Sondy dodatkowej konfiguracji bramy aplikacji, w tym niestandardowe kondycji, adresów puli wewnętrznej bazy danych i dodatkowe reguły są skonfigurowane po skonfigurowaniu bramy aplikacji, a nie podczas początkowego wdrażania.

## <a name="before-you-begin"></a>Przed rozpoczęciem

Azure bramy aplikacji wymaga osobnej podsieci. Podczas tworzenia wirtualnej sieci, upewnij się, pozostawienie miejsca adres do dostępnych jest kilka podsieci. Po rozmieszczeniu bramy aplikacji do podsieci bram tylko dodatkowe aplikacji są w stanie mają zostać dodane do danej podsieci.

## <a name="create-the-application-gateway"></a>Tworzenie bramy aplikacji

### <a name="step-1"></a>Krok 1

Przejdź do portalu Azure kliknij pozycję **Nowy** > **sieci** > **Brama aplikacji**

![Tworzenie bramy aplikacji][1]

### <a name="step-2"></a>Krok 2

Następnie wypełnij podstawowe informacje o aplikacji bramy. Po zakończeniu kliknij **przycisk OK**

Informacje potrzebne do podstawowych ustawień jest:

- **Nazwa** - nazwa bramy aplikacji.
- **Poziom** — jest to warstwa bramy aplikacji. Dostępne są dwie warstwy **WAF** i **Standardowy**. WAF włącza funkcję Zapora aplikacji sieci web.
- Dostępne opcje są (**małe**, **Średnie**i **duże**), **rozmiar SKU** — to ustawienie jest rozmiarem bramy aplikacji. *Mały nie jest dostępna, jeśli WAF warstwa jest zaznaczona*
- **Zliczanie wystąpień** — liczba wystąpień, ta wartość musi być liczbą od 2 do 10.
- **Grupa zasobów** — grupa zasobów, aby wstrzymać bramy aplikacji, może to być nowej lub istniejącej grupy zasobów.
- **Lokalizacja** — region bramy aplikacji jest tej samej lokalizacji w grupie zasobów. *Lokalizacja jest ważne jako wirtualnej sieci i publiczny adres IP musi znajdować się w tej samej lokalizacji co bramy*.

![Karta pokazywanie podstawowych ustawień][2]

>[AZURE.NOTE] Wystąpienia liczba 1 jest dostępny do celów testowych. Należy wiedzieć, że wszelkie licznik wystąpień w dwóch przypadkach nie są objęte umowa dotycząca poziomu usług i dlatego nie zaleca się. Małe bramy są ma być używany dla deweloperów przetestuj, a nie do celów produkcyjnych.

### <a name="step-3"></a>Krok 3

Po zdefiniowaniu podstawowych ustawień następnym krokiem jest określenie wirtualnej sieci może być używany. Wirtualna sieć zawiera bramy aplikacji równoważenia obciążenia dla aplikacji.

Kliknij przycisk **Wybierz wirtualna sieć** skonfigurowanie wirtualnej sieci.

![Karta z ustawieniami aplikacji bramy][3]

### <a name="step-4"></a>Krok 4

W karta *Wybierz wirtualnej sieci* kliknij przycisk **Utwórz nowy**

Gdy nie zostało wyjaśnione w tym scenariuszu, istniejącą sieć wirtualna może zostać wybrany na tym etapie.  Użycie istniejącej sieci wirtualnej należy wiedzieć, że wirtualną sieć wymaga puste podsieci lub podsieci tylko zasoby bramy aplikacji może być używany.

![Wybierz pozycję Karta wirtualnej sieci][4]

### <a name="step-5"></a>Krok 5

Wypełnij informacje o sieci w karta **Tworzenie wirtualnych sieci** zgodnie z opisem w poprzedniej opis [scenariusza](#scenario) .

![Tworzenie wirtualnych sieci karta z wprowadzonymi informacjami][5]

### <a name="step-6"></a>Krok 6

Po utworzeniu wirtualną sieć następnym krokiem jest określenie zewnętrzną IP bramy aplikacji. W tym momencie wybór jest między publiczną lub prywatny adres IP zewnętrzną. Wybór zależy od tego, czy aplikacja jest internet przeciwległych lub wewnętrznego tylko. W tym scenariuszu założono, przy użyciu publicznego adresu IP. Aby wybrać prywatny adres IP, można kliknąć przycisk **prywatne** . Wybrano opcję automatycznie przypisany adres IP lub klikając pole wyboru **Wybierz określony adres IP prywatny** , aby wprowadzić ręcznie.

### <a name="step-7"></a>Krok 7

Kliknij przycisk **Wybierz publiczny adres IP**. Jeśli istniejący publiczny adres IP jest dostępny może zostać wybrany w tym miejscu, w tym scenariuszu można tworzyć nowe publiczny adres IP. Kliknij przycisk **Utwórz nowy**

![Wybierz pozycję publiczna karta adres IP][6]

### <a name="step-8"></a>Krok 8

Następnie przyjazną nazwę publiczny adres IP i kliknij **przycisk OK**

![Tworzenie publicznej karta adres IP][7]

### <a name="step-9"></a>Krok 9

Ostatnie ustawienie, aby skonfigurować podczas tworzenia bramy aplikacji jest Konfiguracja odbiornika.  Użycie **protokołu http** nic się nie zostało skonfigurowanie i można kliknąć **przycisk OK** . Użycie **protokołu https** konfiguracji są wymagane dodatkowe.

Aby użyć **https**, wymagany jest certyfikat. Klucz prywatny certyfikatu jest konieczne, więc PFX eksportowanie certyfikatu i hasła trzeba być udostępniana.

### <a name="step-10"></a>Krok 10

Kliknij **HTTPS**, kliknij ikonę **folderu** obok pola tekstowego **PFX przekazywanie certyfikatu** .
Przejdź do certyfikatu pfx w systemie plików. Gdy jest zaznaczona, przyjazną nazwę certyfikatu i wpisz hasło do pliku pfx.

Po zakończeniu kliknij **przycisk OK** , aby przejrzeć ustawienia bramy aplikacji.

![Sekcja konfiguracji odbiornika na karta Ustawienia][9]

### <a name="step-11"></a>Krok 11

Przejrzyj stronę podsumowania, a następnie kliknij **przycisk OK**.  Teraz bramy aplikacji jest umieszczone w kolejce i utworzone.

### <a name="step-12"></a>Krok 12

Po utworzeniu bramy aplikacji łączyć się z nim w portalu, aby kontynuować konfigurowanie bramy aplikacji.

![Widok zasobów bramy aplikacji][10]

Te kroki tworzenia bramy podstawowe aplikacji z domyślnych ustawień odbiornika, puli wewnętrznej bazy danych, ustawienia http wewnętrznej bazy danych i reguły. Można modyfikować poniższe ustawienia do potrzeb wdrożenia po pomyślnym inicjowania obsługi administracyjnej.

## <a name="next-steps"></a>Następne kroki

W tym scenariuszu tworzy bramy aplikacji domyślnej. Następne kroki mają brama aplikacji przez dodawanie członków puli, modyfikowanie ustawień i dostosowywanie reguły w bramy, aby działał poprawnie.

Dowiedz się, jak tworzenie niestandardowych kondycji sondy, odwiedź witrynę [Tworzenie sondy kondycji niestandardowych](application-gateway-create-probe-portal.md)

Dowiedz się, jak skonfigurować odciążanie protokołu SSL i wykonać kosztów odszyfrowywania SSL wyłączanie serwerów sieci web, odwiedź witrynę [Skonfigurować odciążanie protokołu SSL](application-gateway-ssl-portal.md)

Dowiedz się, jak chronić aplikacji przy użyciu [Zapory aplikacji sieci Web](application-gateway-webapplicationfirewall-overview.md) funkcji aplikacji bramy.

<!--Image references-->
[1]: ./media/application-gateway-create-gateway-portal/figure1.png
[2]: ./media/application-gateway-create-gateway-portal/figure2.png
[3]: ./media/application-gateway-create-gateway-portal/figure3.png
[4]: ./media/application-gateway-create-gateway-portal/figure4.png
[5]: ./media/application-gateway-create-gateway-portal/figure5.png
[6]: ./media/application-gateway-create-gateway-portal/figure6.png
[7]: ./media/application-gateway-create-gateway-portal/figure7.png
[8]: ./media/application-gateway-create-gateway-portal/figure8.png
[9]: ./media/application-gateway-create-gateway-portal/figure9.png
[10]: ./media/application-gateway-create-gateway-portal/figure10.png
[scenario]: ./media/application-gateway-create-gateway-portal/scenario.png
