<properties 
    pageTitle="Włączanie pulpitu zdalnego dla usług w chmurze (Node.js)" 
    description="Dowiedz się, jak włączyć pulpitu zdalnego dostępu dla maszyn wirtualnych obsługę aplikacji Azure Node.js." 
    services="cloud-services" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="enabling-remote-desktop-in-azure"></a>Włączanie pulpitu zdalnego platformy Azure

Pulpit zdalny pozwala uzyskać dostęp do pulpitu wystąpienia roli działa Azure. Podłączanie pulpitu zdalnego umożliwia konfigurowanie maszyny wirtualnej lub Rozwiązywanie problemów z aplikacją.

> [AZURE.NOTE] Ten artykuł dotyczy aplikacji Node.js znajdującej się jako usługa w chmurze Azure.


## <a name="prerequisites"></a>Wymagania wstępne

- Zainstaluj i skonfiguruj [Azure programu Powershell](../powershell-install-configure.md).
- Wdrażanie aplikacji Node.js w usłudze Azure chmury. Aby uzyskać więcej informacji zobacz [Tworzenie i wdrażanie aplikacji Node.js, aby usługa w chmurze Azure](cloud-services-nodejs-develop-deploy-app.md).


## <a name="step-1-use-azure-powershell-to-configure-the-service-for-remote-desktop-access"></a>Krok 1: Użyj Azure PowerShell, aby skonfigurować usługę dostępu zdalnego pulpitu

Aby użyć pulpitu zdalnego, musisz zaktualizować definicji usługi Azure i konfiguracji przy użyciu nazwy użytkownika, hasło i certyfikatu. 

Z komputera, który zawiera pliki źródłowe dla aplikacji, należy wykonać następujące czynności.

1. Uruchamianie **programu Windows PowerShell** jako Administrator. (Z poziomu **Start Menu** lub **Ekranu startowego**wyszukiwania dla **Programu Windows PowerShell**.)

2.  Przejdź do katalogu zawierającego definicji usługi (.csdef) i pliki konfiguracji (.cscfg) usługi.

3. Wpisz następujące polecenie cmdlet programu PowerShell:

        Enable-AzureServiceProjectRemoteDesktop

4. Po wyświetleniu monitu wprowadź nazwę użytkownika i hasło.

    ![Włącz azureserviceprojectremotedesktop][enable-rdp]

3.  Wpisz następujące polecenie cmdlet programu PowerShell, aby opublikować zmiany:

        Publish-AzureServiceProject

    ![Publikowanie azureserviceproject][publish-project]

## <a name="step-2-connect-to-the-role-instance"></a>Krok 2: Połączyć się z wystąpieniem roli

Po opublikowaniu definicji usługi aktualizacji, można połączyć się z wystąpieniem roli.

1.  W [portalu klasyczny Azure]wybierz **Usług w chmurze** , a następnie wybierz pozycję usługi.

    ![Portal Azure klasyczny][cloud-services]

2.  Kliknij przycisk **wystąpienia**, a następnie kliknij **produkcji** lub **tymczasowego** , aby wyświetlić wystąpienia tej usługi. Zaznacz obiekt, a następnie kliknij przycisk **Połącz** u dołu strony.

    ![Na stronie wystąpień][3]

2.  Po kliknięciu przycisku **Połącz**w przeglądarce sieci web zostanie wyświetlony monit o zapisanie pliku .rdp. Otwórz ten plik. (Na przykład, jeśli używasz programu Internet Explorer, kliknij przycisk **Otwórz**.)

    ![monit o otwarcie lub zapisanie pliku RDP][4]

3.  Po otwarciu pliku zostanie wyświetlony następujący monit zabezpieczeń:

    ![Wiersz zabezpieczeń systemu Windows][5]

4.  Kliknij przycisk **Połącz**i zabezpieczeń zostanie wyświetlony komunikat do wprowadzania poświadczenia, aby uzyskać dostęp do tego wystąpienia. Wprowadzenie hasła utworzonego w [kroku 1] [krok 1: Konfigurowanie usługi pulpitu zdalnego dostępu przy użyciu programu PowerShell Azure], a następnie kliknij **przycisk OK**.

    ![monit nazwy użytkownika i hasła][6]

Gdy połączenie zostanie nawiązane, Podłączanie pulpitu zdalnego wyświetla pulpit wystąpienia platformy Azure. 

![Sesji pulpitu zdalnego][7]

## <a name="step-3-configure-the-service-to-disable-remote-desktop-access"></a>Krok 3: Konfigurowanie usługi, aby wyłączyć pulpitu zdalnego dostępu 

Gdy już nie potrzebujesz połączenia pulpitu zdalnego wystąpieniach ról w chmurze, wyłącz dostęp zdalny do pulpitu przy użyciu [Programu PowerShell Azure].

1.  Wpisz następujące polecenie cmdlet programu PowerShell:

        Disable-AzureServiceProjectRemoteDesktop

2.  Wpisz następujące polecenie cmdlet programu PowerShell, aby opublikować zmiany:

        Publish-AzureServiceProject

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Zdalne uzyskiwanie dostępu do roli wystąpienia platformy Azure] 
- [Za pomocą pulpitu zdalnego z rolami Azure]
- [Centrum deweloperów node.js](/develop/nodejs/)

  [Azure programu PowerShell]: http://go.microsoft.com/?linkid=9790229&clcid=0x409

[Portal Azure klasyczny]: http://manage.windowsazure.com
[publish-project]: ./media/cloud-services-nodejs-enable-remote-desktop/publish-rdp.png
[enable-rdp]: ./media/cloud-services-nodejs-enable-remote-desktop/enable-rdp.png
[cloud-services]: ./media/cloud-services-nodejs-enable-remote-desktop/cloud-services-remote.png
[3]: ./media/cloud-services-nodejs-enable-remote-desktop/cloud-service-instance.png
[4]: ./media/cloud-services-nodejs-enable-remote-desktop/rdp-open.png
[5]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-12.png
[6]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-13.png
[7]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-14.png
  
[Zdalne uzyskiwanie dostępu do roli wystąpienia platformy Azure]: http://msdn.microsoft.com/library/windowsazure/hh124107.aspx
[Za pomocą pulpitu zdalnego z rolami Azure]: http://msdn.microsoft.com/library/windowsazure/gg443832.aspx
 