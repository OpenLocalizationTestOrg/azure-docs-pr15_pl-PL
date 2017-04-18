<properties 
    pageTitle="Chmury usługi zadań administracyjnych (klasyczny) | Microsoft Azure" 
    description="Dowiedz się, jak zarządzać usługami w chmurze w portalu klasyczny Azure." 
    services="cloud-services" 
    documentationCenter="" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/10/2016"
    ms.author="adegeo"/>





# <a name="how-to-manage-cloud-services"></a>Jak zarządzać usługami w chmurze

> [AZURE.SELECTOR]
- [Azure portal](cloud-services-how-to-manage-portal.md)
- [Portal Azure klasyczny](cloud-services-how-to-manage.md)

W obszarze **Usług w chmurze** portalu klasyczny Azure można zaktualizować roli usług lub wdrożeniu, promowanie etapowej wdrożenia do produkcji, łącze zasobów do usługi w chmurze, dzięki czemu będzie Zobacz zależności zasobów i skalowanie zasobów razem i usuwanie usługi w chmurze lub wdrożeniu.


## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Jak: aktualizowanie chmury usługi roli lub rozmieszczania

Jeśli musisz zaktualizować kod aplikacji usługi w chmurze, na stronie pulpitu nawigacyjnego, **Usług w chmurze** lub **wystąpienia** strony za pomocą **aktualizacji** . Możesz zaktualizować rolę jednego lub wszystkich ról. Musisz przekazać nowy pakiet usługi i plik konfiguracyjny usługi.

1. W [portalu klasyczny Azure](https://manage.windowsazure.com/)na stronie pulpitu nawigacyjnego, **Usług w chmurze** lub **wystąpienia** strony kliknij przycisk **Aktualizuj**.

    ![UpdateDeployment](./media/cloud-services-how-to-manage/CloudServices_UpdateDeployment.png)

2. **Wdrożenie etykiety**wprowadź nazwę identyfikującą wdrożenia (na przykład mycloudservice4). Etykieta wdrożenia w obszarze **Szybki start** można znaleźć na pulpicie nawigacyjnym.

3. W **pakiecie**użyj **przycisku Przeglądaj** , aby przekazać plik pakietu usługi (.cspkg).

4. W **konfiguracji**użyj **przycisku Przeglądaj** , aby przekazać plik konfiguracyjny usługi (.cscfg).

5. Jeśli chcesz uaktualnić wszystkich ról w usłudze w chmurze w **roli**, wybierz pozycję **wszystko** . Aby przeprowadzić aktualizację pojedynczej roli, wybierz rolę, którą chcesz zaktualizować. Nawet jeśli zaznaczono konkretnej roli, aby zaktualizować aktualizacji w pliku konfiguracji usługi są stosowane do wszystkich ról.

6. Jeśli aktualizacja zmieni liczbę ról lub rozmiar dowolnej roli, zaznacz pole wyboru **Zezwalaj aktualizacji, jeśli zmiany rozmiarów roli lub liczbę ról** , umożliwiające aktualizację kontynuować. 

    Należy pamiętać, że zmiana rozmiaru roli (oznacza to, że rozmiar maszyny wirtualnej obsługującego wystąpienie roli) lub liczba ról każde wystąpienie roli (maszyna wirtualna) muszą być ponownie obrazowanych i wszelkie dane lokalne zostaną utracone.

7. Jeśli ról usługi masz tylko jedno wystąpienie roli, wybierz pozycję **aktualizacji, nawet jeśli co najmniej jeden roli zawierają pole wyboru jedno wystąpienie** umożliwiające uaktualnienie kontynuować. 

    Każda rola ma co najmniej dwa wystąpienia roli (maszyn wirtualnych) Azure tylko może zagwarantować dostępność usługi procent 99,95 podczas aktualizacji usługi cloud. Umożliwiająca maszyn wirtualnych przetwarzania żądania klienta drugi się podczas aktualizacji.

8. Kliknij **przycisk OK** (znacznik wyboru), aby rozpocząć aktualizacji usługi.



## <a name="how-to-swap-deployments-to-promote-a-staged-deployment-to-production"></a>Jak: Zamień wdrożeniach, aby zapewnić promocję etapowej wdrożenia do produkcji

Za pomocą funkcji **Zamień** promowanie tymczasowy wdrażania usługi w chmurze z produkcją. Jeśli zdecydujesz się na wdrażanie nowa wersja usługi w chmurze, możesz etapu i przetestować do nowej wersji w środowisku chmury usługi tymczasowy, gdy klienci bieżącej wersji produkcji. Gdy wszystko będzie już gotowe do promowania nowej wersji produkcji, umożliwia **Zamień** przełączanie adresy URL, które dotyczą dwóch wdrożenia. 

Możesz zamienić wdrożenia na stronie **Usług w chmurze** lub pulpitu nawigacyjnego.

1. W [portalu klasyczny Azure](https://manage.windowsazure.com/)kliknij **Usług w chmurze**.

2. Na liście usług w chmurze kliknij pozycję Usługa w chmurze, aby go zaznaczyć.

3. Kliknij przycisk **Zamień**.

    Zostanie wyświetlona następujący monit o potwierdzenie.

    ![Zamienianie usług w chmurze](./media/cloud-services-how-to-manage/CloudServices_Swap.png)

4. Po upewnieniu się informacje na temat wdrażania, kliknij przycisk **Tak,** Aby zamienić wdrożenia.

    Zamienianie wdrożenia spowodowany szybko wirtualne adresy IP (służącą) jest jedynym elementem, który zmienia się w przypadku wdrożeń.

    Aby zapisać koszty obliczeń, możesz usunąć wdrożenia w środowisku tymczasowy po upewnieniu się, że nowe wdrożenia produkcji działa zgodnie z oczekiwaniami.

## <a name="how-to-link-a-resource-to-a-cloud-service"></a>Jak: łączenie zasobu do usługi w chmurze

Aby wyświetlić usługi cloud zależności usługi na inne zasoby, można połączyć wystąpienia bazy danych SQL Azure lub konta magazynu usługi w chmurze. Można połączyć i odłączyć zasobów na stronie **Połączone zasobów** , a następnie monitorowanie ich zastosowania na pulpicie nawigacyjnym usługi cloud. Jeśli konto połączone miejsca do magazynowania ma monitorowania włączone, można monitorować całkowita liczba żądań na pulpicie nawigacyjnym usługi cloud.

Używanie **łącza** do łączenia nowej lub istniejącej bazy danych SQL wystąpienia lub miejsce do magazynowania konto usługi w chmurze. Następnie można skalować bazy danych oraz role usługi cloud, która jest używany na stronie **Skala** . (Konto miejsca do magazynowania skale automatycznie wraz ze wzrostem zastosowania.) Aby uzyskać więcej informacji zobacz [jak skalowanie usługi w chmurze i połączonych zasobów](cloud-services-how-to-scale.md). 

Możesz również można monitorować, zarządzanie i skalowanie bazy danych w węźle **baz danych** portalu klasyczny Azure. 

"Łączenie" zasobu w ten sposób nie Połącz aplikacji do zasobu. Po utworzeniu nowej bazy danych za pomocą **łącza**, musisz dodać parametry połączenia w kodzie aplikacji, a następnie usługi w chmurze. Musisz także dodać parametry połączenia, jeśli aplikacji używa zasobów z magazynu połączone konta.

Poniższa procedura zawiera opis sposobu łącze nowego wystąpienia bazy danych SQL wdrożone na serwerze nowej bazy danych SQL, do usługi w chmurze.

### <a name="to-link-a-sql-database-instance-to-a-cloud-service"></a>Aby utworzyć łącze wystąpienie bazy danych SQL do usługi w chmurze

1. W [portalu klasyczny Azure](http://manage.windowsazure.com/)kliknij **Usług w chmurze**. Następnie kliknij nazwę usługi w chmurze otworzyć pulpitu nawigacyjnego.

2. Kliknij pozycję **zasoby połączone**.

    Zostanie wyświetlona strona **Połączone zasobów** .

    ![LinkedResourcesPage](./media/cloud-services-how-to-manage/CloudServices_LinkedResourcesPage.png)

3. Kliknij **łącze zasób** lub **łącze**.

    Zostanie uruchomiony Kreator **Łącze zasobów** .

    ![Strona 1 łącza](./media/cloud-services-how-to-manage/CloudServices_LinkedResources_LinkPage1.png)

4. Kliknij pozycję **Utwórz nowy zasób** lub **łącze istniejący zasób**.

5. Wybierz typ zasobu, aby utworzyć łącze. W [portalu klasyczny Azure](http://manage.windowsazure.com/)kliknij **Bazy danych SQL**. (Portal klasyczny Preview Azure nie obsługuje łączenia konto miejsca do magazynowania do usługi w chmurze.)

6. Aby zakończyć konfigurowanie bazy danych, postępuj zgodnie instrukcjami pomocy dla obszaru **Baz danych programu SQL** Azure portalu klasyczny.

    Można śledzić postęp operacji łączenia w obszarze wiadomości.

    ![Postęp łączenia](./media/cloud-services-how-to-manage/CloudServices_LinkedResources_LinkProgress.png)

    Po zakończeniu łączenia można monitorować stan zasobu połączone na pulpicie nawigacyjnym usługi cloud. Aby dowiedzieć się, jak skalowania połączonej bazy danych SQL zobacz [jak skalowanie usługi w chmurze i połączonych zasobów](cloud-services-how-to-scale.md).

### <a name="to-unlink-a-linked-resource"></a>Aby odłączyć połączonych zasobów

1. W [portalu klasyczny Azure](http://manage.windowsazure.com/)kliknij **Usług w chmurze**. Następnie kliknij nazwę usługi w chmurze do otwierania pulpitu nawigacyjnego.

2. Kliknij pozycję **Zasoby połączone**, a następnie wybierz zasób.

3. Kliknij przycisk **Rozłącz**. Następnie kliknij przycisk **Tak** w monicie o potwierdzenie.

    Rozłączanie z bazą danych SQL nie ma wpływu na bazę danych lub aplikacji połączenia z bazą danych. Nadal można zarządzać bazy danych w obszarze **Bazy danych programu SQL** Azure portalu klasyczny.



## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Jak: usuwanie wdrożeń i usługi w chmurze

Przed usunięciem usługi w chmurze, musisz usunąć każdego istniejącego wdrożenia.

Aby zapisać koszty obliczeń, możesz usunąć tymczasowy wdrożenia po upewnieniu się, że wdrożenia produkcji działa zgodnie z oczekiwaniami. Jesteś rozliczonym obliczeń kosztów dla roli wystąpienia, nawet jeśli nie działa usługa w chmurze.

Poniższa procedura umożliwia usuwanie wdrożeniu lub usługa w chmurze. 

1. W [portalu klasyczny Azure](http://manage.windowsazure.com/)kliknij **Usług w chmurze**.

2. Wybierz pozycję Usługa w chmurze, a następnie kliknij polecenie **Usuń**. (Aby wybrać usługi w chmurze bez otwierania pulpitu nawigacyjnego, kliknij dowolne miejsce z wyjątkiem nazwy we wpisie usługa w chmurze).

    Jeśli masz wdrożenia w tymczasowym lub produkcji, pojawi się menu opcji podobny do tego następujące w dolnej części okna. Przed usunięciem usługi w chmurze, musisz usunąć wszelkie istniejące wdrożeniach.

    ![Usuwanie Menu](./media/cloud-services-how-to-manage/CloudServices_DeleteMenu.png)


3. Aby usunąć wdrożeniu, kliknij **usunięcie wdrożenia produkcji** lub **usunięcie tymczasowy wdrożenia**. Następnie w monicie o potwierdzenie, kliknij przycisk **Tak**. 

4. Jeśli zamierzasz usunąć usługi w chmurze, powtórz krok 3, jeśli to konieczne, aby usunąć innych wdrożenia.

5. Aby usunąć usługi w chmurze, kliknij pozycję **Usuń usługi w chmurze**. Następnie w monicie o potwierdzenie, kliknij przycisk **Tak**.

> [AZURE.NOTE]
> Jeśli pełne monitorowania skonfigurowano usługi w chmurze, Azure nie powoduje usunięcia danych monitorowania z Twojego konta miejsca do magazynowania po usunięciu usługi w chmurze. Należy ręcznie usunąć dane. Aby dowiedzieć się, gdzie można znaleźć w tabelach metryki, zobacz "jak: pełne, monitorowanie danych spoza portalu klasyczny Azure dostępu" w temacie [jak Monitor usług w chmurze](cloud-services-how-to-monitor.md).

## <a name="next-steps"></a>Następne kroki

 * [Ogólne Konfiguracja usługi w chmurze](cloud-services-how-to-configure.md).
* Dowiedz się, jak [wdrożyć usługi w chmurze](cloud-services-how-to-create-deploy.md).
* Skonfiguruj [niestandardową nazwę domeny](cloud-services-custom-domain-name.md).
* Konfigurowanie [certyfikatów ssl](cloud-services-configure-ssl-certificate.md).
