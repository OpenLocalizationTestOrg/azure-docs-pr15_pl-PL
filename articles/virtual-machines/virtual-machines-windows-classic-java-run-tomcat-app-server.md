<properties
    pageTitle="Tomcat na komputerze wirtualnych | Microsoft Azure"
    description="Ten samouczek używa zasobów utworzone za pomocą modelu Klasyczny wdrożenia i pokazano, jak utworzyć Windows wirtualne urządzenie i skonfigurować go do uruchamiania serwera aplikacji Apache Tomcat."
    services="virtual-machines-windows"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="azure-service-management" />

<tags
    ms.service="virtual-machines-windows"
    ms.workload="web"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="how-to-run-a-java-application-server-on-a-virtual-machine-created-with-the-classic-deployment-model"></a>Jak uruchomić serwer aplikacji Java na komputerze wirtualnej utworzone za pomocą modelu Klasyczny wdrażania

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Z Azure maszyny wirtualnej umożliwia podanie możliwości serwera. Na przykład uruchomionych Azure maszyny wirtualnej można skonfigurować do obsługi serwera aplikacji Java, takich jak Apache Tomcat. Po ukończeniu tego przewodnika, konieczne będzie wie, jak tworzyć maszyny wirtualnej uruchomionych Azure i skonfiguruj go, aby uruchomić serwer aplikacji Java.

Dowiesz się:

* Jak utworzyć maszyny wirtualnej, która ma Java Development Kit (JDK) już zainstalowany.
* Jak zdalnie logować się do komputera wirtualnych.
* Jak zainstalować na komputerze wirtualnych serwer aplikacji Java.
* Jak utworzyć punkt końcowy dla komputera wirtualnych.
* Jak otworzyć port w zaporze dla serwera aplikacji.

Na potrzeby tego samouczka serwer aplikacji Apache Tomcat zostanie zainstalowana na komputerze wirtualnych. Ukończony instalacji spowoduje instalacji Tomcat, takie jak następujące czynności.

![Uruchamianie Apache Tomcat maszyn wirtualnych][virtual_machine_tomcat]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="to-create-a-virtual-machine"></a>Aby utworzyć maszyny wirtualnej

1. Zaloguj się do [portalu klasyczny Azure](https://manage.windowsazure.com).
2. Kliknij przycisk **Nowy**, kliknij pozycję **obliczenia**, kliknij pozycję **maszyn wirtualnych**, a następnie kliknij **Z galerii**.
3. W oknie dialogowym **Wybieranie obrazu maszyn wirtualnych** wybierz **JDK 7 systemu Windows Server 2012**.
Należy zauważyć, że **JDK 6 systemu Windows Server 2012** jest dostępna, jeśli masz starsze aplikacje, które nie są gotowe do uruchomienia w JDK 7.
4. Kliknij przycisk **Dalej**.
5. W oknie dialogowym **Konfiguracja maszyn wirtualnych** :
    1. Określ nazwę dla maszyny wirtualnej.
    2. Określ wielkość służących do maszyny wirtualnej.
    3. Wprowadź nazwę dla administratora w polu **Nazwa użytkownika** . Zapamiętaj ten nazwę i hasło, które zostanie wyświetlony obok, którego użyjesz ich zdalnie po zalogowaniu się po maszyn wirtualnych.
    4. Wprowadź hasło w polu **nowe hasło** , a następnie wprowadź je ponownie w polu **Potwierdź** . To hasło konta administratora.
    5. Kliknij przycisk **Dalej**.
6. W następnym oknie dialogowym **Konfiguracja maszyn wirtualnych** :
    1. **Usługa w chmurze**należy użyć domyślnego **Tworzenie nowej usługi w chmurze**.
    2. Wartość w polu **Nazwa DNS usługi w chmurze** musi być unikatowa we cloudapp.net. W razie potrzeby zmodyfikuj tej wartości, tak że Azure wskazuje, że jest on unikatowy.
    2. Określ region, grupa koligacji lub wirtualnej sieci. Na potrzeby tego samouczka Określ region, takich jak **Zachodnia USA**.
    2. **Konto miejsca do magazynowania**zaznacz pole wyboru **Użyj konta automatycznie wygenerowanego miejsca do magazynowania**.
    3. **Ustawianie dostępności**wybierz pozycję **(Brak)**.
    4. Kliknij przycisk **Dalej**.
7. W ostatecznym **konfigurację maszyny wirtualnej** , okno dialogowe:
    1. Zaakceptuj domyślne wpisy punktu końcowego.
    2. Kliknij pozycję **pełne**.

## <a name="to-remotely-sign-in-to-your-virtual-machine"></a>Zdalne zalogować się do komputera wirtualnych

1. Zaloguj się do [portalu klasyczny Azure](https://manage.windowsazure.com).
2. Kliknij pozycję **maszyn wirtualnych**.
3. Kliknij nazwę maszyny wirtualnej, który ma się zalogować do.
4. Po rozpoczęciu maszyny wirtualnej menu podręcznego w dolnej części strony umożliwia połączeń.
5. Kliknij przycisk **Połącz**.
6. Odpowiedz na monity, aby połączyć się z komputerem wirtualnych. To powinny być zapisywania i otwierania pliku RDP, który zawiera informacje dotyczące połączenia. Może być konieczne skopiuj adres url: port jako ostatnia część pierwszy wiersz pliku .rdp i wkleić je do zdalna aplikacja logowania.

## <a name="to-install-a-java-application-server-on-your-virtual-machine"></a>Aby zainstalować na komputerze wirtualnych serwer aplikacji Java

Serwer aplikacji Java można kopiować do komputera wirtualnych lub możesz zainstalować serwer aplikacji Java za pośrednictwem Instalatora.

Na potrzeby tego samouczka Tomcat zostanie zainstalowana.

1. Jeśli zalogowano się do komputera wirtualnych, otwórz sesję przeglądarki [Apache Tomcat](http://tomcat.apache.org/download-70.cgi).
2. Kliknij dwukrotnie łącze do **Instalatora usługi Windows 32-bitowej i 64-bitowa**. Za pomocą tej techniki, Tomcat zostanie zainstalowany jako usługa Windows.
3. Po wyświetleniu monitu o uruchomienie Instalatora.
4. W kreatorze **Konfiguracji Tomcat Apache** postępuj zgodnie z instrukcjami, aby zainstalować Tomcat. Na potrzeby tego samouczka akceptować wartości domyślne jest prawidłowy. Po wyświetleniu okna dialogowego **Kończenie pracy Kreatora konfiguracji Tomcat Apache** Opcjonalnie można sprawdzić, **Uruchom Tomcat Apache** mają Tomcat Rozpocznij teraz. Kliknij przycisk **Zakończ** , aby ukończyć proces konfiguracji Tomcat.

## <a name="to-start-tomcat"></a>Aby rozpocząć Tomcat
Jeśli nie wybrano do uruchamiania w oknie dialogowym **Kończenie pracy Kreatora konfiguracji Tomcat Apache** Tomcat uruchomić ją, otwierając wiersz polecenia na komputerze wirtualnych i uruchomiony **netto Rozpoczynanie Tomcat7**.

Powinien zostać wyświetlony Tomcat systemem Uruchom maszyny wirtualnej przeglądarkę i Otwórz <8080>.

Aby wyświetlić Tomcat działa z komputerów zewnętrznych, musisz utworzyć punktu końcowego i otworzyć port.

## <a name="to-create-an-endpoint-for-your-virtual-machine"></a>Aby utworzyć punkt końcowy dla komputera wirtualnych
1. Zaloguj się do [portalu klasyczny Azure](https://manage.windowsazure.com).
2. Kliknij pozycję **maszyn wirtualnych**.
3. Kliknij nazwę maszyny wirtualnej, która jest uruchomiony serwer aplikacji Java.
4. Kliknij pozycję **punkty końcowe**.
5. Kliknij przycisk **Dodaj**.
6. W oknie dialogowym **Dodaj punkt końcowy** upewnij się, **Dodaj autonomicznego końcowy** jest zaznaczone, a następnie kliknij **Dalej**.
7. W oknie dialogowym **nowe szczegóły punktu końcowego** :
    1. Określ nazwę dla punktu końcowego; na przykład **HttpIn**.
    2. Określ **port TCP** dla protokołu.
    3. Określ **80** publicznej portu.
    4. Określ **8080** portu prywatne.
    5. Kliknij przycisk **Zakończ** , aby zamknąć okno dialogowe. Teraz jest tworzona punkt końcowy.

## <a name="to-open-a-port-in-the-firewall-for-your-virtual-machine"></a>Aby otworzyć port w zaporze dla komputera wirtualnych
1. Zaloguj się do komputera wirtualnych.
2. Kliknij przycisk **Start systemu Windows**.
3. Kliknij pozycję **Panel sterowania**.
4. Kliknij pozycję **System i zabezpieczenia**, kliknij **Zapora systemu Windows**, a następnie kliknij pozycję **Ustawienia zaawansowane**.
5. Kliknij przycisk **Reguły przychodzące**, a następnie kliknij przycisk **Nowa reguła**.
 ![Nowa reguła przychodzących][NewIBRule]
6. **Typ reguły**zaznacz **portu**, a następnie kliknij przycisk **Dalej**.
 ![Nowy port reguły przychodzące][NewRulePort]
7. Na ekranie **Protokół i porty** wybierz **TCP**, określ **8080** jako **określonych port lokalny**, a następnie kliknij **Dalej**.
 ![Nowa reguła przychodzących][NewRuleProtocol]
8. Na ekranie **akcji** wybierz pozycję **Zezwalaj na połączenie**, a następnie kliknij przycisk **Dalej**.
 ![Nowej akcji reguły przychodzące][NewRuleAction]
9. Na ekranie **profilu** upewnij się, że zaznaczono **domeny**, **prywatne**i **publiczne** , a następnie kliknij **Dalej**.
 ![Nowy profil reguły przychodzące][NewRuleProfile]
10. Na ekranie **Nazwa** Określ nazwę dla reguły, takich jak **HttpIn** (Nazwa reguły aby pasował do nazwy punktu końcowego, jednak nie wymaga) i kliknij przycisk **Zakończ**.  
 ![Nazwa nowej reguły przychodzące][NewRuleName]

W tym momencie witryny sieci Web Tomcat powinny być widoczne z zewnętrznych przeglądarki przy użyciu adresu URL formularza * *http://*usługi\_DNS\_nazwa*. cloudapp.net**gdzie ** *z\_DNS\_nazwa*** jest nazwą DNS określonej podczas tworzenia maszyny wirtualnej.

## <a name="application-lifecycle-considerations"></a>Uwagi dotyczące cyklu życia aplikacji
* Można tworzyć własne archiwum aplikacji sieci web (też) i dodaj go do folderu **Używanie** . Na przykład tworzenie podstawowe projektu web dynamiczne strony usługi Java (JSP) i je wyeksportować jako plik też, skopiuj też do folderu **Używanie** Apache Tomcat na maszyny wirtualnej, a następnie uruchom go w przeglądarce.
* Domyślnie po zainstalowaniu usługi Tomcat jest ustawiony na ręczne uruchamianie przepływu pracy. Możesz przełączyć ją do uruchamiania automatycznego, przy użyciu przystawki usługi. Kliknięcie przycisku **Start systemu Windows**, **Narzędzia administracyjne**, a następnie **usługi**, aby uruchomić przystawki usługi. Kliknij dwukrotnie usługę **Apache Tomcat** i ustaw **Typ uruchamiania** na **Automatyczne**.

    ![Ustawienia usługi, aby była uruchamiana automatycznie][service_automatic_startup]

    O konieczności rozpoczęcia Tomcat automatycznie jest, aby go uruchomi, jeśli maszyny wirtualnej uruchomieniu (na przykład po zainstalowaniu aktualizacji oprogramowania, które wymaga ponownego uruchomienia).

## <a name="next-steps"></a>Następne kroki
Informacje na temat innych usług (takich jak magazyn Azure bus usługi i baza danych SQL), które warto uwzględnić w aplikacjach Java, wyświetlając dostępnych informacji w [Centrum deweloperów języka Java](https://azure.microsoft.com/develop/java/).

[virtual_machine_tomcat]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/WA_VirtualMachineRunningApacheTomcat.png

[service_automatic_startup]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/WA_TomcatServiceAutomaticStart.png









[NewIBRule]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewInboundRule.png
[NewRulePort]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRulePort.png
[NewRuleProtocol]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleProtocol.png
[NewRuleAction]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleAction.png
[NewRuleName]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleName.png
[NewRuleProfile]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleProfile.png
