<properties 
    pageTitle="Omówienie programu Silverlight SDK Windows Phone" 
    description="Omówienie Windows Phone Silverlight SDK dla Azure zaangażowania urządzeń przenośnych"                     
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

#<a name="windows-phone-silverlight-sdk-overview-for-azure-mobile-engagement"></a>Windows Phone omówienie zestaw SDK programu Silverlight dla Azure zaangażowania urządzeń przenośnych

Rozpocznij tutaj uzyskać szczegółowe informacje na temat jak zintegrować zaangażowania Mobile Azure w aplikacji Silverlight dla Windows Phone. Jeśli chcesz spróbuj najpierw, upewnij się, że możesz ukończyć nasz [Samouczek 15 minut](mobile-engagement-windows-phone-get-started.md).

Kliknij, aby wyświetlić [SDK zawartości](mobile-engagement-windows-phone-sdk-content.md)

##<a name="integration-procedures"></a>Procedury integracji

1. Rozpocznij tutaj: [jak zintegrować zaangażowania Mobile w aplikacji Silverlight Windows Phone](mobile-engagement-windows-phone-integrate-engagement.md)

2. Powiadomienia: [jak zintegrować Reach (powiadomienia) w aplikacji Silverlight Windows Phone](mobile-engagement-windows-phone-integrate-engagement-reach.md)

3. Znakowanie wprowadzenia w życie planu: [jak za pomocą zaawansowanych zaangażowania Mobile znakowania interfejsu API w aplikacji Silverlight Windows Phone](mobile-engagement-windows-phone-use-engagement-api.md)

##<a name="release-notes"></a>Informacje o wersji

###<a name="330-04192016"></a>3.3.0 (2016-04-19)
Część *MicrosoftAzure.MobileEngagement* nuget pakietu **v3.4.0**

-   Dzienniki konsoli dodane "TestLogLevel" interfejsu API do Włączanie i wyłączanie/filtru wysyłane przez zestaw SDK.

Poprzednia wersja zobacz [pełne informacje o wersji](mobile-engagement-windows-phone-release-notes.md)

##<a name="upgrade-procedures"></a>Procedury uaktualniania

Jeśli już masz zintegrowany ze starszej wersji programu naszych SDK do aplikacji, należy wziąć pod uwagę następujące punkty podczas uaktualniania zestawu SDK.

Być może trzeba wykonać kilka procedury nie odebrano kilka wersji pakietu. Zobacz pełny opis [Procedur uaktualniania](mobile-engagement-windows-phone-upgrade-procedure.md). Na przykład możesz przeprowadzić migrację z 0.10.1 do 0.11.0, musisz najpierw należy wykonać procedurę "od 0.9.0 do 0.10.1" następnie procedury "od 0.10.1 do 0.11.0".

###<a name="from-200-to-330"></a>Z 2.0.0 do 3.3.0

####<a name="test-logs"></a>Testowanie dzienników

Dzienniki konsoli tworzone przez zestawu SDK można teraz włączone/wyłączone i filtrowane. Aby dostosować tak, zaktualizuj właściwość `EngagementAgent.Instance.TestLogEnabled` do jednej z wartości dostępne na `EngagementTestLogLevel` wyliczenie, na przykład:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="upgrade-from-older-versions"></a>Uaktualnienie starszych wersji

Zobacz [Uaktualnianie procedury](mobile-engagement-windows-phone-upgrade-procedure.md)
 
