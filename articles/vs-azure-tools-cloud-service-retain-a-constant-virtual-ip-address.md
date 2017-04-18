<properties
   pageTitle="Jak zachować stałej wirtualny adres IP usługi w chmurze | Microsoft Azure"
   description="Dowiedz się, jak zapewnić, że wirtualny adres IP (VIP) do usługi w chmurze Azure nie powoduje zmiany."
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

# <a name="how-to-retain-a-constant-virtual-ip-address-for-a-cloud-service"></a>Jak zachować stałej wirtualny adres IP usługi w chmurze

Gdy zaktualizujesz usługi w chmurze, który znajduje się w Azure, może być konieczne upewnij się, że nie powoduje zmiany wirtualny adres IP (VIP) usługi. Wiele usług zarządzania domeny za pomocą systemu nazw domen (DNS) dla rejestrowanie nazw domen. Tylko wtedy, gdy VIP pozostaje taki sam działania systemu DNS. Za pomocą **Kreatora publikowania** w narzędziach Azure aby upewnić się, że VIP usługi cloud nie zmienia się po zaktualizowaniu go. Aby uzyskać więcej informacji na temat używania zarządzania systemem DNS domeny dla usług w chmurze zobacz [Konfigurowanie niestandardowej nazwy domeny dla usługi Azure chmury](./cloud-services/cloud-services-custom-domain-name.md).

## <a name="publishing-a-cloud-service-without-changing-its-vip"></a>Publikowanie usługi w chmurze bez zmieniania jej VIP

Po pierwszym wdrożeniu jej Azure w konkretnym środowisku, takich jak środowisku produkcyjnym przydzielonego VIP: usługi w chmurze. VIP nie powoduje zmiany, chyba że wdrażanie możesz usunąć jawnie lub niejawnie zostanie usunięty przez proces aktualizacji wdrożenia. Aby zachować VIP, nie można usuwać z wdrożeniem, a także należy się upewnić, że Visual Studio nie powoduje usunięcia wdrożenia automatycznie. Można kontrolować zachowanie, określając ustawienia wdrażania **Kreatora publikowania**, który obsługuje kilka opcji wdrażania. Można określić wdrożeniu świeży lub wdrażanie aktualizacji, która może być przyrostowe lub jednoczesnego, i obydwa rodzaje wdrożenia aktualizacji zachować VIP. Aby uzyskać definicje różnego typu wdrażania zobacz [Kreatora publikowania aplikacji Azure](vs-azure-tools-publish-azure-application-wizard.md).  Ponadto można kontrolować, czy poprzedniego wdrażanie usługi w chmurze zostanie usunięty w przypadku wystąpienia błędu. VIP nieoczekiwanie mogą ulec zmianie, jeśli ta opcja nie skonfigurowano poprawnie.

### <a name="to-update-a-cloud-service-without-changing-its-vip"></a>Aby zaktualizować usługi w chmurze bez zmieniania jej VIP

1. Po wdrożeniu usługi w chmurze co najmniej raz, otwórz menu skrótów dla węzła Azure projektu, a następnie wybierz **Publikuj**. Zostanie wyświetlony Kreator **Publikowanie aplikacji Azure** .

1. Na liście subskrypcji wybierz ten, do którego chcesz wdrożyć, a następnie wybierz przycisk **Dalej** . Zostanie wyświetlona strona **ustawień** kreatora.

1. Na karcie **Ustawienia wspólne** Sprawdź, czy nazwa usług w chmurze do którego jest instalowany, **środowisko**, **Tworzenie konfiguracji**i **Konfiguracja usługi** są wszystkie poprawne.

1. Na karcie **Zaawansowane ustawienia** Sprawdź, czy konto miejsca do magazynowania i etykiety wdrożenia są poprawne, że nie jest zaznaczone pole wyboru **Usuń wdrożenia w przypadku awarii** i że jest zaznaczone pole wyboru Aktualizuj **wdrożenia** . Zaznaczając pole wyboru Aktualizuj **wdrożenia** , można zapewnić wdrożenia nie można usunąć i VIP usługi nie zostaną utracone po ponownym publikowania aplikacji. Wyczyszczenie **Usuwanie wdrożenia pole wyboru błąd**, pewność, że usługi VIP nie zostaną utracone w przypadku wystąpienia błędu podczas wdrażania.

1. Aby dodatkowo Określ sposób role, które mają być aktualizowane, wybierz łącze **Ustawienia** obok pola **wdrażania aktualizacji** , a następnie wybierz przyrostowe Uaktualnij albo opcji jednoczesnego aktualizacji w oknie dialogowym Ustawienia **wdrażania aktualizacji** . Jeśli wybierzesz przyrostowe aktualizacji każdego wystąpienia jest aktualizowana jeden po drugim, dzięki czemu aplikacja jest zawsze dostępna. Jeśli wybierzesz równoczesnej aktualizacji, wszystkie wystąpienia są aktualizowane w tym samym czasie. Jednoczesne aktualizowanie jest szybszy, ale usługi mogą być niedostępne podczas procesu aktualizacji.

1. Po zakończeniu ustawień wybierz przycisk **Dalej** .

1. Na stronie **Podsumowanie** kreatora sprawdź ustawienia, a następnie wybierz przycisk **Publikuj** .

  >[AZURE.WARNING] Jeśli rozmieszczenie nie powiedzie się, należy adres, dlatego nie powiodło się i ponownie wdróż niezwłocznie, aby uniknąć pozostawienia uszkodzenie usługi w chmurze.

## <a name="next-steps"></a>Następne kroki

Aby poznać publikowania Azure z programu Visual Studio, zobacz [Publikowanie Azure Kreator aplikacji](vs-azure-tools-publish-azure-application-wizard.md).
