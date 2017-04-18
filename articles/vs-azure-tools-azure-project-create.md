<properties
   pageTitle="Tworzenie projektu Azure z programem Visual Studio | Microsoft Azure"
   description="Tworzenie projektu Azure za pomocą programu Visual Studio"
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

# <a name="creating-an-azure-project-with-visual-studio"></a>Tworzenie projektu Azure za pomocą programu Visual Studio

Narzędzia Azure programu Visual Studio zapewniają szablonu, który umożliwia utworzenie usługi w chmurze dla Azure. Narzędzia ułatwiają również skonfigurować, debugowania i wdrażanie usług w chmurze Azure.

Rozwiązania usługi Azure chmury zawiera następujące typy projektów:

- **Azure projektu**

    Azure projekt ma skojarzeń do projektów ról w rozwiązaniu. Zawiera również definicji usługi i pliki konfiguracji usługi. Plik definicji usługi określa ustawienia obsługi aplikacji w tym, jakie role są wymagane, punkty końcowe i rozmiar maszyn wirtualnych. Plik konfiguracyjny usługi konfiguruje ile wystąpień roli są uruchamiane i wartości ustawień zdefiniowanych dla roli. Aby uzyskać więcej informacji na temat tych ustawień, zobacz [jak: Konfigurowanie ról dla usługi Azure w chmurze przy użyciu programu Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).

- **Projekt roli sieci Web**

    Rola pracownik wykonuje przetwarzania w tle. Rola pracownika można komunikować się z usługami miejsca do magazynowania i innych usług internetowych. Rola Pracownik może mieć dowolną liczbę punktów końcowych HTTP, HTTPS lub TCP.

    - **Role w sieci Web programu ASP.NET**do tworzenia aplikacji ASP.NET z frontonu sieci web
    - **Role w sieci Web programu ASP.NET MVC5**
    - **Role w sieci Web programu ASP.NET MVC4**
    - **Role w sieci Web programu ASP.NET MVC3**
    - **Role w sieci Web usługa WCF**do tworzenia Usługa WCF
    - **Rola sieci Web aplikacji biznesowych programu Silverlight** (wymaga programu Visual Studio 2012)

- **Rola Pracownik pamięci podręcznej**

    Rola zawierającego dedykowanej pamięci podręcznej w aplikacji.

- **Rola Pracownik z usługi Bus kolejki**

    Kolejka bus usługa, która zawiera funkcje Kolejkowanie wiadomości można komunikować się z procesem roboczym. Aby uzyskać więcej informacji zobacz [jak kolejek Bus używanie usług](http://go.microsoft.com/fwlink/?LinkId=260560).

## <a name="to-create-an-azure-cloud-service-project-in-visual-studio"></a>Aby utworzyć projekt usługi Azure chmury w programie Visual Studio

1. Microsoft Visual Studio Uruchom jako administrator.

1. Na pasku menu wybierz pozycję **plik**, **Nowy** **Projekt**.

1. W okienku **Typu projektu** wybierz **chmury** z projektu Visual C# lub Visual Basic węzłów szablonu.

1. W okienku **Szablony** wybierz pozycję **Usługa w chmurze Azure**.

1. Określ, jaka wersja programu .NET Framework, którego chcesz użyć do projektowania projektu.

1. Wprowadź nazwę i lokalizację projektu i nazwę rozwiązania. Wybierz przycisk **OK** .

1. W oknie dialogowym **Nowy projekt Azure** wybierz role, które chcesz dodać, a następnie kliknij przycisk strzałki w prawo, aby dodać je do rozwiązania. Możesz dodać dowolną liczbę ról potrzebne.

1. Aby zmienić rolę, które zostały dodane do projektu, umieść wskaźnik myszy na rolach w oknie dialogowym **Nowy projekt Azure** i wybierz ikonę **zmienić** po prawej stronie roli. Można również zmienić rolę w rozwiązaniu po jego dodaniu.
