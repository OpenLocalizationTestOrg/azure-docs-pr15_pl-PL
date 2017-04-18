<properties
    pageTitle="Włączanie dostępu zdalnego dla Azure wdrażania Zaćmienie"
    description="Dowiedz się, jak włączyć dostęp zdalny w przypadku wdrożeń Azure za pomocą narzędzi Azure dla programu Eclipse."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690951.aspx -->

# <a name="enabling-remote-access-for-azure-deployments-in-eclipse"></a>Włączanie dostępu zdalnego dla Azure wdrażania Zaćmienie

Aby ułatwić rozwiązywanie problemów z wdrożeń, zostanie włączone i połączyć się z komputerem wirtualnych hostingu wdrożenia za pomocą dostępu zdalnego. Funkcji dostępu zdalnego oparte na protokole RDP (Remote Desktop). Po opublikowaniu Azure lub Zaćmienie za pomocą systemu operacyjnego, można skonfigurować dostęp zdalny przed opublikowaniem Azure, można skonfigurować dostępu zdalnego do wdrożenia. Należy zauważyć, że konieczne będzie kliencie pulpitu zdalnego, który jest zgodny z systemu operacyjnego, aby połączyć się z wdrożeniem maszyn wirtualnych w Azure.

## <a name="how-to-enable-remote-access-before-you-deploy-to-azure"></a>Jak włączyć dostęp zdalny przed wdrożeniem Azure

> [AZURE.NOTE] Aby włączyć dostęp zdalny przed wdrożeniem aplikacji Azure, musisz być uruchomiony Zaćmienie w systemie Windows.

Poniższa ilustracja przedstawia okno dialogowe umożliwia dostęp zdalny do właściwości **Dostępu zdalnego** .

![][ic719494]

Istnieją dwa sposoby, aby wyświetlić okno dialogowe właściwości **Dostępu zdalnego** :

* Kliknij łącze **Zaawansowane** w sekcji **Dostęp zdalny do** okna dialogowego **Publikuj Azure** .
* Otwieranie okna dialogowego **Właściwości** Azure projektu.

Po utworzeniu nowego projektu Azure wdrożenia projektu nie będą mieć dostępu zdalnego domyślnie włączone. Można jednak łatwo włączyć dostęp zdalny, określając nazwę użytkownika i hasło w oknie dialogowym **Publikowanie Azure** . Hasła dostępu zdalnego jest zaszyfrowany przy użyciu certyfikaty X.509. Jeśli nie używasz Podaj swój własny certyfikat szyfrowania zależy od certyfikat z podpisem własnym dołączona do programu wtyczki Azure dla programu Eclipse. Ten certyfikat z podpisem własnym znajduje się w folderze **certyfikatu** Azure projektu, przechowywany plik certyfikatu publicznego (SampleRemoteAccessPublic.cer) oraz w pliku certyfikatu wymiany informacji osobistych (PFX) (SampleRemoteAccessPrivate.pfx). Zawiera klucz prywatny certyfikatu, a został domyślnym hasłem **hasła1**. Jednak ponieważ to hasło jest opinii publicznej, powinien być używany certyfikat domyślny tylko w przypadku nauki śledzenia, a nie do wdrożenia produkcji. Dlatego inne niż szkoleniowe celów, na umożliwia włączone sesji zdalnych w przypadku wdrożeń programu należy kliknięciu łącze **Zaawansowane** w oknie dialogowym **Publikowanie Azure** Określ swój własny certyfikat. Należy zauważyć, że musisz przekazać do usługi hostowanej w portalu zarządzania Azure wersji PFX certyfikatu, aby tego Azure można odszyfrować hasła użytkownika.

Pozostała część samouczka pokazano, jak włączyć dostęp zdalny dla projekt Azure wdrożenia, który został utworzony przy użyciu dostępu zdalnego wyłączone. Na potrzeby tego samouczka utworzymy nowego certyfikatu z podpisem własnym, a jego plik PFX będzie miał hasła wybranych przez użytkownika. Masz również możliwość korzystania certyfikat wystawiony przez urząd certyfikacji.

## <a name="how-to-enable-remote-access-after-you-have-deployed-to-azure"></a>Jak włączyć dostęp zdalny po wdrożeniu Azure

Aby włączyć dostęp zdalny po wdrożeniu Azure, wykonaj następujące czynności:

1. Zaloguj się do portalu zarządzania Azure przy użyciu konta usługi Azure
1. Na liście **Usług w chmurze**wybierz pozycję usługi wdrożonym chmury
1. Na stronie sieci web usługi cloud kliknij łącze **Konfiguruj**
1. Na koniec strony Konfiguracja kliknij łącze **zdalnego**
1. Kiedy zostanie wyświetlone okno dialogowe podręcznym:
    * Określ rolę, dla której ma być Włączanie dostępu zdalnego
    * Kliknij, aby zaznaczyć pole wyboru **Włącz pulpit zdalny**
    * Określ nazwę użytkownika i hasło, którego chcesz użyć dla dostępu zdalnego
    * Wybierz certyfikat, aby użyć
1. Kliknij **przycisk OK** 

Zostanie wyświetlony komunikat informujący, że zmiany konfiguracji jest w toku, które może potrwać kilka minut. Po zakończeniu zmian konfiguracji, wykonaj czynności opisane w sekcji **zdalnego logowania** w dalszej części tego artykułu.
    
## <a name="how-to-enable-remote-access-in-your-package"></a>Jak włączyć dostęp zdalny do pakietu

1. W okienku Eksplorator projektu Zaćmienie osoby kliknij prawym przyciskiem myszy Azure projektu, a następnie kliknij polecenie **Właściwości**.

1. W oknie dialogowym **Właściwości** w okienku po lewej stronie rozwiń węzeł **Azure** , a następnie kliknij pozycję **Dostęp zdalny**.

1. W oknie dialogowym **Dostępu zdalnego** upewnij się, że zaznaczono opcję **Włącz wszystkie role zaakceptować połączenia pulpitu zdalnego z tych poświadczeń logowania** .

1. Określ nazwę użytkownika w celu połączenia pulpitu zdalnego.

1. Określanie i Potwierdź hasło dla użytkownika. W tym oknie dialogowym wartości nazwę i hasło użytkownika będzie używany podczas tworzenia połączenia pulpitu zdalnego. (Zauważ, że to jest osobnym hasło od hasło PFX.)

1. Określ daty wygaśnięcia dla konta użytkownika.

1. Kliknij przycisk **Nowy** , aby utworzyć nowy certyfikat z podpisem własnym. (Również można wybrać certyfikat systemu obszaru roboczego lub plik za pomocą przycisków **obszaru roboczego** lub **systemu plików** , odpowiednio, ale na potrzeby tego samouczka utworzymy nowego certyfikatu).

    * W oknie dialogowym **Nowy certyfikat** Określ i Potwierdź hasło, które będą używane dla pliku PFX.

    * Zaakceptuj wartość **Nazwa (CN)**lub za pomocą niestandardowej nazwy.

    * Określ ścieżkę i nazwę, której zostanie zapisany nowy certyfikat w formularzu cer. Ten krok i następnym krokiem można użyć folderu **certyfikatu** Azure projektu, ale można go dowolnie wybierz inną lokalizację. Na potrzeby tego samouczka użyjemy **c:\mycert\mycert.cer**. (Utwórz folder **c:\mycert** przed kontynuowaniem, lub wybierz istniejący folder, w razie potrzeby).

    * Określ ścieżkę i nazwę, której zostanie zapisany nowy certyfikat i jego klucz prywatny, w formularzu pfx. Na potrzeby tego samouczka użyjemy **c:\mycert\mycert.pfx**. Okno dialogowe z **Nowego certyfikatu** powinno wyglądać podobnie do następujących (aktualizacji ścieżki folderów, jeśli nie użyto **c:\mycert**):

        ![][ic712275]

    * Kliknij **przycisk OK** , aby zamknąć okno dialogowe **Nowego certyfikatu** .

1. Okno dialogowe usługi **Dostępu zdalnego** powinna wyglądać podobnie do następującej:</p>

    ![][ic719495]

1. Kliknij **przycisk OK** , aby zamknąć okno dialogowe **Dostępu zdalnego** .
    
Ponownie aplikacji, za pomocą Konstruuj Ustaw wdrożenia do chmury.

## <a name="to-log-in-remotely"></a>Aby zalogować się zdalnie

Gdy wystąpienia roli będzie gotowa, możesz zdalnie się zalogować maszyn wirtualnych, obsługujący aplikacji.

* Jeśli używasz Zaćmienie na systemu Windows i wybrano opcję **Wdrażanie pulpitu zdalnego Start na** podczas wdrażania usługi Azure, zostaną wyświetlone z ekranu logowania usługi Podłączanie pulpitu zdalnego podczas uruchamiania rozmieszczenia. Po wyświetleniu monitu o nazwę użytkownika i hasło, wprowadź wartości, które określonego użytkownika zdalnego i będzie można się zalogować.

* Innym sposobem zalogować się zdalnie jest za pomocą <a href="http://go.microsoft.com/fwlink/?LinkID=512959">Portalu zarządzania Azure</a>:

    * W widoku **Usług w chmurze** do portalu zarządzania Azure kliknij pozycję Usługa w chmurze, kliknij przycisk **wystąpienia**, kliknij pozycję konkretnego wystąpienia, a następnie kliknij przycisk **Połącz** . Przycisk **Połącz** zostanie wyświetlony następujący na pasku poleceń:

        ![][ic659273]

    * Po kliknięciu przycisku **Połącz** wyświetli monit o otwarcie pliku RDP. Otwórz plik, a następnie postępuj zgodnie z instrukcjami. (Możesz można również zapisać ten plik na komputer lokalny, a następnie uruchom go, klikając je dwukrotnie do zdalnego Zaloguj się do komputera wirtualnych bez konieczności najpierw przejdź do portalu zarządzania.)

    * Po wyświetleniu monitu o nazwę użytkownika i hasło, wprowadź wartości, które określonego użytkownika zdalnego i będzie można się zalogować.

> [AZURE.NOTE] Jeśli korzystasz z systemu operacyjnego Windows nie, należy użyć klienta pulpitu zdalnego, która jest zgodna z systemu operacyjnego, a następnie postępuj zgodnie z instrukcjami, aby skonfigurować tego klienta przy użyciu ustawień w pliku RDP, który został pobrany.

## <a name="see-also"></a>Zobacz też

[Azure zestaw narzędzi dla programu Eclipse][]

[Tworzenie aplikacji Witaj świecie dla Azure w Zaćmienie][]

[Instalowanie narzędzi Azure dla programu Eclipse][] 

Aby uzyskać więcej informacji na temat Azure za pomocą języka Java zobacz [Centrum deweloperów języka Java Azure][].

<!-- URL List -->

[Centrum deweloperów języka Java Azure]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure zestaw narzędzi dla programu Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Tworzenie aplikacji Witaj świecie dla Azure w Zaćmienie]: http://go.microsoft.com/fwlink/?LinkID=699533
[Instalowanie narzędzi Azure dla programu Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic712275]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic712275.png
[ic719495]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719495.png
[ic719494]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719494.png
[ic659273]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic659273.png
