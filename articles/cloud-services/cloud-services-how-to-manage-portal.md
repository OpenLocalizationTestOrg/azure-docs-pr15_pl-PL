<properties 
    pageTitle="Typowe zadania zarządzania usługi w chmurze | Microsoft Azure" 
    description="Dowiedz się, jak zarządzać usługami w chmurze w portalu Azure. W tych przykładach użyto Azure portal." 
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
    ms.date="08/02/2016"
    ms.author="adegeo"/>


# <a name="how-to-manage-cloud-services"></a>Jak zarządzać usługami w chmurze

> [AZURE.SELECTOR]
- [Azure portal](cloud-services-how-to-manage-portal.md)
- [Portal Azure klasyczny](cloud-services-how-to-manage.md)

Usługa w chmurze odbywa się w obszarze **Usług w chmurze (klasyczny)** Azure portal. W tym artykule opisano kilka typowych akcji, które chcesz wykonać podczas zarządzania usługami w chmurze. Zawierająca aktualizowanie, usuwanie, skalowania i promowanie etapowej wdrożenia do produkcji.

Więcej informacji na temat sposobu skalowania usługi w chmurze jest dostępna [w tym miejscu](cloud-services-how-to-scale-portal.md).

## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Jak: aktualizowanie chmury usługi roli lub rozmieszczania

Jeśli musisz zaktualizować kod aplikacji usługi w chmurze, na karta Usługa w chmurze za pomocą **aktualizacji** . Możesz zaktualizować rolę jednego lub wszystkich ról. Aby zaktualizować, możesz przekazać pakiet usługi lub plik konfiguracyjny usługi.

1. W [Azure portal][]wybierz pozycję Usługa w chmurze, który chcesz zaktualizować. W tym kroku zostanie wyświetlona karta wystąpienie usługi cloud.

2. W karta kliknij przycisk **Aktualizuj** .

    ![Przycisk Aktualizuj](./media/cloud-services-how-to-manage-portal/update-button.png)

3. Zaktualizuj wdrażanie nowy plik pakietu usługi (.cspkg) i plik konfiguracyjny usługi (.cscfg).

    ![UpdateDeployment](./media/cloud-services-how-to-manage-portal/update-blade.png)

4. **Opcjonalnie** Aktualizuj etykiety wdrożenia i konta miejsca do magazynowania. 

5. Jeśli ról masz tylko jedno wystąpienie roli, wybierz pozycję **Wdrażanie, nawet jeśli jedną lub więcej ról zawierają jedno wystąpienie** włączenia uaktualnienia kontynuować. 

    Każda rola ma co najmniej dwa wystąpienia roli (maszyn wirtualnych) Azure tylko może zagwarantować dostępność usługi procent 99,95 podczas aktualizacji usługi cloud. Dwa wystąpienia roli maszyn wirtualnych będzie przetwarzał żądania klienta podczas drugi jest aktualizowana.

6. Sprawdzanie, **Rozpocznij wdrożenie** , aby zainstalować aktualizację stosowane po zakończeniu przekazywania pakietu.

7. Kliknij **przycisk OK** , aby rozpocząć aktualizacji usługi.



## <a name="how-to-swap-deployments-to-promote-a-staged-deployment-to-production"></a>Jak: Zamień wdrożenia, aby zapewnić promocję etapowej wdrożenia do produkcji

Jeśli zdecydujesz się na wdrażanie nowa wersja usługi w chmurze, etapu i przetestować do nowej wersji w chmurze środowisko tymczasowy usługi. Umożliwia **zamianę** przełączenie adresy URL, za pomocą których dwóch wdrożeń dotyczą i promowanie nowej wersji do produkcji. 

Możesz zamienić wdrożenia na stronie **Usług w chmurze** lub pulpitu nawigacyjnego.

1. W [Azure portal][]wybierz pozycję Usługa w chmurze, który chcesz zaktualizować. W tym kroku zostanie wyświetlona karta wystąpienie usługi cloud.

2. W karta kliknij przycisk **Zamień** .

    ![Zamienianie usług w chmurze](./media/cloud-services-how-to-manage-portal/swap-button.png)

3. Zostanie wyświetlona następujący monit o potwierdzenie.

    ![Zamienianie usług w chmurze](./media/cloud-services-how-to-manage-portal/swap-prompt.png)

4. Po upewnieniu się informacje na temat wdrażania, kliknij przycisk **OK** , aby zamienić wdrożeniach.

    Zamienianie wdrożenia spowodowany szybko wirtualnych adresów IP (służącą) jest jedynym elementem, który zmienia się w przypadku wdrożeń.

    Kosztów obliczeń, możesz usunąć wdrożenia tymczasowy po upewnieniu się, że wdrożenia produkcji działa zgodnie z oczekiwaniami.

## <a name="how-to-link-a-resource-to-a-cloud-service"></a>Jak: łączenie zasobu do usługi w chmurze

Azure portal nie łączenie zasobów tak, czy bieżący Azure portalu klasycznego. Należy wdrożyć dodatkowe zasoby do tej samej grupy zasobów, które są używane przez usługi w chmurze.

## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Jak: usuwanie wdrożeń i usługi w chmurze

Przed usunięciem usługi w chmurze, musisz usunąć każdego istniejącego wdrożenia.

Kosztów obliczeń, możesz usunąć wdrożenia tymczasowy po upewnieniu się, że rozmieszczenia produkcji działa zgodnie z oczekiwaniami. Konta kosztów do uruchamiania wystąpień wdrożonym roli, które zostały zatrzymane.

Poniższa procedura umożliwia usuwanie wdrożeniu lub usługa w chmurze. 

1. W [Azure portal][]wybierz pozycję Usługa w chmurze, które chcesz usunąć. W tym kroku zostanie wyświetlona karta wystąpienie usługi cloud.

2. W karta kliknij przycisk **Usuń** .

    ![Zamienianie usług w chmurze](./media/cloud-services-how-to-manage-portal/delete-button.png)

3. Można usunąć usługi w chmurze całego, zaznaczając pole wyboru **Usługa w chmurze i jego wdrożeniach** lub wybierz **wdrożenia produkcji** lub **tymczasowego wdrożenia**.

    ![Zamienianie usług w chmurze](./media/cloud-services-how-to-manage-portal/delete-blade.png) 

4. Kliknij przycisk **Usuń** u dołu.

5. Aby usunąć usługi w chmurze, kliknij pozycję **Usuń usługi w chmurze**. Następnie w monicie o potwierdzenie, kliknij przycisk **Tak**.

> [AZURE.NOTE]
> Gdy zostanie usunięty usługi w chmurze i monitorowanie pełne jest skonfigurowany, danych trzeba usunąć ręcznie z Twojego konta miejsca do magazynowania. Aby dowiedzieć się, gdzie można znaleźć w tabelach metryki zobacz [ten](cloud-services-how-to-monitor.md) artykuł.

[Azure portal]: https://portal.azure.com

## <a name="next-steps"></a>Następne kroki

* [Ogólne Konfiguracja usługi w chmurze](cloud-services-how-to-configure-portal.md).
* Dowiedz się, jak [wdrożyć usługi w chmurze](cloud-services-how-to-create-deploy-portal.md).
* Skonfiguruj [niestandardową nazwę domeny](cloud-services-custom-domain-name-portal.md).
* Konfigurowanie [certyfikatów ssl](cloud-services-configure-ssl-certificate-portal.md).