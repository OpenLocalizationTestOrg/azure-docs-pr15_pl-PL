<properties
    pageTitle="Zwiększanie wydajności kompresując pliki w sieci CDN Azure | Microsoft Azure"
    description="Dowiedz się, jak zwiększyć szybkość transferu pliku i zwiększa wydajność ładowania strony kompresując pliki w sieci CDN Azure."
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
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="improve-performance-by-compressing-files"></a>Zwiększanie wydajności kompresując pliki

Kompresji jest metodą prosty i efektywny, aby zwiększyć szybkość transferu pliku i zwiększyć wydajność ładowania strony zmniejszając rozmiar pliku przed wysłaniem z serwera. Zmniejsza koszty przepustowość i zapewnia bardziej odpowiednie dla użytkowników.

Istnieją dwa sposoby na umożliwienie kompresji:

- Możesz włączyć kompresji na serwerze pochodzenia, w której przypadku CDN zostaną przechodzą przez skompresowane pliki i przeprowadzania skompresowane pliki klientom przez firmę.
- Możesz włączyć kompresji bezpośrednio na serwery graniczne CDN, w których przypadku CDN zostaną Skompresuj pliki i służą do użytkowników końcowych, nawet jeśli nie są kompresowane przez serwer pochodzenia.

> [AZURE.IMPORTANT] Zmiany w konfiguracji sieci CDN Poświęć trochę czasu na propagowanie za pośrednictwem sieci.  Profilów <b>CDN Azure z Akamai</b> propagowanie zwykle wykonuje w obszarze minucie.  Profilów <b>CDN Azure z Verizon</b> zazwyczaj zobaczysz zmiany w ciągu 90 minut.  Jeśli po skonfigurowaniu kompresji dla punktu końcowego sieci CDN po raz pierwszy, należy rozważyć, trwa oczekiwanie 1-2 godziny aby upewnić się, że ustawienia jest propagowana do POP przed rozpoczęciem rozwiązywania kompresji

## <a name="enabling-compression"></a>Włączanie kompresji

> [AZURE.NOTE] Poziomy Standard i Premium CDN dostarczenie tej samej funkcji kompresji, ale różni się w interfejsie użytkownika.  Aby uzyskać więcej informacji o różnicach między poziomów standardowym i Premium CDN zobacz [Omówienie CDN Azure](cdn-overview.md).

### <a name="standard-tier"></a>Standardowy warstwy

> [AZURE.NOTE] W tej sekcji dotyczą **Azure CDN standardowe z Verizon** i **Azure CDN standardowy z Akamai** profile.

1. Karta profilu CDN kliknij punkt końcowy CDN, którą chcesz zarządzać.

    ![Punkty końcowe karta profilu sieci CDN](./media/cdn-file-compression/cdn-endpoints.png)

    Zostanie wyświetlona karta CDN punktu końcowego.

2. Kliknij przycisk **Konfiguruj** .

    ![Przycisk Zarządzaj CDN karta profilu](./media/cdn-file-compression/cdn-config-btn.png)

    Zostanie wyświetlona karta CDN konfiguracji.

3. Włącz **kompresję**.

    ![Opcje kompresji CDN](./media/cdn-file-compression/cdn-compress-standard.png)

4. Domyślne typy lub zmodyfikować listę, usuwając lub dodając typów plików.
    
    > [AZURE.TIP] Podczas to możliwe, zalecane jest nie dotyczą kompresji skompresowany formatach, na przykład ZIP MP3, MP4, JPG, itp.
    
5. Po wprowadzeniu zmian, kliknij przycisk **Zapisz** .

### <a name="premium-tier"></a>Poziom Premium

> [AZURE.NOTE] W tej sekcji dotyczą profile **Premium CDN Azure z Verizon** .

1. Z sieci CDN karta profilu kliknij przycisk **Zarządzaj** .

    ![Przycisk Zarządzaj CDN karta profilu](./media/cdn-file-compression/cdn-manage-btn.png)

    Zostanie otwarty do portalu zarządzania CDN.

2. Umieść wskaźnik myszy na karcie **Dużych HTTP** , a następnie umieść wskaźnik myszy na menu wysuwane **Ustawienia pamięci podręcznej** .  Polecenie **kompresji**.

    Zostaną wyświetlone opcje kompresji.

    ![Kompresowanie plików](./media/cdn-file-compression/cdn-compress-files.png)

3. Włącz kompresję, klikając przycisk radiowy **Kompresja włączona** .  Wprowadź typy MIME, które chcesz skompresować jako listy rozdzielany przecinkami (bez spacji) w polu tekstowym **Typy plików** .
        
    > [AZURE.TIP] Podczas to możliwe, zalecane jest nie dotyczą kompresji skompresowany formatach, na przykład ZIP MP3, MP4, JPG, itp. 

4. Po wprowadzeniu zmian, kliknij przycisk **Aktualizuj** .


## <a name="compression-rules"></a>Reguły kompresji

W poniższych tabelach opisano zachowanie kompresji Azure CDN dla każdego scenariusza.

> [AZURE.IMPORTANT] Dla **CDN Azure z Verizon** (Standard i Premium) są kompresowane tylko odpowiednich plików.  Aby kwalifikować się do kompresji, plik musi:
>
> - Być większa niż 128 bajtów.
> - Być mniejszy niż 1 MB.
> 
> **Sieci CDN Azure z Akamai**uprawniony do kompresji są wszystkie pliki.
>
> Dla wszystkich produktów Azure CDN plik musi być typ MIME, który został [skonfigurowany do kompresji](#enabling-compression).
>
> Profile **CDN Azure z Verizon** (Standard i Premium) obsługuje **gzip**, **korygowania**lub kodowanie **bzip2** .  Profile **CDN Azure z Akamai** obsługuje tylko kodowania **gzip** .
>
> Punkty końcowe **CDN Azure z Akamai** zawsze żądanie pliki **gzip** zakodowany od punktu początkowego, bez względu na żądanie klienta.

### <a name="compression-disabled-or-file-is-ineligible-for-compression"></a>Kompresja wyłączona lub plik jest nieodpowiednia kompresji

|Klient zażądał format (za pośrednictwem akceptowania kodowania nagłówek)|Format plików w pamięci podręcznej|Sieci CDN odpowiedzi do klienta|Notatki|
|----------------|-----------|------------|-----|
|Skompresowany|Skompresowany|Skompresowany|   |
|Skompresowany|Nieskompresowane|Nieskompresowane|    | 
|Skompresowany|Nie przechowywanych w pamięci podręcznej|Skompresowany lub nieskompresowane|W zależności od odpowiedzi pochodzenia|
|Nieskompresowane|Skompresowany|Nieskompresowane|    |
|Nieskompresowane|Nieskompresowane|Nieskompresowane|    |   
|Nieskompresowane|Nie przechowywanych w pamięci podręcznej|Nieskompresowane|     |

### <a name="compression-enabled-and-file-is-eligible-for-compression"></a>Kompresja włączona i plik jest uprawniony do kompresji

|Klient zażądał format (za pośrednictwem akceptowania kodowania nagłówek)|Format plików w pamięci podręcznej|Sieci CDN odpowiedzi do klienta|Notatki|
|----------------|-----------|------------|-----|
|Skompresowany|Skompresowany|Skompresowany|Sieci CDN transcodes między obsługiwane formaty|
|Skompresowany|Nieskompresowane|Skompresowany|Sieci CDN wykonuje kompresji|
|Skompresowany|Nie przechowywanych w pamięci podręcznej|Skompresowany|Sieci CDN wykonuje kompresję, jeśli origin zwraca nieskompresowane.  **Sieci CDN Azure z Verizon** będzie przekazać plik nieskompresowane po pierwszym żądaniu następnie skompresować i pamięci podręcznej w pliku o kolejne żądania.  Pliki z `Cache-Control: no-cache` nagłówka nigdy nie będą kompresowane. 
|Nieskompresowane|Skompresowany|Nieskompresowane|Sieci CDN wykonuje dekompresji|
|Nieskompresowane|Nieskompresowane|Nieskompresowane|     |  
|Nieskompresowane|Nie przechowywanych w pamięci podręcznej|Nieskompresowane|     |

## <a name="media-services-cdn-compression"></a>Usługi multimediów CDN kompresji

Sieci CDN usługi multimediów włączona przesyłanie strumieniowe punkty końcowe, kompresja jest włączona domyślnie dla następujących typów zawartości: aplikacji/vnd.ms-sstr xml, application/dash+xml,application/vnd.apple.mpegurl, aplikacji i f4m + xml. Możesz nie mogą włączać/wyłączać kompresji wymienionych typów za pomocą portalu Azure.  

## <a name="see-also"></a>Zobacz też
- [Rozwiązywanie problemów z sieci CDN kompresji plików](cdn-troubleshoot-compression.md)    
