<properties
    pageTitle="Stos Azure aplikacji usługi Technical Preview 1 przed rozpoczęciem | Microsoft Azure"
    description="Czynności do wykonania przed wdrożeniem aplikacji sieci Web na stos Azure"
    services="azure-stack"
    documentationCenter=""
    authors="apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>
    
# <a name="before-you-get-started-with-azure-stack-web-apps"></a>Przed rozpoczęciem pracy z aplikacjami Web stos Azure

> [AZURE.NOTE] Poniższe informacje dotyczą tylko wdrożeń TP1 stos Azure.

Potrzebujesz kilku elementów, aby zainstalować aplikacje sieci Web stos Azure:

- Ukończono wdrażanie [Azure stosem Technical Preview 1](azure-stack-run-powershell-script.md)
- Ilość miejsca dostępnego w systemie stosem Azure small wdrażania aplikacji sieci Web stosem Azure.  Określa wymaganą ilość miejsca to około 20GB miejsca na dysku.
- [Serwer, na którym jest uruchomiony program SQL Server](#SQL-Server).

>[AZURE.NOTE] Następujące czynności, które wszystkie miejsce na maszyn wirtualnych klienta.

## <a name="before-you-deploy-azure-stack-web-apps"></a>Przed rozpoczęciem wdrażania aplikacji sieci Web stosem Azure

Aby wdrożyć dostawcy zasobów, należy uruchomić Environment(ISE) skryptów programu PowerShell zintegrowane jako administrator. Z tego powodu należy zezwolić pliki cookie i JavaScript w profilu programu Internet Explorer, którego używasz, aby zalogować się do usługi Azure Active Directory.

## <a name="turn-off-internet-explorer-enhanced-security"></a>Wyłączanie funkcji zabezpieczeń programu Internet Explorer rozszerzony

1.  Zaloguj się do komputera (aby Zapewnić)-koncepcji stosem Azure **AzureStack-**administrator, a następnie otwórz **Menedżera serwera**.
2.  Wyłącz funkcję **Konfiguracji zwiększonych zabezpieczeń programu Internet Explorer** , zarówno dla administratorów i użytkowników.
3.  Zaloguj się do maszyny wirtualnej ClientVM.AzureStack.local jako administrator, a następnie otwórz **Menedżera serwera**.
4.  Wyłącz funkcję **Konfiguracji zwiększonych zabezpieczeń programu Internet Explorer** , zarówno dla administratorów i użytkowników.

## <a name="enable-cookies"></a>Włączanie plików cookie

1.  Wybierz pozycję **Start**> **wszystkie aplikacje**> **Akcesoria systemu Windows**. Kliknij prawym przyciskiem myszy **Program Internet Explorer** > **więcej** > **polecenie Uruchom jako administrator**.
2.  Jeśli zostanie wyświetlony monit, wybierz pozycję **Użyj zalecanych zabezpieczeń**, a następnie wybierz **przycisk OK**.
3.  W programie Internet Explorer wybierz pozycję **Narzędzia** (ikona koła zębatego) > **Opcje internetowe** > **prywatności** > **Zaawansowane**.
4.  Wybierz pozycję **Zaawansowane**. Upewnij się, że oba pola wyboru **Zaakceptuj** są zaznaczone, a następnie wybierz **przycisk OK** i **OK** ponownie.
5.  Zamknij program Internet Explorer, a następnie ponownie uruchom środowiska PowerShell ISE jako administrator.

## <a name="install-the-latest-version-of-azure-powershell"></a>Zainstaluj najnowszą wersję programu PowerShell Azure

1.  Zaloguj się do komputera Zapewnić stos Azure **AzureStack-**administrator.
2.  Podłączanie pulpitu zdalnego umożliwia logowanie się do komputera wirtualnych ClientVM.AzureStack.local jako administrator.
3.  Otwórz **Panel sterowania** i wybierz pozycję **Odinstaluj program**. 
4.  Kliknij prawym przyciskiem myszy **Microsoft Azure programu PowerShell - 2015 listopada** i wybierz polecenie **Odinstaluj**.
5.  Odinstaluj po zakończeniu, ponownie uruchomić komputer wirtualnych ClientVM.AzureStack.local
6.  Pobierz i zainstaluj najnowszą [Azure programu PowerShell](http://aka.ms/azstackpsh).


## <a name="sql-server"></a>Program SQL Server

Instalator aplikacji sieci Web stos Azure domyślnie używać programu SQL Server zainstalowanego wraz z dostawcę zasobu SQL Azure stosu. Po zainstalowaniu dostawcy zasobów programu SQL Server (SQL RP) upewnij się, że należy wziąć pod uwagę użytkownika administratora bazy danych i hasło. Potrzebne po zainstalowaniu aplikacji sieci Web stos Azure.
Uwaga: Również uzyskuje opcji wdrażanie i uruchamianie programu SQL Server przy użyciu innego serwera. Możesz wybrać wystąpienie programu SQL Server umożliwia po zakończeniu opcje Instalatora Azure stos Web Apps.
