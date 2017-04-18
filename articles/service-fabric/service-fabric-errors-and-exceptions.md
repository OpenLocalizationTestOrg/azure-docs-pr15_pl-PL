<properties
   pageTitle="Typowe wyjątki FabricClient generowane | Microsoft Azure"
   description="W tym artykule opisano typowe wyjątki i błędy, które może zostać wygenerowany przez interfejsy API FabricClient podczas wykonywania aplikacji klaster operacji i zarządzania."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>

# <a name="common-exceptions-and-errors-when-working-with-the-fabricclient-apis"></a>Typowych błędów podczas pracy z interfejsów API FabricClient i wyjątki
Interfejsy API [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) umożliwiają wykonywanie zadań administracyjnych na tkaninie usługi aplikacji, usług bądź klaster klaster i aplikacji administratorom. Na przykład wdrażanie aplikacji, uaktualnianie i usuwania sprawdzania kondycji klastrze lub testowania usługi. Deweloperów aplikacji i klaster Administratorzy mogą używać interfejsów API FabricClient opracowywaniu narzędzia do zarządzania klaster tkaninie usług i aplikacji.

Istnieje wiele różnych rodzajów działań, które mogą być wykonywane przy użyciu FabricClient.  Każda z metod może zostać zgłoszony wyjątki błędów z powodu niepoprawnych danych wejściowych, błędy wykonania lub przejściowych infrastruktury problemy.  Zobacz dokumentacji odwołanie interfejsu API, aby dowiedzieć się, jakie wyjątki są generowane za pomocą metody określonej. Istnieje kilka wyjątków, jednak których może zostać wygenerowany przez wiele różnych interfejsów API [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) . W poniższej tabeli przedstawiono wyjątki, które są wspólne dla całej interfejsy API FabricClient.

|Wyjątku| Kiedy generowany|
|---------|:-----------|
|[System.Fabric.FabricObjectClosedException](https://msdn.microsoft.com/library/system.fabric.fabricobjectclosedexception.aspx)|Obiekt [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) jest w stanie zamkniętym. Dysponować obiektu [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) używasz i utworzyć nowy obiekt [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) . |
|[System.TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx)|Przekroczono limit czasu tej operacji. Podczas tej operacji więcej niż MaxOperationTimeout do wykonania, jest zwracany [OperationTimedOut](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx) .|
|[System.UnauthorizedAccessException](https://msdn.microsoft.com/en-us/library/system.unauthorizedaccessexception.aspx)|Nie można sprawdzić dostępu dla operacji. Zwracany jest błąd E_ACCESSDENIED.|
|[System.Fabric.FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx)|Wystąpił błąd wykonania podczas wykonywania operacji. Jedną z metod FabricClient potencjalnie zostać zgłoszony [FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx), właściwość [Kod błędu](https://msdn.microsoft.com/library/system.fabric.fabricexception.errorcode.aspx) wskazuje dokładną przyczynę wyjątek. Kody błędów są definiowane w wyliczenia [FabricErrorCode](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx) .|
|[System.Fabric.FabricTransientException](https://msdn.microsoft.com/library/system.fabric.fabrictransientexception.aspx)|Niepowodzenie z powodu błędu przejściowych identyfikacji. Na przykład operacji może zakończyć się niepowodzeniem, ponieważ tymczasowo kworum replik nie jest dostępne. Wyjątki przejściowych odpowiadają niepowodzeniu operacji, które mogą być wycofane.|

Niektóre typowe błędy [FabricErrorCode](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx) , które mogą zostać zwrócone w [FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx):

|Błąd| Warunek|
|---------|:-----------|
|CommunicationError|Błąd komunikacji spowodowany operację, która będzie się nie powieść, spróbuj jeszcze raz.|
|InvalidCredentialType|Typ poświadczeń jest nieprawidłowy.|
|InvalidX509FindType|X509FindType jest nieprawidłowy.|
|InvalidX509StoreLocation|X509 lokalizacja przechowywania jest nieprawidłowy.|
|InvalidX509StoreName|X509 nazwa magazynu jest nieprawidłowy.|
|InvalidX509Thumbprint|X509 ciąg odcisku palca certyfikatu jest nieprawidłowy.|
|InvalidProtectionLevel|Poziom ochrony jest nieprawidłowy.|
|InvalidX509Store|X509 nie można otworzyć magazynu certyfikatów.|
|InvalidSubjectName|Nazwa tematu jest nieprawidłowa.|
|InvalidAllowedCommonNameList|Format typowych ciąg listy Nazwa jest nieprawidłowy. Powinno być rozdzielaną średnikami listę.|
