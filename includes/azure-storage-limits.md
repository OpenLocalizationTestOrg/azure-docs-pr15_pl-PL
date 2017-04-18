Zasób|Domyślny Limit
---|---
Liczba kont miejsca do magazynowania dla subskrypcji|200<sup>1</sup>
TB na koncie miejsca do magazynowania|500 TB
Maksymalna liczba kontenerów obiektów blob, obiektów blob, udziały plików, tabele, kolejki, obiektów lub wiadomości na koncie miejsca do magazynowania|Tylko limit wynosi 500 TB pojemności konta
Maksymalny rozmiar pojedynczego obiektów blob kontenera, tabeli lub kolejki|500 TB
Maksymalna liczba bloków w blob blok lub dołączanie obiektów blob|50 000
Maksymalny rozmiar bloku w blob blok lub dołączanie obiektów blob|4 MB
Maksymalny rozmiar blob blok lub dołączanie obiektów blob|50 000 x 4 MB (około 195 GB) 
Maksymalny rozmiar blob strony |1 TB
Maksymalny rozmiar jednostki tabeli|1 MB
Maksymalna liczba właściwości w obiekcie tabeli|252
Maksymalny rozmiar wiadomości w kolejce|64 KB
Maksymalny rozmiar pliku w udziale plików|5 TB
Maksymalny rozmiar pliku w udziale plików|1 TB
Maksymalna liczba plików w udziale plików|Tylko limit wynosi pojemność 5 TB udziału pliku
Maksymalna liczba 8 KB operacji i/o na SEKUNDĘ na Udostępnij|1000.
Maksymalna liczba plików w udziale plików|Tylko limit wynosi pojemność 5 TB udziału pliku
Maksymalna liczba kontenerów obiektów blob, obiektów blob, udziały plików, tabele, kolejki, obiektów lub wiadomości na koncie miejsca do magazynowania|Tylko limit wynosi 500 TB pojemności konta
Maksymalna liczba zasady dostępu przechowywanych na kontenerze, udziału plików, tabeli lub kolejki|5
Suma żądanie szybkością (przy założeniu rozmiar obiektu 1KB) konta miejsca do magazynowania|Do 20 000 operacji i/o na SEKUNDĘ, podmioty na sekundę lub wiadomości na sekundę
Przepustowość docelowej dla pojedynczego obiektów blob|Do 60 MB na drugim lub maksymalnie 500 żądania na sekundę
Przepustowość docelowej dla pojedynczej kolejki (1 KB wiadomości)|Maksymalnie 2000 wiadomości na sekundę
Przepustowość docelowej dla jednej tabeli partition (1 KB jednostki)|Maksymalnie 2000 obiektów na sekundę
Przepustowość docelowy udział w jednym pliku|Do 60 MB na sekundę
MAX ingress<sup>2</sup> dla każdego konta miejsca do magazynowania (US regiony)|10 GB włączenie GRS-ZRS<sup>3</sup> 20 GB dla LRS
MAX wyjściowego<sup>2</sup> dla każdego konta miejsca do magazynowania (nam regiony)|20 GB w przypadku włączenia pomoc Zdalna-GRS-GRS-ZRS<sup>3</sup> , 30 GB dla LRS
Maksymalna liczba ingress<sup>2</sup> dla każdego konta miejsca do magazynowania (Europejskiej i Azji regionami)|5 GB w przypadku włączenia GRS-ZRS<sup>3</sup> , 10 GB dla LRS
Maksymalna liczba wyjściowego<sup>2</sup> dla każdego konta miejsca do magazynowania (Europejskiej i Azji regionami)|10 GB w przypadku włączenia pomoc Zdalna-GRS-GRS-ZRS<sup>3</sup> , 15 GB dla LRS

<sup>1</sup> Ta opcja uwzględnia zarówno Standard i Premium kont miejsca do magazynowania. Jeśli potrzebujesz więcej niż 200 kont miejsca do magazynowania, złożyć wniosek za pośrednictwem [Azure pomocy technicznej](https://azure.microsoft.com/support/faq/). Zespół magazyn Azure zostanie przejrzana dana sprawa firm i może zatwierdzić do 250 kont miejsca do magazynowania. 

<sup>2</sup> *Ingress* odwołuje się do wszystkich danych (liczba żądań) są wysyłane na konto miejsca do magazynowania. *Wyjściowego* odwołuje się do wszystkich danych (odpowiedzi) odbierane z konta miejsca do magazynowania.  

<sup>3</sup> Opcje replikacji usługi Azure miejsca do magazynowania, obejmują:

- **Pomoc Zdalna GRS**: magazynowania zbędne geo dostęp do odczytu. Po włączeniu GRS pomoc Zdalna wyjściowego elementów docelowych dla lokalizacji pomocniczej są identyczne Lokalizacja podstawowa.
- **GRS**: zbędne Geo miejsca do magazynowania. 
- **ZRS**: zbędne strefy miejsca do magazynowania. Dostępne tylko dla obiektów blob blok. 
- **LRS**: lokalnie zbędne miejsca do magazynowania. 

