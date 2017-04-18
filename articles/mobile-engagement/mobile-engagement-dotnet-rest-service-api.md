<properties 
    pageTitle="Aby uzyskać dostęp do API usługi zaangażowania Mobile Azure za pomocą interfejsu API usługi REST" 
    description="Informacje dotyczące używania interfejsy API pozostałych zaangażowania Mobile Aby uzyskać dostęp do API usługi zaangażowania Mobile Azure"       
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="wesmc7777" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-multiple" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/05/2016" 
    ms.author="wesmc;ricksal" />

#<a name="using-rest-to-access-azure-mobile-engagement-service-apis"></a>Aby uzyskać dostęp do API usługi zaangażowania Mobile Azure za pomocą usługi REST

Azure zaangażowania Mobile udostępnia [Interfejsu API usługi REST zaangażowania Mobile Azure](https://msdn.microsoft.com/library/azure/mt683754.aspx) umożliwia zarządzanie urządzeniami, kampanii Reach-wypychanych itp. W tym przykładzie przy użyciu interfejsów API pozostałych bezpośrednio utworzyć kampanii zawiadomienie o aktywowanie, a następnie przekazać go do zestawu urządzeń. 

Jeśli nie chcesz używać bezpośrednio za pośrednictwem interfejsów API pozostałych, oferujemy [Swagger pliku](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) której można przy użyciu narzędzi do generowania SDK dla preferowany język. Zalecamy korzystanie z narzędzia [AutoRest](https://github.com/Azure/AutoRest) do generowania usługi SDK z naszych pliku Swagger. Utworzono .NET SDK w podobny sposób, który pozwala korzystać z tych interfejsów API przy użyciu opakowanie C# i nie trzeba wykonać wymiany informacji token uwierzytelniania i odświeżania samodzielnie. Jeśli chcesz szczegółową podobne próbki przy użyciu tej opakowanie, zobacz [Przykładowe SDK .NET interfejsu API usługi](mobile-engagement-dotnet-sdk-service-api.md)

W tym przykładzie przy użyciu interfejsy API pozostałych bezpośrednio utworzyć i aktywować kampanii zawiadomienie o. 

## <a name="adding-a-username-appinfo-to-the-mobile-engagement-app"></a>Dodawanie appInfo nazwa_użytkownika do aplikacji zaangażowania Mobile

W tym przykładzie wymaga tag informacje o aplikacji dodany do aplikacji zaangażowania Mobile. W portalu zaangażowania dla aplikacji, możesz dodać znacznik, klikając pozycję **Ustawienia** > **znacznika (informacje o aplikacji)** > **Nowy znacznik (informacje o aplikacji)**. Dodaj nowy znacznik o nazwie **nazwa_użytkownika** jako typu **ciąg** .

![](./media/mobile-engagement-dotnet-rest-service-api/user-name-app-info.png)



Jeśli zostały wykonane, [Wprowadzenie Azure Mobile zaangażowania dla systemu Windows uniwersalny aplikacji](mobile-engagement-windows-store-dotnet-get-started.md), możesz przetestować wysłanie znacznik **nazwa_użytkownika** po prostu zastępowanie `OnNavigatedTo()` w swojej `MainPage` zajęć, aby wysłać znacznik informacje o aplikacji podobny do następującego kodu:

    protected override void OnNavigatedTo(NavigationEventArgs e)
    {
        base.OnNavigatedTo(e);

        String name = "Wesley"; // Prompt the user to provide this in your client app.

        Dictionary<object, object> info = new Dictionary<object, object>();
        info.Add("user_name", name);
        EngagementAgent.Instance.SendAppInfo(info);
    }
 


## <a name="creating-the-service-api-app"></a>Tworzenie aplikacji usług interfejsu API

1. Przede wszystkim konieczne będzie cztery parametry uwierzytelniania do użytku z tym przykładzie. Parametry te są **SubscriptionId**, **TenantId**, **Identyfikator aplikacji** i **hasło**. Aby uzyskać tych parametrów uwierzytelniania, zaleca się użycie metody skrypt programu PowerShell wymienione w sekcji *jednorazowej konfiguracji (za pomocą skryptu)* w samouczku [uwierzytelniania](mobile-engagement-api-authentication.md#authentication) . 

2. Wykazać pracy z interfejsy API usługi REST do tworzenia i aktywować nowej kampanii zawiadomienie o użyjemy prostych aplikacji konsoli systemu Windows. Dlatego otwarcie programu Visual Studio i tworzenie nowej **Aplikacji konsoli**.   

3. Następnie dodaj pakiet NuGet **Newtonsoft.Json** do projektu.

4. W `Program.cs` plików, Dodaj następujący `using` instrukcje dla następujących nazw:

        using System.IO;
        using System.Net;
        using Newtonsoft.Json.Linq;
        using Newtonsoft.Json;

5. Następnie należy zdefiniować następujące stałe w `Program` zajęć. Te będą używane do uwierzytelniania i wchodzącymi w interakcje z aplikacją zaangażowania Mobile, w której jest tworzona kampanii zawiadomienie o:


        // Parameters needed for authentication of API calls.
        // These are returned from the PowerShell script in the authentication tutorial. 
        // https://azure.microsoft.com/documentation/articles/mobile-engagement-api-authentication/
        static String SubscriptionId = "<Your Subscription Id>";
        static String TenantId = "<Your TenantId>";
        static String ApplicationId = "<Your Application Id>";
        static String Secret = "<Your Secret>";

        // The token for authenticating with your Mobile Engagement app.
        static String Token = null;

        // This is the Azure Resource group concept for grouping together resources 
        // See: https://azure.microsoft.com/en-us/documentation/articles/resource-group-portal/
        static String ResourceGroup = "MobileEngagement";

        // For Mobile Engagement operations
        // App Collection Name from the Azure portal 
        static String Collection = "<Your App Collection Name>";

        // Application Resource Name - make sure you are using the one as specified in the dashboard of the
        // Azure portal. (This is NOT the App Name)
        static String AppName = "WesmcRestTest-windows";

        // New campaign id returned from creating the new campaign and used to activate and push the campaign
        // a set of devices.
        static String NewCampaignID = null;

        // This list will hold the device Ids we choose to push the campaign to.
        static List<String> deviceList = new List<String>();


6. Dodaj następujące dwie metody `Program` zajęć. Te posłuży do wykonywanie pozostałych interfejsy API i wyświetlanie odpowiedzi na wezwania do konsoli.

        private static async Task<HttpWebResponse> ExecuteREST(string httpMethod, string uri, string authToken, WebHeaderCollection headers = null, string body = null, string contentType = "application/json")
        {
            //=== Execute the request 
            HttpWebRequest request = (HttpWebRequest)HttpWebRequest.Create(uri);
            HttpWebResponse response = null;

            request.Method = httpMethod;
            request.ContentType = contentType;
            request.ContentLength = 0;

            if (authToken != null)
                request.Headers.Add("Authorization", authToken);

            if (headers != null)
            {
                request.Headers.Add(headers);
            }

            if (body != null)
            {
                byte[] bytes = Encoding.UTF8.GetBytes(body);

                try
                {
                    request.ContentLength = bytes.Length;
                    Stream requestStream = request.GetRequestStream();
                    requestStream.Write(bytes, 0, bytes.Length);
                    requestStream.Close();
                }
                catch (Exception e)
                {
                    Console.WriteLine(e.Message);
                }
            }

            try
            {
                response = (HttpWebResponse)await request.GetResponseAsync();
            }
            catch (WebException we)
            {
                if (we.Response != null)
                {
                    response = (HttpWebResponse)we.Response;
                }
                else
                    Console.WriteLine(we.Message);
            }
            catch (Exception e)
            {
                Console.WriteLine(e.Message);
            }

            return response;
        }


        private static void DisplayResponse(dynamic data, HttpWebResponse response)
        {
            Console.WriteLine("HTTP Status " + (int)response.StatusCode + " : " + response.StatusDescription);
            Console.WriteLine(data + "\n");
        }

    }

7. Dodaj następujący kod do swojego `Main` metody do wygenerowania tokenu uwierzytelniania z parametrami uwierzytelniania zebranych:

        //***************************************************************************
        //*** Get a valid authorization token with your authentication parameters ***
        //***************************************************************************

        String methodUrl = "https://login.microsoftonline.com/" + TenantId + "/oauth2/token";
        String requestBody = "grant_type=client_credentials&client_id=" + ApplicationId +
                            "&client_secret=" + Secret +
                            "&resource=https://management.core.windows.net/";
        Console.WriteLine("Getting authorization token...\n");
        HttpWebResponse response = ExecuteREST("POST", methodUrl, null, null, requestBody, null).Result;
        Stream receiveStream = response.GetResponseStream();
        StreamReader readStream = new StreamReader(receiveStream, Encoding.UTF8);
        dynamic data = JObject.Parse(readStream.ReadToEnd());
        readStream.Close();
        receiveStream.Close();
        DisplayResponse(data, response);

        if (response.StatusCode == HttpStatusCode.OK)
        {
            Token = data.token_type + " " + data.access_token;
            Console.WriteLine("Got authorization token...");
            Console.WriteLine("Authorization : " + Token + "\n");
        }
        else
        {
            Console.WriteLine("*** Failed to get authorization token. Check your parameters for API calls.\n");
            return;
        }

8. Teraz gdy mamy już token prawidłowych uwierzytelniania możemy utworzyć kampanii Reach za pomocą interfejsu API usługi REST [Tworzenie kampanii](https://msdn.microsoft.com/library/azure/mt683742.aspx) . Prosty **w dowolnym momencie**będzie nowej kampanii & **tylko do powiadomień** zawiadomienie o kampanii z tytułu i wiadomości. Są to kampanii wypychanych ręcznego, jak pokazano na poniższym obrazie zrzut Oznacza to, że będzie tylko przeniesiony, za pomocą interfejsów API.


    ![](./media/mobile-engagement-dotnet-rest-service-api/manual-push.png)

    Dodaj następujący kod do swojego `Main` metodę w celu utworzenia kampanii zawiadomienie o: 

        //*****************************************************************************
        //*** Create a campaign to send a notification using the user-name app-info ***
        //*****************************************************************************

        String newCampaignMethodUrl = "https://management.azure.com/subscriptions/" + SubscriptionId + "/" +
               "resourcegroups/" + ResourceGroup + "/providers/Microsoft.MobileEngagement/appcollections/" +
               Collection + "/apps/" + AppName + "/campaigns/announcements?api-version=2014-12-01";

        String campaignRequestBody = "{ \"name\": \"BirthdayCoupon\", " +
                                        "\"type\": \"only_notif\", " +
                                        "\"deliveryTime\": \"any\", " +
                                        "\"notificationType\": \"popup\", " +
                                        "\"pushMode\":\"manual\", " +
                                        "\"notificationTitle\": \"Happy Birthday ${user_name}\", " +
                                        "\"notificationMessage\": \"Take extra 10% off on your orders today!\"}";

        Console.WriteLine("Creating new campaign...\n");
        HttpWebResponse newCampaignResponse = ExecuteREST("POST", newCampaignMethodUrl, Token, null, campaignRequestBody).Result;
        Stream receiveCampaignStream = newCampaignResponse.GetResponseStream();
        StreamReader readCampaignStream = new StreamReader(receiveCampaignStream, Encoding.UTF8);
        dynamic newCampaignData = JObject.Parse(readCampaignStream.ReadToEnd());
        readCampaignStream.Close();
        receiveCampaignStream.Close();
        DisplayResponse(newCampaignData, newCampaignResponse);

        if (newCampaignResponse.StatusCode == HttpStatusCode.Created)
        {
            NewCampaignID = newCampaignData.id;
            Console.WriteLine("Created new campaign...");
            Console.WriteLine("New Campaign Id    : " + NewCampaignID + "\n");
        }
        else
        {
            Console.WriteLine("*** Failed to create birthday campaign.\n");
            return;
        }


9. Należy aktywować kampanii, może zostać przeniesiony do wszystkich urządzeń. Firma Microsoft zapisany identyfikator nowej kampanii w `NewCampaignID` zmiennej. Użyjemy go jako parametr ścieżki URI aktywować kampanii przy użyciu interfejsu API usługi REST [kampanii Aktywuj](https://msdn.microsoft.com/library/azure/mt683745.aspx) . To, nawet jeśli jego będzie tylko zostać przeniesiony ręcznie z za pośrednictwem interfejsów API, należy zmienić stan kampanii na **według harmonogramu** .

    Dodaj następujący kod do swojego `Main` metodą aktywowania kampanii zawiadomienie o: 

        //******************************************
        //*** Activate the new birthday campaign ***
        //******************************************

        String activateCampaignUrl = "https://management.azure.com/subscriptions/" + SubscriptionId + "/" +
                  "resourcegroups/" + ResourceGroup + "/providers/Microsoft.MobileEngagement/appcollections/" +
                   Collection + "/apps/" + AppName + "/campaigns/announcements/" + NewCampaignID +
                   "/activate?api-version=2014-12-01";

        Console.WriteLine("Activating new campaign (ID : " + NewCampaignID + ")...\n");
        HttpWebResponse activateCampaignResponse = ExecuteREST("POST", activateCampaignUrl, Token).Result;
        Stream activateCampaignStream = activateCampaignResponse.GetResponseStream();
        StreamReader activateCampaignReader = new StreamReader(activateCampaignStream, Encoding.UTF8);
        dynamic activateCampaignData = JObject.Parse(activateCampaignReader.ReadToEnd());
        activateCampaignReader.Close();
        activateCampaignStream.Close();
        DisplayResponse(activateCampaignData, activateCampaignResponse);

        if (activateCampaignResponse.StatusCode == HttpStatusCode.OK)
        {
            Console.WriteLine("Activated new campaign...");
            Console.WriteLine("New Campaign State : " + activateCampaignData.state + "\n");
        }
        else
        {
            Console.WriteLine("*** Failed to activate birthday campaign.\n");
            return;
        }

10. Aby przekazać kampanii należy podać identyfikatory urządzeń dla użytkowników, którzy będą otrzymywać powiadomienia. Aby uzyskać wszystkie identyfikatory urządzenia użyjemy interfejsu API usługi REST [Kwerenda urządzeń](https://msdn.microsoft.com/library/azure/mt683826.aspx) . Każdy identyfikator urządzenia będą dodawane do listy, jeśli jest skojarzony appInfo **nazwa_użytkownika** .

    Dodaj następujący kod do swojego `Main` metody, aby pobrać wszystkie identyfikatorów urządzenia i wypełnić deviceList:

        //************************************************************************
        //*** Now that the manualPush campaign is activated, get the deviceIds ***
        //*** that you want to trigger the push campaign on.                   ***
        //************************************************************************

        String getDevicesUrl = "https://management.azure.com/subscriptions/" + SubscriptionId + "/" +
                  "resourcegroups/" + ResourceGroup + "/providers/Microsoft.MobileEngagement/appcollections/" +
                   Collection + "/apps/" + AppName + "/devices?api-version=2014-12-01";

        Console.WriteLine("Getting device IDs...\n");
        HttpWebResponse devicesResponse = ExecuteREST("GET", getDevicesUrl, Token).Result;
        Stream devicesStream = devicesResponse.GetResponseStream();
        StreamReader devicesReader = new StreamReader(devicesStream, Encoding.UTF8);
        dynamic devicesData = JObject.Parse(devicesReader.ReadToEnd());
        devicesReader.Close();
        devicesStream.Close();
        DisplayResponse(devicesData, devicesResponse);

        if (devicesResponse.StatusCode == HttpStatusCode.OK)
        {
            Console.WriteLine("Got devices.");
            foreach (dynamic device in devicesData.value)
            {
                // Just adding all in this example
                if (device.appInfo.user_name != null)
                {
                    Console.WriteLine("Adding device for push campaign...");
                    Console.WriteLine("Device ID    : " + device.deviceId);
                    Console.WriteLine("user_name    : " + device.appInfo.user_name);
                    deviceList.Add((String)device.deviceId);
                }
            }
            Console.WriteLine();
        }
        else
        {
            Console.WriteLine("*** Failed to get devices.\n");
            return;
        }


11. Ponadto możemy przesunie kampanii do wszystkich identyfikatorów urządzenie na liście przy użyciu interfejsu API usługi REST [wypychanych kampanii](https://msdn.microsoft.com/library/azure/mt683734.aspx) . To powiadomienie **w aplikacji** . Dlatego aplikacji musi działać na tym urządzeniu, aby go, aby otrzymać przez użytkownika.

    Dodaj następujący kod do swojego `Main` metody, aby przekazać campign do urządzeń w deviceList:

        //**************************************************************
        //*** Trigger the manualPush campaign on the desired devices ***
        //**************************************************************

        String pushCampaignUrl = "https://management.azure.com/subscriptions/" + SubscriptionId + "/" +
                  "resourcegroups/" + ResourceGroup + "/providers/Microsoft.MobileEngagement/appcollections/" +
                   Collection + "/apps/" + AppName + "/campaigns/announcements/" + NewCampaignID + 
                   "/push?api-version=2014-12-01";

        Console.WriteLine("Triggering push for new campaign (ID : " + NewCampaignID + ")...\n");
        String deviceIds = "{\"deviceIds\":" + JsonConvert.SerializeObject(deviceList) + "}";
        Console.WriteLine("\n" + deviceIds + "\n");
        HttpWebResponse pushDevicesResponse = ExecuteREST("POST", pushCampaignUrl, Token, null, deviceIds).Result;
        Stream pushDevicesStream = pushDevicesResponse.GetResponseStream();
        StreamReader pushDevicesReader = new StreamReader(pushDevicesStream, Encoding.UTF8);
        dynamic pushDevicesData = JObject.Parse(pushDevicesReader.ReadToEnd());
        pushDevicesReader.Close();
        pushDevicesStream.Close();
        DisplayResponse(pushDevicesData, pushDevicesResponse);

        if (pushDevicesResponse.StatusCode == HttpStatusCode.OK)
        {
            Console.WriteLine("Triggered push on new campaign.\n");
        }
        else
        {
            Console.WriteLine("*** Failed to push campaign to the device list.\n");
        }


12. Tworzenie i uruchamianie aplikacji konsoli. Danych wyjściowych powinny być podobny do następującego:


        C:\Users\Wesley\AzME_Service_API_Rest\test.exe

        Getting authorization token...
        
        HTTP Status 200 : OK
        {
          "token_type": "Bearer",
          "expires_in": "3599",
          "expires_on": "1458085162",
          "not_before": "1458081262",
          "resource": "https://management.core.windows.net/",
          "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBPW
        b3dzLm5ldC8iLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFh
        NzE4LTQ0YzQtOGVjMS0xM2IwODExMTRmM2UiLCJhcHBpZGFjciI6IjEiLCJpZHAiOiJodHRwczovL3N0cy53
        MTdhNGJkIiwic3ViIjoiOWIzZGQ2MDctNmYwOC00Y2Y5LTk2N2YtZmUyOGU3MTdhNGJkIiwidGlkIjoiNzJm
        F5x9gj8JJ4CjtLaH3mm1_U22Qc_AjB9mtbM97dgu3kCiUm19ISatRBoAmVz3JGW6LSv2TyCeg9TGYVrE3OAE
        hl_pY9COXicc7I501_X67GsmUgs9-EedF3STrEoY5cONTiMvwCLfeBgScgXA0ikAu62KpzMu0VFDYu-HASI8
        }
        
        Got authorization token...
        Authorization : Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNN
        aW5kb3dzLm5ldC8iLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYt
        Zi1jNzE4LTQ0YzQtOGVjMS0xM2IwODExMTRmM2UiLCJhcHBpZGFjciI6IjEiLCJpZHAiOiJodHRwczovL3N0
        OGU3MTdhNGJkIiwic3ViIjoiOWIzZGQ2MDctNmYwOC00Y2Y5LTk2N2YtZmUyOGU3MTdhNGJkIiwidGlkIjoi
        iI-oF5x9gj8JJ4CjtLaH3mm1_U22Qc_AjB9mtbM97dgu3kCiUm19ISatRBoAmVz3JGW6LSv2TyCeg9TGYVrE
        vsf3hl_pY9COXicc7I501_X67GsmUgs9-EedF3STrEoY5cONTiMvwCLfeBgScgXA0ikAu62KpzMu0VFDYu-H
        
        Creating new campaign...
        
        HTTP Status 201 : Created
        {
          "id": 24,
          "state": "draft"
        }
        
        Created new campaign...
        New Campaign Id    : 24
        
        Activating new campaign (ID : 24)...
        
        HTTP Status 200 : OK
        {
          "id": 24,
          "state": "scheduled"
        }
        
        Activated new campaign...
        New Campaign State : scheduled
        
        Getting device IDs...
        
        HTTP Status 200 : OK
        {
          "value": [
            {
              "meta": {
                "lastSeen": 1458080534371,
                "firstSeen": 1458080534371,
                "lastLocation": 1458081389617,
                "lastInfo": 1458080571603
              },
              "appInfo": {
                "user_name": "Wesley"
              },
              "deviceId": "1d6208b8f281203ecb49431e2e5ce6b3"
            },
            {
              "meta": {
                "lastSeen": 1457990584698,
                "firstSeen": 1457949384025,
                "lastLocation": 1457990914895,
                "lastInfo": 1457990584597
              },
              "appInfo": {
                "user_name": "Wesley"
              },
              "deviceId": "302486644890e26045884ee5aa0619ec"
            }
          ]
        }
        
        Got devices.
        Adding device for push campaign...
        Device ID    : 1d6208b8f281203ecb49431e2e5ce6b3
        user_name    : Wesley
        Adding device for push campaign...
        Device ID    : 302486644890e26045884ee5aa0619ec
        user_name    : Wesley
        
        Triggering push for new campaign (ID : 24)...
        
        
        {"deviceIds":["1d6208b8f281203ecb49431e2e5ce6b3","302486644890e26045884ee5aa0619ec"]}
        
        HTTP Status 200 : OK
        {
          "invalidDeviceIds": []
        }
        
        Triggered push on new campaign.
        


<!-- Images. -->

[1]: ./media/mobile-engagement-dotnet-sdk-service-api/include-prerelease.png
