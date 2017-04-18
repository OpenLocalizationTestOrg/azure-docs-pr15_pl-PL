<properties
    pageTitle="Przy użyciu wielu plików wejściowych i właściwości elementów z kodera Premium | Microsoft Azure"
    description="W tym temacie wyjaśniono, jak używać setRuntimeProperties korzystanie z wielu plików wprowadzania i przekazywanie niestandardowe dane do procesora multimediów Media Encoder Premium w przepływie pracy."
    services="media-services"
    documentationCenter=""
    authors="xpouyat"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"  
    ms.author="xpouyat;anilmur;juliako"/>

# <a name="using-multiple-input-files-and-component-properties-with-premium-encoder"></a>Używanie wielu plików wejściowych i właściwości składnika z kodera Premium

## <a name="overview"></a>Omówienie

Istnieją scenariusze, w których może być potrzebne Dostosowywanie właściwości składnika określonej zawartości XML listy klip lub wysłać pliki wprowadzania podczas przesyłania zadań z procesorem multimediów **Media Encoder Premium przepływu** . Oto kilka przykładów są:

- Nakładanie tekstu na wideo i ustawianie wartości tekstowe (na przykład data bieżąca) w czasie wykonywania dla każdego wprowadzania wideo.
- Dostosowywanie XML listy klipu (Aby określić jednego lub kilku pliki źródłowe, z lub bez skracania itp.).
- Nakładanie obrazu logo na wideo wejściowe podczas klip wideo jest kodowane.

Aby umożliwić **Media Encoder Premium przepływu** wiedzieć, że chcesz zmienić niektóre właściwości w przepływie pracy podczas tworzenia zadania lub wysłać pliki wprowadzania danych, należy użyć ciągu konfiguracji, która zawiera **setRuntimeProperties** i/lub **transcodeSource**. W tym temacie wyjaśniono, jak z nich korzystać.


## <a name="configuration-string-syntax"></a>Składnia ciągu konfiguracji

Ciąg konfiguracji, aby ustawić w zadaniu kodowania używa dokumentu XML, która wygląda następująco:

    <?xml version="1.0" encoding="utf-8"?>
    <transcodeRequest>
      <transcodeSource>
      </transcodeSource>
      <setRuntimeProperties>
        <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
      </setRuntimeProperties>
    </transcodeRequest>


Poniżej przedstawiono kod C#, który odczytuje Konfiguracja XML z pliku i przekazuje je do zadań w ramach zadania:

    XDocument configurationXml = XDocument.Load(xmlFileName);
    IJob job = _context.Jobs.CreateWithSingleTask(
                                                  "Media Encoder Premium Workflow",
                                                  configurationXml.ToString(),
                                                  myAsset,
                                                  "Output asset",
                                                  AssetCreationOptions.None);


## <a name="customizing-component-properties"></a>Dostosowywanie właściwości elementów  

### <a name="property-with-a-simple-value"></a>Właściwość z wartością prostych
W niektórych przypadkach jest przydatne dostosować właściwości składnika razem z pliku przepływu pracy, który ma być wykonane przez Media Encoder Premium w przepływu pracy.

Załóżmy, że przeznaczona tekstu nakładki przepływu pracy na swoich klipów wideo, tekst (na przykład data bieżąca) ma być ustawiana w czasie wykonywania. Możesz to zrobić przy użyciu tekstu, który ma być ustawiona jako nową wartość dla właściwości text składnika nakładki z kodowania zadania. Ten mechanizm umożliwia zmienić inne właściwości składnika w ramach przepływu pracy (na przykład położenia lub kolor nakładki, szybkość transmisji bitów AVC encoder itp.).

**setRuntimeProperties** jest używana do zastępowania właściwości w polu składniki przepływu pracy.

Przykład:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
          <property propertyPath="/primarySourceFile" value="MyInputVideo.mp4" />
          <property propertyPath="Optional Overlay/Overlay/filename" value="MyLogo.png"/>
          <property propertyPath="Optional Text Overlay/Text To Image Converter/text" value="Today is Friday the 13th of May, 2016"/>
      </setRuntimeProperties>
    </transcodeRequest>


### <a name="property-with-an-xml-value"></a>Właściwość o wartości XML

Aby ustawić właściwość, która oczekuje wartości XML, umieszczać przy użyciu `<![CDATA[ and ]]>`.

Przykład:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

>[AZURE.NOTE]Upewnij się, że nie umieszczenie karetki w zwracanej tylko po `<![CDATA[`.


### <a name="propertypath-value"></a>wartość propertyPath

W poprzednich przykładach została propertyPath "/ plików multimedialnych wprowadzania/nazwa_pliku" lub "-inactiveTimeout" lub "clipListXml".
Jest to zazwyczaj nazwa składnika, a następnie nazwę właściwości. Ścieżka może mieć więcej lub mniej poziomów, takich jak "/ primarySourceFile" (ponieważ jest właściwością w katalogu głównym przepływ pracy) lub "/ wideo przetwarzania i grafiki w trybie nakładania-nieprzezroczystości" (ponieważ nakładki znajduje się w grupie).    

Aby sprawdzić nazwę ścieżki i właściwości, użyj przycisku akcji, która znajduje się bezpośrednio obok każdej właściwości. Możesz kliknij ten przycisk akcji i wybierz polecenie **Edytuj**. Spowoduje to wyświetlenie rzeczywistej nazwie, właściwości i bezpośrednio nad nią, obszaru nazw.

![Akcja i Edytuj](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture6_actionedit.png)

![Właściwość](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture7_viewproperty.png)

## <a name="multiple-input-files"></a>Istnieje wiele plików wejściowych

Poszczególnych zadań, które przedstawia **Media Encoder Premium przepływu** wymaga dwóch elementów:

- Pierwszy z nich jest *Trwałego przepływu pracy* , który znajduje się plik przepływu pracy. Pliki przepływu pracy można zaprojektować za pomocą [Projektanta przepływów pracy](media-services-workflow-designer.md).
- Drugi jest *Multimedialnej* , która zawiera pliki multimedialne, które ma zostać zakodowany.

Jeśli wysyłasz wielu plików multimedialnych do kodera **Media Encoder Premium przepływu** , zastosować następujące ograniczenia:

- Wszystkie pliki multimedialne musi być w tym samym *Multimedialnej*. Używanie wielu zasobów multimedialnych nie jest obsługiwane.
- Należy ustawić pliku podstawowego w tym multimedialnej (najlepiej, jeśli jest to główny plik wideo, który koder jest monit o procesu).
- Jest to konieczne, w celu przekazania danych konfiguracji, zawierający element **setRuntimeProperties** i/lub **transcodeSource** do procesora.
  - **setRuntimeProperties** umożliwia zastępowanie właściwość filename lub innej właściwości w polu składniki przepływu pracy.
  - **transcodeSource** służy do określania zawartości XML listy klipu.

Połączenia w ramach przepływu pracy:

 - Jeśli za pomocą **setRuntimeProperties** Określ nazwę pliku za pomocą jednej lub kilku składniki wejście multimediów i planowanie następnie nie pin części pliku podstawowego łączyć się z nimi. Upewnij się, że istnieje połączenie między obiektu podstawowego pliku i Input(s) plik multimediów.
 - Jeśli wolisz używać klipu listy XML i jeden składnik Źródło multimediów, następnie można połączyć obie razem.

![Brak połączenia od podstawowego pliku źródłowego wejście multimediów](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture0_nopin.png)

*Użycie setRuntimeProperties ustawić właściwość filename jest nie połączenia z podstawowego pliku składniki wejście multimediów.*

![Połączenie z listy wycinków XML do programu Clip źródła listy](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture1_pincliplist.png)

*Można połączyć klip listy XML się źródłem multimediów i używać transcodeSource.*


### <a name="clip-list-xml-customization"></a>Tworzenie wycinków dostosowywania listy XML
Możesz określić XML listy klipu w ramach przepływu pracy w czasie rzeczywistym przy użyciu **transcodeSource** w ciągu konfiguracji XML. W tym celu pin klipu listy XML do przyłączania się do składnika Źródło multimediów w przepływie pracy.

    <?xml version="1.0" encoding="utf-16"?>
      <transcodeRequest>
        <transcodeSource>
          <clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>video-part1.mp4</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>video-part1.mp4</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
          </clipList>
        </transcodeSource>
        <setRuntimeProperties>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

Jeśli chcesz określić /primarySourceFile za pomocą tej właściwości nazwy plików wyjściowych przy użyciu "Wyrażeń", następnie zalecamy przekazując XML listy klipu jako właściwość *po* właściwości /primarySourceFile, aby uniknąć listy wycinków można zastąpić przez ustawienie /primarySourceFile.

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="c:\temp\start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>c:\temp\start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>c:\temp\start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

Z dodatkowych skracania dokładne ramki:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <trim>
                  <inPoint fps="25">00:00:05:24</inPoint>
                  <outPoint fps="25">00:00:10:24</outPoint>
                </trim>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
               <trim>
                  <inPoint fps="25">00:00:05:24</inPoint>
                  <outPoint fps="25">00:00:10:24</outPoint>
                </trim>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>


## <a name="example"></a>Przykład

Należy rozważyć przykład, w którym chcesz nałożyć obrazu logo na wideo wejściowe podczas klip wideo jest kodowane. W tym przykładzie wprowadzania klip wideo o nazwie "MyInputVideo.mp4" i logo ma nazwę "MyLogo.png". Należy wykonać następujące czynności:

- Utwórz trwały przepływu pracy z pliku przepływu pracy (Zobacz przykład poniżej).
- Tworzenie zawartości multimediów, który zawiera dwa pliki: MyInputVideo.mp4 jako pliku podstawowego i MyLogo.png.
- Wysyłanie zadania przepływu pracy programu Media Encoder Premium procesor multimediów z powyższych wprowadzania składniki majątku i określ następujący ciąg konfiguracji.

Konfiguracja:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
          <property propertyPath="/primarySourceFile" value="MyInputVideo.mp4" />
          <property propertyPath="Media File Input Logo/filename" value="MyLogo.png" />
        </setRuntimeProperties>
      </transcodeRequest>


W powyższym przykładzie nazwę pliku wideo są wysyłane do składnika wejście multimediów i właściwości primarySourceFile. Nazwa pliku logo są wysyłane do innego multimediów wejście podłączonego do składnika nakładki grafiki.

>[AZURE.NOTE]Nazwa pliku wideo są wysyłane do właściwości primarySourceFile. Przyczyną jest dotyczące tworzenia nazwy pliku poprawne dane wyjściowe przy użyciu wyrażeń, na przykład za pomocą tej właściwości w przepływie pracy.


### <a name="step-by-step-workflow-creation-that-overlays-a-logo-on-top-of-the-video"></a>Tworzenie przepływu pracy krok po kroku nakładany na logo u góry klipu wideo     

Poniżej przedstawiono procedurę tworzenia przepływu pracy, która ma dwa pliki jako danych wejściowych: klip wideo i obraz. Będzie on nakładki obraz u góry klipu wideo.

Otwórz **Projektanta przepływów pracy** i wybierz **plik** > **Nowego obszaru roboczego** > **Plan kod transakcji**.

Nowy przepływ pracy zawiera trzy elementy:

- Plik źródłowy podstawowego
- Lista XML klipów
- Dane wyjściowe pliku i zawartości  

![Nowy przepływ pracy kodowania](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture9_empty.png)

*Nowy przepływ pracy kodowania*


Aby zaakceptować plik multimedialny wprowadzania, rozpoczyna się dodawania multimediów plik wprowadzania składnika. Aby dodać składnik do przepływu pracy, poszukaj go w polu wyszukiwania repozytorium i przeciągnij żądaną pozycję na okienka projektanta.

Następnie dodaj plik wideo może być używany do projektowania przepływu pracy. W tym celu kliknij okienko tła w Projektancie przepływów pracy i sprawdź właściwość podstawowy plik źródłowy w okienku właściwości po prawej stronie. Kliknij ikonę folderu, a następnie wybierz odpowiedni plik wideo.

![Plik źródłowy](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture10_primaryfile.png)

*Plik źródłowy*


Następnie określ pliku wideo w składniku wejście multimediów.   

![Źródła danych wejściowych plików multimedialnych](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture11_mediafileinput.png)

*Źródła danych wejściowych plików multimedialnych*


Jak to zrobić, składnik wejście multimediów sprawdzenie pliku i wypełnić PIN jej wynik, aby odzwierciedlała plik, który go inspekcji.

Następnym krokiem jest dodanie "wideo danych typu aktualizacji" Aby określić miejsce kolorów, aby Rec.709. Dodawanie "wideo konwerter formatu" ustawionej na typ układu układu danych = można konfigurować planarnych. Spowoduje to przekonwertowanie strumienia wideo do formatu, który należy podjąć jako źródło składnika nakładki.

![Konwerter formatu i aktualizacji typ danych wideo](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter.png)

*Aktualizacji typ danych wideo i konwerter formatu*

![Typ układu = można konfigurować planarnych](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter2.png)

*Typ układu jest można konfigurować planarnych*

Następnie dodać składnik nakładanie wideo i połączyć (nieskompresowane) wideo numer pin (nieskompresowane) wideo danych wejściowych plik multimediów.

Dodawanie innego wejście pliku multimediów (Aby załadować plik logo), kliknij ten składnik i zmień jego nazwę na "Multimediów plik danych wejściowych Logo" i wybierz pozycję Obraz (PNG pliku na przykład) w polu właściwości pliku. Numer pin nieskompresowane obraz nawiązać połączenia numer pin nieskompresowane obraz nakładki.

![Nakładanie składnik i obrazu źródłowego pliku](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture13_overlay.png)

*Nakładanie składnik i obraz źródło pliku*


Jeśli chcesz zmodyfikować położenie logo na klipie wideo (na przykład warto umieść go na 10 procent poza górnym lewym rogu wideo), wyczyść pole wyboru "Ręcznego wprowadzania". Można to zrobić, ponieważ używasz wejście pliku multimediów o podanie pliku logo do składnika nakładki.

![Położenie nakładki](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture14_overlay_position.png)

*Położenie nakładki*


Do kodowania strumień wideo, aby H.264, Dodaj składniki kodera kodera wideo AVC i AAC na powierzchnię projektanta. Łączenie numery PIN.
Konfigurowanie kodera AAC i wybierz Format Audio konwersji i ustawienie: 2.0 (L, R).

![Kodery audio i wideo](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture15_encoders.png)

*Kodery audio i wideo*


Teraz Dodaj składniki **ISO Mpeg-4 multiplekser** i **Danych wyjściowych w pliku** i połącz PIN, jak pokazano.

![MP4 multiplekser i danych wyjściowych w pliku](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture16_mp4output.png)

*MP4 multiplekser i danych wyjściowych w pliku*


Musisz określić nazwę dla pliku wyjściowego. Kliknij składnik **Danych wyjściowych w pliku** i edytować wyrażenie plik:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_withoverlay.mp4

![Nazwa wyjściowego pliku](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture17_filenameoutput.png)

*Nazwa wyjściowego pliku*

Można uruchamiać przepływ pracy lokalnie, aby sprawdzić, czy działa poprawnie.

Po zakończeniu, można go uruchamiać w usługi multimediów Azure.

Najpierw należy przygotować środka trwałego w usługi multimediów Azure z dwóch plików: pliku wideo i logo. Możesz to zrobić przy użyciu programu .NET lub interfejsu API usługi REST. Można też w tym przy użyciu Azure portal lub [Eksplorator usługi multimediów Azure](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE).

Ten samouczek pokazano, sposób zarządzania zasobami za pomocą AMSE. Istnieją dwa sposoby dodawania plików do środka trwałego:

- Tworzenie folderu lokalnego, skopiuj oba pliki i przeciągnij i upuść folderu na karcie **zawartości** .
- Przekazywanie plików wideo jako środka trwałego, wyświetlać informacje o elemencie zawartości, przejdź na kartę pliki i przekaż plik dodatkowy (logo).

>[AZURE.NOTE]Upewnij się ustawić podstawowego pliku w zawartości (główny plik wideo).

![Pliki zawartości w AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture18_assetinamse.png)

*Pliki zawartości w AMSE*


Zaznacz zasób, a następnie wybierz pozycję kodowania jej pomocą Premium Encoder. Przekaż przepływu pracy i zaznacz go.

Kliknij przycisk w celu przekazania danych do procesora, a następnie dodaj następujące XML, aby ustawić właściwości środowisko uruchomieniowe:

![Koder Premium w AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture19_amsepremium.png)

*Koder Premium w AMSE*


Następnie wklej następujące dane XML. Należy określić nazwę pliku wideo wejście multimediów i primarySourceFile. Określ nazwę pliku logo zbyt.

    <?xml version="1.0" encoding="utf-16"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
          <property propertyPath="/primarySourceFile" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

![setRuntimeProperties](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture20_amsexmldata.png)

*setRuntimeProperties*


Jeśli .NET SDK umożliwia tworzenie i uruchamianie zadania, takie dane XML musi być przekazywane jako ciąg konfiguracji.

    public ITask AddNew(string taskName, IMediaProcessor mediaProcessor, string configuration, TaskOptions options);

Po ukończeniu zadania pliku MP4 w zawartości dane wyjściowe wyświetla nakładki!

![Nakładki na klip wideo](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture21_resultoverlay.png)

*Nakładki na klip wideo*


Przykładowy przepływ pracy można pobrać z [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/).


## <a name="see-also"></a>Zobacz też

- [Wprowadzenie do Premium kodowanie w multimediów Azure usług](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

- [Jak używać kodowanie Premium w usługi multimediów Azure](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)

- [Kodowanie zawartości na żądanie za pomocą usługi multimediów Azure](media-services-encode-asset.md#media_encoder_premium_workflow)

- [Kodery-dekodery i formaty Media Encoder Premium w przepływu pracy](media-services-premium-workflow-encoder-formats.md)

- [Przykładowe pliki przepływu pracy](https://github.com/AzureMediaServicesSamples/Encoding-Presets/tree/master/VoD/MediaEncoderPremiumWorkfows)

- [Narzędzie Eksploratora usługi multimediów Azure](http://aka.ms/amse)

## <a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
