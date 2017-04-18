<properties 
    pageTitle="Azure RemoteApp Rozwiązywanie problemów — błędy połączenia i uruchamianie aplikacji | Microsoft Azure" 
    description="Dowiedz się, jak rozwiązywać problemy z uruchamianiem i nawiązywanie połączeń z aplikacji w Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="ericorman" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



#<a name="troubleshoot-azure-remoteapp---application-launch-and-connection-failures"></a>Rozwiązywanie problemów z Azure RemoteApp - błędy połączenia i uruchamianie aplikacji 

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Aplikacje obsługiwany w Azure RemoteApp mogą nie być uruchomić kilka różnych powodów. Ten artykuł zawiera opis różnych przyczyn i komunikaty o błędach użytkowników może pojawić się podczas próby uruchomienia aplikacji. Również zawiera informacje o błędy połączenia. (Ale w tym artykule opisano problemy podczas logowania się do klienta Azure RemoteApp).  

Poniżej informacji na temat typowych komunikatów o błędach z powodu błędów połączenia i uruchamianie aplikacji.

##<a name="were-getting-you-set-up-try-again-in-10-minutes"></a>Zaczynamy mniej skonfiguruj... Spróbuj ponownie 10 minut.

Ten błąd oznacza, że funkcja RemoteApp platformy Azure jest skalowanie wewnętrzne spotkania konieczności wydajność pracy użytkowników. W tle więcej wystąpień maszyn wirtualnych RemoteApp Azure są tworzone do obsługi potrzeb wydajność pracy użytkowników. Zwykle to trwa około pięć minut, ale może potrwać do 10 minut. Czasami niezbyt wystarczająco szybko i zasoby są wymagane natychmiast. Na przykład 9 AM scenariusz, gdzie wielu użytkowników, musisz użyć aplikacji w Azure RemoteAppn w tym samym czasie. W takim przypadku do Ciebie firma Microsoft włączyć **Tryb zdolności** na końcu Wstecz. W tym celu otwórz bilet pomocy technicznej Azure i/lub wiadomość e-mail na adres [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com). Upewnij się uwzględnić identyfikator subskrypcji w wezwaniu.  

![Firma Microsoft występują Skonfiguruj](./media/remoteapp-apptrouble/ra-apptrouble1.png)

## <a name="could-not-auto-reconnect-to-your-applications-please-re-launch-your-application"></a>Nie można ponownie do automatycznego aplikacji, ponownie uruchom aplikację  

Ten komunikat o błędzie często jest widoczny, jeśli korzystasz ze Azure RemoteApp, a następnie umieść na komputerze wstrzymania dłużej niż 4 godziny i następnie pracy komputera w górę, a klient Azure RemoteApp próby automatycznego ponownego połączenia i Przekroczono limit czasu.  Poinstruuj użytkowników, aby wrócić do aplikacji, a następnie spróbuj otworzyć go w kliencie Azure RemoteApp.

![Nie można automatycznie-ponownie podłączyć aplikacji](./media/remoteapp-apptrouble/ra-apptrouble2.png) 

## <a name="problems-with-the-temp-profile"></a>Problemy z temp profilu 

Ten błąd występuje, gdy nie można zainstalować profil użytkownika (dysk profilu użytkownika) i użytkownik otrzymywane tymczasowy profil.  Administratorzy należy przejść do kolekcji w portalu Azure przejdź na kartę **Sesje** i próba **Wyloguj** użytkownika. To zostanie wymusić pełny dziennik poza sesji użytkownika — a następnie użytkownik próbuje ponownie uruchomić aplikację. W przypadku niepowodzenia kontakt z pomocą techniczną Azure i wiadomość e-mail na adres [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com).

## <a name="azure-remoteapp-has-stopped-working"></a>Funkcja RemoteApp Azure przestał działać

Ten komunikat o błędzie oznacza, że klient Azure RemoteApp ma problem i konieczne jest ponowne uruchomienie. Poinstruuj użytkowników, aby zamknąć: wybierz pozycję **Zamknij program** , a następnie uruchom ponownie klienta Azure RemoteApp.  Jeśli problem będzie nadal występował, Otwórz i bilet pomocy technicznej Azure i lub wiadomość e-mail na adres [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com).

![Funkcja RemoteApp Azure przestał działać](./media/remoteapp-apptrouble/ra-apptrouble3.png)  

## <a name="an-error-occurred-while-remote-desktop-connection-was-accessing-this-resource-retry-the-connection-or-contact-your-system-administrator"></a>Wystąpił błąd podczas Podłączanie pulpitu zdalnego została uzyskiwanie dostępu do tego zasobu. Spróbuj połączyć się ponownie lub skontaktuj się z administratorem systemu

Jest to ogólnego komunikatu o błędzie — kontakt z pomocą techniczną Azure i wiadomość e-mail na adres [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com) , firma Microsoft może analizować problemy. 

![Ogólny komunikat Azure RemoteApp](./media/remoteapp-apptrouble/ra-apptrouble4.png) 