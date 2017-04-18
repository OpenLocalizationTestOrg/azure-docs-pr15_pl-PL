<properties
    pageTitle="Pociągnięcie danych publicznych w koncentratory zdarzenia Azure | Microsoft Azure"
    description="Omówienie koncentratory zdarzenia importowanie z próbki sieci web"
    services="event-hubs"
    documentationCenter="na"
    authors="spyrossak"
    manager="timlt"
    editor=""/>

<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/25/2016"
    ms.author="spyros;sethm" />

# <a name="pulling-public-data-into-azure-event-hubs"></a>Pociągnięcie danych publicznych w koncentratory zdarzenia Azure

Typowe scenariusze Internet czynności (IoT) masz urządzenia, które można programu do przekazania danych do Azure, albo do koncentratora zdarzenia Azure lub koncentratora IoT. Te koncentratory są punktów wejścia do Azure do przechowywania, analizowanie i wizualizowanie z większą liczbą narzędzia udostępniane na Microsoft Azure. Jednak obu wymagają push danych do nich w formacie JSON i zabezpieczeniami w określony sposób. Spowoduje to wyświetlenie następujące pytanie. Co można zrobić, jeśli chcesz, możesz zaimportować dane ze źródeł publiczny czy prywatny miejsce, w którym dane są wyświetlane jako kanał jakąś lub usługa sieci web, ale nie ma możliwości zmiany, jak opublikowane dane? Należy rozważyć, czy pogodzie lub ruchu lub giełdowych — nie wiadomo, NOAA, lub WSDOT lub tabeli NASDAQ, aby skonfigurować naciśnij, aby Twoim Centrum zdarzenia. Aby rozwiązać ten problem, zapisywane i otwórz-powierzając jej ich konserwację small chmury próbce, które można modyfikować i wdrażanie pobierać dane z niektórych źródło i przekazać je do Twoim Centrum zdarzeń. W tym miejscu możesz wykonać dowolne, temat, aby postanowienia licencyjne dotyczące od producenta. Aplikacja można znaleźć [tutaj](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-importfromweb/).

Należy zauważyć, że w tym przykładzie tylko kod sposobu wciąganie danych z sieci web Typowa źródła danych i pisanie do koncentratora zdarzenia Azure. NIE ma być aplikacją produkcji i prób nie wprowadzono był nadaje się do użytku w takim środowisku — jest strictfly DIY ograniczony Deweloper przykład tylko. Ponadto istnienia w tym przykładzie jest równoznaczne z rekomendacji, należy **pobrania** danych do Azure zamiast **wypychanych** go. Należy przejrzeć zabezpieczeń, wydajności, funkcja i czynników kosztowych przed rozpoczęciem na architekturze zakończenia do końca.

## <a name="application-structure"></a>Struktury aplikacji

Aplikacja jest zapisywany w języku C# i [Opis przykładu](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-importfromweb/) zawiera wszystkie informacje, które chcesz zmodyfikować, tworzenie i publikowanie aplikacji. Omówienie działanie aplikacji można znaleźć w poniższych sekcjach.

Możemy uruchomić przy założeniu, że masz dostęp do strumieniowego źródła danych. Można na przykład uwzględniał danych ruchu z Departament stanu Waszyngton transportu lub dane z NOAA, aby wyświetlić raporty niestandardowe lub łączenie danych z innych danych w aplikacji. Również musisz skonfigurowano koncentratora zdarzenia Azure i znasz połączenie ciągu wymaganego do nich dostęp.

Po uruchomieniu rozwiązanie GenericWebToEH, otrzymuje pliku konfiguracji (App.config), aby uzyskać liczba elementów:

1. Adres URL lub listę adresów URL, w witrynie publikowania danych. Najlepiej, jeśli jest to witryna publikującej danych w formacie JSON, takie jak kontakty odwołuje się WSDOT [tutaj](http://www.wsdot.wa.gov/Traffic/api/). 
2. Poświadczenia dla adresu URL, w razie potrzeby. Wiele źródła publiczne nie są potrzebne poświadczenia lub poświadczeń można umieścić w ciąg adresu URL. Inne wymagają podania oddzielnie. (Należy zauważyć, że można określić tylko jeden zestaw poświadczeń w tej aplikacji, więc będzie działać tylko jeśli użytkownik określi tylko jeden adres URL nie listę adresów URL).
3. Parametry połączenia i nazwa Centrum zdarzeń w tej nazw koncentratory zdarzenie, do którego będą push danych. Te informacje można znaleźć w portalu Azure.
4. Wstrzymania interwał (w milisekundach), dla interwału sondowania witryny danych publicznych. To ustawienie wymaga rozwagą. Jeśli możesz skorzystać zbyt rzadko, możesz pominąć danych; z drugiej strony możesz skorzystać zbyt częste, może zostać wyświetlony wiele powtarzających się danych, czy odpowiadającą jeszcze możesz mogła zostać zablokowana jako nefarious robot. Należy rozważyć, czy częstotliwość aktualizowania źródła danych — pogody lub ruch danych mogą być odświeżane co 15 minut, ale być może co kilka sekund, gdzie je znaleźć w zależności od kursy giełdowe. 
5. Flaga aplikacji stwierdzić, czy pochodzą dane formacie JSON lub XML. Ponieważ trzeba przekazać dane do koncentratora zdarzenia, aplikacja zawiera moduł, który chcesz przekonwertować XML JSON przed wysłaniem.

Po przeczytaniu pliku konfiguracji, aplikacja przechodzi do pętli — uzyskiwanie dostępu do publicznej witryny sieci web, konwertowanie danych, jeśli to konieczne, zapisywania go do usługi Centrum zdarzenia, a następnie Trwa oczekiwanie na interwał wstrzymania przed jego wykonanie nowa ponownie. W szczególności:

  * Czytanie publicznej witryny sieci Web. Do odbierania danych gotowy do wysłania wystąpienia klasy RawXMLWithHeaderToJsonReader jest używana z Azure/GenericWebToEH/ApiReaders/RawXMLWithHeaderToJsonReader.cs. Odczytuje strumień źródłowy w metodę GetData(), a następnie dzieli się mniejsze części (to znaczy rekordami) przy użyciu GetXmlFromOriginalText. 
  Ta metoda odczyta XML oraz poprawnego JSON lub JSON tablicy. Następnie przetwarzania jest uruchamiany przy użyciu konfiguracji MergeToXML z App.config (domyślny = puste).
  * Dane wysyłania i odbierania jest zaimplementowana jako pętli w metoda Process() plik Program.cs. 
  Po otrzymaniu wynik z GetData(), enqueues metody oddzielanymi przecinkami do koncentratora zdarzenia.

## <a name="next-steps"></a>Następne kroki

Aby wdrożyć rozwiązanie, klonowanie lub pobrać aplikację [GenericWebToEH](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-importfromweb/) , edytowania pliku App.config, jego tworzenia i na końcu opublikować go. Po opublikowaniu aplikacji, można to sprawdzić działa w portalu klasyczny Azure w obszarze usług w chmurze, a niektóre ustawienia konfiguracji (na przykład docelowej Centrum zdarzeń i interwał wstrzymania) można zmienić na karcie **Konfigurowanie** .

Zobacz więcej przykładów koncentratory zdarzenia w [galerii przykładów Azure](https://azure.microsoft.com/documentation/samples/?service=event-hubs) i w [witrynie MSDN](https://code.msdn.microsoft.com/site/search?query=event%20hubs&f%5B0%5D.Value=event%20hubs&f%5B0%5D.Type=SearchText&ac=5).
