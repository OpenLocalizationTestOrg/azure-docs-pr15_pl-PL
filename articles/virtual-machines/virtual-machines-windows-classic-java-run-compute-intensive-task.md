<properties
    pageTitle="Aplikacja języka Java obliczeniowych na maszyny | Microsoft Azure"
    description="Dowiedz się, jak utworzyć Azure maszyn wirtualnych, uruchamianej aplikacji Java obliczeniowych, która może być monitorowane w innej aplikacji Java."
    services="virtual-machines-windows"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="how-to-run-a-compute-intensive-task-in-java-on-a-virtual-machine"></a>Jak uruchomić zadania obliczeniowych w Java na komputerze wirtualnych

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]
 

Z Azure maszyny wirtualnej umożliwia realizacji zadań obliczeniowych. Na przykład maszyny wirtualnej można realizacji zadań i dostarczania wyników na komputerach klienckich lub aplikacji dla urządzeń przenośnych. Po zapoznaniu się w tym artykule, konieczne będzie zrozumienie sposobu tworzenia maszyn wirtualnych o uruchamiania aplikacji Java obliczeniowych, która może być monitorowane w innej aplikacji Java.

Tego samouczka przyjęto założenie, dowiedzieć się, jak utworzyć aplikacji konsoli Java, można importować biblioteki do aplikacji Java i wygenerować archiwum Java (SŁOIK). Przyjęto żadnych informacji na temat Microsoft Azure.

Dowiesz się:

* Jak utworzyć maszyny wirtualnej z języka Java Development Kit (JDK) już zainstalowany.
* Jak zdalnie logować się do komputera wirtualnych.
* Jak utworzyć nazw bus usługi.
* Jak utworzyć aplikacji Java, która wykonuje zadanie obliczeniowych.
* Jak utworzyć aplikacji Java, która monitoruje postęp zadania obliczeniowych.
* Sposoby uruchamiania aplikacji Java.
* Jak zatrzymać aplikacje Java.

Ten samouczek użyje Problem sprzedawcy Traveling zadań obliczeniowych. Poniżej przedstawiono przykład aplikacji Java uruchomienia zadania obliczeniowych.

![Sprzedawca Traveling Problem dodatku solver][solver_output]

Poniżej przedstawiono przykład monitorowania zadania obliczeniowych aplikacji Java.

![Klienta sprzedawca Traveling Problem][client_output]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="to-create-a-virtual-machine"></a>Aby utworzyć maszyny wirtualnej

1. Zaloguj się do [portalu klasyczny Azure](https://manage.windowsazure.com).
2. Kliknij przycisk **Nowy**, kliknij pozycję **obliczenia**, kliknij pozycję **maszyn wirtualnych**, a następnie kliknij **Z galerii**.
3. W oknie dialogowym **Wybieranie obrazu maszyn wirtualnych** wybierz **JDK 7 systemu Windows Server 2012**.
Należy zauważyć, że **JDK 6 systemu Windows Server 2012** jest dostępna w przypadku, gdy masz starsze aplikacje, które nie są jeszcze gotowe do uruchomienia w JDK 7.
4. Kliknij przycisk **Dalej**.
4. W oknie dialogowym **Konfiguracja maszyn wirtualnych** :
    1. Określ nazwę dla maszyny wirtualnej.
    2. Określ wielkość służących do maszyny wirtualnej.
    3. Wprowadź nazwę dla administratora w polu **Nazwa użytkownika** . Tę nazwę i hasło, które zostanie wyświetlony obok, będzie ich używać, jeśli zdalnie zalogować się do maszyny wirtualnej.
    4. Wprowadź hasło w polu **nowe hasło** , a następnie wprowadź je ponownie w polu **Potwierdź** . To hasło konta administratora.
    5. Kliknij przycisk **Dalej**.
5. W następnym oknie dialogowym **Konfiguracja maszyn wirtualnych** :
    1. **Usługa w chmurze**należy użyć domyślnego **Tworzenie nowej usługi w chmurze**.
    2. Wartość w polu **Nazwa DNS usługi w chmurze** musi być unikatowa we cloudapp.net. W razie potrzeby zmodyfikuj tej wartości, tak że Azure wskazuje, że jest on unikatowy.
    2. Określ region, grupa koligacji lub wirtualnej sieci. Na potrzeby tego samouczka Określ region, takich jak **Zachodnia USA**.
    2. **Konto miejsca do magazynowania**zaznacz pole wyboru **Użyj konta automatycznie wygenerowanego miejsca do magazynowania**.
    3. **Ustawianie dostępności**wybierz pozycję **(Brak)**.
    4. Kliknij przycisk **Dalej**.
5. W ostatecznym **konfigurację maszyny wirtualnej** , okno dialogowe:
    1. Zaakceptuj domyślne wpisy punktu końcowego.
    2. Kliknij pozycję **pełne**.

## <a name="to-remotely-log-in-to-your-virtual-machine"></a>Zdalne zalogować się do komputera wirtualnych

1. Zaloguj się do [portalu klasyczny Azure](https://manage.windowsazure.com).
2. Kliknij pozycję **maszyn wirtualnych**.
3. Kliknij nazwę maszyny wirtualnej, którą chcesz zalogować się do.
4. Kliknij przycisk **Połącz**.
5. Odpowiedz na monity, aby połączyć się z komputerem wirtualnych. Po wyświetleniu monitu o nazwę administratora i hasło, za pomocą wartości, które są dostępne podczas tworzenia maszyny wirtualnej.

Należy zauważyć, że funkcja Bus usługi Azure wymaga certyfikat Baltimore CyberTrust Root musi być zainstalowany w ramach usługi JRE **cacerts** magazynu. Ten certyfikat automatycznie znajduje się w Java Runtime środowiska (JRE) używane przez tego samouczka. Jeśli nie masz ten certyfikat w magazynie **cacerts** JRE, zobacz [Dodawanie certyfikatu do magazynu certyfikatów urzędu certyfikacji Java] [ add_ca_cert] uzyskać informacji na temat dodawania go (a także informacji na temat wyświetlania certyfikatów w sklepie cacerts).

## <a name="how-to-create-a-service-bus-namespace"></a>Jak utworzyć nazw bus usługi

Aby rozpocząć korzystanie z usługi Bus kolejek platformy Azure, musisz najpierw utworzyć obszar nazw usługi. Obszar nazw usługi zawiera kontenerze obejmującym adresowania Bus usługi zasobów w aplikacji.

Aby utworzyć obszar nazw usługi:

1.  Zaloguj się do [portalu klasyczny Azure](https://manage.windowsazure.com).
2.  W okienku nawigacji w lewym dolnym Azure portalu klasyczny kliknij **Bus usługi, kontrola dostępu i buforowanie**.
3.  W okienku lewej górnej Azure portalu klasyczny kliknij węzeł **Bus usługi** , a następnie kliknij przycisk **Nowy** .  
    ![Zrzut ekranu węzła Bus usługi][svc_bus_node]
4.  W oknie dialogowym **Tworzenie nowego Namespace usługi** wprowadź **Namespace**, a następnie, aby upewnić się, że będzie unikatowa, kliknij przycisk **Sprawdź dostępność** .  
    ![Tworzenie nowego Namespace zrzut ekranu][create_namespace]
5.  Po upewnić się, że nazwa obszaru nazw jest dostępna, wybierz kraj lub region, w którym powinna być obsługiwana obszaru nazw, a następnie kliknij przycisk **Utwórz Namespace** .  

    Przestrzeń nazw utworzony pojawią się w portalu klasyczny Azure i chce aktywować. Poczekaj, aż stanu jest **aktywna** , przed kontynuowaniem następnego kroku.

## <a name="obtain-the-default-management-credentials-for-the-namespace"></a>Uzyskać poświadczenia zarządzania domyślna przestrzeń nazw

W celu wykonywania operacji zarządzania, takich jak tworzenie kolejki, na nowy obszar nazw, musisz uzyskać poświadczenia zarządzania obszaru nazw.

1.  W okienku nawigacji po lewej stronie kliknij węzeł **Bus usługi** , aby wyświetlić listę dostępnych nazw.
    ![Dostępne nazw zrzut ekranu][avail_namespaces]
2.  Wybierz obszar nazw, właśnie utworzonego na liście wyświetlane.
    ![Zrzut ekranu listy Namespace][namespace_list]
3.  W okienku **Właściwości** po prawej stronie jest wyświetlana lista właściwości nowy obszar nazw.
    ![Zrzut ekranu okienka właściwości][properties_pane]
4.  **Domyślny klucz** jest ukryty. Kliknij przycisk **Widok** , aby wyświetlić poświadczeń zabezpieczeń.
    ![Zrzut ekranu klucza domyślnego][default_key]
5.  Zanotuj **Wystawcy domyślne** i **Domyślne klucza** jak te informacje poniżej będzie użyć w celu wykonywania operacji z obszarem nazw.

## <a name="how-to-create-a-java-application-that-performs-a-compute-intensive-task"></a>Jak utworzyć aplikacji Java, która wykonuje zadanie obliczeniowych

1. Na tym komputerze rozwoju (która nie musi być maszyny wirtualnej utworzone przez Ciebie) Pobierz [Azure SDK dla języka Java](https://azure.microsoft.com/develop/java/).
2. Tworzenie aplikacji konsoli Java przy użyciu kodu przykład na końcu tej sekcji. W tym samouczku użyjemy **TSPSolver.java** jako nazwę pliku Java. Modyfikowanie **usługi\_usługi\_bus\_nazw**, **z\_usługi\_bus\_właściciela**, i **usługi\_usługi\_bus\_klucza** symboli zastępczych do korzystania z usługi bus odpowiednio **nazw**, **Wystawcy domyślne** i wartości **Domyślne klucza** .
3. Po kodowania, eksportowanie aplikacji do możliwe do uruchomienia archiwum Java (SŁOIK) i pakowanie bibliotek wymagane do wygenerowane SŁOIK. W tym samouczku użyjemy **TSPSolver.jar** nazwą SŁOIK wygenerowane.

<p/>

    // TSPSolver.java

    import com.microsoft.windowsazure.services.core.Configuration;
    import com.microsoft.windowsazure.services.core.ServiceException;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import java.io.*;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import java.util.ArrayList;
    import java.util.Date;
    import java.util.List;

    public class TSPSolver {

        //  Value specifying how often to provide an update to the console.
        private static long loopCheck = 100000000;  

        private static long nTimes = 0, nLoops=0;

        private static double[][] distances;
        private static String[] cityNames;
        private static int[] bestOrder;
        private static double minDistance;
        private static ServiceBusContract service;

        private static void buildDistances(String fileLocation, int numCities) throws Exception{
            try{
                BufferedReader file = new BufferedReader(new InputStreamReader(new DataInputStream(new FileInputStream(new File(fileLocation)))));
                double[][] cityLocs = new double[numCities][2];
                for (int i = 0; i<numCities; i++){
                    String[] line = file.readLine().split(", ");
                    cityNames[i] = line[0];
                    cityLocs[i][0] = Double.parseDouble(line[1]);
                    cityLocs[i][1] = Double.parseDouble(line[2]);
                }
                for (int i = 0; i<numCities; i++){
                    for (int j = i; j<numCities; j++){
                        distances[i][j] = Math.hypot(Math.abs(cityLocs[i][0] - cityLocs[j][0]), Math.abs(cityLocs[i][1] - cityLocs[j][1]));
                        distances[j][i] = distances[i][j];
                    }
                }
            } catch (Exception e){
                throw e;
            }
        }

        private static void permutation(List<Integer> startCities, double distSoFar, List<Integer> restCities) throws Exception {

            try
            {
                nTimes++;
                if (nTimes == loopCheck)
                {
                    nLoops++;
                    nTimes = 0;
                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.print("Current time is " + dateFormat.format(date) + ". ");
                    System.out.println(  "Completed " + nLoops + " iterations of size of " + loopCheck + ".");
                }

                if ((restCities.size() == 1) && ((minDistance == -1) || (distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-1)] < minDistance))){
                    startCities.add(restCities.get(0));
                    newBestDistance(startCities, distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-2)]);
                    startCities.remove(startCities.size()-1);
                }
                else{
                    for (int i=0; i<restCities.size(); i++){
                        startCities.add(restCities.get(0));
                        restCities.remove(0);
                        permutation(startCities, distSoFar + distances[startCities.get(startCities.size()-1)][startCities.get(startCities.size()-2)],restCities);
                        restCities.add(startCities.get(startCities.size()-1));
                        startCities.remove(startCities.size()-1);
                    }
                }
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        private static void newBestDistance(List<Integer> cities, double distance) throws ServiceException, Exception {
            try
            {
                minDistance = distance;
                String cityList = "Shortest distance is "+minDistance+", with route: ";
                for (int i = 0; i<bestOrder.length; i++){
                    bestOrder[i] = cities.get(i);
                    cityList += cityNames[bestOrder[i]];
                    if (i != bestOrder.length -1)
                        cityList += ", ";
                }
                System.out.println(cityList);
                service.sendQueueMessage("TSPQueue", new BrokeredMessage(cityList));
            }
            catch (ServiceException se)
            {
                throw se;
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        public static void main(String args[]){

            try {

                Configuration config = ServiceBusConfiguration.configureWithWrapAuthentication(
                        "your_service_bus_namespace", "your_service_bus_owner",
                        "your_service_bus_key",
                        ".servicebus.windows.net",
                        "-sb.accesscontrol.windows.net/WRAPv0.9");

                service = ServiceBusService.create(config);

                int numCities = 10;  // Use as the default, if no value is specified at command line.
                if (args.length != 0)
                {
                    if (args[0].toLowerCase().compareTo("createqueue")==0)
                    {
                        // No processing to occur other than creating the queue.
                        QueueInfo queueInfo = new QueueInfo("TSPQueue");

                        service.createQueue(queueInfo);

                        System.out.println("Queue named TSPQueue was created.");

                        System.exit(0);
                    }

                    if (args[0].toLowerCase().compareTo("deletequeue")==0)
                    {
                        // No processing to occur other than deleting the queue.
                        service.deleteQueue("TSPQueue");

                        System.out.println("Queue named TSPQueue was deleted.");

                        System.exit(0);
                    }

                    // Neither creating or deleting a queue.
                    // Assume the value passed in is the number of cities to solve.
                    numCities = Integer.valueOf(args[0]);  
                }

                System.out.println("Running for " + numCities + " cities.");

                List<Integer> startCities = new ArrayList<Integer>();
                List<Integer> restCities = new ArrayList<Integer>();
                startCities.add(0);
                for(int i = 1; i<numCities; i++)
                    restCities.add(i);
                distances = new double[numCities][numCities];
                cityNames = new String[numCities];
                buildDistances("c:\\TSP\\cities.txt", numCities);
                minDistance = -1;
                bestOrder = new int[numCities];
                permutation(startCities, 0, restCities);
                System.out.println("Final solution found!");
                service.sendQueueMessage("TSPQueue", new BrokeredMessage("Complete"));
            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }
        }

    }



## <a name="how-to-create-a-java-application-that-monitors-the-progress-of-the-compute-intensive-task"></a>Jak utworzyć aplikacji Java, która monitoruje postęp zadania obliczeniowych

1. Na komputerze rozwoju utworzyć aplikację konsoli Java przy użyciu kodu przykładowego na końcu tej sekcji. W tym samouczku użyjemy **TSPClient.java** jako nazwę pliku Java. Jak pokazano powyżej, modyfikowanie **usługi\_usługi\_bus\_nazw**, **z\_usługi\_bus\_właściciela**, i **usługi\_usługi\_bus\_klucza** symboli zastępczych do korzystania z usługi bus odpowiednio **nazw**, **Wystawcy domyślne** i wartości **Domyślne klucza** .
2. Eksportowanie aplikacji do możliwe do uruchomienia SŁOIK i pakowanie bibliotek wymagane do wygenerowane SŁOIK. W tym samouczku użyjemy **TSPClient.jar** nazwą SŁOIK wygenerowane.

<p/>

    // TSPClient.java

    import java.util.Date;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import com.microsoft.windowsazure.services.core.*;

    public class TSPClient
    {

        public static void main(String[] args)
        {
                try
                {

                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.println("Starting at " + dateFormat.format(date) + ".");

                    String namespace = "your_service_bus_namespace";
                    String issuer = "your_service_bus_owner";
                    String key = "your_service_bus_key";

                    Configuration config;
                    config = ServiceBusConfiguration.configureWithWrapAuthentication(
                            namespace, issuer, key,
                            ".servicebus.windows.net",
                            "-sb.accesscontrol.windows.net/WRAPv0.9");

                    ServiceBusContract service = ServiceBusService.create(config);

                    BrokeredMessage message;

                    int waitMinutes = 3;  // Use as the default, if no value is specified at command line.
                    if (args.length != 0)
                    {
                        waitMinutes = Integer.valueOf(args[0]);  
                    }

                    String waitString;

                    waitString = (waitMinutes == 1) ? "minute." : waitMinutes + " minutes.";

                    // This queue must have previously been created.
                    service.getQueue("TSPQueue");

                    int numRead;

                    String s = null;

                    while (true)
                    {

                        ReceiveQueueMessageResult resultQM = service.receiveQueueMessage("TSPQueue");
                        message = resultQM.getValue();

                        if (null != message && null != message.getMessageId())
                        {

                            // Display the queue message.
                            byte[] b = new byte[200];

                            System.out.print("From queue: ");

                            s = null;
                            numRead = message.getBody().read(b);
                            while (-1 != numRead)
                            {
                                s = new String(b);
                                s = s.trim();
                                System.out.print(s);
                                numRead = message.getBody().read(b);
                            }
                            System.out.println();
                            if (s.compareTo("Complete") == 0)
                            {
                                // No more processing to occur.
                                date = new Date();
                                System.out.println("Finished at " + dateFormat.format(date) + ".");
                                break;
                            }
                        }
                        else
                        {
                            // The queue is empty.
                            System.out.println("Queue is empty. Sleeping for another " + waitString);
                            Thread.sleep(60000 * waitMinutes);
                        }
                    }

            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }

        }

    }

## <a name="how-to-run-the-java-applications"></a>Uruchamianie aplikacji Java
Uruchom aplikację obliczeniowych, najpierw w celu utworzenia kolejki, następnie rozwiązać Saleseman Problem podróże, który spowoduje dodanie bieżącego najlepszej trasy do kolejki bus usługi. Gdy aplikacja obliczeniowych jest uruchomione (lub później), uruchomić klienta, aby wyświetlić wyniki z kolejki bus usługi.

### <a name="to-run-the-compute-intensive-application"></a>Aby uruchomić aplikację obliczeniowych

1. Zaloguj się do komputera wirtualnych.
2. Utwórz folder, w którym spowoduje uruchomienie aplikacji. Na przykład **c:\TSP**.
3. Kopiowanie **TSPSolver.jar** do **c:\TSP**
4. Tworzenie pliku o nazwie **c:\TSP\cities.txt** z następujące zagadnienia.

        City_1, 1002.81, -1841.35
        City_2, -953.55, -229.6
        City_3, -1363.11, -1027.72
        City_4, -1884.47, -1616.16
        City_5, 1603.08, -1030.03
        City_6, -1555.58, 218.58
        City_7, 578.8, -12.87
        City_8, 1350.76, 77.79
        City_9, 293.36, -1820.01
        City_10, 1883.14, 1637.28
        City_11, -1271.41, -1670.5
        City_12, 1475.99, 225.35
        City_13, 1250.78, 379.98
        City_14, 1305.77, 569.75
        City_15, 230.77, 231.58
        City_16, -822.63, -544.68
        City_17, -817.54, -81.92
        City_18, 303.99, -1823.43
        City_19, 239.95, 1007.91
        City_20, -1302.92, 150.39
        City_21, -116.11, 1933.01
        City_22, 382.64, 835.09
        City_23, -580.28, 1040.04
        City_24, 205.55, -264.23
        City_25, -238.81, -576.48
        City_26, -1722.9, -909.65
        City_27, 445.22, 1427.28
        City_28, 513.17, 1828.72
        City_29, 1750.68, -1668.1
        City_30, 1705.09, -309.35
        City_31, -167.34, 1003.76
        City_32, -1162.85, -1674.33
        City_33, 1490.32, 821.04
        City_34, 1208.32, 1523.3
        City_35, 18.04, 1857.11
        City_36, 1852.46, 1647.75
        City_37, -167.44, -336.39
        City_38, 115.4, 0.2
        City_39, -66.96, 917.73
        City_40, 915.96, 474.1
        City_41, 140.03, 725.22
        City_42, -1582.68, 1608.88
        City_43, -567.51, 1253.83
        City_44, 1956.36, 830.92
        City_45, -233.38, 909.93
        City_46, -1750.45, 1940.76
        City_47, 405.81, 421.84
        City_48, 363.68, 768.21
        City_49, -120.3, -463.13
        City_50, 588.51, 679.33

5. W wierszu polecenia Zmień katalog na c:\TSP.
6. Upewnij się, że folder Kosz JRE znajduje się w zmiennej środowiska PATH.
7. Musisz utworzyć kolejkę bus usługi przed uruchomieniem TSP permutacji dodatku solver. Uruchom następujące polecenie, aby utworzyć kolejkę bus usługi.

        java -jar TSPSolver.jar createqueue

8. Teraz, gdy jest tworzona kolejki, może zostać uruchomiony TSP permutacji dodatku solver. Na przykład uruchom następujące polecenie, aby uruchomić dodatku solver 8 miast.

        java -jar TSPSolver.jar 8

 Jeśli nie określisz numeru, spowoduje uruchomienie 10 miast. Jak znalezieniem bieżącego przekierowuje najkrótszy go spowoduje dodanie ich do kolejki.

> [AZURE.NOTE]
> Im większa liczba, jaką możesz określić już solver będzie działać. Na przykład uruchomiony 14 miasta może potrwać kilka minut i uruchamiane przez 15 miasta może zająć kilka godzin. Zwiększanie do więcej niż 16 miast może spowodować dni runtime (po pewnym czasie tygodnie, miesiące i lata). Jest to spowodowane szybkiego wzrostu liczbę permutacji obliczane przez dodatek solver jako liczba podwyżki miast.

### <a name="how-to-run-the-monitoring-client-application"></a>Jak uruchomić monitorowania aplikacji klienckiej
1. Zaloguj się do komputera, na którym spowoduje uruchomienie aplikacji klienckiej. Nie musi być tym samym komputerze, na którym jest uruchomiona aplikacja **TSPSolver** , chociaż może być.
2. Utwórz folder, w którym spowoduje uruchomienie aplikacji. Na przykład **c:\TSP**.
3. Kopiowanie **TSPClient.jar** do **c:\TSP**
4. Upewnij się, że JRE Kosza folder znajduje się w zmiennej środowiska PATH.
5. W wierszu polecenia Zmień katalog na c:\TSP.
6. Uruchom następujące polecenie.

        java -jar TSPClient.jar

    Opcjonalnie określ liczbę minut wstrzymania między sprawdzanie kolejki przez przekazanie argumentu wiersza polecenia. Domyślny okres wstrzymania sprawdzania kolejki to 3 minuty, którym jest używany, jeśli nie podano argumentu wiersza polecenia są przekazywane do **TSPClient**. Jeśli chcesz użyć inną wartość dla interwału wstrzymania, na przykład minutę, uruchom następujące polecenie.

        java -jar TSPClient.jar 1

    Klient będzie działać, dopóki nie widzi ją wiadomości kolejki "Wykonano". Należy zauważyć, że po uruchomieniu wielu wystąpień solver bez uruchamiania klienta, może być konieczne uruchomić klienta kilka razy, aby całkowicie opróżnić kolejki. Możesz również usunąć kolejkę i ponownie go utworzyć. Aby usunąć kolejkę, uruchom następujące polecenie **TSPSolver** (nie **TSPClient**).

        java -jar TSPSolver.jar deletequeue

    Solver będą uruchamiane przed zakończeniem, badania wszystkie trasy.

## <a name="how-to-stop-the-java-applications"></a>Jak zatrzymać aplikacje Java
Zarówno dla dodatku solver i aplikacje klienckie możesz nacisnąć **Klawisze Ctrl + C** , aby zakończyć, jeśli chcesz zakończyć przed ukończeniem normalny.


[solver_output]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/WA_JavaTSPSolver.png
[client_output]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/WA_JavaTSPClient.png
[svc_bus_node]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_02_SvcBusNode.jpg
[create_namespace]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_03_CreateNewSvcNamespace.jpg
[avail_namespaces]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_04_SvcBusNode_AvailNamespaces.jpg
[namespace_list]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_05_NamespaceList.jpg
[properties_pane]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_06_PropertiesPane.jpg
[default_key]: ./media/virtual-machines-windows-classic-java-run-compute-intensive-task/SvcBusQueues_07_DefaultKey.jpg
[add_ca_cert]: ../java-add-certificate-ca-store.md
