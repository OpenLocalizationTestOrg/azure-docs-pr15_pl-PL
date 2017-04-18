<properties 
   pageTitle="Za pomocą pulpitu zdalnego z rolami Azure | Microsoft Azure"
   description="Za pomocą pulpitu zdalnego z rolami Azure"
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

# <a name="using-remote-desktop-with-azure-roles"></a>Za pomocą pulpitu zdalnego z rolami Azure

Przy użyciu zestawu SDK Azure i usługami pulpitu zdalnego, masz dostęp do ról Azure i maszyn wirtualnych, które są obsługiwane przez Azure. W programie Visual Studio można skonfigurować usługami pulpitu zdalnego z Azure projektu. Aby włączyć usługi pulpitu zdalnego, należy utworzyć projekt pracy, który zawiera jedną lub więcej ról i publikowanie projektu w Azure.

>[AZURE.IMPORTANT] Masz dostęp do roli Azure do rozwiązywania problemów lub tylko rozwoju. Przeznaczenie każdej maszyny wirtualnej jest uruchomienie konkretnej roli w aplikacji Azure nie pozwalają na uruchamianie inne aplikacje klienckie. Jeśli chcesz użyć do obsługi maszyn wirtualnych o można używać do celów Azure, zobacz Uzyskiwanie dostępu do maszyn wirtualnych Azure Eksploratora serwera.

## <a name="to-enable-and-use-remote-desktop-for-an-azure-role"></a>Aby włączyć i użyć pulpitu zdalnego dla roli Azure

1. W Eksploratorze rozwiązań Otwieranie menu skrótów dla projektu, a następnie wybierz pozycję **Publikuj**.

    Zostanie wyświetlony Kreator **Publikowanie aplikacji Azure** .

    ![Publikowanie polecenie projektu w usłudze w chmurze](./media/vs-azure-tools-remote-desktop-roles/IC799161.png)

1. U dołu strony **Ustawienia publikowania usługi Microsoft Azure** kreatora wybierz pozycję **Włącz pulpit zdalny** wszystkie pola wyboru role. 

    Zostanie wyświetlone okno dialogowe **Konfiguracja usług pulpitu zdalnego** .

1. W dolnej części okna dialogowego **Konfiguracja usług pulpitu zdalnego** wybierz przycisk **Więcej opcji** . 
 
    Spowoduje to wyświetlenie lista rozwijana pozwala utworzyć lub wybrać certyfikat, tak aby można zaszyfrować informacje o poświadczeniach podczas nawiązywania połączenia za pomocą pulpitu zdalnego.

1. Na liście rozwijanej wybierz ** &lt;Tworzenie >**, lub wybierz istniejący z listy. 

    Jeśli wybierzesz istniejący certyfikat, pomiń następujące kroki.

    >[AZURE.NOTE] Certyfikaty, których potrzebujesz dla połączenia pulpitu zdalnego różnią się od certyfikatów, które są dostępne dla innych operacji Azure. Certyfikat dostępu zdalnego musi mieć klucz prywatny.

    Zostanie wyświetlone okno dialogowe **Tworzenie certyfikatu** .

    1. Podaj przyjazną nazwę dla nowego certyfikatu, a następnie wybierz przycisk **OK** . Nowy certyfikat pojawi się w polu listy rozwijanej.

    1. W oknie dialogowym **Konfiguracja usług pulpitu zdalnego** Podaj nazwę użytkownika i hasło.
    
        Nie można używać istniejącego konta. Nie można określić jako nazwę użytkownika dla nowego konta administratora.

        >[AZURE.NOTE] Jeśli hasło nie spełnia wymagań dotyczących złożoności, czerwoną ikonę pojawi się obok pola tekstowego hasła. Hasło musi zawierać wielkie litery, małe litery i liczby lub symbole.

    1. Wybierz datę, na której konto wygasa, a po którym połączenia pulpitu zdalnego zostaną zablokowane.

    1. Po podane wszystkie wymagane informacje, kliknij przycisk **OK** .
    
        Kilka ustawień, które Włączanie usług dostępu zdalnego zostaną dodane do plików .cscfg i .csdef.

1. W kreatorze **Ustawienia publikowania usługi Microsoft Azure** wybierz przycisk **OK** , gdy zechcesz opublikować usługi w chmurze.

    Jeśli nie chcesz opublikować, wybierz przycisk **Anuluj** . Ustawienia konfiguracji są zapisywane, a później można opublikować usługi w chmurze.

## <a name="connect-to-an-azure-role-by-using-remote-desktop"></a>Łączenie do roli Azure za pomocą pulpitu zdalnego

Po opublikowaniu chmury usługi Azure umożliwia Server Explorer Zaloguj się w środowisku maszyn wirtualnych systemu obsługującego Azure. 

1. W Eksploratorze Server rozwiń węzeł **Azure** , a następnie rozwiń węzeł usługi w chmurze i jedną z jego ról, aby wyświetlić listę wystąpień.

1. Otwieranie menu skrótów dla węzła wystąpienie, a następnie wybierz **Nawiązywanie połączenia za pomocą pulpitu zdalnego**.

    ![Nawiązywanie połączenia przy użyciu pulpitu zdalnego](./media/vs-azure-tools-remote-desktop-roles/IC799162.png)

1. Wprowadź nazwę użytkownika i hasło utworzone przez Ciebie wcześniej. Teraz zalogowaniu się do usługi zdalnej sesji.


