W poniższej tabeli przedstawiono limity skojarzone z innej usługi poziomy (S1, S2, S3, F1). Aby uzyskać informacje o koszt każdej *jednostki* w każdej warstwie zobacz [IoT Centrum ceny](https://azure.microsoft.com/pricing/details/iot-hub/).

| Zasób | Standardowe s1 | Standardowe s2 | Standardowe S3 | Bezpłatne F1 |
| -------- | ----------- | ----------- | ----------- | ------- |
| Wiadomości dzień | 400 000 zł | 6,000,000   | 300,000,000 | 8000   |
| Maksymalna liczba jednostek | 200    | 200         | 200         | 1       |

> [AZURE.NOTE] Jeśli przewidujesz więcej niż 200 sztuk za pomocą Centrum warstwa S1 lub S2 lub S3, skontaktuj się z pomocy technicznej firmy Microsoft.

W poniższej tabeli przedstawiono ograniczenia, które dotyczą IoT Centrum zasobów:

| Zasób | Limit |
| -------- | ----- |
| Maksymalna opłaconej koncentratory IoT na subskrypcję Azure | 10 |
| Maksymalna liczba koncentratorów IoT bezpłatne na subskrypcję Azure | 1 |
| Maksymalna liczba tożsamości urządzenia<br/>  zwrócone w jednym połączenia | 1000. |
| Centrum IoT przechowywania maksymalna liczba wiadomości dla wiadomości urządzenia w chmurze | 7 dni |
| Maksymalny rozmiar wiadomości urządzenia w chmurze | 256 KB |
| Maksymalny rozmiar partii urządzenia w chmurze | 256 KB |
| Maksymalna liczba wiadomości w partii urządzenia w chmurze | 500 |
| Maksymalny rozmiar wiadomości w chmurze do urządzenia | 64 KB |
| Maksymalna liczba TTL dla wiadomości w chmurze do urządzenia | 2 dni |
| Liczba dostarczaniem maksymalny dla chmury do urządzenia <br/> wiadomości | 100 |
| Liczba maksymalna dostarczenia wiadomości opinii <br/> w odpowiedzi na wiadomość w chmurze do urządzenia | 100 |
| Maksymalna wartość TTL wiadomości opinii w <br/> odpowiedzi na wiadomość w chmurze do urządzenia | 2 dni |

> [AZURE.NOTE] Jeśli potrzebujesz więcej niż 10 koncentratory IoT płatnej subskrypcji Azure, skontaktuj się z pomocy technicznej firmy Microsoft.

Usługę Centrum IoT ogranicza żądania przekroczenia przydziałów następujące czynności:

| Ograniczenia | Wartość na Centrum |
| -------- | ------------- |
| Operacje rejestru tożsamości <br/> (Tworzenie, pobrać, listy, aktualizowanie i usuwanie), <br/> indywidualne lub zbiorczo Importuj/Eksportuj | 5000-min i jednostki (S3) <br/> 100-min i jednostka (S1 i S2). |
| Połączenia urządzenia | 6000-s jednostek (S3) 120-s/jednostka (S2) 12-s jednostek (S1). <br/>Co najmniej 100 na sekundę. |
| Wysyła urządzenia w chmurze | 6000-s jednostek (S3) 120-s/jednostka (S2) 12-s jednostek (S1). <br/>Co najmniej 100 na sekundę. |
| Wysyła chmury do urządzenia | 5000-min i jednostki (S3), 100-min i jednostka (S1 i S2). |
| Otrzymuje chmury do urządzenia | 50000-min i jednostki (S3) 1000-min i jednostka (S1 i S2). |
| Operacje przekazywania plików | 5000 przekazywanie plików powiadomienia min i jednostki (S3), 100 plików przekazywania powiadomienia min i jednostki (w przypadku S1 i S2). <br/> 10000 URI skojarzeń zabezpieczeń można się z konta magazynu platformy Azure w tym samym czasie.<br/> 10 skojarzeń zabezpieczeń identyfikatory URI urządzenia można się w tym samym czasie. |
