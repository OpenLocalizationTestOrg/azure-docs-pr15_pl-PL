<properties
    pageTitle="Jak używać Azure podrzędnej wtyczki z integracji ciągły Hudson | Microsoft Azure"
    description="Informacje dotyczące używania Azure podrzędnej wtyczki z Hudson integracji ciągły."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor="" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robmcm"/>

# <a name="how-to-use-the-azure-slave-plug-in-with-hudson-continuous-integration"></a>Jak używać Azure podrzędnej wtyczki z Hudson ciągły integracji

Azure podrzędnej wtyczkę w przypadku Hudson umożliwia obsługi administracyjnej węzły podrzędne Azure podczas uruchamiania rozłożone tworzenia.

## <a name="install-the-azure-slave-plug-in"></a>Instalowanie wtyczki podrzędnej Azure

1. Na pulpicie nawigacyjnym Hudson kliknij pozycję **Zarządzaj Hudson**.

1. Na stronie **Zarządzanie Hudson** kliknij **Zarządzanie wtyczkami**.

1. Kliknij kartę **dostępne** .

1. Kliknij przycisk **Wyszukaj** i wpisz **Azure** ograniczenie listy do odpowiednich wtyczki.

    Jeśli wybierzesz opcję przewijania na liście dostępne dodatki, znajdziesz Azure podrzędnej wtyczki w sekcji **Zarządzanie klastrem i Distributed tworzenie** na karcie **inne osoby** .

1. Zaznacz pole wyboru dla **Azure podrzędna wtyczki**.

1. Kliknij przycisk **Zainstaluj**.

1. Uruchom ponownie Hudson.

Teraz, gdy zainstalowano wtyczkę następne kroki będzie skonfigurowanie wtyczki do profilu Azure subskrypcji i tworzenie szablonu, który będzie używany podczas tworzenia maszyn wirtualnych węzła podrzędny.

## <a name="configure-the-azure-slave-plug-in-with-your-subscription-profile"></a>Konfigurowanie podrzędnej Azure wtyczki z profilu subskrypcji

Profil subskrypcji, również określonych ustawień, publikowania jest plik XML, który zawiera bezpiecznych poświadczeń i trochę dodatkowych informacji, które będą potrzebne do pracy z Azure w środowisku rozwoju. Aby skonfigurować Azure podrzędnej wtyczki, należy następująco:

* Identyfikator subskrypcji
* Certyfikat zarządzania dla subskrypcji

Znajdują się one w swoim [profilu subskrypcji]. Poniżej przedstawiono przykładowy profilu subskrypcji.

    <?xml version="1.0" encoding="utf-8"?>

        <PublishData>

        <PublishProfile SchemaVersion="2.0" PublishMethod="AzureServiceManagementAPI">

        <Subscription

            ServiceManagementUrl="https://management.core.windows.net"

            Id="<Subscription ID>"

            Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />

        </PublishProfile>

    </PublishData>

Po utworzeniu profilu subskrypcji, wykonaj poniższe czynności, aby skonfigurować Azure podrzędnej wtyczki.

1. Na pulpicie nawigacyjnym Hudson kliknij pozycję **Zarządzaj Hudson**.

1. Kliknij przycisk **Konfiguruj System**.

1. Przewiń w dół strony, aby znaleźć sekcję **chmury** .

1. Kliknij pozycję **Dodaj nowe chmury > Microsoft Azure**.

    ![Dodawanie nowego chmury][add new cloud]

    To zostaną wyświetlone pola miejsce, w którym chcesz wprowadzić szczegóły Twojej subskrypcji.

    ![Konfigurowanie profilu][configure profile]

1. Skopiuj certyfikat zarządzanie i identyfikator subskrypcji ze swojego profilu subskrypcji i wklej je w odpowiednich polach.

    Podczas kopiowania certyfikatu identyfikator i zarządzania subskrypcji, **nie** zawiera oferty, które należy ująć wartości.

1. Kliknij przycisk **Sprawdź**konfigurację.

1. Podczas konfiguracji zostanie zweryfikowana pomyślnie, kliknij przycisk **Zapisz**.

## <a name="set-up-a-virtual-machine-template-for-the-azure-slave-plug-in"></a>Konfigurowanie maszyn wirtualnych szablonu dla podrzędnej Azure wtyczki

Szablon maszyn wirtualnych określa parametry, które wtyczki będzie używane do tworzenia węzeł podrzędny Azure. W następującej procedurze możemy zostanie utworzony szablon dla maszyn wirtualnych Ubuntu.

1. Na pulpicie nawigacyjnym Hudson kliknij pozycję **Zarządzaj Hudson**.

1. Polecenie **konfiguracji systemu**.

1. Przewiń w dół strony, aby znaleźć sekcję **chmury** .

1. W sekcji **chmury** znaleźć **Dodawanie Azure maszyn wirtualnych szablon** i kliknij przycisk **Dodaj** .

    ![Dodawanie szablonu maszyn wirtualnych][add vm template]

1. Określ nazwę usługi w chmurze, w polu **Nazwa** . Jeżeli nazwę zadanej odnosi się do istniejącej usługi w chmurze, maszyn wirtualnych będzie przygotowana w tej usłudze. W przeciwnym razie Azure utworzy nową.

1. W polu **Opis** wprowadź opis tworzonego szablonu. Te informacje są tylko do celów dokumentów i nie jest używany w inicjowania obsługi administracyjnej maszyny.

1. W polu **etykiety** wprowadź **linux**. Etykieta służy do identyfikowania szablon, którego tworzysz i następnie służy do odwołanie szablonu podczas tworzenia zadania Hudson.

1. Wybierz region, w której zostanie utworzony maszyn wirtualnych.

1. Wybierz odpowiedni rozmiar pamięci Wirtualnej.

1. Określ miejsce, w którym będą tworzone maszyn wirtualnych konto miejsca do magazynowania. Upewnij się, że znajduje się w tym samym regionie jako usługa w chmurze, który ma być używany. Jeśli chcesz nowego magazynu ma zostać utworzony w tym polu można pozostaw puste.

1. Czas przechowywania określa liczbę minut, zanim Hudson usuwa bezczynne podrzędny. Pozostaw to domyślna wartość 60.

1. W obszarze **zastosowania**zaznacz ten węzeł podrzędny zostanie użyty odpowiednich warunkach. Teraz zaznacz **ten węzeł możliwie Utilize**.

    W tym momencie formularz powinien wyglądać podobnie jak w tym przykładzie:

    ![Konfiguracja szablonu][template config]

1. **Obraz rodziny** lub identyfikator należy określić, jakie obraz systemu zostanie zainstalowana na swojej maszyn wirtualnych. Możesz wybrać z listy rodzin obraz lub określić obraz niestandardowy.

    Jeśli chcesz wybrać z listy rodzin obrazu, wprowadź pierwszy znak (z uwzględnieniem wielkości liter) nazwisko obrazu. Na przykład wpisywanie **U** powoduje wyświetlenie listy z rodziny Ubuntu Server. Po wybraniu z listy, Jenkins będzie korzystać z najnowszą wersją pakietu obrazu system z tej rodziny podczas inicjowania obsługi administracyjnej usługi maszyn wirtualnych.

    ![Listy rodziny systemów operacyjnych][OS family list]

    Jeśli masz obraz niestandardowy, którego chcesz użyć, wprowadź nazwę danego niestandardowego obrazu. Obraz niestandardowy nazwy nie są wyświetlane na liście, więc upewnić się, że nazwa jest prawidłowy.    

    W przypadku tego samouczka wpisz **P** , aby wyświetlić listę Ubuntu obrazy, a następnie wybierz **KÓW 14.04 serwera Ubuntu**.

1. Wybierz **SSH** **Uruchamianie metody**.

1. Skrypt poniżej skopiować i wkleić w polu **skrypt inicjowania** .

        # Install Java

        sudo apt-get -y update

        sudo apt-get install -y openjdk-7-jdk

        sudo apt-get -y update --fix-missing

        sudo apt-get install -y openjdk-7-jdk

        # Install git

        sudo apt-get install -y git

        #Install ant

        sudo apt-get install -y ant

        sudo apt-get -y update --fix-missing

        sudo apt-get install -y ant

    **Skrypt inicjowania** zostanie wykonana po utworzeniu maszyn wirtualnych. W tym przykładzie skrypt instaluje Java, cyfra i który.

1. W polach **Nazwa użytkownika** i **hasło** wprowadź wartości preferowanej utworzonym na swojej maszyn wirtualnych konta administratora.

1. Polecenie **Sprawdź szablonu** , aby sprawdzić, czy są prawidłowe parametry określonej.

1. Wybierz polecenie **Zapisz**.

## <a name="create-a-hudson-job-that-runs-on-a-slave-node-on-azure"></a>Tworzenie zadania Hudson uruchamianej na węzeł podrzędny Azure

W tej sekcji trzeba utworzyć zadania Hudson, która zostanie uruchomiona w węźle podrzędna Azure.

1. Na pulpicie nawigacyjnym Hudson kliknij przycisk **Nowe zadanie**.

1. Wprowadź nazwę dla zadania, które są tworzone.

1. Typ zadania wybierz **utworzyć zadanie styl bezpłatne oprogramowanie**.

1. Kliknij **przycisk OK**.

1. Na stronie Konfiguracja zadania wybierz opcję **Ogranicz miejsce, w którym można uruchamiać tego projektu**.

1. Wybierz **menu węzeł i etykiety** , a następnie wybierz pozycję **linux** (możemy etykieta podczas tworzenia określić szablon maszyn wirtualnych w poprzedniej sekcji).

1. W sekcji **Tworzenie** kliknij przycisk **Dodaj Konstruuj kroku** i wybierz pozycję **Powłoka wykonywanie**.

1. Edytuj poniższy skrypt, zamieniając **{nazwy konta github}**, **{nazwy projektu}**i **{katalogu projektu}** odpowiednie wartości, a następnie wklej skrypt edytowany w obszarze tekstu, które zostanie wyświetlone.

        # Clone from git repo

        currentDir="$PWD"

        if [ -e {your project directory} ]; then

            cd {your project directory}

            git pull origin master

        else

            git clone https://github.com/{your github account name}/{your project name}.git

        fi

        # change directory to project

        cd $currentDir/{your project directory}

        #Execute build task

        ant

1. Wybierz polecenie **Zapisz**.

1. Na pulpicie nawigacyjnym Hudson Znajdź zadanie, po prostu utworzone i kliknij ikonę **Harmonogram kompilacji** .

Hudson następnie utworzy węzeł podrzędny przy użyciu szablon utworzony w poprzedniej sekcji i wykonać skrypt, który określony w kroku kompilacji dla tego zadania.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat Azure za pomocą języka Java zobacz [Centrum deweloperów języka Java Azure].

<!-- URL List -->

[Centrum deweloperów języka Java Azure]: https://azure.microsoft.com/develop/java/
[Profil subskrypcji]: http://go.microsoft.com/fwlink/?LinkID=396395

<!-- IMG List -->

[add new cloud]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addcloud.png
[configure profile]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-configureprofile.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addnewvmtemplate.png
[template config]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-templateconfig1-withdata.png
[OS family list]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-oslist.png

