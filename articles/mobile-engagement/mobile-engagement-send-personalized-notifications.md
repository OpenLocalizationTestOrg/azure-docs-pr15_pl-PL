<properties 
    pageTitle="Wysyłanie spersonalizowanych powiadomienia o zaangażowania Mobile Azure" 
    description="Jak wysłać spersonalizowane powiadomienia, dołączając informacji dotyczących profilów użytkowników w powiadomieniach, takie jak ich nazwy"        
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="all" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="personalize-notifications-by-including-user-name"></a>Dostosować powiadomienia, łącznie z nazwą użytkownika

Do zadania, aby wprowadzić bardziej atrakcyjny powiadomienia użytkowników aplikacji należy rozważyć, personalizowanie je. Jeden ze sposobów zaawansowanych obejmuje selektywne adresu powiadomienia w celu ułatwienia ich więcej osobistych przy użyciu nazw użytkowników aplikacji. Wyrazu tutaj - ostrożności powinna wynosić dodawania nazw użytkowników do powiadomienia o jego starannie ponieważ jeśli używać zbyt wielu tej strategii następnie go może również spotkać w porządku dla niektórych użytkowników aplikacji. Należy upewnić się, że są co korzystania z umożliwia te dane osobowe z pełną zgodę w aplikacji dla urządzeń przenośnych przed rozpoczęciem należy korzystać z tego użytkownika. 

Technicznego z zaangażowania Mobile Azure, można wykonywać personalizowanie powiadomienia o jego, wykonując poniższe czynności, w których użyjemy scenariusz łącznie z nazwą użytkownika w powiadomieniach. Użyje pojęcia informacje o aplikacji lub znaczniki, w których wartości albo może być przekazywane przez SDK zintegrowane w aplikacji Mobile lub za pośrednictwem interfejsów API. Następnie można użyć tych informacje o aplikacji lub znaczników:

1. dla kierowanie powiadomienia o określonym użytkownikom na podstawie wartości informacje o aplikacji lub 
2. jako symbole zastępcze w powiadomień, które zostaną zastąpione wartościami specyficzne dla urządzenia/użytkownika podczas wysyłania powiadomień na tym urządzeniu. 

> [AZURE.IMPORTANT] Należy zauważyć, że szybkości wysyłania powiadomień zostaną wyświetlone zmniejszenia ze względu na to dodatkowe przetwarzanie zastępowania wartości informacje o aplikacji z każdego powiadomienia. 

##<a name="register-app-info-in-the-mobile-engagement-portal"></a>Rejestrowanie informacji o aplikacji w portalu zaangażowania urządzeń przenośnych

1) Rozpoczyna się rejestrowanie w portalu Azure Info aplikacji lub znaczników. Przejdź do pozycji **Ustawienia** -> **znacznika (informacje o aplikacji)** w tym.  

![][1]  

2) Kliknij **Nowy znacznik (informacje o aplikacji)** i podaj nazwę jako *nazwa_użytkownika* i wpisz *ciąg znaków* , a następnie kliknij przycisk **Prześlij**. 

![][2]

3) Pojawią się na końcu tej aplikacji — informacje zarejestrowane podobnej do następującej:

![][3]

##<a name="send-app-info-from-the-client-sdk"></a>Wysyłanie informacji o aplikacji w kliencie SDK

W tym miejscu jest używany system Windows uniwersalny przykład aplikacji, ale istnieją również dla naszych innych SDK odpowiedniej metody. 

Przy założeniu ma metody w aplikacji dla urządzeń przenośnych miejsce, w którym zostanie wyświetlony informacje w profilu użytkownika, takie jak nazwy prawdopodobnie po uwierzytelnieniu je, nawiąże połączenie `SendAppInfo` sposób i wypełnić wartość `user_name` informacje o aplikacji, wcześniej zarejestrowanego do wewnętrznej bazy danych usługi zaangażowania Mobile. 

    Dictionary<object, object> appInfo = new Dictionary<object, object>();
    appInfo.Add("user_name", str);
    EngagementAgent.Instance.SendAppInfo(appInfo); 

##<a name="send-personalized-notifications"></a>Wysyłanie spersonalizowanych powiadomienia

Teraz możesz są ustawione do wysyłania powiadomień przy użyciu tego **nazwa_użytkownika**. 

1) Przejdź do portalu zaangażowania Mobile na karcie **osiągnięcia** , aby utworzyć powiadomienie i za pomocą tego symbolu zastępczego w następującym formacie w dowolnym miejscu w treści lub tytuł powiadomienia. 

![][4]  

> [AZURE.NOTE] Wszyscy użytkownicy, dla których informacje aplikacji nazwa_użytkownika nie zostanie ustawiona, nie otrzyma powiadomienie. Po uruchomieniu kampanii powiadomienie w trybie testowym, a jeśli nie masz informacje aplikacji, ustaw, a następnie wyślemy "?" znaku zamienić symbol zastępczy. 

2) Gdy zaangażowania Mobile będzie wybierz urządzenie wysłania powiadomienia będą przeglądanie tych informacji aplikacji, i Zamień wartość w symbolu zastępczym.  
Na przykład jeśli firma Microsoft `str = "Scott"` dla użytkownika niż urządzenie rejestracji otrzymają skojarzone z informacji o aplikacji z **nazwa_użytkownika = SCOTTA** dla tego użytkownika i tego użytkownika są widoczne w nowym oknie powiadomienia push aplikacji w następującym formacie. 

![][5]  

<!-- Images. -->
[1]: ./media/mobile-engagement-send-personalized-notifications/app-info.png
[2]: ./media/mobile-engagement-send-personalized-notifications/create-app-info.png
[3]: ./media/mobile-engagement-send-personalized-notifications/app-info-user-name.png
[4]: ./media/mobile-engagement-send-personalized-notifications/personal-notification.png
[5]: ./media/mobile-engagement-send-personalized-notifications/notification.png

