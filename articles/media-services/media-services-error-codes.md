<properties
    pageTitle="Kody błędów usługi multimediów Azure | Microsoft Azure"
    description="Temat zawiera omówienie usługi multimediów Azure kody błędów."
    authors="Juliako"
    manager="erikre"
    editor=""
    services="media-services"
    documentationCenter=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016" 
    ms.author="juliako"/>

# <a name="azure-media-services-error-codes"></a>Kody błędów usługi multimediów Azure

Podczas korzystania z usługi multimediów Azure firmy Microsoft, może zostać wyświetlony kody błędów HTTP przez usługę, w zależności od problemów, takie jak tokenów uwierzytelniania wygasa do akcji, które nie są obsługiwane w usługach multimediów. Poniżej przedstawiono listę **kody błędów HTTP** , który może zostać zwrócony przez usługi multimediów i możliwe przyczyny dla nich.  
  
## <a name="400-bad-request"></a>400 — Nieprawidłowe żądanie

Żądanie zawiera nieprawidłowe informacje i zostanie odrzucony ze względu na jedną z następujących przyczyn:

- Podano nieobsługiwaną wersję interfejsu API. Do najnowszej wersji zobacz [Ustawienia dotyczące rozwoju multimedia usługi REST interfejsu API](media-services-rest-how-to-use.md).
- Nie określono wersję interfejsu API usługi multimediów. Aby uzyskać informacje dotyczące sposobu określania wersję interfejsu API zobacz [Łączenie się usługi multimediów z interfejsu API usługi REST usługi multimediów](media-services-rest-connect-programmatically.md). 
   
    >[AZURE.NOTE] Jeśli korzystasz z programu .NET lub języka Java SDK nawiązywania połączenia z usługi multimediów, wersja interfejsu API jest określona dla Ciebie zawsze, gdy spróbujesz i wykonywanie określonej akcji przed usługi multimediów.
- Określono nieokreślone właściwości. Nazwa właściwości jest komunikat o błędzie. Można określić tylko te właściwości, które są członkami danej jednostce. Zobacz [Azure multimediów usługi REST API Reference](http://msdn.microsoft.com/library/azure/hh973617.aspx) listę obiektów i ich właściwości.
- Nieprawidłowa wartość właściwości został określony. Nazwa właściwości jest komunikat o błędzie. Zobacz poprzedniego łącza dla typów prawidłowej właściwości i ich wartości.
- Wartość właściwości nie jest widoczny i jest wymagane.
- Część adresu URL określonego zawiera nieprawidłowe wartości.
- Podjęto zaktualizować właściwości WriteOnce.
- Aby utworzyć zadanie, które ma środka trwałego wprowadzania danych przy użyciu podstawowego AssetFile, który nie został określony lub nie można określić nastąpiła próba.
- Wystąpiła próba aktualizacji Locator skojarzeń zabezpieczeń. Locator skojarzeń zabezpieczeń można tylko utworzone lub usunięte. Przesyłanie strumieniowe Locator można aktualizować. Aby uzyskać więcej informacji zobacz [Locator](http://msdn.microsoft.com/library/azure/hh974308.aspx).
- Dostarczono nieobsługiwana operacja lub kwerendy. 

## <a name="401-unauthorized"></a>401 — Brak autoryzacji

Nie można uwierzytelnić żądanie, (przed mogły zostać autoryzowane) z jednej z następujących przyczyn:

- Brak nagłówka uwierzytelniania.
- Wartość nagłówka nieprawidłowe uwierzytelniania.
    - Tokenu wygasła. Jeśli bezpośrednio z pozostałych interfejsów API, zobacz [Łączenie się z usługi multimediów z interfejsu API usługi REST usługi multimediów](media-services-rest-connect_programmatically.md) się dowiedzieć się, jak wygenerować nowy token uwierzytelniania. Jeśli korzystasz z programu .NET lub SDK Java, utworzyć obiektu CloudMediaContext lub MediaContract do wygenerowania tokenu. Aby uzyskać więcej informacji o tym, jak to zrobić zobacz [Łączenie się usługi multimediów z Media SDK usług dla środowiska .NET](media-services-dotnet-connect-programmatically.md).
    - Token zawiera nieprawidłowy podpis.</li></ul></li></ul>

## <a name="403-forbidden"></a>403 Zabroniony

Żądanie nie jest dozwolona z jednej z następujących przyczyn:

- Konto usługi multimediów nie można odnaleźć lub został usunięty.
- Konto usługi multimediów jest wyłączone i typ żądania nie jest HTTP GET. Operacje usług zwróci także 403 odpowiedź.
- Token uwierzytelniania nie zawiera informacjami o poświadczeniach użytkownika: Nazwa konta i/lub SubscriptionId. Te informacje można znaleźć w rozszerzeniu interfejsu użytkownika usługi multimediów dla Twojego konta usługi multimediów w portalu zarządzania Azure.
- Nie można uzyskać dostępu do tego zasobu.
    - Podjęto umożliwia MediaProcessor, która nie jest dostępna dla Twojego konta usługi multimediów.
    - Podjęto zaktualizować obiekt JobTemplate zdefiniowanych przez usługi multimediów.
    - Aby zastąpić Locator niektóre inne usługi multimediów konta wystąpiła próba.
    - Podjęto Aby zastąpić ContentKey niektóre inne usługi multimediów konta.

- Nie można utworzyć zasobu z powodu przydziału usługi osiągnięto dla konta usługi multimediów. Aby uzyskać więcej informacji dotyczących przydziałów usługi zobacz [przydziałów](media-services-quotas-and-limitations.md).

## <a name="404-not-found"></a>404 — Nie odnaleziono

Żądanie nie jest dozwolona na zasób z jednej z następujących przyczyn:

- Podjęto zaktualizować jednostka nie istnieje.
- Aby usunąć obiekt, który nie istnieje nastąpiła próba.
- Aby utworzyć obiekt z łączem do jednostki, która nie istnieje wystąpiła próba.
- Podjęto uzyskanie jednostka nie istnieje.
- Podjęto określone konto miejsca do magazynowania, który nie jest skojarzony z kontem usługi multimediów.  

## <a name="409-conflict"></a>409 Konflikt

Żądanie nie jest dozwolona z jednej z następujących przyczyn:

- Więcej niż jeden AssetFile występują w obrębie elementu o określonej nazwie.
- Aby utworzyć drugi AssetFile podstawowego w obrębie elementu nastąpiła próba.
- Podjęto tworzenie ContentKey z określonym identyfikatorem już używana.
- Podjęto tworzenie Locator z określonym identyfikatorem już używana.
- Więcej niż jeden IngestManifestFile występują w IngestManifest o określonej nazwie.
- Aby połączyć drugi szyfrowania miejsca do magazynowania ContentKey trwałego szyfrowane miejsca do magazynowania nastąpiła próba.
- Podjęto utworzyć łącze ContentKey samego elementu.
- Aby utworzyć locator do środka trwałego, którego kontenera magazynu brakuje lub jest już skojarzone z elementu nastąpiła próba.
- Aby utworzyć locator do środka trwałego, która już zawiera 5 Locator Użyj wystąpiła próba. (Azure magazynowania wymusza limit pięciu zasad udostępniania w jeden kontener miejsca do magazynowania).
- Łączenie konta miejsca do magazynowania środka trwałego do IngestManifestAsset nie jest taki sam, jak konto miejsca do magazynowania używane przez nadrzędny IngestManifest.  

## <a name="500-internal-server-error"></a>500 — Wewnętrzny błąd serwera

Podczas przetwarzania żądania usługi multimediów napotka wystąpił błąd, który uniemożliwia z dalszego przetwarzania. Może to być spowodowane jedną z następujących przyczyn:

- Tworzenie zasobów lub zadań zakończy się niepowodzeniem, ponieważ informacje o przydziałach usługi konta usługi multimediów jest tymczasowo niedostępny.
- Tworzenie zawartości lub IngestManifest kontenera magazyn obiektów blob zakończy się niepowodzeniem, ponieważ informacje o koncie miejsca do magazynowania dla konta jest tymczasowo niedostępny.
- Inne nieoczekiwany błąd. 

## <a name="503-service-unavailable"></a>503 — Usługa jest niedostępna

Serwer jest obecnie nie może odebrać żądania. Ten błąd może być spowodowany nadmiarowe żądania usługi. Usługi multimediów ograniczania mechanizmu ogranicza użycie zasobów dla aplikacji, które wprowadzić nadmiarowe żądanie usługi.

>[AZURE.NOTE]Sprawdź komunikat o błędzie i ciąg kodu błędu, aby uzyskać bardziej szczegółowe informacje dotyczące przyczyny, że masz błąd 503. Ten błąd nie zawsze oznacza ograniczania.

Opisy stanu możliwe są następujące:

- "Serwer jest zajęty. Poprzedni uruchamia ten typ żądania trwała dłużej niż {0} s."
- "Serwer jest zajęty. Więcej niż żądania {0} na sekundę może być ograniczenie."
- "Serwer jest zajęty. Więcej niż żądania {0} w ciągu {1} sekund może być ograniczenie."

Aby obsługiwać ten błąd, zaleca się przy użyciu Logika ponawiania wykładniczego wycofywania. Oznacza to, za pomocą stopniowo dłużej oczekiwania między kolejnymi próbami odpowiedzi następujących po sobie błędów.  Aby uzyskać więcej informacji zobacz [Blokowanie aplikacji obsługi błędów przejściowych](https://msdn.microsoft.com/library/hh680905.aspx). 

>[AZURE.NOTE]Jeśli korzystasz z [Zestawu SDK usługi multimediów Azure dla środowiska .net](https://github.com/Azure/azure-sdk-for-media-services/tree/master)Logika ponawiania dla błąd 503 została zaimplementowana przez zestaw SDK.  
  
## <a name="see-also"></a>Zobacz też  

[Kody błędów zarządzania usługi multimediów](http://msdn.microsoft.com/library/windowsazure/dn167016.aspx)

## <a name="next-steps"></a>Następne kroki

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
