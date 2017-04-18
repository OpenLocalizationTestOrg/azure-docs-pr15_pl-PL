<properties
   pageTitle="Skalowanie usługi sieci web | Microsoft Azure"
   description="Dowiedz się, jak skalowanie usługi sieci web przez zwiększanie współbieżności oraz dodawania nowych punktów końcowych."
   services="machine-learning"
   documentationCenter=""
   authors="neerajkh"
   manager="srikants"
   editor="cgronlun"
   keywords="Azure maszynowego uczenia, usług sieci web, operationalization skalowania, punktu końcowego współbieżności"
   />
<tags
   ms.service="machine-learning"
   ms.devlang="NA"
   ms.workload="data-services"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.date="10/05/2016"
   ms.author="neerajkh"/>

# <a name="scaling-a-web-service"></a>Skalowanie usługi sieci Web

>[AZURE.NOTE] W tym temacie opisano techniki dotyczące usługi sieci Web uczenia maszynowego klasyczny. 

Domyślnie każdy opublikowanych usługi sieci Web jest skonfigurowana do obsługi 20 równoczesne żądania i można możliwie jak 200 równoczesne żądania. Gdy portalu klasyczny Azure umożliwia ta wartość, Azure maszynowego uczenia automatycznie optymalizuje to ustawienie, aby zapewnić najlepszą wydajność usługi sieci web i portalu wartość jest ignorowana. 

Jeśli plan nawiązać połączenie z interfejsu API z obciążeniem wyższymi niż określona wartość maksymalna liczba wywołania 200 będzie obsługiwać, należy utworzyć wiele punktów końcowych na tej samej usługi sieci Web. Następnie można losowo rozłożenie usługi obciążenia w każdy z nich.

## <a name="add-new-endpoints-for-same-web-service"></a>Dodaj nowe punkty końcowe w tym samym usługi sieci web

Skalowanie usługi sieci Web jest typowych zadań. Przyczyny przeskalować mają obsługuje więcej niż 200 równoczesne żądania, zwiększyć dostępność za pośrednictwem wiele punktów końcowych lub podać osobnych punkty końcowe usługi sieci web. Skala można zwiększyć, dodając dodatkowe punkty końcowe usługi sieci Web za pomocą [portal Azure klasyczny](https://manage.windowsazure.com/) lub z portalu [Usługi sieci Web uczenia Azure](https://services.azureml.net/) .

Aby uzyskać więcej informacji na temat dodawania nowych punktów końcowych zobacz [Tworzenie punktów końcowych](machine-learning-create-endpoint.md).

Należy pamiętać, używanym zestawienia współbieżności duży może być niekorzystne, jeśli nie dzwonisz interfejsu API o wysokim częstotliwości. Jeśli umieszczenie małą obciążenia na interfejs API skonfigurowane dla wysokie obciążenie może zostać wyświetlony z sporadycznie limity czasu i/lub tych najwyższych wartościach opóźnienia.

Synchroniczne interfejsy API zwykle są używane w sytuacjach, gdzie potrzeby krótki czas oczekiwania. W tym polu Opóźnienie oznacza czasu potrzebnego na interfejsu API do wykonania jedno żądanie, a nie stanowią opóźnień w sieci. Załóżmy, że masz interfejsem API z opóźnienie 50 ms. Aby w pełni wykorzystać pojemność ograniczenia poziomu Wysoki i Max wywołania = 20; trzeba zadzwonić ten interfejs API 20 * 1000 / 50 = 400 times na sekundę. Rozszerzanie to dodatkowo, Max wywołania 200 umożliwia nawiązywanie połączenia razy 4000 interfejsu API na sekundę, przy założeniu opóźnienie 50 ms.

<!--Image references-->
[1]: ./media/machine-learning-scaling-webservice/machlearn-1.png
[2]: ./media/machine-learning-scaling-webservice/machlearn-2.png
