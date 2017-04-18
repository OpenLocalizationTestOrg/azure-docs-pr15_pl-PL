<properties
   pageTitle="Konfigurowanie projektu usługi Azure chmury z programu Visual Studio | Microsoft Azure"
   description="Dowiedz się, jak skonfigurować projekt usługi Azure chmury w programie Visual Studio, w zależności od potrzeb dla tego projektu."
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

# <a name="configure-an-azure-cloud-service-project-with-visual-studio"></a>Konfigurowanie projektu usługi Azure chmury z programu Visual Studio

W zależności od potrzeb dla tego projektu można skonfigurować projekt usługi Azure chmury. Można ustawić właściwości projektu dla następujących kategorii:

- **Publikowanie usługi w chmurze Azure**

  Można ustawić właściwości, aby upewnić się, że istniejące usługi w chmurze wdrożony Azure nie jest przypadkowemu usunięciu.

- **Uruchamiania lub debugowania usługi w chmurze na komputerze lokalnym**

  Możesz wybrać konfigurację usługi za pomocą i wskazuje, czy ma być uruchamiany emulatora Azure miejsca do magazynowania.

- **Sprawdź poprawność pakietu usługi cloud po jego utworzeniu**

  Użytkownik chce traktuje ostrzeżenia jako błędy, tak aby można mieć pewność, że wdrażania pakietu z chmury bez żadnych problemów. Zmniejsza czas oczekiwania, jeśli wdrażanie, a następnie wykrywania wystąpił błąd.

Na poniższej ilustracji przedstawiono sposób wybierania konfiguracji używany podczas uruchamiania lub debugowania usługi cloud lokalnie. Można ustawić właściwości projektu, które wymagają z tego okna, jak pokazano na ilustracji.

![Konfigurowanie projektu Microsoft Azure](./media/vs-azure-tools-configuring-an-azure-project/IC713462.png)

## <a name="to-configure-an-azure-cloud-service-project"></a>Aby skonfigurować projekt usługi Azure chmury

1. Aby skonfigurować projektu usługi w chmurze **Eksploratorze rozwiązań**, otwórz menu skrótów dla projektu usługi cloud, a następnie wybierz **Właściwości**.

  Strona z nazwą projektu usługi cloud pojawi się w Edytorze Visual Studio.

1. Wybierz kartę **rozwoju** .

1. Aby upewnić się, że nie przypadkowego usunięcia istniejącego wdrożenia w Azure, w wierszu przed usunięciem istniejącej listy wdrożenia, wybierz **wartość PRAWDA**.

1. Aby wybrać konfigurację usługi, który ma być używany podczas uruchamiania lub debugowania usługi cloud lokalnie, na liście **Konfiguracja usługi** wybierz konfigurację usługi.

  >[AZURE.NOTE] Jeśli chcesz utworzyć konfigurację usługi za pomocą, zobacz jak: Zarządzanie konfiguracje usługi i profilów. Jeśli chcesz zmodyfikować konfigurację usługi dla roli, zobacz [jak skonfigurować ról w usłudze w chmurze Azure z programem Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).

1. Aby rozpocząć Azure magazynowania emulatora podczas uruchamiania lub debugowania usługi cloud lokalnie w **emulatora miejsca do magazynowania rozpocząć Azure**wybierz pozycję **wartość PRAWDA**.

1. Aby upewnić się, że nie można opublikować występują błędy sprawdzania poprawności pakietu, w **Traktuj ostrzeżenia jako błędy**, wybierz **wartość PRAWDA**.

1. Aby upewnić się, że Twoja rola sieci web używa tego samego portu każdym uruchomieniu lokalnie w IIS Express w **używają portów projektu sieci web**, wybierz **wartość PRAWDA**. Do używania określonego portu dla projektu sieci web określonego, otwórz menu skrótów dla projektu sieci web, wybierz kartę **Właściwości** , wybierz kartę **sieci Web** i zmień numer portu w ustawieniu **Adres Url programu Project** w sekcji **Express usług IIS** . Na przykład wprowadź `http://localhost:14020` jako adres URL programu project.

1. Aby zapisać zmiany wprowadzone we właściwościach projektu usługi cloud, wybierz przycisk **Zapisz** na pasku narzędzi.

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat sposobu konfigurowania Azure chmury usługi projekty w programie Visual Studio, zobacz [Konfigurowanie i Azure projektu przy użyciu wielu konfiguracji usługi](vs-azure-tools-multiple-services-project-configurations.md).
