
<properties
    pageTitle="Uzyskiwanie dostępu do swoich aplikacji z dowolnego urządzenia | Microsoft Azure"
    description="Dowiedz się, jakie klientów są obsługiwane w przypadku Azure RemoteApp i jak uzyskać dostęp do aplikacji."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="accessing-your-apps-in-azure-remoteapp"></a>Uzyskiwanie dostępu do aplikacji w Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Jedną z beauties funkcja RemoteApp Azure jest to, że uzyskiwania dostępu do aplikacji z dowolnego urządzenia. Jeszcze lepiej możesz rozpocząć pracę nad jedno urządzenie i bezproblemowe przejście do drugiego urządzenia i skompletowanie prawo miejsce, w którym została przerwana. Aby rozpocząć należy pobrać odpowiedni klient dla danego urządzenia i zaloguj się do usługi.

W tym temacie możemy Przejrzyj obecnie obsługiwanych klientów i sposobach ich pobierania, zanim I pokazano, jak zalogować się do trybu RemoteApp z każdego z klientów.

## <a name="supported-clients"></a>Obsługiwani klienci

Można uzyskać dostęp do RemoteApp, wykonując poniższe czynności, jeśli na urządzeniu jest zainstalowany jeden z następujących systemów operacyjnych:

 - Windows 10 
 - System Windows 8.1
 - Windows 8
 - Windows 7 z dodatkiem Service Pack 1
 - Windows Phone 8.1
 - iOS
 - Mac OS X
 - Android


 Co z otwieraniem cienkie klientów? Obsługiwane są następujące cienki klient osadzonego systemu Windows:

- Osadzonymi standardowy 7
- Osadzone systemu Windows 8 standardowe
- Osadzone systemu Windows 8.1 branżowe Pro
- Przedsiębiorstwo IoT systemu Windows 10


## <a name="downloading-the-client"></a>Pobieranie klienta

Niezależnie od tego, jakie platformy używasz klienta, należy otworzyć RemoteApp można znaleźć na stronie [pobierania klienta pulpitu zdalnego](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx) .

Kliknięcie przycisku różnych łączy albo bezpośrednio rozpocznie się pobieranie klienta lub będzie wysyłać do klienta pobraniu strony w sklepie app store dla tej platformy. Instalowanie klienta pakietu, postępując zgodnie z instrukcjami na ekranie.

Po zainstalowaniu klienta na urządzeniu i uruchomić go, należy przejść do odpowiedniej sekcji poniżej, aby dowiedzieć się, jak zalogować się do trybu RemoteApp z tego klienta.

## <a name="android"></a>Android

Po zainstalowaniu aplikacji Microsoft Remote Desktop ze sklepu Google Play, możesz go znaleźć na liście aplikacji, w obszarze **Pulpit zdalny**.

1. Uruchamianie aplikacji przesuwa możesz pustego Centrum połączenia, jeśli już korzystasz aplikacji. Aby rozpocząć pracę z Azure RemoteApp, naciśnij przycisk Dodaj przycisk **"" +""** , a następnie wybierz polecenie **Azure RemoteApp**. 

     ![Opróżnij Centrum połączeń](./media/remoteapp-clients/Android1.png)

2. Musisz zalogować się przy użyciu adresu e-mail dostęp do usługi. Naciśnij pozycję **wprowadzenie**.

    ![Zaloguj się w wierszu](./media/remoteapp-clients/Android2.png)

3. Na następnej stronie wpisz swój **adres e-mail** i wybierz przycisk **Kontynuuj**. To rozpocznie proces logowania przy użyciu usługi Azure Active Directory.

    ![Pierwsza strona usługi Azure Active Directory](./media/remoteapp-clients/Android3.png)

4. Postępuj zgodnie z instrukcjami na ekranie, aby zalogować się przy użyciu swojego konta Microsoft (wcześniej nazywane "Identyfikator") lub identyfikatora organizacji. Po zalogowaniu się mogą się pojawić się stronę zaproszeń, które zostały odebrane. Jeśli jesteś, wybierz pozycję zaproszeń zaufania i naciśnij przycisk **Gotowe**. 

    ![Strona zaproszeń](./media/remoteapp-clients/Android4.png)

5. Po zaakceptowaniu zaproszeń, na liście aplikacji, których masz dostęp do będą pobierane na urządzeniu i udostępniane w Centrum połączenia. Wybierz jedną z aplikacji, aby zacząć jej używać.

    ![Centrum połączeń z kanału informacyjnego](./media/remoteapp-clients/Android5.png)

6. Jeśli nie masz jeszcze zaproszenia, nadal możesz wypróbować usługę. Aby to zrobić, naciśnij pozycję **Przejdź do bezpłatnej wersji próbnej** , po wyświetleniu monitu.

    ![Pokaz kanału informacyjnego monit](./media/remoteapp-clients/Android6.png)

7. Spowoduje to umożliwiają dostęp do podstawowy zestaw aplikacje ułatwiające rozpoczęcie pracy z RemoteApp.

    ![Pokaz kanału informacyjnego dla Azure RemoteApp](./media/remoteapp-clients/Android7.png)

## <a name="ios"></a>iOS

Po zainstalowaniu aplikacji Microsoft Remote Desktop ze sklepu App store, możesz znaleźć go na liście aplikacji, w obszarze **Klienta usług pulpitu zdalnego**.

1. Uruchamianie aplikacji przesuwa możesz pustego Centrum połączenia, jeśli już korzystasz aplikacji. Aby rozpocząć pracę z Azure RemoteApp, naciśnij przycisk Dodaj przycisk **"" +""** , a następnie wybierz polecenie **Dodaj RemoteApp Azure**.

    ![Opróżnij Centrum połączeń](./media/remoteapp-clients/IOS1.png)

2. Możesz muszą Zaloguj się przy użyciu adresu e-mail dostęp do usługi, aby uruchomić ten proces, wpisz swój **adres e-mail** i wybierz przycisk **Kontynuuj**.

    ![Zaloguj się w wierszu](./media/remoteapp-clients/picture1.png)

3. Postępuj zgodnie z instrukcjami na ekranie, aby zalogować się przy użyciu swojego konta Microsoft (Live ID —) lub identyfikator organizacji. Po zalogowaniu się mogą się pojawić się stronę zaproszeń, które zostały odebrane. Jeśli jesteś, wybierz pozycję zaproszeń zaufania i naciśnij przycisk **Gotowe**.

    ![Strona zaproszeń](./media/remoteapp-clients/IOS3.png)

4. Po zaakceptowaniu zaproszeń, na liście aplikacji, których masz dostęp do będą pobierane na urządzeniu i udostępniane w Centrum połączenia. Wybierz jedną z aplikacje, aby go uruchomić i rozpoczęcie korzystania z niego.

    ![Centrum połączeń z kanału informacyjnego](./media/remoteapp-clients/IOS4.png)

5. Jeśli nie masz jeszcze zaproszenia, nadal możesz wypróbować usługę. Aby to zrobić, naciśnij pozycję **Przejdź do bezpłatnej wersji próbnej** , po wyświetleniu monitu.

    ![Pokaz kanału informacyjnego monit](./media/remoteapp-clients/IOS5.png)

6. Spowoduje to umożliwiają dostęp do podstawowy zestaw aplikacje ułatwiające rozpoczęcie pracy z RemoteApp.

    ![Pokaz kanału dla Azure RemoteApp](./media/remoteapp-clients/IOS6.png)

## <a name="mac-os-x"></a>Mac OS X

Po zainstalowaniu aplikacji Microsoft Remote Desktop ze sklepu z aplikacjami, możesz znaleźć go na liście aplikacji, w obszarze **Pulpit zdalny firmy Microsoft**.

1. Uruchamianie aplikacji przesuwa możesz pustego Centrum połączenia, jeśli już korzystasz aplikacji. Aby rozpocząć pracę z Azure RemoteApp, kliknij przycisk **Azure RemoteApp** .

    ![Opróżnij Centrum połączeń](./media/remoteapp-clients/Mac1.png)

2. Możesz potrzebujesz Zaloguj się przy użyciu adresu e-mail dostęp do usługi, aby uruchomić ten proces, naciśnij pozycję **Wprowadzenie**.

    ![Zaloguj się w wierszu](./media/remoteapp-clients/Mac2.png)

3. Na następnej stronie wpisz swój **adres e-mail** i wybierz przycisk **Kontynuuj**. To rozpoczyna się przy użyciu usługi Azure Active Directory procesu logowania w.

    ![Pierwsza strona usługi Azure Active Directory](./media/remoteapp-clients/picture2.png)

4. Postępuj zgodnie z instrukcjami na ekranie, aby zalogować się przy użyciu swojego konta Microsoft (Live ID —) lub identyfikator organizacji. Po zalogowaniu się mogą się pojawić się stronę zaproszeń, które zostały odebrane. Jeśli jesteś, wybierz pozycję zaproszeń zaufania i zamknij okno dialogowe.

    ![Strona zaproszeń](./media/remoteapp-clients/Mac4.png)

5. Po zaakceptowaniu zaproszeń, na liście aplikacji, których masz dostęp do będą pobierane na urządzeniu i udostępniane w Centrum połączenia. Kliknij dwukrotnie jednej z aplikacji, aby go uruchomić i rozpoczęcie korzystania z niego.

    ![Centrum połączeń z kanału informacyjnego](./media/remoteapp-clients/Mac5.png)

6. Jeśli nie masz jeszcze zaproszenia, nadal możesz wypróbować usługę. Aby to zrobić, kliknij przycisk **Przejdź do bezpłatnej wersji próbnej** , po wyświetleniu monitu.

    ![Pokaz kanału informacyjnego monit](./media/remoteapp-clients/Mac6.png)

7. Spowoduje to umożliwiają dostęp do podstawowy zestaw aplikacje ułatwiające rozpoczęcie pracy z RemoteApp.

    ![Pokaz kanału informacyjnego dla Azure RemoteApp](./media/remoteapp-clients/Mac7.png)

## <a name="windows-all-supported-versions-except-windows-phone"></a>Windows (wszystkich obsługiwanych wersji z wyjątkiem Windows Phone)

Klient jest uruchamiane automatycznie po zakończeniu instalacji, jednak gdy potrzebne do nich dostęp później można znaleźć na liście aplikacji pod nazwą **Azure RemoteApp**.

1. Uruchamianie klienta, pierwszej strony, które widzisz ater czeka do Azure RemoteApp. Aby kontynuować, kliknij przycisk **Rozpocząć pracę**.

    ![Strona powitalna klienta Azure RemoteApp](./media/remoteapp-clients/Windows1.png)

2. Następna strona zaczyna się znaku w procesie dla RemoteApp Azure za pomocą usługi Azure Active Directory. Ten proces powinna wyglądać znanych, jeśli w przeszłości używano usług firmy Microsoft. Zacznij od wpisania swój **adres e-mail** , a następnie kliknij przycisk **Kontynuuj**.

    ![Pierwszy wiersz usługi Azure Active Directory](./media/remoteapp-clients/Windows2.png)

3. Postępuj zgodnie z instrukcjami na ekranie, aby zalogować się przy użyciu swojego konta Microsoft (Live ID —) lub identyfikator organizacji. Po zalogowaniu się mogą się pojawić się stronę zaproszeń, które zostały odebrane. Jeśli jesteś, wybierz pozycję zaproszeń zaufania i kliknij przycisk **Gotowe**.

    ![Strona zaproszeń klienta Azure RemoteApp](./media/remoteapp-clients/Windows3.png)

4. Po zaakceptowaniu zaproszeń, na liście aplikacji, których masz dostęp do będą pobierane na urządzeniu i udostępniane w Centrum połączenia. Kliknij dwukrotnie jednej z aplikacji, aby go uruchomić i rozpoczęcie korzystania z niego.

    ![Centrum połączeń klienta Azure RemoteApp](./media/remoteapp-clients/Windows4.png)

5. Jeśli nie wysłał jeszcze zaproszenia, nie martw się będzie mamy! Nadal będą mieć dostęp do zbioru pokaz, aby można było sprawdzić się usługę.

    ![Pokaz kanału informacyjnego dla Azure RemoteApp](./media/remoteapp-clients/Windows5.png)

## <a name="windows-phone-81"></a>Windows Phone 8.1

Po zainstalowaniu aplikacji Microsoft Remote Desktop ze Sklepu Windows Phone 8.1, możesz go znaleźć na liście aplikacji, w obszarze **Pulpit zdalny**.

1. Uruchamianie aplikacji powoduje możesz bezpośrednio do pustego Centrum połączenia, jeśli już korzystasz aplikacji. Aby rozpocząć pracę z Azure RemoteApp, naciśnij przycisk Dodaj przycisk **"" +""** u dołu ekranu.

    ![Opróżnij Centrum połączeń](./media/remoteapp-clients/WinPhone1.png)

2. Następnie wybierz **Azure RemoteApp**.

    ![Dodawanie elementu strony](./media/remoteapp-clients/WinPhone2.png)

3. Potrzebujesz Zaloguj się przy użyciu adresu e-mail w celu uzyskania dostępu do usługi, aby uruchomić ten proces naciśniesz **połączenia**.

    ![Zaloguj się w wierszu](./media/remoteapp-clients/WinPhone3.png)

4. Na następnej stronie wpisz swój **adres e-mail** i wybierz przycisk **Kontynuuj**. To rozpoczyna się przy użyciu usługi Azure Active Directory procesu logowania w.

    ![Pierwsza strona usługi Azure Active Directory](./media/remoteapp-clients/WinPhone4.png)

5. Postępuj zgodnie z instrukcjami na ekranie, aby zalogować się przy użyciu swojego konta Microsoft (Live ID —) lub identyfikator organizacji. Po zalogowaniu się mogą się pojawić się stronę zaproszeń, które zostały odebrane. Jeśli jesteś, wybierz pozycję zaproszeń zaufania i wybierz przycisk **Zapisz**.

    ![Strona zaproszeń](./media/remoteapp-clients/WinPhone5.png)

6. Po zaakceptowaniu zaproszeń, na liście aplikacji, których masz dostęp do będą pobierane na urządzeniu i udostępniane w Centrum połączenia. Wybierz jedną z aplikacje, aby go uruchomić i rozpoczęcie korzystania z niego.

    ![Centrum połączeń z kanału informacyjnego](./media/remoteapp-clients/WinPhone6.png)

7. Jeśli nie masz jeszcze zaproszenia, nadal możesz wypróbować usługę. Aby to zrobić, wybierz przycisk **Tak,** po wyświetleniu monitu.

    ![Pokaz kanału informacyjnego monit](./media/remoteapp-clients/WinPhone7.png)

8. Spowoduje to umożliwiają dostęp do podstawowy zestaw aplikacje ułatwiające rozpoczęcie pracy z RemoteApp.

    ![Pokaz kanału informacyjnego dla Azure RemoteApp](./media/remoteapp-clients/WinPhone8.png)
 