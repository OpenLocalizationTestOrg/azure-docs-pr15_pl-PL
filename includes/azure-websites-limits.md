Zasób|Bezpłatne|Udostępnione (wersja Preview)|Podstawowe|Standardowe|Premium (wersja Preview)</th>
---|---|---|---|---|---
[Sieci web, telefon komórkowy lub aplikacje interfejsu API](https://azure.microsoft.com/services/app-service/) na [plan usług aplikacji](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)<sup>1</sup>|10|100|Nieograniczoną<sup>2</sup>|Nieograniczoną<sup>2</sup>|Nieograniczoną<sup>2</sup>
[Logika aplikacje](https://azure.microsoft.com/services/app-service/logic/) na [plan usług aplikacji](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)</a><sup>1</sup>|10|10|10|20 za podstawowe|20 za podstawowe
[Plan usług aplikacji](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)|1 w rozbiciu na regiony|10 na Grupa zasobów|100 na Grupa zasobów|100 na Grupa zasobów|100 na Grupa zasobów
Obliczanie Typ wystąpienia|Udostępnione|Udostępnione|Dedykowane<sup>3</sup>|Dedykowane<sup>3</sup>|Dedykowane<sup>3</sup></p>
[Skala w nowym oknie](../articles/app-service-web/web-sites-scale.md) (maksymalna liczba wystąpień)|1 udostępnionych|1 udostępnionych|3 dedykowane<sup>3</sup>|10 dedykowane<sup>3</sup>|20 dedykowane (50 w ASE)<sup>3.4</sup>
Miejsca do magazynowania<sup>5</sup>|1 GB<sup>5</sup>|1 GB<sup>5</sup>|10 GB<sup>5</sup>|50 GB<sup>5</sup>|500 GB<sup>4,5</sup></p>
Procesor czas (skrócona)<sup>6</sup>|3 minut|3 minut|Bez ograniczeń, płatności w standardowe [stawki](https://azure.microsoft.com/pricing/details/app-service/)</a>|Bez ograniczeń, płatności w standardowe stawki|Bez ograniczeń, płatności w standardowe stawki
Godzina (dzień) Procesora<sup>6</sup>|60 minut|240 minut|Bez ograniczeń, płatności w standardowe [stawki](https://azure.microsoft.com/pricing/details/app-service/)</a>|Bez ograniczeń, płatności w standardowe stawki|Bez ograniczeń, płatności w standardowe stawki
Pamięć (1 godzina)|1024 MB na plan usług aplikacji|1024 MB na aplikacji|N/D!|N/D!|N/D!
Przepustowości|165 MB|Bez ograniczeń, stosowanie [szybkość przesyłania danych](https://azure.microsoft.com/pricing/details/data-transfers/)|Bez ograniczeń, transfer danych, które stosować stawki|Bez ograniczeń, transfer danych, które stosować stawki|Bez ograniczeń, transfer danych, które stosować stawki
Architektura aplikacji|32-bitowa|32-bitowa|32-bitowej i 64-bitowa|32-bitowej i 64-bitowa|32-bitowej i 64-bitowa
Sockets sieci Web na wystąpienie<sup>7</sup>|5|35|350|Nieograniczoną|Nieograniczoną
Równoczesne [połączenia debugowania](../articles/app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md) na aplikacji|1|1|1|5|5
[Poddomena azurewebsites.NET z FTP/S i SSL](../articles/app-service-web/web-sites-configure-ssl-certificate.md)|X|X|X|X|X
Obsługa [domeny niestandardowe](../articles/app-service-web/web-sites-custom-domain-name.md)||X|X|X|X
Domeny niestandardowej [pomocy technicznej SSL](../articles/app-service-web/web-sites-configure-ssl-certificate.md)|||Nieograniczoną|Bez ograniczeń, 5 SNI SSL i 1 połączenia IP SSL dostępny|Bez ograniczeń, 5 SNI SSL i 1 połączenia IP SSL uwzględnione
Usługi równoważenia obciążenia zintegrowane||X|X|X|X
[Zawsze włączone](../articles/app-service-web/web-sites-configure.md)|||X|X|X
[Zaplanowane kopie zapasowe](../articles/app-service-web/web-sites-backup.md)||||Raz dziennie|Po co 5 minut<sup>8</sup>
[Automatyczne skalowanie](../articles/app-service-web/web-sites-scale.md)|||X|X|X
[WebJobs](../articles/app-service-web/web-sites-create-web-jobs.md) <sup>9</sup>|X|X|X|X|X
Obsługa [Harmonogramu Azure](https://azure.microsoft.com/services/scheduler/)||X|X|X|X
[Monitorowanie punktu końcowego](../articles/app-service-web/web-sites-monitor.md)|||X|X|X
[Organizowanie gniazda (wersja Preview)](../articles/app-service-web/web-sites-staged-publishing.md)||||5|20
Z domen niestandardowych dla aplikacji</a>||500|500|500|500
UMOWA DOTYCZĄCA POZIOMU USŁUG||<p>|99,9%|99,95%<sup>10</sup>|99,95%<sup>10</sup>

<sup>1</sup> Aplikacje i przydziałów magazynowania są na plan usług aplikacji, chyba że zaznaczono inaczej.  
<sup>2</sup> Rzeczywista liczba aplikacje obsługujące tych komputerów zależy od tego, działania aplikacje, rozmiar wystąpienia komputera i odpowiadające im wykorzystania zasobów.  
<sup>3</sup> Dedykowane wystąpienia może być o różnych rozmiarach. Aby uzyskać więcej informacji, zobacz [Cennik usługi aplikacji](https://azure.microsoft.com/pricing/details/data-transfers/pricing/details/app-service/) .  
<sup>4</sup> Poziom Premium umożliwia 50 oblicza wystąpienia (pod warunkiem dostępności) i 500 GB miejsca na dysku, podczas korzystania z aplikacji usługi środowiskach i 20 obliczeniowych w przeciwnym razie wystąpienia i 250 GB miejsca do magazynowania.  
<sup>5</sup> Limit magazynowania jest całkowity rozmiar zawartości w aplikacjach wszystkie tego samego planu aplikacji usługi. Więcej opcji przechowywania są dostępne w [Środowisku usługi aplikacji](../articles/app-service-web/app-service-web-configure-an-app-service-environment.md#storage)  
<sup>6</sup> Te zasoby są ograniczone przez wszystkie zasoby fizycznie w dedykowanej wystąpienia (rozmiar wystąpienia i liczbę wystąpień).  
<sup>7</sup> Skala aplikacji w warstwie podstawowe dwóch wystąpieniach, masz 350 połączenia równoczesne dla każdego z dwóch wystąpień.  
<sup>8</sup> Poziom Premium umożliwia interwał tworzenia kopii zapasowych w dół do co 5 minut, podczas korzystania z aplikacji usługi środowiskach i 50 godzin dziennie w inny sposób.  
<sup>9</sup> Uruchom niestandardowy pliki wykonywalne i/lub skryptów na żądanie, zgodnie z harmonogramem, lub przez cały czas jako zadania w tle w wystąpienia aplikacji usługi. Zawsze włączone wymagane jest wykonywania WebJobs ciągły. Azure harmonogram bezpłatne lub standardowe jest wymagane na potrzeby WebJobs według harmonogramu. Istnieje nie wstępnie zdefiniowanego ograniczenia liczby WebJobs, który może zostać uruchomiony w wystąpieniu usług aplikacji, ale praktyczne ograniczenia, zależne od kod aplikacji próbuje robić.   
<sup>10</sup> Umowa dotycząca poziomu usług dla wdrożenia korzystające z menedżerem ruch Azure wielu wystąpień % 99,95 skonfigurowany do przełączania awaryjnego.  
