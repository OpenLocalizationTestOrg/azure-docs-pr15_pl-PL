<properties 
    pageTitle="Azure RemoteApp - jak przepustowości sieci i jakość środowiska pracy razem? | Microsoft Azure"
    description="Dowiedz się, jak przepustowość sieci w Azure RemoteApp może mieć wpływ na jakość środowiska użytkownika."
    services="remoteapp"
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />

# <a name="azure-remoteapp---how-do-network-bandwidth-and-quality-of-experience-work-together"></a>Azure RemoteApp - jak przepustowość sieci i jakość środowiska pracy ze sobą?

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Gdy analizując [przepustowości sieci](remoteapp-bandwidth.md) wymagane dla Azure RemoteApp pamiętać o następujących czynników — są częścią dynamicznego systemu, który wpływa na ogólną środowiska użytkownika. 

- **Dostępna przepustowość sieci i bieżącej parametry sieci** — zestaw parametrów (straty, czas oczekiwania, funkcja) w tej samej sieci w danym momencie może mieć wpływ na aplikacji streaming środowisko, co oznacza obniżona ogólnego środowisko użytkownika. Przepustowości sieci jest to funkcja przeciążeń, losowe utratę opóźnienie, ponieważ tych parametrów wpływ przeciążeń mechanizmu kontrolki, które z kolei kontrolek szybkość transmisji, aby uniknąć konfliktów.  Na przykład stratna sieci lub sieci z długim czasem oczekiwania spowoduje zabezpieczenia użytkownika nieprawidłowych nawet w sieci z przepustowości 1000 MB. Utrata i opóźnienie zależą od liczby użytkowników znajdujących się w tej samej sieci i co robią tych użytkowników (na przykład oglądanie klipów wideo, pobieranie lub przekazywanie dużych plików, drukowania).
- **Scenariusz zastosowania** - środowisko zależy od tego, widok, co robią użytkowników, jako osoby i grupy w tej samej sieci. Na przykład jeden slajd do czytania wymaga tylko jedna klatka mają być aktualizowane; Jeśli użytkownik skims i przewija zawartości dokumentu tekst, muszą większa niż liczba ramek, które mają być aktualizowane na sekundę. Komunikacja i z powrotem na serwerze, w tym scenariuszu ostatecznie zajmie większej przepustowości sieci. Również wziąć pod uwagę przykład skrajnych: wielu użytkowników są oglądanie wideo o wysokiej rozdzielczości (na przykład rozdzielczość 4K), przytrzymując połączeń konferencyjnych HD, 3-w gry wideo lub działa w systemach CAD. Wszystkie te mogą być nawet sieci naprawdę szerokopasmowych praktycznie nie będzie można używać.
- **Rozdzielczość ekranu i liczba ekranów** - większą przepustowość sieci jest wymagane do pełnej aktualizacji ekranów większy niż mniejszych ekranach. Technologia wewnętrzna ma całkiem dobra zadania kodowanie i przekazywania tylko regiony ekranów, które zostały zaktualizowane, ale raz na jakiś czas całego ekranu wymaga aktualizacji. Jeśli użytkownik ma wyższa rozdzielczość ekranu (na przykład rozdzielczość 4K), tej aktualizacji wymaga większej przepustowości sieci niż ekran z niższej rozdzielczości (na przykład 1024x768px). Tej samej logiki ma zastosowanie, jeśli korzystasz z więcej niż jeden ekran do przekierowywania. Przepustowość muszą rosnąć wraz z liczbą ekranów.
- **Przekierowywanie Schowka i urządzenia** — jest to problem nie bardzo oczywiste, ale w większości przypadków Jeśli użytkownik zapisuje pobranie większego danych do Schowka, wystarczy nieco czasu te informacje przenieść od klienta pulpitu zdalnego na serwerze. Środowisko podrzędne można dotyczy środowiska od wysłania zawartości Schowka. To samo dotyczy przekierowywania — Jeśli skaner lub kamerę internetową daje wiele danych musi być wysyłana nadrzędny na serwerze lub poligraficznych do odbierania dużego dokumentu, lokalnego magazynu musi być dostępna do aplikacji działa w chmurze, aby skopiować dużych plików, użytkownicy mogą zauważyć porzucone ramki lub tymczasowo "zablokowany" plik wideo, ponieważ dane potrzebne do przekierowywania urządzenia jest zwiększenie przepustowości należy. 

Podczas oceny potrzeb przepustowości sieci, upewnij się, brać pod uwagę wszystkie z tych czynników działa zgodnie z systemem.

Teraz wróć do [artykułu przepustowości sieci głównego](remoteapp-bandwidth.md)lub przejść do testowania usługi [przepustowość sieci](remoteapp-bandwidthtests.md).

## <a name="learn-more"></a>Dowiedz się więcej
- [Szacowanie wykorzystania przepustowości sieci Azure RemoteApp](remoteapp-bandwidth.md)

- [Azure RemoteApp — testowanie do wykorzystania przepustowości sieci z kilka typowych scenariuszy](remoteapp-bandwidthtests.md)

- [Azure przepustowość sieci RemoteApp - ogólne wskazówki (Jeśli nie można przetestować własnych)](remoteapp-bandwidthguidelines.md)