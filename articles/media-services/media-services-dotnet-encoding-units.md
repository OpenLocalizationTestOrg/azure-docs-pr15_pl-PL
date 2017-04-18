<properties 
    pageTitle="Jak dodać kodowania jednostki" 
    description="Dowiedz się, jak dodawanie kodowania jednostek .NET"  
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
    ms.date="09/01/2016"
    ms.author="juliako;milangada;gtrifonov"/>


#<a name="how-to-scale-encoding-with-net-sdk"></a>Jak skalowanie kodowania z .NET SDK

> [AZURE.SELECTOR]
- [Portal](media-services-portal-scale-media-processing.md )
- [.NET](media-services-dotnet-encoding-units.md)
- [POZOSTAŁE](https://msdn.microsoft.com/library/azure/dn859236.aspx)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

##<a name="overview"></a>Omówienie

>[AZURE.IMPORTANT] Upewnij się przejrzeć temat [Omówienie](media-services-scale-media-processing-overview.md) , aby uzyskać więcej informacji na temat skalowania multimediów przetwarzania tematu.
 
Aby zmienić typ jednostki zastrzeżone i liczba kodowanie zastrzeżone jednostki przy użyciu zestawu SDK .NET, wykonaj następujące czynności:

    IEncodingReservedUnit encodingS1ReservedUnit = _context.EncodingReservedUnits.FirstOrDefault();
    encodingS1ReservedUnit.ReservedUnitType = ReservedUnitType.Basic; // Corresponds to S1
    encodingS1ReservedUnit.Update();
    Console.WriteLine("Reserved Unit Type: {0}", encodingS1ReservedUnit.ReservedUnitType);
    
    encodingS1ReservedUnit.CurrentReservedUnits = 2;
    encodingS1ReservedUnit.Update();
    
    Console.WriteLine("Number of reserved units: {0}", encodingS1ReservedUnit.CurrentReservedUnits);

##<a name="opening-a-support-ticket"></a>Otwieranie bilet pomocy technicznej

Domyślnie każdy konta usługi multimediów można skalować maksymalnie 25 kodowanie i 5 na żądanie Streaming zastrzeżone jednostki. Możesz zażądać wyższy limit, otwierając bilet pomocy technicznej.

###<a name="open-a-support-ticket"></a>Otwórz bilet pomocy technicznej

Aby otworzyć Pomoc techniczna biletów wykonaj następujące czynności:

1. Kliknij pozycję [Pomoc](https://manage.windowsazure.com/?getsupport=true). Jeśli użytkownik nie jest zalogowany, wyświetli się monit o wprowadzenie poświadczeń.

1. Wybierz subskrypcję.

1. W obszarze Typ pomocy technicznej wybierz pozycję "Techniczne".

1. Polecenie "Tworzenie biletów".

1. Wybierz pozycję "Usługi multimediów Azure" na liście produktów przedstawione na następnej stronie.

1. Wybieranie typu"Problem" odpowiednią dla problemu.

1. Kliknij przycisk Kontynuuj.

1. Postępuj zgodnie instrukcjami na następnej stronie, a następnie wprowadź szczegółowe informacje o problemu.

1. Kliknij przycisk Zatwierdź, aby otworzyć bilet.



##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
