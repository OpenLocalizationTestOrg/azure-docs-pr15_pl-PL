<properties
    pageTitle="Rozwiązywanie problemów z kompresji plików w sieci CDN Azure | Microsoft Azure"
    description="Rozwiązywanie problemów z kompresji plików Azure CDN."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="casoper"/>
    
# <a name="troubleshooting-cdn-file-compression"></a>Rozwiązywanie problemów z sieci CDN kompresji plików

Ten artykuł ułatwia rozwiązywanie problemów z [sieci CDN kompresji pliku](cdn-improve-performance.md).

Jeśli potrzebujesz dodatkowej pomocy w dowolnym momencie, w tym artykule, można skontaktować się Azure ekspertów na [MSDN Azure i fora przepełnienie stosu](https://azure.microsoft.com/support/forums/). Ponadto można również pliku Azure pomocy technicznej zdarzenia. Przejdź do [witryny pomocy technicznej Azure](https://azure.microsoft.com/support/options/) , a następnie kliknij przycisk **Uzyskiwanie pomocy technicznej**.

## <a name="symptom"></a>Symptom

Stopień kompresji punkt końcowy jest włączona, ale pliki są zwracane nieskompresowane.

>[AZURE.TIP] Aby sprawdzić, czy pliki są zwracane skompresowany, należy użyć narzędzia, takie jak [Fiddler](http://www.telerik.com/fiddler) lub przeglądarki [narzędzi dla deweloperów](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).  Nagłówki odpowiedzi wyboru HTTP zwracane sieci CDN buforowanej zawartości.  Jeśli jest nagłówka o nazwie `Content-Encoding` z wartością **gzip**, **bzip2**lub **korygowania**zawartości jest skompresowany.
>
>![Kodowanie zawartość nagłówka](./media/cdn-troubleshoot-compression/cdn-content-header.png)

## <a name="cause"></a>Przyczyna

Istnieje kilka możliwych przyczyn, w tym:

- Żądanej zawartości nie kwalifikuje się do kompresji.
- Kompresja nie jest włączona dla typu żądanego pliku.
- Żądania HTTP nie zawiera nagłówka żąda typu kompresji prawidłowe.

## <a name="troubleshooting-steps"></a>Procedura rozwiązywania problemów

> [AZURE.TIP] Podobnie jak w przypadku wdrożeniem nowe punkty końcowe, zmian w konfiguracji sieci CDN Poświęć trochę czasu na propagowanie za pośrednictwem sieci.  Zazwyczaj zmiany zostaną zastosowane w ciągu 90 minut.  Jeśli po skonfigurowaniu kompresji dla punktu końcowego sieci CDN po raz pierwszy, należy rozważyć, trwa oczekiwanie 1-2 godziny aby upewnić się, że kompresji, którego ustawienia jest propagowana do POP. 

### <a name="verify-the-request"></a>Sprawdź żądanie

Najpierw należy wykonać wykonuje szybkie sprawdzenie żądanie.  W przeglądarce [narzędzi dla deweloperów](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) służy do wyświetlania wnioski.

- Sprawdź żądanie jest wysyłana do adresu URL punktu końcowego `<endpointname>.azureedge.net`i nie swojego pochodzenia.
- Upewnij się, żądanie zawiera **Akceptowania kodowania** nagłówka, a wartość w polu nagłówka **gzip**, **korygowania**lub **bzip2**.

> [AZURE.NOTE] Profile **CDN Azure z Akamai** obsługuje tylko kodowania **gzip** .

![Sieci CDN nagłówków żądania](./media/cdn-troubleshoot-compression/cdn-request-headers.png)

### <a name="verify-compression-settings-standard-cdn-profile"></a>Sprawdź ustawienia kompresji (CDN standardowy profil)

> [AZURE.NOTE] Ten krok jest stosowany tylko, jeśli Twój profil CDN jest **Azure CDN standardowy z Verizon** lub **Azure CDN standardowy z Akamai** profilu. 

Przejdź do punkt końcowy w [Azure portal](https://portal.azure.com) i kliknij przycisk **Konfiguruj** .

- Sprawdź, czy kompresja jest włączona.
- Sprawdź, czy typ MIME dla zawartości, aby skompresować znajduje się na liście formatów skompresowany.

![Ustawienia kompresji CDN](./media/cdn-troubleshoot-compression/cdn-compression-settings.png)

### <a name="verify-compression-settings-premium-cdn-profile"></a>Sprawdź ustawienia kompresji (Premium CDN profil)

> [AZURE.NOTE] Ten krok jest stosowany tylko w przypadku profilu CDN profilu **Premium CDN Azure z Verizon** .

Przejdź do punkt końcowy w [Azure portal](https://portal.azure.com) i kliknij przycisk **Zarządzaj** .  Dodatkowe portalu zostanie otwarty.  Umieść wskaźnik myszy na karcie **Dużych HTTP** , a następnie umieść wskaźnik myszy na menu wysuwane **Ustawienia pamięci podręcznej** .  Kliknij opcję **kompresji**. 

- Sprawdź, czy kompresja jest włączona.
- Sprawdź, czy na liście **Typów plików** zawiera listę przecinkami (bez spacji) typów MIME.
- Sprawdź, czy typ MIME dla zawartości, aby skompresować znajduje się na liście formatów skompresowany.

![Ustawienia kompresji premium CDN](./media/cdn-troubleshoot-compression/cdn-compression-settings-premium.png)

### <a name="verify-the-content-is-cached"></a>Upewnij się, że zawartość jest przechowywanych w pamięci podręcznej

> [AZURE.NOTE] Ten krok jest stosowany tylko, jeśli sieci CDN profil jest **CDN Azure z Verizon** (Standard lub Premium).

Za pomocą narzędzi dla deweloperów w przeglądarce, sprawdź nagłówków odpowiedzi, aby upewnić się, że plik jest buforowany w regionie, gdzie jest żądanego.

- Zaznacz nagłówek odpowiedzi **serwera** .  Nagłówek powinien mieć format **platformy (identyfikator POP/serwer)**, jak pokazano w poniższym przykładzie.
- Zaznacz nagłówek odpowiedzi **Pamięci podręcznej X** .  Nagłówek należy przeczytać **TRAFIEŃ**.  

![Nagłówki odpowiedzi sieci CDN](./media/cdn-troubleshoot-compression/cdn-response-headers.png)

### <a name="verify-the-file-meets-the-size-requirements"></a>Upewnij się, że plik spełnia wymagania rozmiar

> [AZURE.NOTE] Ten krok jest stosowany tylko, jeśli sieci CDN profil jest **CDN Azure z Verizon** (Standard lub Premium).

Kwalifikować się do kompresji, plik musi spełniać następujące wymagania rozmiar:

- Większe niż 128 bajtów.
- Mniejszy niż 1 MB.

### <a name="check-the-request-at-the-origin-server-for-a-via-header"></a>Sprawdź żądanie na serwerze origin nagłówka **za pośrednictwem**

Na serwerze sieci web nagłówka **za pośrednictwem** protokołu HTTP wskazuje, że żądania jest przekazywanych przez serwer proxy.  Serwery sieci web firmy Microsoft IIS domyślnie nie są kompresowane odpowiedzi, gdy żądanie zawiera nagłówka **za pośrednictwem** .  Aby zmienić to zachowanie, należy wykonać następujące czynności:

- **Usług IIS 6**: [Ustawianie HcNoCompressionForProxies = "FALSE" w oknie dialogowym właściwości metabazy usług IIS](https://msdn.microsoft.com/library/ms525390.aspx)
- **Usług IIS 7 i nowsze**: [ustawić **noCompressionForHttp10** i **noCompressionForProxies** FAŁSZ w konfiguracji serwera](http://www.iis.net/configreference/system.webserver/httpcompression)

