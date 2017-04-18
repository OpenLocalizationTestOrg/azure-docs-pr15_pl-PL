<properties
    pageTitle="Zarządzanie dostępem do funkcji analizy dziennika | Microsoft Azure"
    description="Zarządzanie dostępem do funkcji analizy dziennika za pomocą różnych zadań administracyjnych na użytkowników, konta usługi OMS obszarów roboczych i Azure konta."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/16/2016"
    ms.author="banders"/>

# <a name="manage-access-to-log-analytics"></a>Zarządzanie dostępem do funkcji analizy dziennika

Aby zarządzać dostępem do analizy dziennika, które będą używane różnych zadań administracyjnych użytkowników, konta usługi OMS obszarów roboczych i Azure konta. Tworzenie nowego obszaru roboczego w pakiecie zarządzania operacje usługi (OMS), wybierz nazwę obszaru roboczego, skojarzyć konta i wybierz lokalizacji geograficznej. Obszar roboczy jest zasadniczo kontener, który zawiera informacje o koncie i informacje o prostej konfiguracji konta. Uczestnik organizacji za pomocą wielu usługi OMS obszary robocze można zarządzać różnymi zestawami danych, które są zbierane ze wszystkich lub części infrastrukturę informatyczną.

Artykuł [Wprowadzenie do analizy dziennika](log-analytics-get-started.md) pokazano, jak szybko przejść w górę i uruchamianie i w dalszej części tego artykułu opisano bardziej szczegółowo niektóre akcji, konieczne będzie zarządzanie dostępem do usługi OMS.

Mimo że nie może być konieczne wykonanie każdego zadania zarządzania na początku, omówiono wszystkie często używane zadania, które można użyć w poniższych sekcjach:

- Określenie liczby obszarów roboczych, które są potrzebne
- Zarządzanie kontami i użytkowników
- Dodawanie grupy do istniejącego obszaru roboczego
- Łączenie istniejącego obszaru roboczego z subskrypcji usługi Azure
- Uaktualnienie obszaru roboczego do planu płatnej danych
- Zmienianie typu danych planu
- Dodawanie Azure Active Directory organizacji do istniejącego obszaru roboczego
- Zamykanie usługi OMS obszaru roboczego

## <a name="determine-the-number-of-workspaces-you-need"></a>Określenie liczby obszarów roboczych, które są potrzebne

Obszar roboczy jest Azure zasobów i kontenera miejsce, w którym dane zebrane, agregacją, analizowane oraz przedstawione w portalu usługi OMS.

Istnieje możliwość tworzenia wielu analizy dziennika usługi OMS obszarów roboczych i użytkowników mieć dostęp do jednego lub więcej obszarów roboczych. Ogólnie chcesz zminimalizować liczbę obszarów roboczych, jak to umożliwi kwerendy, a następnie dostosować przez większość danych. W tej sekcji opisano, kiedy może być przydatne utworzyć więcej niż jeden obszar roboczy.

Obecnie obszaru roboczego analizy dziennika zawiera:

- Lokalizacji geograficznej do przechowywania danych
- Szczegółowy dla rozliczeń
- Izolacji danych

W zależności od cech powyżej, można utworzyć wiele obszarów roboczych, jeśli:

- Jesteś firmy globalnej i trzeba dane przechowywane w określonych regionach powodów suwerenności lub zgodności danych.
- W przypadku korzystania z platformy Azure i chcesz uniknięcia opłat za przesyłanie danych wychodzących przez obszaru roboczego analizy dziennika w tym samym regionie jako Azure zasoby, którymi zarządza.
- Chcesz opłaty dla różnych działów lub grup firm według ich zastosowania. Podczas tworzenia obszaru roboczego dla każdego dziale lub grupie firma instrukcji Azure rachunku i zastosowania zawiera opłaty za każdym obszarze roboczym oddzielnie.
- Są zarządzane usługodawcę i potrzebne do przechowywania danych analizy dziennika dla każdego z klientów możesz zarządzać odizolowane od danych innego odbiorcy.
- Zarządzanie wieloma klientami i chcesz każdego klienta lub działu lub grupie firma Zobacz własne dane, ale bez danych dla innych odbiorców, działów lub grup biznesowych.

Zbieranie danych za pomocą czynników, można skonfigurować każdego agenta raportowania do obszaru roboczego wymagane.

Jeśli używasz programu System Center Operations manager każdej grupy zarządzania programu Operations Manager można łączyć przy użyciu tylko jednego obszaru roboczego. Można zainstalować agenta monitorowania firmy Microsoft na komputerach zarządzanych przez programu Operations Manager i raport agenta dla programu Operations Manager i innego obszaru roboczego analizy dziennika.

### <a name="workspace-information"></a>Informacje o obszarze roboczym

W portalu usługi OMS można wyświetlić informacje o obszarze roboczym i wybierz, czy chcesz otrzymywać informacje od firmy Microsoft.

#### <a name="view-workspace-information"></a>Wyświetlanie informacji o obszarze roboczym

1. W usługi OMS kliknij przycisk kafelka **Ustawienia** .
2. Kliknij kartę **konta** .
3. Kliknij kartę **Informacje o obszarze roboczym** .  
  ![Informacje o obszarze roboczym](./media/log-analytics-manage-access/workspace-information.png)

## <a name="manage-accounts-and-users"></a>Zarządzanie kontami i użytkowników

Każdego obszaru roboczego może mieć wiele kont użytkowników skojarzone z nim, a każdego konta użytkownika (konta Microsoft lub konto organizacji) mogą uzyskiwać dostęp do wielu obszarów roboczych usługi OMS.

Domyślnie konto Microsoft lub konto organizacji użyte do utworzenia obszaru roboczego staje się administratorem obszaru roboczego. Administrator można zapraszać kolejne konta Microsoft lub wybierz użytkowników z usługi Azure Active Directory.

Zapewniająca dostęp osób do obszaru roboczego usługi OMS sterują 2 miejsc:

- Kontrola dostępu oparta na rolach Azure, umożliwia dostęp do subskrypcji Azure i skojarzonych z nimi zasobów Azure. To jest również używana dostępu za pomocą programu PowerShell i interfejsu API usługi REST.
- W portalu usługi OMS dostęp do tylko portal usługi OMS — a nie skojarzone Azure subskrypcji.

Jeśli możesz zapewniają użytkownikom dostępu do portalu usługi OMS, ale nie do Azure subskrypcję, do której jest on połączony, następnie Kafelki rozwiązanie automatyzacji, wykonywanie kopii zapasowych i odzyskiwanie witryny nie są wyświetlane wszystkie dane do użytkowników po ich logowanie do portalu usługi OMS.

Aby zezwolić wszystkim użytkownikom wyświetlać dane w tych rozwiązań, upewnij się, że mają co najmniej **czytnika** dostęp dla połączonej z obszaru roboczego usługi OMS magazyn konto automatyzacji, magazynu kopii zapasowych i odzyskiwanie witryny.   

### <a name="managing-access-to-log-analytics-using-the-azure-portal"></a>Zarządzanie dostępem do analiz dziennika za pomocą portalu Azure

Jeśli osoby można udzielić dostępu do obszaru roboczego analizy dziennika przy użyciu Azure uprawnień, w portalu Azure na przykład następnie tym użytkownikom można dostęp do portalu analizy dziennika. Jeśli użytkownicy mają Azure portal, ich przejdź do portalu usługi OMS, klikając pozycję zadanie **Portal usługi OMS** , podczas przeglądania zasobów obszaru roboczego analizy dziennika.

Niektóre informacje, które należy pamiętać o Azure portal:

- Nie jest *Kontrola dostępu oparta na rolach*. Jeśli masz uprawnienia dostępu do *czytnika* w portalu Azure analizy dziennika obszaru roboczego można wprowadź zmiany za pomocą portalu usługi OMS. Portal usługi OMS ma pojęcia administratora, współautorów i użytkownika tylko do odczytu. Jeśli konto, dla którego są zalogowani przy użyciu znajduje się w usłudze Azure Active Directory połączone z obszaru roboczego będzie administratora w portalu usługi OMS, w przeciwnym razie będzie współautora.

- Gdy możesz zalogować się do portalu usługi OMS przy użyciu http://mms.microsoft.com, następnie domyślnie zostanie wyświetlona lista **Wybierz obszaru roboczego** . Zawiera ona tylko obszarów roboczych, które zostały dodane za pomocą portalu usługi OMS. Aby wyświetlić obszarów roboczych użytkownik ma dostęp do z subskrypcją Azure, a następnie należy określić dzierżawy jako część adresu URL. Na przykład:

  `mms.microsoft.com/?tenant=contoso.com`Identyfikator dzierżawy jest zazwyczaj ostatnią część adresu e-mail, który możesz zalogować się przy.

- Jeśli konto możesz zalogować się przy to konto w dzierżawie usługi Azure Active Directory, która jest zazwyczaj wielkości liter chyba że logowanie w jako dostawcy, będzie być *administratora* w portalu usługi OMS. Jeśli Twoje konto nie jest w dzierżawie usługi Azure Active Directory, będzie można *użytkownika* w portalu usługi OMS.

- Jeśli chcesz przejść bezpośrednio do portalu, że masz dostęp do korzystania z platformy Azure uprawnień, należy określić zasobu jako część adresu URL. Użytkownik może uzyskać ten adres URL przy użyciu programu PowerShell.

  Na przykład `(Get-AzureRmOperationalInsightsWorkspace).PortalUrl`.

  Adres URL będzie wyglądać:`https://eus.mms.microsoft.com/?tenant=contoso.com&resource=%2fsubscriptions%2faaa5159e-dcf6-890a-a702-2d2fee51c102%2fresourcegroups%2fdb-resgroup%2fproviders%2fmicrosoft.operationalinsights%2fworkspaces%2fmydemo12`


### <a name="managing-users-in-the-oms-portal"></a>Zarządzanie użytkownikami w portalu usługi OMS

Zarządzanie użytkownikami i grupy na karcie **Zarządzanie użytkownikami** , na karcie **konta** na stronie Ustawienia. Może wykonywać zadania w poniższych sekcjach.  

![Zarządzanie użytkownikami](./media/log-analytics-manage-access/setup-workspace-manage-users.png)

#### <a name="add-a-user-to-an-existing-workspace"></a>Dodawanie użytkownika do istniejącego obszaru roboczego

Wykonaj następujące czynności, aby dodać użytkownika lub grupy z obszarem roboczym usługi OMS. Użytkownikowi lub grupie będzie mógł wyświetlać i działać w wszystkie alerty, które są skojarzone z tego obszaru roboczego.

>[AZURE.NOTE] Jeśli chcesz dodać użytkownika lub grupy z firmowego konta usługi Azure Active Directory, należy najpierw zapewnić, że Twoje konto usługi OMS korzystania z powiązanych ze swoją domeną usługi Active Directory. Zobacz [Dodawanie Azure Active Directory organizacji istniejącego obszaru roboczego](#add-an-azure-active-directory-organization-to-an-existing-workspace).

1. W usługi OMS kliknij przycisk kafelka **Ustawienia** .
2. Kliknij kartę **Klienci** , a następnie kliknij kartę **Zarządzanie użytkownikami** .
3. W sekcji **Zarządzanie użytkownikami** , wybierz typ konta, aby dodać: **Konto organizacji**, **Konta Microsoft**, **Pomocy technicznej firmy Microsoft**.
    - Jeśli wybierzesz Account Microsoft, wpisz adres e-mail użytkownika skojarzonego z Account firmy Microsoft.
    - Jeśli wybierzesz konto organizacji, można wprowadzić część użytkownika lub grupy nazwę lub alias e-mail i zostanie wyświetlona lista użytkowników i grup. Wybierz użytkownika lub grupy.
    - Użyj Support firmy Microsoft, aby nadać Microsoft Support mechanika tymczasowy dostęp do obszaru roboczego w celu ułatwienia rozwiązywania problemów.

    >[AZURE.NOTE] Aby uzyskać najlepsze wyniki wydajności należy ograniczyć liczbę skojarzone z kontem usługi OMS jednego do trzech grup usługi Active Directory — jedną dla administratorów, jedną dla współpracowników i jedną dla użytkowników tylko do odczytu. Za pomocą więcej grup może wpłynąć na działanie analizy dziennika.

5. Wybierz typ dodawanego użytkownika lub grupy, aby dodać: **Administrator**, **współautorów**lub **Użytkownika tylko do odczytu** .  
6. Kliknij przycisk **Dodaj**.

  Jeśli dodajesz konto Microsoft do wiadomości e-mail przez Ciebie informacje są wysyłane zaproszenie do obszaru roboczego. Po użytkownika następuje z instrukcjami podanymi w zaproszenie do dołączenia usługi OMS, użytkownik może przeglądać alerty i informacje o koncie dla tego konta usługi OMS i będzie można wyświetlić informacje o użytkowniku na karcie **konta** na stronie **Ustawienia** .
  Jeśli dodajesz konto organizacji użytkownika będą mieli dostęp do analizy dziennika natychmiast.  
  ![zaproszenie e-mail](./media/log-analytics-manage-access/setup-workspace-invitation-email.png)

#### <a name="edit-an-existing-user-type"></a>Edytowanie istniejącego typu użytkownika

Możesz zmienić rolę konta użytkownika skojarzonego z kontem usługi OMS. Dostępne są następujące opcje ról:

 - *Administrator*: Zarządzanie użytkownikami, wyświetlanie i działać w wszystkie alerty i dodawać i usuwać serwery

 - *Współautor*: wyświetlanie i działać w wszystkie alerty i dodawać i usuwać serwery

 - *Tylko do odczytu użytkownika*: użytkownicy oznaczone jako tylko do odczytu, nie będą mogli:
   1. Dodawanie/usuwanie rozwiązań. Galeria rozwiązań jest ukryty.
   2. Dodawanie/modyfikowanie i usuwanie kafelków na **Moje pulpitu nawigacyjnego**.
   3. Wyświetlanie stron **Ustawienia** . Strony są ukryte.
   4. Wyszukiwanie widoku, PowerBI konfiguracji, zapisane wyszukiwania i alertów zadania są ukryte.


#### <a name="to-edit-an-account"></a>Aby edytować konta

1. W usługi OMS kliknij przycisk kafelka **Ustawienia** .
2. Kliknij kartę **Klienci** , a następnie kliknij kartę **Zarządzanie użytkownikami** .
3. Wybierz rolę, dla użytkownika, który chcesz zmienić.
2. W oknie dialogowym potwierdzenia kliknij przycisk **Tak**.

### <a name="remove-a-user-from-a-oms-workspace"></a>Usuwanie użytkownika z obszaru roboczego usługi OMS

Wykonaj następujące czynności, aby usunąć użytkownika z usługi OMS obszaru roboczego. Należy zauważyć, że to nie zostanie zamknięty obszaru roboczego użytkownika. Zamiast tego usuwa skojarzenie między tego użytkownika i obszaru roboczego. Jeśli użytkownik jest skojarzone z wielu obszarów roboczych, tego użytkownika nadal będzie mógł zalogować się do usługi OMS i zobacz innych obszarów roboczych.

1. W usługi OMS kliknij przycisk kafelka **Ustawienia** .
2. Kliknij kartę **Klienci** , a następnie kliknij kartę **Zarządzanie użytkownikami** .
3. Kliknij przycisk **Usuń** obok nazwy użytkownika, który chcesz usunąć.
4. W oknie dialogowym potwierdzenia kliknij przycisk **Tak**.


### <a name="add-a-group-to-an-existing-workspace"></a>Dodawanie grupy do istniejącego obszaru roboczego

1.  Wykonaj kroki od 1 -4 w "Aby dodać użytkownika do istniejącego obszaru roboczego" powyżej.
2.  W obszarze **Wybierz użytkownika lub grupy**wybierz **grupę**.
    ![Dodawanie grupy do istniejącego obszaru roboczego](./media/log-analytics-manage-access/add-group.png)
3.  Wprowadź nazwę wyświetlaną lub adres E-mail grupy, którą chcesz dodać.
4.  Zaznacz grupę w wynikach listy, a następnie kliknij przycisk **Dodaj**.

## <a name="link-an-existing-workspace-to-an-azure-subscription"></a>Łączenie istniejącego obszaru roboczego z subskrypcji usługi Azure

Istnieje możliwość utworzenia obszaru roboczego z [microsoft.com/oms](https://microsoft.com/oms) witryny sieci Web.  Jednak pewne ograniczenia istnieją dotycząca tych obszarów roboczych najbardziej Godny uwagi o maksymalnie 500MB na dzień przekazywania danych, jeśli korzystasz z bezpłatnego konta. Aby wprowadzić zmiany do tego obszaru roboczego będzie konieczne jest *połączenie istniejącego obszaru roboczego do subskrypcji usługi Azure*.

>[AZURE.IMPORTANT] Aby utworzyć łącze obszaru roboczego, konto Azure musi już masz dostęp do obszaru roboczego, którego chcesz utworzyć łącze.  Innymi słowy konta, którego używasz, aby uzyskać dostęp do portalu Azure musi być **taka sama** jako konto, którego używasz, aby uzyskać dostęp do obszaru roboczego usługi OMS. Jeśli nie jest to możliwe, zobacz [Dodawanie użytkownika do istniejącego obszaru roboczego](#add-a-user-to-an-existing-workspace).

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-oms-portal"></a>Aby utworzyć łącze obszaru roboczego do subskrypcji usługi Azure w portalu usługi OMS

Aby utworzyć łącze obszaru roboczego do subskrypcji usługi Azure w portalu usługi OMS, zalogowany użytkownik musi już konto Azure płatnej. Obszar roboczy, której używasz aktywnie otrzymuje połączone konto Azure.

1. W usługi OMS kliknij przycisk kafelka **Ustawienia** .
2. Kliknij kartę **Klienci** , a następnie kliknij kartę **subskrypcji Azure i planowanie danych** .
3. Kliknij pozycję transmisji danych, którego chcesz użyć.
4. Kliknij przycisk **Zapisz**.  
  ![plany subskrypcji i danych](./media/log-analytics-manage-access/subscription-tab.png)

Nowy plan danych jest wyświetlany na Wstążce portalu usługi OMS w górnej części strony sieci web.

![Usługi OMS wstążki](./media/log-analytics-manage-access/data-plan-changed.png)

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-azure-portal"></a>Aby utworzyć łącze obszaru roboczego do subskrypcji usługi Azure w portalu Azure

1.  Zaloguj się do [portalu Azure](http://portal.azure.com).
2.  Przeglądaj w poszukiwaniu **Usługi dziennika analizy (OMS)** , a następnie zaznacz go.
3.  Zostanie wyświetlona lista istniejących obszarów roboczych. Kliknij przycisk **Dodaj**.  
    ![listy obszarów roboczych](./media/log-analytics-manage-access/manage-access-link-azure01.png)
4.  W obszarze **Usługi OMS obszaru roboczego**kliknij **lub utworzyć łącze istniejące**.  
    ![Łączenie istniejącego](./media/log-analytics-manage-access/manage-access-link-azure02.png)
5.  Kliknij pozycję **Konfiguruj wymagane ustawienia**.  
    ![Konfigurowanie ustawień wymagane](./media/log-analytics-manage-access/manage-access-link-azure03.png)
6.  Zostanie wyświetlona lista obszary robocze, które jeszcze nie są połączone z kontem Azure. Wybierz obszar roboczy.  
    ![Wybierz pozycję obszarów roboczych](./media/log-analytics-manage-access/manage-access-link-azure04.png)
7.  W razie potrzeby można zmienić wartości dla następujących elementów:
    - Subskrypcji
    - Grupa zasobów
    - Lokalizacja
    - Cennik warstwy  
        ![Zmienianie wartości](./media/log-analytics-manage-access/manage-access-link-azure05.png)
8.  Kliknij przycisk **Utwórz**. Obszar roboczy zostanie połączona z kontem Azure.

>[AZURE.NOTE] Jeśli nie widzisz obszaru roboczego, który chcesz połączyć, następnie subskrypcji Azure nie ma dostępu do obszaru roboczego usługi OMS utworzonej przy użyciu usługi OMS witryny sieci Web.  Należy udzielić dostępu do tego konta z wewnątrz obszaru roboczego usługi OMS za pomocą usługi OMS witryny sieci Web. Aby to zrobić, zobacz [Dodawanie użytkownika do istniejącego obszaru roboczego](#add-a-user-to-an-existing-workspace).



## <a name="upgrade-a-workspace-to-a-paid-data-plan"></a>Uaktualnienie obszaru roboczego do planu płatnej danych

Istnieją trzy danych obszaru roboczego planowanie typów usługi OMS: **wolny**, **Standard**i **Premium**.  Jeśli korzystasz z planu *wolny* , może mieć trafisz usługi zakończenie danych 500 MB.  Konieczne będzie uaktualnienie obszaru roboczego do ***planu płatne*** w celu zbierania danych poza ten limit. W dowolnej chwili możesz przekonwertować typu planu.  Aby uzyskać więcej informacji na cennik usługi OMS zobacz [Szczegóły ceny](https://www.microsoft.com/en-us/server-cloud/operations-management-suite/pricing.aspx).

>[AZURE.IMPORTANT] Plany obszaru roboczego można zmienić tylko, jeśli są *połączone* z subskrypcji usługi Azure.  Jeśli utworzona obszaru roboczego w Azure lub jeśli masz *już* połączony obszaru roboczego, można zignorować ten komunikat.  Jeśli utworzono obszaru roboczego z [witryny sieci Web usługi OMS](http://www.microsoft.com/oms), będzie konieczne postępuj zgodnie z instrukcjami na [łącze istniejącego obszaru roboczego do subskrypcji usługi Azure](#link-an-existing-workspace-to-an-azure-subscription).

### <a name="using-entitlements-from-the-oms-add-on-for-system-center"></a>Za pomocą uprawnień z dodatku usługi OMS dla System Center

Dodatek usługi OMS dla System Center udostępnia uprawnienia planu Premium analizy dziennika usługi OMS, opisane w [Cennik usługi OMS](https://www.microsoft.com/en-us/server-cloud/operations-management-suite/pricing.aspx).

Po zakupieniu usługi OMS dodatek dla programu System Center dodatku usługi OMS zostanie dodane jako tytułu na umowie System Center. Można wprowadzić dowolną Azure subskrypcji, która zostanie utworzona w ramach niniejszej Umowy za pomocą uprawnienia. Dzięki temu można na przykład mieć wiele obszarów roboczych usługi OMS korzystające z uprawnienia z dodatku usługi OMS.

W celu zapewnienia zastosowania z obszarem roboczym usługi OMS zastosowana do swojego uprawnień z dodatku usługi OMS, musisz:

1. Łącze usługi OMS obszaru roboczego do subskrypcji usługi Azure, który jest częścią Enterprise Agreement, zawierającą zakupu dodatków usługi OMS i zastosowania Azure subskrypcji
2. Wybierz plan Premium w obszarze roboczym

Po przejrzeniu do użycia w portalu Azure lub usługi OMS nie zobaczysz uprawnień dodatku usługi OMS. Można jednak Zobacz uprawnienia w portalu Enterprise.  

Jeśli trzeba zmienić subskrypcję Azure połączony usługi OMS obszaru roboczego, można użyć polecenia cmdlet programu PowerShell Azure [AzureRmResource Przenieś](https://msdn.microsoft.com/library/mt652516.aspx) .

### <a name="using-azure-commitment-from-an-enterprise-agreement"></a>Za pomocą Azure zobowiązania z Umowa dla przedsiębiorstw

Jeśli chcesz używać autonomicznego ceny składników usługi OMS, będą płacić dla każdego składnika usługi OMS oddzielnie i obciążenie pojawią się na rachunku Azure.

Jeśli masz Azure Zatwierdź pieniężnych przy rejestracji przedsiębiorstwa, do której są połączone subskrypcji Azure wszelkie zastosowania analizy dziennika zostanie automatycznie obciążania przed wszystkie pozostałe Zatwierdź pieniężne.

Jeśli chcesz zmienić subskrypcję Azure, że obszar roboczy usługi OMS jest połączony z możesz użyć polecenia cmdlet programu PowerShell Azure [AzureRmResource Przenieś](https://msdn.microsoft.com/library/mt652516.aspx) .  



### <a name="to-change-a-workspace-to-a-paid-data-plan"></a>Aby zmienić obszaru roboczego do planu płatnej danych

1.  Zaloguj się do [portalu Azure](http://portal.azure.com).
2.  Przeglądaj w poszukiwaniu **Usługi dziennika analizy (OMS)** , a następnie zaznacz go.
3.  Zostanie wyświetlona lista istniejących obszarów roboczych. Wybierz obszar roboczy.  
    ![listy obszarów roboczych](./media/log-analytics-manage-access/manage-access-change-plan01.png)
4.  W obszarze **Ustawienia**kliknij przycisk **warstwy ceny**.  
    ![Cennik warstwy](./media/log-analytics-manage-access/manage-access-change-plan02.png)
5.  W obszarze **ceny warstwa**wybierz plan danych, a następnie kliknij przycisk **Wybierz**.  
    ![Wybierz plan](./media/log-analytics-manage-access/manage-access-change-plan03.png)
6.  Podczas odświeżania widoku w portalu Azure pojawi się **Warstwa ceny** aktualizacja dla wybranego planu.  
    ![Aktualizacja ceny warstwy](./media/log-analytics-manage-access/manage-access-change-plan04.png)

Teraz może zbierać dane poza zakończenie "bezpłatne" dane.


## <a name="add-an-azure-active-directory-organization-to-an-existing-workspace"></a>Dodawanie Azure Active Directory organizacji do istniejącego obszaru roboczego

Możesz skojarzyć obszaru roboczego usługi dziennika analizy (OMS) z domeną usługi Azure Active Directory. Umożliwia dodawanie użytkowników z usługi Active Directory bezpośrednio do obszaru roboczego usługi OMS bez konieczności osobne konta Microsoft.

Tworzenie obszaru roboczego z poziomu portalu Azure lub łącze obszaru roboczego do subskrypcji usługi Azure usługi Azure Active Directory zostanie połączony jako konta swojej organizacji.

Podczas tworzenia obszaru roboczego z portalu usługi OMS zostanie wyświetlony monit o łącze do subskrypcji usługi Azure i konto organizacji.

### <a name="to-add-an-azure-active-directory-organization-to-an-existing-workspace"></a>Aby dodać Azure Active Directory organizacji do istniejącego obszaru roboczego

1. Na stronie Ustawienia w usługi OMS kliknij pozycję **konta** , a następnie kliknij kartę **Informacje o obszarze roboczym** .  
2. Przejrzyj informacje dotyczące konta organizacji, a następnie kliknij **Dodaj organizacji**.  
    ![Dodawanie organizacji](./media/log-analytics-manage-access/manage-access-add-adorg01.png)
3. Wprowadź informacje o tożsamości dla administratora domeny usługi Azure Active Directory. Później pojawi się potwierdzenie informacją, że obszaru roboczego jest połączony z domeną usługi Azure Active Directory.
    ![Potwierdzanie połączonych obszaru roboczego](./media/log-analytics-manage-access/manage-access-add-adorg02.png)

>[AZURE.NOTE] Gdy Twoje konto jest połączone z konta organizacji, łączenia nie można usunąć lub zmienić.

## <a name="close-your-oms-workspace"></a>Zamykanie usługi OMS obszaru roboczego

Po zamknięciu z obszarem roboczym usługi OMS wszystkie dane związane z obszaru roboczego zostanie usunięty z usługę w ciągu 30 dni zamknięcia obszaru roboczego.

Jeśli jesteś administratorem i jest wielu użytkowników skojarzone z obszaru roboczego, skojarzenie między tych użytkowników i obszaru roboczego zostało przerwane. Jeśli użytkownicy są skojarzone z innych obszarów roboczych, następnie są nadal usługi OMS za pomocą tych obszarów roboczych. Jednak jeśli nie są one skojarzone z innych obszarów roboczych następnie musisz utworzyć nowy obszar roboczy, aby użyć usługi OMS.

### <a name="to-close-an-oms-workspace"></a>Aby zamknąć obszar roboczy usługi OMS

1. W usługi OMS kliknij przycisk kafelka **Ustawienia** .
2. Kliknij kartę **Klienci** , a następnie kliknij kartę **Informacje o obszarze roboczym** .
3. Kliknij pozycję **Zamknij obszar roboczy**.
4. Wybierz jedną z przyczyn zamknięcia obszaru roboczego, lub wprowadź inną przyczynę w polu tekstowym.
5. Kliknij pozycję **Zamknij obszar roboczy**.

## <a name="next-steps"></a>Następne kroki

- Zobacz [Łączenie komputery do analizy dziennika](log-analytics-windows-agents.md) , aby dodać agentów i zbieranie danych.
- [Dodawanie analizy dziennika rozwiązań z galerii rozwiązań](log-analytics-add-solutions.md) Dodawanie funkcji do zbierania danych.
- [Konfigurowanie ustawień serwera proxy i zapory w dzienniku analizy](log-analytics-proxy-firewall.md) Jeśli Twoja organizacja korzysta serwer proxy lub zapory, aby czynników można komunikować się z usługą analizy dziennika.
