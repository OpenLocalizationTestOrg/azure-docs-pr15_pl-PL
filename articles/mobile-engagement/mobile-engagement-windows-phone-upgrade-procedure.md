<properties 
    pageTitle="Windows Phone Silverlight SDK procedury uaktualniania" 
    description="Windows Phone Silverlight SDK procedury uaktualniania dla Azure zaangażowania urządzeń przenośnych"                  
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede"
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-sdk-upgrade-procedures"></a>Windows Phone Silverlight SDK procedury uaktualniania

Jeśli już masz zintegrowany ze starszej wersji programu naszych SDK do aplikacji, należy wziąć pod uwagę następujące punkty podczas uaktualniania zestawu SDK.

Być może trzeba wykonać kilka procedury nie odebrano kilka wersji pakietu. Na przykład możesz przeprowadzić migrację z 0.10.1 do 0.11.0, musisz najpierw należy wykonać procedurę "od 0.9.0 do 0.10.1" następnie procedury "od 0.10.1 do 0.11.0".

##<a name="from-200-to-330"></a>Z 2.0.0 do 3.3.0

### <a name="test-logs"></a>Testowanie dzienników

Dzienniki konsoli tworzone przez zestawu SDK można teraz włączone/wyłączone i filtrowane. Aby dostosować tak, zaktualizuj właściwość `EngagementAgent.Instance.TestLogEnabled` do jednej z wartości dostępne na `EngagementTestLogLevel` wyliczenie, na przykład:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

##<a name="from-111-to-200"></a>Z 1.1.1 do 2.0.0

Poniżej opisano, jak przeprowadzić migrację SDK integracji z usługi Capptain oferowanych przez skojarzenia zabezpieczeń Capptain do aplikacji korzystająca z zaangażowania Mobile Azure. 

> [Azure.IMPORTANT] Capptain i zaangażowania Mobile nie są takie same usługi i procedury przedstawionej poniżej wyróżnienie tylko jak przeprowadzić migrację aplikacji klienta. Migrowanie SDK w aplikacji będą migrowane dane z serwerów Capptain się z serwerami zaangażowania Mobile

Jeśli są migrowane z wcześniejszej wersji, zajrzyj do witryny sieci web Capptain, aby przeprowadzić migrację do 1.1.1, a następnie Zastosuj poniższą procedurę

### <a name="nuget-package"></a>Pakiet Nuget

Zamień **Capptain.WindowsPhone** przez pakiet Nuget **MicrosoftAzure.MobileEngagement** .

### <a name="applying-mobile-engagement"></a>Stosowanie zaangażowania urządzeń przenośnych

Termin korzysta z zestawu SDK `Engagement`. Musisz zaktualizować projektu zgodnie z tej zmiany.

Musisz odinstalować pakietu nuget Capptain bieżącego. Weź pod uwagę, że zostaną usunięte wszystkie zmiany w folderze Capptain zasobów. Jeśli chcesz zachować te pliki, a następnie utwórz kopię.

Po wykonaniu tej instalowanie nowego pakietu Microsoft Azure zaangażowania w nuget nad projektem. Możesz go znaleźć bezpośrednio na [Nuget](http://www.nuget.org/packages/MicrosoftAzure.MobileEngagement). Ta akcja powoduje zastąpienie wszystkich plików zasobów używanych przez zaangażowania i dodaje nowe DLL zaangażowania do odwołania projektu.

Należy wyczyścić odwołania projektu, usuwając Capptain DLL odwołania. Jeśli nie wprowadzisz to, spowoduje konflikt wersji Capptain i nastąpi błędy.

Jeśli dostosowanych Capptain zasobów, skopiuj zawartość starych plików i wklej je do nowych plików zaangażowania. Należy pamiętać, że zarówno xaml cs pliki i trzeba będzie aktualizowany.

Po wykonaniu tych kroków wystarczy zamienić stary Capptain odwołuje się nowych odwołań zaangażowania.

1. Wszystkie przestrzenie nazw Capptain muszą być zaktualizowane.

    Przed migracją:
    
        using Capptain.Agent;
        using Capptain.Reach;
    
    Po zakończeniu migracji:
    
        using Microsoft.Azure.Engagement;

2. Wszystkie klasy Capptain, które zawierają "Capptain" powinien zawierać "Zaangażowania".

    Przed migracją:
    
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
    
    Po zakończeniu migracji:
    
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }

3. Pliki xaml Capptain nazw i atrybuty również zmienić.

    Przed migracją:
    
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
    
    Po zakończeniu migracji:
    
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>

4. Dla innych zasobów jak Capptain obrazy należy pamiętać, że one także zmieniono umożliwia "Zaangażowania".

### <a name="application-id--sdk-key"></a>Identyfikator aplikacji / klucz SDK

Zaangażowania używa parametrów połączenia. Nie musisz określić identyfikator aplikacji i klawisza SDK zaangażowania Mobile, wystarczy, że Określ parametry połączenia. Możesz skonfigurować go na plik EngagementConfiguration.

Konfiguracja zaangażowania można ustawić w swojej `Resources\EngagementConfiguration.xml` pliku projektu.

Edytowanie tego pliku, aby określić:

-   Ciąg połączenia aplikacji między znacznikami `<connectionString>` i `<\connectionString>`.

Jeśli chcesz określić je w czasie rzeczywistym, możesz zadzwonić poniższej metody przed inicjowanie agenta zaangażowania:

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        
        /* Initialize Engagement angent with above configuration. */
        EngagementAgent.Instance.Init(engagementConfiguration);

Parametry połączenia dla aplikacji jest wyświetlana w portalu klasyczny Azure.

### <a name="items-name-change"></a>Zmiana nazwy elementów

Wszystkie elementy o nazwie *capptain* zostały nazwy *zaangażowania*. Podobnie do *Capptain* *zaangażowania*.

Przykłady często używanych elementów Capptain:

-   CapptainConfiguration teraz nazwę EngagementConfiguration
-   CapptainAgent teraz nazwę EngagementAgent
-   CapptainReach teraz nazwę EngagementReach
-   CapptainHttpConfig teraz nazwę EngagementHttpConfig
-   GetCapptainPageName teraz nazwę GetEngagementPageName

Należy zauważyć, że również zmienić wpływa na zastąpić metod.



 
