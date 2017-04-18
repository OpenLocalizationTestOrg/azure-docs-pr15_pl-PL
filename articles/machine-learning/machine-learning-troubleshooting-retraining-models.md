<properties
    pageTitle="Rozwiązywanie problemów z Retraining usługi sieci Web Azure maszynowego uczenia klasyczny | Microsoft Azure"
    description="Identyfikowanie i poprawianie typowych problemów encounted podczas usługi sieci Web Azure maszynowego uczenia się szkoleniem modelu."
    services="machine-learning"
    documentationCenter=""
    authors="VDonGlover"
   manager="raymondl"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="v-donglo"/>

# <a name="troubleshooting-the-retraining-of-an-azure-machine-learning-classic-web-service"></a>Rozwiązywanie problemów z przeszkolenie usługi sieci Web Azure maszynowego uczenia klasyczny

## <a name="retraining-overview"></a>Przeszkolenie — omówienie

Po wdrożeniu eksperyment przewidywanych wyników usług sieci web jest model statyczny. Gdy nowe dane stał się dostępny lub konsumenta interfejsu API ma własne dane, model musi być retrained. 

Aby zapoznać się z pełną opisem przekwalifikowania proces usługi sieci Web klasyczny zobacz [Przy przekwalifikowaniu maszynowego uczenia modeli programowy](machine-learning-retrain-models-programmatically.md).

## <a name="retraining-process"></a>Przeszkolenie proces

Gdy trzeba przekwalifikować usługi sieci Web, należy dodać kilka dodatkowych elementów:

* Używany z doświadczenia szkolenia dotyczące usługi sieci Web. Doświadczenia musi być dołączone do pliku modułu **Modelu pociągu** moduł **Wyjściowych usługi sieci Web** .  

    ![Dołącz wyjściowych usługi sieci web do modelu pociągu.][image1]

* Nowy punkt końcowy dodane do wyników usługi sieci Web.  Możesz dodać punkt końcowy programowo przy użyciu przykładowy kod odwoływać się w modelach przy przekwalifikowaniu maszynowego uczenia programowo tematu lub za pośrednictwem portalu klasyczny Azure.

Przykładowy kod C# ze strony pomocy interfejsu API usługi sieci Web szkolenie umożliwia następnie przy przekwalifikowaniu modelu. Po oszacowania wyników i ich zadowalające, możesz zaktualizować przeszkolony modelu usługi sieci web przy użyciu nowy punkt końcowy dodanego wyników.

Z wszystkie wymagane elementy w miejscu głównych kroków, które należy wykonać, aby przy przekwalifikowaniu modelu są następujące:

1.  Połączenie usługi sieci Web szkolenie: połączenie jest do partii wykonanie usługi (BES), nie żądanie odpowiedź usługi (RR). Przykładowy kod C# na stronie pomocy interfejsu API umożliwia nawiązać połączenie. 
2.  Znajdowanie wartości dla *BaseLocation*, *RelativeLocation*i *SasBlobToken*: te wartości są zwracane w wyniku kwerendy z połączenia do szkolenia dotyczące usługi sieci Web. 
      ![Wyświetlanie danych wyjściowych przekwalifikowania próbki i BaseLocation, RelativeLocation i SasBlobToken wartości.][image6]
3.  Aktualizowanie dodano punkt końcowy z wyników usługi sieci web z nowym modelem przeszkolony: za pomocą kodu próbki opisane w modelach przy przekwalifikowaniu maszynowego uczenia programistycznie, zaktualizuj nowy punkt końcowy, dodane do modelu wyników z modelem nowo przeszkolony z usługi sieci Web szkolenie.

## <a name="common-obstacles"></a>Typowe przeszkód

### <a name="check-to-see-if-you-have-the-correct-patch-url"></a>Sprawdź, czy masz poprawny adres URL poprawki

Adres URL poprawki, którego używasz musi być skojarzony z nowy wyników punkt końcowy, dodanych do wyników usługi sieci Web. Istnieje wiele metod uzyskiwania adresu URL poprawka:

**Opcja 1: filmowych**

Aby uzyskać poprawny adres URL poprawka:

1.  Uruchamiać kod przykładowy [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) .
2.  Z wyników AddEndpoint znaleźć wartość *HelpLocation* , a następnie skopiuj adres URL.

    ![HelpLocation w wyniku kwerendy próbki addEndpoint.][image2]

3.  Wklej adres URL w przeglądarce, aby przejść do strony, na której znajdują się łącza pomocy usługi sieci Web.
4.  Kliknij łącze **Zasobu aktualizacji** , aby otworzyć stronę pomocy poprawki.

**Opcja 2: Za pomocą portalu klasyczny Azure**

1.  Zaloguj się do [portalu klasyczny Azure](https://manage.windowsazure.com).
2.  Otwórz kartę nauki komputera. 
     ![Karta oparcie komputera.][image4]
3.  Kliknij swoją nazwę obszaru roboczego, a następnie **Usług sieci Web**.
4.  Kliknij pozycję wyników pracy przy użyciu usługi sieci Web. (Jeśli nie zmodyfikować domyślną nazwę usługi sieci web, będzie kończą się [wyników Exp.]).
5.  Kliknij przycisk **Dodaj punkt końcowy**.
6.  Po dodaniu punktu końcowego, kliknij nazwę punktu końcowego. Następnie kliknij pozycję **Zasobu aktualizacji** , aby otworzyć stronę poprawiania pomocy.

>[AZURE.NOTE] Jeśli dodano punkt końcowy do usługi sieci Web szkolenie zamiast przewidywanych usługi sieci Web, otrzymasz następujący komunikat o błędzie, po kliknięciu łącza **Zasobu aktualizacji** : Niestety, ale ta funkcja nie jest obsługiwane lub dostępne w tym kontekście. Ta usługa sieci Web ma nie można aktualizować zasobów. Firma Microsoft Przepraszamy za niedogodności i pracujemy nad poprawy ten przepływ pracy.

![Nowy punkt końcowy pulpit nawigacyjny.][image3]

Strona pomocy poprawki zawiera adres URL poprawki, należy użyć i udostępnia przykładowy kod, który umożliwia połączenie go.

![Adres URL poprawki.][image5]

### <a name="check-to-see-that-you-are-updating-the-correct-scoring-endpoint"></a>Sprawdź aktualizujesz właściwego wyników punktu końcowego

* Nie poprawka usługi sieci Web szkolenie: operacja poprawki należy wykonać na wyników usługi sieci Web.
* Nie poprawka punktu końcowego usługi sieci Web: operacja poprawki należy wykonać na nowy wyników Web punktu końcowego usługi dodanego.

Można sprawdzić, które usługa sieci Web punkt końcowy jest w portalu klasyczny Azure. 

>[AZURE.NOTE] Upewnij się, że chcesz dodać punkt końcowy do przewidywanych usługi sieci Web nie szkolenia dotyczące usługi sieci Web. Jeśli wdrożono poprawnie, zarówno szkolenia i przewidywanych usługi sieci Web, zobacz dwóch oddzielnych usług sieci Web na liście. Usługa sieci Web przewidywanych należy kończy się "[exp przewidywanych.]".

1.  Zaloguj się do [portalu klasyczny Azure](https://manage.windowsazure.com).
2.  Otwórz kartę nauki komputera. 
     ![Obszar roboczy nauki maszynowego interfejsu użytkownika.][image4]
3.  Wybierz pozycję obszaru roboczego.
4.  Kliknij pozycję **usług sieci Web**.
5.  Wybierz pozycję Usługa sieci Web przewidywanych.
6.  Upewnij się, że usługi nowy punkt końcowy został dodany do usługi sieci Web.

### <a name="check-the-workspace-that-your-web-service-is-in-to-ensure-it-is-in-the-correct-region"></a>Sprawdź obszar roboczy, w której usługa sieci web aby upewnić się, że jest poprawny region

1.  Zaloguj się do [portalu klasyczny Azure](https://manage.windowsazure.com).
2.  Z menu wybierz nauki komputera.
      ![Region nauki maszynowego interfejsu użytkownika.][image4]
3.  Zweryfikuj lokalizację obszaru roboczego.

<!-- Image Links -->

[image1]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-studio-tm-connnected-to-web-service-out.png
[image2]: ./media/machine-learning-troubleshooting-retraining-a-model/addEndpoint-output.png
[image3]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-update-resource.png
[image4]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-machine-learning-tab.png
[image5]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-help-page-patch-url.png
[image6]: ./media/machine-learning-troubleshooting-retraining-a-model/retraining-output.png
[image7]: ./media/machine-learning-troubleshooting-retraining-a-model/web-services-tab.png