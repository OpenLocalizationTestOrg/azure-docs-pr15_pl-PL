<properties
 pageTitle="Rozwiązywanie problemów wdrożenia usługi cloud | Microsoft Azure"
 description="Istnieje kilka typowych problemów, które może pojawić się podczas wdrażania usługi w chmurze Azure. Ten artykuł zawiera niektóre z nich rozwiązań."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="09/02/2016"
   ms.author="v-six" />

# <a name="troubleshoot-cloud-service-deployment-problems"></a>Rozwiązywanie problemów wdrożenia usługi chmury

Po wdrożeniu pakiet aplikacji usług w chmurze Azure spowoduje uzyskanie informacji dotyczących wdrażania z poziomu okienka **Właściwości** w portalu Azure. W tym okienku umożliwiają szczegółowe informacje ułatwiające rozwiązywanie problemów z usług w chmurze i Podaj te informacje do obsługi Azure podczas otwierania nowe żądanie pomocy technicznej.

W okienku **Właściwości** można znaleźć w następujący sposób:

* W portalu usługi Azure kliknij wdrażanie usługi w chmurze, kliknij polecenie **wszystkie ustawienia**, a następnie kliknij polecenie **Właściwości**.
* W portalu klasyczny Azure kliknij wdrażanie usługi w chmurze kliknij pozycję **Pulpit NAWIGACYJNY**znajdujący się w prawym dolnym rogu strony (w obszarze **Szybkie rzut**). Należy pamiętać, że w tym okienku nie etykiet "Właściwości".

> [AZURE.NOTE] Możesz skopiować zawartość w okienku **Właściwości** do Schowka, klikając ikonę w prawym górnym rogu okienka.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="problem-i-cannot-access-my-website-but-my-deployment-is-started-and-all-role-instances-are-ready"></a>Problem: nie można uzyskać dostęp do swojej witryny sieci Web, ale Moje wdrożenia jest uruchomiona i wszystkie wystąpienia roli jest gotowy

Łącze adresu URL witryny sieci Web, które są wyświetlane w portalu nie zawiera portu. Domyślny port dla witryny sieci Web jest 80. Jeśli aplikacja jest skonfigurowany do uruchomienia na inny port, należy dodać poprawny numer portu do adresu URL podczas uzyskiwania dostępu do witryny sieci Web.

1. W portalu usługi Azure kliknij wdrażanie usługi w chmurze.
2. W okienku **Właściwości** Azure portal Sprawdź porty wystąpień roli (w obszarze **Wprowadzania punkty końcowe**).
3. Jeśli port nie jest 80, należy dodać wartość właściwego portu do adresu URL podczas uzyskiwania dostępu do aplikacji. Aby określić port inny niż domyślny, wpisz adres URL, w której następuje dwukropek (:), a po nim numer portu, bez spacji.

## <a name="problem-my-role-instances-recycled-without-me-doing-anything"></a>Problem: Moje wystąpienia roli wykonywane bez mi wykonaniem jakichkolwiek

Usługa korygujący jest tworzony automatycznie po Azure wykrywa węzły problem i w związku z tym przenosi wystąpienia roli do nowych węzłów. W takim przypadku może zostać wyświetlony z wystąpienia roli automatyczne odtwarzanie. Aby dowiedzieć się, czy usługa korygujący pojawił się:

1. W portalu usługi Azure kliknij wdrażanie usługi w chmurze.
2. W okienku **Właściwości** Azure portal zapoznaj się z informacjami i określenia, czy usługa korygujący wystąpiły w czasie obserwowanej role odtwarzanie.

Role będzie także odtworzyć około raz w miesiącu podczas systemu operacyjnego hosta i aktualizacji systemu operacyjnego gościa.  
Aby uzyskać więcej informacji zobacz blogu [Roli wystąpienia ponowne uruchomienie z powodu uaktualnienia systemu operacyjnego](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx)

## <a name="problem-i-cannot-do-a-vip-swap-and-receive-an-error"></a>Problem: nie można wykonać wymiany VIP i komunikat o błędzie

Zamienianie VIP jest niedozwolone, jeśli aktualizacja wdrożenia jest w toku. Wdrożenia aktualizacje można przeprowadzane automatycznie, gdy:

* Nowy system operacyjny gościa jest dostępny i zostanie skonfigurowane aktualizacje automatyczne.
* Usługa korygujący występuje.

Aby dowiedzieć się, jeśli automatyczna aktualizacja uniemożliwia wykonywanie wymiany VIP:

1. W portalu usługi Azure kliknij wdrażanie usługi w chmurze.
2. W okienku **Właściwości** Azure portal Spójrz na wartość **stanu**. Jeśli jest to **Gotowe**, sprawdź **ostatniej operacji** , aby zobaczyć, jeśli jedną ostatnio się stało z którego mogą uniemożliwić wymiany VIP.
3. Powtórz kroki 1 i 2 dla wdrożenia produkcji.
4. Jeśli automatyczna aktualizacja jest w toku, zaczekaj na jego zakończenie przed próbą wykonaj wymiany VIP.

## <a name="problem-a-role-instance-is-looping-between-started-initializing-busy-and-stopped"></a>Problem: W przypadku wystąpienia roli jest pętli między wprowadzenie, inicjowanie, zajęty i zatrzymania

Ten warunek może oznaczać problem z plikiem pakiet, kod lub konfiguracji aplikacji. W takim przypadku powinno być możliwe wyświetlić stan zmieniania co kilka minut i Azure portal może powiedzieć coś takiego jak **Odtwarzanie**, **zajęty**lub **Inicjowanie**. To wskazuje, że wystąpiły problemy z aplikacji, która jest uniemożliwienie wystąpienie roli uruchomiony.

Aby uzyskać więcej informacji na temat rozwiązywania problemów z tego problemu Zobacz blogu [Azure PaaS obliczyć diagnostyki danych](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx) i [typowych problemów powodujących ról do Kosza](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md).

## <a name="problem-my-application-stopped-working"></a>Problem: Moja aplikacja przestał działać

1. W portalu usługi Azure kliknij wystąpienie roli.
2. W okienku **Właściwości** Azure portal należy rozważyć następujące warunki w celu rozwiązania problemu:
   * Jeśli wystąpienie roli ostatnio został zatrzymany (możesz sprawdzić wartość argumentu **liczba przerwań**), można zaktualizować rozmieszczenia. Zaczekaj, aby zobaczyć, jeśli wystąpienie roli życiorysy działać samodzielnie.
   * Jeśli wystąpienie roli jest **zajęty**, sprawdź kodzie aplikacji, aby zobaczyć, jeśli zdarzenie [StatusCheck](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.statuscheck) jest obsługiwane. Może być konieczne, Dodaj lub rozwiązać niektóre kod, który obsługuje to zdarzenie.
   * Przeglądanie danych diagnostycznych i rozwiązywania problemów scenariusze w blogu [Azure PaaS obliczyć diagnostyki danych](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

>[AZURE.WARNING] Jeśli Kosz usługi w chmurze, możesz przywrócić właściwości wdrożenia, skuteczne wymazywania informacje dotyczące oryginalnego problemu.

## <a name="next-steps"></a>Następne kroki

Wyświetl więcej [artykułów Rozwiązywanie problemów z](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) usługami w chmurze.

Aby dowiedzieć się, jak rozwiązywać problemy roli usługi cloud przy użyciu danych diagnostyki komputera Azure PaaS, zobacz [Piotr Williamson serii](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
