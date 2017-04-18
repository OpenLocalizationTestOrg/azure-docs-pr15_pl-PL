<properties
   pageTitle="Jak zarządzać konfiguracje usługi i profilów | Microsoft Azure"
   description="Dowiedz się, jak pracować z plikami konfiguracji usług konfiguracji i profile | które przechowywania ustawień środowisk wdrażania i ustawień publikowania dla usług w chmurze."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="how-to-manage-service-configurations-and-profiles"></a>Jak zarządzać konfiguracje usługi i profilów

## <a name="overview"></a>Omówienie

Podczas publikowania usługi w chmurze, Visual Studio przechowuje informacje o konfiguracji w dwóch rodzajów plików konfiguracji: usług konfiguracji i profile. Konfiguracje usługi (pliki .cscfg) zawierają ustawienia dla środowiska wdrażania usługi Azure chmury. Azure korzysta z tych plików konfiguracji podczas zarządzania usługami w chmurze. Z drugiej strony profile (pliki .azurePubxml) ze sklepu Publikuj ustawienia dla usług w chmurze. Te ustawienia są rejestr wybierz gdy za pomocą Kreatora publikowania, a następnie są używane lokalnie przez program Visual Studio. W tym temacie wyjaśniono, jak pracować z obu typów plików konfiguracji.

## <a name="service-configurations"></a>Konfiguracje usługi

Możesz utworzyć wiele konfiguracji usługi do użycia dla każdej usługi środowisk wdrażania. Na przykład może utworzyć konfigurację usługi w środowisku lokalnym, którego używasz, aby uruchomić i przetestować aplikację Azure i Konfiguracja usługi innej dla środowisku produkcyjnym.

Można dodawać, usuwanie, zmienianie nazwy i modyfikować tę konfigurację usługi zgodnie z własnymi potrzebami. Możesz zarządzać tę konfigurację usługi z programu Visual Studio, jak pokazano na poniższej ilustracji.

![Zarządzanie konfiguracje usługi](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-service-config.png)

Można również otworzyć okno dialogowe **Zarządzanie konfiguracji** ze stron właściwości roli. Aby otworzyć właściwości dla roli w projekcie Azure, otwórz menu skrótów dla danej roli, a następnie wybierz polecenie **Właściwości**. Na karcie **Ustawienia** rozwiń listę **Konfiguracji usługi** , a następnie wybierz pozycję **Zarządzaj** , aby otworzyć okno dialogowe **Zarządzanie konfiguracji** .

### <a name="to-add-a-service-configuration"></a>Aby dodać konfigurację usługi

1. W Eksploratorze rozwiązań Otwieranie menu skrótów dla Azure projektu, a następnie wybierz pozycję **Zarządzaj konfiguracji**.

    Zostanie wyświetlone okno dialogowe **Zarządzanie konfiguracje usługi** .

1. Aby dodać konfigurację usługi, musisz utworzyć kopię istniejącej konfiguracji. Aby to zrobić, wybierz pozycję konfiguracji, który chcesz skopiować z listy Nazwa, a następnie wybierz pozycję **Utwórz kopię**.

1. (Opcjonalnie) Aby nadać konfigurację usługi pod inną nazwą, wybierz nowej konfiguracji usługi z listy Nazwa, a następnie wybierz polecenie **Zmień nazwę**. W polu tekstowym **Nazwa** wpisz nazwę, której chcesz użyć dla tej konfiguracji usługi, a następnie wybierz **przycisk OK**.

    Nowy plik konfiguracji usługi o nazwie ServiceConfiguration. [Nazwa nowego] .cscfg jest dodawany do projektu Azure w Eksploratorze rozwiązań.


### <a name="to-delete-a-service-configuration"></a>Aby usunąć konfigurację usługi

1. W Eksploratorze rozwiązań Otwieranie menu skrótów dla Azure projektu, a następnie wybierz pozycję **Zarządzaj konfiguracji**.

    Zostanie wyświetlone okno dialogowe **Zarządzanie konfiguracje usługi** .

1. Aby usunąć konfigurację usługi, wybierz pozycję konfiguracji, który chcesz usunąć z listy **Nazwa** , a następnie kliknij przycisk **Usuń**. Aby sprawdzić, czy chcesz usunąć tej konfiguracji zostanie wyświetlone okno dialogowe.

1. Wybierz pozycję **Usuń**.

     Plik konfiguracyjny usługi zostanie usunięty z projektu Azure w Eksploratorze rozwiązań.


### <a name="to-rename-a-service-configuration"></a>Aby zmienić konfigurację usługi

1. W oknie Eksplorator rozwiązań Otwieranie menu skrótów dla Azure projektu, a następnie wybierz **Zarządzanie konfiguracji**.

    Zostanie wyświetlone okno dialogowe **Zarządzanie konfiguracje usługi** .

1. Aby zmienić konfigurację usługi, wybierz nową konfigurację usługi z listy **Nazwa** , a następnie wybierz polecenie **Zmień nazwę**. W polu tekstowym **Nazwa** wpisz nazwę, której chcesz użyć dla tej konfiguracji usługi, a następnie wybierz **przycisk OK**.

    Zostanie zmieniona nazwa pliku konfiguracji usługi Azure projektu w Eksploratorze rozwiązań.

### <a name="to-change-a-service-configuration"></a>Aby zmienić konfigurację usługi

- Jeśli chcesz zmienić konfigurację usługi Otwórz menu skrótów dla konkretnej roli, którą chcesz zmienić w projekcie Azure, a następnie wybierz pozycję **Właściwości**. Zobacz [jak: Konfigurowanie ról dla usługi Azure w chmurze przy użyciu programu Visual Studio](https://msdn.microsoft.com/library/azure/hh369931.aspx) uzyskać więcej informacji.

## <a name="make-different-setting-combinations-by-using-profiles"></a>Tworzenie kombinacji ustawienie inne przy użyciu profilów

Przy użyciu profilu, można automatycznie wypełnić w **Kreatorze publikowania** z różnymi kombinacjami ustawienia dla różnych celów. Na przykład można mieć jeden profil do debugowania i tworzy innej wersji. W takim przypadku profilu **Debugowanie** woli **IntelliTrace** włączony i Konfiguracja **Debugowanie** wybrana, a profilu **wersji** woli **IntelliTrace** wyłączone i Konfiguracja **wersji** wybrana. Można także użyć różnych profilów do wdrożenia usługi przy użyciu konta innego miejsca do magazynowania.

Po uruchomieniu kreatora po raz pierwszy, zostanie utworzona profilu domyślnego. Program Visual Studio przechowuje profilu w pliku, który ma rozszerzenie .azurePubXml, która jest dodawana do Azure projektu w folderze **Profile** . Jeśli po uruchomieniu kreatora później ręcznie określić poszczególnych opcjach, plik jest automatycznie aktualizuje. Przed uruchomieniem poniższą procedurę, należy już opublikowany usługi w chmurze co najmniej raz.

### <a name="to-add-a-profile"></a>Aby dodać profil

1. Otwórz menu skrótów dla Azure projektu, a następnie wybierz pozycję **Publikuj**.

1. Obok listy **docelowej profil** wybierz przycisk **Zapisz profil** , jak na poniższej ilustracji pokazano. Spowoduje to utworzenie profilu dla Ciebie.

    ![Utwórz nowy profil](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/create-new-profile.png)

1. Po utworzeniu profilu wybierz **< Zarządzaj >** na liście **docelowej profilu** .

    Zostanie wyświetlone okno dialogowe **Zarządzanie profilami** jak na poniższej ilustracji pokazano.

    ![Okno dialogowe Profile Zarządzanie](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-profiles.png)

1. Na liście **Nazwa** wybierz profil, a następnie wybierz pozycję **Utwórz kopię**.

1. Wybierz przycisk **Zamknij** .

    Nowy profil pojawi się na liście docelowej profilu.

1. Na liście **docelowej profilu** wybierz profil, która została właśnie utworzona. Ustawienia Kreatora publikowania są wypełniane opcji z wybranego profilu.

1. Wybierz przyciski **Poprzedni** i **Następny** , aby wyświetlić stronę Kreatora publikowania, a następnie Dostosuj ustawienia dla tego profilu. Więcej informacji, zobacz [Kreatora publikowania aplikacji Azure](http://go.microsoft.com/fwlink/p/?LinkID=623085) .

1. Po zakończeniu dostosowywania ustawień, wybierz przycisk **Dalej** aby wrócić do strony ustawień. Profil zostanie zapisany podczas publikowania usługi przy użyciu tych ustawień, lub w przypadku wybrania opcji **Zapisz** obok listy profilów.

### <a name="to-rename-or-delete-a-profile"></a>Zmienianie nazwy lub usuwanie profilu

1. Otwórz menu skrótów dla Azure projektu, a następnie wybierz pozycję **Publikuj**.

1. Na liście **docelowej profilu** wybierz pozycję **Zarządzaj**.

1. W oknie dialogowym **Zarządzaj profilami** wybierz profil, który chcesz usunąć, a następnie kliknij przycisk **Usuń**.

1. W oknie dialogowym potwierdzenia wybierz **przycisk OK**.

1. Wybierz pozycję **Zamknij**.

### <a name="to-change-a-profile"></a>Aby zmienić profilu

1. Otwórz menu skrótów dla Azure projektu, a następnie wybierz pozycję **Publikuj**.

1. Na liście **docelowej profilu** wybierz profil, który chcesz zmienić.

1. Wybierz przyciski **Poprzedni** i **Następny** , aby wyświetlić stronę Kreatora publikowania, a następnie zmień odpowiednie ustawienia. Więcej informacji, zobacz [Kreatora publikowania aplikacji Azure](http://go.microsoft.com/fwlink/p/?LinkID=623085) .

1. Po zakończeniu zmieniania ustawień, wybierz przycisk **Dalej** aby wrócić do strony **ustawień** .

1. (Opcjonalnie) wybierz pozycję **Publikuj** publikowanie przy użyciu nowych ustawień usługi w chmurze. Jeśli nie chcesz opublikować usługi w chmurze w tej chwili, a następnie zamknij Kreatora publikowania, Visual Studio zapytaniem, czy chcesz zapisać zmiany profilu.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać informacje o konfigurowaniu innych części Azure projektu z programu Visual Studio, zobacz [Konfigurowanie projektu Azure](http://go.microsoft.com/fwlink/p/?LinkID=623075)
