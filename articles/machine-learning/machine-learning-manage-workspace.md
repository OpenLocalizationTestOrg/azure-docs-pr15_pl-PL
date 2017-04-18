<properties
    pageTitle="Zarządzanie obszaru roboczego maszynowego uczenia | Microsoft Azure"
    description="Zarządzanie dostępem do obszarów roboczych Azure maszynowego uczenia i wdrażanie i zarządzanie nią ML interfejsu API usług sieci web"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="garye"/>


# <a name="manage-an-azure-machine-learning-workspace"></a>Zarządzanie Azure maszynowego uczenia obszaru roboczego

>[AZURE.NOTE] Procedury przedstawione w tym artykule dotyczą usług sieci Web Azure maszynowego uczenia klasyczny. Aby uzyskać informacje o zarządzaniu usług sieci Web w portalu usługi sieci Web uczenia komputera zobacz [Zarządzanie usługi sieci Web za pomocą portalu usług sieci Web uczenia maszynowego Azure](machine-learning-manage-new-webservice.md).

Za pomocą portalu klasyczny Azure, możesz zarządzać obszarów roboczych maszynowego uczenia się:

- Monitorowanie, jak jest używany obszaru roboczego
- Konfigurowanie obszaru roboczego, aby zezwolić lub odmówić dostępu
- Zarządzanie usług sieci Web tworzonych w obszarze roboczym
- Usuwanie obszaru roboczego

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Ponadto karta Pulpit nawigacyjny zawiera omówienie zastosowania do obszaru roboczego i szybkie rzut danych obszaru roboczego.  

> [AZURE.TIP] W Azure maszynowego uczenia Studio, na karcie **Usług sieci WEB** można dodać, aktualizowanie lub usuwanie usługi komputera Learning w sieci Web.

Aby zarządzać obszaru roboczego:

1.  Zaloguj się do [portalu klasyczny Azure](https://manage.windowsazure.com/) za pomocą konta Microsoft Azure — za pomocą konta, który jest skojarzony z subskrypcją Azure.
2.  W panelu usługi Microsoft Azure kliknij **Nauki komputera**.
3.  Kliknij obszar roboczy, którą chcesz zarządzać.

Na stronie obszaru roboczego występują trzy karty:

- **Pulpit NAWIGACYJNY** — umożliwia wyświetlanie obszaru roboczego użycia i informacje
- **Konfigurowanie** — umożliwia zarządzanie dostępem do obszaru roboczego
- **Usług sieci WEB** — umożliwia zarządzanie usług sieci Web, które zostały opublikowane z tego obszaru roboczego

## <a name="to-monitor-how-the-workspace-is-being-used"></a>Monitorowanie, jak jest używany obszaru roboczego

Kliknij kartę **pulpitu NAWIGACYJNEGO** .

Na pulpicie nawigacyjnym można wyświetlić ogólną użycie obszaru roboczego i uzyskiwanie szybkiego rzut danych obszaru roboczego.

- Wykres **OBLICZYĆ** zawiera zasoby obliczeń używany przez obszaru roboczego. Możesz zmienić widok, aby wyświetlić względne lub wartości bezwzględne i można zmienić okres wyświetlane na wykresie.
- **Omówienie zastosowania** Wyświetla Azure przestrzeni dyskowej, używany przez obszaru roboczego.
- **Szybkie rzut** zawiera podsumowanie informacji o obszarze roboczym i przydatne łącza.

> [AZURE.NOTE] Łącze **Zaloguj się do ML Studio** umożliwia otwarcie Studio nauki komputera za pomocą Account Microsoft użytkownik jest obecnie zalogowany na. Automatycznie Account firmy Microsoft, którego użyto do zalogowania się do portalu klasyczny Azure do utworzenia obszaru roboczego nie ma uprawnień do otwarcia tego obszaru roboczego. Aby otworzyć obszar roboczy, należy się zalogować Account firmy Microsoft, które zostało zdefiniowane jako właściciel obszaru roboczego lub musisz otrzymać zaproszenie do obszaru roboczego od właściciela.


## <a name="to-grant-or-suspend-access-for-users"></a>Aby udzielić lub zawiesić dostęp dla użytkowników ##

Kliknij kartę **Konfiguruj** .

Na karcie konfiguracji można wykonywać następujące czynności:

- Zawiesić dostęp do obszaru roboczego nauki komputera, klikając pozycję ODMÓW. Użytkownicy będą już nie można otworzyć obszar roboczy w Studio nauki komputera. Aby przywrócić dostęp, kliknij przycisk Zezwalaj.

Aby zarządzać kolejne konta, którzy mają dostęp do obszaru roboczego w komputerze nauki Studio, kliknij przycisk **Zaloguj się do ML Studio** na karcie **pulpitu NAWIGACYJNEGO** (zobacz uwagę poprzedzających dotyczące **logowania do ML Studio**). Spowoduje to otwarcie obszaru roboczego w Studio nauki komputera. W tym miejscu kliknij kartę **Ustawienia** , a następnie **użytkowników**. Możesz kliknąć pozycję **ZAPROŚ więcej użytkowników** , aby przekazać użytkownikom dostępu do obszaru roboczego, lub zaznacz użytkownika i kliknij przycisk **Usuń**.


## <a name="to-manage-web-services-in-this-workspace"></a>Aby zarządzać usług sieci web, w tym obszarze roboczym

Kliknij kartę **Usług sieci WEB** .

Spowoduje to wyświetlenie listy usług sieci web opublikowanych z tego obszaru roboczego.
Aby zarządzać usługi sieci web, kliknij nazwę na liście, aby otworzyć stronę usługi sieci Web.

Usługa sieci Web może być jeden lub więcej punkty końcowe zdefiniowane.

- Można określić więcej punkty końcowe oprócz punkt końcowy "Wartość domyślna". Aby dodać punkt końcowy, kliknij pozycję **Zarządzaj punkty końcowe** w dolnej części pulpitu nawigacyjnego do Otwórz portal usługi sieci Web uczenia maszynowego Azure.

- Aby usunąć punkt końcowy (nie można usunąć punktu końcowego "Wartość domyślna"), kliknij pole wyboru na początku wiersza punktu końcowego, a następnie kliknij przycisk **Usuń**. Spowoduje to usunięcie punktu końcowego z usługi sieci Web.

    > [AZURE.NOTE] Jeśli aplikacja używa punktu końcowego usługi sieci web po usunięciu punkt końcowy, aplikacja wystąpi błąd podczas kolejnego próbuje uzyskać dostęp do usługi.

Kliknij nazwę punktu końcowego usługi sieci Web, aby go otworzyć. 

Na pulpicie nawigacyjnym ogólnego zastosowania usługi sieci Web można wyświetlać w okresie. Możesz wybrać okresu wyświetlania z menu rozwijanego okresu w prawym górnym rogu wykresy zastosowania. Pulpit nawigacyjny zawiera następujące informacje:

- **Żądania w czasie** Wyświetla wykres kroku liczba żądań dla wybranego okresu. Może pomóc, ustalić, czy występują tych najwyższych wartościach w użyciu.
- **Żądania żądanie odpowiedź** jest wyświetlana całkowita liczba połączeń żądanie odpowiedź, otrzymania usługę wybranego okresu i ile z nich nie powiodło się.
- **Średni czas obliczyć żądanie odpowiedź** wyświetla średni czas potrzebny do wykonywania przychodzących żądań.
- **Żądania partii** jest wyświetlana całkowita liczba żądań partii otrzymał usługę wybranego okresu i ile z nich nie powiodło się.
- **Średni czas oczekiwania zadania** wyświetla średni czas potrzebny do wykonywania przychodzących żądań.
- **Błędy** jest wyświetlana łączna liczba błędów, które wystąpiły na połączenia z usługą sieci web.
- **Koszty usług** Wyświetla opłat rozliczeń planu skojarzonego z usługą.

Na stronie Konfigurowanie możesz zaktualizować następujące właściwości:

* **Opis** umożliwia wprowadź opis usługi sieci Web. Opis pole jest wymagane.
* **Rejestrowanie** umożliwia włączanie i wyłączanie rejestrowania dla punktu końcowego błędów. Aby uzyskać więcej informacji zobacz Włączanie [rejestrowania dla usług sieci web uczenia komputera](machine-learning-web-services-logging.md).
* **Włączanie przykładowych danych** umożliwia podanie dane przykładowe można przetestować usługę żądanie odpowiedź. Jeśli utworzono usługi sieci web w komputerze nauki Studio przykładowych danych jest pobierana z danych usługi umożliwiają przeszkolenie modelu. Jeśli utworzono usługę programowy, dane pochodzi z przykładowymi danymi podanych jako część pakietu JSON.

[consume]: machine-learning-consume-web-services.md
[marketplace]: machine-learning-publish-web-service-to-azure-marketplace.md
