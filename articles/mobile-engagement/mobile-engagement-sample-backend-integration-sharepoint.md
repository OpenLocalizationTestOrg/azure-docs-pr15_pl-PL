<properties 
    pageTitle="Azure zaangażowania urządzeń przenośnych — integracji wewnętrznej bazy danych" 
    description="Łączenie zaangażowania Mobile Azure z wewnętrznej bazy danych programu SharePoint na tworzenie kampanii z witryny programu SharePoint" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-multiple" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

# <a name="azure-mobile-engagement---api-integration"></a>Azure zaangażowania urządzeń przenośnych — interfejs API integracji

W systemie marketingowych automatycznego tworzenia i aktywowanie kampanii marketingowych również są wykonywane automatycznie. W tym celu - Azure zaangażowania Mobile umożliwia tworzenie takich automatyczną kampanii marketingowych, również za pomocą interfejsów API. 

Zazwyczaj klienci umożliwia utworzenie ogłoszenia i ankiety itp jako część ich kampanii marketingowych interfejsu fronton zaangażowania Mobile. Jednak podczas kampanii marketingowych stają się dojrzałych, istnieje potrzeba korzystać z danych zablokowane w systemach wewnętrznej bazy danych (na przykład CRM system lub systemu CMS, takich jak SharePoint) tak, aby w pełni automatyczną planowana można utworzyć kampanii który tworzy w zaangażowania Mobile dynamicznie na podstawie danych ułożony w ramach systemów wewnętrznej bazy danych. 

![][5]

Ten samouczek przechodzi takiej sytuacji, gdzie użytkownikiem biznesowym SharePoint wypełnia listy programu SharePoint z danymi marketingowych i zautomatyzowany proces przejmuje elementów z listy i łączy w systemie zaangażowania Mobile tworzenie kampanii marketingowej z danych programu SharePoint za pomocą dostępnych interfejsów API pozostałych. 
 
> [AZURE.IMPORTANT] Zazwyczaj są dostępne w tym przykładzie jako punktu startowego dla wiedzieć, jak połączeń dowolnego interfejsu API usługi REST zaangażowania Mobile, jak szczegółowe informacje dotyczące dwóch aspektami za pośrednictwem interfejsów API - uwierzytelniania i przechodzącą parametry połączenia. 

## <a name="sharepoint-integration"></a>Integracja z programem SharePoint
1. Poniżej przedstawiono, jak wygląda Przykładowa lista programu SharePoint. **Tytuł**, **Kategoria**, **NotificationTitle**, **wiadomości** i **adres URL** służą do tworzenia w powiadomieniu. Ma kolumnę o nazwie **IsProcessed** jest używany przez proces automatyzacji próbki w formularzu programu konsoli. Można uruchomić konsoli programu jako WebJob Azure, tak aby Zaplanuj termin jego lub bezpośrednio umożliwiają przepływu pracy programu SharePoint do programu tworzenia i aktywowanie powiadomienia, gdy element zostanie wstawiona do listy programu SharePoint. W tym przykładzie używamy programu konsoli, które przechodzi elementów na liście programu SharePoint i utworzyć anons w zaangażowania Mobile Azure dla każdego z nich, a następnie na końcu oznacza flagę **IsProcessed** , aby być spełnione przy tworzeniu pomyślnego zawiadomienie o.

    ![][1]

2. Użyto kod z próbki *zdalnego uwierzytelniania w usłudze SharePoint Online za pomocą modelu obiektów klienta* [tutaj](https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c) do uwierzytelnienia z listą programu SharePoint.
 
3. Po uwierzytelnieniu, możemy pętli elementów listy, aby dowiedzieć się, wszystkich nowo utworzonych elementów (posiadający **IsProcessed** = false). 

        static async void CreateCampaignFromSharepoint()
        {
            using (ClientContext clientContext = ClaimClientContext.GetAuthenticatedContext(targetSharepointSite))
            {
                // Using https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c for authentication     
                Web site = clientContext.Web;
                List list = site.Lists.GetByTitle("VideoContent");
                CamlQuery query = new CamlQuery();
                query.ViewXml = "<View/>";
                ListItemCollection items = list.GetItems(query);

                // Load the SharePoint list
                clientContext.Load(list);
                clientContext.Load(items);
                clientContext.ExecuteQuery();

                // Loop through the list to go through all the items which are newly added
                foreach (ListItem item in items)
                    if (item["IsProcessed"].ToString() != "Yes")
                    {
                        string name = item["Title"].ToString();
                        string notificationTitle = item["NotificationTitle"].ToString();
                        string notificationMessage = item["Message"].ToString();
                        string category = item["Category"].ToString();
                        string actionURL = ((FieldUrlValue)item["URL"]).Url;

                        // Send an HTTP request to create AzME campaign
                        int campaignId = CreateAzMECampaign
                            (name, notificationTitle, notificationMessage, category, actionURL).Result;

                        if (campaignId != -1)
                        {
                            // If creating campaign is successful then set this to true
                            item["IsProcessed"] = "Yes";

                            // Now try to activate the campaign also
                            await ActivateAzMECampaign(campaignId);
                        }
                        else
                        {
                            item["IsProcessed"] = "Error";
                        }
                        item.Update();
                    }
                clientContext.ExecuteQuery();
            }
        }

## <a name="mobile-engagement-integration"></a>Integracja zaangażowania Mobile
1.  Gdy okaże się odpowiedni element, który wymaga przetwarzanie — firma Microsoft wyodrębniać informacje wymagane do utworzenia ogłoszenie z elementu listy i połączenia `CreateAzMECampaign` go utworzyć, a następnie `ActivateAzMECampaign` go uaktywnić. Są to zasadniczo połączeń interfejsu API usługi REST połączeń zaangażowania Mobile wewnętrznej bazy danych. 

2.  Interfejsy API pozostałych zaangażowania Mobile wymaga **uwierzytelniania podstawowego schematu autoryzacji HTTP nagłówek** , który składa się z `ApplicationId` i `ApiKey` który uzyskujesz przez Azure portal. Upewnij się, w sekcji **klawiszy interfejsu api** używasz klucz, a *nie* w sekcji **sdk klawiszy** . 

    ![][2]

        static string CreateAuthZHeader()
        {
            string BasicAuthzHeaderString = "Basic " + EncodeTo64(ApplicationId + ":" + ApiKey);
            return BasicAuthzHeaderString;
        }

        static public string EncodeTo64(string toEncode)
        {
            byte[] toEncodeAsBytes = System.Text.ASCIIEncoding.ASCII.GetBytes(toEncode);
            string returnValue = System.Convert.ToBase64String(toEncodeAsBytes);
            return returnValue;
        }  

3. Do tworzenia anons wpisz kampanii — zapoznaj się z [dokumentacją](https://msdn.microsoft.com/library/azure/mt683750.aspx). Należy upewnić się, że określasz kampanii `kind` jak *powiadomienia* i [ładunku](https://msdn.microsoft.com/library/azure/mt683751.aspx) , przekazując je jako FormUrlEncodedContent. 

        static async Task<int> CreateAzMECampaign(string campaignName, string notificationTitle, 
            string notificationMessage, string notificationCategory, string actionURL)
        {
            string createURIFragment = "/reach/1/create";

            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());
                
                // Create the payload to send the content
                // Reference -> https://msdn.microsoft.com/library/dn913749.aspx
                string data =
                    @"{""name"":""" + campaignName + @"""," + 
                    @"""type"":""only_notif""," + 
                    @"""deliveryTime"":""any"","" + 
                    @""notificationTitle"":""" + notificationTitle + 
                    @""",""notificationMessage"":""" + notificationMessage + 
                    @""",""actionUrl"":""" + actionURL + @"""}";
                
                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("data", data),
                });

                // Send the POST request
                var response = await client.PostAsync(url + createURIFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                int campaignId = -1;
                if (response.StatusCode.ToString() == "OK")
                {
                    // This is the campaign id
                    campaignId = Convert.ToInt32(responseString);
                    Console.WriteLine("Campaign successfully created with id {0}", campaignId);
                }
                else
                {
                    Console.WriteLine("Campaign creation failed with error - '{0}'", responseString);
                }
                return campaignId;
            }
        }

4. Po umieszczeniu anons utworzony, zobaczysz podobną do następujących portalu zaangażowania Mobile (należy zauważyć, że stan = wersja robocza i aktywowany = n/d!)

    ![][3]

5. `CreateAzMECampaign`Tworzy kampanii ogłoszenia i zwraca jej identyfikator rozmówcy. `ActivateAzMECampaign`Ten identyfikator wymaga jako parametru, aby aktywować kampanii. 

        static async Task<bool> ActivateAzMECampaign(int campaignId)
        {
            string activateUriFragment = "/reach/1/activate";
            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());

                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("id", campaignId.ToString()),
                });

                // Send the POST request
                var response = await client.PostAsync(url + activateUriFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                if (response.StatusCode.ToString() == "OK")
                {
                    Console.WriteLine("Campaign successfully activated");
                    return true;
                }
                else
                {
                    Console.WriteLine("Campaign activation failed with error - '{0}'", responseString);
                    return false;
                }
            }
        }

6. Po umieszczeniu anons aktywowane, zostanie wyświetlony podobną do następujących portalu zaangażowania Mobile:

    ![][4]

7. Zaraz po uaktywnieniu otrzymuje kampanii wszystkich urządzeń, które spełniają kryterium kampanii zaczną wyświetlane powiadomienia. 

8. Będzie także zauważyć, że element listy oznaczonych IsProcessed = false została ustawiona na PRAWDA po utworzeniu kampanii powiadomienia.  

W tym przykładzie utworzony kampanii prosty zawiadomienie o Określanie głównie wymagane właściwości. Można dostosować tak jak można z poziomu portalu, korzystając z informacji [w tym miejscu](https://msdn.microsoft.com/library/azure/mt683751.aspx). 

<!-- Images. -->
[1]: ./media/mobile-engagement-sample-backend-integration-sharepoint/sharepointlist.png
[2]: ./media/mobile-engagement-sample-backend-integration-sharepoint/properties.png
[3]: ./media/mobile-engagement-sample-backend-integration-sharepoint/new-announcement.png
[4]: ./media/mobile-engagement-sample-backend-integration-sharepoint/activate-announcement.png
[5]: ./media/mobile-engagement-sample-backend-integration-sharepoint/diagram.png


 
