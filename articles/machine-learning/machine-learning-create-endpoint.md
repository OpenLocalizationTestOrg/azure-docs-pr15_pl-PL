<properties
    pageTitle="Tworzenie punktów końcowych usługi sieci Web w maszynowego uczenia | Microsoft Azure"
    description="Tworzenie punktów końcowych usługi sieci Web w nauki komputera Azure"
    services="machine-learning"
    documentationCenter=""
    authors="hiteshmadan"
    manager="padou"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="tbd"
    ms.date="10/04/2016"
    ms.author="himad"/>


# <a name="creating-endpoints"></a>Tworzenie punkty końcowe

>[AZURE.NOTE] W tym temacie opisano techniki dotyczące klasyczny usługi sieci Web.

Podczas tworzenia usług sieci Web, sprzedawane do przodu do klientów, należy podać modeli przeszkolony do każdego odbiorcy, które nadal są połączone z doświadczenia, z którego utworzono usługi sieci Web. Ponadto wszelkie aktualizacje doświadczenia ma dotyczyć selektywne punktu końcowego bez zastępowania dostosowań.

W tym celu nauki maszynowego Azure umożliwia tworzenie wiele punktów końcowych wdrożonym usługi sieci Web. Każdego punktu końcowego usługi sieci Web jest niezależne zaadresowaną, ograniczenie i zarządzane. Każdy punkt końcowy jest unikatowy adres URL i klucza autoryzacji, którą rozpowszechniasz klientom.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="adding-endpoints-to-a-web-service"></a>Dodawanie punktów końcowych do usługi sieci Web

Istnieją trzy sposoby dodawania punktu końcowego do usługi sieci Web.

* Programistyczne
* Za pośrednictwem portalu usług sieci Web uczenia maszynowego Azure
* Chociaż portalu klasyczny Azure

Po utworzeniu punkt końcowy, można go używać za pośrednictwem interfejsów API synchroniczne, partii interfejsów API i arkusze kalkulacyjne programu excel. Oprócz dodawania punkty końcowe za pośrednictwem tego interfejsu użytkownika, umożliwia także interfejsy API zarządzania punktu końcowego programowy Dodawanie punktów końcowych.

 >[AZURE.NOTE] Po dodaniu dodatkowe punkty końcowe do usługi sieci Web, nie można usunąć punktu końcowego.

## <a name="adding-an-endpoint-programmatically"></a>Dodawanie punktu końcowego programowy

Możesz dodać punkt końcowy do usługi sieci Web programowo przy użyciu [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) przykładowy kod.

## <a name="adding-an-endpoint-using-the-azure-machine-learning-web-services-portal"></a>Dodawanie punktu końcowego za pomocą portalu usług sieci Web uczenia maszynowego Azure

1. W komputerze nauki Studio na kolumnie nawigacji po lewej stronie kliknij usług sieci Web.
2. U dołu pulpitu nawigacyjnego usług sieci Web kliknij pozycję **Zarządzaj punktów końcowych**. Portal usługi sieci Web uczenia maszynowego Azure zostanie wyświetlona na stronie punkty końcowe usługi sieci Web.
3. Kliknij przycisk **Nowy**.
4. Wpisz nazwę i opis nowego punktu końcowego. Nazwy punktu końcowego musi być 24 znaków lub mniej długości i składa się małą alfabetów lub liczb. Wybierz poziom rejestrowania i czy jest włączony przykładowych danych. Aby uzyskać więcej informacji zobacz [Włączanie rejestrowania dla usług sieci Web uczenia komputera](machine-learning-web-services-logging.md).

## <a name="adding-an-endpoint-using-the-azure-classic-portal"></a>Dodawanie punktu końcowego za pomocą portalu klasyczny Azure


1. Zaloguj się do [portalu klasyczny Azure](http://manage.windowsazure.com), kliknij przycisk **Maszynowego uczenia** w kolumnie po lewej stronie. Kliknij obszar roboczy, zawierający usługi sieci Web, w której interesuje Cię.

    ![Przejdź do obszaru roboczego](./media/machine-learning-create-endpoint/figure-1.png)

2. Kliknij pozycję **usług sieci Web**.

    ![Przejdź do usługi sieci Web](./media/machine-learning-create-endpoint/figure-2.png)

3. Kliknij pozycję Usługa sieci Web, które Cię interesują wyświetlić listę dostępnych punktów końcowych.

    ![Przejdź do punktu końcowego](./media/machine-learning-create-endpoint/figure-3.png)

4. U dołu strony kliknij pozycję **Dodaj punkt końcowy**. Wpisz nazwę i opis, upewnij się, że nie ma żadnych innych punktów końcowych o takiej samej nazwie w tej usłudze sieci Web. Jeśli nie masz specjalnych wymagań pozostaw wartość domyślną poziom ograniczenia. Aby dowiedzieć się więcej na temat ograniczania, zobacz [Skalowanie punkty końcowe interfejsu API](machine-learning-scaling-webservice.md).

    ![Tworzenie punktu końcowego](./media/machine-learning-create-endpoint/figure-4.png)

## <a name="next-steps"></a>Następne kroki

[Jak używać opublikowana usługa Azure maszynowego Learning w sieci Web](machine-learning-consume-web-services.md).
