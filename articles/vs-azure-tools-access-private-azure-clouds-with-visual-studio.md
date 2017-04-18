<properties 
   pageTitle="Uzyskiwanie dostępu do prywatnych chmury Azure z programem Visual Studio | Microsoft Azure"
   description="Dowiedz się, jak uzyskać dostęp do zasobów prywatnych chmurze przy użyciu programu Visual Studio."
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

# <a name="accessing-private-azure-clouds-with-visual-studio"></a>Uzyskiwanie dostępu do prywatnych chmury Azure z programem Visual Studio

##<a name="overview"></a>Omówienie

Domyślnie program Visual Studio obsługuje punkty końcowe pozostałych publicznej chmura Azure. Może to być problem, jeśli korzystasz z programu Visual Studio z prywatnych chmura Azure. Certyfikaty służy do konfigurowania programu Visual Studio, aby uzyskać dostęp do punktów końcowych pozostałych prywatne chmura Azure. Możesz uzyskać te certyfikaty za pośrednictwem usługi Azure publikowanie plik ustawień.

## <a name="to-access-a-private-azure-cloud-in-visual-studio"></a>Aby uzyskać dostęp do prywatnych Azure chmury w programie Visual Studio

1. W programie [portal Azure klasyczny](http://go.microsoft.com/fwlink/?LinkID=213885) w chmurze prywatne Pobierz plik ustawienia publikowania lub skontaktuj się z administratorem pliku ustawienia publikowania. W publicznej wersji Azure łącza w celu pobrania to jest [https://manage.windowsazure.com/publishsettings/](https://manage.windowsazure.com/publishsettings/). (Plik do pobrania powinien mieć rozszerzenie .publishsettings).

1. W **Eksploratorze serwera** w programie Visual Studio wybierz węzeł **Azure** , a następnie w menu skrótów wybierz polecenie **Zarządzaj subskrypcjami** .

    ![Zarządzanie subskrypcjami polecenia](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790778.png)

1. W oknie dialogowym **Zarządzaj subskrypcjami programu Microsoft Azure** wybierz kartę **Certyfikaty** , a następnie wybierz przycisk **Importuj** .

    ![Importowanie certyfikatów Azure](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790779.png)

1. W oknie dialogowym **Importowanie Microsoft Azure subskrypcje** przejdź do folderu, w miejsce, w którym zostanie zapisany plik ustawień publikowania i wybierz plik, a następnie wybierz przycisk **Importuj** . Certyfikaty w pliku ustawienia publikowania to zaimportowanie do programu Visual Studio. Teraz należy możliwość interakcji z zasobami prywatne chmury.

    ![Importowanie ustawienia publikowania](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790780.png)

## <a name="next-steps"></a>Następne kroki

[Publikowanie w usłudze w chmurze Azure z programu Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx)

[Jak: pobieranie i importowanie publikowanie ustawienia i informacje o subskrypcji](https://msdn.microsoft.com/library/dn385850(v=nav.70).aspx)

