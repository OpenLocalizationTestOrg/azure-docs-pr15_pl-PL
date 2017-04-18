<properties
    pageTitle="Zarządzanie usługi sieci Web za pomocą portalu Azure maszynowego uczenia Web Serivces | Microsoft Azure"
    description="Zarządzanie dostępem do obszarów roboczych Azure maszynowego uczenia i wdrażanie i zarządzanie nią ML interfejsu API usług sieci web"
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="v-donglo"/>


# <a name="manage-a-web-service-using-the-azure-machine-learning-web-services-portal"></a>Zarządzanie usługi sieci Web za pomocą portalu usług sieci Web uczenia komputera Azure

Możesz zarządzać maszynowego uczenia nowych i usług klasyczny sieci Web za pomocą portalu usług sieci Web nauki Microsoft Azure komputera. Ponieważ usług klasyczny sieci Web i usług sieci Web nowego są oparte na różnych technologii źródłowych, masz funkcje zarządzania nieco inne dla każdego z nich.

W portalu usługi sieci Web uczenia komputera możesz wykonywać następujące czynności:

- Monitorowanie, jak jest używana usługa sieci web.
- Konfigurowanie opis, aktualizowanie kluczy dla sieci web usługi (tylko nowe), aktualizacji usługi miejsca do magazynowania konta klucza (tylko nowe) Włącz rejestrowanie i włączanie lub wyłączanie przykładowych danych.
- Usuwanie usługi sieci web.
- Tworzenie, usuń lub Aktualizuj rozliczeń planów (tylko nowe).
- Dodawanie i usuwanie punktów końcowych (tylko klasyczny)

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="manage-new-web-services"></a>Zarządzanie usługami nowej sieci Web

Aby zarządzać usług nowej sieci Web:

1.  Zaloguj się do portalu [Usługi sieci Web nauki Microsoft Azure komputera](https://services.azureml.net/quickstart) przy użyciu konta usługi Microsoft Azure — za pomocą konta, który jest skojarzony z subskrypcją Azure.
2.  W menu kliknij polecenie **Usług sieci Web**.

Spowoduje to wyświetlenie listy wdrożonym usług sieci Web dla subskrypcji. 

Aby zarządzać usługi sieci Web, kliknij pozycję usług sieci Web. Na stronie usługi sieci Web można wykonywać następujące czynności:

- Kliknij pozycję Usługa sieci web do zarządzania nim.
- Kliknij pozycję rozliczenia Plan usługi sieci web do jego aktualizacji.
- Usuwanie usługi sieci web.
- Skopiuj usługi sieci web i Wdroż innego regionu.

Po kliknięciu usługi sieci web zostanie wyświetlona strona Szybki Start usługi sieci web. Strony sieci web usługi Szybki Start występują dwie opcje menu, które umożliwiają zarządzanie usługi sieci web:

- **Pulpit NAWIGACYJNY** — umożliwia wyświetlanie użycie usługi sieci Web.
- **Konfigurowanie** — pozwala dodać opis, zaktualizować klucz konta magazynu skojarzone z usługi sieci Web i włączanie lub wyłączanie przykładowych danych.

### <a name="monitoring-how-the-web-service-is-being-used"></a>Monitorowanie, jak jest używana usługa sieci web ###

Kliknij kartę **pulpitu NAWIGACYJNEGO** .

Na pulpicie nawigacyjnym ogólnego zastosowania usługi sieci Web można wyświetlać w okresie. Możesz wybrać okresu wyświetlania z menu rozwijanego okresu w prawym górnym rogu wykresy zastosowania. Pulpit nawigacyjny zawiera następujące informacje:

- **Żądania w czasie** Wyświetla wykres kroku liczba żądań dla wybranego okresu. Może pomóc, ustalić, czy występują tych najwyższych wartościach w użyciu.
- **Żądania żądanie odpowiedź** jest wyświetlana całkowita liczba połączeń żądanie odpowiedź, otrzymania usługę wybranego okresu i ile z nich nie powiodło się.
- **Średni czas obliczyć żądanie odpowiedź** wyświetla średni czas potrzebny do wykonywania przychodzących żądań.
- **Żądania partii** jest wyświetlana całkowita liczba żądań partii otrzymał usługę wybranego okresu i ile z nich nie powiodło się.
- **Średni czas oczekiwania zadania** wyświetla średni czas potrzebny do wykonywania przychodzących żądań.
- **Błędy** jest wyświetlana łączna liczba błędów, które wystąpiły na połączenia z usługą sieci web.
- **Koszty usług** Wyświetla opłat rozliczeń planu skojarzonego z usługą.

### <a name="configuring-the-web-service"></a>Konfigurowanie usługi sieci web ###

Kliknij opcję **Konfiguruj** menu.

Można zaktualizować następujące właściwości:

* **Opis** umożliwia wprowadź opis usługi sieci Web.
* **Tytuł** pozwala na wpisz tytuł usługi sieci Web
* **Klucze** umożliwia obracanie kluczy interfejsu API głównego i pomocniczego.
* **Klucz konta miejsca do magazynowania** umożliwia aktualizowanie klucz konta magazynu skojarzone ze zmianami usługi sieci Web. 
* **Włączanie przykładowych danych** umożliwia podanie dane przykładowe można przetestować usługę żądanie odpowiedź. Jeśli utworzono usługi sieci web w komputerze nauki Studio przykładowych danych jest pobierana z danych usługi umożliwiają przeszkolenie modelu. Jeśli utworzono usługę programowy, dane pochodzi z przykładowymi danymi podanych jako część pakietu JSON.

### <a name="managing-billing-plans"></a>Zarządzanie planów rozliczeń ###

Kliknij opcję menu **Plany** na stronie sieci web usług Szybki Start. Możesz również kliknąć plan skojarzone z określonym usługi sieci Web do zarządzania planu.

* **Nowy** umożliwia utworzenie nowego planu.
* **Plan Dodaj lub Usuń wystąpienie** umożliwia "skalowania" istniejącego planu, aby zwiększyć wydajność.
* **Uaktualnianie i obniżanie wersji** umożliwia "rozbudowy" istniejącego planu, aby zwiększyć wydajność.
* **Usuwanie** umożliwia usuwanie planu.

Kliknij pozycję plan, aby wyświetlić jego pulpitu nawigacyjnego. Pulpit nawigacyjny pozwala zastosowania migawki lub plan w wybranym okresie czasu. Aby zaznaczyć przedział czasu, aby wyświetlić, kliknij listę rozwijaną **okresu** w prawym górnym rogu pulpitu nawigacyjnego. 

Pulpit nawigacyjny plan zawiera następujące informacje:

* **Opis planu** Wyświetla informacje dotyczące kosztów i pojemnością skojarzonych z planem.
* **Planowanie użycia** jest wyświetlana liczba transakcji i godzin obliczeń, które zostały obciążone względem planu.
* **Usługi sieci Web** jest wyświetlana liczba usług sieci Web, korzystających z tego planu.
* **Góry usługi sieci Web przez połączenia** Wyświetla czterech najwyższych usług sieci Web, które są nawiązywanie połączeń, które są powiązane z planem.
* **Góry usług sieci Web przez obliczenia godz** Wyświetla usług sieci Web czterech najwyższych używają zasoby obliczeń, które są powiązane z planem.

## <a name="manage-classic-web-services"></a>Zarządzanie usługami klasyczny sieci Web

> [AZURE.NOTE] Procedury przedstawione w tej sekcji dotyczą zarządzania klasyczny usług sieci web za pośrednictwem portalu usług sieci Web uczenia komputera Azure. Aby uzyskać informacje o zarządzaniu usług sieci Web klasyczny przez Studio nauki komputera i portalu klasyczny Azure zobacz [Zarządzanie z obszarem roboczym Azure maszynowego uczenia](machine-learning-manage-workspace.md).

Aby zarządzać usług klasyczny sieci Web:

1.  Zaloguj się do portalu [Usługi sieci Web nauki Microsoft Azure komputera](https://services.azureml.net/quickstart) przy użyciu konta usługi Microsoft Azure — za pomocą konta, który jest skojarzony z subskrypcją Azure.
2.  W menu kliknij pozycję **Klasyczny usług sieci Web**.

Aby zarządzać klasyczny usługi sieci Web, kliknij pozycję **Klasyczny usług sieci Web**. Na stronie klasyczny usług sieci Web można wykonywać następujące czynności:

- Kliknij pozycję Usługa sieci web, aby wyświetlić skojarzone punkty końcowe.
- Usuwanie usługi sieci web.

Gdy zarządzasz usługi sieci Web klasyczny, możesz zarządzać poszczególnych punktów końcowych oddzielnie. Po kliknięciu usługi sieci web na stronie usługi sieci Web zostanie wyświetlona na liście punkty końcowe skojarzone z usługą. 

Na stronie punktu końcowego klasyczny usługi sieci Web można dodawać i usuwać punkty końcowe w usłudze. Aby uzyskać więcej informacji o dodawaniu punktów końcowych zobacz [Tworzenie punktów końcowych](machine-learning-create-endpoint.md).

Kliknij jeden z punktów końcowych, aby otworzyć stronę sieci web usługi Szybki Start. Istnieją dwie opcje menu, które umożliwiają zarządzanie usługi sieci web, na stronie Szybki Start:

- **Pulpit NAWIGACYJNY** — umożliwia wyświetlanie użycie usługi sieci Web.
- **Konfigurowanie** — pozwala dodać opis, a następnie włączyć rejestrowanie błędów, włączanie i wyłączanie funkcji aktualizacji klucz do przechowywania kont skojarzonych z usługi sieci Web i włączanie i wyłączanie przykładowych danych.

### <a name="monitoring-how-the-web-service-is-being-used"></a>Monitorowanie, jak jest używana usługa sieci web ###

Kliknij kartę **pulpitu NAWIGACYJNEGO** .

Na pulpicie nawigacyjnym ogólnego zastosowania usługi sieci Web można wyświetlać w okresie. Możesz wybrać okresu wyświetlania z menu rozwijanego okresu w prawym górnym rogu wykresy zastosowania. Pulpit nawigacyjny zawiera następujące informacje:

- **Żądania w czasie** Wyświetla wykres kroku liczba żądań dla wybranego okresu. Może pomóc, ustalić, czy występują tych najwyższych wartościach w użyciu.
- **Żądania żądanie odpowiedź** jest wyświetlana całkowita liczba połączeń żądanie odpowiedź, otrzymania usługę wybranego okresu i ile z nich nie powiodło się.
- **Średni czas obliczyć żądanie odpowiedź** wyświetla średni czas potrzebny do wykonywania przychodzących żądań.
- **Żądania partii** jest wyświetlana całkowita liczba żądań partii otrzymał usługę wybranego okresu i ile z nich nie powiodło się.
- **Średni czas oczekiwania zadania** wyświetla średni czas potrzebny do wykonywania przychodzących żądań.
- **Błędy** jest wyświetlana łączna liczba błędów, które wystąpiły na połączenia z usługą sieci web.
- **Koszty usług** Wyświetla opłat rozliczeń planu skojarzonego z usługą.

### <a name="configuring-the-web-service"></a>Konfigurowanie usługi sieci web ###

Kliknij opcję **Konfiguruj** menu.

Można zaktualizować następujące właściwości:

* **Opis** umożliwia wprowadź opis usługi sieci Web. Opis pole jest wymagane.
* **Rejestrowanie** umożliwia włączanie i wyłączanie rejestrowania dla punktu końcowego błędów. Aby uzyskać więcej informacji zobacz Włączanie [rejestrowania dla usług sieci web uczenia komputera](machine-learning-web-services-logging.md).
* **Włączanie przykładowych danych** umożliwia podanie dane przykładowe można przetestować usługę żądanie odpowiedź. Jeśli utworzono usługi sieci web w komputerze nauki Studio przykładowych danych jest pobierana z danych usługi umożliwiają przeszkolenie modelu. Jeśli utworzono usługę programowy, dane pochodzi z przykładowymi danymi podanych jako część pakietu JSON.

## <a name="grant-or-suspend-access-to-web-services-for-users-in-the-portal"></a>Udzielanie lub zawiesić dostęp do usług sieci Web dla użytkowników w portalu

Za pomocą portalu klasyczny Azure, możesz zezwolić lub odmówić dostępu do określonych użytkowników.

### <a name="access-for-users-of-new-web-services"></a>Dostęp dla użytkowników usług nowej sieci Web

Aby umożliwić innym użytkownikom do pracy z usługami sieci Web w portalu usługi sieci Web uczenia maszynowego Azure, należy dodać je jako administratorzy co w subskrypcji usługi Azure.

Zaloguj się do [portalu klasyczny Azure](https://manage.windowsazure.com/) za pomocą konta Microsoft Azure — za pomocą konta, który jest skojarzony z subskrypcją Azure.

1. W okienku nawigacji kliknij pozycję **Ustawienia**, a następnie kliknij pozycję **Administratorzy**.
2. U dołu okna kliknij przycisk **Dodaj**. 
3. W oknie dialogowym Dodawanie A Współtworzenie ADMINISTRATOR wpisz adres e-mail osoby, którą chcesz dodać jako administrator Współtworzenie, a następnie wybierz subskrypcję, do której chcesz Współtworzenie administratorem, aby uzyskać dostęp do.
4. Kliknij przycisk **Zapisz**.

### <a name="access-for-users-of-classic-web-services"></a>Dostęp dla użytkowników usług sieci Web klasyczny

Aby zarządzać obszaru roboczego:

Zaloguj się do [portalu klasyczny Azure](https://manage.windowsazure.com/) za pomocą konta Microsoft Azure — za pomocą konta, który jest skojarzony z subskrypcją Azure.

1. W panelu usługi Microsoft Azure kliknij **Nauki komputera**.
1. Kliknij obszar roboczy, którą chcesz zarządzać.
1. Kliknij kartę **Konfiguruj** .

Na karcie konfiguracji można wstrzymać dostęp do obszaru roboczego nauki komputera, klikając pozycję **ODMÓW**. Użytkownicy będą już nie można otworzyć obszar roboczy w Studio nauki komputera. Aby przywrócić dostęp, kliknij przycisk **Zezwalaj**.

Do określonych użytkowników:

Aby zarządzać kolejne konta, którzy mają dostęp do obszaru roboczego w Studio nauki komputera, kliknij przycisk **Zaloguj się do ML Studio** na karcie **pulpitu NAWIGACYJNEGO** . Spowoduje to otwarcie obszaru roboczego w Studio nauki komputera. W tym miejscu kliknij kartę **Ustawienia** , a następnie **użytkowników**. Możesz kliknąć pozycję **ZAPROŚ więcej użytkowników** , aby przekazać użytkownikom dostępu do obszaru roboczego, lub zaznacz użytkownika i kliknij przycisk **Usuń**.

> [AZURE.NOTE] Łącze **Zaloguj się do ML Studio** umożliwia otwarcie Studio nauki komputera za pomocą Account Microsoft użytkownik jest obecnie zalogowany na. Automatycznie Account firmy Microsoft, którego użyto do zalogowania się do portalu klasyczny Azure do utworzenia obszaru roboczego nie ma uprawnień do otwarcia tego obszaru roboczego. Aby otworzyć obszar roboczy, należy się zalogować Account firmy Microsoft, które zostało zdefiniowane jako właściciel obszaru roboczego lub musisz otrzymać zaproszenie do obszaru roboczego od właściciela.
