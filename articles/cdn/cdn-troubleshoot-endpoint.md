<properties
    pageTitle="Rozwiązywanie problemów z punktów końcowych Azure CDN zwracanie stanu 404 | Microsoft Azure"
    description="Rozwiązywanie problemów z 404 kody odpowiedzi z punktów końcowych Azure CDN."
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
    
# <a name="troubleshooting-cdn-endpoints-returning-404-statuses"></a>Rozwiązywanie problemów z punktów końcowych CDN zwracanie statusy 404

Ten artykuł ułatwia rozwiązywanie problemów z [punktów końcowych CDN](cdn-create-new-endpoint.md) zwracanie błędami 404.

Jeśli potrzebujesz dodatkowej pomocy w dowolnym momencie, w tym artykule, można skontaktować się Azure ekspertów na [MSDN Azure i fora przepełnienie stosu](https://azure.microsoft.com/support/forums/). Ponadto można również pliku Azure pomocy technicznej zdarzenia. Przejdź do [witryny pomocy technicznej Azure](https://azure.microsoft.com/support/options/) i kliknij pozycję **Uzyskiwanie pomocy technicznej**.

## <a name="symptom"></a>Symptom

Utworzono profil CDN i punktu końcowego, ale zawartości nie powiodła się będzie dostępny na CDN.  Użytkownicy, którzy będą próbować uzyskać dostęp do zawartości za pomocą adresu URL CDN otrzymywać kody stanu HTTP 404. 

## <a name="cause"></a>Przyczyna

Istnieje kilka możliwych przyczyn, w tym:

- Pochodzenie pliku nie jest widoczny dla sieci CDN
- Punkt końcowy są nieprawidłowo skonfigurowane, powoduje CDN do wyszukiwania w nieodpowiednich miejscach
- Host jest odrzucenie nagłówek hosta z sieci CDN
- Punkt końcowy nie ma czasu na propagowanie w całej sieci CDN

## <a name="troubleshooting-steps"></a>Procedura rozwiązywania problemów

> [AZURE.IMPORTANT] Po utworzeniu punkt końcowy CDN, nie natychmiast będzie dostępny do użytku, jak czas rejestracji propagowanie za pośrednictwem sieci CDN.  Profilów <b>CDN Azure z Akamai</b> propagowanie zwykle wykonuje w ciągu minuty.  Profilów <b>CDN Azure z Verizon</b> propagowanie zakończy się zazwyczaj w ciągu 90 minut, ale w niektórych przypadkach może trwać dłużej.  Wykonaj kroki opisane w tym dokumencie nadal jest wyświetlany 404 odpowiedzi, warto rozważyć Trwa oczekiwanie na kilka godzin, aby sprawdzić ponownie przed otwarciem bilet pomocy technicznej.

### <a name="check-the-origin-file"></a>Zaznacz plik pochodzenia

Najpierw weryfikacji, że plik, potrzebne jest włączony tryb buforowanej jest dostępna w naszym pochodzenia i są publicznie dostępne.  Najszybszym sposobem na tym jest Otwórz przeglądarkę sieci prywatnej w lub w sesji Incognito i przejść bezpośrednio do pliku.  Po prostu wpisz lub wklej adres URL w polu adres i zobacz, jeśli który wyników w pliku, który można się spodziewać.  W tym przykładzie mam zamiar użyć pliku jest na koncie magazyn Azure dostępne w `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`.  Jak widać, pomyślnie przekazuje test.

![Powodzenia!](./media/cdn-troubleshoot-endpoint/cdn-origin-file.png)

> [AZURE.WARNING] Chociaż jest najszybszym i Najprostszym sposobem upewnij się, że plik znajduje się publicznie, niektóre konfiguracje sieciowe w Twojej organizacji nadać wrażenie ten plik jest publicznie dostępne, gdy jest, w rzeczywistości widoczne tylko dla użytkowników sieci (nawet gdy jest ona hostowana Azure).  Jeśli masz zewnętrznej przeglądarce, z której można sprawdzić, takich jak urządzenia przenośnego, który nie jest podłączony do sieci swojej organizacji lub maszyny wirtualnej Azure, który będzie zalecane.

### <a name="check-the-origin-settings"></a>Sprawdź ustawienia pochodzenia

Teraz, gdy firma Microsoft została zweryfikowana, że plik znajduje się publicznie w Internecie, weryfikacji naszych ustawienia punktu początkowego.  W [Azure Portal](https://portal.azure.com)przejdź do swojego profilu CDN i kliknij punkt końcowy, z którym masz problem.  W wyniku karta **punktu końcowego** kliknij przycisk punktu początkowego.  

![Karta punktu końcowego wraz ze źródłem wyróżnione](./media/cdn-troubleshoot-endpoint/cdn-endpoint.png)

Zostanie wyświetlona karta **pochodzenia** . 

![Karta pochodzenia](./media/cdn-troubleshoot-endpoint/cdn-origin-settings.png)

#### <a name="origin-type-and-hostname"></a>Typ pochodzenia i nazwa hosta

Upewnij się, że **Typ pochodzenia** jest poprawny i sprawdź **Origin nazwa hosta**.  W naszym przykładzie `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`, hostname część adresu URL jest `cdndocdemo.blob.core.windows.net`.  Jak widać ekranu, to jest poprawny.  Magazyn Azure, aplikacji sieci Web i miejsc pochodzenia usługi w chmurze pole **Origin hostname** jest listy rozwijanej, więc nie trzeba pamiętać o poprawnie sprawdzania pisowni.  Jeśli korzystasz z niestandardowej pochodzenia, jest jednak *znaczenie krytyczne* które do nazwy hosta jest poprawna!

#### <a name="http-and-https-ports"></a>Porty HTTP i HTTPS

Inne elementu, aby sprawdzić, w tym miejscu jest usługi **HTTP** i **HTTPS porty**.  W większości przypadków 80 i 443 są poprawne i będą wymagane żadne zmiany.  Jednak jeśli serwer pochodzenia nasłuchują do innego portu, który będzie konieczne przedstawione w tym miejscu.  Jeśli nie masz pewności, Znajdź pod adresem URL pochodzenia pliku.  Specyfikacje HTTP i HTTPS można określić porty 80 i 443 jako domyślne. W polu Mój adres URL `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`, port nie zostanie określony, przyjmowana jest wartość domyślna 443 i ustawienia są poprawne.  

Jednak powiedzieć adres URL dla pliku pochodzenia, które przetestowane w starszym jest `http://www.contoso.com:8080/file.txt`.  Uwaga `:8080` na końcu części nazwa hosta.  Informuje przeglądarkę do używania portu, który `8080` nawiązywania połączenia z serwerem sieci web u `www.contoso.com`, więc musisz wprowadzić 8080 w polu **HTTP port** .  Należy pamiętać, że te ustawienia portu dotyczą tylko portu, jakie punkt końcowy używa w celu pobierania informacji z punktu początkowego.

> [AZURE.NOTE] Punkty końcowe **CDN Azure z Akamai** gamę port TCP dla miejsc pochodzenia nie jest możliwe.  Aby uzyskać listę portów pochodzenia, które nie są dozwolone zobacz [CDN Azure z Akamai dozwolone Origin porty](https://msdn.microsoft.com/library/mt757337.aspx).  
  
### <a name="check-the-endpoint-settings"></a>Sprawdź ustawienia punktu końcowego

Ponownie na karta **punktu końcowego** kliknij przycisk **Konfiguruj** .

![Karta punkt końcowy z wyróżnionym przyciskiem Konfiguruj](./media/cdn-troubleshoot-endpoint/cdn-endpoint-configure-button.png)

Zostanie wyświetlona karta **Konfiguruj** punkt końcowy.

![Konfigurowanie karta](./media/cdn-troubleshoot-endpoint/cdn-configure.png)

#### <a name="protocols"></a>Protokoły

**Protokoły**upewnij się, że jest zaznaczone dla protokołu używanego przez klientów.  Ten sam protokół używany przez klienta będzie używać do uzyskiwania dostępu do pochodzenia, dlatego istotne porty origin skonfigurowane poprawnie w poprzedniej sekcji.  Punkt końcowy wykrywa tylko domyślne HTTP i HTTPS porty 80 i 443, bez względu na to porty pochodzenia.

Załóżmy powrócić do naszym przykładzie teoretyczna z `http://www.contoso.com:8080/file.txt`.  Jak zapamiętasz, Contoso określone `8080` jako ich HTTP portu, ale również Załóżmy one określone `44300` jako ich port HTTPS.  Jeśli utworzonego punktu końcowego o nazwie `contoso`, będzie ich hostname punktu końcowego CDN `contoso.azureedge.net`.  Wniosek o `http://contoso.azureedge.net/file.txt` jest żądanie HTTP, więc punkt końcowy użyć protokołu HTTP na porcie 8080 na pobranie ich z punktu początkowego.  Żądanie bezpiecznego za pomocą protokołu HTTPS, `https://contoso.azureedge.net/file.txt`, może spowodować punkt końcowy użycie protokołu HTTPS na porcie 44300 po retriving pliku względem punktu początkowego.

#### <a name="origin-host-header"></a>Nagłówek hosta pochodzenia

**Nagłówek hosta Origin** jest wartość nagłówka hosta wysyłane do punktu początkowego przy każdym żądaniu.  W większości przypadków to powinna być taka sama **Origin hostname** udostępniamy zweryfikowana wcześniej.  Nieprawidłowa wartość w tym polu ogólnie nie będzie powodować statusy 404, ale może spowodować inne statusy 4xx, w zależności od punktu początkowego oczekuje.

#### <a name="origin-path"></a>Ścieżka pochodzenia

Ponadto weryfikacji naszych **Ścieżka pochodzenia**.  Domyślnie jest puste.  W tym polu należy używać tylko, jeśli chcesz zawęzić zakres hostowanej origin zasoby, które chcesz udostępnić w sieci CDN.  

Na przykład w Mój punkt końcowy miała wszystkie zasoby z mojego konta miejsca do magazynowania, aby był dostępny, więc mogę puste **ścieżki Origin** .  Oznacza to, że żądania w celu `https://cdndocdemo.azureedge.net/publicblob/lorem.txt` powoduje połączenie z moim punkt końcowy do `cdndocdemo.core.windows.net` które żąda `/publicblob/lorem.txt`.  Ponadto na żądanie `https://cdndocdemo.azureedge.net/donotcache/status.png` powoduje żąda punktu końcowego `/donotcache/status.png` względem punktu początkowego.

Ale co zrobić, jeśli nie chcesz używać CDN dla każdej ścieżki Moje ds?  Wypowiedz I tylko chce udostępnić `publicblob` ścieżki.  Jeśli */publicblob* wpisać w mojej pole **Origin ścieżkę** , który spowoduje, że punkt końcowy wstawić */publicblob* przed każdej wniosek pochodzenie.  Oznacza to, że wniosek o `https://cdndocdemo.azureedge.net/publicblob/lorem.txt` teraz zajmie faktycznie żądanie część adresu URL, `/publicblob/lorem.txt`i dołączanie `/publicblob` na początku. Powoduje żądanie `/publicblob/publicblob/lorem.txt` względem punktu początkowego.  Jeśli to nie rozwiąże do pliku, pochodzenie zwróci 404 stanu.  Faktycznie będzie prawidłowy adres URL do pobrania lorem.txt w tym przykładzie `https://cdndocdemo.azureedge.net/lorem.txt`.  Uwaga, że firma Microsoft nie zawierają ścieżkę */publicblob* , ponieważ żądanie część adresu URL jest `/lorem.txt` i dodaje punkt końcowy `/publicblob`, w wyniku `/publicblob/lorem.txt` żądania przesyłane do punktu początkowego.
