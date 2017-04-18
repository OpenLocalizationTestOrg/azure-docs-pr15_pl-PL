<properties 
    pageTitle="Wstawianie reklam po stronie klienta | Microsoft Azure" 
    description="W tym temacie przedstawia sposób włożenia reklam po stronie klienta." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="juliako"/>


#<a name="inserting-ads-on-the-client-side"></a>Wstawianie reklam po stronie klienta

Ten temat zawiera informacje na temat wstawiania różne typy reklam po stronie klienta.

Informacje na temat zamknięte podpisy kodowane oraz ad obsługi w Live strumieniowego przesyłania wideo zobacz [obsługiwane kodowane i standardy wstawiania Ad](media-services-live-streaming-with-onprem-encoders.md#cc_and_ads).

>[AZURE.NOTE] Azure Media Player nie obsługuje obecnie reklam.

##<a id="insert_ads_into_media"></a>Wstawianie reklam do multimediów

Usługi multimediów Azure umożliwia wstawianie reklam platformy Windows Media: RAM odtwarzacza. Ramy odtwarzacza z obsługą ad są dostępne dla systemu Windows 8, Silverlight, Windows Phone 8 i iOS urządzeń. Każde z ram odtwarzacza zawiera przykładowy kod, który pokazano, jak wdrażać aplikacja odtwarzacza. Istnieją trzy różne typy reklam, którą można wstawić do multimediów: listy.

- **Liniowy** — reklam pełny ramki Wstrzymaj wideo głównym.
- **Nonlinear** — nakładki reklam, które są wyświetlane podczas odtwarzania wideo głównym, zazwyczaj logo lub inny obraz statyczny umieszczony w odtwarzaczu.
- **Pomocnik** — reklam, wyświetlane poza odtwarzacza.

Oferty można umieścić w dowolnym miejscu na osi czasu głównym klip wideo. Odtwarzacza należy określić, kiedy odtwarzać ad i które reklam, aby odtworzyć. Można to zrobić przy użyciu zestawu standardowych plików opartym na języku XML: wideo Ad usługi szablonu (VAST), cyfrowego wideo wielu Ad odtwarzania (VMAP), szablon abstrakcyjne harmonogramu w multimediów (KAT) i cyfrowego wideo odtwarzacza Ad interfejsu definicji (VPAID). DUŻYCH plików Określ, jakie reklam, aby wyświetlić. Pliki VMAP Określ, kiedy odtwarzać różne reklam i zawierają OGROMNĄ XML. Pliki kat są reklam sekwencji, które zawierają można również OGROMNĄ XML w inny sposób. Pliki VPAID Definiowanie interfejs między odtwarzacza wideo i ad lub serwer usług ad.

Każdy framework odtwarzacza działaniu i wszystkie będą opisane w temacie własny. W tym temacie opisując podstawowe mechanizmy służące do wstawiania reklam. Aplikacje odtwarzacza wideo Poproś serwer usług ad reklam. Serwer ad może odpowiadać na kilka sposobów:

- Zwracana bardzo pliku
- Zwraca plik VMAP (z osadzonego VAST)
- Zwraca plik kat (z osadzonego VAST)
- Zwracana OGROMNĄ plik z VPAID AD

 
###<a name="using-a-video-ad-service-template-vast-file"></a>Za pomocą pliku szablonu (VAST) usług Ad wideo

Plik OGROMNĄ Określa, jaki ad lub reklam, aby wyświetlić. Następujące XML przedstawiono przykładowy plik OGROMNĄ liniowej ad:
    
    <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="115571748">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://www.myserver.com/tracking-resource]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <TrackingEvents>
                  <Tracking event="start"><![CDATA[http://www.myserver.com/start-tracking-resource]]></Tracking>
                  <Tracking event="midpoint"><![CDATA[http://www.myserver.com/midpoint-tracking-resource]]></Tracking>
                  <Tracking event="complete"><![CDATA http://www.myserver.com/complete-tracking-resource]]></Tracking>
                  <Tracking event="expand"><![CDATA[http://www.myserver.com/expand-tracking-resource]]></Tracking>
                </TrackingEvents>
                <VideoClicks>
                  <ClickThrough id="Atlas Redirect"><![CDATA[http://www.myserver.com/click-resource]]></ClickThrough>
                  <ClickTracking id="Spare"></ClickTracking>
                </VideoClicks>
                <MediaFiles>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_200_4x3.wmv]]>
                  </MediaFile>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_300_4x3.wmv]]>
                  </MediaFile>
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
          <Extensions>
            <Extension type="Atlas">
            </Extension>
          </Extensions>
        </InLine>
      </Ad>
    </VAST>
    
Liniowy ad jest opisywany przez **<Linear>** elementu. Określa czas trwania ad, śledzenia zdarzeń, kliknij pozycję za pośrednictwem kliknij pozycję śledzenie i liczba **<MediaFile>** elementy. Śledzenie zdarzeń są określane w **<TrackingEvents>** element i zezwolić na serwer usług ad do śledzenia różnych zdarzeń występujących podczas przeglądania ad. W tym przypadku uruchomienia, punkt środkowy, ukończono i rozwijanie wydarzenia są śledzone. Po wyświetleniu ad wystąpieniu zdarzenia start. Zdarzenia punkt środkowy występuje, gdy co najmniej że 50% osi czasu jest ad mogły zobaczyć. Zdarzenie complete występuje, gdy ad zostało uruchomione na końcu. Zdarzenia rozwijania występuje, gdy użytkownik powiększa odtwarzacza wideo do pełnego ekranu. Clickthroughs są określane za pomocą **<ClickThrough>** element **<VideoClicks>** element i określić identyfikatora URI do zasobu, aby wyświetlić, gdy użytkownik kliknie ad. ClickTracking jest określona w **<ClickTracking>** elementu, także w **<VideoClicks>** element i określić zasobu śledzenia odtwarzacza żądania, gdy użytkownik kliknie ad. **<MediaFile>** Elementów Określ informacje dotyczące określonego kodowania ad. Jeśli istnieje więcej niż jedna **<MediaFile>** element odtwarzacza wideo można wybrać najlepsze kodowanie platformy. 

Liniowy reklam mogą być wyświetlane w określony sposób. W tym celu należy dodać kolejne <Ad> elementy, aby VAST plik i określić kolejność przy użyciu atrybutu sekwencji. Poniższy przykład przedstawia się następująco:
    
    <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="1" sequence="0">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad1trackingResouce]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
      <Ad id="2" sequence="1">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad2trackingResouce]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:30</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
    </VAST>
    
Nieliniowa reklam są określane w <Creative> także element. W poniższym przykładzie pokazano <Creative> element, który opisuje nieliniowa ad.

    <Creative id="video" sequence="1" AdID="">
      <NonLinearAds>
        <NonLinear width="216" height="121" minSuggestedDuration="00:00:15">
          <StaticResource creativeType="image/png"><![CDATA[http://myserver/images/image.png]]></StaticResource>
          <StaticResource creativeType="image/jpg"><![CDATA[http://myserver/images/image.jpg]]></StaticResource>
        </NonLinear>
        <TrackingEvents>
             <Tracking event="acceptInvitation"><![CDATA[http://myserver/tracking/trackingID]></Tracking>
             <Tracking event="collapse"><![CDATA[http://myserver/tracking/trackingID2]]></Tracking>
         </TrackingEvents>
       </NonLinearAds>
    </Creative>

 
**<NonLinearAds>** Element może zawierać jedną lub więcej **<NonLinear>** elementów, z których każdy będzie można opisać nieliniowa ad. **<NonLinear>** Element określa zasobowi nieliniowa ad. Zasób może być **<StaticResouce>**, **<IFrameResource>**, lub **<HTMLResouce>**.**<StaticResource>** w tym artykule opisano zasób nie HTML i Określa atrybut creativeType, który określa sposób wyświetlania tego zasobu:

Obraz gif, obraz jpeg, obraz png — zasobu jest wyświetlany w formacie HTML **<img>** znacznik.

Aplikacja/x-javascript — zasobu jest wyświetlany w tag HTML <**script**>.

Aplikacja/x-shockwave-flash — zasobu jest wyświetlany w programu Flash player.

**<IFrameResource>**w tym artykule opisano zasobu HTML, która może być wyświetlana w ramce IFrame. **<HTMLResource>**w tym artykule opisano fragment kodu HTML, który można wstawić na stronę sieci web. **<TrackingEvents>**Określ zdarzeń śledzenia i identyfikator URI żądania po wystąpieniu zdarzenia. W tym przykładzie zdarzenia acceptInvitation i zwijania są śledzone. Aby uzyskać więcej informacji na temat **<NonLinearAds>** element i jej elementów podrzędnych, zobacz IAB.NET/VAST. Należy zauważyć, że **<TrackingEvents>** element jest umieszczony w** <NonLinearAds> ** elementu zamiast **<NonLinear>** elementu.

Pomocnik reklam są zdefiniowane w <CompanionAds> elementu. <CompanionAds> Element może zawierać jedną lub więcej <Companion> elementy. Każdy <Companion> element w tym artykule opisano ad pomocniczym i mogą zawierać <StaticResource>, <IFrameResource>, lub <HTMLResource> ujętych w taki sam sposób jak nieliniowa ad. OGROMNĄ plik może zawierać wiele reklam pomocniczym, a aplikacja odtwarzacza możesz wybrać bardziej odpowiednia ad do wyświetlenia. Aby uzyskać więcej informacji o VAST zobacz [OGROMNĄ 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).

###<a name="using-a-digital-video-multiple-ad-playlist-vmap-file"></a>Przy użyciu wielu Ad (VMAP) odtwarzania wideo

Plik VMAP umożliwia określenie, kiedy występują przerwy ad, jak długo trwa każdym podziale ile reklam mogą być wyświetlane w podziału i jakie typy reklam może być wyświetlany podczas podziału. Poniższy przykładowy plik VMAP, który definiuje podziału pojedynczego ad:
    
    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
              <Ad id="115571748">
                <InLine>
                  <AdSystem version="2.0 alpha">Atlas</AdSystem>
                  <AdTitle>Unknown</AdTitle>
                  <Description>Unknown</Description>
                  <Survey></Survey>
                  <Error></Error>
                  <Impression id="Atlas"><![CDATA[http://view.atdmt.com/000/sview/115571748/direct;ai.201582527;vt.2/01/634364885739970673]]></Impression>
                  <Creatives>
                    <Creative id="video" sequence="0" AdID="">
                      <Linear>
                        <Duration>00:00:32</Duration>
                        <MediaFiles>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_200_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_300_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_500" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="500" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_500_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_700" maintainAspectRatio="true" scaleable="true" delivery="progressive" bitrate="700" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv]]>
                          </MediaFile>
                        </MediaFiles>
                      </Linear>
                    </Creative>
                  </Creatives>
                </InLine>
              </Ad>
            </VAST>
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>
     
Plik VMAP zaczyna się od <VMAP> element, który zawiera co najmniej jeden <AdBreak> elementów, definiując podział ad. Każdym podziale ad Określa typ podziału, identyfikator podział i przesunięcie czasowe. Atrybut breakType Określa typ ad, które można odtwarzać w podziale: liniowy, nieliniowa, lub wyświetlanie. Wyświetlanie map reklam reklam OGROMNĄ pomocniczym. Można określić więcej niż jeden typ ad na liście przecinkami (bez spacji). BreakID jest opcjonalny identyfikator ad. TimeOffset umożliwia określenie, kiedy mają być wyświetlane ad. Można je określić w jednym z następujących sposobów:

1. Godzina w formacie gg lub GG:mm:ss.mmm, gdzie .mmm jest milisekund. Wartość tego atrybutu określa czas od początku wideo oś czasu na początek podział ad.
1. Wartość procentową — w formacie n % gdzie n jest wartością procentową wideo osi czasu do odtwarzania, aby odtworzyć ad
1. Początek/koniec — Określa, że ad powinien być wyświetlany przed lub po została wyświetlona klipu wideo
1. Umieść — określa kolejność podziałów ad, gdy chronometrażu podziałów ad jest nieznany, tak jak live streaming. Kolejność każdym podziale ad jest określony w formacie #n, gdzie n jest liczbą całkowitą, 1 lub większy. 1 oznacza ad powinny być odtwarzane przy okazji pierwszego 2 oznacza ad powinny być odtwarzane przy okazji drugiego i tak dalej.

W elemencie <**AdBreak**> można jeden element <**AdSource**>. Element <**AdSource**> zawiera następujące atrybuty:

1. Identyfikator — Określa identyfikator źródła ad
1. allowMultipleAds — wartość logiczną, która określa, czy wiele reklam mogą być wyświetlane podczas podziału ad
1. followRedirects — opcjonalnie wartość logiczna określająca, jeśli odtwarzacza wideo należy przestrzegać przekierowuje w odpowiedzi ad

Element <**AdSource**> udostępnia odtwarzacza odpowiedzią ad w tekście lub odwołanie do odpowiedzi ad. Nazwa może zawierać jedną z następujących elementów:

- <VASTAdData>Wskazuje, że odpowiedzi OGROMNĄ ad jest osadzony w pliku VMAP
- <AdTagURI>Identyfikator URI, który odwołuje się do odpowiedzi ad z innego systemu
- <CustomAdData>-dowolnego ciągu tego respresents-OGROMNĄ odpowiedzi

W tym przykładzie zostanie użyty odpowiedź w tekście ad <VASTAdData> element, który zawiera odpowiedzi na OGROMNĄ ad. Aby uzyskać więcej informacji o innych elementów zobacz [VMAP](http://www.iab.net/guidelines/508676/digitalvideo/vsuite/vmap).

Element <**AdBreak**> może także zawierać jeden element <**TrackingEvents**>. Element <**TrackingEvents**> umożliwia śledzenie początek lub koniec podział ad lub czy wystąpił błąd podczas podziału ad. Element <**TrackingEvents**> zawiera co najmniej jeden <**Śledzenie**> elementów, z których każdy określa zdarzenie śledzenia i śledzeniem identyfikatora URI. Śledzenie możliwe zdarzenia, które:

1. breakStart — śledzi na początek podział ad
1. breakEnd — ukończenie podziału ad ścieżki
1. Błąd — śledzi komunikat o błędzie, który wystąpił podczas podziału ad

W poniższym przykładzie pokazano pliku VMAP, który określa zdarzeń śledzenia

    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <!--Inline VAST -->
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
          <vmap:Tracking event="breakend">
            http://MyServer.com/breakend.gif
          </vmap:Tracking>
          <vmap:Tracking event="error">
            http://MyServer.com/error.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>

Aby uzyskać więcej informacji o elemencie <**TrackingEvents**> i jej elementów podrzędnych zobacz http://iab.org/VMAP.pdf

###<a name="using-a-media-abstract-sequencing-template-mast-file"></a>Za pomocą abstrakcyjne multimediów, harmonogramu plik szablonu (KAT)

Plik kat umożliwia określenie wyzwalaczy, które określają, kiedy wyświetlane jest ad. Poniżej przedstawiono przykładowy plik kat, który zawiera wyzwalaczy ad rzut litery pre, ad połowie listy płac i ad po wprowadzeniu poprawek.

    <MAST xsi:schemaLocation="http://openvideoplayer.sf.net/mast http://openvideoplayer.sf.net/mast/mast.xsd" xmlns="http://openvideoplayer.sf.net/mast" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <triggers>
        <trigger id="preroll" description="preroll every item"  >
          <startConditions>
            <condition type="event" name="OnItemStart" />
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
    
        <trigger id="midroll" description="midroll at 15 sec."  >
          <startConditions>
            <condition type="property" name="Position" value="00:00:15.0" operator="GEQ" />
          </startConditions>
          <endConditions>
            <condition type="event" name="OnItemEnd"/>
            <!--This 'resets' the trigger for the next clip-->
          </endConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
    
        <trigger id="postroll" description="postroll"  >
          <startConditions>
            <condition type="event" name="OnItemEnd"/>
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
      </triggers>
    </MAST>

 

Plik kat zaczyna się od **<MAST>** element, który zawiera jedną **<triggers>** elementu. <triggers> Element zawiera co najmniej jedną **<trigger>** elementów, które określają, kiedy można odtwarzać ad. 

**<trigger>** Element zawiera **<startConditions>** element, który Określ rozpoczęcia ad odtwarzania. **<startConditions>** Element zawiera co najmniej jedną <condition> elementy. Po każdym <condition> wartość PRAWDA wyzwalacza została zainicjowana lub odwołany, w zależności od tego, czy <condition> znajduje się w **< startConditions**> lub **<endConditions>** element odpowiednio. Gdy wielu <condition> pierwiastki, są traktowane jako niejawne lub dowolny z warunków oceny PRAWDA spowoduje, że wyzwalacza zainicjować. <condition>elementy można zagnieżdżać. Gdy podrzędne <condition> elementy są widoczne, są traktowane jako niejawne i wszystkie warunki musi być wartość PRAWDA zainicjować wyzwalacza. <condition> Element zawiera następujące atrybuty, które definiują warunek: 

1. **Typ** — Określa typ warunku, zdarzenia lub właściwości
1. **Nazwa** — nazwa właściwości lub zdarzenia, które ma być używane podczas obliczania
1. **wartość** — wartość właściwość będzie obliczane pod kątem zgodności
1. **operator** — operacji przy użyciu podczas obliczania: EQ (równości), NEQ (nie równa), GTR (znak), GEQ (większe lub równe), LT (mniejsze niż), LEQ (mniejsze niż lub równe), MOD (modulo)

**<endConditions>**zawierają również <condition> elementy. Kiedy warunek zwraca wartość true wyzwalacz powoduje zresetowanie. <trigger> Element zawiera również <sources> element, który zawiera co najmniej jeden <source> elementy. <source> Elementy zdefiniować identyfikator URI odpowiedź ad i typ odpowiedzi ad. W tym przykładzie identyfikator URI otrzymuje OGROMNĄ odpowiedź. 


    <trigger id="postroll" description="postroll"  >
      <startConditions>
        <condition/>
      </startConditions>
      <sources>
        <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
          <sources />
        </source>
      </sources>
    </trigger>
 

###<a name="using-video-player-ad-interface-definition-vpaid"></a>Za pomocą definicji interfejsu Ad odtwarzacza wideo (VPAID)

VPAID jest interfejs API umożliwiających jednostki wykonywalny ad można komunikować się z odtwarzacza wideo. Dzięki temu ad wysoce interakcyjne środowiska. Użytkownik może korzystać z usługą Active Directory i ad może reagować na akcje wykonywane przez Podgląd. Na przykład ad mogą być wyświetlane przyciski, które umożliwiają użytkownikom wyświetlanie więcej informacji lub dłuższa wersja ad. Odtwarzacza wideo musi obsługiwać interfejs API VPAID i zaimplementować API wykonywalny ad. Gdy odtwarzaczu żądania ad z serwera ad serwer będzie działać z SZEROKĄ odpowiedź, która zawiera VPAID ad.

Plik wykonywalny ad jest tworzona w kodzie, które muszą zostać wykonane w środowisku środowisko uruchomieniowe, takie jak Adobe Flash™ lub kodu JavaScript, które mogą być wykonywane w przeglądarce sieci web. Gdy serwer usług ad zwraca OGROMNĄ odpowiedź zawierającą VPAID ad, wartość apiFramework atrybutów w <MediaFile> element musi być "VPAID". Ten atrybut określa zawartych ad ad wykonywalny VPAID. Atrybut typu musi być równa typu MIME plik wykonywalny, takich jak "application/x-shockwave-flash" lub "aplikacji x-javascript". Poniższej wstawki kodu XML <MediaFile> element z SZEROKĄ odpowiedź zawierającą ad wykonywalny VPAID. 

    
    <MediaFiles>
       <MediaFile id="1" delivery="progressive" type=”application/x-shockwaveflash”
                  width=”640” height=”480” apiFramework=”VPAID”>
           <!-- CDATA wrapped URI to executable ad -->
       </MediaFile>
    </MediaFiles>
 

Wykonywalny ad mogą być inicjowane za pomocą <AdParameters> element <Linear> lub <NonLinear> elementy w OGROMNĄ odpowiedzi. Aby uzyskać więcej informacji na temat <AdParameters> elementu, zobacz [OGROMNĄ 3.0](http://www.iab.net/media/file/VASTv3.0.pdf). Aby uzyskać więcej informacji na temat interfejsu API VPAID zobacz [VPAID 2.0](http://www.iab.net/media/file/VPAID_2.0_Final_04-10-2012.pdf).

##<a name="implementing-a-windows-or-windows-phone-8-player-with-ad-support"></a>Implementacji z systemem Windows lub Windows Phone 8 odtwarzacza z obsługą Ad

Platforma Microsoft Media: Framework Player dla systemu Windows 8 i Windows Phone 8 zawiera zbiór przykładowych aplikacji, pokazujące, jak wdrażać aplikację odtwarzacza wideo przy użyciu struktury. Struktura odtwarzacza i próbki można pobrać z [Framework Player dla systemu Windows 8 i Windows Phone 8](https://playerframework.codeplex.com).

Po otwarciu rozwiązanie Microsoft.PlayerFramework.Xaml.Samples pojawi się liczba folderów w projekcie. Folder reklamami zawiera przykładowy kod istotne w przypadku tworzenia odtwarzacza wideo z obsługą ad. Wewnątrz reklamę folder jest liczbą XAML/cs plików, których pokazująca, jak wstawić reklam w inny sposób. Poniższa lista zawiera opis poszczególnych:

- AdPodPage.xaml przedstawia sposób wyświetlania pod ad.
- AdSchedulingPage.xaml pokazano, jak zaplanować reklam.
- FreeWheelPage.xaml pokazano, jak za pomocą wtyczki FreeWheel planowanie reklam.
- MastPage.xaml pokazano, jak zaplanować reklam z plikiem kat.
- ProgrammaticAdPage.xaml pokazano, jak planować programowy reklam w plik wideo.
- ScheduleClipPage.xaml pokazano, jak zaplanować ad bez OGROMNĄ pliku.
- VastLinearCompanionPage.xaml pokazano, jak wstawić liniowy i ad pomocniczym.
- VastNonLinearPage.xaml przedstawia sposób włożenia ad liniowego.
- VmapPage.xaml przedstawiono sposób określania reklam z plikiem VMAP.

Każdy z tych przykładów używa klasy MediaPlayer zdefiniowane w ramach odtwarzacza. Większość próbek za pomocą wtyczki, który dodać obsługę różnych formatów odpowiedzi ad. Przykładowe ProgrammaticAdPage programowy współdziała z wystąpieniem MediaPlayer.

###<a name="adpodpage-sample"></a>Przykładowe AdPodPage

W przykładzie użyto AdSchedulerPlugin, aby określić, kiedy będą wyświetlane ad. W tym przykładzie zaplanowano ogłoszeń połowie listy płac ma być odtwarzany po 5 sekundach. W pliku OGROMNĄ zwrócone przez serwer usług ad określono pod ad (grupa reklam, aby wyświetlić w kolejności). Identyfikator URI OGROMNĄ plik jest określonego w <RemoteAdSource> elementu.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
    
        <mmppf:MediaPlayer.Plugins>
            <ads:AdSchedulerPlugin>
                <ads:AdSchedulerPlugin.Advertisements>
    
                    <ads:MidrollAdvertisement Time="00:00:05">
                        <ads:MidrollAdvertisement.Source>
                            <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_adpod.xml" Type="vast"/>
                        </ads:MidrollAdvertisement.Source>
                    </ads:MidrollAdvertisement>
    
                </ads:AdSchedulerPlugin.Advertisements>
            </ads:AdSchedulerPlugin>
            <ads:AdHandlerPlugin/>
        </mmppf:MediaPlayer.Plugins>
    </mmppf:MediaPlayer>

Aby uzyskać więcej informacji na temat AdSchedulerPlugin zobacz [reklamami w ramach odtwarzacza w systemie Windows 8 i Windows Phone 8](http://playerframework.codeplex.com/wikipage?title=Advertising&referringTitle=Windows%208%20Player%20Documentation)

###<a name="adschedulingpage"></a>AdSchedulingPage

W tym przykładzie użyto również AdSchedulerPlugin. Planowana trzy ogłoszenia, ad przywracania starszych niż ad połowie listy płac i ad po wprowadzeniu poprawek. Identyfikator URI VAST dla każdego ad jest określona w <RemoteAdSource> elementu.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:PrerollAdvertisement>
                                <ads:PrerollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PrerollAdvertisement.Source>
                            </ads:PrerollAdvertisement>
    
                            <ads:MidrollAdvertisement Time="00:00:15">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
    
                            <ads:PostrollAdvertisement>
                                <ads:PostrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PostrollAdvertisement.Source>
                            </ads:PostrollAdvertisement>
    
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>


###<a name="freewheelpage"></a>FreeWheelPage

W tym przykładzie użyto FreeWheelPlugin, który określa atrybutu Source, który określa identyfikator URI, która kieruje do pliku SmartXML, który określa ad zawartości, a także informacje o planowaniu ad.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:FreeWheelPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/freewheel.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="mastpage"></a>MastPage

W tym przykładzie użyto MastSchedulerPlugin, która umożliwia używanie pliku kat. Atrybut źródła Określa lokalizację pliku kat.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:MastSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/mast.xml" />
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="programmaticadpage"></a>ProgrammaticAdPage

W tym przykładzie programowy współdziała z MediaPlayer. Plik ProgrammaticAdPage.xaml tworzy MediaPlayer:

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4"/>

Plik ProgrammaticAdPage.xaml.cs tworzy AdHandlerPlugin dodaje TimelineMarker Określ ad powinny być wyświetlane, a następnie dodaje obsługi dla zdarzenia MarkerReached, która ładuje RemoteAdSource, określając identyfikator URI OGROMNĄ pliku, a następnie odtworzyć ad.
    
    public sealed partial class ProgrammaticAdPage : Microsoft.PlayerFramework.Samples.Common.LayoutAwarePage
        {
            AdHandlerPlugin adHandler;
    
            public ProgrammaticAdPage()
            {
                this.InitializeComponent();
                adHandler = new AdHandlerPlugin();
                player.Plugins.Add(new AdHandlerPlugin());
                player.Markers.Add(new TimelineMarker() { Time = TimeSpan.FromSeconds(5), Type = "myAd" });
                player.MarkerReached += pf_MarkerReached;
            }
    
            async void pf_MarkerReached(object sender, TimelineMarkerRoutedEventArgs e)
            {
                if (e.Marker.Type == "myAd")
                {
                    var adSource = new RemoteAdSource() { Type = VastAdPayloadHandler.AdType, Uri = new Uri("http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml") };
                    //var adSource = new AdSource() { Type = DocumentAdPayloadHandler.AdType, Payload = SampleAdDocument };
                    var progress = new Progress<AdStatus>();
                    try
                    {
                        await player.PlayAd(adSource, progress, CancellationToken.None);
                    }
                    catch { /* ignore */ }
                }
            }

###<a name="scheduleclippage"></a>ScheduleClipPage

W tym przykładzie użyto AdSchedulerPlugin zaplanować ad połowie listy płac, określając plik wmv, który zawiera ad.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.cloudapp.net/html5/media/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:AdSource Type="clip">
                                        <ads:AdSource.Payload>
                                            <ads:ClipAdPayload MediaSource="http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv" MimeType="video/x-ms-wmv" />
                                        </ads:AdSource.Payload>
                                    </ads:AdSource>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
    
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="vastlinearcompanionpage"></a>VastLinearCompanionPage

Ten przykład przedstawia sposób umożliwia planowanie ad liniowej połowie listy płac, z usługą Active Directory companion AdSchedulerPlugin. <RemoteAdSource> Element określa lokalizację pliku OGROMNĄ.
    
    <mmppf:MediaPlayer Grid.Row="1"  x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_companions.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
    
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="vastlinearnonlinearpage"></a>VastLinearNonLinearPage

W przykładzie użyto AdSchedulerPlugin zaplanować liniowy i ad liniowego. Lokalizacja pliku OGROMNĄ jest określona przy użyciu <RemoteAdSource> elementu.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_nonlinear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
                            
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="vmappage"></a>VMAPPage

Ten próbki używa VmapSchedulerPlugin zaplanować AD przy użyciu pliku VMAP. Identyfikator URI pliku VMAP jest określona w atrybucie źródła <VmapSchedulerPlugin> elementu.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:VmapSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/vmap.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

##<a name="implementing-an-ios-video-player-with-ad-support"></a>Implementacji iOS odtwarzacza wideo z obsługą Ad


Platforma Microsoft Media: Framework Player dla systemu iOS zawiera zbiór przykładowych aplikacji, pokazujące, jak wdrażać aplikację odtwarzacza wideo przy użyciu struktury. Struktura odtwarzacza i próbki można pobrać z [Framework odtwarzacza multimediów Azure](https://github.com/Azure/azure-media-player-framework). Na stronie github znajduje się łącze do witryny typu Wiki, zawierającego dodatkowe informacje w ramach odtwarzacza i wprowadzenie do próbki odtwarzacza: [Azure Media Player stron typu Wiki](https://github.com/Azure/azure-media-player-framework/wiki/How-to-use-Azure-media-player-framework).


###<a name="scheduling-ads-with-vmap"></a>Planowanie reklam VMAP

W poniższym przykładzie pokazano, jak zaplanować AD przy użyciu pliku VMAP.

    // How to schedule an Ad using VMAP.
    //First download the VMAP manifest
    
    if (![framework.adResolver downloadManifest:&manifest withURL:[NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVMAP.xml"]])
            {
                [self logFrameworkError];
            }
            else
            {
                // Schedule a list of ads using the downloaded VMAP manifest
                if (![framework scheduleVMAPWithManifest:manifest])
                {
                    [self logFrameworkError];
                }          
            }

###<a name="scheduling-ads-with-vast"></a>Planowanie reklam VAST

Poniższy przykład pokazano, jak zaplanować opóźnione ad OGROMNĄ powiązanie.
    
    //Example:3 How to schedule a late binding VAST ad.
    // set the start time for the ad
    adLinearTime.startTime = 13;
    adLinearTime.duration = 0;
    // Specify the URI of the VAST file
    NSString *vastAd1=@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml";
    // Create an AdInfo object
     AdInfo *vastAdInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URL to VAST file
    vastAdInfo1.clipURL = [NSURL URLWithString:vastAd1];
    // set running time of ad
    vastAdInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    vastAdInfo1.mediaTime.clipBeginMediaTime = 0;
    vastAdInfo1.mediaTime.clipEndMediaTime = 10;
    vastAdInfo1.policy = @"policy for late binding VAST";
    // specify ad type
    vastAdInfo1.type = AdType_Midroll;
    vastAdInfo1.appendTo=-1;
    adIndex = 0;
    // schedule ad
    if (![framework scheduleClip:vastAdInfo1 atTime:adLinearTime forType:PlaylistEntryType_VAST andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }
         
   Poniższy przykład pokazano, jak zaplanować najwcześniejsze ad OGROMNĄ powiązanie.
Przykład: 4 harmonogram najwcześniejsze //Download OGROMNĄ ad powiązanie VAST plik, jeśli (! [ framework.adResolver downloadManifest: & manifestu withURL: [NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml"]]) {[logFrameworkError samodzielnie];} jeszcze {adLinearTime.startTime = 7; adLinearTime.duration = 0;
        
        // Create AdInfo instance
        AdInfo *vastAdInfo2 = [[[AdInfo alloc] init] autorelease];
        vastAdInfo2.mediaTime = [[[MediaTime alloc] init] autorelease];
        vastAdInfo2.policy = @"policy for early binding VAST";
        // specify ad type
        vastAdInfo2.type = AdType_Midroll;
        vastAdInfo2.appendTo=-1;
        // schedule ad
        if (![framework scheduleVASTClip:vastAdInfo2 withManifest:manifest atTime:adLinearTime andGetClipId:&adIndex])
        {
            [self logFrameworkError];
        }
    }

Poniższy przykład przedstawia sposób włożenia ad przy użyciu drukowania Wycinanie do edycji (RCE)

    //Example:1 How to use RCE.
    // specify manifest for ad content
    NSString *secondContent=@"http://wamsblureg001orig-hs.cloudapp.net/6651424c-a9d1-419b-895c-6993f0f48a26/The%20making%20of%20Microsoft%20Surface-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    
    // specify ad length
    mediaTime.currentPlaybackPosition = 0;
    mediaTime.clipBeginMediaTime = 0;
    mediaTime.clipEndMediaTime = 80;
    // append ad content
    if (![framework appendContentClip:[NSURL URLWithString:secondContent] withMediaTime:mediaTime andGetClipId:&clipId])
    {
        [self logFrameworkError];
    }

W poniższym przykładzie pokazano, jak zaplanować pod ad.

    //Example:5 Schedule an ad Pod.
    // Set start time for ad
    adLinearTime.startTime = 23;
    adLinearTime.duration = 0;
    
    // Specify URL to content
    NSString *adpodSt1=@"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    // Create an AdInfo instance
    AdInfo *adpodInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URI to ad content
    adpodInfo1.clipURL = [NSURL URLWithString:adpodSt1];
    // Set ad running time
    adpodInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    adpodInfo1.mediaTime.clipBeginMediaTime = 0;
    adpodInfo1.mediaTime.clipEndMediaTime = 17;
    adpodInfo1.policy = @"policy for ad pod 1";
    // Set ad type
    adpodInfo1.type = AdType_Midroll;
    adpodInfo1.appendTo=-1;
    adIndex = 0;
    // Schedule ad
    if (![framework scheduleClip:adpodInfo1 atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

W poniższym przykładzie pokazano, jak zaplanować-lepkich ad połowie listy płac. Lepkich ad tylko jest odtwarzany, gdy bez względu na dowolnym znalezienia wykonuje przeglądarki.
    
    //Example:6 Schedule a single non sticky mid roll Ad
    // specify URL to content
    NSString *oneTimeAd=@"http://wamsblureg001orig-hs.cloudapp.net/5389c0c5-340f-48d7-90bc-0aab664e5f02/Windows%208_%20You%20and%20Me%20Together-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    
    // create an AdInfo instance
    AdInfo *oneTimeInfo = [[[AdInfo alloc] init] autorelease];
    // set URL of ad
    oneTimeInfo.clipURL = [NSURL URLWithString:oneTimeAd];
    oneTimeInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    oneTimeInfo.mediaTime.clipBeginMediaTime = 0;
    oneTimeInfo.mediaTime.clipEndMediaTime = -1;
    oneTimeInfo.policy = @"policy for one-time ad";
    // set ad start time
    adLinearTime.startTime = 43;
    adLinearTime.duration = 0;
    // set ad type
    oneTimeInfo.type = AdType_Midroll;
    // non sticky ad
    oneTimeInfo.deleteAfterPlayed = YES;
    // schedule ad
    if (![framework scheduleClip:oneTimeInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

W poniższym przykładzie pokazano, jak zaplanować lepkich ad połowie listy płac. Lepkich ad będą wyświetlane zawsze, gdy osiągnie określony punkt w wideo na osi czasu.

    //Example:7 Schedule a single sticky mid roll Ad
    NSString *stickyAd=@"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *stickyAdInfo = [[[AdInfo alloc] init] autorelease];
    // set URI to ad
    stickyAdInfo.clipURL = [NSURL URLWithString:stickyAd];
    stickyAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    stickyAdInfo.mediaTime.clipBeginMediaTime = 0;
    stickyAdInfo.mediaTime.clipEndMediaTime = 15;
    stickyAdInfo.policy = @"policy for sticky mid-roll ad";
    // set ad start time
    adLinearTime.startTime = 64;
    adLinearTime.duration = 0;
    // set ad type
    stickyAdInfo.type = AdType_Midroll;
    stickyAdInfo.deleteAfterPlayed = NO;
    // schedule ad
    if (![framework scheduleClip:stickyAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }


Następujący przykład przedstawia sposób tworzenia harmonogramu ad po wprowadzeniu poprawek.

    //Example:8 Schedule Post Roll Ad
    NSString *postAdURLString=@"http://wamsblureg001orig-hs.cloudapp.net/aa152d7f-3c54-487b-ba07-a58e0e33280b/wp-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *postAdInfo = [[[AdInfo alloc] init] autorelease];
    postAdInfo.clipURL = [NSURL URLWithString:postAdURLString];
    postAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    postAdInfo.mediaTime.clipBeginMediaTime = 0;
    // set ad length
    postAdInfo.mediaTime.clipEndMediaTime = 45;
    postAdInfo.policy = @"policy for post-roll ad";
    // set ad type
    postAdInfo.type = AdType_Postroll;
    adLinearTime.duration = 0;
    if (![framework scheduleClip:postAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Poniższy przykład pokazano, jak zaplanować ad przed poprawek.
    
    //Example:9 Schedule Pre Roll Ad
    NSString *adURLString = @"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 40; //You could play a portion of an Ad. Yeh!
    adInfo.mediaTime.clipEndMediaTime = 59;
    adInfo.policy = @"policy for pre-roll ad";
    adInfo.appendTo = -1;
    adInfo.type = AdType_Preroll;
    adLinearTime.duration = 0;
    // schedule ad
    if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Poniższy przykład pokazano, jak zaplanować ad nakładki połowie listy płac.
    
    // Example10: Schedule a Mid Roll overlay Ad
    NSString *adURLString = @"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    //Create AdInfo instance
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 0;
    // specify ad length
    adInfo.mediaTime.clipEndMediaTime = 20;
    adInfo.policy = @"policy for mid-roll overlay ad";
    adInfo.appendTo = -1;
    // specify ad type
    adInfo.type = AdType_Midroll;
    // specify ad start time & duration
    adLinearTime.startTime = 300;
    adLinearTime.duration = 20;
    // schedule ad            if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }



##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

 
##<a name="see-also"></a>Zobacz też

[Można opracowywać aplikacje odtwarzacza wideo](media-services-develop-video-players.md)
