## <a name="dynamic-service-plan"></a>Plan usług dynamicznych

Dynamiczne usługi plan funkcja aplikacji są przypisywane do wystąpienia aplikacji funkcji. Jeśli potrzebne kolejne wystąpienia zostaną dodane dynamicznie.
Tych wystąpień może obejmować u wielu zasobów komputerowych wprowadzeniu wszystkich możliwości dostępnej infrastruktury Azure. Ponadto funkcje będą równolegle zminimalizowaniu całkowity czas potrzebny do przetworzenia żądania. Czas wykonywania dla każdej funkcji zostanie dodany, w sekundach i agregowane przez aplikację zawierające funkcji. Z kosztów wg przez liczbę wystąpień, rozmiar pamięci i całkowity czas wykonania mierzony w sekundach gigabajt. Jeśli Twoje potrzeby obliczeń są przerw lub Twoje czasy zadania mogą być bardzo krótki jako umożliwia tylko zapłaty dla zasobów obliczeń, gdy są faktyczną używane jest doskonałym rozwiązaniem.   

### <a name="memory-tier"></a>Warstwa pamięci

W zależności od funkcja musi można wybrać ilość pamięci wymaganą do używania ich w aplikacji funkcji (kontener funkcji).
Opcje rozmiar pamięci różnią się od **128 MB 1536 MB**. Rozmiar pamięci zaznaczonego odpowiada zestawu roboczego wymagane przez wszystkich funkcji, które są częścią aplikacji funkcji. Jeśli kod wymaga więcej pamięci niż wybrany rozmiar, funkcja wystąpienia aplikacji zostanie zamknięty z powodu braku pamięci.

### <a name="scaling"></a>Skalowanie

Platformy Azure funkcje oceni ruch potrzeb, oparte na skonfigurowane wyzwalacze zdecydować, kiedy skalowanie w górę lub w dół. Stopień skalowania to aplikacja funkcji. W tym przypadku Skalowanie wewnętrzne oznacza dodawanie więcej wystąpień aplikacji dla funkcji. Odwrotnie przechodzi ruch w dół, funkcja wystąpień aplikacji są wyłączone-ostatecznie skalowania na zero gdy nie działają.  

### <a name="resource-consumption-and-billing"></a>Użycie zasobów i rozliczenia

W dynamicznej alokacji zasobów tryb odbywa się inaczej niż standardowy plan usług aplikacji, w związku z tym modelu zużycie różni się również, umożliwiającą modelu "płatności użycia". Zużycie jest informowany na aplikacji funkcji tylko raz, kiedy wykonać kod.  
Jest ona obliczana przez pomnożenie rozmiar pamięci (w GB) przez łączną liczbę czas wykonywania (w sekundach) dla wszystkich funkcji uruchomiony, aplikacja funkcji. Jednostki zużycie będzie **GB-s gigabajt (w sekundach)**.