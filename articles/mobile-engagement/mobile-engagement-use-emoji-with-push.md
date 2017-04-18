<properties 
    pageTitle="Używanie emotikonów Emoji w ramach zaangażowania Mobile Azure" 
    description="Jak używać emotikonów Emoji w powiadomienia wypychane"     
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

#<a name="use-emoji-emoticon-within-push-notifications"></a>Używanie emotikonów Emoji w powiadomienia Push

Możesz umieścić Emoji emotikony w przypadku wypychanych powiadomień w kilka prostych kroków: 

1. Przede wszystkim trzeba znaleźć Emoji chcesz wysłać w wiadomości. Upewnij się, że Emoji wybierasz będzie obsługiwana przez urządzenia jako producentów urządzenia zająć trochę czasu, aby dodać nowo zatwierdzone Emojis platformy urządzeń. 

2. Na **systemu Windows** — możesz przejść do tego [łącza](http://apps.timwhitlock.info/emoji/tables/unicode) i skopiuj ikonę "Macierzystych".

    ![][7] 

3. Na **Mac** — można znaleźć, że Emojis w aplikacji słownika w obszarze Edycja -> Emoji i symboli.

    ![][6] 

4. Teraz, przejdź do karty **osiągnięcia** portal Azure Mobile zaangażowania. Wybierz typ usługi powiadomień wypychanych (anons, sprawdza itp). W tym przykładzie wybrany powiadomienia push.

5. Określ innego pola powiadomienia, aż przejdziesz tekst powiadomienia. Jest to miejsce, w którym spowoduje dodanie emotikonu Emoji. Możesz umieścić ją tytuł i wiadomość. Należy przeciągać i upuszczać lub kopiować Emoji, że Twoim zdaniem z powyższych lokalizacji. 

    ![][1]

6. Wypełnij inne pola zgłaszania i zapisz go. 

7. Podczas uruchamiania test lub uaktywnienie anons zostanie wyświetlony powiadomienia o emotikon, zgodnie z ustawieniami.   

    ![][3] ![][4] ![][5]

<!-- Images. -->
[1]: ./media/mobile-engagement-use-emoji-with-push/notification_input.png
[3]: ./media/mobile-engagement-use-emoji-with-push/iOS_Emoji.png
[4]: ./media/mobile-engagement-use-emoji-with-push/Android_Emoji.png
[5]: ./media/mobile-engagement-use-emoji-with-push/WindowsPhone_Emoji.png
[6]: ./media/mobile-engagement-use-emoji-with-push/Mac_SelectEmoji.png
[7]: ./media/mobile-engagement-use-emoji-with-push/Windows_SelectEmoji.png

