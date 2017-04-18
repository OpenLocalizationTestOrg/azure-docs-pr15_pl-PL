<properties
    pageTitle="Jak używać Azure podrzędnej wtyczki z integracji ciągły Jenkins | Microsoft Azure"
    description="Informacje dotyczące używania Azure podrzędnej wtyczki z Jenkins integracji ciągły."
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

# <a name="how-to-use-the-azure-slave-plug-in-with-jenkins-continuous-integration"></a>Jak używać Azure podrzędnej wtyczki z Jenkins ciągły integracji

Umożliwia Azure podrzędnej wtyczki dla Jenkins węzły podrzędne świadczenia Azure podczas uruchamiania rozłożone tworzenia.

## <a name="install-the-azure-slave-plug-in"></a>Instalowanie wtyczki podrzędnej Azure

1. Na pulpicie nawigacyjnym Jenkins kliknij pozycję **Zarządzaj Jenkins**.

1. Na stronie **Zarządzanie Jenkins** kliknij pozycję **Zarządzaj wtyczki**.

1. Kliknij kartę **dostępne** .

1. W polu filtr powyżej listy dostępne dodatki wpisz **Azure** ograniczenie listy do odpowiednich wtyczki.

    Jeśli wybierzesz opcję przewijania na liście dostępne dodatki, znajdziesz Azure podrzędnej wtyczki w sekcji **Zarządzanie klastrem i Distributed tworzenie** .

1. Zaznacz pole wyboru **Dodatek podrzędna Azure** .

1. Kliknij przycisk **Zainstaluj bez ponownego uruchamiania** lub **Pobierz i zainstaluj po ponownym uruchomieniu**.

Teraz, gdy zainstalowano wtyczkę następne kroki są do konfigurowania wtyczki do profilu Azure subskrypcji i tworzenie szablonu, który będzie używany podczas tworzenia maszyny wirtualnej węzła podrzędny.


## <a name="configure-the-azure-slave-plug-in-with-your-subscription-profile"></a>Konfigurowanie Azure podrzędnej wtyczki z profilu subskrypcji

Profil subskrypcji, również określonych ustawień, publikowania jest plik XML, który zawiera bezpiecznych poświadczeń i trochę dodatkowych informacji, które będą potrzebne do pracy z Azure w środowisku rozwoju. Aby skonfigurować Azure podrzędnej wtyczki, należy następująco:

* Identyfikator subskrypcji
* Certyfikat zarządzania dla subskrypcji

Znajdują się one w swoim [profilu subskrypcji]. Poniżej przedstawiono przykładowy profilu subskrypcji.

    <?xml version="1.0" encoding="utf-8"?>

        <PublishData>

        <PublishProfile SchemaVersion="2.0" PublishMethod="AzureServiceManagementAPI">

        <Subscription

            ServiceManagementUrl="https://management.core.windows.net"

            Id="<Subscription ID value>"

            Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />

        </PublishProfile>

    </PublishData>

Po umieszczeniu profilu subskrypcji, wykonaj następujące kroki konfigurowania Azure podrzędnej wtyczki:

1. Na pulpicie nawigacyjnym Jenkins kliknij pozycję **Zarządzaj Jenkins**.

1. Kliknij przycisk **Konfiguruj System**.

1. Przewiń w dół strony, aby znaleźć sekcję **chmury** .

1. Kliknij pozycję **Dodaj nowe chmury > Microsoft Azure**.

    ![sekcja chmury][cloud section]

    To zostaną wyświetlone pola miejsce, w którym chcesz wprowadzić szczegóły Twojej subskrypcji.

    ![konfigurację subskrypcji][subscription configuration]

1. Skopiuj identyfikator subskrypcji i zarządzania certyfikat wartości z subskrypcji profilu i wklej je w odpowiednich polach.

    Podczas kopiowania certyfikatu identyfikator i zarządzania subskrypcji, nie zawiera oferty, które należy ująć wartości.

1. Kliknij przycisk **Sprawdź konfigurację**.

1. Po zweryfikowaniu konfiguracji są prawidłowe, kliknij przycisk **Zapisz**.

## <a name="set-up-a-virtual-machine-template-for-the-azure-slave-plug-in"></a>Konfigurowanie maszyn wirtualnych szablonu dla Azure podrzędnej wtyczki

Szablon maszyn wirtualnych określa parametry, które wtyczki będzie używane do tworzenia węzeł podrzędny Azure. W następującej procedurze utworzymy szablonu dla maszyn wirtualnych Ubuntu.

1. Na pulpicie nawigacyjnym Jenkins kliknij pozycję **Zarządzaj Jenkins**.

1. Kliknij przycisk **Konfiguruj System**.

1. Przewiń w dół strony, aby znaleźć sekcję **chmury** .

1. W sekcji **chmury** znaleźć **Dodawanie Azure maszyn wirtualnych szablon**, a następnie kliknij **Dodaj**.

    ![Dodawanie szablonu maszyn wirtualnych][add vm template]

    To zostaną wyświetlone pola, w którym wprowadź szczegółowe informacje o tworzonego szablonu.

    ![pusty konfiguracji ogólne][blank general configuration]

1. W polu **Nazwa** wprowadź nazwę usługi Azure chmury. Jeżeli wprowadzona nazwa odwołuje się do istniejącej usługi w chmurze, maszyna wirtualna zostanie przygotowana w tej usłudze. W przeciwnym razie Azure utworzy nową.

1. W polu **Opis** wprowadź opis tworzonego szablonu. To jest tylko dla rekordów i nie jest używany w inicjowania obsługi administracyjnej maszyny wirtualnej.

1. Pole **etykiety** służy do identyfikowania szablon, którego tworzysz i następnie służy do odwołanie szablonu podczas tworzenia zadania Jenkins. W naszym celu w tym polu należy wprowadzić **linux** .

1. Na liście **Region** kliknij pozycję region, w której zostanie utworzony maszyny wirtualnej.

1. Na liście **Rozmiar maszyn wirtualnych** kliknij odpowiedni rozmiar.

1. W oknie dialogowym **Nazwę konta magazynu** określ miejsce, w którym będą tworzone maszyny wirtualnej konto miejsca do magazynowania. Upewnij się, że znajduje się w tym samym regionie jako usługa w chmurze, który ma być używany. Jeśli chcesz nowego magazynu do utworzenia puste tego pola.

1. Czas przechowywania określa liczbę minut, zanim Jenkins usuwa bezczynne podrzędny. Pozostaw to domyślna wartość 60. Można również zamknąć podrzędnej zamiast zostanie usunięta, gdy jest bezczynny. Aby to zrobić, zaznacz pole wyboru **Tylko do zamknięcia (nie usuwaj) po czas przechowywania** .

1. Na liście **Użycie** kliknij odpowiednie warunek, gdy ten węzeł podrzędny będzie używana. Teraz kliknij **ten węzeł możliwie Utilize**.

    W tym momencie formularza powinna wyglądać podobnie podobnie do następującej:

    ![punkt kontrolny ogólne szablonu konfiguracji][checkpoint general template config]

    Następnym krokiem jest zapewnienie szczegółowe informacje o obraz systemu operacyjnego, który ma swojego podrzędny ma być utworzony w.

1. W polu **rodziny obraz lub identyfikator** należy określić, jakie obraz systemu zostanie zainstalowana na tym komputerze wirtualnych. Możesz wybrać z listy rodzin obraz lub określić obraz niestandardowy.

    Jeśli chcesz wybrać z listy rodzin obrazu, wprowadź pierwszy znak (z uwzględnieniem wielkości liter) nazwisko obrazu. Na przykład wpisywanie **U** powoduje wyświetlenie listy z rodziny Ubuntu Server. Po wybraniu z listy Jenkins użyje najnowszą wersję obrazu system z tej rodziny podczas inicjowania obsługi administracyjnej komputera wirtualnych.

    ![Przykładowa lista obraz systemu operacyjnego][OS Image list sample]

    Jeśli masz obraz niestandardowy, którego chcesz użyć, wprowadź nazwę danego niestandardowego obrazu. Obraz niestandardowy nazwy nie są wyświetlane na liście, więc upewnić się, że nazwa jest prawidłowy.

    Ten samouczek wpisz **P** , aby wyświetlić listę Ubuntu obrazy, a następnie kliknij **KÓW 14.04 serwera Ubuntu**.

1. Na liście **Metoda uruchamianie** kliknij pozycję **SSH**.

1. Skopiuj poniższe skrypt i wklej je w polu **Skrypt inicjowania** .

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

    Skrypt inicjowania zostanie wykonana po utworzeniu maszyny wirtualnej. W tym przykładzie skrypt instaluje Java, cyfra i który.

1. W polach **Nazwa użytkownika** i **hasło** wprowadź wartości preferowanej utworzonym na tym komputerze wirtualnych konta administratora.

1. Kliknij przycisk **Sprawdź szablonu** , aby sprawdzić, czy są prawidłowe parametry określonej.

1. Kliknij przycisk **Zapisz**.


## <a name="create-a-jenkins-job-that-runs-on-a-slave-node-on-azure"></a>Tworzenie zadania Jenkins uruchamianej na węzeł podrzędny Azure

W tej sekcji trzeba utworzyć zadania Jenkins, która zostanie uruchomiona w węźle podrzędna Azure. Musisz mieć własnego projektu w górę na GitHub, aby wykonać opisane czynności.

1. Na pulpicie nawigacyjnym Jenkins kliknij przycisk **Nowy element**.

1. Wprowadź nazwę dla zadania, które są tworzone.

1. Typ projektu kliknij pozycję **Projekt stylu**.

1. Kliknij przycisk **Ok**.

1. Na stronie Konfiguracja zadania wybierz opcję **Ogranicz miejsce, w którym można uruchamiać tego projektu**.

1. W polu **Wyrażenia etykiety** wprowadź **linux**. W poprzedniej sekcji możemy utworzony szablon podrzędna możemy wskazanym **linux**, czyli co możemy określone w tym miejscu.

1. W sekcji **Tworzenie** kliknij przycisk **Dodaj Konstruuj kroku** i wybierz pozycję **Powłoka wykonywanie**.

1. Edytuj poniższy skrypt, zamieniając **(Twoja nazwa konta GitHub)**, **(Twoja nazwa projektu)**i **(katalogu projektu)** odpowiednie wartości, a następnie wklej skrypt edytowany w obszarze tekstu, które zostanie wyświetlone.

        # Clone from git repo

        currentDir="$PWD"

        if [ -e (your project directory) ]; then

            cd (your project directory)

            git pull origin master

        else

            git clone https://github.com/(your GitHub account name)/(your project name).git

        fi

        # change directory to project

        cd $currentDir/(your project directory)

        #Execute build task

        ant

1. Kliknij przycisk **Zapisz**.

1. Na pulpicie nawigacyjnym Jenkins umieść wskaźnik myszy na zadania, które właśnie utworzony, a następnie kliknij strzałkę listy rozwijanej, aby wyświetlić opcje zadania.

1. Kliknij przycisk **Konstruuj teraz**.

Jenkins następnie utworzy węzeł podrzędny przy użyciu szablon utworzony w poprzedniej sekcji i wykonać skrypt, który określony w kroku kompilacji dla tego zadania.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat Azure za pomocą języka Java zobacz [Centrum deweloperów języka Java Azure].

<!-- URL List -->

[Centrum deweloperów języka Java Azure]: https://azure.microsoft.com/develop/java/
[Profil subskrypcji]: http://go.microsoft.com/fwlink/?LinkID=396395

<!-- IMG List -->

[cloud section]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-cloud-section.png
[subscription configuration]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-account-configuration-fields.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-add-vm-template.png
[blank general configuration]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-slave-template-general-configuration-blank.png
[checkpoint general template config]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-slave-template-general-configuration.png
[OS Image list sample]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-os-family-list-sample.png