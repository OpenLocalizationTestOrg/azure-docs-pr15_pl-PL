<properties
   pageTitle="Wdrażanie nowej usługi sieci Web"
   description="Przepływ pracy wdrażania RĘKĘ podstawie usługi sieci web"
   services="machine-learning"
   documentationCenter=""
   authors="vDonGlover"
   manager="raymondl"
   editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="v-donglo"/>

# <a name="deploy-a-new-web-service"></a>Wdrażanie nowej usługi sieci web

Microsoft Azure Machine nauki teraz umożliwia usług sieci web, opartych na [Azure Menedżera zasobów](../azure-resource-manager/resource-group-overview.md) , co dla nowych rozliczenia opcje planu i wdrażanie usługi sieci web do wielu obszarów.

Ogólne przepływu pracy do wdrożenia usługi sieci web przy użyciu usługi sieci Web firmy Microsoft Azure maszynowego nauki jest:

* Tworzenie eksperyment przewidywanych
* Wdrażanie go
* Konfigurowanie nazwy
* plan rozliczeń
* należy je przetestować
* Korzystanie ze go.

Poniższa ilustracja przedstawia przepływ pracy.

![Przepływ wdrażania usługi sieci Web][1]
 
## <a name="deploy-web-service-from-studio"></a>Wdrażanie usługi sieci web z Studio 

Aby wdrożyć doświadczenia jako nowa usługa sieci web. Zaloguj się do komputera Studio szkoleniowe i tworzenie nowej usługi przewidywanych sieci web. 

**Uwaga**: Jeśli już wdrożono doświadczenia jako usługi sieci web klasyczny nie można wdrożyć go jako nowa usługa sieci web.
 
Kliknij przycisk **Uruchom** w dolnej części obszaru roboczego doświadczenia, a następnie kliknij pozycję **Wdrażanie usługi sieci Web** i **Wdrażanie usługi sieci Web [nowy]**. Zostanie otwarta strona wdrożenia Menedżera nauki usługi sieci Web.

## <a name="machine-learning-web-service-manager-deploy-experiment-page"></a>Menedżer usługi sieci Web uczenia maszynowego wdrażania doświadczenia strony
Na stronie wdrażanie doświadczenia wprowadź nazwę usługi sieci web.
Wybierz plan cennik. Jeśli masz istniejącego planu cennik, możesz je wybrać, w przeciwnym razie należy utworzyć nowy plan cena w usłudze. 

1.  **Plan cena** listy rozwijanej, zaznacz istniejący plan lub wybierz opcję **Wybierz nowy plan** .
2.  W polu **Nazwa planu**wpisz nazwę, która będzie identyfikować planu na rachunku.
3.  Wybierz jeden z **Poziomów Plan miesięczny**. Należy zauważyć, że ustawienia domyślne poziomy plan plany dla danego regionu domyślne i usługi sieci web zostanie wdrożony do tego regionu.

Kliknij pozycję **Rozmieszczanie** i stronie Szybki Start dla programu otwiera usługi sieci web.

## <a name="quickstart-page"></a>Szybki Start strony
Strony sieci web usługi Szybki Start zapewnia dostęp i wskazówki na najbardziej typowe zadania wykonywane po utworzeniu nowej usługi sieci web. W tym miejscu można łatwo dostępny strona **testowa** i **Consume** strony.

## <a name="testing-your-web-service"></a>Testowanie usługi sieci web

Na stronie Szybki Start kliknij przycisk Test usługi sieci web w obszarze Typowe zadania.   

Aby przetestować usługi sieci web jako usługi żądanie odpowiedź (RR):

* Kliknij przycisk **Test** na pasku menu.
* Kliknij pozycję **Żądanie odpowiedź**.
* Wprowadź odpowiednie wartości dla kolumn wprowadzania danych do doświadczenia.
* Kliknij przycisk Testuj **Żądanie odpowiedź**.

Możesz wyniki są wyświetlane w prawej części strony.

Aby przetestować usługi sieci web usługi wykonanie partii (BES), którego użyjesz pliku CSV:

* Kliknij przycisk **Test** na pasku menu.
* Kliknij pozycję **partię**.
* W obszarze dane wejściowe kliknij przycisk Przeglądaj i przejdź do pliku danych przykładowych.
* Kliknij przycisk **Testuj**.

Stan usługi testu jest wyświetlany w obszarze **Testowanie zadań**.

## <a name="consuming-your-web-service"></a>Przez inne usługi sieci Web

Po wdrożeniu jako usługi sieci web doświadczeń Azure maszynowego uczenia zapewniają interfejsu API usługi REST, która może być używana przez szeroką gamę urządzeń i platformach. Jest to spowodowane uproszczonych interfejsu API usługi REST akceptowanych oraz odpowiada przy JSON format wiadomości. Portal Azure maszynowego uczenia zawiera kod, który może służyć do wywoływania usługi sieci web w R, C# i Python.
 
Na stronie Consuming można znaleźć:

* Klucz interfejsu API i URI umożliwiającego korzystanie z usługi sieci web w aplikacjach.
* Szablony aplikacji sieci web i programu Excel do grzybków Rozpoczynanie procesu zużycie.
* Przykładowy kod w języku C#, python i R ułatwiające rozpoczęcie pracy.

Aby uzyskać więcej informacji o przez inne usługi sieci web zobacz [jak do korzystania z usługi sieci web Azure nauki komputera, który został wdrożony na komputerze nauki doświadczenia](machine-learning-consume-web-services.md).

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji o przez inne usługi sieci web zobacz:

[Jak korzystać z usługi sieci web Azure maszynowego uczenia, który został wdrożony na komputerze nauki doświadczenia](machine-learning-consume-web-services.md)

[Azure usług sieci Web nauki komputera: Wdrożenie i zużycie](machine-learning-deploy-consume-web-service-guide.md)

<!--Image references-->
[1]: ./media/machine-learning-webservice-deploy-a-web-service/armdeploymentworkflow.png


<!--links-->
