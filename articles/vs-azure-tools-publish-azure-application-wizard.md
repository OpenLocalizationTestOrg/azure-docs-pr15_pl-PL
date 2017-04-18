<properties 
   pageTitle="Kreator aplikacji Azure publikacji | Microsoft Azure"
   description="Kreator aplikacji Azure publikacji"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="publish-azure-application-wizard"></a>Kreator aplikacji Azure publikacji

## <a name="overview"></a>Omówienie

Po opracowywania aplikacji sieci web w programie Visual Studio, można opublikować łatwiej aplikacji usługi w chmurze Azure za pomocą kreatora **Publikowania aplikacji Azure** . Pierwsza sekcja przedstawiono kroki należy wykonać przed za pomocą kreatora, a pozostałe sekcjach opisano funkcje kreatora.

>[AZURE.NOTE] Ten temat dotyczy wdrażanie usług w chmurze, nie do witryn sieci web. Aby uzyskać informacji na temat wdrażania witryn sieci web zobacz [jak wdrożyć serwer Azure witryny sieci Web](https://social.msdn.microsoft.com/Search/windowsazure?query=How%20to%20Deploy%20an%20Azure%20Web%20Site&Refinement=138&ac=4#refinementChanges=117&pageNumber=1&showMore=false).

## <a name="prerequisites"></a>Wymagania wstępne

Przed opublikowaniem aplikacji sieci web Azure, trzeba mieć konto Microsoft i Azure subskrypcji, a należy skojarzyć aplikacji sieci web z usługą Azure chmury. Jeśli te zadania już zostały wykonane, można przejść do następnej sekcji.

1. Uzyskiwanie konta Microsoft i Azure subskrypcji. Możesz wypróbować bezpłatną jeden miesiąc bezpłatna subskrypcja Azure [tutaj](https://azure.microsoft.com/pricing/free-trial/)

1. Tworzenie usługi w chmurze i konto miejsca do magazynowania Azure. Można to zrobić Eksploratora serwera w programie Visual Studio lub za pomocą [portal Azure klasyczny](http://go.microsoft.com/fwlink/?LinkID=213885).

1. Włączanie aplikacji sieci web Azure. Aby umożliwić aplikacji sieci web publikowane Azure z programu Visual Studio, musisz skojarzyć projekt usługi Azure chmury w programie Visual Studio. Aby utworzyć projekt usługi cloud skojarzone, otwórz menu skrótów dla projektu dla aplikacji sieci web, a następnie wybierz Konwertuj **Konwertowanie Azure chmury usługi Project**.

1. Po dodaniu do rozwiązania projektu usługi cloud ponownie otworzyć tego samego menu skrótów, a następnie wybierz pozycję **Publikuj**. Aby uzyskać więcej informacji na temat włączania wniosków o Azure, zobacz [jak: Migrowanie i publikowanie aplikacji sieci Web do usługi w chmurze Azure z programu Visual Studio](https://msdn.microsoft.com/library/azure/hh420322.aspx).

>[AZURE.NOTE] Pamiętaj uruchomić program Visual Studio przy użyciu poświadczeń administratora (Uruchom jako Administrator).

1. Gdy wszystko będzie już gotowe do opublikowania aplikacji, otwórz menu skrótów dla projektu usługi Azure chmurze, a następnie wybierz **Publikuj**. Poniższe kroki pokazują Kreatora publikowania aplikacji Azure.

## <a name="choosing-your-subscription"></a>Wybieranie subskrypcji

### <a name="to-choose-a-subscription"></a>Aby wybrać subskrypcji

1. Przed skorzystaniem z kreatora po raz pierwszy, musisz się zalogować. Wybierz łącze **Zaloguj się** . Zaloguj się do portalu Azure po wyświetleniu monitu i podaj nazwę Azure użytkownika i hasło. 

    ![Jest to jeden z ekranów Kreatora publikowania](./media/vs-azure-tools-publish-azure-application-wizard/IC799159.png)

    Lista dostępnych subskrypcji wypełnia z subskrypcją skojarzonego z kontem. Może też wyświetlić subskrypcje z pliki subskrypcji, które wcześniej zaimportowane.

1. Na liście **Wybierz Twoja subskrypcja** Wybierz subskrypcję służących do wdrażania.

   Jeśli wybierzesz **< Zarządzaj >**, zostanie wyświetlone okno dialogowe **Zarządzaj subskrypcjami** i możesz wybrać konto subskrypcji i użytkownika, dla którego chcesz użyć. Na karcie **konta** pokazuje wszystkie swoje konta, a na karcie **Subskrypcje** pokazuje wszystkie subskrypcje skojarzony z konta. Możesz również wybrać region, z których można korzystać z zasobów Azure, a także tworzenia lub importowania certyfikatów dla subskrypcji z portalu Azure. Po zaimportowaniu żadnej subskrypcji z pliku subskrypcji skojarzone certyfikaty pojawi się na karcie **Certyfikaty** . Gdy skończysz, wybierz przycisk **Zamknij** .

    ![Zarządzanie subskrypcjami](./media/vs-azure-tools-publish-azure-application-wizard/IC799160.png)

    >[AZURE.NOTE] Subskrypcja może zawierać więcej niż jedną subskrypcję.

1. Wybierz przycisk **Dalej** , aby kontynuować. 

    Jeśli nie usług w chmurze w ramach subskrypcji, należy utworzyć usługi w chmurze platformy Azure do obsługi projektu. Zostanie wyświetlone okno dialogowe **Tworzenie usługi w chmurze i konto miejsca do magazynowania** .

    Określ nazwę usługi w chmurze. Nazwa musi być unikatowa w Azure. Następnie określ region lub grupie koligacji centrum danych, które znajduje się w pobliżu możesz lub większość klientów. Ta nazwa jest również używana dla nowego konta miejsca do magazynowania, Azure tworzące dla usługi w chmurze.

1. Modyfikowanie ustawienia mają ten wdrożenia, a następnie opublikować go, wybierając przycisk **Publikuj** (następnej sekcji zawiera szczegółowe informacje o różnych ustawień). Aby zapoznać się z ustawienia przed opublikowaniem, wybierz przycisk **Dalej** .

    >[AZURE.NOTE] Jeśli wybierzesz Publikuj w tym kroku można monitorować stan tego rozmieszczenia w programie Visual Studio.

Za pomocą kreatora **Publikowania aplikacji Azure** , można modyfikować zarówno wspólne, jak i zaawansowane ustawienia wdrożenia. Na przykład możesz wybrać ustawienie wdrażania aplikacji do testowania przed udostępnieniem. Na poniższej ilustracji przedstawiono kartę **Ustawienia wspólne** dla wdrożenia usługi Azure.

![Typowe ustawienia](./media/vs-azure-tools-publish-azure-application-wizard/IC749013.png)

## <a name="configuring-your-publish-settings"></a>Konfigurowanie usługi ustawienia publikowania

### <a name="to-configure-the-publish-settings"></a>Aby skonfigurować ustawienia publikowania

1. Na liście **usługi w chmurze** wykonaj jedną z następujących czynności:

   1. W polu listy rozwijanej wybierz istniejące usługi w chmurze. Lokalizacja centrum danych usługi zostanie wyświetlona. Należy Zapisz tę lokalizację i upewnij się, że do lokalizacji przechowywania konta znajduje się w tym samym centrum danych.

    1. Wybierz pozycję **Utwórz nowy** Tworzenie usługi w chmurze obsługującego Azure. W oknie dialogowym **Tworzenie usługi w chmurze** Podaj nazwę usługi, a następnie określ region lub grupę koligacji, aby określić lokalizację centrum danych, który chcesz udostępnić usługi w chmurze. Nazwa musi być unikatowa w Azure.

1. Na liście **środowiska** wybierz **produkcji** lub **tymczasowego**. Wybierz pozycję środowiska wzorcowego, jeśli chcesz wdrożyć aplikację, aby środowisku testowym. Możesz przenieść aplikacji w środowisku produkcyjnym później.

1. Na liście **tworzenia konfiguracji** wybierz **Debugowanie** lub **wersji**.

1. Na liście **Konfiguracja usługi** wybierz **chmury** lub **lokalny**.

    Zaznacz pole wyboru **Włącz pulpit zdalny dla wszystkich ról** , jeśli chcesz można było zdalnie połączyć się z usługą. Ta opcja jest używana przede wszystkim dotyczących rozwiązywania problemów. Po zaznaczeniu tego pola wyboru, zostanie wyświetlone okno dialogowe **Konfiguracja usług pulpitu zdalnego** . Wybierz łącze ustawienia, aby zmienić konfigurację.

    Zaznacz pole wyboru **Włącz wdrażanie sieci Web dla wszystkich ról w sieci web** , aby włączyć wdrażanie sieci web dla usługi. Należy włączyć pulpitu zdalnego użyć tej funkcji. Aby uzyskać więcej informacji zobacz [[Publikowanie usługi w chmurze za pomocą narzędzi Azure](https://msdn.microsoft.com/library/azure/ff683672.aspx)](https://msdn.microsoft.com/library/azure/ff683672.aspx). Aby uzyskać więcej informacji na temat wdrażania sieci Web zobacz [[Publikowanie usługi w chmurze za pomocą narzędzi Azure](https://msdn.microsoft.com/library/azure/ff683672.aspx)](https://msdn.microsoft.com/library/azure/ff683672.aspx).

1. Wybierz kartę **Ustawienia zaawansowane** . W polu **Etykieta wdrożenia** zaakceptować domyślną nazwę lub wpisz nazwę. Aby dołączyć datę do etykiety wdrożenia, pozostaw zaznaczone pole wyboru.

    ![Trzecim ekranie Kreatora publikowania](./media/vs-azure-tools-publish-azure-application-wizard/IC749014.png)

1. Na liście **miejsca do magazynowania konto** wybierz konto miejsca do magazynowania dla tego wdrożenia. Porównanie lokalizacje centrami danych usługi w chmurze i konta miejsca do magazynowania. Najlepiej, jeśli te lokalizacje powinna być taka sama.

    >[AZURE.NOTE] Konto Azure magazynowania przechowuje pakiet wdrażania aplikacji. Po wdrożeniu aplikacji pakietu zostanie usunięta z konta miejsca do magazynowania.

1. Zaznacz pole wyboru **Aktualizuj wdrożenia** , jeśli chcesz wdrożyć tylko zaktualizowane składniki. Tego typu wdrożenia może być większa niż pełne wdrożenie. Wybierz łącze **Ustawienia** , aby otworzyć okno dialogowe **wdrożenia aktualizowanie ustawień** , pokazano na poniższej ilustracji. 

    ![Ustawienia wdrażania](./media/vs-azure-tools-publish-azure-application-wizard/IC617060.png)

    Możesz wybrać jedną z dwóch opcji wdrażanie aktualizacji, przyrostowe lub jednoczesnego. Przyrostowe wdrożenie aktualizacji jednego wystąpienia wdrożonym w danym momencie, tak, aby aplikacja pozostaje online i dostępne dla użytkowników. Jednoczesne wdrożenie aktualizuje wszystkie wystąpienia wdrożonym jednocześnie. Równoczesnej aktualizacji jest większa niż przyrostowe aktualizacji, ale jeśli wybierzesz tę opcję, aplikacja mogą być niedostępne podczas procesu aktualizacji.

    Należy zaznaczyć pole wyboru, jeśli nie można zaktualizować wdrożenia, wykonaj pełne wdrożenie, jeśli chcesz, aby pełne wdrożenie się odbywać automatycznie jeśli wdrażanie aktualizacji nie powiodła się. Pełne wdrożenie resetuje wirtualny adres IP (VIP) usług w chmurze. Aby uzyskać więcej informacji, zobacz [jak: zachowanie stałej wirtualny adres IP dla usługi w chmurze](https://msdn.microsoft.com/library/azure/jj614593.aspx).


1. Aby debugowania usługi, zaznacz pole wyboru **Włącz IntelliTrace** lub jeśli wdrażania konfiguracji **Debugowanie** i chcesz debugowania usługi cloud platformy Azure, zaznacz pole wyboru **Włącz zdalnego debugowania dla wszystkich ról** wdrożenia usługi zdalnej debugowania.

2. Do profilu aplikacji, zaznacz pole wyboru **Włącz profilowanie** , a następnie wybierz polecenie łącze **Ustawienia** , aby wyświetlić opcje profilowania. 


    >[AZURE.NOTE] Aby włączyć IntelliTrace lub profilowania interakcji warstwa (nachylenie) należy użyć programu Visual Studio Ultimate, a nie można włączyć zarówno w tym samym czasie.

    Aby uzyskać więcej informacji zobacz [Debugowanie usługi w chmurze opublikowany z IntelliTrace i Visual Studio](https://msdn.microsoft.com/library/azure/ff683671.aspx) i [Testowanie wydajność usługi w chmurze](https://msdn.microsoft.com/library/azure/hh369930.aspx).

1. Wybierz pozycję **Dalej** Aby wyświetlić stronę podsumowania aplikacji.

## <a name="publishing-your-application"></a>Publikowanie aplikacji

1. Możesz utworzyć profil publikowania z wybranych ustawień. Na przykład utworzyć jeden profil dla środowisku testowym, a drugi do produkcji. Aby zapisać ten profil, wybierz ikonę **Zapisz** . Kreator utworzy profil i zapisuje go w programie project Visual Studio. Aby zmienić nazwę profilu, Otwórz listę **profil docelowy** , a następnie wybierz **< Zarządzaj >**.

    ![Podsumowanie ekranie Kreatora publikowania](./media/vs-azure-tools-publish-azure-application-wizard/IC749015.png)

    >[AZURE.NOTE] Publikowanie profilu jest wyświetlana w Eksploratorze rozwiązań w programie Visual Studio, a ustawienia profilu są zapisywane w pliku z rozszerzeniem .azurePubxml. Ustawienia zostaną zapisane jako atrybuty tagi XML.

1. Wybierz pozycję **Publikuj** publikowanie aplikacji. Można monitorować stan procesu w oknie **dane wyjściowe** w programie Visual Studio.

## <a name="see-also"></a>Zobacz też

[Jak: Migrowanie i publikowanie aplikacji sieci Web do usługi w chmurze Azure z programu Visual Studio](https://msdn.microsoft.com/library/azure/hh420322.aspx)

[Publikowanie usługi w chmurze za pomocą narzędzi Azure](https://msdn.microsoft.com/library/azure/ff683672.aspx)

[Debugowanie usługi w chmurze opublikowanych z IntelliTrace i programu Visual Studio](https://msdn.microsoft.com/library/azure/ff683671.aspx)

[Testowanie wydajność usługi w chmurze](https://msdn.microsoft.com/library/azure/hh369930.aspx)

