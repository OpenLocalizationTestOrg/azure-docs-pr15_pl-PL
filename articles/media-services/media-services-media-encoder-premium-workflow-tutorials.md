<properties 
    pageTitle="Samouczki Avanced Media Encoder Premium w przepływu pracy" 
    description="Ten dokument zawiera instruktaży, które pokazują sposób wykonywania zaawansowanych zadań przepływu pracy programu Media Encoder Premium, a także jak utworzyć złożone przepływy pracy przy użyciu projektanta przepływów pracy." 
    services="media-services" 
    documentationCenter="" 
    authors="xstof" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016"  
    ms.author="xstof;xpouyat;juliako"/>

#<a name="advanced-media-encoder-premium-workflow-tutorials"></a>Zaawansowane samouczki Media Encoder Premium w przepływu pracy

##<a name="overview"></a>Omówienie 

Ten dokument zawiera instruktaży, aby pokazać, jak dostosować przepływach pracy przy użyciu **Projektanta przepływów pracy**. Pliki faktyczne można znaleźć [tutaj](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/PremiumEncoderWorkflowSamples).  

##<a name="toc"></a>SPIS TREŚCI

Omówione są następujące tematy:

- [Kodowanie MXF do pojedynczej szybkość transmisji bitów MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)
    - [Uruchamianie nowego przepływu pracy](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_start_new) 
    - [Za pomocą wejście pliku multimediów](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_file_input)
    - [Sprawdzanie strumienie multimediów](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_streams)
    - [Dodawanie wideo kodera dla. Generowanie pliku MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_file_generation)
    - [Kodowanie w strumieniu audio](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio)
    - [Multipleksowanie strumienie Audio i wideo do kontenera MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio_and_fideo)
    - [Zapisywanie pliku MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_writing_mp4)
    - [Tworzenie zawartości usługi multimediów z plik docelowy](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_asset_from_output)
    - [Testowanie ukończona przepływu pracy lokalnie](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_test)
- [Kodowanie MXF do multibitrate MP4s - dynamiczne opakowań włączone](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)
    - [Dodawanie jednego lub więcej dodatkowych wyjściowe MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_more_outputs)
    - [Konfigurowanie nazwy dane wyjściowe plików](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_conf_output_names)
    - [Dodawanie osobnych ścieżkę Audio](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_audio_tracks)
    - [Dodawanie. Plik SMIL ISM](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_ism_file)
- [Kodowanie MXF do multibitrate MP4 — plan rozszerzone](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4)
    - [Omówienie przepływu pracy w celu poprawienia](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_overview)
    - [Konwencje nazewnictwa plików](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_file_naming)
    - [Właściwości składnika publikacji na głównym przepływu pracy](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_publishing)
    - [Masz wygenerowany plik docelowy, który nazwy zależne od wartości właściwości opublikowanych](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_output_files)
- [Dodawanie miniatury do multibitrate wynik MP4](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4)
    - [Omówienie przepływu pracy, Dodawanie miniatury](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to_multibitrate_MP4_overview)
    - [Dodawanie kodowania JPG](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4__with_jpg)
    - [Sposób postępowania z konwersji przestrzeń kolorów](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_color_space)
    - [Pisanie miniatur](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_writing_thumbnails)
    - [Wykrywanie błędów w przepływie pracy](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_errors)
    - [Na koniec przepływu pracy](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_finish)
- [Przycinanie opartych na czasie produkcji multibitrate MP4](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim)
    - [Omówienie przepływu pracy, aby rozpocząć dodawanie skracania do](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_start)
    - [Za pomocą przycinarka strumienia](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_use_stream_trimmer)
    - [Na koniec przepływu pracy](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_finish)
- [Wprowadzenie do składnika skryptów](media-services-media-encoder-premium-workflow-tutorials.md#scripting)
    - [Wykonywanie skryptów w ramach przepływu pracy: Witaj świecie](media-services-media-encoder-premium-workflow-tutorials.md#scripting_hello_world)
- [Na podstawie ramki przycinania produkcji multibitrate MP4](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim)
    - [Przegląd plan, aby zacząć dodawać skracania do](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_start)
    - [Za pomocą listy wycinków XML](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clip_list)
    - [Modyfikowanie listy wycinków ze składnika ładowanie](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_modify_clip_list)
    - [Dodawanie właściwości wygody ClippingEnabled](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clippingenabled_prop)

##<a id="MXF_to_MP4"></a>Kodowanie MXF do pojedynczej szybkość transmisji bitów MP4
 
W tym instruktażu utworzymy pojedynczy szybkość transmisji bitów. Zakodowany plik MP4 z AAC-HE audio z. Plik wejściowy MXF. 

###<a id="MXF_to_MP4_start_new"></a>Uruchamianie nowego przepływu pracy 

Otwórz projektanta przepływów pracy i wybierz pozycję "Plan kod transakcji plik"-"nowy obszar roboczy"-"" 

Nowy przepływ pracy zostanie wyświetlona 3 elementy: 

- Plik źródłowy podstawowego
- Lista XML klipów
- Dane wyjściowe pliku i zawartości  

![Nowy przepływ pracy kodowania](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-transcode-blueprint.png)

*Nowy przepływ pracy kodowania*

###<a id="MXF_to_MP4_with_file_input"></a>Za pomocą wejście pliku multimediów

Aby zaakceptować naszą plik multimedialny wprowadzania, jedną zaczyna się dodawania multimediów plik wprowadzania składnika. Aby dodać składnik do przepływu pracy, poszukaj go w polu wyszukiwania repozytorium i przeciągnij żądaną pozycję na okienka projektanta. Powtórz tę czynność dla wprowadzania plik multimediów i Połącz składnik podstawowy plik źródłowy do wprowadzania numeru pin Filename z wprowadzania plik multimediów.

![Plików multimedialnych połączonych danych wejściowych](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-input.png)

*Plików multimedialnych połączonych danych wejściowych*

Przed możemy wiele więcej możemy najpierw należy wskazać Projektant przepływów pracy, jakie przykładowy plik chcemy służy do projektowania naszych przepływu pracy z. Aby to zrobić, kliknij przycisk Tło okienku projektanta i sprawdź właściwość podstawowy plik źródłowy w okienku właściwości po prawej stronie. Kliknij ikonę folderu i wybierz żądany plik do testowania przepływu pracy z. Jak to zrobić, składnik wejście multimediów sprawdzenie pliku i wypełnić PIN jej wynik, aby odzwierciedlała plik, który go inspekcji.

![Plików multimedialnych wypełnione danych wejściowych](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-populated-media-file-input.png)

*Plików multimedialnych wypełnione danych wejściowych*

Gdy to ustawienie określa z jakich wprowadzania chcemy do pracy z jej nie sprawdzić jeszcze miejsce, w którym zakodowany wynik powinna trafić do. Podobnie jak podstawowy plik źródłowy został skonfigurowany, teraz skonfigurować właściwości dane wyjściowe folderu zmiennych, tuż poniżej.

![Skonfigurowane właściwości dane wejściowe i wyjściowe](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configured-io-properties.png)

*Skonfigurowane właściwości dane wejściowe i wyjściowe*

###<a id="MXF_to_MP4_streams"></a>Sprawdzanie strumienie multimediów

Często występują żądane wiedzieć, jak strumienia wygląda tak jak przepływów przepływie pracy. Aby sprawdzić strumienia w dowolnym momencie w ramach przepływu pracy, wystarczy kliknąć produkcji lub wprowadzania numeru pin w dowolnej części. W tym przypadku próbuj, klikając pin wyjściowy nieskompresowane klip wideo z naszych wejście multimediów. Okno dialogowe otworzą umożliwiająca sprawdzanie ruchu wychodzącego wideo.

![Sprawdzanie pin wyjściowy nieskompresowane wideo](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-inspecting-uncompressed-video-output.png)

*Sprawdzanie pin wyjściowy nieskompresowane wideo*

W naszym przypadku informuje nam na przykład czy mamy już zajmujących się wprowadzanie 1920 x 1080 na 24 ramek na sekundę w 4:2:2 przy próbkowaniu klipu wideo niemal 2 minuty.

###<a id="MXF_to_MP4_file_generation"></a>Dodawanie wideo kodera dla. Generowanie pliku MP4

Wiele pin wyjściowy nieskompresowane Audio i wideo nieskompresowane są dostępne do użycia w naszym wejście multimediów, należy pamiętać, że teraz. Aby kodowania ruchu przychodzącego wideo, potrzebujemy składnik kodowania — w tym przypadku do wygenerowania. Pliki mp4.

Do kodowania strumień wideo, aby H.264, Dodaj składnik AVC Encoder wideo na powierzchnię projektanta. Ten składnik pobiera Dekompresuj strumienia wideo jako danych wejściowych i zapewnia AVC skompresowany strumienia wideo na jej wynik numeru pin.

![Niepołączone Encoder AVC](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-avc-encoder.png)

*Niepołączone Encoder AVC*

Właściwości Określ, jak kodowanie dokładnie tak się dzieje. Przyjrzyjmy się temu bliżej na kilka ważnych ustawień:

- Dane wyjściowe szerokość i wysokość wyjściowego: te określenie rozdzielczości zakodowany plik wideo. W naszym przypadku Przyjrzyjmy się z 640 x 360
- Szybkość odtwarzania: ustawiona na przekazywanie tylko przyjmie szybkość odtwarzania źródła, jest możliwe jednak zastąpić. Należy zauważyć, że takie konwersji framerate nie ruchu wyrównywane jest.
- Profil i poziomu: te określają profilu AVC i poziom. Aby wygodnie uzyskać więcej informacji o różnych poziomach i profile, kliknij ikonę znaku zapytania na składniku AVC Video Encoder i na stronie Pomoc zostanie wyświetlona szczegółowe informacje o wszystkich poziomów. Dla naszych próbki Przyjrzyjmy się z głównego profilu na poziomie 3,2 (ustawienie domyślne).
- Ocenianie sterowania trybem i szybkość transmisji bitów (KB/s): w naszym scenariuszu wyboru możemy stałą szybkość transmisji bitów (CBR) wyjściowy na 1200 kb/s
- Format wideo: jest to o VUI (informacje związane z użytecznością wideo), które elementy są zapisywane w strumieniu H.264 (strony informacje, które mogą być używane przez dekoder można rozszerzyć wyświetlanie, ale nie są niezbędne do poprawnie dekodowanie):
- NTSC (typowy dla Stanów Zjednoczonych i Japonii, za pomocą 30 k/s)
- PAL (zazwyczaj Europe przy użyciu 25 k/s)
- Tryb rozmiaru GOP: Firma Microsoft będzie skonfigurować stały rozmiar GOP z naszego punktu widzenia przy użyciu klucza interwał dwie sekundy z GOPs zamknięty. Dzięki temu zapewnia zgodność z usługi multimediów Azure opakowań dynamiczne.

Kanał naszych encoder AVC, nawiązać pin wyjściowy nieskompresowane wideo ze składnika multimediów plik wprowadzania nieskompresowane wideo numeru pin z kodera AVC.

![Połączone Encoder AVC](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-avc-encoder.png)

*Połączone encoder główne AVC*

###<a id="MXF_to_MP4_audio"></a>Kodowanie w strumieniu audio

W tym momencie możemy mieć zakodowany klip wideo, ale oryginalny nieskompresowane strumieniu audio nadal musi być kompresowany. W tym zostały one opisane AAC kodowania przez składnik Encoder AAC (Dolby). Dodaj go do przepływu pracy.

![Niepołączone Encoder AVC](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-aac-encoder.png)

*Niepołączone encoder AAC*

Teraz niezgodności: podczas wprowadzania plik multimediów więcej niż prawdopodobnie ma dwa różne nieskompresowane strumieniu audio dostępne jest tylko jednym nieskompresowane audio wprowadzania numeru pin z kodera AAC: jedną dla lewego kanału audio i jeden dla po prawej stronie. (Jeśli masz zajmujących się otaczanie dźwięk, który jest kanałów 6). Dlatego nie jest możliwe bezpośrednio połączyć dźwięk ze źródła danych wejściowych plik multimediów do kodera audio AAC. Składnik AAC oczekuje tak zwany "przeplotem" strumieniu audio: pojedynczy strumień, który ma po lewej i prawej kanałów interleaved ze sobą. Po znamy z naszych plik multimedialny źródła audio utwory są, w jakiej pozycji w źródle możemy wygenerować takie przeplotem strumieniu audio z pozycji prelegenta poprawnie przypisane do lewego i prawego.

Najpierw jedną chcesz wynikiem strumień przeplotem kanałów audio wymagane źródło. Składnik Interleaver strumieniu Audio będzie obsługiwany to dla nas. Dodaj go do przepływu pracy i połącz wyjściowe audio z wejście pliku multimediów do niego.

![Połączenie Interleaver strumieniu Audio](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-audio-stream-interleaver.png)

*Połączenie Interleaver strumieniu Audio*

Teraz gdy mamy już przeplotem strumieniu audio, możemy nadal nie określ miejsce pozycji prelegenta w lewo lub w prawo, które chcesz przypisać. Aby określić to, możemy wykorzystać Assigner położenie prelegenta.

![Dodawanie Assigner położenie prelegenta](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-speaker-position-assigner.png)

*Dodawanie Assigner położenie prelegenta*

Konfigurowanie Assigner położenie prelegenta do użytku z stereo strumień wejściowy przez filtr kodera wstępnie ustawione "Niestandardowe" i ustawienia kanału o nazwie "2.0 (L, R)". (Spowoduje to przypisanie pozycji po lewej stronie prelegenta do kanału 1 i położenie prawego głośnika do kanału 2.)

Połącz dane wyjściowe Assigner położenie prelegenta danych wejściowych AAC Encoder. Następnie należy wskazać Encoder AAC do pracy z "2.0 (L, R)" zdefiniowane kanału, tak aby wiedzieli, na zajęcie się dźwięku stereo jako danych wejściowych.

###<a id="MXF_to_MP4_audio_and_fideo"></a>Multipleksowanie strumienie Audio i wideo do kontenera MP4

Podane naszych AVC zakodowany strumienia wideo i naszych AAC zakodowany strumieniu audio, firma Microsoft może certyfikatem do przechwytywania. Kontener MP4. Proces mieszania różnych strumieni w jedno jest nazywany "Multipleksowanie" (lub "muxing"). W tym przypadku możemy korzystasz z przeplotem audio i strumienie wideo w jednym spójne. Pakiet MP4. Składnik, który współrzędne ten formularz. Kontener MP4 jest nazywany multiplekser ISO MPEG-4. Dodać je do projektanta powierzchni i połącz zarówno AVC Video Encoder i AAC Encoder jego danych wejściowych.

![Połączone MPEG4 multiplekser](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-mpeg4-multiplexer.png)

*Połączone MPEG4 multiplekser*

###<a id="MXF_to_MP4_writing_mp4"></a>Zapisywanie pliku MP4

Podczas zapisywania pliku wyjściowego, składnik danych wyjściowych w pliku jest używany. Firma Microsoft można nawiązać to wynik multiplekser ISO MPEG-4, aby jej wyniki elementy są zapisywane na dysku. W tym celu należy połączyć pin wyjściowy kontenera (MPEG-4) do wprowadzania numeru pin zapisu pliku danych wyjściowych.

![Połączenie danych wyjściowych w pliku](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-file-output.png)

*Połączenie danych wyjściowych w pliku*

Nazwa pliku, który będzie używany jest określona przez właściwości pliku. Tej właściwości mogą być ustalony do określonej wartości, najbardziej prawdopodobne będzie chcesz ustawić go za pomocą wyrażenia.

Przepływu pracy automatycznie określić dane wyjściowe plików właściwość nazwisko z wyrażenia, kliknij przycisku obok nazwy pliku (obok ikony folderu). Z menu rozwijanego, a następnie wybierz "Wyrażenie". To powoduje wyświetlenie Edytor wyrażeń. Najpierw wyczyść zawartość edytora.

![Pusty Edytor wyrażeń](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-empty-expression-editor.png)

*Pusty Edytor wyrażeń*

Edytor wyrażeń umożliwia wprowadzanie literał, razem z jedną lub więcej zmiennych. Zmienne rozpoczyna się znakiem dolara. Jak naciśnięcie klawisza $ edytorze pojawi się pole listy rozwijanej z wybranej dostępnych zmiennych. W naszym przypadku użyjemy kombinację zmienną wyjściową katalogu i Zmienna nazwy podstawowej wprowadzania pliku:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}.MP4

![Wypełniony się Edytor wyrażeń](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-expression-editor.png)

*Wypełniony się Edytor wyrażeń*

>[AZURE.NOTE]Aby zobaczyć zobacz plik docelowy kodowania zadania w Azure, należy podać wartość w edytorze wyrażeń. 

Jeśli wyrażenie jest potwierdzić przez naciśnięcie ok, okno właściwości preview jest skojarzona z właściwości pliku co wartości w tym momencie.

![Wyrażenie plik jest rozpoznawana jako wynik dir](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-expression-resolves-output-dir.png)

*Wyrażenie plik jest rozpoznawana jako wynik dir*

###<a id="MXF_to_MP4_asset_from_output"></a>Tworzenie zawartości usługi multimediów z plik docelowy

Podczas możemy mieć zapisywania pliku wyjściowego MP4, nadal należy wskazać, że ten plik należy do zasobów dane wyjściowe, co spowoduje usługi multimediów w wyniku wykonywania tego przepływu pracy. W tym celu jest używany węzeł dane wyjściowe pliku-zawartości w obszarze roboczym przepływu pracy. Wszystkie pliki przychodzące do tego węzła spowoduje, że część wyniku trwałego usługi multimediów Azure.

Połącz składnik danych wyjściowych w pliku do składnika aktywów pliku wyjściowego do zakończenia przepływu pracy.

![Na koniec przepływu pracy](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow.png)

*Na koniec przepływu pracy*

###<a id="MXF_to_MP4_test"></a>Testowanie ukończona przepływu pracy lokalnie

Aby przetestować lokalnie przepływu pracy, kliknij przycisk Odtwórz na pasku narzędzi u góry. Po zakończeniu wykonywania przepływu pracy inspekcja wyniki generowane w folderze wyjściowym skonfigurowane. Pojawi się na koniec pliku wyjściowego MP4, który został zakodowany z MXF pliku źródła danych wejściowych.

##<a id="MXF_to_MP4_with_dyn_packaging"></a>Kodowanie MXF do MP4 - multibitrate włączone dynamiczne opakowań

W tym instruktażu utworzymy zestawu wielu plików MP4 szybkość transmisji bitów z AAC zakodowany dźwięk z jednym. Plik wejściowy MXF.

Po wyjściowe trwałego szybkość transmisji bitów wielokrotne jest żądane do użycia w połączeniu z funkcjami dynamiczne opakowań oferowanych przez usługi multimediów Azure, wielu plików MP4 wyrównany do lewej GOP poszczególnych, które różnych szybkość transmisji bitów i rozdzielczość mają być do wygenerowania. W tym celu Instruktaż [Kodowanie MXF do pojedynczej szybkość transmisji bitów MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4) zapewnia dobry punkt wyjścia.

![Uruchamianie przepływu pracy](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow.png)

*Uruchamianie przepływu pracy*

###<a id="MXF_to_MP4_with_dyn_packaging_more_outputs"></a>Dodawanie jednego lub więcej dodatkowych wyjściowe MP4

Każdy plik MP4 w naszym wyniku trwałego usługi multimediów Azure będzie obsługiwać różne szybkości transmisji bitów i rozdzielczość. Możesz dodać jeden lub więcej plików wyjściowych MP4 z przepływem pracy.

Aby upewnić się, że mamy naszych kodery wideo utworzone za pomocą tych samych ustawień, jest najwygodniejsze duplikujące się już istniejące AVC Encoder wideo i skonfiguruj innej kombinacji rozdzielczość i szybkość transmisji bitów (Dodawanie jedną 960 x 540 w 25 ramek na sekundę 2,5 MB/s). Aby zduplikować istniejących encoder, Kopiuj go wkleić na powierzchni projektanta.

Podłącz pin wyjściowy nieskompresowane klip wideo: wejście pliku multimediów do naszych nowy składnik AVC.

![Druga encoder AVC połączenia](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-avc-encoder-connected.png)

*Druga encoder AVC połączenia*

Teraz dopasować konfiguracja dla naszych nowych encoder AVC celu wyprowadzenia 960 x 540 2,5 MB/s. (Użyj jego właściwości "szerokości wyjściowego", "Wysokość wynik" i "Szybkość transmisji bitów (KB/s)" to).

Podane chcemy wyniku trwałego razem z opakowania dynamiczne usługi multimediów Azure za pomocą, przesyłanie strumieniowe punktu końcowego musi mieć możliwość tworzenia z tych plików MP4 fragmenty HLS-Fragmented MP4-KRESKI, dokładnie wyrównane do siebie w sposób, że klienci, którzy są przełączanie różnych bitrates zapoznać się z jednym wygładzonymi ciągły klip wideo i obsługi audio. Aby to zrobić, trzeba upewnij się, że we właściwościach obu kodery AVC GOP ("grupa obrazów") rozmiar obu plików MP4 jest ustawiona na 2 sekundy, które można wykonać:

- Ustawianie trybu rozmiar GOP rozmiarowi stały GOP i
- Interwał ramki klucz do dwie sekundy.
- również ustawić formant IDR GOP zamknięty GOP, aby upewnić się, wszystkie GOP są stojących na własne bez zależności

Aby uprościć naszych przepływu pracy do zrozumienia, Zmień nazwę pierwszej encoder AVC do "AVC Video Encoder 640 x 360 1200 kb/s" i drugi encoder AVC "AVC Video Encoder 960 x 540 2500 KB/s".

Teraz Dodaj drugi multiplekser ISO MPEG-4 i drugi danych wyjściowych w pliku. Podłącz multiplekser do nowego kodera AVC i upewnij się, że jego dane są kierowane do pliku danych wyjściowych. Następnie nawiązywania połączenia danych wejściowych AAC kodera dźwięku wynik do nowego multiplekser firmy. Plik wyjściowy z kolei następnie można łączyć do węzła dane wyjściowe do pliku aktywów ją dodać do usług multimedialnej, która zostanie utworzona.

![Druga Muxer i połączenia danych wyjściowych w pliku](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-muxer-file-output-connected.png)

*Druga Muxer i połączenia danych wyjściowych w pliku*

Pod kątem zgodności z opakowania dynamiczne usługi multimediów Azure skonfiguruj multiplekser trybu fragmentu liczby GOP lub czasu trwania oraz ustaw GOPs na fragmencie 1. (Musi to być domyślnie).

![Tryby fragmentu Muxer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-muxer-chunk-modes.png)

*Tryby fragmentu Muxer*

Uwaga: można powtórzyć te czynności dla innymi szybkość transmisji bitów i rozdzielczość kombinacjami, które ma zostać dodane do wyników zawartości.

###<a id="MXF_to_MP4_with_dyn_packaging_conf_output_names"></a>Konfigurowanie nazwy dane wyjściowe plików

Zawiera więcej niż jeden Jednoplikowa dodane do elementu danych wyjściowych. Dzięki temu potrzeba upewnij się, że nazwy plików dla każdego z plików wyjściowych różnią się od siebie i może nawet stosowanie konwencji nazewnictwa plików, aby okazuje się od nazwy pliku co to jest praca z.

Nazywanie dane wyjściowe pliku można kontrolować za pomocą wyrażeń w projektancie. Otwieranie okienka właściwości dla jednej części danych wyjściowych w pliku i Otwórz Edytor wyrażeń właściwości pliku. Nasze pierwszy plik docelowy został skonfigurowany przez następujące wyrażenie (zobacz samouczek przechodzenia z [MXF do pojedynczej szybkość transmisji bitów wyjściowe MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)):

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}.MP4

Oznacza to, że nasze filename zależy od dwóch zmiennych: katalog wyjściowy do pisania i nazwa podstawowa pliku źródłowego. Pierwsza są wyświetlane jako właściwości w katalogu głównym przepływu pracy i jego zależy od importowanego pliku. Należy zauważyć, że katalog wyjściowy używać do testowania lokalne; Ta właściwość zostaną zastąpione przez aparat przepływu pracy, gdy przepływ pracy jest wykonywane przez procesor opartej na chmurze multimediów w usługi multimediów Azure.
Aby nadać nasze pliki wyjściowe nazewnictwa spójne dane wyjściowe, zmienić pierwszego pliku nazewnictwa wyrażenia:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

a drugi do:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_960x540_2.MP4

Wykonywanie testu pośrednie Uruchom, aby się upewnić, że oba pliki wyjściowe MP4 prawidłowo są generowane.

###<a id="MXF_to_MP4_with_dyn_packaging_audio_tracks"></a>Dodawanie osobnych ścieżkę Audio

Jak poznamy w później, gdy mamy generuje plik .ism, aby przejść z naszych pliki wyjściowe MP4, firma Microsoft wymaga także pliku MP4 tylko audio jako ścieżki dźwiękowej naszych adaptacyjne przesyłania strumieniowego. Aby utworzyć ten plik, Dodaj dodatkowe muxer do przepływu pracy (multiplekser ISO — MPEG-4) i połącz pin wyjściowy AAC encoder za jego wprowadzania numeru pin dla śledzenia 1.

![Audio Muxer dodane](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-added.png)

*Audio Muxer dodane*

Tworzenie innego składnika danych wyjściowych w pliku wyjściowy strumienia ruchu wychodzącego z muxer i skonfiguruj nazw wyrażeń jako plików:
    
    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_128kbps_audio.MP4

![Audio Muxer tworzenia danych wyjściowych w pliku](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-creating-file-output.png)

*Audio Muxer tworzenia danych wyjściowych w pliku*

###<a id="MXF_to_MP4_with_dyn_packaging_ism_file"></a>Dodawanie. Plik SMIL ISM

Dynamiczne opakowania do pracy w połączeniu z zarówno MP4 pliki (i MP4 tylko audio) w naszym trwałego usługi multimediów, potrzebujemy również pliku manifestu (zwanych również pliku "SMIL": synchronizowane Multimedia integracji języka). Ten plik wskazuje usługi multimediów Azure, jakie pliki MP4 są dostępne dla dynamicznego opakowań i które z tych, które należy wziąć pod uwagę audio strumień. Typowe pliku manifestu zestawu MP4 osoby z jednym strumieniu audio wygląda następująco:
    
    <?xml version="1.0" encoding="utf-8" standalone="yes"?>
    <smil xmlns="http://www.w3.org/2001/SMIL20/Language">
      <head>
        <meta name="formats" content="mp4" />
      </head>
      <body>
        <switch>
          <video src="H264_1900kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_1300kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_900kbps_AAC_und_ch2_96kbps.mp4" />
          <audio src="AAC_ch2_96kbps.mp4" title="AAC_und_ch2_96kbps" />
        </switch>
      </body>
    </smil>

Plik .ism zawiera w instrukcji przełącznika, odwołanie do każdego z poszczególnych plików wideo MP4 i oprócz te również jednej (lub więcej) odwołania plik dźwiękowy MP4 zawiera tylko dźwięku.

Generowanie pliku manifestu naszych zestawu MP4 osoby można zrobić za pomocą elementu o nazwie "AMS pojawiają Writer". Używać tej funkcji, przeciągnij go na powierzchnię i połącz "Pisanie ukończono" Sworznie dane wyjściowe z trzech składników danych wyjściowych w pliku danych wejściowych AMS pojawiają Writer. Następnie upewnij się połączyć dane wyjściowe Writer pojawiają AMS do pliku danych wyjściowych i zawartości.

Podobnie jak w przypadku naszych innych plików dane wyjściowe składników, skonfiguruj nazwę .ism pliku wynik wyrażenia:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_manifest.ism

Nasze zakończeniu przepływu pracy wygląda poniżej:

![Na koniec MXF multibitrate MP4 z przepływem pracy](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-mxf-to-multibitrate-mp4-workflow.png)

*Na koniec MXF multibitrate MP4 z przepływem pracy*

##<a id="MXF_to__multibitrate_MP4"></a>Kodowanie MXF do multibitrate MP4 — plan rozszerzone

W [poprzedniej Instruktaż przepływu pracy](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging) firma Microsoft zobaczył, jak pojedynczy środek wprowadzania MXF mogą być konwertowane na środka trwałego dane wyjściowe z szybkość transmisji bitów wielu plików MP4, pliku MP4 tylko audio i manifestu pliku w celu użycia w połączeniu z opakowania dynamiczne usługi multimediów Azure.

W tym instruktażu procedurach pokazano, jak niektórych aspektów mogą być rozszerzone, a wprowadzone wygodniejsze.

###<a id="MXF_to_multibitrate_MP4_overview"></a>Omówienie przepływu pracy w celu poprawienia

![Multibitrate MP4 przepływu pracy w celu poprawienia](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-enhance.png)

*Multibitrate MP4 przepływu pracy w celu poprawienia*

###<a id="MXF_to__multibitrate_MP4_file_naming"></a>Konwencje nazewnictwa plików

Poprzedni przepływ pracy firma Microsoft określonego proste wyrażenie jako podstawy generowania nazw plików danych wyjściowych. Mimo że mamy kilka duplikatów: wszystkie składniki pliku poszczególnych dane wyjściowe określone takie wyrażenie.

Na przykład składnik dane wyjściowe naszych file pierwszego pliku wideo skonfigurowano z tego wyrażenia:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

Podczas drugiego wyjścia wideo zostały wyrażeń, takich jak:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_960x540_2.MP4

Nie można uporządkować mniej błędów podatne i wygodniejszy, jeśli można usunąć niektóre z tym duplikowanie i prośbą więcej można konfigurować zamiast tego? Szczęście możemy: projektanta możliwości wyrażenia w połączeniu z możliwością Tworzenie właściwości niestandardowych na poziomie głównym naszym przepływu pracy dadzą nam dodatkowej warstwy wygody.

Załóżmy, że firma Microsoft będzie sterują konfiguracji filename z bitrates pojedynczych plików MP4. Te bitrates, które będzie możemy mają na celu skonfigurować w jednym miejscu centralnej (w katalogu głównym naszym wykresu), z którym będzie są dostępne do konfigurowania i generowania nazw plików dysk. Aby to zrobić, możemy uruchomić przez opublikowanie właściwości szybkość transmisji bitów z obu kodery AVC do głównego naszych przepływ pracy, tak, aby stał się dostępny zarówno na poziomie głównym także od kodery AVC. (Nawet jeśli wyświetlane w dwóch różnych miejscach, jest tylko jednej wartości źródłowych).

###<a id="MXF_to__multibitrate_MP4_publishing"></a>Właściwości składnika publikacji na głównym przepływu pracy

Otwórz pierwszy encoder AVC, przejdź do właściwości szybkość transmisji bitów (KB/s) i z menu rozwijanego wybierz pozycję Publikuj.

![Publikowanie właściwości szybkość transmisji bitów](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-bitrate-property.png)

*Publikowanie właściwości szybkość transmisji bitów*

Konfigurowanie okna dialogowego Publikuj Publikuj w katalogu głównym naszym wykresu przepływu pracy z opublikowana nazwa "video1bitrate" i czytelne wyświetlaną nazwę "1 w klipie wideo szybkość transmisji bitów". Skonfiguruj niestandardową nazwę grupy o nazwie "Streaming Bitrates" i naciśnij Publikuj.

![Publikowanie właściwości szybkość transmisji bitów](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-bitrate-property.png)

*Okno dialogowe właściwości szybkość transmisji bitów publikowania*

Powtórzenie tej samej właściwości szybkość transmisji bitów drugiego kodera AVC i nadaj mu nazwę "video2bitrate" z nazwę wyświetlaną "Klip wideo 2 szybkość transmisji bitów", w grupie niestandardowej "Streaming Bitrates".

Teraz możemy sprawdzić właściwości głównego przepływu pracy, poznamy naszych grupy niestandardowej z dwóch opublikowanych właściwości są wyświetlane. Obie są odzwierciedlające wartość ich odpowiednich AVC encoder szybkość transmisji bitów.

![Wyposażenia opublikowanych szybkość transmisji bitów w głównym przepływu pracy](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-bitrate-props-on-workflow-root.png)

Gdy chcemy uzyskać dostęp do tych właściwości z kodu lub wyrażenie, możemy to zrobić tak:

- z kodu w tekście ze składnika bezpośrednio poniżej głównego: node.getPropertyAsString('.. / video1bitrate ", zawiera wartości null)
- w wyrażeniu: ${ROOT_video1bitrate}
 
Załóżmy wykonaj grupie "Streaming Bitrates" przez opublikowanie naszych ścieżki dźwiękowej szybkość transmisji bitów nad nim również. We właściwościach kodera AAC Wyszukaj ustawienie szybkości transmisji bitów, a następnie wybierz pozycję Publikuj z menu rozwijanego obok. Publikowanie w głównym wykres za pomocą nazwy "audio1bitrate" i wyświetlana nazwa "1 w Audio szybkość transmisji bitów" w naszym grupy niestandardowej "Streaming Bitrates".

![Okno dialogowe publikowania szybkość transmisji bitów audio](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-audio-bitrate.png)

*Okno dialogowe publikowania szybkość transmisji bitów audio*

![Wynikowa wyposażenia audio i wideo na poziomie głównym](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-resulting-video-and-audio-props-on-root.png)

*Wynikowa wyposażenia audio i wideo na poziomie głównym*

Zauważ, że zmiana którejkolwiek z tych trzech wartości również ponownie konfiguruje i zmiany wartości na odpowiednie składniki są połączone (i w przypadku gdy opublikowane z).

###<a id="MXF_to__multibitrate_MP4_output_files"></a>Masz wygenerowany plik docelowy, który nazwy zależne od wartości właściwości opublikowanych

Zamiast hardcoding naszych nazwy wygenerowanych plików, możemy teraz można zmienić naszych wyrażenie filename dla poszczególnych składników danych wyjściowych w pliku, które korzystają z właściwości szybkość transmisji bitów, które możemy właśnie opublikowany w katalogu głównym wykresu. Począwszy od naszych pierwszego danych wyjściowych w pliku, Znajdź właściwość plików i edytowanie wyrażenia w następujący sposób:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video1bitrate}kbps.MP4

Różne parametry w tym wyrażeniu można uzyskać do nich dostęp i wprowadzone przez naciśnięcie znak dolara na klawiaturze przy aktywnym oknie wyrażenia. Jeden z parametrów dostępnych jest naszych właściwość video1bitrate, która możemy opublikowane wcześniej.

![Uzyskiwanie dostępu do parametrów w wyrażeniu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-accessing-parameters-within-an-expression.png)

*Uzyskiwanie dostępu do parametrów w wyrażeniu*

Zrobić to samo dla wyjściowych filmie drugiego pliku:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video2bitrate}kbps.MP4

i do wydruku pliku audio:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_audio1bitrate}bps_audio.MP4

Jeśli teraz możemy zmienić szybkość transmisji bitów dla plików audio lub wideo, odpowiednich kodera zostanie skonfigurowane i Konwencja nazwę pliku oparty na szybkość transmisji bitów będzie uznane całego automatycznego.

##<a id="thumbnails_to__multibitrate_MP4"></a>Dodawanie miniatury do multibitrate wyjściowe MP4

Rozpoczynając od przepływu pracy, który umożliwia generowanie [wprowadzania multibitrate MP4 wynikiem MXF](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), firma Microsoft będzie teraz być szukasz do dodawania miniatury w wyniku.

###<a id="thumbnails_to__multibitrate_MP4_overview"></a>Omówienie przepływu pracy, Dodawanie miniatury

![Przepływ pracy Multibitrate MP4 zacząć od](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-start-from.png)

*Przepływ pracy Multibitrate MP4 zacząć od*

###<a id="thumbnails_to__multibitrate_MP4__with_jpg"></a>Dodawanie kodowania JPG

Serce naszych generowanie miniatur będą składnika JPG Encoder, możliwość pliki JPG wyjściowe.

![Koder JPG](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-jpg-encoder.png)

*Koder JPG*

Firma Microsoft nie można jednak bezpośrednio połączyć naszych strumienia wideo nieskompresowane z wejście pliku multimediów do kodera JPG. Zamiast tego oczekuje były przekazywane poszczególnych ramkach. Tak, można zrobić za pomocą składnika bramy ramkę wideo.

![Łączenie brama ramki do kodera JPG](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-frame-gate-to-jpg-encoder.png)

*Łączenie brama ramki do kodera JPG*

Brama ramki raz na tyle sekund lub ramek umożliwia ramki wideo do przekazania. Interwał i godzinę, przesunięcie, z której dzieje się tak jest konfigurowany we właściwościach.

![Właściwości bramy ramki wideo](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-video-frame-gate-properties.png)

*Właściwości bramy ramki wideo*

Tworzenie miniatury co minutę przez ustawienie trybu na czas (w sekundach) i interwał do 60.

###<a id="thumbnails_to__multibitrate_MP4_color_space"></a>Sposób postępowania z konwersji przestrzeń kolorów

Podczas wydaje się logiczne, że teraz można łączyć obu PIN nieskompresowane wideo bramy ramki i wejście pliku multimediów, firma Microsoft może się ostrzeżenie, jeśli firma Microsoft może to zrobić.

![Kolor wprowadzania komunikat o błędzie](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-input-color-space-error.png)

*Kolor wprowadzania komunikat o błędzie*

Jest to spowodowane sposób w kolorze, które informacje jest reprezentowane w naszym oryginalny nieprzetworzonych nieskompresowane strumienia wideo, pochodzące z naszych MXF różni się od czego oczekuje JPG Encoder. Dokładniej tak zwany "przestrzeń kolorów" "RGB" lub "Skala odcieni szarości" oczekuje się przepływ. Oznacza to, że brama ramkę wideo ruchu przychodzącego strumienia wideo muszą mieć konwersja pierwszej stosowane dotyczące jej przestrzeń kolorów.

Przeciągnij przepływu pracy konwertera miejsca kolor - firmy Intel i połączyć go z naszych bramy ramki.

![Łączenie konwertera miejsca kolorów](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-color-space-convertor.png)

*Łączenie konwertera miejsca kolorów*

W oknie właściwości wybierz wpis BGR 24 z listy ustawienie wstępne.

###<a id="thumbnails_to__multibitrate_MP4_writing_thumbnails"></a>Pisanie miniatur

Inne niż filmie MP4, składnik JPG Encoder będzie wyjściowy więcej niż jeden plik. Aby poradzić sobie z tym, Edytor plik JPG sceny w wyszukiwania można też użyć: zostanie wykonać przychodzące miniatury JPG i zapisać je, każdej nazwy pliku jest umieszczona przez inną liczbę. (Liczba zwykle wskazująca liczbę sekund i jednostek w strumieniu, której miniaturę został wystawiony z.)


![Wprowadzenie do Writer pliku JPG wyszukiwania sceny](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer.png)

*Wprowadzenie do Writer pliku JPG wyszukiwania sceny*

Konfigurowanie właściwości ścieżka folderu danych wyjściowych z wyrażeniem: ${ROOT_outputWriteDirectory} 

i właściwości prefiks nazwy pliku z: 

    ${ROOT_sourceFileBaseName}_thumb_

Prefiks Określ, jak są nazywane miniatur plików. Będą one zostać umieszczona z liczbę wskazującą położeniu wskaźnika w strumieniu.


![Właściwości: zapis pliku JPG wyszukiwania sceny](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer-properties.png)

*Właściwości: zapis pliku JPG wyszukiwania sceny*

Połącz Writer pliku JPG wyszukiwania sceny do węzła aktywów pliku danych wyjściowych.

###<a id="thumbnails_to__multibitrate_MP4_errors"></a>Wykrywanie błędów w przepływie pracy

Połącz wprowadzania konwertera miejsca na kolor do nieprzetworzonych nieskompresowane wyjścia wideo. Teraz wykonać testy lokalne przepływu pracy. Jest dobrym pomysłem przepływ pracy zostanie nieoczekiwanie wykonywanie przerwane i wskazywanie z czerwone obramowanie na składnik, który wystąpił błąd:

![Błąd konwersji miejsca kolorów](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error.png)

*Błąd konwersji miejsca kolorów*

Kliknij niewielką ikonę czerwony "E" w górnym prawym rogu składnikiem konwertera miejsca kolorów, aby zobaczyć, co to jest przyczyna kodowania próba nie powiodła się.

![Okno dialogowe błędu konwersji miejsca kolorów](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error-dialog.png)

*Okno dialogowe błędu konwersji miejsca kolorów*

Okaże się, jak widać, że przychodzące przestrzeni kolorów standard konwertera miejsca koloru musi być rec601 dla naszych wymagane konwersja YUV RGB. Pozornie naszych strumienia nie oznacza jego rec601. (Rekordów 601 to standard kodowania z przeplotem analogowych sygnały wideo w formie cyfrowej wideo. Określa aktywnego regionu, obejmujące 720 jasności oraz 360 próbki chrominance dla każdego wiersza. Kolor kodowanie system nosi nazwę YCbCr 4:2:2.)

Aby rozwiązać ten problem, będzie wskazano na metadanych naszych strumienia mamy już zajmujących rec601 zawartości. W tym celu użyjemy składnik aktualizacji typ danych wideo, możemy przykleić między naszych nieprzetworzonych źródłowej i składnika konwersji miejsca koloru. Tej aktualizacji typ danych umożliwia ręczna aktualizacja określonych danych wideo właściwości typu. Konfigurowanie, aby wskazać kolor miejsca Standard "Rekordów 601". Spowoduje to aktualizacji typ danych wideo oznakować strumienia z przestrzeni kolorów "Rekordów 601", jeśli wystąpiło bez spacji kolor jeszcze zdefiniowany. (Nie zastąpi on metadane, o ile nie zostało zaznaczone pole wyboru Zastąp.)

![Aktualizowanie kolor standardowy miejsca na aktualizacji typu danych](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-update-color-space-standard-on-data-type.png)

*Aktualizowanie kolor standardowy miejsca na aktualizacji typu danych*

###<a id="thumbnails_to__multibitrate_MP4_finish"></a>Na koniec przepływu pracy

Teraz, gdy naszych naszych przepływ pracy został zakończony, czy inny test Uruchom, aby wyświetlić go przekazać.

![Na koniec przepływu pracy dla wielu mp4 wyjście z miniaturami](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-for-multi-mp4-thumbnails.png)

*Na koniec przepływu pracy dla wielu mp4 wyjście z miniaturami*

##<a id="time_based_trim"></a>Przycinanie opartych na czasie produkcji multibitrate MP4

Rozpoczynając od przepływu pracy, który umożliwia generowanie [wprowadzania multibitrate MP4 wynikiem MXF](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), firma Microsoft będzie teraz być szukasz do przycinania źródła wideo oparte na sygnatury czasowe.

###<a id="time_based_trim_start"></a>Omówienie przepływu pracy, aby rozpocząć dodawanie skracania do

![Uruchamianie przepływu pracy, aby dodać skracania do](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow-to-add-trimming.png)

*Uruchamianie przepływu pracy, aby dodać skracania do*

###<a id="time_based_trim_use_stream_trimmer"></a>Za pomocą przycinarka strumienia

Składnik przycinarka strumienia umożliwia przyciąć początek i koniec strumienia wprowadzania na chronometrażu informacji (sekund, minut,...). Przycinarka nie obsługuje przycinania na podstawie ramki.

![Przycinarka strumienia](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-stream-trimmer.png)

*Przycinarka strumienia*

Zamiast łączenie kodery AVC i assigner położenie głośnika z wejście pliku multimediów bezpośrednio, firma Microsoft będzie należy umieścić między tymi przycinarka strumienia. (Po jednej dla sygnału wideo i jedną dla przeplotem sygnału dźwiękowego.)

![Umieszczenie przycinarka strumienia między](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-put-stream-trimmer-in-between.png)

*Umieszczenie przycinarka strumienia między*

Załóżmy przycinarka tak skonfigurować program będą przetwarzane są jedynie wideo i audio między 15 sekundach i 60 sekund w klipie wideo.

Przejdź do właściwości przycinarka strumienia wideo i skonfigurować czas rozpoczęcia (15 s) i czas zakończenia (60s) właściwości. Aby upewnić się, że zarówno naszych przycinarka audio i wideo są zawsze skonfigurowane na tym samym początek i koniec wartości, firma Microsoft opublikuje te do głównego przepływu pracy.

![Publikowanie właściwość czasu rozpoczęcia przycinarka strumienia](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-start-time-from-stream-trimmer.png)

*Publikowanie właściwość czasu rozpoczęcia przycinarka strumienia*

![Publikowanie okno dialogowe właściwości dla czas rozpoczęcia](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-start-time.png)

*Publikowanie okno dialogowe właściwości dla czas rozpoczęcia*

![Publikowanie okno dialogowe właściwości dla czas zakończenia](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-end-time.png)

*Publikowanie okno dialogowe właściwości dla czas zakończenia*


Jeśli firma Microsoft teraz inspekcji na poziomie głównym naszym przepływu pracy, obie właściwości zostanie starannie wyświetlony można konfigurować stamtąd.

![Właściwości opublikowanych dostępne na poziomie głównym](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-properties-available-on-root.png)

*Właściwości opublikowanych dostępne na poziomie głównym*

Teraz otwieranie właściwości skracania z przycinarka audio i konfigurowanie zarówno rozpoczęcia i zakończenia z wyrażenie, które odwołuje się do właściwości opublikowanych w katalogu głównym naszym przepływu pracy.

Czas rozpoczęcia audio przycinania:

    ${ROOT_TrimmingStartTime}

i jego czas zakończenia:

    ${ROOT_TrimmingEndTime}

###<a id="time_based_trim_finish"></a>Na koniec przepływu pracy

![Na koniec przepływu pracy](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-time-base-trimming.png)

*Na koniec przepływu pracy*


##<a id="scripting"></a>Wprowadzenie do składnika skryptów

Składniki skryptów mogą wykonywanie skryptów dowolnego fazach wykonanie naszych przepływu pracy. Istnieją cztery różne skrypty, które mogą być wykonywane, każda z określonej właściwości i miejsca w cyklu życia przepływu pracy:

- **commandScript**
- **realizeScript**
- **processInputScript**
- **lifeCycleScript**

Dokumentacja składnika tworzone przechodzi bardziej szczegółowo dla każdego z powyższych. W [poniższej sekcji](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim)składnik skryptów **realizeScript** służy do tworzenia xml cliplist w czasie rzeczywistym podczas uruchamiania przepływu pracy. Ten skrypt jest nazywany podczas konfiguracji składnika dzieje się tylko raz w jego cyklu życia.


###<a id="scripting_hello_world"></a>Wykonywanie skryptów w ramach przepływu pracy: Witaj świecie

Przeciągnij składnik tworzone na powierzchnię projektanta i Zmień nazwę (na przykład "SetClipListXML").

![Dodawanie składnik skryptów](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

*Dodawanie składnik skryptów*

Inspekcji właściwości składnika tworzone cztery typy inny skrypt będzie wyświetlana, każdy konfigurowane w celu inny skrypt.

![Właściwości składnika skryptowych](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

*Właściwości składnika skryptów*

Wyczyść processInputScript i Otwórz edytor dla realizeScript. Teraz możemy już skonfigurowane i rozpocząć wykonywanie skryptów.

Skrypty są zapisywane w Groovy, dynamicznie skompilowany języka skryptowego dla języka Java platform, który zachowuje zgodność z języka Java. W rzeczywistości większość kodu języka Java jest prawidłowym kodem Groovy.

Załóżmy napisać skrypt groovy świata prosty powitania w kontekście naszych realizeScript. W edytorze wprowadź następujący:

    node.log("hello world");

Teraz wykonać lokalne testowa. Po tego uruchomienia inspekcja (za pomocą karty System na składniku tworzone) właściwości dzienników.

![Witaj świecie dziennika wyjścia](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output.png)

*Witaj świecie dziennika wyjścia*

Obiekt węzeł, który nazywamy metody dziennika, odwołuje się do naszych bieżącego "węzła" lub składnik, który jest firma Microsoft skryptów w. Każdy składnik sposób ma możliwość rejestrowania dane, dostępne na karcie system. W tym przypadku możemy wyjściowy ciąg znaków "Witaj świecie". Ważne dowiedzieć się, Oto, że to mogą stanowić cenne narzędzie debugowania, zapewniając omówienie skrypt faktycznie czynności.

Z naszych środowisku skryptów, możemy również mają dostęp do właściwości na inne składniki. Spróbuj wykonać następujące czynności:


    //inspect current node: 
    def nodepath = node.getNodePath(); 
    node.log("this node path: " + nodepath);
    
    //walking up to other nodes: 
    def parentnode = node.getParentNode(); 
    def parentnodepath = parentnode.getNodePath(); 
    node.log("parent node path: " + parentnodepath);
    
    //read properties from a node: 
    def sourceFileExt = parentnode.getPropertyAsString( "sourceFileExtension", null ); 
    def sourceFileName = parentnode.getPropertyAsString("sourceFileBaseName", null); 
    node.log("source file name with extension " + sourceFileExt + " is: " + sourceFileName);

Nasze okno Dziennik będzie zawierać us następujące informacje:

![Wynik dziennika do uzyskiwania dostępu do ścieżki](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output2.png)

*Wynik dziennika do uzyskiwania dostępu do ścieżki*


##<a id="frame_based_trim"></a>Na podstawie ramki przycinania produkcji multibitrate MP4

Rozpoczynając od przepływu pracy, który umożliwia generowanie [wprowadzania multibitrate MP4 wynikiem MXF](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), firma Microsoft będzie teraz być szukasz do przycinania źródła wideo według liczby ramki.

###<a id="frame_based_trim_start"></a>Przegląd plan, aby zacząć dodawać skracania do

![Przepływ pracy, aby zacząć dodawać skracania do](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-workflow-start-adding-trimming-to.png)

*Przepływ pracy, aby zacząć dodawać skracania do*

###<a id="frame_based_trim_clip_list"></a>Za pomocą listy wycinków XML

W wszystkich poprzednich samouczków przepływu pracy użyliśmy składnika multimediów plik wprowadzania jako naszych źródła wejściowego wideo. W tym scenariuszu dla określonych, możemy obsługiwać za pomocą składnika źródła listy klip zamiast tego. Należy zauważyć, że nie należy preferowany sposób pracy tylko źródło listy klip jest używane w przypadku rzeczywistą przyczyny w tym celu (jak w poniżej przypadkach, gdy mamy wprowadzasz wykorzystanie możliwości skracania listy klip).

Aby przełączyć się z naszym wejście multimediów w źródle listy klipu, przeciągnij składnik źródła listy Clip powierzchnię projektu i połącz pin klip listy XML do węzła XML listy klip projektanta przepływów pracy. To należy wypełnić źródło listy klipu z PIN dane wyjściowe, zgodnie z naszych wideo wejściowe. Teraz łączyć z PIN nieskompresowane wideo i Audio nieskompresowane z klipu źródło listy do odpowiednich kodery AVC i Interleaver strumieniu Audio. Teraz usunąć wejście pliku multimediów.

![Zastępowana wejście pliku multimediów źródło listy klipu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-replaced-media-file-with-clip-source.png)

*Zastępowana wejście pliku multimediów źródło listy klipu*

Składnik źródła listy klip ma jako własnych danych wejściowych "Klip listy XML". Wybierając plik źródłowy, aby przetestować z lokalnie, ten klip listy xml jest automatycznie wypełnione na podstawie dla Ciebie.

![Automatycznie wypełnione na podstawie właściwości klipu listy XML](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-auto-populated-clip-list-xml-property.png)

*Automatycznie wypełnione na podstawie właściwości klipu listy XML*

Odnaleźć nieco bliżej xml, Oto jak wygląda jak:

![Edytowanie okno Lista klipu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-edit-clip-list-dialog.png)

*Edytowanie okno Lista klipu*

To nie odzwierciedlać możliwości xml listy klipu. Jedną z opcji nasze jest dodanie elementu "Przycinanie" w obszarze obu audio i wideo źródła, tak jak poniżej:

![Dodawanie elementu Usuń.zbędne.odstępy do listy wycinków](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-trim-element-to-clip-list.png)

*Dodawanie elementu Usuń.zbędne.odstępy do listy wycinków*

Jeśli modyfikowanie xml listy klip tak powyżej i wykonać testy lokalnych, zobaczysz wideo poprawnie zostały przycięte od 10 do 20 sekund w klipie wideo.

Sprzeczne co się dzieje po wykonaniu lokalne Uruchom mimo tego samego kodu xml cliplist nie będzie zawierało ten sam efekt, gdy są stosowane w przepływ pracy jest uruchamiany w usługi multimediów Azure. Po uruchomieniu Azure Premium kodera cliplist xml jest generowany każdorazowo ponownie, na podstawie wprowadzania pliku otrzymaną kodowania zadania. Oznacza to, że wszelkie zmiany, które czynności na xml Niestety czy zastąpić.

Aby wyeliminować xml cliplist jest czyszczenia po uruchomieniu kodowania zadania, firma Microsoft może ponownie wygenerować go w czasie rzeczywistym tylko po rozpoczęciu naszych przepływu pracy. Takie akcje niestandardowe mogą być pobierane przez co nosi nazwę "Tworzone składnik". Aby uzyskać więcej informacji zobacz [Wprowadzenie do składnika tworzone](media-services-media-encoder-premium-workflow-tutorials.md#scripting).


Przeciągnij składnik tworzone na powierzchnię projektanta i zmień jego nazwę na "SetClipListXML".

![Dodawanie składnik skryptów](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

*Dodawanie składnik skryptów*

Inspekcji właściwości składnika tworzone cztery typy inny skrypt będzie wyświetlana, każdy konfigurowane w celu inny skrypt.

![Właściwości składnika skryptowych](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

*Właściwości składnika skryptowych*


###<a id="frame_based_trim_modify_clip_list"></a>Modyfikowanie listy wycinków ze składnika ładowanie

Przed możemy ponownie zapisać xml cliplist, który jest generowany podczas uruchamiania przepływu pracy, potrzebujemy mają dostęp do właściwości xml cliplist i zawartość. Firma Microsoft może to zrobić tak:

    // get cliplist xml: 
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: " + clipListXML);

![Przychodzące listy wycinków, rejestrowane](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-incoming-clip-list-logged.png)

*Przychodzące listy wycinków, rejestrowane*

Najpierw musisz sposobem na określenie z czym, do których punkt chcemy przycinanie klipu wideo. Aby to wygodny użytkownikowi techniczne mniej przepływu pracy, publikowanie dwie właściwości głównego wykresu. Aby to zrobić, kliknij prawym przyciskiem myszy Projektanta powierzchnia i wybierz pozycję "Dodaj właściwość":

- Pierwszej właściwości: "ClippingTimeStart" typu: "Kod CZASOWY"
- W drugim właściwości: "ClippingTimeEnd" typu: "Kod CZASOWY"

![Dodawanie okno dialogowe właściwości dla czas rozpoczęcia wycinek](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-start-time.png)

*Dodawanie okno dialogowe właściwości dla czas rozpoczęcia wycinek*

![Opublikowany wycinek wyposażenia czasu na poziomie głównym przepływu pracy](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-time-props.png)

*Opublikowany wycinek wyposażenia czasu na poziomie głównym przepływu pracy*

Konfigurowanie obu właściwości do odpowiedniej wartości:

![Konfigurowanie wycinek rozpoczęcia i zakończenia właściwości](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configure-clip-start-end-prop.png)

*Konfigurowanie start wycinek i kończą właściwości*

Teraz z naszych skryptu firma Microsoft jest dostępna dla obu właściwości, tak jak poniżej:

    
    // get start and end of clipping:
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();
    
    node.log("clipping start: " + clipstart);
    node.log("clipping end: " + clipend);

![Okno Dziennik przedstawiający początek i koniec wycinek](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-show-start-end-clip.png)

*Okno Dziennik przedstawiający początek i koniec wycinek*

Załóżmy analizować ciągi kod czasowy do wygodniej jest za pomocą formularza za pomocą prostych wyrażeń regularnych:
    
    //parse the start timing: 
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart); 
    startregresult.matches(); 
    def starttimecode = startregresult.group(1); 
    node.log("timecode start is: " + starttimecode); 
    def startframerate = startregresult.group(2); 
    node.log("framerate start is: " + startframerate);
    
    //parse the end timing: 
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend); 
    endregresult.matches(); 
    def endtimecode = endregresult.group(1); 
    node.log("timecode end is: " + endtimecode); 
    def endframerate = endregresult.group(2); 
    node.log("framerate end is: " + endframerate);

![Okno Dziennik o wyjściowej zanalizowana kod czasowy](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-output-parsed-timecode.png)

*Okno Dziennik o wyjściowej zanalizowana kod czasowy*

Za pomocą tych informacji w wykonywanego możemy teraz modyfikować xml cliplist tak, aby odzwierciedlała godziny rozpoczęcia i zakończenia dla odpowiedniej wycinek dokładne ramkę filmu.

![Kod skryptu, aby dodać elementy przycięcia](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-trim-elements.png)

*Kod skryptu, aby dodać elementy przycięcia*

Zostało to zrobić za pomocą operacji na ciągach normalny manipulowania. Wynikowy kod xml listy zmodyfikowany klip jest zapisywane właściwość clipListXML w katalogu głównym przepływu pracy za pomocą metody "setProperty". Okno Dziennik po uruchomieniu inny test będzie zawierać nam następujące informacje:

![Rejestrowanie uzyskanej liście klipu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-result-clip-list.png)

*Rejestrowanie uzyskanej liście klipu*

Wykonaj Uruchom test, aby zobaczyć, jak strumienie audio i wideo został przycięty. Jak będzie więcej niż jeden testowa o różnych wartościach dla punktów skracania można zauważyć, że te nie bierze się pod uwagę jednak! Przyczyną to, że projektanta w przeciwieństwie do wykonywania Azure nie zastępuje cliplist xml co Uruchom. Oznacza to, że tylko podczas pierwszego ustawiono punkty wewnętrzne i zewnętrzne, spowoduje, że xml przekształcić, wszystkich innych terminów, naszych klauzula straż (jeśli (clipListXML.indexOf ("<trim>") == -1)) uniemożliwi dodawania innego elementu Usuń.zbędne.odstępy, gdy istnieje już Prezentuj przepływu pracy.

Aby uprościć naszych przepływu pracy do testowania lokalnie, najlepsze dodajemy niektóre kody zachowania domu sprawdza, jeśli jest już Usuń.zbędne.odstępy elementu. Jeśli tak, firma Microsoft można usunąć, przed kontynuowaniem, modyfikując xml z nowymi wartościami. Zamiast używać operacje zwykły ciąg, jest prawdopodobnie bezpieczniejsze to zrobić przy użyciu modelu obiektów xml rzeczywistą analizy.

Zanim dodamy takiego kodu, gdy potrzebujemy najpierw dodać wiele instrukcje importu na początku naszych skrypt:
    
    import javax.xml.parsers.*; 
    import org.xml.sax.*; 
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*; 
    import javax.xml.transform.*; 
    import javax.xml.transform.stream.*; 
    import javax.xml.transform.dom.*;

Następnie można dodać wymagane czyszczenia kodu:

    //for local testing: delete any pre-existing trim elements from the clip list xml by parsing the xml into a DOM:
    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder();
    InputSource is=new InputSource(new StringReader(clipListXML)); 
    Document dom=builder.parse(is);

    //find the trim element inside videoSource and audioSource and remove it if it exists already: 
    XPath xpath = XPathFactory.newInstance().newXPath();
    String findAllTrimElements = "//trim"; 
    NodeList trimelems = xpath.evaluate(findAllTrimElements,dom,XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection 
    Set<Element> elementsToDelete = new HashSet<Element>(); 
    for (int i = 0; i < trimelems.getLength(); i++) { 
        Element e = (Element)trimelems.item(i); 
        elementsToDelete.add(e); 
    }

    node.log("about to delete any existing trim nodes");
     //delete the trim nodes: 
    elementsToDelete.each{ 
        e -> e.getParentNode().removeChild(e);
    }; 
    node.log("deleted any existing trim nodes");
    
    //serialize the modified clip list xml dom into a string: 
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result); 
    clipListXML = result.getWriter().toString();
    
Kod przechodzi powyżej punktu przecięcia dodajemy Usuń.zbędne.odstępy elementów XML cliplist.

W tym momencie możemy Uruchom i modyfikowanie naszych przepływu pracy jako podobnie jak chcemy, mając wprowadzone kiedykolwiek czasu.    

###<a id="frame_based_trim_clippingenabled_prop"></a>Dodawanie właściwości wygody ClippingEnabled

Wymaganą może nie zawsze skracanie nastąpić, Przejdźmy zakończenia, wyłączanie naszych przepływu pracy, dodając flagę logiczną wygodny, która wskazuje, czy chcemy włączyć skracanie / wycinek.

Tak jak wcześniej, Opublikuj nową właściwość do głównego naszych przepływu pracy o nazwie "ClippingEnabled" typ "boolean".

![Opublikowany właściwości umożliwiających wycinek](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-enable-clip.png)

*Opublikowany właściwości umożliwiających wycinek*

Z poniżej klauzuli straż proste, firma Microsoft Sprawdź, czy wymagane jest przycinanie i zdecydować, jeśli naszej listy klipu jako takie nie trzeba można modyfikować, czy nie.

    //check if clipping is required: 
    def clippingrequired = node.getProperty("../ClippingEnabled"); 
    node.log("clipping required: " + clippingrequired.toString()); 
    if(clippingrequired == null || clippingrequired == false) 
    {
        node.setProperty("../clipListXml",clipListXML); 
        node.log("no clipping required"); 
        return; 
    }


###<a id="code"></a>Kompletny kod

    import javax.xml.parsers.*; 
    import org.xml.sax.*; 
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*; 
    import javax.xml.transform.*; 
    import javax.xml.transform.stream.*; 
    import javax.xml.transform.dom.*;
    
    // get cliplist xml: 
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: \n" + clipListXML);
    // get start and end of clipping: 
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();
    
    //parse the start timing:
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart); 
    startregresult.matches(); 
    def starttimecode = startregresult.group(1);
    node.log("timecode start is: " + starttimecode);
    def startframerate = startregresult.group(2);
    node.log("framerate start is: " + startframerate);
    
    //parse the end timing: 
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend);
    endregresult.matches(); 
    def endtimecode = endregresult.group(1); 
    node.log("timecode end is: " + endtimecode); 
    def endframerate = endregresult.group(2);

    node.log("framerate end is: " + endframerate);
    
    //for local testing: delete any pre-existing trim elements 
    //from the clip list xml by parsing the xml into a DOM:
    
    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder(); 
    InputSource is=new InputSource(new StringReader(clipListXML)); 
    Document dom=builder.parse(is);

    //find the trim element inside videoSource and audioSource and remove it if it exists already:
    XPath xpath = XPathFactory.newInstance().newXPath(); 
    String findAllTrimElements = "//trim"; 
    NodeList trimelems = xpath.evaluate(findAllTrimElements, dom, XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection 
    Set<Element> elementsToDelete = new HashSet<Element>(); 
    for (int i = 0; i < trimelems.getLength(); i++) { 
        Element e = (Element)trimelems.item(i); 
        elementsToDelete.add(e); 
    }
    
    node.log("about to delete any existing trim nodes");
    //delete the trim nodes:
    elementsToDelete.each{ e -> 
        e.getParentNode().removeChild(e); 
    };
    node.log("deleted any existing trim nodes");

    //serialize the modified clip list xml dom into a string:
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result);
    clipListXML = result.getWriter().toString();

    //check if clipping is required:
    def clippingrequired = node.getProperty("../ClippingEnabled");
    node.log("clipping required: " + clippingrequired.toString()); 
    if(clippingrequired == null || clippingrequired == false) 
    {
        node.setProperty("../clipListXml",clipListXML);
        node.log("no clipping required");
        return; 
    }

    //add trim elements to cliplist xml 
    if ( clipListXML.indexOf("<trim>") == -1 ) 
    {
        //trim video 
        clipListXML = clipListXML.replace("<videoSource>","<videoSource>\n <trim>\n <inPoint fps=\""+ 
            startframerate +"\">" + starttimecode + 
            "</inPoint>\n" + "<outPoint fps=\"" + endframerate +"\"> " + endtimecode + 
            " </outPoint>\n </trim> \n"); 
        //trim audio 
        clipListXML = clipListXML.replace("<audioSource>","<audioSource>\n <trim>\n <inPoint fps=\""+ 
            startframerate +"\">" + starttimecode + 
            "</inPoint>\n" + "<outPoint fps=\""+ endframerate +"\">" + 
            endtimecode + "</outPoint>\n </trim>\n");
        node.log( "clip list going out: \n" +clipListXML ); 
        node.setProperty("../clipListXml",clipListXML); 
    }


##<a name="also-see"></a>Zobacz też 

[Wprowadzenie do Premium kodowanie w multimediów Azure usług](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

[Jak używać Premium kodowanie w multimediów Azure usług](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)

[Zawartość na żądanie za pomocą usługi multimediów Azure](media-services-encode-asset.md#media_encoder_premium_workflow)

[Formatów przepływu pracy Premium Encoder multimediów oraz koderów-dekoderów](media-services-premium-workflow-encoder-formats.md)

[Przykładowe pliki przepływu pracy](http://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows)

[Narzędzie Eksploratora usługi multimediów Azure](http://aka.ms/amse)

##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
