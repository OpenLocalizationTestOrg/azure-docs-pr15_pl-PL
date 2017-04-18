<properties 
    pageTitle="Dostęp do API usługi zaangażowania Mobile Azure za pomocą .NET SDK" 
    description="Informacje dotyczące używania SDK .NET zaangażowania Mobile Aby uzyskać dostęp do API usługi zaangażowania Mobile Azure"        
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-multiple" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="using-net-sdk-to-access-azure-mobile-engagement-service-apis"></a>Dostęp do API usługi zaangażowania Mobile Azure za pomocą .NET SDK

Azure zaangażowania Mobile udostępnia zestaw interfejsów API umożliwiający zarządzanie urządzeniami, kampanii Reach-wypychanych itp. Aby wykonać czynność związaną z tych interfejsów API, również otrzymasz [pliku Swagger](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) , której można przy użyciu narzędzi do generowania SDK dla preferowany język. Zalecamy korzystanie z narzędzia [AutoRest](https://github.com/Azure/AutoRest) do generowania usługi SDK z naszych pliku Swagger. 

Utworzono .NET SDK w podobny sposób, który pozwala korzystać z tych interfejsów API przy użyciu opakowanie C# i nie trzeba wykonać wymiany informacji token uwierzytelniania i odświeżania samodzielnie.  

W tym przykładzie przechodzi zestawu kroków, które należy wykonać, aby użyć .NET SDK:

1. Przede wszystkim należy ustawienia uwierzytelniania dla Twojej interfejsy API przy użyciu usługi Azure Active Directory jako opisane [tutaj](mobile-engagement-api-authentication.md#authentication). Na końcu tej procedury użytkownik ma prawidłową **SubscriptionId**, **TenantId**, **Identyfikator aplikacji** i **hasło**. 

2. Wykazać pracy z .NET SDK z tego scenariusza tworzenia kampanii zawiadomienie o użyjemy prostych aplikacji konsoli systemu Windows. Dlatego otwarcie programu Visual Studio i tworzenie **Aplikacji konsoli**.   

3. Następnie należy pobrać zestaw SDK programu .NET, który jest dostępny jako **Biblioteki zarządzania Microsoft Azure zaangażowania** w galerii Nuget [tutaj](https://www.nuget.org/packages/Microsoft.Azure.Management.Engagement/).
Jeśli instalujesz Nuget z programu Visual Studio, należy się upewnić, że masz wyboru oznaczone opcji **Uwzględnij wstępną** podczas wyszukiwania pakietu:

    ![][1]

4. W `Program.cs` pliku, Dodaj następujące obszary nazw:

        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.Engagement;
        using Microsoft.Azure.Management.Engagement.Models;

5. Następnie należy zdefiniować następujące stałe, które będą używane do uwierzytelniania i interakcja z aplikacji zaangażowania Mobile, w której jest tworzona kampanii zawiadomienie o:

        // For authentication
        const string TENANT_ID = "<Your TenantId>";
        const string CLIENT_ID = "<Your Application Id>";
        const string CLIENT_SECRET = "<Your Secret>";
        const string SUBSCRIPTION_ID = "<Your Subscription Id>";

        // This is the Azure Resource group concept for grouping together resources 
        //  see here: https://azure.microsoft.com/en-us/documentation/articles/resource-group-portal/
        const string RESOURCE_GROUP = "";

        // For Mobile Engagement operations
        // App Collection Name 
        const string APP_COLLECTION_NAME = "";
        // Application Resource Name - make sure you are using the one as specified in the Azure portal (NOT the App Name)
        const string APP_RESOURCE_NAME = "";

6. Definiowanie `EngagementManagementClient` zmienną, w której użyjemy wywoływanie metod SDK zaangażowania Mobile:

        static EngagementManagementClient engagementClient; 

7. Poniższe czynności, aby dodać do `Main` metody:

        try
            {
                // Initialize the Engagement SDK to call out APIs. 
                InitEngagementClient().Wait();

                // Create a Reach campaign
                CreateCampaign().Wait();
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.InnerException.Message);
                throw ex;
            }

8. Definiowanie następujące metodę, która ma zwracać inicjowania `EngagementManagementClient` , najpierw uwierzytelniania, a następnie kojarząc się z aplikacji zaangażowania Mobile, w którym ma zostać utworzona kampanii zawiadomienie o:

        private static async Task InitEngagementClient()
        {
            var credentials = await ApplicationTokenProvider.LoginSilentAsync(TENANT_ID, CLIENT_ID, CLIENT_SECRET);
            engagementClient = new EngagementManagementClient(credentials) { SubscriptionId = SUBSCRIPTION_ID };
            
            // This is the Azure concept of ResourceGroup
            engagementClient.ResourceGroupName = RESOURCE_GROUP;

            // This is your Mobile Engagement App Collection & App Resource Name in which you create the campaign
            engagementClient.AppCollection = APP_COLLECTION_NAME;
            engagementClient.AppName = APP_RESOURCE_NAME;
        }

    > [AZURE.IMPORTANT] Należy zauważyć, że możesz do korzystania z **Aplikacji Nazwa zasobu** , zgodnie z definicją w portalu zarządzania Azure parametru Nazwa aplikacji. 

9. Ponadto zdefiniować metodę CreateCampaign, która będzie zajmować Tworzenie prostych **w dowolnym momencie**za pomocą wcześniej zainicjowany EngagementClient & **tylko do powiadomień** kampanii z tytułu i wiadomości: 

        private async static Task CreateCampaign()
        {
            //  Refer to the Announcement Campaign format from here - 
            //      https://msdn.microsoft.com/en-us/library/azure/mt683751.aspx
            // Make sure you are passing all the non-optional parameters
            Campaign parameters = new Campaign(
                name:"WelcomeCampaign",
                notificationTitle: "Welcome", 
                notificationMessage: "Thank you for installing the app!",
                type:"only_notif",
                deliveryTime:"any"
                );

            // Refer to the Campaign Kinds from here - https://msdn.microsoft.com/en-us/library/azure/mt683742.aspx
            CampaignStateResult result = 
                await engagementClient.Campaigns.CreateAsync(CampaignKinds.Announcements, parameters);
            Console.WriteLine("Campaign Id '{0}' was created successfully and it is in '{1}' state", result.Id, result.State);
        }

10. Uruchom aplikację konsoli i na utworzenia kampanii będzie wyglądał następująco:

    **Identyfikator kampanii "159" został utworzony pomyślnie i jest w stanie "wersja robocza"**

<!-- Images. -->

[1]: ./media/mobile-engagement-dotnet-sdk-service-api/include-prerelease.png
