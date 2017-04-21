<properties
   pageTitle="Tytuł strony, jaki będzie wyświetlany w przeglądarce karty i wyników wyszukiwania"
   description="Opis artykułu, który pojawi się na strony docelowe, a w większości wyników wyszukiwania"
   services="service-name"
   documentationCenter="dev-center-name"
   authors="GitHub-alias-of-only-one-author"
   manager="manager-alias"
   editor=""/>

<tags
   ms.service="required"
   ms.devlang="may be required"
   ms.topic="article"
   ms.tgt_pltfrm="may be required"
   ms.workload="required"
   ms.date="mm/dd/yyyy"
   ms.author="Your MSFT alias or your full email address;semicolon separates two or more"/>

# <a name="markdown-template-article-heading-1-or-h1-heading-at-the-top-of-the-article"></a>Szablon promocji cenowych (Artykuł Nagłówek 1 lub H1): nagłówek w górnej części tego artykułu

Aby skopiować promocji cenowych przy użyciu tego szablonu, skopiuj tego artykułu w sieci lokalnej repo, lub kliknij przycisk nieprzetworzonych w Interfejsie użytkownika GitHub i skopiuj promocji cenowych.

  ![Tekst alternatywny; nie jest pusta. Opis obrazu.][8]

Akapit wprowadzający: Lorem dolor amet, adipiscing elit. Phasellus interdum nulla risus, lacinia porta nisl imperdiet sed. Mauris dolor mauris, tempus sed lacinia nec, felis innych niż euismod. Nunc semper porta ultrices. Maecenas neque nulla, condimentum vitae ipsum znajdują się amet, dignissim aliquet nisi.

## <a name="heading-2-h2"></a>Nagłówek 2 (H2)

Aenean znajdują się amet leo nec purus placerat fermentum ac gravida odio. Aenean tellus lectus faucibus w rhoncus w faucibus sed urna.  volutpat mi identyfikator purus ultrices iaculis nec innych niż neque. [Tekst łącza dla łącza poza azure.microsoft.com](http://weblogs.asp.net/scottgu). Nullam dolor dictum w aliquam pharetra. Vivamus ac hendrerit mauris [Przykładowy tekst łącza łącze do artykułu w folderze usługi](../articles/expressroute/expressroute-bandwidth-upgrade.md).

Pobrać 10 razy większy ruch z [usługi Google]  [ gog] niż z [Yahoo]  [ yah] lub [MSN] [msn].

> [AZURE.NOTE] Tekst z wcięciem Uwaga.  Wyraz "Uwaga" zostanie dodana w publikacji. Ut lacus pretium Unii Europejskiej. Nullam purus est fikacje est sed iaculis, euismod vehicula odio. Curabitur lacinia, erat tristique iaculis rutrum, erat sem sodales nisi Unii Europejskiej condimentum turpis nisi purus.

1. Aenean znajdują się amet leo nec **Purus** placerat fermentum ac gravida odio.

2. Aenean tellus lectus faucius w **Rhoncus** w faucibus sed urna. Suspendisse volutpat mi identyfikator purus ultrices iaculis nec innych niż neque.

    ![Tekst alternatywny; nie jest pusta. Samochód zbierających dane w wyścigowe czerwony.][5]

3. Nullam dolor dictum w aliquam pharetra. Vivamus ac hendrerit mauris. Sed dolor dui, condimentum i varius a vehicula na nisl.

    ![Tekst alternatywny; nie jest pusta][6]


Suspendisse volutpat mi identyfikator purus ultrices iaculis nec innych niż neque. Nullam dolor dictum w aliquam pharetra. Vivamus ac hendrerit mauris. Otrus informatus: [1 łącza do innego tematu dokumentacji azure.microsoft.com](virtual-machines-windows-hero-tutorial.md)

## <a name="heading-h2"></a>Nagłówek (H2)

Ut lacus pretium Unii Europejskiej. Nullam purus est fikacje est sed iaculis, euismod vehicula odio.

1. Curabitur lacinia, erat tristique iaculis rutrum, erat sem sodales nisi Unii Europejskiej condimentum turpis nisi purus.

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:
        (NSDictionary *)launchOptions
        {
            // Register for remote notifications
            [[UIApplication sharedApplication] registerForRemoteNotificationTypes:
            UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];
            return YES;
        }

2. Vestibulum badanie ipsum primis w faucibus orci luctus i ultrices posuere cubilia.

        // Because toast alerts don't work when the app is running, the app handles them.
        // This uses the userInfo in the payload to display a UIAlertView.
        - (void)application:(UIApplication *)application didReceiveRemoteNotification:
        (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
            [userInfo objectForKey:@"inAppMessage"] delegate:nil cancelButtonTitle:
            @"OK" otherButtonTitles:nil, nil];
            [alert show];
        }


    > [AZURE.NOTE] Duis sed diam nie <i>nisl molestie</i> pharetra eget est. [Łącze 2 do innego tematu dokumentacji azure.microsoft.com](web-sites-custom-domain-name.md)


Quisque commodo eros fikacje lectus euismod auctor eget znajdują się amet leo. Proin faucibus suscipit tellus dignissim ultrices.

## <a name="heading-2-h2"></a>Nagłówek 2 (H2)

1. Maecenas sed condimentum nisi. Suspendisse potenti.

  + Fusce
  + Malesuada
  + Funkcji mapowania zdarzeń scenariusza

2. Nullam w massa Unii Europejskiej tellus tempus hendrerit.

    ![Tekst alternatywny; nie jest pusta. Przykład klipu wideo kanału 9.][7]

3. Quisque felis enim, fermentum ut aliquam nec, pellentesque pulvinar magna.




<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Następne kroki

Vestibul badanie ipsum primis w faucibus orci luctus i ultrices posuere cubilia Curae; Ultricies Nullam, ipsum vitae volutpat hendrerit, purus diam pretium eros, vitae tincidunt nulla lorem sed turpis: [3 łącze do innego tematu dokumentacji azure.microsoft.com](storage-whatis-account.md).

<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png
[8]: ./media/markdown-template-for-new-articles/copytemplate.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[gog]: http://google.com/        
[yah]: http://search.yahoo.com/  
[msn]: http://search.msn.com/    
