<properties
    pageTitle="Samouczki za pomocą usługi REST Bus przekazywanie wiadomości | Microsoft Azure"
    description="Tworzenie prostej aplikacji do hosta przekazywania Bus usługi, która udostępnia pozostałych interfejsem."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/27/2016"
    ms.author="sethm" />

# <a name="service-bus-relay-rest-tutorial"></a>Samouczek pozostałych przekazywania Bus usługi

Ten samouczek w tym artykule opisano sposób tworzenia prostej aplikacji hosta usługi Bus pokazujących pozostałych interfejsem. Umieść umożliwia klienta sieci web, takich jak przeglądarka sieci web uzyskać dostęp do interfejsów API usługi Bus za pośrednictwem żądania HTTP.

Samouczku pozostałych Windows Communication Foundation (WCF) modelu programowania do utworzenia usługi REST na Bus usługi. Aby uzyskać więcej informacji zobacz [Model programowania pozostałych WCF](https://msdn.microsoft.com/library/bb412169.aspx) i [Projektowanie i implementowania usług](https://msdn.microsoft.com/library/ms729746.aspx) w dokumentacji WCF.

## <a name="step-1-create-a-service-namespace"></a>Krok 1: Tworzenie nazw usługi

Pierwszym krokiem jest utworzenie przestrzeń nazw, aby uzyskać klucz udostępniony podpis programu Access (SA). Przestrzeń nazw zawiera granic aplikacji dla każdej aplikacji dostępne za pośrednictwem usługi Bus. Klucz skojarzeń zabezpieczeń jest generowane automatycznie przez system podczas tworzenia nazw usługi. Połączenie usługi nazw i klucza skojarzeń zabezpieczeń zawiera poświadczenia dla usługi Bus do uwierzytelnienia dostęp do aplikacji.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="step-2-define-a-rest-based-wcf-service-contract-to-use-with-service-bus"></a>Krok 2: Definiowanie umowy serwisowej WCF opartych na pozostałych za pomocą usługi Bus

Jako z innymi usługami usługi Bus, podczas tworzenia usługi REST stylu, należy zdefiniować zamówienia. Umowa określa, jakie operacje obsługuje hosta. Operacja usługi można traktować jako metody usługi sieci web. Umowy są tworzone przez zdefiniowanie interfejsu C++, C# i Visual Basic. Każda z metod w interfejsie odpowiada operacji określonej usługi. Atrybut [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) muszą być stosowane do każdego interfejsu, a atrybut [Atrybut OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) muszą być stosowane do każdej operacji. Jeśli metoda w interfejsie zawierającą [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) nie ma [Atrybut OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx), ta metoda nie jest dostępna. Kod używany dla tych zadań są wyświetlane w tym przykładzie zgodnie z procedurą.

Podstawowa różnica pomiędzy podstawowe umowy Bus usługi i umowy pozostałych styl jest właściwość do [Atrybut OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx): [obiektu WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx). Ta właściwość umożliwia mapowanie metody w interfejsie użytkownika do metody po drugiej stronie interfejsu. W tym przypadku użyjemy [obiektu WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx) utworzyć łącze metody HTTP GET. Dzięki temu Bus usługi dokładnie pobrać i interpretować polecenia wysyłane do interfejsu.

### <a name="to-create-a-service-bus-contract-with-an-interface"></a>Aby utworzyć umowy Bus usługi z interfejsem

1. Otwórz program Visual Studio jako administrator: kliknij prawym przyciskiem myszy program w **Start** menu, a następnie kliknij **polecenie Uruchom jako administrator**.

2. Utwórz nowy projekt aplikacji konsoli. Kliknij menu **plik** wybierz pozycję **Nowy**, a następnie wybierz **Projekt**. W oknie dialogowym **Nowy projekt** kliknij przycisk **Visual C#**, wybierz szablon **Aplikacji konsoli** i nadaj mu nazwę **ImageListener**. Użyj domyślnej **lokalizacji**. Kliknij przycisk **OK** , aby utworzyć projekt.

3. W przypadku C# projektu, tworzy Visual Studio `Program.cs` pliku. Ta klasa zawiera pusta `Main()` metody wymagane dla projektu aplikacji konsoli poprawnie.

4. Dodaj odwołania do Bus usługi i **System.ServiceModel.dll** do projektu, instalacji pakietu NuGet Bus usługi. Ten pakiet automatycznie dodaje odwołania do bibliotek Bus usługi, a także WCF **System.ServiceModel**. W oknie Eksplorator rozwiązań projektu **ImageListener** kliknij prawym przyciskiem myszy, a następnie kliknij **Zarządzanie pakietów NuGet**. Kliknij kartę **Przeglądaj** , a następnie wyszukaj `Microsoft Azure Service Bus`. Kliknij przycisk **Zainstaluj**i zaakceptuj warunki użytkowania.

5. Odwołanie do **System.ServiceModel.Web.dll** należy bezwzględnie dodać do projektu:

    . W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder **odwołania** w folderze projektu i kliknij przycisk **Dodaj odwołanie**.

    b. W oknie dialogowym **Dodawanie odwołania** kliknij kartę **Framework** po lewej stronie i w polu **Wyszukaj** , wpisz **System.ServiceModel.Web**. Zaznacz pole wyboru **System.ServiceModel.Web** , a następnie kliknij **przycisk OK**.

6. Dodaj następujący `using` instrukcji u góry pliku Plik Program.cs.

    ```
    using System.ServiceModel;
    using System.ServiceModel.Channels;
    using System.ServiceModel.Web;
    using System.IO;
    ```

    [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) jest nazw, która umożliwia programowy dostęp do podstawowych funkcji WCF. Usługa Bus używa wielu obiekty i atrybuty WCF do definiowania zamówień. Należy użyć tego obszaru nazw w większości aplikacji przekazywania Bus usługi. Podobnie [System.ServiceModel.Channels](https://msdn.microsoft.com/library/system.servicemodel.channels.aspx) ułatwia definiowanie kanał, który jest obiektem, za pomocą którego można komunikować się za pomocą przeglądarki sieci web klienta i Bus usługi. Na koniec [System.ServiceModel.Web](https://msdn.microsoft.com/library/system.servicemodel.web.aspx) zawiera typy, które umożliwiają tworzenie aplikacji sieci web.

7. Zmienianie nazwy `ImageListener` nazw w celu **Microsoft.ServiceBus.Samples**.

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        ...
    ```

8. Bezpośrednio po Definiowanie otwierający nawias klamrowy deklaracji nazw nowy interfejs o nazwie **IImageContract** i zastosować atrybut **ServiceContractAttribute** interfejsu o wartości `http://samples.microsoft.com/ServiceModel/Relay/`. Wartość nazw różni się od nazw używane w całym zakresie kodu. Wartość nazw jest używany jako unikatowy identyfikator dla tej Umowy, a powinien mieć informacje o wersji. Aby uzyskać więcej informacji zobacz [Usługi przechowywania wersji](http://go.microsoft.com/fwlink/?LinkID=180498). Wyraźnie określający obszar nazw zapobiega dodawane do nazwy kontraktu wartość domyślna przestrzeń nazw.

    ```
    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/RESTTutorial1")]
    public interface IImageContract
    {
    }
    ```

9. W ramach `IImageContract` interfejs, zadeklarować metody dla jednej operacji `IImageContract` umowy udostępnia w interfejsie i stosowanie `OperationContractAttribute` atrybutu metody, który chcesz udostępnić w ramach zamówienia publicznego Bus usługi.

    ```
    public interface IImageContract
    {
        [OperationContract]
        Stream GetImage();
    }
    ```

10. W atrybucie **OperationContract** dodać wartość **WebGet** .

    ```
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }
    ```

    To umożliwia Bus usługi rozsyłanie żądań HTTP GET do `GetImage`i aby przetłumaczyć zwracane wartości `GetImage` do odpowiedzi HTTP GETRESPONSE. W dalszej części samouczka będą używane przeglądarki sieci web, aby uzyskać dostęp do tej metody można użyć i aby wyświetlić obraz w przeglądarce.

11. Bezpośrednio po `IImageContract` definicji zadeklarować kanałem, które dziedziczą z obu `IImageContract` i `IClientChannel` interfejsów.

    ```
    public interface IImageChannel : IImageContract, IClientChannel { }
    ```

    Kanał jest obiektem WCF, za pomocą którego usługa i klienta przekazywania informacji ze sobą. Później utworzy kanału w aplikacji. Usługa Bus następnie używa tego kanału do przekazania żądania HTTP GET z przeglądarki do implementacji **GetImage** . Usługa Bus używa też kanał wziąć zwracana wartość **GetImage** i tłumaczenie go na GETRESPONSE HTTP dla przeglądarki klienta.

12. Z menu **Tworzenie** kliknij przycisk **Konstruuj rozwiązanie** pory potwierdzenie dokładności pracy.

### <a name="example"></a>Przykład

Poniższy kod zawiera podstawowe interfejs, który definiuje umowy Bus usługi.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="step-3-implement-a-rest-based-wcf-service-contract-to-use-service-bus"></a>Krok 3: Implementowanie umowy serwisowej oparte na pozostałych WCF umożliwia Bus usługi

Tworzenie usługi Bus usługi REST styl wymaga utworzenia zamówienia, która jest zdefiniowana przy użyciu interfejsu. Następnym krokiem jest interfejs. Ten proces obejmuje tworzeniu klasę o nazwie **ImageService** interfejsu **IImageContract** zdefiniowane przez użytkownika. Konfigurowanie interfejs za pomocą pliku App.config po zastosowaniu zamówienia. Plik konfiguracyjny zawiera informacje niezbędne dla aplikacji, takich jak nazwa usługi, nazwę zamówienia i typ protokołu używany do komunikowania się z usługi Bus. W tym przykładzie zgodnie z procedurą podaną znajduje się kod używany dla tych zadań.

Podobnie jak w przypadku powyższe czynności, jest bardzo mało różnica między realizacji kontraktu pozostałych stylu i podstawowe umowy Bus usługi.

### <a name="to-implement-a-rest-style-service-bus-contract"></a>Do wykonania umowy Bus usługi REST stylu

1. Utwórz nową klasę o nazwie **ImageService** bezpośrednio po definicji interfejsu **IImageContract** . Klasy **ImageService** zawiera interfejsu **IImageContract** .

    ```
    class ImageService : IImageContract
    {
    }
    ```
    Podobnie jak w innych implementacji interfejsu, można zaimplementować definicji w drugim pliku. Jednak dla tego samouczka wykonania pojawi się w tym samym pliku jako definicji interfejsu i `Main()` metody.

2. Zastosowanie atrybutu [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) do klasy **IImageService** , aby wskazać, że klasa jest realizacji umowy WCF.

    ```
    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
    }
    ```

    Jak już wspomniano, ten obszar nazw jest tradycyjnych nazw. Zamiast tego jest elementem architektury WCF, który identyfikuje zamówienia. Aby uzyskać więcej informacji zobacz temat [Nazwy danych](https://msdn.microsoft.com/library/ms731045.aspx) w dokumentacji WCF.

3. Dodaj obraz jpg do projektu.  

    To jest wyświetlane przez usługę w przeglądarce odbierania obrazu. Kliknij prawym przyciskiem myszy projektu, a następnie kliknij przycisk **Dodaj**. Kliknij **Istniejący element**. Przejdź do odpowiedniej jpg za pomocą okna dialogowego **Dodawanie istniejącego elementu** , a następnie kliknij przycisk **Dodaj**.

    Podczas dodawania pliku, upewnij się, że **Wszystkie pliki** jest zaznaczone na liście rozwijanej obok pozycji **nazwy pliku:** pola. Podczas przerabiania tego samouczka przyjęto założenie, że nazwa obrazu jest "obraz.jpg". Jeśli masz inny plik, musisz zmienić obraz lub zmienić swój kod, aby zadośćuczynienia.

4. Aby upewnić się, że uruchomionej usługi można znaleźć pliku obrazu, w **Eksploratorze rozwiązań** kliknij prawym przyciskiem myszy plik obrazu, a następnie kliknij polecenie **Właściwości**. W okienku **Właściwości** ustaw **Kopiuj do katalogu wyjściowego** do **skopiowania, jeśli nowszego**.

5. Dodawanie odwołania do zestawu **System.Drawing.dll** do projektu, a także dodać następujący skojarzone `using` instrukcji.  

    ```
    using System.Drawing;
    using System.Drawing.Imaging;
    using Microsoft.ServiceBus;
    using Microsoft.ServiceBus.Web;
    ```

6. W klasie **ImageService** Dodaj następujące konstruktora, który ładuje mapa bitowa i przygotowanie do wysyłania go do przeglądarki klienta.

    ```
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }
    }
    ```

7. Bezpośrednio po kodzie poprzedniego Dodaj następujące metody **GetImage** klasy **ImageService** , aby przywrócić wiadomość HTTP, zawierający obraz.

    ```
    public Stream GetImage()
    {
        MemoryStream stream = new MemoryStream();
        this.bitmap.Save(stream, ImageFormat.Jpeg);

        stream.Position = 0;
        WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

        return stream;
    }
    ```

    Ta implementacja używa **elementu MemoryStream** , aby pobrać obrazu i przygotowywanie go do strumieniowego przesyłania do przeglądarki. Jego uruchamiania położenie strumienia od zera, deklaruje przesyłanie strumieniowe zawartości w formacie jpeg i umożliwia strumieniowe przesyłanie danych.

8. Z menu **Tworzenie** kliknij przycisk **Konstruuj rozwiązanie**.

### <a name="to-define-the-configuration-for-running-the-web-service-on-service-bus"></a>Aby zdefiniować konfigurację do uruchamiania usługi sieci web na Bus usługi

1. W **Eksploratorze rozwiązań**kliknij dwukrotnie **App.config** , aby otworzyć go w Edytorze Visual Studio.

    Pliku **App.config** przypomina pliku konfiguracji WCF i zawiera nazwę usługi, punktu końcowego (czyli lokalizacji Bus Usługa udostępnia dla klientów i hosts komunikować się ze sobą) i wiązanie (typ protokół, który jest używany do komunikowania się). W tym miejscu głównym różnica polega na tym, że punktu końcowego usługi skonfigurowane odwołuje się do powiązania [WebHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.webhttprelaybinding.aspx) , który nie jest częścią programu .NET Framework.

2. `<system.serviceModel>` XML element jest elementem WCF, który definiuje co najmniej jednej usługi. W tym miejscu jest używana do zdefiniować nazwę usługi i punkt końcowy. W dolnej części `<system.serviceModel>` elementu (, ale nadal w `<system.serviceModel>`), Dodaj `<bindings>` elementu, który ma następującą zawartość. Definiuje powiązań używany w aplikacji. Można zdefiniować wielu powiązań, ale dla tego samouczka jest definiowana tylko jeden.

    ```
    <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
            <binding name="default">
                <security relayClientAuthenticationType="None" />
            </binding>
        </webHttpRelayBinding>
    </bindings>
    ```

    W tym kroku Określa powiązanie Bus usługi [WebHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.webhttprelaybinding.aspx) z **relayClientAuthenticationType** **Brak**. To ustawienie wskazuje, że punktu końcowego za pomocą tego powiązania nie wymaga poświadczeń klienta.

3. Po `<bindings>` elementu, Dodaj `<services>` elementu. Podobnie jak powiązań, można zdefiniować wiele usług w pliku jednego konfiguracji. Jednak dla tego samouczka można zdefiniować tylko jeden.

    ```
    <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
            <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IImageContract"
                    binding="webHttpRelayBinding"
                    bindingConfiguration="default"
                    behaviorConfiguration="sbTokenProvider"
                    address="" />
        </service>
    </services>
    ```

    W tym kroku konfiguruje usługa, która korzysta z uprzednio zdefiniowanej wartości domyślnej **webHttpRelayBinding**. Ponadto użyto **sbTokenProvider**domyślnych, która jest zdefiniowana w następnym kroku.

4. Po `<services>` elementu, Utwórz `<behaviors>` element o następującej treści, zamieniając "SAS_KEY" klucz *Udostępniony podpis programu Access* (SA) uzyskanego poprzednio z [Azure portal][].

    ```
    <behaviors>
        <endpointBehaviors>
            <behavior name="sbTokenProvider">
                <transportClientEndpointBehavior>
                    <tokenProvider>
                        <sharedAccessSignature keyName="RootManageSharedAccessKey" key="SAS_KEY" />
                    </tokenProvider>
                </transportClientEndpointBehavior>
            </behavior>
            </endpointBehaviors>
            <serviceBehaviors>
                <behavior name="default">
                    <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
                </behavior>
            </serviceBehaviors>
    </behaviors>
    ```

5. Nadal w App.config, w `<appSettings>` element, Zamień wartość ciągu całe połączenie przy użyciu parametrów połączenia uzyskanego poprzednio z portalu. 

    ```
    <appSettings>
    <!-- Service Bus specific app settings for messaging connections -->
    <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```

6. Z menu **Tworzenie** kliknij przycisk **Konstruuj rozwiązanie** całego rozwiązania.

### <a name="example"></a>Przykład

Poniższy kod zawiera umowy i usługi wdrażania usługi opartej na pozostałych, na którym działa usługa Bus za pomocą tego powiązania **WebHttpRelayBinding** .

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{


    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

W poniższym przykładzie pokazano pliku App.config skojarzone z usługą.

```
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2"/>
    </startup>
    <system.serviceModel>
        <extensions>
            <!-- In this extension section we are introducing all known service bus extensions. User can remove the ones they don't need. -->
            <behaviorExtensions>
                <add name="connectionStatusBehavior"
                    type="Microsoft.ServiceBus.Configuration.ConnectionStatusElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="transportClientEndpointBehavior"
                    type="Microsoft.ServiceBus.Configuration.TransportClientEndpointBehaviorElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="serviceRegistrySettings"
                    type="Microsoft.ServiceBus.Configuration.ServiceRegistrySettingsElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </behaviorExtensions>
            <bindingElementExtensions>
                <add name="netMessagingTransport"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingTransportExtensionElement, Microsoft.ServiceBus,  Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="tcpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.TcpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpsRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpsRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="onewayRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.RelayedOnewayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingElementExtensions>
            <bindingExtensions>
                <add name="basicHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.BasicHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="webHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WebHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="ws2007HttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WS2007HttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netOnewayRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetOnewayRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netEventRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetEventRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netMessagingBinding"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingExtensions>
        </extensions>
      <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
          <binding name="default">
            <security relayClientAuthenticationType="None" />
          </binding>
        </webHttpRelayBinding>
      </bindings>
      <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
          <endpoint name="RelayEndpoint"
                  contract="Microsoft.ServiceBus.Samples.IImageContract"
                  binding="webHttpRelayBinding"
                  bindingConfiguration="default"
                  behaviorConfiguration="sbTokenProvider"
                  address="" />
        </service>
      </services>
      <behaviors>
        <endpointBehaviors>
          <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
              <tokenProvider>
                <sharedAccessSignature keyName="RootManageSharedAccessKey" key="[SAS_KEY]" />
              </tokenProvider>
            </transportClientEndpointBehavior>
          </behavior>
        </endpointBehaviors>
        <serviceBehaviors>
          <behavior name="default">
            <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
          </behavior>
        </serviceBehaviors>
      </behaviors>
    </system.serviceModel>
    <appSettings>
        <!-- Service Bus specific app setings for messaging connections -->
        <add key="Microsoft.ServiceBus.ConnectionString"
            value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
</configuration>
```

## <a name="step-4-host-the-rest-based-wcf-service-to-use-service-bus"></a>Krok 4: Hosta usługi opartej na pozostałych WCF do Bus usługi

W tym kroku opisano sposób uruchamiania usługi sieci web za pomocą aplikacji konsoli na Bus usługi. Pełna lista kodu napisanego w tym kroku znajduje się w tym przykładzie zgodnie z procedurą.

### <a name="to-create-a-base-address-for-the-service"></a>Aby utworzyć podstawowy adres usługi

1. W `Main()` funkcja deklaracji, Utwórz zmienną do przechowywania nazw projektu Bus usługi. Upewnij się zamienić `yourNamespace` o nazwie nazw usługi został utworzony wcześniej.

    ```
    string serviceNamespace = "yourNamespace";
    ```
    Bus usługi używa nazwy obszaru nazw, aby utworzyć unikatowy identyfikator URI.

2. Tworzenie `Uri` wystąpienie na adres podstawowy usługi opartej na obszarze nazw.

    ```
    Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");
    ```

### <a name="to-create-and-configure-the-web-service-host"></a>Aby utworzyć i skonfigurować hosta usługi sieci web

- Tworzenie hosta usługi sieci web, używając adresu URI utworzony wcześniej w tej sekcji.

    ```
    WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
    ```
    Hosta usługi jest obiektem WCF, który tworzy aplikacji hosta. W tym przykładzie przekazuje typ hosta, którego chcesz utworzyć (jest, **ImageService**), a także adres, w którym chcesz udostępnić aplikacji hosta.

### <a name="to-run-the-web-service-host"></a>Aby uruchomić hosta usługi sieci web

1. Otwórz usługę.

    ```
    host.Open();
    ```
    Usługa działa teraz.

2. Wyświetlanie komunikat z informacją, że usługa jest uruchomiona oraz jak zatrzymać usługę.

    ```
    Console.WriteLine("Copy the following address into a browser to see the image: ");
    Console.WriteLine(address + "GetImage");
    Console.WriteLine();
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```

3. Po zakończeniu zamknij hosta usługi.

    ```
    host.Close();
    ```

## <a name="example"></a>Przykład

W poniższym przykładzie obejmuje umowy serwisowej oraz wdrożenie z powyższych czynności spowoduje samouczka i obsługuje usługę w aplikacji konsoli. Poniższy kod kompilacji do plik wykonywalny o nazwie ImageListener.exe.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string serviceNamespace = "InsertServiceNamespaceHere";
            Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");

            WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
            host.Open();

            Console.WriteLine("Copy the following address into a browser to see the image: ");
            Console.WriteLine(address + "GetImage");
            Console.WriteLine();
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            host.Close();
        }
    }
}
```

### <a name="compiling-the-code"></a>Kompilowanie kodu

Po utworzeniu rozwiązanie, wykonaj poniższe czynności, aby uruchomić aplikację:

1. Naciśnij klawisz **F5**lub przejdź do lokalizacji plik wykonywalny (ImageListener\bin\Debug\ImageListener.exe), aby uruchomić usługę. Zachowaj aplikacji uruchomiony, ponieważ są wymagane do wykonywania następnego kroku.

2. Skopiuj i wklej adres w wierszu polecenia w przeglądarce, aby wyświetlić obraz.

3. Po zakończeniu naciśnij klawisz **Enter** w oknie wiersza polecenia, aby zamknąć aplikację.

## <a name="next-steps"></a>Następne kroki

Teraz, gdy zostały utworzone aplikacji, która korzysta z usługi przekazywania Bus usługi, zobacz następujące artykuły, aby dowiedzieć się więcej na temat odnośnie wiadomości:

- [Omówienie architektury Azure Bus usługi](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#relays)

- [Jak używać usługi przekazywania Bus](service-bus-dotnet-how-to-use-relay.md)

[Azure portal]: https://portal.azure.com